# User Intent Analyzer Reference — Google Ads Optimization Skill

> 从用户搜索意图角度分析关键词和广告，理解"用户为什么搜这个词"以及"如何匹配用户意图"

---

## 1. 用户搜索意图分类模型

### 1.1 意图金字塔

```
                    ┌─────────────┐
                    │  购买决策    │  ← 最深层意图：已决定购买，在比较具体选项
                    │  (Purchase)  │     例："sri dryq vs dyson", "buy dryq now"
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │  产品调研    │  ← 深层意图：对某类产品感兴趣，在研究具体产品
                    │  (Research)  │     例："sri dryq reviews", "dryq hair dryer reddit"
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │  方案探索    │  ← 中层意图：有需求但不确定解决方案
                    │  (Explore)   │     例："best lightweight hair dryer", "quiet hair dryer 2025"
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │  问题认知    │  ← 浅层意图：意识到问题但还没开始找方案
                    │  (Aware)     │     例："why is my hair so frizzy", "tired of heavy hair dryer"
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │  信息浏览    │  ← 最浅层意图：无明确购买意图，纯浏览
                    │  (Browse)    │     例："hair dryer", "amazon hair dryer"
                    └─────────────┘
```

### 1.2 意图与转化概率（Affiliate场景）

| 意图层级 | 典型搜索词 | Amazon购买概率 | CPC范围 | 利润效率 | 广告策略 |
|----------|-----------|---------------|---------|---------|---------|
| 购买决策 | buy, order, purchase, deal, discount | 8-15% | 高($2-$5) | **中**（购买率高但CPC高） | 直接CTA，强调便捷购买 |
| 产品调研 | reviews, reddit, worth it, vs, compare | 3-8% | 中($1-$3) | **高**（购买率中等+CPC适中） | 社会证明，详细卖点，信任 |
| 方案探索 | best, top, 2025, lightweight, quiet | 1-3% | 低($0.5-$2) | **高**（购买率低但CPC极低=利润空间大） | 差异化卖点，教育引导 |
| 问题认知 | why, how to, tired of, fix | 0.5-1% | 低($0.3-$1) | **中**（转化链路长但成本低） | 痛点共鸣，解决方案 |
| 信息浏览 | amazon, cheap, generic category | 0.1-0.5% | 极低($0.1-$0.5) | **低**（几乎不购买） | 严格否定或最低出价 |

**Affiliate核心洞察**：
- 购买决策意图虽然购买概率最高，但CPC也最高，利润效率未必最佳
- **方案探索意图是Affiliate的隐藏金矿**：用户主动搜索"best XXX"→CPC通常很低→一旦购买佣金很高→利润效率最高
- 这就是为什么Category Campaign对Affiliate至关重要：它专门捕获方案探索意图
- 信息浏览意图对Affiliate是剧毒：几乎不购买，但会消耗预算→必须严格否定

---

## 2. 搜索字词意图分析方法

### 2.1 意图识别规则

#### 购买决策意图信号词

| 信号词 | 示例搜索词 | 用户心态 |
|--------|-----------|---------|
| buy/purchase/order | "buy sri dryq", "order dryq hair dryer" | 已决定购买，找购买入口 |
| deal/discount/sale | "sri dryq sale", "dryq discount code" | 想买但希望省钱 |
| price/cost/how much | "sri dryq price", "how much is dryq" | 预算确认，接近购买 |
| where to buy | "where to buy sri dryq", "dryq amazon" | 找购买渠道 |
| shipping/fast delivery | "sri dryq free shipping", "dryq next day delivery" | 购买决策后关注物流 |

#### 产品调研意图信号词

| 信号词 | 示例搜索词 | 用户心态 |
|--------|-----------|---------|
| reviews/rating | "sri dryq reviews", "dryq hair dryer rating" | 想确认产品质量 |
| reddit/forum | "sri dryq reddit", "dryq hair dryer forum" | 想看真实用户反馈 |
| worth it | "is sri dryq worth it", "dryq worth the money" | 价格敏感，在做价值判断 |
| vs/compare | "sri dryq vs dyson", "dryq compare to shark" | 在做竞品对比 |
| pros and cons | "sri dryq pros cons", "dryq hair dryer problems" | 想全面了解产品 |
| real/user experience | "sri dryq real review", "dryq user experience" | 不信任官方宣传 |

