---
name: analysis-report
description: 生成专业的暗色系 HTML 单页分析报告，含固定侧边栏导航、标题统计区、多类型内容组件（信息卡片、表格、风险矩阵、时间轴、对比表等）。当用户需要生成现状分析报告、技术评估报告、系统调研报告、业务分析报告时使用本 skill。触发词包括：现状分析、AS-IS 分析、系统调研报告、技术评估、业务分析报告、风险评估报告、分析报告、帮我做个报告。
license: MIT
metadata:
  version: "1.0"
---
# Analysis Report Skill

生成专业暗色系单页 HTML 分析报告。自带侧边栏导航、丰富的信息展示组件，适合系统现状分析、技术调研、业务分析等场景。

---

## 一、设计系统

### 色彩语义

| 用途              | CSS 变量             | 十六进制    | 使用场景                 |
| ----------------- | -------------------- | ----------- | ------------------------ |
| 主背景            | `--bg`             | `#020617` | body 背景                |
| 卡片面            | `--surface`        | `#0f172a` | 卡片、sidebar            |
| 边框              | `--border`         | `#1e293b` | 所有分隔线               |
| 正文主色          | `--text-primary`   | `#f8fafc` | 标题                     |
| 正文次色          | `--text-secondary` | `#e2e8f0` | 普通文字                 |
| 正文弱色          | `--text-muted`     | `#94a3b8` | 说明文字                 |
| 正文暗色          | `--text-dim`       | `#475569` | 辅助标注                 |
| 青色（信息/主题） | `--cyan`           | `#22d3ee` | 主色调、导航激活、信息类 |
| 绿色（正常/完成） | `--emerald`        | `#34d399` | 正常状态、完成状态       |
| 紫色（分析/数据） | `--violet`         | `#a78bfa` | 数据层、分析类           |
| 琥珀（警告/注意） | `--amber`          | `#fbbf24` | 警告、注意事项           |
| 玫红（危险/风险） | `--rose`           | `#fb7185` | 高风险、错误、必须处理   |
| 橙色（集成/过渡） | `--orange`         | `#fb923c` | 中间件、集成层           |

**颜色背景变体**（用于卡片填充）：

```css
--cyan-bg:    rgba(8,51,68,0.4)
--emerald-bg: rgba(6,78,59,0.4)
--violet-bg:  rgba(76,29,149,0.4)
--amber-bg:   rgba(120,53,15,0.25)
--rose-bg:    rgba(136,19,55,0.25)
--orange-bg:  rgba(124,45,18,0.25)
```

### 字体

```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;600;700&display=swap" rel="stylesheet">
```

- 正文：`'Noto Sans SC'`（中文内容优先）
- 数字/代码/标签：`'JetBrains Mono'`（所有数字、英文标签、角标）

---

## 二、整体页面结构

```
.layout (display:flex)
├── .sidebar (fixed, 260px)            ← 左侧固定导航
└── .main (margin-left:260px)          ← 右侧主内容区
    ├── .report-header                 ← 报告标题区
    ├── .section#overview              ← 第01节：执行摘要
    ├── .section#system                ← 第02节：...
    ├── ...                            ← 更多章节
    └── .report-footer                 ← 页脚
```

---

## 三、组件库

### 3.1 侧边栏 `.sidebar`

```html
<nav class="sidebar">
  <div class="sidebar-logo">
    <div class="badge"><div class="pulse"></div>报告类型标签</div>
    <h2>报告标题</h2>
    <p>英文副标题 · 年份</p>
  </div>
  <!-- 每个导航分组 -->
  <div class="nav-section">
    <div class="nav-section-title">分组标题</div>
    <a href="#section-id" class="nav-item active">
      <div class="nav-dot" style="background:#22d3ee"></div>章节名
    </a>
    <a href="#section-id2" class="nav-item">
      <div class="nav-dot" style="background:#34d399"></div>章节名2
    </a>
  </div>
</nav>
```

**规则**：

