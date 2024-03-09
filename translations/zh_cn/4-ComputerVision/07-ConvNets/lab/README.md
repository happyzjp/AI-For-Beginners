人工智能初学者课程的实验作业。

##  任务



想象一下，你需要为宠物育婴室开发一个应用程序来编目所有宠物。此类应用程序的一大特点是能够从照片中自动识别品种。这可以通过神经网络成功完成。

你需要训练一个卷积神经网络来使用宠物面部数据集对不同品种的猫和狗进行分类。

##  数据集



我们将使用源自牛津-IIIT 宠物数据集的宠物面孔数据集。它包含 35 种不同的狗和猫品种。

[![Dataset we will deal with](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/07-ConvNets/lab/images/data.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/07-ConvNets/lab/images/data.png)

要下载数据集，请使用此代码片段：

```
!wget https://mslearntensorflowlp.blob.core.windows.net/data/petfaces.tar.gz
!tar xfz petfaces.tar.gz
!rm petfaces.tar.gz
```



##  说明笔记本



通过打开 PetFaces.ipynb 启动实验

##  外卖



您从头解决了图像分类的一个相对复杂的问题！有相当多的类别，您仍然能够获得合理的准确度！测量 top-k 准确度也很有意义，因为即使对于人类来说，也很容易混淆一些差异不明显的类别。
