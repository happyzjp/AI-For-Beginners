# 语言建模



语义嵌入（如 Word2Vec 和 GloVe）实际上是朝着语言建模迈出的第一步，即创建以某种方式理解（或表示）语言本质的模型。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/115)



语言建模背后的主要思想是以无监督的方式在未标记数据集上对其进行训练。这很重要，因为我们有大量的未标记文本可用，而标记文本的数量将始终受到我们可在标记上花费的精力限制。大多数情况下，我们可以构建可以预测文本中缺失单词的语言模型，因为在文本中屏蔽一个随机单词并将其用作训练样本很容易。

##  训练嵌入



在我们的前一个示例中，我们使用了预训练的语义嵌入，但了解如何训练这些嵌入很有趣。有几个可能的想法可以使用：

- N-Gram 语言建模，当我们通过查看 N 个前一个标记（N-gram）来预测一个标记时
- 连续词袋模型 (CBoW)，当我们预测一个标记序列 �−� , ..., �� 中的中间标记 �0 时。
- Skip-gram，其中我们从中间标记 �0 预测一组相邻标记 {$W_{-N},\dots, W_{-1}, W_1,\dots, W_N$}。

[![image from paper on converting words to vectors](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/14-Embeddings/images/example-algorithms-for-converting-words-to-vectors.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/14-Embeddings/images/example-algorithms-for-converting-words-to-vectors.png)

>  本文中的图片

## ✍️ 示例笔记本：训练 CBoW 模型



在以下笔记本中继续学习：

- [使用 TensorFlow 训练 CBoW Word2Vec](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/15-LanguageModeling/CBoW-TF.ipynb)
- [使用 PyTorch 训练 CBoW Word2Vec](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/15-LanguageModeling/CBoW-PyTorch.ipynb)

##  结论



在上节课中，我们已经看到词嵌入就像魔术一样！现在我们知道训练词嵌入并不是一项非常复杂的任务，并且我们应该能够根据需要为特定领域的文本训练我们自己的词嵌入。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/215)



##  复习与自学



- 官方 PyTorch 语言建模教程。
- 官方 TensorFlow Word2Vec 模型训练教程。
- 本说明档中介绍了使用 gensim 框架通过几行代码训练最常用的嵌入。

## 🚀 任务：训练 Skip-Gram 模型



在实验中，我们挑战您修改本课程中的代码以训练跳跃图模型，而不是 CBoW。阅读详细信息
