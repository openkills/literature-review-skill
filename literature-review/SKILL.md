---
name: literature-review
description: 文献综述生成器：根据用户提供的文献（DOI、论文链接、arXiv 链接、本地 PDF/Word/tex 论文文件）总结每篇论文的主要工作与贡献并评价其意义与价值，生成中英文一句话总结、参考文献级 bibtex（期刊信息缺失时必须向用户询问）、以及可直接粘贴进论文的 [1][2]… 编号式文献综述，并将综述、参考文献列表与 bibtex 同时输出到 Markdown 文件和对话框。当用户提到「文献综述」「总结这篇/这几篇论文」「总结文献」「生成参考文献」「bibtex」「引用格式」「literature review」「summarize this paper」「generate citations」等需求时触发——即使用户没有说"文献综述"三个字，只要涉及总结学术论文并生成可引用的材料，就应触发本 skill。
---

# 文献综述生成器

把用户提供的文献（DOI / 论文链接 / 本地论文文件）转化为一套可直接放进论文的综述材料：逐篇总结工作与贡献并评价意义与价值、中英文一句话总结、参考文献级 bibtex、[n] 编号式文献综述段落，全部同时输出到 Markdown 文件和对话框。

按下面七步顺序执行。多篇文献时逐篇处理，编号在所有环节保持一致。

## 第 1 步：解析文献输入

用户可能给三种输入，识别后分别处理：

- **DOI**：形如 `10.xxxx/xxxx`（可能带 `https://doi.org/` 前缀）。用 WebFetch 抓 `https://api.crossref.org/works/<DOI>`，一次拿到标题、作者、期刊、年份、卷期页、出版商等全部元数据。
- **链接**：arXiv 链接（`arxiv.org/abs/<id>`）→ 用 WebFetch 抓 `https://export.arxiv.org/api/query?id_list=<id>` 拿结构化元数据，再抓 abs 页面或 PDF 拿摘要与正文；出版商页面（Nature、IEEE、Springer 等）→ WebFetch 直接读取页面内容；`doi.org` 链接按 DOI 处理。
- **本地文件**：PDF 用 Read 工具直接读取；`.docx` 先用 docx 工具链转出文本；`.tex` / `.md` 直接读。元数据（期刊、卷期页）从文件首页或页眉提取，提取不到的按第 5 步向用户询问。
- 用户口述的文献信息：直接采用，缺什么字段就问用户要什么。

用户给了多篇文献时，为每一篇独立执行第 2–5 步。

## 第 2 步：获取元数据与全文

- **元数据**（作者、期刊、年份、卷期页、DOI）只从权威源获取，优先级：CrossRef > arXiv API > 出版商页面 > 本地文件首页。禁止凭记忆填写卷、期、页码——这些字段错一个就会让参考文献不可用。
- **内容**（摘要、方法、结果）尽量读全文：arXiv PDF、出版商 HTML、用户给的本地文件。只能拿到摘要时，基于摘要总结，并在输出中注明「（基于摘要）」。
- 某篇文献实在拿不到内容：明确告诉用户，请用户补充文件或链接，不要硬写。

## 第 3 步：总结工作与贡献、评价意义与价值

每篇写两段，内容必须来自论文本身：

- **工作与贡献**（做了什么）：提出的方法 / 模型 / 系统 / 理论 / 数据集是什么，核心创新点在哪，关键实验结果如何。有具体数字就写具体数字（如「在 WMT14 英德翻译上取得 28.4 BLEU」），不要空泛地说「效果显著」。
- **意义与价值**（为什么重要）：解决了领域的什么问题，是开创性、方法论突破还是工程实用价值，对后续研究有什么启发或奠基作用。评价要克制、有依据，堆砌「里程碑」「划时代」这类词前先确认论文确实配得上。

## 第 4 步：中英文一句话总结

每篇各写一条中文一句话和一条英文一句话。**前半句必须是工作和贡献，后半句必须是意义和价值**，各自只允许一句话：

```text
中文：本文提出了 AlphaFold2，将蛋白质结构预测精度提升到与实验测定相当的水平；该工作解决了困扰结构生物学界五十年的难题，为药物研发和蛋白质设计开辟了新范式。
English: This paper proposes AlphaFold2, achieving experimental-level accuracy in protein structure prediction; the work resolves a 50-year grand challenge in biology and opens a new paradigm for drug discovery and protein design.
```