- 第一个 `nav-item` 默认加 `active` 类
- `nav-dot` 颜色与对应章节的 `section-number` 颜色一致
- 分组标题用全大写英文，如 `概况`、`技术分析`、`问题与风险`

---

### 3.2 报告标题区 `.report-header`

```html
<div class="report-header">
  <!-- 元标签行：报告类型、状态、密级等 -->
  <div class="report-meta">
    <span class="meta-tag cyan">AS-IS 现状分析</span>
    <span class="meta-tag amber">⚠ 注意事项</span>
    <span class="meta-tag rose">紧急程度</span>
    <span class="meta-tag violet">目标/里程碑</span>
    <span class="meta-tag orange">密级</span>
  </div>
  <h1>报告主标题</h1>
  <p class="subtitle">组织名称 · 项目名称 · 信息来源</p>
  <!-- 4个关键数字统计 -->
  <div class="stats-row">
    <div class="stat-card">
      <div class="stat-value" style="color:#fbbf24">数值</div>
      <div class="stat-label">指标名</div>
    </div>
    <!-- 重复3次 -->
  </div>
</div>
```

**`meta-tag` 颜色语义**：

- `cyan` → 报告类型（AS-IS/TO-BE/评估等）
- `amber` → 警告/注意
- `rose` → 状态/紧急
- `violet` → 目标/截止日期
- `orange` → 密级/保密程度

**`stat-card` 颜色建议**：4个统计数字用不同颜色区分，建议 `amber` / `rose` / `violet` / `cyan`。

---

### 3.3 章节头 `.section-header`

```html
<div class="section" id="section-id">
  <div class="section-header">
    <div class="section-number [color-class]">01</div>
    <div>
      <div class="section-title">章节标题</div>
      <div class="section-subtitle">English Subtitle · Keywords</div>
    </div>
  </div>
  <!-- 章节内容 -->
</div>
```

**`section-number` 颜色类**（根据章节性质选择）：

| 类名               | 颜色 | 适用章节               |
| ------------------ | ---- | ---------------------- |
| （默认，无附加类） | cyan | 概述、摘要             |
| `.emerald`       | 绿色 | 系统概况、正常状态描述 |
| `.amber`         | 琥珀 | 技术架构、注意事项     |
| `.orange`        | 橙色 | 技术栈、集成分析       |
| `.violet`        | 紫色 | 数据分析、统计         |
| `.rose`          | 玫红 | 痛点、风险、问题       |

---

### 3.4 高亮框 `.highlight-box`

用于章节内的关键结论、重要提示。

```html
<!-- 默认 cyan 左边框 -->
<div class="highlight-box">
  <p>关键内容 <strong style="color:#fb7185">强调文字</strong></p>
</div>

<!-- 其他颜色变体 -->
<div class="highlight-box amber">...</div>
<div class="highlight-box rose">...</div>
<div class="highlight-box emerald">...</div>
<div class="highlight-box violet">...</div>
```

**使用规则**：每个章节开头可用 1 个 highlight-box 给出该节核心结论，章节末尾可用 1 个特殊颜色的 highlight-box 给出特别提示。

---

### 3.5 信息卡片网格

```html
<!-- 2列网格 -->
<div class="cards-2">
  <div class="info-card">
    <div class="info-card-header">
      <div class="info-card-icon" style="background:#22d3ee"></div>
      <h4>卡片标题</h4>
    </div>
    <ul>
      <li>条目一：<strong>关键值</strong></li>
      <li>条目二：<strong>关键值</strong></li>
    </ul>
    <!-- 可选底部标签 -->
    <span class="tag" style="background:rgba(34,211,238,0.15);color:#22d3ee">标签文字</span>
  </div>
</div>

<!-- 3列、4列分别用 cards-3 / cards-4 -->
```

**卡片图标颜色**：与 `section-number` 颜色保持一致；同一章节内多卡片可用不同颜色区分。

---

### 3.6 数据表格

