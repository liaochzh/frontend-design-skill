# frontend-design

一个用于 **Web 前端视觉设计** 的 agent skill：新建或重塑 Web UI、页面、组件、落地页、仪表盘、海报素材。强调独特艺术方向、字体排印与签名元素，拒绝模板化 AI 风。

> 面向 opencode / Claude 一类支持 `SKILL.md` 的 agent 运行时。纯提示词实现，无依赖、无脚本、不联网也能用。

## 它解决什么问题

| 常见失败 | 本 skill 的做法 |
|---|---|
| 出来一股「AI 风」：紫蓝渐变、居中 Hero + 三卡片、无差别玻璃拟态 | 先定**艺术方向与签名元素**，再动手；判据来自哲学层 §俗套校准表，落地时逐项过 |
| 只给「高级感」形容词，给不出可执行参数 | Web 实现层落到字体排印、间距节奏、网格、色彩系统、层级与动效的具体口径 |
| 页面各自为政，跨页不一致 | 多页骨架与跨页不变量：一套令牌源、布局原语、组件类单一来源 |
| 桌面端好看，手机端崩 | 内容断点 + 容器查询、大屏加列、窄窗与分屏抽样、无意外横滚 |
| 性能被视觉吃掉 | 性能预算（p75）与首屏不作弊的判据 |

## 何时触发

用户要求**新建 / 重塑 / 美化** Web UI：页面、组件、落地页、仪表盘、海报、React 组件、HTML/CSS 布局，或提到「这个页面太模板了 / 没有设计感 / 想要更高级的质感」且目标为 Web。

不负责：App 端视觉（用 app-design）、无障碍专项审计（用 accessibility）、从零产品的 PRD→原型全流程（用 product-prototype-generation）。

## 安装

```bash
# 方式一：整个目录放到 skills 根目录
git clone https://github.com/liaochzh/frontend-design-skill.git
cp -R frontend-design-skill ~/.config/opencode/skills/frontend-design

# 方式二：软链（便于跟随仓库更新）
ln -s "$(pwd)/frontend-design-skill" ~/.config/opencode/skills/frontend-design
```

不同运行时的 skills 目录不同（例如 Claude Code 的 `~/.claude/skills/`），把本目录放进去即可，`SKILL.md` 的 frontmatter（`name` / `description`）就是触发依据。

## 结构

```
SKILL.md     # 主体：触发条件 → 哲学层引用 → 步骤0 → Web 实现层 → 边界 → 示例 → 维护说明
```

## 设计要点

- **哲学层是唯一权威源**：视觉立场、设计原则、两遍工作法、通用自检清单、输出模板都在同一份共享文档里（`design-spec` 仓的 `references/ui-design-philosophy.md`），本文件只写 **Web 端专属**落地，不复制通篇。文件头给出该文档的路径与「配套版本」锚。
- **令牌源与单一来源**：组件类样式必须有唯一来源，避免「同一按钮五处定义」这类结构性问题；跨页不变量与页面衔接单独成节。
- **锚语义**：维护说明里的「配套 vX.Y.Z」= 本文件内容最后一次同步的哲学层版本；哲学层未触及本文件章节时允许滞后。

## License

MIT（见 `LICENSE`）。
