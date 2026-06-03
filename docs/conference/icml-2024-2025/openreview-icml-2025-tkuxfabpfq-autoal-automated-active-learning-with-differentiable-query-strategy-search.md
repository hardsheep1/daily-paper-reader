---
title: "AutoAL: Automated Active Learning with Differentiable Query Strategy Search"
title_zh: AutoAL：基于可微查询策略搜索的自动化主动学习
authors: "Yifeng Wang, Xueying Zhan, Siyu Huang"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=TkUxFAbpfQ"
tags: ["query:active-clust"]
score: 7.0
evidence: 可微查询策略搜索的主动学习方法
tldr: 为了解决不同数据场景下主动学习算法效果差异大以及手动选择策略困难的问题，本文提出了AutoAL，一种可微分的主动学习策略搜索方法。该方法通过两个神经网络搜索最优的采样策略，从而自动适配给定任务。实验表明，AutoAL在多个数据集上显著优于固定策略和传统搜索方法，为主动学习领域提供了一种自动化的解决方案。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-tkuxfabpfq/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1773, \"height\": 774, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-tkuxfabpfq/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1789, \"height\": 1176, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-tkuxfabpfq/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1785, \"height\": 563, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-tkuxfabpfq/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1786, \"height\": 555, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-tkuxfabpfq/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1624, \"height\": 642, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2025-tkuxfabpfq/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 499, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-tkuxfabpfq/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1210, \"height\": 405, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-tkuxfabpfq/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1778, \"height\": 810, \"label\": \"Table\"}]"
motivation: 手动选择主动学习策略耗时且效果不稳定，需要一种自动化方法来搜索最优策略。
method: 提出AutoAL，通过两个神经网络实现可微的查询策略搜索。
result: 在多个深度学习数据集上，AutoAL自动搜索的策略优于人工选择的固定策略。
conclusion: AutoAL为主动学习提供了自动化策略搜索技术，具有广泛的适用性。
---

## Abstract
As deep learning continues to evolve, the need for data efficiency becomes increasingly important. Considering labeling large datasets is both time-consuming and expensive, active learning (AL) provides a promising solution to this challenge by iteratively selecting the most informative subsets of examples to train deep neural networks, thereby reducing the labeling cost. However, the effectiveness of different AL algorithms can vary significantly across data scenarios, and determining which AL algorithm best fits a given task remains a challenging problem. This work presents the first differentiable AL strategy search method, named AutoAL, which is designed on top of existing AL sampling strategies. AutoAL consists of two neural nets, named SearchNet and FitNet, which are optimized concurrently under a differentiable bi-level optimization framework. For any given task, SearchNet and FitNet are iteratively co-optimized using the labeled data, learning how well a set of candidate AL algorithms perform on that task. With the optimal AL strategies identified, SearchNet selects a small subset from the unlabeled pool for querying their annotations, enabling efficient training of the task model. Experimental results demonstrate that AutoAL consistently achieves superior accuracy compared to all candidate AL algorithms and other selective AL approaches, showcasing its potential for adapting and integrating multiple existing AL methods across diverse tasks and domains.

---

## 论文详细总结（自动生成）

