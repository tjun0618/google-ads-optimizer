# Data Parser Reference — Google Ads Optimization Skill

> 解析Google Ads后台导出的两个Excel报告（搜索关键字报告 + 搜索字词报告）

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
| 状况原因 | status_reason | string | 暂停原因（如有） |
| 展示次数 | impressions | int | 展示数 |
| 点击率 | ctr | float | CTR百分比 |
| 费用 | cost | float | 总费用（CNY） |
| 点击次数 | clicks | int | 点击数 |
| 转化率 | conversion_rate | float | 转化率百分比 |
| 转化次数 | conversions | int | 转化数 |
| 平均每次点击费用 | cpc | float | 平均CPC（CNY） |
| 每次转化费用 | cpa | float | CPA（CNY） |

### 解析步骤

```python
# Step 1: 读取Excel，header在第3行（index=2）
df = pd.read_excel('搜索关键字报告.xlsx', header=2)

# Step 2: 重命名列
df.columns = ['keyword_status', 'keyword', 'match_type', 'ad_group', 'status', 
              'status_reason', 'landing_url', 'impressions', 'ctr', 'currency',
              'cost', 'clicks', 'conversion_rate', 'conversions', 'cpc', 'cpa']

# Step 3: 清洗数据
# 移除总计行和空行
df_clean = df[df['keyword'].notna() & ~df['keyword'].str.contains('总计', na=False)].copy()

# 转换数值列
num_cols = ['impressions', 'clicks', 'cost', 'cpc', 'conversions', 'cpa']
for col in num_cols:
    df_clean[col] = pd.to_numeric(df_clean[col], errors='coerce')

# Step 4: 汇总统计
total_cost = df_clean['cost'].sum()
total_clicks = df_clean['clicks'].sum()
total_impr = df_clean['impressions'].sum()
total_conv = df_clean['conversions'].sum()
avg_ctr = total_clicks / total_impr if total_impr > 0 else 0
avg_cpc = total_cost / total_clicks if total_clicks > 0 else 0
```

### 关键输出指标

| 指标 | 计算方式 | 用途 |
|------|---------|------|
| 总费用 | sum(cost) | 评估投放规模 |
| 总点击 | sum(clicks) | 评估流量规模 |
| 总展示 | sum(impressions) | 评估曝光规模 |
| 总转化 | sum(conversions) | 评估效果 |
| 整体CTR | total_clicks / total_impr | 评估吸引力 |
| 整体CPC | total_cost / total_clicks | 评估成本效率 |
| 整体CPA | total_cost / total_conv | 评估转化成本 |
| 整体转化率 | total_conv / total_clicks | 评估转化效率 |

---

## 2. 搜索字词报告解析

### 文件结构
与搜索关键字报告相同：
- Row 0: 报告标题
- Row 1: 时间范围
- Row 2: 列标题（实际header行）
- Row 3+: 数据行
- 最后几行: 总计行

### 列名映射

| 原始列名 | 映射名 | 类型 | 说明 |
|----------|--------|------|------|
| 搜索字词 | search_term | string | 用户实际搜索词 |
| 匹配类型 | match_type | string | 触发该字词的匹配类型 |
| 已添加/已排除 | added_excluded | string | 都没有/已添加/已排除 |
| 广告组 | ad_group | string | 所属广告组 |
| 点击次数 | clicks | int | 点击数 |
| 展示次数 | impressions | int | 展示数 |
| 点击率 | ctr | float | CTR百分比 |
| 货币代码 | currency | string | CNY |
| 平均每次点击费用 | cpc | float | 平均CPC |
| 费用 | cost | float | 总费用 |
| 转化率 | conversion_rate | float | 转化率 |
| 转化次数 | conversions | int | 转化数 |
| 每次转化费用 | cpa | float | CPA |

### 解析步骤

```python
# Step 1: 读取Excel，header在第3行
df = pd.read_excel('搜索字词报告.xlsx', header=2)

# Step 2: 重命名列
df.columns = ['search_term', 'match_type', 'added_excluded', 'ad_group', 
              'clicks', 'impressions', 'ctr', 'currency', 'cpc', 
              'cost', 'conversion_rate', 'conversions', 'cpa']

# Step 3: 清洗数据
df_clean = df[df['search_term'].notna()].copy()

# 转换数值列
num_cols = ['clicks', 'impressions', 'cost', 'cpc', 'conversions']
for col in num_cols:
    df_clean[col] = pd.to_numeric(df_clean[col], errors='coerce')

# Step 4: 分类统计
# 按广告组分组
for ag in df_clean['ad_group'].dropna().unique():
    ag_data = df_clean[df_clean['ad_group'] == ag]
    print(f"{ag}: {len(ag_data)} terms, cost={ag_data['cost'].sum():.2f}")

# 按添加状态分组
added = df_clean[df_clean['added_excluded'].str.contains('已添加', na=False)]
excluded = df_clean[df_clean['added_excluded'].str.contains('已排除', na=False)]
neither = df_clean[~df_clean['added_excluded'].str.contains('已添加|已排除', na=False)]
```

