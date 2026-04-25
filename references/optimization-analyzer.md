# Optimization Analyzer Reference — Google Ads Optimization Skill

> 基于解析后的数据，系统化诊断问题并生成优化建议

---

## 1. 问题诊断框架

### 1.1 问题分级标准

| 级别 | 代号 | 定义 | 处理优先级 |
|------|------|------|-----------|
| P0 | 致命 | 导致ROI为负或严重浪费预算的问题 | 今天必须处理 |
| P1 | 严重 | 显著影响效果或浪费预算的问题 | 本周内处理 |
| P2 | 中等 | 有优化空间但不紧急的问题 | 两周内处理 |

### 1.2 问题诊断清单

#### 问题1: 零转化（Conversion = 0）

**判定条件**: 总费用 > ¥50 且 总转化 = 0

**严重程度**: P0（致命）

**分析维度**:
1. **价格因素**: 产品价格是否过高？搜索词的价格预期是否匹配？
   - 如果搜索词是 "cheap"、"budget"、"under $50" 等低价词 → 价格错配
   - 如果搜索词是 "premium"、"luxury"、"high end" 等高价词 → 价格匹配
   
2. **流量质量**: 搜索字词是否与产品相关？
   - 如果大量搜索字词是竞品品牌名 → 流量错配
   - 如果搜索字词包含产品品类核心词 → 流量匹配
   
3. **着陆页问题**: 
   - Amazon产品页是否正常显示？
   - 库存是否充足？
   - 评价数量/评分是否足够？
   
4. **联盟链接摩擦**: 
   - 联盟链接跳转是否增加了一层摩擦？
   - 用户是否信任联盟链接？

**优化建议模板**:
```
问题: 零转化 — ¥[总费用]花费零回报
原因分析:
  1. [价格因素分析]
  2. [流量质量分析]
  3. [着陆页/联盟链接分析]
优化措施:
  - [具体措施1]
  - [具体措施2]
  - [具体措施3]
```

---

#### 问题2: 广告结构失衡（Campaign/AG覆盖率不足）

**判定条件**: 实际有流量的广告组数量 < 广告方案中设计的广告组数量的50%

**严重程度**: P0（致命）

**分析维度**:
1. **哪些Campaign未启动**: 对比广告方案中的Campaign列表 vs 实际有流量的Campaign
2. **哪些AG未启动**: 对比广告方案中的AG列表 vs 实际有流量的AG
3. **预算集中在哪里**: 哪个AG/ Campaign 占用了绝大部分预算

**常见模式**:
- 模式A: 只投了Brand Campaign，Category和Pain Points完全未启动 → 漏掉最大流量池
- 模式B: 只投了1-2个AG，其他AG零流量 → 主题覆盖不足
- 模式C: 预算过度集中在单一AG → 风险集中

**优化建议模板**:
```
问题: 广告结构严重失衡
当前状态: 设计了[X]个AG，实际只运行了[Y]个
未启动的AG: [列表]
预算集中: [AG名称]占[Z]%
优化措施:
  - 立即启动[AG名称]（优先级最高）
  - 分配预算:[AG1]=¥X/天, [AG2]=¥Y/天
  - 从[过度集中的AG]转移[Z]%预算
```

---

#### 问题3: 广泛匹配触发大量无关词

**判定条件**: 搜索字词报告中，通过广泛匹配触发的无关词数量 ≥ 总触发词数量的30%

**严重程度**: P1（严重）

**无关词判定规则**:
1. **竞品品牌词**: 非己方品牌的品牌名（如广告是DryQ但触发了dyson、shark）
2. **不同产品类型**: 同一品类但不同产品形态（如吹风机触发了bonnet hair dryer、diffuser）
3. **不同品类**: 完全无关的品类（如吹风机触发了hair curlers tutorial）
4. **非目标语言**: 非英语搜索（如西班牙语、法语）
5. **教程/信息类**: tutorial、how to、without rollers等
6. **极低意图泛词**: amazon hair dryer、hair dryer heat等

**优化建议模板**:
```
问题: 广泛匹配触发大量无关词
无关词数量: [X]条，占总触发词的[Y]%
主要类别:
  - 竞品品牌词: [数量]条（如[dyson, shark, ...]）
  - 不同产品类型: [数量]条（如[bonnet, diffuser, ...]）
  - 非目标语言: [数量]条
  - 教程/信息类: [数量]条
优化措施:
  - 添加否定关键词: [具体列表]
  - 将[关键词]从广泛匹配改为词组匹配
  - 暂停触发大量无关词的[关键词]
```

