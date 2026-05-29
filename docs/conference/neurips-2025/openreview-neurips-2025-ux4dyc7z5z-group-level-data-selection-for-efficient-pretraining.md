---
title: Group-Level Data Selection for Efficient Pretraining
title_zh: 高效预训练的组级数据选择
authors: "Zichun Yu, Fei Peng, Jie Lei, Arnold Overwijk, Wen-tau Yih, Chenyan Xiong"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=uX4dyc7Z5Z"
tags: ["query:ac"]
score: 6.0
evidence: 利用聚类划分数据集进行大语言模型预训练数据选择，与使用大模型的聚类相关
tldr: 针对大语言模型预训练数据选择效率低的问题，论文提出了Group-MATES方法，通过关系数据影响模型参数化组级选择，将数据集划分为小簇，并利用轨迹采样近似数据影响。该方法使用聚类来优化预训练数据分布，在速度-质量前沿上取得了提升。虽然不涉及主动学习，但其聚类与LLM结合的思想可迁移至主动学习聚类场景。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-ux4dyc7z5z/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 774, \"height\": 402, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ux4dyc7z5z/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1380, \"height\": 407, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ux4dyc7z5z/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1448, \"height\": 384, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ux4dyc7z5z/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 668, \"height\": 367, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ux4dyc7z5z/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 682, \"height\": 369, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ux4dyc7z5z/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 695, \"height\": 362, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ux4dyc7z5z/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 853, \"height\": 415, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1450, \"height\": 675, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1446, \"height\": 245, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1435, \"height\": 459, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1294, \"height\": 272, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1439, \"height\": 718, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1449, \"height\": 515, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1447, \"height\": 254, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1438, \"height\": 279, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1445, \"height\": 217, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1443, \"height\": 221, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1433, \"height\": 794, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ux4dyc7z5z/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1430, \"height\": 792, \"label\": \"Table\"}]"
motivation: 预训练数据选择对语言模型效率和质量至关重要，但现有方法计算成本高。
method: 将数据集聚类成簇，并使用关系数据影响模型近似集群的贡献以指导选择。
result: 在多个预训练任务上提升了速度-质量权衡。
conclusion: 提出了一种基于聚类的数据选择方法，与主动学习结合有潜力。
---

## Abstract
The efficiency and quality of language model pretraining are largely determined by the way pretraining data are selected. In this paper, we introduce *Group-MATES*, an efficient group-level data selection approach to optimize the speed-quality frontier of language model pretraining. Specifically, Group-MATES parameterizes costly group-level selection with a relational data influence model. To train this model, we sample training trajectories of the language model and collect oracle data influences alongside. The relational data influence model approximates the oracle data influence by weighting individual influence with relationships among training data. To enable efficient selection with our relational data influence model, we partition the dataset into small clusters using relationship weights and select data within each cluster independently. Experiments on DCLM 400M-4x, 1B-1x, and 3B-1x show that Group-MATES achieves 3.5\%-9.4\% relative performance gains over random selection across 22 downstream tasks, nearly doubling the improvements achieved by state-of-the-art individual data selection baselines. Furthermore, Group-MATES reduces the number of tokens required to reach a certain downstream performance by up to 1.75x, substantially elevating the speed-quality frontier. Further analyses highlight the critical role of relationship weights in the relational data influence model and the effectiveness of our cluster-based inference. Our code is open-sourced at https://github.com/facebookresearch/Group-MATES.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大语言模型（LLM）预训练的数据选择对训练效率和质量至关重要。传统的个体数据选择方法（如影响函数、MATES等）假设每个数据点独立贡献，忽略了数据点之间的交互作用，导致组级（group-level）数据影响估计不准确。
- **关键挑战**：直接优化组级数据选择需要枚举指数级子集，计算上不可行。现有工作（如MATES）仅建模个体影响，无法捕捉数据间的协同或抵消效应。
- **核心问题**：如何在可接受的计算成本下实现高效的组级数据选择，以提升预训练速度-质量前沿（speed-quality frontier）。
- **整体意义**：通过建模数据间关系，将数据选择从个体范式提升到组级范式，显著提升预训练效率，为大规模预训练的数据治理提供新思路。

