---
title: Algorithms and Hardness for Active Learning on Graphs
title_zh: 图上主动学习的算法与难度
authors: "Vincent Cohen-Addad, Silvio Lattanzi, Simon Meierhans"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=EAnuqHF0tx"
tags: ["query:ac"]
score: 9.0
evidence: 直接研究图上的主动学习问题
tldr: 该论文研究图上的离线主动学习问题，旨在选择最优顶点集以预测所有顶点标签。针对一般加权图，首次提出O(log n)资源增强算法，并证明了常数近似难度。这项工作为主动学习在复杂图结构上的应用奠定了理论基础，是主动学习领域的重要进展。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 649, \"height\": 279, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1393, \"height\": 442, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 830, \"height\": 227, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 853, \"height\": 576, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 322, \"height\": 256, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 443, \"height\": 160, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1063, \"height\": 713, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1063, \"height\": 676, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1064, \"height\": 703, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-eanuqhf0tx/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1065, \"height\": 703, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2025-eanuqhf0tx/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 428, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-eanuqhf0tx/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 885, \"height\": 246, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-eanuqhf0tx/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1672, \"height\": 246, \"label\": \"Table\"}]"
motivation: 现有主动学习算法仅对树等特殊图有理论保证，缺乏对一般加权图的算法。
method: 提出首个针对一般加权图离线主动学习问题的O(log n)资源增强算法。
result: 该算法具有对数近似比，同时证明了问题的常数难度下界。
conclusion: 首次为一般图上的主动学习提供了理论保证和算法，推动了主动学习理论的发展。
---

## Abstract
We study the offline active learning problem on graphs. In this problem, one seeks to select k vertices whose labels are best suited for predicting the labels of all the other vertices in the graph.
Guillory and Bilmes (Guillory & Bilmes, 2009) introduced a natural theoretical model motivated by a label smoothness assumption. Prior to our work, algorithms with theoretical guarantees were only known for restricted graph types such as trees (Cesa-Bianchi et al., 2010) despite the models simplicity. We present the first O(log n)-resource augmented algorithm for general weighted graphs. To complement our algorithm, we show constant hardness of approximation.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义（研究动机和背景）
- **研究问题**：图上的离线主动学习——给定一个加权图，如何选择 `k` 个顶点（称为标签集 `L`），使得基于这些标签能最好地预测图中所有其他顶点的标签。
- **动机**：Guillory & Bilmes (2009) 提出基于标签平滑假设的理论模型，目标函数 `Ψ(L)` 定义为在不包含 `L` 的顶点子集中，最小切稀疏度（sparsity）。但此前仅有对树结构图（Cesa-Bianchi et al., 2010）存在理论保证的算法，对一般图只有启发式方法。
- **意义**：本文首次为**一般加权图**上的离线主动学习问题提供理论保证的算法（`O(log n)` 资源增强近似），同时证明问题的**常数近似难度**，填补了该领域的理论空白。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：将图标签选择问题转化为**最大流问题**，与最密集子图（densest subgraph）问题类比。构造一个流图（flow gadget）`bG_{L,τ}`，其中选择顶点加入 `L` 相当于添加从该顶点到汇点 `t` 的无限容量边，从而增加最大流值。若最大流达到 `n·τ`，则保证 `Ψ(L) ≥ τ`。
- **算法流程**：
  1. 通过二分搜索猜测最优阈值 `τ`。
  2. 初始化 `L = ∅`。
  3. 对于每个候选顶点 `v`，计算将其加入 `L` 后流图的最大流增量，选择增量最大的顶点加入。
  4. 重复直到 `|L|` 达到 `O(log n)·k` 或最大流达到 `n·τ`。
  5. 利用子模性（流图最大流函数是子模的）保证贪心算法在松弛 `O(log n)` 倍预算下达到最优值。
- **关键技术细节**：  
  - 流图构造：源点 `s` 连向每个顶点（容量 `τ`），原图边保留，选中顶点连汇点 `t`（容量 `W+τ+1`，视为无穷）。  
  - 证明等价的引理（Claim 4.5 和 4.6）：`Ψ(L) < τ` 当且仅当最小割值小于 `n·τ`。  
  - 通过四舍五入处理极大权重，实现 `O(log n/ϵ)` 松弛和 `ϵ` 误差的版本。
- **定理**：算法在多项式时间内返回 `|L'| = O(log n)·k` 的集合，使得 `Ψ(L') ≥ max_{|L|≤k} Ψ(L)`。

