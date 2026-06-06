---
title: Interactive Deep Clustering via Value Mining
title_zh: 基于价值挖掘的交互式深度聚类
authors: "Honglin Liu, Peng Hu, Changqing Zhang, Yunfan Li, Xi Peng"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=Y7HPB7pL1f"
tags: ["query:active-clust"]
score: 8.0
evidence: 交互式深度聚类，通过价值挖掘利用用户查询改进聚类
tldr: 针对现有深度聚类方法难以处理边界硬样本的问题，提出交互式深度聚类（IDC）方法，通过定量评估样本价值并引入用户交互来查询最有价值的样本，以最小交互开销提升预训练聚类模型的性能。实验表明该方法能有效突破聚类性能瓶颈。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-y7hpb7pl1f/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1456, \"height\": 549, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-y7hpb7pl1f/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1436, \"height\": 749, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-y7hpb7pl1f/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1439, \"height\": 538, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-y7hpb7pl1f/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1455, \"height\": 424, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2024-y7hpb7pl1f/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 815, \"height\": 300, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-y7hpb7pl1f/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1443, \"height\": 894, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-y7hpb7pl1f/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1166, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-y7hpb7pl1f/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1159, \"height\": 440, \"label\": \"Table\"}]"
motivation: 现有深度聚类方法难以区分聚类边界上的硬样本，需要引入用户交互来突破性能瓶颈。
method: 提出交互式深度聚类（IDC）方法，定量评估样本价值，优先查询最有价值的样本。
result: 该方法能以最小交互开销显著提升预训练聚类模型的性能。
conclusion: 交互式查询是提升聚类模型性能的有效手段，IDC是即插即用的通用方法。
---

## Abstract
In the absence of class priors, recent deep clustering methods resort to data augmentation and pseudo-labeling strategies to generate supervision signals. Though achieved remarkable success, existing works struggle to discriminate hard samples at cluster boundaries, mining which is particularly challenging due to their unreliable cluster assignments. To break such a performance bottleneck, we propose incorporating user interaction to facilitate clustering instead of exhaustively mining semantics from the data itself. To be exact, we present Interactive Deep Clustering (IDC), a plug-and-play method designed to boost the performance of pre-trained clustering models with minimal interaction overhead. More specifically, IDC first quantitatively evaluates sample values based on hardness, representativeness, and diversity, where the representativeness avoids selecting outliers and the diversity prevents the selected samples from collapsing into a small number of clusters. IDC then queries the cluster affiliations of high-value samples in a user-friendly manner. Finally, it utilizes the user feedback to finetune the pre-trained clustering model. Extensive experiments demonstrate that IDC could remarkably improve the performance of various pre-trained clustering models, at the expense of low user interaction costs. The code could be accessed at pengxi.me.

---

## 论文详细总结（自动生成）

# 中文总结：Interactive Deep Clustering via Value Mining

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有深度聚类方法在缺乏类先验的情况下，依赖数据增强和伪标签生成监督信号，但对聚类边界上的“硬样本”（hard samples）区分能力差，因为这些样本的聚类分配不可靠，导致性能瓶颈。
- **研究动机**：单纯从数据内部挖掘语义无法突破性能上限，尤其是硬样本难以通过内部伪标签策略纠正。作者提出引入外部用户交互，以低代价修正硬样本的聚类归属，从而显著提升预训练聚类模型的性能。
- **整体含义**：本文提出了一种即插即用的交互式深度聚类方法 IDC，通过一种价值挖掘策略（考虑硬度、代表性和多样性）选择最有价值的样本进行用户询问，并利用用户反馈微调模型，以极低的交互成本实现显著的聚类性能提升。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

### 2.1 核心思想
- 对于任意一个预训练的深度聚类模型，IDC 首先评估每个样本的价值（由硬度、代表性、多样性三个指标构成），然后筛选出价值最高的 M 个样本进行用户交互。
- 交互中，用户只需从该样本最近的 T 个聚类中心候选者中辨认其所属类别，或拒绝所有候选（负反馈）。
- 根据正/负反馈，设计正损失、负损失和正则化损失来微调模型，同时保留原有决策边界，防止过拟合于少量询问样本。

### 2.2 关键技术细节
- **价值评估公式**：  
  \( v_i = h_i + r_i + d_i \)  
  - **硬度** \( h_i = \log(1 - z_i \cdot c_{g1} + z_i \cdot c_{g2}) \)：基于样本与最近和第二近聚类中心的距离度量不确定性。
  - **代表性** \( r_i = -\log \sum_{j=1}^K \|z_i - z_{i(j)}\|_2^2 \)：基于 K 近邻的局部密度，避免选择离群点。
  - **多样性** \( d_i = \min_{j \in S} \log(1 - z_i \cdot z_j) \)：与已选样本的相似度最小值，防止选中样本集中在少数类别。
- **选择算法**：迭代选择当前价值最高的样本，每选一个后更新剩余样本的多样性分值，直至选出 M 个样本。
- **用户交互**：对每个选中样本，提供其最近的 T=5 个聚类中心候选者（用最近邻样本表示），用户选择匹配的类别或全部拒绝。
- **模型优化损失**：
  - **正损失** \( \mathcal{L}_{\text{pos}} \)：对正反馈样本，使用交叉熵拉近样本与选定聚类中心。
  - **负损失** \( \mathcal{L}_{\text{neg}} \)：对负反馈样本，使用交叉熵（1-p）推远样本与候选中心。
  - **正则化损失** \( \mathcal{L}_{\text{reg}} \)：对高置信度预测（置信度 > τ=0.99）的样本保持原有预测，避免过拟合查询样本。
  - 总损失：\( \mathcal{L} = \mathcal{L}_{\text{pos}} + \mathcal{L}_{\text{neg}} + \mathcal{L}_{\text{reg}} \)。

