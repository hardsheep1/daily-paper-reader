---
title: "SPEAK: Spatial Prompting with Expert Aligned Knowledge for Tissue Domain Identification in Spatial Transcriptomics"
title_zh: SPEAK：基于专家对齐知识的空间提示方法用于空间转录组学中的组织域识别
authors: "Wei, H., Luo, X., Yu, H., Liang, J., Yang, L., Sauler, M., Kaminski, N., Popa, A., Yan, X."
date: 2026-06-26
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.22.733750v1.full.pdf"
tags: ["query:ac"]
score: 8.0
evidence: 基于大模型的空间域识别（聚类），包含原型更新，类似主动学习
tldr: 空间转录组学数据分析中，准确识别空间域是下游分析的关键。现有方法往往忽略先验知识且鲁棒性不足。SPEAK方法利用大语言模型和人类专家知识，通过构建空间上下文提示，实现零样本推理和专家引导微调。在多个数据集上，SPEAK在域预测准确性、鲁棒性和可解释性上均优于现有方法，并展现出良好的泛化能力。
source: biorxiv
selection_source: fresh_fetch
motivation: 现有空间域识别方法精度低且缺乏对先验知识的有效利用，难以适应复杂组织微环境。
method: SPEAK基于大语言模型，结合细胞类型与标记基因构建空间上下文提示，通过两阶段提示实现零样本推理、专家引导微调和原型更新。
result: 在STARmap、Visium、MERFISH和Xenium数据集上，SPEAK在域预测准确性、鲁棒性和生物学可解释性方面均优于现有方法。
conclusion: SPEAK能有效整合LLM与专家先验知识，提高空间域识别精度，且具备高效微调与跨切片泛化能力。
---

## 摘要
空间分辨转录组学（SRT）数据需要通过空间域识别来实现组织微环境特异性的下游分析。本文提出SPEAK（基于专家对齐知识的空间提示方法），这是一种基于大语言模型（LLM）的方法，通过利用来自LLM和人类专家的先验知识，从SRT数据中识别空间域。SPEAK基于每个细胞/点的细胞类型及其邻近细胞的标记基因构建空间上下文提示，通过两阶段提示实现零样本推理、专家引导的微调和原型更新。在STARmap、Visium、MERFISH和Xenium数据集上的应用表明，SPEAK在域预测准确性、对有限先验知识的鲁棒性、生物学可解释性以及高效专家引导微调的能力（并具有对其他组织切片的泛化性）方面优于现有空间域识别方法。

## Abstract
Spatially resolved transcriptomic (SRT) data requires spatial domain identification to enable tissue microenvironment-specific downstream analyses. Here we present SPEAK (Spatial Prompting with Expert-Aligned Knowledge), a large language model (LLM) -based method to identify spatial domains from SRT data by taking advantage of the prior knowledge from both LLM and human experts. SPEAK constructs a spatial context prompt for each cell/spot based on cell types and marker genes of its neighboring cells, enabling zero-shot inference, expert-guided fine-tuning, and prototype updating through two-stage prompting. Applications to STARmap, Visium, MERFISH and Xenium datasets showed advantages of SPEAK over existing spatial domain identification methods in domain prediction accuracy, robustness to limited prior knowledge, biological interpretability, and capacity for efficient expert-guided fine-tuning with generalizability to other tissue sections.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：空间转录组学（SRT）数据中，准确识别具有生物学意义的空间域（functional tissue units）是进行组织微环境下游分析的关键前提。现有方法（如图神经网络、聚类算法）存在四个主要局限：无法融入先验生物学知识和专家指导；每张切片需从头训练，无法跨样本迁移；输出无生物意义的抽象聚类标签；对预设聚类数目高度敏感，鲁棒性不足。
- **研究动机**：利用大语言模型（LLM）中蕴含的广泛生物学知识，并结合人类专家（如病理学家）的反馈，实现高精度、可解释、泛化性强的空间域识别。
- **整体含义**：提出一种基于LLM的端到端方法SPEAK，通过构造空间上下文提示（Spatial Context Prompt），在零样本、微调、两阶段提示三种模式下工作，在多个SRT平台（STARmap, Visium, MERFISH, Xenium）上均显著超越现有方法，并具备可解释性（输出推理步骤）和轻量级人工标注优势。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将每个细胞/spot的邻域信息（细胞类型组成和标记基因表达）转化为自然语言提示，利用LLM的语义理解和推理能力，结合人类专家提供的少量标注进行模型微调，再通过两阶段提示更新域原型（domain prototypes），从而实现精准且可解释的空间域识别。
- **关键技术细节**：
  - **空间上下文提示构建**：
    - 输入：空间坐标、细胞类型注释（单细胞分辨率下为one-hot，spot级别为去卷积比例）、细胞类型标记基因表达矩阵。
    - 邻域定义：以每个细胞为中心、预设半径（δ，因技术平台而异：STARmap 72μm, MERFISH 100μm, Visium 344μm, Xenium 50μm）的圆形区域内的所有细胞。
    - 空间轮廓（Spatial Profile）：包含两个有序列表——邻域细胞类型列表（按频率降序）和邻域标记基因列表（按平均表达降序）。
    - 任务描述：指定物种、组织类型、潜在域名称列表、输出格式（默认只输出域名称，可选输出推理步骤）。
  - **零样本推断（SPEAK-Z）**：直接将每个细胞的上下文提示输入预训练LLM（如GPT-4o mini, GPT-4o, Gemini 1.5 Pro, Llama-3.1-8B/70B, Qwen3-30B），进行域预测。
  - **微调（SPEAK-F）**：
    - 使用低秩适配（LoRA）对LLM进行参数高效微调。
    - 输入：少量细胞（如30%）的带标签数据，其提示被扩展为包含域原型（domain prototypes）——每个域的平均空间轮廓。
    - 训练配置：3个epoch，batch size=2，默认学习率（OpenAI API）。
  - **两阶段提示（SPEAK-Fs）**：
    - 第一阶段：用微调模型对未标注细胞进行初始预测，筛选出高置信度细胞（超过50%邻域预测为相同域）。
    - 第二阶段：用高置信度细胞更新域原型（若某域无高置信度细胞则保留原始原型），将更新后的原型与上下文提示一起输入微调模型，生成最终预测。
  - **后处理平滑**：对于每个细胞，若多数邻域预测为不同域，则将该细胞重新标注为邻域最流行域。

