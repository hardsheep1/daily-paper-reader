---
title: Customized Multiple Clustering via Multi-Modal Subspace Proxy Learning
title_zh: 通过多模态子空间代理学习的定制化多重聚类
authors: "Jiawei Yao, Qi Qian, Juhua Hu"
date: 2024-09-25
pdf: "https://openreview.net/pdf?id=xbuaSTqAEz"
tags: ["query:ac"]
score: 4.0
evidence: 使用GPT-4大模型指导聚类
tldr: 现有多重聚类方法难以灵活适应个性化需求。本文提出Multi-Sub，利用CLIP和GPT-4的多模态能力，通过自动学习子空间代理将用户文本提示与视觉特征对齐，实现端到端的定制化多重聚类。实验表明该方法能有效生成符合用户偏好的多种聚类结构。
source: NeurIPS-2024-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2024-xbuastqaez/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 588, \"height\": 503, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-xbuastqaez/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1435, \"height\": 1004, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-xbuastqaez/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 722, \"height\": 430, \"label\": \"Figure\"}, {\"url\": \"assets/figures/openreview/openreview-neurips-2024-xbuastqaez/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1146, \"height\": 773, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/openreview/openreview-neurips-2024-xbuastqaez/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1296, \"height\": 344, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-xbuastqaez/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1441, \"height\": 468, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-xbuastqaez/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1013, \"height\": 661, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-xbuastqaez/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1012, \"height\": 517, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-xbuastqaez/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1447, \"height\": 247, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-xbuastqaez/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1154, \"height\": 550, \"label\": \"Table\"}, {\"url\": \"assets/tables/openreview/openreview-neurips-2024-xbuastqaez/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 561, \"height\": 378, \"label\": \"Table\"}]"
motivation: 现有深度多重聚类方法难以灵活适应多样化的用户分组需求。
method: 提出Multi-Sub框架，结合CLIP和GPT-4，通过多模态子空间代理学习对齐文本提示与视觉表示。
result: 实验证明该方法能有效生成符合用户偏好的多种聚类，优于现有方法。
conclusion: 通过引入大语言模型，实现了更具个性化和灵活性的多重聚类。
---

## Abstract
Multiple clustering aims to discover various latent structures of data from different aspects. Deep multiple clustering methods have achieved remarkable performance by exploiting complex patterns and relationships in data. However, existing works struggle to flexibly adapt to diverse user-specific needs in data grouping, which may require manual understanding of each clustering. To address these limitations, we introduce Multi-Sub, a novel end-to-end multiple clustering approach that incorporates a multi-modal subspace proxy learning framework in this work. Utilizing the synergistic capabilities of CLIP and GPT-4, Multi-Sub aligns textual prompts expressing user preferences with their corresponding visual representations. This is achieved by automatically generating proxy words from large language models that act as subspace bases, thus allowing for the customized representation of data in terms specific to the user’s interests. Our method consistently outperforms existing baselines across a broad set of datasets in visual multiple clustering tasks. Our code is available at https://github.com/Alexander-Yao/Multi-Sub.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有多重聚类方法能自动发现数据的不同潜在结构，但难以灵活适应多样化的用户个性化分组需求。用户需要从多个聚类结果中手动理解和选择符合自己兴趣的聚类，效率低下且不实用。
- **背景**：传统多重聚类方法（如基于特征子空间、信息论等）和深度学习方法（如ENRC、iMClusts、AugDMC）通常产生多个聚类后由用户自行挑选。最近Multi-MaP利用CLIP通过对比概念指导聚类，但需要用户提供与感兴趣概念不同的对比概念，且将表征学习和聚类分成两个阶段，导致次优性能。
- **意义**：提出一种能直接根据用户高维兴趣（如“颜色”、“种类”）自动生成定制化聚类结果的端到端方法，降低用户干预，提升聚类实用性和效率。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- **利用多模态模型（CLIP + GPT-4）**：用户输入简洁的高层概念（如“color”），GPT-4自动生成该概念下的常见类别（如红、黄、绿）作为**参考词**（reference words）。假设目标聚类类别与这些参考词共享相同的子空间，因此可通过参考词的线性组合构造每个图像的**代理词**（proxy word）表示。
- **两阶段交替迭代学习**：Phase I（代理学习与对齐）：学习可学习的潜因子，计算出每个图像代理词的权重，并通过CLIP的图像-文本对齐损失优化图像编码器的投影层（其余冻结），使代理词嵌入与图像嵌入对齐。Phase II（聚类损失）：基于当前代理词和图像嵌入生成伪标签，引入簇内紧凑性损失（L_intra）和簇间分离性损失（L_inter），进一步优化投影层。两阶段交替迭代直到收敛（Phase I 100epoch，Phase II 10epoch）。

