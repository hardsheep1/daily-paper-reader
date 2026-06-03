---
title: Query-Efficient Correlation Clustering with Noisy Oracle
title_zh: 带有噪声Oracle的查询高效相关聚类
authors: "Yuko Kuroki, Atsushi Miyauchi, Francesco Bonchi, Wei Chen"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=WRCFuoiz1h"
tags: ["query:active-clust"]
score: 9.0
evidence: 通过噪声oracle进行查询高效的主动聚类
tldr: 针对相关聚类问题，传统方法需要大量相似度查询，代价高昂。本文提出将聚类问题建模为纯探索组合多臂老虎机，在固定置信度和固定预算两种设定下，设计融合采样策略与经典近似相关聚类算法的新方法。理论保证算法在多项式时间内达到最优查询复杂度，实验验证其在多种噪声场景下的有效性。与现有方法相比，显著减少了查询次数并保持近似精度。这项工作为主动聚类中的查询策略提供了理论保障和高效算法，可推广至各类应用。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-wrcfuoiz1h/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1419, \"height\": 294, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-wrcfuoiz1h/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1417, \"height\": 290, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-wrcfuoiz1h/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1416, \"height\": 293, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2024-wrcfuoiz1h/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 797, \"height\": 277, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-wrcfuoiz1h/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 876, \"height\": 170, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-wrcfuoiz1h/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1409, \"height\": 446, \"label\": \"Table\"}]"
motivation: 现有聚类算法假设可获取全部相似度信息，但实际中相似度计算代价高且含噪声，因此需要最小化查询次数。
method: 将聚类问题建模为纯探索组合多臂老虎机，结合采样策略与近似相关聚类算法，设计固定置信度和固定预算两种算法。
result: 理论分析和实验表明，算法在保证近似比的同时，显著降低了查询复杂度，是首个多项式时间的主动聚类方法。
conclusion: 本文为噪声环境下主动聚类提供了理论保障和实用算法，可推广至多种应用场景。
---

## Abstract
We study a general clustering setting in which we have $n$ elements to be clustered, and we aim to perform as few queries as possible to an oracle that returns a noisy sample of the weighted similarity between two elements. Our setting encompasses many application domains in which the similarity function is costly to compute and inherently noisy. We introduce two novel formulations of online learning problems rooted in the paradigm of Pure Exploration in Combinatorial Multi-Armed Bandits (PE-CMAB): fixed confidence and fixed budget settings. For both settings, we design algorithms that combine a sampling strategy with a classic approximation algorithm for correlation clustering and study their theoretical guarantees. Our results are the first examples of polynomial-time algorithms that work for the case of PE-CMAB in which the underlying offline optimization problem is NP-hard.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：相关聚类（Correlation Clustering）是一种不依赖先验簇数的聚类方法，广泛应用于重复检测、垃圾邮件过滤、生物信息学等领域。传统方法需要预先计算全部 $\Theta(n^2)$ 个元素对的相似度，这在大型数据集上代价高昂，且在实际应用中相似度测量往往伴有噪声（如实验误差、人为偏差）。
- **核心问题**：在仅能查询一个返回噪声相似度样本的 Oracle 的条件下，如何以最少的查询次数获得高质量聚类结果？
- **整体含义**：本文首次将相关聚类与纯探索组合多臂老虎机（PE-CMAB）范式结合，在固定置信度（Fixed Confidence）和固定预算（Fixed Budget）两种设定下，提出了多项式时间的算法，解决了 PE-CMAB 中离线优化问题为 NP-hard 时的算法设计难题。

### 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：
  - 利用阈值老虎机（Threshold Bandits）识别“高相似度”的元素对，然后将其输入经典的 KwikCluster 近似算法。
  - 通过将 NP-hard 的聚类问题转化为可处理的“识别相似度是否高于 0.5”的二值学习问题，规避了离线优化 NP-hard 带来的统计复杂性。