## 3. 实验设计

- **数据集**：
  1. **STARmap**（小鼠视皮层，单细胞，3个切片，3268细胞，4个域：Layer 1/2/3/5/6，有真实标注）
  2. **Visium**（人背外侧前额叶皮层，spot级别，3个受试者各4切片，47681 spots，7个域：6层+白质，有真实标注；细胞类型通过SDePER去卷积获得）
  3. **MERFISH**（小鼠下丘脑视前区，单细胞，1个受试者5切片，28317细胞，8个域，有真实标注）
  4. **Xenium**（人COPD肺，单细胞，9051细胞，无真实标注，作为示范数据集）
- **基准方法**：对比了16种方法，包括：
  - 图嵌入类：CCST, GraphST, STAGATE, SpaGCN, SEDR
  - 集成类：Seurat, IRIS, BASS, conST, SpaceFlow, SCAN-IT, stLearn, BayesSpace
  - 直方图增强：SpaGCN(HE)
  - 非空间聚类：Leiden, Louvain
- **评估指标**：
  - 准确性：NMI、Homogeneity (HOM)、Completeness (COM)
  - 空间连续性：CHAOS、PAS、ASW
- **实验设置**：
  - 零样本对比：在STARmap上所有方法；在Visium/MERFISH上对比SPEAK-Z与BASS等。
  - 微调/两阶段对比：使用30%标注数据进行微调，并在同一受试者不同切片（between-section）和跨受试者（cross-subject）测试。
  - 消融实验：
    - 域列表错配鲁棒性（移除、添加、替换域）
    - 标注比例对微调性能影响（5%~70%）
    - Dropout鲁棒性（0%~80%基因表达置零）
    - 不同LLM规模对比（开源/闭源）
    - 语义标签实验（用抽象标签替换细胞类型名称以验证语义理解）

## 4. 资源与算力

- **明确说明**：
  - 开源LLM使用vLLM部署在**2块NVIDIA H200 GPU**和**2个CPU核心**上。
  - 推理速度：Qwen3-30B约3秒/1024细胞，Llama-3.1-70B约18秒/1024细胞。
  - API调用（GPT-4o mini等）时间依赖网络，STARmap样本（1088细胞）约5-10分钟。
  - 微调采用LoRA，通过OpenAI API完成，未说明具体GPU型号和时长，仅提及3 epochs, batch size 2。
- **未明确说明**：预训练LLM的原始训练成本；总实验天数或总GPU小时数。

## 5. 实验数量与充分性

- **实验组数**：至少包含：
  - 零样本对比：3个数据集 x 多个LLM + 16种方法 → 多轮（每方法每切片3次取平均）。
  - 微调对比：2个数据集（Visium, MERFISH）的within-section/between-section/cross-subject。
  - 两阶段对比：Visium的cross-subject（3受试者交叉）。
  - 消融：4种域列表错配（每种4次试验）、标注比例（7个梯度）、dropout（7个梯度）、LLM规模（3种开源+3种闭源）、语义标签实验（1次）。
  - 真实案例（Xenium）定性+定量（差异表达分析）。
