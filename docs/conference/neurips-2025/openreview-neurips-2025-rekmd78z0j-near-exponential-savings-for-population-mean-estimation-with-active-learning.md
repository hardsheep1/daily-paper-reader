---
title: Near-Exponential Savings for Population Mean Estimation with Active Learning
title_zh: 主动学习实现近指数级节省的总体均值估计
authors: "Julian Morimoto, JACOB GOLDIN, Daniel E. Ho"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=REKmD78Z0J"
tags: ["query:ac"]
score: 5.0
evidence: 利用基于分区的聚类进行主动学习
tldr: 研究在有限标签下估计k类随机变量均值的问题，提出PartiBandits主动学习算法。算法第一阶段学习未标注数据的划分（即聚类），第二阶段利用该划分指导标签查询。理论证明该算法可以达到近指数级的标签节省，其均方误差以指数速率衰减。该工作展示了主动学习结合聚类在统计估计中的巨大优势。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-rekmd78z0j/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1437, \"height\": 570, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-rekmd78z0j/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1289, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-rekmd78z0j/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1451, \"height\": 570, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-rekmd78z0j/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1449, \"height\": 570, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-rekmd78z0j/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1448, \"height\": 572, \"label\": \"Figure\"}]"
motivation: 在少量标签下高效估计总体均值，利用辅助信息。
method: 提出两阶段主动学习算法PartiBandits，先学习数据划分，再基于划分进行标签采样。
result: 理论上证明了均方误差随标签数指数级下降，远优于传统方法。
conclusion: 主动学习结合聚类能大幅提升均值估计的标签效率。
---

## Abstract
We study the problem of efficiently estimating the mean of a $k$-class random variable, $Y$, using a limited number of labels, $N$, in settings where the analyst has access to auxiliary information (i.e.: covariates) $X$ that may be informative about $Y$. We propose an active learning algorithm ("PartiBandits") to estimate $\mathbb{E}[Y]$. The algorithm  yields an estimate, $\widehat{\mu}_{\text{PB}}$, such that $\left( \widehat{\mu}_{\text{PB}} - \mathbb{E}[Y]\right)^2$ is $\tilde{\mathcal{O}}\left( \frac{\nu + \exp(c \cdot (-N/\log(N))) }{N} \right)$, where $c > 0$ is a constant and $\nu$ is the risk of the Bayes-optimal classifier. PartiBandits is essentially a two-stage algorithm. In the first stage, it learns a partition of the unlabeled data that shrinks the average conditional variance of $Y$. In the second stage it uses a UCB-style subroutine ("WarmStart-UCB") to request labels from each stratum round-by-round. Both the main algorithm's and the subroutine's convergence rates are minimax optimal in classical settings. PartiBandits bridges the UCB and disagreement-based approaches to active learning despite these two approaches being designed to tackle very different tasks. We illustrate our methods through simulation using nationwide electronic health records. Our methods can be implemented using the \textbf{PartiBandits} package in R.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：在统计学和机器学习中，经常需要在有限标签预算下估计一个k类随机变量Y的总体均值。传统方法如简单随机抽样（SRS）在拥有辅助信息X（协变量）时可能效率低下。虽然分层随机抽样可以改善效率，但最优分层方案未知，且方差估计需要自适应学习。
- **核心问题**：如何在有限标签预算N下，利用未标注的辅助信息X，自适应地选择样本进行标注，从而高效、准确地估计E[Y]。
- **整体含义**：该论文提出了一种新的主动学习算法PartiBandits，能够实现均方误差随标签数N以接近指数级速度下降（即“近指数节省”），远优于经典SRS的1/N速率。该工作桥接了UCB主动学习与基于分歧的主动学习两种范式，为主动学习在参数估计中的应用提供了理论保证和实用方法。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：
  - 两阶段算法：第一阶段利用分歧式主动学习算法（如A2）学习一个数据划分（即聚类），使得每个子类内Y的方差尽可能小；第二阶段对学到的划分使用UCB风格的自适应采样策略（WarmStart-UCB）从每个子类中请求标签，并加权聚合得到均值估计。
  - 关键洞察：通过快速降低平均条件方差Σ₁(G)，实现比SRS更快的收敛速率，且若X对Y预测力强（ν小），可达到近指数级加速。

- **关键技术细节**：
  - **假设**：存在一个主动学习算法S，能以指数速率学习分类器，使得过剩风险为O(exp(c·(-N/log N)))（Assumption 1）。
  - **第一阶段（Stage 1）**：运行S（如A2算法），用一半标签预算（⌊N/2⌋）学习分类器ĥ，然后根据ĥ的逆映射定义划分G={Aᵢ}。
  - **第二阶段（Stage 2）**：用剩余标签预算运行WarmStart-UCB子程序，该子程序在经典Variance-UCB基础上加入warm-start步骤（分配τ比例预算均匀采样各组），之后每轮选择UCB值最大的组进行采样。
  - **主定理**：\(\left( \widehat{\mu}_{\text{PB}} - \mathbb{E}[Y] \right)^2 = \tilde{\mathcal{O}}\left( \frac{\nu + \exp(c \cdot (-N/\log N))}{N} \right)\)，其中ν是贝叶斯最优分类器风险。
  - **子程序定理**：若给定先验划分G，WarmStart-UCB的误差为\(\tilde{\mathcal{O}}\left( \frac{\Sigma_1(G)}{N} \right)\)，且均为极小极大最优。

