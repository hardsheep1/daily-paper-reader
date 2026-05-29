---
title: "Clustering Items through Bandit Feedback: Finding the Right Feature out of Many"
title_zh: 通过赌博机反馈进行物品聚类：从众多特征中寻找正确特征
authors: "Maximilian Graf, Thuot Victor, Nicolas Verzelen"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=99zsyZpUqp"
tags: ["query:ac"]
score: 5.0
evidence: 利用赌博机反馈进行主动聚类：通过顺序选择物品和特征以减少观测次数
tldr: 针对带有高斯噪声的聚类问题，提出一种基于赌博机反馈的主动学习算法，在每一步选择物品和特征进行观测，利用序列消除算法找到相关特征，以最少观测次数恢复正确聚类划分。理论分析证明其样本复杂度最优。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 697, \"height\": 518, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 675, \"height\": 530, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 839, \"height\": 512, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-99zsyzpuqp/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 778, \"height\": 594, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2025-99zsyzpuqp/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 869, \"height\": 295, \"label\": \"Table\"}]"
motivation: 在特征维度高且观测成本高的场景下，需要主动选择特征来高效聚类。
method: 利用序列消除算法在赌博机反馈下选择相关特征，逐步优化聚类。
result: 算法能够以较少的观测次数恢复正确聚类，并具有理论保证。
conclusion: 主动选择特征是降低聚类观测成本的有效策略。
---

## Abstract
We study the problem of clustering a set of items based on bandit feedback. Each of the $n$ items is characterized by a feature vector, with a possibly large dimension $d$. The items are  partitioned into two unknown groups, such that items within the same group share the same feature vector. We consider a sequential and adaptive setting in which, at each round, the learner selects one item and one feature, then observes a noisy evaluation of the item's feature. The learner's objective is to recover the correct partition of the items, while keeping the number of observations as small as possible. 
We provide an algorithm which relies on finding a relevant feature for the clustering task, leveraging the Sequential Halving algorithm. With probability at least $1-\delta$, we obtain an accurate recovery of the partition and derive an upper bound on the required budget . Furthermore, we derive an instance-dependent lower bound, which is tight in some relevant cases.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究问题**：在特征维度 \(d\) 可能很大且每次只能观测一个“物品-特征”对（含噪声）的约束下，如何以最少的观测次数将 \(n\) 个物品正确划分为两个未知组？
- **动机**：实际场景中（如众包图像标注），平台需通过提问（特征）获取信息，但提问成本高，且不同特征对区分类别的贡献差异大。利用自适应采样可集中预算于最有区分力的特征，而非均匀采样全部特征。
- **整体含义**：该工作首次在纯探索赌博机框架下系统刻画两类别聚类问题的样本复杂度，并设计出最优（至多对数因子）的自适应算法。

## 2. 论文提出的方法论

### 核心思想
1. **三步走策略**：
   - **第一步**：选定一个固定代表 \(r_0\)（如物品1），利用 **CompareSequentialHalving (CSH)** 自适应地找到另一个组的代表 \(r_1\)。
   - **第二步**：已知两个代表后，再次使用 **CSH** 找到一个最区分两组的特征 \(j\)（即 \(|\Delta_j|\) 足够大）。
   - **第三步**：利用该特征对全体物品执行二分类（阈值检验），分配组标签。

2. **关键技术**：
   - **CSH 算法**：一种基于顺序消除的变体，在固定预算下通过子采样（参数 \(L\)）平衡探索与利用，保证高概率返回间隙 \(\geq \Delta_{(s)}/2\) 的条目。
   - **指数增长预算网格**：算法 2（CandidateRow）和算法 3（ClusterByCandidates）均以 \(T_k = 2^{k+1}\) 递增预算，自动适应未知的稀疏度 \(s\) 和间隙大小。
   - **子采样机制**：CSH 初始随机抽取 \(2^L\) 个候选（物品-特征对），后续逐步淘汰，避免早期浪费预算在大量无关条目上。

