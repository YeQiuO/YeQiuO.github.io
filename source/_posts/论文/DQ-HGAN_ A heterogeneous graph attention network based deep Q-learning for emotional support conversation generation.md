---
title: DQ-HGAN
date: 2023-09-14 12:00:00
categories: 
- 论文
tags:
- 情绪支持对话
---

# 论文速览
## Abstract

- 关注的问题
   - 动态建模对用户状态，包含个体的意图和情感
   - 综合各类因素选择最合适的支持策略
- 提出的方法【基于异构图注意力网络的深度Q-learning情感支持对话生成】
   - 为了捕获用户意图、情感和历史对话之间的交互关系，基于意图词典和情感分类器，构建了异构图注意力网络
   - 采用基于DQN的最优响应策略以指导响应生成，优于传统的基于规则或启发式方法
## Introduction

- 目前研究
   - ESC任务要求能够确定求助者的心理意图和情绪状态，以便提供适当的支持。因此整合意图识别和情感识别对于提高情感支持对话的质量至关重要，且目前的方法对用户状态建模不充分。
- 关注的问题
   - 建模用户状态
   - 选择最优策略，以产生有效的保障响应
- 提出的方法
   - 设计了基于注意力的**异构图网络**，与用户的意图、情感和历史对话交互，可以有效地捕获和建模图中不同类型的节点和边
   - 构建**意图词典**和**情感分类器**来捕捉求助者在语境中的细微情感表达
   - **DQN算法**对用户未来反馈的期望值进行估计，帮助系统选择获得最优长期值的策略。其允许系统从用户的反馈中学习，调整其策略，以提供最有效的支持响应。
- 主要贡献
   - 提出了一种新的方法DQ-HGAN，将**意图和情感识别与策略生成相结合**，以提高情感支持对话系统的质量和个性化
   - 构建**意图词典和情感分类器**，捕捉求助者在语境中的细微情感表达并跟踪其状态
   - 设计了一种基于注意力机制的**异构图网络**，与用户的意图、情感和历史对话进行交互，并选择最优的支持策略以生成有效的支持响应
   - ESC生成中使用ESC生成中使用**强化学习**，具体来说，使用DQN算法（Deep Q-Network）估计用户未来反馈的期望值，动态调整策略以提供最有效的支持响应
## Related Work

- 对话中的意图和情感识别【在模型中融合了“意图”这个特征】
   - 多头注意力机制
      - 多头注意力机制来捕捉用户的意图和情感。缺点：缺乏有效捕捉用户细微情感表达的能力
      - 使用预训练模型，增强PLM对话相关性，识别对话意图、推断对话情感。缺点：不是专门为ESC任务定制的，性能差
   - 词典
      - 词典包含特定意图或情感相关的词汇和短语，利用基于规则的算法将context与意图词典进行匹配，并分配相应的意图标签。缺点：只将单个单词与标签匹配，可能会忽略整个句子的意图或情感含义
- 图建模【捕获会话系统中用户意图、情感和对话历史之间的复杂关系】
   - 同构图【忽略了用户意图和情感的异构性】
      - GAT 图注意力网络，利用自注意力机制来捕获对话图中意图和情感节点之间的交互
      - GCN 图卷积网络，利用图结构在节点之间传播信息，并捕获对话数据中的上下文依赖
   - **异构图注意力网络是专为表示图中不同类型的节点和边而设计的**，它擅长对**不同的节点**类型进行建模，如用户话语、系统响应、情感状态和意图，从而更全面地了解用户的情感状态；还擅长捕捉**不同类型的边**，包括顺序依赖、自依赖和状态依赖，从而能够更准确地表示用户的情感状态。此外，它还包含了一种**注意力机制**来进行重要性加权，允许它在聚合过程中专注于最相关的信息，从而更全面地了解用户的状态。
- 策略选择
   - 基于规则或启发式方法
   - 强化学习方法（如：Q-learning）
      - 采用DQN估计不同对话动作的期望值，并学习了一种最大化该值的策略。从用户反馈中学习，并生成更有吸引力和信息量的响应
- 响应生成
   - 目前流行的Encoder-Decoder模型往往专注于根据对话历史生成回复，而没有考虑用户的意图、情感以及合适的支持策略
# Preliminaries

