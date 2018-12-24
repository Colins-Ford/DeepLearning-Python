# 附录 A　Softmax-with-Loss 层的计算图

这里，我们给出 softmax 函数和交叉熵误差的计算图，来求它们的反向传播。softmax 函数称为 softmax 层，交叉熵误差称为 Cross Entropy Error 层，两者的组合称为 Softmax-with-Loss 层。先来看一下结果，Softmax-with-Loss 层可以画成图 A-1 所示的计算图。

![](http://image.colinsford.top/DeepLearning-Python/00344.jpeg)

**图 A-1 Softmax-with-Loss 层的计算图**

图 A-1 的计算图中假定了一个进行 3 类别分类的神经网络。从前面的层输入的是 ![](http://image.colinsford.top/DeepLearning-Python/00221.gif)，softmax 层输出 ![](http://image.colinsford.top/DeepLearning-Python/00222.gif)。此外，教师标签是 ![](http://image.colinsford.top/DeepLearning-Python/00223.gif)，Cross Entropy Error 层输出损失 _L_。

如图 A-1 所示，在本附录中，Softmac-with-Loss 层的反向传播的结果为 ![](http://image.colinsford.top/DeepLearning-Python/00225.gif)。

## A.1　正向传播

图 A-1 的计算图中省略了 Softmax 层和 Cross Entropy Error 层的内容。这里，我们来画出这两个层的内容。

首先是 Softmax 层。softmax 函数可由下式表示。

![](http://image.colinsford.top/DeepLearning-Python/00345.gif)

因此，用计算图表示 Softmax 层的话，则如图 A-2 所示。

![](http://image.colinsford.top/DeepLearning-Python/00346.jpeg)

**图 A-2 Softmax 层的计算图（仅正向传播）**

图 A-2 的计算图中，指数的和（相当于式 \(A.1\) 的分母）简写为 _S_，最终的输出记为 ![](http://image.colinsford.top/DeepLearning-Python/00222.gif)。

接下来是 Cross Entropy Error 层。交叉熵误差可由下式表示。

![](http://image.colinsford.top/DeepLearning-Python/00347.gif)

根据式 \(A.2\)，Cross Entropy Error 层的计算图可以画成图 A-3 那样。

图 A-3 的计算图很直观地表示出了式 \(A.2\)，所以应该没有特别难的地方。

![](http://image.colinsford.top/DeepLearning-Python/00348.jpeg)

**图 A-3 Cross Entropy Error 层的计算图（仅正向传播）**

下一节，我们将看一下反向传播。

## A.2　反向传播

首先是 Cross Entropy Error 层的反向传播。Cross Entropy Error 层的反向传播可以画成图 A-4 那样。

![](http://image.colinsford.top/DeepLearning-Python/00349.jpeg)

**图 A-4 交叉熵误差的反向传播**

求这个计算图的反向传播时，要注意下面几点。

> * 反向传播的初始值（图 A-4 中最右边的值）是 1（因为 ![](http://image.colinsford.top/DeepLearning-Python/00350.gif)）。
> * “×”节点的反向传播将正向传播时的输入值翻转，乘以上游传过来的导数后，再传给下游。
> * “+”节点将上游传来的导数原封不动地传给下游。
> * “log”节点的反向传播遵从下式。

![](http://image.colinsford.top/DeepLearning-Python/00351.gif)

遵从以上几点，就可以轻松求得 Cross Entropy Error 的反向传播。结果 ![](http://image.colinsford.top/DeepLearning-Python/00352.gif) 是传给 Softmax 层的反向传播的输入。

下面是 Softmax 层的反向传播的步骤。因为 Softmax 层有些复杂，所以我们来逐一进行确认。

**步骤 1**

![](http://image.colinsford.top/DeepLearning-Python/00353.jpeg)

前面的层（Cross Entropy Error 层）的反向传播的值传过来。

**步骤 2**

![](http://image.colinsford.top/DeepLearning-Python/00354.jpeg)

“×”节点将正向传播的值翻转后相乘。这个过程中会进行下面的计算。

![](http://image.colinsford.top/DeepLearning-Python/00355.gif)

**步骤 3**

![](http://image.colinsford.top/DeepLearning-Python/00356.jpeg)

正向传播时若有分支流出，则反向传播时它们的反向传播的值会相加。因此，这里分成了三支的反向传播的值 ![](http://image.colinsford.top/DeepLearning-Python/00357.gif) 会被求和。然后，还要对这个相加后的值进行“/”节点的反向传播，结果为 ![](http://image.colinsford.top/DeepLearning-Python/00358.gif)。这里，![](http://image.colinsford.top/DeepLearning-Python/00223.gif) 是教师标签，也是 one-hot 向量。one-hot 向量意味着 ![](http://image.colinsford.top/DeepLearning-Python/00223.gif)中只有一个元素是 1，其余都是 0。因此，![](http://image.colinsford.top/DeepLearning-Python/00223.gif) 的和为 1。

**步骤 4**

![](http://image.colinsford.top/DeepLearning-Python/00359.jpeg)

“+”节点原封不动地传递上游的值。

**步骤 5**

![](http://image.colinsford.top/DeepLearning-Python/00360.jpeg)

“×”节点将值翻转后相乘。这里，式子变形时使用了 ![](http://image.colinsford.top/DeepLearning-Python/00361.gif)。

**步骤 6**

![](http://image.colinsford.top/DeepLearning-Python/00362.jpeg)

“exp”节点中有下面的关系式成立。

![](http://image.colinsford.top/DeepLearning-Python/00363.gif)

根据这个式子，向两个分支的输入和乘以 ![](http://image.colinsford.top/DeepLearning-Python/00364.gif) 后的值就是我们要求的反向传播。用式子写出来的话，就是 ![](http://image.colinsford.top/DeepLearning-Python/00365.gif)，整理之后为 ![](http://image.colinsford.top/DeepLearning-Python/00366.gif)。综上，我们推导出，正向传播时输入是 ![](http://image.colinsford.top/DeepLearning-Python/00367.gif) 的节点，它的反向传播是 ![](http://image.colinsford.top/DeepLearning-Python/00366.gif)。剩下的 ![](http://image.colinsford.top/DeepLearning-Python/00368.gif)、![](http://image.colinsford.top/DeepLearning-Python/00369.gif) 也可以按照相同的步骤求出来（结果分别为 ![](http://image.colinsford.top/DeepLearning-Python/00370.gif) 和 ![](http://image.colinsford.top/DeepLearning-Python/00371.gif)）。此外，除了这里介绍的 3 类别分类外，对于 _n_ 类别分类的情况，也可以推导出同样的结果。

## A.3　小结

上面，我们画出了 Softmax-with-Loss 层的计算图的全部内容，并求了它的反向传播。未做省略的 Softmax-with-Loss 层的计算图如图 A-5 所示。

![](http://image.colinsford.top/DeepLearning-Python/00372.jpeg)

**图 A-5 Softmax-with-Loss 层的计算图**

图 A-5 的计算图看上去很复杂，但是使用计算图逐个确认的话，求导（反向传播的步骤）也并没有那么复杂。除了这里介绍的 Softmax-with-Loss 层，遇到其他看上去很难的层（如 Batch Normalization 层）时，请一定按照这里的步骤思考一下。相信会比只看数学式更容易理解。

