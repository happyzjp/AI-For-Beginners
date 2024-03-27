# 目标检测



到目前为止，我们处理的图像分类模型获取图像并产生分类结果，例如 MNIST 问题中的“数字”类别。但是，在许多情况下，我们不仅想知道图片描绘了哪些物体，还希望能够确定它们的确切位置。这正是**目标检测**的意义所在。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/111)



[![Object Detection](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/Screen_Shot_2016-11-17_at_11.14.54_AM.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/Screen_Shot_2016-11-17_at_11.14.54_AM.png)

> 图片来自[YOLO v2官网](https://pjreddie.com/darknet/yolov2/)

## 目标检测的初级方法



假设我们想在图片中找到一只猫，那么一种非常初级的目标检测方法如下：

1. 将图片分解成一系列的块
2. 对每个图块运行图像分类。
3. 激活度足够高的图块可以被认为包含目标物体。

[![Naive Object Detection](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/naive-detection.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/naive-detection.png)

> *图片来自[NoteBook](https://github.com/happyzjp/AI-For-Beginners/tree/main/lessons/4-ComputerVision/11-ObjectDetection/ObjectDetection.ipynb)*

然而，这种方法远非理想，因为它只允许算法非常不精确地定位对象的边界框。为了更精确地定位，我们需要运行某种**回归**来预测边界框的坐标，为此，我们需要特定的数据集。



## 目标检测回归

[这篇博客文章](https://towardsdatascience.com/object-detection-with-neural-networks-a4e2c46b4491)对于形状检测提供了一个很好的入门介绍。

## 目标检测数据集



你可能会在这项任务中遇到以下的数据集：

- [PASCAL VOC](http://host.robots.ox.ac.uk/pascal/VOC/) - 20个类别
- [COCO](http://cocodataset.org/#home) - 上下文中的常见对象。80个类别，边界框和分割蒙版

[![COCO](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/coco-examples.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/coco-examples.jpg)

##  目标检测指标



###  交并比



对于图像分类来说，衡量算法性能的好坏很容易，但在对象检测中，我们需要衡量类别的准确性以及推断边界框位置的精确性。对于后者，我们采用所谓的 **交并比** (IoU)，它用于衡量两个框（或两个任意区域）重叠的程度。

[![IoU](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/iou_equation.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/iou_equation.png)

> *图片2 来自[这篇出色的关于IoU的博客文章](https://pyimagesearch.com/2016/11/07/intersection-over-union-iou-for-object-detection/)*

这个概念很简单 - 我们将两个图形的交集区域除以它们的并集区域。对于两个相同的区域，IoU 将为 1，而对于完全不相交的区域，它将为 0。否则它将在 0 到 1 之间变化。我们通常只考虑 IoU 超过某个值的那些边界框。



###  平均准确率



假设我们要衡量给定类别的对象  $C$  的识别程度。为了衡量它，我们使用平均精度指标，其计算方式如下：

假设我们想衡量给定对象类别的识别效果。要衡量这个效果，我们使用**平均精度**指标，其计算步骤如下：

1. 考虑精确度-召回率曲线显示了根据检测阈值（从 0 到 1）的准确度。
2. 根据阈值的不同，我们将在图像中检测到更多或更少的对象，从而得到不同的精度和召回率值
3. 曲线将如下所示：

[![img](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecall.png)](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecall.png)

>  *图片来源：[NeuroWorkshop](http://github.com/shwars/NeuroWorkshop)*

给定类别 $C$  的平均精度是该曲线下的面积。更准确地说，召回轴通常分为 10 部分，并且精度在所有这些点上取平均值：

$$AP = {1\over11}\sum_{i=0}^{10}\mbox{Precision}(\mbox{Recall}={i\over10})
$$

给定类别的平均精度是这条曲线下的面积。更准确地说，召回率轴通常被分成10个部分，精确度在这些点上取平均值：

精度召回率



###  AP 和 IoU



我们只考虑那些 IoU 高于某个值的检测。例如，在 PASCAL VOC 数据集中通常假设 $\mbox{IoU 阈值} = 0.5$  ，而在 COCO 中，AP 是针对 $\mbox{IoU 阈值}$ 的不同值进行测量的。

我们只考虑那些IoU值超过某个特定值的检测。例如，在PASCAL VOC数据集中，通常假定阈值，而在COCO数据集中，AP是针对不同的阈值进行测量的。

[![img](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecallIoU.png)](https://github.com/shwars/NeuroWorkshop/raw/master/images/ObjDetectionPrecisionRecallIoU.png)

>  *图片来源：[NeuroWorkshop](http://github.com/shwars/NeuroWorkshop)*

### 平均准确率 - mAP



目标检测的主要指标被称为**平均精度均值**，或者**mAP**。它是平均精度的值，平均计算在所有物体类别上，有时也计算在阈值上。更详细的，计算**mAP**的过程可在 [这个博客文章](https://medium.com/@timothycarlen/understanding-the-map-evaluation-metric-for-object-detection-a07fe6962cf3)和 [这里包含代码示例](https://gist.github.com/tarlen5/008809c3decf19313de216b9208f3734)中查找。

## 不同的目标检测方法



目标检测算法可以分为两大类：

- **区域建议网络** (R-CNN，快速R-CNN，更快的R-CNN)。其主要思想是生成**感兴趣区域** (ROI)，然后在其上运行CNN以寻找最大激活值。这与简单方法类似，只不过ROI的生成方式更智能。这类方法的主要缺点之一是它们运行缓慢，因为我们需要通过CNN分类器多次扫描图像。

- **单次传递** (YOLO，SSD，RetinaNet)方法。在这些架构中，我们设计网络在一次通过中预测类别和ROI。

  

###  R-CNN：基于区域的 CNN



[R-CNN](http://islab.ulsan.ac.kr/files/announcement/513/rcnn_pami.pdf) 使用 [选择性搜索](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)生成 ROI 区域的分层结构，然后通过 CNN 特征提取器和 SVM 分类器进行计算以确定对象类别，并使用线性回归来确定 *边框* 坐标。[官方论文](https://arxiv.org/pdf/1506.01497v1.pdf)

[![RCNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn1.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn1.png)

> *来自 van de Sande 等人的图像 ICCV’11*

[![RCNN-1](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn2.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/rcnn2.png)

>  *图片来源：[此博客](https://towardsdatascience.com/r-cnn-fast-r-cnn-faster-r-cnn-yolo-object-detection-algorithms-36d53571365e)*

###  F-RCNN - 快速 R-CNN



这种方法类似于 R-CNN，但区域是在应用卷积层之后定义的。

[![FRCNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/f-rcnn.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/f-rcnn.png)

> 图片来源：[官方论文](https://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Girshick_Fast_R-CNN_ICCV_2015_paper.pdf)，[arXiv](https://arxiv.org/pdf/1504.08083.pdf)，2015年

### Faster R-CNN



这种方法的主要思想是使用神经网络来预测ROI，也就是所谓的*区域提议网络*。[论文](https://arxiv.org/pdf/1506.01497.pdf)，2016年

[![FasterRCNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/faster-rcnn.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/faster-rcnn.png)

> 来自[官方论文](https://arxiv.org/pdf/1506.01497.pdf)的图像

### R-FCN：基于区域的全卷积网络



该算法甚至比 Faster R-CNN 还要快。主要思想如下：

1. 我们使用 ResNet-101 提取特征
2. 特征由位置敏感评分图处理。来自 $C$ 类的每个对象由 $k\times k$ 个区域划分，我们正在训练以预测对象的各个部分。
3. 对于来自 $k\times k$ 区域的每个部分，所有网络都对目标类别进行投票，并选择获得最多票的目标类别。

[![r-fcn image](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/r-fcn.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/r-fcn.png)

> 来自[官方论文](https://arxiv.org/abs/1605.06409)的图像

### YOLO - 单次算法



YOLO 是一种实时单次算法。其主要思想如下：

- 图像被划分为  $S\times S$ 个区域
- 对于每个区域，CNN 预测  $$n$  个可能的物体、边界框坐标和置信度=概率*IoU。

[![YOLO](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/yolo.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/images/yolo.png)

> 来自[官方论文](https://arxiv.org/abs/1506.02640)的图像

###  其他算法



- RetinaNet:
  
  官方论文
  
  - [PyTorch在Torchvision的实现](https://pytorch.org/vision/stable/_modules/torchvision/models/detection/retinanet.html)
  - [Keras的实现](https://github.com/fizyr/keras-retinanet)
  - [在Keras样本中使用RetinaNet进行目标检测](https://keras.io/examples/vision/retinanet/)
- SSD（单击检测器）：[官方论文](https://arxiv.org/abs/1512.02325)



## ✍️ 练习：目标检测



在以下 NoteBook 中继续学习：

[ObjectDetection.ipynb](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/ObjectDetection.ipynb)

##  结论



在本课中，你粗略地了解了完成目标检测的各种方法

##  🚀 挑战



阅读有关 YOLO 的这些文章和 Notebook，并亲自尝试一下

- [一篇很好的博客文章](https://www.analyticsvidhya.com/blog/2018/12/practical-guide-object-detection-yolo-framewor-python/) 描述了YOLO
- [官方网站](https://pjreddie.com/darknet/yolo/)
- Yolo: [Keras实现](https://github.com/experiencor/keras-yolo2), [一步一步的NoteBook](https://github.com/experiencor/basic-yolo-keras/blob/master/Yolo Step-by-Step.ipynb)
- Yolo v2: [Keras实现](https://github.com/experiencor/keras-yolo2), [一步一步的NoteBook](https://github.com/experiencor/keras-yolo2/blob/master/Yolo Step-by-Step.ipynb)

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/211)



## 复习与自学

- [目标检测](https://tjmachinelearning.com/lectures/1718/obj/) 由 Nikhil Sardana提供
- [对目标检测算法的良好比较](https://lilianweng.github.io/lil-log/2018/12/27/object-detection-part-4.html)
- [对目标检测的深度学习算法的综述](https://medium.com/comet-app/review-of-deep-learning-algorithms-for-object-detection-c1f3d437b852)
- [对基本目标检测算法的逐步介绍](https://www.analyticsvidhya.com/blog/2018/10/a-step-by-step-introduction-to-the-basic-object-detection-algorithms-part-1/)
- [Python中实现的Faster R-CNN进行目标检测](https://www.analyticsvidhya.com/blog/2018/11/implementation-faster-r-cnn-python-object-detection/)



## [作业：目标检测](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/11-ObjectDetection/lab/README.md)
