---
title: Relative Error Fair Clustering in the Weak-Strong Oracle Model
title_zh: 弱强Oracle模型中的相对误差公平聚类
authors: "Vladimir Braverman, Prathamesh Dharangutte, Shaofeng H.-C. Jiang, Hoai-An Nguyen, Chen Wang, Yubo Zhang, Samson Zhou"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=70YgwpIxbc"
tags: ["query:active-clust"]
score: 5.0
evidence: 弱强oracle聚类模型，节点查询策略
tldr: 提出弱强oracle模型用于公平聚类，强oracle提供精确距离但成本高，弱oracle提供近似估计。目标是以最少强oracle查询得到近优解。论文首次给出了公平k-median聚类的(1+epsilon)-coresets，并扩展到k-means。该模型类似主动学习中的查询策略，有助于减少标注成本。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-70ygwpixbc/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1505, \"height\": 567, \"label\": \"Figure\"}]"
motivation: 解决公平聚类中精确距离昂贵而近似距离便宜但可能不准确的问题。
method: 提出弱强oracle模型，利用弱oracle的廉价估计和少量强oracle查询实现公平聚类。
result: 首次构造出公平k-median的(1+ε)-coresets，并给出中心点计算算法。
conclusion: 该模型有效权衡了查询成本与聚类公平性，为主动聚类提供了新视角。
---

## Abstract
We study fair clustering problems in a setting where distance information is obtained from two sources: a strong oracle providing exact distances, but at a high cost, and a weak oracle providing potentially inaccurate distance estimates at a low cost. The goal is to produce a near-optimal fair clustering on $n$ input points with a minimum number of strong oracle queries. This models the increasingly common trade-off between accurate but expensive similarity measures (e.g., large-scale embeddings) and cheaper but inaccurate alternatives. The study of fair clustering in the model is motivated by the important quest of achieving fairness with the presence of inaccurate information. We achieve the first $(1+\varepsilon)$-coresets for fair $k$-median clustering using $\text{poly}\left(\frac{k}{\varepsilon}\cdot\log n\right)$ queries to the strong oracle. Furthermore, our results imply coresets for the standard setting (without fairness constraints), and we could in fact obtain $(1+\varepsilon)$-coresets for $(k,z)$-clustering for general $z=O(1)$ with a similar number of strong oracle queries. In contrast, previous results achieved a constant-factor $(>10)$ approximation for the standard $k$-clustering problems, and no previous work considered the fair $k$-median clustering problem.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：在聚类任务中，精确的相似度度量（如大规模嵌入模型）往往计算成本高昂，而廉价但可能不准确的近似度量广泛存在（如基于元数据的启发式距离）。如何在保证聚类质量（尤其是公平性）的前提下，尽可能少地使用昂贵但精确的“强 oracle”查询，是该论文要解决的核心问题。
- **整体含义**：论文提出了**弱强 oracle 模型**（weak-strong oracle model），其中强 oracle 返回精确距离但成本高，弱 oracle 以大概率返回正确距离但有 1/3 概率返回任意错误值（且随机性只抽取一次）。在该模型下，首次为**公平 k-median 聚类**构造出 **\( (1+\varepsilon) \)-coreset**，并进一步推广到无公平约束的 \((k,z)\)-聚类（\(z=O(1)\)）。这标志着在近似距离信息下也能获得高精度、低查询成本的公平聚类解决方案。

#### 2. 方法论：核心思想、关键技术细节
- **核心思想**：利用**环采样（ring sampling）** 和**重击者采样 + 递归剥离（heavy-hitter sampling + recursive peeling）**。首先通过弱 oracle 获得一个 \(O(1)\)-近似聚类（使用 Proposition 2 的算法）。对于每个中心，将点按距离划分为递增的环。由于无法对所有点查询强 oracle，算法在每轮从剩余点中均匀采样少量点，仅对采样点查询强 oracle 以确定它们所属的环。然后识别“重”环（采样点足够多），为该环构造 coreset 并剥离该环及其内部所有点；对于“轻”环（采样点少），通过代价上界将它们的成本“转移”到相邻的重环上。如此递归，直到所有点被处理。
- **关键技术细节**：
  - **距离估计器**（Proposition 3）：对已知强 oracle 的点集 \(S\)，可利用少量弱 oracle 查询估计任意点 \(x\) 到 \(S\) 的距离，误差受 \(S\) 直径的常数倍控制。
  - **环划分**：以中心为圆心，按半径 \( (2C_0)^j R \) 划分环（\(C_0 \leq 5\) 为常数，\(R\) 为平均半径下界）。
  - **算法流程**（Algorithm 1 和 Algorithm 5）：
    1. 用 Proposition 2 得到弱近似聚类 \(WC\)。
    2. 对每个中心，重复多轮：采样 \(O(k\log^3 n / \varepsilon^3)\) 点，查询强 oracle 分配环；找到拥有最多采样点的环索引 \(j^*\)；对重环（采样数足够大）用 Coreset-Update 添加加权点；再通过 Proposition 3 的估计器和剥离步骤移除 \(j^*+1\) 环内的所有点；标记已处理环；递归。
    3. 针对公平聚类（assignment-preserving），算法（Algorithm 5）在每轮从剥离的所有点中均匀采样并加权加入 coreset，以保证分配约束。
  - **公平 coreset 与 assignment-preserving coreset 的关系**：利用 Proposition 1，对每个子群体分别构造 assignment-preserving coreset 再取并集即可得到公平 coreset。