- **算法流程（文字描述）**：
  1. 输入：假设类C、主动学习算法S、标签预算N、置信度δ、缓冲分数τ。
  2. 第一阶段：运行S（例如A2）使用⌊N/2⌋个标签得到分类器ĥ；构造划分G = {ĥ⁻¹(i)}。
  3. 第二阶段：运行WarmStart-UCB使用剩余标签（N - ⌊N/2⌋）、划分G和缓冲分数τ，得到各子类均值估计并加权求和输出最终估计\(\widehat{\mu}_{\text{PB}}\)。

## 3. 实验设计：使用了哪些数据集/场景，基准是什么，对比了哪些方法

- **实验场景**：
  1. **模拟数据（Theorem 1和3）**：X~Unif[0,1]，Y = 1{X≥0.5}，部分标签随机翻转模拟噪声。ν表示翻转比例。考察不同ν下PartiBandits与SRS的比较。
  2. **电子健康记录（AFC数据集）**：来自美国全科医学委员会的PRIME Registry，包含超过600万患者的纵向数据。定义Y = H×B（H为高血压，B为黑人），X为基于邮政编码的黑人概率。仅关注X位于顶部和底部5%分位数的患者，估计该子总体均值。
  3. **额外模拟**（附录中）：逻辑斯蒂DGP、不对称Probit DGP、与Thompson采样比较等。

- **基准方法**：简单随机抽样（SRS）；部分实验还对比了Thompson采样、分层随机抽样（StRS）及Variance-UCB。

- **对比方法**：
  - 主要对比：PartiBandits vs. SRS。
  - 子程序实验：WarmStart-UCB vs. SRS（在不同先验划分G下）。
  - 附录中：PartiBandits vs. SRS vs. Thompson采样。

- **评价指标**：90%置信水平下的估计误差（即绝对误差的90%分位数）。

## 4. 资源与算力

- **文中未明确说明**所用GPU型号、数量或训练时长。实验主要基于蒙特卡洛模拟，计算负担较小，使用R语言实现（PartiBandits包）。作者也未报告大规模分布式训练等算力需求。
- **合理推断**：模拟实验在普通CPU上即可完成，无需GPU。AFC数据实验可能涉及最多600万患者，但只随机抽取子集，计算量可控。

## 5. 实验数量与充分性

- **模拟实验**：针对Figure 1左右两个面板，各使用500次随机重复（500 hypothetical datasets），对不同标签预算（10~100和50~200）及不同ν值进行测试。
- **额外模拟**（附录）：包括逻辑斯蒂DGP、Probit DGP、与Thompson采样比较，也各使用了500次重复。
- **AFC数据实验**：对每个标签预算，抽取500个随机子集（每个子集10,000名患者），计算90%分位数误差。
- **充分性**：实验覆盖了多种数据生成过程（阈值规则、逻辑回归、Probit、真实数据），且包含不同噪声水平、不同预定义划分质量。对比方法合理（SRS为自然基准，Thompson采样为常见自适应方法）。实验设计较为充分、公平，重现了理论预测的趋势。
- **潜在不足**：未在更多真实世界数据集上验证；未测试高维X或大规模类别数k>2的情况（虽有理论保证）。

## 6. 论文的主要结论与发现

- PartiBandits算法在估计总体均值时，均方误差收敛速率可达到\(\tilde{\mathcal{O}}\left( \frac{\nu + \exp(c \cdot (-N/\log N))}{N} \right)\)，在ν小（X高度预测Y）时接近指数级快于SRS。
- WarmStart-UCB子程序在给定先验划分时，误差为\(\tilde{\mathcal{O}}(\Sigma_1(G)/N)\)，且极小极大最优；其依赖于平均条件方差而非最小方差，解决了已有工作中的开放问题。
- 两个速率均达到极小极大最优（Theorem 2和4）。
- 模拟和真实数据实验验证了理论预测：PartiBandits在标签预算足够时优于SRS，且X预测力越强增益越大。
- 该工作桥接了UCB主动学习与基于分歧的主动学习，为均值估计提供了新范式。

## 7. 优点

- **理论贡献**：首次将分歧式主动学习的指数节省思想应用于总体均值估计，并证明其最优性。
- **算法设计**：两阶段框架清晰，第一阶段学习划分降低方差，第二阶段自适应采样；引入warm-start解决了方差估计不稳定问题。
- **桥接意义**：连接了UCB类和分歧类主动学习，为后续研究提供新方向。
- **实证验证**：通过合成数据和真实电子健康记录数据证明了有效性，且结果稳健。
- **可复现性**：提供R包PartiBandits，代码开源。

## 8. 不足与局限

- **实验覆盖有限**：仅在一个真实数据集（AFC）上验证，且限定为两分位子总体（顶部和底部5%）。未在更广泛的数据集（如图像、文本）或高维协变量上测试。
- **假设较强**：Assumption 1要求存在指数速率主动学习算法，这在某些噪声条件下可能不成立（尽管论文通过多个推论给出了放松条件）。
- **常数依赖性**：速率中的常数c依赖于假设类VC维和分歧系数，实际中难以计算。
- **标签预算分配**：第一阶段消耗一半标签，可能导致小预算下第一阶段不充分。论文未讨论自适应分配比例。
- **k-class一般化**：实验仅涉及二元Y（k=1），未实证验证多类别情况（k>2）。
- **偏差风险**：若第一阶段学习到的划分与真实最优划分偏差大，可能影响第二阶段效率，论文未充分讨论鲁棒性。
- **计算资源未报告**：缺少详细算力信息，虽对结果影响不大，但可复现性稍弱。

（完）
