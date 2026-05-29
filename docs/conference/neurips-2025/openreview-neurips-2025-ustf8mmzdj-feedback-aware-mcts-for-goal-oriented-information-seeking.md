---
title: Feedback-Aware MCTS for Goal-Oriented Information Seeking
title_zh: 基于反馈感知MCTS的目标导向信息寻求
authors: "Harshita Chopra, Chirag Shah"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ustF8MMZDJ"
tags: ["query:ac"]
score: 7.0
evidence: 基于LLM的问题生成，结合MCTS与聚类进行主动信息寻求
tldr: 针对对话系统中的目标导向信息寻求问题，提出结合大语言模型和蒙特卡洛树搜索的框架。LLM生成潜在问题，MCTS策略选择最大化信息增益的问题，并引入层次反馈机制，将每个新问题映射到基于语义的聚类中，利用历史交互模式指导未来决策。实验证明该方法在多个信息寻求基准上显著优于基线，有效减少了所需问题的数量。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-ustf8mmzdj/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1382, \"height\": 654, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ustf8mmzdj/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 568, \"height\": 462, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ustf8mmzdj/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1009, \"height\": 747, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1450, \"height\": 655, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1448, \"height\": 306, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 795, \"height\": 709, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1452, \"height\": 489, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 964, \"height\": 576, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 778, \"height\": 300, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 841, \"height\": 282, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 956, \"height\": 185, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1380, \"height\": 115, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1356, \"height\": 76, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1132, \"height\": 70, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1424, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1381, \"height\": 858, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 1472, \"height\": 632, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ustf8mmzdj/table-015.webp\", \"caption\": \"\", \"page\": 0, \"index\": 15, \"width\": 1447, \"height\": 415, \"label\": \"Table\"}]"
motivation: 在对话系统中高效地通过提问获取缺失信息，减少不确定性。
method: 利用LLM生成问题，MCTS选择问题，层次反馈聚类利用历史模式指导策略。
result: 在多种信息寻求任务中，所需问题数量大幅减少，性能提升。
conclusion: 反馈感知的聚类和MCTS组合能显著提升信息寻求的效率。
---

## Abstract
Effective decision-making and problem-solving in conversational systems require the ability to identify and acquire missing information through targeted questioning. A key challenge lies in efficiently narrowing down a large space of possible outcomes by posing questions that minimize uncertainty. To address this, we introduce a novel framework that leverages Large Language Models (LLMs) to generate information-seeking questions, with Monte Carlo Tree Search (MCTS) to strategically select questions that maximize information gain, as a part of inference-time planning. Our primary contribution includes a hierarchical feedback mechanism that exploits past interaction patterns to guide future strategy. Specifically, each new problem is mapped to a cluster based on semantic similarity, and our UCT (Upper Confidence bound for Trees) formulation employs a cluster-specific bonus reward to prioritize successful question trajectories that have proven effective for similar problems in the past. Extensive empirical evaluation across medical diagnosis and technical troubleshooting domains shows that our method achieves an average of 12\% improvement in success rates and about 10x reduction in the number of LLM calls made for planning per conversation, compared to the state of the art. An additional 8\% gain in success rate is observed on average when we start with a constrained set of possibilities. Our results underscore the efficacy of feedback-aware MCTS in enhancing information-seeking in goal-oriented dialogues.

---

## 论文详细总结（自动生成）

好的，作为资深学术论文分析助手，我将按照您的要求，对这篇论文进行结构化、深入、客观的中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）

*   **核心问题**：在目标导向的对话系统中，智能体（如AI医生、技术支持）需要通过与用户的互动，主动提问以获取缺失的关键信息，从而缩小可能的解决方案空间，并最终准确诊断问题或完成任务。核心挑战在于如何在庞大的可能性空间中，提出最具信息量、最高效的问题，最小化不确定性，同时降低交互成本。
*   **研究动机**：当前的大语言模型具备强大的自然语言能力，但在需要复杂推理和高效规划的对话场景中，其提问策略往往是预定义的或缺乏全局规划，导致效率低下（提问过多）或效果不佳（无法诊断出特定问题）。
*   **整体含义**：本文旨在提出一种新的框架，将大语言模型的问题生成能力与蒙特卡洛树搜索的规划能力相结合，并引入一种从历史交互中学习的反馈机制，从而使对话系统能够更聪明、更高效、更具适应性地进行信息寻求。这对于构建下一代具备真正推理和协作能力的AI助手至关重要。

### 2. 论文提出的方法论：核心思想、关键技术细节