### 关键分类分析

#### 2.1 按意图分类

```python
# 品牌词（包含品牌名）
brand_terms = df[df['search_term'].str.contains('品牌名|品牌变体', case=False, na=False)]

# 竞品词（包含竞品品牌名）
competitor_brands = ['dyson', 'shark', 'revlon', 'conair', ...]  # 根据品类动态识别
competitor_terms = df[df['search_term'].str.contains('|'.join(competitor_brands), case=False, na=False)]

# 通用词（非品牌非竞品）
generic_terms = df[~df['search_term'].str.contains('品牌名|'+'|'.join(competitor_brands), case=False, na=False)]
```

#### 2.2 按效果分类

```python
# 有转化字词
converting = df[df['conversions'] > 0]

# 有点击但零转化
no_conv = df[(df['clicks'] > 0) & (df['conversions'] == 0)]

# 零点击（仅展示）
zero_click = df[df['clicks'] == 0]

# 高费用零转化（浪费严重）
waste = df[(df['cost'] > 10) & (df['conversions'] == 0)]  # 费用>10元且0转化
```

#### 2.3 按添加状态分类

```python
# 已添加到关键词列表
added_terms = df[df['added_excluded'].str.contains('已添加', na=False)]

# 已排除（否定关键词）
excluded_terms = df[df['added_excluded'].str.contains('已排除', na=False)]

# 未处理（"都没有"）— 这些是优化机会
unhandled = df[~df['added_excluded'].str.contains('已添加|已排除', na=False)]
```

---

## 3. 数据交叉分析

### 3.1 关键词 vs 搜索字词对比

```python
# 找出哪些搜索字词触发了关键词
df_kw['keyword_clean'] = df_kw['keyword'].str.replace('"', '').str.replace('[', '').str.replace(']', '')

# 检查搜索字词是否匹配任何关键词
for term in df_st['search_term']:
    matched = False
    for kw in df_kw['keyword_clean']:
        if kw.lower() in term.lower() or term.lower() in kw.lower():
            matched = True
            break
    if not matched:
        print(f"未匹配关键词的搜索字词: {term}")
```

### 3.2 广告组覆盖率分析

```python
# 广告方案中设计的广告组
planned_groups = ['AG1A', 'AG1B', 'AG2A', 'AG2B', ...]

# 实际有流量的广告组
active_groups = df_kw[df_kw['impressions'] > 0]['ad_group'].unique()

# 未启动的广告组
missing_groups = set(planned_groups) - set(active_groups)
```

---

## 4. 常见Excel解析问题及解决

### 问题1: 列数不匹配
**原因**: 不同Google Ads账户导出的报告列数可能不同（有些有"广告系列"列，有些没有）
**解决**: 先读取原始列名，动态映射，不要硬编码列名

### 问题2: 编码问题
**原因**: Windows系统默认GBK编码，Excel中的特殊字符（如¥）可能导致编码错误
**解决**: 
```python
import sys, io
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')
```

### 问题3: 数值列含字符串
**原因**: Google Ads导出的报告中，0值可能显示为"--"或空字符串
**解决**: 使用 `pd.to_numeric(errors='coerce')` 将非数值转为NaN

### 问题4: 总计行干扰
**原因**: 报告末尾有"总计"行，会干扰统计分析
**解决**: 过滤掉包含"总计"字样的行

---

## 5. 输出数据结构

解析完成后，应输出以下结构化数据供后续分析使用：

```json
{
  "keyword_report": {
    "total_keywords": 32,
    "total_cost": 556.44,
    "total_clicks": 120,
    "total_impressions": 2397,
    "total_conversions": 0,
    "avg_ctr": 0.0501,
    "avg_cpc": 4.64,
    "keywords": [
      {
        "keyword": "sri dry q hair dryer reviews",
        "match_type": "广泛匹配",
        "ad_group": "Brand Core",
        "status": "已启用",
        "impressions": 1071,
        "clicks": 37,
        "cost": 140.82,
        "cpc": 3.81,
        "conversions": 0
      }
    ],
    "by_ad_group": {
      "Brand Core": {"keywords": 21, "cost": 506.42, "clicks": 108},
      "Brand Long-tail": {"keywords": 11, "cost": 50.02, "clicks": 12}
    }
  },
  "search_term_report": {
    "total_terms": 100,
    "total_cost": 556.44,
    "total_clicks": 120,
    "total_impressions": 2397,
    "terms_with_clicks": 20,
    "zero_click_terms": 80,
    "terms": [
      {
        "search_term": "ionic hair dryer amazon",
        "match_type": "广泛匹配",
        "ad_group": "Brand Core",
        "added_excluded": "都没有",
        "impressions": 1,
        "clicks": 0,
        "cost": 0
      }
    ],
    "by_category": {
      "brand": {"count": 5, "cost": 50.0},
      "competitor": {"count": 30, "cost": 100.0},
      "generic": {"count": 65, "cost": 406.44}
    }
  }
}
```
