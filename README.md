# 传统机器学习和深度学习方法解决医疗诊断文本分类问题
> * [对应的CSDN文章](https://blog.csdn.net/lengyue29/article/details/128791630)
> * [对应个人博客网站文章](https://alicewanttobackuw.github.io/2023/01/29/%E5%8C%BB%E7%96%97%E8%AF%8A%E6%96%AD%E6%96%87%E6%9C%AC%E5%A4%9A%E5%88%86%E7%B1%BB%E9%97%AE%E9%A2%98%EF%BC%88NLP\)%EF%BC%88%E5%90%88%E5%B7%A5%E5%A4%A7%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%EF%BC%89/)

## 1、介绍

​		针对自然语言处理来实现文本分类的常用的传统机器学习算法有很多，如：K-近邻算法、朴素贝叶斯算法、决策树以及集成学习方法之随机森林。而相应的深度学习算法也不少，如： TextCNN、FastText、DPCNN、TextRNN、TextRCNN以及Bert。

这里我采用传统机器学习方法**集成学习方法之随机森林**和深度学习算法**TextCNN和FastText**。

​		传统机器学习方法中，K-近邻算法虽然简单，易于理解，易于实现，还无需训练，但缺点很明显，它是一种懒惰算法，对测试样本分类时的计算量大，内存开销大，而且还需要手动指定K值， K值选择不当则分类精度不能保证。朴素贝叶斯算法虽然也比较简单，分类准确的较高，速度还比较快，但却有个较大的缺点，该算法由于使用了样本属性独立性的假设，所以如果特征属性有关联时其效果不好。由于我需要处理的是医疗诊断文本，其中有很多专业词是高度关联的，因此也不太适合。决策树通过信息增益等方法来进行作为分类依据，它的效果就明显，准确率高，不过最大的缺点就是，容易出过拟合。因此我选择采用随机森林算法，控制森林的高度和森林中树木的数量，确保准确率的同时有效防止过拟合。由于森林的高度和树木的数量属于超参数，所以我再加上一个网格搜索，实现自动调参，选择最合适的超参数，作为模型。

​		深度学习方法中，一听到CNN就会联想到图像处理邻域。而TextCNN创新之处就在于，通过将卷积神经网络CNN应用到文本分类任务中，利用多个不同size的kernel来提取句子中的关键信息。从而能够更好地捕捉局部相关性。与传统图像的CNN网络相比, textCNN 在网络结构上没有任何变化(甚至更加简单了)。虽然TextCNN网络结构简单 ,但在模型网络结构如此简单的情况下，通过引入已经训练好的词向量依旧有很不错的效果，在多项数据数据集上超越benchmark。所以，我选用了TextCNN算法。而FastText有它独特的优点。FastText是Facebook于2016年开源的一个词向量计算和文本分类工具，在学术上并没有太大创新。但是它的优点也非常明显，在文本分类任务中，FastText（浅层网络）往往能取得和深度网络相媲美的精度，却在训练时间上比深度网络快许多数量级。在标准的多核CPU上， 能够训练10亿词级别语料库的词向量在10分钟之内，能够分类有着30万多类别的50多万句子在1分钟之内。FastText的核心思想： 将整篇文档的词及n-gram向量叠加平均得到文档向量，然后使用文档向量做 softmax 多分类。这中间涉及到两个技巧：字符级 n-gram 特征的引入以及分层Softmax 分类。而且，官网给的文档还有指定时间自动调优的方法，所以我选用了FastText算法。

## 2、资源目录

src：包含样本数据文件gastic.xlsx和停用词stop_words.txt文件

gastic.xlsx预览（**共250条数据，5分类**）

![image-20221225125034624](/src/gastic预览.png)
