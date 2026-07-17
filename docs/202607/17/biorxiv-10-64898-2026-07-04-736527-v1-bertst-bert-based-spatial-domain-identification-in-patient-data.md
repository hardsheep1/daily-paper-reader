---
title: "BertST: BERT-based Spatial Domain Identification in Patient Data"
title_zh: BertST：基于BERT的患者数据空间域识别
authors: "Nnadi, G. O."
date: 2026-07-09
pdf: "https://www.biorxiv.org/content/10.64898/2026.07.04.736527v1.full.pdf"
tags: ["query:ac"]
score: 6.0
evidence: 利用BERT进行空间域识别，属于大型语言模型的聚类应用
tldr: 空间转录组学面临如何联合基因表达与空间信息划分组织区域的挑战。现有方法多基于图神经网络，而Transformer模型尚未充分探索。本文提出BertST，将空间转录组学转化为图到文本表示学习问题，通过构建多图表示和层级传播策略，利用BERT学习节点嵌入。实验表明，BertST在多个Visium数据集上优于或匹敌GNN方法，展现了Transformer在空间组学中的潜力。
source: biorxiv
selection_source: fresh_fetch
figures_json: "[{\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-04-736527-v1/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1693, \"height\": 554, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-04-736527-v1/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1708, \"height\": 386, \"label\": \"Table\"}, {\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-04-736527-v1/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 454, \"height\": 227, \"label\": \"Table\"}]"
motivation: 现有空间域识别方法多基于GNN，但Transformer在捕捉局部与长程依赖方面的潜力尚未被探索。
method: 构建空间邻接、剪枝基因表达和全连接基因表达的多图，通过随机游走生成序列，用BERT学习嵌入；采用层级多图传播策略逐步优化。
result: 在DLPFC和人类乳腺癌数据集上，BertST的ARI和AMI指标一致优于或匹敌ConST、CCST等方法。
conclusion: BertST证明Transformer能有效捕捉空间-分子依赖，为空间组学提供新范式。
---

## 摘要
空间转录组学能够研究基因表达在其天然组织背景下的情况，为了解细胞组织及微环境驱动的生物过程提供了关键见解。该领域的一个关键挑战是空间域识别，旨在通过联合利用基因表达和空间信息将组织划分为连贯的区域。现有方法主要基于图神经网络（GNN），而基于Transformer的方法，特别是双向编码器表示Transformer（BERT）模型，用于建模局部和长距离依赖关系的研究仍基本未探索。

在这项工作中，我们提出了用于空间转录组学的BERT（BertST），这是一个基于Transformer的框架，将空间转录组学重新表述为图到文本表示学习问题。基于BERTwalk范式，我们构建了一个任务特定的多图表示，整合了空间邻接、剪枝的基因表达相似性和全连接基因表达图。这种设计能够同时建模局部空间结构和全局分子关系。在这些图上的随机游走被视为序列，使得BERT模型能够学习上下文相关的节点嵌入。

为了进一步提高表示质量，我们引入了一种分层多图传播策略，其中嵌入精炼按顺序进行：首先在全连接图上捕获全局结构，然后在剪枝图上精炼分子关系，最后在空间图上强制局部平滑性。这种顺序确保了全局信息被有效分布并逐渐被生物学上有意义的邻域约束。

我们还通过利用PecanPy（一种快速且可扩展的node2vec实现）提高了计算效率，从而能够在密集图上高效生成随机游走。在多个10x Visium数据集（包括DLPFC和人乳腺癌）上的实验结果表明，BertST在调整兰德指数（ARI）和调整互信息（AMI）方面始终优于或匹配基于GNN的方法，如ConST、CCST和SpaceFlow。

总体而言，BertST通过有效捕获局部和长距离空间-分子依赖关系，突显了基于Transformer的架构在空间组学分析中的潜力，为传统的基于图的方法提供了一个有前景的替代方案。

## Abstract
AO_SCPLOWBSTRACTC_SCPLOWSpatial transcriptomics enables the study of gene expression within its native tissue context, providing critical insights into cellular organization and microenvironment-driven biological processes. A key challenge in this field is spatial domain identification, which aims to partition tissue into coherent regions by jointly leveraging gene expression and spatial information. Existing approaches are predominantly based on Graph Neural Networks (GNNs), and approach based on Transformers particularly, Bidirectional Encoder Reppresentation Transformer (BERT) model for modelling both local and long-range dependencies remains largely unexplored.

In this work, we propose BERT for Spatial Transcriptomics (BertST), a transformer-based framework that reformulates spatial transcriptomics as a graph-to-text representation learning problem. Building upon the BERTwalk paradigm, we construct a task-specific multi-graph representation integrating spatial adjacency, pruned gene-expression similarity, and a fully connected gene-expression graph. This design enables the modelling of both local spatial structure and global molecular relationships. Random walks over these graphs are treated as sequences, allowing a BERT model to learn contextualised node embeddings.

To further enhance representation quality, we introduce a hierarchical multi-graph propagation strategy, where embedding refinement is performed sequentially: first on the fully connected graph to capture global structure, followed by the pruned graph to refine molecular relationships, and finally on the spatial graph to enforce local smoothness. This ordering ensures that global information is effectively distributed and progressively constrained by biologically meaningful neighbourhoods.

We also improve computational efficiency by leveraging PecanPy, a fast and scalable implementation of node2vec, enabling efficient random walk generation on dense graphs. Experimental results on multiple 10x Visium datasets, including DLPFC and Human Breast Cancer, demonstrate that BertST consistently outperforms or matches GNN-based methods such as ConST, CCST, and SpaceFlow in terms of Adjusted Rand Index (ARI) and Adjusted Mutual Information (AMI).

Overall, BertST highlights the potential of transformer-based architectures for spatial omics analysis by effectively capturing both local and long-range spatial-molecular dependencies, offering a promising alternative to traditional graph-based methods.