#### 方案探索意图信号词

| 信号词 | 示例搜索词 | 用户心态 |
|--------|-----------|---------|
| best/top | "best lightweight hair dryer", "top quiet hair dryer" | 想找最优解 |
| 2025/new | "best hair dryer 2025", "new hair dryer technology" | 想要最新最好的 |
| for [specific need] | "hair dryer for fine hair", "dryer for frizzy hair" | 有具体需求，在找匹配方案 |
| lightweight/quiet/fast | "lightweight professional hair dryer", "quietest hair dryer" | 有明确功能偏好 |
| premium/luxury/high end | "premium hair dryer", "luxury blow dryer" | 愿意为品质付费 |

#### 问题认知意图信号词

| 信号词 | 示例搜索词 | 用户心态 |
|--------|-----------|---------|
| why/tired of/sick of | "why is my hair frizzy", "tired of heavy hair dryer" | 意识到问题，开始找原因 |
| how to/fix/stop | "how to stop hair frizz", "fix damaged hair from heat" | 想解决问题 |
| damage/breakage/frizz | "hair dryer damage", "heat damage hair" | 有具体痛点 |

#### 信息浏览意图信号词

| 信号词 | 示例搜索词 | 用户心态 |
|--------|-----------|---------|
| amazon/website | "amazon hair dryer", "hair dryer amazon" | 无明确目标，浏览 |
| cheap/budget/under | "cheap hair dryer", "hair dryer under $50" | 价格导向，无品牌偏好 |
| generic category | "hair dryer", "blow dryer" | 极度宽泛，无明确意图 |

---

### 2.2 搜索字词意图标注方法

对搜索字词报告中的每条有效字词，标注其意图层级：

```python
def classify_intent(search_term):
    term_lower = search_term.lower()
    
    # 购买决策
    purchase_signals = ['buy', 'purchase', 'order', 'deal', 'discount', 'sale', 'price', 'cost', 'where to buy', 'shipping']
    if any(s in term_lower for s in purchase_signals):
        return "Purchase"
    
    # 产品调研
    research_signals = ['review', 'reddit', 'worth it', 'vs', 'compare', 'pros and cons', 'real', 'user experience', 'rating']
    if any(s in term_lower for s in research_signals):
        return "Research"
    
    # 方案探索
    explore_signals = ['best', 'top', '2025', 'new', 'for ', 'lightweight', 'quiet', 'fast', 'premium', 'luxury', 'high end']
    if any(s in term_lower for s in explore_signals):
        return "Explore"
    
    # 问题认知
    aware_signals = ['why', 'tired of', 'sick of', 'how to', 'fix', 'stop', 'damage', 'breakage', 'frizz']
    if any(s in term_lower for s in aware_signals):
        return "Aware"
    
    # 默认：信息浏览
    return "Browse"
```

### 2.3 意图分布分析

```markdown
### 搜索字词意图分布

| 意图层级 | 字词数 | 点击数 | 费用 | 转化 | 平均CPC | 转化概率 |
|----------|--------|--------|------|------|---------|---------|
| 购买决策 | [N] | [N] | ¥[X] | [N] | ¥[X] | [X]% |
| 产品调研 | [N] | [N] | ¥[X] | [N] | ¥[X] | [X]% |
| 方案探索 | [N] | [N] | ¥[X] | [N] | ¥[X] | [X]% |
| 问题认知 | [N] | [N] | ¥[X] | [N] | ¥[X] | [X]% |
| 信息浏览 | [N] | [N] | ¥[X] | [N] | ¥[X] | [X]% |
```

---

## 3. 用户意图与广告匹配分析

### 3.1 意图-广告匹配度检查

对每条高费用的搜索字词，检查当前触发的广告是否匹配用户意图：

| 搜索字词 | 用户意图 | 当前触发的广告组 | 匹配度 | 问题 |
|----------|---------|----------------|--------|------|
| "sri dryq reviews" | 产品调研 | Brand Core | ⚠️ 部分匹配 | 标题应强化reviews/社会证明 |
| "best lightweight hair dryer" | 方案探索 | Brand Core | ❌ 不匹配 | 应触发Category Campaign |
| "dyson hair dryer alternative" | 方案探索 | — | ❌ 未触发 | 应触发Pain Points AG3D |
| "buy sri dryq" | 购买决策 | Brand Core | ✅ 匹配 | 标题应强化购买CTA |