### 关键技术细节
- **代理词生成**：对于图像\(x_i\)，GPT-4提供K个参考词\(z_k\)，其嵌入\(\phi(z_k)\)。代理词嵌入：\( w_i = \sum_{k=1}^K a_{i,k} \phi(z_k) \)，权重\( a_{i,k} = \frac{\exp(p_i z_k)}{\sum_j \exp(p_i z_j)} \)，其中\(p_i\)是可学习的潜因子。
- **对齐损失**：\( L(w_i) = -\langle f(x_i), h(\phi(t^*_i) \| \phi(w_i)) \rangle \)，其中\(t^*_i\)是包含用户概念和代理词的文本提示（如“a fruit with the color of *”）。文本编码器\(h\)冻结，图像编码器\(f\)仅开放投影层。
- **聚类损失**：最终表示\(v_i = [w_i, x_i]\)。\( L_{intra} = \frac{1}{N_{intra}} \sum_{i,j\in intra} \|v_i - v_j\|^2 \)，\( L_{inter} = \frac{1}{N_{inter}} \sum_{i,j\in inter} \max(0, m - \|v_i - v_j\|) \)，总损失\( L_{total} = \lambda L_{intra} + (1-\lambda)L_{inter} \)，λ=0.5。
- **过程**：Phase I 100 epochs → Phase II 10 epochs，交替进行直到收敛（总计1000 epochs，Adam优化器，学习率从{1e-1,...,5e-4}中调，权重衰减从{5e-4,...,0}中调）。

## 3. 实验设计

### 数据集
- **7个数据集**：Stanford Cars（1200张，颜色/类型）、Card（8029张，点/花色）、CMUface（640张，表情/眼镜/身份/姿势）、Fruit（105张，颜色/种类）、Fruit360（4856张，颜色/种类）、Flowers（1600张，颜色/种类）、CIFAR-10（60000张，类型/环境——作者自己标注的多重聚类）。
- **特点**：覆盖不同规模、不同属性类型（颜色、种类、身份、姿态等）。

### Benchmark与对比方法
- **对比7种方法**：传统方法MSC、MCV；深度方法ENRC、iMClusts、AugDMC、DDMC、Multi-MaP。
- **评价指标**：NMI（归一化互信息）和RI（Rand指数），运行k-means 10次取平均（除Multi-Sub端到端外）。

### 额外分析
- **Zero-shot变体对比**：CLIP GPT（GPT-4生成候选标签做零样本分类）、CLIP label（用真实标签做零样本分类），用于评估Multi-Sub相对于直接使用CLIP的增益。
- **消融实验**：不同子空间构造（token嵌入/prompt嵌入/文本嵌入）以及不同文本编码器（CLIP、ALIGN、BLIP）。
- **可视化**：t-SNE可视化Fruit数据集的颜色和种类聚类。
- **超参数敏感性**：λ从0.1到1.0各取0.1步长。
- **新需求适应性**：在Fruit数据集测试“形状”这一新需求。

## 4. 资源与算力

- **文中明确说明**：“The experiments are performed on four NVIDIA GeForce RTX 2080 Ti GPUs.”
- **未具体说明**：单个实验的训练时长、总GPU小时数等。仅提到每个偏好训练1000 epochs，未给出具体时间。

