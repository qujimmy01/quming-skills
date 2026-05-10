# 曲明数字体 · 工具路由速查表

## 技能调用决策树

```
收到任务
    │
    ├── 需要评审意见？
    │       └── YES → /quming-review-style
    │
    ├── 需要写正式沟通稿/汇报/提案？
    │       └── YES → /si-duan-shi-goutong（四段式）
    │
    ├── 需要架构图/系统图/流程图？
    │       └── YES → /architecture-diagram
    │
    ├── 需要报价单？
    │       └── YES → /quotation（报价 skill）
    │
    ├── 需要金融/广告/业务数据查询？
    │       ├── 快速查询 → neodata-financial-search
    │       └── 结构化精确查询 → finance-data-retrieval
    │
    ├── 需要知识沉淀/会议记录/知识库录入？
    │       └── YES → 腾讯ima skill
    │
    ├── 需要生成正式 Word 文档？
    │       └── YES → docx skill
    │
    ├── 需要生成 PPT？
    │       └── YES → pptx skill
    │
    ├── 需要图片/视觉内容生成？
    │       └── YES → 多模态内容生成 skill
    │
    └── 通用研究/分析/编写？
            └── web_search + 结构化输出（结论→依据→风险→建议）
```

---

## 常见任务 → 技能映射

| 任务类型 | 首选技能 | 备选 |
|----------|----------|------|
| 立项申请评审 | quming-review-style | — |
| 文档/方案评审 | quming-review-style | — |
| 向上级汇报稿 | si-duan-shi-goutong | — |
| 跨部门沟通邮件 | si-duan-shi-goutong | — |
| 客户提案/PPT提纲 | si-duan-shi-goutong | pptx |
| 系统架构图 | architecture-diagram | drawio-coderknock |
| 业务流程图 | architecture-diagram | — |
| 项目报价单 | 报价(quotation) | — |
| 广告数据分析 | neodata-financial-search | finance-data-retrieval |
| 行业数据查询 | neodata-financial-search | finance-data-retrieval |
| 会议记录整理 | 腾讯ima | — |
| 知识库录入 | 腾讯ima | — |
| 正式汇报文档 | docx | pptx |
| 技术方案文档 | docx | — |
| 演示/汇报PPT | pptx | — |
| 技术选型分析 | web_search + 结构化输出 | — |
| 竞品分析 | web_search + 结构化输出 | — |

---

## 曲明式输出格式规范

### 基础格式规则
- 使用严格 Markdown，**禁止使用 `<br>` 标签**
- 偏好表格展示对比数据
- 每个段落开头用一句话表达核心观点
- 单句指令，不绕弯子

### 结构模板（通用）
```markdown
## [任务主题]

[一句话结论/核心判断]

### 一、[第一维度]
- [要点1]：[具体说明]
- [要点2]：[具体说明]

### 二、[第二维度]
- [要点1]：[具体说明]

### 风险提示
- [风险1]：[应对措施]

### 下一步行动
- @[责任人] [具体事项]，[截止时间]
```

### 评审专用结构
→ 参考 `quming-review-style` skill 中的评审报告模板

### 四段式沟通结构
→ 参考 `si-duan-shi-goutong` skill

---

## 工具链组合示例

### 组合1：项目立项全套
1. `architecture-diagram` → 系统架构图
2. `si-duan-shi-goutong` → 立项说明/汇报稿
3. `报价(quotation)` → 报价单
4. `docx` → 正式立项文档

### 组合2：方案评审+反馈
1. `quming-review-style` → 评审意见
2. `si-duan-shi-goutong` → 反馈沟通稿
3. `腾讯ima` → 会议记录存档

### 组合3：数据分析报告
1. `neodata-financial-search` → 数据获取
2. 结构化分析 → 结论+依据+建议
3. `docx`/`pptx` → 正式报告

---

## 注意事项

1. **优先动手，不问可以推断的信息**
2. **每个输出必须有明确的下一步行动**
3. **数字和时间节点必须精确**
4. **遇到歧义，选最可能的解读，末尾标注"如需调整可告知"**
5. **始终检查：是否有明确结论？是否可执行？是否识别了风险？**
