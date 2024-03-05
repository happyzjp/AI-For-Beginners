# 人工智能介绍和历史



[![Summary of Introduction of AI content in a doodle](https://github.com/happyzjp/AI-For-Beginners/raw/main/lessons/sketchnotes/ai-intro.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/sketchnotes/ai-intro.png)

> Tomomi Imura 的速写笔记

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/101)

**人工智能**是一门令人兴奋的科学学科，研究如何使计算机表现出智能行为，例如执行人类擅长的任务。

最初，计算机是由 [查尔斯·巴贝奇（Charles Babbage）](https://en.wikipedia.org/wiki/Charles_Babbage) 发明的，目的是按照明确定义的步骤 - 即算法 - 处理数字。即使现代计算机比19世纪提出的原始模型先进得多，但仍然遵循受控计算的相同理念。因此，如果我们知道实现目标所需的准确步骤序列，就可以对计算机进行编程以执行某些操作。

[![Photo of a person](https://github.com/happyzjp/AI-For-Beginners/raw/main/lessons/1-Intro/images/dsh_age.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/1-Intro/images/dsh_age.png)

> 摄影：Vickie Soshnikova

> ✅ 从一个人的照片中确定他或她的年龄是一项无法明确编程的任务，因为我们不知道我们在执行此操作时如何在脑中生成一个数字。

------

然而，有些任务是我们不知道如何明确解决的。考虑从一个人的照片中判断他的/她的年龄。我们不知不觉地学会了如何做到这一点，因为我们已经看到了许多不同年龄的人的例子，但我们无法明确解释我们是如何做到的，我们也无法对计算机进行编程来做到这一点。这正是**人工智能**（简称AI）感兴趣的任务类型。

✅ 考虑一些你可以将 AI 应用于其中并从中受益的任务。考虑金融、医学和艺术领域——这些领域今天如何从 AI 中受益？

##### 弱人工智能与强人工智能

| 弱人工智能                                                   | 强人工智能                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 弱人工智能是指专为特定任务或狭窄任务而设计和训练的人工智能系统。 | 强人工智能，或称为人工通用智能（AGI），是指具有人类水平智能和理解能力的AI系统。 |
| 这些人工智能系统通常不具备普遍智能；它们擅长执行预定义的任务，但缺乏真正的理解或意识。 | 这些人工智能系统能够执行人类可以执行的任何智力任务，适应不同的领域，并具有一定形式的意识或自我意识。 |
| 弱人工智能的例子包括虚拟助手如Siri或Alexa，流媒体服务使用的推荐算法，以及专为特定客户服务任务而设计的聊天机器人。 | 实现强人工智能是人工智能研究的长期目标，需要开发能够在广泛任务和背景下进行推理、学习、理解和适应的AI系统。 |
| 弱人工智能是高度专业化的，不具备人类般的认知能力或超出其狭窄领域的一般问题解决能力。 | 目前，强人工智能是一个理论概念，尚未有AI系统达到这种广泛智能水平。 |

更多信息请参阅 [人工通用智能](https://en.wikipedia.org/wiki/Artificial_general_intelligence)（AGI）。

##### 智能的定义和图灵测试

[
智能](https://en.wikipedia.org/wiki/Intelligence) 这个术语的一个问题是没有明确的定义。有人认为智能与**抽象思维**或**自我意识**有关，但我们无法对其进行恰当的定义。

[![Photo of a Cat](https://github.com/happyzjp/AI-For-Beginners/raw/main/lessons/1-Intro/images/photo-cat.jpg)](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/1-Intro/images/photo-cat.jpg)

> [照片](https://unsplash.com/photos/75715CVEJhI)由[Amber Kipp](https://unsplash.com/@sadmax)提供

要了解术语“智能”的模糊性，请尝试回答一个问题：“猫有智能吗？”。不同的人倾向于对这个问题给出不同的答案，因为没有普遍接受的测试来证明这个说法是否真实。如果你认为有 - 试着让你的猫参加智商测试...

✅ 花一分钟时间思考一下你如何定义智能。能够解决迷宫并获得食物的乌鸦有智能吗？孩子有智能吗？

------

当谈论 AGI 时，我们需要某种方法来判断我们是否创造了一个真正智能的系统。[Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing)提出了一种称为**[图灵测试](https://en.wikipedia.org/wiki/Turing_test)**的方式，它也充当了智能的定义。该测试将给定的系统与本质上智能的东西（真实的人类）进行比较，并且由于任何自动比较都可以被计算机程序绕过，我们使用人类询问者。因此，如果一个人无法在基于文本的对话中区分真人和计算机系统，则该系统被认为是智能的。

> 2014年，由圣彼得堡开发的名为[Eugene Goostman](https://en.wikipedia.org/wiki/Eugene_Goostman)的聊天机器人通过使用一种聪明的个性化技巧接近通过了图灵测试。它一开始就宣布自己是一个 13 岁的乌克兰男孩，这解释了其缺乏知识和文本中的一些不一致之处。在5分钟的对话后，这个机器人说服了30%的评委认为它是人类，图灵认为机器到 2000 年将能够通过这一指标。然而，人们应该明白，这并不表明我们已经创造出一个智能系统，或者计算机系统欺骗了人类问询者 - 系统并没有欺骗人类，而是机器人的创建者欺骗了人类！

✅ 你是否曾经被聊天机器人愚弄，以为自己正在与人类交谈？它是如何说服你的？

## 人工智能的不同方法

如果我们希望计算机像人类一样行事，我们需要在计算机内部以某种方式模拟我们的思维方式。因此，我们需要尝试理解是什么让人类变得聪明。

> 为了能够将智能编程到机器中，我们需要了解我们自己的决策过程是如何工作的。如果你稍微自我反省一下，你就会意识到有一些过程是下意识发生的——例如，我们可以不假思索地将猫和狗区分开来——而另一些过程则涉及推理。

对于这个问题，有两种可能的方法：

| 自顶向下方法（符号推理）                                     | 自下而上的方法（神经网络）                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 自上而下的方法模拟人解决问题时的推理方式。它涉及从人类那里提取**知识**，并以计算机可读的形式表示它。我们还需要开发一种在计算机内部建模**推理**的方法。 | 自下而上的方法模拟人脑的结构，它由大量称为**神经元**的简单单元组成。每个神经元都像其输入的加权平均值一样起作用，通过提供**训练数据**，我们可以训练神经元网络来解决实际问题。 |

还有一些其他可能的智能方法：

- 一个**涌现**、**协同**或**多主体方法**基于这样一个事实，即通过大量简单主体的相互作用可以获得复杂的智能行为。根据[进化控制论](https://en.wikipedia.org/wiki/Global_brain#Evolutionary_cybernetics)，智能可以在元系统转换过程中从更简单的反应行为中产生。
- **进化方法**或**遗传算法**是一种基于进化原理的优化过程。

我们将在本课程的后面考虑这些方法，但现在我们将重点放在两个主要方向：自上而下和自下而上。

###  自上向下方法

在**自上而下方法**中，我们尝试对我们的推理进行建模。因为我们可以跟随我们的思维进行推理，我们可以尝试将这个过程形式化并将其编程到计算机中。这被称为**符号推理**。

人们倾向于在头脑中有一些规则来指导他们的决策过程。例如，当医生诊断病人时，他们可能会意识到患者发烧了，因此身体内部可能有一些炎症。通过将一大组规则应用于一个具体的问题，医生可能能够得出最终的诊断。

这种方法在很大程度上依赖于**知识表示**和**推理**。从人类专家那里提取知识可能是最困难的部分，因为在许多情况下，医生并不知道为什么他们会得出特定的诊断。有时，解决方案只是在他们的脑海中出现，而没有明确的思考。有些任务，比如从照片中确定一个人的年龄，根本无法简化为知识操作。

###  自下而上的方法

或者，我们可以尝试对我们大脑中最简单的元素——神经元进行建模。我们可以在计算机中构建一个所谓的**人工神经网络**，然后尝试通过提供示例来教它解决问题。这个过程类似于新生儿通过观察来学习周围环境的方式。

✅ 做一些关于婴儿如何学习的研究。婴儿大脑的基本元素是什么？

> | 什么是机器学习？                                             |                                                              |
> | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | 人工智能的一部分，它基于计算机学习来根据某些数据解决问题，称为**机器学习**。在本课程中，我们将不考虑传统的机器学习 - 我们建议你参考单独的[机器学习入门](http://aka.ms/ml-beginners)课程。 | [![ML for Beginners](https://github.com/happyzjp/AI-For-Beginners/raw/main/lessons/1-Intro/images/ml-for-beginners.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/1-Intro/images/ml-for-beginners.png) |

## 人工智能简史

人工智能始于二十世纪中叶。最初，符号推理是一种流行的方法，它取得了许多重要的成功，例如专家系统——能够在某些有限的问题领域充当专家的计算机程序。然而，很快人们就发现这种方法的可扩展性不佳。从专家那里提取知识，在计算机中表示它，并保持该知识库的准确性是一项非常复杂的任务，在许多情况下过于昂贵而不切实际。这导致了 20 世纪 70 年代所谓的[人工智能寒冬”](https://en.wikipedia.org/wiki/AI_winter)。

[![Brief History of AI](https://github.com/happyzjp/AI-For-Beginners/raw/main/lessons/1-Intro/images/history-of-ai.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/1-Intro/images/history-of-ai.png)

> 图片作者：Dmitry Soshnikov

随着时间的推移，计算资源变得更加便宜，可用的数据变得更加丰富，因此神经网络方法开始在与人类在许多领域的竞争中表现出巨大的性能，例如计算机视觉或语音理解。在过去十年中，术语人工智能主要用作神经网络的同义词，因为我们听到的大多数人工智能成功案例都是基于它们。

我们可以观察到方法是如何改变的，例如，在创建国际象棋博弈计算机程序方面：

- 早期的国际象棋程序基于搜索——程序明确地尝试估计对手在给定数量的下一步棋中的可能走法，并根据可以在几步棋中达到的最佳位置选择最佳走法。这导致了所谓的[alpha-beta修剪](https://en.wikipedia.org/wiki/Alpha–beta_pruning)搜索算法的发展。
- 搜索策略在游戏结束时效果很好，因为搜索空间受到少量可能的移动限制。然而，在游戏开始时，搜索空间很大，可以通过从人类玩家之间的现有比赛中学习来改进算法。后续实验采用了所谓的[基于案例的推理](https://en.wikipedia.org/wiki/Case-based_reasoning)，其中程序在知识库中寻找与游戏中当前位置非常相似的案例。
- 击败人类玩家的现代程序基于神经网络和[强化学习](https://en.wikipedia.org/wiki/Reinforcement_learning)，其中程序通过长时间与自己对弈并从自己的错误中学习来学会下棋——这很像人类学习下棋的方式。然而，计算机程序可以在更短的时间内玩更多游戏，因此可以学习得更快。

✅ 研究一下 AI 玩过的其他游戏。

同样，我们可以看到创建“对话程序”（可能通过图灵测试）的方法是如何改变的：

- 早期的这类程序（如 [Eliza](https://en.wikipedia.org/wiki/ELIZA)）基于非常简单的语法规则，并将输入句子重新表述为一个问题。
- 现代助手（如 Cortana、Siri 或 Google Assistant）都是混合系统，它们使用神经网络将语音转换为文本并识别我们的意图，然后采用一些推理或显式算法来执行所需的操作。
- 在未来，我们可能会期待一个完全基于神经网络的模型来自己处理对话。最近的 GPT 和 [Turing-NLG](https://turing.microsoft.com/) 神经网络家族在这方面取得了巨大的成功。

[![the Turing test's evolution](https://github.com/happyzjp/AI-For-Beginners/raw/main/lessons/1-Intro/images/turing-test-evol.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/1-Intro/images/turing-test-evol.png)

> 图片由 Dmitry Soshnikov 拍摄，照片由 Marina Abrosimova 拍摄，Unsplash

##  最近的人工智能研究

最近神经网络研究的巨大增长始于 2010 年左右，当时大型公共数据集开始变得可用。一个名为 [ImageNet](https://en.wikipedia.org/wiki/ImageNet) 的包含约 1400 万张带注释图像的巨大图像集合，催生了 [ImageNet大规模视觉识别挑战赛](https://image-net.org/challenges/LSVRC/)。

[![ILSVRC Accuracy](https://github.com/happyzjp/AI-For-Beginners/raw/main/lessons/1-Intro/images/ilsvrc.gif)](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/1-Intro/images/ilsvrc.gif)

> 图片由 Dmitry Soshnikov 拍摄

2012 年，[卷积神经网络](https://chat.openai.com/4-ComputerVision/07-ConvNets/README.md)首次用于图像分类，这导致分类错误率大幅下降（从近 30% 降至 16.4%）。2015 年，微软研究院的 ResNet 架构[实现了人类水平的准确性](https://doi.org/10.1109/ICCV.2015.123)。

从那时起，神经网络在许多任务中表现出非常成功的行为：

------

| 年   | 达到人类同等水平                                   |
| ---- | -------------------------------------------------- |
| 2015 | [ 图像分类](https://doi.org/10.1109/ICCV.2015.123) |
| 2016 | [会话语音识别](https://arxiv.org/abs/1610.05256)   |
| 2018 | 自动机器翻译（中文到英文）                         |
| 2020 | [ 图像字幕](https://arxiv.org/abs/2009.13682)      |

在过去的几年里，我们见证了大型语言模型（例如 BERT 和 GPT-3）取得了巨大的成功。这主要归功于大量通用文本数据的可用性，这些数据使我们能够训练模型以捕捉文本的结构和含义，在通用文本集合上对它们进行预训练，然后针对更具体的任务对这些模型进行专门化。我们将在本课程的后面部分详细了解[自然语言处理](https://github.com/happyzjp/AI-For-Beginners/5-NLP/README.md)。

##  🚀 挑战

在互联网上进行一番游览，以确定在你看来，人工智能在哪里得到最有效的利用。是在地图应用程序中，还是在一些语音转文本服务或视频游戏中？研究该系统是如何构建的。

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/201)

## 复习与自学

通过阅读[本课程](https://github.com/microsoft/ML-For-Beginners/tree/main/1-Introduction/2-history-of-ML)回顾人工智能和机器学习的历史。从本课程或上一课程顶部的手绘笔记中选取一个元素，并深入研究它，以了解影响其演变的文化背景。

 作业：[游戏果酱](https://github.com/happyzjp/AI-For-Beginners/tree/main/translations/zh_cn/assignment.md)
