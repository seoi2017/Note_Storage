# Answer A - In Unreal
## Problem Model
给定一个$R$行$C$列的正整数矩阵，请你从这个矩阵中选出一个$N$行$M$列的子矩阵，使得在这个子矩阵中，每一对相邻元素之差的绝对值之和最小。
## Solution
以$2^R$代价枚举所有可能选中的行，对列的选择进行DP，理论时间复杂度$O(\tbinom{N}{R} C^3)$。
## Source Code
见目录下“std/alpha.cc”。

## Original
[NOIP 2014 普及组 T4](https://www.luogu.org/problem/show?pid=2258 "洛谷题目链接")
# Answer B - To Discover
## Problem Model
求$n^k$末尾$t$位在不断乘$n$中是否会发生循环，若是则求出循环节长度。
## Solution
我们设$L(t)$为末尾t位的循环节长度，不难发现$L(t)=mL(t-1)$，其中$1\leqslant m\leqslant 10$，不难发现$m$为$n^{L(t-1)},n^{2L(t-1)},\cdots$中第t位的循环节长度。因此，我们只需从小到大枚举位数$now$，然后在$n^{L(now-1)},n^{2L(now-1)},\cdots$中找第$now$位的循环节长度，因为循环节长度$\leqslant 10$，所以如果枚举超过$10$次仍然没有出现循环，则表明无解，否则，最后的结果就是各位的$m$的乘积。
## Source Code
见目录下“std/beta.cc”。

## Original
[NOIP 2005 普及组 T4](https://www.luogu.org/problem/show?pid=1050 "洛谷题目链接")
# Answer C - By Yourself
## Problem Model
求带环未知变量的期望。
## Solution
由于变量之间存在环，朴素DP和搜索肯定不能使用，考虑列出未知量方程，例如如下数据：
```
a = [b [c 2]]
b = [c [b 3]]
c = [b 4]
```
可列出方程如下：

$x_a=\frac{1}{2}x_b+\frac{1}{4}x_c+\frac{1}{4}*2$

$x_b=\frac{1}{2}x_c+\frac{1}{4}x_b+\frac{1}{4}*3$

$x_c=\frac{1}{2}x_b+\frac{1}{2}*4$

容易发现移项后转为线性方程组，高斯消元求解即可。
## Source Code
见目录下“std/gamma.cc”。

## Original
[POJ 1487](https://vjudge.net/problem/POJ-1487 "VJudge题目链接")
# Answer D - For Victory
## Problem Model
在字符串集$B$内寻找字符串集$A$内的字串出现了多少种，以及包含字符串集$A$内字串种类最多的最短序列长度。
## Solution
第一问Hash即可，第二问考虑答案的单调性。

假设现已找到区间$[L,R]$包含$A$中尽可能多种类的字串，设$req=0$，更新答案，同时$L++$，如果出区间外的字串是整个区间内唯一的，且又是$A$中的，则$req++$；然后$R++$，如果加入区间内的字串是$A$中字串，且是整个区间内唯一的，则$req--$，然后不断继续$R++$，直到$req=0$，重复上述操作。

## Source Code
见目录下“std/delta.cc”。

## Original
[洛谷 P1381](https://www.luogu.org/problem/show?pid=1381 "洛谷题目链接")
# Answer E - Be Clever
## Problem Model
求无向图从$1$号点到$N$号点的一条路径，使路径上经过的最大边权值最小化，计算时至多可以无视路径上的$K$条边。
## Solution
二分答案$ans$，使边权$\leqslant ans$的边权值为$0$，否则为$1$，然后跑最短路，如果最短路$<K$则向下二分，$>K$则向上二分，直到$=K$即为答案。
## Source Code
见目录下“std/sigma.cc”。

## Original
[USACO 2008 一月月赛 银组](https://www.luogu.org/problem/show?pid=1948 "洛谷题目链接")

# Answer F - To Future

## Problem Model

给你$N$个字符，它们是$A$或$B$，要求分出最少集合，使每个集合中要么只有$A$或只有$B$，要么$A$和$B$的个数之差不超过$K$。

## Solution

不难想到使用动态规划解决该问题，设$f[i]$为到第$i$个扇区最少需要划分的防御阵列数量，最朴素的是$O(N^2)$动规，发现大多数状态都是中间状态，于是思考如何简化状态及其转移。

考虑维护每种模块数量的前缀，使用数组记录：$a[i]$表示到第$i$个扇区有多少个$A$模块，$b[i]$同理。然后可得到结论，对于区间$[i,j]$可以划入一个防御阵列，有以下几种情况：

1. $a[i-1]=a[j]$
2. $b[i-1]=b[j]$
3. $abs((a[j]-a[i-1])-(b[j]-b[i-1]))\leqslant K$

对于第三种情况，可以展开成如下形式：

$a[j]-b[j]-M\leqslant a[i-1]-b[i-1]\leqslant a[j]-b[j]+M$

综上，第一、二种情况直接可以求出，第三种情况用线段树维护区间最值即可。

## Source Code

见目录下“std/omega.cc”。

## Original

[洛谷 P2418](https://www.luogu.org/problem/show?pid=2418 "洛谷题目链接")

# Answer F(Fake) - To Future(Really?)
## Problem Model
有一个数列，初始值均为$0$，进行$N$次操作，每次将数列$[A_i,B_i)$这个区间中所有比$C_i$小的数改为$C_i$，计算$N$次操作后数列中所有元素的和。
## Solution
简化后的矩形面积并问题。

需要支持区间插入、删除、求最大值，考虑使用线段树或者平衡树，STL里的multiset也可做。
## Original
[USACO 2007 OPEN 银组](https://www.luogu.org/problem/show?pid=2061 "洛谷题目链接")

# Epilogue
初次出成套的题，尝试重写了题面，可能有些地方表述不清，欢迎批评指正。

每道题前有一段斜体字，连起来就是一个小小说一样的东西。但是由于篇幅限制，表达不出什么太多的东西，不过希望以第一人称闯关的视角，可以给这套枯燥的题目增添些许的趣味性。

事实上，由于难度编排错乱，先考Day2再考Day1可能会好一点，但是毕竟故事的剧情顺序在那里摆着呢，也不方便改了。

最后妥协的方案是，把Day2T3换掉了；最后拿给LYS看的时候，他说这可不能保证你的生命安全，不过由于Day2前两题挺水的，所以也就这样了。

标着“Fake”的那道题解是原本F题的题解，有兴趣的可以看看，不过就不提供代码了。

不过总体来说的效果我还是比较满意的，感谢验题人LYS的毒奶。

>~~”HYS是我们的红太阳 LYY是我们的蓝月亮 我是他们的小迷弟“——LYS~~

本次试题的题解为每道题目提供了标程，全部位于目录下的std文件夹内，希望能起到一定的参考价值。

接下来呢，这个奇幻向的历险故事，还需要一个结尾，不然就烂尾了不是？文笔拙劣，描述多有不周，还望大家看得开心～
>*数据流的攻击似乎是停止了，显像管显示器的屏幕上也不再出现新的警示信息*  
>*额头和手心仍在不断渗出细密的汗珠，老式键盘的亚光键帽上，汗渍的反光正默默地诉说着这无声的战斗*  
>*“终于，结束了么……”*  
>*活动着略显僵硬的手脚，脑海中不由得浮现出她的面容*  
>*“希望一切安好吧”*  
>*银灰色的墙壁开始泛起诡谲的波纹，司空见惯地，意识再一次断开了连接*  
>*……*  
>*和之前截然不同的感觉，除了肌肉的酸疼感，背后的柔软一时没让我反应过来*  
>*缓缓地睁开沉重的眼皮，映入眼帘的是熟悉而又陌生的天花板*  
>*阳光透过窗帘的缝隙打到墙上，依稀能听到断续的蝉鸣*  
>*强撑着身体坐了起来，环视着四周，有种恍然隔世的感觉*  
>*“总算是回来了啊”*  
>*没有像往常一样留恋舒适的床垫，我立即起身、洗漱、换衣，出了门*  
>*呼吸着新鲜的空气，仰望着湛蓝无云的天空，感受着温暖的阳光，一切都是那么的温馨而美好*  
>*毫无目的地漫步在大街上，看着车水马龙的市井街道，竟有种说不出的感受*  
>*回想起来，果然是类似于梦境的东西啊，真是神奇*  
>*话说回来，那诡异的世界，真的是一个彻头彻尾的错误吗?它也应该有它存在的意义和价值吧*  
>*……*  
>*不知道为什么，我只是想在街上走，没有目标，没有理由*  
>*如果硬要说有原因的话，大概只是为了和这阔别许久的现实世界打个招呼罢了*  
>*然而，无情的命运似乎无论如何都不想让我好过，在经过一个岔路口的时候，我和一团黑影相撞了*  
>*好在我的骨头一直很硬，小磕小碰基本没什么感觉，我回过神来，定睛向前方看去*  
>*“一个人，穿着一身黑衣，比较瘦，还有着能垂到手肘的长发，应该是个女生”*  
>*我如是想到*  
>*那人做着和我一样的动作，也将目光向我投来*  
>*看到对方的面容之后，我们互相都是一愣，紧接着便不自主地伸出了手，牢牢地指着对方*  
>*”是你！“”是你？“*  
>*缘，妙不可言*  
>*不知道我们今后又会经历怎样离奇的事情，不过，相信只要有着这份信念和意志，哪怕有一天要离这个世界而去……*  
>*”想必也是面带微笑，无愧人生的吧。“*  
>*……*  
>*炎夏的阳光依然火热，青春的故事才刚刚开始*

最后的最后，衷心祝愿大家NOIP2017都能考出理想的成绩，为校争光，为省添彩！