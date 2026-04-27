# Data Parser Reference — Google Ads Optimization Skill v3.0

> 解析Google Ads后台导出的两个Excel报告 + YP平台品牌转化报表
>
> **v3.0定位**：本文件只负责数据解析（读取和结构化）。
> 利润分析（盈亏平衡CPC计算、品牌级ROI、关键词利润评级）已迁移到诊断技能（google-ads-diagnosis）。
> 本文件作为诊断技能的数据输入模块被引用。

---

## 1. 搜索关键字报告解析

### 文件结构
Google Ads导出的Excel文件格式：
- Row 0: 报告标题（如"搜索关键字报告"）
- Row 1: 时间范围（如"所有时间"）
- Row 2: 列标题（实际header行）
- Row 3+: 数据行
- 最后几行: 总计行（包含"总计"字样）

### 列名映射

| 原始列名 | 映射名 | 类型 | 说明 |
|----------|--------|------|------|
| 关键字状态 | keyword_status | string | 已启用/已暂停 |
| 关键字 | keyword | string | 关键词文本 |
| 匹配类型 | match_type | string | 广泛匹配/词组匹配/精确匹配 |
| 广告组 | ad_group | string | 所属广告组 |
| 状态 | status | string | 有效/已暂停/很少投放 |
| 展示次数 | impressions | int | 展示数 |
| 费用 | cost | float | 总费用（CNY） |
| 点击次数 | clicks | int | 点击数 |
| 点击率 | ctr | float | CTR百分比 |
| 转化次数 | conversions | int | 转化数 |
| 平均每次点击费用 | cpc | float | 平均CPC（CNY） |

### 解析步骤

```python
# Step 1: 读取Excel，header在第3行（index=2）
df = pd.read_excel('搜索关键字报告.xlsx', header=2)

# Step 2: 清洗数据
df_clean = df[df['keyword'].notna() & ~df['keyword'].str.contains('总计', na=False)].copy()
num_cols = ['impressions', 'clicks', 'cost', 'cpc', 'conversions']
for col in num_cols:
    df_clean[col] = pd.to_numeric(df_clean[col], errors='coerce')

# Step 3: 汇总统计
total_cost = df_clean['cost'].sum()
total_clicks = df_clean['clicks'].sum()
total_impr = df_clean['impressions'].sum()
avg_cpc = total_cost / total_clicks if total_clicks > 0 else 0
```

### 关键输出指标

| 指标 | 计算方式 | Affiliate备注 |
|------|---------|--------------|
| 总费用 | sum(cost) | 直接成本 |
| 总点击 | sum(clicks) | 用于计算实际CPC |
| 总展示 | sum(impressions) | — |
| 整体CTR | total_clicks / total_impr | — |
| 整体CPC | total_cost / total_clicks | **核心指标：与盈亏平衡CPC对比** |

**Affiliate特殊说明**：
- Google Ads的"转化次数"列在Affiliate场景下通常为0，因为Amazon Associates转化无法被Google Ads追踪
- 不要以"0转化"作为问题判断依据（这是Affiliate的常态）

---

## 2. 搜索字词报告解析

### 文件结构
与搜索关键字报告相同（Row 0-2为标题/时间/header，Row 3+为数据，末尾有总计行）。

### 列名映射

| 原始列名 | 映射名 | 类型 | 说明 |
|----------|--------|------|------|
| 搜索字词 | search_term | string | 用户实际搜索词 |
| 匹配类型 | match_type | string | 触发该字词的匹配类型 |
| 已添加/已排除 | added_excluded | string | 都没有/已添加/已排除 |
| 广告组 | ad_group | string | 所属广告组 |
| 点击次数 | clicks | int | 点击数 |
| 展示次数 | impressions | int | 展示数 |
| 费用 | cost | float | 总费用 |
| 平均每次点击费用 | cpc | float | 平均CPC |

### 解析步骤

```python
df = pd.read_excel('搜索字词报告.xlsx', header=2)
df_clean = df[df['search_term'].notna()].copy()
num_cols = ['clicks', 'impressions', 'cost', 'cpc']
for col in num_cols:
    df_clean[col] = pd.to_numeric(df_clean[col], errors='coerce')

# 按添加状态分类
added = df_clean[df_clean['added_excluded'].str.contains('已添加', na=False)]
excluded = df_clean[df_clean['added_excluded'].str.contains('已排除', na=False)]
unhandled = df_clean[~df_clean['added_excluded'].str.contains('已添加|已排除', na=False)]
```

