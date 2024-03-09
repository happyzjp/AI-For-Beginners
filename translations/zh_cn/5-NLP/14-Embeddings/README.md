# 嵌入



## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/114)



在基于 BoW 或 TF/IDF 训练分类器时，我们使用长度为 `vocab_size` 的高维词袋向量进行操作，并且我们明确地将低维位置表示向量转换为稀疏独热表示。然而，这种独热表示并不节省内存。此外，每个单词都是独立处理的，即独热编码向量不会表达单词之间的任何语义相似性。

嵌入的想法是用低维稠密向量来表示单词，这些向量在某种程度上反映了单词的语义含义。我们稍后将讨论如何构建有意义的单词嵌入，但现在让我们将嵌入视为降低单词向量维数的一种方式。

因此，嵌入层将一个单词作为输入，并生成一个指定 `embedding_size` 的输出向量。从某种意义上说，它与 `Linear` 层非常相似，但它不会采用独热编码向量，而是可以将单词编号作为输入，从而避免创建大型独热编码向量。

通过在我们的分类器网络中使用嵌入层作为第一层，我们可以从词袋模型切换到嵌入袋模型，在该模型中，我们首先将文本中的每个单词转换为相应的嵌入，然后计算所有这些嵌入的某个聚合函数，例如 `sum` 、 `average` 或 `max` 。

[![Image showing an embedding classifier for five sequence words.](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/14-Embeddings/images/embedding-classifier-example.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/14-Embeddings/images/embedding-classifier-example.png)

> 作者的图片

##  ✍️ 练习：嵌入



在以下笔记本中继续学习：

- [Embeddings with PyTorch](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/14-Embeddings/EmbeddingsPyTorch.ipynb) 重试 错误原因
- [Embeddings TensorFlow](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/14-Embeddings/EmbeddingsTF.ipynb) 重试 错误原因

## Semantic Embeddings: Word2Vec 重试 错误原因



虽然嵌入层学会将单词映射到向量表示，但是，这种表示不一定有太多的语义含义。学习一个向量表示会很好，这样相似的单词或同义词对应于在某个向量距离（例如欧几里得距离）方面彼此接近的向量。

为此，我们需要以特定方式在大量文本集合上预训练我们的嵌入模型。训练语义嵌入的一种方法称为 Word2Vec。它基于两个主要架构，用于生成单词的分布式表示：

- 连续词袋 (CBoW) — 在此架构中，我们训练模型从周围语境中预测单词。给定 ngram (�−2,�−1,�0,�1,�2) ，模型的目标是从 (�−2,�−1,�1,�2) 中预测 �0 。
- 连续跳跃图与 CBoW 相反。该模型使用上下文单词的周围窗口来预测当前单词。

CBoW 速度更快，而跳跃图速度较慢，但能更好地表示不频繁出现的单词。

[![Image showing both CBoW and Skip-Gram algorithms to convert words to vectors.](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/14-Embeddings/images/example-algorithms-for-converting-words-to-vectors.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/14-Embeddings/images/example-algorithms-for-converting-words-to-vectors.png)

>  本文中的图片

Word2Vec pre-trained embeddings (as well as other similar models, such as GloVe) can also be used in place of embedding layer in neural networks. However, we need to deal with vocabularies, because the vocabulary used to pre-train Word2Vec/GloVe is likely to differ from the vocabulary in our text corpus. Have a look into the above Notebooks to see how this problem can be resolved. 重试 错误原因

##  上下文嵌入



传统预训练嵌入表示（如 Word2Vec）的一个关键限制是词义消歧问题。虽然预训练嵌入可以捕捉到上下文中单词的某些含义，但单词的每种可能含义都编码到同一个嵌入中。这可能会给下游模型带来问题，因为许多单词（如单词“play”）具有不同的含义，具体取决于它们所使用的上下文。

例如，在以下两个不同的句子中，“play”一词具有完全不同的含义：

- 我去剧院看了一场戏。
- 约翰想和他的朋友们玩。

上面的预训练嵌入在同一个嵌入中表示了“play”一词的这两种含义。为了克服这一限制，我们需要基于语言模型构建嵌入，该模型在大量文本语料库上进行训练，并且知道如何在不同的上下文中组合单词。讨论上下文嵌入超出了本教程的范围，但我们将在课程后面讨论语言模型时重新讨论它们。

##  结论



在本课程中，您发现了如何在 TensorFlow 和 Pytorch 中构建和使用嵌入层，以更好地反映单词的语义含义。

##  🚀 挑战



Word2Vec 已用于一些有趣的应用程序，包括生成歌词和诗歌。看看这篇文章，它介绍了作者如何使用 Word2Vec 来生成诗歌。另外，观看 Dan Shiffmann 的这段视频，了解对该技术的不同解释。然后尝试将这些技术应用到您自己的文本语料库中，或许可以从 Kaggle 中获取。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/214)



##  复习与自学



阅读这篇关于 Word2Vec 的论文：向量空间中单词表示的有效估计

## [ 作业：笔记本](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/14-Embeddings/assignment.md)