## 2. 论文提出的方法论

### 核心思想
- 用**关系数据影响模型**（Relational Data Influence Model）参数化昂贵的组级选择过程，该模型通过加权个体影响与数据间关系权重来近似真实组级影响。
- 训练时通过采样语言模型的训练轨迹（rollout）收集 oracle 数据影响作为监督信号。
- 推理时通过**影响感知聚类**（Influence-Aware Clustering）将数据集划分为小簇，在各簇内独立执行组级选择，大幅降低计算复杂度。

### 关键技术细节
1. **关系数据影响模型**（Eq. 13）：
   - 预测给定已选集合 \( D^{(t-1)} \) 时候选数据点 \( x_t \) 的组级影响：
     \[
     \Theta_{\text{rel}}(x_t \mid D^{(t-1)}) = \left[ \alpha - \frac{\alpha \beta}{t-1} \sum_{1 \le i < t} R_{x_i, x_t} \right] \cdot (w_o \cdot h_{x_t})
     \]
     - \( R_{x_i, x_t} = \text{sim}(h_{x_i}, h_{x_t}) \)：关系权重（余弦相似度）。
     - \( w_o \cdot h_{x_t} \)：个体影响预测。
     - \(\alpha, \beta\)：可训练缩放因子。
   - 关系权重捕捉数据点间的交互效应，理论上有轨迹特定影响函数连接。

2. **训练过程**（§4.2）：
   - 随机 rollout 策略 \( \pi_{\text{rand}} \)：采样训练轨迹，收集 oracle 个体影响。
   - 引导（bootstrap）rollout 策略 \( \pi_{\text{boot}} \)：关注影响分布的尾部（最低和最高预测值），提升模型对重要样本的拟合能力。
   - 联合两个策略的监督信号训练最终模型，最小化 MSE 损失。

3. **高效推理**（§4.3）：
   - 使用聚类减少计算：将数据集划分为 \( d \) 个簇，在各簇内独立执行贪心选择。
   - 每个簇的选择预算按比例分配：\( \lceil n \cdot |C_i|/|D| \rceil \)。
   - 复杂度从 \( O(N \cdot n) \) 降为 \( O(N \cdot n / d) \)。
   - 采用关系权重作为聚类相似度度量（影响感知聚类），保留关键关系。

## 3. 实验设计

### 数据集与场景
- **主要基准**：DataComp-LM (DCLM)，包含三个规模：
  - **400M-4x**：412M 参数模型，32.8B tokens。
  - **1B-1x**：1.4B 参数模型，28.0B tokens。
  - **3B-1x**：2.8B 参数模型，55.9B tokens。
- **评估**：22个下游任务，涵盖常识推理、语言理解、阅读理解、符号问题解决、世界知识（零样本或少量样本）。指标为中心准确率（Core score）。
- **额外实验**：在 C4 数据集上复现 MATES 设置（Table 8）；1B-3x 扩展设置（Table 7）。

### 对比方法（Baselines）
- Random（DCLM-Baseline）
- Edu Classifier（基于 LLama3-70B-Instruct 的教育质量评分）
- WebOrganizer（基于 LLM 的领域构建+RegMix 混合优化）
- MATES（个体数据影响模型）
- QUAD（簇级影响+多臂赌博机）
- 消融中对比：DSIR、SemDeDup、DsDm、QuRating（详见附录 C.4）。

## 4. 资源与算力

论文详细提供了计算分解（Table 5），摘要如下：

| 过程 | 400M-4x (H100小时) | 1B-1x (H100小时) | 3B-1x (H100小时) |
|------|-------------------|------------------|------------------|
| 模型预训练 | 104.0 (82.9%) | 240.0 (90.0%) | 740.0 (94.2%) |
| Oracle影响收集 | 4.5 (3.6%) | 12.1 (4.5%) | 18.7 (2.4%) |
| 影响模型训练 | 2.7 (2.2%) | 2.7 (1.0%) | 2.7 (0.4%) |
| 影响模型推理 | 14.2 (11.3%) | 12.0 (4.5%) | 23.9 (3.0%) |
| **总计** | **125.4** | **266.8** | **785.3** |

