# 一、什么是机器学习
即便是在机器学习专业人士中，也不存在一个被广泛认可的机器学习的定义。

最早关于机器学习的定义来自Arthur Samuel，他将机器学习定义为，在特定编程的前提下，给与计算机学习的能力。在50年代，他在将机器学习运用在西洋棋中，通过上万次对战训练，机器能判断出好的棋局。

另一个年代较近的定义由卡内基梅隆大学的Tom Mitchell提出。一个程序能从经验E中学习，并解决任务T，达到性能度量值P。当有了经验E之后，经过P的判断，程序在处理T时性能有所提升。

我认为一个程序运用不同的学习算法，在喂入大量数据学习之后，处理任务时准确率不断的提高。这个过程可以被认为是机器学习，而目前主要使用的学习算法为监督学习和无监督学习。

## 1.监督学习
教会计算机如何完成任务。

当我们给程序一个数据集，这个数据集每个数据都给出了“正确答案”。程序对这个数据集进行学习，当我们再给程序一个相关数据时，程序能对这个数据进行预测。

对连续的数据进行学习，预测的输出为连续的值，被认为是**回归问题**。比如根据房子的大小判断房价的问题。当我们告诉程序大量不同面积的房子所对应的房价，其中房价就是正确答案，在进行了大量的学习之后，程序拟合出一条**二维曲线**，当我们再输入一个面积值时，程序会输出对应的房价。

对离散的数据进行学习，预测的输出为离散的值，被认为是**分类问题**。比如说根据肿瘤大小判断是否患有乳腺癌。当我们告诉程序大量不同大小的肿瘤所对应的乳腺癌类型（良或者不同类癌症），经过大量学习，程序会拟合出一个离散图像，当我们再次输入肿瘤的大小时，程序会输出某几个固定值中（良或者不同类癌症）的某一个。

上面两个例子中，房子面积大小和肿瘤大小都是一个特征，房价和乳腺癌类型都是输出。但实际问题中会涉及到很多特征。在处理特征较多的数据集时，可以通过**支持向量机**中的数学技巧来让计算机处理无限多的特征。

小测试：假设一家公司想通过学习算法来处理两个问题：
> 1.有一大批同样的货物，现在又上千件一摸一样的货物等待出售，预测接下来三个月能卖多少件。<br>
> 2.公司想通过一个软件检测用户账户是否被盗过。<br>
> <br>
> 第一个是回归问题。<br>
> 第二个是分类问题。<br>

## 2.无监督学习
让计算机自己学习如何去完成任务。

给定程序一个数据集，但该数据集没有任何标签。无监督学习能判断出数据集中不同数据结构的聚集簇。这就叫做**聚类算法**。比如说谷歌新闻。无监督学习没有提前告知算法一些信息，比如说这是第一类，这是第二类，等等。通过程序自动聚类。

无监督学习本质上是一种学习策略，交给算法大量的数据，并让算法为我们从数据中找出某种结构。**将数据集中的数据进行分类**。

> 垃圾邮件问题 -> 监督学习<br>
> 新闻事件分类 -> 无监督学习<br>
> 细分市场     -> 无监督学习<br>
> 疾病判断     -> 监督学习<br>

# 二、单变量线性回归（Linear Regression with One Variable）
线性回归属于监督学习中的一种。线性回归算法属于比较简单的算法模型。

单变量线性回归本质上是只对**单个特征->输出**进行学习，其中的单变量为特征。实际应用中的线性回归通常都包含大量的特征（即变量）。

虽然不同数量的变量所对应的线性回归对算力要求天差地别，但是迭代方法在本质上没有不同。

## 1.模型表示
我们以房屋交易问题为例，在下图房屋size-price曲线图中，将收集到所有(size,price)坐标标注在坐标系中，画出一条回归曲线。因为给出了**price**这个**正确答案**，该学习成为监督学习。