3. **关键公式**（文字描述）：
   - 样本复杂度上界主要由两项组成：  
     - 寻找代表：\(\frac{d}{\theta \|\Delta\|_2}\log(1/\delta)\)  
     - 分类：\(\min_s\left(\frac{d}{s} + n\right)\frac{1}{\Delta_{(s)}^2}\log(1/\delta)\)  
     其中 \(\theta\) 为小组比例，\(\Delta_{(s)}\) 为排序后第 \(s\) 大间隙。

## 3. 实验设计

### 实验场景与数据集
- **全部为合成数据**，无真实数据集。
- **基础设置**：\(M\) 的行取自两个均值向量（0 和 \(\Delta\)），观测噪声为标准高斯。

### 对比方法
1. **均匀采样 + K-means**（基线）：对每个条目取相同次数样本，再对行向量做 k-means 聚类。
2. **Adaptive Clustering**（Ariu et al., 2024）：固定预算下的主动聚类算法（用于实验 3）。

### 实验设置
- **实验 1**：\(n=20, d=1000\)，改变稀疏度 \(s\)（对应间隙大小 \(h_s = 15/\sqrt{s}\)），报告候选行检测（CR）和候选聚类（CBC）各自的平均预算。
- **实验 2**：\(n\) 和 \(d=10n\) 共同增长，测试不同置信度 \(\delta\)。
- **实验 3**：固定 \(n=30\)、\(\theta=0.5\)，改变特征数 \(d_\gamma = 2^\gamma\)，比较与 Adaptive Clustering 的误差率和预算。
- **实验 4（附录）**：固定 \(n=d=1000\)，改变小组比例 \(\theta\)。

## 4. 资源与算力

- **文中未明确说明**所使用的 GPU 型号、数量或训练时长。
- 仅提及使用 Python（Scikit-learn）和 MATLAB 进行仿真，每次实验运行 500 次或 5000 次独立重复。

## 5. 实验数量与充分性

- **实验数量**：4 组主要实验（含附录 1 组），每组均重复 5000 次（部分 500 次），并报告均值和 5%/95% 分位数。
- **充分性**：
  - ✅ 覆盖了稀疏度、维度、置信度、类别平衡度等多个关键参数的影响。
  - ✅ 对比了两种基线（均匀采样+K-means 和 Adaptive Clustering）。
  - ✅ 使用分位数和误差控制线，统计可靠性较好。
  - ❌ 缺乏真实数据验证；未与更多同类主动聚类方法（如 Yang et al. 2024）直接对比。

## 6. 论文的主要结论与发现

1. **自适应采样显著优于均匀采样**：在稀疏场景下，本算法预算远低于均匀基线（如实验 1 中预算随 \(s\) 线性增长，而非 \(nd\)）。
2. **预算主要由第二步（CBC）主导**：寻找代表（CR）的预算基本恒定，分类阶段预算与 \(s\) 成正比。
3. **理论下界匹配**：当 \(\Delta\) 为常数值的稀疏向量时，上界与下界（\(\frac{d}{\theta \|\Delta\|_2}\) 和 \(\frac{n}{\Delta_{(1)}^2}\)）匹配，证明算法最优。
4. **对特征数不敏感**：实验 3 显示，随特征数增加 Adaptive Clustering 误差上升，而本算法误差保持极低且预算几乎不变。

## 7. 优点

- **主动选择特征**：突破传统必须观测全部特征的限制，大幅降低样本复杂度。
- **理论保障完整**：同时给出高概率上界和实例化下界，并证明在某些情形下最优。
- **自适应性强**：无需知道稀疏度 \(s\)、间隙大小或组平衡度，算法自动指数增长预算适配未知难度。
- **可扩展性**：提供 K>2 组的扩展算法（虽非最优），思路清晰。

## 8. 不足与局限

- **模型假设过强**：要求组内所有物品特征向量完全相同，实际中组内变异可能导致分类错误。
- **仅验证合成数据**：缺乏真实众包或图像标注数据的实验，实用性待检验。
- **K>2 扩展非最优**：算法 5 的样本复杂度为 \(O(K^2)\)，在聚类中心几何关系简单时可进一步优化。
- **噪声假设限制**：仅考虑 1-subGaussian 噪声，对非高斯或异方差环境未分析。
- **对数因子较多**：上界中含 \(\log(\cdot)^5\) 等项，在小规模问题中可能不够高效。

（完）
