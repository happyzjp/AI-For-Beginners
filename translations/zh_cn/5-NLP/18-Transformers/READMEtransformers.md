# 注意力机制和 Transformer



## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/118)



NLP 领域最重要的一个问题是机器翻译，它是 Google 翻译等工具的基础任务。在本节中，我们将重点关注机器翻译，或者更一般地，关注任何序列到序列的任务（也称为句子转换）。

使用 RNN，序列到序列由两个循环网络实现，其中一个网络（编码器）将输入序列折叠成一个隐藏状态，而另一个网络（解码器）将这个隐藏状态展开成一个翻译结果。这种方法存在几个问题：

- 编码器网络的最终状态很难记住句子的开头，从而导致模型对长句子的质量较差
- 序列中的所有单词对结果的影响都是相同的。然而，在现实中，输入序列中的特定单词通常对顺序输出的影响比其他单词更大。

注意力机制提供了一种对每个输入向量对 RNN 的每个输出预测的上下文影响进行加权的方法。它的实现方式是在输入 RNN 和输出 RNN 的中间状态之间创建捷径。这样，在生成输出符号 y t 时，我们将考虑所有输入隐藏状态 h，并使用不同的权重系数 α t,i 。

[![Image showing an encoder/decoder model with an additive attention layer](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/18-Transformers/images/encoder-decoder-attention.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/images/encoder-decoder-attention.png)

> Bahdanau 等人在 2015 年提出的具有加性注意力机制的编码器-解码器模型，引用自此博客文章

注意力矩阵 {α i,j } 将表示某些输入词在生成输出序列中给定词语时所起的作用程度。下面是此类矩阵的一个示例：

[![Image showing a sample alignment found by RNNsearch-50, taken from Bahdanau - arviz.org](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/18-Transformers/images/bahdanau-fig3.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/images/bahdanau-fig3.png)

> Bahdanau 等人，2015 年（图 3）中的图

注意力机制在当前或接近当前的 NLP 技术中发挥着重要作用。然而，添加注意力极大地增加了模型参数的数量，这导致了 RNN 的扩展问题。扩展 RNN 的一个关键限制是模型的循环性质使得批处理和并行化训练变得困难。在 RNN 中，序列的每个元素都需要按顺序处理，这意味着它不能轻松并行化。

[![Encoder Decoder with Attention](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/18-Transformers/images/EncDecAttention.gif)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/images/EncDecAttention.gif)

> 来自 Google 博客的图片

注意力机制的采用与这一限制相结合，催生了我们今天所知并使用的最先进的 Transformer 模型，例如 BERT 到 Open-GPT3。

##  Transformer 模型



变压器的主要思想之一是避免 RNN 的顺序性质，并创建一个在训练期间可并行的模型。这是通过实现两个思想来实现的：

-  位置编码
- 使用自注意力机制来捕获模式，而不是 RNN（或 CNN）（这就是为什么介绍 Transformer 的论文被称为注意力就是你所需要的

### 位置编码/嵌入



位置编码的思想如下。

1. 使用 RNN 时，标记的相对位置由步数表示，因此不需要显式表示。
2. 但是，一旦我们切换到注意力，我们就需要知道序列中标记的相对位置。
3. 为了获得位置编码，我们用序列中的标记位置序列（即数字序列 0,1, ...）来扩充我们的标记序列。
4. 然后，我们将标记位置与标记嵌入向量混合。为了将位置（整数）转换为向量，我们可以使用不同的方法：

- 可训练嵌入，类似于标记嵌入。这是我们在此考虑的方法。我们在标记及其位置之上应用嵌入层，从而生成相同维度的嵌入向量，然后将它们相加。
- 固定位置编码函数，如原始论文中所建议的。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/18-Transformers/images/pos-embedding.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/images/pos-embedding.png)

> 作者的图片

我们使用位置嵌入获得的结果同时嵌入了原始标记及其在序列中的位置。

### 多头自注意力



接下来，我们需要捕获序列中的一些模式。为此，转换器使用自注意力机制，该机制本质上是对输入和输出相同序列应用的注意力。应用自注意力使我们能够考虑句子中的上下文，并查看哪些单词相互关联。例如，它使我们能够看到哪些单词是由核心引用（如它）引用的，并考虑上下文：

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/18-Transformers/images/CoreferenceResolution.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/images/CoreferenceResolution.png)

> 来自 Google 博客的图片

在 Transformer 中，我们使用多头注意力，以便赋予网络捕获多种不同类型依赖关系的能力，例如长期与短期单词关系、共指与其他内容等。

TensorFlow Notebook 包含有关 Transformer 层实现的更多详细信息。

### 编码器-解码器注意力



在 Transformer 中，注意力在两个地方使用：

- 使用自注意力捕获输入文本中的模式
- 执行序列翻译 - 这是编码器和解码器之间的注意力层。

编码器-解码器注意力与本节开头描述的 RNN 中使用的注意力机制非常相似。此动画图表解释了编码器-解码器注意力的作用。

[![Animated GIF showing how the evaluations are performed in transformer models.](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/18-Transformers/images/transformer-animated-explanation.gif)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/images/transformer-animated-explanation.gif)

由于每个输入位置都独立映射到每个输出位置，因此 Transformer 比 RNN 更适合并行化，这使得语言模型更大、更具表现力。每个注意力头都可以用来学习单词之间的不同关系，从而改善下游自然语言处理任务。

## BERT



BERT（来自 Transformer 的双向编码器表示）是一个非常大的多层 Transformer 网络，BERT-base 有 12 层，BERT-large 有 24 层。该模型首先在大型文本数据（维基百科 + 书籍）上使用无监督训练（预测句子中的掩码词）进行预训练。在预训练期间，该模型吸收了大量的语言理解，然后可以使用微调与其他数据集一起利用。这个过程称为迁移学习。

[![picture from http://jalammar.github.io/illustrated-bert/](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/18-Transformers/images/jalammarBERT-language-modeling-masked-lm.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/images/jalammarBERT-language-modeling-masked-lm.png)

>  图片源

## ✍️ 练习：Transformer



在以下笔记本中继续学习：

- [ PyTorch 中的 Transformer](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/TransformersPyTorch.ipynb)
- [TensorFlow 中的 Transformer](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/TransformersTF.ipynb)

##  结论



在本课程中，您学习了 Transformer 和注意力机制，它们都是 NLP 工具箱中的基本工具。Transformer 架构有很多变体，包括 BERT、DistilBERT、BigBird、OpenGPT3 等，都可以进行微调。HuggingFace 包提供了一个存储库，用于使用 PyTorch 和 TensorFlow 训练其中许多架构。

##  🚀 挑战



## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/218)



##  复习与自学



- 博客文章，解释了关于 Transformer 的经典 Attention is all you need 论文。
- 一系列关于 Transformer 的博客文章，详细解释了该架构。

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/assignment.md)