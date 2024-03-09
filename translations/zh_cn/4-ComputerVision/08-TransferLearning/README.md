# 预训练网络和迁移学习



训练 CNN 需要花费大量时间，并且该任务需要大量数据。然而，大部分时间都花在学习网络可用于从图像中提取模式的最佳低级过滤器上。一个自然而然的问题出现了 - 我们是否可以使用在某个数据集上训练的神经网络，并将其调整为对不同的图像进行分类，而不需要完整的训练过程？

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/108)



这种方法称为迁移学习，因为我们将一些知识从一个神经网络模型转移到另一个模型。在迁移学习中，我们通常从预训练模型开始，该模型已在一些大型图像数据集（例如 ImageNet）上进行过训练。这些模型已经能够很好地从通用图像中提取不同的特征，并且在许多情况下，仅在这些提取的特征之上构建分类器即可产生良好的结果。

> ✅ 迁移学习是一个你在其他学术领域（例如教育）中发现的术语。它指的是从一个领域获取知识并将其应用到另一个领域的进程。

## 预训练模型作为特征提取器



我们在上一节中讨论的卷积网络包含许多层，每一层都应该从图像中提取一些特征，从低级别的像素组合（例如水平/垂直线或笔画）开始，到高级别的特征组合，对应于火焰的眼睛等事物。如果我们在足够大的通用和多样化图像数据集上训练 CNN，网络应该学会提取那些共同的特征。

Keras 和 PyTorch 都包含函数，可以轻松加载一些常见架构的预训练神经网络权重，其中大部分是在 ImageNet 图像上训练的。最常用的那些在上一课的 CNN 架构页面上进行了描述。特别是，您可能需要考虑使用以下之一：

- VGG-16/VGG-19 是相对简单的模型，但仍然能提供良好的准确性。通常使用 VGG 作为首次尝试是一个不错的选择，以了解迁移学习是如何工作的。
- ResNet 是微软研究院在 2015 年提出的模型系列。它们有更多层，因此需要更多资源。
- MobileNet 是一个尺寸较小的模型系列，适用于移动设备。如果您资源不足，并且可以牺牲一点准确性，请使用它们。

以下是 VGG-16 网络从一张猫的图片中提取的样本特征：

[![Features extracted by VGG-16](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/features.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/features.png)

##  猫狗数据集



在这个示例中，我们将使用猫和狗的数据集，这非常接近于现实生活中的图像分类场景。

## ✍️ 练习：迁移学习



让我们在相应的笔记本中查看迁移学习的实际操作：

- [迁移学习 - PyTorch](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/TransferLearningPyTorch.ipynb)
- [迁移学习 - TensorFlow](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/TransferLearningTF.ipynb)

## 可视化对抗猫



预训练神经网络在其大脑中包含不同的模式，包括理想猫（以及理想狗、理想斑马等）的概念。以某种方式可视化此图像将很有趣。然而，这并不简单，因为模式分布在整个网络权重中，并且还以分层结构组织。

我们可以采取的一种方法是从一张随机图像开始，然后尝试使用梯度下降优化技术调整该图像，以使网络开始认为它是一只猫。

[![Image Optimization Loop](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/ideal-cat-loop.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/ideal-cat-loop.png)

然而，如果我们这样做，我们会得到一些非常类似于随机噪声的东西。这是因为有很多方法可以使网络认为输入图像是一只猫，包括一些在视觉上没有意义的方法。虽然那些图像包含了许多典型的猫的图案，但没有什么可以约束它们在视觉上具有独特性。

为了改善结果，我们可以在损失函数中添加另一个术语，称为变异损失。这是一个度量，显示图像中相邻像素的相似程度。最小化变异损失使图像更平滑，并消除噪声 - 从而揭示出更具视觉吸引力的模式。以下是一些此类“理想”图像的示例，它们被归类为猫和斑马，且概率很高：

| [![Ideal Cat](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/ideal-cat.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/ideal-cat.png) | [![Ideal Zebra](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/ideal-zebra.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/ideal-zebra.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| *理想猫*                                                     | *理想斑马*                                                   |

类似的方法可用于对神经网络执行所谓的对抗性攻击。假设我们想欺骗神经网络，让一只狗看起来像一只猫。如果我们拍摄一张狗的图像，该图像被网络识别为狗，那么我们可以对其进行微调，但使用梯度下降优化，直到网络开始将其归类为猫：

| [![Picture of a Dog](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/original-dog.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/original-dog.png) | [![Picture of a dog classified as a cat](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/adversarial-dog.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/images/adversarial-dog.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| *一只狗的原图*                                               | *将一只狗的照片归类为猫*                                     |

在以下笔记本中查看代码以重现上述结果：

- [理想猫和对抗猫 - TensorFlow](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/AdversarialCat_TF.ipynb)

##  结论



利用迁移学习，您可以快速为自定义对象分类任务构建一个分类器，并获得较高的准确度。您会看到，我们现在解决的更复杂的任务需要更高的计算能力，并且无法在 CPU 上轻松解决。在下一单元中，我们将尝试使用更轻量级的实现来使用较低的计算资源训练相同的模型，这将导致准确度略有下降。

##  🚀 挑战



在随附的笔记本中，底部有关于如何将迁移知识与一些类似的训练数据（也许是某种新的动物）结合使用的说明。尝试使用完全新的图像类型，看看您的迁移知识模型的性能如何。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/208)



##  复习与自学



阅读 TrainingTricks.md 以加深您对训练模型的其他方式的了解。

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/08-TransferLearning/lab/README.md)



在这个实验中，我们将使用包含 35 种猫和狗品种的真实牛津-IIIT 宠物数据集，并且我们将构建一个迁移学习分类器。
