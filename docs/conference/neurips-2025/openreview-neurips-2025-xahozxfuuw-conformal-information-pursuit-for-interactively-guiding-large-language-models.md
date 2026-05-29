---
title: Conformal Information Pursuit for Interactively Guiding Large Language Models
title_zh: 共形信息追求：交互式引导大语言模型
authors: "Kwan Ho Ryan Chan, Yuyan Ge, Edgar Dobriban, Hamed Hassani, Rene Vidal"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=xAHozxfuUW"
tags: ["query:ac"]
score: 4.0
evidence: 为大语言模型设计的主动学习查询策略
tldr: 针对指令微调大语言模型的交互式问答场景，提出共形信息追求策略。该策略在每一轮迭代中选择最大化信息增益的查询，以最小化预期查询次数。由于LLM概率估计不准确，直接应用传统信息论方法存在困难，本文引入共形预测框架进行校准，提高了查询效率。实验表明该策略在多个问答任务上显著减少所需查询轮次。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 567, \"height\": 577, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 569, \"height\": 501, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1443, \"height\": 649, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 583, \"height\": 838, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1437, \"height\": 756, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1432, \"height\": 533, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1418, \"height\": 587, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1417, \"height\": 587, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1423, \"height\": 1055, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1414, \"height\": 523, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1401, \"height\": 393, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1421, \"height\": 936, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-013.webp\", \"caption\": \"\", \"page\": 0, \"index\": 13, \"width\": 1298, \"height\": 525, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-xahozxfuuw/fig-014.webp\", \"caption\": \"\", \"page\": 0, \"index\": 14, \"width\": 645, \"height\": 461, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2025-xahozxfuuw/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 798, \"height\": 231, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-xahozxfuuw/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1442, \"height\": 401, \"label\": \"Table\"}]"
motivation: LLM在交互式问答中需要高效查询以节省用户时间。
method: 提出共形信息追求算法，结合共形预测校准LLM概率，贪婪选择最信息量的查询。
result: 在多个任务上显著减少查询次数，同时保持预测准确性。
conclusion: 共形预测有效提升了LLM交互式查询的效率和可靠性。
---

## Abstract
A significant use case of instruction-finetuned Large Language Models (LLMs) is to solve question-answering tasks interactively. In this setting, an LLM agent is tasked with making a prediction by sequentially querying relevant information from the user, as opposed to a single-turn conversation. This paper explores sequential querying strategies that aim to minimize the expected number of queries. One such strategy is Information Pursuit (IP), a greedy algorithm that at each iteration selects the query that maximizes information gain or equivalently minimizes uncertainty. However, obtaining accurate estimates of mutual information or conditional entropy for LLMs is very difficult in practice due to over- or under-confident LLM probabilities, which leads to suboptimal query selection and predictive performance. To better estimate the uncertainty at each iteration, we propose *Conformal Information Pursuit (C-IP)*, an alternative approach to sequential information gain based on conformal prediction sets. More specifically, C-IP leverages a relationship between prediction sets and conditional entropy at each iteration to estimate uncertainty based on the average size of conformal prediction sets. In contrast to conditional entropy, we find that conformal prediction sets are a distribution-free and robust method of measuring uncertainty. Experiments with 20 Questions show that C-IP obtains better predictive performance and shorter query-answer chains compared to previous approaches to IP and uncertainty-based chain-of-thought methods. Furthermore, extending to an interactive medical setting between a doctor and a patient on the MediQ dataset, C-IP achieves competitive performance with direct single-turn prediction while offering greater interpretability.

---

## 论文详细总结（自动生成）

# 中文总结：Conformal Information Pursuit for Interactively Guiding Large Language Models

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：大语言模型（LLM）在交互式问答场景中需要顺序地向用户询问相关信息以减少查询次数，但传统基于信息理论的 `Information Pursuit (IP)` 算法依赖准确的概率估计来计算条件熵或互信息，而 LLM 的输出概率往往严重失准（过度自信或缺乏自信），导致查询选择次优、预测性能下降。
- **整体含义**：本文旨在提出一种更为鲁棒的不确定性度量方法，以替代直接使用 LLM 概率的熵估计，从而在交互式问答中实现更高效的查询策略，减少用户负担并提升最终预测准确性。

## 2. 论文提出的方法论
- **核心思想**：利用共形预测（Conformal Prediction）构造的预测集（prediction sets）的平均大小来近似不确定性，代替直接计算条件熵。基于信息论中熵与预测集期望大小的上界关系（Proposition 3.1），将 IP 中的条件熵最小化转化为预测集大小的期望最小化。
- **关键技术细节**：
  - 采用分裂共形预测（Split Conformal Prediction）从校准数据中为每个查询长度 $k$ 设置阈值 $\hat{\tau}_k$，构造预测集 $C_{\hat{\tau}_k}(q_{1:k}(x)) = \{y \in \mathcal{Y} \mid f(q_{1:k}(x))_y > \hat{\tau}_k\}$。
  - **长度边际化**（length marginalization）：为了保证计算可行性，对查询历史长度进行边际化，而不是对每个具体历史实例单独校准，从而只需构造 $L$ 个预测集函数。
  - **历史采样方法**：
    - **均匀参数化**：当查询集为封闭集合时，在每个长度 $k$ 上从查询集中均匀采样 $k$ 个查询组成历史。
    - **LLM 模拟采样（Direct Prompting, DP）**：当查询集为开放集合时，利用 LLM 直接生成下一个查询，从而得到历史分布。
  - **查询选择规则**：在每一轮 $k$，选择使 $\log \mathbb{E}[|C_{\hat{\tau}_{k+1}}(q(X), q_{1:k}(x_{\text{obs}}))|]$ 最小的查询 $q$。
  - **停止准则**：当所有候选查询对应的预测集大小期望不再变化（方差小于阈值 $\varepsilon$）时停止查询。

