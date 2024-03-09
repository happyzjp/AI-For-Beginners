# 分词



我们之前已经了解了目标检测，它允许我们通过预测目标的边界框来定位图像中的目标。然而，对于某些任务，我们不仅需要边界框，还需要更精确的目标定位。此任务称为分割。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/112)



分割可以看作是像素分类，而对于图像的每个像素，我们必须预测其类别（背景是类别之一）。有两种主要的分割算法：

- 语义分割仅告诉像素类别，并且不会区分同一类别的不同对象
- **Instance segmentation** divides classes into different instances. 重试 错误原因

对于实例分割，这些羊是不同的对象，但对于语义分割，所有羊都由一个类别表示。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/instance_vs_semantic.jpeg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/instance_vs_semantic.jpeg)

> 来自此博客文章的图片

用于分割的不同神经架构，但它们都具有相同的结构。在某种程度上，它类似于你之前了解过的自动编码器，但我们的目标不是解构原始图像，而是解构掩码。因此，分割网络具有以下部分：

- 编码器从输入图像中提取特征
- 解码器将这些特征转换为掩码图像，其大小和通道数与类别的数量相对应。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/segm.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/segm.png)

> 来自此出版物的图片

我们应该特别提到用于分割的损失函数。在使用经典自动编码器时，我们需要测量两幅图像之间的相似性，我们可以使用均方误差 (MSE) 来做到这一点。在分割中，目标掩码图像中的每个像素都表示类号（沿第三维进行独热编码），因此我们需要使用特定于分类的损失函数 - 交叉熵损失，平均所有像素。如果掩码是二进制的 - 使用二进制交叉熵损失 (BCE)。

> ✅ 独热编码是一种将类标签编码为长度等于类数的向量的编码方式。看看这篇关于此技术的文章。

## 医学影像分割



在本课程中，我们将通过训练网络识别医学图像上的人类痣（也称为痣）来了解分割的实际应用。我们将使用 PH 2 皮肤镜图像数据库作为图像源。此数据集包含 200 张三类图像：典型痣、非典型痣和黑色素瘤。所有图像还包含一个相应的轮廓线，勾勒出痣。

> ✅ 此技术特别适用于此类医学影像，但您还能设想哪些其他实际应用？

[![navi](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/navi.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/images/navi.png)

> 来自 PH 2 数据库的图像

我们将训练一个模型，将任何痣从其背景中分割出来。

## ✍️ 练习：语义分割



打开以下笔记本，以了解有关不同语义分割架构的更多信息，练习使用它们，并查看它们在实际中的应用。

- [语义分割 Pytorch](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/SemanticSegmentationPytorch.ipynb)
- [语义分割 TensorFlow](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/SemanticSegmentationTF.ipynb)

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/212)



##  结论



分割是一种非常强大的图像分类技术，它超越了边界框，达到了像素级分类。它是一种用于医学成像和其他应用的技术。

##  🚀 挑战



身体分割只是我们可以用人物图像完成的常见任务之一。其他重要任务包括骨骼检测和姿势检测。尝试使用 OpenPose 库，了解如何使用姿势检测。

##  复习与自学



维基百科上的这篇文章很好地概述了该技术的各种应用。在此研究领域中，自行了解实例分割和全景分割的子域。

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/12-Segmentation/lab/README.md)



在此实验室中，尝试使用 Kaggle 上的分割全身 MADS 数据集进行人体分割。
