---
title: Unified K-Means Clustering with Label-Guided Manifold Learning
title_zh: 基于标签引导流形学习的统一K均值聚类
authors: "Qianqian Wang, Mengping Jiang, Zhengming Ding, Quanxue Gao"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=YD9ZoqUDAY"
tags: ["query:ac"]
score: 4.0
evidence: 提出一种结合流形学习的新K均值聚类框架
tldr: 该论文针对K均值聚类对初始质心敏感、难以发现非线性流形结构等问题，提出一种结合流形学习的统一K均值聚类框架。该方法消除质心计算，利用聚类指示矩阵对齐流形结构，并引入高斯核距离和K近邻等机制。实验表明该方法在非线性数据集上提升了聚类准确性，为传统聚类算法提供了新的改进方向。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-yd9zoquday/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 836, \"height\": 384, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-yd9zoquday/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1753, \"height\": 390, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-yd9zoquday/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1774, \"height\": 779, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-yd9zoquday/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1764, \"height\": 811, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-yd9zoquday/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1758, \"height\": 419, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-yd9zoquday/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1767, \"height\": 404, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2025-yd9zoquday/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1765, \"height\": 556, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-yd9zoquday/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1767, \"height\": 551, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-yd9zoquday/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 896, \"height\": 509, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-yd9zoquday/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 857, \"height\": 260, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-yd9zoquday/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 841, \"height\": 531, \"label\": \"Table\"}]"
motivation: 针对K均值聚类对初始质心敏感、难以发现非线性流形结构以及聚类不平衡等问题，提出改进方法。
method: 提出无需质心计算的K均值框架，利用聚类指示矩阵对齐流形结构，并引入高斯核距离和K近邻。
result: 在多个非线性数据集上，新方法相比传统K均值聚类提升了聚类准确性和平衡性。
conclusion: 该工作增强了K均值聚类在复杂数据上的适用性，为结合流形学习的聚类算法提供了新思路。
---

## Abstract
K-Means clustering is a classical and effective unsupervised learning method attributed to its simplicity and efficiency. However, it faces notable challenges, including sensitivity to random initial centroid selection, a limited ability to discover the intrinsic manifold structures within nonlinear datasets, and difficulty in achieving balanced clustering in practical scenarios. To overcome these weaknesses, we introduce a novel framework for K-Means that leverages manifold learning. This approach eliminates the need for centroid calculation and utilizes a cluster indicator matrix to align the manifold structures, thereby enhancing clustering accuracy. Beyond the traditional Euclidean distance, our model incorporates Gaussian kernel distance, K-nearest neighbor distance, and low-pass filtering distance to effectively manage data that is not linearly separable. Furthermore, we introduce a balanced regularizer to achieve balanced clustering results. The detailed experimental results demonstrate the efficacy of our proposed methodology.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：传统K‑Means聚类存在三个主要缺陷：① 对随机初始质心敏感，易陷入局部最优；② 使用欧氏距离难以处理非线性可分数据；③ 聚类结果往往不平衡，小簇易被大簇吞并。
- **研究动机**：针对上述问题，论文旨在设计一种无需质心计算、能够自适应非线性数据结构、且能产生平衡聚类的统一K‑Means框架。通过将K‑Means转化为标签引导的流形学习版本，同时引入多种距离度量与平衡正则项，提升聚类鲁棒性和准确性。
- **整体含义**：该工作提出了一种中心化的、与流形学习紧密结合的K‑Means变体，在理论上统一了传统K‑Means、核K‑Means和模糊K‑Means，并显著改进了对复杂非线性数据的聚类性能。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过定理1，将传统K‑Means的目标函数等价转换为基于样本间距离和相似性矩阵的流形学习形式，从而消除对质心的依赖。
  - 相似性矩阵由聚类指示矩阵F构造：S = G Gᵀ，其中G = F P^{-1/2}，P为对角阵（Pkk = 第k簇样本数）。
  - 转化后目标：min tr(Gᵀ D G)，D为距离矩阵。
- **关键技术细节**：
  - **标签引导流形学习**：用可学习的指示矩阵F构建相似性矩阵，使数据流形结构与簇标签一致。
  - **平衡正则化**：引入ℓ₂,₁范数项 -λ‖Fᵀ‖₂,₁，通过定理2证明最大化该项可促使各簇样本数相等（即平衡聚类），同时迫使F退化为离散指示矩阵。
  - **四种距离度量**：① 欧氏平方距离；② K近邻距离（非邻居设为大常数）；③ 高斯核距离；④ 低通滤波距离（基于相似性矩阵R的滤波器特性，高相似映射为小距离）。
