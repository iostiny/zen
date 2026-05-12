# AGENTS.md — iostiny / zen

给在本仓库工作的 AI 编码代理的项目指南。

## Git 工作流（硬性）

**每次完成一次有意义的修改后，必须 `git commit` + `git push origin main`，无需再次征求同意。**

- 这是 iostiny 设定的默认行为，目的是让小步快跑的内容更新立刻上线（站点是 GitHub Pages，push 即发布）。
- 该规则**仅由 iostiny 手动修改本节**变更；其他 agent 不得绕过、不得"为了谨慎"省略 push。
- commit message 用中文一句话，描述这次改动的"为什么"或者"做了什么"，跟随仓库已有风格（参考 `git log`）。

## 仓库定位

`zen` 是 iostiny 维护的**中文读书笔记 / 思想卡片**集合。

| 维度 | 说明 |
|---|---|
| 主题 | 读书 · 思想 · 心智 |
| 视觉 | 枯山水 · 沙白 + 苔藓绿 + 沙金 · Noto Serif SC |
| 阅读姿态 | 慢读 · 反复回味 · 当下做选择 |

部署：`zen.iottiny.top`（GitHub Pages，main branch / root）。

## 目录结构

```
zen/
├── index.html              ← 站点 hub：报头 + 本期目录 + 特辑卡 + 筹备中
├── naval/                  ← 特辑 № 01 · 纳瓦尔宝典
│   └── index.html
├── tian-dao/               ← 特辑 № 02 · 遥远的救世主
│   └── index.html
├── README.md / AGENTS.md / CLAUDE.md / CNAME / .gitignore
```

每本书一个子目录（kebab-case slug），目录里至少有一个 `index.html` 作为该书的卡片合集。子目录可以单独下载离线看，跟根 hub 解耦。

## 工程约束

- **零依赖**：纯静态 HTML + 原生 CSS + 原生 JS。不引入 npm、构建链、SPA 框架、外部 JS 库。Google Fonts 是唯一外链。
- **浏览器双击即看**：所有路径必须是相对路径。从 hub 到子页用 `./<slug>/`，从子页回 hub 用 `../`。
- **每页 self-contained**：单文件可以很长（800+ 行接受），所有 CSS / JS / SVG 都内联。**不**跨页面共享 CSS 文件——hub 和每个子页各自内联同一套 CSS 的拷贝。这是 self-contained 的代价，可接受。
- **每个子目录必须有 `index.html`**：作为该书的 landing 页。

## 视觉规范

走**枯山水 / 日式禅**主调：

```css
--paper:       #ece8df;  /* 沙白 / 风化石面 */
--paper-light: #f4f1e8;  /* 浅一层 */
--paper-card:  #f2eee5;  /* 卡片底（比 paper 略亮，浮起来）*/
--ink:         #1f1f1f;  /* 石墨 */
--ink-deep:    #0e0e0e;  /* 重墨 / 题字 */
--ink-soft:    #5e5a52;  /* 灰墨 / 次要 */
--ink-faint:   #8d8a82;  /* 远石灰 */
--rule:        #bab5a8;  /* 石线 */
--rule-soft:   #d4d0c4;  /* 浅石线 */
--accent:      #5e7548;  /* 苔藓绿 / 主标记 */
--accent-soft: #8aa370;  /* 浅苔 */
--ember:       #a08856;  /* 沙金 / 次要 accent */
```

字体：

- 中文正文 / 标题：`Noto Serif SC`（衬线）
- 西文 / 编号 / attribution / italic：`Cormorant Garamond`（衬线 italic）
- **不使用** mono 字体（保持纸张衬线感）

枯山水感的关键视觉元素：

- **Masthead 报头**：4px 双线 top（hub）/ 3px 双线 top（子页） + 1px 紧贴的 ::after 形成"双线压尾"
- **Section header 章节刊头**：双层底线（2px 实 + ::after 1px 紧贴），编号 `№ 01` italic 用苔绿
- **Lede 前言**：italic 居中 + 上下短横线装饰
- **着重号**：`<strong>` 用 `text-emphasis: filled var(--accent)` + `position: under` 在每个字下加苔绿小点（中文传统重点符号）；**不要**回到 highlighter 背景色块
- **背景微纹理**：极浅的 radial-gradient 灰点（rgba(30,30,30,0.05) 1px @ 22px）模拟石面沙粒
- **Ornament**：章节间用 `─── ◆ ───` 居中装饰线
- **Colophon 版权印刷信息**：底部 mono italic + dotted 下划线链接

### Hub vs 子页的视觉区别（要保留）

- **Hub 的 masthead 更大**：标题 3.4rem（子页 2.8rem），双线更粗（4px vs 3px），"`/`" 用 accent（苔绿）色突出
- **Hub 的内容是"目录"**：issue cards 是大卡（420px+，单卡占据大量空间），不像子页的内联 cards（小、平铺）
- **Hub 不带 TOC 浮动栏**：内容短，不需要
- **子页带 TOC 浮动栏**：贴右边垂直排列，仅 ≥720px 显示

## 内容设计原则

- **金句优先**：每张卡以一句核心判断开篇（`.card-quote`），≤ 30 字。读者扫一眼就能记住。
- **3-5 段细解**：紧跟金句给出 2-4 行展开，每段独立成立。
- **苔绿着重号**：用 `<strong>` 包关键词，CSS 自动加 `text-emphasis` 小点。一张卡 1-3 处即可，多了就麻木。
- **可折叠的"原文延伸 / 延伸金句"**：用 `<details class="card-extras">` 装 1-3 条原书引言。默认折叠，让卡面保持干净。
- **不要**强行套 SVG。文字 + typography 就是视觉。
- **不要**为了对称凑卡数。每个 section 5-8 张刚刚好；写不动就停。
- **金句保留原书味道**：英文书可保留 italic 英文；古典中文书全中文呈现。

## 添加新书的 checklist

1. 选 slug：kebab-case，简短、名词性。如 `dao-de-jing` / `antifragile` / `poor-charlie`
2. `mkdir <slug>/` → 写新的子页 `<slug>/index.html`
   - 复用同一套 CSS（直接复制，self-contained 的代价）
   - 章节数 3-4 章，每章 4-8 张卡，总卡数 12-25 之间为宜
   - masthead 的 vol 改为 `特辑 № NN`（接续编号）
   - by-line 链接 `../`
   - TOC 锚改成该书章节锚（如 `#culture #dao #awakened`）
3. **更新根 `index.html`**：
   - 在 `.issues` grid 里加一张 `.issue` 卡，编号 `特辑 № NN`
   - 选一句最具识别度的金句作 pull-quote
   - 改 stats: `N 张卡 · 章名 / 章名 / 章名`
   - 从 `.upcoming-books` 里删除该书（如果之前列在 upcoming）
4. **更新 README.md** 的"当前内容"列表
5. 测试 hover、CTA、mobile 响应式（≤600px）

## 不要做的事

- ❌ 引入 npm / 构建步骤 / SPA
- ❌ 把读书笔记写成"技术文档"语气（这里是慢读、是品味、是判断；不是 API 文档）
- ❌ 为了"看起来丰富"塞图标、emoji、装饰图 —— 枯山水的力量在留白和 typography
- ❌ 把多本书塞进一个 HTML 文件（每本书一个子目录是硬性结构）
- ❌ 跨子目录共享 CSS / JS 文件
- ❌ 回到 highlighter 背景色块（`linear-gradient(...transparent 62%...)`）—— 已经决定用着重号