```html
<table>
  <thead>
    <tr>
      <th>列名1</th>
      <th>列名2</th>
      <th>状态列</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="td-name">主要名称</td>
      <td>普通内容</td>
      <td><span class="badge rose">高风险</span></td>
    </tr>
  </tbody>
</table>
```

**badge 颜色语义**（用于表格内状态标注）：

| 类名              | 含义                       |
| ----------------- | -------------------------- |
| `badge rose`    | 高风险 / 必须处理 / 高影响 |
| `badge amber`   | 中等风险 / 注意 / 中影响   |
| `badge emerald` | 正常 / 已完成 / 低影响     |
| `badge cyan`    | 进行中 / 信息              |
| `badge violet`  | 分析数据                   |
| `badge orange`  | 集成/过渡状态              |
| `badge slate`   | 低优先级 / 不适用          |

---

### 3.7 风险条目 `.risk-item`

适用于痛点分析、问题列举等需要带严重程度标注的场景。

```html
<div class="risk-item">
  <div class="risk-level risk-high">极高</div>
  <div class="risk-content">
    <h5>风险/痛点标题</h5>
    <p>详细描述，可包含 <strong style="color:#fb7185">强调关键词</strong>。</p>
  </div>
</div>

<!-- 等级类：risk-high（玫红） / risk-medium（琥珀） / risk-low（绿色） -->
<!-- 等级文字：极高 / 高 / 中等 / 低 -->
```

---

### 3.8 时间轴 `.timeline`

适用于迁移计划、项目阶段、历史沿革。

```html
<div class="timeline">
  <!-- 已完成阶段 -->
  <div class="timeline-item done">
    <div class="timeline-phase">阶段一 · 2025年</div>
    <div class="timeline-title">阶段名称</div>
    <div class="timeline-desc">描述文字。<span class="badge emerald">已完成</span></div>
  </div>
  <!-- 进行中阶段（圆点会闪烁） -->
  <div class="timeline-item in-progress">
    <div class="timeline-phase">阶段二 · 2026年</div>
    <div class="timeline-title">阶段名称</div>
    <div class="timeline-desc">描述文字。<span class="badge amber">进行中</span></div>
  </div>
  <!-- 未来阶段 -->
  <div class="timeline-item">
    <div class="timeline-phase">阶段三 · 2027年</div>
    <div class="timeline-title">阶段名称</div>
    <div class="timeline-desc">描述文字。</div>
  </div>
</div>
```

---

### 3.9 对比表（AS-IS vs TO-BE）

用于现状与目标的对比展示。

```html
<div class="compare-header">
  <div>对比维度</div>
  <div style="color:#fb7185">现状（AS-IS）</div>
  <div style="color:#34d399">目标（TO-BE）</div>
</div>
<div class="compare-row">
  <div>维度名称</div>
  <div class="bad">现状值（问题点）</div>
  <div class="good">目标值（改进点）</div>
</div>
<!-- 最后一行加 style="border-radius:0 0 8px 8px" -->
<div class="compare-row" style="border-radius:0 0 8px 8px">
  <div>最后维度</div>
  <div>现状</div>
  <div class="good">目标</div>
</div>
```

---

## 四、完整 CSS 样式块

将以下 CSS 完整嵌入 `<style>` 标签：

