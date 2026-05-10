---
name: obsidian-vault-manager
description: Use when the user asks to 收录到知识库 / 存到 Obsidian / 整理笔记 / 知识库检索 / Wiki Link / 双向链接 / 加链接 / 补链接 / PARA / 翻一下推特公众号 / 个人经验 / 概念提炼 / 草稿评审. Manages Obsidian vault with 4+1 layer info-stratification + PARA + P.A.I.R + Cold Storage + auto wikilink discovery + concept extraction pipeline + alias/stub repair. Unified entry for capturing, retrieving, organizing, refining.
version: 3.2
license: MIT
---

# Obsidian Vault Manager — 知识库管理 Skill (v3.2)

> Obsidian 知识库的收录、检索、整理、Wiki Link 自动维护、概念提炼的统一入口。
> 架构哲学:**4+1 层信息分层 + PARA + P.A.I.R + 冷藏区隔离 + 命名前缀规范 + 高频场景路由 + Wiki Link 自动识别 + 概念提炼管道 + 别名/桩链修复 + 人工编辑保护**。
> v3.2 关键升级:**融合 kytmanov olw(obsidian-llm-wiki-local)的概念提炼管道思想——新增 5 步概念萃取 SOP(Ingest→Validate→Match→Draft→Review)、Aliases 别名机制、Stubs 桩节点机制、Rejection 反馈闭环、Hand-edit Protection 人工编辑保护原则、frontmatter 新增 status/confidence/aliases/rejected_drafts 字段**。
> v3.1 关键升级:**新增 Wiki Link 自动识别 SOP(8 维实体扫描 + 7 步流程),新建/更新文件后强制扫描候选 wikilink,自动建立向上/横向/向下三层链接**。
> v3.0 关键升级:**引入 4+1 层信息分层模型(基于余一框架)、新增个人经验层 L1 目录、新增高频场景路由表、frontmatter 强制 layer 字段**。
>
> 思想根基详见同目录 [PHILOSOPHY.md](./PHILOSOPHY.md)。本文写**怎么做**,PHILOSOPHY 写**为什么**。

## 触发时机

- 用户说"收录到知识库"、"存到 Obsidian"、"入库"
- 用户提问时需要检索知识库
- 整理/归类/移动知识库文件
- 补充或维护 Wiki Link、双向链接、加链接、补链接(v3.1 自动触发)
- 创建任何新笔记(v3.1 强制扫描 wikilink)
- 创建新笔记或更新 MOC 索引
- 从冷藏区淘金提炼
- 记录"个人经验"(原话/亲历/感受/判断)→ 走 L1 流程

## Vault 路径

`<vault-root>/`

> **使用前请把上方 `<vault-root>/` 占位符改成你自己的 Obsidian Vault 实际路径**(例如 `/Users/<你的用户名>/Documents/MyVault/`)。

---

## 信息分层模型(4+1 层 · v3 核心)

