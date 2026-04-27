# User Intent Analyzer — Google Ads Optimization Skill v3.0

> 用户搜索意图分类参考 + 意图导向文案规则
>
> **v3.0定位**：本文件是意图分析的快速参考手册。
> 完整的意图分析流程（搜索词标注、意图分布统计、旅程断点分析）已迁移到诊断技能。
> 本文件保留意图分类体系和文案匹配规则，供优化技能在Step 6.6（文案调整）时查阅。

---

## 1. 意图金字塔

```
                ┌─────────────┐
                │  购买决策    │  ← 最深层：已决定购买，在比较具体选项
                │  (Purchase)  │     例："sri dryq vs dyson", "buy dryq now"
                └──────┬──────┘
                       │
                ┌──────┴──────┐
                │  产品调研    │  ← 深层：对某类产品感兴趣，在研究
                │  (Research)  │     例："sri dryq reviews", "dryq reddit"
                └──────┬──────┘
                       │
                ┌──────┴──────┐
                │  方案探索    │  ← 中层：有需求但不确定方案
                │  (Explore)   │     例："best lightweight hair dryer"
                └──────┬──────┘
                       │
                ┌──────┴──────┐
                │  问题认知    │  ← 浅层：意识到问题但还没找方案
                │  (Aware)     │     例："tired of heavy hair dryer"
                └──────┬──────┘
                       │
                ┌──────┴──────┐
                │  信息浏览    │  ← 最浅层：无明确购买意图
                │  (Browse)    │     例："hair dryer", "amazon hair dryer"
                └─────────────┘
```

---

## 2. 意图与Affiliate利润效率

| 意图层级 | 购买概率 | CPC范围 | 利润效率 | 广告策略 |
|----------|---------|---------|---------|---------|
| 购买决策 | 8-15% | 高($2-$5) | **中** | 直接CTA |
| 产品调研 | 3-8% | 中($1-$3) | **高** | 社会证明 |
| 方案探索 | 1-3% | 低($0.5-$2) | **高（Affiliate金矿）** | 差异化卖点 |
| 问题认知 | 0.5-1% | 低($0.3-$1) | **中** | 痛点共鸣 |
| 信息浏览 | 0.1-0.5% | 极低 | **低（剧毒）** | 严格否定 |

**Affiliate核心洞察**：方案探索意图是Affiliate的隐藏金矿——CPC通常很低，但一旦购买佣金很高，利润效率最高。Category Campaign专门捕获这类意图。

---

## 3. 意图信号词速查

### 购买决策
buy, purchase, order, deal, discount, sale, price, cost, where to buy, shipping, free shipping

### 产品调研
reviews, rating, reddit, forum, worth it, vs, compare, pros and cons, real, user experience

### 方案探索
best, top, 2025, new, for [need], lightweight, quiet, fast, premium, luxury, high end

### 问题认知
why, tired of, sick of, how to, fix, stop, damage, breakage, frizz

### 信息浏览
amazon, website, cheap, budget, under, generic category name

---

## 4. 意图导向文案规则

当优化技能在Step 6.6需要调整文案时，按意图层级匹配：

### 购买决策意图
**用户心态**: "已经决定买了，告诉我怎么买最方便"
- 标题要素: 直接CTA（Buy Now, Order Today, Shop on Amazon）
- 描述要素: 价格透明 + 便捷性（Prime, Free Shipping）
- CTA: "Buy Now", "Shop on Amazon", "Order Today"

### 产品调研意图
**用户心态**: "我在研究这个产品，它好吗？值不值？"
- 标题要素: 社会证明（4.6★ from 1,111+, Trusted by）
- 描述要素: 详细卖点 + 第三方背书
- CTA: "See Reviews", "Learn More"（非硬卖）
- **Affiliate注意**: 避免硬卖CTA，用"See Why on Amazon"

### 方案探索意图
**用户心态**: "我有需求，哪个最适合我？"
- 标题要素: 差异化卖点（Lighter than, Quieter than）
- 描述要素: 教育内容 + 场景匹配
- CTA: "Discover", "View on Amazon", "Compare"
- **Affiliate注意**: 这是利润效率最高的意图，文案必须强调差异化

### 问题认知意图
**用户心态**: "我有这个问题，你们能帮我吗？"
- 标题要素: 痛点共鸣（Tired of? Sick of?）
- 描述要素: 解决方案（非产品参数）
- CTA: "See How It Helps", "Find Your Solution"
- **Affiliate注意**: 低压力CTA，转化链路长但CPC低

### 信息浏览意图
- **Affiliate专用**: 严格否定（-hair dryer, -amazon hair dryer, -cheap hair dryer）
- 这类流量几乎不购买，每个点击都是纯亏损

---

## 5. 文案合规替换规则（基于诊断P1-9）

当诊断报告发现文案存在绝对化措辞时，按以下规则替换：

| 禁止措辞 | 替换为 | 适用场景 |
|---------|--------|---------|
| Stop [问题] | Targets / Fights / Reduces [问题] | 标题 |
| No More [问题] | Less [问题] / Smoother / Clearer | 标题 |
| Eliminate [问题] | Minimize / Reduce [问题] | 标题 |
| Cut [时间] in Half | Faster / In Less Time | 标题 |
| Instant / Miracle | Quick / Fast / Rapid | 标题 |
| Guarantee / 100% | Customer-Rated / Trusted | 标题/描述 |

---

## 6. Affiliate专用否定词（按意图过滤）

除常规否定词外，以下意图类否定词必须检查：

**免费/赠送**: -free, -giveaway, -sample, -contest
**教程/DIY**: -tutorial, -how to make, -diy, -instructions
**竞品平台**: -walmart, -target, -best buy, -costco, -ebay
**二手/翻新**: -used, -refurbished, -open box
**维修/配件**: -repair, -fix, -parts, -replacement parts
**助眠/噪音**: -asmr, -white noise, -sound machine
**低价探索**: -cheap, -budget, -under $50（当产品售价>$100时）