所有实验使用 8 张 GPU，未明确说明 GPU 具体型号（猜测为 H100，从“H100 Hours”推断），训练硬件环境在 Meta 内部，使用了 AdamW 优化器，余弦学习率。数据选择计算的额外开销占总 FLOPs 的 3.4%～12.2%。

## 5. 实验数量与充分性

- **主实验数量**：三个规模（400M-4x, 1B-1x, 3B-1x），每个规模对比5–6个基线，覆盖22个任务。
- **消融实验**（Table 2）：
  - 去除关系权重
  - 去除 bootstrap
  - 替换影响聚类为语义聚类
- **额外实验**：
  - 1B-3x 设置（Table 7）
  - 对比贪婪组级选择（Fig. 4）
  - 模型设计选择（BERT vs BGE, 相似度函数等，Fig. 7a）
  - 轨迹数量影响（Fig. 7b）
  - 选择比例（10%, 25%, 50%, Table 9）
  - Rollout长度（2,10,20, Table 10）
  - C4 数据集对比（Table 8）
  - 等量计算对比（Table 6）
- **公平性**：遵循 DCLM 标准化流程，控制训练超参数一致，对比基线为原始论文复现结果；代码开源可复现。实验设计规范，消融充分，但未报告多次运行的标准差（大型预训练通常如此）。整体实验数量丰富，覆盖面广。

## 6. 论文的主要结论与发现

1. **组级数据选择显著优于个体选择**：在 DCLM 三个规模上，Group-MATES 相对随机提升 3.5%–9.4% Core score，收益几乎是 MATES 的两倍（表1）。
2. **提升速度-质量前沿**：达到相同下游性能所需 tokens 减少最多 1.75 倍；H100 小时净效率增益在 400M-4x 和 1B-1x 上分别为 31.5% 和 20.5%（图3）。
3. **关系权重是核心**：消融去掉关系权重后性能下降超 30%，说明模型成功捕获了数据间交互。
4. **影响感知聚类有效**：聚类大幅降低推理时间（>10⁶倍），且保留关键关系。
5. **关系数据影响模型优于个体模型**：在近似贪婪组级选择时，本文模型在参考损失和 Core score 上均优于个体影响模型（图4）。
6. **引导采样提升模型训练**：bootstrap 策略使影响分布更分散，提升 Spearman 相关性 0.18（图5）。

## 7. 优点

- **方法创新性**：首次将关系建模引入预训练数据选择的组级影响，理论联系实际，填补了个体与组级之间的鸿沟。
- **计算高效**：通过聚类将组级选择复杂度从指数级降至实际可行，且额外开销仅占小部分总计算量。
- **实验扎实**：在三个模型规模、22个任务上验证，消融充分，对比多种 SOTA 基线（包括依赖更强外部 LLM 的方法）。
- **可解释性**：案例研究（表3）展示了关系权重能识别“抵消”和“放大”效应，为数据理解提供新工具。
- **开源代码**：便于复现和后续研究。
- **通用性**：在 DCLM 和 C4 上均有效，在更大模型（3B）上保持一致优势。

## 8. 不足与局限

- **模型规模限制**：实验最大为 2.8B 参数，尚未在 7B/13B 等更大模型上验证；大规模训练中稳定性等挑战未被讨论。
- **多次运行误差未报告**：由于预训练成本高昂，主实验未多次重复以提供置信区间，可能影响统计显著性判断。
- **聚类假设**：聚类依赖关系权重的相似度度量，假设跨簇关系可忽略；极端情况下可能丢失全局关键交互。
- **对初始模型敏感**：数据影响依赖当前模型状态，不同阶段需重新收集 oracle 影响，流程较复杂。
- **理论深度**：关系权重与轨迹特定影响函数的链接提供在附录中，但缺乏严格的理论保证（如收敛性、最优性）。
- **仅限预训练**：方法针对从头预训练，未探索微调或持续训练场景。

（完）
