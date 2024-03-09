# 计算机视觉简介



计算机视觉是一门学科，其目标是让计算机对数字图像获得高级理解。这是一个相当广泛的定义，因为理解可以意味着许多不同的事情，包括在图片上找到一个物体（物体检测）、理解正在发生的事情（事件检测）、用文本描述图片或以 3D 重建场景。还有一些与人像相关的特殊任务：年龄和情绪估计、人脸检测和识别以及 3D 姿态估计，仅举几例。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/106)



计算机视觉最简单的任务之一是图像分类。

计算机视觉通常被认为是人工智能的一个分支。如今，大多数计算机视觉任务都是使用神经网络解决的。在本节中，我们将详细了解用于计算机视觉的特殊类型的神经网络，卷积神经网络。

但是，在将图像传递给神经网络之前，在许多情况下，使用一些算法技术来增强图像是有意义的。

有几个 Python 库可用于图像处理：

- imageio 可用于读取/写入不同的图像格式。它还支持 ffmpeg，一个将视频帧转换为图像的有用工具。
- Pillow（也称为 PIL）功能更强大，还支持一些图像处理，例如变形、调色板调整等。
- OpenCV 是一个用 C++ 编写的强大图像处理库，已成为图像处理的事实标准。它有一个方便的 Python 接口。
- dlib 是一个 C++ 库，实现了许多机器学习算法，包括一些计算机视觉算法。它也有一个 Python 接口，可用于执行诸如人脸和面部特征检测等具有挑战性的任务。

## OpenCV



OpenCV 被认为是图像处理的实际标准。它包含许多有用的算法，用 C++ 实现。您也可以从 Python 调用 OpenCV。

学习 OpenCV 的一个好地方是这门 OpenCV 课程。在我们的课程中，我们的目标不是学习 OpenCV，而是向您展示一些可以使用 OpenCV 的示例，以及如何使用它。

###  正在加载图片



在 Python 中，图像可以用 NumPy 数组方便地表示。例如，大小为 320x200 像素的灰度图像将存储在 200x320 数组中，而相同尺寸的彩色图像将具有 200x320x3 的形状（对于 3 个颜色通道）。要加载图像，可以使用以下代码：

```
import cv2
import matplotlib.pyplot as plt

im = cv2.imread('image.jpeg')
plt.imshow(im)
```



传统上，OpenCV 使用 BGR（蓝-绿-红）编码来处理彩色图像，而其他 Python 工具则使用更传统的 RGB（红-绿-蓝）编码。为了使图像看起来正确，您需要将其转换为 RGB 色彩空间，方法是交换 NumPy 数组中的维度或调用 OpenCV 函数：

```
im = cv2.cvtColor(im,cv2.COLOR_BGR2RGB)
```



相同的 `cvtColor` 函数可用于执行其他颜色空间转换，例如将图像转换为灰度或 HSV（色相-饱和度-值）颜色空间。

您还可以使用 OpenCV 逐帧加载视频 - 练习 OpenCV Notebook 中给出了一个示例。

###  图像处理



在将图像输入神经网络之前，您可能希望应用几个预处理步骤。OpenCV 可以做很多事情，包括：

- 使用 `im = cv2.resize(im, (320,200),interpolation=cv2.INTER_LANCZOS)` 调整图像大小
- 使用 `im = cv2.medianBlur(im,3)` 或 `im = cv2.GaussianBlur(im, (3,3), 0)` 模糊图像
- 如 Stackoverflow 注释中所述，可以通过 NumPy 数组操作来更改图像的亮度和对比度。
- 使用阈值化通过调用 `cv2.threshold` / `cv2.adaptiveThreshold` 函数，这通常比调整亮度或对比度更可取。
- 对图像应用不同的变换：
  - 仿射变换在需要对图像进行旋转、缩放和倾斜，并且知道图像中三个点的源位置和目标位置时很有用。仿射变换保持平行线平行。
  - 透视变换在知道图像中 4 个点的源位置和目标位置时很有用。例如，如果您从某个角度通过智能手机摄像头拍摄矩形文档的照片，并且您想制作文档本身的矩形图像。
- 利用光流理解图像内的运动。

## 计算机视觉的应用示例



在我们的 OpenCV 笔记本中，我们给出了计算机视觉可用于执行特定任务的一些示例：

- 对盲文书的照片进行预处理。我们重点介绍如何使用阈值处理、特征检测、透视变换和 NumPy 操作来分离单个盲文符号，以便神经网络进一步分类。

| [![Braille Image](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/data/braille.jpeg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/data/braille.jpeg) | [![Braille Image Pre-processed](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-result.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-result.png) | [![Braille Symbols](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-symbols.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/braille-symbols.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |                                                              |

>  来自 OpenCV.ipynb 的图像

- 使用帧差检测视频中的运动。如果摄像机是固定的，那么来自摄像机馈送的帧应该彼此非常相似。由于帧表示为数组，只需减去两个后续帧的那些数组，我们就可以得到像素差，对于静态帧，像素差应该很低，而一旦图像中有实质性运动，像素差就会变高。

[![Image of video frames and frame differences](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/frame-difference.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/frame-difference.png)

>  来自 OpenCV.ipynb 的图像

- 使用光流检测运动。光流使我们能够了解视频帧上的各个像素如何移动。光流有两种类型：
  - 稠密光流计算出向量场，该向量场显示每个像素的移动位置
  - 稀疏光流基于获取图像中的一些显著特征（例如边缘），并构建它们从一帧到另一帧的轨迹。

[![Image of Optical Flow](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/optical.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/optical.png)

>  来自 OpenCV.ipynb 的图像

## ✍️ 示例笔记本：OpenCV 在实践中尝试 OpenCV



让我们通过探索 OpenCV Notebook 来做一些 OpenCV 实验

##  结论



有时，相对复杂的任务（例如运动检测或指尖检测）可以通过计算机视觉来解决。因此，了解计算机视觉的基本技术以及 OpenCV 等库可以做什么非常有帮助。

##  🚀 挑战



观看 AI 秀的这段视频，了解 Cortic Tigers 项目以及他们如何构建基于块的解决方案，通过机器人实现计算机视觉任务的民主化。对其他类似项目进行一些研究，帮助新学习者进入该领域。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/206)



##  复习与自学



阅读本精彩教程，了解更多有关光流的信息。

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/lab/README.md)



在本实验中，您将使用简单的姿势拍摄视频，您的目标是使用光流提取向上/向下/向左/向右的运动。

[![Palm Movement Frame](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/palm-movement.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/06-IntroCV/images/palm-movement.png)