### 3.2 意图错配诊断

**错配类型1: 深层意图触发了浅层广告**
- 例：用户搜 "sri dryq reviews"（调研意图）→ 触发了Brand Core的硬卖广告
- 问题：广告标题是"SRILabs DryQ Hair Dryer"（产品名），没有回应reviews需求
- 优化：标题改为"DryQ Reviews — 4.6★ Rated"或"See Why 1,111+ Love DryQ"

**错配类型2: 浅层意图触发了深层广告**
- 例：用户搜 "hair dryer"（浏览意图）→ 触发了Brand Core的品牌广告
- 问题：用户还没听说过DryQ，品牌广告无效
- 优化：这类词应在Category Campaign中用教育型文案

**错配类型3: 意图对应的Campaign未启动**
- 例：用户搜 "best quiet hair dryer"（探索意图）→ 无Campaign可触发
- 问题：AG2B Quiet Hair Dryer未启动
- 优化：启动对应Campaign/AG

---

## 4. 基于用户意图的关键词优化

### 4.1 意图分层关键词策略

| 意图层级 | 关键词类型 | 匹配类型建议 | 出价策略 | 广告文案方向 |
|----------|-----------|-------------|---------|-------------|
| 购买决策 | 品牌名+buy/order/price | 精确/词组 | 最高出价 | 直接CTA，强调购买便捷 |
| 产品调研 | 品牌名+reviews/reddit/worth it | 词组 | 中高出价 | 社会证明，详细卖点，信任 |
| 方案探索 | best/top/品类特征词 | 词组/广泛 | 中等出价 | 差异化卖点，教育引导 |
| 问题认知 | why/how to/痛点词 | 广泛 | 低出价 | 痛点共鸣，解决方案 |
| 信息浏览 | 宽泛品类词 | 广泛 | 最低出价 | 品牌曝光，宽泛匹配 |

### 4.2 关键词-意图对齐检查

对现有关键词逐一检查：

```markdown
| 关键词 | 当前匹配类型 | 用户意图 | 意图匹配度 | 建议调整 |
|--------|------------|---------|-----------|---------|
| [sri dryq] | 精确 | 购买决策/调研 | ✅ 匹配 | 保持 |
| "sri dryq reviews" | 词组 | 产品调研 | ✅ 匹配 | 保持 |
| sri dry q hair dryer reviews | 广泛 | 产品调研 | ⚠️ 匹配但触发无关词 | 改为词组 |
| bellezza blowout brush | 广泛 | 购买决策(错配产品) | ❌ 错配 | 暂停 |
```

### 4.3 新增关键词建议（基于意图缺口）

根据搜索字词报告中高价值但未添加的字词，按意图层级补充：

```markdown
### 购买决策意图（高优先级添加）
| 搜索字词 | 点击 | 费用 | 建议关键词 | 匹配类型 | 目标AG |
|----------|------|------|-----------|---------|--------|
| [字词] | [N] | ¥[X] | [关键词] | 精确 | Brand Core |

### 产品调研意图（中优先级添加）
| 搜索字词 | 点击 | 费用 | 建议关键词 | 匹配类型 | 目标AG |
|----------|------|------|-----------|---------|--------|
| [字词] | [N] | ¥[X] | [关键词] | 词组 | Brand Long-tail |

### 方案探索意图（必须启动对应Campaign）
| 搜索字词 | 点击 | 费用 | 建议关键词 | 匹配类型 | 目标AG |
|----------|------|------|-----------|---------|--------|
| [字词] | [N] | ¥[X] | [关键词] | 词组 | Category/Pain Points |
```

---

## 5. 基于用户意图的广告文案优化

### 5.1 意图导向的文案框架

#### 购买决策意图文案

**用户心态**: "我已经决定买了，告诉我怎么买最方便、最划算"

**文案要素**:
- 直接购买CTA: "Buy Now", "Order Today", "Shop on Amazon"
- 价格透明度: "$339.99 — Free Shipping"
- 便捷性: "1-Click Order", "Prime Eligible"
- 稀缺性: "Limited Stock", "Only X Left"

