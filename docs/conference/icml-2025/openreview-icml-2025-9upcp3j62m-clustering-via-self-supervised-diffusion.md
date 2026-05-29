---
title: Clustering via Self-Supervised Diffusion
title_zh: 基于自监督扩散的聚类
authors: "Roy Uziel, Irit Chelly, Oren Freifeld, Ari Pakman"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=9uPCP3j62m"
tags: ["query:ac"]
score: 4.0
evidence: 提出一种利用扩散模型的自监督聚类框架
tldr: 该论文探索将扩散模型应用于聚类任务，提出自监督聚类框架CLUDI。通过教师-学生范式，教师使用随机扩散采样产生多样化聚类分配，学生精炼为稳定预测。该方法在多个高维数据集上取得最先进的无监督分类性能，展示了扩散模型在聚类中的巨大潜力。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 804, \"height\": 894, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 850, \"height\": 467, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 856, \"height\": 280, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1729, \"height\": 820, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1771, \"height\": 1004, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 779, \"height\": 564, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 775, \"height\": 576, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 703, \"height\": 1079, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1716, \"height\": 1502, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1714, \"height\": 1505, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-9upcp3j62m/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1460, \"height\": 2108, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2025-9upcp3j62m/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 855, \"height\": 1723, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-9upcp3j62m/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 865, \"height\": 1115, \"label\": \"Table\"}]"
motivation: 扩散模型在生成任务中成功，但尚未被应用于聚类任务。
method: 提出CLUDI，结合扩散模型和Vision Transformer特征，通过教师-学生范式进行自监督聚类。
result: 在多个挑战性数据集上，CLUDI在无监督分类任务中达到最先进水平。
conclusion: 该工作首次将扩散模型成功应用于聚类，为无监督学习开辟了新方向。
---

## Abstract
Diffusion models, widely recognized for their success in generative tasks, have not yet been applied to clustering. We introduce Clustering via Diffusion (CLUDI), a self-supervised framework that combines the generative power of diffusion models with pre-trained Vision Transformer features to achieve robust and accurate clustering. CLUDI is trained via a teacher–student paradigm: the teacher uses stochastic diffusion-based sampling to produce diverse cluster assignments, which the student refines into stable predictions. This stochasticity acts as a novel data augmentation strategy, enabling CLUDI to uncover intricate structures in high-dimensional data. Extensive evaluations on challenging datasets demonstrate that CLUDI achieves state-of-the-art performance in unsupervised classification, setting new benchmarks in clustering robustness and adaptability to complex data distributions.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：聚类是无监督学习的基础任务，但传统方法在高维复杂数据（如图像）中往往难以捕捉内在结构，而现有深度聚类方法常面临模型坍塌（representation collapse）或未能充分利用预训练特征的问题。
- **背景**：扩散模型在生成任务（图像、文本等）中表现卓越，但尚未被应用于聚类。证据表明，预训练的 Vision Transformer（如 DINO）特征可极大地提升聚类性能，但如何在此基础上进一步挖掘数据中的聚类结构仍是一个开放挑战。
- **整体含义**：本文提出 CLUDI（Clustering via Diffusion），首次将扩散模型引入聚类，利用其随机采样能力生成多样化的聚类分配，并通过自蒸馏获得稳定预测，从而在高维数据中实现鲁棒且准确的聚类。

### 2. 论文提出的方法论
- **核心思想**：使用预训练的 ViT 提取图像特征，然后训练一个以特征为条件、输出聚类分配嵌入（assignment embeddings）的扩散模型。推理时通过多次采样并平均概率向量获得最终聚类。
- **关键技术细节**：
  - **扩散模型**：采用 DDIM 变体，初始噪声从 \( \mathcal{N}(0, F^2 \mathbf{I}) \) 采样，经 25 步反向去噪得到嵌入 \( z_0 \in \mathbb{R}^d \)。使用 sqrt 噪声调度。
  - **分类头**：将 \( z_0 \) 通过线性层 \( L \in \mathbb{R}^{K \times d} \) 和带温度 \( \tau \) 的 softmax 得到类别概率 \( p(k|x) \)。
  - **教师-学生自蒸馏**：教师模型（扩散模型）生成去噪后的嵌入 \( z_0 \) 和概率 \( u \)；学生模型以增强后的特征（特征丢弃 + 高斯噪声）为输入，预测目标嵌入 \( \hat{z}_0 \) 和概率 \( \hat{u} \)。
  - **损失函数**：两个部分：
    1. 嵌入的 MSE 损失 \( \ell_{\text{dif}} \)（结合 Min-SNR-γ 加权）。
    2. 分类的对称交叉熵损失 \( \ell_{\text{cls}} \)，引入均匀先验避免坍塌（借鉴 Self-Classifier 的方法）。
  - **总损失**：\( \mathcal{L} = \frac{1}{BN} \sum w_b ( \ell_{\text{dif}} + \lambda \ell_{\text{cls}} ) \)。
