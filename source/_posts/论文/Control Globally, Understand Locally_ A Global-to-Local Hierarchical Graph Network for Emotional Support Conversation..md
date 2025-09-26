---
title: Control Globally, Understand Locally
date: 2023-09-09 12:24:00
categories: 
- 论文
tags:
- 情绪支持对话
---

论文的创新点在于在Encoder和Decoder之间加了一层 GCNConv 和 RGCNConv
# 论文速览
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701754768803-a08b63f1-ca9b-4e2c-8b64-5bec0980a0ad.png#averageHue=%23e6e5e3&clientId=u93869adf-5c8c-4&from=paste&height=515&id=u783acc1a&originHeight=515&originWidth=1007&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112786&status=done&style=none&taskId=u247291fd-334a-4c21-84a8-047685fd8fa&title=&width=1007)
## Abstract

- 目前研究的缺陷
   - 关注序列上下文信息，忽略了全局原因和局部心理意图与其的层次关系
- 本文
   - 提出了一个全局到局部的**层次图网络**来捕获多源信息(全局原因、局部意图和对话历史)并建模它们之间的层次关系，该网络由一个多源编码器、一个层次图推理器和一个全局引导解码器组成
   - 设计了一个新的训练目标来监测全局的语义信息
## Introduction

- 探索求助者情绪问题的原因：**全局**地控制情感支持对话的整个流程
- 了解求助者的心理意图：帮助系统**局部**了解求助者当前时的心理状态
- 本文目标
   - 捕获全局原因和局部心理意图
   - 建模全局与局部之间的关系
- 解决方法【全局到局部层次图网络GLHG】
   - 多源编码器：COMET提取局部心理意图
   - 层次图推理机：全局原因(对话级)、局部心理意图(句子级)和对话历史之间的层次关系进行建模
   - 解码器中设计了一个新的训练目标来监控全局原因的语义信息
## Related Work

- 图建模对话
   - GCN 利用自我和对话者间依赖性来模拟会话上下文
   - EGAE 使用图网络捕获面向任务对话中的对话模式
- 常识性知识
   - 与本人有关的心理状态：xReact、xIntent
## Approach

- 问题定义
- 多源编码器
   - BlenderBot Encoder + Max-pooling
   - 上下文 + 全局原因 + 局部原因
- 分层图推理机
   - GAT 图注意力网络：其他邻域信息的特征传播到当前节点，具有确定节点之间重要性和相关性的优点
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777471738-5656cb06-b30d-458c-9dd1-432cf59fc949.png#averageHue=%23f8f5f1&clientId=u73488553-035a-4&from=paste&height=42&id=ubc5a798e&originHeight=42&originWidth=457&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6264&status=done&style=none&taskId=u04ba91e3-f3d5-4568-a451-091732e55c0&title=&width=457)注意力函数（2017出版）
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777606138-33d97b8c-8966-4a59-9359-ce99195ba4db.png#averageHue=%23f9f5f1&clientId=u73488553-035a-4&from=paste&height=31&id=u8c13badf&originHeight=31&originWidth=169&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2843&status=done&style=none&taskId=ue18490a5-070e-4545-b7f1-de70dbedbe9&title=&width=169)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777482262-fd172bc6-5f7f-4b43-b667-c12cd9b52505.png#averageHue=%23f8f5f3&clientId=u73488553-035a-4&from=paste&height=72&id=u7eaded81&originHeight=72&originWidth=306&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7021&status=done&style=none&taskId=uf79d1ca1-fe0c-4f65-9c17-d79f4414a1d&title=&width=306)注意力机制
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777489757-3d64bf9e-8b95-4298-a754-239e59ec6e7b.png#averageHue=%23faf8f6&clientId=u73488553-035a-4&from=paste&height=65&id=u19b0d451&originHeight=65&originWidth=273&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5031&status=done&style=none&taskId=uda80cd4d-bc05-4a08-9e2e-292835a64ef&title=&width=273)
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777519559-801ad4da-0c7f-47f2-8ab6-ed20e4e19101.png#averageHue=%23f9f6f2&clientId=u73488553-035a-4&from=paste&height=36&id=u01a11541&originHeight=36&originWidth=385&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5467&status=done&style=none&taskId=ua98af47c-a323-4ce8-8350-0fff4e93482&title=&width=385)注意力机制
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777507833-1ff3f1f2-17cb-4ba0-991b-a61b42818cfc.png#averageHue=%23faf8f6&clientId=u73488553-035a-4&from=paste&height=123&id=uc8ddddc6&originHeight=123&originWidth=272&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9760&status=done&style=none&taskId=u47b417ce-1d15-4b6b-a60e-54dec454107&title=&width=272)
- Global-guide 解码器
   - 响应生成
      - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777873314-d88aa900-555f-40b2-8a87-4589351ce738.png#averageHue=%23f9f7f4&clientId=u73488553-035a-4&from=paste&height=37&id=u660da265&originHeight=37&originWidth=453&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5522&status=done&style=none&taskId=ueaf8d72e-c66e-4e53-9943-2d657179864&title=&width=453)，v表示图神经网络得到的全局变量
   - 监督全局语义信息：预测问题类型
      - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701777996376-86594b70-a6b5-4f21-90e2-e6bbdbc222b0.png#averageHue=%23f7f3ef&clientId=u73488553-035a-4&from=paste&height=34&id=ue4727031&originHeight=34&originWidth=265&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4093&status=done&style=none&taskId=u9f79cbd4-3dc5-4903-aae4-e7515a03f3f&title=&width=265)
- 联合训练
   - 对数似然损失+交叉熵损失
   - ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701778036470-9378fc9b-a533-40a9-9990-69ffe0036656.png#averageHue=%23fbfaf9&clientId=u73488553-035a-4&from=paste&height=107&id=u8f6ea386&originHeight=107&originWidth=368&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7522&status=done&style=none&taskId=u63c018fe-f6a9-4bf3-867a-9ed7eaf03cf&title=&width=368)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701778063569-1fc4c281-c256-4804-ba39-785ddd2e4b1e.png#averageHue=%23f9f6f3&clientId=u73488553-035a-4&from=paste&height=34&id=u0fa5faee&originHeight=34&originWidth=193&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2545&status=done&style=none&taskId=u1e079cb0-33c7-4dcc-94ab-2120d4452d1&title=&width=193)
## Experiments

- 指标：plexity (PPL), BLEU-n (B-n), ROUGE-L (R-L), Distinct-1(D-1), and Distinct-2 (D-2)
- 由于有了意图特征，提出建议更具体有效
## Conclusion

- 全局到局部的层次图网络(Global-to-Local Hierarchical Graph network, GLHG)来捕获多源信息并从全局到局部的角度建模层次关系
- 新的训练目标“预测Seeker遇到问题的类型”
# 关注的问题 / 本文的优势

# 解决方法 / 创新点

- **多源编码器**利用情境信息并将心理意图与COMET结合，捕获全局原因和局部意图
- **分层图推理机**在全局原因、局部心理意图和对话历史之间进行交互，建模不同层次的关系（hierarchical graph reasoner）
# 实验结论

# 有待提升的部分

