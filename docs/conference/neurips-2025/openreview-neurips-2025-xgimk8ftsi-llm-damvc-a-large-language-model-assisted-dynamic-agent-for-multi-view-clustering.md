---
title: "LLM-DAMVC: A Large Language Model Assisted Dynamic Agent for Multi-View Clustering"
title_zh: LLM-DAMVC：大语言模型辅助的多视图聚类动态智能体
authors: "HaiMing Xu, Qianqian Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=xgiMK8FtSI"
tags: ["query:ac"]
score: 9.0
evidence: 利用大语言模型进行聚类
tldr: 针对多视图聚类中特征提取易受噪声影响且动态融合策略缺乏自适应性的问题，提出LLM-DAMVC框架，利用大语言模型作为动态智能体辅助聚类。该框架通过LLM增强特征表示，并自适应调整融合权重，在多个数据集上取得优异性能，展示了LLM在无监督聚类中的潜力。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1429, \"height\": 780, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1415, \"height\": 472, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1413, \"height\": 500, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1448, \"height\": 299, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2025-xgimk8ftsi/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1447, \"height\": 342, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-xgimk8ftsi/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1451, \"height\": 209, \"label\": \"Table\"}]"
motivation: 现有方法在特征域提取易受噪声影响，且静态融合策略无法应对数据分布变化。
method: 引入大语言模型构建动态智能体，自适应调整多视图融合权重，并增强特征表示。
result: 在多个多视图聚类基准上达到最先进性能，显著提升了聚类准确性和鲁棒性。
conclusion: 大语言模型能有效辅助无监督聚类任务，为结合语义信息进行数据分组提供了新思路。
---

## Abstract
Multi-view clustering integrates the consistency and complementarity of different views to achieve unsupervised data grouping. Existing multi-view clustering methods primarily confront two challenges: i) they generally perform feature extraction in the feature domain, which is sensitive to noise and may neglect cluster-specific information that is indistinguishable in the original space; ii) current dynamic fusion methods adopt static strategies to learn weights, lacking capability to adjust strategies adaptively under complex scenarios according to variations in data distribution and view quality. To address these issues, we propose a large language model assisted dynamic agent for multi-view clustering (LLM-DAMVC), a novel framework that recasts multi-view clustering as a dynamic decision-making problem orchestrated by a large language model. Specifically, each view is equipped with complementary agents dedicated to feature extraction. A dual-domain contrastive module is introduced to optimize feature consistency and enhance cluster separability in both the feature domain and frequency domain. Additionally, an LLM-assisted view fusion mechanism provides a flexible fusion weight learning strategy that can be adaptively applied to complex scenarios and significantly different views. Extensive experimental results validate the effectiveness and superiority of the proposed method.

---

## 论文详细总结（自动生成）

# 论文总结：LLM-DAMVC: A Large Language Model Assisted Dynamic Agent for Multi-View Clustering

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：多视图聚类旨在利用不同视图间的一致性与互补性实现无监督数据分组。现有方法面临两大挑战：
  - 特征提取局限于原始特征域，对噪声敏感，且难以捕捉在原始空间中不可区分的聚类特定信息。
  - 动态融合方法多采用静态策略学习权重，缺乏根据数据分布变化和视图质量自适应调整的能力。
- **核心问题**：如何设计一种能动态评估视图质量、自适应分配融合权重的多视图聚类框架，以克服静态策略的局限性。
- **整体含义**：论文将多视图聚类重新定义为由大语言模型（LLM）协调的动态决策问题，首次将LLM作为决策模块引入视图融合过程，提升了聚类在复杂场景下的鲁棒性和适应性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出LLM-DAMVC框架，包含三个关键模块：多智能体协作模块、双域对比学习模块、LLM辅助动态特征聚合模块。通过LLM的语义推理能力实时评估视图质量并生成聚合策略。
- **关键技术细节**：
  - **多智能体协作模块**：
    - 每个视图配备一个**标准智能体**（Standard Agent）和**对抗智能体**（Adversarial Agent）。
    - 标准智能体提取稳定特征与聚类概率分布；对抗智能体通过输入梯度生成对抗样本并训练判别器，增强鲁棒性。
    - 使用残差门控编码器（Residual Gated Encoder）生成潜在表示，并加入对齐损失（alignment loss）确保跨视图一致性。
  - **双域对比学习模块**：
    - 在**特征域**进行标准对比学习（InfoNCE风格）。
    - 在**频域**对嵌入进行FFT变换，提取幅度谱，然后构造对比损失，捕捉特征空间中难以区分的全局结构模式。
    - 总对比损失：L_cont = L_feature + ρ L_frequency。
  - **LLM辅助动态特征聚合模块**：
    - 为每个智能体计算四维质量指标向量（包括平均预测置信度、聚类结构质量、预测确定性、表示质量分数）。
    - 将质量向量构造为结构化提示，输入冻结的LLM（如DeepSeek-8B）。
    - LLM输出高层面路由策略（核心是置信度阈值τc），用于筛选不可靠智能体并计算原始权重，再经softmax归一化获得最终融合权重。
    - 融合所有智能体预测得到最终聚类分配。
