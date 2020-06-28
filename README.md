# Introduction-of-paper-KG_1<br>
This is introduction of paper "[Mning quality phrases from massive Text Corpora](https://dl.acm.org/doi/10.1145/2723372.2751523)"<br>
#**摘要**<br>
    文本数据（Text data）无处不在，并在大数据应用中起着至关重要的作用。然而，文本数据大多是非结构化的。将非结构化文本转化为结构化单元可以大大减少语义歧义，并易于数据库处理。因此，高质量短语的挖掘是数据库领域一个重要研究课题。本文提出了一种结合短语切分的文本语料库高质量短语提取框架。该框架只需要有限的训练，且这样生成的短语的质量接近于人类的判断。此外，该方法具有可扩展性：计算时间和所需空间均随语料大小的增加而线性增长。在大型文本语料库上的实验证明了该方法的有效性和质量。  
#**1.简介**<br>
    从语料库中提取高质量短语，在很多领域中都是一个重要任务，如科研，新闻，社交媒体，企业等。在这些大型、动态的文档集合中，分析人员通常对可变长度的短语感兴趣，包括科学概念、事件、组织、产品、口号等。高效提取高质量的短语使大量应用程序能够从单词粒度转换到短语粒度。包括“topic tracking[21]”,"OLAP on multi-dimensional text collection[40]","docment categorization"等。短语的抽取对于信息抽取是至关重要的，因为许多概念、实体和关系都表现在短语中。这项工作的研究起源于NLP，但将NLP工具应用在偏离严格的语言规则的大数据上也是一个引人注目的挑战，如应用在查询日志、社交媒体消息、文本事物记录上等。因此研究者们一直在寻找更通用的数据驱动方法，主要是基于频繁模式挖掘原理（frequent pattern mining）[2,34]。早期的工作集中在有效地检索重复出现的单词序列，但是许多这样的序列并没有形成有意义的短语。最近的工作会根据基于频率的统计信息对它们进行筛选或排序。然而，数据的原始频率往往会产生误导性的质量评估，并且结果不令人满意，如下例所示。  
##**例1**（Raw Frequency-based phrase mining)<br>
    考虑一组科学出版物和两个短语的原始频次计数(表1)，表中数字是假设的，但依然表明了一些事实：
* 短语越长，频次越低
* 好的短语与劣质短语都会有较高的频次
* 一个序列（例如“关系数据库系统”）及其子序列的频率可以具有与另一序列（例如“支持向量机”）及其对应序列相似的scale。
<br>
    显然，一个只根据频次对短语进行rank的方法会输出很多错误的短语，如“vector machine"。为了解决这个问题，提出了不同的启发式的原则，其基本思想是：“一个好的短语，它的频次会比它的子短语或者父短语高出很多”[29,12]。但该原则有很大的局限性。比如"support vector"和"vector machine"的频次为160和150，利用该原则可以在两个数字之间画线将其区分开来，但这条线对于 ‘relational database’ 和 ‘database system’是不适用的。这个例子说明，使用原始频次计数进行rank的约束，尤其是在遇到过长或过短的短语时。这是所有基于频次的质量评估的瓶颈。<br>
    本文解决了这个瓶颈，通过“修正”那些阻碍发现短语真实质量的决定性的原始频次。修正的目标是估计每个单词序列在其上下文中作为一个整体被描述、解释的次数。如下用一个例子来阐述这个idea。<br>
##**例2**（修正:rectification)<br>
     依然考虑表1中列出的6个单词序列。<br>
* A [relational database system] for images...
* [Database system] empowers everyone in your organization..
* More formally, a [support vector machine] constructs a hyperplane...
* The [support vector] method is a new general method of [function estimation]...
* A standard [feature vector] [machine learning] setup is used to describe...
* [Relevance vector machine] has an identical [functional form] to the [support vector machine]...
* The basic goal for [object-oriented relational database] is to [bridge the gap] between...
<br>
     前4个实例应向这些序列提供正计数，而后3个实例不应向“support vector”或“relational database”提供正计数，因为它们不应被解释为一个整体短语（相反，“feature vector”和“relevance vector machine”等序列可以）。假如可以正确的发现序列的出现，并收集修正的频次（表1的rectified列），修正的频次可以足够清晰的分辨"vector machine"或者其他短语，因为"vector machine"很少作为一个整体短语出现（表1所示，只出现了6次）。<br>
     这种方法的成功依赖于合理准确的修正。简单的原始频次算法，如用高质量的超序列计数减去一个序列的计数，容易出错。第一，哪个超序列是高质量序列本身就是一个问题。第二，判断一个序列是否是一个整体依赖于上下文环境。例如，例2的第5个例子中，"feature vector" and "machine learning" 的出现次数高于"vector machine"，but "feature vector machine" and "vector machine learning" are not quality phrase。如果只收集频次的话，上下文信息就丢失了。<br>
     为了尽最大努力恢复真实频次，我们应该检查每一个词序列出现的上下文，并决定是否将其作为短语。对一个序列出现事件的检查可能涉及列举替代可能性，例如扩展序列或拆分序列，并对它们进行比较。对单词序列出现率的测试可能很昂贵，失去了频繁模式挖掘方法在效率上的优势。<br>
