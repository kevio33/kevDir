



# Encoder

![1727596350501](E:\kevin\kevDir\NN\Transformer.assets\1727596350501.png)



## 1.输入部分

- Embedding
- 位置嵌入

### Embedding



### 位置嵌入

自己理解的是：因为transformer是并行的，多头注意力机制，所以不是按顺序one by one，不共享一套参数(RNN共享)，**所以需要位置编码**



**位置编码**

由位置编码公式来确定不同位置的编码

例如下图：对于“爱”字，先变为512维度的向量，然后向量的奇数位置使用`sin`、偶数位置使用`cos`进行位置编码公式处理。

![1727596957203](E:\kevin\kevDir\NN\Transformer.assets\1727596957203.png)

最后把Embedding和位置编码结果相加就得到最终“爱”字的编码结果

![1727597295233](E:\kevin\kevDir\NN\Transformer.assets\1727597295233.png)



## 2.注意力机制

![1727597534015](E:\kevin\kevDir\NN\Transformer.assets\1727597534015.png)

注意力机制计算公式涉及到三个重要的向量：

- `Q`（query）
- `K`（key）
- `V`（value）

**例如：**拿“我爱你”这三个字来分别作为Q、K、V。操作流程图就如下

![1727598008198](E:\kevin\kevDir\NN\Transformer.assets\1727598008198.png)



**如果只有文本向量，如何拿到QKV**

嵌入之后的输入向量分别和右边的向量矩阵相乘，[1,512]×[512,3]最终得到一个[1,3]的Q。K和V同理

![1727598217030](E:\kevin\kevDir\NN\Transformer.assets\1727598217030.png)

然后计算attention value

![1727598508854](E:\kevin\kevDir\NN\Transformer.assets\1727598508854.png)

> 这里为啥除`根号dk`，没太明白？
>
> 图中的 Thinking Machine是啥？



**多头注意力机制**

有多个注意力头计算attention value

![1727598852520](E:\kevin\kevDir\NN\Transformer.assets\1727598852520.png)

