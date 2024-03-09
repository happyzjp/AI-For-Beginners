# 目标检测



到目前为止，我们处理的图像分类模型获取图像并产生分类结果，例如 MNIST 问题中的“数字”类别。但是，在许多情况下，我们不仅想知道图片描绘了哪些物体，还希望能够确定它们的确切位置。这正是目标检测的意义所在。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/111)



[![Object Detection](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/Screen_Shot_2016-11-17_at_11.14.54_AM.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/Screen_Shot_2016-11-17_at_11.14.54_AM.png)

> 来自 YOLO v2 网站的图片

## 目标检测的一种朴素方法



假设我们想在图片中找到一只猫，那么一种非常朴素的目标检测方法如下：

1. 将图片分解为多个图块
2. 对每个图块运行图像分类。
3. 激活度足够高的图块可以被认为包含目标物体。

[![Naive Object Detection](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/naive-detection.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/naive-detection.png)

> *来自练习笔记本的图片*

但是，这种方法远非理想，因为它只允许算法非常不精确地定位对象的边界框。为了更精确地定位，我们需要运行某种回归来预测边界框的坐标，为此，我们需要特定的数据集。

## 目标检测回归



这篇博文对形状检测有一个很好的温和介绍。

## 目标检测数据集



您可能会遇到以下数据集：

- PASCAL VOC - 20 类
- COCO - 上下文中常见的对象。80 个类别、边界框和分割蒙版

[![COCO](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/coco-examples.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/coco-examples.jpg)

##  目标检测指标



###  交并比



对于图像分类，很容易衡量算法的执行情况，对于对象检测，我们需要衡量类的正确性以及推断出的边界框位置的精度。对于后者，我们使用所谓的交并比 (IoU)，它衡量两个框（或两个任意区域）的重叠程度。

[![IoU](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/iou_equation.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/iou_equation.png)

> *来自这篇关于 IoU 的优秀博文中的图 2*

这个想法很简单 - 我们将两个图形的交集区域除以它们的并集区域。对于两个相同的区域，IoU 将为 1，而对于完全不相交的区域，它将为 0。否则它将在 0 到 1 之间变化。我们通常只考虑 IoU 超过某个值的那些边界框。

###  平均准确率



假设我们要衡量给定类别的对象 � 的识别程度。为了衡量它，我们使用平均精度指标，其计算方式如下：

1. 考虑精确度-召回率曲线显示了根据检测阈值（从 0 到 1）的准确度。
2. 根据阈值，我们将在图像中检测到或多或少的对象，以及不同的精确度和召回率值。
3. 曲线将如下所示：

[![img](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecall.png)](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecall.png)

>  *来自 NeuroWorkshop 的图像*

给定类别 � 的平均精度是该曲线下的面积。更准确地说，召回轴通常分为 10 部分，并且精度在所有这些点上取平均值：

��=111∑�=010Precision(Recall=�10)

###  AP 和 IoU



我们只考虑那些 IoU 高于某个值的检测。例如，在 PASCAL VOC 数据集中通常假设 IoU Threshold=0.5 ，而在 COCO 中，AP 是针对 IoU Threshold 的不同值进行测量的。

[![img](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecallIoU.png)](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecallIoU.png)

>  *来自 NeuroWorkshop 的图像*

### 平均准确率 - mAP



目标检测的主要指标称为平均平均精度或 mAP。它是平均精度值，平均分布在所有对象类别中，有时也分布在 IoU Threshold 中。更详细地说，计算 mAP 的过程在此博客文章中进行了描述，并且还提供了代码示例。

## 不同的目标检测方法



目标检测算法有两大类：

- 区域提议网络 (R-CNN、Fast R-CNN、Faster R-CNN)。其主要思想是生成感兴趣区域 (ROI)，并在其上运行 CNN，寻找最大激活。这与朴素方法有点类似，但不同之处在于 ROI 的生成方式更巧妙。这种方法的主要缺点之一是速度慢，因为我们需要对图像进行多次 CNN 分类器传递。
- 单次传递 (YOLO、SSD、RetinaNet) 方法。在这些架构中，我们设计网络以一次预测类别和 ROI。

###  R-CNN：基于区域的 CNN



R-CNN 使用选择性搜索来生成 ROI 区域的层次结构，然后通过 CNN 特征提取器和 SVM 分类器来确定对象类别，并通过线性回归来确定边界框坐标。官方论文

[![RCNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn1.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn1.png)

> *来自 van de Sande 等人的图像 ICCV’11*

[![RCNN-1](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn2.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn2.png)

>  *此博客的图片

###  F-RCNN - 快速 R-CNN



这种方法类似于 R-CNN，但区域是在应用卷积层之后定义的。

[![FRCNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/f-rcnn.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/f-rcnn.png)

> 来自官方论文，arXiv，2015

### Faster R-CNN



这种方法的主要思想是使用神经网络来预测 ROI，即所谓的区域提议网络。论文，2016

[![FasterRCNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/faster-rcnn.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/faster-rcnn.png)

> 来自官方论文的图片

### R-FCN：基于区域的全卷积网络



该算法甚至比 Faster R-CNN 还要快。主要思想如下：

1. 我们使用 ResNet-101 提取特征
2. 特征由位置敏感评分图处理。来自 � 类的每个对象由 �×� 个区域划分，我们正在训练以预测对象的各个部分。
3. 对于来自 �×� 区域的每个部分，所有网络都对目标类别进行投票，并选择获得最多票的目标类别。

[![r-fcn image](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/r-fcn.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/r-fcn.png)

> 来自官方论文的图片

### YOLO - 只看一次



YOLO 是一种实时单次算法。其主要思想如下：

- 图像被划分为 �×� 个区域
- 对于每个区域，CNN预测 � 个可能的物体、边界框坐标和置信度=概率*IoU。

[![YOLO](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/yolo.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/yolo.png)

> 来自官方论文的图片

###  其他算法



- RetinaNet：官方论文
  - [PyTorch 在 Torchvision 中的实现](https://pytorch.org/vision/stable/_modules/torchvision/models/detection/retinanet.html)
  - [ Keras 实现](https://github.com/fizyr/keras-retinanet)
  - Keras 样本中的 RetinaNet 目标检测
- SSD（单次检测器）：官方论文

## ✍️ 练习：目标检测



在以下笔记本中继续学习：

[ObjectDetection.ipynb](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/ObjectDetection.ipynb)

##  结论



在本课程中，您对对象检测的所有不同方法进行了旋风般的游览！

##  🚀 挑战



阅读有关 YOLO 的这些文章和笔记本，并亲自尝试一下

- 一篇描述 YOLO 的好博客文章
- [ 官方网站](https://pjreddie.com/darknet/yolo/)
- Yolo：Keras 实现，逐步笔记本
- Yolo v2：Keras 实现，逐步笔记本

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/211)



##  复习与自学



- Nikhil Sardana 的目标检测
- [目标检测算法的良好比较](https://lilianweng.github.io/lil-log/2018/12/27/object-detection-part-4.html)
- [深度学习目标检测算法综述](https://medium.com/comet-app/review-of-deep-learning-algorithms-for-object-detection-c1f3d437b852)
- [基本目标检测算法的分步介绍](https://www.analyticsvidhya.com/blog/2018/10/a-step-by-step-introduction-to-the-basic-object-detection-algorithms-part-1/)
- [Faster R-CNN在Python中的目标检测实现](https://www.analyticsvidhya.com/blog/2018/11/implementation-faster-r-cnn-python-object-detection/)

## [作业：目标检测](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/lab/README.md)