## 3. 实验设计：数据集、场景、对比方法
- **数据集与场景**：
  - **20 Questions (20Q)**：使用 Animals with Attributes 2 (AwA2) 数据集中的 20 种动物，模拟两个 LLM（Querier 和 Answerer）之间的猜动物游戏。设置了封闭查询集 $Q_{\text{closed}}$（85 个预定义二元属性问题）和开放查询集 $Q_{\text{open}}$（LLM 自由生成问题），以及二元答案（Yes/No）和自由文本答案两种类型。
  - **Interactive Medical Question Answering (MediQ)**：基于 MedQA 医学考试数据集，模拟医生与患者之间的交互诊断。选用三个专科：Internal Medicine (IM, 290 样本)、Pediatrics (P, 217 样本)、Neurology (N, 108 样本)。LLM 作为 Expert（医生）和 Patient（患者），预测任务为多项选择题。
- **对比方法**：
  - 20Q：Random（随机选择）、IP（直接熵估计）、Uncertainty of Thought (UoT)、以及经过 Platt Scaling 或 Temperature Scaling 校准后的 IP。
  - MediQ：Non-sequential prediction（一次性给出所有信息作为上限）、Enumerate（按顺序逐个提供事实）、以及 IP。
- **模型**：Llama-3.1-8B、Qwen2.5-7B、Phi-3-small（以 Llama-3.1-8B 为主要结果，其他在附录）。

## 4. 资源与算力
- 论文中明确说明：实验使用 **8 块 NVIDIA A5000 GPU** 的工作站，每个 LLM 可在单块 A5000 上加载运行。但 **未提及具体的训练/推理时长**。

## 5. 实验数量与充分性
- **实验数量**：包含两个主要数据集（20Q 和 MediQ），20Q 上进行了多种设置（封闭/开放查询集、二元/自由文本答案），并对比了多个基线方法（Random、IP、UoT、概率校准方法）。MediQ 上在三个专科上评估。此外还进行了多项消融实验：不同 $\alpha$（目标覆盖）、不同估计样本数 $n_{\text{est}}$、不同采样查询数 $m$。
- **充分性与公平性**：实验较为全面，覆盖了不同查询集类型、不同答案格式、不同模型，且对超参数进行了探索。基准方法（如 UoT）按照原论文默认设置，并进行了对比。所有结果均报告多次运行的平均值和标准差（20Q 上 5 次，MediQ 上 3 折交叉验证）。但开放查询集设置下 C-IP 在 Qwen2.5-7B 和 Phi-3-small 上未能优于 DP，作者分析原因是校准不匹配，表明存在局限。

## 6. 论文的主要结论与发现
- C-IP 在 20Q 中相比 IP 和随机选择能更早达到更高准确率，且不确定性估计更符合直觉（逐渐下降），而 IP 的熵估计几乎不变。
- 在 MediQ 交互式诊断中，C-IP 达到了与一次性预测（Non-sequential）竞争力相当的性能，同时提供了可解释的问答链。
- 共形预测集的大小是一种分布自由且鲁棒的不确定性度量，适合处理 LLM 概率失准问题。
- 实验表明，当历史采样分布与测试时实际查询链分布一致时（如封闭查询集、Llama-3.1-8B），C-IP 效果最好；当两者存在分布漂移时（如开放查询集、Qwen2.5-7B），覆盖可能不足，性能下降。

## 7. 优点
- **理论连接**：利用 Proposition 3.1 建立了预测集大小与条件熵的上界关系，为使用共形预测进行信息增益提供了理论基础。
- **无需额外训练**：C-IP 仅需少量校准数据和 LLM 的前向推理即可运行，不改变模型参数。
- **鲁棒性**：相比于直接使用 LLM 概率的 IP，C-IP 对概率失准不太敏感，分布自由的共形预测提升了不确定性估计的质量。
- **可解释性**：在 MediQ 上生成的问答链和解释有助于临床理解。
- **实验设计全面**：覆盖多种设置、多个模型，并包含消融研究。

## 8. 不足与局限
- **校准依赖**：C-IP 需要使用独立的校准集，并在足够多的历史样本上估计预测集阈值。当校准集与测试分布不匹配（如开放查询集）时，覆盖保证可能不成立，导致性能下降。
- **未实现风险控制**：论文仅使用共形预测作为不确定性度量，未对最终预测的正确性提供风险控制保证。作者在 Remark 2 中承认这是未来工作。
- **开放查询集的覆盖问题**：在 Qwen2.5-7B 和 Phi-3-small 上，C-IP 在开放查询集设置中未能达到目标覆盖，导致性能不如直接提示（DP），表明对模型的鲁棒性有限。
- **假设查询集满足充分性**：方法依赖于查询集能够确定后验分布，但实际中可能不满足，影响效果。
- **计算开销**：每轮迭代需要为多个候选查询估计预测集大小，涉及多次 LLM 推理，在开放查询集下还需额外生成候选查询，计算量较大。
- **停止准则基于经验阈值**：使用标准差阈值 $\varepsilon$ 作为停止条件，缺乏理论指导。

（完）
