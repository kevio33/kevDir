





# Token

Transformer 模型的核心思想是处理一系列的 tokens，并通过注意力机制（self-attention）来捕捉这些 tokens 之间的关系。Transformer 模型的输入是经过 tokenization 的文本，它会为每个 token 生成一个相应的向量表示，然后通过编码器-解码器结构进行处理。 

## Token 在输入阶段的具体步骤

- **Tokenization（标记化）**：将文本分割成 token，并为每个 token 生成唯一的标识符。常见的 tokenization 方法有 WordPiece、BPE（Byte Pair Encoding）、SentencePiece 等。
- **Embedding（嵌入层）**：每个 token 都会被转换为一个固定长度的向量表示，这些向量传递给 Transformer 模型的输入层。
- **Position Embedding（位置嵌入）**：由于 Transformer 模型本身没有顺序信息，需要为 tokens 加上位置信息，以捕捉序列中的顺序关系。

## 特殊的 Tokens

除了普通的单词 token，Transformer 中还会使用一些特殊的 tokens：

- **[CLS] Token**：用于分类任务，通常作为句子或文本的开始 token，最后输出用于分类决策。
- **[SEP] Token**：用于区分不同句子或段落，尤其在句子对任务中（如问答系统、自然语言推理）。
- **[PAD] Token**：用于填充，确保所有输入序列的长度相同。
- **[MASK] Token**：在 BERT 模型中用于掩盖部分输入，训练时模型需要预测被掩盖的 token。

# Encoder

![1727596350501](E:\kevin\kevDir\NN\Transformer.assets\1727596350501.png)



## 1.输入部分

- Embedding
- 位置编码

### Embedding



### 位置编码

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

`Embedding`的输入向量分别和右边的向量矩阵相乘，`[1,512]×[512,3]`最终得到一个`[1,3]`的Q。K和V同理

![1727598217030](E:\kevin\kevDir\NN\Transformer.assets\1727598217030.png)

然后计算attention value

![1727598508854](E:\kevin\kevDir\NN\Transformer.assets\1727598508854.png)

> 这里为啥除`根号dk`，没太明白？
>
> 图中的 Thinking Machine是啥？



**多头注意力机制**

有多个注意力头计算attention value

![1727598852520](E:\kevin\kevDir\NN\Transformer.assets\1727598852520.png)

## 3.残差

整个编码部分的结构图如下：

![1728281103325](E:\kevin\kevDir\NN\Transformer.assets\1728281103325.png)

- 输入x1，x2，和位置嵌入编码对位相加，得到新的x1，x2
- x1，x2计算注意力值得到z1，z2
- 将z值和x值进行残差操作，然后放入LayerNorm层



**残差部分**

![1728281321170](E:\kevin\kevDir\NN\Transformer.assets\1728281321170.png)

![1728281348369](E:\kevin\kevDir\NN\Transformer.assets\1728281348369.png)

残差保证了求导后不会梯度消失，因为始终有1的存在



## 4.Layer Normalization

transformer没有用到`BN`，而是用`LN`

**Layer Normalization**（层归一化）是深度学习中常用的一种技术，用于改善神经网络的训练过程。它的作用主要是通过对每一层的输出进行归一化处理，帮助加速收敛、稳定训练过程，并提升模型的性能。以下是它的具体作用：

**减少内部协变量偏移（Internal Covariate Shift）**

- **内部协变量偏移**指的是在训练过程中，每一层的输入分布会随着模型参数的更新而发生变化，导致后续层需要不断适应这种变化。这样会使得训练变得不稳定，需要更多的调整和调参。
- Layer Normalization通过对每一层的输入进行归一化处理，使得每一层的输出具有稳定的分布，减少了内部协变量偏移，进而提升了训练稳定性。

**加速训练过程**

- 通过减少内部协变量偏移，Layer Normalization可以使得网络在训练时更容易收敛，减少了梯度消失或爆炸的情况。这样在优化过程中，网络能够更快速有效地找到最佳参数。

**提高网络的泛化能力**

- Layer Normalization通过标准化输入，使得模型的训练对初始值、学习率等超参数不那么敏感。这样，它能够更好地避免过拟合，从而提高网络的泛化能力。

**对小批量训练更有效**

- 在小批量训练中，由于每个批次中的样本较少，统计量（如均值和方差）的估计可能不稳定。而Layer Normalization不依赖于批量样本的统计量，而是对每个样本的特征进行归一化，使其能够在小批量训练时表现得更加稳定。

**适用于RNN和Transformer架构**

- 与Batch Normalization（批量归一化）不同，Layer Normalization不依赖于批次大小，而是对每个样本的所有特征进行标准化。因此，它非常适合于序列数据的处理，尤其是**循环神经网络（RNN）\**和\**Transformer**架构。在这些模型中，由于每个时间步的输入数据变化较大，Layer Normalization能够有效地稳定训练。