- ESConv：标记对话，并将其转换为词嵌入，以将其输入到模型中
- COMET：使用COMET初始化模型的词嵌入，并在ESConv数据集上进行微调，以提高其构建意图词典的有效性
- ATOMIC：得到意图或目的（xIntent）
- NRC VAD 词典：得到情感词典，每个单词对应的效价-觉醒-支配（Valence-Arousal-Dominance）
- 问题定义：上下文+策略+Query =>响应Yt。最优策略基于当前状态和期望的长期回报（通过Q-learning预测）
## Method
![88c61f5be72a795a087441904fcd0ad9_3_Figure_2_780332990.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701484543763-403c4648-68f4-4644-bf00-e2356c06841c.png#averageHue=%23faf7f2&clientId=u14b93dfd-364a-4&from=drop&height=414&id=u929b8784&originHeight=426&originWidth=1515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=120534&status=done&style=none&taskId=ucc43224e-f0e2-42ef-afb9-2febaa85f4b&title=&width=1472)

- 多源编码器
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701484574764-c5051f60-2b3c-40cf-a161-5848c1d67bc2.png#averageHue=%23edb36f&clientId=u14b93dfd-364a-4&from=paste&height=436&id=u5e53a835&originHeight=669&originWidth=641&originalType=binary&ratio=1&rotation=0&showTitle=false&size=158719&status=done&style=none&taskId=uecd27598-ce01-4240-b430-2d212d2d537&title=&width=418)
   - transformer编码器（TransformerEncoder）
      - ht = TransformerEncoder(Ht)
   - 意图词典（COMET）：通过对ATOMIC 微调，同去意图关键词，构建意图词典（意图关键词，对应的词嵌入）
      - ![](https://cdn.nlark.com/yuque/__latex/8f5ffb715d4daf22950bca916bd2a7a5.svg#card=math&code=g_%7Bt%7D%3D%20%20TransformerEncoder%20%20%5Cleft%28%5Csum_%7Bw_%7Bi%7D%20%5Cin%20%5Cmathscr%7BF%7D%7D%20%5Coperatorname%7Bsoftmax%7D%5Cleft%28c_%7Bi%7D%5E%7BT%7D%20h_%7Bt%7D%5Cright%29%20c_%7Bi%7D%5Cright%29%20%0A&id=zrkX0)
   - 情感分类器（NRC VAD词典）
      - ![](https://cdn.nlark.com/yuque/__latex/b7de70c62c7cecf816e4530ddb3cf455.svg#card=math&code=e_%7Bt%7D%3D%5Ctext%20%7B%20TransformerEncoder%20%7D%5Cleft%28%5Csum_%7Bw_%7Bk%7D%20%5Cin%20%5Cmathscr%7BZ%7D%7D%20%5Coperatorname%7Bsoftmax%7D%5Cleft%28z_%7Bk%7D%5E%7BT%7D%20h_%7Bt%7D%5Cright%29%20g_%7Bt%7D%5Cright%29%20%5Ctext%20%7B%20%7D%0A%0A&id=vfFyc)
- 基于异构图的用户状态跟踪
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701484605842-86e4e5d2-9d20-4ffd-ae60-37220c9258dd.png#averageHue=%23f5f3f0&clientId=u14b93dfd-364a-4&from=paste&height=582&id=u04abbd51&originHeight=582&originWidth=1250&originalType=binary&ratio=1&rotation=0&showTitle=false&size=276927&status=done&style=none&taskId=u03066adb-a4cc-4e2e-a2d3-ddc077ce213&title=&width=1250)
- DQN策略规划
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701484626201-fde66e58-39a7-40f7-9e6e-fe70a7cd8dd7.png#averageHue=%23fbf2e5&clientId=u14b93dfd-364a-4&from=paste&height=404&id=ub87ff6c0&originHeight=404&originWidth=1219&originalType=binary&ratio=1&rotation=0&showTitle=false&size=106295&status=done&style=none&taskId=u5730c13b-26f6-4678-9432-485c186e0d3&title=&width=1219)
- 响应生成解码器
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701484638477-fd1f5c6b-b6f5-43f0-af14-a22d10dfe762.png#averageHue=%23f4bf93&clientId=u14b93dfd-364a-4&from=paste&height=747&id=ucd6b560d&originHeight=747&originWidth=668&originalType=binary&ratio=1&rotation=0&showTitle=false&size=178968&status=done&style=none&taskId=u53c8d01d-af1f-4c41-b539-f8303c34bdd&title=&width=668)

Experiments
Conclusion
# 关注的问题 / 本文的优势

# 解决方法 / 创新点

# 实验结论

# 有待提升的部分