**示例标题**:
```
1. Buy SRILabs DryQ — $339.99
2. Order DryQ on Amazon Today
3. SRILabs — Free Shipping Included
4. Get Your DryQ — Shop Now
5. SRILabs DryQ — In Stock & Ready
```

**示例描述**:
```
1. Ready to buy? SRILabs DryQ: $339.99 on Amazon. Free shipping. Easy 30-day returns.
2. Order SRILabs DryQ today. 4.6★ rated, 1,111+ reviews. Ships fast from Amazon.
```

---

#### 产品调研意图文案

**用户心态**: "我在研究这个产品，告诉我它真的好吗？值不值这个价？"

**文案要素**:
- 社会证明: "4.6★ from 1,111+ users", "Trusted by thousands"
- 第三方背书: "Amazon's Choice", "Highly Rated"
- 详细卖点: 具体技术参数、差异化功能
- 风险逆转: "Easy returns", "See reviews on Amazon"
- 避免硬卖CTA: 用 "Learn More", "See Reviews" 替代 "Buy Now"

**示例标题**:
```
1. DryQ Reviews — 4.6★ from 1,111+
2. See Why Users Love SRILabs DryQ
3. SRILabs — Worth Every Penny?
4. 1,111+ Reviews — Read Them Now
5. Is DryQ Worth $339? See Reviews
```

**示例描述**:
```
1. Wondering if DryQ is worth it? 4.6★ from 1,111+ real Amazon users. See reviews.
2. SRILabs DryQ: infrared + ionic + red light. Read 1,111+ reviews on Amazon.
3. Real users rate DryQ 4.6★. Ultra-lightweight, whisper-quiet. See why they love it.
```

---

#### 方案探索意图文案

**用户心态**: "我有这个需求，但不知道哪个产品最适合我，告诉我为什么选你"

**文案要素**:
- 差异化卖点: "Lighter than Dyson", "Quieter than competitors"
- 教育内容: 技术解释（ionic + infrared的好处）
- 场景匹配: "For fine hair", "For frizz control"
- 价值主张: 为什么值得$339（技术、品质、耐用性）
- 引导性CTA: "Discover DryQ", "Learn More", "Compare"

**示例标题**:
```
1. Best Lightweight Hair Dryer 2025
2. SRILabs — Lighter & Quieter
3. Why DryQ Beats Heavy Dryers
4. Ionic + Infrared — See The Difference
5. Premium Hair Dryer — From $339
```

**示例描述**:
```
1. Looking for the best lightweight hair dryer? SRILabs DryQ: under 1lb, salon power. Compare on Amazon.
2. SRILabs DryQ combines ionic + infrared + red light. Smoother, shinier hair. Learn more.
3. Tired of heavy, loud dryers? SRILabs is ultra-light & whisper-quiet. Discover the difference.
```

---

#### 问题认知意图文案

**用户心态**: "我有这个问题，你们能帮我解决吗？"

**文案要素**:
- 痛点共鸣: "Tired of frizz?", "Sick of heavy dryers?"
- 解决方案: "DryQ smooths frizz with ionic tech"
- 情感连接: "Your hair deserves better"
- 低压力CTA: "See How DryQ Helps", "Learn More"

**示例标题**:
```
1. Tired of Frizzy Hair?
2. Sick of Heavy Hair Dryers?
3. Stop Heat Damage — Try DryQ
4. Your Hair Deserves Better
5. Fix Frizz — See How DryQ Helps
```

**示例描述**:
```
1. Frizz driving you crazy? SRILabs DryQ's ionic + infrared tech targets frizz at the source.
2. Tired of arm fatigue? SRILabs DryQ weighs less than a pound. Powerful yet gentle.
```

---

### 5.2 文案-意图匹配度检查清单

对现有广告文案中的每条标题和描述，检查：

```markdown
| 标题/描述 | 目标意图 | 实际传达的意图 | 匹配度 | 建议调整 |
|-----------|---------|--------------|--------|---------|
| "SRILabs $339.99 — Free Ship" | 购买决策 | ✅ 购买决策 | 匹配 | 保持 |
| "Order Your DryQ Today" | 购买决策 | ✅ 购买决策 | 匹配 | 保持 |
| "SRILabs DryQ Hair Dryer" | 产品调研 | ⚠️ 信息展示 | 偏弱 | 加入社会证明 |
| "4.6★ Rated — Shop SRILabs" | 产品调研 | ✅ 调研+购买 | 匹配 | 保持 |
| "Your Best Blowout Awaits" | 方案探索 | ⚠️ 过于模糊 | 偏弱 | 加入具体卖点 |
```

