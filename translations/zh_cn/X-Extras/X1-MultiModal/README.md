# 多模态网络



在 Transformer 模型成功解决 NLP 任务后，相同或类似的架构已应用于计算机视觉任务。人们越来越有兴趣构建能够结合视觉和自然语言功能的模型。其中一项尝试由 OpenAI 完成，称为 CLIP 和 DALL.E。

## 对比图像预训练 (CLIP)



CLIP 的主要思想是能够将文本提示与图像进行比较，并确定图像与提示的匹配程度。

[![CLIP Architecture](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/clip-arch.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/clip-arch.png)

> *来自此博文中的图片*

该模型在从互联网获得的图像及其标题上进行训练。对于每个批次，我们取 N 对（图像、文本），并将它们转换为一些向量表示 I 1 ,..., I N / T 1 , ..., T N 。然后将这些表示匹配在一起。损失函数被定义为最大化对应于一对（例如 I 和 T）的向量的余弦相似度，并最小化所有其他对之间的余弦相似度。这就是这种方法被称为对比度的原因。

CLIP 模型/库可从 OpenAI GitHub 获得。该方法在此博文中进行了描述，并在本文中进行了更详细的描述。

一旦该模型经过预训练，我们就可以给它一批图像和一批文本提示，它将返回概率张量。CLIP 可用于以下几个任务：

 **图像分类**

假设我们需要对图像进行分类，例如猫、狗和人类。在这种情况下，我们可以给模型一张图像和一系列文本提示：“一张猫的图片”、“一张狗的图片”、“一张人的图片”。在结果为 3 个概率的向量中，我们只需要选择具有最高值的索引。

[![CLIP for Image Classification](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/clip-class.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/clip-class.png)

> *来自此博文中的图片*

 **基于文本的图像搜索**

我们也可以做相反的事情。如果我们有一组图像，我们可以将这组图像传递给模型，并提供一个文本提示 - 这将为我们提供与给定提示最相似的图像。

## ✍️ 示例：使用 CLIP 进行图像分类和图像搜索



打开 Clip.ipynb 笔记本以查看 CLIP 的实际应用。

## 使用 VQGAN+CLIP 生成图像



CLIP 还可以用于根据文本提示生成图像。为了做到这一点，我们需要一个生成器模型，该模型能够根据一些矢量输入生成图像。其中一个这样的模型称为 VQGAN（矢量量化 GAN）。

VQGAN 的主要思想使其区别于普通 GAN 的如下：

- 使用自回归变换器架构生成一系列组成图像的富含上下文的视觉部分。这些视觉部分反过来又由 CNN 学习
- 使用子图像判别器来检测图像的各个部分是“真实的”还是“假的”（与传统 GAN 中的“全有或全无”方法不同）。

在 Taming Transformers 网站上了解有关 VQGAN 的更多信息。

VQGAN 和传统 GAN 之间的一个重要区别在于，后者可以从任何输入向量生成一个像样的图像，而 VQGAN 可能会生成一个不连贯的图像。因此，我们需要进一步指导图像创建过程，而这可以使用 CLIP 来完成。

[![VQGAN+CLIP Architecture](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/vqgan.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/vqgan.png)

要生成与文本提示相对应的图像，我们从一些随机编码向量开始，该向量通过 VQGAN 传递以生成图像。然后使用 CLIP 来生成一个损失函数，该函数显示图像与文本提示的对应程度。然后目标是使用反向传播来调整输入向量参数，从而最小化此损失。

实现 VQGAN+CLIP 的一个很棒的库是 Pixray

| [![Picture produced by Pixray](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/a_closeup_watercolor_portrait_of_young_male_teacher_of_literature_with_a_book.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/a_closeup_watercolor_portrait_of_young_male_teacher_of_literature_with_a_book.png) | [![Picture produced by pixray](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/a_closeup_oil_portrait_of_young_female_teacher_of_computer_science_with_a_computer.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/a_closeup_oil_portrait_of_young_female_teacher_of_computer_science_with_a_computer.png) | [![Picture produced by Pixray](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/a_closeup_oil_portrait_of_old_male_teacher_of_math.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/a_closeup_oil_portrait_of_old_male_teacher_of_math.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 从提示中生成图片：一本年轻男老师的文学水彩特写肖像           | 从提示中生成图片：一本年轻女老师的计算机科学油画特写肖像     | 从提示中生成图片：一本黑板前的老年男数学老师的油画特写肖像   |

> Dmitry Soshnikov 的人工智能教师系列图片

## DALL-E



### [DALL-E 1](https://openai.com/research/dall-e)



DALL-E 是 GPT-3 的一个版本，经过训练可以根据提示生成图像。它已经过 120 亿个参数的训练。

与 CLIP 不同，DALL-E 将文本和图像作为单个标记流接收，用于图像和文本。因此，您可以根据文本从多个提示生成图像。

### [DALL-E 2](https://openai.com/dall-e-2) 重试 错误原因



DALL.E 1 和 2 之间的主要区别在于，它生成更逼真的图像和艺术。

使用 DALL-E 生成的图像示例：

| [![Picture produced by Pixray](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/DALL%C2%B7E%202023-06-20%2015.56.56%20-%20a%20closeup%20watercolor%20portrait%20of%20young%20male%20teacher%20of%20literature%20with%20a%20book.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/DALL·E 2023-06-20 15.56.56 - a closeup watercolor portrait of young male teacher of literature with a book.png) | [![Picture produced by pixray](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/DALL%C2%B7E%202023-06-20%2015.57.43%20-%20a%20closeup%20oil%20portrait%20of%20young%20female%20teacher%20of%20computer%20science%20with%20a%20computer.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/DALL·E 2023-06-20 15.57.43 - a closeup oil portrait of young female teacher of computer science with a computer.png) | [![Picture produced by Pixray](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/X-Extras/X1-MultiModal/images/DALL%C2%B7E%202023-06-20%2015.58.42%20-%20%20a%20closeup%20oil%20portrait%20of%20old%20male%20teacher%20of%20mathematics%20in%20front%20of%20blackboard.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/X-Extras/X1-MultiModal/images/DALL·E 2023-06-20 15.58.42 -  a closeup oil portrait of old male teacher of mathematics in front of blackboard.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 根据提示生成的图片：年轻的男性文学老师的特写水彩肖像，拿着一本书 | 根据提示生成的图片：年轻的女计算机科学老师的特写油画肖像，拿着一台电脑 | 根据提示生成的图片：年老的男性数学老师在黑板前的特写油画肖像 |

##  参考文献



- VQGAN 论文：驯服用于高分辨率图像合成的 Transformer
- CLIP 论文：从自然语言监督中学习可迁移的视觉模型