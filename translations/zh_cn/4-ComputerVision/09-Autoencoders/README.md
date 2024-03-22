# 自动编码器



在训练卷积神经网络（CNNs）时，我们面临的一个问题是需要大量的标注数据。在图像分类的情况下，我们需要将图像分成不同的类别，这是一项手工劳动。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/109)



然而，在训练CNN特征提取器时，我们可能希望使用原始的（未标注的）数据，这称为**自监督学习**。与其使用标签，不如使用训练图像作为网络的输入和输出。**自编码器**的主要思想是，我们将有一个**编码器网络**，它将输入图像转换成某种**潜在空间**（通常只是一个较小尺寸的向量），接着是**解码器网络**，其目标是重构原始图像。

> ✅ [自编码器](https://wikipedia.org/wiki/Autoencoder)是“一种用于学习未标注数据有效编码的人工神经网络。

由于我们训练自编码器捕捉尽可能多的原始图像信息以进行精确重构，网络尝试找到最佳的**嵌入**来捕获输入图像的含义。

[![AutoEncoder Diagram](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/autoencoder_schema.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/autoencoder_schema.jpg)

>  来自 Keras 博客的图片

## 自动编码器的使用场景



虽然重建原始图像本身看起来没什么用，但在以下几种情况下自动编码器特别有用：

- **降维图像用于可视化**或**训练图像嵌入**。通常情况下，自编码器比PCA（主成分分析）提供更好的结果，因为它考虑了图像的空间特性以及分层特征。
- **去噪**，即去除图像中的噪声。因为噪声携带了大量无用信息，自动编码器无法将其全部拟合进相对较小的潜在空间中，因此，它只捕获图像的重要部分。在训练去噪器时，我们从原始图像开始，并使用人为添加噪声的图像作为自编码器的输入。
- **超分辨率**，提高图像分辨率。我们从高分辨率图像开始，并将低分辨率图像用作自动编码器的输入。
- **生成模型**，一旦我们训练了自动编码器，就可以使用解码器部分从随机潜在向量开始创建新对象。

## 变分自动编码器 (VAE)



传统的自动编码器以某种方式降低输入数据的维度，找出输入图像的重要特征。然而，潜在向量通常没有多大意义。换句话说，以 MNIST 数据集为例，找出哪些数字对应不同的潜在向量并不是一项容易的任务，因为接近的潜在向量不一定对应相同的数字。

另一方面，为了训练生成模型，最好对潜在空间有一定的了解。这个想法引导我们到**变分自动编码器 **(VAE)。

VAE 是学习预测潜在参数的统计分布（即**潜在分布**）的自动编码器。例如，我们可能希望潜在向量以某种均值 z<sub>mean</sub> 和标准差 z<sub>sigma</sub>正态分布（均值和标准差都是某个维度 d 的向量）。VAE 中的编码器会预测这些参数，然后解码器从该分布中获取一个随机向量来重建对象。

 总结：

- 从输入向量中，我们预测 `z_mean` 和 `z_log_sigma` （我们不预测标准差本身，而是预测其对数）
- 我们从分布 N(z mean ,exp(z<sub>log_sigma</sub>)) 中采样一个向量 `sample`
- 解码器尝试使用`sample`作为输入向量解码原始图像

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/vae.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/vae.png)

> 图片来自Isaak Dykeman的[这篇博客文章](https://ijdykeman.github.io/ml/2016/12/21/cvae.html)

变分自动编码器使用一个由两部分组成的复杂损失函数：

- **重构损失**（Reconstruction loss）是一个损失函数，用来显示重构的图像与目标图像的接近程度（可以使用均方误差，即MSE）。这与普通自编码器中使用的损失函数是相同的。
- **KL损失**（KL loss），确保潜在变量分布接近于正态分布。它基于[Kullback-Leibler散度](https://www.countbayesie.com/blog/2017/5/9/kullback-leibler-divergence-explained)的概念——一种度量两个统计分布相似程度的指标。

变分自动编码器的一个重要优势在于，它们允许我们相对容易地生成新图像，因为我们知道从哪个分布中对潜在向量进行采样。例如，如果我们在 MNIST 上使用 2D 潜在向量训练 VAE，那么我们就可以改变潜在向量的分量来获得不同的数字：

[![vaemnist](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/vaemnist.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/vaemnist.png)

> 图片由[Dmitry Soshnikov](http://soshnikov.com/)提供

观察图像如何相互融合，因为我们开始从潜在参数空间的不同部分获取潜在向量。我们还可以在 2D 中可视化此空间：

[![vaemnist cluster](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/vaemnist-diag.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/images/vaemnist-diag.png)

> 图片由 Dmitry Soshnikov 拍摄

## ✍️ 练习：自动编码器



在这些相应的 notebooks 中了解有关自动编码器的更多信息：

- [TensorFlow 中的自动编码器](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/AutoencodersTF.ipynb)
- [ PyTorch 中的自动编码器](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/09-Autoencoders/AutoEncodersPyTorch.ipynb)

## 自动编码器的特性



- **数据特异性** - 它们仅适用于经过训练的图像类型。例如，如果我们在花朵上训练超分辨率网络，它将无法很好地处理人像。这是因为网络可以通过从训练数据集中学习到的特征中获取精细的细节来生成更高分辨率的图像。
- **有损** - 重建的图像与原始图像不同。损失的性质由训练期间使用的损失函数定义
-  可以处理**无标签数据**

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/209)



##  结论



在本课中，你了解了人工智能科学家可用的各种类型的自动编码器。学习了如何构建它们以及如何使用它们来重建图像。还了解了 VAE 以及如何使用它来生成新图像。



##  🚀 挑战



在本课中，你学习了如何将自动编码器用于图像。但它们也可以用于音乐！查看 Magenta 项目的  [MusicVAE](https://magenta.tensorflow.org/music-vae)  项目，该项目使用自动编码器来学习重建音乐。使用此库进行一些[实验](https://colab.research.google.com/github/magenta/magenta-demos/blob/master/colab-notebooks/Multitrack_MusicVAE.ipynb)，看看你可以创建什么。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/208)



##  复习与自学



参考以下资源以了解更多关于自动编码器的信息：

- [在 Keras 中构建自动编码器](https://blog.keras.io/building-autoencoders-in-keras.html)
- [NeuroHive 博客文章](https://neurohive.io/ru/osnovy-data-science/variacionnyj-avtojenkoder-vae/)
- [变分自动编码器详解](https://kvfrans.com/variational-autoencoders-explained/)
- [条件变分自动编码器](https://ijdykeman.github.io/ml/2016/12/21/cvae.html)

##  作业



在使用 TensorFlow 的这个 [notebooks](https://github.com/happyzjp/AI-For-Beginners/edit/main/lessons/4-ComputerVision/09-Autoencoders/AutoencodersTF.ipynb) 的最后，你会找到一个“任务” - 将其作为你的作业。