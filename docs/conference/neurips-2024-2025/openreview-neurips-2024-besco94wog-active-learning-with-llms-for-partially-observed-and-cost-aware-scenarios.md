---
title: Active Learning with LLMs for Partially Observed and Cost-Aware Scenarios
title_zh: 部分观测与成本感知场景下的大模型主动学习
authors: "Nicolás Astorga, Tennison Liu, Nabeel Seedat, Mihaela van der Schaar"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=bescO94wog"
tags: ["query:ac"]
score: 4.0
evidence: 大模型主动学习方法
tldr: 该论文针对部分观测数据和标签获取成本高昂的现实场景，提出一种利用大语言模型（LLM）的主动学习方法，通过策略性选择需要观测的特征和标签来最大化预测性能同时最小化成本。实验表明该方法在部分观测设置下优于传统数据获取策略，为主动学习与大模型结合提供了新思路。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 736, \"height\": 367, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 676, \"height\": 486, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1457, \"height\": 551, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 674, \"height\": 163, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 691, \"height\": 210, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1398, \"height\": 358, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 430, \"height\": 333, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 850, \"height\": 396, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1294, \"height\": 417, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1293, \"height\": 411, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 857, \"height\": 732, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1219, \"height\": 364, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1233, \"height\": 1327, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 1227, \"height\": 1030, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 375, \"height\": 237, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-016.webp\", \"caption\": \"\", \"page\": 0, \"index\": 16, \"width\": 882, \"height\": 243, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-besco94wog/fig-017.webp\", \"caption\": \"\", \"page\": 0, \"index\": 17, \"width\": 1417, \"height\": 309, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2024-besco94wog/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 765, \"height\": 171, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-besco94wog/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 643, \"height\": 466, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-besco94wog/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1454, \"height\": 501, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-besco94wog/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1437, \"height\": 1367, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-besco94wog/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1144, \"height\": 259, \"label\": \"Table\"}]"
motivation: 现有主动学习方法假设数据完全观测，忽略了现实中的部分特征可观测性和获取成本问题。
method: 提出Partially Observed Cost-Aware Active Learning框架，利用LLM的推理能力指导特征和标签的获取决策。
result: 在多个基准数据集上，该方法在部分观测条件下显著降低了获取成本并保持了模型性能。
conclusion: 将LLM融入主动学习可以有效应对部分观测和成本约束，拓展了主动学习的应用范围。
---

## Abstract
Conducting experiments and gathering data for machine learning models is a complex and expensive endeavor, particularly when confronted with limited information. Typically, extensive _experiments_ to obtain features and labels come with a significant acquisition cost, making it impractical to carry out all of them. Therefore, it becomes crucial to strategically determine what to acquire to maximize the predictive performance while minimizing costs. To perform this task, existing data acquisition methods assume the availability of an initial dataset that is both fully-observed and labeled, crucially overlooking the **partial observability** of features characteristic of many real-world scenarios. In response to this challenge, we present Partially Observable Cost-Aware Active-Learning (POCA), a new learning approach aimed at improving model generalization in data-scarce and data-costly scenarios through label and/or feature acquisition. Introducing $\mu$POCA as an instantiation, we maximise the uncertainty reduction in the predictive model when obtaining labels and features, considering associated costs. $\mu$POCA enhance traditional Active Learning metrics based solely on the observed features by generating the unobserved features through Generative Surrogate Models, particularly Large Language Models (LLMs). We empirically validate $\mu$POCA across diverse tabular datasets, varying data availability, acquisition costs, and LLMs.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现实世界中，训练数据的特征往往**部分可观测**（某些样本缺少部分特征），且获取特征和标签的成本高昂。传统主动学习（Active Learning, AL）假设数据完全观测（特征已知、仅标签未知），忽略了部分观测性和获取成本，导致在数据稀缺和成本限制场景下性能不佳。
- **研究动机**：在客户流失预测、医疗诊断、金融风控等应用中，初始数据通常不完整，进一步获取额外信息和标签需要花费金钱、时间或风险。因此，需要一种策略性地决定**获取哪些实例的哪些特征和标签**，以最大化模型泛化能力并最小化成本。
- **整体含义**：论文提出了**Partially Observable Cost-Aware Active-Learning (POCA)** 这一新范式，旨在解决部分观测和成本感知下的数据获取问题，并提供了基于贝叶斯不确定性的具体实现 $\mu$POCA。

#### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将传统主动学习的不确定性度量扩展到部分观测场景。通过**生成式替代模型（Generative Surrogate Model, GSM）** 对缺失特征进行采样，从而更准确地估计不确定性减少量，并考虑特征和标签的获取成本。
- **关键技术**：
  - **POCA框架**：形式化为优化问题：$(i, j)^* = \arg\max_{i, j} U_t(i, j), \text{s.t. } r_t(i, j)$，其中 $U_t$ 是效用函数（如不确定性减少与成本的权衡），$r_t$ 是预算约束。
  - **$\mu$POCA实例化**：采用贝叶斯方法，将效用函数设定为不确定性减少 $\hat{\mu}_{i,j}$，约束为获取成本小于预算 $c$。
  - **部分观测下的不确定性度量**：
    - **PO-EIG**（Partially Observable Expected Information Gain）：$E_{x_j} [I(\Omega, Y | x_j, x_o, D)]$，即对缺失特征 $x_j$ 求期望的BALD度量。
    - **PO-EPIG**：类似地扩展EPIG。
    - 理论上证明（Proposition 1 & Corollary 1）：在独立性假设下，PO-EIG ≥ 传统EIG，即获取特征和标签比仅获取标签带来更多不确定性减少。
  - **LLM作为GSM**：使用LLM（如Mistral-7B-Instruct）通过监督微调（SFT）学习从观测特征生成缺失特征。训练时随机掩码，使LLM能够根据任意观测组合进行生成。
  - **算法流程**（Algorithm 2）：
    1. 使用无标注数据训练GSM。
    2. 对池集中每个实例，生成S个蒙特卡洛样本。
    3. 对于每个实例，通过贪婪排除法（Algorithm 1）在预算内选择最优特征子集 $j^*$（计算 PO-EPIG/PO-EIG）。
    4. 选择效用最大的实例 $(i^*, j^*)$ 进行实际获取（标签和选定特征）。
    5. 更新训练集，重复直到预算用完。