```css
:root {
  --bg: #020617; --surface: #0f172a; --border: #1e293b; --border-light: #334155;
  --text-primary: #f8fafc; --text-secondary: #e2e8f0; --text-muted: #94a3b8; --text-dim: #475569;
  --cyan: #22d3ee; --emerald: #34d399; --violet: #a78bfa; --amber: #fbbf24; --rose: #fb7185; --orange: #fb923c;
  --cyan-bg: rgba(8,51,68,0.4); --emerald-bg: rgba(6,78,59,0.4); --violet-bg: rgba(76,29,149,0.4);
  --amber-bg: rgba(120,53,15,0.25); --rose-bg: rgba(136,19,55,0.25); --orange-bg: rgba(124,45,18,0.25);
}
* { margin: 0; padding: 0; box-sizing: border-box; }
body { background: var(--bg); color: var(--text-secondary); font-family: 'Noto Sans SC', sans-serif; font-size: 14px; line-height: 1.7; }
.layout { display: flex; min-height: 100vh; }
.sidebar { width: 260px; background: var(--surface); border-right: 1px solid var(--border); padding: 24px 0; position: fixed; height: 100vh; overflow-y: auto; z-index: 100; }
.sidebar-logo { padding: 0 20px 20px; border-bottom: 1px solid var(--border); margin-bottom: 16px; }
.sidebar-logo .badge { display: inline-flex; align-items: center; gap: 6px; background: rgba(34,211,238,0.1); border: 1px solid rgba(34,211,238,0.3); border-radius: 12px; padding: 3px 10px; font-size: 10px; color: var(--cyan); font-family: 'JetBrains Mono', monospace; margin-bottom: 8px; }
.pulse { width: 6px; height: 6px; background: var(--cyan); border-radius: 50%; animation: pulse 2s infinite; }
@keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:.5;transform:scale(.8)} }
.sidebar-logo h2 { font-size: 13px; font-weight: 600; color: var(--text-primary); line-height: 1.4; }
.sidebar-logo p { font-size: 10px; color: var(--text-dim); font-family: 'JetBrains Mono', monospace; margin-top: 2px; }
.nav-section { margin-bottom: 8px; }
.nav-section-title { padding: 4px 20px; font-size: 9px; font-weight: 600; color: var(--text-dim); font-family: 'JetBrains Mono', monospace; letter-spacing: 1px; text-transform: uppercase; }
.nav-item { display: flex; align-items: center; gap: 8px; padding: 8px 20px; font-size: 12px; color: var(--text-muted); cursor: pointer; text-decoration: none; transition: all 0.15s; border-left: 2px solid transparent; }
.nav-item:hover { color: var(--text-primary); background: rgba(255,255,255,0.03); }
.nav-item.active { color: var(--cyan); border-left-color: var(--cyan); background: var(--cyan-bg); }
.nav-dot { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; }
.main { margin-left: 260px; padding: 40px 48px; max-width: 1100px; }
.report-header { background: linear-gradient(135deg, var(--surface) 0%, rgba(8,51,68,0.3) 100%); border: 1px solid var(--border); border-radius: 16px; padding: 40px; margin-bottom: 40px; position: relative; overflow: hidden; }
.report-header::before { content: ''; position: absolute; top: 0; right: 0; width: 300px; height: 300px; background: radial-gradient(circle, rgba(34,211,238,0.05) 0%, transparent 70%); pointer-events: none; }
.report-meta { display: flex; gap: 12px; margin-bottom: 20px; flex-wrap: wrap; }
.meta-tag { display: inline-flex; align-items: center; gap: 4px; padding: 4px 12px; border-radius: 20px; font-size: 11px; font-family: 'JetBrains Mono', monospace; }
.meta-tag.cyan { background: rgba(34,211,238,0.1); border: 1px solid rgba(34,211,238,0.3); color: var(--cyan); }
.meta-tag.amber { background: rgba(251,191,36,0.1); border: 1px solid rgba(251,191,36,0.3); color: var(--amber); }
.meta-tag.rose { background: rgba(251,113,133,0.1); border: 1px solid rgba(251,113,133,0.3); color: var(--rose); }
.meta-tag.violet { background: rgba(167,139,250,0.1); border: 1px solid rgba(167,139,250,0.3); color: var(--violet); }
.meta-tag.orange { background: rgba(251,146,60,0.1); border: 1px solid rgba(251,146,60,0.3); color: var(--orange); }
.report-header h1 { font-size: 30px; font-weight: 700; color: var(--text-primary); letter-spacing: -0.5px; margin-bottom: 8px; }
.report-header .subtitle { font-size: 14px; color: var(--text-muted); margin-bottom: 24px; }
.stats-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }
.stat-card { background: rgba(255,255,255,0.03); border: 1px solid var(--border); border-radius: 10px; padding: 16px; text-align: center; }
.stat-value { font-size: 24px; font-weight: 700; font-family: 'JetBrains Mono', monospace; margin-bottom: 4px; }
.stat-label { font-size: 11px; color: var(--text-muted); }
.section { margin-bottom: 48px; }
.section-header { display: flex; align-items: center; gap: 12px; margin-bottom: 24px; padding-bottom: 12px; border-bottom: 1px solid var(--border); }
.section-number { width: 28px; height: 28px; background: var(--cyan-bg); border: 1px solid rgba(34,211,238,0.3); border-radius: 6px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 700; color: var(--cyan); font-family: 'JetBrains Mono', monospace; flex-shrink: 0; }
.section-number.amber { background: var(--amber-bg); border-color: rgba(251,191,36,0.3); color: var(--amber); }
.section-number.rose { background: var(--rose-bg); border-color: rgba(251,113,133,0.3); color: var(--rose); }
.section-number.violet { background: var(--violet-bg); border-color: rgba(167,139,250,0.3); color: var(--violet); }
.section-number.emerald { background: var(--emerald-bg); border-color: rgba(52,211,153,0.3); color: var(--emerald); }
.section-number.orange { background: var(--orange-bg); border-color: rgba(251,146,60,0.3); color: var(--orange); }
.section-title { font-size: 18px; font-weight: 600; color: var(--text-primary); }
.section-subtitle { font-size: 12px; color: var(--text-muted); font-family: 'JetBrains Mono', monospace; margin-top: 2px; }
h2 { font-size: 15px; font-weight: 600; color: var(--text-primary); margin: 24px 0 12px; }
h3 { font-size: 13px; font-weight: 600; color: var(--text-secondary); margin: 16px 0 8px; }
p { color: var(--text-muted); margin-bottom: 12px; line-height: 1.8; }
.highlight-box { background: var(--surface); border: 1px solid var(--border); border-left: 3px solid var(--cyan); border-radius: 0 8px 8px 0; padding: 16px 20px; margin: 16px 0; }
.highlight-box.amber { border-left-color: var(--amber); }
.highlight-box.rose { border-left-color: var(--rose); }
.highlight-box.violet { border-left-color: var(--violet); }
.highlight-box.emerald { border-left-color: var(--emerald); }
.highlight-box p { margin: 0; }
.cards-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin: 16px 0; }
.cards-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin: 16px 0; }
.cards-4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 14px; margin: 16px 0; }
@media(max-width:900px) { .cards-2,.cards-3,.cards-4 { grid-template-columns:1fr; } }
.info-card { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 18px; }
.info-card-header { display: flex; align-items: center; gap: 8px; margin-bottom: 12px; }
.info-card-icon { width: 8px; height: 8px; border-radius: 50%; }
.info-card h4 { font-size: 13px; font-weight: 600; color: var(--text-primary); }
.info-card ul { list-style: none; }
.info-card li { font-size: 12px; color: var(--text-muted); padding: 3px 0; font-family: 'JetBrains Mono', monospace; }
.info-card li strong { color: var(--text-secondary); font-weight: 500; }
.info-card .tag { display: inline-block; padding: 1px 6px; border-radius: 3px; font-size: 10px; font-family: 'JetBrains Mono', monospace; margin-top: 6px; }
table { width: 100%; border-collapse: collapse; margin: 16px 0; font-size: 12px; }
thead tr { background: rgba(30,41,59,0.8); }
th { text-align: left; padding: 10px 14px; color: var(--text-muted); font-weight: 600; font-family: 'JetBrains Mono', monospace; font-size: 11px; border-bottom: 1px solid var(--border); }
td { padding: 10px 14px; color: var(--text-muted); border-bottom: 1px solid rgba(30,41,59,0.5); vertical-align: top; }
tr:hover td { background: rgba(255,255,255,0.02); }
.td-name { color: var(--text-secondary); font-weight: 500; }
.risk-item { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 14px 16px; margin-bottom: 10px; display: flex; gap: 12px; align-items: flex-start; }
.risk-level { flex-shrink: 0; width: 56px; padding: 3px 0; border-radius: 4px; font-size: 10px; font-weight: 700; font-family: 'JetBrains Mono', monospace; text-align: center; }
.risk-high { background: rgba(251,113,133,0.2); color: var(--rose); border: 1px solid rgba(251,113,133,0.3); }
.risk-medium { background: rgba(251,191,36,0.2); color: var(--amber); border: 1px solid rgba(251,191,36,0.3); }
.risk-low { background: rgba(52,211,153,0.2); color: var(--emerald); border: 1px solid rgba(52,211,153,0.3); }
.risk-content h5 { font-size: 13px; font-weight: 600; color: var(--text-primary); margin-bottom: 4px; }
.risk-content p { font-size: 12px; color: var(--text-muted); margin: 0; }
.timeline { position: relative; padding-left: 32px; }
.timeline::before { content: ''; position: absolute; left: 10px; top: 8px; bottom: 8px; width: 2px; background: linear-gradient(to bottom, var(--cyan), var(--violet)); opacity: 0.3; }
.timeline-item { position: relative; margin-bottom: 24px; }
.timeline-item::before { content: ''; position: absolute; left: -26px; top: 6px; width: 10px; height: 10px; border-radius: 50%; background: var(--cyan); border: 2px solid var(--bg); }
.timeline-item.done::before { background: var(--emerald); }
.timeline-item.in-progress::before { background: var(--amber); animation: pulse 2s infinite; }
.timeline-phase { font-size: 10px; color: var(--text-dim); font-family: 'JetBrains Mono', monospace; margin-bottom: 4px; }
.timeline-title { font-size: 13px; font-weight: 600; color: var(--text-primary); margin-bottom: 6px; }
.timeline-desc { font-size: 12px; color: var(--text-muted); }
.badge { display: inline-flex; align-items: center; gap: 4px; padding: 2px 8px; border-radius: 4px; font-size: 10px; font-family: 'JetBrains Mono', monospace; margin: 2px; }
.badge.cyan { background: rgba(34,211,238,0.15); color: var(--cyan); }
.badge.emerald { background: rgba(52,211,153,0.15); color: var(--emerald); }
.badge.amber { background: rgba(251,191,36,0.15); color: var(--amber); }
.badge.rose { background: rgba(251,113,133,0.15); color: var(--rose); }
.badge.violet { background: rgba(167,139,250,0.15); color: var(--violet); }
.badge.orange { background: rgba(251,146,60,0.15); color: var(--orange); }
.badge.slate { background: rgba(100,116,139,0.2); color: var(--text-muted); }
.compare-header { display: grid; grid-template-columns: 1.2fr 1fr 1fr; gap: 1px; background: var(--border); border-radius: 8px 8px 0 0; overflow: hidden; }
.compare-header > div { padding: 10px 16px; background: rgba(30,41,59,0.8); font-size: 11px; font-weight: 600; font-family: 'JetBrains Mono', monospace; color: var(--text-muted); text-align: center; }
.compare-row { display: grid; grid-template-columns: 1.2fr 1fr 1fr; gap: 1px; background: var(--border); }
.compare-row:last-child { border-radius: 0 0 8px 8px; overflow: hidden; }
.compare-row > div { padding: 10px 16px; background: var(--surface); font-size: 12px; color: var(--text-muted); font-family: 'JetBrains Mono', monospace; }
.compare-row > div:first-child { color: var(--text-secondary); font-weight: 500; }
.compare-row .bad { color: var(--rose); }
.compare-row .good { color: var(--emerald); }
.report-footer { border-top: 1px solid var(--border); padding: 24px 0; display: flex; justify-content: space-between; align-items: center; color: var(--text-dim); font-size: 11px; font-family: 'JetBrains Mono', monospace; }
```