- **目标函数**：总损失L_total = L_disc + γ L_align + λ L_cont + β L_clus，包含判别器损失、对齐损失、对比损失和聚类损失。

## 3. 实验设计

- **数据集**：6个基准多视图数据集
  - NUS-WIDE（图像，6251样本，5视图）
  - BDGP（果蝇图像，2500样本，2视图）
  - Handwritten（手写数字，2000样本，6视图）
  - MNIST-USPS（跨域数字，5000样本，2视图）
  - Reuters（多语言新闻，1800样本，5视图）
  - CCV（消费者视频，6773样本，3视图）
- **基准方法**：10种对比方法，涵盖经典基线（K-Means）、深度MVC方法（DCP、EE-IMVC、ASR、MFLVC）、对比学习（CVCL、COMPLETER）、动态融合（ADMC、COPER）、安全聚类（DSIMVC）等。
- **评估指标**：ACC、NMI、PUR。

## 4. 资源与算力

- 论文明确指出：所有实验在PyTorch上实现，运行于**NVIDIA GeForce RTX 4090 GPU**。
- **未说明**：具体训练时长、模型参数量、显存占用等详细资源消耗信息。仅提及使用了不同参数量级的LLM（DeepSeek-1.5B/7B/8B）进行对比。

## 5. 实验数量与充分性

- **实验数量**：较为充分。
  - 主实验结果表1，在6个数据集上对比了10种方法，记录ACC/NMI/PUR。
  - 消融实验表2，逐步添加5个组件（基线→+L_disc→+L_frequency→+L_clus→+L_align），在6个数据集上验证。
  - 超参数敏感性分析（图2），对λ、β、γ进行两两网格搜索。
  - 不同LLM规模对比（图4右），比较DeepSeek-1.5B/7B/8B在6个数据集上的性能。
  - 收敛性分析（图4左），展示ACC/NMI/PUR和Loss随迭代变化。
  - t-SNE可视化（图3），展示BDGP上特征演化过程。
- **充分性与客观性**：实验设计较为全面，消融实验验证了每个组件的贡献，超参数分析说明了模型对部分参数的鲁棒性，不同LLM规模对比验证了框架的可扩展性。但缺少误差棒/统计显著性检验，也未报告重复运行的标准差，可能在公平性上存在轻微不足。

## 6. 主要结论与发现

- **性能优势**：LLM-DAMVC在所有6个数据集上均取得最优或次优结果，尤其在包含噪声和异构视图的数据集（如Reuters、CCV）上提升显著，验证了LLM辅助动态融合的有效性。
- **关键组件贡献**：
  - 频域对比学习（+L_frequency）和聚类损失（+L_clus）带来最大性能提升，说明捕捉全局结构和直接优化聚类目标至关重要。
  - 对齐损失（+L_align）和判别器损失（+L_disc）进一步提供微调。
- **LLM的作用**：以DeepSeek-8B为例，在复杂数据集上表现最好；即使1.5B小模型在结构良好数据集上也能维持可接受性能，表明LLM决策模块的鲁棒性。
- **收敛性**：模型在约60次迭代后稳定收敛，loss单调下降。
- **超参数敏感性**：γ和β需精细调节，λ则较鲁棒。

## 7. 优点

- **创新性**：首次将LLM作为动态决策模块引入多视图聚类，而非仅用于特征提取，赋予模型根据数据质量和分布自适应调整融合策略的能力。
- **方法完整性**：双域对比学习（特征域+频域）有效弥补了传统单一域特征提取的缺陷；多智能体协作（标准+对抗）增强了鲁棒性。
- **实验充分性**：在多个数据集上对比了多种先进方法，并进行了消融、超参数、LLM规模、收敛性、可视化等多维度分析，验证了各组件的必要性。
- **框架可扩展性**：支持不同规模的LLM，可根据计算资源灵活选择。

## 8. 不足与局限

- **实验局限**：
  - 未报告误差棒或统计显著性检验，无法评估结果的波动性。
  - 未提供详细计算资源消耗（如训练时间、内存占用），对于实际部署的参考价值有限。
- **方法局限**：
  - LLM是冻结使用的，未针对视图融合任务进行微调，作者指出未来可通过对LLM微调进一步提升性能。
  - 依赖于LLM的推理能力，可能引入额外延迟和计算开销，对于大规模实时应用可能不敏感。
  - 仅验证了图像、文本、视频等模态，未涉及更复杂或缺失视图的场景（论文本身注明了未来方向）。
- **应用限制**：需要预训练LLM可用，且LLM的决策依赖于人工设计的质量指标向量（prompt），若指标不完善可能影响效果。

（完）
