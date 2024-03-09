# 命名实体识别



到目前为止，我们主要集中于一项 NLP 任务 - 分类。然而，还有其他 NLP 任务可以使用神经网络来完成。其中一项任务是命名实体识别 (NER)，它处理识别文本中的特定实体，例如地点、人名、日期时间间隔、化学公式等。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/119)



##  使用 NER 的示例



假设您想开发一个类似于 Amazon Alexa 或 Google Assistant 的自然语言聊天机器人。智能聊天机器人工作的原理是通过对输入句子进行文本分类来理解用户想要什么。此分类的结果是所谓的意图，它决定了聊天机器人应该做什么。

[![Bot NER](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/19-NER/images/bot-ner.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/19-NER/images/bot-ner.png)

> 作者的图片

但是，用户可能会提供一些参数作为短语的一部分。例如，在询问天气时，她可能会指定一个位置或日期。机器人应该能够理解这些实体，并在执行操作之前相应地填写参数槽。这正是 NER 的用武之地。

> ✅ 另一个示例是分析科学医学论文。我们需要关注的主要内容之一是特定医学术语，例如疾病和医学物质。虽然可以使用子字符串搜索提取少量疾病，但更复杂的实体（例如化合物和药物名称）需要更复杂的方法。

## NER 作为标记分类



NER 模型本质上是标记分类模型，因为对于每个输入标记，我们需要决定它是否属于实体，如果是，则属于哪个实体类。

考虑以下论文标题：

新生儿三尖瓣关闭不全和碳酸锂中毒。

 此处的实体为：

- 三尖瓣关闭不全是一种疾病（ `DIS` ）
- 碳酸锂是一种化学物质 ( `CHEM` )
- 毒性也是一种疾病 ( `DIS` )

注意一个实体可以跨越多个标记。并且，就像在这个例子中，我们需要区分两个连续的实体。因此，通常为每个实体使用两个类别——一个指定实体的第一个标记（通常使用 `B-` 前缀，表示开头），另一个指定实体的延续（ `I-` ，表示内部标记）。我们还使用 `O` 作为类别来表示所有其他标记。这种标记标记称为 BIO 标记（或 IOB）。标记后，我们的标题将如下所示：

| 标记                          | 标签   |
| ----------------------------- | ------ |
| 三尖瓣                        | B-DIS  |
| 瓣膜                          | I-DIS  |
| 反流                          | I-DIS  |
| 和                            | O      |
| 碳酸锂                        | B-CHEM |
| 毒性                          | B-DIS  |
| toxicity                      | B-DIS  |
| in                            | O      |
| a                             | O      |
| 新生儿<Keep This Symbol> 婴儿 | O      |
| infant                        | O      |
| .                             | O      |

由于我们需要在标记和类别之间建立一一对应的关系，我们可以根据此图片训练一个最右边的多对多神经网络模型：

[![Image showing common recurrent neural network patterns.](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/unreasonable-effectiveness-of-rnn.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/images/unreasonable-effectiveness-of-rnn.jpg)

> *Andrej Karpathy 的这篇博文中的图片。NER 标记分类模型对应于此图片中最右边的网络架构。*

##  训练 NER 模型



由于 NER 模型本质上是一个标记分类模型，因此我们可以使用我们已经熟悉的 RNN 来执行此任务。在这种情况下，每个循环网络块将返回标记 ID。以下示例笔记本展示了如何训练 LSTM 进行标记分类。

## ✍️ 示例笔记本：NER



在以下笔记本中继续学习：

- [ TensorFlow 中的 NER](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/19-NER/NER-TF.ipynb)

##  结论



NER 模型是一种标记分类模型，这意味着它可用于执行标记分类。这是 NLP 中一项非常常见的任务，有助于识别文本中的特定实体，包括地点、名称、日期等。

##  🚀 挑战



完成下方链接的作业，以训练一个用于医学术语的命名实体识别模型，然后在不同的数据集上对其进行尝试。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/219)



##  复习与自学



阅读博客文章《循环神经网络的非凡效能》，并按照该文章中的延伸阅读部分进行操作，以加深您的知识。

## [ 作业](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/19-NER/lab/README.md)



在本课的作业中，您将必须训练一个医学实体识别模型。您可以按照本课中所述开始训练 LSTM 模型，然后继续使用 BERT Transformer 模型。阅读说明以获取所有详细信息。
