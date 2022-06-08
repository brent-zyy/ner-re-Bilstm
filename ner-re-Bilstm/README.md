

# 介绍

本文根据https://github.com/crownpku/Information-Extraction-Chinese/tree/master/RE_BGRU_2ATT 和 https://github.com/crownpku/Information-Extraction-Chinese/tree/master/NER_IDCNN_CRF 两篇文章做的，需要的话可以参考

主要是把数据集换成了自己的数据结构数据集，并进行了测试



# 需求环境 

```
tensorflow~=1.14.0
scikit_learn==0.23.2
jieba==0.42.1
python 3.6
numpy~=1.16.0
```



# 操作方法：

两个文件，NER和RE

采用 cd 操作进入对应文件夹内部

如果需要修改，可以再config_file里面修改你的参数



#实体识别部分：

##训练：

```
python3 main.py --train=True --clean=True --model_type=bilstm
```

##测试：

```
python3 main.py --ckpt_path=ckpt_biLSTM
```

## 预测：

```
python3 main.py
```

看一下跑出来的结果：

![image-20220608113518789](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220608113518789.png)



#关系抽取部分：

## 训练：

```
python3 initial.py
```

```
python3 train_GRU.py
```

##预测：

```
python3 test_GRU.py
```

看一下跑出来的结果：

![image-20220608113839468](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220608113839468.png)



我这里效果不太好的原因是，数据量太少的缘故，数据量增大后，效果会变好很多

# 如果你想要替换成自己的数据集，那么可以看这里：

## 首先需要准备数据集

你需要先准备一个初始的语料库，并将它转换成实体识别和关系抽取的数据集，这并不难，只需要标注即可

（你可以按你想要的任何方式，任何分类来标注，这并不是严格要求的，只需要达到你的想法就行）

有一些自动化的标注工具可以用，可以选择自己想要的工具来标注

对应的是data文件里面的train，dev，test

## 然后需要对数据集处理

首先你需要对初始的语料库进行分词：

你可以用jieba分词器或者其他工具，

然后，对生成的文本，你可以用word2vec 或者 bert对数据进行预处理，这些看你自己的想法来选，最后是生成的vec.txt文件，这就是词嵌入文件



##之后导入模型就行了

当然模型里面也需要修改

对RE模型来说：

origin_data文件里的标签文件relation2id文件要改成你自己的，

同时数据集的test和train也要导入，这里的vec.txt文件其实就是上面处理过的文件。



**注意**，如果数据量太大，一定要把model里的checkpoint删掉，因为这是跑的我的数据集跑出来的结果，所以如果你不删掉，结果肯定会报错。因为数据集合这个是不符的。

删掉之后，再跑就是你的数据集的结果，然后需要设置一个东西

在RE的main文件的125行，我这里的意思是，每10次，保存一下，这是对数据量小的数据集这样操作，

如果你的数据及文件很大，这里就可以改的大一点，不然一直保存会很慢
