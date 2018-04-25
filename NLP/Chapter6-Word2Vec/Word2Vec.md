## word2vec:CBOW(连续词袋)  
由两边的n个词，推断中间的一个词（去掉了神经网络的hidden layers）  
* 无hidden layers
* 使用双向上下文窗口
* 上下文词序无关（Bow）
* 输入层直接使用低维稠密表示
* 投影层简化为求和（平均）  
为了解决softmax分类函数计算量大的问题（负样本太大），提出了2个方案：  
#### 1.CBOW：层次softmax  
使用huffman tree来编码输出层的词典：softmax变成多个sigmoid  
#### 2.CBOW:负采样（工业界使用较多）  
一个正样本，V-1个负样本，对负样本进行采样  
如何采样？  
保证采样样本的数据和正文本有相似度  
比如采集999个负样本，则softmax只需要分类1000种，大大降低softmax的分类个数  
词典中每一个词对应一条线段，所有词划分在[0,1]区间上，根据出现的词频确定不同的长度  
## word2vec:Skip-Gram模型  
与CBOW相反，由中间的词推断两边的词  
* 无hidden layers
* 投影层也可以省略
* 每个词向量作为log-linear模型的输入  
目标函数：  
![PicName](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/Chapter6-Word2Vec/formula/25.png)  

**语料较大时，Skip-Gram一般比CBOW效果好**  
## word2vec存在的问题
* 对每个local context window单独训练，没用利用包含在global co-currence矩阵中的统计信息  
**改进：GLOVE**
* 对多义词无法很好的表示和处理，因为使用了唯一的词向量  
## 总结  
### 离散表示 
* one-hot
* bag of words
* N-gram
* co-currence（共现矩阵）：矩阵的行（列）向量作为词向量  
解决bag of words中词的多种表达和词表膨胀问题，但问题是稀疏且高纬度
### 分布式连续表示  
* co-currence矩阵的svd降维
* CBOW
* Skip-gram Model