---

## 6. 用户旅程映射

### 6.1 从搜索字词还原用户旅程

根据搜索字词报告，还原典型用户的搜索路径：

```markdown
### 典型用户旅程A：品牌认知型

Step 1 (Aware): "tired of heavy hair dryer" → 问题认知
  ↓ 看到广告 → 了解到DryQ是 lightweight 的
Step 2 (Explore): "best lightweight hair dryer" → 方案探索
  ↓ 看到广告 → 对比多个品牌
Step 3 (Research): "sri dryq reviews" → 产品调研
  ↓ 看到广告 → 查看reviews
Step 4 (Research): "sri dryq vs dyson" → 竞品对比
  ↓ 看到广告 → 确认DryQ的优势
Step 5 (Purchase): "buy sri dryq" → 购买决策
  ↓ 点击广告 → 完成购买

### 典型用户旅程B：品牌直接型

Step 1 (Research): "sri dryq hair dryer reviews" → 直接调研品牌
  ↓ 看到广告 → 查看reviews
Step 2 (Purchase): "sri dryq price" → 确认价格
  ↓ 点击广告 → 完成购买
```

### 6.2 旅程断点分析

检查用户旅程中是否存在断点：

```markdown
| 旅程阶段 | 当前覆盖 | 问题 | 优化建议 | Affiliate利润影响 |
|----------|---------|------|---------|------------------|
| 问题认知 | ❌ 无Campaign | 用户搜"tired of heavy dryer"看不到广告 | 启动Pain Points Campaign | 低CPC流量，利润效率中等 |
| 方案探索 | ❌ 无Campaign | 用户搜"best lightweight dryer"看不到广告 | 启动Category Campaign | **Affiliate金矿**：低CPC+高佣金=高利润 |
| 产品调研 | ✅ Brand Campaign | 但文案不够强 | 优化Brand文案，强化社会证明 | 核心利润来源，必须优化 |
| 购买决策 | ✅ Brand Campaign | 但CTA不够直接 | 增加购买导向标题 | 高CPC但高转化，控制成本 |
```

**Affiliate旅程断点特殊影响**：
- 方案探索阶段（Explore）是Affiliate最被低估的利润来源
- 用户搜"best lightweight hair dryer"时，如果看到你的广告并点击→即使当时没买→24小时Cookie已种下
- 这个用户之后回Amazon搜索并购买→你赚佣金
- 而Category Campaign的CPC通常只有Brand的30-50%
- **结论：Category Campaign对Affiliate的ROI往往高于Brand Campaign**

---

## 7. 意图驱动的否定关键词策略

### 7.1 按意图过滤否定词

不仅按"是否相关"判断，还要按"意图是否匹配"判断：

```markdown
| 搜索字词 | 表面相关 | 实际意图 | 意图匹配度 | 决策 |
|----------|---------|---------|-----------|------|
| "dyson hair dryer" | 相关（同品类） | 竞品品牌（购买Dyson） | ❌ 不匹配 | 否定 |
| "hair dryer tutorial" | 相关（同品类） | 学习使用（无购买意图） | ❌ 不匹配 | 否定 |
| "how to fix hair dryer" | 相关（同品类） | 维修（已有产品） | ❌ 不匹配 | 否定 |
| "sri dryq reviews reddit" | 相关（品牌词） | 调研（高价值） | ✅ 匹配 | 保留 |
| "best hair dryer under $50" | 相关（同品类） | 低价探索（预算不匹配） | ⚠️ 部分匹配 | 否定（价格错配） |
```

### 7.2 意图错配的否定词案例

**案例: $339吹风机 vs "cheap hair dryer"**
- 表面：都是hair dryer，相关
- 实际意图：用户预算<$50，与$339产品完全不匹配
- 决策：否定 `-cheap`, `-budget`, `-under $50`, `-affordable`

