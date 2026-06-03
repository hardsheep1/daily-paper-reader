---
title: "LLM-DAMVC: A Large Language Model Assisted Dynamic Agent for Multi-View Clustering"
title_zh: LLM-DAMVC：大语言模型辅助的多视图聚类动态代理
authors: "HaiMing Xu, Qianqian Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=xgiMK8FtSI"
tags: ["query:ac"]
score: 8.0
evidence: 大语言模型协助的多视图聚类动态代理
tldr: 现有多视图聚类方法在特征域提取特征易受噪声影响，且动态融合策略缺乏自适应性。为此，本文提出LLM-DAMVC框架，利用大语言模型作为动态代理，根据数据分布和视图质量自适应调整融合策略，并提取簇特定信息。在多个基准数据集上的实验表明，该方法显著提升了聚类性能，为多视图聚类提供了新的LLM辅助范式。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1429, \"height\": 780}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1415, \"height\": 472}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1413, \"height\": 500}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xgimk8ftsi/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1448, \"height\": 299}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2025-xgimk8ftsi/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1447, \"height\": 342}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-xgimk8ftsi/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1451, \"height\": 209}]"
motivation: 针对多视图聚类中特征域提取对噪声敏感且静态融合策略缺乏适应性的问题。
method: 提出LLM-DAMVC框架，利用大语言模型作为动态代理，自适应学习视图权重并提取簇特定信息。
result: 实验表明所提方法在多个多视图数据集上优于现有方法，验证了LLM辅助聚类的有效性。
conclusion: LLM可以作为一种动态代理有效提升多视图聚类性能，为结合大模型与聚类提供了新思路。
---

## Abstract
Multi-view clustering integrates the consistency and complementarity of different views to achieve unsupervised data grouping. Existing multi-view clustering methods primarily confront two challenges: i) they generally perform feature extraction in the feature domain, which is sensitive to noise and may neglect cluster-specific information that is indistinguishable in the original space; ii) current dynamic fusion methods adopt static strategies to learn weights, lacking capability to adjust strategies adaptively under complex scenarios according to variations in data distribution and view quality. To address these issues, we propose a large language model assisted dynamic agent for multi-view clustering (LLM-DAMVC), a novel framework that recasts multi-view clustering as a dynamic decision-making problem orchestrated by a large language model. Specifically, each view is equipped with complementary agents dedicated to feature extraction. A dual-domain contrastive module is introduced to optimize feature consistency and enhance cluster separability in both the feature domain and frequency domain. Additionally, an LLM-assisted view fusion mechanism provides a flexible fusion weight learning strategy that can be adaptively applied to complex scenarios and significantly different views. Extensive experimental results validate the effectiveness and superiority of the proposed method.

---

## 论文详细总结（自动生成）

# LLM-DAMVC：大语言模型辅助的多视图聚类动态代理（详细总结）

## 1. 核心问题与整体含义（研究动机和背景）

多视图聚类（Multi-View Clustering, MVC）旨在整合不同视图的一致性与互补性，实现无监督数据分组。然而，现有方法面临两大挑战：

- **特征域噪声与信息丢失**：大多数方法仅在原始特征域进行特征提取，对噪声敏感，且无法捕捉在原始空间中难以区分的簇特定信息（这些信息可能在频域中易于分离）。
- **融合策略静态化**：现有的动态融合方法采用静态规则学习视图权重，无法根据数据分布和视图质量的变化自适应调整策略，尤其是在复杂场景或样本级别视图质量剧烈变化时缺乏灵活性。

为此，本文提出 **LLM-DAMVC** 框架，将多视图聚类重塑为由大语言模型（LLM）协调的动态决策问题，利用LLM的语义推理和决策能力实现自适应视图融合。

## 2. 方法论

### 2.1 核心思想

LLM-DAMVC 包含三个关键模块：**多智能体协作模块**、**双域对比学习模块**、**LLM辅助动态特征聚合模块**。整体框架将每个视图配备一个标准智能体和一个对抗智能体，分别进行语义特征提取和对抗学习；通过双域对比学习优化特征一致性与簇可分离性；最后利用LLM根据多面质量指标动态生成融合权重。

### 2.2 关键技术细节

#### 多智能体协作模块
- **残差门控编码器**：将输入 \(X_v\) 通过 \(L-1\) 层（线性+BN+ReLU）和最后一层自门控+残差连接，得到潜在表示 \(H_v\)。
- **标准智能体**：从 \(H_v\) 学习聚类概率分布 \(P_v^{std}\) 和标准嵌入 \(Z_v^{std}\)。
- **对抗智能体**：从 \(H_v\) 学习对抗分布 \(P_v^{adv}\) 和对抗嵌入 \(Z_v^{adv}\)；通过梯度生成对抗样本 \(\tilde{X}_v\) 并训练鉴别器 \(\psi\)，形成最小-最大博弈，增强鲁棒性。同时使用对齐损失 \(L_{align}\) 促进跨视图一致性和KL散度匹配。

#### 双域对比学习模块
- **特征域对比学习**：对同一样本不同视图的嵌入计算余弦相似度，采用InfoNCE损失 \(L_{feature}\)。
- **频域对比学习**：对嵌入应用FFT得到幅度谱，构造对比三元组，使用L1距离定义损失 \(L_{frequency}\)，捕捉全局结构模式。
- 总对比损失：\(L_{cont} = L_{feature} + \rho L_{frequency}\)。

