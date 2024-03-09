### 神经网络简介：多层感知器

在上节中，你了解了最简单的神经网络模型 - 单层感知器，这是一个线性两类分类模型。

在本节中，我们将把这个模型扩展到一个更灵活的框架，使我们能够：

- 除了两类分类之外，执行**多类别分类**
- 除了分类之外，解决 **回归问题**
- 分离不可线性分离的类别

我们还将在 Python 中开发我们自己的模块化框架，这将使我们能够构建不同的神经网络架构。

### [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/104)

### 机器学习的形式化

让我们从形式化机器学习问题开始。假设我们有一个带有标签 **Y** 的训练数据集 **X**，我们需要构建一个模型 f，它将做出最准确的预测。预测的质量由**损失函数**  ℒ 衡量。以下是常用的损失函数：

- 对于回归问题，当我们需要预测一个数字时，我们可以使用 绝对误差 ∑<sub>i</sub>|f(x<sup>(i)</sup>)-y<sup>(i)</sup>|，或者 平方误差 ∑<sub>i</sub>(f(x<sup>(i)</sup>)-y<sup>(i)</sup>)<sup>2</sup>。
- 对于分类问题，我们使用 **0-1 损失**（本质上与模型的 **准确度** 相同）或 **逻辑损失**。

对于一级感知器，函数 f 被定义为线性函数 f(x)=wx+b（其中 w 是权重矩阵，x 是输入特征的向量，b 是偏差向量）。对于不同的神经网络架构，此函数可以采用更复杂的形式。

> 在分类的情况下，通常希望获得网络输出作为相应类别的概率。为了将任意数字转换为概率（例如，为了对输出进行归一化），我们经常使用 **softmax**  函数 σ，并且函数 f 变为 f(x)=σ(wx+b)

在上面的 f 定义中，w 和 b 被称为 **参数** θ=⟨w,b⟩。给定数据集 ⟨**X**,**Y**⟩，我们可以计算整个数据集上的总体误差，作为参数 θ 的函数。

> ✅ 神经网络训练的目标是通过改变参数 θ 来最小化误差

### 梯度下降优化

有一个众所周知的方法来进行函数优化，称为**梯度下降**。其思想是我们可以计算损失函数对参数的导数（在多维情况下称为梯度），并以这种方式改变参数，从而减少误差。这可以形式化为以下内容：

- 用一些随机值 w (0) , b (0) 初始化参数
- 重复以下步骤多次：
  - w(i+1) = w(i)-η∂ℒ/∂w
  - b(i+1) = b(i)-η∂ℒ/∂b

在训练期间，优化步骤应该考虑整个数据集来计算（记住，损失是通过所有训练样本的总和计算的）。然而，在实际生活中，我们取数据集的小部分，称为**小批量（minibatches）**，并根据数据子集计算梯度。由于每次随机抽取子集，因此这种方法称为**随机梯度下降**(SGD)。

### 多层感知器和反向传播

如上所述，单层网络能够对线性可分类别进行分类。为了构建更丰富的模型，我们可以组合网络的多个层。从数学上来说，这意味着函数 f 将具有更复杂的形式，并且将分几个步骤计算：

- z1=w1x+b1
- z2=w2α(z1)+b2
- f = σ(z2)

此处，α 是**非线性激活函数**，σ 是 softmax 函数，参数 θ= ,b 1 ,w 2 ,b 2 >。

梯度下降算法将保持不变，但计算梯度会更困难。给定链式求导法则，我们可以计算导数为：

- ∂ℒ/∂w2 = (∂ℒ/∂σ)(∂σ/∂z2)(∂z2/∂w2)
- ∂ℒ/∂w1 = (∂ℒ/∂σ)(∂σ/∂z2)(∂z2/∂α)(∂α/∂z1)(∂z1/∂w1)

> ✅ 使用链式求导法则计算损失函数对参数的导数。

请注意，所有这些表达式的最左侧部分是相同的，因此我们可以从损失函数开始并通过计算图“向后”计算导数。因此，训练多层感知器的方法称为反向传播或 **反向传播** 。

[![compute graph](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/04-OwnFramework/images/ComputeGraphGrad.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/04-OwnFramework/images/ComputeGraphGrad.png)

>  待办事项：图片引用

> ✅ 我们将在笔记本示例中更详细地介绍反向传播。

###  结论

在本课中，我们构建了自己的神经网络库，并将其用于简单的二维分类任务。

###  🚀 挑战

在附带的笔记本中，你将实现自己的框架来构建和训练多层感知器。你将能够详细了解现代神经网络如何运作。

继续查看 [OwnFramework](https://chat.openai.com/c/OwnFramework.ipynb) 笔记本并逐步完成它。

### [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/204)

###  复习与自学

反向传播是人工智能和机器学习中常用的算法，值得[更详细地研究](https://wikipedia.org/wiki/Backpropagation)。

### [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/04-OwnFramework/lab/README.md)

在本实验中，要求你使用本课程中构建的框架来解决 MNIST 手写数字分类问题。

- [ 说明](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/04-OwnFramework/lab/README.md)
- [  Notebook](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/04-OwnFramework/lab/MyFW_MNIST.ipynb)