## 5. 实验数量与充分性

- **实验组数**：主实验在7个数据集、每个数据集2个聚类任务（共14个任务）上完成，每个任务报告NMI/RI。此外还有：CLIP零样本对比（14个任务）、不同文本编码器对比（14个任务）、消融实验（以CIFAR-10为例展示不同子空间构造组合7种×2种环境→14种情况，但只展示了CIFAR-10的结果，对其他数据集隐去）、超参数敏感性（CIFAR-10的λ）、新需求实验（Fruit形状聚类）、可视化（Fruit）。
- **充分性**：实验覆盖主流多重聚类数据集，对比了7种最新方法，指标标准化，有95%置信度检验（表格中加粗最好结果）。消融实验虽只展示CIFAR-10，但已足够说明各模块贡献。实验设计公平，所有基线遵循其原始设置。
- **不足**：部分数据集（如Fruit只有105样本）较小，但已覆盖多数公开数据集；大规模多样化数据集仍缺乏。

## 6. 论文的主要结论与发现

- **Multi-Sub在14个聚类任务中全部超越所有基线**，在NMI和RI上均获得显著提升（如Fruit颜色NMI从0.8973→0.9693，CMUface身份NMI从0.6625→0.7441等）。
- **优于CLIP零样本变体**：Multi-Sub不仅超过CLIP GPT（有标签噪声），还超过直接使用真实标签的CLIP label，表明代理词学习能更好地捕捉用户感兴趣的子空间，过滤无关噪声。
- **消融研究**：使用词token嵌入（\(\phi(w_i)\)）作为子空间通常优于prompt或文本编码嵌入；结合图像和文本嵌入的簇表示能进一步提升性能。
- **文本编码器影响**：ALIGN在某些抽象属性（颜色、情绪）上优于CLIP和BLIP，CLIP在身份任务上更强，提示需要根据任务选择文本编码器。
- **新需求适应性**：成功将形状这一新需求转化为高质量聚类（NMI 0.752，RI 0.891），表明方法可泛化至未预定义的属性。

## 7. 优点

1. **创新性**：首次在多重聚类中利用GPT-4自动生成参考词作为子空间基，无需用户提供具体类别名或对比概念，极大降低了用户负担。
2. **端到端训练**：将表征学习与聚类目标耦合在交替迭代框架中，避免了两阶段分离导致的次优性。
3. **灵活性**：可适应任意用户定义的文本概念（如颜色、种类、形状），且无需重新训练CLIP主模型（仅更新投影层和潜因子），高效且参数少。
4. **鲁棒性**：在多个数据集、多种属性上一致领先，消融实验验证了各组件的必要性。
5. **实用性强**：开源代码，便于复现和应用。

## 8. 不足与局限

1. **依赖大语言模型偏差**：GPT-4生成的参考词可能带有偏见（如种族、性别刻板印象），可能导致聚类结果歧视性；对于无语义特征的任务（如人脸身份），随机选WordNet词可能不可靠。
2. **数据集规模有限**：当前多重聚类公开数据集普遍较小（最大CIFAR-10，但仅有两个聚类任务），缺乏大规模、多属性、真实场景的数据集，限制了验证全面性。
3. **算力报告不完整**：未提供单次实验耗时、收敛曲线等，对于实际部署的可行性缺乏量化说明。
4. **超参数敏感性**：尽管λ设置为0.5通用，但消融显示不同λ对性能有影响（如CIFAR-10上NMI波动约0.05），需要在更多数据上验证鲁棒性。
5. **对比基线稍弱**：部分传统基线（如MSC、MCV）性能远低于深度方法，对比选择性可能不够强；未与最新的大规模视觉语言聚类方法（如基于DINOv2的）比较。
6. **身份聚类处理粗糙**：对于无语义标签的聚类（如CMUface身份），随机选WordNet词作为参考词，缺乏理论支撑，可能需要专门策略。

（完）
