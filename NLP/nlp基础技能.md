## Python的字符串处理操作
```
s = 'hello,world! '
```
#### 去空格和特殊符号

```
s.strip() 
```
#### 查找字符
```
s.index()
```
#### 比较字符串
```
cmp(str1,str2) #return -1,0,1
```
#### 大小写
```
s.upper()
s.lower()
```
#### 翻转字符串
```
str = s[::-1]
```
#### 查找字符串
```
s.find()
```
#### 分割字符串
```
s.split() #return a list 
```

## Python正则表达式
正则表达式是处理字符串的强大工具，拥有独特的语法和独立的处理引擎。  
我们在大文本中匹配字符串时，有些情况用str自带的函数(比如find, in)可能可以完成，有些情况会稍稍复杂一些(比如说找出所有“像邮箱”的字符串），这个时候我们需要一个某种模式的工具，这个时候正则表达式就派上用场了。  
说起来正则表达式效率上可能不如str自带的方法，但匹配功能实在强大太多。对啦，正则表达式不是Python独有的。  
**[正则表达式在线验证工具](http://regexr.com/)**  
**[正则表达式语法](https://github.com/jiaruncao/jiaruncao.github.io/blob/master/NLP/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%AF%AD%E6%B3%95.jpg)**

#### re模块  
Python通过re模块提供对正则表达式的支持。  

使用re的一般步骤是

1.将正则表达式的字符串形式编译为Pattern实例
2.使用Pattern实例处理文本并获得匹配结果（一个Match实例）
3.使用Match实例获得信息，进行其他的操作。


