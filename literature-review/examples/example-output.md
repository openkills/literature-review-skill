# 文献综述：蛋白质结构预测与序列建模（2 篇文献示例）

> 本示例假设用户按顺序提供了两篇文献：① DOI `10.1038/s41586-021-03819-2`；② arXiv 链接 `https://arxiv.org/abs/1706.03762`。

## 一、文献综述（可直接用于论文）

[1] Jumper 等人提出了 AlphaFold2，通过基于注意力的网络架构将蛋白质结构预测精度提升至与实验测定相当的水平，在 CASP14 评估中取得中位 GDT_TS 92.4 的成绩；该工作解决了困扰结构生物学界五十年的蛋白质折叠难题，为药物研发、蛋白质设计与结构生物学研究开辟了新范式。
[2] Vaswani 等人提出了 Transformer 架构，完全依靠自注意力机制完成序列转换任务，在 WMT14 英德翻译任务上取得 28.4 BLEU 的当时最优结果；该工作摆脱了循环网络的顺序计算限制、大幅提升训练并行度，成为大规模预训练语言模型的基石。

## 二、逐篇总结

### [1] Highly accurate protein structure prediction with AlphaFold

- 中文：本文提出了 AlphaFold2，将蛋白质结构预测精度提升到与实验测定相当的水平；该工作解决了困扰结构生物学界五十年的难题，为药物研发和蛋白质设计开辟了新范式。
- English: This paper proposes AlphaFold2, achieving experimental-level accuracy in protein structure prediction; the work resolves a 50-year grand challenge in biology and opens a new paradigm for drug discovery and protein design.
- 工作与贡献：提出基于注意力机制（Evoformer 与结构模块）的 AlphaFold2 网络，在 CASP14 竞赛中取得中位 GDT_TS 92.4、95% 残基主链 RMSD95 约 0.96 Å 的预测精度，达到与实验解析结构相当的准确度。
- 意义与价值：解决了长期被视为生物学"大挑战"的蛋白质折叠问题，使高精度结构预测成为常规手段，深刻改变了结构生物学、药物研发与蛋白质工程的研究范式。

### [2] Attention Is All You Need

- 中文：本文提出了完全基于注意力机制的 Transformer 架构，在机器翻译上取得当时最优结果；该工作去除了循环与卷积结构、支持高度并行训练，成为后续大规模预训练语言模型的基石。
- English: This paper proposes the Transformer, an architecture built entirely on attention that achieves state-of-the-art machine translation results; the work removes recurrence and convolution to enable highly parallel training and lays the foundation for large-scale pretrained language models.
- 工作与贡献：提出完全依赖自注意力机制的 Transformer 编码-解码架构，在 WMT14 英德翻译上取得 28.4 BLEU（超过当时最佳集成模型 2 BLEU 以上），英法翻译上以单模型取得 41.8 BLEU，且仅在 8 块 GPU 上训练 3.5 天。
- 意义与价值：证明了注意力机制可以完全取代循环与卷积结构，兼顾效果与训练效率，直接催生了 BERT、GPT 等大规模预训练模型，是现代自然语言处理与大模型时代的奠基性工作。（基于摘要与公开结果）

## 三、参考文献

[1] Jumper J, Evans R, Pritzel A, et al. Highly accurate protein structure prediction with AlphaFold[J]. Nature, 2021, 596(7873): 583-589.
[2] Vaswani A, Shazeer N, Parmar N, et al. Attention is all you need[EB/OL]. arXiv preprint arXiv:1706.03762, 2017.

> 注：两篇文献的期刊名均由用户输入确认后写入 bibtex（skill 会先展示元数据查到的结果作为建议）。文献 [2] 在 arXiv 上未登记期刊信息，若用户告知发表于 NeurIPS 2017，则参考文献与 bibtex 按用户输入改写为会议论文格式。

## 四、BibTeX

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
