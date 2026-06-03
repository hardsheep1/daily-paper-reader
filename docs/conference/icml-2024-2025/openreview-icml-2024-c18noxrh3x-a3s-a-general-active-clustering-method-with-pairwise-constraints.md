---
title: "A3S: A General Active Clustering Method with Pairwise Constraints"
title_zh: A3S：一种基于成对约束的通用主动聚类方法
authors: "Xun Deng, Junlong Liu, Han Zhong, Fuli Feng, Chen Shen, Xiangnan He, Jieping Ye, Zheng Wang"
date: 2024-05-02
pdf: "https://openreview.net/pdf?id=c18noxRh3X"
tags: ["query:active-clust"]
score: 10.0
evidence: 通用的主动聚类方法，使用成对约束
tldr: 主动聚类通过整合人工标注的成对约束来提升聚类性能，但传统方法在大数据集上查询成本高。本文提出自适应主动聚合与分裂（A3S）框架，在初始聚类结果上通过信息论指导的查询策略进行主动调整。理论证明该方法能高效提升归一化互信息，实验显示在多个基准数据集上以更少的查询次数达到更优聚类效果，是一种通用的主动聚类方法。
source: ICML-2024-Public
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2024-c18noxrh3x/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1749, \"height\": 441, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2024-c18noxrh3x/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1771, \"height\": 752, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2024-c18noxrh3x/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1771, \"height\": 736, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2024-c18noxrh3x/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 851, \"height\": 350, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2024-c18noxrh3x/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 852, \"height\": 348, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2024-c18noxrh3x/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 854, \"height\": 332, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2024-c18noxrh3x/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1765, \"height\": 363, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2024-c18noxrh3x/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 824, \"height\": 172, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2024-c18noxrh3x/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 861, \"height\": 204, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2024-c18noxrh3x/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 855, \"height\": 226, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2024-c18noxrh3x/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1430, \"height\": 149, \"label\": \"Table\"}]"
motivation: 现有主动聚类方法在大数据集上查询成本高，需要更高效的查询策略。
method: 提出A3S框架，在初始聚类基础上通过自适应聚合和分裂操作，基于归一化互信息增益进行主动查询。
result: 在多个数据集上，A3S用更少的成对约束查询实现了更高的聚类准确率。
conclusion: A3S是一种高效且通用的主动聚类方法，显著降低了查询成本。
---

## Abstract
Active clustering aims to boost the clustering performance by integrating human-annotated pairwise constraints through strategic querying. Conventional approaches with semi-supervised clustering schemes encounter high query costs when applied to large datasets with numerous classes. To address these limitations, we propose a novel Adaptive Active Aggregation and Splitting (A3S) framework, falling within the cluster-adjustment scheme in active clustering. A3S features strategic active clustering adjustment on the initial cluster result, which is obtained by an adaptive clustering algorithm. In particular, our cluster adjustment is inspired by the quantitative analysis of Normalized mutual information gain under the information theory framework and can provably improve the clustering quality. The proposed A3S framework significantly elevates the performance and scalability of active clustering. In extensive experiments across diverse real-world datasets, A3S achieves desired results with significantly fewer human queries compared with existing methods.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：主动聚类通过主动选择成对约束（must-link / cannot-link）来提升聚类质量。传统方法基于半监督聚类（SSC）的主动查询策略，在大规模、多类别数据集上需要大量人工查询，成本高昂。且现有基于簇调整的方案（如COBRA）需要预先指定簇数，对异常样本敏感，且无法区分低纯度的簇。
- **动机**：希望设计一种通用、高效、无需先验知识的主动聚类方法，在极少查询次数下显著提升聚类性能，并且理论上保证聚合操作不会降低归一化互信息（NMI）。
- **整体含义**：本文提出的A3S（自适应主动聚合与分裂）框架属于簇调整方案，通过信息论分析指导查询选择，结合自适应初始化与主动分裂/聚合操作，解决上述痛点。

## 2. 方法论
### 核心思想
- **两阶段流程**：
  1. **自适应初始化**：使用自适应聚类算法（如Fast Probabilistic Clustering）自动确定初始簇数（通常大于真实类别数），得到初始聚类结果。
  2. **主动聚合与分裂**：迭代选择最具信息增益的簇对，进行纯度检测，然后根据查询结果执行合并或分裂，同时利用快速传递推理更新约束。
- **理论支撑**：
  - **定理2.5**：当两个簇的纯度均≥0.7，且当前NMI ≥ 2·(1.0586 - min{purity})时，合并它们不会降低NMI。
  - **期望NMI增益**（式4）：E[ΔNMI | w_i,w_j] ∝ P(must-link) · Δh，其中Δh为合并前后的熵减，P(must-link)通过成对概率估计（式2）计算。
- **关键技术细节**：
  - **纯度测试**（式6）：首先通过密度测试（DT）判断簇是否紧密；若密度低，则查询中心样本与边界样本（含70%样本的球形边界），若结果cannot-link则认为簇不纯。
  - **查询策略**：先筛选聚合概率top-k的簇对，再计算期望NMI增益，选择最优对。
  - **分裂算法（Algorithm 2）**：对未通过纯度测试的簇，迭代地将样本分配给已有子簇或新建子簇，直至所有样本分配完毕，产生原子簇和异常样本。
  - **快速传递推理（Algorithm 3）**：当新约束加入时，只需更新与该样本相关的前缀约束，避免全量扩展，保证传递闭包正确性。