*   **核心思想**：提出 **MISQ-HF** 框架，其核心是“规划-执行-学习”循环：
    1.  **规划 (Planning)**：利用 MCTS 在由 LLM 生成的问题所构成的决策树中进行搜索和规划，以找到当前状态下信息增益最大的最优问题。
    2.  **执行 (Execution)**：向用户（由 LLM 模拟）提出该问题，并根据回答更新当前的可能性集合。
    3.  **学习 (Learning)**：在成功解决问题后，通过一种**分层反馈机制**，将成功路径上的信息和奖励反向传播回决策树，并关联到特定“问题簇”，以便未来遇到类似问题时能更快找到有效策略。

*   **关键技术细节**：
    1.  **问题生成与决策树**：
        *   LLM 作为“问题生成器”，在每步根据当前可能性集合 `Ω_v` 和历史上下文，生成 `m` 个候选问题。
        *   每个问题将集合 `Ω_v` 二元分割为“是 (Ω_Av)”和“否 (Ω_Nv)”两个子集，从而构建一棵决策树。鼓励生成能使分割尽可能平衡（`|Ω_Av| ≈ |Ω_Nv|`）的问题，以最大化信息增益。
    2.  **信息增益与奖励函数**：
        *   使用**信息增益**作为问题的即时奖励 `R_IG(v)`。奖励函数的设计使得当问题对可能性集合的**分割最为平衡**时，其信息增益和奖励值达到最大。
        *   **期望奖励** `R_e(v)`：通过递归地计算子节点的信息增益得到，用于评估一个问题的长期价值。
    3.  **MCTS 进行问题选择**：
        *   在每个决策步，运行 K 次 MCTS 迭代来评估和选择最佳问题。
        *   **选择（Selection）**：使用 **UCT** 公式在树中导航，平衡探索（选访问次数少的节点）和利用（选累积奖励高的节点）。本文**创新性地修改了 UCT 公式**，加入了一个簇特定的奖励项，使得历史成功的轨迹更可能被选中：
        `UCT_feedback(v, k) = R_total(v) / N_v + C * sqrt(ln(N_p) / N_v) + B_k(v)`
        *   **扩展（Expansion）**：在选定的节点上，生成并添加一个或多个新的子问题节点。
        *   **模拟（Simulation）**：从当前节点开始，进行深度限制的随机模拟，以估计该节点的长期价值。
        *   **反向传播（Backpropagation）**：将模拟结果奖励值沿路径反向传播，更新各节点的累积奖励和访问次数。
    4.  **分层反馈机制 (Hierarchical Feedback)**：
        *   这是本文的核心贡献。系统会维护一个语义相似的“问题簇”数据库。
        *   **聚类**：当新问题到来时，将其问题描述进行文本嵌入，并与现有簇的“中心”（medoid）进行比对。若相似度超过阈值 `τ`，则加入该簇，否则创建新簇。
        *   **奖励调整**：当一次对话**成功**后，会更新 **簇特定的奖励** `B_k(v)`。沿着成功路径上的节点，根据其深度 `d_v` 获得一个指数衰减的奖励加成 (`β * R_total(v) * γ^`)。这意味着**越早期的、具有广泛指导性的成功问题，获得的奖励加成越高**, 从而能更好地指导未来相似问题的解决。

### 3. 实验设计：数据集、基准、对比方法

*   **数据集与场景**：覆盖三个领域，共 5 个数据集，均为对话目标导向型任务：
    *   **医疗诊断 (MD)**：
        *   **DX**: 104个医患对话，5种疾病。
        *   **MedDG**: 454个高质量样本，15种疾病。支持开放式回答（更挑战空间）。
    *   **技术故障排除 (TS)**：
        *   **FloDial**: 153个对话，153种故障类型。
    *   **20 Questions (通用信息寻求)**：
        *   **Common**：111个常见物品。
        *   **Things**：200个物品。
    *   **附加验证**：在开放集的 **Situation Puzzles** 基准上也进行了测试，增加了任务的挑战性和鲁棒性。

*   **基准 (Benchmark) 与对比方法**：
    *   **DP (Direct Prompting)**：直接让 LLM 生成下一个问题，无结构规划。
    *   **UoT (Uncertainty of Thoughts)**：当前最先进的方法，使用不确定性感知的奖励和树结构进行规划。本文认为是直接且强大的基线。
    *   **MISQ**：本文提出的 MISQ-HF 框架的去反馈版本（消融研究），用于单独验证反馈机制的效果。

*   **评估指标**：
    *   **SR (Success Rate)**：成功率，最关键指标。
    *   **MSC (Mean Conversational length in Successful cases)**：成功案例的平均对话轮次。
    *   **QGC (Question Generation Calls)**：**本文提出的新指标**，衡量规划过程中调用的 LLM 次数，更准确地反映计算效率。

### 4. 资源与算力

本文**未明确透露使用的具体GPU型号、数量和训练时长**。它提到使用了“8-core CPU with 16 GB RAM”来运行 MCTS 和相关实验。LLM（如 Llama 3.3 70B, Mixtral 8*7B, GPT-4o）是通过 AWS Bedrock 或 OpenAI API 调用的，因此算力消耗体现在 API 调用上。论文中重点衡量的效率指标 **QGC** 就是通过减少昂贵的 LLM API 调用来计算节约的成本。