**工作原理**

Layer Normalization的操作是在每一层的激活值上进行归一化，步骤如下：

1. 对某一层的输出（假设是一个向量）进行标准化处理：首先计算该向量的均值和标准差，然后用均值和标准差对其进行归一化。
2. 然后通过一个可学习的缩放因子（`gamma`）和偏移量（`beta`）进行调整，使得归一化后的值适配目标分布。

公式如下：

x^=x−μσ\hat{x} = \frac{x - \mu}{\sigma}x^=σx−μy=γx^+βy = \gamma \hat{x} + \betay=γx^+β

- xxx 是当前样本的输入向量
- μ\muμ 是输入向量的均值
- σ\sigmaσ 是输入向量的标准差
- γ\gammaγ 是可学习的缩放参数
- β\betaβ 是可学习的偏移参数

### BN

**优点**

- 第一个就是可以解决内部协变量偏移
- 第二个优点就是缓解了梯度饱和问题（如果使用sigmoid激活函数的话），加快收敛

**缺点**

- batch_size较小的时候，效果差

  > 因为其使用Batch中的样本的均值和方差模拟全部数据的均值和方差

- BN在RNN中效果比较差

# Decoder

![1728282329755](E:\kevin\kevDir\NN\Transformer.assets\1728282329755.png)



## 1.多头注意力机制

decoder的多头注意力机制多了个`mask`



## 2.交互层

![1728282606578](E:\kevin\kevDir\NN\Transformer.assets\1728282606578.png)

encoder和每一层decoder都进行交互

![1728282874915](E:\kevin\kevDir\NN\Transformer.assets\1728282874915.png)

**encoder生成K,V矩阵，Decoder生成Q矩阵，进行多头注意力计算**

![1728282974562](E:\kevin\kevDir\NN\Transformer.assets\1728282974562.png)

完整图片如下，encoder输出的`K、V`矩阵和每一个decoder都进行attention：

![1728283066746](E:\kevin\kevDir\NN\Transformer.assets\1728283066746.png)





# 问题：

## 1.多头注意力

模型中出现了好几次计算attention：

- Encoder-Decoder结构中 ，Encoder 输出K，V 、 Decoder 生成Q，这种情况是**attention** 

  > 用于计算解码器的输入与编码器的输出之间的注意力。这种注意力机制帮助解码器在生成每个输出时，关注编码器输出中的相关部分。

- 在Encoder和Decoder当中的前面部分都使用了 **self-attention**，也就是Q,K,V 都是Encoder 和 Decoder自身模块中产生 

  > 用于计算输入序列中每个元素与自身序列中其他元素之间的相关性。它通常用于处理单个序列，例如编码器输入或解码器输入 





# ViT

> https://blog.csdn.net/qq_37541097/article/details/118242600

## Token

在 Vision Transformer 中，**token 是图像的分块（patch）**。以下是 Vision Transformer 中 token 的详细解释： 

**图像首先会被分割成一系列的固定大小的图像块（patches）**，每个图像块被视为一个 token。然后，这些图像块会被线性映射为向量，类似于文本 Transformer 中的 `word embedding`，接着传递到 Transformer 模型中。 

**图像分块（Patch Tokenization）**：

- 假设输入图像的大小为 H×W（高度和宽度），图像被切割成大小为 P×P的小块（patches），因此整个图像可以被划分成 H/P×W/P 个 patch。每个 patch 会被看作一个 token。
- 举个例子：对于一个 224×224 像素的图像，如果将其分割为 16×16 的 patch，则会得到 224/16×224/16=14×14=196个 patch，即 196 个 token。

**线性映射（Linear Projection）**：

- 每个 patch 的像素值（通常为 P×P×3 的矩阵，3 表示 RGB 通道）被展平成一个向量，接着通过一个可训练的线性投影层映射到固定长度的向量空间（称为 embedding）。
- 这个过程将每个图像块转换成与文本 Transformer 中 word embedding 类似的表示向量。

**位置编码（Position Embedding）**：

- 由于 Transformer 模型无法捕捉输入序列的顺序信息，Vision Transformer 也需要为每个 patch 添加位置编码，以表明它们在原始图像中的相对位置。这类似于文本 Transformer 中的 position embedding。



**特殊的Token**

**Vision Transformer 中也引入了一个特殊的 [CLS] token**，用于分类任务。

- **[CLS] Token** 被添加到所有 patch tokens 的前面，表示整个图像的全局信息。
- 最后，经过 Transformer 层的处理后，[CLS] token 的输出将作为图像分类的最终表示，用于下游的分类任务。