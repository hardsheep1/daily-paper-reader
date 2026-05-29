---
title: "SeCon-RAG: A Two-Stage Semantic  Filtering and Conflict-Free Framework for Trustworthy RAG"
title_zh: SeCon-RAG：面向可信RAG的两阶段语义过滤与无冲突框架
authors: "Xiaonan si, Meilin Zhu, Simeng Qin, Lijia Yu, Lijun Zhang, Shuaitong Liu, Xinfeng Li, Ranjie Duan, Yang Liu, Xiaojun Jia"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=tTwZhy8JqY"
tags: ["query:ac"]
score: 5.0
evidence: 在RAG中使用基于聚类的过滤与LLM
tldr: 针对检索增强生成系统的安全漏洞，提出SeCon-RAG两阶段语义过滤与无冲突框架。第一阶段联合使用语义和基于聚类的过滤，由实体-意图-关系抽取器指导，有效去除恶意污染文档。该方法在保障生成可靠性的同时，避免了过度过滤导致的信息损失，提升了RAG系统的鲁棒性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-ttwzhy8jqy/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1436, \"height\": 700, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ttwzhy8jqy/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1417, \"height\": 469, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ttwzhy8jqy/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1282, \"height\": 314, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ttwzhy8jqy/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1125, \"height\": 472, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ttwzhy8jqy/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1066, \"height\": 903, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ttwzhy8jqy/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1158, \"height\": 916, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-ttwzhy8jqy/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1101, \"height\": 818, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1448, \"height\": 624, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1453, \"height\": 157, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1445, \"height\": 832, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1444, \"height\": 830, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1446, \"height\": 835, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1444, \"height\": 179, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1444, \"height\": 156, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1438, \"height\": 157, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1443, \"height\": 204, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2025-ttwzhy8jqy/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1442, \"height\": 200, \"label\": \"Table\"}]"
motivation: 现有RAG防御方法过度过滤导致信息损失，影响生成可靠性。
method: 提出两阶段框架，第一阶段联合语义与聚类过滤，第二阶段进行无冲突处理。
result: 在多个数据集上验证了方法能够有效防御污染攻击，同时保留有用信息。
conclusion: 聚类过滤有效提升了RAG系统的安全性，且不牺牲信息完整性。
---

## Abstract
Retrieval-augmented generation (RAG) systems enhance large language models (LLMs) with external knowledge but are vulnerable to corpus poisoning and contamination attacks, which can compromise output integrity. Existing defenses often apply aggressive filtering, leading to unnecessary loss of valuable information and reduced reliability in generation.
To address this problem, we propose a two-stage semantic filtering and conflict-free framework for trustworthy RAG. 
In the first stage, we perform a joint filter with semantic and cluster-based filtering  which is guided by the Entity-intent-relation extractor (EIRE). EIRE extracts entities, latent objectives, and entity relations from both the user query and filtered documents, scores their semantic relevance, and selectively adds valuable documents into the clean retrieval database. 
In the second stage, we proposed an EIRE-guided conflict-aware filtering module, which analyzes semantic consistency between the query, candidate answers, and retrieved knowledge before final answer generation, filtering out internal and external contradictions that could mislead the model.
Through this two-stage process, SeCon-RAG effectively preserves useful knowledge while mitigating conflict contamination, achieving significant improvements in both generation robustness and output trustworthiness.
Extensive experiments across various LLMs and datasets demonstrate that the proposed SeCon-RAG markedly outperforms state-of-the-art defense methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
检索增强生成（RAG）系统通过引入外部知识库提升大语言模型的事实准确性，但面临**语料库投毒攻击**和**污染攻击**的严重威胁。现有防御方法（如TrustRAG、AstuteRAG）多采用**粗粒度过滤**（如聚类移除异常文档）或**多数投票机制**，导致两个主要局限：
- 过度过滤同时移除有害和有用内容，造成信息损失；
- 未解决检索内容与模型内部知识之间的冲突，输出可信度低。

为此，论文提出 **SeCon-RAG**，一个**两阶段语义过滤与无冲突框架**，旨在保留有用知识的同时有效消除污染与冲突，提升生成鲁棒性和可信度。

## 2. 方法论

### 核心思想
- 设计 **EIRE（Entity-Intent-Relation Extractor）** 模块，从文档中提取**结构化语义信息**（实体、意图、实体间关系），为后续过滤提供细粒度语义基础。
- 第一阶段：**语义与聚类联合过滤（SCF）**，在检索前从统计和语义两个维度去除污染文档。
- 第二阶段：**冲突感知过滤（CAF）**，在生成前检查检索文档与查询、其他文档、模型内部知识的一致性，移除矛盾/误导信息。

### 关键技术细节
- **EIRE**：基于提示的LLM，为每个文档生成三元组 (Ed, Id, Rd)，即实体集、意图、关系集。
- **SCF**：
  - **聚类过滤**：对文档嵌入进行K-means聚类，移除与聚类中心余弦相似度超过阈值 τcluster 的文档（攻击文档常形成紧密簇）。
  - **语义图过滤**：利用EIRE为每个文档构建语义图（节点为实体，边为关系），与少量已验证干净文档的语义图进行比较，LLM输出相似度分数 ssG，过滤低分文档（ssG < τsemantic）。
  - **联合决策**：使用 **AND 逻辑**，被两个过滤器同时标记的文档才被移除，即 Dfinal = Dcluster ∩ Dsemantic，保留剩余文档。