### 3. 实验设计
- **数据集/场景**：
  - **简单图**：星图 `S(50)`（50个顶点）、`3C(10,30,10)`（路径型图，中心团30节点，两端各10节点）。
  - **小型真实图**：Davis 南方女人社交图（32顶点）。
  - **SNAP真实图**：ca-GrQc（最大连通分量4158顶点，Arxiv合作网络）、ego-Facebook（4039顶点，社交网络）。
- **基准（Benchmark）**：
  - Guillory & Bilmes (2009) 的启发式算法（GB）。
  - Cesa-Bianchi et al. (2010) 的算法（BGVZ，原用于树，作者建议对一般图采样随机生成树后应用）。
- **评估指标**：直接比较达到的 `Ψ(L)` 值（无松弛，预算严格等于 `k`）。对随机算法报告多次（10次）均值±标准差。
- **额外实验**：附录中对四种 Watts-Strogatz 随机图（50顶点，不同度数和重链接概率）进行全预算扫描。

### 4. 资源与算力
- 文中**未明确说明**使用的 GPU 型号、数量或训练时长。所有实验均在中小规模图上运行（最大 4158 节点），算法复杂度为 `k·|V|·(|V|+|E|)^{1+o(1)}`，依赖最大流计算（使用标准库）。未涉及深度学习训练，因此无 GPU 算力需求。

### 5. 实验数量与充分性
- **实验数量**：共包含约 4 组主要实验（表1、表2、图4、附录中4个图），每组含若干预算设置。对比方法为2种基线。
- **充分性分析**：
  - 覆盖了简单人造图（揭示算法优势）和真实图。
  - 对 Davis 图和两个 SNAP 图分析了不同预算下的全趋势，结果清晰。
  - **不足**：
    - 缺少消融实验（如不同松弛参数的影响）。
    - 未与最新的深度主动学习方法比较（但本文属于理论驱动，方法不依赖深度学习）。
    - 未测试算法在超大规模图上的可扩展性（运行时间未报告）。
  - **客观性**：对比基线的结果均多次运行取均值，公平比较。但基线自身是启发式，本文算法理论上占优。

### 6. 主要结论与发现
- **理论成果**：
  - 首次给出一般加权图上离线主动学习问题的 `O(log n)` 资源增强算法（预算松弛 `O(log n)`，目标值不减）。
  - 证明该问题的常数近似难度（NP-hard 区分 `Ψ ≤ 2` 和 `Ψ ≥ 3`）。
- **实验发现**：
  - 在星图和人造“陷阱”图中，本文算法稳定达到最优值，而 GB 算法常选错顶点，BGVZ 算法在某些图中表现差。
  - 在真实图上，本文算法达到的 `Ψ(L)` 值通常优于或等于基线，尤其是预算较小时优势明显。
  - 当预算接近总顶点数时，GB 算法可能略优（因为此时更优策略是标记所有叶子，而本文算法优先标记高中心度顶点）。

### 7. 优点
- **理论贡献突出**：首次为一般图提供理论保证的算法，解决了长期开放问题。
- **技术巧妙**：将主动学习与最大流问题关联，利用子模性设计贪心策略，并给出松弛分析。
- **可扩展性**：算法可推广到带顶点重要性的情形（I-GLS），增加了实际适用范围。
- **实验验证**：虽然规模不大，但设计的人造图能直观揭示核心问题，真实图结果支持理论。

### 8. 不足与局限
- **实验规模与充分性**：实验仅覆盖中小规模图（≤4158节点），未在大规模图（百万节点）上测试，运行时间未报告，可扩展性存疑。
- **对比基线不足**：仅与两个经典启发式算法比较，未与现代基于学习的主动学习方法（如 GALAXY、GNN-based）对比（虽然那些方法侧重不同设置）。
- **理论松弛较大**：预算松弛因子 `O(log n)` 在实际中可能较大（例如 n=10^6 时约为20倍），且算法每一步需计算全图最大流，开销高。
- **应用限制**：模型假设标签满足平滑性，且目标函数仅基于图结构，忽略了特征信息；实际数据中噪声和异常值可能削弱模型有效性（作者提出了顶点重要性扩展但未充分实验）。
- **没有消融实验**：未分析不同参数（如四舍五入精度、最大流求解器）对结果的影响。

（完）
