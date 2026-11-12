---
name: frontend-design
description: 高质量 Web 前端视觉设计。新建或重塑 Web UI、页面、组件、落地页、仪表盘、海报素材、样式美化视觉提升时用。强调独特艺术方向、字体排印与签名元素，拒绝模板化 AI 风。仅做视觉层；不用于从零产品的 PRD→原型全流程（用 product-prototype-generation，全局可用）、不用于无障碍专项审计（用 accessibility）、不用于 App 端视觉设计（用 app-design）。
license: MIT
metadata:
  version: "2.4.0"
  platforms: "web"
  last-updated: "2026-11-12"
---

# Frontend Design V2.4.0（Web 端）

## 触发条件
- 用户要求**重塑/美化/去模板化**现有 Web UI：页面、组件、落地页、仪表盘、海报、React 组件、HTML/CSS 布局
- 关键词：样式美化、视觉提升、设计稿、UI 重做、去模板化、Awwwards 级别
- **仅做 Web 视觉层**；App 端（Flutter/SwiftUI/Compose）→ 用 `app-design`
- **不接 PRD 流水线、后端、部署**；从零产品想法→PRD→原型流水线 → 用 `product-prototype-generation`

## 哲学层引用（开工前必读者）
- **执行本 Skill 前，先读取 `~/.config/opencode/design-spec/references/ui-design-philosophy.md`**，获取设计立场、设计原则、两遍工作法、通用自检清单、输出模板与纪律。
- 本文件只负责 **Web 平台实现层**，哲学层内容不在此重复。
- 涉及 AI 生成 / 改写内容的界面，另读哲学层 §交互规范 · 生成式 AI 交互基线（跨端，Web 同样适用）。

### 加载分层（省 token，禁止全量读取哲学层）
- **L0 常驻**：快速决策树 + 四层模型（每次必读；任何任务都须读权威源 §俗套校准表 段内首个「### Web」）
- **L1 任务相关**：新建界面读 布局与对齐 / 组件基线 + Token/色彩/字体/排印 treatment/AI 指纹/间距 + 交互（性能预算、表单与校验）；改造读俗套表（权威源）+ AI 指纹 + 交互（含边界状态、性能预算、表单与校验）；多端/大屏读自适应布局；多页/衔接读跨页一致性 + 导航与转场；形态/输入读形态与共存；语言/RTL 读国际化与 RTL；涉及 AI 生成 / 改写则读生成式 AI 基线；兼容/机型读兼容基线；图标读图标规范；贴图读贴图规范；交付前读交付闸门（权威源）
- **L2 参考**：无障碍完整条款 / 额度 / 版本演进 / 反面案例库（仅冲突或审计时读）

## 步骤0：推断优先，不阻断
- 能推断直接采用，不要追问：主题/受众/任务、技术栈（有代码跟随项目栈，无则单文件 HTML+CSS+JS）、交付形态（单页/组件/海报）。
- 仅当缺失会显著跑偏时追问 1 个最关键的（如：目标用户是谁 / 要保留现有品牌色吗）。
- 推断假设在最终交付开头 `假设` 一行显式标注。
- **步骤-1（先定标准单）**：`<WORKSPACE>` = 项目根；已有 `<WORKSPACE>/docs/design-standard.md` 直接引用；没有则先出单再画页，标准单字段见哲学层 §两遍工作法第一遍。

## Web 实现层

### 技术栈检测
- 检测到 `package.json` + 框架特征文件（`vite.config.*`/`next.config.*`/`astro.config.*`/`svelte.config.*`/`tailwind.config.*` 等）时，输出对应框架组件/页面结构。
- 仅无任何构建配置时退回单文件可直接打开的 HTML+CSS+JS。
- 图片/图标用可达 CDN，`img` 带失败兜底，不用占位图服务。

### 最小 CSS Reset（仅单文件模式内联，可选）
```css
*, *::before, *::after { box-sizing: border-box; }
* { margin: 0; padding: 0; }
html { -webkit-text-size-adjust: 100%; text-size-adjust: 100%; }
body { line-height: 1.6; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; }
img, picture, video, canvas, svg { display: block; max-width: 100%; height: auto; }
input, button, textarea, select { font: inherit; }
p, h1, h2, h3, h4, h5, h6 { overflow-wrap: break-word; }
```

