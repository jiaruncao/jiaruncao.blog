## LDA主题模型

### 什么是LDA主题模型？
Latent Dirichlet Allocation:  

* 是一种无监督的贝叶斯模型
* 是一种主题模型，它可以将文档集中每篇文档的主题按照概率分布的形式给出。同时它是一种无监督学习算法，在训练时不需要手工标注的训练集，需要的仅仅是文档集以及指定主题的数k即可。此外LDA的另一个优点则是，对于每个主题均可找出一些词语来描述它。
* 是一种典型的词袋模型，即它认为一篇文档是由一组词构成的一个集合，词与词之间没有顺序以及先后的关系。一篇文档可以包含多个主题，文档中每个词都由其中的一个主题生成。  
  
 
  
### LDA生成过程  
  
![PicName](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/Chapter3-topic%20model/formula/15.png)  
  
  
  
对于语料库中的每篇文档，LDA定义如下生成过程(generative process): 
> 1. 对每一篇文档，从主题分布中抽取一个主题;  
> 2. 从上述被抽到的主题所对应的单词分布中抽取一个单词;  
> 3. 重复上述过程直到遍历文档中的每一个单词。  
  
具体来说，(w代表单词;d代表文档;t代表主题;大写代表总集合，小写代表个体。)  

D中每个文档d看作一个单词序列<w1,w2,...,wn>，wi表示第i个单词。D中涉及的所有不同单词组成一个词汇表集合V (vocabulary)，LDA以文档集合D
作为输入，希望训练出的两个结果向量(假设形成k个topic，V中一共m个词):
* 1.对每个D中的文档d，对应到不同Topic的概率θd<pt1,...,ptk>，其中，pti表示d对应T中第i个topic的概率。pti=nti/n，其中nti表示d中对应第i个topic的词的数目，n是d中所有词的总数。
* 2.对每个T中的topic t，生成不同单词的概率φt<pw1,...,pwm>，其中，pwi表示t生成V中第i个单词的概率。pwi=Nwi/N，其中Nwi表示对应到topic t的V中第i个单词的数目，N表示所有对应到topict的单词总数。  
**所以LAD的核心公式是：**  
  
  
![PicName](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/Chapter3-topic%20model/formula/16.png)  
  
  

直观的看这个公式，就是以Topic作为中间层，可以通过当前的θd和φt给出文档d中出现单词w的概率。其中p(t|d)利用θd计算得到，p(w|t)利用φt计算得到。  

实际上，利用当前的θd和φt，我们可以为一个文档中的一个单词计算它对应任意一个Topic时的p(w|d)，然后根据这些结果来更新这个词应该对应的topic。然后，如果这个更新改变了这个单词所对应的Topic，就会反过来影响θd和φt。  
  
**一篇长文从数学角度分析了LDA：[通俗理解LDA模型](https://blog.csdn.net/v_july_v/article/details/41209515)**