---

#### 问题4: 零效果关键词

**判定条件**: 展示 > 0 但 点击 = 0，或展示 = 0 且状态为"已启用"

**严重程度**: P1（严重）

**分类**:
1. **有展示但零点击**: 关键词有曝光但无人点击 → 可能匹配类型太宽泛或文案不吸引
2. **零展示**: 关键词完全无曝光 → 搜索量极低或出价不足
3. **已暂停**: 系统已自动暂停（如"很少投放"）→ 搜索量不足

**优化建议模板**:
```
问题: 零效果关键词浪费
零点击关键词: [X]个
零展示关键词: [Y]个
已暂停关键词: [Z]个
优化措施:
  - 暂停零展示关键词:[列表]
  - 将有展示但零点击的关键词改为更精确的匹配类型
  - 删除搜索量不存在的[关键词]
```

---

#### 问题5: CPC异常（过高或过低）

**判定条件**: 
- CPC > 整体平均CPC × 1.5 且 0转化 → CPC过高
- CPC < 整体平均CPC × 0.3 且 有转化 → CPC可能过低（有优化空间）

**严重程度**: P1（严重）

**分析维度**:
1. **过高原因**: 
   - 竞争激烈（如Dyson相关词）
   - 匹配类型太宽泛（广泛匹配竞争激烈）
   - 品牌词但品牌知名度低（CPC反而高）
   
2. **过低原因**:
   - 搜索量极低
   - 匹配类型太精确（精确匹配竞争少）
   - 出价策略设置过低

**优化建议模板**:
```
问题: CPC异常
CPC过高的关键词:
  - [关键词]: CPC=[X], 平均=[Y], 倍数=[Z]x
  - 原因:[分析]
  - 建议:降低出价[A]%或改为[匹配类型]
CPC过低的关键词:
  - [关键词]: CPC=[X], 有转化=[是/否]
  - 建议:提高出价[B]%以获取更多流量
```

---

#### 问题6: 流量错配（搜索字词与广告组不匹配）

**判定条件**: 搜索字词触发的广告组与字词意图不匹配

**严重程度**: P1（严重）

**常见场景**:
- 品牌旗下其他产品词触发了当前产品广告组（如bellezza blowout brush触发了DryQ吹风机组）
- 竞品品牌词触发了己方品牌广告组
- 通用词触发了品牌广告组（说明品牌Campaign的否定词不足）

**优化建议模板**:
```
问题: 流量错配
错配案例:
  - [搜索字词]触发了[广告组]，但[原因说明]
优化措施:
  - 将[关键词]移至正确的[广告组]
  - 添加否定词:[具体列表]
  - 暂停[错配关键词]
```

---

#### 问题7: 未处理的高价值搜索字词

**判定条件**: 搜索字词报告中"已添加/已排除"="都没有"，但该字词有实际价值

**严重程度**: P2（中等）

**价值判定**:
- 有转化 → 必须添加为关键词
- 有点击且CTR > 整体CTR → 建议添加
- 搜索量大且与产品相关 → 建议添加

**优化建议模板**:
```
问题: 未处理的高价值搜索字词
高价值未添加字词:
  - [字词]: 点击=[X], 费用=[Y], 转化=[Z]
  - 建议:添加为[匹配类型]关键词到[广告组]
```

---

## 2. 优化建议生成规则

### 2.1 否定关键词生成规则

**规则1: 竞品品牌词 → 账户级否定**
- 从搜索字词中提取所有竞品品牌名
- 排除已添加为关键词的竞品词（如Dyson Alternative AG中的dyson词）
- 格式: `-[brand_name]`

**规则2: 不同产品类型 → 账户级否定**
- 识别与广告产品同品类但不同形态的词
- 例: 吹风机广告中，bonnet/diffuser/stick/reverse等是不同形态

**规则3: 非目标语言 → 账户级否定**
- 识别非英语搜索字词
- 常见: 西班牙语（secador, alisado）、法语、德语等

**规则4: 教程/信息类 → 账户级否定**
- tutorial、how to、without rollers、round brush tutorial等

**规则5: 品牌Campaign专用否定**
- 社区平台: -reddit、-quora、-forum
- 品牌旗下其他产品线

**规则6: Category/Pain Points Campaign否定**
- 二手/维修/配件: -used、-repair、-parts、-replacement
- 保留竞品对比词（vs、compare、alternative）

### 2.2 关键词调整规则

