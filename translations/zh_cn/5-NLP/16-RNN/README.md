# 循环神经网络



## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/116)



在前面的章节中，我们一直在使用文本的丰富语义表示和嵌入之上的一个简单的线性分类器。此架构的作用是捕获句子中单词的聚合含义，但它不考虑单词的顺序，因为嵌入之上的聚合操作从原始文本中删除了此信息。由于这些模型无法对单词顺序进行建模，因此它们无法解决更复杂或模棱两可的任务，例如文本生成或问题解答。

为了捕获文本序列的含义，我们需要使用另一种神经网络架构，称为循环神经网络或 RNN。在 RNN 中，我们将句子一次一个符号地传递到网络中，并且网络会产生一些状态，然后我们再次将该状态与下一个符号一起传递到网络中。

[![RNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/16-RNN/images/rnn.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/images/rnn.png)

> 作者的图片

给定输入序列标记 X 0 ,...,X n , RNN 创建神经网络块序列，并使用反向传播端到端训练此序列。每个网络块将对 (X,S) 作为输入，并产生 S i+1 作为结果。最终状态 S n 或（输出 Y n ）进入线性分类器以产生结果。所有网络块共享相同的权重，并使用一次反向传播端到端进行训练。

因为状态向量 S 0 ,...,S n 通过网络传递，它能够学习单词之间的顺序依赖关系。例如，当单词 not 出现在序列中的某个位置时，它可以学会否定状态向量中的某些元素，从而导致否定。

> ✅ 由于上图中所有 RNN 块的权重都是共享的，因此同一张图片可以表示为一个块（右侧），其中包含一个循环反馈回路，该回路将网络的输出状态传递回输入。

## RNN 单元的剖析



我们来看看一个简单的 RNN 单元是如何组织的。它接受前一个状态 S i-1 和当前符号 X 作为输入，并且必须产生输出状态 S（有时，我们也对一些其他输出 Y 感兴趣，就像生成网络中的情况一样）。

一个简单的 RNN 单元内部有两个权重矩阵：一个转换输入符号（我们称之为 W），另一个转换输入状态（H）。在这种情况下，网络的输出计算为 σ(W×X+H×S i-1 +b)，其中 σ 是激活函数，b 是附加偏差。

[![RNN Cell Anatomy](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/16-RNN/images/rnn-anatomy.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/images/rnn-anatomy.png)

> 作者的图片

在许多情况下，输入标记在进入 RNN 之前会通过嵌入层以降低维度。在这种情况下，如果输入向量的维度为 emb_size，状态向量为 hid_size - W 的大小为 emb_size×hid_size，H 的大小为 hid_size×hid_size。

## 长短期记忆（LSTM）



经典 RNN 的主要问题之一是所谓的梯度消失问题。由于 RNN 在一次反向传播中端到端地进行训练，因此它难以将错误传播到网络的第一层，因此网络无法学习远距离标记之间的关系。避免这个问题的方法之一是通过使用所谓的门来引入显式状态管理。有两种众所周知这种架构：长短期记忆 (LSTM) 和门控循环单元 (GRU)。

[![Image showing an example long short term memory cell](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/16-RNN/images/long-short-term-memory-cell.svg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/images/long-short-term-memory-cell.svg)

>  图片源待定

LSTM 网络的组织方式与 RNN 类似，但有两层状态从一层传递到另一层：实际状态 C 和隐藏向量 H。在每个单元中，隐藏向量 H 与输入 X 连接，它们通过门控制状态 C 的变化。每个门都是一个具有 sigmoid 激活（输出范围 [0,1]）的神经网络，当乘以状态向量时，可以将其视为按位掩码。有以下几个门（从上图的左到右）：

- 遗忘门获取一个隐藏向量并确定我们需要忘记哪些 C 向量的分量，哪些需要传递。
- 输入门从输入和隐藏向量中获取一些信息并将其插入状态。
- 输出门通过具有 tanh 激活的线性层转换状态，然后使用隐藏向量 H 选择其一些分量以生成新的状态 C i+1 。

状态 C 的组件可以被认为是一些可以开关的标志。例如，当我们在序列中遇到名字 Alice 时，我们可能希望假设它指的是一个女性角色，并在状态中升起标志，表明句子中有一个女性名词。当我们进一步遇到短语 and Tom 时，我们将升起标志，表明我们有一个复数名词。因此，通过操作状态，我们理论上可以跟踪句子部分的语法属性。

> ✅ 了解 LSTM 内部机制的优秀资源是 Christopher Olah 撰写的这篇精彩文章 Understanding LSTM Networks。

## 双向和多层 RNN



我们已经讨论了从序列开始到结束的单向循环网络。这看起来很自然，因为它类似于我们阅读和聆听语音的方式。然而，由于在许多实际情况下我们对输入序列有随机访问，因此双向运行循环计算可能更有意义。此类网络称为双向 RNN。在处理双向网络时，我们需要两个隐藏状态向量，每个方向一个。

循环网络（单向或双向）捕获序列中的特定模式，并可以将它们存储到状态向量中或传递到输出中。与卷积网络一样，我们可以在第一层之上构建另一层循环层，以捕获更高级别的模式，并从第一层提取的低级模式中构建。这将我们引向了多层 RNN 的概念，它由两个或更多个循环网络组成，其中前一层的输出作为输入传递到下一层。

[![Image showing a Multilayer long-short-term-memory- RNN](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/16-RNN/images/multi-layer-lstm.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/images/multi-layer-lstm.jpg)

*来自 Fernando López 的这篇精彩帖子的图片*

##  ✍️ 练习：嵌入



在以下笔记本中继续学习：

- [ PyTorch 中的 RNN](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/RNNPyTorch.ipynb)
- [ 使用 TensorFlow 的 RNN](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/RNNTF.ipynb)

##  结论



在本单元中，我们已经看到 RNN 可用于序列分类，但实际上，它们可以处理更多任务，例如文本生成、机器翻译等。我们将在下一单元中考虑这些任务。

##  🚀 挑战



阅读有关 LSTM 的一些文献并考虑其应用：

- [网格长短期记忆](https://arxiv.org/pdf/1507.01526v1.pdf)
- [Show, Attend and Tell：带有视觉注意力的神经图像标题生成](https://arxiv.org/pdf/1502.03044v2.pdf)

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/216)



##  复习与自学



- 克里斯托弗·奥拉赫对 LSTM 网络的理解。

## [ 作业：笔记本](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/assignment.md)