### 令牌源与布局原语
- 写码前先定位令牌源（`:root` 自定义属性 / Tailwind theme / 主题文件）；存在则只消费，不另造第二套；不存在则先建再写码。
- 布局原语只能从以下选：`.stack`（纵向 + `gap`）、`.cluster`（横向可换行）、`.grid`（二维）、`.sidebar`、`.cover`。区间距一律由原语给出，禁止逐块写 `margin`。
- 容器只允许 `--layout-container-max` 与 `--layout-gutter` 两个值（对应哲学层 `layout/container-max`、`layout/gutter`）；块级元素左右边界贴 `--layout-gutter`。
- 数字、金额、表格数值列：`text-align: end` + `font-variant-numeric: tabular-nums`。

### 组件类单一来源
- `.btn` / `.input` / `.card` / `.chip` / `.badge` 各只定义一次；页面里只加变体类，不改基础类。
- 变体白名单：`size`（sm/md/lg）× `variant`（primary/secondary/ghost/danger）；新增变体回写本文，不写临时类。
- 状态集：`default` / `hover`（`@media (hover:hover)` 内）/ `focus-visible` / `active` / `disabled` / `loading` / `error`；数据视图另加 `empty` / `loading` / `error`。
- 语义固定写法：表单 → 字段组 + 可见标签；选项集 → 分段控件或单选组；危险操作 → 确认对话框；空数据 → 空态 + 下一步动作。
- 组件类内部不得出现字面色值 / 像素值（全部 `var(--…)`）。

### 按钮位
- 主 CTA 唯一常驻（无主操作页记“本页无主 CTA”），层级首选位序/间距/字号，tint/高度按硬规则；命中区不改。

### 多页骨架与跨页不变量
- 多页产物共用一个 layout 片段（服务端 include / 模板 / 组件），`header`/`main`/`footer` 的结构与类名逐页一致，不复制粘贴改写。
- 槽位固定：header 高度、页面 `--layout-gutter`、footer 结构只定义一次。
- 导航当前态用 `aria-current="page"`，同位置同视觉。
- 交付抽 2 页比对：header 高、gutter、卡片圆角、按钮高、字号角色。

### 页面衔接
- 先定层级模型（push / modal / replace / reveal），再选动效；方向与层级一致，返回反向。
- 同文档导航可用 View Transitions（`document.startViewTransition`）作增强；跨文档 `@view-transition` 属 limited availability（2026-10 核读），必须放在 `@supports` 或能力检测内，缺失时直切可用。
- 无框架静态多页默认交叉淡化或直切，不引入路由库；返回恢复滚动位置与筛选状态（`history.scrollRestoration` + 表单记忆）。
- `prefers-reduced-motion: reduce` 时降为瞬时。

### 兼容基线与降级
- 能力上限：MDN Baseline `widely available`；新特性只在 `@supports` 内使用并留退路；`limited` 不得作唯一路径。
- 红旗清单（每项写退路）：裸 `100vh` 定高（改 `dvh`/`svh`）；输入字号 <16px（iOS 聚焦缩放）；`backdrop-filter` 缺 `-webkit-` 前缀；缺 `viewport-fit=cover` + `env(safe-area-inset-*)`；`text-box-trim`（Baseline 2026 新增，2026-10 核读，仅增强）；滚动容器内 sticky；未设 `overscroll-behavior`。
- 交付贴验证矩阵：320/360/390/430/768/1024/1440 × Safari(iOS)/Chrome(Android)/桌面三引擎；未测写 N/A + 理由。
- 平板 / 多窗口：组件级断点用**容器查询**（`@container`），不用 UA / `screen.width` / 方向分支；窗口宽是**范围不是承诺**（1/3–2/3 分屏、自由窗口、外接显示器），窄窗下主操作不被遮挡、无横滚；平板**加列/加信息**，不把手机稿等比放大或只做居中留白。矩阵补平板横竖与分屏两行（多形态单独列，不只测整屏）。

