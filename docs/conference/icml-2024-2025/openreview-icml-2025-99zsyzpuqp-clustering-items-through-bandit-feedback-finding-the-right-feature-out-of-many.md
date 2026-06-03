---
title: "Clustering Items through Bandit Feedback: Finding the Right Feature out of Many"
title_zh: 通过赌博机反馈进行聚类：从众多特征中找到正确特征
authors: "Maximilian Graf, Thuot Victor, Nicolas Verzelen"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=99zsyZpUqp"
tags: ["query:active-clust"]
score: 7.0
evidence: 通过赌博机反馈进行聚类中的主动特征选择
tldr: 该论文研究基于赌博机反馈的聚类问题，在每个回合中学习器主动选择一个样本和一个特征来获取带噪声的评估，目标是恢复样本的正确划分。提出一种利用Sequential Halving算法找出相关特征的方法，在理论上保证以高概率正确聚类的同时最小化观察次数。实验证实了该算法在多种设置下的有效性，为主动聚类提供了新的查询策略。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 697, \"height\": 518}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 675, \"height\": 530}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 839, \"height\": 512}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 778, \"height\": 594}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2025-99zsyzpuqp/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 869, \"height\": 295}]"
motivation: 传统聚类需要大量特征观察，本文旨在通过主动选择特征和样本来高效聚类。
method: 利用Sequential Halving算法自适应地选择最有信息量的特征，结合赌博机反馈逐步恢复聚类结构。
result: 理论分析表明算法能以高概率正确聚类，且观察次数接近最优。
conclusion: 提出了一种新颖的主动聚类查询策略，显著降低了聚类所需的特征观测成本。
---

## Abstract
We study the problem of clustering a set of items based on bandit feedback. Each of the $n$ items is characterized by a feature vector, with a possibly large dimension $d$. The items are  partitioned into two unknown groups, such that items within the same group share the same feature vector. We consider a sequential and adaptive setting in which, at each round, the learner selects one item and one feature, then observes a noisy evaluation of the item's feature. The learner's objective is to recover the correct partition of the items, while keeping the number of observations as small as possible. 
We provide an algorithm which relies on finding a relevant feature for the clustering task, leveraging the Sequential Halving algorithm. With probability at least $1-\delta$, we obtain an accurate recovery of the partition and derive an upper bound on the required budget . Furthermore, we derive an instance-dependent lower bound, which is tight in some relevant cases.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：在众包标注、图像分类等场景中，每个物品（item）由高维特征向量描述，但特征中只有少数对区分不同聚类有用。传统聚类需要观察全部特征，成本高昂。本文研究如何通过自适应地选择性查询（bandit feedback）来高效恢复物品的二分聚类。
- **核心问题**：给定 $n$ 个物品，每个物品有 $d$ 维特征向量，物品被未知地分为两个组（组内特征向量相同）。学习器可在每一轮选择 **一个物品** 和 **一个特征**，获得该特征上的带噪声观测值。目标是以至少 $1-\delta$ 的概率正确恢复分组，同时最小化总观测次数（budget）。
- **整体含义**：本文首次在理论上刻画了该主动聚类问题的样本复杂度，提出了一个能自动适应特征稀疏性和组平衡性的算法，并给出了匹配的下界，证明了算法在稀疏场景下的最优性。

#### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将聚类分解为三个步骤：① 找出两个分别来自不同组的代表物品；② 找出最能区分这两个组的特征（即差距最大的特征）；③ 利用该特征一次性地对所有物品进行分类。核心在于平衡“探索”（寻找好的特征）和“利用”（使用该特征聚类）之间的预算。
- **关键技术细节**：
  - **子算法1：CompareSequentialHalving (CSH)** — 改编自Sequential Halving。给定一个参考物品 $r_0$ 和候选物品集 $I$，CSH在固定预算下输出一个物品-特征对 $(i,j)$，使得 $|M_{i,j}-M_{r_0,j}|$ 尽可能大。它通过子采样（从 $I\times[d]$ 中均匀采样 $2^L$ 对）并逐轮淘汰表现最差的一半来高效搜索。
  - **子算法2：CandidateRow (CR)** — 通过不断调用CSH并递增预算（指数增长），检测第一个与 $r_0$ 显著不同的物品作为另一个组的代表 $r_1$。停止条件基于Hoeffding不等式。
  - **子算法3：ClusterByCandidates (CBC)** — 使用CSH在已知两个代表物品 $r_0,r_1$ 的基础上寻找差距最大的特征 $j$，然后用该特征对所有物品进行Neyman-Pearson式检验（比较与 $r_0$ 的差异是否超过阈值），从而划分所有物品。
  - **主算法：BanditClustering** — 顺序调用CR（$\delta/2$）和CBC（$\delta/2$），以总置信度 $1-\delta$ 输出聚类。
- **理论保证**：
  - 上界（Theorem 3.1）：以至少 $1-\delta$ 概率正确聚类，预算不超过 $\tilde{C}\log(1/\delta)\cdot H$，其中 $H$ 包含两项：$d\theta/\|\Delta\|_2^2$（检测代表）和 $\min_{s} (d/s + n)/\Delta_{(s)}^2$（特征探测与分类的权衡）。
  - 下界（Theorem 4.1）：对于任意 $\delta$-PAC算法，存在一个排列后的环境使得预算至少为 $\Omega\big( d\theta/\|\Delta\|_2^2 \log(1/\delta) \vee (n-2)/\Delta_{(1)}^2 \log(1/\delta) \big)$，匹配稀疏常数间隙场景的上界。