- **优化流程**：
  - 对ℓ₂,₁项进行一阶泰勒展开，迭代求解松弛后的子问题。
  - 逐行更新F：对每一行f_i，通过求解`min f_i (2 Fᵀ d_i - λ (h_i)ᵀ)`（h_i为H的第i行，H=FΛ，Λkk=1/‖f_k‖₂）得到硬分配。
  - 提供加速策略，每次迭代时间复杂度从O(n²c)降至O(nc)。

### 3. 实验设计：使用的数据集 / 场景、基准、对比方法

- **数据集**：使用10个基准数据集：CMUPIE, digits, FERET, Mpeg7, olivetti, Palm, Pendigits, PEAL, STL10, USPS；以及2个人造数据集（双螺旋数据集用于测试非线性可分能力；无结构随机点集用于验证聚类平衡性）。
- **基准**：无监督聚类任务，评价指标为ACC、NMI、Purity。
- **对比方法**：6种经典或最新方法——K‑Means、KKM（核K‑Means）、RKM（正则化K‑Means）、CDKM（坐标下降K‑Means）、K‑sum、K‑sum-x。

### 4. 资源与算力

- 论文正文中**未明确说明**GPU型号、数量或训练时长。实验环境为：Windows 11系统、13th Gen Intel Core CPU、MATLAB R2023a。所有实验均运行在CPU上，未使用GPU加速。

### 5. 实验数量与充分性：是否充分、客观、公平

- **实验数量**：在10个真实数据集上全面报告了ACC/NMI/Purity（表1-3），并额外进行了：
  - 2个玩具数据集上的可视化与定量分析（图2、3、表4）。
  - T‑SNE可视化（附录图4）。
  - 参数λ的敏感性分析（附录图5）。
  - 收敛性分析（附录图6）。
- **充分性**：充分涵盖了不同规模（小型至大型）、不同维度、不同领域（图像、手写数字、人脸）的数据集，并对比了多种基线方法。消融方面，通过四种距离度量的变体（Our‑ED、Our‑KNN、Our‑K‑ED、Our‑LF）对比，间接验证了各组件贡献。
- **客观性与公平性**：对比方法均为已发表或广泛使用的算法；参数设置部分在附录中公开；实验在相同环境（单CPU、MATLAB）下进行，但未提供多次随机种子下的标准差，可能影响统计显著性判断。总体而言，实验较为充分且设计合理。

### 6. 论文的主要结论与发现

- 提出的无质心K‑Means框架（UK‑Means）在大多数数据集上优于传统K‑Means及其变体，尤其在使用低通滤波距离时表现最佳。
- 平衡正则化项（ℓ₂,₁‑norm）能有效促使聚类结果接近平衡，并在无结构数据集上获得几乎均匀的簇大小。
- 低通滤波距离通过保留高相似样本、过滤低相似样本，对非线性可分数据（如双螺旋）有显著优势。
- 算法收敛快（约10次迭代内稳定），且参数λ对性能有单调先增后减的影响，需要适当调节。

### 7. 优点：方法或实验设计上的亮点

- **理论贡献**：严格证明了将K‑Means转化为中心化流形学习形式的等价性（定理1），以及ℓ₂,₁正则化促使平衡与离散指示矩阵的充要性（定理2）。
- **方法创新**：
  - 首个统一K‑Means、核K‑Means、模糊K‑Means为同一中心化框架的工作。
  - 引入低通滤波距离，新颖地利用信号处理中的滤波特性处理非线性数据。
- **实验设计**：
  - 使用人造数据集直观展示算法对非线性数据和平衡性的适应能力。
  - 提供了详细的参数分析与收敛性分析，便于复现。

### 8. 不足与局限

- **实验覆盖**：虽然使用了10个数据集，但缺少对高维文本数据或图数据的评估；对比方法中未包含最新的深度聚类方法（如Deep‑K‑Means），限制了算法在更广泛场景下的竞争力评估。
- **偏差风险**：参数λ在不同数据集上差异较大（附录A.4），未提供自动选取策略或调参的通用指南，实际应用时依赖手动搜索。
- **应用限制**：
  - 算法基于硬分配（严格0‑1），未提供软聚类输出。
  - 加速策略仅适用于离散指示矩阵，无法直接扩展到连续松弛版本。
  - 低通滤波距离依赖锚点图构建（通过DAS方法），锚点数量和邻域参数等超参数仍需额外设定。
- **算力报告缺失**：未说明运行时间或与基线方法的效率对比，仅给出了理论复杂度。

（完）
