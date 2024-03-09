# 卷积神经网络



我们之前已经看到神经网络在处理图像方面非常出色，甚至单层感知器也能以合理的准确度识别 MNIST 数据集中的手写数字。然而，MNIST 数据集非常特殊，所有数字都集中在图像内部，这使得任务更简单。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/107)



在现实生活中，我们希望能够识别图片上的物体，而不管它们在图片中的确切位置。计算机视觉不同于通用分类，因为当我们尝试在图片中找到某个物体时，我们正在扫描图像，寻找一些特定的模式及其组合。例如，在寻找猫时，我们首先可能会寻找可以形成胡须的水平线，然后胡须的特定组合可以告诉我们它实际上是一张猫的图片。某些模式的相对位置和存在很重要，而不是它们在图像上的确切位置。

为了提取模式，我们将使用卷积滤波器的概念。如你所知，图像由 2D 矩阵或具有色彩深度的 3D 张量表示。应用滤波器意味着我们采用相对较小的滤波器核矩阵，并针对原始图像中的每个像素计算与相邻点的加权平均值。我们可以将此视为在整个图像上滑动的小窗口，并根据滤波器核矩阵中的权重对所有像素进行平均。

| [![Vertical Edge Filter](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/filter-vert.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/filter-vert.png) | [![Horizontal Edge Filter](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/filter-horiz.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/filter-horiz.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

> 图片由 Dmitry Soshnikov

例如，如果我们对 MNIST 数字应用 3x3 垂直边缘和水平边缘滤波器，我们可以在原始图像中存在垂直和水平边缘的地方获得高亮（例如高值）。因此，这两个滤波器可用于“寻找”边缘。类似地，我们可以设计不同的滤波器来寻找其他低级模式：

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/lmfilters.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/lmfilters.jpg)

> 梁-马利克滤波器组图像

然而，虽然我们可以手动设计过滤器来提取一些模式，但我们也可以设计网络，使其自动学习模式。这是 CNN 背后的主要思想之一。

##  CNN背后的主要思想



CNN 的工作方式基于以下重要思想：

- 卷积滤波器可以提取模式
- 我们可以设计网络，使滤波器自动训练
- 我们可以使用相同的方法来查找高级特征中的模式，而不仅仅是在原始图像中。因此，CNN 特征提取工作在特征层次结构上，从低级像素组合开始，直到高级图片部分组合。

[![Hierarchical Feature Extraction](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/FeatureExtractionCNN.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/FeatureExtractionCNN.png)

> 来自 Hislop-Lynch 的论文中的图像，基于他们的研究

## ✍️ 练习：卷积神经网络



让我们继续探索卷积神经网络的工作原理，以及如何通过处理相应的笔记本来实现可训练的过滤器：

- [卷积神经网络 - PyTorch](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/ConvNetsPyTorch.ipynb)
- [卷积神经网络 - TensorFlow](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/ConvNetsTF.ipynb)

##  金字塔架构



用于图像处理的大多数 CNN 都遵循所谓的金字塔架构。应用于原始图像的第一层卷积层通常具有相对较少的滤波器（8-16），这些滤波器对应于不同的像素组合，例如水平/垂直笔画线。在下一层，我们减少网络的空间维度，并增加滤波器的数量，这对应于更多可能的简单特征组合。随着每一层，当我们朝向最终分类器移动时，图像的空间维度减小，滤波器的数量增加。

作为一个例子，让我们看看 VGG-16 的架构，这是一个在 2014 年 ImageNet 的前 5 名分类中达到 92.7% 精度的网络：

[![ImageNet Layers](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch1.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch1.jpg)

[![ImageNet Pyramid](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch.jpg)

>  来自 Researchgate 的图片

## 最著名的 CNN 架构



[继续学习最著名的 CNN 架构](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/CNN_Architectures.md)