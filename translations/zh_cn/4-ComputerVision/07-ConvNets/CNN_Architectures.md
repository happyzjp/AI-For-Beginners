# 著名的 CNN 架构



### VGG-16



VGG-16 是一个网络，在 2014 年 ImageNet top-5 分类中获得了 92.7% 的准确率。它具有以下层结构：

[![ImageNet Layers](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch1.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch1.jpg)

如你所见，VGG 遵循传统的金字塔架构，它是一系列卷积池化层。

[![ImageNet Pyramid](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/vgg-16-arch.jpg)

>  来自 Researchgate 的图片

### ResNet



ResNet 是微软研究院在 2015 年提出的模型系列。ResNet 的主要思想是使用残差块：

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/resnet-block.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/resnet-block.png)

>  本文中的图片

使用恒等传递的原因是让我们的层预测前一层的结果与残差块的输出之间的差异 - 因此得名残差。这些块更容易训练，并且可以构建具有数百个此类块的网络（最常见的变体是 ResNet-52、ResNet-101 和 ResNet-152）。

您还可以将此网络视为能够根据数据集调整其复杂性。最初，当您开始训练网络时，权重值很小，并且大部分信号都通过传递恒等层。随着训练的进行和权重变大，网络参数的重要性会增加，并且网络会调整以适应正确分类训练图像所需的表达能力。

### Google Inception 重试 错误原因



谷歌 Inception 架构将这一理念更进一步，并将每一层网络构建为多条不同路径的组合：

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/inception.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/images/inception.png)

>  来自 Researchgate 的图片

在这里，我们需要强调 1x1 卷积的作用，因为它们一开始没有意义。我们为什么要用 1x1 滤波器遍历图像？但是，您需要记住，卷积滤波器也适用于多个深度通道（最初为 RGB 颜色，在后续层中为不同滤波器的通道），并且 1x1 卷积用于使用不同的可训练权重将这些输入通道混合在一起。它也可以被视为通道维度上的下采样（池化）。

这里有一篇关于该主题的优秀博文和原始论文。

### MobileNet



MobileNet 是一个尺寸较小的模型系列，适用于移动设备。如果您资源不足，并且可以牺牲一点准确性，请使用它们。它们背后的主要思想是所谓的深度可分离卷积，它允许通过空间卷积和深度通道上的 1x1 卷积的组合来表示卷积滤波器。这显著减少了参数的数量，使网络的尺寸更小，并且更容易使用更少的数据进行训练。

这里有一篇关于 MobileNet 的优秀博文。

##  结论



在本单元中，您学习了计算机视觉神经网络背后的主要概念 - 卷积网络。用于图像分类、对象检测甚至图像生成网络的现实架构都基于 CNN，只是具有更多层和一些额外的训练技巧。

##  🚀 挑战



在附带的笔记本中，底部有关于如何获得更高精度的注释。做一些实验，看看您是否可以获得更高的准确性。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/207)



##  复习与自学



虽然 CNN 最常用于计算机视觉任务，但它们通常适用于提取固定大小的模式。例如，如果我们处理声音，我们可能还想使用 CNN 来查找音频信号中的一些特定模式 - 在这种情况下，滤波器将是一维的（并且此 CNN 将被称为 1D-CNN）。此外，有时 3D-CNN 用于提取多维空间中的特征，例如视频中发生的某些事件 - CNN 可以捕获随着时间推移而改变的某些特征模式。对可以使用 CNN 完成的其他任务进行一些复习和自学。

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/lab/README.md)



在本实验中，您的任务是对不同的猫和狗品种进行分类。这些图像比 MNIST 数据集更复杂，维度更高，并且有 10 多个类别。