> 基于余一公开分享的 4 层分类法 + 本 Skill 第 5 层冷藏的扩展。完整思想见 [PHILOSOPHY.md](./PHILOSOPHY.md#二4-1-层信息分层模型本-skill-的核心)。

| 层 | 名称 | 定义 | 物理目录 | frontmatter |
|---|------|-----|---------|-------------|
| **L1** | 个人经验层 | 用户原话、亲历、一手感受、带个体烙印的判断 | `02-Areas/个人经验/` | `layer: experience` |
| **L2** | 在场素材层 | 用户在场的录音、会议、对话原文 | `07-Archive/播客转录/`、`02-Areas/<your-recordings>/` | `layer: scene` |
| **L3** | 外部入口层 | 收件箱、未分类、未消化原料 | `00-Inbox/` | `layer: inbox` |
| **L4** | 萃取成品层 | 加工过的成品(SOP / 概念 / 方法论) | `02-Areas/`、`04-Concepts/`、`05-Playbooks/`、`06-Library/`、`07-Archive/已结案项目/` | `layer: extract` |
| **L5** | 冷藏原料层 | 不删除但不主动检索的低频原料 | `99-冷藏/` | `layer: cold` |

### 关键判断:L1 vs L4

| | L1 个人经验 | L4 萃取成品 |
|--|------------|-----------|
| 加工度 | 原汁原味 | 高度抽象 |
| 时态 | "那一刻"的感受 | "下次该怎么做" |
| 文体 | 散文/碎片/独白 | SOP/Checklist |
| 目录 | `02-Areas/个人经验/` | `05-Playbooks/<your-domain>/` |

判断标准:**这一篇是事件本身的原始记录,还是为了萃取出可复用的 SOP?** 前者 → L1,后者 → L4。

---

## 目录结构与路由表

### 主区(默认检索)

| 目录 | 定位 | 收录什么 |
|------|------|---------|
| `!_MOC/` | 导航中心 | Vault 总入口、画像、知识库总览(`!` 排序置顶) |
| `00-Inbox/` | 收件箱(L3) | 不确定分类的新内容先扔这里 |
| `01-Projects/` | 当前项目(L4) | 有明确交付物的进行中项目 |
| `02-Areas/<领域>/` | 长期领域(L4) | 按领域分子目录(用户自定义) |
| `02-Areas/个人经验/` | **L1 个人经验层(v3 新增)** | 用户原话/亲历/一手感受;含 原话与决策 / 感受与判断 / 亲历事件 三个子目录 |
| `02-Areas/<knowledge-meta>/` | **元方法论(v3 新增)** | 关于知识库本身的方法论(分层模型、命名规范) |
| `03-People/同行案例/` | 人物库 | 同行深度拆解 |
| `03-People/客户档案/` | 人物库 | 具体客户项目记录 |
| `03-People/思想导师/` | 人物库 | 导师索引 |
| `03-People/合作伙伴/` | 人物库 | 合作方档案 |
| `03-People/团队沟通/` | 人物库 | 内部沟通、活动复盘 |
| `04-Concepts/` | 概念枢纽(L4) | 跨域核心概念(双向链接枢纽,命名带前缀) |
| `05-Playbooks/` | SOP/方法论(L4) | 各类可复用方法论 |
| `06-Library/工具清单/` | 资源库(L4) | 工具指南、Skill 资产台账 |
| `06-Library/失败案例/` | 资源库 | 失败案例复盘 |
| `06-Library/书籍笔记/` | 资源库 | 书籍读书笔记 |
| `06-Library/论文笔记/` | 资源库 | 论文笔记(含科研类) |
| `06-Library/网络基建/` | 资源库 | 网络/服务器/基建笔记 |
| `06-Library/课程笔记/` | 资源库 | 课程笔记、播客转录精选 |
| `07-Archive/已结案项目/` | 归档 | 完成的项目记录 |
| `07-Archive/播客转录/` | 归档(高价值 · L2) | 用户在场播客原文 |
| `07-Archive/论文原文/` | 归档(高价值) | 论文原文存档 |

### 个人经验层(L1)细分路由(v3 新增)

| 子目录 | 准入规则 | frontmatter event_type |
|--------|---------|----------------------|
| `02-Areas/个人经验/原话与决策/` | 用户亲口/亲手说出的话(朋友圈、对话、断言、表态、重大决策原话) | `原话` |
| `02-Areas/个人经验/感受与判断/` | 旁观者观察到的用户情绪反应、直觉判断、价值观显形 | `感受` |
| `02-Areas/个人经验/亲历事件/` | 现场记录的事件(客户对话、关键节点、当下感受),不带 SOP 萃取意图 | `亲历` |

**触发关键词**(任一命中走 L1):
- "我当时说……"、"我朋友圈写了"、"我刚跟 X 说……" → `原话与决策/`
- "我感觉……"、"我直觉判断"、"我对这个人的看法" → `感受与判断/`
- "今天发生了……"、"现场记录"、"那一刻"、"留个底" → `亲历事件/`

### 冷藏区(默认不检索)

| 目录 | 定位 | 收录什么 |
|------|------|---------|
| `99-冷藏/推特/` | 低频原料(L5) | 推特/X 自动抓取目标 |
| `99-冷藏/公众号博主库/` | 低频原料(L5) | 公众号 RSS 自动抓取目标 |
| `99-冷藏/朋友圈素材/` | 低频原料(L5) | 社交媒体素材 |
| `99-冷藏/活动文案/` | 低频原料(L5) | 活动文案、广告文案 |

> 高价值原料(精读公众号文章、精选论文、精选播客)保留在主区 `07-Archive/`,**不并入冷藏**。
> 自动抓取的批量原文写入 `99-冷藏/`,与精读层物理隔离。

### 系统区(隐藏)

| 目录 | 定位 |
|------|------|
| `_system/_log/` | AI 助手运行日志 |
| `Assets/` | Obsidian 标准资产 |
| `Excalidraw/` | Excalidraw 插件目录 |
| `.obsidian/` `.smart-env/` | Obsidian 配置文件 |

---

## 高频场景路由表(v3 新增)

> 高频场景 → 应该去哪个目录找/存。AI 助手按这张表自动路由。

| 场景关键词 | 内容类型 | 候选目录(优先级) |
|-----------|---------|-----------------|
| **客户/商务/定价/合同** | 客户档案、报价单、商务记录 | `03-People/客户档案/` → `05-Playbooks/<sales-sop>/` → `01-Projects/<active-engagement>/` |
| **内容创作/选题/标题/文案** | 写作素材、选题灵感、文案模板 | `02-Areas/内容创作/` → `05-Playbooks/<content-sop>/` → `04-Concepts/[方法论] <写作主题>` |
| **产品/程序开发/代码** | 项目代码、技术决策、架构 | `01-Projects/<project-name>/` → `02-Areas/<engineering-domain>/` → `06-Library/工具清单/` |
| **AI / Agent 理论** | Agent 架构、Prompt 工程、LLM 原理 | `04-Concepts/[理论] <主题>` → `02-Areas/<ai-domain>/` |
| **AI 行业动态/新闻** | 模型发布、行业趋势、竞品 | `02-Areas/<ai-industry>/` → `99-冷藏/公众号博主库/` |
| **创业/团队/战略决策** | 创业心得、团队管理、战略思考 | `05-Playbooks/<startup-sop>/` → `02-Areas/<strategy>/` |
| **个人原话/复盘/感受** | 自己说的话、自己的感受判断 | `02-Areas/个人经验/<原话与决策\|感受与判断\|亲历事件>/` → `05-Playbooks/<diary>/`(萃取后) |
| **工具/网络/基建** | CLI 工具、VPN、服务器配置 | `02-Areas/<infra>/` → `06-Library/工具清单/` → `06-Library/网络基建/` |
| **同行/导师/案例研究** | 行业同行研究、思想导师拆解 | `03-People/同行案例/` → `03-People/思想导师/` → `06-Library/失败案例/` |
| **概念查询/术语定义** | 名词解释、概念定义 | `04-Concepts/[理论] <概念名>` |
| **失败案例/踩坑** | 自己/他人的失败教训 | `06-Library/失败案例/` |
| **课程/读书/论文** | 学习材料笔记 | `06-Library/课程笔记/` → `06-Library/书籍笔记/` → `06-Library/论文笔记/` |

> 说明:`<占位符>` 是用户自定义的领域名,按自己 vault 实际命名替换。

---

## 核心原则

### 1. 先收再理(P.A.I.R)
- 分类不确定 → 先扔 `00-Inbox/`(L3)
- 定期整理 Inbox → 归位

### 2. 原料与成品分离(PARA + 4+1 层)
- **原料层**(L1 / L2 / L5):保留原汁原味,不萃取
- **成品层**(L4):经过消化提炼的高密度知识
- **缓冲层**(L3):待分类
- **L1 vs L4 物理隔离**:个人经验绝不混入 SOP 流程

### 3. 项目有终点,领域没终点
- `01-Projects/` 完成后 → 方法论提炼到 `05-Playbooks/`,记录归档到 `07-Archive/已结案项目/`
- `02-Areas/` 只做加法,持续积累

### 4. 原文神圣不可侵犯
- 收录原文一字不改
- AI 助手分析追加在原文之后的独立章节
- 朋友圈素材只读不写,序号不动

### 5. 复利优先
- 每次入库都要思考:这个知识能不能复用?
- 能提炼为 SOP/方法论 → 同步写入 `05-Playbooks/`
- 能抽象为概念 → 同步创建/更新 `04-Concepts/` 节点

### 6. 冷藏不删除
- 99-冷藏区是隔离不是丢弃
- 冷藏区文件原文一字不改
- 高价值发现 → 复制(不剪切)到主区,原文件加 `cherry_picked: true`

---

## 命名规范

### 04-Concepts 节点命名

格式:`[前缀] 主题名.md`(全简体中文 + 必带前缀)

| 前缀 | 含义 | 示例 |
|------|------|------|
| `[MOC]` | 索引页 / 多文件汇总 | `[MOC] 用户分层与定价体系.md` |
| `[理论]` | 概念定义 / 原理 | `[理论] 知识库与RAG完全指南.md` |
| `[工程]` | 具体实现 / 技巧 | `[工程] 稀缺性推理在上下文工程中的应用.md` |
| `[方法论]` | SOP 之上的元方法论 | `[方法论] 知识库管理.md` |

**冗余词黑名单**(不得用于节点名):调研、报告、深度、完全、详解、全景

**英文节点名**:除专有名词(NVIDIA、API、Harness、Hermes、Karpathy 等)外,全部翻译成中文。专有名词附中文说明。

### 02-Areas 文件命名

- 全中文,不带前缀(前缀仅用于 04-Concepts)
- 日期类用 `YYYY-MM-DD-主题.md`

### Frontmatter 强制字段(v3.2 升级)

```yaml
---
tags: [主标签, 辅助标签]
created: YYYY-MM-DD
source: <URL 或来源说明>
author: <作者>
layer: extract  # v3 必填:experience / scene / inbox / extract / cold,详见信息分层模型
status: draft   # v3.2 新增:stub / draft / published(仅 L4 萃取层使用)
confidence: 0.6 # v3.2 新增:0.0-1.0 概念置信度(仅 L4 萃取层使用)
aliases: []     # v3.2 新增:别名列表,用于 wikilink 别名匹配(详见 Aliases 机制)
rejected_drafts: []  # v3.2 新增:被人工驳回的 AI 草稿哈希(详见 Rejection Feedback)
cherry_picked: false # 仅冷藏区淘金后才设 true
---
```

**layer 字段速查**:

- `experience`:用户原话/亲历/一手感受(L1,放 `02-Areas/个人经验/`)
- `scene`:在场素材原文(L2,放 `07-Archive/播客转录/` 等)
- `inbox`:收件箱待分类(L3,放 `00-Inbox/`)
- `extract`:萃取成品(L4,放 `02-Areas/04-Concepts/05-Playbooks/06-Library`)
- `cold`:冷藏原料(L5,放 `99-冷藏/`)

**status 字段速查(v3.2 · 仅 L4)**:

- `stub`:占位桩节点,只有标题没有内容(详见 Stubs 机制)
- `draft`:AI 助手生成或人工初稿,待人审核
- `published`:人审核通过,稳定可引用

**confidence 字段速查(v3.2 · 仅 L4)**:

- `0.0-0.3`:低置信度,候选概念,可能误识别
- `0.4-0.7`:中置信度,需要人工核对
- `0.8-1.0`:高置信度,稳定概念

**aliases 字段速查(v3.2)**:

- 列出该概念的所有别名(中英文/缩写/全称),wikilink 自动识别时优先匹配
- 示例:`aliases: [PC, Program Counter, 程序计数器]` → `[[PC]]` 自动跳转到该节点

L1 个人经验层额外字段:

```yaml
event_date: YYYY-MM-DD     # 事件实际发生日期
event_type: 原话 / 感受 / 亲历
```

---

## 收录流程

### Step 1: 判断内容类型 → 选择目录

```
内容是什么?
├── 用户自己的原话/感受/亲历 → 02-Areas/个人经验/<子目录>/(L1,v3 新增)
├── 用户在场的录音/会议原文 → 07-Archive/播客转录/(L2)
├── 具体项目交付物 → 01-Projects/(L4)
├── 某领域的深度内容 → 02-Areas/对应子目录/(L4)
├── 关于某个人的深度拆解 → 03-People/(L4)
├── 可跨域复用的核心概念 → 04-Concepts/(L4,必带 [前缀])
├── 可复用的方法论/SOP → 05-Playbooks/(L4)
├── 工具/资源/失败案例/论文/网络/课程 → 06-Library/对应子目录/(L4)
├── 已结案项目 / 高价值原料 → 07-Archive/对应子目录/
├── 推特/公众号/朋友圈/活动文案 → 99-冷藏/对应子目录/(L5)
└── 不确定 → 00-Inbox/(L3)
```

### Step 2: 创建笔记

通用模板:

```markdown
---
tags: [主标签, 辅助标签]
created: YYYY-MM-DD
source: <来源URL或说明>
author: <作者>
layer: extract
---

# 标题

> 一句话摘要

## 正文内容

(原文完整保留)

## AI 助手分析与梳理

(助手的补充分析,与原文明确分隔)

## 相关链接

- [[相关概念1]]
- [[相关文档2]]
```

L1 个人经验层模板:

```markdown
---
tags: [个人经验, 主标签]
created: YYYY-MM-DD
layer: experience
event_date: YYYY-MM-DD
event_type: 原话 / 感受 / 亲历
---

# <事件标题>

> 一句话上下文

## 原话/感受/亲历记录

(原汁原味,不萃取,不润色)
```

### Step 3: 建立 Wiki Link

每篇精炼笔记必须:
1. **向上链接**:链接到所属领域的 MOC(`_MOC-*.md`)
2. **横向链接**:链接到相关概念节点(`04-Concepts/`)
3. **向下链接**:链接到引用的具体文档

### Step 4: 更新 MOC 索引

- 新笔记加入对应目录的 `_MOC-*.md`
- 新增子领域 → 创建新 MOC
- `!_MOC/知识库总览.md` 必要时同步更新

---

## 检索流程

### 默认 Grep / Glob 命令模板

```bash
# 主区检索(默认豁免冷藏 + 系统目录)
grep -r "QUERY" <vault-root>/ \
  --exclude-dir=99-冷藏 \
  --exclude-dir=_system \
  --exclude-dir=.obsidian \
  --exclude-dir=.smart-env \
  --exclude-dir=Assets \
  --exclude-dir=Excalidraw

# 淘金检索(显式声明,仅冷藏)
grep -r "QUERY" <vault-root>/99-冷藏

# L1 个人经验专项检索
grep -r "QUERY" <vault-root>/02-Areas/个人经验/

# 全量检索(极少用,需明确目的)
grep -r "QUERY" <vault-root>/ \
  --exclude-dir=.obsidian --exclude-dir=.smart-env
```

### 检索三步法

1. **目录路由**:根据问题主题匹配目录(参考高频场景路由表)
2. **关键词 Grep**:在匹配目录内搜,默认豁免冷藏
3. **Wiki Link 跳转**:读取 `[[]]` 链接追溯关联知识

### 检索豁免

纯代码执行、文件操作、≤20 字简单确认不需要检索。

### 检索结果引用

融入回答时标注:「根据知识库 [[XX文档]]...」

---

## 淘金流程

### 触发时机

- AI 助手判断某主题在主区无果,但冷藏区可能有
- 用户明确说"翻一下推特/公众号有没有 XX"

### 淘金 SOP(3 步)

1. **定位**:在 `99-冷藏/` 内显式 grep 关键词
2. **评估**:核对目标文件 frontmatter 的 source/author/date,判断是否值得提炼
3. **迁移**:
   - **复制**(不是 mv)到 02-Areas 对应子目录
   - 原冷藏文件 frontmatter 加 `cherry_picked: true`
   - 主区版本加 Wiki Link 反向指向 `99-冷藏/原文件`
   - 主区版本添加「AI 助手分析」章节

### 淘金禁忌

- 不要批量从冷藏区往主区搬(违背冷藏初衷)
- 同一主题已有主区文件 → 在已有文件追加引用,不新建

---

## Wiki Link 自动识别与维护(v3.1 升级 · 强制)

### 触发时机(必须自动执行)

每次以下操作完成后**必须**自动触发 wikilink 扫描:

- 创建任何新文件(frontmatter 写完后,正文落定后)
- Edit 已有文件且追加内容 ≥ 50 字
- 用户明确说"加 wikilink"、"补双向链接"、"加链接"
- 回写 AI 助手分析到知识库时

### 自动识别 SOP(8 维实体扫描)

新建/更新文件后,必须按以下 8 维度主动扫描候选 wikilink:

| 维度 | 实体类型 | 候选目录 | 链接形态 |
|------|---------|---------|---------|
| 1 | 跨域概念 | `04-Concepts/[*]` | `[[[前缀] 概念名]]` |
| 2 | 人物档案 | `03-People/<同行/客户/导师/合作/团队>/` | `[[人物档案]]` |
| 3 | 项目交付物 | `01-Projects/`、`07-Archive/已结案项目/` | `[[项目名]]` |
| 4 | 长期领域 | `02-Areas/<领域>/` | `[[_MOC-领域]]` |
| 5 | 方法论 SOP | `05-Playbooks/` | `[[SOP 名]]` |
| 6 | 工具/Skill | `06-Library/工具清单/` | `[[工具名]]` |
| 7 | 课程/书籍/论文 | `06-Library/<课程/书籍/论文>/` | `[[资源名]]` |
| 8 | L1 个人经验 | `02-Areas/个人经验/` | `[[原话/感受/亲历]]` |

### 7 步自动扫描流程

```
Step 1: NER 候选词提取
  - 从正文识别专有名词、概念名、工具名、人物名
  - 从 frontmatter tags 提取主题词
  - 从段落标题、列表项首词提取

Step 2: vault 内 Glob/Grep 候选词
  ls <vault-root>/04-Concepts/ | grep -iE "<候选词>"
  ls <vault-root>/03-People/*/ | grep -iE "<候选词>"
  ls -R <vault-root>/05-Playbooks/ | grep -iE "<候选词>"
  ls -R <vault-root>/02-Areas/ | grep -iE "<候选词>"

Step 3: 命中分级
  - 强匹配(全词匹配文件名)→ 必加 wikilink
  - 中匹配(文件名包含候选词)→ 推荐加 wikilink
  - 弱匹配(仅 tags 重合 ≥ 2)→ 助手判断
  - 不匹配 → 不创建死链

Step 4: 在「## 相关链接」追加 [[]]
  - 没有「## 相关链接」区块 → 末尾创建
  - 已有 → 按维度分组追加,去重

Step 5: 反向链接补充(推荐)
  - 在被链接文件追加反向引用(若该文件存在「## 相关链接」区块)
  - 不写入冷藏区文件

Step 6: MOC 同步
  - 文件落到的目录有 _MOC-*.md → 在 MOC 加索引行
  - 新建的概念节点 → 同步注册到「!_MOC/知识库总入口.md」

Step 7: 链接质量门槛验证
  - L4 萃取层 ≥ 2 个 wikilink
  - 04-Concepts 节点 ≥ 5 个,至少 3 跨域
  - L1 个人经验层 ≥ 1 个(向上指 _MOC-个人经验)
  - 06-Library 工具清单 ≥ 1 个
  不达标 → 二次扫描或报告给用户决定
```

### 链接形态规范

| 强度 | 形式 | 用途 |
|------|------|------|
| 强链接 | `[[文件名]]` | 内容核心相关,直接引用 |
| 别名链接 | `[[文件名\|别名]]` | 文件名长/不通顺时用别名 |
| 反向链接 | 双向写入 | 跨域知识网枢纽 |

### 链接质量标准

| 文件类型 | wikilink 数量要求 |
|---------|------------------|
| `04-Concepts/` 节点 | **≥ 5,至少 3 个跨域**(跨 02-Areas/03-People/05-Playbooks 等) |
| `02-Areas/` 文件(L4) | ≥ 2 |
| `02-Areas/个人经验/` L1 文件 | ≥ 1(向上指 `_MOC-个人经验`) |
| `06-Library/` 工具清单 | ≥ 1(指向相关 SOP 或概念) |
| `07-Archive/` 高价值原料 | 建议 ≥ 1 |
| `99-冷藏/` | 不强制 |

### 链接示例(标准模板)

```markdown
## 相关链接

**跨域概念(L4)**:
- [[[方法论] 信息分层模型]]
- [[[理论] 知识库与RAG完全指南]]

**领域索引(向上)**:
- [[_MOC-<your-domain>]]
- [[!_MOC/知识库总入口]]

**人物档案(横向)**:
- [[客户档案-XX]]
- [[同行案例-XX]]

**SOP/方法论**:
- [[<sales-sop>-客户对话]]

**项目记录(向下)**:
- [[YYYY-MM-DD-具体记录]]
```

### Wikilink 禁止事项

1. **不创建死链**:写 `[[XX]]` 前必须 Glob/Grep 确认目标存在
2. **不重复链接**:同一文件同一目标只链一次
3. **不污染冷藏**:`99-冷藏/` 内不强制 wikilink,反向引用也不写入冷藏
4. **不创建无意义弱链接**:tags 重合 < 2 个 → 不强加链接
5. **不在 frontmatter 内写 wikilink**:仅在正文「## 相关链接」或正文段落

### 批量补充方法(已存量文件)

1. 读取目标文件
2. 识别概念、人物、工具、项目(走 8 维扫描)
3. 检查目标实体是否已有节点
4. 在「相关链接」区块追加 `[[]]`
5. 实体不存在且重要 → 创建新概念节点(带前缀)

---

## 概念提炼管道(v3.2 新增 · 5 步 SOP)

> 灵感来自 kytmanov olw(obsidian-llm-wiki-local)。把"原文 → 概念枢纽"这条路径标准化,避免概念散落、重复、遗漏。

### 适用范围

- 从 L2 在场素材(播客转录/会议原文)提炼概念
- 从 L5 冷藏区淘金到主区时同步提炼
- 用户明确说"把这篇文章/对话提炼成概念"

### 5 步 SOP

```
Step 1: Ingest(收录)
  - 原文落入对应层(L2/L3/L5),frontmatter 写齐
  - 生成内容指纹(MD5 前 8 位)用于后续 dedup

Step 2: Validate(验证)
  - 跳过 layer=experience(L1 不萃取)
  - 跳过 cherry_picked=true 的冷藏文件(已淘过金)
  - 跳过被 5 次以上驳回的源(详见 Rejection Feedback)

Step 3: Match(匹配)
  - NER 抽取候选概念(8 维扫描)
  - 对每个候选 → 在 04-Concepts/ + aliases 表里查现有节点
  - 命中 → 走 Step 4a(增强现有节点)
  - 未命中 → 走 Step 4b(创建新草稿或 stub)

Step 4a: Draft - 增强现有节点
  - 在已有节点的「## AI 助手补充」区块追加新观点
  - 标注来源:`> 来源:{原文路径} ({YYYY-MM-DD})`
  - 不动「## 相关链接(自动补充)」之外的人工撰写区
  - confidence 不变,status 维持

Step 4b: Draft - 创建新草稿/stub
  - 信息足够 → status: draft, confidence: 0.4-0.7, 写入 04-Concepts/[前缀] 概念名.md
  - 信息不足 → status: stub, confidence: 0.1-0.3, 写入 04-Concepts/_stubs/[stub] 概念名.md
  - aliases 字段填入识别到的所有别名

Step 5: Review(审核)
  - 写入 _system/_log/extraction-YYYY-MM-DD.md 待审清单
  - 用户审核后 →
    - approve: status: draft → published, confidence → 0.8+
    - reject: 把内容指纹追加到源文件 rejected_drafts 字段
    - edit: 用户修改 → 走 Hand-edit Protection 流程
```

### 提炼禁忌

- 不对 L1 个人经验做提炼(违反"原汁原味"原则)
- 不在原文上修改(只在 04-Concepts/ 创建新节点)
- 不批量自动 publish(必须人审)
- 同一概念已有节点不重复创建(走 Step 4a 增强)

---

## Aliases 别名机制(v3.2 新增)

> 解决问题:同一个概念有多种叫法(中英文/缩写/全称),wikilink 写哪个都该跳转到同一节点。

### 工作机制

1. **frontmatter aliases 字段**:每个 04-Concepts 节点声明所有别名
   ```yaml
   aliases: [PC, Program Counter, 程序计数器, 指令计数器]
   ```

2. **全局别名表**:`_system/_aliases.md` 维护全 vault 别名 → 主节点映射
   - 自动生成,每次新建/更新 04-Concepts 节点时同步
   - 格式:`| 别名 | 主节点路径 |`

3. **Wikilink 自动修复**:8 维扫描时优先查别名表
   - 用户写 `[[PC]]` → 检测到 PC 是别名 → 自动改写为 `[[[理论] 程序计数器|PC]]`(显示文本仍为 PC)
   - 减少死链

### 别名命名禁忌

- 不用过于宽泛的词(如"系统"、"概念"作别名 → 容易撞车)
- 中英文都要写(避免单语别名失效)
- 别名 ≤ 6 个/节点(超过说明节点定义不清)

### 别名冲突处理

两个节点声明同一别名 → 报警到 `_system/_log/alias-conflicts.md`,用户裁决。

---

## Stubs 桩节点机制(v3.2 新增)

> 解决问题:写笔记时引用了一个还不存在的概念,要不要立刻创建完整节点?

### 桩节点定义

桩节点 = **只有标题、frontmatter、最简定义的占位概念节点**。

- 路径:`04-Concepts/_stubs/[stub] 概念名.md`
- frontmatter:`status: stub, confidence: 0.1-0.3`
- 内容:1-2 句话定义 + 出现位置反向链接

### 何时创建桩节点

- Wikilink 8 维扫描时强匹配但目标节点不存在 → 创建桩
- 概念提炼管道 Step 4b 信息不足时 → 创建桩
- 用户写笔记主动 `[[新概念]]` 链到不存在的节点 → 创建桩

### 桩节点模板

```markdown
---
tags: [stub, 主题领域]
created: YYYY-MM-DD
layer: extract
status: stub
confidence: 0.2
aliases: []
---

# [stub] 概念名

> ⚠️ 桩节点 — 待补充完整定义

## 出现位置(反向链接)

- [[来源文件1]]
- [[来源文件2]]
```

### 桩节点提升规则

满足任一时桩节点提升为 draft:

- 反向链接数 ≥ 3
- 用户主动来编辑此节点
- 概念提炼管道再次命中并补充内容

提升时:
- mv 从 `_stubs/` 到 `04-Concepts/[前缀] 概念名.md`(去掉 [stub] 前缀)
- frontmatter:`status: stub → draft, confidence: 0.2 → 0.5`
- 通知用户审核

### 桩节点禁忌

- 不强制全部桩节点都要提升(有些就是低频引用,留着即可)
- 不在桩节点上写大段内容(违反"占位"本质)
- 不创建已存在概念的桩(走 aliases 解决重复识别)

---

## Rejection Feedback 反馈闭环(v3.2 新增)

> 解决问题:AI 助手反复从同一来源生成被人驳回的草稿,浪费时间。

### 工作机制

1. **驳回记录**:用户拒绝 AI 草稿时
   - 在源文件 frontmatter 追加 rejected_drafts 字段
     ```yaml
     rejected_drafts:
       - hash: a1b2c3d4
         date: YYYY-MM-DD
         reason: "误识别为概念,实际是案例"
     ```
   - 草稿文件 mv 到 `_system/_rejected/YYYY-MM-DD-{hash}.md` 留底

2. **驳回预防**:概念提炼管道 Step 2 检查
   - 同一源 + 同一概念再次触发提炼 → 跳过
   - 同一源累计驳回 ≥ 5 次 → 写入 `_system/_blocked.md`,后续完全跳过

3. **解除阻塞**:用户主动从 `_system/_blocked.md` 移除条目 → 重新参与提炼

### 反馈禁忌

- 不删除 rejected_drafts(留作历史)
- 不自动从 _blocked.md 解除(必须人工操作)
- 不让 reject 次数超过 5 次还继续尝试(浪费 token)

---

## Hand-edit Protection 人工编辑保护(v3.2 新增 · 第 7 大原则)

> 灵感来自 kytmanov olw 的 hand-edit detection。AI 助手写的内容和人手写的内容必须物理隔离,人改过的地方 AI 永远不重写。

### 区块标记

每个 AI 助手维护的节点必须用区块标记区分:

```markdown
# [理论] 概念名

> 一句话定义(人工撰写区,AI 不动)

## 核心要点(人工撰写区,AI 不动)

(用户手写内容)

## AI 助手补充

> AI 自动维护,人工修改后 AI 不会覆盖

(AI 增量追加内容,标注每段来源)

## 相关链接(自动补充)

> AI 自动维护,人工修改后 AI 不会覆盖

(8 维扫描自动维护)
```

### 检测机制

1. **AI 写入前必查标记**:在「## AI 助手补充」或「## 相关链接(自动补充)」区块下方写入
2. **人工编辑标记**:用户在 AI 区块内手动改动 → 在 frontmatter 加 `ai_managed: false`,后续 AI 跳过该节点
3. **区块外永不写**:AI 助手永远不写「核心要点」「## XXX(人工撰写区)」等区块

### Hand-edit 禁忌

- 不在没有标记的区块写入(默认所有未标记区块都是人工区)
- 不删除 `ai_managed: false` 标记(只能用户自己改回 true)
- 不混合 AI 与人工内容(物理隔离原则)

---

## 回写流程(outputs → wiki)

### 触发条件

AI 助手回复满足任一时主动回写:
- 综合性分析 / 跨文档综合
- 提出新方法论、框架、判断
- 用户明确肯定("这个总结得好"等)

### 回写流程

1. 判断归属:04-Concepts 是否已有节点?
2. 已有 → Edit 追加章节
3. 全新 → 创建新文件(带前缀,符合命名规范)
4. 标注来源:`> 来源:AI 助手分析(YYYY-MM-DD 对话)`
5. 补 Wiki Link(≥ 5 个)
6. 更新 MOC

### 不回写

纯代码、简单确认、转述原文、一句话回答。

---

## 整理流程

### Inbox 定期清理

1. 读取 `00-Inbox/`
2. 逐个判断归属(用 4+1 层 + 高频场景路由表)
3. mv 移动(不复制)
4. 更新 MOC

### 冷藏区定期淘金(季度)

1. 列出冷藏区 frontmatter 含高价值标签的文件
2. 评估是否值得提炼到主区
3. 走淘金 SOP

### Archive 提炼

从 `07-Archive` 高价值原料发现内容时:
1. 在 02-Areas / 05-Playbooks 创建精炼笔记
2. 提炼核心观点(不复制全文)
3. 建立 Wiki Link
4. Archive 原文保留不动

---

## 自动化脚本写入路径

| 脚本类型 | 写入目标 | 备注 |
|------|--------|------|
| Twitter / X 抓取 | `99-冷藏/推特/<推主名>/` | **不再写主区** |
| 公众号 RSS | `99-冷藏/公众号博主库/<博主名>/` | 默认冷藏 |
| 播客转录(用户在场)| `07-Archive/播客转录/` | L2,主区保留 |
| 论文抓取 | `07-Archive/论文原文/` | 主区保留 |
| 朋友圈/活动文案入库 | `99-冷藏/朋友圈素材/` 或 `99-冷藏/活动文案/` | - |
| 用户语音转写(原话)| `02-Areas/个人经验/原话与决策/` | L1,v3 新增 |
| 用户朋友圈本人发布 | `02-Areas/个人经验/原话与决策/` | L1,与 99-冷藏/朋友圈素材/ 区分 |

新增自动化脚本入库前必须更新此表,且必须默认写入冷藏区(除非来源是用户本人 L1 内容)。

---

## 禁止事项

1. **不删除任何文件**(除非用户明确要求)
2. **不修改原文内容**(只追加)
3. **不用 Write 覆盖已有文件**(用 Edit)
4. **不改朋友圈素材的序号和内容**
5. **不把敏感信息写入 Vault**(凭证、密钥、Token 保留在本地配置文件)
6. **不批量从冷藏区往主区搬**
7. **不在主区写入低频原料**(推特/公众号必须走冷藏区)
8. **不创建无前缀的 04-Concepts 节点**
9. **不在 `02-Areas/个人经验/` 用 SOP 化格式写**(v3 新增)——L1 必须保留原汁原味,SOP 萃取后走 05-Playbooks
10. **frontmatter 必须含 layer 字段**(v3 新增)——分层是 v3 的核心,缺失视为不合规
11. **不在「人工撰写区」写入 AI 内容**(v3.2 新增)——遵循 Hand-edit Protection 第 7 大原则,AI 只在「## AI 助手补充」「## 相关链接(自动补充)」标记区块写
12. **不批量自动 publish 概念草稿**(v3.2 新增)——所有 status: draft → published 必须人审,违反视为污染概念枢纽
13. **不对同一源反复尝试已被驳回 5 次以上的提炼**(v3.2 新增)——必须先经用户从 `_system/_blocked.md` 解除阻塞

---

## 适用范围

- Obsidian 知识库文件数 ≥ 500
- 有自动抓取流(推特/公众号/RSS)产生大量原料
- 配合 Claude Code(或类似支持 skill 的 AI 助手)使用
- 重视个人经验保鲜(L1 不被 SOP 萃取流程稀释)

## 不适用范围

- 文件数 < 200 的小型 Vault(过度工程)
- 只用 Obsidian 写日记/碎碎念(无萃取需求)
- 没有自动抓取流(L5 冷藏区无意义)
- 不使用 AI 助手(失去自动化价值)

详见 [PHILOSOPHY.md § 六](./PHILOSOPHY.md#六什么时候不该用本-skill)。

---

## License

MIT

## Changelog

- **v3.2**(2026-05-10): 融合 kytmanov olw(obsidian-llm-wiki-local)思想——新增概念提炼管道 5 步 SOP(Ingest→Validate→Match→Draft→Review)、Aliases 别名机制(frontmatter aliases 字段 + `_system/_aliases.md` 全局表)、Stubs 桩节点机制(`_stubs/` 目录 + `[stub]` 前缀 + 提升规则)、Rejection Feedback 反馈闭环(rejected_drafts 字段 + 5 次驳回阻塞)、Hand-edit Protection 第 7 大原则(AI 区块标记 + 人工编辑检测)、frontmatter 新增 status/confidence/aliases/rejected_drafts 字段、禁止事项 +3 条
- **v3.1**(2026-05): 新增 Wiki Link 自动识别 SOP(8 维实体扫描 + 7 步流程)、强制新建/更新文件后扫描 wikilink、新增链接形态规范、新增链接示例标准模板、新增 wikilink 禁止事项 5 条
- **v3.0**(2026-05): 引入 4+1 层信息分层模型(余一框架 + 冷藏扩展)、新增 L1 个人经验层及三个子目录、新增高频场景路由表(12 场景)、frontmatter 强制 layer 字段、新增独立 PHILOSOPHY.md 思想文档
- **v2.0**(2026-05): 新增 99-冷藏区隔离机制 + 命名前缀规范 + 淘金 SOP + Grep 默认豁免规则
- **v1.0**(2026-04): 初始版本,基于 PARA + P.A.I.R 融合架构
