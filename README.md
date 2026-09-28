# literature-review-skill

一个 [ZCode](https://zcode.ai) skill：把文献（DOI / 论文链接 / 本地论文文件）一键变成可直接放进论文的综述材料。

## 功能

给定一篇或多篇文献（DOI、arXiv/出版商链接、本地 PDF/Word/tex 文件），skill 会：

1. **总结工作与贡献，评价意义与价值** —— 基于论文真实内容（全文优先，只有摘要时注明），不编造数据；
2. **中英文一句话总结** —— 前半句是工作和贡献，后半句是意义和价值；
3. **生成参考文献级 bibtex** —— 字段齐全（作者、标题、期刊/会议、年份、卷期页、DOI）；期刊信息缺失（如预印本）时**必须由用户输入期刊**，绝不编造；
4. **组装 [n] 编号式文献综述** —— 每条前半句工作和贡献、后半句意义和价值，编号与参考文献、bibtex 一一对应，可直接粘贴进论文；
5. **双通道输出** —— 综述、参考文献列表、bibtex 同时写入 Markdown 文件和对话框。

## 安装

把 `literature-review/` 文件夹复制到 ZCode 的 skill 目录：

```bash
# 用户级（所有项目可用）
git clone https://github.com/openkills/literature-review-skill.git
mkdir -p ~/.agents/skills
cp -r literature-review-skill/literature-review ~/.agents/skills/
```

重启 ZCode 会话后生效。

## 使用示例

```
帮我总结这篇文献并生成综述：10.1038/s41586-021-03819-2

把这两篇做成文献综述：
- https://arxiv.org/abs/1706.03762
- D:\papers\alphafold.pdf
```

## 输出示例

见 [literature-review/examples/example-output.md](literature-review/examples/example-output.md)。

## 元数据来源

- 期刊论文：[CrossRef API](https://api.crossref.org/works/<DOI>)（一次拿到标题、作者、期刊、卷期页、DOI）
- 预印本：[arXiv API](https://export.arxiv.org/api/query?id_list=<id>)
- 本地文件：直接读取，缺元数据时询问用户

## License

[MIT](LICENSE)