### 5. 实验数量与充分性

*   **实验数量**：实验非常充分。
    *   **主要实验**：在三个主要领域（MD、TS、20 Questions）上进行了大量对比实验，并以表格形式呈现（Table 1, 3）。
    *   **消融实验**：通过对比 MISQ（无反馈）和 MISQ-HF（有反馈）来明确反馈机制的贡献（Table 1）。对比了“全集合初始化”和“约束集合初始化”两种策略（Table 2, Figure 2）。
    *   **开放集实验**：在开放集设定下进行了实验（Table 6），增加了对模型泛化能力的要求。
    *   **多模型验证**：使用了三种不同的 LLM（Llama 3.3 70B, Mixtral 8*7B, GPT-4o）作为提问者，验证了方法的通用性。
    *   **统计学检验**：在附录中（Table 5）报告了多次实验的平均值和标准差，并进行了 t 检验，证明了结果的统计学显著性。
*   **充分性与公平性**：
    *   实验覆盖了封闭集和开放集，涵盖了不同规模和复杂度的任务，**充分性很高**。
    *   公平性方面，与作者承认的 SOTA 方法（UoT）进行了直接对比，并且使用了相同的 LLM 作为回答者，控制变量合理。论文也提及未与 CoT 和 ToT 对比，因为先前已有工作证明 UoT 优于它们，这是合理的省略。

### 6. 论文的主要结论与发现

1.  **效率与性能双提升**：MISQ-HF 在所有数据集上，相比 SOTA 基线 UoT，平均**成功率提升 12%**，同时将用于规划的 **LLM 调用次数减少了约 10 倍**。这证明了其在性能和计算效率上的双重优势。
2.  **反馈机制的有效性**：通过消融实验（MISQ vs. MISQ-HF）和不同 LLM 的对比，证实了**分层反馈机制是性能提升的关键驱动力**。它能利用历史经验指导未来，尤其是在重复出现的场景（如医疗诊断、故障排查）中效果显著。
3.  **信息增益最大化被有效实现**：LLM 可以生成近乎最优的平衡分割问题（平均分割比率 52.82%），这为 MCTS 和信息增益计算的结合提供了坚实基础。
4.  **方法的可扩展性**：在 20 Questions 等更大搜索空间的任务（Common, Things）上，MISQ-HF（无反馈）依然表现出色，证明了其方法具有良好的可扩展性。

### 7. 优点

1.  **方法论创新**：将 MCTS、LLM 和基于语义聚类的层次反馈机制巧妙结合，形成了一个完整且逻辑自洽的框架。这种“规划+执行+学习”的思路非常清晰。
2.  **明确的效率指标**：提出 **QGC** 作为评估效率的核心指标，比传统的运行时间更稳定、可复现，并直接反映了昂贵 LLM 调用成本的关键痛点。
3.  **广泛的实验验证**：在多个领域、多个数据集、多个 LLM 上进行了充分且系统的实验，并有消融实验和统计显著性测试，结果的说服力很强。
4.  **开源贡献**：提供了公开的代码，有助于社区进行复现和进一步研究。

### 8. 不足与局限

1.  **未处理失败案例**：当前系统只从**成功**的交互中学习（通过正向奖励）。论文指出其“没有纳入从失败中学习的机制”，这意味着系统无法主动修正策略来避免重蹈覆辙。
2.  **问题质量评估**：文中的信息增益奖励是**基于对可能性集合的平衡分割程度 **计算的，这假设问题总是“合理”的。但 LLM 生成的问题可能存在冗余、低效或违反对话规范的情况，系统没有设计机制来惩罚这类“次优”但信息增益仍高的问题。
3.  **实验覆盖的局限性**：
    *   **模拟环境**：用户（Answerer）完全由 LLM 模拟，虽然这是常见的评估方式，但其与真实人类的交互模式、答案的模糊性和不可预测性可能存在差距。
    *   **算力报告模糊**：未详细报告实际运行时的 GPU 等硬件资源开销，使得“10倍效率提升”这个结论对于实际落地部署的绝对成本评估不够具体。
4.  **理论与应用的限制**：论文未探讨在更复杂、非二元（如多选、开放式文本）问答场景下的适用性。此外，没有深入讨论如何设计奖励函数以兼顾交互的自然性和用户信任感。
5.  **伦理与偏见**：虽然论文提到了广泛应用的伦理问题，但未具体讨论本框架中基于过去成功路径进行学习而可能引入的系统性偏差（例如，模型可能过度依赖数据集中常见的模式，而忽略少数边缘情况，尤其是在医疗诊断这类高风险场景中）。

（完）