- **关键技术细节**：
  - **固定置信度算法 KC-FC**：
    1. 调用阈值老虎机算法 TB-HS，以误差参数 $\epsilon' = \epsilon/(12m)$ 和置信度 $\delta$，输出被认为是高相似度的边集合 $\hat{G}_{\epsilon'}$。
    2. 运行 KwikCluster，在每一轮随机选择一个 pivot，将 pivot 及 $\hat{G}_{\epsilon'}$ 中与之相连的未聚类元素合并为一个簇，直至所有元素被聚类。
    3. 理论保证：输出解的期望代价不超过 $5\cdot \text{OPT} + \epsilon$，查询复杂度为 $O\left(\sum_{e\in E} \frac{1}{\tilde{\Delta}_{e,\epsilon'}^2}\log\left(\frac{n}{\tilde{\Delta}_{e,\epsilon'}^2\delta}\log(\cdots)\right) + \frac{n^2}{\max\{\Delta_{\min},\epsilon'/2\}^2}\right)$。
  - **固定预算算法 KC-FB**：
    1. 分配初始预算 $\tau_1 = \lfloor T/m\rfloor$ 给所有 $m$ 个元素对。
    2. 在每一阶段 $r$，随机选取 pivot $p_r$，对 $p_r$ 与 $V_r$ 中其他元素构成的边各查询 $\tau_r$ 次，根据经验均值 $\hat{s}_r(e)$ 将高于 0.5 的邻居归入簇 $C_r$。
    3. 动态更新下一阶段预算 $\tau_{r+1}$，将未被查询的边的预算重新分配给剩余边。
    4. 理论保证：输出解为 $(5,\epsilon)$-近似解的概率至少为 $1 - 2n^3\exp(-2T\Delta_{\min,\epsilon}^2/n^2)$。

- **算法流程（文字说明）**：
  - KC-FC：TB-HS（双阈值 LCB/UCB 采样）→ 构建高相似图 → KwikCluster 聚类。
  - KC-FB：逐阶段 pivot 选择 → 限定查询次数 → 基于经验均值构建簇 → 重分配预算。

### 3. 实验设计

- **数据集**：使用 7 个公开真实图数据集（Lesmis, Adjnoun, Football, Jazz, Email, ego-Facebook, Wiki-Vote），节点数从 77 到 7066 不等。
- **场景构建**：
  - **FC 设置**：人为控制最小间隔 $\Delta_{\min}$（0.10~0.50），边或非边分别从 $[0.5+\text{LB},1]$ 和 $[0,0.5-\text{LB}]$ 均匀采样。
  - **FB 设置**：使用 node2vec 将图节点嵌入 64 维空间，用余弦相似度归一化至 $[0,1]$ 作为真实相似度。
- **对比方法**：
  - KC-FC vs. Uniform-FC（均匀采样+相同近似算法）。
  - KC-FB vs. Uniform-FB（均匀采样+相同近似算法）。
  - 此外与拥有真实相似度的 KwikCluster 作为强基线比较。
- **评价指标**：聚类代价（cost）、样本复杂度（查询次数）。

### 4. 资源与算力

- 文中说明实验在 **Apple M1 Chip 和 16 GB RAM** 的机器上完成，**未使用 GPU**，也未报告具体训练时长。
- 代码使用 Python 3 编写，已公开于 GitHub。

### 5. 实验数量与充分性

- **实验数量**：FC 设置下对 4 个小数据集（n<1000）进行了 5 个不同 $\Delta_{\min}$ 值的实验，每组运行 100 次；FB 设置下对 4 个小数据集做了 10 个不同预算 T（从 $n^{2.1}$ 到 $n^{3.0}$）的实验，以及对 3 个大数据集（n≥1000）在固定预算 $T=n^{2.2}$ 下实验。
- **充分性**：覆盖了不同规模、不同相似度分布（可控噪声边界 vs. 真实嵌入）的场景，并对比了均匀采样基线，结果展示了明显的优势。但缺少对多种噪声分布（如亚高斯噪声）的实验验证，也没有进行消融实验分析各组件（如 TB-HS 内部参数）的贡献。

### 6. 主要结论与发现

- 提出的 KC-FC 和 KC-FB 在绝大多数设置下均 **显著优于均匀采样基线**，且与使用真实相似度的 KwikCluster 性能接近。
- 在 FC 设置下，样本复杂度随 $\Delta_{\min}$ 增大而降低，体现了算法对问题实例的适应性。
- 在 FB 设置下，KC-FB 通过预算再分配实现了更优的聚类代价，尤其在预算紧张时效果更明显。
- 理论保证与实验一致，证明算法在实践中的有效性。

### 7. 优点

- **理论创新**：首次为 PE-CMAB 中离线 NP-hard 问题提供了多项式时间算法，填补了领域空白。
- **算法简洁实用**：结合阈值老虎机与经典 KwikCluster，易于实现。
- **样本高效**：利用问题结构将学习焦点缩小到“相似度是否大于 0.5”，而非估计精确值，从而显著降低查询次数。
- **双重设定覆盖**：分别应对“允许停机”和“固定预算”两个实际场景，提供适配的算法与分析。

### 8. 不足与局限

- **实验覆盖**：未在更大规模（如百万节点）或更复杂噪声模型（如异方差、非伯努利分布）下验证。
- **偏差风险**：FB 设置下的实验仅采用了一个固定预算 $T=n^{2.2}$ 用于大规模图，可能未充分展示算法在不同预算下的行为。
- **缺少消融**：未对 KC-FC 中 TB-HS 的阈值参数 $\epsilon'$ 设计进行敏感性分析。
- **应用限制**：假设噪声为独立同分布且均值等于真实相似度，不适用于有偏或对抗性噪声。
- **理论基础**：缺少信息论下界，无法证明算法的最优性。

（完）