#### 3. 实验设计：使用了哪些数据集/场景，基准是什么，对比了哪些方法
- **数据集**：5个表格数据集（Magic、Adult、Housing、Cardio、Banking），具有不同样本量、特征类型（数值/类别）和任务（二分类）。
- **实验场景**：
  - **Scenario 1**：假设存在一个完全观测的历史无标注数据集用于训练GSM；池集中每个实例预先指定一半特征（高相关性）为未观测，另一半为观测。此场景为了公平比较传统AL（因为传统AL需要固定输入维度）。
  - **Scenario 2**：更现实——池集本身部分观测（某些特征随机缺失），GSM也仅能在部分观测数据上训练。
- **基准方法**：
  - **Random**：随机选择实例。
  - **Vanilla AL metrics**：传统BALD/EPIG（使用观测到的特征直接计算，缺失特征用确定性插补或忽略）。
  - **Oracle**：使用所有特征（包括未观测的真实值）计算BALD/EPIG（理想上界）。
  - **$\mu$POCA metrics**：PO-EIG、PO-EPIG。
- **对比结果**：在5个数据集上，$\mu$POCA在大部分情况下优于Vanilla AL，接近Oracle；在成本约束实验中，展示了选择性特征获取的优势。

#### 4. 资源与算力
- 论文在附录F.1中说明：GSM使用Mistral7B-Instruct-v0.3，通过QLoRA 4-bit量化微调。
  - **训练时间**：约1–2小时。
  - **测试生成时间**：根据数据集不同，约3–8小时。
  - **GPU型号**：未明确说明，但推测使用了单张/多张高显存GPU（如A100 80GB）。
- 总体来看，计算代价适中，但对于更大模型或更大数据集可能需要更多资源。

#### 5. 实验数量与充分性
- **实验数量**：
  - 主实验：5个数据集 × 2种指标（EIG和EPIG）× 3种方法（Vanilla, $\mu$POCA, Oracle）+ Random，共约5×2×4=40种设置，每种设置重复60个随机种子，报告95%置信区间。
  - 合成实验（Section 4.2）：验证理论（不同特征-标签相关性下的表现）。
  - 成本感知实验（Section 4.3）：在Magic数据集上比较不同特征获取比例（20%, 60%, 100%）和不同成本分配。
  - 消融实验（Appendix J）：更换不同LLM（Llama 3.1, Gemma 2）作为GSM，分析GSM质量影响。
  - 此外还有Scenario 2结果（Appendix I）。
- **充分性评价**：实验覆盖了多个数据集、不同缺失模式、多种不确定度量和成本约束，重复次数多（60 seeds），统计显著。但局限在于**仅限表格数据**，未在图像、文本等其他模态上验证；场景设置可能偏向于表格域。

#### 6. 论文的主要结论与发现
1. **部分观测下传统AL失效**：当只使用观测特征计算不确定性时，性能甚至不如随机选择（如图3中的Magic数据集）。
2. **$\mu$POCA的部分观测度量更优**：PO-EIG和PO-EPIG在大部分数据集上显著优于或持平于其全观测对应物（Vanilla AL），且理论上不确定性减少更大。
3. **GSM质量是关键**：LLM的生成能力直接影响$\mu$POCA性能；更好的LLM（如Mistral）带来更大提升。
4. **成本有效性**：在预算约束下，选择性获取部分特征（而非所有缺失特征）可以在相同预算下获取更多实例，从而获得更好的整体性能（图6）。
5. **插补不能替代获取**：即使使用LLM插补，直接获取实际特征仍比单纯插补性能更优（图7），尤其是当预算允许时。

#### 7. 优点：方法或实验设计上的亮点
- **问题新颖**：首次形式化部分观测且成本感知的主动学习问题（POCA），填补了现有AL的空白。
- **理论支撑**：严格证明了PO-metrics的不确定性减少大于传统metrics（Proposition 1, Corollary 1），并给出独立性条件。
- **灵活利用LLM**：将LLM作为GSM，利用其生成能力和任意条件训练，解决数据稀疏和混合类型特征问题。
- **实验设计细致**：设置了公平的比较场景（Scenario 1），考虑了多种成本分配、不同特征获取比例，并进行了丰富的消融实验和合成验证。
- **开源代码**：承诺开源（链接已提供），便于复现。

#### 8. 不足与局限
- **实验领域局限**：仅评估了表格数据集，未涉及图像、文本、时间序列等常见模态，结论的泛化性有限。
- **计算开销**：使用7B参数的LLM进行大量MC采样，在特征数多的数据集上计算成本高（O(J²)），实际部署可能需要优化。
- **LLM偏差**：论文未讨论GSM可能引入的偏差（如社会偏见或生成分布偏移），可能影响获取决策。
- **GSM质量依赖**：当观测特征与缺失特征相关性低时，$\mu$POCA提升有限（如合成实验图15）。
- **特征获取约束**：未考虑“哪些特征可以同时获取”的实际限制（如某些特征必须一起获取或互斥），仅假设独立获取。

（完）
