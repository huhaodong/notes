# OnLine Multi-Target Tracking Using Recurrent Neural Networks

论文链接：[https://arxiv.org/pdf/1604.03635.pdf](https://arxiv.org/pdf/1604.03635.pdf)

代码链接：[https://bitbucket.org/amilan/rnntracking](https://bitbucket.org/amilan/rnntracking)
## 概要

作者提出使用RNN做端到端的在线多目标跟踪，多目标跟踪的挑战有三点：

>* 目标数量的不确定性，目标可能会无规律的消失和出现
>* 对各个目标的位置进行连续的预测
>* 目标与预测点的分配问题

作者提出的模型创新点在于：

>* 是第一个使用RNN做多目标跟踪的模型。模型虽然精度不是非常的突出，但是速度快。
>* 作者使用RNN实现了预测和分配的一整套的多目标跟踪的任务。模型是“model-free”的它不需要知道目标运动规律的先验知识。模型可以很好的捕获线性和非线性运动以及一些高阶的运动关系。
>* 模型可以通过训练学习到如何处理物体消失和出现的情况。以解决分配和关联目标路径的难题。这是因为模型不仅可以通过规整过的输入输出数据对序列进行预测也可以通过不规则的数据集进行相关的处理。
>* 作者提出了一种从生成模型中抽样来生成任意数量的训练样本的方法。

作者使用的数据集是[*2D MOT2015*](https://motchallenge.net/data/2D_MOT_2015/)

<div align=center>
    <img src="./img/f1.PNG"/>
</div>