## 第 5 步：生成参考文献级 bibtex

- **类型选择**：期刊论文用 `@article`，会议论文用 `@inproceedings`，未正式发表的预印本用 `@misc`。
- **字段必须齐全到参考文献级别**：`author`（`姓, 名 and 姓, 名` 格式）、`title`、`journal` / `booktitle`、`year`、`volume`、`number`、`pages`、`publisher`、`doi`，有 URL 就带 `url`。某个字段从元数据里拿不到时先问用户，宁缺勿错。
- **key 命名**：`<第一作者姓氏小写><年份><标题第一个关键词>`，如 `jumper2021alphafold`、`vaswani2017attention`。
- **期刊信息规则（本 skill 的硬性规则）**：元数据里带期刊名就直接用；论文是 arXiv 等预印本、或元数据里没有期刊字段时，**必须先问用户**：「这篇论文是否已正式发表？发表在哪个期刊或会议？卷、期、页码是多少（如果知道）？」拿到用户答复前不得编造期刊名；用户说未发表，就用 `@misc` + arXiv 字段。用户指定的期刊信息以用户为准。

```bibtex
@article{jumper2021alphafold,
  title     = {Highly accurate protein structure prediction with AlphaFold},
  author    = {Jumper, John and Evans, Richard and Pritzel, Alexander and others},
  journal   = {Nature},
  year      = {2021},
  volume    = {596},
  number    = {7873},
  pages     = {583--589},
  publisher = {Springer Science and Business Media LLC},
  doi       = {10.1038/s41586-021-03819-2}
}

@misc{vaswani2017attention,
  title         = {Attention Is All You Need},
  author        = {Vaswani, Ashish and Shazeer, Noam and Parmar, Niki and others},
  year          = {2017},
  eprint        = {1706.03762},
  archivePrefix = {arXiv},
  primaryClass  = {cs.CL}
}
```

## 第 6 步：组装 [n] 编号式文献综述

按用户给出文献的顺序编号，每条一段，**前半句写工作和贡献，后半句写意义和价值**，使整段可以直接粘贴进论文：

```text
[1] Jumper 等人提出了 AlphaFold2，将蛋白质结构预测精度提升至与实验测定相当的水平；该工作解决了困扰结构生物学界五十年的难题，为药物研发和蛋白质设计奠定了新的基础。
[2] Vaswani 等人提出了 Transformer 架构，完全依靠自注意力机制完成序列转换任务并在机器翻译上取得当时最优结果；该工作摆脱了循环网络的顺序计算限制，成为大规模预训练语言模型的基石。
```

`[n]` 在综述、参考文献列表、bibtex 三处必须严格一一对应。

## 第 7 步：双通道输出

同时完成两件事，缺一不可：

1. **写 Markdown 文件**：默认写到当前工作目录的 `literature-review-<YYYYMMDD>.md`；用户指定了路径或文件名就用用户的。文件按下面的模板组织。
2. **输出到对话框**：把文件的四部分内容完整渲染在回复里，不能只回一个文件路径了事。

### 输出文件模板

````markdown
# 文献综述：<主题或「N 篇文献」>

## 一、文献综述（可直接用于论文）

[1] ……
[2] ……

## 二、逐篇总结

### [1] <论文标题>
- 中文：……
- English: ……
- 工作与贡献：……
- 意义与价值：……

## 三、参考文献

[1] Jumper J, Evans R, Pritzel A, et al. Highly accurate protein structure prediction with AlphaFold[J]. Nature, 2021, 596(7873): 583-589.
[2] ……

## 四、BibTeX

（bibtex 代码块，顺序同上）
````

## 红线

1. 卷、期、页码、期刊名、作者名只能来自权威元数据源或用户输入，缺了就问，绝不编造。
2. 总结只基于论文真实内容；只有摘要时注明「（基于摘要）」，不推测未读到的实验细节。
3. `[n]` 编号在文献综述、参考文献列表、bibtex 三处必须一致。
4. 结果必须同时写入 md 文件和对话框，两者内容一致。
