###Bag of words(BoW)及其在image retrieval中是应用

##Bag of Words(BoW)
举例：
100个词（w1,w2,...w10)
10个文档（d1,d2,...,d10)
每个文档有5个词
d1=[w7,w7,w1,w7,w7]
d2=[w4,w1,w7,w1,w3]
d3=[w9,w3,w8,w10,w9]
d4=[w1,w4,w6,w7,w9]
d5=[w8,w10,w4,w9,w6]
d6=[w8,w3,w2,w9,w7]
d7=[w9,w7,w8,w3,w6]
d8=[w1,w4,w4,w7,w3]
d9=[w2,w7,w7,w3,w9]
d10=[w8,w4,w9,w4,w8]

统计一下每个词出现次数
w1:5
w2:2
w3:6
w4:7
w5:0
w6:3
w7:11
w8:6
w9:8
w10:2

word_scores:词出现次数/uniqued词的个数（正规化）
d1=[w1:1,w7:4]/2
d2=[w1:2,w3:1,w4:1,w7:1]/4
d3=[w3:1,w8:1,w9:2,w10:1]/4
d4=[w1:1,w4:1,w6:1,w7:1,w9:1]/5
d5=[w4:1,w6:1,w8:1,w9:1,w10:1]/5
d6=[w2:1,w3:1,w7:1,w8:1,w9:1]/5
d7=[w3:1,w6:1,w7:1,w8:1,w9:1]/5
d8=[w1:1,w3:1,w4:2,w7:1]/5
d9=[w2:1,w3:1,w7:2,w9:1]/5
d10=[w4:2,w8:2,w9:1]/5


##BoW在image retrivial中应用
CNN的feature map作为特征
kmeans求聚类，得到25000个中心，每个image计算到中心点的最小距离