- **充分性评估**：
  - **优点**：覆盖4种不同技术、2个物种、3个有标注数据集+1个真实案例；对比方法数量多（16种）；评估指标全面（6个）；消融实验覆盖主要超参数（域列表、标注量、噪声、模型大小）。
  - **不足之处**：
    - 所有有标注数据集均来自正常组织（小鼠脑、人脑），缺乏疾病组织（如肿瘤）的定量基准。
    - 未进行领域外（跨组织类型）泛化测试（如用脑组织训练的模型测试肺组织）。
    - 微调阶段仅使用GPT-4o mini（因OpenAI API唯一可微调闭源模型），未测试开源LLM的微调效果。
    - 标注比例实验仅在MERFISH一个数据集上进行，未在多个数据集上验证。
    - 域列表错配实验仅使用GPT-4o mini，未测试更大模型。

## 6. 论文的主要结论与发现

1. **零样本性能领先**：在STARmap上，SPEAK-Z（Gemini 1.5 Pro）在准确性和连续性指标上均优于所有非LLM方法；且性能与LLM参数规模正相关。
2. **自动生成有意义的域名称**：克服了非LLM方法输出无意义聚类标签的缺点，且跨切片可直接合并结果。
3. **对域列表错配高度鲁棒**：允许缺少或冗余域名称，仅在缺失主要域时性能略有下降，但远优于同K值的BASS。
4. **微调可显著提升低质量细胞类型注释下的性能**：在Visium（去卷积估计）和MERFISH（粗粒度细胞类型）上，SPEAK-F较SPEAK-Z有大幅提升，并超越所有非LLM方法。
5. **两阶段提示实现跨受试者泛化**：SPEAK-Fs通过高置信度细胞更新原型，有效适应切片间变异，使得仅用少量标注（30%细胞）即可在跨受试者场景下达到接近本域微调的效果。
6. **极少量标注即可高效微调**：仅5%标注细胞就能使SPEAK优于BASS，且性能随标注增加而持续提升。
7. **真实案例（COPD肺）验证**：SPEAK能区分气道和血管中的平滑肌细胞，而Seurat和IRIS则混淆；SPEAK-F纠正了零样本的错误，并识别出疾病相关的分子特征。
8. **可解释性与计算效率**：SPEAK可输出推理步骤，便于生物学验证；并行处理避免内存瓶颈，适合大规模数据。

## 7. 优点

- **创新性**：首次将LLM与空间转录组学域识别结合，并引入人类专家反馈的微调机制，实现语义理解驱动的聚类。
- **实用性**：
  - 输出域名称而非数字标签，免去后处理注释。
  - 跨切片无需重新训练，直接合并结果。
  - 可提供推理过程，辅助生物学家判断模型合理性。
- **鲁棒性**：对域列表错配、基因表达缺失、标注比例低均表现稳定。
- **泛化能力**：微调模型可高效迁移到同一受试者的其他切片甚至跨受试者（借助两阶段提示）。
- **计算效率**：并行处理每个细胞，避免图神经网络的内存限制，易扩展到大样本。

## 8. 不足与局限

- **依赖LLM先验知识**：对于罕见或研究不足的组织/疾病，模型可能缺乏足够知识，性能受限。论文建议利用置信度识别此类情况，但未定量评估。
- **需要预设域列表**：用户必须提供候选域名称，对于完全未知的样本仍显不便（论文提到未来可借助LLM自动生成）。
- **邻域半径固定**：所有细胞使用同一半径，未考虑不同域或细胞类型的最优尺度（论文承认需未来自适应）。
- **微调LLM的局限性**：
  - 当前仅GPT-4o mini支持API微调，更大模型（如GPT-4o, Gemini 1.5 Pro）无法微调，限制了性能上限。
  - 开源模型微调实验未进行，可能影响部署在敏感数据场景下的选择。
- **实验覆盖不足**：
  - 基准数据集仅包含正常脑组织，未在肿瘤等复杂组织上定量验证（Xenium为疾病组织但无真实标注）。
  - 跨组织泛化（如脑→肺）未测试。
  - 未与基于图像的方法（如SpaGCN(HE)）在适用平台上充分比较（因图像数据缺失）。
- **过度依赖细胞类型注释质量**：零样本在Visium（去卷积）和MERFISH（粗粒度）上表现差，说明细胞类型信息质量是关键瓶颈。
- **代码可用性**：论文声称开源，但未验证实际可用性；需关注后续维护与文档完整性。

（完）
