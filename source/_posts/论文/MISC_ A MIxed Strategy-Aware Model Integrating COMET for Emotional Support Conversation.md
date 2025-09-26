---
title: MISC
date: 2023-10-09 12:00:00
categories: 
- 论文
tags:
- 情绪支持对话
---

# 论文速览
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35963383/1699523191286-4f020d4d-6fee-4fc2-b943-e0443b1ca667.png#averageHue=%23f3efeb&clientId=uf4134aed-994d-4&from=paste&height=699&id=u2eef75a2&originHeight=699&originWidth=1693&originalType=binary&ratio=1&rotation=0&showTitle=false&size=171637&status=done&style=none&taskId=ub0fcb7ec-b1d7-4e7b-9ad9-75b720f0d53&title=&width=1693)
## Abstract

- 先前工作的局限性
   - 采用对话级别的情感标签，这种标签过于粗粒度，无法捕捉用户的即时精神状态
   - 大多侧重于在回应中表达同理心，而不是逐渐减轻用户的痛苦
- 本文提出了MISC
   - 首先推断用户的细粒度情感状态，然后使用混合策略巧妙地响应
   - 在基准数据集上的实验结果证明了方法的有效性，并揭示了细粒度情感理解和混合策略建模的好处
## Introduction

- 目前的工作不适用于ESC
   - 粗粒度静态的对话级情感标签无法关注到对话过程中用户情感发生的变化
   - 只有移情反应，而不考虑解决求助者的情感问题
- 本文提出的解决方法
   - 有选择地采用COMET（预训练生成式常识推理模型）生成的知识元组进行细粒度情感理解
   - 混合策略，而不是预测单一策略
   - 设计一套注意力机制
- 实验分析
   - 回答策略建模的重要性，能够提高模型的共情能力
## Related Work

- 情绪感知响应生成
- NLP中的常识性知识
- 策略感知对话模型
## Preliminaries

- ESConv数据集
- 问题设定
   - 通过对话历史、场景、求助者最后一句话，预测回复策略和回复内容
## Model: MISC

- 情感状态增强Encoder
   - 利用COMET提取场景situation和语句x的常识性知识，将常识性知识输入Encoder得到H's和H'x![](https://cdn.nlark.com/yuque/__latex/8b324d356e859d6876c7272556c60b43.svg#card=math&code=%5Cboldsymbol%7BB%7D%5E%7Bs%7D%3D%5Cbigcup_%7Bj%3D1%7D%5E%7BN_%7Br%7D%7D%20%5Coperatorname%7BCOMET%7D%5Cleft%28%5Cmathrm%7Brel%7D_%7Bj%7D%2C%20%5Cboldsymbol%7Bs%7D%5Cright%29%5C%5C%0A&id=PF1XF)
   - 将H's和H'x分别与历史对话c做cross-attention，得到Hs和Hx
   - 将历史对话c输入Encoder得到C
- 混合策略学习模块【从VQ-VAE's codebook文章中抄来的】
   - C输入多层感知机+softmax得到Pg（Pg大小是strategy_size）
   - Pg作为概率分布，hg=Pg*T（T为策略嵌入）
      - 长对话回复中可以引入多种策略，且模型学习灵活方便
- 多因素感知Decoder
   - 将情绪状态和策略表征传入Decoder里的Cross-attention
## Experiments

- ESConv中的每十个话语作为一个样本
- 评价指标
   - 策略预测精度：Acc
   - 传统NLP指标：PPL、BLEU、ROUGE-L
   - 相应多样性：Distinct
   - 人类评估
- 基准模型
   - MT Transformer、MoEL、MIME、BlenderBot-Joint
- 具体实现
- 实验结果
   - 动态细粒度情感标签更能准确给予用户回应
   - 细粒度共情，多策略平稳过渡策略，可以更自然地表达共情并提供帮助
   - 策略作为单独的任务进行预测比单一预测更有利
   - MISC的知识Know得分最高，成功学习到了COMET中的心理状态知识
## Analysis

- 消融实验
- 案例研究
- 细粒度情感理解
   - 用粗粒度的情感标签代替细粒度的心理信息可以显著提高指标
- 混合策略感知移情反应
   - 混合策略有利于平滑的情感支持
   - 混合策略比单一策略更有效
   - 混合策略适用于ESC框架
## Conclusions

- 引入COMET来捕捉用户的即时心理状态
- 设计了一个混合策略感知解码器来产生支持响应
# 解决了什么问题 / 怎么解决的

# 该方法的优势

- 长对话中的过度更加顺畅
# 有什么创新点

- 提出seq2seq模型MISC，在ESC中添加了常识性知识和混合反应策略
- 提出了不同的策略模型并在对话中给予提示
# 实验结果好在哪里，怎么证明的

- 从SOTA延续下来的细粒度情感表现较粗粒度静态情感更好
# 相关工作分析

# 可以提升的地方

- 以动态的方式学习混合响应策略
