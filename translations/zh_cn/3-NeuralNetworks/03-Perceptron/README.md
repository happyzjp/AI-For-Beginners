# 神经网络简介：感知器

### [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/103)

1957 年，康奈尔航空实验室的弗兰克·罗森布拉特首次尝试实现类似于现代神经网络的东西。这是一个名为“Mark-1”的硬件实现，旨在识别三角形、正方形和圆形等原始几何图形。

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [![Frank Rosenblatt](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/images/Rosenblatt-wikipedia.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/images/Rosenblatt-wikipedia.jpg) | [![The Mark 1 Perceptron](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/images/Mark_I_perceptron_wikipedia.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/images/Mark_I_perceptron_wikipedia.jpg) |

>  来自维基百科的图片

输入图像由 20x20 光电阵列表示，因此神经网络有 400 个输入和一个二进制输出。一个简单的网络包含一个神经元，也称为**阈值逻辑单元**。神经网络权重就像电位器，需要在训练阶段进行手动调整。

> ✅ 电位器是一种允许用户调整电路电阻的设备。

> 《纽约时报》当时这样写道：感知器是电子计算机的胚胎，[海军] 希望它能够走路、说话、看、写、自我复制并意识到自己的存在。

###  感知器模型

假设我们的模型中有 N 个特征，在这种情况下，输入向量将是一个大小为 N 的向量。感知器是一个**二元分类**模型，即它可以区分两类输入数据。我们将假设对于每个输入向量 x，我们的感知器的输出将是 +1 或 -1，具体取决于该类。输出将使用以下公式计算：

y(x) = f(wTx)

其中 f 是一个阶跃激活函数

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/images/activation-func.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/images/activation-func.png)

###  训练感知器

为了训练感知器，我们需要找到一个权重向量 w，它可以对大多数值进行正确分类，即产生最小的**误差**。此误差由**感知器准则**以下方式定义：

E(w) = -∑wTxiti

 其中：

- 求和是在导致错误分类的那些训练数据点 i 上进行的
- x 是输入数据，t 对于负例和正例分别为 -1 或 +1。

此准则被视为权重 w 的一个函数，我们需要将其最小化。通常，使用一种称为**梯度下降**的方法，其中我们从一些初始权重 w (0) 开始，然后在每一步根据以下公式更新权重：

w(t+1) = w(t) - η∇E(w)

这里 η 是所谓的**学习速率**，∇E(w) 表示 E 的**梯度**。在计算出梯度后，我们最终得到

w(t+1) = w(t) + ∑ηxiti

Python 中的算法如下所示：

```
def train(positive_examples, negative_examples, num_iterations = 100, eta = 1):

    weights = [0,0,0] # Initialize weights (almost randomly :)
        
    for i in range(num_iterations):
        pos = random.choice(positive_examples)
        neg = random.choice(negative_examples)

        z = np.dot(pos, weights) # compute perceptron output
        if z < 0: # positive example classified as negative
            weights = weights + eta*weights.shape

        z  = np.dot(neg, weights)
        if z >= 0: # negative example classified as positive
            weights = weights - eta*weights.shape

    return weights
```

###  结论

在本课程中，你了解了感知器，它是一个二元分类模型，以及如何通过使用权重向量对其进行训练。

###  🚀 挑战

如果您想尝试构建自己的感知器，请尝试使用 [Azure ML  设计器](https://docs.microsoft.com/en-us/azure/machine-learning/concept-designer?WT.mc_id=academic-77998-cacaste)的 [Microsoft Learn ](https://docs.microsoft.com/en-us/azure/machine-learning/component-reference/two-class-averaged-perceptron?WT.mc_id=academic-77998-cacaste) 上的此实验室。

### [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/203)

###  复习与自学

若要了解如何使用感知器解决玩具问题和现实问题，以及继续学习，请查看 [Perceptron](https://chat.openai.com/c/Perceptron.ipynb) 笔记本。

这里有一篇有趣的[关于感知器的文章](https://towardsdatascience.com/what-is-a-perceptron-basics-of-neural-networks-c4cfea20c590)。

### [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/lab/README.md)

在本课程中，我们为二元分类任务实现了一个感知器，并使用它对两个手写数字进行分类。在此实验室中，要求你完全解决数字分类问题，即确定哪个数字最有可能对应于给定的图像。

- [说明](https://github.com/happyzjp/AI-For-Beginners/tree/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/lab/README.md)
- [ Notebook](https://github.com/happyzjp/AI-For-Beginners/tree/main/translations/zh_cn/3-NeuralNetworks/03-Perceptron/lab/PerceptronMultiClass.ipynb)
