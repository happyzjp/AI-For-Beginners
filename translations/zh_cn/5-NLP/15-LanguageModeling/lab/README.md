# 训练 Skip-Gram 模型



人工智能初学者课程的实验作业。

##  任务



在本实验中，我们挑战您使用 Skip-Gram 技术训练 Word2Vec 模型。训练一个带有嵌入的网络，以预测 � 个标记宽的 Skip-Gram 窗口中的相邻单词。您可以使用本课程中的代码，并对其进行一些修改。

##  数据集



欢迎您使用任何书籍。您可以在古腾堡计划中找到许多免费文本，例如，这里有刘易斯·卡罗尔的《爱丽丝漫游仙境》的直接链接）。或者，您可以使用莎士比亚的戏剧，您可以使用以下代码获取这些戏剧：

```
path_to_file = tf.keras.utils.get_file(
   'shakespeare.txt', 
   'https://storage.googleapis.com/download.tensorflow.org/data/shakespeare.txt')
text = open(path_to_file, 'rb').read().decode(encoding='utf-8')
```



##  探索！



如果您有时间并且想更深入地了解这个主题，请尝试探索以下几个方面：

- 嵌入大小如何影响结果？
- 不同的文本样式如何影响结果？
- 选取几种非常不同的单词及其同义词，获取它们的向量表示，应用 PCA 将维度降至 2，并将它们绘制在 2D 空间中。您能看到任何模式吗？