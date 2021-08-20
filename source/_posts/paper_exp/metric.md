---
title: 混淆矩阵
categories:
- Paper
tags:
- metric
---

# Confusion Matrix 混淆矩阵

| 预测类别\实际类别 | Positive | Negative |
| ----------------- | -------- | -------- |
| Positive          | TP       | FP       |
| Negative          | FN       | TN       |

纵向是**真实类别**

横向是**预测类别**

左上到右下的对角线上是正确分类的个数。

# 评价指标

## Accuracy 准确率

在全部集合中正确分类的个数占比。

**分类器分对了多少**
$$
accuracy = {TP+TN \over TP+TN+FP+FN}
$$


## Precision 精确率

预测为正向的样本中，预测正确的比例。

**返回的正确的有多少**
$$
precision = {TP\over TP+FP}
$$

## Recall 召回率

真实数据集为正向的样本中，预测正确的比例。

**返回的占应该返回的多少**
$$
recall = {TP\over TP+FN}
$$

## F1 精确率与召回率的调和平均

由于不能单纯的看精确率与召回率，因此需要对这两个值进行调和平均。
$$
F_1 = {2*Precision*Recall\over Precision+Recall}
$$

$$
F_\beta = {(1+\beta^2)*{Precision*Recall\over \beta^2*Precision+Recall}}
$$


比如在医疗领域，我们会对模糊的案例也判为正向，不放过每一个可能，因此就**需要召回率的权重更大**。

## 注意

1. 精确率与召回率属于一个**此消彼长**的关系，反应的结果会很片面。如果我们预测的结果全部设为正确，那么**召回率就会很高**，但是**精确率就会很低**；如果真实数据集中全部是正向的样本，那么**精确率就会很高**，**召回率就会很低**。
2. 如果是N分类问题那么可以计算**宏观的(macro)指标**，就是各个分类相对于正确分类的值；也可以计算**微观的(micro)指标**，就是只保留分类正确或错误的个数，转化为TP, TN, FP, FN.
3. 如果分类器最后只返回一个类别的label, 那么准确率，精确率，召回率都相等？

# ROC AUC

分类器返回的结果是一个概率值。我们通过不断改变阈值，来影响分类结果（大于阈值则为正向，小于阈值则为负向），从而计算各个阈值下的混淆矩阵。

把所有的混淆矩阵表示在一个二维空间，就是ROC(Receiver Operator Characteristic)曲线。

TPR预测为正向的占真实正向的比例；FPR预测为正向的占真实负向的比例。
$$
TPR = {TP\over TP+FN}
$$

$$
FPR = {FP\over FP+TN}
$$

坐标轴上横轴是：FPR，纵轴是：TPR。我们想让TPR尽可能的大，FPR尽可能的小。

AUC就是ROC曲线与`x`轴围成的面积。

## N分类

宏观：对于每一个类别都可以画一个ROC曲线，最后在所有类别上做某种平均。

微观：转化为正确分类的概率与其他的概率。

![image-20210224235712286](/Users/edlison/Library/Application Support/typora-user-images/image-20210224235712286.png)

























