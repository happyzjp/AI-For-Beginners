# 生成对抗网络



在上节中，我们了解了**生成模型**：能够生成与训练数据集中图像类似的新图像的模型。变分自编码器（VAE） 是生成模型的一个很好的例子。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/110)



然而，如果我们试图使用 VAE 生成一些真正有意义的东西，比如一个合理分辨率的画作，我们会看到训练并没有很好地收敛。对于这样的用例，我们应该了解另一种特别针对生成模型的架构——**生成对抗网络**（Generative Adversarial Networks，或 GANs）。

GAN 的主要思想是拥有两个将相互对抗的神经网络：

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/10-GANs/images/gan_architecture.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/10-GANs/images/gan_architecture.png)

> 图片由 [Dmitry Soshnikov](http://soshnikov.com/) 制作

> ✅ 一些词汇：
>
> - **生成器**（Generator）是一个网络，它接收某个随机向量，并生成图像作为结果；
> - **鉴别器**（Discriminator）是一个网络，它接收一个图像，并且它应该判断这是一个真实的图像（来自训练数据集）还是由生成器生成的图像。它本质上是一个图像分类器。。

###  判别器



判别器的架构与普通图像分类网络没有区别。在最简单的情况下，它可以是全连接分类器，但很可能它是一个卷积网络。

> ✅ 基于卷积网络的 GAN 被称为 [DCGAN](https://arxiv.org/pdf/1511.06434.pdf)

一个CNN鉴别器由以下层组成：数个卷积+池化层（空间尺寸逐渐减小），一个或多个全连接层以获得“特征向量”，最终的二进制分类器。

> ✅ 在这个上下文中，“池化”是一种减少图像尺寸的技术。"池化层通过将一个层的神经元群组的输出结合成下一层的单个神经元来减少数据的尺寸。" - [来源](https://wikipedia.org/wiki/Convolutional_neural_network#Pooling_layers)

###  生成器



生成器稍微复杂一些。你可以将其视为反向判别器。从潜在向量（代替特征向量）开始，它有一个全连接层，将其转换为所需的尺寸/形状，然后进行反卷积+上采样。这类似于[自动编码器](https://github.com/happyzjp/AI-For-Beginners/edit/main/lessons/4-ComputerVision/09-Autoencoders/README.md)的解码器部分。

> ✅ 由于卷积层是作为遍历图像的线性滤波器实现的，因此反卷积本质上类似于卷积，并且可以使用相同的层逻辑实现。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/4-ComputerVision/10-GANs/images/gan_arch_detail.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/10-GANs/images/gan_arch_detail.png)

> 图片由 Dmitry Soshnikov 拍摄

###  训练 GAN



GAN 被称为**对抗性**网络，因为生成器和判别器之间存在持续的竞争。在此竞争过程中，生成器和判别器都会得到改进，因此网络学会生成越来越好的图片。

训练分两个阶段进行：

- **训练鉴别器** - 此任务非常简单：我们通过生成器生成一批图像，将它们标记为 0，表示假图像，并从输入数据集（标签为 1，真图像）中获取一批图像。我们获得一些判别器损失，并执行反向传播。
- **训练生成器** - 这稍微有点棘手，因为我们不知道生成器的预期输出。我们采用由生成器和判别器组成的整个 GAN 网络，用一些随机向量对其进行馈送，并期望结果为 1（对应于真图像）。然后，我们冻结判别器的参数（我们不希望它在此步骤中接受训练），并执行反向传播。

在此过程中，生成器和判别器的损失都没有显著下降。在理想情况下，它们应该振荡，对应于两个网络都提高了它们的性能。



##  ✍️ 练习：GAN



- [TensorFlow/Keras 中的 GAN notebooks](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/10-GANs/GANTF.ipynb)
- [PyTorch 中的 GAN notebooks]](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/4-ComputerVision/10-GANs/GANPyTorch.ipynb)

### GAN 训练中的问题



众所周知，GAN的训练特别困难。以下是一些常见的问题：

- **模式崩溃（Mode Collapse）** - 这个术语的意思是生成器学会生成一个成功欺骗鉴别器的图像，而不是生成各种不同的图像。

- **对超参数的敏感性**  - 你经常会看到 GAN 根本不收敛，然后突然降低学习率导致收敛。

- 在生成器和判别器之间保持**平衡** - 在许多情况下，判别器损失可以相对较快地降至零，这导致生成器无法进一步训练。为了克服这个问题，我们可以尝试为生成器和判别器设置不同的学习率，或者在损失已经太低的情况下跳过判别器训练。

- **高分辨率**训练 - 与自动编码器一样，这个问题的触发原因是重建卷积网络的过多层会导致伪影。这个问题通常通过**渐进式增长**来解决，即首先在低分辨率图像上训练几层，然后“解除”或添加层。另一个解决方案是在层之间添加额外的连接，并一次训练多个分辨率——有关详细信息，请参阅这篇[多尺度梯度GANs论文](https://arxiv.org/abs/1903.06048)。

  

##  风格转换



GAN 是生成艺术图像的绝佳方式。另一种有趣的技术是所谓的**风格转换**移，它采用一张**内容图像**，并以不同的风格重新绘制它，应用来自**风格图像**的滤镜。

它的工作方式如下：

- 我们从一个随机噪声图像开始（或从一个内容图像开始，但为了理解，从随机噪声开始更容易）

- 我们的目标是创建一个这样的图像，它既接近内容图像又接近风格图像。这将由两个损失函数决定：
  - 内容损失是基于 CNN 在某些层从当前图像和内容图像中提取的特征计算的
  - **风格损失**以一种使用Gram矩阵（更多细节见示例笔记本）巧妙方式计算当前图像和风格图像之间的差异
  
- 为了使图像更平滑并去除噪声，我们还引入了**变异损失**，它计算相邻像素之间的平均距离

- 主要的优化循环使用梯度下降（或其他一些优化算法）调整当前图像以最小化总损失，总损失是所有三个损失的加权和。

  

## ✍️ 示例：样式转换



## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/210)



##  结论



在本课中，你学习了 GAN 以及如何训练它们。还了解了这种类型的神经网络可能面临的特殊挑战，以及一些克服这些挑战的策略。



##  🚀 挑战



使用自己的图像运行[样式转换 notebooks](https://github.com/happyzjp/AI-For-Beginners/edit/main/lessons/4-ComputerVision/09-Autoencoders/)。

##  复习与自学



参考下面的资源，了解更多关于GANs的信息：：

- Marco Pasini，《我训练生成对抗网络GANs一年学到的10个教训》(https://towardsdatascience.com/10-lessons-i-learned-training-generative-adversarial-networks-gans-for-a-year-c9071159628)
- [StyleGAN](https://en.wikipedia.org/wiki/StyleGAN)，一个*实际上*需要考虑的GAN架构
- [在Azure ML上使用GAN创建生成性艺术](https://soshnikov.com/scienceart/creating-generative-art-using-gan-on-azureml/)

##  作业



重访与本课关联的两个 notebooks 之一，并在自己的图像上重新训练GAN。你能创造什么