# Optimization Analyzer Reference — Google Ads Optimization Skill v3.0

> 诊断结论→优化行动映射规则 + 清洁版广告调整方法
>
> **v3.0定位**：本文件是优化技能的核心执行手册。
> 诊断技能（google-ads-diagnosis）负责"发现问题"，本文件负责"解决问题"。
> 本文件不包含P0/P1/P2检测规则（已迁移到诊断技能），只包含优化行动映射。

---

## 优化铁律

1. CPC超过盈亏平衡的关键词 = **必须暂停**（无论CTR多高）
2. 广泛匹配触发无关词 = **必须收紧**（Affiliate没有预算浪费在测试上）
3. 未启动的Category/Pain Points Campaign = **必须启动**（这是利润最高的流量）
4. Amazon产品不可用时 = **必须暂停**（点击=纯亏）

---

## 诊断结论→优化行动映射规则

> 以下规则定义了如何将诊断技能的输出转化为清洁版广告文件中的具体变更。
> 诊断技能产出"问题清单"，本文件定义"如何修复这些问题"。
> 诊断报告中的P0/P1/P2编号以诊断报告为准。

### 映射总表

| 诊断问题 | 优化行动类型 | 清洁版变更位置 | 变更方式 |
|---------|------------|--------------|---------|
| P0-1 CPC超过盈亏平衡 | 暂停关键词 + 降出价 | Keywords区域 | ⛔暂停注释 |
| P0-2 Amazon产品不可用 | 暂停所有Campaign | 文件头部注释 | ⚠️全局暂停标记 |
| P0-3 广告结构严重失衡 | 新增/启动Campaign/AG | 新增Campaign/AG区块 | ✅新增AG |
| P0-4 零否定操作 | 批量否定词 | Negative Keywords区域 | 追加否定词 |
| P0-5 竞品流量截获 | 竞品否定词 | Account-Level Negative | 追加否定词 |
| P1-1 广泛匹配触发无关词 | 匹配类型修改 + 否定词 | Keywords + Negative区域 | 🔄改匹配 |
| P1-2 零效果关键词 | 暂停关键词 | Keywords区域 | ⛔暂停注释 |
| P1-3 CPC异常偏高 | 降低出价/改匹配 | Bidding Strategy区域 | 📉降出价 |
| P1-4 非购买意图词 | 添加否定词 | Account/Campaign Negative | 追加否定词 |
| P1-5 意图分布失衡 | 调整Campaign预算 | Bidding Strategy区域 | 预算调整 |
| P1-6 意图-广告错配 | 调整文案匹配意图 | Headlines/Descriptions | 🔄替换文案 |
| P1-7 用户旅程断点 | 启动缺失Campaign/AG | 新增Campaign/AG区块 | ✅新增AG |
| P1-8 Amazon产品页面风险 | 收紧CPC上限 | Bidding Strategy区域 | Max CPC调整 |
| P1-9 文案绝对承诺 | 替换绝对化措辞 | Headlines/Descriptions | 🔄替换文案 |
| P1-10 跨AG标题重复 | 差异化CTA标题 | Headlines区域 | 🔄替换标题 |
| P2-1 匹配类型集中 | 渐进收紧 | Keywords区域 | 逐步改匹配类型 |
| P2-2 否定词覆盖不足 | 批量否定词 | Negative Keywords区域 | 追加否定词 |
| P2-3 AG内意图混杂 | AG拆分/重组 | Campaign/AG区块 | 重组关键词归属 |
| P2-4 文案未标注意图 | 意图标注 | Headlines/Descriptions注释 | 添加意图注释 |
| P2-5 描述字符超限 | 描述精简 | Descriptions区域 | 替换描述 |
| P2-6 缺乏风险逆转 | 新增风险逆转文案 | Headlines/Descriptions | 新增/替换 |

### 变更注释格式规范

在清洁版广告文件中，所有变更必须使用标准化注释：

