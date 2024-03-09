# 将文本表示为张量



## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/113)



##  文本分类



在本节的第一部分中，我们将重点关注文本分类任务。我们将使用 AG 新闻数据集，其中包含以下新闻文章：

-  类别：科学技术
- 标题：肯塔基州公司获得资助研究肽（美联社）
- 正文：美联社 - 一家由路易斯维尔大学化学研究员创立的公司获得了一笔资助，用于开发...

我们的目标是根据文本将新闻项目分类到其中一类。

##  表示文本



如果我们想用神经网络解决自然语言处理 (NLP) 任务，我们需要某种方法将文本表示为张量。计算机已经使用 ASCII 或 UTF-8 等编码将文本字符表示为映射到屏幕上字体的数字。

[![Image showing diagram mapping a character to an ASCII and binary representation](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/13-TextRep/images/ascii-character-map.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/13-TextRep/images/ascii-character-map.png)

> [ 图片来源](https://www.seobility.net/en/wiki/ASCII)

作为人类，我们理解每个字母代表什么，以及所有字符如何组合在一起形成一个句子的单词。然而，计算机本身并不具备这种理解力，神经网络必须在训练期间学习含义。

因此，在表示文本时，我们可以使用不同的方法：

- 字符级表示，当我们通过将每个字符视为一个数字来表示文本时。假设我们的文本语料库中有 C 个不同的字符，那么单词 Hello 将由 5xC 张量表示。每个字母将对应于独热编码中张量列。
- 单词级表示，其中我们创建文本中所有单词的词汇表，然后使用独热编码表示单词。这种方法在某种程度上更好，因为每个字母本身没有太多意义，因此通过使用更高级别的语义概念 - 单词 - 我们简化了神经网络的任务。然而，鉴于字典大小很大，我们需要处理高维稀疏张量。

无论表示如何，我们首先需要将文本转换为一系列标记，一个标记可以是字符、单词，有时甚至是单词的一部分。然后，我们使用词汇表将标记转换为数字，并且可以使用独热编码将此数字输入神经网络。

##  N元语法



在自然语言中，词语的精确含义只能在上下文中确定。例如，“神经网络”和“渔网”的含义完全不同。考虑这一点的一种方法是在词对上构建我们的模型，并将词对视为单独的词汇标记。这样，句子“我喜欢去钓鱼”将由以下标记序列表示：我喜欢，喜欢，去，去钓鱼。这种方法的问题在于字典大小会显著增加，并且像“去钓鱼”和“去购物”这样的组合由不同的标记表示，尽管动词相同，但它们没有任何语义相似性。

在某些情况下，我们也可以考虑使用三元组——三个词的组合。因此，这种方法通常被称为 n 元组。此外，使用字符级表示的 n 元组是有意义的，在这种情况下，n 元组将大致对应于不同的音节。

##  词袋和 TF/IDF



在解决文本分类等任务时，我们需要能够用一个固定大小的向量表示文本，我们将其用作最终稠密分类器的输入。最简单的方法之一是组合所有单个单词表示，例如通过将它们相加。如果我们添加每个单词的一热编码，我们将得到一个频率向量，显示每个单词在文本中出现的次数。这种文本表示称为词袋 (BoW)。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/13-TextRep/images/bow.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/13-TextRep/images/bow.png)

> 作者的图片

BoW 基本上表示哪些单词出现在文本中以及以什么数量出现，这实际上可以很好地表明文本的内容。例如，有关政治的新闻文章可能包含总统和国家等词，而科学出版物则会有对撞机、发现等词。因此，在许多情况下，单词频率可以很好地指示文本内容。

BoW 的问题在于某些常用词，例如 and、is 等，出现在大多数文本中，并且它们的频率最高，掩盖了真正重要的单词。我们可以通过考虑单词在整个文档集合中出现的频率来降低这些单词的重要性。这是 TF/IDF 方法背后的主要思想，该方法在本课程附带的笔记本中进行了更详细的介绍。

但是，这些方法都不能完全考虑文本的语义。我们需要更强大的神经网络模型来做到这一点，我们将在本节后面讨论这一点。

## ✍️ 练习：文本表示



在以下笔记本中继续学习：

- [PyTorch 文本表示](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/13-TextRep/TextRepresentationPyTorch.ipynb)
- [TensorFlow 文本表示](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/13-TextRep/TextRepresentationTF.ipynb)

##  结论



到目前为止，我们已经学习了可以为不同的单词添加频率权重的技术。然而，它们无法表示含义或顺序。正如著名的语言学家 J. R. Firth 在 1935 年所说，“一个单词的完整含义始终是语境的，任何脱离语境的含义研究都不能被认真对待。”我们将在本课程后面学习如何使用语言建模从文本中捕获上下文信息。

##  🚀 挑战



尝试使用词袋和不同的数据模型进行一些其他练习。您可能会受到 Kaggle 上的这场比赛的启发

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/213)



##  复习与自学



在 Microsoft Learn 上练习你的文本嵌入和词袋技术技能

## [ 作业：笔记本](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/13-TextRep/assignment.md)
