# Interview PDF Review

一个用于复盘面试 PDF 的 Codex Skill，针对面试猫等产品导出的面试记录进行了优化。

它会从原始问答中提取证据，检查项目介绍是否具备有效的 STAR 结构，诊断回答中的内容与表达问题，生成不虚构经历的升级答案，并给出可执行的训练计划。

## 什么时候使用

适合以下场景：

- 上传面试猫或其他工具导出的面试 PDF，希望系统复盘。
- 分析自我介绍、实习经历或项目介绍是否符合 STAR。
- 诊断项目深挖题、行为题、技术题或业务开放题。
- 对照个人资料、岗位题库或历史面经评估回答。
- 将薄弱回答改写成可口述版本，并制定下一轮训练计划。

典型请求：

```text
使用 $interview-pdf-review 分析我上传的面试 PDF。
请重点检查项目介绍的 STAR 完整性，并参考我的本地知识库。
```

## 怎么使用

1. 将 `interview-pdf-review` 文件夹放入 Codex 可发现的 Skills 目录。
2. 在对话中调用 `$interview-pdf-review`，并上传面试 PDF。
3. 可选：提供目标公司、岗位、职级、面试轮次和本地知识库路径。
4. Skill 会输出面试地图、证据化诊断、STAR 检查、答案升级稿和训练计划。

如果 PDF 是扫描件，Skill 会尝试 OCR。若存在缺页、转录错误或说话人混淆，会降低相关判断的置信度，而不是补写不存在的内容。

## 知识库接口

知识库是可选的，不需要预先生成 embedding。

### 默认目录

默认知识库目录位于：

```text
interview-pdf-review/knowledge/
```

可以直接把 PDF、DOCX、Markdown、TXT、CSV、XLSX、网页导出或图片放入该目录。Skill 会先按文件名和文本相关性筛选，再读取与当前公司、岗位和问题相关的材料。

### 自定义目录

也可以在请求中传入任意可读的本地绝对路径：

```text
使用 $interview-pdf-review 复盘这份 PDF。
知识库目录是：/absolute/path/to/my-interview-knowledge
```

目录优先级为：

1. 当前请求明确给出的目录。
2. 用户确认继续使用的历史目录。
3. Skill 自带的 `knowledge/` 目录。

知识库不存在、为空或没有覆盖当前问题时，Skill 会回退到联网检索，优先查找公司/岗位官方信息、权威专业资料和多来源交叉验证的近期面经，并在复盘中列出来源。

## 目录结构

```text
interview-pdf-review/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
├── knowledge/
└── references/
    ├── knowledge-base.md
    ├── output-format.md
    ├── rubric.md
    └── star-project.md
```

`SKILL.md` 定义核心工作流；`references/` 保存评价量表、STAR 标准、知识库读取规则和输出结构；`knowledge/` 是用户资料入口。

## 评价边界

- 所有关键判断尽量绑定 PDF 页码或短证据。
- 不根据面试记录臆测录用结果或面试官未表达的动机。
- 不虚构候选人的职责、项目数据或业务结果。
- 网络面经只作为经验性参考，不视为公司官方评分标准。
