## 自解码器Auto-Encoder

### 引子  

**问题原型**：Text("X")->Label("Y")  (文本分类)  
行业Baseline:  
用Bag of Word 表示Sentences,然后用LR/SVM做回归  
  
### Auto-Encoder  
pic20  

作用：  
* 无监督学习，适用于no lable的情况  
* 维度太大时降维，将输入数据压缩表示  
（**可能会导致局部最优解**）
使命：  
数据降噪，数据降维  
### Auto-Encoder应用  
```python
from keras.layers import Input, Dense
from keras.models import Model
from sklearn.cluster import KMeans
class ASCIIAutoencoder(): “""基于字符的Autoencoder."""
    def __init__(self, sen_len=512, encoding_dim=32, epoch=50, val_ratio=0.3):
        """
        Init.
        :param sen_len: 把sentences pad成相同的长度 
        :param encoding_dim: 压缩后的维度dim 
        :param epoch: 要跑多少epoch
        :param kmeanmodel: 简单的KNN clustering模型
        """
        self.sen_len = sen_len
        self.encoding_dim = encoding_dim self.autoencoder = None
        self.encoder = None
        self.kmeanmodel = KMeans(n_clusters=2)
        self.epoch = epoch
```  
  
模型构建  
```python

def fit(self, x):
     """
      模型构建。
     :param x: input text
     """
     # 把所有的trainset都搞成同一个size，并把每个字符都换成ascii码
     x_train = self.preprocess(x, length=self.sen_len)  #将每条text扩展或者截断为len_sentence，并将每个word用ASCII做toknize
     # 然后给input预 好位置
     input_text = Input(shape=(self.sen_len,))
     # "encoded" 
     encoded = Dense(1024, activation='tanh')(input_text)
     encoded = Dense(512, activation='tanh')(encoded)
     encoded = Dense(128, activation='tanh')(encoded)
     encoded = Dense(self.encoding_dim, activation='tanh')(encoded)
     # "decoded" 就是把刚刚压缩完的东西，给反过来还原成input_text 
     decoded = Dense(128, activation='tanh')(encoded)
     decoded = Dense(512, activation='tanh')(decoded)
     decoded = Dense(1024, activation='tanh')(decoded)
     decoded = Dense(self.sen_len, activation='sigmoid')(decoded)
     # 整个从大到小再到大的model，叫 autoencoder
     self.autoencoder = Model(input=input_text, output=decoded)
     # 那么 只从大到小(也就是一半的model)就叫 encoder 
     self.encoder = Model(input=input_text, output=encoded)
     
     
     # 同理，我们接下来搞个decoder出来，也就是从小到大的model 
     # 来，首先encoded的input size给预留好
     encoded_input = Input(shape=(1024,))
     # autoencoder的最后一层，就应该是decoder的第一层
     decoder_layer = self.autoencoder.layers[-1] # 然后我们从头到尾连起来，就是一个decoder !
     decoder = Model(input=encoded_input, output=decoder_layer(encoded_input)) 
     # compile
     self.autoencoder.compile(optimizer='adam', loss='mse')
     # 跑起来
     self.autoencoder.fit(x_train, x_train,
                            nb_epoch=self.epoch,
                            batch_size=1000,
                            shuffle=True,
                            )
# 这 部分是自己拿自己train一下Kmeans，一个简单的基于距离的分类  
x_train = self.encoder.predict(x_train) 
self.kmeanmodel.fit(x_train)


def predict(self, x):
        """
        做预测。
        :param x: input text
        :return: predictions
        """
        # 同理，第一步把来的 都给搞成ASCII化，并且长度相同 
        x_test = self.preprocess(x, length=self.sen_len)
        # 然后 encoder把test集给压缩
        x_test = self.encoder.predict(x_test)
        # Kmeans给分类出来
        preds = self.kmeanmodel.predict(x_test)
        return preds
#512位等长压缩
def preprocess(self, s_list, length=256):
    ...
```
  
