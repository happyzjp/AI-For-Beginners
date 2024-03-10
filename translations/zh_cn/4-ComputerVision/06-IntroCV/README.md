# 计算机视觉简介

[计算机视觉](https://wikipedia.org/wiki/Computer_vision)是一门学科，其目标是让计算机对数字图像获得高级理解。这是一个相当广泛的定义，因为理解可以意味着许多不同的事情，包括在图片上找到一个物体（**目标检测**）、理解正在发生的事情（**事件检测**）、用文本描述图片或以 3D 重建场景。还有一些与人像相关的特殊任务：年龄和情绪估计、人脸检测和识别以及 3D 姿态估计，仅举几例。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/106)

计算机视觉中最简单的任务之一是**图像分类**。

计算机视觉通常被认为是人工智能的一个分支。如今，大多数计算机视觉任务都是使用神经网络解决的。在本节中，我们将详细了解用于计算机视觉的特殊类型的神经网络，[卷积神经网络](https://chat.openai.com/07-ConvNets/README.md)。

但是，在将图像传递给神经网络之前，在许多情况下，使用一些算法技术来增强图像是有意义的。

有几个 Python 库可用于图像处理：

- **[imageio](https://imageio.readthedocs.io/en/stable/)**  可用于读取/写入不同的图像格式。它还支持 ffmpeg，一个将视频帧转换为图像的有用工具。
- **[Pillow](https://pillow.readthedocs.io/en/stable/index.html)**（也称为 PIL）功能更强大，还支持一些图像处理，例如变形、调色板调整等。
- **[OpenCV](https://opencv.org/)** 是一个用 C++ 编写的强大图像处理库，已成为图像处理的事实标准。它有一个方便的 Python 接口。
- **[dlib](http://dlib.net/)**  是一个 C++ 库，实现了许多机器学习算法，包括一些计算机视觉算法。它也有一个 Python 接口，可用于执行诸如人脸和面部特征检测等具有挑战性的任务。

## OpenCV

[OpenCV](https://opencv.org/)  被认为是图像处理的实际标准。它包含许多有用的算法，用 C++ 实现。你也可以从 Python 调用 OpenCV。

学习 OpenCV 的一个好地方是[Learn OpenCV课程](https://learnopencv.com/getting-started-with-opencv/)。在我们的课程中，我们的目标不是学习 OpenCV，而是向您展示一些可以使用 OpenCV 的示例，以及如何使用它。

###  正在加载图片

在 Python 中，图像可以用 NumPy 数组方便地表示。例如，大小为 320x200 像素的灰度图像将存储在 200x320 数组中，而相同尺寸的彩色图像将具有 200x320x3 的形状（对于 3 个颜色通道）。要加载图像，可以使用以下代码：

```
import cv2
import matplotlib.pyplot as plt

im = cv2.imread('image.jpeg')
plt.imshow(im)
```

传统上，OpenCV 使用 BGR（蓝-绿-红）编码来处理彩色图像，而其他 Python 工具则使用更传统的 RGB（红-绿-蓝）编码。为了使图像看起来正确，你需要将其转换为 RGB 色彩空间，方法是交换 NumPy 数组中的维度或调用 OpenCV 函数：

```
im = cv2.cvtColor(im,cv2.COLOR_BGR2RGB)
```

相同的 `cvtColor` 函数可用于执行其他颜色空间转换，例如将图像转换为灰度或 HSV（色相-饱和度-值）颜色空间。

你还可以使用 OpenCV 逐帧加载视频- 示例请参见练习 [OpenCV Notebook](https://chat.openai.com/c/OpenCV.ipynb)。

###  图像处理

在将图像输入神经网络之前，您可能希望应用几个预处理步骤。OpenCV 可以做很多事情，包括：

- 使用 `im = cv2.resize(im, (320,200),interpolation=cv2.INTER_LANCZOS)` 调整图像大小
- 使用 `im = cv2.medianBlur(im,3)` 或 `im = cv2.GaussianBlur(im, (3,3), 0)` 模糊图像
- 通过NumPy数组操作可以改变图像的**亮度和对比度**，具体可参考[这个Stackoverflow帖子](https://stackoverflow.com/questions/39308030/how-do-i-increase-the-contrast-of-an-image-in-python-opencv)。
- 通过调用 `cv2.threshold`/`cv2.adaptiveThreshold` 函数进行[阈值处理](https://docs.opencv.org/4.x/d7/d4d/tutorial_py_thresholding.html)，这通常优于调整亮度或对比度。
- 对图像应用不同的[变换](https://docs.opencv.org/4.5.5/da/d6e/tutorial_py_geometric_transformations.html)：
  - **[仿射变换](https://docs.opencv.org/4.5.5/d4/d61/tutorial_warp_affine.html)** 在需要对图像进行旋转、缩放和倾斜，并且知道图像中三个点的源位置和目标位置时很有用。仿射变换保持平行线平行。
  - **[透视变换](https://medium.com/analytics-vidhya/opencv-perspective-transformation-9edffefb2143)** 在知道图像中 4 个点的源位置和目标位置时很有用。例如，如果您从某个角度通过智能手机摄像头拍摄矩形文档的照片，并且您想制作文档本身的矩形图像。
- 通过使用**[光流法](https://docs.opencv.org/4.5.5/d4/dee/tutorial_optical_flow.html)**来理解图像内的运动。

## 计算机视觉的应用示例

在我们的[OpenCV笔记本](https://chat.openai.com/c/OpenCV.ipynb)中，我们提供了计算机视觉如何执行特定任务的一些示例：

- **预处理盲文书的照片**。我们重点介绍如何使用阈值处理、特征检测、透视变换和 NumPy 操作来分离单个盲文符号，以便神经网络进一步分类。

| [![Braille Image](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/data/braille.jpeg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/data/braille.jpeg) | [![Braille Image Pre-processed](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-result.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-result.png) | [![Braille Symbols](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-symbols.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-symbols.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |                                                              |

>  来自 OpenCV.ipynb 的图像

- **使用帧差异检测视频中的运动**。如果摄像机是固定的，那么来自摄像机馈送的帧应该彼此非常相似。由于帧表示为数组，只需减去两个后续帧的那些数组，我们就可以得到像素差，对于静态帧，像素差应该很低，而一旦图像中有实质性运动，像素差就会变高。

[![Image of video frames and frame differences](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/frame-difference.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/frame-difference.png)

>  来自 OpenCV.ipynb 的图像

- **使用光流检测运动**。 [光流](https://docs.opencv.org/3.4/d4/dee/tutorial_optical_flow.html) 使我们能够了解视频帧上的各个像素如何移动。光流有两种类型：
  - **稠密光流** 计算出向量场，该向量场显示每个像素的移动位置
  - **稀疏光流** 基于获取图像中的一些显著特征（例如边缘），并构建它们从一帧到另一帧的轨迹。

[![Image of Optical Flow](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/optical.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/optical.png)

>  来自 OpenCV.ipynb 的图像

## ✍️ 示例笔记本：OpenCV  [在实践中使用OpenCV](https://chat.openai.com/c/OpenCV.ipynb)

让我们通过探索[OpenCV笔记本](https://chat.openai.com/c/OpenCV.ipynb)来做一些 OpenCV 实验

##  结论

有时，相对复杂的任务（例如运动检测或指尖检测）可以通过计算机视觉来解决。因此，了解计算机视觉的基本技术以及 OpenCV 等库能做什么是非常有帮助的。

##  🚀 挑战

观看[此视频](https://docs.microsoft.com/shows/ai-show/ai-show--2021-opencv-ai-competition--grand-prize-winners--cortic-tigers--episode-32?WT.mc_id=academic-77998-cacaste) ，了解Cortic Tigers项目以及他们如何通过机器人构建基于块的解决方案，以使计算机视觉任务民主化。查找其他类似的项目，这些项目帮助新学习者进入该领域。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/206)

##  复习与自学

在[这个很好的教程中](https://learnopencv.com/optical-flow-in-opencv/)深入了解光流。

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/lab/README.md)

在本实验中，你将使用简单的姿势拍摄视频，你的目标是使用光流提取向上/向下/向左/向右的运动。

[![Palm Movement Frame](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/palm-movement.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/palm-movement.png)