## 论文中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在深度学习中，标注大量数据成本高昂，主动学习（Active Learning, AL）通过迭代选择最具信息量的样本进行标注来降低标注成本。然而，不同AL策略（如不确定性采样、多样性采样）在不同数据场景下的效果差异很大，手动选择最优策略既耗时又不稳定。现有自适应选择方法（如SelectAL、ALBL）依赖黑箱搜索或近似估计，计算效率低且不可微，难以高效优化。
- **整体含义**：本文旨在提出一种自动化的、可微分的AL策略搜索方法，使得框架能够根据给定任务和数据分布，自动从一组候选策略中选出最优的组合，从而提升AL的性能和泛化能力。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：设计两个神经网络——**SearchNet**（搜索网络）和**FitNet**（拟合网络），在可微的双层优化框架下联合优化。FitNet利用已有标注数据学习数据分布，SearchNet基于FitNet的任务损失动态调整各候选AL策略的权重，从而实现对最优策略的自动搜索。
- **关键技术细节**：
  - **问题设置**：基于池的AL，初始有标注集L和未标注集U。每轮从U中选择b个最信息量的样本进行标注。
  - **双层优化**：内层优化FitNet（最小化交叉熵损失），外层优化SearchNet（最大化所选样本的信息量损失）。优化目标为：
    \[
    \Omega_S^* = \arg\max_{\Omega_S} \sum_{j=1}^{M/2} L_S((x_j,y_j),\Omega_S,\Omega_F^*), \quad \text{s.t. } \Omega_F^* = \arg\min_{\Omega_F} \sum_{j=1}^{M/2} L_F((x_j,y_j),\Omega_F).
    \]
  - **概率查询策略**：对每个候选策略Aκ，计算得分Sκ(xj) ∈ {0,1}。采用高斯混合模型建模总体得分分布，并生成样本，得到每个样本的加权得分Ŝ。
  - **可微松弛**：将离散的策略选择松弛为连续空间，使用Sigmoid函数混合各策略得分：
    \[
    \bar{S}(x_j) = \sum_{\kappa \in K} \frac{\lambda}{1 + \exp(-\Theta^{(j)}_{S'_\kappa})} \hat{S}_\kappa(x_j, \Omega_S),
    \]
    其中Θ是可学习的策略混合权重向量，使得整个优化可通过梯度反向传播。
  - **损失函数**：
    - FitNet损失：\( L_F = \frac{1}{B} \sum_{j'} \bar{S}_{\text{detach}}(x'_{j'}) \cdot \mathcal{L}(x'_{j'}, y'_{j'}) + \bar{\lambda} L_{\text{re}}(t,B) \)
    - SearchNet损失：\( L_S = -\frac{1}{B} \sum_j \bar{S}(x_j) \cdot \mathcal{L}_{\text{detach}}(x_j, y_j) - \bar{\lambda} L_{\text{re}}(t,B) \)
    其中\(L_{\text{re}}\)是正则化损失，控制选择样本的数量。
  - **算法流程**：每轮AL中，先交替优化FitNet和SearchNet约400个epoch（FitNet先更新200 epoch），然后对未标注池中的每个样本计算最终得分\(\bar{S}(x_i)\)，选择得分最高的b个样本进行标注，更新L和U，最后用更新后的L训练任务模型。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集**：共7个数据集，涵盖自然图像和医学图像。
  - 自然图像：CIFAR-10、CIFAR-100、SVHN、TinyImageNet。
  - 医学图像：OrganCMNIST、PathMNIST、TissueMNIST（来自MedMNIST数据库）。
- **Benchmark**：以不同标注比例（如2%～44%）下的分类准确率为评估指标，重复3次取均值和标准差。
- **对比方法**：
  - 7种候选AL策略：最大熵、边界采样、最小置信度、KMeans、BALD、VarRatio、MeanSTD。
  - 其他SOTA方法：BADGE、LPL、VAAL、Coreset、ENSvarR、DDU、ALBL（自适应选择方法）。

### 4. 资源与算力

- 论文附录A.1中提供了相对运行时间分析（以EntropySampling为基准），但**未明确说明使用的GPU型号、数量或绝对训练时长**。所有实验在单张Nvidia A100 GPU上完成。AutoAL的总相对时间约为EntropySampling的2.95～6.54倍，其中大部分时间用于计算候选AL得分，SearchNet和FitNet的更新耗时很少（约0.09～0.20倍基准时间）。

### 5. 实验数量与充分性：大概做了多少组实验，是否充分、客观、公平

- **实验数量**：
  - 主实验：在7个数据集上对比15种方法，绘制了7张性能曲线图（图2）。
  - 消融实验（图3、图4）：在CIFAR-100、SVHN、OrganCMNIST上分别做SearchNet组件消融（ResNet backbone vs. Loss Prediction module vs. 完整）和候选池大小消融（1,3,5,7个候选）。
  - 策略权重可视化（图5）：在CIFAR-100和OrganCMNIST上展示各AL策略得分随轮次的变化。
  - 附录中补充了复杂度分析（表2）和数据集统计（表1）。
- **充分性**：实验覆盖多种数据规模和难度（平衡/不平衡、类别数不同、自然/医学），对比方法全面，重复3次报告标准差，结论稳健。消融实验验证了各组件的必要性，策略可视化揭示了AutoAL的行为。
- **客观性/公平性**：所有方法使用相同的ResNet-18骨干网络，训练设置一致。但未对超参数进行广泛调优（如学习率固定为0.005），可能存在对某些基线方法不公平的风险。

### 6. 论文的主要结论与发现

- AutoAL在7个数据集上**一致超越**所有候选策略和其他自适应方法，平均准确率最高。
- AutoAL的曲线更平滑，标准偏差更小，说明其鲁棒性更好，能避免有害样本选择导致的性能骤降。
- 消融实验表明：SearchNet的ResNet backbone和Loss Prediction模块均不可或缺；候选池越大性能越好，但3个候选即可取得显著提升。
- 策略权重可视化显示：**初期以多样性策略（KMeans）为主，后期逐步切换为不确定性策略（最小置信度、MeanSTD）**，符合模型学习阶段的需求。

### 7. 优点：方法或实验设计上的亮点

- **创新性**：首次将AL策略搜索转化为可微分的双层优化问题，实现端到端梯度优化，相比传统黑箱搜索更高效。
- **灵活性**：候选池可轻松包含大多数现有AL策略，易于扩展。
- **数据驱动**：完全基于已有标注数据训练SearchNet和FitNet，无需访问未标注数据的真实标签。
- **实验全面**：涵盖自然和医学图像、平衡与不平衡数据集，对比方法众多，消融实验设计合理。
- **实用性强**：自动适应不同任务，减少人工策略选择负担，在实际部署中潜力大。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制

- **计算成本**：尽管搜索网络更新快，但候选策略的数量直接影响运行时间（AutoAL总时间约为EntropySampling的3–6.5倍），对于超大规模数据集或实时性要求高的场景可能不够高效。
- **候选策略依赖**：性能上限受限于候选池；若所有候选策略在特定任务上都很差，AutoAL也无法超越它们。
- **实验偏差**：超参数（如学习率、优化器选择、训练epoch数）未进行系统搜索，可能偏向AutoAL。
- **应用限制**：仅验证了图像分类任务，未在NLP、语音或结构化预测等任务上测试，通用性有待进一步验证。
- **可解释性**：虽然可视化了策略权重，但SearchNet和FitNet的具体决策过程仍不够透明，可能存在偏见风险（如对某些类别的样本过度选择）。

（完）
