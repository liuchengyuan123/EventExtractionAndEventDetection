## 基于多分类或序列打标签（如BIO）的方法

- [x] Joint3EE
    - One for All: Neural Joint Modeling of Entities and Events
    - 以前的sota，将实体提及检测（MED）、事件检测、参数抽取合并在一起训练
- [x] PLMEE
    - 提出了数据生成方式，值得学习
- [x] GATE
    - GATE: Graph Attention Transformer Encoder for Cross-lingual Relation and Event Extraction
    - 使用图神经网络，修改注意力掩码
    - 应用于跨语种的RE和Role Argument判别中
- [x] Event Time Extraction and Propagation via Graph Attention Networks
    - 获取事件的时间线
- [x] CASEE
    - CasEE: A Joint Learning Framework with Cascade Decoding for Overlapping Event Extraction
    - 主要提出解决三个 overlap 问题：
        - trigger word在不同的事件中可能重叠
        - argument在不同的事件中可能重叠
        - argument在同一事件中可能重叠
- [x] Event Extraction by Associating Event Types and Argument Roles
    - 解决多个参数意思接近却被划分为不同类别、导致低频类别准确率低
    - 设计了新的schema模式
    - 用图神经网络DGAT（但是没有详细解释DGAT）来学习association event extraction
    - parameters Inheritance
- [x] Modeling Document-Level Context for Event Detection via Important Context Selection
    - 解决长文本的问题
    - 迭代选择关键的上下文句子来辅助检测句子Si的事件
- [x] Graph Convolutional Networks with Argument-Aware Pooling for Event Detection
    - 提出entity-mention based pooling
    - 在句法依存图上进行GCN
- [x] CLEVE: Contrastive Pre-training for Event Extraction
    - 基于预训练的方法
        - 让模型学习事件结构
        - 充分利用大规模无监督数据
- [x] MOGANED
    - Event Detection with Multi-Order Graph Convolution and Aggregated Attention
    - 在依存句法树上建立多跳的连接，再用attention计算logits
- [x] MLBiNet
    - MLBiNet: A Cross-Sentence Collective Event Detection Network
    - 解决跨句子的问题
    - 用隐层向量建立跨句子间的依赖和句子内部trigger word的依赖
    - 对第一次trigger word的误判有一定的容错性
- [x] CorED
    - CorED: Incorporating Type-level and Instance-level Correlations for Fine-grained Event Detection
    - 考虑事件类型本身之间的关系，用图表示出来
    - 将事件类型表示和文本表示结合，同masked self attention预测
- [x] Hierarchical Chinese Legal event extraction via Pedal Attention Mechanism
    - 提出跳板注意力机制
    - 层级化schemas的使用方式
    - 涉及到参数提取
- [x] OneIE
    - A Joint Neural Model for Information Extraction with Global Features
    - 考虑用全局特征信息来约束最后生成的模型
    - 考虑利用跨任务和跨示例/关系的依赖
    - 用图神经网络来表征上述依赖，并用一个通过图计算出的分数优化解码出的图
- [x] FourIE
    - Cross-Task Instance Representation Interactions and Label Dependencies for Joint Information Extraction with Graph Convolutional Networks
    - 以OneIE作为sota，并对比了他们二者的区别
    - FourIE将
        - Entity Mention Extraction
        - Trigger Detection
        - Argument Detection
        - Relation Extraction
    - 四项信息抽取任务联合建模，学习跨任务/实例/关系的依赖
    - 区别于OneIE，使用的全局特征由神经网络建模，而全局依赖作为一种正则损失加入到总损失中
    - 为了让全局特征能够对训练过程产生影响（这是OneIE没有考虑到的），设计了一套“近似+gumbel softmax”的方法来优化图
- [x] The Art of Prompting: Event Detection based on Type Specific Prompts
    - 探究了多种prompt方式对事件检测的影响
    - 探究3中setting下：supervised；few-shot；zero-shot
    - 模型结构简单，prompt与文本注意力交互，加一个分类头
    - 最佳prompt：综合的APEX
        - event type name
        - prototype seed trigger word
        - definition
