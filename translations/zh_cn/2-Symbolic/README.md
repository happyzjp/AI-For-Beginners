# 知识表示和专家系统

[![Summary of Symbolic AI content](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/sketchnotes/ai-symbolic.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/sketchnotes/ai-symbolic.png)

> Tomomi Imura 的速写笔记

人工智能的探索基于对知识的探索，以类似于人类的方式理解世界。但你如何去做呢？

### [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/102)

在人工智能的早期，自顶向下的创建智能系统的方法（在上节课中讨论过）很流行。这个想法是从人那里提取知识，转换成机器可读的形式，然后用它来自动解决问题。这种方法基于两个重要的思想：

- 知识表示
-  推理

###  知识表示

符号人工智能中一个重要的概念是**知识**。将知识与信息或数据区分开来非常重要。例如，人们可以说书中包含知识，因为人们可以学习书本并成为专家。然而，书中包含的内容实际上被称为数据，通过阅读书籍并将这些数据整合到我们的世界模型中，我们将这些数据转换为知识。

> ✅ **知识** 是我们头脑中包含的东西，代表我们对世界的理解。它是通过主动 **学习** 过程获得的，该过程将我们接收到的信息片段整合到我们对世界的主动模型中。

大多数情况下，我们不会严格定义知识，但我们会使用 [DIKW金字塔](https://en.wikipedia.org/wiki/DIKW_pyramid) 将其与其他相关概念联系起来。它包含以下概念：

- **数据** 是表示在物理介质中的东西，例如书面文本或口语。数据独立于人类存在，可以在人与人之间传递。
- **信息** 是我们在大脑中解释数据的方式。例如，当我们听到“计算机”这个词时，我们对它是什么有一些了解。
- **知识** 是信息被整合到我们的世界模型中的过程。例如，一旦我们了解了计算机是什么，我们就会开始对它的工作原理、成本以及用途有一些想法。这个相互关联的概念网络构成了我们的知识。
- **智慧** 是我们对世界理解的又一个层次，它代表了元知识，例如关于如何以及何时使用知识的一些概念。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/DIKW_Pyramid.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/DIKW_Pyramid.png)

*来自维基百科的图片，作者 Longlivetheux - 自有作品，知识共享署名-相同方式共享 4.0*

因此，知识表示的问题是要找到一种有效的方法，以数据形式在计算机中表示知识，使其能够自动使用。这可以看作是一个频谱：

[![Knowledge representation spectrum](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/knowledge-spectrum.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/knowledge-spectrum.png)

> 图片由 Dmitry Soshnikov 拍摄

- 在左侧，有一些非常简单的知识表示类型，计算机可以有效地使用它们。最简单的一种是算法，当知识由计算机程序表示时。然而，这不是表示知识的最佳方式，因为它不灵活。我们头脑中的知识通常是非算法的。
- 在右侧，有自然文本等表示。它是最强大的，但不能用于自动推理。

> ✅ 想一想你如何在你的头脑中表示知识并将其转换成笔记。是否有特定的格式可以帮助你保留知识？

### 计算机知识表示分类

我们可以将不同的计算机知识表示方法归类为以下类别：

- **网络表示** 基于这样一个事实：我们的头脑中有一个相互关联的概念网络。我们可以尝试在计算机中将相同的网络复制为一个图 - 一个所谓的**语义网络**。

**对象-属性-值三元组** 或 **属性-值对**。由于图可以在计算机中表示为节点和边的列表，因此我们可以通过包含对象、属性和值的列表来表示语义网络。例如，我们构建了以下有关编程语言的三元组：

| 对象         | 属性       | 值                    |
| ------------ | ---------- | --------------------- |
| Python       | 是         | 非类型化语言          |
| Python       | 发明者     | Guido van Rossum 发明 |
| Python       | 代码块语法 | 缩进                  |
| 非类型化语言 | 没有       | 类型定义              |

> ✅ 思考如何使用三元组来表示其他类型的知识。

**分层表示** 强调了这样一个事实：我们经常在头脑中创建对象的层次结构。例如，我们知道金丝雀是一种鸟，所有鸟都有翅膀。我们也对金丝雀通常是什么颜色以及它们的飞行速度有一些了解。

- **框架表示** 基于将每个对象或对象类表示为包含**插槽**的**框架**。插槽具有可能的默认值、值限制或可以调用的存储过程以获取插槽的值。所有框架都形成一个类似于面向对象编程语言中对象层次结构的层次结构。
- **场景** 是特殊类型的框架，表示可以随着时间推移而展开的复杂情况。

**Python**

| 插槽       | 值           | 默认值       | 间隔      |
| ---------- | ------------ | ------------ | --------- |
| 名称       | Python       |              |           |
| 是-A       | 非类型化语言 |              |           |
| 大小写混合 |              | 驼峰式命名法 |           |
| 程序长度   |              |              | 5-5000 行 |
| 块语法     | 缩进         |              |           |

**过程化表示** 是基于通过一系列动作来表示知识，当某个条件发生时，这些动作可以被执行。

- 产生规则是允许我们得出结论的 if-then 语句。例如，医生可以有一个规则，即如果患者发高烧或血液检查中 C 反应蛋白水平高，那么他就有炎症。一旦我们遇到其中一个条件，我们就可以对炎症做出结论，然后在进一步的推理中使用它。
- 算法可以被认为是另一种形式的程序化表示，尽管它们几乎从未直接用于基于知识的系统中。

**逻辑** 最初由亚里士多德提出，作为表示普遍人类知识的一种方式。

- 作为一种数学理论，谓词逻辑过于丰富而无法计算，因此通常使用它的某些子集，例如 Prolog 中使用的 Horn 子句。
- 描述逻辑是一系列逻辑系统，用于表示和推理关于对象层次结构的分布式知识表示，例如语义网。

###  专家系统

符号人工智能的早期成功之一是所谓的**专家系统**——计算机系统，旨在充当某些有限问题领域的专家。它们基于从一个或多个人类专家那里提取的**知识库**，并且包含一个在该知识库之上执行某些推理的**推理引擎**。

| [![Human Architecture](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/arch-human.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/arch-human.png) | [![Knowledge-Based System](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/arch-kbs.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/arch-kbs.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 人类神经系统的简化结构                                       | 基于知识的系统的架构                                         |

专家系统构建得像人类推理系统，其中包含短期记忆和长期记忆。类似地，在基于知识的系统中，我们区分以下组件：

- **问题记忆**：包含有关当前正在解决的问题的知识，例如患者的体温或血压、是否有炎症等。此知识也称为**静态知识**，因为它包含了我们当前对问题了解的快照——所谓的“问题状态”。
- **知识库**：表示对问题域的长期知识。它由人类专家手动提取，并且不会因咨询而改变。因为它允许我们从一个问题状态导航到另一个问题状态，所以它也被称为**动态知识**。
- **推理引擎**：协调在问题状态空间中搜索的整个过程，在必要时向用户提问。它还负责为每个状态找到要应用的正确规则。

作为一个例子，让我们考虑以下基于其物理特征确定动物的专家系统：

[![AND-OR Tree](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/AND-OR-Tree.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/AND-OR-Tree.png)

> 图片由 Dmitry Soshnikov 拍摄

这个图表被称为**与-或树**，它是一组产生规则的图形表示。绘制树对于从专家那里提取知识的初期阶段是有用的。为了在计算机内部表示知识，使用规则更加方便：

```
IF the animal eats meat
OR (animal has sharp teeth
    AND animal has claws
    AND animal has forward-looking eyes
) 
THEN the animal is a carnivore
```

你可以注意到规则左侧的每个条件和动作本质上都是对象-属性-值 (OAV) 三元组。**工作内存**包含与当前正在解决的问题相对应的 OAV 三元组集。**规则引擎**查找条件得到满足的规则并应用它们，将另一个三元组添加到工作内存中。

> ✅ 在你喜欢的主题上写出你自己的 AND-OR 树！

### 前向推理与反向推理

上面描述的过程称为前向推理。它从工作内存中关于问题的某些初始数据开始，然后执行以下推理循环：

1. 如果目标属性存在于工作内存中 - 停止并给出结果
2. 查找当前满足其条件的所有规则 - 获取规则冲突集。
3. 执行冲突解决 - 选择将在此步骤中执行的一条规则。可能有不同的冲突解决策略：
   - 选择知识库中第一个适用的规则
   - 选择一个随机规则
   - 选择一个更具体的规则，即满足“左侧”（LHS）中最多条件的规则
4. 应用选定的规则并将新知识插入问题状态
5. 从步骤 1 重复。

但是，在某些情况下，我们可能希望从对问题一无所知开始，并提出一些问题来帮助我们得出结论。例如，在进行医学诊断时，我们通常不会在开始诊断患者之前提前进行所有医学分析。我们宁愿在需要做出决定时进行分析。

可以使用**反向推理**对这个过程进行建模。它由**目标**驱动 - 我们正在寻找的属性值：

1. 选择所有可以为我们提供目标值（即在 RHS（“右侧”）上具有目标）的规则 - 冲突集
2. 如果没有针对此属性的规则，或者有一条规则说我们应该向用户询问值 - 询问它，否则：
3. 使用冲突解决策略选择一条我们将用作假设的规则 - 我们将尝试证明它
4. 对规则 LHS 中的所有属性重复此过程，尝试将它们证明为目标
5. 如果进程在任何时候失败 - 在步骤 3 中使用另一条规则。

> ✅ 在哪些情况下前向推理更合适？反向推理呢？

### 实施专家系统

专家系统可以使用不同的工具实施：

- 直接用某种高级编程语言对它们进行编程。这不是最好的主意，因为基于知识的系统的最大优势在于知识与推理是分开的，而且潜在的问题领域专家应该能够编写规则，而无需理解推理过程的细节
- 使用**专家系统 shell**，即专门设计为使用某种知识表示语言填充知识的系统。

##### ✍️ 练习：动物推理

请参阅[Animals.ipynb](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/2-Symbolic/Animals.ipynb)以获取实施前向和后向推理专家系统的示例。

> 注意：此示例相当简单，仅提供专家系统的外观概念。一旦开始创建这样的系统，只有在达到一定数量的规则（约 200 条以上）后，才会注意到其中的一些智能行为。在某些时候，规则会变得过于复杂，无法记住所有规则，此时你可能会开始疑惑系统为何做出某些决策。然而，基于知识的系统的最重要特征是，你始终可以准确解释任何决策是如何做出的。

### 本体和语义网

在 20 世纪末，有一项倡议是使用知识表示来注释互联网资源，以便可以找到与非常具体的查询相对应的资源。此动议被称为语义网，它依赖于几个概念：

- 一种基于描述逻辑 (DL) 的特殊知识表示。它类似于框架知识表示，因为它构建了一个具有属性的对象层次结构，但它具有形式逻辑语义和推理。DL 家族在推理的表达能力和算法复杂性之间取得平衡。
- 分布式知识表示，其中所有概念都由全局 URI 标识符表示，从而可以创建跨越互联网的知识层次结构。
- 用于知识描述的一系列基于 XML 的语言：RDF（资源描述框架）、RDFS（RDF 模式）、OWL（本体 Web 语言）。

语义网中的核心概念是本体的概念。它指的是使用某种形式知识表示对问题域进行明确规范。最简单的本体可以只是问题域中对象的层次结构，但更复杂的本体将包括可用于推理的规则。

在语义网中，所有表示都基于三元组。每个对象和每个关系都由 URI 唯一标识。例如，如果我们想说明这个 AI 课程是由 Dmitry Soshnikov 在 2022 年 1 月 1 日开发的，这里是我们能使用的三元组：

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/triplet.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/triplet.png)

```
http://github.com/microsoft/ai-for-beginners http://www.example.com/terms/creation-date “Jan 13, 2007”
http://github.com/microsoft/ai-for-beginners http://purl.org/dc/elements/1.1/creator http://soshnikov.com
```



> ✅ 在这里， `http://www.example.com/terms/creation-date` 和 `http://purl.org/dc/elements/1.1/creator` 是表达创建者和创建日期概念的一些众所周知且普遍接受的 URI。

在更复杂的情况下，如果我们想要定义创建者列表，我们可以使用 RDF 中定义的一些数据结构。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/triplet-complex.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/triplet-complex.png)

> 上图由 Dmitry Soshnikov 绘制

语义网的构建进程在某种程度上因搜索引擎和自然语言处理技术的成功而放缓，这些技术允许从文本中提取结构化数据。然而，在某些领域，仍然有大量的努力来维护本体和知识库。值得注意的几个项目：

- WikiData 是与维基百科关联的机器可读知识库集合。大多数数据都来自维基百科信息框，即维基百科页面中的结构化内容片段。您可以在 SPARQL 中查询 wikidata，SPARQL 是一种语义网的特殊查询语言。以下是一个示例查询，显示人类中最流行的眼睛颜色：

```
#defaultView:BubbleChart
SELECT ?eyeColorLabel (COUNT(?human) AS ?count)
WHERE
{
  ?human wdt:P31 wd:Q5.       # human instance-of homo sapiens
  ?human wdt:P1340 ?eyeColor. # human eye-color ?eyeColor
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
GROUP BY ?eyeColorLabel
```



- DBpedia 是另一个类似于 WikiData 的项目。

> ✅ 如果你想尝试构建自己的本体，或打开现有的本体，有一个很棒的可视化本体编辑器，名为 Protégé。下载它，或在线使用它。

[![img](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/2-Symbolic/images/protege.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/2-Symbolic/images/protege.png)

*Web Protégé 编辑器打开罗曼诺夫家族本体。截图来自 Dmitry Soshnikov*

### ✍️ 练习：一个家庭本体

参阅 [FamilyOntology.ipynb](https://github.com/Ezana135/AI-For-Beginners/blob/main/lessons/2-Symbolic/FamilyOntology.ipynb)，了解如何使用语义网技术推理家庭关系。我们将采用通用 GEDCOM 格式表示的家谱和一个家庭关系本体，并为给定的一组个体构建所有家庭关系的图。

请参阅[FamilyOntology.ipynb](https://github.com/Ezana135/AI-For-Beginners/blob/main/lessons/2-Symbolic/FamilyOntology.ipynb)以获取使用语义网络技术推理家庭关系的示例。我们将获取以常见GEDCOM格式表示的家谱以及家庭关系的本体，并为给定的个体构建所有家庭关系的图形。

###  Microsoft 概念图

在大多数情况下，本体都是手工精心创建的。然而，也可以从非结构化数据中挖掘本体，例如，从自然语言文本中。

微软研究院曾进行过一次这样的尝试，并由此产生了[微软概念图谱](https://blogs.microsoft.com/ai/microsoft-researchers-release-graph-that-helps-machines-conceptualize/?WT.mc_id=academic-77998-cacaste)。

它是一个大型实体集合，使用 `is-a` 继承关系将其分组在一起。它可以回答诸如“微软是什么？”这样的问题——答案可能是“一家公司，概率为 0.87，一个品牌，概率为 0.75”。

该图谱可以作为REST API使用，也可以作为一个包含所有实体对的大型可下载文本文件。

### ✍️ 练习：概念图

尝试使用 [MSConceptGraph.ipynb](https://github.com/happyzjp/AI-For-Beginners/blob/main/lessons/2-Symbolic/MSConceptGraph.ipynb) 笔记本，了解如何使用 Microsoft Concept Graph 将新闻文章分组到几个类别中。

#####  结论

如今，人工智能通常被认为是机器学习或神经网络的同义词。然而，人类也表现出明确的推理，这是神经网络目前无法处理的。在实际项目中，明确的推理仍然用于执行需要解释的任务，或者能够以受控方式修改系统的行为。

###  🚀 挑战

在与本课程关联的家庭本体笔记本中，有机会尝试其他家庭关系。尝试发现家谱中人物之间的新联系。

### [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/202)

###  复习与自学

在互联网上做一些研究，以发现人类尝试量化和编纂知识的领域。看看布鲁姆分类法，并回顾历史，了解人类如何尝试理解他们的世界。探索林奈的工作，以创建生物分类法，并观察德米特里·门捷列夫如何创造一种描述和分组化学元素的方法。你还能找到哪些其他有趣的例子？

**作业**: [构建本体](https://github.com/happyzjp/AI-For-Beginners/tree/main/translations/zh_cn/2-Symbolic/assignment.md)
