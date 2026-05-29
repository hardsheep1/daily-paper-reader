---
title: "Synthesizing Privacy-Preserving Text Data via Finetuning *without* Finetuning Billion-Scale LLMs"
title_zh: 无需微调十亿级大语言模型的隐私保护文本数据合成
authors: "Bowen Tan, Zheng Xu, Eric P. Xing, Zhiting Hu, Shanshan Wu"
date: 2025-05-01
pdf: "https://openreview.net/pdf?id=FCm4laCLiH"
tags: ["query:ac"]
score: 4.0
evidence: 在数据合成框架中使用聚类
tldr: 该论文针对差分隐私微调大语言模型作为数据生成器计算成本高的问题，提出CTCL框架。通过预训练一个轻量级条件生成器，并引入聚类机制来控制合成数据的质量，无需提示工程或微调数十亿参数模型。实验表明该方法在隐私保护下生成的数据有效性高，为合成数据生成提供了高效实用的方案。
source: ICML-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-icml-2025-fcm4laclih/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1716, \"height\": 747, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-fcm4laclih/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1684, \"height\": 555, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-fcm4laclih/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1682, \"height\": 435, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-icml-2025-fcm4laclih/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 736, \"height\": 473, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1734, \"height\": 423, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1771, \"height\": 547, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1528, \"height\": 477, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 848, \"height\": 207, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 866, \"height\": 953, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 763, \"height\": 320, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 804, \"height\": 239, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1125, \"height\": 946, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1092, \"height\": 323, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1692, \"height\": 325, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1808, \"height\": 778, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-icml-2025-fcm4laclih/table-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 1773, \"height\": 1402, \"label\": \"Table\"}]"
motivation: 为了解决差分隐私微调大语言模型计算成本高、提示工程依赖手动设计的问题。
method: 提出CTCL框架，预训练轻量级条件生成器，并利用聚类实现可控的数据合成。
result: 在多个隐私保护数据生成任务上，CTCL生成的数据有效支持下游模型训练，且计算开销低。
conclusion: CTCL为资源受限场景下的隐私保护数据合成提供了一种高效且实用的方法。
---

## Abstract
Synthetic data offers a promising path to train models while preserving data privacy. Differentially private (DP) finetuning of large language models (LLMs) as data generator is effective, but is impractical when computation resources are limited. Meanwhile, prompt-based methods such as private evolution depend heavily on the manual prompts, and ineffectively use private information in their iterative data selection process. To overcome these limitations, we propose CTCL (Data Synthesis with **C**on**T**rollability and **CL**ustering), a novel framework for generating privacy-preserving synthetic data without extensive prompt engineering or billion-scale LLM finetuning. CTCL pretrains a lightweight 140M conditional generator and a  clustering-based topic model on large-scale public data. To further adapt to the private domain, the generator is DP finetuned on private data for fine-grained textual information, while the topic model extracts a DP histogram representing distributional information. The DP generator then samples according to the DP histogram to synthesize a desired number of data examples. Evaluation across five diverse domains demonstrates the effectiveness of our framework, particularly in the strong privacy regime. Systematic ablation validates the design of each framework component and highlights the scalability of our approach.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：如何在不依赖微调十亿级大语言模型（LLMs）的前提下，高效生成具有差分隐私（DP）保证的合成文本数据，以用于下游模型训练，同时避免传统提示工程方法的依赖。
- **背景**：现有方法要么需要计算昂贵的DP微调大型LLM（如Llama-7B），要么依赖手动提示（如Private Evolution），后者仅通过DP近邻直方图利用私有数据的嵌入层信息，难以捕获细粒度文本分布，尤其对生成式任务效果不佳。CTCL旨在以轻量级模型和无需领域特定提示的方式解决此问题。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过解耦“高层主题分布信息”和“低层细粒度文本信息”，分别用DP主题直方图（DP topic histogram）和DP微调轻量级条件生成器（140M参数）来捕获。生成器在公共语料上预训练，获得强可控性；主题模型在Wikipedia上聚类得到通用主题。
- **关键组件**：
  - **CTCL-Generator（140M参数）**：基于BART-base进行连续预训练，构建了4.3亿对（条件，文档）数据。条件由Gemma-2-2B从SlimPajama文档中抽取（如关键词、文档类型），模型学会根据条件生成文档。
  - **CTCL-Topic**：基于Wikipedia（600万页面）的文档嵌入（20M模型）进行HDBSCAN聚类得到1300个主题，每个主题用KeyBERT标注10个关键词。
- **私有域适应**：
  - 对私有数据每个文档分配主题，生成带噪声（高斯噪声）的DP主题直方图。
  - 利用主题关键词作为条件，对生成器在私有数据上进行DP微调（DP-Adam，批大小4096，梯度裁剪1.0）。
