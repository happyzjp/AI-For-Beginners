# 神经网络简介



[![Summary of Intro Neural Networks content in a doodle](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/sketchnotes/ai-neuralnetworks.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/sketchnotes/ai-neuralnetworks.png)

正如我们在引言中所讨论的，实现智能的一种方法是训练计算机模型或人造大脑。自 20 世纪中叶以来，研究人员尝试了不同的数学模型，直到近年来，这一方向被证明非常成功。这种大脑数学模型被称为神经网络。

> 有时神经网络被称为人工神经网络 (ANN)，以表明我们讨论的是模型，而不是神经元的真实网络。

##  机器学习



神经网络是机器学习这一更大领域的一部分，机器学习的目标是使用数据训练计算机模型，使其能够解决问题。机器学习构成了人工智能的很大一部分，然而，我们在这个课程中不涵盖经典机器学习。

> 访问我们单独的机器学习初学者课程，以了解有关经典机器学习的更多信息。

在机器学习中，我们假设我们有一些示例数据集 X，以及相应的输出值 Y。示例通常是包含特征的 N 维向量，而输出称为标签。

我们将考虑两个最常见的机器学习问题：

- 分类，我们需要将输入对象分类为两个或更多个类别。
- 回归，我们需要为每个输入样本预测一个数字。

> 在将输入和输出表示为张量时，输入数据集是一个大小为 M×N 的矩阵，其中 M 是样本数，N 是特征数。输出标签 Y 是大小为 M 的向量。

在这个课程中，我们只关注神经网络模型。

## 神经元的模型



从生物学中我们知道，我们的大脑由神经细胞组成，每个神经细胞都有多个“输入”（轴突）和一个输出（树突）。轴突和树突可以传导电信号，轴突和树突之间的连接可以表现出不同程度的电导率（受神经递质控制）。

| [![Model of a Neuron](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/images/synapse-wikipedia.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/images/synapse-wikipedia.jpg) | [![Model of a Neuron](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/images/artneuron.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/images/artneuron.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 真实神经元（来自维基百科的图片）                             | 人工神经元（作者图片）                                       |

因此，神经元的简单数学模型包含几个输入 X 1 , ..., X N 和一个输出 Y，以及一系列权重 W 1 , ..., W N 。输出计算如下：

[![Y = f\left(\sum_{i=1}^N X_iW_i\right)](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/images/netout.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/images/netout.png)

其中 f 是某个非线性激活函数。

> 神经元的早期模型在沃伦·麦卡洛克和沃尔特·皮茨 1943 年的经典论文《神经活动中固有思想的逻辑演算》中进行了描述。唐纳德·赫布在他的著作《行为的组织：神经心理学理论》中提出了训练这些网络的方法。

##  在本节中



在本节中，我们将学习： <Keep This Symbol> 感知器，最早的神经网络模型之一，用于两类分类 <Keep This Symbol> 多层网络，以及如何构建我们自己的框架的配对笔记本

- [Perceptron](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/README.md), one of the earliest neural network models for two-class classification
- [Multi-layered networks](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/04-OwnFramework/README.md) with a paired notebook [how to build our own framework](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/04-OwnFramework/OwnFramework.ipynb)
- 神经网络框架，使用这些笔记本：PyTorch 和 Keras/Tensorflow
- [ 过拟合](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/05-Frameworks/Overfitting.md)