- **复杂度**：核心结果（Theorem 1）：强 oracle 点查询数 \(\widetilde{O}(\Lambda \cdot k)\)，弱 oracle 查询数 \(\widetilde{O}(\Lambda \cdot nk)\)，coreset 大小 \(\widetilde{O}(\Lambda \cdot k^2 / \varepsilon^2)\)，时间 \(\widetilde{O}(\Lambda \cdot (nk + k^2/\varepsilon^2))\)。

#### 3. 实验设计
- **数据集**：两个真实世界数据集：
  - **Adult**（约 50,000 实例，8 特征）
  - **Default of Credit Card Clients**（约 30,000 实例，9 特征）
- **敏感属性**：性别（gender）。
- **预处理**：先将数据转为数值表示；因计算资源有限，用 Meyerson 采样将每个数据集子采样到约 2000 个点。
- **弱 oracle 设计**：在错误情况下返回非常小的距离（论文中说明是“returns very small distances if it runs into an erroneous case”）。
- **对比方法**：**均匀采样 baseline**（uniformly random sampling and re-weighting），均采样 100 个点作为 coreset。
- **评估指标**：相对成本 = (数据集上的成本 - coreset 上的成本) / 数据集上的成本。使用 fairtree 算法（Backurs et al., 2019）在数据集和 coreset 上运行 k-median 公平聚类。参数 \(p=1, q=10\)。
- **报告方式**：对每个 \(k\) 值（5 到 10）报告 10 次独立运行的平均相对成本。

#### 4. 资源与算力
- **文中明确说明**：所有实验均在本地 Macbook Air 上运行。未提及 GPU 型号、数量或训练时长，仅表示计算资源有限。

#### 5. 实验数量与充分性
- **实验数量**：两个数据集，每个数据集上 \(k\) 从 5 到 10 共 6 个值，每个配置 10 次独立运行。未进行消融实验（如不同采样率、不同 ε 的影响、不同公平参数等）。
- **充分性评价**：实验规模较小，仅验证了所提算法优于均匀采样 baseline，但未能展示与更多其他 coreset 算法（如 Chen 2009 或 Braverman 2022 的变体）的对比，也未测试强 oracle 查询数量变化的影响。实验公平性和客观性可接受，但充分性不足。

#### 6. 主要结论与发现
- 所提出的公平 coreset 算法在所有测试的 \(k\) 值下，相对成本均低于均匀采样 baseline，且差距显著（见 Figure 1）。
- 仅使用约 200 次强 oracle 查询（占数据量约 10%）即可实现优于 uniform 的效果。
- 实验表明在弱强 oracle 模型下构造的 fair coreset 在实际数据上是有效的。

#### 7. 优点
- **理论创新**：首次在弱强 oracle 模型下实现公平 k-median 的 \((1+\varepsilon)\)-coreset，且查询复杂度接近最优（匹配下界 \(\Omega(k)\) 至 polylog 因子）。
- **技术充分性**：方法结合了重击者采样、递归剥离和环采样，避免了直接对所有点查询强 oracle，同时保持了 coreset 的高精度。
- **适用性**：可扩展到一般 \((k,z)\)-clustering（包括 k-means），且对重叠群体有处理（\(\Lambda\) 因子）。
- **实验直观性**：在真实数据上验证了超出均匀采样的效果，且 SO 查询比例低。

#### 8. 不足与局限
- **实验局限性**：仅使用了两个小规模子采样数据集，且未在更大规模或其他类型数据上测试；未与现有的无弱强 oracle 模型的公平聚类 coreset 方法（如 Schmidt 2019, Bandyapadhyay 2024）直接对比，因为那些方法假设精确距离已知。
- **弱 oracle 设置**：人为设计弱 oracle 的错误模式（返回非常小的距离），可能不能完全代表实际应用中的随机或对抗性错误。
- **缺少超参数敏感性分析**：未研究不同采样率、环数、ε 值对结果的影响。
- **理论假设**：假设距离为整数且 aspect ratio = poly(n)，以及弱 oracle 的失败概率为常数（1/3）且随机性只抽取一次，这些假设在实际中可能不总是满足。
- **可扩展性**：强 oracle 查询复杂度包含 polylog 因子，且 \(n\) 很大时弱 oracle 查询 \(O(nk\log^3 n)\) 可能仍然较高。

（完）