---

## 五、常用章节模式

根据报告类型，从以下模式中选择组合：

| 章节名          | 主要组件                                     | 适用类型     |
| --------------- | -------------------------------------------- | ------------ |
| 执行摘要        | highlight-box + cards-3                      | 所有报告必有 |
| 系统/对象概况   | cards-2/4 + table                            | 现状分析     |
| 技术架构现状    | highlight-box + table                        | 技术类报告   |
| 技术栈/能力分析 | cards-4 + highlight-box                      | 技术类报告   |
| 集成/接口分析   | cards-2 + table                              | 集成类报告   |
| 业务域划分      | cards-3/4                                    | 业务分析报告 |
| 数据现状分析    | cards-2 + table + cards-3                    | 数据迁移报告 |
| 痛点深度分析    | h2分组 + risk-item列表                       | 问题分析类   |
| 风险评估矩阵    | table（含 badge）                            | 风险评估类   |
| 迁移/实施策略   | highlight-box + timeline + compare           | 规划类报告   |
| 结论与建议      | highlight-box + cards-3 + highlight-box.rose | 所有报告必有 |

---

## 六、创作流程

当用户提供分析材料，按以下步骤生成报告：

### Step 1 — 理解报告需求

读取用户提供的材料，明确：

- **分析对象**：系统/业务/项目/方案
- **报告目的**：现状分析 / 风险评估 / 可行性分析 / 对比评估
- **关键维度**：抽取3-6个核心分析维度
- **关键数字**：找出4个可以放入 `stats-row` 的关键指标