![房屋size-price曲线图](https://github.com/yiyading/NLP-and-ML/blob/master/img/%E6%88%BF%E5%B1%8Bsize-price%E6%9B%B2%E7%BA%BF%E5%9B%BE.jpg)<br>

在监督学习中，我们有一个数据集，这个数据集被称为训练集。还是以房屋问题为例，我们回归问题的训练集（Training Set）如下图所示：

![房屋size-price训练集](https://github.com/yiyading/NLP-and-ML/blob/master/img/%E6%88%BF%E5%B1%8Bsize-price%E8%AE%AD%E7%BB%83%E9%9B%86.jpg)
> 使用小写m来代表训练样本的数目。<br>
> x代表特征/输入变量。<br>
> y代表目标变量/输出变量。<br>
> (x,y)代表训练集中的实例。<br>
> (xi,yi)代表第i个观察实例。<br>

h代表学习算法的解决方案或函数，也称作假设（hypothesis）。其实这个h就是数学中的映射，给定输入，经过h变化，得到输出。

![房屋size-price学习过程](https://github.com/yiyading/NLP-and-ML/blob/master/img/%E6%88%BF%E5%B1%8Bsize-price%E5%AD%A6%E4%B9%A0%E8%BF%87%E7%A8%8B.jpg)

学习算法通过对Training Set的学习，输出一个函数h。这个函数的输入为size，输出为price。h代表一个从size到price的映射。

一种可能表达式为h(x)=θ0+θ1\*x，因为只含有一个特征/输入变量，因此这样的问题叫做单变量线性回归问题。

## 2.代价函数（Cost Function） ——又叫“损失函数”
Cost Function的定义，是为了搞清楚如何把最有可能的直线与数据进行拟合。

![Training and Hypothesis](https://github.com/yiyading/NLP-and-ML/blob/master/img/Training-Set%20and%20Hypothesis.jpg)

直线与数据进行拟合，需要选定合适的参数a、b。在房价问题上便是直线在y轴上的截距和址线斜率。而由参数所决定的直线上的预测值与实际值之间的差距（下图中蓝线）就是**建模误差**。

![某个参数的拟合图像](https://github.com/yiyading/NLP-and-ML/blob/master/img/%E6%9F%90%E4%B8%AA%E5%8F%82%E6%95%B0%E7%9A%84%E6%8B%9F%E5%90%88%E5%9B%BE%E5%83%8F.jpg)

我们的目标是选择合适的参数，使得**建模误差的平方和最小**，即使得Cost Function最小。下图展示了Cost Function J和等高线图，等高线图中的坐标有3个。可以看出在三维空间中存在一点使得J最小。

![Cost Function and 等高线图](https://github.com/yiyading/NLP-and-ML/blob/master/img/%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0%E4%B8%8E%E7%AD%89%E9%AB%98%E7%BA%BF%E5%9B%BE.png)

下图展示了单变量线性回归的本质。

![单变量线性回归的本质](https://github.com/yiyading/NLP-and-ML/blob/master/img/%E5%8D%95%E5%8F%98%E9%87%8F%E7%BA%BF%E6%80%A7%E5%9B%9E%E5%BD%92%E6%9C%AC%E8%B4%A8.jpg)

## 3.梯度下降
梯度下降是用来求Cost Function J minimum的算法。

梯度下降的思想是：开始时随机选择一组h的参数组合（在实际问题中，特征可能有很多个，与其对应的参数也会有很多个），计算J，然后找出能让代价函数下降最多的参数组合。直到找到一个局部最小值（local minimum），因为没有尝试所有参数组合，所以不能确认我们得到的局部最小值是否为全局最小值（global minimum）。

![梯度下降找局部最小值](https://github.com/yiyading/NLP-and-ML/blob/master/img/%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D%E6%8B%9B%E5%B1%80%E9%83%A8%E6%9C%80%E5%B0%8F%E5%80%BC.jpg)

> 在这个图像中，平面两坐标为h函数中的两个参数θ0和θ1纵坐标为损失J。<br>
> 梯度下降的目标是找寻一组参数，使得由这组参数确定的函数图像再与实际数据拟合时误差最小。

想象一下，你站在一个山顶，环顾360°，用小碎步尽快下山。每次小碎步都是寻找一个当前最快速的的下山方向，即当前斜率最大的方向下山。

批量梯度下降（batch gradient descent）算法的公式为：

![批量梯度下降](https://github.com/yiyading/NLP-and-ML/blob/master/img_ML/%E6%89%B9%E9%87%8F%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D.png)

其中α是学习率，α决定我们沿代价函数下降最大的地方迈多大的步。

梯度下降算法中，反复执行上边的公式，直到收敛。每次都是同时更新，即都使用旧的θ0和θ1来更新，不能用更新过的θ0来更新旧的θ1。

对梯度下降最直观的理解：**参数的更新就是使用旧参数，减去旧参数这一点的斜率，这个斜率是J值减小方向的斜率（斜率可能正可能负）**
> 二维图像直接减斜率就好了<br>
> 三维图像需要减去下降最快方向的斜率<br>

![梯度下降直观理解](https://github.com/yiyading/NLP-and-ML/blob/master/img_ML/%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D%E7%9B%B4%E8%A7%82%E7%90%86%E8%A7%A3.png)
> 这里假设了θ0为0。
在参数更新的过程中，如果α太小，更新速度会过慢；如果α太大，梯度下降法可能会越过最低点，甚至可能无法收敛。

假设一开始选择的θ1就处在局部最低点，那么下一步使用梯度下降法实际上什么都没做，因为这一点的斜率为0，用旧参数减去0还是旧参数，没有变化。在下降过程中，参数更新会自动地缩小步伐，不需要总是减小α地值。
![梯度更新逐渐变缓](https://github.com/yiyading/NLP-and-ML/blob/master/img_ML/%E6%A2%AF%E5%BA%A6%E6%9B%B4%E6%96%B0%E9%80%90%E6%B8%90%E5%8F%98%E7%BC%93.png)

**总结**：
在梯度下降法中，当我们接近局部最低点时，梯度下降法会自动采取更小的幅度，这是因为当我们接近局部最低点时，很显然在局部最低时导数等于零，所以当我们接近局部最低时，导数值会自动变得越来越小，所以梯度下降将自动采取较小的幅度，这就是梯度下降的做法。所以实际上没有必要再另外减小a。

这就是梯度下降算法，你可以用它来最小化任何代价函数J，不只是线性回归中的代价函数J。

## 4.梯度下降地线性回归

这是第一个学会地机器学习算法：**线性回归的梯度下降算法**

首先看梯度下降算法和线性回归算法的对比：<br>
![梯度下降和线性回归](https://github.com/yiyading/NLP-and-ML/blob/master/img_ML/%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D%E5%92%8C%E7%BA%BF%E6%80%A7%E5%9B%9E%E5%BD%92.png)

将这两个算法合并：<br>
![线性回归梯度下降算法](https://github.com/yiyading/NLP-and-ML/blob/master/img_ML/%E7%BA%BF%E6%80%A7%E5%9B%9E%E5%BD%92%E7%9A%84%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D%E7%AE%97%E6%B3%95.png)

我们刚刚使用的算法有时也称为批量梯度下降。”批量梯度下降 指的是在梯度下降的每一步中我们都用到了所有的训练样本。

在梯度下降中，在计算微分求导项时，我们需要进行求和运算。所以，在每一个单独的梯度下降中，我们最终都要计算这样一个东西，这个项需要对所有m个训练样本求和。

事实上，有时也有其他类型的梯度下降法，不是这种"批量"型的，不考虑整个的训练集，而是每次只关注训练集中的一些小的子集。

