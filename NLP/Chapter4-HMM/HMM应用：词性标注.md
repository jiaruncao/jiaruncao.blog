# 使用HMM进行词性标注  
### 导入库  
```python
import nltk
import sys
from nltk.corpus import brown
```  
### 预处理  
这里需要做的预处理是：给词们加上开始和结束符号。  

Brown里面的句子都是自己标注好了的，长这个样子：(I , NOUN), (LOVE, VERB), (YOU, NOUN)  

那么，我们的开始符号也得跟他的格式符合，  

我们用：  

(START, START) (END, END)  


来代表  
```python
brown_tags_words = [ ]
for sent in brown.tagged_sents():
    # 先加开头
    brown_tags_words.append( ("START", "START") )
    # 为了省事儿，我们把tag都省略成前两个字母
    brown_tags_words.extend([ (tag[:2], word) for (word, tag) in sent ])
    # 加个结尾
    brown_tags_words.append( ("END", "END") )
```  
### 词统计  
这个时候，我们要把我们所有的词库中拥有的单词与tag之间的关系，做个简单粗暴的统计。  
 
P(wi | ti) = count(wi, ti) / count(ti)  


这里NLTK给了我们做统计的工具  
```python
# conditional frequency distribution
cfd_tagwords = nltk.ConditionalFreqDist(brown_tags_words)
# conditional probability distribution
cpd_tagwords = nltk.ConditionalProbDist(cfd_tagwords, nltk.MLEProbDist)
```  
接下来还有第二个公式需要计算：  


P(ti | t{i-1}) = count(t{i-1}, ti) / count(t{i-1})  


这个公式跟words没有关系。它是属于隐层的马科夫链。  

所以 我们先取出所有的tag。  
```python
brown_tags = [tag for (tag, word) in brown_tags_words ]
# count(t{i-1} ti)
# bigram的意思是 前后两个一组，联在一起
cfd_tags= nltk.ConditionalFreqDist(nltk.bigrams(brown_tags))
# P(ti | t{i-1})
cpd_tags = nltk.ConditionalProbDist(cfd_tags, nltk.MLEProbDist)
```  

### 一些有趣的结果  

那么，比如， 一句话，"I want to race"， 一套tag，"PP VB TO VB"  


他们之间的匹配度有多高呢？  


其实就是：  


 P(START) * P(PP|START) * P(I | PP) *
            P(VB | PP) * P(want | VB) *
            P(TO | VB) * P(to | TO) *
            P(VB | TO) * P(race | VB) *
            P(END | VB)  
```python
prob_tagsequence = cpd_tags["START"].prob("PP") * cpd_tagwords["PP"].prob("I") * \
    cpd_tags["PP"].prob("VB") * cpd_tagwords["VB"].prob("want") * \
    cpd_tags["VB"].prob("TO") * cpd_tagwords["TO"].prob("to") * \
    cpd_tags["TO"].prob("VB") * cpd_tagwords["VB"].prob("race") * \
    cpd_tags["VB"].prob("END")

print( "The probability of the tag sequence 'START PP VB TO VB END' for 'I want to race' is:", prob_tagsequence)
```
The probability of the tag sequence 'START PP VB TO VB END' for 'I want to race' is: 1.0817766461150474e-14

