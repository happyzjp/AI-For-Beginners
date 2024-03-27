# 分词



我们之前已经学习了目标检测，它能够通过预测*边界框*来定位图像中的物体。然而，对于一些任务，我们不仅需要边界框，而且需要更精确的物体定位。这个任务被称为**分割**。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/112)



分割可以被视为**像素分类**，对于图片中的**每个**像素，我们都需要预测其类别（*背景*是其中的一个类别）。有两种主要的分割算法：

- **语义分割**只提供像素类别，不区分同一类别的不同对象
- **实例分割**将类别分为不同的实例。

对于实例分割，这些羊是不同的对象，但对于语义分割，所有的羊都被表示为一个类别。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/instance_vs_semantic.jpeg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/instance_vs_semantic.jpeg)

> 图片来自[此出版物](https://arxiv.org/pdf/2001.05566.pdf)

用于分割的不同神经架构，但它们都具有相同的结构。在某种程度上，它类似于你之前了解过的自动编码器，但我们的目标不是解构原始图像，而是解构掩码。因此，分割网络具有以下部分：

- 编码器从输入图像中提取特征
- 解码器将这些特征转换为掩码图像，其大小和通道数与类别的数量相对应。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/segm.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/segm.png)

> 图片来自[此出版物](https://arxiv.org/pdf/2001.05566.pdf)

我们应该特别提到用于分割的损失函数。当使用传统的自动编码器时，我们需要测量两个图像之间的相似性，我们可以使用均方误差(MSE)来做到这一点。在分割中，目标掩码图像中的每个像素代表类别编号（沿第三维进行一位热编码），所以我们需要使用特定于分类的损失函数 - 对所有像素求平均的交叉熵损失。如果掩码是二元的 - 使用**二元交叉熵损失**（BCE）。

> ✅ 一位热编码是一种将类标签编码到等于类数量的向量长度的方法。看看关于这种技术的[这篇文章](https://datagy.io/sklearn-one-hot-encode/)。



## 医学影像分割



在这节课中，我们将通过训练网络识别人体痣（也被称为痣）在医学图像上的行为来看到分割实践。我们将使用<a href="https://www.fc.up.pt/addi/ph2%20database.html">PH<sup>2</sup>数据库</a>作为图像来源。这个数据集包含200张三类的图片：典型的痣、不典型的痣和黑色素瘤。所有的图像都包括一个对应的**掩码**，用于勾勒痣的轮廓。

> ✅ 这种技术特别适合这种类型的医学成像，但你还能想象其他什么真实世界的应用场景呢？

[![navi](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/navi.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/navi.png)

> 图片来自PH<sup>2</sup>数据库

我们将训练一个模型，将任何痣从其背景中分割出来。

## ✍️ 练习：语义分割



打开以下 Notebook，以了解有关不同语义分割架构的更多信息，练习使用它们，并查看它们在实际中的应用。

- [语义分割 Pytorch](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/SemanticSegmentationPytorch.ipynb)
- [语义分割 TensorFlow](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/SemanticSegmentationTF.ipynb)

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/212)



##  结论



分割是一种非常强大的图像分类技术，它超越了边界框，达到了像素级分类。它是一种用于医学成像和其他应用的技术。



##  🚀 挑战



身体分割只是我们可以用人物图像完成的常见任务之一。其他重要的任务包括**骨骼检测**和**姿态检测**。尝试使用[OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose)库，了解如何使用姿势检测。



##  复习与自学



维基百科上的[维基百科文章](https://wikipedia.org/wiki/Image_segmentation)很好地概述了该技术的各种应用。在此研究领域中，自行了解实例分割和全景分割的子域。



## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/lab/README.md)



在这个实验中，尝试使用Kaggle上的[全身MADS数据集](https://www.kaggle.com/datasets/tapakah68/segmentation-full-body-mads-dataset)来进行**人体分割**。