### Step 2 — 规划章节结构

设计 sidebar 导航，一般结构：

```
概况分组：执行摘要、对象概况
核心分析分组：根据报告类型决定（2-4个技术/业务章节）
问题分组：痛点分析、风险评估
建议分组：改进策略/迁移方案、结论建议
```

### Step 3 — 选择章节颜色

为每个章节分配 `section-number` 颜色：

- 越靠前（概述类）→ cyan/emerald
- 技术深入类 → amber/orange
- 数据分析类 → violet
- 问题风险类 → rose
- 末尾结论类 → cyan 或默认

### Step 4 — 填充内容

- 每个章节至少含 1 个 `highlight-box` 给出该节核心结论
- 结构化信息优先用卡片或表格，避免大段纯文字
- 痛点、风险用 `risk-item` 列举，控制每组 2-5 条
- 有时序的内容用 `timeline`
- 有对比的结论用 `compare-header + compare-row`

### Step 5 — 输出规范

- 单个 `.html` 文件，完整嵌入 CSS 和 JS（无外部依赖，Google Fonts 除外）
- 文件名格式：`[主题]分析报告.html` 或 `[主题]评估报告.html`
- 保存路径：询问用户或根据工作区上下文判断

---

## 七、典型章节配色方案参考

### 现状分析报告（AS-IS）

