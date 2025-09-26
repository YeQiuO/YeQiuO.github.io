---
title: Feedback-Aware Double
date: 2023-09-19 12:00:00
categories: 
- 论文
tags:
- 情绪支持对话
---

![8b28a0b1a05a3699b9284f2383bf86b4_3_Figure_2_-570062161.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1701141604157-75101287-52dc-426c-abea-73184a8b8cda.png#averageHue=%23dadebe&clientId=u6c0d523e-6890-4&from=drop&height=888&id=uc566fb95&originHeight=888&originWidth=1476&originalType=binary&ratio=1&rotation=0&showTitle=false&size=122422&status=done&style=none&taskId=udba71df4-8441-48be-a8c2-7c98cf62d00&title=&width=1476)
# 论文速览
## Abstract

- 双层反馈策略选择器：通过反馈信息预测策略
- 双层控制阅读器：通过策略约束上下文响应
- 策略词典：丰富策略的语义信息
## Introduction
## Related Work
## Problem Formulation
## Approach

- 上下文编码器
   - BlenderBot预训练编码器编码上下文历史对话U，得到Ht
- 双层反馈策略选择器
   - 策略选择
      - 输入
         - BlenderBot Encoder 编码上下文历史对话U，得到隐状态Ht
         - BlenderBot Encoder 编码策略S
         - EmoBERTa Encoder 编码上下文对话U
      - 公式
         - 上下文编码器（策略S同理）：![](https://cdn.nlark.com/yuque/__latex/ac1fc5ab1be07d82a257b37bd33d97ec.svg#card=math&code=%5Cboldsymbol%7BH%7D%3D%5Coperatorname%7BEnc%7D_%7Bcxt%7D%5Cleft%28%5Cboldsymbol%7B%5BCLS%5D%7D%2C%20%5Cboldsymbol%7Bu%7D_%7B1%7D%2C%20%5Cboldsymbol%7B%5BSEP%5D%7D%2C%20%5Cboldsymbol%7Bu%7D_%7B2%7D%2C%20...%2C%20%5Cboldsymbol%7Bu%7D_%7BM%7D%5Cright%29%EF%BC%8CM%E4%B8%BA%E5%AF%B9%E8%AF%9D%E6%95%B0&id=yPlVv)
            - ![](https://cdn.nlark.com/yuque/__latex/b0bba674bde5b13c3ee9ddcbf4308ab6.svg#card=math&code=%5Cboldsymbol%7BH%7D%3D%5Cleft%28%5Cboldsymbol%7Bu%7D_%7B1%7D%2C...%2C%20%5Cboldsymbol%7Bu%7D_%7BT%7D%5Cright%29%EF%BC%8CT%E4%B8%BAToken%E6%95%B0&id=STmQB)
         - 情感编码器：![](https://cdn.nlark.com/yuque/__latex/706d8697b401f422b02bb0c9b8431ff2.svg#card=math&code=%5Cboldsymbol%7BE%7D%3D%5Coperatorname%7BEmoBERTa%7D%5Cleft%28%5Cboldsymbol%7B%5BCLS%5D%7D%2C%20%5Cboldsymbol%7Bu%7D_%7B1%7D%2C%20%5Cboldsymbol%7B%5BSEP%5D%7D%2C%20%5Cboldsymbol%7Bu%7D_%7B2%7D%2C%20...%2C%20%5Cboldsymbol%7Bu%7D_%7BM%7D%5Cright%29%EF%BC%8CM%E4%B8%BA%E5%AF%B9%E8%AF%9D%E6%95%B0&id=tPTRg)
            - ![](https://cdn.nlark.com/yuque/__latex/a456d9c67cf7b9cc120cbf195a184784.svg#card=math&code=%5Cboldsymbol%7BE%7D%3D%5Cleft%28%5Cboldsymbol%7Be%7D_%7B1%7D%2C...%2C%20%5Cboldsymbol%7Be%7D_%7BT%7D%5Cright%29%EF%BC%8CT%E4%B8%BAToken%E6%95%B0&id=dVwQF)
         - 分类：![](https://cdn.nlark.com/yuque/__latex/65b0e5a67b4d4bccf3659a17b88bcf42.svg#card=math&code=%5Cboldsymbol%7Bo%7D%3D%5Coperatorname%7BMLP%7D%5Cleft%28%5Ctanh%20%5Cleft%28%5Cboldsymbol%7BW%7D_%7Bo%7D%5E%7BT%7D%5B%5Cboldsymbol%7Bs%7D%20%3B%20%5Cboldsymbol%7Bc%7D%20%3B%20%5Cboldsymbol%7Br%7D%5D%2B%5Cboldsymbol%7Bb%7D_%7B%5Cboldsymbol%7Bo%7D%7D%5Cright%29%5Cright%29&id=BrWzL)
            - s、c、r 为策略S、上下文H、情感分类E。编码+平均池化操作后得到
   - 双层反馈
      - 回合级反馈：局部变量，当前用户的感受。包含每轮对话Seeker情感Δe和Seeker评分变化Δr
      - 对话级反馈：全局变量Δc，用户的全局状态。包含Seeker在谈话后的情绪压力、Supporter对话题的回应的相关性、Supporter对Seeker感受的理解和共情
      - 融合adapter：整合回合级和会话级反馈的两类语义信息。Δs = Δe + Δr + uΔc【计算损失时，给予正向或负向的反馈】
- 双控读取器（模仿情感聊天机器ECM）
   - context-to-strategy：利用上下文信息来选择上下文相关的策略
      - ![](https://cdn.nlark.com/yuque/__latex/848550b9b9335aa2404d21e95bfe0dbc.svg#card=math&code=%5Cboldsymbol%7Bg%7D%5E%7Bc%7D%20%3D%20%5Coperatorname%7Bsigmoid%7D%5Cleft%28%0A%5Cboldsymbol%7BW%7D_%7Bc%7D%5E%7BT%7D%20%5Cboldsymbol%7Bc%7D%20%2B%20%5Cboldsymbol%7Bb%7D_%7Bc%7D%0A%5Cright%29&id=zvWrz)
   - strategy-to-context：编码阶段可以关注与策略相关的上下文，从而生成策略约束的响应
      - ![](https://cdn.nlark.com/yuque/__latex/9933ba0c82370fb154d585d94bd8bc4b.svg#card=math&code=%5Cboldsymbol%7Bg%7D%5E%7Bo%7D%20%3D%20%5Coperatorname%7Bsigmoid%7D%5Cleft%28%0A%5Cboldsymbol%7BW%7D_%7Bo%7D%5E%7BT%7D%20%5Cboldsymbol%7Bo%7D%20%2B%20%5Cboldsymbol%7Bb%7D_%7Bo%7D%0A%5Cright%29&id=T8ioX)
   - 残差连接：在原始信息和更新信息之间进行权衡
      - ![](https://cdn.nlark.com/yuque/__latex/9e0f6063137f36a3fe3b33b825580bb6.svg#card=math&code=%5Cbegin%7Barray%7D%7Br%7D%0Ao%5E%7B%5Cprime%7D%3D%281-%5Cbeta%29%20%5Ccdot%20o%2B%5Cbeta%20%5Ccdot%20g%5E%7Bc%7D%20%5Cotimes%20o%20%5C%5C%0Ah_%7Bt%7D%5E%7B%5Cprime%7D%3D%281-%5Calpha%29%20%5Ccdot%20h_%7Bt%7D%2B%5Calpha%20%5Ccdot%20g%5E%7Bo%7D%20%5Cotimes%20h_%7Bt%7D%0A%5Cend%7Barray%7D&id=Pq3H4)
- 策略字典
   - 输入策略令牌的描述， 而不是策略令牌，以便模型对策略进行更深入的理解
   - Encoder-Decoder 之间的状态传输类似于MISC，采用cross-attention代替self-attention[MISC: A MIxed Strategy-Aware Model Integrating COMET for Emotional Support Conversation](https://www.yuque.com/jinzang/lnx420/wrq82cgnp6p47q59?view=doc_embed)
- 响应生成
   - BlenderBot Decoder：![](https://cdn.nlark.com/yuque/__latex/2fcc3a7b7d221d4c14647c62cf7fb632.svg#card=math&code=%5Cboldsymbol%7Bp%7D%5Cleft%28y_%7Bz%7D%20%5Cmid%20%5Cboldsymbol%7By%7D_%7B%3Cz%7D%2C%20%5Cboldsymbol%7Bh%7D_%7B%5Cboldsymbol%7Bt%7D%7D%5E%7B%5Cprime%7D%2C%20%5Cboldsymbol%7BV%7D%5Cright%29%3D%5Ctext%20%7B%20Generator%20%7D%5Cleft%28%5Cboldsymbol%7BW%7D_%7By%3Cz%7D%2C%20%5Cboldsymbol%7Bh%7D_%7B%5Cboldsymbol%7Bt%7D%7D%5E%7B%5Cprime%7D%2C%20%5Cboldsymbol%7BV%7D%5Cright%29&id=iPJNE)
- 联合训练
   - 策略预测：反馈感知负对数似然 feedback-aware negative log-likelihood
      - ![](https://cdn.nlark.com/yuque/__latex/1f89ff356697af37e100981c317958c6.svg#card=math&code=%5Cmathcal%7BL%7D_%7B1%7D%3D%5Cleft%5C%7B%5Cbegin%7Barray%7D%7Bccc%7D%0A-%5Chat%7Bo%7D%20%5Clog%20%5Cleft%28%5Coperatorname%7Bsoftmax%7D%5Cleft%28%5Cboldsymbol%7Bo%7D%5E%7B%5Cprime%7D%5Cright%29%5Cright%29%20%26%20%5Ctext%20%7B%20if%20%7D%20%26%20%5CDelta%20s%3E0%20%5C%5C%0A-%5Chat%7Bo%7D%20%5Clog%20%5Cleft%281-%5Coperatorname%7Bsoftmax%7D%5Cleft%28%5Cboldsymbol%7Bo%7D%5E%7B%5Cprime%7D%5Cright%29%5Cright%29%20%26%20%5Ctext%20%7B%20if%20%7D%20%26%20%5CDelta%20s%20%5Cleq%200%0A%5Cend%7Barray%7D%5Cright.%5C%5C&id=bgFRi)
   - 响应生成：标准交叉熵损失优化 cross-entropy
      - ![](https://cdn.nlark.com/yuque/__latex/f44f5ed23dcd84ed7a1a074ffa98da58.svg#card=math&code=%5Cmathcal%7BL%7D_%7B2%7D%3D-%5Csum_%7Bz%3D1%7D%5E%7BZ%7D%20%5Clog%20%5Cboldsymbol%7Bp%7D%5Cleft%28y_%7Bz%7D%20%5Cmid%20%5Cboldsymbol%7By%7D_%7B%3Cz%7D%2C%20%5Cboldsymbol%7Bh%7D_%7B%5Cboldsymbol%7Bt%7D%7D%5E%7B%5Cprime%7D%2C%20%5Cboldsymbol%7BV%7D%5Cright%29&id=tw7yi)
## Experiment

- 采用EmoBERTa-base作为特征提取器，以获取Seeker的情感得分和情感表征，情感得分由softmax函数使用EmoBERTa-base的[CLS]表示获得
## Experimental Results
## Analyses
## conclusion
# 关注的问题

1. 预测策略只依靠对话历史，而不考虑求助者反馈，导致预测的结果与用户无关
2. 建模过程只关注上下文到策略，而不关注策略到上下文和与策略相关的上下文
# 解决方法

1. 双层反馈策略选择器：利用回合级和会话级反馈信息来激励或惩罚策略
2. 双层控制阅读器：策略到上下文流来生成策略约束响应
# 创新点 / 本文的优势

1. 

# 实验结论

# 有待提升的部分

