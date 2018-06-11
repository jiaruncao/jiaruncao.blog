## 注意力机制（attention）  

  
起源于以机器翻译为例，输入的一个序列，注意力机制可以让神经网络只关注一部分输入序列  
注意力机制给每一个Rnn 时间步一个权重值，来控制花多少注意力在该时间步上，之后用这个权重值与当前的节点激活函数的值相乘，把上下文所有的节点都相加起来，  
就是当前节点attention后最终的值    

#### 序列整个的权重值相加和为1，且每个权重值都是非负的  
  ![PicName](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/Chapter5-Deep%20Learning%20in%20NLP/formula/1.png)  
  
  
  a<t,t'>代表输出值y<t>在a<t'>上的注意力，使用softmax函数保证a<t,t'>  
  ![PicName](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/Chapter5-Deep%20Learning%20in%20NLP/formula/2.png)  
  
  那么，如何计算e呢？  
  ![PicName](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/Chapter5-Deep%20Learning%20in%20NLP/formula/3.png)  
  
  可以使用如下的一个神经网络：  
  s<t-1>表示上一个时间步的隐藏状态  
  a<t'>表示第t'个节点的激活函数值  
  
 **缺点**： 时间复杂度是O（x^3）  
 附上一张attention简图：  
 ![PicName](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/Chapter5-Deep%20Learning%20in%20NLP/formula/4.png)