```markdown
# 关键词变更注释
[kw]  # ⛔ 暂停: CPC $4.50 > 盈亏平衡 $3.83 | 已消耗 ¥258
[kw]  # ✅ 新增 | 利润评级: A | 来源: "搜索词报告 TOP10"
[kw]  # 🔄 改匹配: +kw → "kw" | 原因: 广泛匹配触发80+无关词
[kw]  # 📉 降出价: Max CPC $2.00 → $1.50 | 原因: CPC接近安全上限

# 文案变更注释
[标题]  # 🔄 修改: "Stop Frizz Fast" → "Targets Frizz" | P1-9 合规
[描述]  # 🔄 修改: 原描述→新描述 | 意图错配: 调研意图用户看到硬卖文案

# 结构变更注释
## Ad Group X: New AG Name  # ✅ 新增AG | 诊断P0-3: 填补旅程断点
```

### 文案替换规则（基于诊断发现的意图错配）

当诊断报告发现意图-广告错配（P1-6）时，按以下规则替换文案：

**规则1：调研意图词 → 社会证明型文案**
```
触发条件：用户搜 reviews/reddit/does it work
文案调整：
  - 标题加入评分/评论数（如 "4.6★ Rated [Product]"）
  - 描述加入社会证明（如 "Trusted by 1,000+ customers"）
  - CTA改为 "See Why on Amazon" 而非 "Buy Now"
```

**规则2：探索意图词 → 差异化卖点型文案**
```
触发条件：用户搜 best/top/compare
文案调整：
  - 标题强调差异化卖点（如 "Lighter Than Most Hair Dryers"）
  - 描述列出Top 3差异化点
  - CTA改为 "View on Amazon"
```

**规则3：痛点意图词 → 痛点共鸣型文案**
```
触发条件：用户搜 tired of/sick of/fix/help with
文案调整：
  - 标题直接回应痛点（如 "Tired of Heavy Dryers?"）
  - 描述强调解决方案而非产品参数
  - CTA改为 "Find Your Solution on Amazon"
```

### 文案合规替换规则（基于P1-9绝对承诺诊断）

当诊断报告发现文案存在绝对化措辞（P1-9）时，按以下规则替换：

| 禁止措辞 | 替换为 | 适用场景 |
|---------|--------|---------|
| Stop [问题] | Targets / Fights / Reduces [问题] | 标题 |
| No More [问题] | Less [问题] / Smoother / Clearer | 标题 |
| Eliminate [问题] | Minimize / Reduce [问题] | 标题 |
| Cut [时间] in Half | Faster / In Less Time | 标题 |
| Instant / Miracle | Quick / Fast / Rapid | 标题 |
| Guarantee / 100% | Customer-Rated / Trusted | 标题/描述 |

### 出价策略调整规则（基于利润模型）

```
IF 利润评级 = D（亏损）:
  → 暂停该关键词或AG
  
IF 利润评级 = C（边缘）:
  → 降低出价至安全上限 × 0.8
  → 收紧匹配类型
  → 添加否定词减少无效点击
  
IF 利润评级 = B（盈利）:
  → 维持当前出价
  → 关注CPC趋势
  
IF 利润评级 = A（高利润）:
  → 考虑提高出价10-20%
  → 扩展相关关键词
```

### Affiliate专用否定词速查

以下是必须检查的否定词类别，诊断报告如发现相关搜索词应直接添加：

**免费/赠送类**: -free, -giveaway, -sample, -contest, -sweepstakes
**教程/DIY类**: -tutorial, -how to make, -diy, -manual, -instructions, -blowout tutorial, -how to blow dry
**竞品平台类**: -walmart, -target, -best buy, -costco, -ebay
**二手/翻新类**: -used, -refurbished, -open box, -second hand
**维修/配件类**: -repair, -fix, -parts, -replacement parts, -accessories
**助眠/噪音类**: -asmr, -white noise, -sound machine, -binaural, -no ads
**多语言排除**: 语言设置应限定English（Campaign级），但以下否定词作为兜底：
  -secador de pelo, -ヘア ドライヤー, -sèche-cheveux（常见非英语搜索词）

### 新增关键词选择规则

从诊断报告的搜索字词分析中提取高价值词时，按以下优先级：

```
优先级1（P0）：购买决策意图 + CPC < 安全上限×0.5 → 精确匹配，加码出价
优先级2（P1）：方案探索意图 + CPC < 安全上限 → 词组匹配，正常出价
优先级3（P2）：调研意图 + CPC < 安全上限 → 词组匹配，正常出价
优先级4（P3）：痛点认知意图 + CPC < 安全上限×0.7 → 词组匹配，保守出价
不添加：信息浏览意图（如 "hair dryer", "amazon hair dryer"）
```