| 场景 | 操作 | 数据依据 |
|------|------|---------|
| 展示>0 且 点击=0 且 费用>0 | 暂停或改匹配类型 | 有曝光无点击 |
| 展示=0 且 状态=已启用 | 暂停 | 搜索量不存在 |
| 广泛匹配触发≥5条无关词 | 改为词组匹配 | 匹配质量差 |
| CPC > 均值×1.5 且 0转化 | 降低出价15-30% | 成本效率低 |
| CTR > 均值×2 且 有转化 | 提高出价10-20% | 效果优秀 |
| 搜索字词有价值但未添加 | 添加为关键词 | 新机会 |

### 2.3 出价策略调整规则

| 场景 | 当前策略 | 建议调整 |
|------|---------|---------|
| Brand Campaign 0转化 | Maximize Conversions | Maximize Clicks |
| Category Campaign 未启动 | — | 新建，Maximize Conversions |
| Pain Points Campaign 未启动 | — | 新建，Maximize Conversions |
| AG CPA > 盈亏平衡×150% | Maximize Conversions | 降低Target CPA 20%或暂停 |
| AG 转化率 > 3% 且 CPA < 盈亏平衡 | Maximize Conversions | 提高预算20% |

### 2.4 文案调整规则

**Brand Campaign文案调整**:
- 如果搜索字词中大量reviews/reddit词 → 强化社会证明
- 如果搜索字词中大量price/cheap词 → 弱化价格展示，强化价值
- 如果搜索字词中大量vs/compare词 → 强化差异化卖点

**Category Campaign文案调整**:
- 首次启动时沿用原方案
- 根据数据后续迭代

**Pain Points Campaign文案调整**:
- 首次启动时沿用原方案
- 根据数据后续迭代

---

## 3. 盈亏平衡分析

### 3.1 计算盈亏平衡CPA

```
盈亏平衡CPA = 产品佣金金额

例: 产品$339.99，佣金率22.5%
佣金 = $339.99 × 22.5% = $76.50
盈亏平衡CPA = $76.50（约¥555）
```

### 3.2 安全目标CPA

```
安全目标CPA = 盈亏平衡CPA × 安全系数
安全系数通常取 0.6-0.7

例: $76.50 × 0.65 = $49.73（约¥360）
```

### 3.3 当前CPA评估

```
当前CPA = 总费用 / 总转化

如果当前CPA > 盈亏平衡CPA → 亏损，需优化
如果当前CPA < 安全目标CPA → 盈利，可扩量
如果 0转化 → 无法计算CPA，需先解决转化问题
```

---

## 4. 竞品识别规则

### 4.1 从搜索字词中自动识别竞品

```python
def identify_competitors(search_terms, brand_name):
    """
    从搜索字词中识别竞品品牌
    """
    # 排除己方品牌和通用词后的高频词
    stop_words = ['hair', 'dryer', 'blow', 'best', 'for', 'the', 'a', 'and']
    
    # 提取所有搜索字词中的单词
    all_words = []
    for term in search_terms:
        words = term.lower().split()
        all_words.extend([w for w in words if w not in stop_words and len(w) > 2])
    
    # 统计词频
    from collections import Counter
    word_freq = Counter(all_words)
    
    # 排除己方品牌相关词
    brand_related = [brand_name.lower(), brand_name.lower().replace(' ', '')]
    
    # 高频词中可能是竞品的（出现次数≥2且非通用词）
    potential_competitors = []
    for word, count in word_freq.most_common(30):
        if count >= 2 and word not in brand_related and word not in stop_words:
            # 检查是否像品牌名（非英文单词）
            potential_competitors.append(word)
    
    return potential_competitors
```

### 4.2 常见品类竞品库（参考）

**吹风机品类**:
- 高端: dyson, shark, ghd, t3
- 中端: conair, revlon, remington, hot tools
- 专业线: parlux, elchim, rusk
- 新兴: laifen, jinri

**猫砂盆品类**:
- 高端: litter robot, meowant
- 中端: petsafe, catgenie
- 品牌: catlink（己方）

---

## 5. 用户意图相关诊断问题

### 问题8: 意图分布失衡（P1）

**判定条件**: 某一意图层级的点击占比 > 70% 或 < 5%

**严重程度**: P1（严重）

**分析维度**:
1. **过度集中**: 如果90%点击来自"产品调研"意图 → 说明只有Brand Campaign在运行
2. **完全缺失**: 如果"方案探索"意图点击为0 → 说明Category Campaign未启动
3. **意图错配**: 如果"购买决策"意图的CPC远高于"产品调研" → 出价策略可能有问题

