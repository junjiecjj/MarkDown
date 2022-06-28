Transformer模型的输入为一系列词，词需要转化为词向量。一般的语言模型都需要使用Embedding层，用以将词转化为词向量。在此基础上，Transformer增加了位置编码（Positional Encoding）。**Transformer没有采用RNN的结构，不能利用单词的顺序信息，但顺序信息对于NLP任务来说非常重要。**为解决无位置信息的问题，Transformer使用位置编码来增加位置信息。

$$ PE_{(pos, 2i)}=\sin(pos / 10000^{2i/d}) \\ PE_{(pos, 2i + 1)}=\cos(pos / 10000^{2i/d}) $$

其中，pos表示单词在句子中的位置，d表示词向量的维度，2i表示偶数维度，2i+1表示奇数维度$ (2i≤d, 2i+1≤d)$。



PE公式生成的是[-1, 1][−1,1]区间内的实数。词向量维度为d，d是模型设计时就确定的。给定一个词，其在句子中的位置为pos ，就是上图中的某一行。Positional Encoding就是从上图中找到一行，将这行加到词向量上。

使用这种公式计算 PE 有以下的好处：可以让模型容易地计算出相对位置，对于固定长度的间距k，$PE(pos+k)$可以用$PE(pos)$计算得到。因为：

$$\sin(A+B) = \sin A\cos B + \cos A \sin B \\ \cos(A+B) = \cos A \cos B - \sin A \sin B \\ sin(A+B)=sinAcosB+cosAsinB \\ cos(A+B)=cosAcosB−sinAsinB$$

将词向量Embedding和PE相加，就可以得到词的表示向量$ \mathbf{X}$，$\mathbf{X}$就是 Transformer 的输入。