### 算法流程（文字说明）
1. 使用自适应聚类算法得到初始聚类Ω。
2. 循环直到查询预算耗尽或不再有操作：
   - 计算所有簇对的聚合概率，筛选top-k。
   - 对每个候选簇对，计算期望NMI增益，选择最大者。
   - 对选中的两个簇执行纯度测试：
     - 若均通过，查询中心样本对：must-link则合并，否则保持分离。
     - 若至少一个未通过，对该簇执行分裂（Algorithm 2）。
   - 更新约束矩阵（Algorithm 3），更新查询计数。

## 3. 实验设计
### 数据集
- 使用6个真实图像数据集：
  - MK20（20类，351样本）、MK100（100类，1650样本）：来自Market-1501。
  - Handwritten（10类，2000样本）：手写数字，含4种特征视图。
  - Humbi-Face（100类，5600样本）：人脸表情。
  - MS1M-10k（146类，10000样本）、MS1M-100k（1469类，100000样本）：大规模人脸。
- 特征维度、平衡性等信息见表1。

### 对比方法（Baselines）
- Random、FFQS、NPU：均基于PCKMeans半监督聚类。
- URASC：基于谱聚类的主动方法。
- COBRA：基于簇调整方法（使用相同初始簇数以公平比较）。
- 所有对比方法均被提供真实类别数（对A3S不提供，更具优势）。

### 评估指标
- 主指标：NMI、ARI。
- 辅助指标：裂变率Υ（结果簇数/真实类别数）、熵比r（H(Ω)/H(C)）、聚类纯度。

### 额外实验
- 不同初始化算法（K-means、Spectral、Agglo）下A3S的鲁棒性。
- 多视图特征（1/2/4视图）对性能的影响。
- 大规模数据（MS1M-10k和MS1M-100k）的扩展性。
- 不同自适应簇数（r=1,1.5,2）的消融。

## 4. 资源与算力
- 论文在Appendix B.5提及：
  - 特征提取使用GeForce RTX 3090 Ti GPU。
  - 主实验在Intel Xeon Platinum 8163 CPU @ 2.50GHz机器上运行。
  - 未提供GPU训练时长或具体数量，但给出了A3S的总运行时间（见表2）：MK20 0.93秒，MK100 5.58秒，Handwritten 6.39秒，Humbi-Face 37.62秒，MS1M-100k 8581.86秒。
- 总体算力需求较低（以CPU运行为主，仅特征提取需GPU）。

## 5. 实验数量与充分性
- **实验数量**：对比实验涉及4个数据集×5种方法×不同查询次数，生成多组曲线（图3）；消融实验包括初始化算法（3种）、视图数（3种）、簇数比（3种）；大规模实验2个；案例研究1个。总体实验量较为充分。
- **充分性与公平性**：
  - 对比方法均使用原论文超参数，且获得真实类别数作为先验，而A3S无此先验，条件对A3S更苛刻，结果仍明显优于基线。
  - 多次重复实验（未明确说明，但标注了误差线/标准差？从图3看有阴影区域，表示多次运行均值）。
  - 消融实验覆盖了关键组件（初始化、多视图、簇数）。
- **不足**：缺少在非图像数据（如文本、图数据）上的验证；缺少与最新半监督聚类方法（如PCSKM）的主动集成实验；未与某些更近期的主动聚类方法（如AQM+MEE）在同一设置下对比（仅在正文句子提及）。

## 6. 主要结论与发现
- A3S在所有数据集上均以远少于基线方法的查询次数达到更高或相近的NMI/ARI。例如MK100上A3S仅需约400次查询即可达到NMI 0.93，而基线方法需>4000次。
- A3S能够通过分裂操作处理异常样本和低纯度簇，克服COBRA等方法的性能天花板。
- A3S对初始聚类算法具有鲁棒性（即使使用不同的聚类算法，最终性能接近）。
- 多视图特征可显著提升初始纯度和最终A3S的收敛速度。
- 在大规模数据集MS1M-100k上，A3S在3100次查询内达到0.995 ARI，验证了可扩展性。
- 理论定理2.5在实践中得到验证：满足纯度条件时合并操作确实不会降低NMI。

## 7. 优点
- **理论驱动**：提供了簇聚合不降低NMI的充分条件，以及期望NMI增益的定量估计，使查询选择有理论基础。
- **无需先验类别数**：自适应初始化自动确定簇数，适用于陌生数据环境。
- **通用性**：可与多种聚类算法（K-means、谱聚类、层次聚类等）结合，不依赖特定算法。
- **高效查询**：通过纯度测试和异常分裂，避免在低质量簇上浪费查询；查询次数仅为传统方法的十分之一。
- **可扩展性**：复杂度为O(NM + A + k²)，适合大规模数据（验证了10万样本）。
- **实用策略**：快速传递推理避免重复查询；密度测试快速识别低纯度簇；分裂算法精细处理异常点。

## 8. 不足与局限
- **初始聚类质量依赖**：若初始聚类NMI过低（<0.2），A3S可能无法有效提升（论文作者提到此时半监督方法更优）。
- **纯度测试可能遗漏**：纯度测试只能发现纯度低于0.7的簇，对于纯度恰好0.7~0.8但含关键错误的簇无法触发分裂。
- **成对概率估计质量**：依赖isotonic regression和pseudo labels，若数据特征差或视图单一，估计不准会降低效果（但多视图可缓解）。
- **实验覆盖范围**：仅在图像数据集上测试，缺少文本、图数据等场景；与最新半监督聚类方法的主动集成未实验。
- **可解释性**：虽然理论部分提供了聚合条件，但实际中“期望NMI增益”的估计依赖多个近似，可能不总是最优。
- **百万级数据**：仅测试了10万级，论文展望未来将探索更大规模（百万级），但未在本文完成。

（完）