### 2.3 算法流程（文字说明）
1. 输入预训练聚类模型的特征，计算所有样本的硬度和代表性分值，初始化多样性分值为 0。
2. 迭代 M 次：选择当前剩余样本中价值最高的样本加入选中集，更新剩余样本的多样性分值。
3. 对选中的 M 个样本（实验中 M=500），向用户询问其与最近 5 个聚类中心候选者的关系，收集正/负反馈。
4. 使用正损失、负损失和正则化损失微调预训练模型共 100 个 epoch（对无聚类头的模型先预热 50 个 epoch）。
5. 返回微调后的模型聚类结果。

## 3. 实验设计

### 3.1 数据集
- CIFAR-10（60,000 张，10 类）、CIFAR-20（60,000 张，20 类）、STL-10（13,000 张，10 类）、ImageNet-10（13,000 张，10 类）、ImageNet-Dogs（19,500 张，15 类）。

### 3.2 Benchmark 及对比方法
- **对比方法**：14 种深度聚类方法（CC、SCAN、NMM、MiCE、BYOL、GCC、SPICE、IDFD、TCC、DivClust、SeCu、CoNR、TCL、ProPos）以及两个半监督基线（FixMatch、Cop-Kmeans）。
- **评价指标**：NMI、ACC、ARI（越高越好）。
- **附加基线**：手动修正 500 个查询样本的聚类分配（记为 TCL†、ProPos†）。

### 3.3 实验场景
- 将 IDC 应用到两个代表性模型 TCL（有聚类头）和 ProPos（无聚类头），在五个数据集上评估。
- 消融实验：样本选择策略（Hard vs Hard+Rep vs Hard+Rep+Div）；选择样本数 M 的影响；候选数 T 的影响；损失函数组合的贡献。

## 4. 资源与算力
- **文中说明**：所有实验在单个 Nvidia RTX 3090 GPU 上，Ubuntu 20.04 平台，CUDA 12.0。
- **训练时长**：微调阶段为 100 个 epoch，但未给出具体时间（用户交互时间不可控）。未明确说明总 GPU 时长或能耗。

## 5. 实验数量与充分性

### 5.1 实验数量
- 主表（Table 2）包含 5 个数据集、16 种对比方法，IDC 分别基于 TCL 和 ProPos 进行测试。
- 消融实验（Table 3、Table 4、Figure 3、Figure 4）包括：
  - 样本选择策略的三种组合对比。
  - 不同 M（0～700）和不同 T（1～7）的影响。
  - 损失函数 7 种组合的对比。
- 可视化分析：T-SNE 展示选择样本分布。

### 5.2 充分性
- 实验覆盖了多个规模和难度不同的图像聚类基准，对比方法全面，消融实验系统。
- 但未报告误差棒或统计显著性（作者解释因计算资源限制且前人工作也未报告）。
- 总体客观公平，方法即插即用，基线选择合理，实验设计较为充分。

## 6. 论文的主要结论与发现

- IDC 能显著提升预训练聚类模型的性能，尤其在难数据集上（如 CIFAR-20 上 TCL 的 ACC 从 53.1% 提升至 69.4%，ProPos 从 59.1% 提升至 78.3%）。
- 与传统半监督方法（FixMatch）和约束聚类（Cop-Kmeans）相比，IDC 表现更优，得益于定制化的价值样本选择策略。
- 价值选择中的三个指标缺一不可：仅考虑硬度会选中边缘样本且不具代表性；加入代表性可能导致样本坍缩到少数类；多样性保证样本分散。
- 损失函数中正损失贡献最大，负损失和正则化损失均不可或缺。
- 随着选择样本数增加，性能先快速上升后趋于饱和，验证了样本价值递减（Theorem 1）。
- 候选数 T=5 时效果最佳。

## 7. 优点

- **即插即用**：IDC 可轻松集成到任意预训练深度聚类模型，无需修改原有架构。
- **交互友好**：询问样本与最近聚类中心的隶属关系，而非直接要求类别标签，降低用户认知负担。
- **价值挖掘策略新颖**：综合硬度、代表性和多样性三个维度，理论保证所选样本价值随选择过程单调递减，有效控制交互成本。
- **利用正负反馈**：设计了对应的正损失和负损失，并加入正则化损失防止过拟合，优化目标合理。
- **实验扎实**：在多个数据集和多种方法上验证，消融实验完整，证明各组件贡献。

## 8. 不足与局限

- **用户反馈错误**：未考虑用户可能给出错误反馈，缺乏鲁棒性机制。
- **交互界面仍有限**：仅询问最近聚类中心候选，可能无法覆盖更细粒度的划分需求，未来可设计更高级的交互流程对齐用户个性化标准。
- **实验标准差缺失**：未报告多次重复实验的误差棒，难以判断结果统计显著性。
- **计算资源描述不完整**：仅说明单 GPU 和 100 epoch，未给出微调的实际时间或总计算量。
- **应用限制**：方法依赖预训练模型的特征质量；若预训练模型本身较差，价值评估可能不准确。
- **未讨论可扩展性**：对于更大规模数据集（如 ImageNet 完整版），500 个样本的交互是否足够以及计算开销需进一步验证。

（完）