### Web 俗套表（唯一权威源：哲学层 §俗套校准表 段内首个「### Web」）
条目清单、逐条特征、适用条件与 **禁用** 判定全在权威源，本节不重复、**不得据本节判定**。
纪律：Brief 明示要才用，否则换自由轴。

### 暗色/高对比 Token 策略
- 暗色必须走**语义重映射**（见哲学层「色彩规范」的语义映射表），**不是把亮色 token 反转**——反转会得到刺眼、层级错乱的结果；不自造纯黑纯白
- `prefers-contrast: more` 下提升对比度至 7:1

### 字体加载策略
- Display 用 webfont（预连接+swap）
- Body/Utility 可回退系统栈

### Web 质量自检（交付前必过，在哲学层通用清单之上）
- [ ] 底线齐：375px/1440px 无非预期横滚（显式画廊例外），并抽窄窗与分屏各一次；焦点可见、无 Lorem/残留英文；目标语言为中文时标点正确（多语言判据见哲学层 §国际化与 RTL）
- [ ] 尊重 `prefers-reduced-motion` / `prefers-color-scheme` / `prefers-contrast`
- [ ] 自适应：内容断点 + 容器查询（`@container`），不用 JS 读屏宽分支；新界面先定布局模式（list-detail / supporting pane / feed），导航形态随内容断点切换；大屏加列不加字号；除显式画廊外无横滚
- [ ] 滚动驱动：`animation-timeline` 仅在 `@supports` 内使用，无动画时内容完整可读、不用于长列表
- [ ] 安全区与键盘：`viewport-fit=cover` + `env(safe-area-inset-*)`；聚焦输入不被软键盘遮挡；`VirtualKeyboard API` 只在特性检测内作增强
- [ ] hover：`@media (hover: hover) and (pointer: fine)` 包裹增强，触屏有等价可见反馈，hover 不作唯一反馈
- [ ] 输入法：组合输入未上屏时回车不提交、不校验（行为判据，不依赖 `isComposing` 单独成立）
- [ ] 文案：无按字符数裁切、无写死宽度；截断不丢语义；译文膨胀留量
- [ ] 镜像与方向：逻辑属性（`margin-inline-*` / `inset-inline-*` / `text-align: start`）；`dir` + `lang` 声明，无 JS 反转或 `scaleX(-1)` 伪造；固有方向元素不镜像
- [ ] 本地化格式：数字 / 日期 / 货币 / 复数走 `Intl`；双向文本用 `<bdi>` / `dir="auto"` 隔离
- [ ] 共存：多窗口 / 标签失焦不弹模态、不自动播放、不抢焦点（见哲学层 §形态与共存）
- [ ] 性能预算：LCP ≤ 2.5s / INP ≤ 200ms / CLS ≤ 0.1（按 p75；无 field 数据记 N/A，**不得**把 lab 当 p75）
- [ ] 性能不作弊：首屏内容未被 `opacity: 0` / 延迟挂载隐藏；首屏关键图不 `loading="lazy"`（改 `fetchpriority="high"`）
- [ ] 表单错误：以文本指出字段与原因、聚焦错误字段或摘要置顶（回指 3.3.1/3.3.3）
- [ ] 表单标签：每个输入有可见标签，占位符不作标签（回指 3.3.2）
- [ ] 对齐：单对齐轴、贴 `--layout-gutter`、跨字号走文本基线、数值列 `tabular-nums`；无逐块 margin 微调
- [ ] 组件唯一：`.btn/.input/.card` 单一定义；按钮高度与控件圆角各只一种；组件类内无字面值
- [ ] 跨页：header/gutter/footer 单一定义；导航 `aria-current`；抽 2 页五项实测一致
- [ ] 衔接：方向与层级一致；返回恢复滚动与筛选；跨文档 VT 在 `@supports` 内且有直切退路
- [ ] 兼容：红旗清单逐项有退路；验证矩阵已贴，未测项注明 N/A
- [ ] 语义化标签、**非首屏**图片懒加载、交互有可访问名称
- [ ] 边界状态：断网/弱网/服务端错误三态区分、「空数据 vs 无权限」区分、无裸转圈（见哲学层「交互规范」边界状态节）
- [ ] 注意 CSS 特异性：class 选择器与元素选择器（`p`/`h1`）、ID、内联样式互相抵消，尤其段间距 padding/margin，先定层级再写
- [ ] 可选自动化：任一 HTML/CSS lint（`html-validate` / `stylelint`）通过即加分；Lighthouse 需配置 budget.json 与目标 URL
- [ ] Web 淘汰闸门：见哲学层 §交付闸门 ›「### Web 淘汰闸门」（11 项；无 Brief 依据时命中即淘汰，Brief 点名则按 §俗套校准表 放行）
- [ ] AI：涉及生成式 AI 的界面已过哲学层 §交互规范 · 生成式 AI 交互基线（标识 / 可编辑·撤销·重试 / 不可逆先确认 / 不确定标注 / 等待 / 多版本 / 隐私）；无 AI 功能则跳过
- [ ] AI 指纹与排印：无 Brief 依据时未命中 chrome 级指纹；标题无单词级强调、标签非全大写、行长符合 §排印 treatment（含衬线例外）、双 family 可辨（回指哲学层 §AI 指纹 / §字体规范 · 排印 treatment）
- [ ] 截图自审：交付前截图两档（本仓 `bshot`；档宽取本文「底线齐」行既有值），逐张核对一眼 AI 与布局塌陷（无截图能力记 N/A + 理由）
- [ ] 标准单：项目标准单存在且全页同值，色号/字号/控件档未超单
- [ ] 按钮位：主 CTA 唯一常驻，层级按位序/间距/字号（tint/高度按硬规则）
- [ ] 布局消费小节 no-op：布局原语/容器两值/返回恢复/布局模式已存在保留，仅新增按钮位一行并指向该节
- [ ] 图标/插图：按哲学层风格生成分支（白名单生成/黑名单固定/角色级回退），16px 三判据见哲学层 §图标规范·风格生成分支

