# NER



人工智能初学者课程的实验作业。

##  任务



在这个实验中，你需要训练医学术语的命名实体识别模型。

##  数据集



要训练 NER 模型，我们需要有正确标记的医学实体数据集。BC5CDR 数据集包含来自 1500 多篇论文的标记疾病和化学实体。您可以在其网站上注册后下载该数据集。

BC5CDR 数据集如下所示：

```
6794356|t|Tricuspid valve regurgitation and lithium carbonate toxicity in a newborn infant.
6794356|a|A newborn with massive tricuspid regurgitation, atrial flutter, congestive heart failure, and a high serum lithium level is described. This is the first patient to initially manifest tricuspid regurgitation and atrial flutter, and the 11th described patient with cardiac disease among infants exposed to lithium compounds in the first trimester of pregnancy. Sixty-three percent of these infants had tricuspid valve involvement. Lithium carbonate may be a factor in the increasing incidence of congenital heart disease when taken during early pregnancy. It also causes neurologic depression, cyanosis, and cardiac arrhythmia when consumed prior to delivery.
6794356	0	29	Tricuspid valve regurgitation	Disease	D014262
6794356	34	51	lithium carbonate	Chemical	D016651
6794356	52	60	toxicity	Disease	D064420
...
```



在此数据集中，前两行是论文标题和摘要，然后是各个实体，以及标题 + 摘要块中的开始和结束位置。除了实体类型外，您还可以在某些医学本体中获取此实体的本体 ID。

您需要编写一些 Python 代码将其转换为 BIO 编码。

##  网络



第一次尝试 NER 可以使用 LSTM 网络，就像您在课程中看到的示例一样。然而，在 NLP 任务中，transformer 架构，特别是 BERT 语言模型显示出更好的结果。预训练的 BERT 模型了解语言的一般结构，并且可以通过相对较小的数据集和计算成本针对特定任务进行微调。

由于我们计划将 NER 应用于医学场景，因此使用在医学文本上训练的 BERT 模型是有意义的。Microsoft Research 发布了一个名为 [PubMedBERT][PubMedBERT] ([出版物][PubMedBERT-Pub]) 的预训练模型，该模型使用 PubMed 存储库中的文本进行了微调。

训练 Transformer 模型的事实标准是 Hugging Face Transformers 库。它还包含一个由社区维护的预训练模型存储库，包括 PubMedBERT。要加载并使用此模型，我们只需要几行代码：

```
model_name = "microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract"
classes = ... # number of classes: 2*entities+1
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = BertForTokenClassification.from_pretrained(model_name, classes)
```



这给了我们 `model` 本身，它使用 `classes` 类的数量构建用于标记分类任务，以及可以将输入文本拆分为标记的 `tokenizer` 对象。您需要将数据集转换为 BIO 格式，同时考虑 PubMedBERT 标记化。您可以使用这段 Python 代码作为灵感。

##  外卖



如果您想深入了解大量自然语言文本，这项任务非常接近您可能拥有的实际任务。在我们的案例中，我们可以将训练好的模型应用于与 COVID 相关的论文数据集，看看我们能够获得哪些见解。这篇博文和这篇论文描述了可以使用 NER 在这组论文上进行的研究。
