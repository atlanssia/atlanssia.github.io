---
title: Bayes Spam Filtering
date: 2017-03-20 16:43:22
tags:
---

## Bayes' Theorem
Bayes' Theorem（贝氏定理/贝叶斯定理/贝叶斯方法/贝叶斯推断）

Bayes定理是现代机器学习的核心，其基本公式如下：

![Bayes rule](https://wikimedia.org/api/rest_v1/media/math/render/svg/b1078eae6dea894bd826f0b598ff41130ee09c19)

其中A和B为事件且`P(B) ≠ 0`

- P(A)和P(B)分别为A与B事件独立发生的概率(先验概率)，其中P(B)也称标准化常量(Normalized Constant)
- P(A|B)为事件B发生时A的条件概率
- P(B|A)为事件A发生时B的条件概率

一个经典的例子是：

> 一座别墅在过去的 20 年里一共发生过 2 次被盗，别墅的主人有一条狗，狗平均每周晚上叫 3 次，在盗贼入侵时狗叫的概率被估计为 0.9，问题是：在狗叫的时候发生入侵的概率是多少？
> 
> 假设 A 事件为狗在晚上叫，B 为盗贼入侵，则以天为单位统计，P(A) = 3/7，P(B) = 2/(20*365) = 2/7300，P(A|B) = 0.9，按照公式很容易得出结果：P(B|A) = 0.9*(2/7300) / (3/7) = 0.00058

## Bayes推断在垃圾邮件扫描的应用

早在1996年，Jason Rennie的ifile便已采用Bayes推断过滤邮件，然而2002年，Paul Graham大大降低了过滤的错误率。

Paul Graham的做法是，首先通过对正常邮件和垃圾邮件的样本进行扫描和解析，计算出关键字/关键词在正常和垃圾邮件中出现的频率。比如我们假定对于“发票”这个词，通过扫描样本分析，在正常邮件中出现的频率是0.1%，在垃圾邮件样本中出现的频率是10%。

以此样本分析数据为基础，假如我们收到一封邮件，其中内容包含“发票”这个词，首先假定该邮件是正常邮件和垃圾邮件的先验概率为50%（这是一个中性的假定，实际根据情况也可以适当调整），于是根据公式可得：

`P(A|B) = (0.1 x 0.5)/(0.1 x 0.5 + 0.001 x 0.5) = 0.990099` 

由此我们这封邮件是垃圾邮件的概率是99%。

当然，实际产品应用中并不是以一个单词或者词组的概率来确定该邮件是否为垃圾邮件，因为正常邮件里也会有出现这些关键字的可能。实际做法是需要通过一系列关键字计算这封邮件中的联合概率。