**优化建议模板**:
```
问题: 意图分布失衡
当前分布:
  - 购买决策: [X]% ([N]次点击)
  - 产品调研: [X]% ([N]次点击)
  - 方案探索: [X]% ([N]次点击)
  - 问题认知: [X]% ([N]次点击)
  - 信息浏览: [X]% ([N]次点击)
问题:
  - [意图层级]过度集中/缺失，说明[Campaign/AG状态]
优化措施:
  - 启动[缺失的Campaign/AG]
  - 调整[过度集中的Campaign]预算分配
  - 为[意图层级]优化文案
```

### 问题9: 意图-广告错配（P1）

**判定条件**: 搜索字词的意图与触发的广告文案方向不匹配

**严重程度**: P1（严重）

**常见错配模式**:
1. **调研意图 + 硬卖广告**: 用户搜"reviews"但广告标题是"Buy Now"
2. **探索意图 + 品牌广告**: 用户搜"best"但广告只讲品牌名
3. **购买意图 + 调研广告**: 用户搜"buy"但广告在讲reviews

**优化建议模板**:
```
问题: 意图-广告错配
错配案例:
  - 搜索字词: [词] (意图: [意图])
  - 当前触发广告: [AG] — [标题]
  - 问题: 用户意图是[意图]，但广告传达的是[意图]
优化措施:
  - 为[AG]新增[意图导向]标题: [建议标题]
  - 调整[AG]描述，强化[意图]方向
```

### 问题10: 用户旅程断点（P1）

**判定条件**: 用户旅程中的某个阶段完全无广告覆盖

**严重程度**: P1（严重）

**分析维度**:
1. **Aware阶段缺失**: 用户有痛点但搜不到相关广告
2. **Explore阶段缺失**: 用户在比较方案但看不到品类广告
3. **Research阶段薄弱**: 用户在做功课但品牌广告不够强
4. **Purchase阶段缺失**: 用户决定买了但找不到购买入口

**优化建议模板**:
```
问题: 用户旅程断点
典型旅程: [Aware] → [Explore] → [Research] → [Purchase]
断点:
  - [阶段]: 用户搜[词]时无广告覆盖
  - 原因: [Campaign/AG未启动]
  - 影响: 漏掉[X]%的潜在用户
优化措施:
  - 启动[Campaign/AG]
  - 添加关键词: [列表]
  - 文案方向: [意图导向]
```

---

## 6. 输出格式

诊断分析完成后，输出结构化的问题列表：

```json
{
  "problems": [
    {
      "id": "P0-1",
      "level": "P0",
      "title": "零转化",
      "description": "¥556.44花费120次点击0转化",
      "evidence": {
        "total_cost": 556.44,
        "total_clicks": 120,
        "total_conversions": 0,
        "avg_cpc": 4.64
      },
      "root_causes": [
        "价格因素:$339.99高价与调研阶段用户不匹配",
        "流量质量:品牌词搜索量低",
        "联盟链接摩擦"
      ],
      "actions": [
        "启动Category Campaign获取高流量",
        "Brand Campaign改为Maximize Clicks",
        "监控Amazon页面转化率"
      ]
    },
    {
      "id": "P0-2",
      "level": "P0",
      "title": "广告结构失衡",
      ...
    }
  ],
  "negative_keywords": {
    "account_level": ["-dyson", "-shark", "-bonnet", ...],
    "campaign_level": {
      "Brand": ["-reddit", "-quora"],
      "Category": ["-used", "-repair"],
      "Pain Points": []
    }
  },
  "keyword_adjustments": {
    "pause": ["keyword1", "keyword2"],
    "tighten_match": [{"keyword": "kw1", "from": "广泛", "to": "词组"}],
    "lower_bid": [{"keyword": "kw1", "current_cpc": 8.22, "suggested_cpc": 6.58}],
    "raise_bid": [{"keyword": "kw2", "current_cpc": 2.0, "suggested_cpc": 2.4}],
    "add_new": [{"term": "new term", "match_type": "词组", "ad_group": "AG1"}]
  },
  "bidding_adjustments": {
    "Brand": {"from": "Maximize Conversions", "to": "Maximize Clicks"},
    "Category": {"action": "新建", "strategy": "Maximize Conversions", "budget": 20},
    "Pain Points": {"action": "新建", "strategy": "Maximize Conversions", "budget": 20}
  }
}
```