- **公式/算法流程**（文字说明）：
  1. 输入特征 \( x \)。
  2. 教师从噪声开始，经 25 步去噪得到嵌入 \( z_0 \)。
  3. 通过 \( L \) 与 softmax 得到概率 \( u \)。
  4. 学生以增强特征为输入，在随机时间步对教师嵌入加噪后进行一步去噪预测 \( \hat{z}_0, \hat{u} \)。
  5. 计算两个损失并反向传播（仅更新学生）。

### 3. 实验设计
- **数据集**：ImageNet 子集（50/100/200 类）、Oxford-IIIT Pets (K=32)、Oxford 102 Flower (K=102)、Caltech 101、CIFAR-10、STL-10。
- **Benchmarks**：使用 DINO (ViT-S/16 和 ViT-B/16) 提取固定特征。
- **对比方法**：SCAN、Propos、TSP、TEMI（均为自监督聚类方法），以及自己实现的 Self-Classifier*（冻结特征、仅优化分类头）。
- **评估指标**：标准化互信息（NMI）、聚类准确率（ACC）、调整兰德指数（ARI）。

### 4. 资源与算力
- **文中未明确说明**：未提及使用的 GPU 型号、数量或训练时长。附录中仅给出超参数扫描细节，无算力信息。

### 5. 实验数量与充分性
- **实验数量**：共 8 个数据集（ImageNet 三子集 + 其他 5 个），结果见表 1 和表 2；此外还有消融实验（λ 扫描、d 扫描、F² 扫描）以及 t-SNE 可视化。
- **充分性**：覆盖了广泛的数据规模和难度（从 10 类到 200 类，细粒度到通用），对比了主流方法。但 Self-Classifier* 是自行实现的变体，可能未完全公平；未与近期的 DeepDPM、IIC 等对比。消融实验系统但仅限 ImageNet-100 验证集。整体较充分，但 benchmarks 可更全面。

### 6. 论文的主要结论与发现
- CLUDI 在所有测试数据集上均达到**最先进**性能，尤其在 ImageNet 子集上大幅超越先前方法。
- 扩散模型的随机性可作为一种新颖的数据增强策略，提升聚类鲁棒性。
- 教师-学生自蒸馏结合两种损失（嵌入 + 概率）可有效避免模型坍塌。
- 超参数 \( F^2 \)（噪声缩放因子）和嵌入维度 \( d \) 对性能影响显著，需要仔细调节。

### 7. 优点
- **方法新颖**：首次将扩散模型引入聚类，开辟了新方向。
- **设计精巧**：
  - 利用扩散的随机性生成多样化分配，避免局部最优。
  - 自蒸馏框架无需复杂的数据增强（如裁剪/颜色变换），仅对特征进行简单增强。
  - 两种损失互补，结合 Min-SNR 加权和均匀先验有效防止坍塌。
- **性能优异**：在多个基准上均明显优于现有方法，尤其细粒度数据集。
- **可视化验证**：t-SNE 展示嵌入空间分簇良好，高/低置信度样本可视化直观。

### 8. 不足与局限
- **超参数敏感**：\( F^2 \)、\( d \)、\( \lambda \) 需要针对数据集调优，未提供自动选择方案。
- **扩展性**：当类别数 \( K \) 很大时，维持高质量嵌入可能更复杂，论文仅测试到 200 类，未在大规模如完整 ImageNet-1000 上验证。
- **对比不够全面**：未与近年同样使用扩散思想的聚类方法（如基于流或分数的其他模型）对比；未与无监督分类中假设分布形式的方法（如 DeepDPM）比较。
- **算力缺失**：未报告训练时间或资源需求，难以评估实际落地成本。
- **理论分析薄弱**：未从理论上解释为何扩散模型能在嵌入空间中有效聚类，仅凭实验验证。

（完）