**案例: DryQ vs "bonnet hair dryer"**
- 表面：都是hair dryer，相关
- 实际意图：用户想要bonnet（hood dryer），不是手持吹风机
- 决策：否定 `-bonnet`, `-hood`

**案例: Affiliate专用 — "hair dryer tutorial"**
- 表面：都是hair dryer，相关
- 实际意图：用户想学习使用技巧，已有吹风机或不想买
- 对Affiliate影响：点击=纯亏损（不可能购买）
- 决策：否定 `-tutorial`, `-how to use`, `-instructions`

**案例: Affiliate专用 — "free hair dryer giveaway"**
- 表面：hair dryer相关
- 实际意图：用户想要免费产品
- 对Affiliate影响：剧毒流量，点击=纯亏损
- 决策：否定 `-free`, `-giveaway`, `-sample`, `-contest`

**案例: Affiliate专用 — "walmart hair dryer"**
- 表面：hair dryer相关
- 实际意图：用户想去Walmart购买
- 对Affiliate影响：用户不会在你的Amazon链接购买
- 决策：否定 `-walmart`, `-target`, `-best buy`, `-costco`

---

## 8. 输出格式

### 8.1 用户意图分析章节（插入优化报告第三章后）

```markdown
## 三、用户意图分析

### 3.1 搜索字词意图分布

| 意图层级 | 字词数 | 点击 | 费用 | 转化 | 平均CPC |
|----------|--------|------|------|------|---------|
| 购买决策 | [N] | [N] | ¥[X] | [N] | ¥[X] |
| 产品调研 | [N] | [N] | ¥[X] | [N] | ¥[X] |
| 方案探索 | [N] | [N] | ¥[X] | [N] | ¥[X] |
| 问题认知 | [N] | [N] | ¥[X] | [N] | ¥[X] |
| 信息浏览 | [N] | [N] | ¥[X] | [N] | ¥[X] |

**关键发现**:
- [X]%的点击来自[意图层级]意图，说明用户主要处于[阶段]
- [意图层级]意图的转化概率最高/最低，因为[原因]
- [意图层级]意图完全没有覆盖，漏掉了[用户群体]

### 3.2 用户旅程映射

**典型用户旅程**:
```
[Aware] → [Explore] → [Research] → [Purchase]
  ↓         ↓           ↓            ↓
[搜索词]   [搜索词]     [搜索词]      [搜索词]
  ↓         ↓           ↓            ↓
[当前覆盖?] [当前覆盖?]  [当前覆盖?]   [当前覆盖?]
```

**旅程断点**:
- [阶段]无覆盖：用户搜[词]时看不到广告
- [阶段]文案不匹配：用户搜[词]时看到[广告]，但[原因]

### 3.3 意图-广告匹配度检查

| 搜索字词 | 用户意图 | 当前触发广告 | 匹配度 | 问题 | 优化建议 |
|----------|---------|------------|--------|------|---------|
| [词] | [意图] | [AG] | [匹配度] | [问题] | [建议] |

### 3.4 基于意图的文案优化建议

**[意图层级]意图文案优化**:

当前文案问题:
- [标题] → 传达的是[意图]，但用户实际意图是[意图]

建议调整:
- 原标题: [旧]
- 新标题: [新]（更符合[意图]意图）
- 原因: [解释]
```

### 8.2 意图驱动的关键词调整表

```markdown
| 关键词 | 当前匹配 | 用户意图 | 意图匹配度 | 建议调整 | 目标AG |
|--------|---------|---------|-----------|---------|--------|
| [kw] | [类型] | [意图] | [度] | [调整] | [AG] |
```

---

## 9. 质量检查清单（用户意图专项）

### OPT-QC-7: 用户意图分析完整性
- [ ] 搜索字词已按意图层级分类
- [ ] 意图分布统计已完成
- [ ] 典型用户旅程已还原
- [ ] 旅程断点已识别

### OPT-QC-8: 意图-广告匹配度
- [ ] 高费用搜索字词的意图已识别
- [ ] 当前触发的广告与意图匹配度已评估
- [ ] 意图错配案例已列出

### OPT-QC-9: 意图导向优化建议
- [ ] 每个意图层级有对应的文案优化建议
- [ ] 新增关键词建议按意图层级分类
- [ ] 否定关键词决策包含意图分析
