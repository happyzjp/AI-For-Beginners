# 自然语言处理



[![Summary of NLP tasks in a doodle](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/sketchnotes/ai-nlp.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/sketchnotes/ai-nlp.png)

在本节中，我们将重点关注使用神经网络来处理与自然语言处理 (NLP) 相关的任务。有许多 NLP 问题是我们希望计算机能够解决的：

- 文本分类是一个典型的分类问题，涉及文本序列。示例包括将电子邮件消息分类为垃圾邮件与非垃圾邮件，或将文章分类为体育、商业、政治等。此外，在开发聊天机器人时，我们通常需要理解用户想要说什么——在这种情况下，我们正在处理意图分类。通常，在意图分类中，我们需要处理许多类别。
- 情感分析是一个典型的回归问题，我们需要分配一个数字（情感），对应于句子的含义是积极/消极的程度。情感分析的更高级版本是基于方面的观点分析 (ABSA)，我们在此将观点归因于整个句子，而不是其不同部分（方面），例如。在这家餐厅，我喜欢美食，但气氛很糟糕。
- 命名实体识别 (NER) 指从文本中提取特定实体的问题。例如，我们可能需要理解在短语 I need to fly to Paris tomorrow 中，单词 tomorrow 指代 DATE，而 Paris 是 LOCATION。
- 关键词提取类似于 NER，但我们需要自动提取对句子含义重要的单词，而无需针对特定实体类型进行预训练。
- 当我们想要将相似的句子分组在一起时，文本聚类可能很有用，例如，技术支持对话中的相似请求。
- 问答是指模型回答特定问题的能力。该模型接收文本段落和问题作为输入，它需要在文本中提供包含问题答案的位置（或有时生成答案文本）。
- 文本生成是模型生成新文本的能力。它可以被视为基于一些文本提示预测下一个字母/单词的分类任务。高级文本生成模型（如 GPT-3）能够使用称为提示编程或提示工程的技术解决其他 NLP 任务，例如分类
- 文本摘要是一种技术，当我们希望计算机“阅读”长文本并将其总结为几句话时，可以使用这种技术。
- 机器翻译可以看作是理解一种语言的文本，并生成另一种语言的文本的组合。

最初，大多数 NLP 任务都是使用传统方法（如语法）来解决的。例如，在机器翻译中，解析器用于将初始句子转换为语法树，然后提取更高级别的语义结构来表示句子的含义，并基于此含义和目标语言的语法来生成结果。如今，许多 NLP 任务使用神经网络可以更有效地解决。

> 许多经典的 NLP 方法在 Natural Language Processing Toolkit (NLTK) Python 库中实现。网上有一本很棒的 NLTK 书籍，介绍了如何使用 NLTK 解决不同的 NLP 任务。

在我们的课程中，我们将主要专注于使用神经网络进行 NLP，并且在需要时我们将使用 NLTK。

我们已经了解了如何使用神经网络处理表格数据和图像。这些类型的数据与文本之间的主要区别在于，文本是可变长度的序列，而图像的输入大小是预先已知的。虽然卷积网络可以从输入数据中提取模式，但文本中的模式更为复杂。例如，我们可能会有否定从主题中分离出来，对于许多单词来说是任意的（例如，我不喜欢橘子，与我不喜欢那些又大又五颜六色又好吃的橘子），并且仍然应该将其解释为一个模式。因此，为了处理语言，我们需要引入新的神经网络类型，例如循环网络和转换器。

##  安装库



如果您使用本地 Python 安装来运行此课程，则可能需要使用以下命令安装所有必需的 NLP 库：

 **对于 PyTorch**

```
pip install -r requirements-torch.txt
```



 **适用于 TensorFlow**

```
pip install -r requirements-tf.txt
```



> 您可以在 Microsoft Learn 上使用 TensorFlow 尝试 NLP

##  GPU 警告



在本节中，在一些示例中，我们将训练相当大的模型。建议在支持 GPU 的计算机上运行笔记本，以最大程度地减少等待时间。

在 GPU 上运行时，您可能会遇到 GPU 内存不足的情况。在训练期间，消耗的 GPU 内存量取决于许多因素，包括小批量大小。如果您遇到任何内存问题，您可以尝试最小化代码中的小批量大小。

此外，如果您在一个 Python 内核中训练多个模型，一些较旧版本的 TensorFlow 无法正确释放 GPU 内存。为了谨慎使用 GPU 内存，您可以将 TensorFlow 选项设置为仅在需要时增加 GPU 内存分配。您需要在笔记本中包含以下代码：

```
physical_devices = tf.config.list_physical_devices('GPU') 
if len(physical_devices)>0:
    tf.config.experimental.set_memory_growth(physical_devices[0], True) 
```



如果您有兴趣从经典 ML 角度了解 NLP，请访问这套课程

##  在本节中



在本节中，我们将学习： <Keep This Symbol> 感知器，最早的神经网络模型之一，用于两类分类 <Keep This Symbol> 多层网络，以及如何构建我们自己的框架的配对笔记本

- [将文本表示为张量](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/13-TextRep/README.md)
- [ 词嵌入](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/14-Emdeddings/README.md)
- [ 语言建模](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/15-LanguageModeling/README.md)
- [循环神经网络](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/16-RNN/README.md)
- [ 生成式网络](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/17-GenerativeNetworks/README.md)
- [ 变形金刚](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/5-NLP/18-Transformers/README.md)
