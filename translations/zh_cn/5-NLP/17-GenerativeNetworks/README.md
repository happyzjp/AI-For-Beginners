# 生成网络



## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/117)



循环神经网络 (RNN) 及其门控单元变体（如长短期记忆单元 (LSTM) 和门控循环单元 (GRU)）为语言建模提供了一种机制，因为它们可以学习单词顺序并为序列中的下一个单词提供预测。这使我们能够将 RNN 用于生成性任务，例如普通文本生成、机器翻译，甚至图像字幕。

> ✅ 想想您在键入时从文本完成等生成性任务中受益的所有时间。研究一下您最喜欢的应用程序，看看它们是否利用了 RNN。

在上一单元中我们讨论的 RNN 架构中，每个 RNN 单元将下一个隐藏状态作为输出。但是，我们还可以在每个循环单元中添加另一个输出，这将允许我们输出一个序列（其长度等于原始序列）。此外，我们可以使用在每个步骤不接受输入的 RNN 单元，只需获取一些初始状态向量，然后生成一系列输出。

这允许使用下图所示的不同神经架构：

[![Image showing common recurrent neural network patterns.](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/unreasonable-effectiveness-of-rnn.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/unreasonable-effectiveness-of-rnn.jpg)

> 图片来自 Andrej Karpaty 的博客文章《循环神经网络的不合理有效性》

- 一对一是一种传统的神经网络，有一个输入和一个输出
- 一对多是一种生成性架构，它接受一个输入值，并生成一系列输出值。例如，如果我们想要训练一个图像字幕网络，该网络将生成图片的文本描述，我们可以将图片作为输入，通过 CNN 传递它以获得其隐藏状态，然后让循环链逐字生成字幕
- 多对一对应于我们在前一个单元中描述的 RNN 架构，例如文本分类
- 多对多或序列到序列对应于机器翻译等任务，其中我们首先让 RNN 从输入序列中收集所有信息到隐藏状态，然后另一个 RNN 链将此状态展开到输出序列中。

在本单元中，我们将重点关注帮助我们生成文本的简单生成模型。为简单起见，我们将使用字符级标记化。

我们将训练此 RNN 逐步生成文本。在每一步中，我们将取一个长度为 `nchars` 的字符序列，并要求网络为每个输入字符生成下一个输出字符：

[![Image showing an example RNN generation of the word 'HELLO'.](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/rnn-generate.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/rnn-generate.png)

在生成文本（在推理期间）时，我们从一些提示开始，这些提示通过 RNN 单元传递以生成其中间状态，然后从该状态开始生成。我们一次生成一个字符，并将状态和生成的字符传递给另一个 RNN 单元以生成下一个字符，直到生成足够的字符。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/rnn-generate-inf.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/rnn-generate-inf.png)

> 作者的图片

## ✍️ 练习：生成式网络



在以下笔记本中继续学习：

- [PyTorch 生成网络](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/GenerativePyTorch.ipynb)
- [TensorFlow 生成网络](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/GenerativeTF.ipynb)

## 软文本生成和温度



每个 RNN 单元的输出都是一个字符的概率分布。如果我们总是将概率最高的字符作为生成文本中的下一个字符，那么文本经常会在相同的字符序列之间“循环”，就像在这个示例中一样：

```
today of the second the company and a second the company ...
```



但是，如果我们查看下一个字符的概率分布，则最高概率之间的差异可能不是很大，例如一个字符的概率可以是 0.2，另一个是 0.19，等等。例如，在寻找序列“play”中的下一个字符时，下一个字符同样可能是空格或 e（如单词 player 中）。

这让我们得出结论，选择具有较高概率的字符并不总是“公平”的，因为选择第二高的字符仍然可能让我们得到有意义的文本。从网络输出给出的概率分布中对字符进行采样更为明智。我们还可以使用一个参数（温度），它将使概率分布变平坦，以防我们想要增加更多随机性，或者使它更陡峭，如果我们想要更多地坚持最高概率的字符。

探索如何在上面链接的笔记本中实现这种软文本生成。

##  结论



虽然文本生成本身可能很有用，但主要好处来自于使用 RNN 从一些初始特征向量生成文本的能力。例如，文本生成被用作机器翻译的一部分（在这种情况下，编码器的状态向量用于生成或解码翻译后的消息），或生成图像的文本描述（在这种情况下，特征向量将来自 CNN 提取器）。

##  🚀 挑战



在 Microsoft Learn 上学习有关此主题的一些课程

- 使用 PyTorch/TensorFlow 进行文本生成

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/217)



##  复习与自学



这里有一些文章可以扩展您的知识

- 使用马尔可夫链、LSTM 和 GPT-2 生成文本的不同方法：博客文章
- Keras 文档中的文本生成示例

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/lab/README.md)



我们已经了解了如何逐个字符生成文本。在实验中，您将探索单词级别的文本生成。
