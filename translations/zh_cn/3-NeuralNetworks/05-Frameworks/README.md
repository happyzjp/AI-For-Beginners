### 神经网络框架

就像我们之前学过的一样，为了有效地训练神经网络，我们需要做到两件事：

- **操作张量**: 例如进行乘法、加法以及计算一些函数，例如 sigmoid 函数或 softmax 函数。
- **计算所有表达式的梯度**: 为了执行梯度下降优化算法。

##### [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/105)

虽然 `numpy` 库可以完成第一部分，但我们需要一些机制来计算梯度。在我们上一节中开发的[框架](https://chat.openai.com/04-OwnFramework/OwnFramework.ipynb)中，我们必须在 `backward` 方法中手动编写所有导数函数，该方法执行反向传播。理想情况下，一个框架应该让我们有机会计算我们能定义的任何表达式的梯度。

另一件重要的事情是能够在 GPU 或任何其他专用计算单元（如 [TPU](https://en.wikipedia.org/wiki/Tensor_Processing_Unit)）上执行计算。深度神经网络训练需要大量的计算，并且能够在 GPU 上并行化这些计算非常重要。

> ✅ 术语“并行化”是指将计算分布到多个设备上。

目前，最流行的两个神经网络框架是：[TensorFlow](http://tensorflow.org/) 和 [PyTorch](https://pytorch.org/)。两者都提供低级 API，可在 CPU 和 GPU 上使用张量进行操作。在低级 API 之上，还有高级 API，分别称为 [Keras](https://keras.io/) 和 [PyTorch Lightning](https://pytorchlightning.ai/)。

| 低级 API | [TensorFlow](http://tensorflow.org/) | [PyTorch](https://pytorch.org/)                   |
| -------- | ------------------------------------ | ------------------------------------------------- |
| 高级 API | [Keras](https://keras.io/)           | [PyTorch Lightning](https://pytorchlightning.ai/) |

**低级API** 在这两个框架中都允许构建所谓的 **计算图**。此图定义了如何使用给定的输入参数计算输出（通常是损失函数），并且可以在有 GPU 的情况下将其推送到 GPU 上进行计算。有一些函数可以区分此计算图并计算梯度，然后可将其用于优化模型参数。

**高级API**  几乎将神经网络视为 **层序列**，并且使得构建大多数神经网络变得更加容易。训练模型通常需要准备数据，然后调用 `fit` 函数来完成这项工作。

高级 API 允许你在无需担心大量细节的情况下非常快速地构建典型的神经网络。同时，低级 API 提供了对训练过程的更多控制，因此在研究中，当涉及到新的神经网络架构时，它们被广泛使用。

还需要理解的一点是，你可以同时使用这两种API，例如，你可以使用低级API开发自己的网络层架构，然后在使用高级API构建和训练的较大网络中使用它。或者，你可以使用高级API将网络定义为层序列，然后使用自己的低级训练循环执行优化。这两种API使用相同的基本概念，并且它们被设计成能够很好地协同工作。

#####  学习

在本课程中，我们为 PyTorch 和 TensorFlow 提供了大部分内容。你可以选择喜欢的框架，只浏览相应的 notebooks。如果不确定选择哪个框架，请阅读互联网上关于**PyTorch与TensorFlow** 的一些讨论。还可以查看这两个框架以获得更好的理解。

在可能的情况下，我们将使用高级 API 以简化操作。但是，我们认为了解神经网络从底层开始的工作原理非常重要，因此一开始我们从使用低级 API 和张量开始。但是，如果你想快速上手，并且不想花很多时间学习这些细节，你可以跳过这些内容，直接进入 notebooks 的 高级 API 部分。

#####  ✍️ 练习：框架

在以下 notebooks 中继续学习：

| 低级 API | [TensorFlow+Keras 笔记本](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/05-Frameworks/IntroKerasTF.ipynb) | [PyTorch](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/05-Frameworks/IntroPyTorch.ipynb) |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 高级 API | [Keras](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/05-Frameworks/IntroKeras.ipynb) | *PyTorch Lightning*                                          |

掌握框架后，让我们回顾一下过拟合的概念。

#####  过拟合

过拟合是机器学习中一个非常重要的概念，正确理解它非常重要！

考虑以下关于逼近 5 个点（在下面的图表中用 `x` 表示）的问题：

| [![linear](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/images/overfit1.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/images/overfit1.jpg) | [![overfit](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/images/overfit2.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/images/overfit2.jpg) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **线性模型，2 个参数**                                       | **非线性模型，7 个参数**                                     |
| 训练误差 = 5.3                                               | 训练误差 = 0                                                 |
| 验证误差 = 5.1                                               | 验证误差 = 20                                                |

- 在左侧，我们看到一个良好的直线逼近。由于参数数量充足，模型正确地理解了点分布背后的思想。
- 在右侧，模型过于强大。由于我们只有 5 个点，而模型有 7 个参数，因此它可以调整为通过所有点，从而使训练误差为 0。然而，这会阻止模型理解数据背后的正确模式，因此验证误差非常高。

在模型的丰富性（参数数量）和训练样本数量之间取得正确的平衡非常重要。

#####  为什么会出现过拟合

- 训练数据不足
-  模型过于强大
- 输入数据中噪声太多

##### 如何检测过拟合

从上面的图表中可以看到，过拟合可以通过非常低的训练误差和较高的验证误差来检测。通常在训练期间，我们会看到训练和验证误差都开始下降，然后在某个时刻验证误差可能会停止下降并开始上升。这将是过拟合的标志，并且表明我们可能应该在此时停止训练（或至少对模型进行快照）。

[![overfitting](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/images/Overfitting.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/images/Overfitting.png)

##### 如何防止过拟合

如果你发现发生了过拟合，可以采取以下措施：

- 增加训练数据量
- 降低模型的复杂度
- 使用一些正则化技术，例如 Dropout，我们将在后面考虑。

##### 过拟合和偏差-方差权衡

过拟合实际上是统计学中一个更通用问题的一个案例，称为[偏差-方差平衡](https://en.wikipedia.org/wiki/Bias–variance_tradeoff)。如果我们考虑模型中可能的误差来源，我们可以看到两种类型的误差：

- **偏差错误** 是由我们的算法无法正确捕捉训练数据之间的关系引起的。它可能源于我们的模型不够强大（**欠拟合**））。
- **方差错误** 是由于模型逼近输入数据中的噪声而不是有意义的关系（**过拟合**）引起的。

在训练期间，偏差误差会减小（因为我们的模型学会了逼近数据），而方差误差会增加。重要的是停止训练 - 手动（当我们检测到过拟合时）或自动（通过引入正则化） - 以防止过拟合。

#####  结论

在本课程中，你学到了两种最流行的 AI 框架 TensorFlow 和 PyTorch 的各种 API 之间的差异。此外，你还了解了一个非常重要的主题，即过拟合。

#####  🚀 挑战

在附带的笔记本中，你将在底部找到“任务”；逐步完成这些 notebooks 中的任务。

##### [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/205)

#####  复习与自学

对以下主题进行一些研究：

- TensorFlow
-  PyTorch 
- 过拟合

思考以下问题：

- TensorFlow 和 PyTorch 有什么区别？
- 过拟合和欠拟合有什么区别？

##### [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/05-Frameworks/lab/README.md)

在本实验中，要求你使用 PyTorch 或 TensorFlow 使用单层和多层全连接网络解决两个分类问题。

- [ 说明](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/05-Frameworks/lab/README.md)
- [ Notebook](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/05-Frameworks/lab/LabFrameworks.ipynb)