#### 3. 实验设计：使用了哪些数据集 / 场景，benchmark是什么，对比了哪些方法
- **数据集/场景**：全部使用合成数据，模拟高斯噪声环境。
  - **实验1**：$n=20, d=1000$，固定 $\|\Delta\|_2=15$，改变稀疏度 $s$（从1到400），间距 $h_s=15/\sqrt{s}$。一半物品为0，一半为 $\Delta_s$。
  - **实验2**：$n$ 从100到5000，$d=10n$，固定 $\Delta$ 前10维为5，其余为0，平衡分组。改变置信度 $\delta$（0.8,0.5,0.2,0.05）。
  - **实验3**：$n=30$，$d=2^\gamma$（$\gamma=1..5$），$s=d/2$ 个特征有差距 $h=0.5$，观测为Bernoulli，对比了Ariu等（2024）的Adaptive Clustering算法。
  - **实验4（附录）**：$n=d=1000$，改变组平衡度 $\theta$。
- **Benchmark**：
  - **K-means 均匀采样基线**：对所有 $n\times d$ 条目的相同次数均匀采样，然后对均值向量运行K-means，通过网格搜索调节预算使错误率≤0.01。
  - **Adaptive Clustering (Ariu et al., 2024)**：使用作者提供的MATLAB代码，固定预算 $T=400,000$，对比错误率。
- **对比方法**：仅对比上述两种基线的预算或错误率，未与其他主动聚类bandit算法直接对比。

#### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量或训练时长。实验运行在CPU环境（MATLAB和Python），每轮模拟次数为500或5000，总计算量不大，未提及任何大规模算力需求。

#### 5. 实验数量与充分性
- **实验数量**：共4组实验（正文3组+附录1组），每组覆盖参数变化（如稀疏度、规模、特征数、平衡度）。
- **充分性分析**：
  - 充分展示了算法在不同稀疏度、规模、特征维度、平衡度下的平均预算与错误率，并与两种基线对比。
  - **不足**：
    - 全部为合成数据，缺少真实应用场景（如众包标注、图像特征）的验证。
    - 对比方法仅两种：均匀K-means和Ariu2024（后者问题设定更复杂），缺乏与同类bandit聚类方法（如Yang et al. 2024, Thuot et al. 2025）的直接对比，虽然理论部分有讨论。
    - 实验1和4中置信度 $\delta$ 偏大（0.8、0.4），未测试更严格的 $\delta$（如0.05）下的预算增长是否符合理论。
    - 未进行消融实验（如去掉CSH子采样步骤的影响，或改变 $L$ 参数的影响）。

#### 6. 论文的主要结论与发现
- 提出了一种 **自适应、固定置信度** 的聚类算法 BanditClustering，能在未知间隙向量 $\Delta$ 和组平衡度 $\theta$ 的情况下高效聚类。
- 理论分析 **匹配** 了上下界，证明在稀疏常数间隙场景下算法最优，预算主要受 $d/(\theta\|\Delta\|_2^2)$ 和 $n/\Delta_{(s)}^2$ 支配。
- 实验表明：
  - 在稀疏场景下，BanditClustering 显著优于均匀K-means。
  - 当 $d$ 随 $n$ 增长时，算法预算仍近似线性于 $n$，而均匀采样会因维数灾难而失效。
  - 相比 Adaptive Clustering，本算法在特征数增多时仍保持低错误率且预算稳定。

#### 7. 优点：方法或实验设计上的亮点
- **理论完整**：同时给出上界和下界，并证明在稀疏常数间隙下紧的下界，是首个对此类主动聚类问题的精确刻画。
- **算法自适应**：无需预知 $\theta$、$\Delta$ 的稀疏模式，通过指数增长的预算自动适应难度。
- **子采样策略**：CSH中的子采样（$L$ 参数）借鉴了“多好臂识别”文献，能平衡探索与利用，避免过早陷入非最优特征。
- **实验设计合理**：
  - 分离两个子步骤（CR和CBC）的预算，便于理解瓶颈。
  - 实验2展示了算法随 $n$ 和 $d$ 同时增长时的可扩展性。
  - 实验3对比了不同特征数下的错误率，表明本算法对特征维数不敏感。

#### 8. 不足与局限
- **实验局限**：
  - 仅在合成高斯数据上验证，缺乏真实数据集（如UCI图像/文本分类）的测试。
  - 未与其他主动聚类bandit方法（如Thuot et al. 2025，Yang et al. 2024）直接实验对比，仅在理论部分提及。
  - 实验置信度 $\delta$ 取值较大（0.8等），未展示严格置信度下的性能。
  - 未提供消融实验，例如子采样大小 $L$ 的影响、CSH vs 其他搜索策略的对比。
- **应用限制**：
  - 假设组内所有物品特征向量完全相同，无法处理组内异质性。
  - 仅处理 $K=2$ 组，扩展到 $K>2$ 的算法（附录C）复杂度与 $K^2$ 成正比，且未证明最优性。
  - 未考虑观测噪声可能不是亚高斯的情况（如重尾噪声）。
- **理论局限**：
  - 下界仅对某些实例（稀疏常数间隙）紧，对一般 $\Delta$ 是否紧仅作猜想。
  - 上界中的对数因子（如 $\log^5(dn)$）较大，实际常数可能较高。

（完）