- **CAF**：
  - 对检索后的 top-k 文档，再次利用EIRE提取语义结构。
  - LLM从三个维度评估每个文档：**查询一致性（Q）**、**语料一致性（C）**、**模型一致性（M）**。
  - 仅保留被判定为“可信”的文档，用于最终生成。

### 算法流程（文字说明）
1. 输入查询 q、检索语料库 D、已验证干净文档集合 Dcor、生成模型 F。
2. **第一阶段 SCF**：
   - 嵌入所有文档，进行K-means聚类并计算每个文档与所属簇中心的相似度，标记超过阈值的文档。
   - 对每个文档调用EIRE提取语义结构，并与 Dcor 的语义图比较，获得语义相似度分数，标记分数低于阈值的文档。
   - 取两个标记集合的交集作为待移除文档，得到净化后语料库 D'。
3. **第二阶段 CAF**：
   - 从 D' 中检索 top-k 文档 Dk(q)。
   - 对每个文档调用EIRE，并由LLM评估三个一致性维度；仅保留三项均可信的文档形成 DCAF。
4. 生成最终答案 A(q) = F(q, DCAF)。

## 3. 实验设计

### 数据集与场景
- **QA 基准**：Natural Questions（NQ）、HotpotQA、MS-MARCO。
- **攻击设置**：
  - 语料投毒攻击（PoisonedRAG）：分别设置 20%、40%、60%、80%、100% 的投毒比率。
  - 提示注入攻击（PIA）：通过离散令牌扰动创建对抗性提示。
- **评估指标**：准确率（ACC，精确匹配）和攻击成功率（ASR）。

### 基准方法
- VanillaRAG（无防御）
- InstructRAG（自合成推理引导检索）
- AstuteRAG（自适应融合内外部知识）
- TrustRAG（聚类过滤+冲突解决）

### 对比的LLM
- Mistral-12B、Qwen-7B、LLaMA-3.1-8B（开源）
- GPT-4o、DeepSeek-R1（闭源/混合）

## 4. 资源与算力
论文明确说明所有实验在 **NVIDIA A100-SXM4-40GB GPU** 上运行，但**未提供具体GPU数量、训练时长或总计算量**。仅给出了运行时分析：每批(batch)平均耗时约1.21–1.45分钟（因多阶段LLM调用而高于其他方法）。

## 5. 实验数量与充分性
论文进行了**大量且系统的实验**，具体包括：
- **主实验**：5个LLM × 3个数据集 × 4种设置（Clean、PIA、20%投毒、100%投毒）= 60组结果（表1）。
- **多投毒比率实验**：精确到20%、40%、60%、80%、100%共5个水平（附录表3-5），覆盖完整攻击强度谱。
- **消融实验**（图3、表6-8）：
  - 核心模块（SCF、CAF）去除对比；
  - SCF子组件（仅聚类、仅语义、联合）对比；
  - EIRE模块有无对比；
  - 验证干净文档集 Dcor 有无对比。
- **嵌入模型泛化性**（表2）：4种嵌入模型（MiniLM、SimCSE、BERT、BGE）下的性能对比。
- **阈值敏感性分析**（表9-10）：对 τcluster 和 τsemantic 两个参数在合理范围内变化，结果稳定。

**公平性**：多个LLM、多种攻击比率、SOTA基线对比、消融验证充分，实验设计客观、公平、完整。

## 6. 主要结论与发现
- SeCon-RAG **在所有数据集、所有LLM、所有攻击场景下**均一致优于或等于现有防御方法。
- 在高投毒率（100%）下仍保持高ACC（如GPT-4o在HotpotQA上83.6% ACC / 2.4% ASR）和低ASR。
- 在干净场景下性能不退化，甚至有所提升（如DeepSeek-R1在NQ上98% ACC）。
- 两个阶段互补：SCF去除明显异常，CAF解决残留语义冲突，联合效果最强。
- 对过滤阈值不敏感，便于实际部署。

## 7. 优点
1. **首次将结构化语义信息引入RAG防御**，EIRE提供细粒度可解释的语义过滤基础。
2. **两阶段设计思路清晰**：检索前粗过滤 + 生成前细验证，互相弥补。
3. **实验极其全面**：覆盖多种LLM、攻击比率、嵌入模型、消融、阈值分析，结论可靠。
4. **可插拔性**：不依赖特定LLM或嵌入模型，可作为通用防御模块。
5. **稳健性**：AND逻辑联合过滤避免过度过滤，干净场景性能不减，实用性强。

## 8. 不足与局限
- **推理延迟增加**：SCF和CAF均需多次LLM调用（EIRE提取、语义比较、一致性判断），比基线慢约1.5–2倍。
- **依赖语义提取质量**：EIRE基于LLM，可能对领域专有文本或模糊语义表现不佳。
- **需要少量人工验证的干净文档集合 Dcor**（每个数据集10篇），虽小但增加了部署门槛。
- **未开放代码与数据**（论文声明将公开，但当前不可复现）。
- **未验证对抗性更强的新型攻击**（如隐式后门攻击、多步推理攻击等）。
- **实验仅在问答任务上进行**，对其他RAG应用（如摘要、对话）的泛化性未验证。

（完）