- **合成数据生成**：根据DP主题直方图比例，对每个主题用对应10个关键词作为条件，采样生成任意数量文档（后处理无额外隐私成本）。

## 3. 实验设计

- **下游任务与数据集**：共5个领域，涵盖3个生成式任务（PubMed医学论文摘要、Chatbot Arena人机指令、Multi-Session Chat人人对话）和2个分类任务（Yelp评论、OpenReview论文评审）。评估指标：生成任务用下游模型（BERT Mini/BERT Small）的下一个词预测准确率，分类任务用RoBERTa-base准确率。
- **Baseline对比**：
  - 直接DP微调下游模型（Downstream DPFT）
  - 标准DP微调（BART Base, GPT2 Small, GPT2 XL-1.5B作为上界，以及LoRA变体）
  - 后处理重采样（Resample）
  - Private Evolution (PE) 和 AUG-PE（基于GPT-3.5、Mixtral-8x7B等）
- **关键对比**：CTCL在所有隐私预算（ε=∞,4,2,1）下，在生成任务上显著优于同等规模（140M级）的基线，尤其严格隐私下优势更大（如PubMed上ε=1时，CTCL比BART Base高约8个点）。分类任务中表现与最佳基线持平或稍优。

## 4. 资源与算力

- **预训练阶段**：
  - 数据构建：使用Gemma-2-2B（2B参数）处理SlimPajama，生成4.3亿条件-文档对。
  - 生成器预训练：256个TPU-v4核，训练约24小时（bf16混合精度，批大小4096）。
  - 主题模型：使用20M参数嵌入模型处理Wikipedia（600万文档），然后进行HDBSCAN聚类（计算量相对较小）。
- **DP微调阶段**：使用单机或小规模GPU即可训练140M模型（批大小4096，2000步），无需大批量资源。文中未明确给出GPU型号或数量，但强调比微调十亿级模型计算成本大幅降低。

## 5. 实验数量与充分性

- **实验数量**：覆盖5个数据集、4个隐私预算、多个对比方法（约10个基线），总计数十组实验。进行了消融实验（验证关键词条件和预训练的作用）、MAUVE分布相似性分析、可扩展性研究（隐私预算和合成数据规模的影响）、以及样本定性分析。
- **充分性**：实验设计较为全面，包含生成式和分类两种任务，覆盖强隐私和弱隐私配置，消融实验验证了组件有效性。可扩展性实验表明CTCL随隐私预算和合成数据量增大而持续提升，而PE方法饱和。公平性方面，注意了数据污染检测（确保无重叠）。总体结论可信。

## 6. 论文的主要结论与发现

- CTCL在严格隐私（小ε）下显著优于同等规模模型（如BART Base, GPT2 Small）和PE方法，尤其生成式任务中优势更突出（因为PE仅捕获嵌入层信息，缺乏细粒度文本内容）。
- 可扩展性强：增加隐私预算或合成数据量能持续改善性能，而PE方法趋于饱和（受限于DP-NN的噪声和固定样本量）。
- 消融显示：关键词条件比无条件的DP微调好，预留练在无噪声下提升有限但在DP噪声下帮助显著。
- 轻量级模型经过良好设计能达到接近十亿级DP微调上界的性能（如PubMed上ε=4时CTCL为35.9%，GPT2 XL-1.5B为37.7%）。

## 7. 优点

- **计算效率高**：仅需微调140M模型，无需大规模资源，适用于资源受限场景（例如设备端）。
- **无需领域特定提示**：预训练阶段使用通用描述性提示，适应任何私有域，降低工程成本。
- **双层信息捕获**：同时利用主题直方图（高层分布）和DP微调（细粒度文本），优于仅依赖近邻选取的PE。
- **无限合成规模**：生成无额外隐私成本，且随规模提升性能持续增长，优于PE的样本量限制。

## 8. 不足与局限

- **合成数据流畅性**：140M模型生成的文本流畅性不如大型LLM（如GPT-3.5）直接提示生成，但下游任务性能可能更重要。
- **依赖预训练成本**：虽然推理阶段轻量，但预训练（4.3亿样本的生成器+Wikipedia聚类）仍需较大算力（256 TPU-v4核24小时）。不过一旦完成，便可共享使用。
- **仅限文本模态**：论文仅针对纯文本数据，未扩展到多模态。未来工作方向提到需要替换为扩散模型等。
- **实验覆盖**：生成任务仅测量语言模型的下一个词预测准确率，未评估合成数据在实际应用（如微调对话模型）的直接效果。分类任务只有两个，且部分领域（如OpenReview）数据量较小（8.4K），结果可能受噪声影响。
- **主题模型假设**：假设Wikipedia覆盖所有私有域，虽然实验显示未分类样本极少，但在极端领域可能失效。

（完）