## 与其他 Skill 边界
- 要走 `PRD→模板渲染` 流水线（电商/SaaS 原型、移动端/PC 端多页）→ 用 `product-prototype-generation`。
- 要定制视觉身份、单页/组件级打磨、去 AI 味 → 用本 Skill。两者不混调、不互相调用脚本。
- App 端视觉设计（Flutter/SwiftUI/Compose）→ 用 `app-design`，本 Skill 不覆盖。
- 无障碍专项审计/修复 → 交 `accessibility`，本 Skill 只保底线。
- 视觉产出评审→本 Skill 自检章节；技术方案/架构评审→`design-review`。

## 使用示例
- `做一个独立咖啡馆落地页，要有烘焙感，别像模板` → 定主题烘焙工坊+Token+签名（如烘焙曲线分隔线），单文件交付
- `把这个仪表盘视觉提升一下，保持密度但更有个性` → 保留信息结构，只换字型/层级/签名数据呈现
- `用这套文案做一张 Web 海报，移动端也要好看` → 海报式 Hero+大字型，375px 自检

## 维护说明
- 版本：2.4.0（2026-11-12），哲学层共享 `design-spec/references/ui-design-philosophy.md`（配套 v2.10.0）
- v2.4.0：L1 路由补「AI 指纹」「排印 treatment」；Web 质量自检 +2 行（chrome 级指纹与排印 treatment / 交付前 bshot 截图自审）；同步哲学层 v2.10.0
- v2.3.1：§兼容基线与降级 +1 条（平板/多窗口：容器查询断点、窗口宽是范围、加列不加倍、矩阵补平板与分屏）；同步哲学层 v2.8.0（Android 16 强制自适应、iPadOS 26 自由窗口）
- v2.3.0：标准单前置（步骤-1）+ 图标/插图风格生成分支 + 布局消费 no-op 声明 + 按钮位；配套哲学层 v2.7.0
- v2.2.3：同步哲学层 v2.6.1（兼容基线：`dvh`/`svh` 已 widely available、`animation-timeline` 升 Baseline 2026 newly available；红旗清单措辞不变——「裸 `100vh` → 改 `dvh`/`svh`」结论保留；仅同步锚）
- v2.2.2：同步哲学层 v2.6.0（`俗套校准表 · Web` 的「渐变光晕 Aurora」特征列扩张，本文件为指针故判据未动，仅同步锚）；锚 v2.6.0
- v2.2.1：修正 L1 哲学层指针双前缀（`design-spec/design-spec/references/` → `design-spec/references/`，原指向不存在路径）；许可证 MIT-0 → MIT；**判据未动**，锚不变 v2.5.0
- v2.2.0：新增 5 节（令牌源与布局原语 / 组件类单一来源 / 多页骨架与跨页不变量 / 页面衔接 / 兼容基线与降级）+ 自检 5 行；L1 补 5 章路由；配套哲学层 v2.5.0
- v2.1.15：L1「新建界面」分支补性能预算 + 表单与校验路由（T13 收口）；配套哲学层 v2.4.14（路由修正，无新阈值）
- v2.1.14：质量自检 +2 行（表单错误文本定位 / 表单标签）；L1 括注补表单与校验；配套哲学层 v2.4.14
- v2.1.13：质量自检 +2 行（性能预算 p75 / 首屏不作弊）；懒加载行限定为「非首屏」；L1 括注补性能预算；配套哲学层 v2.4.13
- v2.1.12：质量自检补文案 / 镜像与方向 / 本地化格式 3 行；底线行「中文标点」限定为「目标语言为中文时」；L1 补语言/RTL 路由；配套哲学层 v2.4.12
- v2.1.11：镜像审计补 2 处（消费哲学层既有判据）——自检「自适应」行补「先定布局模式 + 导航随内容断点切换」，新增「共存」行（失焦不弹模态 / 不自动播放 / 不抢焦点）；锚 v2.4.7 → **v2.4.10**（2.4.8–2.4.10 经核未触及本文件章节，本轮同步后即为最后同步版本）
- v2.1.10：配套哲学层 v2.4.7（版本/指针同步，无新增条款；本批只动 §俗套校准表 · App 与结构整理，Web 侧无变化）
- v2.1.9：配套哲学层 v2.4.5（版本/指针同步，无新增条款；2026-10-26 HIG Materials 瞬态玻璃例外只涉 Apple 条）
- v2.1.8：质量自检补安全区与键盘避让、hover 查询、输入法组合态三项；L1 补形态/输入路由；配套哲学层 v2.4.4
- v2.1.7：质量自检补自适应（内容断点 + 容器查询、大屏加列、无意外横滚）与滚动驱动降级两项；底线行补画廊例外与窄窗/分屏抽样；L1 补多端/大屏路由；AI 基线简写 6 标签补齐为 7 标签；配套哲学层 v2.4.3
- v2.1.6：版本行补「（配套 v2.4.2）」锚——本文件此前是唯一不被 `anchors` 覆盖的消费方；哲学层已把「必须有配套锚」纳入判红
- v2.1.5：哲学层引用与 L1 加载分层补「生成式 AI 交互基线」指针，Web 质量自检加 AI 项（配套哲学层 v2.4.1；本文件无版本锚，靠本节留痕）
- v2.1.4：Web 淘汰闸门改指针（并入哲学层 §交付闸门；质量自检加「交付前过闸门」项；加载分层补交付闸门）
- v2.1.3：Web 俗套表改指针引用（单一 owner 化；不列条目名，禁据本节判定）
- v2.1.2：新增 2 条 Web 俗套（Barely-there 极简克制 / Anti-grid 刻意破碎网格），与哲学层 2.2.2 对齐
- v2.1.1：Web 俗套表与哲学层逐字对齐（修措辞与分隔行差异）
- v2.1.0：暗色「反转」改为「语义重映射」；俗套表并入 2026 新风格 5 条；新增加载分层、边界状态自检
- 消费方：`app-design` 共享哲学层；两者互不覆盖