#### LLM辅助动态特征聚合模块
- 对每个智能体计算四维质量指标向量 \(q_{va}\)：平均预测置信度、簇结构质量（紧凑度）、预测确定性（1-熵）、平均表示质量分数。
- 将质量向量格式化为结构化提示，输入冻结的LLM（如DeepSeek系列），LLM输出置信度阈值 \(\tau_c\) 作为路由策略。
- 根据阈值计算原始权重 \(r_{va}\)，若平均置信度≥\(\tau_c\) 则保留，否则置零；再通过softmax归一化得到最终权重 \(w_{va}\)。
- 加权融合所有智能体的预测得到最终聚合分布 \(P\)，并定义聚类损失 \(L_{clus}\)（包含高置信度样本交叉熵、平衡项、清晰度项）。
- 总损失：\(L_{total} = L_{disc} + \gamma L_{align} + \lambda L_{cont} + \beta L_{clus}\)。

## 3. 实验设计

### 3.1 数据集与指标
- **6个基准数据集**：NUS-WIDE (NUS)、BDGP、Handwritten、MNIST-USPS、Reuters、CCV。覆盖图像、文本、视频等多种模态。
- **评价指标**：准确率 (ACC)、归一化互信息 (NMI)、纯度 (PUR)。

### 3.2 对比方法
共对比10种方法，包括：
- 经典基线：K-Means
- 深度MVC方法：DCP、EE-IMVC、ASR、MFLVC、DSIMVC
- 对比学习方法：CVCL、COMPLETER
- 先进动态融合方法：ADMC、COPER

### 3.3 实验设置
- 实现框架：PyTorch
- 硬件：NVIDIA GeForce RTX 4090 GPU（未说明数量）
- 超参数：进行了λ、β、γ的敏感性分析（固定γ=1.0变化λ和β等）

## 4. 资源与算力

文中明确说明实验在一张 **NVIDIA GeForce RTX 4090 GPU** 上完成，未提及多卡或训练时长。代码及更详细的环境要求将在论文发表后公开。

## 5. 实验数量与充分性

- **主实验**：在6个数据集上全面对比10种方法，报告3个指标，共18个结果（方法×数据集），LLM-DAMVC全部取得第一或第二。
- **超参数敏感性分析**：三组二维参数扫描（λ-β，γ-β，λ-γ），验证各损失的必要性。
- **消融实验**：逐步添加组件（基线→+\(L_{disc}\)→+\(L_{frequency}\)→+\(L_{clus}\)→+\(L_{align}\)），在6个数据集上展示每一步提升。
- **多维度分析**：t-SNE可视化特征演化（BDGP数据集125个epoch）、收敛曲线（迭代至60次后稳定）、不同规模LLM对比（DeepSeek-1.5B/7B/8B），验证LLM规模影响。
- **局限性讨论**：明确指出LLM未针对视图融合微调，未来方向包括微调与蒸馏。

实验覆盖全面，消融和参数分析充分，对比方法涵盖主流类别，结果客观且具有说服力。唯一不足是未报告误差棒或统计显著性。

## 6. 主要结论与发现

- **LLM-DAMVC在全部6个数据集上的ACC、NMI、PUR均显著优于所有基线方法**，特别是在复杂数据集（如Reuters、CCV）上保持领先。
- **双域对比学习是关键**：频域对比的加入（\(L_{frequency}\)）显著提升性能，验证了频域信息对特征分离的重要性。
- **动态融合机制有效**：LLM辅助的权重分配比静态规则更适应视图质量变化，且不同规模LLM均带来提升，其中8B模型最佳。
- **所有损失组件缺一不可**：去掉任何一个都会导致性能下降，证明各组件协同作用。
- **模型收敛稳定**：约60个迭代后性能饱和，损失单调下降。

## 7. 优点

- **创新性**：首次将LLM作为决策模块而非特征提取器，用于视图融合的实时评估与自适应权重生成。
- **技术融合**：结合残差门控编码器、双域对比学习（特征域+频域）、对抗训练，形成鲁棒且可区分的表示。
- **可解释性**：质量指标（置信度、紧凑度、确定性、判别分数）为LLM提供结构化输入，策略输出明确（阈值）。
- **鲁棒性**：在噪声较大或视图差异明显的场景（如CCV）仍表现优异。
- **实验充分**：多数据集、多指标、多消融、多维度分析，验证全面。

## 8. 不足与局限

- **LLM未微调**：当前采用冻结的通用LLM（DeepSeek），性能有进一步提升空间。未来可针对视图融合任务微调LLM。
- **效率问题**：引入LLM可能增加推理开销，文中未报告具体时间或复杂度对比。作者提及未来可通过蒸馏提升效率。
- **统计显著性缺失**：未提供多次运行的误差棒或置信区间，结果稳定性缺乏量化证明。
- **数据集规模**：实验数据集样本量在1,800~6,773之间，未在大规模（如超过10万样本）数据上验证，扩展性待考。
- **超参数敏感性**：γ和β存在性能峰值，需仔细调节，而λ（对比损失权重）鲁棒性较好，说明超参数选择仍有一定依赖。

（完）