- [x] Saliency as Evidence: Event Detection with Trigger Saliency Attribution
    - 有些事件类型与触发词高度相关，有些与触发词相关性很低，但是与上下文高度相关
    - 本文提出了一个方法衡量每个trigger word的显著性归因
    - 并基于显著性归因设计两个基于BERT的模型，最后ensemble
    - SOTA效果
- [x] Treasures Outside Contexts: Improving Event Detection via Global Statistics
    - 用全局统计co-occurence信息辅助进行触发词预测
    - 为了丰富全局统计特征，使用一个临时任务来增加全局特征的维度
- [ ] Honey or Poison? Solving the Trigger Curse in Few-shot Event Detection via Causal Intervention
- [x] Self-Attention Graph Residual Convolutional Network for Event Detection with dependency relations
  - 基于依存句法树的图神经网络
  - 提出图残差网络和图上自注意力机制
  - <i>文章写的不是很清楚，没代码细节看不懂</i>
- [x] Learning Prototype Representations Across Few-Shot Tasks for Event Detection
    - 应用于Few-Shot场景
    - 基于原型网络（Prototypical Netword）的方法很容易受到support set中异常数据的影响。因此本文利用跨任务的关系来实现鲁棒的few-shot分类器。
    - 首先修改了prototype的计算方法，让两个包含相同事件类型的task互相影响，以减少某个task中离群点的影响（我认为这里的task指的是把一组 (N+1)-way K-shot的数据采样为两部分）
    - 为了提高两个任务得到的模型预测的一致性，借助知识蒸馏的方法让两个得到的模型的预测分布尽量接近，用KL散度衡量。
- [x] Lifelong Event Detection with Knowledge Transfer
  - 本文研究的持续学习场景：随着时间继续，模型要学习预测新的事件类型，但是旧的事件类型不能丢失
  - 使用最基础的基于span的预测框架
  - 应用Experience Replay和Knowledge Distillation等技术学习新类型
  - 用新类型调整旧类型的表达
  - 用旧类型来更新新类型的表达，其中对于长尾部分的新类型提出了新的表示更新方式
- [x] EEGCN
    - 本文提出依存句法树上的边和点一样，也应该是动态变化的（contextualize），而不是与上下文无关的，因为在不同上下文中的相同依存关系可能表达的是不一样的事件关系。
    - 提出EEGCN，包括Node Aware Edge Update Module(NAEU)和Edge Aware Node Update Module(EANU)两个部分
- [x] A Graph Convolutional Network with Adaptive Graph Generation and Channel Selection for Event Detection
  - 很多之前的基于依存句法树的工作，生成图之后，图无法优化，并且图上表达的只有语法信息，没有语义信息
  - 这篇工作提出使用自适应的建图方法，应用gumbel softmax trick，使得图可以梯度更新
  - 另外提出MCG-GCN，让网络对不同的信息通道有不同的权重，并且加一个门限制某些信息流通

## 生成式模型

- [x] TANL
    - structured prediction as translation between augmented natural languages
    - 多任务整合在一起的生成形式

### Text $\rightarrow$ Structure

- [x] UIE
    - Unified Structure Generation for Universal Information Extraction
- [x] Text2Event
    - TEXT2EVENT: Controllable Sequence-to-Structure Generation for End-to-end Event Extraction

## 阅读理解式

- [x] EEQA
    - Event Extraction by Answering (Almost) Natural Questions
- [x] MQAEE
    - Event Extraction as Multi-turn Question Answering
- [x] Reading the Manual: Event Extraction as Definition Comprehension
    - 使用了对于每一个事件的定义
        - `someone` killed `someone else` with `something` in `some place` at `some time`
    - 对于低资源场景和零样本场景效果更好
    - 基于阅读理解方式
- [x] Zero-shot Event Extraction via Transfer Learning: Challenges and Insights
    - 关注于研究零样本场景下从text entailment和QA任务迁移到Event Extraction的效果
    - 达到了零样本的新sota
    - 但是距离有监督方法还有很大差距
    - 文章给出了错误分析
- [x] Event Extraction as Machine Reading Comprehension
    - 提出了一套基于现有陈述语句生成对应问题的预训练方法
    - 事件检测水平达到了SOTA