### 关键分类分析

**按效果分类**：
```python
no_conv = df_clean[(df_clean['clicks'] > 0) & (df_clean['conversions'] == 0)]
zero_click = df_clean[df_clean['clicks'] == 0]
waste = df_clean[(df_clean['cost'] > 10) & (df_clean['conversions'] == 0)]  # 费用>10元且0转化
```

**按广告组覆盖分析**：
```python
planned_groups = ['AG1A', 'AG1B', 'AG2A', ...]  # 来自广告方案文件
active_groups = df_kw[df_kw['impressions'] > 0]['ad_group'].unique()
missing_groups = set(planned_groups) - set(active_groups)  # 未启动的AG
```

---

## 3. YP平台品牌转化报表解析

### 数据来源与格式

**来源**：YP（YieldPlanet/Partnerize）后台 → Reports → Brand Performance
**格式**：截图或CSV导出
**数据粒度**：品牌级聚合（Merchant维度），非ASIN级，非关键词级

| 列名 | 说明 |
|------|------|
| Merchant / Brand | 品牌名称 |
| Click | 联盟链接总点击数（所有渠道） |
| Detail Page Views | 详情页浏览数 |
| Add to Carts | 加购数 |
| Purchases | 购买订单数 |
| Amount | 订单总金额 |
| Commission | 佣金收入 |

### 关键限制

```
YP报表提供：品牌级聚合（所有商品 + 所有渠道的总和）
Google Ads提供：关键词/搜索词/Campaign级粒度

结果：只能做品牌级ROI计算，无法知道"哪个关键词"带来了"哪笔订单"
```

### 解析步骤

```python
# 从截图/CSV提取数据（手动识别或OCR）
yp_data = {
    '品牌名': {
        'clicks': N,
        'dp_views': N,
        'add_to_carts': N,
        'purchases': N,
        'amount': N,
        'commission': N
    },
    ...
}

# 计算品牌级指标
for brand, data in yp_data.items():
    data['epc'] = data['commission'] / data['clicks'] if data['clicks'] > 0 else 0
    data['purchase_rate'] = data['purchases'] / data['clicks'] if data['clicks'] > 0 else 0
```

### 关键输出指标

| 指标 | 计算方式 | 备注 |
|------|---------|------|
| YP Clicks | 报表中的click列 | 品牌级总点击（所有渠道） |
| YP Commission | 报表中的Commission | **核心收入指标** |
| 品牌 EPC | Commission / Clicks | 品牌级每点击收益 |
| 加购率 | Add to Carts / Clicks | 反映产品页吸引力 |
| 购买率 | Purchases / Clicks | 反映整体转化效率 |

---

## 4. 常见解析问题

| 问题 | 原因 | 解决 |
|------|------|------|
| 列数不匹配 | 不同账户导出列不同 | 动态映射列名，不要硬编码 |
| 编码问题 | Windows默认GBK | `pd.to_numeric(errors='coerce')` |
| 数值列含字符串 | 0值显示为"--"或空 | 使用 `errors='coerce'` 转NaN |
| 总计行干扰 | 报告末尾有"总计" | 过滤 `str.contains('总计')` |

---

## 5. 输出数据结构

解析完成后，输出以下结构化数据供诊断技能使用：

```json
{
  "keyword_report": {
    "total_keywords": N,
    "total_cost": N,
    "total_clicks": N,
    "total_impressions": N,
    "avg_ctr": N,
    "avg_cpc": N,
    "keywords": [...],
    "by_ad_group": {...}
  },
  "search_term_report": {
    "total_terms": N,
    "total_cost": N,
    "total_clicks": N,
    "terms_with_clicks": N,
    "zero_click_terms": N,
    "unhandled_terms": N,
    "terms": [...]
  },
  "yp_report": {
    "brand_name": {...},
    "epc": N,
    "purchase_rate": N,
    "commission": N
  }
}
```