```
01 执行摘要      → cyan（默认）
02 系统概况      → emerald
03 技术架构      → amber
04 技术栈分析    → orange
05 集成架构      → violet
06 业务域分析    → emerald
07 数据现状      → violet
08 痛点分析      → rose
09 风险评估      → rose
10 迁移策略      → cyan（默认）
11 结论建议      → cyan（默认）
```

### 技术评估报告

```
01 摘要          → cyan
02 评估对象      → emerald
03 功能评估      → amber
04 性能评估      → violet
05 安全评估      → rose
06 成本分析      → orange
07 结论建议      → cyan
```

### 业务可行性分析

```
01 摘要          → cyan
02 业务背景      → emerald
03 业务流程分析  → amber
04 资源需求      → violet
05 风险评估      → rose
06 收益预测      → emerald
07 实施建议      → cyan
```

---

## 八、禁止事项

- **禁止**使用外部 CSS 框架（Bootstrap、Tailwind CDN 等）
- **禁止**将大段文字作为 `<p>` 堆砌，应拆分为卡片/列表
- **禁止**在同一章节内连续使用超过 3 个同类组件（如连续 3 个 highlight-box）
- **禁止**侧边栏超过 15 个导航项，超过时合并相近章节
- **禁止**在 `stats-row` 的 4 个卡片中用相同颜色
