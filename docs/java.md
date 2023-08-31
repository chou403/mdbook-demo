# 算法

## 算法复杂度

算法的复杂度：复杂度分析、事后统计法、大O表示法、时间复杂度。

### 时间复杂度分析

衡量算法运行速度的指标。

- 只关注循环执行次数最多的一段代码。

- 总复杂度等于最高阶项的复杂度。

- 嵌套代码的复杂度等于嵌套内外代码复杂度的乘积。

  推导大O阶：

  1. 用常数1取代运行时间中的所有加法常数。
  2. 在修改后的运行次数函数中，只保留最高阶项。
  3. 如果最高阶项存在切不是1，则去除与这个项相乘的常数。得到的结果就是大O阶。

  常见的时间复杂度：

  1. O(1) 常数阶
  2. O(n) 线性阶
  3. O(n<sup>2</sup>) 平方阶
  4. O(logn) 对数阶
  5. O(nlogn) 线性对数阶
  6. O(n<sup>3</sup>) 立方阶
  7. O(2<sup>n</sup>) 指数阶
  8. O(n!) 阶乘阶

  从小到大依次是：

  - O(1)<O(logn)<O(n)<O(nlogn)<O(n<sup>2</sup>)<O(n<sup>3</sup>)<O(2<sup>n</sup>)<O(n!)<O(n<sup>n</sup>)。

### 空间复杂度

衡量程序运行临时占用存储空间大小的指标。

- 算法的存储量包括：

  1. 程序本身所占空间。
  2. 输入数据所占空间。
  3. 辅助变量所占空间。

- 输入数据所占空间只取决于问题本身，和算法无关，则只需分析除输入和程序之外的辅助变量所占额外空间。

- 空间复杂度对一个算法在运行过程中临时占用的存储空间大小的量度，一般也作为问题规模n的函数，以数量级形式给出，记作：S(n)=O(g(n))。

  - g(n)的计算规则和时间复杂度一致。

    - 分析1

      ```java
      public int fun(int n){
              int i,j,k,s;
              s=0;
              for (i=0;i<=n;i++)
                  for (j=0;j<=i;j++)
                      for (k=0;k<=j;k++)
                          s++;
              return(s);
          }
      ```

      由于算法中临时标量的个数与问题规模n无关，所以空间复杂度均为S(n) = O(1)。

    - 分析2

      ```java
      public void fun(int a[], int n, int k) {
              int i;
              if (k == n - 1) {
                  for (i = 0; i < n; i++) {
                      //执行n次
                      System.out.println(a[i]);
                  }
              } else {
                  for (i = k; i < n; i++) {
                      //执行n-k次
                      a[i] = a[i] + i * i;
                  }
                  fun(a, n, k + 1);
              }
          }
      ```

      S(n) = O(g(1*n))，此方法属于递归算法，每次调用本身都要分配空间，fun(a,n,0)的空间复杂度为O(n)。

  注意：

  1. 空间复杂度相比时间复杂度分析要少。
  2. 对于递归算法来说，代码一般都比较简短，算法本身所占用的存储空间较少，但运行时需要占用较多的临时工作单元。
  3. 若写成非递归算法，代码一般可能比较长，算法本身占用的存储空间较多，但运行时将可能需要较少的存储单元。

### 递归

1. 一个问题的解可以分解为几个子问题的解。
2. 这个问题与分解之后的子问题，除了数据规模不同，求解思路完全一样。
3. 存在基线/终止条件。

### 栈和队列

1. 栈(堆栈)：后进先出的结构，简称LIFO结构。
2. 队列：先进先出的结构，简称FIFO结构。

### 树

树是n(n>=0) 个结点的有限集。n=0时称为空数。在任何一棵非空树中：

1. 有且仅有一个特定的称为跟(Root)的结点。
2. 当n>1时，其余结点可分为m(m>0)个互不相交的有限集T1、T2、...、Tm，其中每一个集合本身又是一棵树，并且称为跟的子树(SubTree)。

**树的度**：

- 结点拥有的字段数量称为结点的度。根据各个结点的度的不同，结点可以分为：
  - 叶子结点：度为0的结点。
  - 分支结点：度不为0
    - 内部结点：除了跟结点之外，分支结点也叫作内部结点。
- 树的度是树内各结点的度的最大值。

**结点间关系**：

- 当前结点的子树的跟称为该结点的孩子(子结点)。
- 当前结点称为其子结点的双亲【因为当前结点只有一个，所以叫做双亲结点，而不是父/母结点】。
- 同一个双亲的孩子之间互称为兄弟。
- 结点的祖先是从跟结点到当前结点所经分支上的所有结点。
- 以某结点为跟的子树中任一结点都称为该结点的子孙。

**树的深度**(层)：

- 结点的层：从跟结点开始，跟结点为第一层，根的孩子为第二层。若某结点为第i层，则其子树的跟就在第i+1层。
- 其双亲在同一层的结点互为堂兄弟。
- 树中结点的最大层次称为树的深度。

结点的高度=结点到叶子结点的最长路径(边数)。

结点的深度=跟结点到这个结点所经历的边的个数。

结点的层数=结点的深度+1。

树的高度==跟结点的高度。

**树的有序性**：

- 如果树中结点的各子树从左向右是有序的，子树间不能互换位置，则称该树为有序树，否则为无序树。

**二叉树**：

- 二叉树是n(n>=0)个结点的有限集合，该集合或者为空集。或者由一个根结点和两棵互不相交的、分别称为跟结点的左子树和右子树的二叉树组成。
- 特殊二叉树、斜树、满二叉树、完全二叉树。
  - 满二叉树：
    - 二叉树中除了叶子结点，每个结点的度都为2。
    - 满二叉树钟第i层的结点数为2<sup>n-1</sup> 个。
    - 满二叉树为k的满二叉树必有2<sup>k</sup>-1个结点，叶子数为2<sup>k-1</sup>。
    - 满二叉树中不存在度为1的结点，每个分支点钟都有两棵深度相同的子树，切叶子结点都在最底层。
    - 具有n个结点的满二叉树的深度为log<sub>2</sub>(n+1)。
  - 完全二叉树：
    - 二叉树中除去最后一层结点为满二叉树，且最后一层的结点依次从左到右分布。
    - 对于位置为k的结点，左子结点=2*k+1,右子结点=2*(k+1)。
    - 最后一个非叶结点的位置为(n/2)-1,n为数组长度。

## 排序

![image-20230201155528854](https://github.com/chou401/pic-md/raw/master/img/image-20230201155528854.png)

### 冒泡排序

一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果它们的顺序错误就把它们交换过来。走访数列的工作室重复地进行直到没有再需要交换，也就是说该数列已排序完成。这个算法的名字又来是因为元素会经由交换慢慢‘浮’到数列的顶端。

1. 比较响铃的元素。如果第一个比第二个大，就交换它们两个;
2. 对每一对相邻元素做同样的工作，从开始第一队到结尾的最后一对，这样在最后的元素应该会是最大的数;
3. 针对所有的元素重复以上的步骤，除了最后一个;
4. 重复步骤1~3，直到排序完成。

### 选择排序

选择排序的思想其实和冒泡排序有点类似，选择排序可以看成冒泡排序的优化。

* 首先，找到数组中最大（小）的那个元素;
* 其次，将它和数组的第一个元素交换位置（如果第一个元素就是最大（小）元素那么它就和自己交换）;
* 再次，在剩下的元素中找到最大（小）的元素，将它与数组的第二个元素交换位置。如此往复，直到将整个数组排序；
* 这种方法叫做选择排序，因为它在不断地选择剩余元素之中的最大（小）者。

### 插入排序

* 对于未排序数据，在已排序序列中从后往前扫描，找到对应位置并插入。
* 为了给要插入的元素腾出空间，我们需要将插入位置之后的已排序元素在都向右移动一位。插入排序所需的时间取决于输入中元素的初始顺序。例如，对一个很大且其中的元素已经有序（或接近有序）的数组进行排序将会比对随机书序的数组或是逆序数组进行排序要快很多。总的来说，插入排序对于部分有序的数组十分高效，也很适合小规模数组。

### 快速排序

* 快速排序是对冒泡排序的一种改进，也是采用分治法的一个典型的应用。

* 首先任意选取一个数据（比如数组中的第一个数）作为关键数据，我们称为基准数，然后将所有比它小的数都放到它前面，所有比它大的数都放到它后面，这个过程称为一趟快速排序，也称为分区操作。

* 通过一趟快速排序将要排序的数据分隔成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

  ##### 快速排序中的基准数

* 基准的选取：最优的情况是基准值刚好取在无序区的中间，这样能够最大效率地让两边排序，同时最大地减少递归划分的次数，但是一般很难做到最优。基准的选取一般有三种方式，选取数组的第一个元素，选取数组的最后一个元素，以及选取第一个、最后一个以及中间的元素的中位数（如4 5 6 7，第一个4，最后一个7，中间的为5，这三个数的中位数为5，所以选择5作为基准）。

* Dual-Pivot快排：两个基准数的快速排序算法，其实就是用两个基准数，把整个数组分成三份来进行快速排序，在这种新的算法下面，比经典快排从实验来看节省了10%的时间。
  **为了提升性能，有时我们在分隔后独立的两部分的个数小于某个数（比如15）的情况下，会采用其他排序算法，比如插入排序。**

### 希尔排序

* 一种基于插入排序的快速的排序算法。简单插入排序对于大规模乱序数组很慢，因为元素只能一点一点地从数组的一端移动到另一端。

* 希尔排序为了加快速度简单地改进了插入排序，也称为缩小增量排序，同时该算法是冲破O(n<sup>2</sup>)的第一批算法之一。

* 希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；然后缩小增量继续分组排序，随着增量逐渐减少，每组包含的关键字越来越多，当增量减至1时，整个文件恰被分成一组，再次排序，完成整个数组的排序。这个不断缩小的增量，就构成了一个增量序列。

  ##### 希尔排序中的增量序列

* 从理论上说，只要一个数组是递减的，并且最后一个值是1，都可以作为增量序列使用，有没有一个步长序列，使得排序过程中所需的比较和移动次数相对较少，并且无论待排序列记录有多少，算法的时间复杂度都能渐近最佳？但是目前从数学上来说，无法证明某个序列是“最好的”。

* 常用的增量序列

  - 希尔增量序列：{N/2,(N/2)/2,...,1}，其中N为原始数组的长度，这是最常用的序列，但却不是最好的。
  - Hibbard序列：{2<sup>k</sup>-1,..,3,1}。
  - Sedgewick序列：{...,109,41,19,5,1} 表达式为 9*4<sup>i</sup>-9*2<sup>i</sup>+1或者4<sup>i</sup>-3*2<sup>i</sup>+1。

### 归并排序

* 对于给定的一组数据，利用递归与分治技术将数据序列划分成为越来越小的半子表，在对半子表排序后，再用递归方法将排好序的半子表合并成为越来越大的有序序列。
* 为了提升性能，有时我们在半子表的个数小于某个数（比如15）的情况下，对半子表的排序采用其他排序算法，比如插入排序。
* 若将两个有序表合并成一个有序表，称为2-路归并，与之对应的还有多路归并。

### 堆排序

* 许多应用程序都需要处理有序的元素，但不一定要求他们全部有序，或者不一定要一次就将他们排序，很多时候，我们每次只需要操作数据中的最大元素（最小元素），那么有一种基于二叉堆的数据结构可提供支持。
* 所谓二叉堆，是一个完全二叉树的结构，同时满足堆的性质：即子结点的键值或索引总是小于（或者大于）它的父结点。再一个二叉堆中，跟结点总是最大（或者最小）结点，这样堆我们称之为最大（小）堆。
* 堆排序算法就是抓住了这一特点，每次都取堆顶的元素，然后将剩余的元素重新调整为最大（最小）堆，以此类推，最终得到排序的序列。

### 计数排序

* 计数排序、基数排序、桶排序三种排序算法都利用了桶的概念，但对桶的使用方法上有差异。
* 计数排序是一个排序时不比较元素大小的排序算法。
* 计数排序对一定范围内的整数排序时候的速度非常快，一般快于其他排序算法。但计数排序局限性比较大，只限于对整数进行排序，而且待排序元素值分布较连续、跨度小的情况。
* 如果一个数组里所有元素都是整数，而且都在 0-k以内。那对于数组里每个元素来说，如果能知道数组里有多少项小于或等于该元素，就能准确给出该元素在排序后的数组的位置。
  ![image-20230201143216345](https://github.com/chou401/pic-md/raw/master/img/image-20230201143216345.png)
  对于这个数组来说，元素5之前有8个元素小于等于5（含5本身），因此排序后5所在的位置肯定是7，只要构造一个（5+1)
  大小的数组，里面存下所有对应A中每个元素之前的元素个数，就能在线性时间内完成排序。
* 实际应用中我们会同时找出数组中的max和min，主要是为了尽量节省空间。试想[1003,1001,1030,1050]这样的数据要排序，真的需要建立长度为1050+1的数组吗？我们只需要长度为1050-1003+1=48的数组（先不考虑额外+1的长度），就能囊括从最小到最大元素之间的所有元素了。
* 如果待排序数组的元素值跨度很大，比如[9999,1,2]，为三个元素排序要使用9999-1+1的空间，实在是浪费。所以计数排序适用于待排序元素值分布较连续、跨度小的情况。

### 桶排序

* 桶排序是计数排序的升级版。

* 桶排序的工作原理：假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序）。

  ![image-20230201151434039](https://github.com/chou401/pic-md/raw/master/img/image-20230201151434039.png)

* **在桶排序中保证元素均匀分不到每个桶尤为关键。**举个例子，有数组[0,9,4,5,8,7,6,3,2,1]要排序，它们都是10以下的数，如果还按照上面的范围[0,10)建立桶，全部的元素将进入同一个桶中，此时桶排序就失去了意义。实际情况我们很可能事先就不知道输入数据是什么，为了保证元素均匀分不到各个桶中，需要建立多少个桶，每个桶的范围是什么呢？

  其实我们可以这样：简单点，首先限定桶的容量，再根据元素的个数来决定桶的个数。当然使用更复杂的方法也是可以的。

* 桶排序利用函数的映射关系，减少了几乎所有的比较工作。实际上，桶排序的f(k)值得计算，其作用就相当于快排中划分，已经把大量数据分隔成了基本有序的数据块(桶)。然后只需要对桶中的少量数据做先进的比较排序即可。

### 基数排序

- 常见的数据元素一般是由若干位组成的，比如字符串由若干字符组成，整数由若干位0~9数字组成。基数排序按照从右往左的顺序，依次将每一位都当做一次关键字，然后按照该关键字对数组的元素入桶，每一轮入桶都基于上轮入桶的结果；完成所有位的入桶后，整个数组就达到有序状态。
- 比如对于数字2985，从右往左就是先以个位为关键字进行入桶，然后是十位、百位、千位，总共需要四轮。基数排序也是一种无需比较的排序算法。

![image-20230201154905705](https://github.com/chou401/pic-md/raw/master/img/image-20230201154905705.png)

## 位运算

- 位运算基本概念：
  - 十进制：逢十进一
  - 二进制：逢二进一
- 负数的表示
  - 计算机中负数的表示，是以补码的形式呈现的。
  - ![image-20230208153406112](https://github.com/chou401/pic-md/raw/master/img/image-20230208153406112.png)
- 常用的位运算
  - 按位与 & (1&1=1、0&0=0、1&0=0)
  - 按位或 | (1|1=1、0|0=0、1|0=1)
  - 按位非 ~ (-1=0、~0=1)
  - 按位异或 ^ (1^1=0、1^0=1、0^0=0，很明显任何一个数和自己异或结果一定是0)
  - 有符号右移 >> (若正数，高位补0，负数，高位补1)
  - 有符号左移 << (低位补0)
  - 无符号右移 >>> (不论正负，高位均补0)
  - 无符号左移 <<< (低位补0)
- 运用场景
  - 网络连接(SelectionKey.java)
  - HashMap (tableSizeFor)
  - ...
- 常见简单面试题
  - 取模
  - 判断奇偶数
  - 实现数字翻倍或减半
  - 交换两数

## 字符串处理

- 字符串匹配之BF（Bruce Force）算法
- 字符串匹配之BM（Boyer-Moore）算法
  - 坏字符：坏字符的位置 - 模式串中的上一次出现位置
  - 好后缀：好后缀的位置 - 模式串中的上一次出现位置
- 字符串匹配之KMP（Knuth-Morris-Pratt）算法

## 动态规划

### 动态规划定义

动态规划是运筹学的一个分支，是求解决策过程最优化的过程。

在现实生活中，有一类活动，由于它的特殊性，可将过程分成若干个互相联系的阶段，在它的每一阶段都需要作出决策，从而使整个过程达到最好的活动效果。因此各个阶段决策的选取不能任意确定，它依赖于当前面临的状态，又影响以后的发展。

所以如果一类活动过程可以分为若干个互相联系的阶段，在每一个阶段都需作出决策，每一个阶段都有若干个策略可供选择，一个阶段的策略确定以后，形成了本阶段的决策，常常影响到下一阶段的决策，从而就完全确定了一个过程的活动路线，则称它为$\textcolor{red}{多阶段决策问题}$。

当各个阶段决策确定后，就组成一个决策序列，因而也就确定了整个过程的一条活动路线。在多阶段决策问题中，决策依赖于当前状态，又随即引起状态的转义，一个决策序列就是在变化的状态中产生出来的，故有“动态”的含义，称这种解决多阶段决策最优化的过程为$\textcolor{red}{动态规划方法}$。

### 动态规划的解题步骤

1. 确定状态转移公式，当前的状态是怎么由前面的状态变化而来的及其与之相关联的辅助的dp数组（dp table）以及下标的含义。这一步往往也是最难的，这一步想清楚了，整个动态规划的问题基本上可以说解决了一大半。一般来说，首先要确定dp数组中元素代表的意义，然后在这个意义之下，确定状态是在dp数组的元素之间如何变化的。
2. 初始化dp数组。
3. 根绝题目条件确定遍历顺序，并实现状态转移公式。

	同时在实现的过程中，可以适当的输出dp数组的值，确定自己的代码实现思路无误。

### 什么样的问题适合用动态规划？

多阶段决策最优解模型。

1. 最优子结构。
2. 无后效性。
3. 重复子问题。

## 埃氏筛和欧拉筛

- 自然数（非负整数）：从0开始：0,1,2,3,4....。
- 素数（质数）：指在大于1的自然数中，除了1和它本身以外不再有其它因数的自然数。
- 合数：指自然数中除了能被1和本身整除外，还能被其他数（0除外）整除的数。

### 埃氏筛

埃拉托色尼筛选法，简称埃氏筛法， 是针对自然数列中的自然数而实施的，用于求一定范围内的质数。也就是给定整数n，求小于n的所有质数（素数）。
埃拉托斯特尼筛法，简称埃氏筛或爱氏筛，是一种由希腊数学家埃拉托斯特尼所提出的一种简单检定素数的算法。要得到自然数n以内的全部素数，必须把不大于根号n的所有素数的倍数剔除，剩下的就是素数。

**基本思想**

从2开始，将每个质数的倍数都标记成合数，以达到筛选素数的目的。

**缺陷**

对于一个合数，有可能被筛多次。例如 30 = 2*15 = 3*10 = 5*6……（那么如何确保每个合数只被筛选一次呢？我们只要用它的最小[质因子](https://so.csdn.net/so/search?q=质因子&spm=1001.2101.3001.7020)来筛选即可）。

**代码案例**

给出要筛数值的范围n，找出以内的素数。先用2去筛，即把2留下，把2的倍数剔除掉；再用下一个质数，也就是3筛，把3留下，把3的倍数剔除掉；接下去用下一个质数5筛，把5留下，把5的倍数剔除掉；不断重复下去……。

1. 首先将0、1排除：
2. 创建从2到n的连续整数列表，[2,3,4,…,n]；
3. 初始化 p = 2，因为2是最小的质数；
4. 枚举所有p的倍数(2p,3p,4p,…)，标记为非质数（合数）；
5. 找到下一个 没有标记 且 大于p 的数。如果没有，结束运算；如果有，将该值赋予p，重复步骤4;
6. 运算结束后，剩下所有未标记的数都是找到的质数。

```java
public static int eratosthenes(int n) {
    boolean[] isPrime = new boolean[n];// false 代表素数
    int count = 0;
    for(int i = 2;i < n; i++) {
        if(!isPrime[i]) {
            count++;
            for(int j = 2 * i;j < n;j += i) { // j就是合数的标记位
                isPrime[j] = true;
            }
        }
    }
}
```



### 欧拉筛（线性筛）

**前置知识**
正整数的唯一分解定理，即：每个大于1的自然数均可写为质数的积，而且这些素因子按大小排列之后，写法仅有一种方式。

$\textcolor{red}{由唯一分解定理可知，任意一个自然数i必然可以分解为 i = a*b （a为i的最小质因子）}$。

$\textcolor{red}{这里非常重要， 为什么我必须要规定 a为c的最小质因子呢？}$

```java
public static int eratosthenes(int n) {
    // 用来存放质数
    int[] set = new int[n + 1];
    // 用来标记合数
    boolean[] bol = new boolean[n + 1];

    int count = 0;
    for (int i = 2; i < n; i++) {
        // 如果 i 是素数 存放在 set 里
        if (!bol[i]){
            // count 统计的目前 质数的个数 正好可以用这个 count 将 质数顺序的存放在 set 集合中
            set[count] = i;
            count++;
        }

        /*
         根据 合数的定义(一个合数肯定是若干素数的乘积) 所以素数的倍数一定是合数
         所以 用 这个素数, 乘以现在已知的其他素数, 那么得到的会是一个个合数 在 bol 中标记
          */
        for (int j = 0; j < count; j++) {
            // 排除掉 超过 n 的合数
            if (i * set[j] > n) break;
            // 为合数在 bol 中做标记
            bol[i*set[j]] = true;
            if (i%set[j] == 0 ) break;
        }

    }
}
```



这里prime[j] 就代表质数数组 2，3，5，7…。
不难发现，每一轮循环 ，对于每一个prime[j] （质数），他只用了 i 值 一次，这里也是和埃氏筛法 不同的地方（埃氏筛法是 把每一个素数的 倍数在一个循环内标记完，标记出来的就不是素数）， 而欧拉筛法每次只标记每个质数的i倍，标记出来的就不是素数，这是欧拉筛法的大体方向。

细细思考，这两种方法似乎是一样的 ， 有点类似与 a * b 和 b * a 的感觉。

但是真的是这样吗？

大家都知道， 埃氏筛法会有重复标记，这是不可避免的，因为这个算法一下子标记了一个质数的 2，3，4，5，6…n倍，而就在这 2，3，4，5，6…n 这些数中，还可能分解出更小的质数，而这个更小的质数，在执行它的循环时，肯定标记过 从 2到n中的数，而到这次循环时又标记了一次，这种重复计算 极大的降低了效率，而且没有了优化的空间。

比如 12 = 2*6 在 埃氏筛中， 第一轮 质数2 就已经把 12 给筛走了 但是12 = 3 *4 ，第二轮 质数 3 又筛选了一次。

而欧拉筛呢 ，他是第一轮循环 把 记录的所有质数 的 2 倍给 筛出来， 筛出来的就不是素数，第二轮把记录的所有的质数 的 3倍筛出来，筛出来的就不是素数，第三轮把记录的所有质数的4倍筛出来，筛出来的就不是素数…。
如果仅仅按照这个思路， 欧拉筛和埃氏筛一模一样，就是a * b 和b * a 的关系 。

所以欧拉筛的 精华来了。

if(i % prime[j]==0) break

先告诉你它的作用，防止重复筛选。那为什么呢？

我们来看我们一开始说的内容。
由唯一分解定理可知，任意一个自然数i必然可以 i = a*b （ a为c的最小质因子这里非常重要，为什么我必须要规定 a为c的最小质因子呢？
可能 i 还可以被写成 i = c *d 的形式 ，但我们只要 a * b。
我们举个例子

12 = 2 * 6
12 = 3 * 4
当对12 进行筛选的时候 ，在 12 % 2 ==0 那么直接跳出循环，3没有机会再对12进行标记。

## 双指针算法

双指针指的是在遍历对象的过程中，不是普通的使用单个指针进行访问，而是使用两个相同方向（快慢指针）或者相反方向（对撞指针）的指针进行扫描，从而达到相应的目的。最常见的双指针算法有两种：一种是，再一个序列里边，用两个指针维护一段区间；另一种是，在两个序列里边，一个指针指向其中一个序列，另外一个指针指向另一个序列，来维护某种次序。

双指针算法的核心思想（作用）：优化。

在利用双指针算法解题时，考虑原问题如何用暴力算法解出，观察是否可构成单调性，若可以，就可采用双指针算法对暴力算法进行优化，当我们采用朴素的方法即暴力枚举每一种可能的情况，时间复杂度为O(n<sup>2</sup>)，而当我们使用双指针算法通过某种性质就可以将上述的O(n<sup>2</sup>)的操作优化到O(n)。

**暴力算法和双指针算法区别**

由于具有某种单调性，朴素算法往往能优化为双指针算法。

区别：

- 朴素算法每次在第二层遍历的时候，是会从新开始（j会回溯到初始位置）,然后再遍历下去。（假设i是终点，j是起点）。
- 双指针算法：由于具有某种单调性，每次在第二层遍历的时候，不需要回溯到初始位置（单调性），而是在满足要求的位置继续走下去或者更新掉。

```java
public static int removeDuplicates(int[] nums) {
    if(nums.length == 0) {
        return 0;
    }
    int i = 0;
    for(int j = 1;j < nums.length; j++){
        if(nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```



## 二分法

二分法通常又叫二分查找，一般用于查找一个有序数组中的某个值的位置或者给定的特定值的插入位置。

相比把整个数组遍历一次的O(n)复杂度，二分查找可以把复杂度降低到O(logn)。

二分法一定是建立在元素有序的前提下（或数据具有二段性），所以看到题目中出现有序数组等词时，并且要求我们查找某个值或者给一个值求插入位置，或者判断其中是否存在某个值或者利用二分思路求最大最小值等。

二分法思路很简单，无非就是每次根据中值判断，然后缩减区间到原来的一半，二分法最容易出错的地方在于边界和细节处理。

二分法的边界模板分为两种：

- 一种是左闭右闭的区间写法[left，right]；while(left <= right) left 的改变为left=mid+1，right的改变为right=mid-1
- 一种是左闭右开的区间写法[left，right)；while(left < right) left的改变为left=mid+1，right的改变为right=mid

x的平方根肯定在0到x之间，使用二分查找定位该数字，该数字的平方一定是最接近x的，m平方值如果大于x，则往左边找，如果小于等于x则往右边找。找到0和x的最中间的数m，如果m * m > x，则m取x/2到x的中间数字，直到m * m < x，m则为平方根的整数部分。

如果m * m < =x，则取0到x/2的中间值，直到两边的界限重合，找到最大的整数，则为x平方根的整数部分。时间复杂度：O(logn)。

```java 
public static int binarySearch(int x) {
    int index = -1,l = 0,r = x;
    while(l <= r) {
        int mid = l + (r - l)/2;
        if(mid * mid <= x) {
            index = mid;
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    return index;
}
```

## 牛顿迭代

牛顿迭代法是一种求方程f(x)=0近似解的一种方法。

假设我们现在得到的近似解是xi，然后我们画出在(xi，f(xi))点与曲线相切的直线，该直线与x轴相交会得到一个xi+1。根据导数与直线斜率的关系，我们可以得到：f'(xi)=(f(xi))/(xi-xi+1)。整理后得到如下递推式：xi+1=xi-f(xi)/f'(xi)。根据本递推式我们可以知道如果曲线平滑，随着迭代次数的增加，xi将愈发接近方程的解。牛顿迭代法的收敛率是平方级的，即每次迭代精度会翻倍。然而在方程有多个解时可能最后收敛的结果只会确定少数的解。

应用牛顿-拉弗森方法，要注意以下问题：

- 函数在整个定义域内最好是二阶可导的
- 起始点对求根计算影响重大，可以增加一些别的判断手段进行试错

假设平方根是i，则i和x/i必然都是x的因子，而x/i必然等于i，推导出i + x/i = 2 * i，得出i = (i + x/i) / 2。

由此得出解法，i可以任选一个值，只要上述公式成立，i必然就是x的平方根，如果不成立， (i + x/i) / 2 得出的值进行递归，直至得出正确解。

```java
public int newton(int x) {
    if(x == 0) {
        return 0;
    }
    return (int)sqrt(x,x);
}
public double sqrt(double i, int x) {
    double res = (i + x/i)/2;
    if(res == i) {
        return i;
    } else {
        return sqrt(res, x);
    }
}
```

# Spring

## 常见异常

| 异常                                   | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| ClassCastException                     | 类型强制转换异常                                             |
| NegativeArrayException                 | 数组负下标异常                                               |
| ArrayIndexOutOfBoundsException         | 数组下标越界异常                                             |
| SecturityException                     | 违背安全原则异常                                             |
| EOFException                           | 文件已结束异常                                               |
| FileNotFoundException                  | 文件未找到异常                                               |
| NumberFormatException                  | 字符串转换为数字异常                                         |
| SQLException                           | 操作数据库异常                                               |
| IOException                            | 输入输出异常                                                 |
| NoSuchMethodException                  | 方法未找到异常                                               |
| Java.lang.AbstractMethodError          | 抽象方法错误。当应用试图调用抽象方法时抛出。                 |
| java.lang.AssertionError               | 断言错。用来指示一个断言失败的情况。                         |
| java.lang.ClassCircularityError        | 类循环依赖错误。在初始化一个类时，若检测到类之间循环依赖则抛出该异常。 |
| java.lang.ClassFormatError             | 类格式错误。当Java虚拟机试图从一个文件中读取Java类，而检测到该文件的内容不符合类的有效格式时抛出。 |
| java.lang.Error                        | 错误。是所有错误的基类，用于标识严重的程序运行问题。这些问题通常描述一些不应被应用程序捕获的反常情况。 |
| java.lang.ExceptionInInitializerError  | 初始化程序错误。当执行一个类的静态初始化程序的过程中，发生了异常时抛出。静态初始化程序是指直接包含于类中的static语句段。 |
| java.lang.IllegalAccessError           | 违法访问错误。当一个应用试图访问、修改某个类的域(Field)或者调用其方法，但是又违反域或方法的可见性声明，则抛出该异常。 |
| java.lang.IncompatibleClassChangeError | 不兼容的类变化错误。当正在执行的方法所依赖的类定义发生了不兼容的改变时，抛出该异常。一般在修改了应用中的某些类的声明定义而没有对整个应用重新编译而直接运行的情况下，容易引发该错误。 |
| java.lang.InstantiationError           | 实例化错误。当一个应用试图通过Java的new操作符构造一个抽象类或者接口时抛出该异常. |
| java.lang.InternalError                | 内部错误。用于指示Java虚拟机发生了内部错误。                 |
| java.lang.LinkageError                 | 链接错误。该错误及其所有子类指示某个类依赖于另外一些类，在该类编译之后，被依赖的类改变了其类定义而没有重新编译所有的类，进而引发错误的情况。 |
| java.lang.NoClassDefFoundError         | 未找到类定义错误。当Java虚拟机或者类装载器试图实例化某个类，而找不到该类的定义时抛出该错误。 |
| java.lang.NoSuchFieldError             | 域不存在错误。当应用试图访问或者修改某类的某个域，而该类的定义中没有该域的定义时抛出该错误。 |
| java.lang.NoSuchMethodError            | 方法不存在错误。当应用试图调用某类的某个方法，而该类的定义中没有该方法的定义时抛出该错误。 |
| java.lang.OutOfMemoryError             | 内存不足错误。当可用内存不足以让Java虚拟机分配给一个对象时抛出该错误。 |
| java.lang.StackOverflowError           | 堆栈溢出错误。当一个应用递归调用的层次太深而导致堆栈溢出时抛出该错误。 |
| java.lang.ThreadDeath                  | 线程结束。当调用Thread类的stop方法时抛出该错误，用于指示线程结束。 |
| java.lang.UnknownError                 | 未知错误。用于指示Java虚拟机发生了未知严重错误的情况。       |
| java.lang.UnsatisfiedLinkError         | 未满足的链接错误。当Java虚拟机未找到某个类的声明为native方法的本机语言定义时抛出。 |
| java.lang.UnsupportedClassVersionError | 不支持的类版本错误。当Java虚拟机试图从读取某个类文件，但是发现该文件的主、次版本号不被当前Java虚拟机支持的时候，抛出该错误。 |
| java.lang.VerifyError                  | 验证错误。当验证器检测到某个类文件中存在内部不兼容或者安全问题时抛出该错误。 |
| java.lang.VirtualMachineError          | 虚拟机错误。用于指示虚拟机被破坏或者继续执行操作所需的资源不足的情况 |

## lambda

求两个对象 list 的交集

```java
private List<People> sameList(List<People> oldArrayList, List<People> newArrayList) {
    List<People> resultList = newArrayList.stream()
            .filter(item -> oldArrayList.stream().map(e -> e.getCode())
                    .collect(Collectors.toList()).contains(item.getCode()))
            .collect(Collectors.toList());
    return resultList;
}
```

求两个对象 list 的差集

```java
private List<People> diffList(List<People> firstArrayList, List<People> secondArrayList) {
        List<People> resultList = firstArrayList.stream()
                .filter(item -> !secondArrayList.stream().map(e -> e.getCode()).collect(Collectors.toList()).contains(item.getCode()))
                .collect(Collectors.toList());
        return resultList;
    }
```

list 去除另一个 list 重复元素的数据

```java
firstList.removeIf(first -> secondList.stream().anyMatch(second -> first.getCode().equals(second.getCode())));
```



## 正则

(?!0(\.0+)?$) 使用负向前瞻，排除0、0.0、0.00等数字。

(?!0+(\.\d+)?$) 使用负向前瞻，排除以0开头的数字，如0.123、0.001等。

\d+(\.\d+)? 匹配正整数和小数，其中小数点后面至少有一位数字



## Java SPI

SPI: Service Provider Interface，它是从 Java6 开始引入的，是一种基于 Classloader 来发现并加载服务的机制。

一个标准的 SPI，由三个组件构成，分别是：

- Service：是一个公开的接口或抽象类，定义了一个抽象的功能模块
- Service Provider：是 Service 接口的一个实现类
- ServiceLoader：是 SPI 机制中的核心组件，负责在运行时发现并加载 Service Provider

### 运行流程

![image-20230821143535836](https://github.com/chou401/pic-md/raw/master/image-20230821143535836.png)

### 三大规范要素

- 规范的配置文件
  - 文件路径：必须在 jar 包中 META-INF/services 目录下
  - 文件名称：Service 接口的全限定名
  - 文件内容：Service 实现类的全限定名。如果有多个实现类，那么每一个实现类中单独占据一行
- Service Provider 类必须具备无参的默认构造方法
  - Service 接口的实现类，即 Service Provider 类，必须具备无参的默认构造方法。因为随后通过反射技术实例化它时，是不带参数的
- 保证能加载到配置文件和 Service Provider 类
  - 方式一：将 Service Provider 的 jar 包放到 classpath 中
  - 方式二：将 jar 包安装到 jre 的扩展目录中
  - 方式三：自定义一个 ClassLoader

### Java SPI 与 SpringBoot 自动配置

![image-20230821154836454](https://github.com/chou401/pic-md/raw/master/image-20230821154836454.png)

该如何实现 Auto-Configuration

- 首先，不能脱离 springboot 框架另起炉灶
  - 继续使用@Configuration 等已有注解
- 第二，对用户友好，将“全自动”贯彻到底
  - 参考 Java SPI 的设计思想

![image-20230821155311581](https://github.com/chou401/pic-md/raw/master/image-20230821155311581.png)



## Slf4j 

SLF4J：全称是 Simple Logging Facade for Java，Java 的简单日志门面，是现在 Java 生态中最流行的一个门面日志框架。

![image-20230822165303257](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230822165303257.png)

![image-20230822165834387](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230822165834387.png)

 



## Spring boot

为什么打成的jar包，通过java -jar xxx.jar 就能启动运行？

jar包里面含有一下三部分文件

![image-20230404172555912](https://github.com/chou401/pic-md/raw/master/img/image-20230404172555912.png)

> org：主要存放springboot相关的class文件
> META-INF：主要存放maven和MANIFEST.MF文件
> BOOT-INF/classes：主要存放应用编译后的class文件
> BOOT-INF/lib：主要存放应用依赖的jar包文件

打开META-INF下MENIFEST.MF文件，内容如下：

![image-20230404172728434](https://github.com/chou401/pic-md/raw/master/img/image-20230404172728434.png)

从上述MANIFEST.MF中可以看到Main-Class及Start-Class配置，其中Main-Class指定jar文件的入口类JarLauncher，当使用java -jar执行jar包的时候会调用JarLauncher的main方法，而不是我们编写的启动类。

JarLauncher叫做jar包启动器，当我们运行java -jar 的时候就会找到这个启动器。

这个启动器是我们在打包的时候弄进去的，也就是我们在pom文件中加入的插件：

```xml
<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
</plugins>
</build>
```

> 注意：
> 只有添加了spring-boot-maven-plugin插件，运行mvn:package命令后打成的jar包才能直接运行。

这个插件在打包时候就会把jar启动器添加进去。

**java -jar 做了什么**

> 官网对java -jar的解释如下：
>
> If the -jar option is specified, its argument is the name of the JAR file containing class and resource files for the application. The startup class must be indicated by the Main-Class manifest header in its source code.
>
> 如果指定了-jar选项，它的参数是包含应用程序的类和资源文件的JAR文件的名称。启动类必须由其源代码中的Main-Class清单头指明。

由此可见：当使用java -jar 启动springboot jar包时是去找Manifast.MF文件中的Main-Class指定的类来启动项目。

**JarLauncher的执行流程**

> org.springframework.boot.loader.JarLauncher#main
>     org.springframework.boot.loader.Launcher#launch(java.lang.String[])
>         org.springframework.boot.loader.Launcher#launch(java.lang.String[], java.lang.String, java.lang.ClassLoader) 中的第二个参数mainClass-》org.springframework.boot.loader.ExecutableArchiveLauncher#getMainClass
>             org.springframework.boot.loader.Launcher#createMainMethodRunner
>                 org.springframework.boot.loader.MainMethodRunner#run

添加spring-boot-loader依赖，才可以查看源码

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-loader</artifactId>
	<version>2.1.16.RELEASE</version>
</dependency>
```

```java
package org.springframework.boot.loader;
 
import org.springframework.boot.loader.archive.Archive;
 
/**
 * {@link Launcher} for JAR based archives. This launcher assumes that dependency jars are
 * included inside a {@code /BOOT-INF/lib} directory and that application classes are
 * included inside a {@code /BOOT-INF/classes} directory.
 *
 * {@link Launcher}用于基于JAR的档案。该启动程序假设依赖jar包含在{@code BOOT-INFlib}目录中，并且应用程序类包含在{@code BOOT-INFclasses}目录中。
 *
 * @author Phillip Webb
 * @author Andy Wilkinson
 * @since 1.0.0
 */
public class JarLauncher extends ExecutableArchiveLauncher {
 
	static final String BOOT_INF_CLASSES = "BOOT-INF/classes/";
 
	static final String BOOT_INF_LIB = "BOOT-INF/lib/";
 
	public JarLauncher() {
	}
 
	protected JarLauncher(Archive archive) {
		super(archive);
	}
 
	@Override
	protected boolean isNestedArchive(Archive.Entry entry) {
		if (entry.isDirectory()) {
			return entry.getName().equals(BOOT_INF_CLASSES);
		}
		return entry.getName().startsWith(BOOT_INF_LIB);
	}
	//main方法入口， 构造JarLauncher，然后调用它的launch方法。参数是控制台传递的
	public static void main(String[] args) throws Exception {
		new JarLauncher().launch(args);
	}
 
}
```

**启动类基类**：org.springframework.boot.loader.Launcher-》org.springframework.boot.loader.Launcher#launch(java.lang.String[])

```java
/**
 * Base class for launchers that can start an application with a fully configured
 * classpath backed by one or more {@link Archive}s.
 *	启动器的基类，可以用一个或多个{@link Archive}支持的完整配置的类路径启动应用程序。
 * @author Phillip Webb
 * @author Dave Syer
 * @since 1.0.0
 */
public abstract class Launcher {
 
	/**
	 * Launch the application. This method is the initial entry point that should be
	 * called by a subclass {@code public static void main(String[] args)} method.
	 * 启动应用程序。这个方法是一个初始入口点，它应该被子类{@code public static void main(String[] args)}方法调用。
	 * @param args the incoming arguments
	 * @throws Exception if the application fails to launch
	 */
	protected void launch(String[] args) throws Exception {
		//在系统属性中设置注册了自定义的URL处理器：org.springframework.boot.loader.jar.Handler。如果URL中没有指定处理器，会去系统属性中查询
		JarFile.registerUrlProtocolHandler();
		// getClassPathArchives方法在会去找lib目录下对应的第三方依赖JarFileArchive，同时也会项目自身的JarFileArchive
        // 根据getClassPathArchives得到的JarFileArchive集合去创建类加载器ClassLoader。这里会构造一个LaunchedURLClassLoader类加载器，这个类加载器继承URLClassLoader，并使用这些JarFileArchive集合的URL构造成URLClassPath
        // LaunchedURLClassLoader类加载器的父类加载器是当前执行类JarLauncher的类加载器
		ClassLoader classLoader = createClassLoader(getClassPathArchives());
		// getMainClass方法会去项目自身的Archive中的Manifest中找出key为Start-Class的类
		// 调用重载方法launch
		launch(args, getMainClass(), classLoader);
	}
	......
	......
}
```

可执行存档启动类的基类：org.springframework.boot.loader.ExecutableArchiveLauncher -》org.springframework.boot.loader.ExecutableArchiveLauncher#getMainClass

```java
/**
 * Base class for executable archive {@link Launcher}s.
 * 可执行存档{@link启动器}的基类。
 * @author Phillip Webb
 * @author Andy Wilkinson
 * @since 1.0.0
 */
public abstract class ExecutableArchiveLauncher extends Launcher {
 
	private final Archive archive;
 
	public ExecutableArchiveLauncher() {
		try {
			this.archive = createArchive();
		}
		catch (Exception ex) {
			throw new IllegalStateException(ex);
		}
	}
 
	protected ExecutableArchiveLauncher(Archive archive) {
		this.archive = archive;
	}
 
	protected final Archive getArchive() {
		return this.archive;
	}
 
	@Override
	protected String getMainClass() throws Exception {
		//跟代码this.archive.getManifest()发现对应的是org.springframework.boot.loader.archive.JarFileArchive#getManifest
		//继续跟进入org.springframework.boot.loader.jar.JarFile#getManifest
		//最后org.springframework.boot.loader.jar.JarFile#getManifest对应的是META-INF/MANIFEST.MF文件的内容
		Manifest manifest = this.archive.getManifest();
		String mainClass = null;
		if (manifest != null) {
			//获取META-INF/MANIFEST.MF文件内容中的Start-Class，这才是我们自己写的启动类
			mainClass = manifest.getMainAttributes().getValue("Start-Class");
		}
		if (mainClass == null) {
			throw new IllegalStateException("No 'Start-Class' manifest entry specified in " + this);
		}
		return mainClass;
	}
	......
	......
}
```

到此可以看出org.springframework.boot.loader.Launcher#launch(java.lang.String[], java.lang.String, java.lang.ClassLoader)中的mainClass参数正是我们自己写的启动类，也就是META-INF/MANIFEST.MF文件中Start-Class指定的类。

**Launcher的launch方法**：org.springframework.boot.loader.Launcher#launch(java.lang.String[], java.lang.String, java.lang.ClassLoader)

```java
/**
 * Launch the application given the archive file and a fully configured classloader.
 * 启动应用程序，给出归档文件和一个完全配置的类加载器。
 * @param args the incoming arguments
 * @param mainClass the main class to run
 * @param classLoader the classloader
 * @throws Exception if the launch fails
 */
protected void launch(String[] args, String mainClass, ClassLoader classLoader) throws Exception {
	Thread.currentThread().setContextClassLoader(classLoader);
	createMainMethodRunner(mainClass, args, classLoader).run();
}
```

**org.springframework.boot.loader.Launcher#createMainMethodRunner**

```java
/**
 * Create the {@code MainMethodRunner} used to launch the application.
 * @param mainClass the main class
 * @param args the incoming arguments
 * @param classLoader the classloader
 * @return the main method runner
 */
protected MainMethodRunner createMainMethodRunner(String mainClass, String[] args, ClassLoader classLoader) {
	return new MainMethodRunner(mainClass, args);
}
```

**MainMethodRunner**：org.springframework.boot.loader.MainMethodRunner-》org.springframework.boot.loader.MainMethodRunner#run 

```java
/**
 * Utility class that is used by {@link Launcher}s to call a main method. The class
 * containing the main method is loaded using the thread context class loader.
 * 被{@link Launcher}用来调用main方法的实用程序类。包含main方法的类是使用线程上下文类装入器装入的。
 * @author Phillip Webb
 * @author Andy Wilkinson
 * @since 1.0.0
 */
public class MainMethodRunner {
 
	private final String mainClassName;
 
	private final String[] args;
 
	/**
	 * Create a new {@link MainMethodRunner} instance.
	 * @param mainClass the main class
	 * @param args incoming arguments
	 */
	public MainMethodRunner(String mainClass, String[] args) {
		this.mainClassName = mainClass;
		this.args = (args != null) ? args.clone() : null;
	}
 
	public void run() throws Exception {
		Class<?> mainClass = Thread.currentThread().getContextClassLoader().loadClass(this.mainClassName);
		Method mainMethod = mainClass.getDeclaredMethod("main", String[].class);
		mainMethod.invoke(null, new Object[] { this.args });
	}
 
}
```

上述关键代码执行流程简化如下图:

![img](https://raw.githubusercontent.com/chou401/pic-md/master/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0VmbHlpbmdz,size_16,color_FFFFFF,t_70.png)



**使用依赖包common** 

common 执行mvn install时，会报错提示 Unable to find a single main class。spring boot项目使用maven打包，如果没有做配置的话，会自动寻找签名是public static void main(String[] args)的方法。common 只是一个服务工程，本来就不会存在启动入口。

在common中添加过滤配置即可打包成功：

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <skip>true</skip>
    </configuration>
</plugin>
```

**jar启动器的作用**

当我们使用java -jar的时候 JarLauncher 会将BOOT-INF/classes下的类文件和BOOT-INF/lib下依赖的jar包加载到classpath下，最后调用META-INF下的MANIFEST.MF文件的Start-Class属性来完成应用程序的启动。

### Spring 自动配置

![image-20230823113954382](/Users/chouchou/Library/Application Support/typora-user-images/image-20230823113954382.png)

SpringBoot 自动配置，Auto-Configuration

- 它是指基于你引入的依赖 jar 包，对 SpringBoot 应用进行自动配置
- 它是SpringBoot框架的“开箱即用”提供了基础支撑

术语“配置类”，Configuration Class

- 广义的“配置类”：被注解@Component 直接或间接修饰的某个类，即我们常说的 Spring 组件，其中包括了@Configuration 类
- 狭义的“配置类”：特指被注解 @Configuration 所修饰的某个类，又成为 @Configuration 类

![image-20230822172218836](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230822172218836.png)

![image-20230822172402498](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230822172402498.png)

![image-20230822173011734](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230822173011734.png)

![image-20230823113418389](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823113418389.png)

![image-20230823113448199](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823113448199.png)

![image-20230823113602551](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823113602551.png)

![image-20230823113646014](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823113646014.png)



#### @Conditional

- 它的作用是实现：只有在特定条件满足时，才会向 IOC 容器注册指定的组件
- 我们可以将 @Conditional 理解为某种 if 语句

![image-20230823113909162](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823113909162.png)



####  @ComponentScan

@ComponentScan，来自 Spring 框架的一个注解

- 对指定的 package 进行扫描，找到其中符合条件的类，默认是搜索被注解@Component 修饰的配置类
- 通过属性 basePackages 或 basePackageClasses，来指定要进行扫描的 package
- 如果未指定 package，则默认扫描当前 @ComponentScan 所修饰的类所在的 package

####  @Import & @Configuration

通常，`@Import` 和 `@Configuration` 共同使用，用于在容器中注册对应的 **BeanDefinition**。

##### @Configuration

被 `@Configuration` 标注的类，通常称为 **配置类**，但配置类，并不单单指标注了 `@Configuration` 的类

- 标注了 @Configuration 的配置类，称为 FULL 配置类，在 FULL 配置类中注册的 组件类，会在 ConfigurationClassPostProcessor#postProcessBeanFactory 方法中被 ConfigurationClassEnhancer 代理，交由 容器 处理依赖关系
- 而对于没有标注 `@Configuration`，但是标注了 `@Import` `Component` `ComponentScan` `ImportResource` 或者含有被 `Bean` 注解标注方法的配置类，称为 **LITE** 配置类
- 对于 FULL配置类 和 LITE配置类，ConfigurationClassPostProcessor 会委托 ConfigurationClassParser 类解析处理对应的 Component @PropertySource @ComponentScan @Import @ImportResource 注解以及标注了 @Bean 的方法

###### FULL & LITE

**FULL配置类** & **LITE配置类** 的区别，可以看以下示例体会

```java
public class A {
	public A() {
		System.out.println("...A");
	}
}

public class B {
	public B(A a) {
		System.out.println("...B");
	}
}

@Configuration
@ComponentScan("com.xsn.configurationtest")
public class MyConfigurationConfig {

	@Bean
	public A a() {
		return new A();
	}

	@Bean
	public B b() {
		return new B(a());
	}
}

public static void main(String[] args) {
	AnnotationConfigApplicationContext ac =
			new AnnotationConfigApplicationContext(MyConfigurationConfig.class);

	A a = ac.getBean(A.class);
	System.out.println(a);

	B b = ac.getBean(B.class);
	System.out.println(b);

}

结果：
FULL配置类（加了 @Configuration）：
...A
...B

LITE配置类（注释掉 @Configuration）：
...A
...A
...B

```

可以看到在 **FULL配置类** 下，**A** 和 **B** 的 **依赖关系** 将由 **容器** 处理。

我们可以看到，没有 @Configuration 修饰的时候，A 被实例化了两次。

现在我们搞清楚了@Configuration主要是用来做什么的了吧，它就是为了能够让我们spring，比如所我们的springboot或者spring在写上这种@Bean注解的时候（利用Java Config注解来完成对spring环境开发的时候），保证bean的一个作用域，保证它（bean）的生命周期跟它的作用域，包括Scope，Scope就是我们讲的单例（singletion）和原型（prototype）。

正常来说，我们在 B 中实现了 A 就应该是实例化两次，为什么会出现添加了 @Configuration 只实例化一次的情况那？

那么它没有执行两遍只有一个原因，你们可以思考一下，当我们一个方法调用的时候它的预期结果跟你想的不一样，那么你觉得是什么导致的。说白了，就是这个方法被改变了，这个方法不再是原来的这个方法，。因为我们这个方法，它的结果是必然的，它必然会产生一个类，但是我们调用两次它只产生一个类。那就违背了它的必然结果，那就说明了这个代码已经被别人改了。那么被随改了，肯定是被spring改了。那么按照我们现在所学的知识当中，我们要去改变一个方法的行为，没有改源码，可以用什么实现？

在这里用的是cglib动态代理。

不添加 @Configuration

```java
public static void main(String[] args) {
	AnnotationConfigApplicationContext ac =
			new AnnotationConfigApplicationContext(MyConfigurationConfig.class);

	MyConfigurationConfig myConfigurationConfig = ac.getBean(MyConfigurationConfig.class);
	System.out.println(myConfigurationConfig);

}

结果：
  （注释掉 @Configuration）
  xxx.MyConfigurationConfig@2833cc44
  （加了 @Configuration）
  xxx.MyConfigurationConfig$$EnhancerBySpringCGLIB$$e52e37ff@588df31b
```

当然这里，会有疑问既然加不加 @Configuration 都可以获取 bean，那为什么还要使用那？

别傻了，小伙子，这里只是一个特殊情况，我们自己手动注册实现的，AnnotationConfigApplicationContext 里面会执行 this.register(componentClasses)，正常情况，不添加 @Configuration，Spring 容器根本就不会扫描，怎么可能会获取到 bean 那。

##### @Import

- 提供了一种显示地从其他地方加载配置类的方式，这样可以避免使用性能较差的组件扫描（Component Scan）
- 属性值
  - @Import 的属性值可以是一个 配置类，此处的配置类，可以是一个 @Configuration 注解标注的配置类（FULL 配置类）、一个标注了 @Import Component ComponentScan ImportResource 或者含有被 Bean 注解标注的方法（LITE 配置类）、甚至一个普通类

  - 一个实现了 **ImportSelector** 的类
  - 一个实现了 **ImportBeanDefinitionRegistrar** 的类

`@Import` 和 `@Configuration` 共同使用，用于在容器中注册对应的 **BeanDefinition**

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Import {

	Class<?>[] value();

}
```

**配置类**

```java
public class C {
	public C() {
		System.out.println("...c");
	}
}

@Configuration
@ComponentScan("com.xsn.configurationtest")
@Import(C.class)
public class MyConfigurationConfig {

}

public static void main(String[] args) {
	AnnotationConfigApplicationContext ac =
			new AnnotationConfigApplicationContext(MyConfigurationConfig.class);

	C c = ac.getBean(C.class);
	System.out.println(c);
}

结果：
com.xsn.configurationtest.C@71ba6d4e
```

类 **C** 就是一个普通类，同样也可以 **Import** 一个配置类（**FULL** & **LITE**），如下

```java
@Configuration
@Import(C.class)
public class ImportConfiguration {

}

@Configuration
@ComponentScan("com.xsn.configurationtest")
@Import(ImportConfiguration.class)
public class MyConfigurationConfig {

}

public static void main(String[] args) {
	AnnotationConfigApplicationContext ac =
			new AnnotationConfigApplicationContext(MyConfigurationConfig.class);

	C c = ac.getBean(C.class);
	System.out.println(c);
}

结果：
com.xsn.configurationtest.C@71ba6d4e
```

###### ImportSelector

同样，`@Import` 注解可以 **Import** 一个 **ImportSelector** 的实现类

```java
public interface ImportSelector {

	// 返回要 Import 的配置类名
	String[] selectImports(AnnotationMetadata importingClassMetadata);

	// 允许提供一个 Predicate 过滤 selectImports 方法对应的类
	@Nullable
	default Predicate<String> getExclusionFilter() {
		return null;
	}

}
```

类似于直接 Import 配置类，但是 ImportSelector 的实现类可以基于对应配置类的 AnnotationMetadata 属性进行 select，同时还可以实现各种 Aware 接口类似 EnvironmentAware BeanFactoryAware 等，持有对应的 Environment BeanFactory 来进行 select

```java
public class MyImportSelector implements ImportSelector {

	@Override
	public String[] selectImports(AnnotationMetadata importingClassMetadata) {
		StandardAnnotationMetadata standardAnnotationMetadata
				= (StandardAnnotationMetadata) importingClassMetadata;
		return new String[]{"com.xsn.configurationtest.C"};
	}

	@Override
	public Predicate<String> getExclusionFilter() {
		return null;
	}
}

@Configuration
@ComponentScan("com.xsn.configurationtest")
@Import(MyImportSelector.class)
public class MyConfigurationConfig {

}

public static void main(String[] args) {
	AnnotationConfigApplicationContext ac =
			new AnnotationConfigApplicationContext(MyConfigurationConfig.class);

	C c = ac.getBean(C.class);
	System.out.println(c);
}

结果：
com.xsn.configurationtest.C@5ab956d7
```

###### DeferredImportSelector

**DeferredImportSelector** 继承了 **ImportSelector**，当 **Import** 的是一个 **DeferredImportSelector** 时，该 **DeferredImportSelector** 会在最后解析，主要用于有 `@Conditional` 注解的配置类

###### ImportBeanDefinitionRegistrar

同样，`@Import` 注解可以 **Import** 一个 **ImportBeanDefinitionRegistrar** 的实现类

```java
public interface ImportBeanDefinitionRegistrar {

	// 基于 AnnotationMetadata BeanDefinitionRegistry 注册对应的 BeanDefinition
	default void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry,
			BeanNameGenerator importBeanNameGenerator) {

		registerBeanDefinitions(importingClassMetadata, registry);
	}

	default void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
	}

}
```

**ImportBeanDefinitionRegistrar** 也可以实现各种 **Aware** 接口类似 **EnvironmentAware** **BeanFactoryAware** 等，持有对应的 **Environment** **BeanFactory** 来进行 `register`

我们使用 **Spring AOP** 时，在配置类上添加的 `@EnableAspectJAutoProxy` 注解

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(AspectJAutoProxyRegistrar.class)
public @interface EnableAspectJAutoProxy
```

它 Import 的 AspectJAutoProxyRegistrar 便是一个 ImportBeanDefinitionRegistrar，该类会帮我们注册一个 AnnotationAwareAspectJAutoProxyCreator 的 BeanDefinition，实现 AOP 代理



### Spring 自动装配

自动配置：Auto-Configuration

自动装配：Autowire

这是两个不同的东西。

#### Spring注解发展过程

SpringBoot的自动装配依赖于注解，所以我们先来看一下注解的发展过程。

以下主要对核心注解进行说明

- **Spring1.0**：刚刚出现注解。

  - @Transaction：简化了事务的操作

- **Spring2.0**：一些配置开始被 xml 代替，但是还不能完全摆脱xml，主要是component-scan标签。

  - @Required：用在set方法上，如果加上该注解，表示在xml中必须设置属性的值，不然就会报错。
  - @Aspect ：AOP相关的一个注解，用来标识配置类。
  - @Autowired，@Qualifier：依赖注入
  - @Component，@Service，@Controller，@Repository：主要是声明一些bean对象放入IOC中。
  - @RequestMapping： 声明请求对应的处理方法

- **Spring3.0**：已经完全可以用注解代替xml文件了

  - @Configuration：配置类，代理xml配置文件

  - @ComponentScan：扫描其他注解，代理xml中的component-scan标签。

  - @Import：只能用在类上，主要是用来加载第三方的类。

    - @import(value = {XXX.class})：加载一个普通的类

    - @Import(MyImportSelector.class)：这种主要是根据业务选择性加载一些类。

      ```java
      public class MyImportSelector implements ImportSelector {//继承该接口
          @Override　　//重写selectImports方法
          public String[] selectImports(AnnotationMetadata importingClassMetadata) {
              //返回对象对应的类型的全类路径的字符串数组
              return new String[]{XXX1.class.getName(), XXX2.class.getName()};
          }
      }
      ```

    - @Import(MyImportBeanDefinitionRegistrar.class)：跟上面一样，都是根据业务选择性的加载一些类。只是返回的内容不一样，上面是直接返回选择的类的全路径，这个是将加载的类注册到一个BeanDefinitionRegistry中返回。

      ```java
      public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {//继承该接口
      
          @Override   //重写registerBeanDefinitions方法
          public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
               // 将需要注册的对象封装为 RootBeanDefinition 对象 
              RootBeanDefinition xxx1 = new RootBeanDefinition(XXX1.class);
              registry.registerBeanDefinition("xxx1", xxx1);
              //再注册一个
              RootBeanDefinition xxx2 = new RootBeanDefinition(XXX2.class);
              registry.registerBeanDefinition("xxx2", xxx2);
          }
      }
      ```

- **Spring4.0**：

  - @Conditional：按照一定的条件进行判断，满足条件就给容器注册Bean实例。

    ```java
    /**
     * 定义一个 Condition 接口的是实现
     */
    public class MyCondition implements Condition {
        @Override
        public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
            //业务逻辑...
            return false; // 默认返回false
        }
    }
    ```

    ```java
    //使用
    @Configuration
    public class JavaConfig {
        @Bean
        // 条件注解，添加的类型必须是 实现了 Condition 接口的类型
        // MyCondition的 matches 方法返回true 则注入，返回false 则不注入
        @Conditional(MyCondition.class)
        public StudentService studentService() {
            return new StudentService();
        }
    }
    ```

- **Spring5.0**:

  - @Indexed：在Spring Boot应用场景中，大量使用@ComponentScan扫描，导致Spring模式的注解解析时间耗时增大，因此，5.0时代引入@Indexed，为Spring模式注解添加索引。
    - 当我们在项目中使用了 @Indexed 之后，编译打包的时候会在项目中自动生成METAINT/spring.components文件。根据该文件进行扫描注入，可以提高效率。

#### SpringBoot自动装配原理

自动装配还是利用了SpringFactoriesLoader来加载META-INF/spring.factoires文件里所有配置的EnableAutoConfgruation，它会经过exclude和filter等操作，最终确定要装配的类。

**1.一切的开始都源于@SpringBootApplication，它是一个组合注解**

除了元注解之外，关注这三个注解：

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```

- @SpringBootConfiguration该注解的作用是用来指定扫描路径的，如果不指定特定的扫描路径的话，扫描的路径是当前修饰的类所在的包及其子包。
- @SpringBootConfiguration这个注解的本质其实是@Configuration注解。

**2.看来这个@EnableAutoConfiguration不简单**

```java
@Import(AutoConfigurationImportSelector.class)
```

它的内部主要是使用@import注解导入一个选择器。

**3.那么我们看看这个AutoConfigurationImportSelector类**

上文提到继承ImportSelector接口的类，需要重写 selectImports( )，那我们就看看这个方法

```java
@Override
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        if (!isEnabled(annotationMetadata)) {
            return NO_IMPORTS;
        }
        AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(annotationMetadata);
        return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
    }
```

该方法其实也没说啥，现在的重心就放在getAutoConfigurationEntry()中。

**4.getAutoConfigurationEntry()**

```java
protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
    if (!isEnabled(annotationMetadata)) {
        return EMPTY_ENTRY;
    }　　　　　
    AnnotationAttributes attributes = getAttributes(annotationMetadata);
  	//获取候选配置信息,加载的是当前项目的classpath目录下的所有的 spring.factories 文件中的 key 为  
  	//org.springframework.boot.autoconfigure.EnableAutoConfiguration 的信息。
  	//点进去通过"SpringFactoriesLoader"进行加载
 		List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
    // removeDuplicates方法的作用是 移除同名的
    configurations = removeDuplicates(configurations);
    // 获取我们配置的 exclude 信息
    // 比如：@SpringBootApplication(exclude = {RabbitAutoConfiguration.class}) ,显示的指定不要加载那个配置类
    Set<String> exclusions = getExclusions(annotationMetadata, attributes);
    checkExcludedClasses(configurations, exclusions);
    configurations.removeAll(exclusions);
    // filter的作用是 过滤掉咱们不需要使用的配置类。
    configurations = getConfigurationClassFilter().filter(configurations);
    fireAutoConfigurationImportEvents(configurations, exclusions);
    return new AutoConfigurationEntry(configurations, exclusions);
}
```

**5.前面几个都好理解，现在我们主要看看filter()，是怎么移除不需要的类**

![img](https://github.com/chou401/pic-md/raw/master/2597186-20220218230920130-226480162.png)

我们可以看到有具体的匹配方法 match。里面有个关键的属性是 autoConfigurationMetadata , 的本质是 加载的 META-INF/spring-autoconfigure-metadata.properties 的文件中的内容。

![img](https://github.com/chou401/pic-md/raw/master/2597186-20220218231102908-1017908901.png)

其实原理很简单，如果没有对应的实现类，就不进行加载。

#### 何时进行自动装配

- 处理@Configuration的核心还是ConfigurationClassPostProcessor，这个类实现了BeanFactoryPostProcessor,
- 因此当AbstractApplicationContext执行refresh方法里的invokeBeanFactoryPostProcessors(beanFactory)方法时会执行自动装配

#### run 方法

```java
@SpringBootApplication
public class SpringBootVipDemoApplication {
    public static void main(String[] args) {
        // 基于配置文件的方式
        ApplicationContext ac1 = new ClassPathXmlApplicationContext("");
        // 基于Java配置类的方式
        ApplicationContext ac2 = new AnnotationConfigApplicationContext(SpringBootVipDemoApplication.class);
        // run 方法的返回对象是 ConfigurableApplicationContext 对象,
        //ConfigurableApplicationContext就是ApplicationContext的一个子接口
        ConfigurableApplicationContext ac3 = SpringApplication.run(SpringBootVipDemoApplication.class, args);
    }
}
```

根据返回结果，我们猜测SpringBoot项目的启动其实就是Spring的初始化操作【IOC】。

![img](https://github.com/chou401/pic-md/raw/master/2597186-20220218232932423-556100027.png)



### 什么是 Starter

starter 是“一站式服务（one-stop）”的依赖 jar 包：

- 包含Spring以及相关技术（比如 Redis）的所有依赖
- 提供了自动配置的功能，开箱即用
- 提供了良好的依赖管理，避免了包遗漏、版本冲突等问题。

![image-20230819192249343](https://github.com/chou401/pic-md/raw/master/image-20230819192249343.png)

### Maven 中的可选依赖

可选依赖（optional）的作用：阻断“**依赖传递**”













### Spring 中的注解处理器

注解处理器：在编译阶段，生成指定注解的元数据。

- spring-boot-configuration-processor
- spring-boot-autoconfigure-processor





### 自定义装换器

1. 实现WebMvcConfigurer： 不会覆盖WebMvcAutoConfiguration的配置。
2. 实现WebMvcConfigurer+注解@EnableWebMvc：会覆盖WebMvcAutoConfiguration的配置。
3. 继承WebMvcConfigurationSupport：会覆盖WebMvcAutoConfiguration的配置。
4. 继承DelegatingWebMvcConfiguration：会覆盖WebMvcAutoConfiguration的配置。

#### 配置类

##### WebMvcConfigurationAdapter

- WebMvcConfigurerAdapter 是 WebMvcConfigurer 的实现类大部分为空方法，是 WebMvcConfigurer的子类实现，由于Java8中可以使用default关键字为接口添加默认方法，为在源代码中spring5.0之后就已经弃用本类，如果需要可以实现WebMvcConfigurer类。
- `WebMvcConfigurationAdapter`已经废弃，最好用`WebMvcConfigurer`代替。

##### WebMvcConfigurationSupport 

- WebMvcConfigurationSupport 是mvc的基本实现并包含了WebMvcConfigurer接口中的方法。

##### WebMvcAutoConfiguration

- WebMvcAutoConfiguration 是mvc的自动装在类并部分包含了WebMvcConfigurer接口中的方法。

- springboot会自动启用WebMvcAutoConfiguration类做自动加载；项目中的配置都是默认的，比如静态资源文件的访问。

- ```java
  @Configuration
  @ConditionalOnWebApplication(type = Type.SERVLET)
  @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
  @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
  @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
  @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class,
  		TaskExecutionAutoConfiguration.class, ValidationAutoConfiguration.class })
  public class WebMvcAutoConfiguration {
  	...
  }
  ```
  
  - @ConditionalOnMissingBean({WebMvcConfigurationSupport.class})，当Spring容器中不存在WebMvcConfigurationSupportbean，WebMvcAutoConfiguration才会注入。
  
  - 如果有配置文件继承了DelegatingWebMvcConfiguration，或者WebMvcConfigurationSupport，或者配置类注解了@EnableWebMvc，那么WebMvcAutoConfiguration 将不会被自动配置，而是使用WebMvcConfigurationSupport的配置。那么所有实现了WebMvcConfigurer的配置类有可能会全部失效。
  

##### WebMvcConfigurer

- 用途：跨域、拦截器、静态资源处理。

- 接口方法的作用：

  ```
  addInterceptors：拦截器
  addViewControllers：页面跳转
  addResourceHandlers：静态资源
  configureDefaultServletHandling：默认静态资源处理器
  configureViewResolvers：视图解析器
  configureContentNegotiation：配置内容裁决的一些参数
  addCorsMappings：跨域
  configureMessageConverters：信息转换器
  ```

- WebMvcConfigurer配置类其实是`Spring`内部的种配置方式，可以自定义一些Handler，Interceptor，ViewResolver，MessageConverter等等的东西对springmvc框架进行配置。

- ```java
  @Configuration
  public class MyWebMvcConfigurer implements WebMvcConfigurer {
      @Override
      public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
          //创建fastJson消息转换器
          FastJsonHttpMessageConverter converter = new FastJsonHttpMessageConverter();
  
          List<MediaType> supportedMediaTypes = new ArrayList<MediaType>();
          MediaType mediaTypeJson = MediaType.valueOf(MediaType.APPLICATION_JSON_UTF8_VALUE);
          supportedMediaTypes.add(mediaTypeJson);
          converter.setSupportedMediaTypes(supportedMediaTypes);
          //创建配置类
          FastJsonConfig config = new FastJsonConfig();
          config.getSerializeConfig().put(Json.class, new SwaggerJsonSerializer());
  
          //修改配置返回内容的过滤
          //WriteNullListAsEmpty  ：List字段如果为null,输出为[],而非null
          //WriteNullStringAsEmpty ： 字符类型字段如果为null,输出为"",而非null
          //DisableCircularReferenceDetect ：消除对同一对象循环引用的问题，默认为false（如果不配置有可能会进入死循环）
          //WriteNullBooleanAsFalse：Boolean字段如果为null,输出为false,而非null
          //WriteMapNullValue：是否输出值为null的字段,默认为false
          //WriteDateUseDateFormat：全局修改日期格式，不添加会导致@JsonFormat失效
          config.setSerializerFeatures(SerializerFeature.WriteMapNullValue
                  , SerializerFeature.WriteNonStringKeyAsString,
                  SerializerFeature.WriteNullListAsEmpty,
                  SerializerFeature.DisableCircularReferenceDetect,
                  SerializerFeature.WriteDateUseDateFormat,
                  SerializerFeature.WriteNullBooleanAsFalse);
          converter.setFastJsonConfig(config);
          //将fastjson添加到视图消息转换器列表内
          converters.add(0, converter);
          //WebMvcConfigurer失效
          converters.add(converter);
          
      }
  }
  ```


- 默认已经有多个消息转换器了。而 configureMessageConverters 方法中是一个 list 参数。直接向其中添加 HttpMessageConverter后，默认是排在最后的。就造成了你自定义的消息转换器不生效。其实是被其他转换器接管了。因此，想要让我们自定义的消息转换器生效只需要把它添加到list 的第一个就可以了。
  
- SerializerFeature属性

  ![image-20230203141903620](https://github.com/chou401/pic-md/raw/master/img/image-20230203141903620.png)

#### 注解

##### **@EnableWebMVC**

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
}

```

如果引用了`@EnableWebMVC`注解，就会往spring容器中注入了一个`DelegatingWebMvcConfiguration`来统一管理所有的配置类。

```java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
	private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
	...
}

```

**失效问题解决**

1. 在类上加注解解决失效问题

   ```java
   @EnableWebMvc
   @Configuration
   public class WebMvcConfig implements WebMvcConfigurer {
       
   }
   ```

   @EnableWebMvc表示完全自己控制mvc配置，也就是说所有配置自己重写，所有默认配置都没了！有时会导致很多请求进不来，或者参数转换出错之类的，因为spring mvc默认的转换器已经不生效了,包括全局配置的Jackson也会失效，所以在大多数情况下我们需要的是在其基础配置上添加自定义配置。

2. 注解+继承解决失效问题

   ```java
   @EnableWebMvc
   @Configuration
   public class WebMvcConfig extends WebMvcAutoConfiguration implements WebMvcConfigurer {
   
   }
   ```



@SpringBootApplication

- ```java
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Inherited
  @SpringBootConfiguration
  @EnableAutoConfiguration
  @ComponentScan(excludeFilters = {
  		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
  		@Filter(type = FilterType.CUSTOM,
  				classes = AutoConfigurationExcludeFilter.class) })
  public @interface SpringBootApplication {
  	@AliasFor(annotation = EnableAutoConfiguration.class)
  	Class<?>[] exclude() default {};
  	@AliasFor(annotation = EnableAutoConfiguration.class)
  	String[] excludeName() default {};
  	@AliasFor(annotation = ComponentScan.class, attribute = "basePackages")
  	String[] scanBasePackages() default {};
  	@AliasFor(annotation = ComponentScan.class, attribute = "basePackageClasses")
  	Class<?>[] scanBasePackageClasses() default {};
  }
   
  ```
  
	- 这里引用了`@EnableAutoConfiguration`注解
	

##### @EnableAutoConfiguration

- ```java
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Inherited
  @AutoConfigurationPackage
  @Import(AutoConfigurationImportSelector.class)
  public @interface EnableAutoConfiguration {
  	String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";
  	Class<?>[] exclude() default {};
  	String[] excludeName() default {};
  
  }
  ```


- 注解`@AutoConfigurationPackage`的主要作用就是：将主程序类所在包及所有子包下的组件到扫描到spring容器中。


- ```java
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Inherited
  @Import(AutoConfigurationPackages.Registrar.class)
  public @interface AutoConfigurationPackage {
  }
  ```


- `AutoConfigurationImportSelector`的作用可以参考：[springboot源码自动装配-AutoConfigurationImportSelector](https://blog.csdn.net/qq_39482039/article/details/120585957)

- ```java
  public class AutoConfigurationImportSelector implements DeferredImportSelector, BeanClassLoaderAware,
  		ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered {
  	...
  	@Override
  	public String[] selectImports(AnnotationMetadata annotationMetadata) {
  		// 是否启用自动配置
  		if (!isEnabled(annotationMetadata)) {
  			return NO_IMPORTS;
  		}
  		// 获取META-INF/spring-autoconfigure-metadata.properties文件的配置，返回AutoConfigurationMetadata类
  		AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader
  				.loadMetadata(this.beanClassLoader);
  		// 获取自动配置类，返回AutoConfigurationEntry
  		AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(
  				autoConfigurationMetadata, annotationMetadata);
  		// 返回要自动配置的类名
  		return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
  	}
  	...
  }
  ```


- 如果引用了`@EnableAutoConfiguration`注解，就会往spring容器中注入两个类。
	1. `AutoConfigurationPackages.Registrar`：扫包。
	2. `AutoConfigurationImportSelector`：等所有类全加载到spring容器之后扫描配置类。

### junit

spring boot 2，在进行单元测试的时候，**不支持.yml文件**，配件文件需要设置为properties。

pom 中增加testResources，其他操作按照正常创建 test 流程即可。

```xml
<build>
        <resources>
            <resource>
                <directory>${basedir}/config/${build.profile.id}/</directory>
            </resource>
            <resource>
                <directory>${basedir}/src/main/resources/</directory>
            </resource>
        </resources>
				<finalName>xxxx</finalName>
        <testResources>
            <testResource>
                <directory>${basedir}/config/${build.profile.id}/</directory>
            </testResource>
            <testResource>
                <directory>${basedir}/src/main/resources/</directory>
            </testResource>
        </testResources>
    </build>
```



## Mybatis

### Mybatis使用

ORM框架：Object Relational Mapping。用于实现面向对象编程语言里不同类型系统的数据之间的转换。

- 添加依赖

  - ```java
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.0.1</version>
    </dependency>
    ```

- 配置文件中配置mybatis的mapper文件位置。

  - ```java
    mybatis.mapper-locations=classpath:mapper/*.xml
    ```

- pom.xml 设置springboot在包中扫描xml文件。

- 启动类添加注解用于给出需要扫描的mapper文件路径@MapperScan("xxx.xxx.xxx.xxx")。

- 逆向工程依赖

  ```java
  <plugin>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-maven-plugin</artifactId>
      <version>1.4.1</version>
      <configuration>
          <!--允许移动生成的文件 -->
          <verbose>true</verbose>
          <!-- 是否覆盖 -->
          <overwrite>true</overwrite>
          <!-- 自动生成的配置文件路径。启动插件时，插件会根据这里配置的路径去找到generatorConfig.xml配置文件，
          根据配置文件里的配置，去自动生成Mapper接口（可以理解为Dao层）、实体类、Mapper.xml文件
          -->
          <configurationFile>src/main/resources/GeneratorConfig.xml</configurationFile>
      </configuration>
      <dependencies>
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>8.0.29</version>
          </dependency>
          <dependency>
              <groupId>org.mybatis.generator</groupId>
              <artifactId>mybatis-generator-core</artifactId>
              <version>1.4.1</version>
          </dependency>
  
      </dependencies>
      <executions>
          <execution>
              <id>Generate MyBatis Artifacts</id>
              <phase>package</phase>
              <goals>
                  <goal>generate</goal>
              </goals>
          </execution>
      </executions>
  </plugin>
  ```

  generatorConfig.xml

  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE generatorConfiguration
          PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
          "http://mybatis.org/dtd/mybatis-generator-config_1.0.dtd">
  <generatorConfiguration>
  
      <!-- 数据库驱动:选择你的本地硬盘上面的数据库驱动包 -->
      <!--    因为已经在pom.xml添加逆向工程插件时添加了驱动依赖，所以省略这一步-->
      <!--    <classPathEntry  location="C:\Users\lenovo\Desktop\Software_project\java\mysql-connector-java-8.0.28/mysql-connector-java-8.0.28.jar"/>-->
  
      <context id="DB2Tables" targetRuntime="MyBatis3">
          <!-- 实体类生成序列化属性-->
          <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
          <!-- 实体类重写HashCode()和equals()-->
          <plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>
          <!-- 实体类重写toString() -->
          <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>
  
          <commentGenerator>
              <!-- 是否去除自动生成的注释 -->
              <property name="suppressAllComments" value="true"/>
              <!-- 生成注释是否带时间戳-->
              <property name="suppressDate" value="true"/>
              <!-- 生成的Java文件的编码格式 -->
              <property name="javaFileEncoding" value="utf-8"/>
              <!-- 数据库注释支持 -->
              <property name="addRemarkComments" value="true"/>
              <!-- 时间格式设置 -->
              <property name="dateFormat" value="yyyy-MM-dd HH:mm:ss"/>
              <!-- 格式化java代码-->
              <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
              <!-- 格式化XML代码-->
              <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>
          </commentGenerator>
  
          <!-- 数据库连接驱动类,URL，用户名、密码 -->
          <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                          connectionURL="jdbc:mysql://119.45.27.47:3306/dms_usp?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8"
                          userId="FordDms"
                          password="FordDms.1234">
              <property name="nullCatalogMeansCurrent" value="true"/>
          </jdbcConnection>
  
          <!-- java类型处理器：处理DB中的类型到Java中的类型 -->
          <javaTypeResolver type="org.mybatis.generator.internal.types.JavaTypeResolverDefaultImpl">
              <!-- 是否有效识别DB中的BigDecimal类型 -->
              <property name="forceBigDecimals" value="true"/>
          </javaTypeResolver>
  
          <!-- 生成Domain模型：包名(targetPackage)、位置(targetProject) -->
          <javaModelGenerator targetPackage="com.yonyou.dms.web.entity" targetProject="E:\workspace\Parent\Login\src\main\java">
              <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
              <property name="enableSubPackages" value="true"/>
              <!-- 设置是否在getter方法中，对String类型字段调用trim()方法-->
              <property name="trimStrings" value="true"/>
          </javaModelGenerator>
  
          <!-- 生成xml映射文件：包名(targetPackage)、位置(targetProject) -->
          <sqlMapGenerator targetPackage="mapper" targetProject="E:\workspace\Parent\Login\src\main\resources">
              <property name="enableSubPackages" value="true"/>
          </sqlMapGenerator>
  
          <!-- 生成DAO接口：包名(targetPackage)、位置(targetProject) -->
          <javaClientGenerator type="XMLMAPPER" targetPackage="com.yonyou.dms.web.dao"
                               targetProject="E:\workspace\Parent\Login\src\main\java">
              <property name="enableSubPackages" value="true"/>
          </javaClientGenerator>
  
          <table tableName="tc_menu_action" domainObjectName="MenuAction">
              <!--            若数据库中某个属性类型为text或类似类型，可能会发生无法生成这个属性，此时可以在这里指定转换类型为VARCHAR-->
              <!--            <columnOverride column="teacher_name" jdbcType="VARCHAR" />-->
  <!--        　　table标签下的设置属性useActualColumnNames用于指定生成实体类时是否使用实际的列名作为实体类的属性名，取值true或false。-->
  <!--        　　true：MyBatis Generator会使用数据库中实际的字段名字作为生成的实体类的属性名。-->
  <!--        　　false：这是默认值。如果设置为false,则MyBatis Generator会将数据库中实际的字段名字转换为Camel Case风格作为生成的实体类的属性名。-->
              <property name="useActualColumnNames" value="false" />
          </table>
      </context>
  </generatorConfiguration>
  ```

  

先执行二级缓存，再执行一级缓存。

```java
public <E> List<E> query(MappedStatement ms, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler) throws SQLException {
    BoundSql boundSql = ms.getBoundSql(parameterObject);
    CacheKey key = this.createCacheKey(ms, parameterObject, rowBounds, boundSql);
    return this.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
}
```

mybatis调用query时如何使用缓存，createCacheKey生成key。

```java
public CacheKey createCacheKey(MappedStatement ms, Object parameterObject, RowBounds rowBounds, BoundSql boundSql) {
    if (this.closed) {
        throw new ExecutorException("Executor was closed.");
    } else {
        CacheKey cacheKey = new CacheKey();
        cacheKey.update(ms.getId());
        cacheKey.update(rowBounds.getOffset());
        cacheKey.update(rowBounds.getLimit());
        cacheKey.update(boundSql.getSql());
        List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
        TypeHandlerRegistry typeHandlerRegistry = ms.getConfiguration().getTypeHandlerRegistry();
        Iterator var8 = parameterMappings.iterator();

        while(var8.hasNext()) {
            ParameterMapping parameterMapping = (ParameterMapping)var8.next();
            if (parameterMapping.getMode() != ParameterMode.OUT) {
                String propertyName = parameterMapping.getProperty();
                Object value;
                if (boundSql.hasAdditionalParameter(propertyName)) {
                    value = boundSql.getAdditionalParameter(propertyName);
                } else if (parameterObject == null) {
                    value = null;
                } else if (typeHandlerRegistry.hasTypeHandler(parameterObject.getClass())) {
                    value = parameterObject;
                } else {
                    MetaObject metaObject = this.configuration.newMetaObject(parameterObject);
                    value = metaObject.getValue(propertyName);
                }

                cacheKey.update(value);
            }
        }

        if (this.configuration.getEnvironment() != null) {
            cacheKey.update(this.configuration.getEnvironment().getId());
        }

        return cacheKey;
    }
}
```

执行查询方法。

```java
public <E> List<E> query(MappedStatement ms, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql) throws SQLException {
    // 查询时先从二级缓存中获取数据
    Cache cache = ms.getCache();
    if (cache != null) {
        this.flushCacheIfRequired(ms);
        if (ms.isUseCache() && resultHandler == null) {
            this.ensureNoOutParams(ms, boundSql);
            List<E> list = (List)this.tcm.getObject(cache, key);
            if (list == null) {
                list = this.delegate.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
                this.tcm.putObject(cache, key, list);
            }

            return list;
        }
    }

    return this.delegate.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
}
```

```java
public <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql) throws SQLException {
    ErrorContext.instance().resource(ms.getResource()).activity("executing a query").object(ms.getId());
    if (this.closed) {
        throw new ExecutorException("Executor was closed.");
    } else {
        if (this.queryStack == 0 && ms.isFlushCacheRequired()) {
            this.clearLocalCache();
        }

        List list;
        try {
            ++this.queryStack;
            // 根据key获取缓存中是否存在数据，存在数据从缓存中查询数据，否则从数据库查询数据
            list = resultHandler == null ? (List)this.localCache.getObject(key) : null;
            if (list != null) {
                this.handleLocallyCachedOutputParameters(ms, key, parameter, boundSql);
            } else {
                list = this.queryFromDatabase(ms, parameter, rowBounds, resultHandler, key, boundSql);
            }
        } finally {
            --this.queryStack;
        }

        if (this.queryStack == 0) {
            Iterator var8 = this.deferredLoads.iterator();

            while(var8.hasNext()) {
                DeferredLoad deferredLoad = (DeferredLoad)var8.next();
                deferredLoad.load();
            }

            this.deferredLoads.clear();
            if (this.configuration.getLocalCacheScope() == LocalCacheScope.STATEMENT) {
                this.clearLocalCache();
            }
        }

        return list;
    }
}
```

```java
private <E> List<E> queryFromDatabase(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql) throws SQLException {
    this.localCache.putObject(key, ExecutionPlaceholder.EXECUTION_PLACEHOLDER);

    List list;
    try {
        list = this.doQuery(ms, parameter, rowBounds, resultHandler, boundSql);
    } finally {
        this.localCache.removeObject(key);
    }

    this.localCache.putObject(key, list);
    if (ms.getStatementType() == StatementType.CALLABLE) {
        this.localOutputParameterCache.putObject(key, parameter);
    }

    return list;
}
```

```java
public RoutingStatementHandler(Executor executor, MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) {
    switch (ms.getStatementType()) {
            // 普通类型
        case STATEMENT:
            this.delegate = new SimpleStatementHandler(executor, ms, parameter, rowBounds, resultHandler, boundSql);
            break;
            // 预编译
        case PREPARED:
            this.delegate = new PreparedStatementHandler(executor, ms, parameter, rowBounds, resultHandler, boundSql);
            break;
            // 存储过程
        case CALLABLE:
            this.delegate = new CallableStatementHandler(executor, ms, parameter, rowBounds, resultHandler, boundSql);
            break;
        default:
            throw new ExecutorException("Unknown statement type: " + ms.getStatementType());
    }

}
```



```java
public ResultSetWrapper(ResultSet rs, Configuration configuration) throws SQLException {
    this.typeHandlerRegistry = configuration.getTypeHandlerRegistry();
    this.resultSet = rs;
    ResultSetMetaData metaData = rs.getMetaData();
    // 获取表行数
    int columnCount = metaData.getColumnCount();

    for(int i = 1; i <= columnCount; ++i) {
        // columnNames 字段名称
        this.columnNames.add(configuration.isUseColumnLabel() ? metaData.getColumnLabel(i) : metaData.getColumnName(i));
        // jdbcTypes 数据库字段类型
        this.jdbcTypes.add(JdbcType.forCode(metaData.getColumnType(i)));
        // classNames java 字段类型
        this.classNames.add(metaData.getColumnClassName(i));
    }
}
```



**spring整合mybatis 实现**

```java
@Autowired
private UserMapper userMapper; // Mybatis生成的UserMapper代理对象 --> Bean对象
```



### SqlSessionFactoryBean的底层原理

SqlSessionFactoryBean是MyBatis框架中的一个重要组件，它的主要作用是创建SqlSessionFactory对象，SqlSessionFactory是MyBatis框架中的核心对象，它负责管理MyBatis的所有配置信息，并且提供了创建SqlSession对象的方法。SqlSessionFactoryBean的底层原理是通过读取MyBatis的配置文件，解析其中的配置信息，然后根据配置信息创建SqlSessionFactory对象。在创建SqlSessionFactory对象的过程中，SqlSessionFactoryBean会使用MyBatis框架中的多个组件，包括Configuration、MapperRegistry、TypeHandlerRegistry等，这些组件都是MyBatis框架中的核心组件，它们负责管理MyBatis的各种配置信息，并且提供了各种功能的实现。SqlSessionFactoryBean的底层原理比较复杂，需要深入了解MyBatis框架的各个组件之间的关系和作用，才能更好地理解它的实现原理。

```java
@Bean
public SqlSessionFactory sqlSessionFactory() throws IOException{
    InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
    return new SqlSessionFactoryBuilder().build(inputStream);
}

@Bean
public UserMapper userMapper(SqlSessionFactory sqlSessionFactory) {
    // Mybatis 代理对象
    sqlSessionFactory.getConfiguration().addMapper(UserMapper.class);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    return sqlSession.getMapper(UserMapper.class);
}
```



### FactoryBean的作用和底层工作原理

FactoryBean是Spring框架中的一个接口，它的作用是用于创建和管理Bean对象。FactoryBean可以将复杂的Bean对象的创建过程封装起来，使得应用程序只需要通过FactoryBean获取Bean对象即可，而不需要关心Bean对象的创建过程。FactoryBean的底层工作原理是通过实现getObject()方法来创建Bean对象，同时还可以通过实现其他方法来控制Bean对象的生命周期和行为。

```java
@Component
public class TestFactoryBean implements FactoryBean {
    // 实现FactoryBean接口会生成两个Bean对象 1、TestFactoryBean 2、getObject
    // 先生成TestFactoryBean 之后再生成 getObject
    
    @Autowired
    private SqlSessionFactory sqlSessionFactory;
    
    @Override
    public Object getObject() throws Exception {
        sqlSessionFactory.getConfiguration().addMapper(UserMapper.class);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession.getMapper(UserMapper.class);
    }

    @Override
    public Class<?> getObjectType() {
        return UserMapper.class;
    }
}
```

FactoryBean复用

```java
AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext();
applicationContext.register(DegreeApplication.class);

// 与@Component 一样的作用
AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
beanDefinition.setBeanClass(TestFactoryBean.class);
beanDefinition.getConstructorArgumentValues().addGenericArgumentValue(UserMapper.class);
applicationContext.registerBeanDefinition("userMapper", beanDefinition);
```

```java
@Component
public class TestFactoryBean implements FactoryBean {

  @Autowired
  private SqlSessionFactory sqlSessionFactory;

  private final Class mapperClass;

  public TestFactoryBean(Class mapperClass) {
    this.mapperClass = mapperClass;
  }

  @Override
  public Object getObject() throws Exception {
    sqlSessionFactory.getConfiguration().addMapper(mapperClass);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    return sqlSession.getMapper(mapperClass);
  }

  @Override
  public Class<?> getObjectType() {
    return User.class;
  }
}
```



### ImportBeanDefinitionRegistrar底层原理

1. ImportBeanDefinitionRegistrar实现类重写registerBeanDefinitions方法。
2. 在registerBeanDefinitions方法中，通过BeanDefinitionRegistry接口注册bean定义。

```java
public class TestImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        ArrayList<Class> list = new ArrayList<>();
        list.add(UserMapper.class);

        for (Class mapperClass : list) {
            AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
            beanDefinition.setBeanClass(TestFactoryBean.class);
            beanDefinition.getConstructorArgumentValues().addGenericArgumentValue(mapperClass);
            registry.registerBeanDefinition(mapperClass.getName(), beanDefinition);
        }
    }
}
```

```java
// 使用@Import 导入配置的类
@Import(TestImportBeanDefinitionRegistrar.class)
public class DegreeApplication {
    
}
```



### MapperScannerConfigurer底层源码分析

 * MapperScannerConfigurer底层源码分析。
 * MapperScannerConfigurer是MyBatis提供的一个BeanFactoryPostProcessor，用于扫描指定包下的Mapper接口，并将其注册到Spring容器中。
 * 在MyBatis中，Mapper接口是通过MapperProxyFactory来创建代理对象的，而MapperScannerConfigurer的作用就是将Mapper接口的代理对象注册到Spring容器中，以便在需要使用Mapper接口时，可以直接从Spring容器中获取代理对象。
 * MapperScannerConfigurer的实现原理是通过Spring的ClassPathBeanDefinitionScanner来扫描指定包下的所有类，然后判断是否是Mapper接口，如果是，则将其注册到Spring容器中。
 * 在注册Mapper接口时，MapperScannerConfigurer会为每个Mapper接口创建一个MapperFactoryBean，MapperFactoryBean是一个FactoryBean，用于创建Mapper接口的代理对象。
 * MapperFactoryBean的实现原理是通过MapperProxyFactory来创建Mapper接口的代理对象，然后将其返回给Spring容器。
 * 总结一下，MapperScannerConfigurer的作用是将Mapper接口的代理对象注册到Spring容器中，以便在需要使用Mapper接口时，可以直接从Spring容器中获取代理对象。MapperScannerConfigurer的实现原理是通过Spring的ClassPathBeanDefinitionScanner来扫描指定包下的所有类，然后判断是否是Mapper接口，如果是，则将其注册到Spring容器中，并为每个Mapper接口创建一个MapperFactoryBean，用于创建Mapper接口的代理对象。

```java
public class TestMapperScanner extends ClassPathBeanDefinitionScanner {

    @Override
    protected boolean isCandidateComponent(MetadataReader metadataReader) throws IOException {
        // 是否 @component 注解
        return true;
    }

    @Override
    protected boolean isCandidateComponent(AnnotatedBeanDefinition beanDefinition) {
        // 是否 接口类
        return beanDefinition.getMetadata().isInterface();
    }

    public TestMapperScanner(BeanDefinitionRegistry registry) {
        super(registry);
    }

    @Override
    protected Set<BeanDefinitionHolder> doScan(String... basePackages) {
        // 重写 doScan 方法 
        Set<BeanDefinitionHolder> beanDefinitionHolders = super.doScan(basePackages);
        for (BeanDefinitionHolder beanDefinitionHolder : beanDefinitionHolders) {
            GenericBeanDefinition genericBeanDefinition = (GenericBeanDefinition) beanDefinitionHolder.getBeanDefinition();
            genericBeanDefinition.getConstructorArgumentValues().addGenericArgumentValue(Objects.requireNonNull(genericBeanDefinition.getBeanClassName()));
            genericBeanDefinition.setBeanClass(TestFactoryBean.class);
        }

        return beanDefinitionHolders;
    }
}
```



使用MapperScanner自定义扫描mapper。

```java
public class TestImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        TestMapperScanner testMapperScanner = new TestMapperScanner(registry);
        testMapperScanner.scan("com.second.degree.java.mybatis.mapper");
    }
}
```



### @MapperScan注解的底层源码分析

@MapperScan注解是MyBatis框架提供的一个注解，用于扫描Mapper接口并将其注册到Spring容器中。其底层源码分析可以从以下几个方面入手：

1. @MapperScan注解的定义：可以查看该注解的定义，了解其属性和作用。
2. 扫描器的实现：@MapperScan注解的底层实现是通过扫描器实现的，可以查看扫描器的源码，了解其扫描的规则和实现方式。
3. 注册Mapper接口：扫描器扫描到Mapper接口后，需要将其注册到Spring容器中，可以查看注册的源码，了解其实现方式和注册的规则。
4. 与Spring集成：@MapperScan注解是与Spring集成的，可以查看其与Spring集成的源码，了解其实现方式和与Spring的交互方式。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Import(TestImportBeanDefinitionRegistrar.class)
public @interface TestMapperScan {
    String value();
}
```



```java
@TestMapperScan("com.second.degree.java.mybatis.mapper")
public class DegreeApplication {}
```



```java
public class TestImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {

        Map<String, Object> annotationAttributes = importingClassMetadata.getAnnotationAttributes(TestMapperScan.class.getName());
        String path = (String) annotationAttributes.get("value");

        TestMapperScanner testMapperScanner = new TestMapperScanner(registry);
        testMapperScanner.scan(path);
    }
}
```



### Spring整合Mybatis的底层源码分析

mybaits 2.0.6 以下版本直接在registerBeanDefinitions 中实现 Scanner 扫描器，使用注解@MapperScan。

```java
@MapperScan
public class DegreeApplication {}
```



```java
void registerBeanDefinitions(AnnotationAttributes annoAttrs, BeanDefinitionRegistry registry) {
        ClassPathMapperScanner scanner = new ClassPathMapperScanner(registry);
	...
}
```



mybaits 2.0.6 以上版本 是通过另一个Bean MapperScannerConfigurer 实现Scanner 扫描器，可以不使用@MapperScan，通过注册MapperScannerConfigurer 实现扫描。

```java
void registerBeanDefinitions(AnnotationMetadata annoMeta, AnnotationAttributes annoAttrs, BeanDefinitionRegistry registry, String beanName) {
        BeanDefinitionBuilder builder = BeanDefinitionBuilder.genericBeanDefinition(MapperScannerConfigurer.class);
	...
}
```

```java
public class DegreeApplication {

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer() {
        MapperScannerConfigurer configurer = new MapperScannerConfigurer();
        configurer.setBasePackage("com.second.degree.java.mybatis.mapper");
        return configurer;
    }
}
```

### SpringBoot整合Mybatis的底层源码分析

1. 使用@MapperScan，会扫到很多无用的类，效率会差点。mybatis-spring包。

2. 自动配置类MybatisAutoConfiguration。mybatis-spring-boot-autoconfigure包。

   ```java
   public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
       if (!AutoConfigurationPackages.has(this.beanFactory)) {
           MybatisAutoConfiguration.logger.debug("Could not determine auto-configuration package, automatic mapper scanning disabled.");
       } else {
           MybatisAutoConfiguration.logger.debug("Searching for mappers annotated with @Mapper");
           // 扫描的包路径 为 springboot 启动 run 方法的路径
           List<String> packages = AutoConfigurationPackages.get(this.beanFactory);
           if (MybatisAutoConfiguration.logger.isDebugEnabled()) {
               packages.forEach((pkg) -> {
                   MybatisAutoConfiguration.logger.debug("Using auto-configuration base package '{}'", pkg);
               });
           }
   
           BeanDefinitionBuilder builder = BeanDefinitionBuilder.genericBeanDefinition(MapperScannerConfigurer.class);
           builder.addPropertyValue("processPropertyPlaceHolders", true);
           // 只识别@Mapper 注解定义的接口类
           builder.addPropertyValue("annotationClass", Mapper.class);
           builder.addPropertyValue("basePackage", StringUtils.collectionToCommaDelimitedString(packages));
           BeanWrapper beanWrapper = new BeanWrapperImpl(MapperScannerConfigurer.class);
           Set<String> propertyNames = (Set)Stream.of(beanWrapper.getPropertyDescriptors()).map(FeatureDescriptor::getName).collect(Collectors.toSet());
           if (propertyNames.contains("lazyInitialization")) {
               builder.addPropertyValue("lazyInitialization", "${mybatis.lazy-initialization:false}");
           }
   
           if (propertyNames.contains("defaultScope")) {
               builder.addPropertyValue("defaultScope", "${mybatis.mapper-default-scope:}");
           }
   
           boolean injectSqlSession = (Boolean)this.environment.getProperty("mybatis.inject-sql-session-on-mapper-scan", Boolean.class, Boolean.TRUE);
           if (injectSqlSession && this.beanFactory instanceof ListableBeanFactory) {
               ListableBeanFactory listableBeanFactory = (ListableBeanFactory)this.beanFactory;
               Optional<String> sqlSessionTemplateBeanName = Optional.ofNullable(this.getBeanNameForType(SqlSessionTemplate.class, listableBeanFactory));
               Optional<String> sqlSessionFactoryBeanName = Optional.ofNullable(this.getBeanNameForType(SqlSessionFactory.class, listableBeanFactory));
               if (!sqlSessionTemplateBeanName.isPresent() && sqlSessionFactoryBeanName.isPresent()) {
                   builder.addPropertyValue("sqlSessionFactoryBeanName", sqlSessionFactoryBeanName.get());
               } else {
                   builder.addPropertyValue("sqlSessionTemplateBeanName", sqlSessionTemplateBeanName.orElse("sqlSessionTemplate"));
               }
           }
   
           builder.setRole(2);
           registry.registerBeanDefinition(MapperScannerConfigurer.class.getName(), builder.getBeanDefinition());
       }
   }
   ```

## Mysql

### Mysql InnoDB

MYSQL InnoDB二级索引存储主键值而不是存储行指针的优点与缺点。

> 优点：
>
> - 减少了出现行移动或者数据页分裂时二级索引的维护工作（当数据需要更新的时候，二级索引不需要修改，只需要修改聚簇索引，一个表只能有一个聚簇索引，其他的都是二级索引，这样只需要修改聚簇索引就可以了，不需要重新构建二级索引）
>
> 缺点：
>
> - 二级索引体积可能会变大，因为二级索引中存储了主键的信息
> - 二级索引的访问需要两次索引查找。第一次通过查找 *二级索引* 找二级索引中叶子节点存储的 *主键的值*；第二次通过这个主键的值去
>    ***聚簇索引*** 中查找对应的行

#### InnoDB 简介

InnoDB是一个将表中的数据存储到磁盘上的存储引擎。而真正处理数据的过程是发生在内存中的，所以需要把磁盘中的数据加载到内存中，如果是处理写入或修改请求的话，还需要把内存中的内容刷新到磁盘上。而我们知道读写磁盘的速度非常慢，和内存读写差了几个数量级。所以当我们想从表中获取某些记录时，InnoDB存储引擎需要一条一条的把记录从磁盘上读出来么？想要了解这个问题，我们首先需要了解InnoDB的存储结构是怎样的。

![image-20230703165807448](https://github.com/chou401/pic-md/raw/master/image-20230703165807448.png)

InnoDB采取的方式是：将数据划分为若干个页，以页作为磁盘和内存之间交互的基本单位innodb_page_size选项指定了MySQL实例的所有InnoDB表空间的页面大小。这个值是在创建实例时设置的，之后保持不变。有效值为64KB，32KB，16KB(默认值 )，8kB和4kB。也就是在一般情况下，一次最少从磁盘中读取16KB的内容到内存中，一次最少把内存中的16KB内容刷新到磁盘中。

#### InnoDB 的行格式

我们平时是以记录为单位来向表中插入数据的，这些记录在磁盘上的存放方式也被称为行格式或者记录格式。一行记录可以以不同的格式存在InnoDB中，行格式分别是compact、redundant、dynamic和compressed行格式。可以在创建或修改的语句中指定行格式：

> -- 创建数据表时,显示指定行格式CREATE TABLE 表名 (列的信息) ROW_FORMAT=行格式名称;
>
> -- 创建数据表时,修改行格式ALTER TABLE 表名 ROW_FORMAT=行格式名称;
>
> -- 查看数据表的行格式show table status like '';

mysql5.0之前默认的行格式是redundant，mysql5.0之后的默认行格式为compact ， 5.7之后的默认行格式为dynamic。

##### compact 格式

![img](https://github.com/chou401/pic-md/raw/master/859ab55da7e84b9da3ec34cd4ca9d611.png)

###### 变长字段长度列表

我们知道 MySQL 支持一些变长的数据类型，比如 VARCHAR(M) 、 VARBINARY(M) 、各种 TEXT 类型，各种 BLOB 类型，我们也可以把拥有这些数据类型的列称为 变长字段 ，变长字段中存储多少字节的数据是不固定的，所以我们在存储真实数据的时候需要顺便把这些数据占用的字节数也存起来，这样才不至于把 MySQL 服务器搞懵，所以这些变长字段占用的存储空间分为两部分：

1. 真正的数据内容
2. 占用的字节数

在 Compact 行格式中，把所有变长字段的真实数据占用的字节长度都存放在记录的开头部位，从而形成一个变长字段长度列表，各变长字段数据占用的字节数按照列的顺序逆序存放。

举个例子：

> 一个表中有c1、c2、c3三列数据为varchar，其中有一行数据存储了(“1234”,“123”,“1”)，它们分别的字符长度就为04、03、01，若其使用ascii字符集存储，则每个的字节大小为，04、03、01（ascii用一字节表示一个字符，utf-8为3字节），则这一行在”变长字段长度列表“中存储的则为”01 03 04“（实际存储为二进制且没有空格）
>

由于上面的字符串都比较短，也就是说内容占用的字节数比较小，用1个字节就可以表示，但是如果变长列的内容占用的字节数比较多，可能就需要用2个字节来表示。具体用1个还是2个字节来表示真实数据占用的字节数， InnoDB 有它的一套规则，首先我们声明一下 W 、 M 和 L 的意思：

> 假设某个字符集中表示一个字符最多需要使用的字节数为 W ，比方说 utf8 字符集中的 W 就是 1-3 ， ascii 字符集中的 W 就是1 。
> 对于 VARCHAR(M) 来说，表示此列最多能储存 M 个字符，所以这个类型能表示的字符串最多占用的字节数就是 M×W 。（比如：对于一个字符串”aaa“使用ascii表示则占用1*3个字节，而对于utf-8则为3*3个字节）
> 假设某字符串实际占用的字节数是 L 。

基于以上的声明，则使用1字节还是2 字节来表示变长字段长度的规则为： 

- 如果一个字段最长可以储存的字节数小于等于255 B，即`W*M <= 255`: 使用一个字节表示
- 如果W*M > 255 B，则分为两种情况：
  - 若L <= 127 B 则用1字节表示
  - 若L > 127 B 则用2字节表示

此外，innoDB使用 字节的第一位作为标志位，如果第一位为0，则此字节就是一个单独的字段长度。如果为1，则该字节为半个字段长度。

对于一些占用字节数非常多的字段，比方说某个字段长度大于了16KB，那么如果该记录在单个页面中无法存储时，InnoDB会把一部分数据存放到所谓的溢出页中，在变长字段长度列表处只存储留在本页面中的长度，所以使用两个字节也可以存放下来。

另外需要注意的一点是，变长字段长度列表中只存储值为 非NULL 的列内容占用的长度，值为 NULL 的列的长度是不储存的 。

字符集utf-8，英文字符占用1个字节，中文字符3字节，对于char类型来说，若使用utf-8字符集，则char也属于 可变长字段

###### NULL值列表

我们知道表中的某些列可能存储 NULL 值，如果把这些 NULL 值都放到记录的真实数据中存储会很占地方，所以 Compact 行格式把这些值为 NULL 的列统一管理起来，存储到 NULL 值列表中，它的处理过程是这样的：

1. 首先统计表中允许存储 NULL 的列有哪些。我们前边说过，主键列、被 NOT NULL 修饰的列都是不可以存储 NULL 值的，所以在统计的时候不会把这些列算进去。

2. 如果表中没有允许存储 NULL 的列，则 NULL值列表不存在。若允许，则将每个允许存储 NULL 的列对应一个二进制位，二进制位按照列的顺序逆序排列：二进制位的值为 1 时，代表该列的值为 NULL 。二进制位的值为 0 时，代表该列的值不为 NULL 。
3. MySQL 规定 NULL值列表 必须用整数个字节的位表示，如果使用的二进制位个数不是整数个字节，则在字节的高位补 0 。即若一个表有9个值允许为null，则这个记录null值列表的部分需要用 2 字节表示。

**举个例子**： 若有一张表，有c1 c2 c3 c4四个字段，其中c2 被NOT NULL修饰，则其NULL值列表 表示如下：

![img](https://github.com/chou401/pic-md/raw/master/4f4a765331f84efda30d2ae165f45707.png)



###### 记录头信息

记录头信息部分如下图所示：

![img](https://github.com/chou401/pic-md/raw/master/d35c888d876641768d782acb0e6668d3.png)

![img](https://github.com/chou401/pic-md/raw/master/9fac7f2392674842aba39dc969dfa126.png)

![img](https://github.com/chou401/pic-md/raw/master/aef632ad1be7455ab7e23909f68255f1.png)





我们使用如下的sql语句插入几行数据：

```mysql
INSERT INTO page_demo 
VALUES
(1, 100, 'aaaa'), 
(2, 200, 'bbbb'), 
(3, 300, 'cccc'),
(4, 400, 'dddd');
```

则它们这几条数据记录在 页 的 User Records 部分为：

![img](https://github.com/chou401/pic-md/raw/master/ae0a8a7826e4429683f2c12de76760a1.png)

**delete_mask**

这个属性标记着当前记录是否被删除，占用1个二进制位，值为 0 的时候代表记录并没有被删除，为 1 的时候代表记录被删除掉了。

被删除的记录还在页中。这些被删除的记录之所以不立即从磁盘上移除，是因为移除它们之后把其他的记录在磁盘上重新排列需要性能消耗，所以只是打一个删除标记而已，所有被删除掉的记录都会组成一个所谓的 **垃圾链表** ，在这个链表中的记录占用的空间称之为所谓的 **可重用空间** ，之后如果有新记录插入到表中的话，可能把这些被删除的记录占用的存储空间覆盖掉。

将这个delete_mask位设置为1和将被删除的记录加入到垃圾链表中其实是两个阶段。

**min_rec_mask**

B+树的每层非叶子节点中的最小记录都会添加该标记。上方插入的四条记录的 min_rec_mask 值都是 0 ，意味着它们都不是 B+ 树的非叶子节点中的最小记录。

**n_owned**

当前组的最大记录，记录当前组有几个元素的字段。

**heap_no**

在数据页的User Records中插入的记录是一条一条紧凑的排列的，这种紧凑排列的结构又被称为**堆**。为了便于管理这个堆，把记录在堆中的相对位置给定一个编号——heap_no。所以heap_no这个属性表示当前记录在本页中的位置。

在例子中，插入的4条记录在本页中的位置分别是： 2 、3 、4 、5。为什么没有0和1呢？

是因为每个页里面加了两个记录，这两个记录并不是我们自己插入的，所以有时候也称为**伪记录**或者**虚拟记录**。这两个伪记录一个代表**最小记录** Infimum，一个代表**最大记录 **Supremum，对应的heap_no分别为0和1。

记录可以比较大小，对于一条完整的记录来说，比较记录的大小就是比较主键的大小。

不管我们向页中插入了多少记录，InnoDB 规定任何用户记录都要比最小记录大，比最大记录小。这两条记录的构造，都是由5字节大小的记录头信息和8字节大小的固定部分组成的，如图：

![image-20230703174800471](https://github.com/chou401/pic-md/raw/master/image-20230703174800471.png)



最大最小记录不是自己定义的记录，所以它们并不存放在页的User Records 部分，而是被单独放在一个称为Infimum + Supremum 的部分，如图所示：

![image-20230703174836315](https://github.com/chou401/pic-md/raw/master/image-20230703174836315.png)

从图中可以看出来，最小记录和最大记录的heap_no 值分别是0 和1 ，也就是说它们的位置也在Uesr Records前面。

**record_type**

这个属性表示当前记录的类型，一共有4种类型的记录， 0 表示普通用户记录， 1 表示B+树非叶节点记录， 2 表示最小记录， 3 表示最大记录。

**next_record**

这个属性非常重要！！它表示从当前记录的真实数据到下一条记录的真实数据的地址偏移量，可以理解为指向下一条记录地址的指针。值为正数说明下一条记录在当前记录后面，为负数说明下一条记录在当前记录的前面。比方说第1条记录的next_record 值为32 ，意味着从第1条记录的真实数据的地址处向后找32 个字节便是下一条记录的真实数据。这里的下一条记录指得并不是按照我们插入顺序的下一条记录，而是按照主键值由小到大的顺序的下一条记录。


![image-20230703175113161](https://github.com/chou401/pic-md/raw/master/image-20230703175113161.png)

从图中可以看出来，我们的记录按照主键从小到大的顺序形成了一个单链表。最大记录的next_record 的值为0 ，这也就是说最大记录是没有下一条记录了，它是这个单链表中的最后一个节点。

如果从中删除掉一条记录，这个链表也是会跟着变化的，比如我们把第2条记录删掉：

```mysql
DELETE FROM page_demo WHERE c1 = 2;
```

删掉第2条记录后的示意图就是：

![image-20230703175303142](https://github.com/chou401/pic-md/raw/master/image-20230703175303142.png)

从图中可以看出来，删除第2条记录前后主要发生的变化：

- 被删记录没有从存储空间中移除，而是把该记录的delete_mask 设置为1 ，next_record 变为0；
- 被删记录的前一条记录的next_record 指向后一条记录：第1条记录的next_record 指向了第3条记录；
- 最大记录的n_owned 值减1。

所以，不论我们怎么对页中的记录做增删改操作，InnoDB始终会维护一条记录的单链表，链表中的各个节点是按照主键值由小到大的顺序连接起来的。并规定 Infimum记录（也就是最小记录） 的下一条记录就是本页中主键值最小的用户记录，而本页中主键值最大的用户记录的下一条记录就是 Supremum记录（也就是最大记录）。


而当我们再次插入第二条记录的时候 不会申请新的空间，而是直接连接被删的记录的next_record。



###### 默认隐藏列信息

MySQL 会为每个记录默认的添加一些列（也称为 隐藏列 ）

![img](https://github.com/chou401/pic-md/raw/master/e3ed90eaaf7747c49acfc2c59225a1d2.png)

实际上这几个列的真正名称其实是：DB_ROW_ID、DB_TRX_ID、DB_ROLL_PTR，我们为了美观才写成了row_id、transaction_id和roll_pointer。

row_id是可选的，表中没有主键的，则选取一个 Unique 键作为主键。如果表中连 Unique 键都没有定义的话，则 InnoDB 会为表默认添加一个名为row_id 的隐藏列作为主键。

roll_pointer 是一个指向记录对应的 undo日志 的一个指针。

![img](https://github.com/chou401/pic-md/raw/master/05aa65d74e8e4f1481eb7409fca14f96.png)

###### 行溢出的数据

我们知道对于 VARCHAR(M) 类型的列最多可以占用 65535 个字节。其中的 M 代表该类型最多存储的字符数量，如果我们使用 ascii 字符集的话，一个字符就代表一个字节。但是实际上，创建一张表并设置一个字段为`VARCHAR(65535)`则会报错。

```mysql
CREATE TABLE varchar_size_demo(
    c VARCHAR(65535)
) CHARSET=ascii ROW_FORMAT=Compact;

ERROR 1118 (42000): Row size too large. The maximum row size for the used table type, not
counting BLOBs, is 65535. This includes storage overhead, check the manual. You have to c
hange some columns to TEXT or BLOBs
```

从报错信息里可以看出， MySQL 对一条记录占用的最大存储空间是有限制的，除了 BLOB 或者 TEXT 类型的列之外，其他所有的列（不包括隐藏列和记录头信息）占用的字节长度加起来不能超过 65535 个字节。所以 MySQL 服务器建议我们把存储类型改为 TEXT 或者 BLOB 的类型。这个 65535 个字节除了列本身的数据之外，还包括一些其他的数据（ storage overhead ），比如说我们为了存储一个 VARCHAR(M) 类型的列，其实需要占用3部分存储空间：

- 真实数据
- 真实数据占用字节的长度
- NULL 值标识，如果该列有 NOT NULL 属性则可以没有这部分存储空间

如果该 VARCHAR 类型的列没有 NOT NULL 属性，那最多只能存储 65532 个字节的数据，因为真实数据的长度可能占用2个字节， NULL 值标识需要占用1个字节。

如果 VARCHAR 类型的列有 NOT NULL 属性，那最多只能存储 65533 个字节的数据，因为真实数据的长度可能占用2个字节，不需要 NULL 值标识。

相应的，如果不使用ascii字符集，而使用utf-8的话，则要按照3个字节一个字符来计算。

> 另外，这里我们只讨论了一张表只有一个字段的情况，实际上是一行数据最多只能储存上面那些字节。



###### 记录中的数据太多产生的溢出

我们知道，一页最大为16KB也就是16384字节，而一个varchar类型的列最多可以储存65532字节，这样就可能造成一张数据页放不了一行数据的情况。

在 Compact 和 Reduntant 行格式中，对于占用存储空间非常大的列，在记录的真实数据处只会存储该列的一部分数据，把剩余的数据分散存储在几个其他的页中，然后 记录的真实数据 处用20个字节存储指向这些页的地址（当然这20个字节中还包括这些分散在其他页面中的数据的占用的字节数），从而可以找到剩余数据所在的页。

对于 Compact 和 Reduntant 行格式来说，如果某一列中的数据非常多的话，在本记录的真实数据处只会存储该列的前 768 个字节的数据和一个指向其他页的地址，然后把剩下的数据存放到其他页中，这个过程也叫做 行溢出 ，存储超出 768 字节的那些页面也被称为 溢出页 。


![img](https://github.com/chou401/pic-md/raw/master/285d9c00300a45af94c31b74e5a9df19.png)

###### 行溢出的临界点

首先，MySQL 中规定一个页中至少存放两行记录。其次，以创建只有一个varchar(65532) 字段的表为例，的我们分析一下 一个页面的空间是如何利用的：

- 除了用户储存的真实信息外，储存文件头、文件尾、页面头等信息，需要136个字节。
- 每条记录需要的额外信息是27字节，这27字节包括：
  - 2个字节用于存储真实数据的长度
  - 1个字节用于存储列是否是NULL值
  - 5个字节大小的头信息
  - 6个字节的 row_id 列
  - 6个字节的 transaction_id 列
  - 7个字节的 roll_pointer 列

假设一个列中存储的数据字节数为n，那么发生行溢出现象时需要满足这个式子：136 + 2×(27 + n) > 16384。

求解这个式子得出的解是： n > 8098 。也就是说如果一个列中存储的数据不大于 8098 个字节，那就不会发生行溢出 ，否则就会发生 行溢出 。

不过这个 8098 个字节的结论只是针对只有一个`varchar(65532) `列的表来说的，如果表中有多个列，那上边的式子和结论都需要改一改了，所以重点就是: 不用关注这个临界点是什么，只要知道如果我们想一个行中存储了很大的数据时，可能发生 行溢出 的现象。



##### redundant 格式

与compact 格式相比，没有了变长字段列表以及 NULL值列表，取而代之的是记录了所有真实数据的偏移地址表，偏移地址表是倒序排放的，但是计算偏移量却还是正序开始的从row_id作为第一个， 第一个从0开始累加字段对应的字节数。在记录头信息中, 大部分字段和compact 中的相同，但是对比compact多了。

n_field(记录列的数量)、1byte_offs_flag(字段长度列表每一列占用的字节数)，少了record_type字段。

因为redundant是mysql 5.0 以前就在使用的一种格式，已经非常古老，使用频率非常的低，这里就不过多表述。

##### dynamic 格式

在现在 mysql 5.7 的版本中，使用的格式就是 dynamic。

dynamic 和 compact 基本是相同的，只有在溢出页的处理上面，有所不同。

在compact行格式中，对于占用存储空间非常大的列，在记录的真实数据处只会存储该列的前768个字节的数据，把剩余的数据分散存储在几个其他的页中，然后记录的真实数据处用20个字节存储指向这些页的地址，从而可以找到剩余数据所在的页。

这种在本记录的真实数据处只会存储该列的前768个字节的数据和一个指向其他页的地址，然后把剩下的数据存放到其他页中的情况就叫做行溢出，存储超出768字节的那些页面也被称为溢出页（uncompresse blob page）。

dynamic中会直接在真实数据区记录 20字节 的溢出页地址，而不再去额外记录一部分的数据了。

##### compressed 格式

compressed 格式将会在Dynamic 的基础上面进行压缩处理特别是对溢出页的压缩处理，存储在其中的行数据会以zlib的算法进行压缩，因此对于blob、text这类大长度类型的数据能够进行非常有效的存储。但compressed格式其实也是以时间换空间，性能并不友好，并不推荐在常见的业务中使用。



#### Page Directory（页目录）

记录在页中按照主键值由小到大顺序串联成一个单链表，那如果我们想根据主键值查找页中的某条记录该咋办呢？比如说这样的查询语句：

```mysql
SELECT * FROM page_demo WHERE c1 = 3;
```

可以采用遍历链表的方式，从Infimum 记录（最小记录）开始，向后查找，因为是按照主键值从小到大存放的，当找到或找到大于查找的主键值的时候，就结束了。但这种方法效率太低了。

为了解决直接遍历查询缓慢的问题，设计了类似于课本目录的**页目录**：

- **记录分组**：将所有正常的记录（包括最大和最小记录，不包括标记为已删除的记录）划分为几个组。
- **组内最大记录的n_owned 属性记录记录条数**：每个组的最后一条记录（也就是组内最大的那条记录）的头信息中的n_owned 属性表示该记录拥有多少条记录，也就是该组内共有几条记录。
- **根据最大最小记录的地址偏移量构造页目录**：将每个组的最后一条记录的地址偏移量单独提取出来按顺序存储到靠近页的尾部的地方，这个地方就是所谓的Page Directory ，也就是页目录（此时应该返回头看看页面各个部分的图）。页面目录中的这些地址偏移量被称为槽（英文名： Slot ），所以这个页面目录就是由槽组的。

![image-20230703181623066](https://github.com/chou401/pic-md/raw/master/image-20230703181623066.png)

页目录里面有两个槽，说明分为了两个组，分别是最小记录为一组，四条用户记录与最大记录为一组。所以最小记录的n_owned 属性为1，最大记录的n_owned 属性为5。

对于分组规定的规则：

- 对于最小记录所在的分组只能有 1 条记录，最大记录所在的分组拥有的记录条数只能在 1~8 条之间，剩下的分组中记录的条数范围只能在是 4~8 条之间。
- 初始情况下一个数据页里只有最小记录和最大记录两条记录，它们分属于两个分组。
- 之后每插入一条记录，都会从页目录中找到主键值比本记录的主键值大并且差值最小的槽，然后把该槽中对应的最大记录的n_owned 值加1，表示本组内又添加了一条记录，直到该组中的记录数等于8个。
- 在一个组中的记录数等于8个后再插入一条记录时，会将组中的记录拆分成两个组，一个组中4条记录，另一个5条记录。这个过程会在页目录中新增一个槽来记录这个新增分组中最大的那条记录的偏移量。

往表里继续插入数据

```mysql
INSERT INTO page_demo VALUES(5, 500, 'eeee'), (6, 600, 'ffff'), (7, 700, 'gggg'),(8, 800, 'hhhh'), (9, 900, 'iiii'), (10, 1000, 'jjjj'), (11, 1100, 'kkkk'), (12, 1200, 'llll'), (13, 1300, 'mmmm'), (14, 1400, 'nnnn'), (15, 1500, 'oooo'), (16, 1600, 'pppp');
```

注意看，最小记录始终是在用户最小记录之前，最大记录始终是在用户最大记录之后。

![image-20230703181828283](https://github.com/chou401/pic-md/raw/master/image-20230703181828283.png)

可以看到，每个槽点都记录了每组中最大记录的地址偏移量。

当我们需要在一个数据页中查找指定主键值的记录的过程分为两步：

- 通过**二分法**确定该记录所在的槽，并找到该槽中主键值最小的那条记录。
- 通过记录的next_record 属性遍历该槽所在的组中的各个记录。

#### Page Header（页面头部）

为了能得到一个数据页中存储的记录的状态信息，比如本页中已经存储了多少条记录，第一条记录的地址是什么，页目录中存储了多少个槽等等，特意在页中定义了一个叫Page Header 的部分，它是页结构的第二部分，这个部分占用固定的56 个字节，专门存储各种状态信息，具体各个字节都是干嘛的看下表：

| 名称              | 占用空间（字节） | 描述                                                         |
| ----------------- | ---------------- | ------------------------------------------------------------ |
| PAGE_N_DIR_SLOTS  | 2                | 在页目录中的槽数量                                           |
| PAGE_HEAP_TOP     | 2                | 还未使用的空间最小地址，也就是说从该地址之后就是Free Space   |
| PAGE_BTR_SEG_TOP  | 10               | 本页中的记录的数量（包括最小和最大记录以及标记为删除的记录） |
| PAGE_N_HEAP       | 2                | 第一个已经标记为删除的记录地址（各个已删除的记录通过next_record 也会组成一个单链表，单链表中的记录可以被重新利用） |
| PAGE_FREE         | 2                | 已删除记录占用的字节数                                       |
| PAGE_GARBAGE      | 2                | 最后插入记录的位置                                           |
| PAGE_LAST_INSERT  | 2                | 记录插入的方向                                               |
| PAGE_DIRECTION    | 2                | 一个方向连续插入的记录数量                                   |
| PAGE_N_DIRECTION  | 2                | 该页中记录的数量（不包括最小和最大记录以及被标记为删除的记录） |
| PAGE_N_RECS       | 2                | 修改当前页的最大事务ID，该值仅在二级索引中定义               |
| PAGE_MAX_TRX_ID   | 8                | 当前页在B+树中所处的层级                                     |
| PAGE_LEVEL        | 2                | 索引ID，表示当前页属于哪个索引                               |
| PAGE_INDEX_ID     | 8                | B+树叶子段的头部信息，仅在B+树的Root页定义                   |
| PAGE_BTR_SEG_LEAF | 10               | B+树非叶子段的头部信息，仅在B+树的Root页定义                 |

- PAGE_DIRECTION

  - 假如新插入的一条记录的主键值比上一条记录的主键值大，我们说这条记录的插入方向是右边，反之则是左边。用来表示最后一条记录插入方向的状态就是PAGE_DIRECTION 。

- PAGE_N_DIRECTION

  - 假设连续几次插入新记录的方向都是一致的， InnoDB 会把沿着同一个方向插入记录的条数记下来，这个条数就用PAGE_N_DIRECTION 这个状态表示。当然，如果最后一条记录的插入方向改变了的话，这个状态的值会被清零重新统计。

#### File Header（文件头部）

 Page Header 是专门针对数据页记录的各种状态信息，比方说页里头有多少个记录，有多少个槽等信息。

而File Header 是针对各种类型的页都通用，也就是说不同类型的页都会以File Header 作为第一个组成部分，它描述了一些针对各种页都通用的一些信息，比方说这个页的编号是多少，它的上一个页、下一个页是谁， 这个部分占用固定的38 个字节，是由下边这些内容组成的：  

| 名称                             | 占用空间（字节） | 描述                                                         |
| -------------------------------- | ---------------- | ------------------------------------------------------------ |
| FIL_PAGE_SPACE_OR_CHKSUM         | 4                | 页的校验和（checksum值）                                     |
| FIL_PAGE_OFFSET                  | 4                | 页号                                                         |
| FIL_PAGE_PREV                    | 4                | 上一个页的页号                                               |
| FIL_PAGE_NEXT                    | 4                | 下一个页的页号                                               |
| FIL_PAGE_LSN                     | 8                | 页面被最后修改时对应的日志序列位置（英文名是：Log SequenceNumber） |
| FIL_PAGE_TYPE                    | 2                | 该页的类型                                                   |
| FIL_PAGE_FILE_FLUSH_LSN          | 8                | 仅在系统表空间的一个页中定义，代表文件至少被刷新到了对应的LSN值 |
| FIL_PAGE_ARCH_LOG_NO_OR_SPACE_ID | 4                | 页属于哪个表空间                                             |

看几个重要的部分：

- FIL_PAGE_SPACE_OR_CHKSUM

  这个代表当前页面的校验和（checksum）。校验和：就是对于一个很长很长的字节串来说，通过某种算法来计算一个比较短的值来代表这个很长的字节串，这个比较短的值就称为校验和。这样在比较两个很长的字节串之前先比较这两个长字节串的校验和，如果校验和都不一样两个长字节串肯定是不同的，所以省去了直接比较两个比较长的字节串的时间损耗。

- FIL_PAGE_OFFSET

  每一个页都有一个单独的页号，就跟你的身份证号码一样， InnoDB 通过页号来可以唯一定位一个页。

- FIL_PAGE_TYPE

  InnoDB 为了不同的目的而把页分为不同的类型，这篇文章介绍的其实都是存储记录的**数据页**，也就是所谓的索引页。其实还有很多别的类型的页：日志页、溢出页等。

- FIL_PAGE_PREV 和FIL_PAGE_NEXT

  InnoDB 都是以页为单位存放数据的，存放某种类型的数据占用的空间非常大（比方说一张表中可以有成千上万条记录）， InnoDB 可能不可以一次性为这么多数据分配一个非常大的存储空间，如果分散到多个不连续的页中存储的话需要把这些页关联起来， FIL_PAGE_PREV 和FIL_PAGE_NEXT就分别代表本页的上一个和下一个页的页号。这样通过建立一个双向链表把许许多多的页就都串联起来了，而无需这些页在物理上真正连着。


需要注意的是，并不是所有类型的页都有上一个和下一个页的属性，不过本文中唠叨的数据页（也就是类型为FIL_PAGE_INDEX 的页）是有这两个属性的，所以索引的数据页其实是一个双链表。

#### File Trailer（文件尾部）

InnoDB 存储引擎会把数据存储到磁盘上，但是磁盘速度太慢，需要以页为单位把数据加载到内存中处理，如果该页中的数据在内存中被修改了，那么在修改后的某个时间需要把数据同步到磁盘中。但是在同步了一半的时候中断电了咋办，这不是莫名尴尬么？为了检测一个页是否完整（也就是在同步的时候有没有发生只同步一半的尴尬情况），在每个页的尾部都加了一个File Trailer 部分，这个部分由8 个字节组成，可以分成2个小部分：

- 前4个字节代表页的校验和

  这个部分是和File Header 中的校验和相对应的。每当一个页面在内存中修改了，在同步之前就要把它的校验和算出来，因为File Header 在页面的前边，所以校验和会被首先同步到磁盘，当完全写完时，校验和也会被写到页的尾部，如果完全同步成功，则页的首部和尾部的校验和应该是一致的。如果写了一半儿断电了，那么在File Header 中的校验和就代表着已经修改过的页，而在File Trialer 中的校验和代表着原先的页，二者不同则意味着同步中间出了错。

- 后4个字节代表页面被最后修改时对应的日志序列位置（LSN）

  这个部分也是为了校验页的完整性的，只不过我们目前还没说LSN 是个什么意思，所以大家可以先不用管这个属性。这个File Trailer 与File Header 类似，都是所有类型的页通用的。


### 疑问

#### 小数精度问题

fload和double在存取时因为精度不一致会发生丢失，这里的丢失指的是扩展或者截断，丢失了原有的精度。

在mysql中，我们用【小数数据类型（总长度，小数点长度）】来表示小数的总长度和小数点后面的长度，如deicmal(m，n)。n就是小数点后面的数字个数。float(m,n)、double(m,n) 含义差不多，都是定义长度和精度的。既然定义了精度，为什么还会发生所谓的精度丢失问题呢？

float和double在**存取**时因为精度不一致会发生丢失，不能盲目的说float和double精度可能丢失。具体原因如下：

1. **没有设置精度位数。**
   没有设置精度就是使用默认的精度，此时的策略就是，尽可能保证精度，因此一般使用最高精度存储数据。如果设置数据类型指定了精度，那么存储数据时就按照设置的精度来存储。例如，6.214522存入6位小数的float和double是不会丢失小数精度的，取出来的数还是6.214522。也就是说，一个小数存入相同的精度的数据类型时，精度是不会丢失的。
2. **设置的精度和存储时的精度不一致。**
   当7或更多位精度的数字存入6位精度类型字段时，会发生什么？结果会发生四舍五入。四舍五入的结果就是匹配字段的数据类型的精度长度。此时精度也会丢失。不管内部如何处理，我们得到的数据是经过四舍五入的。但是有一点可以确定，我们在读取取舍后的数字时，是固定的。虽然浮点数存储的不是确切的数值，但是在你指定的精度长度条件下，存取都是确定的一个数值。而发生精度变化的就是数值的精度和字段的精度长度不匹配，从而发生数值扩展精度和截断精度问题，这也就是浮点数精度不准确的问题。
3. **mysql数据库使用其他数据库引擎来查询。**
   这个精度丢失的原因，就可能是不同的数据库引擎对浮点数的精度扩展和截断处理策略不一致，而且，存储时策略也不一致。所以导致精度会出现各种变化。这种问题也就是催生decimal类型的出现。我们前面看到的decimal是可以确切存储小数的精度的。因为在存储的时候会将小数以字符串存储，就不会再发生精度的扩展问题。但是decimal依然会发生精度截断问题。如果decimal指定精度为2位小数，存入的是这样的值：12.123，你觉得结果如何？当然还是会发生四舍五入。结果就是12.12，然而12.12以字符串形式存入了数据库，此后，12.12始终都是12.12，变现出来的是小数，然而内部是字符串形式存储，所以，小数精度不会再发生变化了。我们不管以什么精度来获取这个值，都是12.12，而且，不管是一般数据库引擎读取到的也都是12.12，所以decimal才是大家推荐使用的金额存储类型。

浮点数类型是把十进制数转换成二进制数存储，decimal是把十进制的整数部分和小数部分拆开，分别装换成十六进制，进行存储。这样，所有的数值，就都可以精准表达了。

#### 大数据查询

**创建表**

```mysql
CREATE TABLE `user_operation_log`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` varchar(64) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `ip` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `op_data` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr1` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr2` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr3` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr4` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr5` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr6` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr7` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr8` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr9` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr10` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr11` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  `attr12` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 1 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = Dynamic;
```

**创建数据脚本**

采用批量插入，效率会快很多，而且每1000条数就commit，数据量太大，也会导致批量插入效率慢

```mysql
DELIMITER ;;
CREATE PROCEDURE batch_insert_log()
BEGIN
  DECLARE i INT DEFAULT 1;
  DECLARE userId INT DEFAULT 10000000;
 set @execSql = 'INSERT INTO `test`.`user_operation_log`(`user_id`, `ip`, `op_data`, `attr1`, `attr2`, `attr3`, `attr4`, `attr5`, `attr6`, `attr7`, `attr8`, `attr9`, `attr10`, `attr11`, `attr12`) VALUES';
 set @execData = '';
  WHILE i<=10000000 DO
   set @attr = "'测试很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长的属性'";
  set @execData = concat(@execData, "(", userId + i, ", '10.0.69.175', '用户登录操作'", ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ",", @attr, ")");
  if i % 1000 = 0
  then
     set @stmtSql = concat(@execSql, @execData,";");
    prepare stmt from @stmtSql;
    execute stmt;
    DEALLOCATE prepare stmt;
    commit;
    set @execData = "";
   else
     set @execData = concat(@execData, ",");
   end if;
  SET i=i+1;
  END WHILE;

END;;
DELIMITER ;
```

**开始测试**

> “
>
> 电脑配置比较低：win10 标压渣渣i5 读写约500MB的SSD

由于配置低，本次测试只准备了3148000条数据，占用了磁盘5G(还没建索引的情况下)，跑了38min，电脑配置好的同学，可以插入多点数据测试

```mysql
SELECT count(1) FROM `user_operation_log`
```

返回结果：3148000

三次查询时间分别为：

- 14060 ms
- 13755 ms
- 13447 ms

**普通分页查询**

MySQL 支持 LIMIT 语句来选取指定的条数数据， Oracle 可以使用 ROWNUM 来选取。

MySQL分页查询语法如下：

```mysql
SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
```

- 第一个参数指定第一个返回记录行的偏移量
- 第二个参数指定返回记录行的最大数目

下面我们开始测试查询结果：

```mysql
SELECT * FROM `user_operation_log` LIMIT 10000, 10
```

查询3次时间分别为：

- 59 ms
- 49 ms
- 50 ms

这样看起来速度还行，不过是本地数据库，速度自然快点。

换个角度来测试

**相同偏移量，不同数据量**

```mysql
SELECT * FROM `user_operation_log` LIMIT 10000, 10
SELECT * FROM `user_operation_log` LIMIT 10000, 100
SELECT * FROM `user_operation_log` LIMIT 10000, 1000
SELECT * FROM `user_operation_log` LIMIT 10000, 10000
SELECT * FROM `user_operation_log` LIMIT 10000, 100000
SELECT * FROM `user_operation_log` LIMIT 10000, 1000000
```

查询时间如下：

| 数量      | 第一次  | 第二次  | 第三次  |
| :-------- | :------ | :------ | :------ |
| 10条      | 53ms    | 52ms    | 47ms    |
| 100条     | 50ms    | 60ms    | 55ms    |
| 1000条    | 61ms    | 74ms    | 60ms    |
| 10000条   | 164ms   | 180ms   | 217ms   |
| 100000条  | 1609ms  | 1741ms  | 1764ms  |
| 1000000条 | 16219ms | 16889ms | 17081ms |

从上面结果可以得出结束：**数据量越大，花费时间越长**

**相同数据量，不同偏移量**

```mysql
SELECT * FROM `user_operation_log` LIMIT 100, 100
SELECT * FROM `user_operation_log` LIMIT 1000, 100
SELECT * FROM `user_operation_log` LIMIT 10000, 100
SELECT * FROM `user_operation_log` LIMIT 100000, 100
SELECT * FROM `user_operation_log` LIMIT 1000000, 100
```

| 偏移量  | 第一次 | 第二次 | 第三次 |
| :------ | :----- | :----- | :----- |
| 100     | 36ms   | 40ms   | 36ms   |
| 1000    | 31ms   | 38ms   | 32ms   |
| 10000   | 53ms   | 48ms   | 51ms   |
| 100000  | 622ms  | 576ms  | 627ms  |
| 1000000 | 4891ms | 5076ms | 4856ms |

从上面结果可以得出结束：**偏移量越大，花费时间越长**

```mysql
SELECT * FROM `user_operation_log` LIMIT 100, 100
SELECT id, attr FROM `user_operation_log` LIMIT 100, 100
```

**如何优化**

既然我们经过上面一番的折腾，也得出了结论，针对上面两个问题：偏移大、数据量大，我们分别着手优化

优化偏移量大问题

**采用子查询方式**

我们可以先定位偏移位置的 id，然后再查询数据

```mysql
SELECT * FROM `user_operation_log` LIMIT 1000000, 10

SELECT id FROM `user_operation_log` LIMIT 1000000, 1

SELECT * FROM `user_operation_log` WHERE id >= (SELECT id FROM `user_operation_log` LIMIT 1000000, 1) LIMIT 10
```

查询结果如下：

| sql                  | 花费时间 |
| :------------------- | :------- |
| 第一条               | 4818ms   |
| 第二条(无索引情况下) | 4329ms   |
| 第二条(有索引情况下) | 199ms    |
| 第三条(无索引情况下) | 4319ms   |
| 第三条(有索引情况下) | 201ms    |

从上面结果得出结论：

- 第一条花费的时间最大，第三条比第一条稍微好点
- 子查询使用索引速度更快

缺点：只适用于id递增的情况

id非递增的情况可以使用以下写法，但这种缺点是分页查询只能放在子查询里面

注意：某些 mysql 版本不支持在 in 子句中使用 limit，所以采用了多个嵌套select

```mysql
SELECT * FROM `user_operation_log` WHERE id IN (SELECT t.id FROM (SELECT id FROM `user_operation_log` LIMIT 1000000, 10) AS t)
```

**采用 id 限定方式**

这种方法要求更高些，id必须是连续递增，而且还得计算id的范围，然后使用 between，sql如下

```mysql
SELECT * FROM `user_operation_log` WHERE id between 1000000 AND 1000100 LIMIT 100

SELECT * FROM `user_operation_log` WHERE id >= 1000000 LIMIT 100
```

查询结果如下：

| sql    | 花费时间 |
| :----- | :------- |
| 第一条 | 22ms     |
| 第二条 | 21ms     |

从结果可以看出这种方式非常快

*注意：这里的 LIMIT 是限制了条数，没有采用偏移量*

**优化数据量大问题**

返回结果的数据量也会直接影响速度

```mysql
SELECT * FROM `user_operation_log` LIMIT 1, 1000000

SELECT id FROM `user_operation_log` LIMIT 1, 1000000

SELECT id, user_id, ip, op_data, attr1, attr2, attr3, attr4, attr5, attr6, attr7, attr8, attr9, attr10, attr11, attr12 FROM `user_operation_log` LIMIT 1, 1000000
```

查询结果如下：

| sql    | 花费时间 |
| :----- | :------- |
| 第一条 | 15676ms  |
| 第二条 | 7298ms   |
| 第三条 | 15960ms  |

从结果可以看出减少不需要的列，查询效率也可以得到明显提升

第一条和第三条查询速度差不多，这时候你肯定会吐槽，那我还写那么多字段干啥呢，直接 * 不就完事了

注意本人的 MySQL 服务器和客户端是在_同一台机器_上，所以查询数据相差不多，有条件的同学可以测测客户端与MySQL分开

**SELECT \* 它不香吗？**

在这里顺便补充一下为什么要禁止 `SELECT *`。难道简单无脑，它不香吗？

主要两点：

1. 用 "`SELECT * `" 数据库需要解析更多的对象、字段、权限、属性等相关内容，在 SQL 语句复杂，硬解析较多的情况下，会对数据库造成沉重的负担。
2. 增大网络开销，`*` 有时会误带上如log、IconMD5之类的无用且大文本字段，数据传输size会几何增涨。特别是MySQL和应用程序不在同一台机器，这种开销非常明显。

#### 批量修改数据表和数据表中所有字段的字符集

查看数据表的行格式：

```mysql
show table status like 库名
```

查看库的字符集：

```mysql
show database status from 库名
```

查看表中所有列的字符集：

```mysql
show full columns from 表名
```

更改表编码(字符集)**和表中所有字段**的编码(字符集)：

```mysql
ALTER TABLE TABLE_NAME CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

如果一个数据库有很多表要修改，可以使用如下办法：

查询某个数据库所有表名的语句：

```mysql
SELECT TABLE_NAME from information_schema.`TABLES` WHERE TABLE_SCHEMA = 'DATABASE_NAME';
```

得到所有的表名，我们可以把表名拼接到上面更改表编码(字符集)**和表中所有字段**的编码(字符集)的语句中去，得到如下语句：

```sql
SELECT
	CONCAT(
		'ALTER TABLE ',
		TABLE_NAME,
		' CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;'
	)
FROM
	information_schema.`TABLES`
WHERE
	TABLE_SCHEMA = 'DATABASE_NAME';
```

### 性能优化

#### 建立索引的几个准则

1. 合理的建立索引能够加速数据读取效率，不合理的建立索引反而会拖慢数据库的响应速度。
2. 索引越多，更新数据的速度越慢。
3. 尽量在采用MyIsam作为引擎的时候使用索引（因为MySQL以BTree存储索引），而不是InnoDB，但MyISAM不支持Transcation。
4. 当你的程序和数据库结构/SQL语句已经优化到无法优化的程度，而程序瓶颈并不能顺利解决，那就是应该考虑使用诸如memcached这样的分布式缓存系统的时候了。
5. 习惯和强迫自己用EXPLAIN来分析你SQL语句的性能。



#### count的优化

比如：计算id大于5的城市。

a：

```mysql
select count(*) from world.city where id > 5;
```

b：

```mysql
select (select count(*) from world.city) – count(*) from world.city where id <= 5;
```

a语句当行数超过11行的时候需要扫描的行数比b语句要多，b语句扫描了6行，此种情况下，b语句比a语句更有效率。

当没有where语句的时候直接select count(*) from world.city这样会更快，因为MySQL总是知道表的行数。



#### 避免使用不兼容的数据类型

例如float和int、char和varchar、binary和varbinary是不兼容的，数据类型的不兼容可能使优化器无法执行一些本来可以进行的优化操作。

**在程序中，保证在实现功能的基础上：**

- 尽量减少对数据库的访问次数；
- 通过搜索参数，尽量减少对表的访问行数,最小化结果集，从而减轻网络负担；
- 能够分开的操作尽量分开处理，提高每次的响应速度。

**在数据窗口使用SQL时：**

- 尽量把使用的索引放在选择的首列；
- 算法的结构尽量简单。

**在查询时：**

- 不要过多地使用通配符如 SELECT * FROM T1语句，要用到几列就选择几列如：SELECT COL1,COL2 FROM T1；
- 在可能的情况下尽量限制尽量结果集行数如：SELECT TOP 300 COL1,COL2,COL3 FROM T1,因为某些情况下用户是不需要那么多的数据的。

**不要在应用中使用数据库游标：**

- 游标是非常有用的工具，但比使用常规的、面向集的SQL语句需要更大的开销；
- 按照特定顺序提取数据的查找。



#### 索引字段上进行运算会使索引失效

尽量避免在WHERE子句中对字段进行函数或表达式操作，这将导致引擎放弃使用索引而进行全表扫描。

如：

```mysql
SELECT * FROM T1 WHERE F1/2=100
```

应改为：

```mysql
SELECT * FROM T1 WHERE F1=100*2
```



#### 避免使用某些操作符

避免使用!=或＜＞、IS NULL或IS NOT NULL、IN ，NOT IN等这样的操作符。

因为这会使系统无法使用索引，而只能直接搜索表中的数据。

例如: SELECT id FROM employee WHERE id != “B%” 优化器将无法通过索引来确定将要命中的行数，因此需要搜索该表的所有行。

在in语句中能用exists语句代替的就用exists。



#### 尽量使用数字型字段

一部分开发人员和数据库管理人员喜欢把包含数值信息的字段设计为字符型，这会降低查询和连接的性能，并会增加存储开销。

这是因为引擎在处理查询和连接回逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了。



#### 合理使用EXISTS、NOT EXISTS子句

如下所示：

1：

```mysql
SELECT SUM(T1.C1) FROM T1 WHERE (SELECT COUNT(*)FROM T2 WHERE T2.C2=T1.C2>0)
```

2：

```mysql
SELECT SUM(T1.C1) FROM T1WHERE EXISTS(SELECT * FROM T2 WHERE T2.C2=T1.C2)
```

两者生相同的结果，但是后者的效率显然要高于前者，因为后者不会产生大量锁定的表扫描或是索引扫描。

如果你想校验表里是否存在某条纪录，不要用count(*)那样效率很低，而且浪费服务器资源。可以用EXISTS代替。

如：

```mysql
IF (SELECT COUNT(*) FROM table_name WHERE column_name = ‘xxx’)
```

可以写成：

```mysql
IF EXISTS (SELECT * FROM table_name WHERE column_name = ‘xxx’)
```



#### 避免使用一些语句

- 能够用BETWEEN的就不要用IN；
- 能够用DISTINCT的就不用GROUP BY；
- 尽量不要用SELECT INTO语句。SELECT INTO 语句会导致表锁定，阻止其他用户访问该表。



#### 必要时强制查询优化器使用某个索引

```mysql
SELECT * FROM T1 WHERE nextprocess = 1 AND processid IN (8,32,45)
```

改成：

```mysql
SELECT * FROM T1 (INDEX = IX_ProcessID) WHERE nextprocess = 1 AND processid IN (8,32,45)
```

则查询优化器将会强行利用索引IX_ProcessID 执行查询。



#### 消除对大型表行数据的顺序存取

尽管在所有的检查列上都有索引，但某些形式的WHERE子句强迫优化器使用顺序存取。

如：

```mysql
SELECT * FROM orders WHERE (customer_num=104 AND order_num>1001) OR order_num=1008
```

解决办法可以使用并集来避免顺序存取：

```mysql
 SELECT * FROM orders WHERE customer_num=104 AND order_num>1001 UNION SELECT * FROM orders WHERE order_num=1008
```

这样就能利用索引路径处理查询。jacking 数据结果集很多，但查询条件限定后结果集不大的情况下，后面的语句快。



#### 避免使用非打头字母搜索

尽量避免在索引过的字符数据中，使用非打头字母搜索。这也使得引擎无法利用索引。

见如下例子：

```mysql
SELECT * FROM T1 WHERE NAME LIKE ‘%L%’ SELECT * FROM T1 WHERE SUBSTING(NAME,2,1)=’L’ SELECT * FROM T1 WHERE NAME LIKE ‘L%’
```

即使NAME字段建有索引，前两个查询依然无法利用索引完成加快操作，引擎不得不对全表所有数据逐条操作来完成任务。

而第三个查询能够使用索引来加快操作，不要习惯性的使用 ‘%L%’这种方式(会导致全表扫描)，如果可以使用`L%’相对来说更好。



#### 建议

虽然UPDATE、DELETE语句的写法基本固定，但是还是对UPDATE语句给点建议：

- 尽量不要修改主键字段；
- 当修改VARCHAR型字段时，尽量使用相同长度内容的值代替；
- 尽量最小化对于含有UPDATE触发器的表的UPDATE操作；
- 避免UPDATE将要复制到其他数据库的列；
- 避免UPDATE建有很多索引的列；
- 避免UPDATE在WHERE子句条件中的列。



#### 能用UNION ALL就不要用UNION

UNION ALL不执行SELECT DISTINCT函数，这样就会减少很多不必要的资源。

在跨多个不同的数据库时使用UNION是一个有趣的优化方法，UNION从两个互不关联的表中返回数据，这就意味着不会出现重复的行，同时也必须对数据进行排序。

我们知道排序是非常耗费资源的，特别是对大表的排序，UNION ALL可以大大加快速度，如果你已经知道你的数据不会包括重复行，或者你不在乎是否会出现重复的行，在这两种情况下使用UNION ALL更适合。

此外，还可以在应用程序逻辑中采用某些方法避免出现重复的行，这样UNION ALL和UNION返回的结果都是一样的，但UNION ALL不会进行排序。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### 字段数据类型优化

避免使用NULL类型：NULL对于大多数数据库都需要特殊处理，MySQL也不例外。

它需要更多的代码，更多的检查和特殊的索引逻辑，有些开发人员完全没有意识到，创建表时NULL是默认值，但大多数时候应该使用NOT NULL，或者使用一个特殊的值，如0，-1作为默认值。

尽可能使用更小的字段：MySQL从磁盘读取数据后是存储到内存中的，然后使用cpu周期和磁盘I/O读取它。

这意味着越小的数据类型占用的空间越小，从磁盘读或打包到内存的效率都更好，但也不要太过执着减小数据类型，要是以后应用程序发生什么变化就没有空间了。

修改表将需要重构，间接地可能引起代码的改变，这是很头疼的问题，因此需要找到一个平衡点。

优先使用定长型。



#### 一次性插入多条数据

程序中如果一次性对同一个表插入多条数据，比如以下语句：

```mysql
insert into person(name,age) values(‘xboy’, 14);
insert into person(name,age) values(‘xgirl’, 15);
insert into person(name,age) values(‘nia’, 19);
```

把它拼成一条语句执行效率会更高：

```mysql
insert into person(name,age) values(‘xboy’, 14), (‘xgirl’, 15),(‘nia’, 19);
```



#### 无意义语句

不要在选择的栏位上放置索引，这是无意义的。应该在条件选择的语句上合理的放置索引，比如where、order by。

```mysql
SELECT id,title,content,cat_id FROM article WHERE cat_id = 1;
```

上面这个语句，你在id/title/content上放置索引是毫无意义的，对这个语句没有任何优化作用。但是如果你在外键cat_id上放置一个索引，那作用就相当大了。



#### ORDER BY语句的MySQL优化

ORDER BY + LIMIT组合的索引优化。如果一个SQL语句形如：

```mysql
SELECT [column1],[column2],…. FROM [TABLE] ORDER BY [sort] LIMIT [offset],[LIMIT];
```

这个SQL语句优化比较简单，在[sort]这个栏位上建立索引即可。

b. WHERE + ORDER BY + LIMIT组合的索引优化，形如：

```mysql
SELECT [column1],[column2],…. FROM [TABLE] WHERE [columnX] = [VALUE] ORDER BY [sort] LIMIT [offset],[LIMIT];
```

这个语句，如果你仍然采用第一个例子中建立索引的方法，虽然可以用到索引，但是效率不高。

更高效的方法是建立一个联合索引(columnX,sort)。WHERE + IN + ORDER BY + LIMIT组合的索引优化，形如：

```mysql
SELECT [column1],[column2],…. FROM [TABLE] WHERE [columnX] IN ([value1],[value2],…) ORDER BY [sort] LIMIT [offset],[LIMIT];
```

这个语句如果你采用第二个例子中建立索引的方法，会得不到预期的效果（仅在[sort]上是using index，WHERE那里是using where;using filesort），理由是这里对应columnX的值对应多个。目前还没有找到比较优秀的办法，等待高手指教。

WHERE+ORDER BY多个栏位+LIMIT，比如:

```mysql
SELECT * FROM [table] WHERE uid=1 ORDER x,y LIMIT 0,10;
```

对于这个语句，大家可能是加一个这样的索引：(x,y,uid)。但实际上更好的效果是(uid,x,y)。这是由MySQL处理排序的机制造成的。



## JDBC

### 事务

事务（Transaction）：一般是指要做的或所做的事情。

  - 在计算机中指：访问并可能更新的数据库中各种数据项的一个程序单元（unit）。

程序执行单元（unit）：数据库操作一组SQL语句的执行。

  - 由高级数据库操作语言或者编程语言书写。


  - 由事务开始（begin transaction）和事务结束（end transaction）之间执行的全体操作组成。

一个事务是由一条或多条对数据库操作的SQL语句所组成的一个不可分割的工作单元，只有当事务中的所有操作都正常执行完了，整个事务才会被提交给数据库。

例如：

一个银行转账操作，首先从A账户减掉指定的金额，然后B账户增加指定的金额，此时转账操作结束。上面的操作如果对应成数据库操作，那么就需要执行两条Update语句。数据库把这两条Update语句的执行就是一个事务。

### 数据库的事务四大特征【ACID】

- **A 原子性（atomicity）**：一个事务一个不可分割的工作单位,事务中包括的操作要么做，要么不做。

- **C 一致性（consistency）**：事务必须使数据库从一个一致性的状态转变成另一个一致性的状态。一致性与原子性密切相关。

- **I 隔离性（isolation）**：一个事务的执行不能干扰其他事务。即：一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的个个事务直接不能相互干扰。

- **D 持久性（durability）**：持久性也称永久性（permanence）,指一个事务一旦提交，他对数据库中的数据的改变就应该是永久的。接下来的其他操作或故障不应该对其有任何影响。

### 事务的隔离级别

在数据库操作中，为了有效保证并发读取数据的正确性，提出的事务隔离级别。

**JDBC定义了五种事务隔离级别**

- TRANSACTION_READ_UNCOMMITTED：允许脏读、不可重复读和幻读。

- TRANSACTION_READ_COMMITTED：禁止脏读，但允许不可重复读和幻读。


- TRANSACTION_REPEATABLE_READ：禁止脏读、不可重复读，但允许幻读。


- TRANSACTION_NONE JDBC：驱动不支持事务。


| JDBC                         | 数据库隔离级别 | 数据库访问情况                                               |
| ---------------------------- | -------------- | ------------------------------------------------------------ |
| TRANSACTION_READ_UNCOMMITTED | ur             | 就是俗称“脏读”，在没有提交数据时，能够读到已经更新得是数据。 |
| TRANSACTION_RED_COMMITTED    | cs             | 在一个事务中进行查询时，允许读取提交的数据，数据提交后，当前查询就可以读取到数据。Update数据时候并不锁住表。 |
| TRANSACTION_REPEATABLE       | rs             | 在一个事务中进行查询时，不允许读取其他事务update的中，允许读取其他事物提交的新增数据。 |
| TRANSACTION_SERIALIZABLE     | rr             | 在一个事务中进行查询时，不允许任何对这个表查询表的数据修改。 |

### 并发事务带来的问题

在典型的应用程序中，多个事务并发运行，经常会操作相同的数据来完成个自的任务（多个用对统一数据进行操作）。并发虽然是必须的，但有可能出现以下问题。

- **脏读（Dirty read）**：当一个事务正在访问数据并且对数据进行修改，而这种修改还没有提交到数据库中，这时另外一个事务也访问到这个数据，然后使用用了这个数据。因为这个数据是还没有提交数据，那么另一个事务读到的数据就是“脏数据”，根据“脏数据”所做的操作可能是不正确的。

- **丢失修改（lost to modify）**：指在一个事务读取一个数据时，另外一个事务也在访问该数据，那么在第一个事务中修改了这个数据后，，第二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修改。例如：事务1读取某表数据A=20，事务2也读取A=20，事务1修改A=A-1,事务2也修改A=A-1，最终结果A=19，事务1修改被丢弃。

- **不可重复读（Unrepeatableread）**：指在一个事务内多次读取同一个数据。在这个事务还没有结束时，另一个事务也访问该数据，那么，在第一个事务中的两次读数据之间，由于第二个事务修改导致第一个事物两次读取的数据可能不太一样，这就发生了在一个事务内两次读到的数据不一样的情况，因此称为不可重复读。

- **幻读（Phantom read）**：类似与不可重复读类似。它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）插入了一些数据时。在随后的查询中，第一个事物（T1）就会发现对了一些根本不存在的记录，就好像是发生幻觉。称为幻读。

**不可重复读和幻读的区别**

不可重复读的重点是修改，幻读的中单在于新增或者删除。

### JDBC的事务管理操作

JDBC的事务管理操作需要通过java.sql.Connection接口来设置。

**隔离级别**

| static int | [TRANSACTION_NONE](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_NONE) 指示不支持事务的常量。 |
| ---------- | ------------------------------------------------------------ |
| static int | [ TRANSACTION_READ_COMMITTED](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_READ_COMMITTED) 一个常数表示防止脏读; 可能会发生不可重复的读取和幻像读取。 |
| static int | [ TRANSACTION_READ_UNCOMMITTED](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_READ_UNCOMMITTED) 一个常量表示可能会发生脏读，不可重复读和幻读。 |
| static int | [ TRANSACTION_REPEATABLE_READ](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_REPEATABLE_READ) 一个常量表示防止了脏读和不可重复读; 可以发生幻读。 |
| static int | [TRANSACTION_SERIALIZABLE](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_SERIALIZABLE) 一个常数表示防止脏读，不可重复读和幻读。 |

| Modifier and Type       | Constant Field                                               | Value |
| ----------------------- | ------------------------------------------------------------ | ----- |
| public static final int | [TRANSACTION_NONE](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_NONE) | 0     |
| public static final int | [TRANSACTION_READ_COMMITTED](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_READ_COMMITTED) | 2     |
| public static final int | [TRANSACTION_READ_UNCOMMITTED](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_READ_UNCOMMITTED) | 1     |
| public static final int | [TRANSACTION_REPEATABLE_READ](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_REPEATABLE_READ) | 4     |
| public static final int | [TRANSACTION_SERIALIZABLE](https://blog.csdn.net/weixin_49576031/article/details/121632045#TRANSACTION_SERIALIZABLE) | 8     |

 设置事务隔离级别的方法：

| void | [setTransactionIsolation](https://blog.csdn.net/weixin_49576031/article/details/121632045#setTransactionIsolation-int-)(int level) 尝试将此 Connection对象的事务隔离级别更改为给定的对象。 |
| ---- | ------------------------------------------------------------ |

Connection接口对象.setTransactionIsolation(Connection.TRANSACTION_SERIALIZABLE)。

Connection接口对象.setTransactionIsolation(8)。

设置是否自动提交事务方法【默认JDBC事务是自动提交的】：

| void | [setAutoCommit](https://blog.csdn.net/weixin_49576031/article/details/121632045#setAutoCommit-boolean-)(boolean autoCommit) 将此连接的自动提交模式设置为给定状态。 |
| ---- | ------------------------------------------------------------ |

Connection接口对象.setAutoCommit(false) ; 设置为手动提交事务。

事务的提交方法 ：

| void | [commit](https://blog.csdn.net/weixin_49576031/article/details/121632045#commit--)() 使自上次提交/回滚以来所做的所有更改都将永久性，并释放此 Connection对象当前持有的任何数据库锁。 |
| ---- | ------------------------------------------------------------ |

事务的回滚方法【异常中执行】：

| void | [rollback](https://blog.csdn.net/weixin_49576031/article/details/121632045#rollback--)() 撤消在当前事务中所做的所有更改，并释放此 Connection对象当前持有的任何数据库锁。 |
| ---- | ------------------------------------------------------------ |

### Class.forName()的作用

在Java语言中，任何类只有被装载到JVM上才能运行。Class.forName() 方法的作用就是把类加载到JVM中，它会返回一个与带有给定字符串名的类或接口相关联的Class对象，并且JVM会加载这个类，同时JVM会执行该类的静态代码段。

### Statement、PreparedStatement、CallableStatement

- Statement用于执行不带参数的简单SQL语句，并返回它所生成结果的对象，每次执行SQL语句时，数据库都要编译该SQL语句。


- PreparedStatement表示预编译的SQL语句的对象，用于执行带参数的预编译SQL语句。


- CallableStatement提供了用来调用数据库中存储过程的接口。

  - 使用Connection对象的prepareCall()方法创建CallableStatement对象，它为所有的DBMS（Database Management System）提供了一种以标准形式调用已存储过程的方法；

  - IN参数使用 CallableStatement.setXxx()设置；

  - OUT参数使用CallableStatement.registerOutParameter()设置，使用CallableStatement.getXxx()获取输出参数。

  - 执行使用execute()，或者executeUpdate()、executeQuery()。


Statement对象与PreparedStatement对象能够完成相同的功能，PreparedStatement具有以下优点：

- 效率更高：在使用PreparedStatement对象执行SQL命令时，命令会被数据库进行编译和解析，并放到命令缓冲区，然后，每当执行同一个PreparedStatement对象时，由于在缓存区中可以发现预编译的命令，虽然它会被再解析一次，但是不会被再一次编译，是可以重复使用的，能够有效提高系统性能，因此，如果要执行插入，更新，删除等操作，最好使用PreparedSatement。
- 代码可读性和可维护性更好。
- 安全性更好：使用PreparedStatement能够预防SQL注入攻击。

**总结**：

1. SQL语句执行的connection与事务的connection对象要相同。

2. 开始事务connection对象.setAutoCommit(false)。
3. 设置事务的隔离级别 connection对象.setTransactionIsoation(Connection.TRANSACTION_SERIALIZABLE)。
4. 提交事务 connection对象.commit()。
5. 设置事务回滚【异常中进行】connection对象.rollback()。



## Dubbo



## Netty

### BIO（Blocking IO）

![image-20230313103444295](https://github.com/chou401/pic-md/raw/master/img/image-20230313103444295.png)

### NIO（Non Blocking IO）

![image-20230313103559073](https://github.com/chou401/pic-md/raw/master/img/image-20230313103559073.png)

selector（多路复用器）

epoll

I/O多路复用底层主要用的linux内核函数（select，poll，epoll）来实现，windows不支持epoll实现，windows底层是基于winsock2的select函数实现的（不开源）



创建两个线程组bossGroup和workerGroup，含有的子线程NioEventLoop的个数默认为cpu合数的两倍，bossGroup只是处理连接请求，真正的和客户端业务处理，会交给workerGroup完成。

零拷贝机制

![image-20230314182800432](https://github.com/chou401/pic-md/raw/master/img/image-20230314182800432.png)

长连接心跳保活机制

直接内存

DiretByteBuffer 分配在物理内存

## CompletableFuture

### supplyAsync

用来开启异步任务。

```java
public static void main(String[] args) {
    SmallTool.print("小白进入餐厅");
    SmallTool.print("小白点了菜");
    CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> {
        SmallTool.print("厨师炒菜");
        SmallTool.sleep(200);
        SmallTool.print("厨师打饭");
        SmallTool.sleep(100);
        return "菜 做好了";
    });

    SmallTool.print("小白在打游戏");
    // join() 会等待任务执行结束，然后返回任务的结果
    SmallTool.print(String.format("%s,小白开吃", cf.join()));
}
```

### thenCompose

把前面任务的结果交给下一个异步任务。

```java
public static void main(String[] args) {
    SmallTool.print("小白进入餐厅");
    SmallTool.print("小白点了菜 + 米饭");
    CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> {
        SmallTool.print("厨师炒菜");
        SmallTool.sleep(200);
        return "菜";
    }).thenCompose(dish -> CompletableFuture.supplyAsync(() -> {
        SmallTool.print("服务员打饭");
        SmallTool.sleep(100);
        return dish + " + 米饭";
    }));

    SmallTool.print("小白在打游戏");
    // join() 会等待任务执行结束，然后返回任务的结果
    SmallTool.print(String.format("%s,小白开吃", cf.join()));
}
```

### thenConbine

等待两个任务都执行完，得到两个结果，再把两个加工成一个结果。

```java
public static void main(String[] args) {
    SmallTool.print("小白进入餐厅");
    SmallTool.print("小白点了菜 + 米饭");
    CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> {
        SmallTool.print("厨师炒菜");
        SmallTool.sleep(200);
        return "菜";
    }).thenCombine(CompletableFuture.supplyAsync(() -> {
        SmallTool.print("服务员蒸饭");
        SmallTool.sleep(300);
        return "米饭";
    }), (dish, rice) -> {
        SmallTool.print("服务员打饭");
        SmallTool.sleep(100);
        return String.format("%s + %s 好了", dish, rice);
    });

    SmallTool.print("小白在打游戏");
    SmallTool.print(String.format("%s,小白开吃", cf.join()));
}
```

### applyToEither

上个任务和这个任务一起运行，哪个先运行完成，就把哪个任务结果交个function。

```java
public static void main(String[] args) {
    SmallTool.print("张三走出餐厅，来到公交站");
    SmallTool.print("等待 700路 或者 800路 公交到来");
    CompletableFuture<String> bus = CompletableFuture.supplyAsync(() -> {
        SmallTool.print("700路公交正在赶来");
        SmallTool.sleep(300);
        return "700路到了";
    }).applyToEither(CompletableFuture.supplyAsync(() -> {
        SmallTool.print("800路公交正在赶来");
        SmallTool.sleep(200);
        return "800路到了";
    }), firstComeBus -> firstComeBus);

    SmallTool.print(String.format("%s,小白坐车回家", bus.join()));
}
```

### thenApply

把前面异步任务的结果，交给后面的function，一个线程操作。

```java
public static void main(String[] args) {
    SmallTool.print("小白吃好了");
    SmallTool.print("小白 结账、要求开发票");
    CompletableFuture<String> invoice = CompletableFuture.supplyAsync(() -> {
        SmallTool.print("服务员收款 500元");
        SmallTool.sleep(200);
        return "500";
    }).thenApply(money -> {
        SmallTool.print(String.format("服务员开发票 面额 %s元", money));
        SmallTool.sleep(200);
        return String.format("%s元发票", money);
    });

    SmallTool.print("小白 街道朋友的电话，想一起打游戏");
    SmallTool.print(String.format("小白拿到%s,准备回家", invoice.join()));
}
```

### thenApplyAsync

把前面异步任务的结果，交给后面的function，两个线程操作，若线程一样（线程复用）。

```java
public static void main(String[] args) {
    SmallTool.print("小白吃好了");
    SmallTool.print("小白 结账、要求开发票");
    CompletableFuture<String> invoice = CompletableFuture.supplyAsync(() -> {
        SmallTool.print("服务员收款 500元");
        SmallTool.sleep(300);
        return "500";
    }).thenApplyAsync(money -> {
        SmallTool.print(String.format("服务员开发票 面额 %s元", money));
        SmallTool.sleep(200);
        return String.format("%s元发票", money);
    });

    SmallTool.print("小白 街道朋友的电话，想一起打游戏");
    SmallTool.print(String.format("小白拿到%s,准备回家", invoice.join()));
}
```

### exceptionally

处理异常情况，链路上面的任何一个任务抛出异常，都会调用。

```java
public static void main(String[] args) {
    SmallTool.print("张三走出餐厅，来到公交站");
    SmallTool.print("等待 700路 或者 800路 公交到来");
    CompletableFuture<String> bus = CompletableFuture.supplyAsync(() -> {
        SmallTool.print("700路公交正在赶来");
        SmallTool.sleep(100);
        return "700路到了";
    }).applyToEither(CompletableFuture.supplyAsync(() -> {
        SmallTool.print("800路公交正在赶来");
        SmallTool.sleep(200);
        return "800路到了";
    }), firstComeBus -> {
        SmallTool.print(firstComeBus);
        if (firstComeBus.startsWith("700")) {
            throw new RuntimeException("撞树了。。。");
        }
        return firstComeBus;
    }).exceptionally(e -> {
        SmallTool.print(e.getMessage());
        SmallTool.print("小白叫出租车");
        return "出租车 叫到了";
    });

    SmallTool.print(String.format("%s,小白坐车回家", bus.join()));
}
```

## 阻塞队列

先进先出（FIFO）的数据结构，与普通队列不同的是，他支持两个附加操作，即阻塞添加和阻塞删除方法。

- 阻塞添加：当阻塞队列是满时，往队列里添加元素的操作将被阻塞。
- 阻塞删除：当阻塞队列是空时，从队列中获取元素/删除元素的操作将被阻塞。

在多线程中，阻塞的意思是，在某些情况下会 挂起线程，一旦条件成熟，被阻塞的线程就会被自动唤醒。

好处：

- 阻塞队列不用手动控制什么时候该被阻塞，什么时候该被唤醒，简化了操作。

### BlockingQueue

根据插入和取出两种类型的操作，具体分为下面一些类型：

| 方法类型 | 抛出异常  |  返回布尔  |   阻塞   |            超时            |
| :------: | :-------: | :--------: | :------: | :------------------------: |
|   插入   | add(E e)  | offer(E e) | put(E e) | offer(E e, Time, TimeUnit) |
|   取出   | remove()  |   poll()   |  take()  |    poll(Time, TimeUnit)    |
|   对首   | element() |   peek()   |    无    |             无             |

- 抛出异常是指当队列满时，再次插入会抛出异常（如果队列未满，插入返回值为true）。
- 返回布尔是指当队列满时，再次插入会返回false。
- 阻塞是指当队列满时，再次插入会被阻塞，直到队列取出一个元素，才能插入。
- 超市是指当一个时限过后，才会插入或者取出。

> 生产
>
> add、offer、put这三个方法都是往队列尾部添加元素，区别如下：
>
> - add：不会阻塞，添加成功时返回true，不响应中断，当队列已满导致添加失败时抛出IllegalStateException。
> - offer：不会阻塞，添加成功时返回true，因队列已满导致添加失败时返回false，不响应中断。
> - put：会阻塞，会响应中断。
>
> 消费
>
> take、poll方法能获取队列头部第一个元素，区别如下：
>
> - take：会响应中断，会一直阻塞直到取得元素或当前线程中断。
> - poll：会响应中断，会阻塞，阻塞时间参照方法里参数timeout.timeUnit，当阻塞时间到了还没取得元素会返回null。

### ArrayBlockingQueue

- 数据结构：静态数组，容量固定必须指定长度，没有扩容机制，没有元素的下标位置null占位。

- 锁：ReentrantLock 存取时同一把锁，操作的是同一个数组对象。

- 阻塞：

    - notEmpty，出队：队列count为0，无元素可取时，阻塞在该对象上。

    - notFull，入队：队列count为数组的length，放不进元素时，阻塞在该对象上。

- 入队：从对首开始添加元素，记录putIndex（到队尾时置为0），唤醒notEmpty。
- 出队：从对首开始取元素，记录takeIndex，唤醒notFull。
- 先进先出，读写互相排斥。

由**数组**构成的**有界**阻塞队列，通过**ReentrantLock**和**Condition**条件队列来实现阻塞，一些成员变量如下：

```java
    //存储数据
    final Object[] items;

    //返回获取数据的索引，主要用于take、poll、peek、remove方法
    int takeIndex;

    //返回添加数据的索引，主要用于 put、offer、add 方法
    int putIndex;

    // 队列元素的个数
    int count;

    //可重入锁
    final ReentrantLock lock;

    //条件对象，用于通知take方法队列的线程
    private final Condition notEmpty;

    //条件对象，用于通知put方法队列的线程
    private final Condition notFull;

	//迭代器
    transient Itrs itrs = null;


```

**添加元素原理**

添加方法有add，offer，put。

```java
//add方法
public boolean add(E e) {
    if (offer(e))
        return true;
    else
        throw new IllegalStateException("Queue full");
}

//offer方法
public boolean offer(E e) {
    //判断是否为null
     checkNotNull(e);
     final ReentrantLock lock = this.lock;
     lock.lock();
     try {
         //判断队列是否满
         if (count == items.length)
             return false;
         else {
             //添加元素到队列
             enqueue(e);
             return true;
         }
     } finally {
         lock.unlock();
     }
 }

//元素入队操作
private void enqueue(E x) {
    //获取当前数组
    final Object[] items = this.items;
    //通过putIndex索引对数组进行赋值
    items[putIndex] = x;
    //索引自增，如果已是最后一个位置，重新设置 putIndex = 0;
    if (++putIndex == items.length)
        putIndex = 0;
    //队列中元素数量加1
    count++;
    //唤醒调用take()方法的线程，执行元素获取操作。
    notEmpty.signal();
}
```

add方法本质调用的是offer方法，而在offer的最关键处，也就是enqueue入队操作。

1. reentrantLock保证的线程的互斥性，即统一时间只能有一个线程操作。如果队列已满，返回true，add方法则是抛出异常；如果队列未满，则开始入队操作；
2. 在入队操作时，他会通过一个全局变量putIndex作为索引，指引着新来元素的位置。在这里有个小细节，就是判断putIndex是否与队列长度相等，如果队列已满，而且队列的操作时先进先出，索引下一次来插入元素的位置肯定是对头，也就是索引0的位置；
3. 队内已经有元素了，然后开始唤醒take操作来消费元素。signal() 其实是 notify() 的升级版。

在添加的操作中，put方法，他是会导致线程阻塞的。

```java
//put方法，阻塞时可中断
public void put(E e) throws InterruptedException {
 checkNotNull(e);
  final ReentrantLock lock = this.lock;
  lock.lockInterruptibly();//该方法可中断
  try {
      //当队列元素个数与数组长度相等时，无法添加元素
      while (count == items.length)
          //将当前调用线程挂起，添加到notFull条件队列中等待唤醒
          notFull.await();
      enqueue(e);//如果队列没有满直接添加。。
  } finally {
      lock.unlock();
  }
}
```

他是通过condition的await方法来实现阻塞的，但由于又添加了lockInterruptibly标识，说明其阻塞可被打断。

**获取元素/删除元素原理**

方法有remove，poll，take。

```java
public E poll() {
    //reentrantLock互斥锁
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        //如果队列为0,返回null，反之进入移除队列
        return (count == 0) ? null : dequeue();
    } finally {
        lock.unlock();
    }
}
//移除队列
private E dequeue() {
    //获取当前队列数据
    final Object[] items = this.items;
    @SuppressWarnings("unchecked")
    //获取当前队头数据
    E x = (E) items[takeIndex];
    //将队头数据置为null
    items[takeIndex] = null;
    //如果队头索引自增与数组长度相等，则将其索引设置为第一位
    if (++takeIndex == items.length)
        takeIndex = 0;
    count--;
    if (itrs != null)
        //更新迭代器中的元素数据
        itrs.elementDequeued();
    //唤醒put/offer/add等方法
    notFull.signal();
    return x;
}
```

poll方法是通过删除队头数据来进行移除元素，唤醒与沉睡机制采用reentrantLock 的 condition 机制。

```java
public E take() throws InterruptedException {
    final ReentrantLock lock = this.lock;
      lock.lockInterruptibly();//中断
      try {
          //队列没有元素，阻塞移除方法的线程
          while (count == 0)
              notEmpty.await();
          //有元素进行元素移除操作
          return dequeue();
      } finally {
          lock.unlock();
      }
}
```

take方法跟 poll方法一样，也是通过dequeue() 方法进行移除元素，但不同的是，他会进行一个线程阻塞，也就是运用condition的 awati()方法，同时这个阻塞是可被打断的，关键词lockInterruptibly。

remove() 方法相对来说比较复杂，他跟以上两个方法的不同点在于remove可以根据索引来删除元素，而另两个则是通过删除队列的头元素。

```java
public boolean remove(Object o) {
    //确保传入元素不为null
    if (o == null) return false;
    //获取队列当前数据
    final Object[] items = this.items;
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        if (count > 0) {
            final int putIndex = this.putIndex;
            int i = takeIndex;
            //找出O元素的索引值
            do {
                if (o.equals(items[i])) {//如果匹配到，删除元素，i为o的索引
                    removeAt(i);
                    return true;
                }
                //只有一个元素时，重置索引值
                if (++i == items.length)
                    i = 0;
            } while (i != putIndex);
        }
        return false;
    } finally {
        lock.unlock();
    }
}
```

removeAt() 方法。

```java
void removeAt(final int removeIndex) {
    final Object[] items = this.items;
    //判断当前元素是否是头部索引值
    if (removeIndex == takeIndex) {
        items[takeIndex] = null;
        if (++takeIndex == items.length)
            takeIndex = 0;
        count--;
        if (itrs != null)
            itrs.elementDequeued();
    } else {
       //如果不是，通过移动元素位置，将要删除的元素置为队尾删除
        final int putIndex = this.putIndex;
        for (int i = removeIndex;;) {
            //确保当前队列大小大于1
            int next = i + 1;
            if (next == items.length)
                next = 0;
            //如果不是队尾元素，进行元素移动
            if (next != putIndex) {
                items[i] = items[next];
                i = next;
            } else {
                //如果是队尾，元素移动完毕，直接将队尾为null，即删除
                items[i] = null;
                this.putIndex = i;
                break;
            }
        }
        count--;
        if (itrs != null)
            itrs.removedAt(removeIndex);
    }
    notFull.signal();
}
```

### LinkedBlockingQueue

由**链表**构成的**有界**阻塞队列，需要注意的是虽然是有界的，但是其默认大小是Integer.MAX_VALUE，高达21亿，一般情况下内存早爆了（在线程池中ThreadPoolExecutor有体现）。

- 数据结构：链表Node，可以指定容量，默认为Integer.MAX_VALUE，内部类Node存储元素。

- 锁分离：存取互不排斥，操作的是不同的Node对象。

    - takeLock：取Node节点保证前驱后继不会乱。

    - putLock：存Node节点保证前驱后继不会乱。

- 阻塞：

    - notEmpty，出队：队列count为0，无元素可取时，阻塞在该对象上。

    - notFull，入队：队列count为数组的length，放不进元素时，阻塞在该对象上。

- 入队：队尾入队，记录last节点。
- 出队：队首出队，记录head节点。
- 删除元素时两个锁一起加。
- 先进先出。

### PriorityBlockingQueue

支持优先级排序的无界阻塞队列。

- 数据结构：数组 + 平衡二叉树堆，可以指定初始容量，会自动扩容，最大容量为Integer.MAX_VALUE。
- 锁：ReenLock存取东一把锁。
- 阻塞：notEmpty，出队，队列为空是阻塞。
- 入队：
    - 不阻塞，永远返回成功，无界。
    - 根绝比较器进行堆化，根据二叉堆进行排序，自下而上。
- 出队：
    - 弹出堆顶元素。
    - 堆尾元素放到堆顶。
    - 自上而下堆化。
    - 为空则阻塞。

### DelayQueue

支持优先级的延迟无界阻塞队列。

### SynchronousQueue

单个元素的阻塞队列，队列中只有一个元素，如果想插入多个，必须等队列元素取出后，才能插入，只能有一个“坑位”，用一个插一个。

- 存取调用同一个方法：transfer。

    - put、offer为生产者，携带数据e，为Data模式，设置到QNode属性中。

    - take、poll为消费者，不携带数据，为Request模式，设置到QNode属性中。

- 数据结构：链表Node。

- 锁：cas + 自旋。

- 阻塞：LockSupport。

- 判断队尾节点或者栈顶节点Node与入队模式是否相同：

    - 相同则构造节点Node入队，并阻塞当前线程，元素e和相乘赋值给Node属性。

    - 不同则将元素e（部位null）返回给取数据线程，配对线程被唤醒，出队。

- 公平模式：TransferQueue，队尾匹配，队头出队，先进先出。
- 非公平模式：TransferStack，栈顶匹配，栈顶出栈，后进先出。

### LinkedTransferQueue

由链表构成的无界阻塞队列。

- 数据结构：链表Node。
- 锁：cas + 自旋。
- 阻塞：LockSupport。
- 可以自己控制放元素需要阻塞线程，比如使用四个添加元素的方法就不会阻塞线程，只入队元素，使用transfer()会阻塞线程。
- 取元素与SynchronousQueue基本一样，都会阻塞等待有新的元素进入被匹配到。

### LinkedBlockingDeque

由链表构成的双向阻塞队列。

### LinkedBlockingQueue和ArrayBlockingQueue区别

1. 队列大小不同：

    - ArrayBlockingQueue在初始化的时候，必须指定队列的大小。
    - LinkedBlockingQueue在初始化的时候，如果没有指定大小，则会默认Integer.MAX_VALUE，是一个很大的值，这样就会导致数据再一个不可控的范围，一旦添加速度大于移除的速度时，可能会有内存泄露的风险。

2. 底层实现不同：
    ArrayBlockingQueue的底层是一个数组，而LinkedBlockingQueue底层是一个链表结构。官方文档介绍中，LinkedBlockingQueue的吞吐性是高于ArrayBlockingQueue；但是在添加或移除元素，LinkedBlockingQueue则会生成一个额外的Node对象，对GC可能存在影响。
    
      - > 至于为什么说LinkedBlockingQueue的吞吐性高于ArrayBlockingQueue：
        >
      > 吞吐性强是因为有两个锁，Array里面使用的是一个锁，不管put还是take行为，都可能被这个锁卡住，而Linked里put和take是两个锁，put只会被put行为卡住，而不会被take卡住，因此吞吐性能自然强于Array。而“less
      > predictable performance” 这个也是显而易见的，Array采用的是固定内存，而Linked采用的是动态内存，无论是分配内存还是释放内存（甚至GC）动态内存的性能自然都会比固定内存要差。
    
3. 锁机制不一样：

   ArrayBlockingQueue使用的一个锁控制，LinkedBlockingQueue使用两个锁控制，一个putLock，另一个takeLock，但是锁的本质都是ReentrantLock。

LinkedBlockingQueue是一个基于链表实现的阻塞queue，它的性能ArrayBlockingQueue，但是差于ConcurrentLinkedQueue；并且它非常适于生产者消费者的环境中，比Executors.newFixedThreadPool()。
就是基于这个队列的，使用LinkedBlockingQueue时一定要设置容量，不然会有内存溢出的风险。

## Lock

### ReentrantLock

实现加锁 lock() ----> 线程阻塞：

- wait() 可以阻塞线程，但是必须要结合synchronized 一起使用。
- sleep() 可以阻塞线程，但是必须指定时间，只能通过中断方式唤醒。
- park() 可以阻塞线程，unpark() 进行唤醒。
- while(true) {} 可以阻塞线程，cas。

**公平锁**

```java
static final class FairSync extends Sync {
    private static final long serialVersionUID = -3000897897090466540L;

    /**
     * Fair version of tryAcquire.  Don't grant access unless
     * recursive call or no waiters or is first.
     */
    @ReservedStackAccess private boolean tryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        int c = getState();
        // state 0 加锁
        if (c == 0) {
            // hasQueuedPredecessors() 被设计为用于公平的同步器，以避免碰撞
            // 如果当前线程之前有一个排队线程，则为true，如果当前线程位于队列的头部或者队列为空，则为false
            // compareAndSetState(0, acquires) cas
            if (!hasQueuedPredecessors() &&
                    compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(current);
                return true;
            }
        }
        // 判断加锁线程是否为当前线程 state+1 重入
        else if (current == getExclusiveOwnerThread()) {
            int nextc = c + acquires;
            if (nextc < 0)
                throw new Error("Maximum lock count exceeded");
            setState(nextc);
            return true;
        }
        // 加锁失败
        return false;
    }
}
```

```java
public final boolean hasQueuedPredecessors(){
        Node h,s;
        if((h=head)!=null){
        // (s = h.next) == null 防止空指针的标准写法，除了防止空指针，还会将当前节点复制到h变量上
        if((s=h.next)==null||s.waitStatus>0){
        s=null; // traverse in case of concurrent cancellation
        for(Node p=tail;p!=h&&p!=null;p=p.prev){
        if(p.waitStatus<=0)
        s=p;
        }
        }
        if(s!=null&&s.thread!=Thread.currentThread())
        return true;
        }
        return false;
        }
```

**非公平锁**

```java
static final class NonfairSync extends Sync {
  private static final long serialVersionUID = 7316153563782823691L;

  private boolean tryAcquire(int acquires) {
    return nonfairTryAcquire(acquires);
  }
}
```

```java
final boolean nonfairTryAcquire(int acquires){
final Thread current=Thread.currentThread();
        int c=getState();
        if(c==0){
        if(compareAndSetState(0,acquires)){
        setExclusiveOwnerThread(current);
        return true;
        }
        }
        else if(current==getExclusiveOwnerThread()){
        int nextc=c+acquires;
        if(nextc< 0) // overflow
        throw new Error("Maximum lock count exceeded");
        setState(nextc);
        return true;
        }
        return false;
        }
```

### 死锁

**形成死锁的四个必要条件**

1. **互斥条件**：进程要求对所分配的资源（如打印机）进行排他性控制，即在一段时间内莫资源仅为一个进程所占有。此时若有其他进程请求资源，则请求进程只能等待。
2. **不可剥夺条件**：进程所获得的资源在未使用完毕之前，不能被其他进程强行夺走，即只能由获得该资源的进程自己来释放（只能是主动释放）。
3. **请求与保持条件**：进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源已被其他进程占有，此时请求进程被阻塞，但对自己获得的资源保持不放。
4. **循环等待条件**：存在一种进程资源的循环等待链，链中每一个进程已获取的资源同时被链中下一个进程所请求。即存在一个处于等待状态的进程集合
   {P1,P2,...Pn}，其中Pi等待的资源被 P(i+1)占有（i=0,1,...n-1）,Pn等待的资源被P0占有。

以上四个条件是死锁的必要条件，只要系统产生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。

**处理死锁的方法**

1. **预防死锁**

   通过设置某些限制条件，去破坏产生死锁的四个必要条件中的一个或几个条件，来防止死锁的产生。
   
     - 破坏“互斥”条件
   
       就是在系统里取消互斥。若资源不被一个进程独占使用，那么死锁是肯定不会发生的。但一般来说在所列的四个条件中，“互斥”条件是无法破坏的。
   
   
     - 破坏“占有并等待”条件
   
       破坏“占有并等待”条件，就是在系统中不允许进程在已获得某种资源的情况下，申请其他资源。即要想出一个办法，阻止进程在持有资源的同时申请其他资源：
   
       1. 一次申请所需的全部资源，即“一次性分配”。
       2. 要求每个进程提出新的资源申请前，释放它所占有的资源，即“先释放后申请”。
   
   
     - 破坏“不可抢占”条件
   
       破坏“不可抢占”条件就是允许对资源实行抢夺：
   
       1. 占有某些资源的同时再请求被拒绝，则该进程必须释放已占有的资源，如果有必要，可再次请求这些资源和另外的资源。
       2. 设置进程优先级，优先级高的可以抢占资源。
   
   
     - 破坏“循环等待”条件
   
       将系统中的所有资源统一编号，所有进程必须按照资源编号顺序提出申请。
   
2. **避免死锁**

   在资源的动态分配过程中，用某种方法去防止系统进入不安全状态，从而避免死锁的产生。
   
     - 加锁顺序：线程按照一定的顺序加锁。
   
     - 加锁时限：线程尝试获取锁的时候加上一定的时限，超过时限则放弃对该锁的请求，并释放自己占有的锁。
   
     - 死锁检测：每当一个线程获得了锁，会在线程和锁相关的数据结构中（map、graph等等）将其记下。除此之外，每当有线程请求锁，也需要记录在这个数据结构中。
   
3. **检测死锁**

   允许系统在运行过程中产生死锁，但可设置检测机构及时检测死锁的发生，并采取适当措施加以清除。

   一般来说，由于操作系统有并发，共享以及随机性等特点，通过预防和避免的手段达到排除死锁的目的是很困难的。这需要较大的系统开销，而且不能充分利用资源。为此，一种简便的方法是系统为进程分配资源时，不采取任何限制性措施，但是提供了检测和解脱死锁的手段：能发现死锁并从死锁状态中恢复出来。因此，在实际的操作系统中往往采用死锁的检测与恢复方法来排除死锁。

4. **解除死锁**

   当检测出死锁后，便采取适当措施将进程从死锁状态中解脱出来。
   
     - 资源剥夺法
   
       挂起某些死锁进程，并抢占它的资源，将这些资源分配给其他的死锁进程。但应防止被挂起的进程长时间得不到资源，而处于资源匮乏的状态。
   
   
     - 撤销进程法
   
       强制撤销部分、甚至全部死锁进程并剥夺这些进程的资源。撤销的原则可以按进程优先级和撤销进程代价的高低进行。
   
   
     - 进程回退法
   
       让一（多）个进程回退到足以回避死锁的地步，进程回退时自愿释放资源而不是被剥夺。要求系统保持进程的历史信息，设置还原点。
   

## ThreadPoolExecutor

### 线程池介绍

![image-20230411174505779](https://github.com/chou401/pic-md/raw/master/img/image-20230411174505779.png)

```java
// 是个int类型的数值 表达了两个意思 1：声明当前线程池的状态 2：声明线程池中的线程数
// 高3位是线程池状态 低29位是线程池中线程个数
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
// 29 方便后面的位运算
private static final int COUNT_BITS = Integer.SIZE - 3;
// 通过位运算得出最大容量
private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;

// runState is stored in the high-order bits
// 线程池状态
private static final int RUNNING    = -1 << COUNT_BITS; // 111 代表线程池为RUNNING，代表正常接收任务
private static final int SHUTDOWN   =  0 << COUNT_BITS; // 000 代表线程池为SHUTDOWN，不接收新任务，但是内部还会处理阻塞队列中的任务，正在进行的任务也会正常处理
private static final int STOP       =  1 << COUNT_BITS; // 001 代表线程池为STOP，不接收新任务，也不会处理阻塞队列中的任务，同时会中断正在进行的任务
private static final int TIDYING    =  2 << COUNT_BITS; // 010 代表线程池为TIDYING，过渡的状态，代表当前线程池即将 Game Over
private static final int TERMINATED =  3 << COUNT_BITS; // 011 代表线程池TERMINATED，代表当前线程池已经 Game Over，要执行terminated()

// Packing and unpacking ctl
private static int runStateOf(int c)     { return c & ~COUNT_MASK; } // 得到线程池的状态
private static int workerCountOf(int c)  { return c & COUNT_MASK; } // 得到当前线程池的线程数量
private static int ctlOf(int rs, int wc) { return rs | wc; } // 得到上面提到的 32 位 int类型的数值
```

### 线程池状态变化

![image-20230411173800910](https://github.com/chou401/pic-md/raw/master/img/image-20230411173800910.png)

```java
public void execute(Runnable command) {
    // 代码健壮性判断
    if (command == null)
        throw new NullPointerException();
    /*
     * Proceed in 3 steps:
     *
     * 1. If fewer than corePoolSize threads are running, try to
     * start a new thread with the given command as its first
     * task.  The call to addWorker atomically checks runState and
     * workerCount, and so prevents false alarms that would add
     * threads when it shouldn't, by returning false.
     *
     * 2. If a task can be successfully queued, then we still need
     * to double-check whether we should have added a thread
     * (because existing ones died since last checking) or that
     * the pool shut down since entry into this method. So we
     * recheck state and if necessary roll back the enqueuing if
     * stopped, or start a new thread if there are none.
     *
     * 3. If we cannot queue task, then we try to add a new
     * thread.  If it fails, we know we are shut down or saturated
     * and so reject the task.
     */
    
    // 拿到32 位的int
    int c = ctl.get();
    // 获取 工作线程数 < 核心线程数
    if (workerCountOf(c) < corePoolSize) {
        // 创建核心线程数
        if (addWorker(command, true))
            return;
        // 创建核心线程数失败，重新获取 ctl
        c = ctl.get();
    }
    // 判断线程池是不是 RUNNING 将任务添加到阻塞队列中
    if (isRunning(c) && workQueue.offer(command)) {
        int recheck = ctl.get();
        // 再次判断是否 RUNNING 如果不是RUNNING 移除任务
        if (! isRunning(recheck) && remove(command))
            // 拒绝策略
            reject(command);
        // 如果线程池 处在RUNNING 而且工作线程为0 
        else if (workerCountOf(recheck) == 0)
            // 阻塞队列中有任务 但是没有工作线程 添加一个任务为空的工作线程处理阻塞队列中的任务
            addWorker(null, false);
    }
    // 创建非核心线程 进行处理任务
    else if (!addWorker(command, false))
        // 拒绝策略
        reject(command);
}
```

```java
private boolean addWorker(Runnable firstTask, boolean core) {
    retry:
    for (int c = ctl.get();;) {
        // Check if queue empty only if necessary.
        if (runStateAtLeast(c, SHUTDOWN) // 除了 RUNNING 都有可能
            && (runStateAtLeast(c, STOP) // 只可能是STOP或者更高的状态
                || firstTask != null // 任务不为空
                || workQueue.isEmpty())) // 阻塞队列为空
            // 构建工作线程失败
            return false;

        for (;;) {
            if (workerCountOf(c) // 获取工作线程个数
                >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK)) // 判断是否超过核心线程或者最大线程
                return false;
            // 将工作线程数+1 采用cas的方式
            if (compareAndIncrementWorkerCount(c))
                break retry;
            c = ctl.get();  // Re-read ctl
            // 重新判断线程池的状态
            if (runStateAtLeast(c, SHUTDOWN))
                continue retry;
            // else CAS failed due to workerCount change; retry inner loop
        }
    }

    boolean workerStarted = false;
    boolean workerAdded = false;
    Worker w = null;
    try {
        // 创建Worker 传入任务
        w = new Worker(firstTask);
        // 从Worker中获取线程 t
        final Thread t = w.thread;
        if (t != null) {
            // 获取线程池的全局锁 避免线程添加任务时，其他线程干掉了线程池 干掉线程池需要先获取这个锁
            final ReentrantLock mainLock = this.mainLock;
            // 加锁
            mainLock.lock();
            try {
                // Recheck while holding lock.
                // Back out on ThreadFactory failure or if
                // shut down before lock acquired.
                int c = ctl.get();

                if (isRunning(c) ||
                    (runStateLessThan(c, STOP) && firstTask == null)) {
                    // RUNNING 或者 是STOP之后的状态 创建空任务工作线程 用来处理阻塞队列中的任务
                    if (t.getState() != Thread.State.NEW)
                        throw new IllegalThreadStateException();
                    
                    // 将工作线程添加到集合
                    workers.add(w);
                    // workerAdded为true 添加工作线程成功
                    workerAdded = true;
                    // 获取工作线程个数
                    int s = workers.size();
                    // 如果工作线程个数大于之前记录的最大工作线程数 就替换一下
                    if (s > largestPoolSize)
                        largestPoolSize = s;
                }
            } finally {
                mainLock.unlock();
            }
            if (workerAdded) {
                // 启动工作线程
                t.start();
                // 启动工作线程成功
                workerStarted = true;
            }
        }
    } finally {
        // 启动工作线程失败
        if (! workerStarted)
            addWorkerFailed(w);
    }
    return workerStarted;
}
```

```java
// 如果可以在不超过队列容量的情况下立即插入指定的元素，则在该队列的尾部插入该元素；如果成功，则返回true；如果该队列已满，则返回false
public boolean offer(E e) {
    // 不允许元素为空
    Objects.requireNonNull(e);
    final ReentrantLock lock = this.lock;
    lock.lock(); // 加锁 保证调用 offer 方法的时候只有一个线程
    try {
        // 队列满
        if (count == items.length)
            return false;
        else {
            // 队列未满
            enqueue(e);
            return true;
        }
    } finally {
        // 释放锁 让其他线程可以调用 offer 方法
        lock.unlock();
    }
}
```

```java
// 在当前放置位置插入元素、前进和发出信号。只有在锁定的情况下才能呼叫。
private void enqueue(E e) {
    final Object[] items = this.items;
    // 元素添加到数组中
    items[putIndex] = e;
    // 当索引满了 修改为0
    if (++putIndex == items.length) putIndex = 0;
    count++;
    // 使用条件对象 notEmpty 通知 比如使用 take 方法的时候队列中没有数据 被阻塞 这个时候队列中插入了数据 需要调用 signal() 进行通知
    notEmpty.signal();
}
```

### Woker

```java
private final class Worker
        extends AbstractQueuedSynchronizer
        implements Runnable {
  /**
   * This class will never be serialized, but we provide a
   * serialVersionUID to suppress a javac warning.
   */
  private static final long serialVersionUID = 6138294804551838833L;

  /** Thread this worker is running in.  Null if factory fails. */
  final Thread thread;
  /** Initial task to run.  Possibly null. */
  Runnable firstTask;
  /** Per-thread task counter */
  volatile long completedTasks;

  // TODO: switch to AbstractQueuedLongSynchronizer and move
  // completedTasks into the lock word.

  /**
   * Creates with given first task and thread from ThreadFactory.
   * @param firstTask the first task (null if none)
   */
  Worker(Runnable firstTask) {
    setState(-1); // inhibit interrupts until runWorker
    this.firstTask = firstTask;
    // 创建 worker 线程 
    this.thread = getThreadFactory().newThread(this);
  }

  /** Delegates main run loop to outer runWorker. */
  public void run() {
    runWorker(this);
  }

  // Lock methods
  //
  // The value 0 represents the unlocked state.
  // The value 1 represents the locked state.

  private boolean isHeldExclusively() {
    return getState() != 0;
  }

  private boolean tryAcquire(int unused) {
    if (compareAndSetState(0, 1)) {
      setExclusiveOwnerThread(Thread.currentThread());
      return true;
    }
    return false;
  }

  private boolean tryRelease(int unused) {
    setExclusiveOwnerThread(null);
    setState(0);
    return true;
  }

  public void lock() {
    acquire(1);
  }

  public boolean tryLock() {
    return tryAcquire(1);
  }

  public void unlock() {
    release(1);
  }

  public boolean isLocked() {
    return isHeldExclusively();
  }

  void interruptIfStarted() {
    Thread t;
    if (getState() >= 0 && (t = thread) != null && !t.isInterrupted()) {
      try {
        t.interrupt();
      } catch (SecurityException ignore) {
      }
    }
  }
}
```

```java
final void runWorker(Worker w) {
    // 获取当前线程
    Thread wt = Thread.currentThread();
    // 获取任务
    Runnable task = w.firstTask;
    w.firstTask = null;
    w.unlock(); // allow interrupts
    boolean completedAbruptly = true;
    try {
        // 任务不为空 执行任务  如果任务为空 通过getTask()从阻塞队列中获取任务
        while (task != null || (task = getTask()) != null) {
            // 加锁 避免被 SHUTDOWN 任务也不会中断
            w.lock();
            //如果线程池状态大于等于STOP，请确保线程被中断；如果没有，请确保线程没有中断。这需要在第二种情况下重新检查，以处理关闭在清除中断时无竞争
            if ((runStateAtLeast(ctl.get(), STOP) ||
                 (Thread.interrupted() &&
                  runStateAtLeast(ctl.get(), STOP))) &&
                !wt.isInterrupted())
                // 中断
                wt.interrupt();
            try {
                // 执行任务前的操作
                beforeExecute(wt, task);
                try {
                    task.run();
                    afterExecute(task, null);
                } catch (Throwable ex) {
                    // 执行任务后的操作
                    afterExecute(task, ex);
                    throw ex;
                }
            } finally {
                task = null;
                w.completedTasks++;
                w.unlock();
            }
        }
        completedAbruptly = false;
    } finally {
        processWorkerExit(w, completedAbruptly);
    }
}
```

## GC

### 如何判断一个对象是否存活

#### 引用计数法

引用计数，就是记录每个对象被引用的次数，每次新建对象、赋值引用和删除引用的同时更新计数器，如果计数器值为0则直接回收内存。很明显，引用计数最大的优势就是暂停时间短。

优点：

  - 可即刻回收垃圾。

  - 最大暂停时间短。

  - 没有必要沿指针查找。

缺点：

  - 计数器的增减处理繁重。

  - 计数器需要占用很多位。

  - 实现繁琐复杂，每个赋值操作都得替换成引用更新操作。

  - 循环引用无法回收（**最大的缺点**）。

#### 可达性分析法

从一个被称为 GC Roots 的对象向下搜索，如果一个对象到 GC Roots 没有任何引用链相连接时，说明此对象不可用，在java中可以作为
GC Roots 的对象有以下几种：

  - 虚拟机栈中引用的对象。

  - 方法区类静态属性引用的变量。

  - 方法区常量池引用的对象。

  - 本地方法栈 JNI 引用的对象。

### 垃圾回收算法

#### 标记-清除（Mark-Sweep）

- 标记阶段：从跟集合出发，将所有活动对象及其子对象打上标记。

- 清除阶段：遍历堆，将非活动对象（未打上标记）的连接到空闲链表上。

缺点：

  - 碎片化，会导致吴舒小分块散落在堆的各处。

  - 分配速度不理想，每次分配都需要遍历空闲列表找到足够大的分块。

  - 与写时复制技术不兼容，因为每次都要在活动对象上打上标记。

#### 拷贝（复制）（Copying）

为了解决碎片化的问题。原理是将内存分为两块，每次申请内存时都使用其中一块，当内存不够时，将这一块内存中所有存活的复制到另一块上，然后再把已使用的内存整个清理掉。复制算法解决了空间碎片的问题，但是，因为每次在申请内存时，都只能使用一半的内存空间，内存利用率严重不足。

JVM 中新生代采用的就是复制算法。针对内存利用率不足做了一下优化：

- IBM公司的专门研究表明，新生代中的对象 98% 是“朝生夕死”的，意思是说，在新生代中，经过一次 GC 之后能够存活下来的对象仅有 2%左右。所以并不需要按照1:1的比例划分出两块内存空间。而是将内存划分出三块，一块较大的 Eden 区，和两块较小的 Survivor 区。其中Eden 区占 80% 的内存，两块 Survivor 各占 10% 的内存。在创建新的对象时，只使用 Eden 区和其中的一块 Survivor 区，当进行 GC时，把 Eden 区和 Survivor 区存活的对象全部复制到另一块 Survivor 区中，然后清理掉 Eden 区和刚刚用过的 Survivor 区。

  这种内存的划分方式就解决了内存利用率的问题，每次在创建对象时，可用的内存为 90%(80% + 10%) 当前内存容量。

优点：

  - 优秀的吞吐量，只需要关心活动对象。

  - 可实现高速分配，因为分块是连续的，不需要使用空闲链表。

  - 不会发生碎片化。

  - 与缓存兼容。

缺点：

  - 堆利用率低。

  - 递归调用函数，负质子对象需要递归调用复制函数，消耗栈。

#### 标记-压缩/整理（Mark-Compact）

复制算法在 GC 之后存活对象较少的情况下效率比较高，但如果存活对象比较多时，会执行较多的复制操作，效率就会下降。而老年代的对象在 GC 之后的存活率就比较高，所以就有人提出了“标记-整理算法”。

“标记-整理”算法的“标记”过程与“标记-清除”算法的标记过程一致，但标记之后不会直接清理，而是将所有存活对象都移动到内存的一端，移动结束后直接清理掉剩余部分。

优点：

  - 有效利用了堆，不会出现内存碎片，也不会像复制算法那样只能利用堆的一部分。

缺点：

  - 压缩过程的开销，需要多次搜索堆。

#### 分代

**出发点**：大部分对象生成后马上就变成垃圾，很少有对象能活很久。

  - 新生代 = 生成空间 + 2 * 幸存区 **复制算法**

  - 老年代 **标记-清除算法**

对象在生成空间创建，当生成空间满之后进行 minor gc，将活动对象复制到第一个幸存区，并增加其“年龄”age，当这个幸存区满之后再将此次生成空间和这个幸存区的活动对象复制到另一个幸存区，如此反复，当活动对象的 age 达到一定次数（默认是15，可以通过参数 ==-XX:MaxTenuringThreshold== 来设定）后将其移动到老年代； 当老年代满的时候就用标记-清除或标记-压缩算法进行major gc 吞吐量得到改善，分代回收花费的时间是 GC 复制算法的四分之一；但是如果部分程序新生成对象存活很久的话分代回收会适得其反。

**具体流程**

  1. 对象优先在Eden分配。当 eden 区没有足够空间进行分配时，虚拟机将发起一次 Minor GC。
  1. 在 Eden 区执行了第一次 GC 之后，存活的对象会被移动到 form 分区。
  1. Eden 区再次 GC，这时会采用复制算法，将 Eden 和 from 区一起清理，存活的对象会被复制到 to 区。
  1. 当后续Eden又发生Minor GC的时候，会对Eden和 to 区进行垃圾回收，存活的对象复制到 from 区，并将Eden 和 to 区清空。
  1. 部分对象会在 from 和 to 区中来回的复制，如此的交换15次（由JVM参数 Max Tenuring Threshold 决定，默认是15），最终如果还是存活，就存入老年代。
  1. Survivor 区内存不足会发生**担保分配**，超过指定大小的对象可以直接进入老年代(此时如果老年代的内存大小，小于对象的大小，可能会发生一次Full GC)。
  1. 老年代满了而**无法容纳更多的对象**，Minor GC 之后通常就会进行Full GC，Full GC 清理整个内存堆 – **包括年轻代和老年代**。

**注意事项**

- **新生代**：对于一般创建的对象都会进入。

- **老年代**：对于大对象为了避免为大对象分配内存时由于分配担保机制带来的复制而降低效率，或者经过N次（一般默认15次）的垃圾回收依然存活下来的对象，从新生代移动到老年代。

- **Minor GC、Major GC、Full GC的关系**
         1. Minor GC 又称为新生代 GC：指的是发生在新生代的垃圾回收。因为Java对象大多都具备朝生夕灭的特性，因此Minor GC（采用的是复制算法）非常频繁，一般回收速度也比较快。
         2. Major GC 又被称为老年代 GC ，一般的老年代 GC 总是由于每次 Minor GC 引起的，所以Major GC 发生的时候也是 Full GC，可以看作他两个等效。


- **空间分配担保原则**

  如果 Young GC 时新生代有大量对象存活下来，而 survivor 区放不下，这时必须转移到老年代中，但这时发现老年代也放不下这些对象了，那怎么处理？其实JVM有一个老年代空间分配担保机制来保证对象能够进入老年代。

  在执行每次 Young GC 之前，JVM 会先检查老年代最大可用连续空间是否大于新生代所有对象的总大小，因为在极端情况下，可能新生代 Young GC 后，所有对象都存活下来了，而 survivor 区又放不下，那可能所有对象都要进入老年代了。这个时候如果老年代的可用连续空间大于新生代所有对象的总大小的，那就可以放心进行 Young GC，但是如果老年代的内存大小小于新生代对象总大小的，那就可能老年代空间不够放入新生代所有存活对象，这个时候 JVM就会先检查 ==-XX:HandlePromotionFailure== 参数是否允许担保失败，如果允许，就会判断老年代最大可用连续空间是否大于历次晋升到老年代对象的平均大小（就是每次从新生代到老年代的对象的平均大小），如果大于，将尝试进行一次Young GC ，尽管这次 Young GC 是有风险的，如果小于，或者 ==-XX:HandlePromotionFailure== 参数不允许担保失败，这时就要进行一次Full GC。

- **在允许担保失败并尝试进行 Young GC 后，可能会出现三种情况**
    1. Young GC 后，存活对象小于 survivor 大小，此时存活对象进入 survivor 区中。
    1. Young GC 后，存活对象大于 survivor 大小，但是小于老年代可用空间大小，此时直接进入老年代。
    1. Young GC 后，存活对象大于 survivor 大小，也大于老年代可用空间大小，老年代也放不下这些对象了，此时就会发生“Handle Promotion Failure”，就触发了 Full GC。如果 Full GC 后，老年代还是没有足够的空间，此时就会发生 OOM 内存溢出了。

### 三色标记算法

#### 概述

主流的垃圾收集器基本上都是基于【可达性分析】算法来判定对象是否存活的。根据对象是否被垃圾收集器扫描过而用白、灰、黑三种颜色来标记对象的状态的一种方法。在可达性分析中，CMS、G1、ZGC都采用三色标记算法（**并发**）。

- **白色**

  对象尚未被垃圾收集器访问过。显然在可达性分析刚刚开始阶段，所有的对象都是白色的，若在分析结束之后对象仍然为白色，则表示这些对象为不可达对象，对这些对象进行回收。

- **灰色**                

  对象已经被垃圾收集器访问过，但是这个对象至少存在一个引用（属性）还没有被扫描过。

- **黑色**

  对象已经被垃圾收集器访问过，且这个对象的所有引用都已经被扫描过。黑色表示这个对象扫描之后依然存活，是可达性对象，如果有其他对象引用指向了黑色对象，无需重新扫描，黑色对象不可能不经过灰色对象直接指向某个白色对象。

![image-20230424154311461](https://github.com/chou401/pic-md/raw/master/img/image-20230424154311461.png)

#### 标记的过程

- 初始状态

  初始阶段只有GC Roots 是黑色的，其他对象都是白色的，如果没有被黑色对象引用那么最终都会被当做垃圾对象回收。

  ![image-20230424164613104](https://github.com/chou401/pic-md/raw/master/img/image-20230424164613104.png)

- **开始扫描**

  A和B均为扫描过的对象并且其引用也已经被垃圾回收器扫描过所以此时A、B对象均变为了黑色，而刚扫描到对象C，由于C的D和E还没有扫描到，所以C暂时为灰色。

  ![image-20230424164543590](https://github.com/chou401/pic-md/raw/master/img/image-20230424164543590.png)



- **扫描结束**

  此时扫描完成，黑色对象就是存活的对象，即可达对象，白色对象G为不可达对象，在垃圾回收时会被回收掉。

  ![image-20230424164809038](https://github.com/chou401/pic-md/raw/master/img/image-20230424164809038.png)

  这个过程正确执行的前提是没有其他线程改变对象间的引用关系，然而，并发标记的过程中，用户线程仍在运行，因此就会产生漏标和错标的情况。

#### 缺点

##### 多标

![image-20230424165421241](https://github.com/chou401/pic-md/raw/master/img/image-20230424165421241.png)

```java
C.E = null
```

假如GC线程已经扫描到了E对象，此时E对象为灰色，这个时候用户线程将C的引用E断开，那么GC就会认为E对象是可达对象，而不会对E进行垃圾回收，但实际上E是个垃圾对象，这个时候就会产生多标的问题，多标问题其实还可以接受，E作为浮动垃圾，那么等到下次垃圾回收的时候回收掉。

另外，针对并发标记开始后的新对象，通常的做法是直接全部当做黑色，本轮不会进行清除。这部分对象期间可能会变成垃圾，这也算是浮动垃圾的一部分。

实际上，这个问题依然可以通过【写屏障】来解决，只要在C写E的时候加入写屏障，记录下E被切断的记录，重新标记时可以再把他们标为白色即可。

##### 漏标

![image-20230424170559888](https://github.com/chou401/pic-md/raw/master/img/image-20230424170559888.png)

```java
var E = C.E;
C.E = null; // 灰色C 断开引用 白色E
B.E = E; // 黑色B 引用 白色E
```

假如用户线程先断开了C到E的引用，那么E对象就认为是不可达对象，而此时B对象又引用了E对象，但是三色标记又不会重新从B点开始标记到E，那么E就会被认为是垃圾对象，但实际上E是有引用的，那么此时对E进行垃圾回收，之后就一定会产生错误。



#### 漏标解决方案

漏标**只有在满足下面两种情况**下才会发生，那么要想解决并发扫描时漏标的问题只需要破坏任何一个条件即可。

![image-20230424171617866](https://github.com/chou401/pic-md/raw/master/img/image-20230424171617866.png)

##### 增量更新

增量更新（Incremental Update）是站在**新增引用对象**的角度来解决问题。当黑色对象插入新的指向白色对象的引用关系时，就将这个新插入的引用记录下来，等并发扫描结束之后，再将这些记录过的引用关系中的黑色对象作为跟对象，再重新扫描一遍。比如漏标问题中，一旦B对象直接指向了E对象，那么在并发扫描之后，就会把B对象作为灰色对象，再重新扫描一遍。这样虽然避免了漏标问题，但是重新标记会导致STW的时间变长。

##### 原始快照

原始快照（Snapshot At The Beginning, SATB）是站在**减少引用的对象**的角度来解决问题。当灰色对象要删除指向白色对象的引用关系时，就将这个要删除的引用记录下来，在并发扫描结束后，再将这些记录过的引用关系中的灰色对象作为跟对象，再重新扫描一遍。例如漏标问题中，C断开了E的引用关系时会保存一个快照，然后等扫描结束之后，会把C当作跟再重新扫描一遍，例如B没有引用E，那么E对象会认为是不可达对象，这样E就成了浮动垃圾，只能等下次垃圾回收时再回收。

无论是对引用关系记录的插入还是删除，虚拟机的记录操作都是通过写屏障实现的，在HotSpot 虚拟机中，**CMS是基于增量更新来做并发标记的，G1、Shenandoah则是用STAB来实现的。**



##### 读写屏障

> 这个屏障指的不是并发编程里的屏障，这里的屏障可以理解成就是在读写操作前后插入一段代码，用于记录一些信息、保存某些数据等，概念类似于AOP。

从代码角度看：

```java
var E = C.E; // 1.读
C.E = null;  // 2.写
B.E = E;     // 3.写
```

1. **读取**对象C的成员变量E的引用值，即对象E。
2. 对象C 往其成员变量 E，**写入** null。
3. 对象B 往其成员变量 E，**写入**对象 E。

**写屏障（Store Barrier）**

给某个对象的成员变量赋值时，代码大概如此：

```c
/**
* @param field 某对象的成员变量，如 D.fieldG
* @param new_value 新值，如 null
*/
void oop_field_store(oop* field, oop new_value) { 
    *field = new_value; // 赋值操作
} 
```

所谓的写屏障，其实就是指在赋值操作前后，加入一些处理（可以参考AOP的概念）：

```c
void oop_field_store(oop* field, oop new_value) {  
    pre_write_barrier(field); // 写屏障-写前操作
    *field = new_value; 
    post_write_barrier(field, value);  // 写屏障-写后操作
}
```

**写屏障 + STAB**

当对象 C 的成员变量的引用发生变化时（C.E = null），我们可以利用写屏障，将C原来的成员变量的引用对象 E 记录下来：

```c
void pre_write_barrier(oop* field) {
    oop old_value = *field; // 获取旧值
    remark_set.add(old_value); // 记录 原来的引用对象
}
```

**当原来成员变量的引用发生变化之前，记录下原来的引用对象**。这种做法的思路是：尝试保留开始时的对象图，即原始快照，当某个时刻的GC Roots 确定后，当时的对象图就已经确定了。比如当时B是引用着E的，那后续的标记也应该是按照这个时刻的对象图走（B引用着E）。如果期间发生了变化，则可以记录起来，保证标记依然按照原本的视图来。

扫描所有GC Roots 这个操作通常是需要STW，否则有可能永远都扫不完，因为并发期间可能增加新的GC Roots 。

优化：如果不是处于垃圾回收的并发标记阶段，或者已经被标记过了，其实是没必要再记录了，所以可以加个简单的判断：

```c
void pre_write_barrier(oop* field) {
  // 处于GC并发标记阶段 且 该对象没有被标记（访问）过
  if($gc_phase == GC_CONCURRENT_MARK && !isMarkd(field)) { 
      oop old_value = *field; // 获取旧值
      remark_set.add(old_value); // 记录  原来的引用对象
  }
}
```

**写屏障 + 增量更新**

当对象B的成员变量的引用发生变化时（B.E = E），可以利用写屏障，将B新的成员变量引用对象E记录下来：

```c
void post_write_barrier(oop* field, oop new_value) {  
  if($gc_phase == GC_CONCURRENT_MARK && !isMarkd(field)) {
      remark_set.add(new_value); // 记录新引用的对象
  }
}
```

**当有新引用插入进来时，记录下新的引用对象**。思路：不要求保留原始快照，而是针对新增的引用，将其记录下来等待遍历，即增量更新。

**读屏障（Load Barrier）**

读屏障是直接针对：var E = C.E，当读取成员变量时，一律记录下来：

```java
oop oop_field_load(oop* field) {
    pre_load_barrier(field); // 读屏障-读取前操作
    return *field;
}

void pre_load_barrier(oop* field, oop old_value) {  
  if($gc_phase == GC_CONCURRENT_MARK && !isMarkd(field)) {
      oop old_value = *field;
      remark_set.add(old_value); // 记录读取到的对象
  }
}
```

这种做法是保守的，但也是安全的。【黑色对象重新引用白色对象】，重新引用的前提是：得获取到该白色对象，此时已经读屏障就发挥作用了。

对于读写屏障，以 Java HotSpot VM 为例，其并发标记时对漏标的处理方案如下：

- **CMS：写屏障 + 增量更新**
- **G1、Shenandoah：写屏障 + SATB**
- **ZGC：读屏障**

读写屏障还有其他功能，比如写屏障可以用来记录跨代/区引用的变化，读屏障可以用于支持移动对象的并发执行等。

CMS中使用的增量更新，再重新标记阶段，除了需要遍历写屏障的记录，还需要重新扫描遍历GC Roots（标记过的无需再遍历），这是由于CMS对于astore_x等指令不添加写屏障的原因。

**为什么G1用SATB？CMS用增量更新？**

SATB（原始快照）相对增量更新效率比较高（当然SATB可能会造成更多的浮动垃圾），因为不需要在重新标记阶段再次深度扫描被删除引用对象，而CMS 对增量引用对象会做深度扫描，G1因为很多对象都处于不同的 Region，CMS就一块老年代区域，重新深度扫描对象的话，G1的代价会比CMS高，所以G1选择STAB不深度扫描对象，只是简单标记，等到下一轮再深度扫描。



### 垃圾收集器

![image-20230424105126389](https://github.com/chou401/pic-md/raw/master/img/image-20230424105126389.png)

**GC 为什么要暂停用户线程？**

首先，如果不暂停用户线程，就意味着期间会不断有垃圾产生，永远也清理不干净。

其次，用户线程的运行必然会导致对象的引用关系发生改变，这就会导致两种情况：多标和漏标。

1. **多标**

   原本不是垃圾，但是GC的过程中，用户线程将其引用关系修改，导致GC Roots不可达，成为了垃圾。这种情况还好一点，无非就是产生了一些浮动垃圾，下次GC再清理就好了。

2. **漏标**

   原来是垃圾，但是GC的过程中，用户线程将引用重新指向了它，这时如果GC一旦将其回收，将会导致程序运行错误。

   

#### 串行垃圾收集器

为单线程环境设计且只使用一个线程进行垃圾回收，会暂停所有的用户线程，所以不适合服务器环境。

- **堆内存中新生代垃圾回收器（Serial）**

  串行收集器是最古老，最稳定以及效率高的收集器，只使用一个线程去回收，但其在进项垃圾收集过程中可能会产生较长的停顿（Stop-The-World）状态。虽然在收集垃圾的过程找那个需要暂停所有其他的工作线程，但是它简单高效，对于限定单个CPU环境来说，没有线程交互的开销，可以获得最高的单线程垃圾收集效率，因此Serial 垃圾收集器依然是Java虚拟机运行在 Client 模式下默认的新生代垃圾收集器。

- **堆内存中老年代垃圾回收器（Serial Old）**

  Serial Old 是 Serial 垃圾收集器老年代版本，它同样是单线程的收集器，使用标记-压缩算法，这个收集器也主要是运行在 Client
  默认的老年代垃圾收集器，在老年代中，也充当 CMS 收集器的后备垃圾收集方案（$\textcolor{red}{GC 单线程}$）。

#### 并行垃圾收集器

多个垃圾回收器并行工作，此时用户线程是暂时的，适用于科学计算/大数据处理等弱交互场景。

- **堆内存中新生代垃圾回收器（ParNew、Parallel Scavenge）**

  ParNew 收集器其实就是 Serial 收集器新生代的并行多线程版本，最常见的应用场景是配合老年代 CMS GC 工作，其余的行为和
  Serial 收集器完全一样，ParNew 垃圾收集器在垃圾收集过程中同样也要暂停所有其他的工作线程（$\textcolor{red}{GC 多线程}$）。$\textcolor{CornflowerBlue}{JVM 配置：-XX:+UseParNewGC 使用ParNew收集器}$

  Parallel Scavenge 收集器类似 ParNew 也是一个新生代垃圾收集器，使用拷贝算法，也是一个并行的多线程的垃圾收集器，俗称吞吐量优先收集器（$\textcolor{red}{GC
  多线程}$）。

  $\textcolor{CornflowerBlue}{JVM配置：-XX:+UseParallelGC 使用Parallel Scavenge收集器}$

- **堆内存中老年代垃圾回收器（Parallel Old）**

  Parallel Old 收集器是Parallel Scavenge 的老年代版本，使用多线程的标记-压缩算法，Parallel Old 收集器在 JDK1.6 才开始提供在 JDK1.6 之前，新生代使用Parallel Scavenge 收集器只能搭配老年代的 Serial Old 收集器，只能保证新生代的吞吐量优先，无法保证整体吞吐量。

  JDK1.8 后考虑新生代 Parallel Scavenge 和老年代 Parallel Old 收集器的搭配策略。

  $\textcolor{CornflowerBlue}{JVM配置：-XX:+UseParallelOldGC 使用Parallel Old收集器}$

#### CMS（并发）垃圾收集器

CMS（Concurrent Mark Sweep:并发标记清除）是一款里程碑式的垃圾收集器，因为在它之前，GC线程和用户线程是无法同时工作的，即使是 Parallel Scavenge，也不过是GC时开启多个线程并行回收而已，GC的整个过程依然要暂停用户线程，即Stop The World。这带来的后果就是Java程序运行一段时间就会卡顿一会，降低应用的响应速度，这对于运行在服务端的程序是不会被接收的。

CMS 是一种以**获取最短回收停顿时间为目标**的收集器。CMS 非常适合堆内存大、CPU核数多的服务器端应用，也是G1出现之前大型应用的首选收集器。**用户线程和垃圾收集线程同时执行（不一定是并行，可能交替执行），不需要停顿用户线程**，适用对响应时间有要求的场景，主要在老年代回收。

$\textcolor{CornflowerBlue}{JVM配置：-XX:+UseConcMarkSweepGC 使用CMS收集器}$

##### 垃圾收集流程

![image-20230424111120289](https://github.com/chou401/pic-md/raw/master/img/image-20230424111120289.png)

**大概分为四个主要步骤**

![image-20230424111223763](https://github.com/chou401/pic-md/raw/master/img/image-20230424111223763.png)

  - **初始标记（Initial Mark）**

    只是标记一下GC Roots 能关联的对象，速度很快。**仍然需要暂停所有工作线程**。不过这个过程非常快，而且**初始标记的耗时不会因为堆空间的变大而变慢**，是可控的，因此可以忽略这个过程导致的短暂停顿。


  - **并发标记（Concurrent Mark）**

    进项 GC Roots 跟踪过程，和用户线程一起执行，不需要暂停工作线程，主要标记过程、标记全部对象。将初始标记的对象进行深度遍历，以这些对象为根，遍历整个对象图，这个过程耗时较长，而且**标记的时间会随着堆空间的变大而变大**。不会触发STW，用户线程仍然可以工作，程序依然可以响应，只是程序的性能会受到一点影响。因为GC线程会占用一定的CPU和系统资源，对处理器比较敏感。CMS默认开启的GC线程数是：（CPU核心数+3）/ 4，当CPU核心数超过4个时，GC线程会占用不到25%的CPU资源，如果CPU数不足4个，GC线程对程序的影响就会非常大，导致程序的性能大幅降低。


  - **重新标记（Remark）**

    由于并发标记时，用户线程仍在运行，这意味着并发标记期间，用户线程有可能改变了对象间的引用关系，可能会发生两种情况：一种是原本不能被回收的对象，现在可以被回收了，另一种是原本可以被回收的对象，现在不能被回收了。针对这两种情况，CMS需要**暂停所有用户线程**，进行一次重新标记。


  - **并发清除（Concurrent Sweep）**

    清楚 GC Roots 不可达对象，和用户线程一起工作，不需要暂停工作线程，基于标记结果，直接清除对象。这个过程耗时也比较长，且清理的开销会随着堆空间的变大而变大，和并发标记一样，清理时GC线程依然要占用一定的CPU和系统资源，会导致程序的性能降低。

##### 缺点

尽管CMS是一款里程碑式的垃圾收集器，开启了GC线程和用户线程同时工作的先河，但是不管是哪个JDK版本，CMS从来都不是默认的垃圾收集器。

![image-20230424144233399](https://github.com/chou401/pic-md/raw/master/img/image-20230424144233399.png)

- **对处理器敏感**

  并发标记、并发清理阶段，虽然CMS不会触发STW，但是标记和清理需要GC线程介入处理，GC线程会占用一定的CPU资源，进而导致程序的性能下降，程序响应速度变慢。CPU核心数多的话还稍微好一点，CPU资源紧张的情况下，GC线程对程序的性能影响比较大。

- **浮动垃圾**

  并发清理阶段，由于用户线程仍在运行，在次期间用户线程制造的垃圾就被称为“浮动垃圾”，浮动垃圾本次GC无法清理，只能留到下次GC再清理。

- **并发失败**

  由于浮动垃圾的存在，因此CMD必须预留一部分空间来装载这些新产生的垃圾。CMS不能像Serial Old收集器那样，等到Old区填满了再来清理。在JDK5时，CMS会在老年代使用了68%的空间时激活，预留了32%的空间来装载浮动垃圾，这是一个比较偏保守的配置。如果实际引用中，老年代增长的不是太快，可以通过 ==-XX：CMSInitiatingOccupancyFraction== 参数适当调高这个值。到了 JDK6，触发的阈值就被提升至92%，只预留了8%的空间来装载浮动垃圾。

  如果CMS预留的内存无法容纳浮动垃圾，那么就会导致并发失败，这时JVM不得不触发预备方案，启用Serial Old 收集器来回收Old区，这时停顿时间就变得更长了。

- **内存碎片**

  由于CMS采用的是[标记清除]算法，这就意味着清理完成厚会在堆中产生大量的内存碎片。内存碎片过多会带来很多麻烦，其一就是很难为大对象分配内存。导致的后果就是：堆空间明明还有很多，但就是找不到一块连续的内存区域为大对象分配内存，而不得不触发一次Full GC，这样GC的停顿时间又会变得更长。

  针对这种情况，CMS提供了一种备选方案，通过==-XX：CMSFullGCsBeforeCompaction== 参数设置，当CMS由于内存碎片导致出发了N次 Full GC 后，下次进入Full GC 前先整理内存碎片，不过这个参数在 JDK9 被弃用了。

  

#### G1垃圾收集器

##### 概述

G1（Garbage-First）垃圾回收器是在JDK7引入的一个新的垃圾收集器。在JDK9时被选定为默认收集收集器。

G1 最大的特点是引入分区的思路，弱化了分代的概念，合理利用垃圾收集各个周期的资源，解决了其他收集器的众多缺陷。

G1垃圾回收器将堆内存分割成不同的区域，然后并发的对其进行垃圾回收，G1 收集器的设计目标是取代 CMS 收集器。G1 和 CMS 相比，有以下不同：

- G1 能充分利用多CPU、多核环境硬件优势，尽量缩短 STW。部分其他收集器原本需要停顿 Java 线程执行的 GC 动作，G1 收集器仍然可以通过并发的方式让Java程序继续运行。

- 与 CMS 的 “标记-清除”算法不同，G1 从整体来看是基于“**标记-压缩**”算法实现的收集器，从局部来看是基于“复制”算法。因此其回收得到的空间是连续的。这避免了 CMS 回收器因为不连续空间所造成的问题，例如：需要更大的堆空间，更多的 floating garbage。连续空间意味着 G1 垃圾收集器可以不必采用空闲链表的内存分配方式，而可以直接采用 bump-the-pointer （撞针） 的方式。

- G1 的内存与 CMS 要求的内存模型有极大的不同。G1 将内存划分一个个固定大小的region，每个region既可以是年轻代，也可以是老年代的。**内存的回收是以region作为基本单位的**。

- G1 还要一个极其重要的特性：**软实时**（soft real-time）。所谓的实时垃圾回收，是指在要求的时间内完成垃圾回收。软实时则是指，用户可以指定垃圾回收时间的现时，G1 会努力在这个时限内完成垃圾回收，但是 G1 并不担保每次都能在这个时限内完成垃圾回收。通过设定一个合理的目标，可以让达到90%以上的垃圾回收时间都在这个时限内。

  用户可以指定期望停顿时间（可通过配置 XX:MaxGCPauseMills=n最大停顿时间）。期**望停顿时间模型是能够支持指定在一个长度为M毫秒的时间片段内，消耗在垃圾收集上的时间大概率不超过N毫秒这样的目标。**

- G1可以面向堆内存任何部分来组成回收集（Collection Set，CSet）进行回收，衡量标准不再是它属于哪个分代，而是哪块内存中存放的垃圾数量最多，回收收益最大。

- G1 虽然也是分代收集器，但整个内存分区不存在物理上的年轻代和老年代的区别，也不需要完全独立的 Survivor 堆做复制准备，G1只有逻辑上的分代概念，或者说每个分区都可以随 G1 的运作在不同代之间前后切换。

   $\textcolor{CornflowerBlue}{JVM配置：-XX:+UseG1GC 使用G1收集器}$

##### 内存模型

![image-20230425145527908](https://github.com/chou401/pic-md/raw/master/img/image-20230425145527908.png)

G1收集器采用一种不同的方式来管理内存：

![image-20230414181654804](https://github.com/chou401/pic-md/raw/master/img/image-20230414181654804.png)

**堆内存被划分为多个大小相等的heap区，每个heap区都是逻辑上连续的一段内存（virtual memory)，其中一部分区域被当成收集器相同的角色（eden，survivor，old），但每个角色的区域个数都不是固定的。**这在内存使用上提供了更多的灵活性。



###### 分区 Region

G1 采用了分区（Region）的思路，将整个堆内存区域分成**大小相同**的子区（Region），在JVM 启动时会自动设置这些子区域的大小，在堆的使用上，G1并不要求对象的存储一定要在物理上连续，只要逻辑上连续即可，每个分区也不会固定地为某个代服务，可以按需在年轻代和老年代之间切换。启动时可以通过参数设置 ==-XX:G1HeapRegionSize=n== 可指定分区大小（1MB~32MB，且必须是2的幂），默认将整堆划分为2048个分区。也即能够支持的最大内存为：32MB*2048 = 65536MB ≈ 64G内存。

这些 Region 的一部分包含新生代，新生代的垃圾收集依然采用暂停所有应用线程的方式，将存活的对象拷贝到老年代或者 Survivor空间。这些 Region 的一部分包含老年代，G1 收集器通过将对象从一个区域复制到另一个区域，完成了清理工作。这就意味着，在正常的处理过程中，G1完成了堆的压缩（至少是部分堆的压缩），这样就不会有CMS 的内存碎片问题了。

###### 卡片 Card

在每个分区内部又被分成了若干个大小为512Byte卡片（Card），标识堆内存最小可用粒度。所有分区的卡片，将会记录在全局卡片表（Global Card Table）中，分配的对象会占用物理上连续的若干个卡片，当查找分区内对象的引用时，便可通过卡片来查找该引用对象（见 RSet）。每次对内存的回收，都是对指定分区的卡片进行处理。

G1 对内存的使用以分区（Region）为单位，而对对象的分配则以卡片（Card）为单位。

![image-20230425160046585](https://github.com/chou401/pic-md/raw/master/img/image-20230425160046585.png)

###### 巨型对象 Humongous Region

**Region中还有一类特殊的Humongous区域,专门用来存储大对象。G1认为只要大小超过了一个Region容量一半的对象即可判定为大对象**，而对于那些超过整个Region容量的超级大对象，将会被存放在N个连续的Humongous Region之中，G1的大多数行为都把Humongous Region作为老年代的一部分来进行看待。G1 内部做了一个优化，一旦发现没有引用指向巨型对象，则可直接在年轻代收集周期中被回收。

巨型对象会独占一个或多个连续分区，其中第一个分区被标记为开始巨型（StartsHumongous），相邻连续分区被标记为连续巨型（ContinuesHumongous）。由于需要一片连续的内存空间需要扫描整堆，因此确定巨型对象开始位置的成本比较高，如果可以，应用程序应避免生成巨型对象。

###### 已记忆集合 Remember Set（RSet)

除了 Card Table 数组之外，每个 Region 还会有一个 RSet 数据结构。RSet 主要用来记录哪个 Region 的哪个 Card 上的对象引用了本 Region 中的对象。Remebered Set “**记住谁引用了我，以后我垃圾回收的时候，我好找它去** ！”。

实际实现中，RSet 默认是一个 HashMap，Map的key是引用的 Region，value 是一个 List，List 中存储引用Region 中的引用 Card 列表。Region、Card Table 以及 RSet 的示意图：

![image-20230425162127860](https://github.com/chou401/pic-md/raw/master/img/image-20230425162127860.png)

上图中，RegionA 和 RegionB 中分别有对象引用 RegionC 中的对象，在 RegionC 对应的 RSet 就会记录这样的引用关系。这时候只需要扫描那两张Card 里的对象就可以了。这是一种**典型的空间换时间**的方法，避免了整个堆的扫描，提高效率。该 RSet 中有两个 KV 对，第一个 KV 的key是 RegionA，value 是一个列表，列表中有两个元素 3 和 65534，分别代表 RegionA 中引用对象在对应 Card Table 中的下标。第二个 KV 对的 key 是RegionB，value 中列表只有一个元素 1565，代表 RegionB 中引用对象在对应 Card Table 中的下标。

###### 收集集合（CSet）

CSet 收集示意图：

![image-20230425172916290](https://github.com/chou401/pic-md/raw/master/img/image-20230425172916290.png)

收集集合（Collection Set）代表每次GC暂停时回收的一系列目标分区。在任意一次收集暂停中，CSet 所有分区都会被释放，内部存活的对象都会被转移到分配的空闲分区中。因此无论是年轻代收集，还是混合收集，工作的机制都是一致的。年轻代收集 CSet 只容纳年轻代分区，而混合收集会通过启发式算法，在老年代候选回收分区中，筛选出回收收益最高的分区添加到 CSet 中。

候选老年代分区的 CSet 准入条件，可以通过活跃度阈值 ==-XX:G1MixedGCLiveThresholdPercent==（默认85%）进行设置，即只有存活对象低于85%的 Region 才可能被回收，从而拦截那些回收开销巨大的对象；同时，每次混合收集可以包含候选老年代分区，可根据 CSet 对堆的总大小占比 ==-XX:G1OldCSetRegionThresholdPercent==（默认10%）设置数量上限，即老年代一次最大收集总内存的10%。

由上述可知，G1 的收集都是根据 CSet 进行操作的，年轻代收集与混合收集没有明显的不同，最大的区别在于两种收集的触发条件。

##### G1垃圾收集分类

G1的垃圾收集分为 ==Young GC==、==Mixed GC== 和 ==Full GC==。

###### Young GC

G1 与之前垃圾收集器的 Young GC 有所不同，并不是当新生代的 Eden 区放满了就进行垃圾回收，G1 会计算当前 Eden 区回收大概需要多久的时间，如果回收时间远小于参数 ==-XX:MaxGCPauseMills== 设定的值，那么 G1 就会增加年轻代的 Region（可以从老年代或Humongous区划分 Region 给新生代），继续给新对象存放；直到下一次 Eden 区放满，G1 计算回收时间接近参数 ==-XX:MaxGCPauseMills== 设定的值，那么就会触发 **Young GC** 。

###### Mixed GC

如果老年代的堆空间内存占用达到了参数 ==-XX:InitiatingHeapOccupancyPercent== 设定的值就会触发 ==Mixed GC==，回收所有的新生代和部分老年代（根据用户设置的 GC 停顿时间来确定老年代垃圾收集的先后顺序）以及 Humongous 区。正常情况下 G1 的垃圾收集是先做 ==Mixed GC==，主要是使用复制算法，需要把每个 Region 中存活的对象复制到另一个空闲的 Region，如果在复制过程中发现没有足够的空 Region 放复制的对象，那么就会触发一次 ==Full GC==。

###### Full GC

停止系统程序，然后采用单线程进行标记、清理和压缩整理，以便空闲出来一批 Region 供下一次 ==Mixed GC== 使用，这个过程是非常耗时的。

**G1 是如何满足目标暂停时间的？**

前提：G1 的JVM内存模型在物理上分区Region，逻辑上分代。

1. 在年轻代收集（Young GC）期间，G1 GC 调整年轻代（Eden 和 Survivor）的大小以来匹配软实时（soft real-time）的目标。
2. 在混合收集（Mixed GC）期间，G1 GC 根据混合垃圾收集的目标数量、堆中每个区域中活动对象的百分比以及总体可接收的堆浪费百分比来调整收集的旧区域的数量，以满足目标暂停时间目标。

##### 收集过程

G1 在进行垃圾收集的时候，会根据每个 Region 预计垃圾收集所需时间与预计回收内存大小的占比来选择对哪些区域进行回收，也就是不再有 ==Minor GC/Young GC== 和 ==Major GC/Full GC== 的概念，而是采用一种 ==Mixed GC== 的方式，即混合回收的 GC 方式。

###### 年轻代收集

G1 的 Young GC 和 CMS 的 Young GC ，其标记-复制全过程 STW。

![image-20230426112834867](https://github.com/chou401/pic-md/raw/master/img/image-20230426112834867.png)

###### 混合收集

年轻代收集不断活动后，老年代的空间也会被逐渐填充。当老年代占用空间超过整堆比阈值 ==-XX:InitiatingHeapOccupancyPercent==（默认45%）时，G1 就会启动一次混合垃圾收集周期，即==Mixed GC== 。

该算法并不是一个老年代，除了回收整个年轻代，还会回收一部分老年代。需要注意：是一部分老年代，而不是全部老年代，可以选择哪些老年代进行收集，从而可以对垃圾回收的耗时时间进行控制。

G1 没有Full GC概念，需要 Full GC 时，调用 serialOldGC 进行全堆扫描（包括 Eden、Survivor、o、perm）。

为了满足暂停目标，G1 可能不能一口气将所有的候选分区收集掉，因此G1 可能会产生连续多次的混合收集要应用线程交替执行，每次STW 的混合收集与年轻代收集过程相类似。

![image-20230426140913494](https://github.com/chou401/pic-md/raw/master/img/image-20230426140913494.png)

GC 步骤分两步：

1. 全局并发标记（Global Concurrent Marking）
2. 拷贝存活对象（Evacuation）

**全局并发标记：**

  - **初始标记（Initial Mark，STW）：**从 GC Roots 出发标记全部直接子节点的过程，这个阶段会执行一次年轻代 GC，会产生全局停顿。由于 GC Roots 数量不多，通常该阶段耗时比较短。
  - **跟区域扫描（Root Region Scan）：**
    - G1 GC 在初始标记的存活区扫描对老年代的引用，并标记该被引用的对象。
    - 该阶段与应用程序（非 STW）同时运行，并且只有完成该阶段后，才能开始下一次 STW 年轻代垃圾回收。

  - **并发标记（Concurrent Marking）：**从 GC Roots 开始对堆中对象进行可达性分析，找出可访问的（存活的）对象。该阶段是并发的，即应用线程和 GC 线程可以同时活动。**可以被 STW 年轻代垃圾回收中断**。并发标记耗时相对长很多，但因为不是 STW，所以沃恩不太关心阶段耗时的长短。
  - **重新标记（Remark，STW）：**修正并发标记期间，因程序运行而导致标记发生变化的那一部分对象。该阶段是 STW 的。
  - **清除垃圾（Cleanup，STW）：**清点和重置标记状态，该阶段会 STW，**这个阶段不会清理垃圾对象**，也不会执行存活对象的复制，等待 evacuation 阶段来回收。

**拷贝存活对象（并发清理）：**

该阶段把一部分 Region 里的存活对象拷贝到另一部分 Region 中，从而实现垃圾的回收清理。复制算法的转移阶段需要分配新内存和复制对象的成员变量。**转移阶段是 STW 的**，其中内存分配通常耗时非常短，但对象成员变量的复制耗时有可能较长，这是因为复制耗时与存活对象数量和对象复杂度成正比。对象越复杂，复制耗时越长。

四个 STW 过程中，出事标记因为只标记 GC Roots，耗时较短。重新标记因为对象较少，耗时也比较短。清理阶段因为内存分区数量少，耗时也较短。转移阶段要处理所有存活对象，耗时会较长。因此，G1 停顿时间的瓶颈主要是标记-复制中的转移阶段 STW。为什么转移阶段不能喝标记阶段一样并发执行？**主要是 G1 未能解决转移过程中准确定位对象地址的问题。**

##### G1 相关参数

|                参数                |                             说明                             |
| :--------------------------------: | :----------------------------------------------------------: |
|            -XX:+UseG1GC            |                     开启使用G1垃圾收集器                     |
|       -XX:ParallelGCThreads        |                     指定GC工作的线程数量                     |
|        -XX:G1HeapRegionSize        | 指定分区大小(1MB~32MB，且必须是2的N次幂)，默认将整堆划分为2048个分区 |
|        -XX:MaxGCPauseMillis        |                 目标暂停(STW)时间(默认200ms)                 |
|        -XX:G1NewSizePercent        | 新生代内存初始空间(默认整堆5%，**值配置整数，比如5**，默认就是百分比) |
|      -XX:G1MaxNewSizePercent       |           新生代内存最大空间(最大60%，值配置整数)            |
|      -XX:TargetSurvivorRatio       | Survivor区的填充容量(默认50%)，Survivor区域里的一批对象(年龄1+年龄2+年龄n的多个年龄对象)总和超过了Survivor区域的50%，此时就会把年龄n(含)以上的对象都放入老年代 |
|      -XX:MaxTenuringThreshold      |                     最大年龄阈值(默认15)                     |
| -XX:InitiatingHeapOccupancyPercent | 老年代占用空间达到整堆内存阈值(默认45%)，则执行新生代和老年代的混合收集(MixedGC)，比如堆默认有2048个region，如果有接近1000个region都是老年代的region，则可能就要触发MixedGC了 |
| -XX:G1MixedGCLiveThresholdPercent  | 默认85%，Region中的存活对象低于这个值时才会回收该Region，如果超过这个值，存活对象过多，回收的的意义不大 |
|      -XX:G1MixedGCCountTarget      | 在一次回收过程中指定做几次筛选回收(默认8次)，在最后一个筛选回收阶段可以回收一会，然后暂停回收，恢复系统运行，一会再开始回收，这样可以让系统不至于单次停顿时间过长。 |
|       -XX:G1HeapWastePercent       | 默认5%，GC过程中空出来的Region是否充足阈值，在混合回收的时候，对Region回收都是基于复制算法进行的，都是把要回收的Region里的存活对象放入其他Region，然后这个Region中的垃圾对象全部清理掉，这样的话在回收过程就会不断空出来新的Region，一旦空闲出来的Region数量达到了堆内存的5%，此时就会立即停止混合回收，意味着本次混合回收就结束了 |

##### 优化

假设参数 ==-XX:MaxGCPauseMills== 设置的值很大，导致系统运行很久，年轻代可能都占用了堆内存的60%了，此时才触发年轻代 GC。那么存活下来的对象可能就会很多，此时就会导致 Survivor 区域放不下那么多的对象，就会进入老年代中。或者是年轻代 GC 过后，存活下来的对象过多，导致进入 Survivor 区域后出发了动态年龄判断规则，达到了 Survivor 区域的50%，也会快速导致一些对象进入老年代中。

所以核心还是在于调节 ==-XX:MaxGCPauseMills== 这个参数的值，在保证他的年轻代 GC 别太频繁的同时，还得考虑每次 GC 过后的存活对象有多少，避免存活对象太多快速进入老年代，频繁触发 ==Mixed GC==。

什么场景适合使用 G1？

- 50%以上的堆被存活对象占用
- 对象分配和晋升的速度变化非常大
- 垃圾回收时间特别长，超过1秒
- 8GB以上的堆内存（建议值）
- 停顿时间是500ms以内

##### 安全点与安全区域

JVM 的所有垃圾收集器在做垃圾收集时，以 G1 的 ==Mixed GC== 为例，初始标记、并发标记等每一步都需要到达一个安全点或安全区域时才能开始执行。

安全点就是指代码中一些特定的位置，当线程运行到这些位置时它的状态是确定的，这样 JVM 就可以安全的进行一些操作，比如 GC 等，所以 GC 不是想什么时候做就立即触发的，是需要等待所有线程运行到安全点后才能触发。

这些特定的安全点文职主要有以下几种：

- 方法返回之前
- 调用某个方法之后
- 抛出异常的位置
- 循环的末尾

大体实现思想是当垃圾收集需要中断线程的时候，不直接对线程操作，仅仅简单地设置一个标志位，各个线程执行过程时会不停地主动去轮询这个标志，一旦发现中断标志位镇时就自己在最近的安全点上主动中断挂起。轮询标志的地方和安全点是重合的。==Safe Point== 是对正在执行的线程设定的。如果一个线程处于 Sleep 或中断状态，它就不能响应 JVM 的中断请求，再运行到 ==Safe Point== 上。因此 JVM 引入了 ==Safe Region== 。==Safe Region== 是指在一段代码片段中，引用关系不会发生变化。在这个区域内的任何地方开始 GC 都是安全的。

##### 注意事项

- 避免通过 -Xmn 或其他相关选项（如==-XX:NewRate==）显式设置年轻代的大小，因为年轻代的大小被固定后会导致 G1 的目标暂停机制失效。
- 当设置目标暂停时间 MaxGCPauseMillis 时，需要评估 G1 GC 的延迟和应用吞吐量之间的取舍，当该值设置较小时，标明你愿意承担垃圾收集开销的增加，从而会导致应用程序吞吐量的降低。
- Mixed GC 的调优
  - ==-XX:InitiatingHeapOccupancyPercent==： 控制并发标记开始的内存占比阈值。
  - ==-XX:G1HeapWastePercent==： 设置G1中愿意浪费的堆的百分比，如果可回收Region的占比小于该值，G1不会启动Mixed GC，默认值10%，主要用来控制Mixed GC的触发时机。
  - ==-XX:G1MixedGCLiveThresholdPercent==：设置老年代Region进入CSet的活跃对象占比阈值，避免活跃对象占比过高的Region进入CSet。
  - ==-XX:G1MixedGCCountTarget== 和 ==-XX:G1OldCSetRegionThresholdPercent==: 主要是为了控制单次Mixed GC中Region的个数，CSet中Region的个数越多，GC过程中暂停时间越长。

##### 典型问题

###### 疏散失败（Evacuation Failure）

当没有更多的空闲Region被提升到老年代或者复制到Survivor时，并且由于堆已经达到最大值，堆不能扩展，从而发生 Evacuation Failure，这时 G1 的 GC 已经无能为力，只能使用通过 Serial Old GC 进行 Full GC 来收集整个Java堆空间，这个过程就是转移失败。

**解决方案**

- 如果有大量“空间耗尽（to-space exhausted）”或“空间溢出（to-space overflow）” GC 事件，则增加 ==-XX:G1ReservePercent== 以增加“to-space” 的预留内存量，默认值是Java堆的10%。注意：G1 GC 将此值限制在50%以内。
- 通过减少 ==-XX: InitiatingHeapOccupancyPercent== 的值来更早地启动并发标记周期，来及时回收不包含活跃对象的区域，同时促使 Mixed GC 更快发生。
- 增加选项 ==-XX:ConcGCThreads== 的值以增加并行标记线程的数量，减少并行标记阶段的耗时。

###### 大对象分配（Humongous Allocation）

出现大对象分配导致的内存耗尽问题，一般老年代剩余的 Region 中已经不能够找到一组连续的区域分配给新的巨型对象。

**解决方案**

- 通过 ==-XX: G1HeapRegionSize== 选项增加内存区域 Region 的大小，提升 Region 对象的判断标准，以减少巨大对象的数量。
- 增加堆Java的大小使得有更多的空间来存放巨型对象。
- 通过 ==-XX:G1MaxNewSizePercent== 降低年轻代 Region 的占比，给老年代预留更多的空间，从而给巨型对象提供更多的内存空间。

一般在疏散暂停（Evacuation Pause）和大对象分配（Humongous Allocation）会比较容易出现“空间耗尽（to-space exhausted）”或“空间溢出（to-space overflow）”的GC 事件，导致出现转移失败，进而引发 Full GC 从而导致GC的暂停时间超过 G1 设置的目标暂停时间。所以要尽量避免出现转移失败。

###### Young GC 花费时间太长

通常 Young GC 的耗时与年轻代的大小成正比，具体地说，是需要复制的集合中的活跃对象的数量。

如果 Young GC 中 CSet 的疏散阶段（Evacuate Collection Set phase）需要很长时间，尤其是其中的对象复制-转移，可以通过降低 ==-XX:G1NewSizePercent== 的值，降低年轻代的最小尺寸，从而降低停顿时间。

还可以使用 ==-XX:G1MaxNewSizePercent== 降低年轻代的最大占比，从而减少 Young GC 暂停期间需要处理的对象数量。

###### Mixed GC 耗时太长

- 通过降低 ==-XX:InitiatingHeapOccupancyPercent== 的值，来调低并发标记阶段开始的阈值，让并发标记阶段更早触发，只有并发标记完成才能开始执行 Mixed GC。
- 通过调节 ==-XX:G1MixedGCCountTarget== 和 ==-XX:G1OldCSetRegionThresholdPercent== 参数，降低单次回收的Region 数量，减少暂停时间。
- 通过调节 ==-XX:G1MixedGCLiveThresholdPercent== 的值，避免活跃对象占比过高的Region进入 CSet。因为活的对象越多，Region中可回收的空间就越少，暂停时间就越长，GC 效果就越不明显。
- 通过调节 ==-XX:G1HeapWastePercent== 的值，设置愿意浪费的堆的百分比，只有垃圾占比大于此参数，才会发生 Mixed GC，该值越小，会越早触发 Mixed GC。

##### 总结

G1 是一款非常优秀的垃圾收集器，不仅适合堆内存大的应用，同时也简化了调优的工作。通过主要的参数初始和最大堆空间、以及最大容忍的 GC 暂停目标，就能得到不错的性能。

- G1 的设计原则是**首先收集尽可能多的垃圾**（Garbage First）。因此，G1 并不会等内存耗尽的时候开始垃圾收集，而是在内部采用了启发式算法，在老年代找出具有高收集收益的分区进行收集。同时G1 可以根据用户设置的暂停时间目标自动调整年轻代和总堆大小，暂停目标越短，年轻代空间越小、总空间就越大；
- G1 采用内存分区（Region）的思路，将内存划分为一个个相等大小的内存分区，回收时则以分区为单位进行回收，存活的对象复制到另一个空闲分区中。由于都是以相等大小的分区为单位进行操作，因此 G1 天然就是一种压缩方案（局部压缩）；
- G1 虽然也是分代收集器，但整个内存分区不存在物理上的年轻代与老年代的区别，也不需要完全独立的 Survivor 堆做复制准备。G1 只有逻辑上的分代概念，或者说每个分区都可能随 G1 的运行在不同代之间前后切换；
- G1 的收集都是 STW 的，但年轻代和老年代的收集界限比较模糊，采用了混合（Mixed）收集的方式，即每次收集既可能只收集年轻代分区（年轻代收集），也可能在收集年轻代的同时，包含部分老年代分区（混合收集），这样即使堆内存很大时，也可以限制收集范围，从而降低停顿。

#### ZGC垃圾收集器

从G1 垃圾收集器开始，后面的垃圾收集器都不再将堆按照新生代和老年代作为整体进行回收，都采用了局部收集的设计思想。可能是由于G1作为第一代局部收集的垃圾收集器，所以它继续保留了新生代和老年代的概念。

ZGC设计目标：

- 停顿时间不超过10ms。
- 停顿时间不会随堆的大小，或者活跃对象的大小而增加。
- 支持8MB~4TB级别的堆。

从设计目标来看，我们知道ZGC适用于大内存低延迟服务的内存管理和回收。

与CMS中的ParNew和G1类似，ZGC也采用标记-复制算法，不过ZGC对该算法做了重大改进：ZGC在标记、转移和重定位阶段几乎都是并发的，这是ZGC实现停顿时间小于10ms目标的最关键原因。

![image-20230417102646070](https://github.com/chou401/pic-md/raw/master/img/image-20230417102646070.png)

ZGC只有三个STW：初始标记、再标记、初始转移。其中，初始标记和初始转移分别都只需要扫描所有GC Roots，其处理时间和GC Roots的数量成正比，一般情况耗时比较短；再标记阶段STW时间很短，最多1ms，超过1ms则再次进入并发标记阶段，即，ZGC几乎所有的暂停都只依赖于GC Roots集合大小，停顿时间不会随着堆的大小或者活跃对象的大小而增加。与ZGC对比，G1的转移阶段完全STW的，且停顿时间随存活对象的大小增加而增加。

##### 内存布局

ZGC（Z Garbage Collector）完全抛弃了按代收集理论，它与G1一样将内存划分成各个小的区域，但与G1有所不同的是，ZGC的各个内存区域称为页面（Page或ZPage），而且页面也不是全部大小相等。ZGC按照页面大小将页面分为三类：小页面、中页面和大页面。

在x64的硬件平台上，ZGC的页面大小为：

- 小页面：容量固定为2MB，用来存放小于256KB的小对象。

- 中页面：容量固定为32MB，用来存放大于等于256KB但小于4MB的对象。

- 大页面：容量不固定，可以动态变化，但必须为2MB的整数倍，用来存放大小大于等于4MB的对象。每个大页面只会存放一个大对象，也就是虽然它叫大页面，但它的容量可能还没中页面大，最小容量4MB。

  ![image-20230414172817666](https://github.com/chou401/pic-md/raw/master/img/image-20230414172817666.png)

ZGC对于大页面的回收策略是不同的，简单说就是小页面优先回收，中页面和大页面则尽量不回收。

##### 支持NUMA

在过去，对于X86架构的计算机，内存控制器还没有整合进CPU，所有对内存的访问都需要通过北桥芯片来完成。X86系统中的所有内存都可以通过CPU进行同等访问。任何CPU访问任何内存的速度是一致的，不必考虑不同内存地址之间的差异，这称为“统一内存访问”（Uniform Memory Access，UMA）。UMA系统的架构示意图如图所示：

![image-20230414175606068](https://github.com/chou401/pic-md/raw/master/img/image-20230414175606068.png)

在UMA中，各处理器与内存单元通过互联总线进行连接，各个CPU之间没有主从关系。之后的X86平台经历了一场从“拼频率”到“拼核心数”的转变，越来越多的核心被尽可能的塞进了同一块芯片上，各个核心对于内存带宽的争抢访问称为瓶颈，所以人们希望能够把CPU和内存集成在一个单元上（称Socket），这就是非统一内存访问（Non-Uniform Memory Access，NUMA）。在NUMA下，CPU访问本地存储器的速度比访问非本地存储器快一些。

![image-20230414175933503](https://github.com/chou401/pic-md/raw/master/img/image-20230414175933503.png)

ZGC是支持NUMA的，在进行小页面分配时优先从本地内存分配，当不能分配时才会从远端的内存分配。对于中页面和大页面的分配，ZGC并没有要求从本地内存分配，而是直接交给操作系统，由操作系统找到一块能满足ZGC页面的空间。ZGC这样设计的目的在于，对于小页面，存放的都是小对象，从本地内存分配速度很快，且不会造成内存使用的不平衡，而中页面和大页面因为需要空间大，如果也优先从本地内存分配，极易造成内存使用不均衡，反而影响性能。

##### 着色指针

ZGC收集器有一个标志性的设计就是它采用染色指针技术（Colored Pointer，也可以称为Tag Pointer或Version Pointer）。在之前如果想要在对象上存储一些额外的、只供垃圾收集器或虚拟机本身使用的数据，通常都会在对象头中添加额外的字段，比如对象的哈希码、分代年龄、锁状态等。

对于垃圾收集器而言，可能关注更多的是对象的引用指针，比如三色标记，这些标记本质上就只和对象的引用有关，而与对象本身无关，也就是某个对象只有它的引用关系能决定它存活与否，对象上的其他属性无法影响它的存活判断。

HotSpot集中收集器的标记实现方案，有得把标记直接记录在对象头（Serial），有得把标记记录在与对象完全独立的数据结构上（如G1采用BitMap的结构来记录标记信息，BitMap相当于堆内存大小的1/64）。而ZGC的染色指针是最最直接的，直接把标记信息记在引用对象的指针上，这时，与其说可达性分析是遍历对象来标记对象，还不如说是遍历**引用图标**来标记**引用**。

染色指针是一种直接将少量额外的信息存储到指针上的技术。在ZGC之前的虚拟机中，可以采用指针压缩技术，将35位以内的对象引用地址压缩到32位，而ZGC的染色质真就是要借助指针的bit位来存储额外信息。

![image-20230417111630085](https://github.com/chou401/pic-md/raw/master/img/image-20230417111630085.png)

![image-20230417113559100](https://github.com/chou401/pic-md/raw/master/img/image-20230417113559100.png)

ZGC必须要在64bit的机器上才能运行，其中使用低42位来表示对空间，然后借用几个高位来记录GC中的状态信息，分别为M0、M1、Remapped和一个预留字段，因为对象的引用地址大于35bit，所以在ZGC中是无法使用压缩指针的。

当应用程序创建对象时，首先在堆空间申请一个虚拟地址，但该虚拟地址并不会映射到真正的物理地址。ZGC同时会为该对象在M0、M1、Remapped地址空间分别申请一个虚拟地址，且这三个虚拟地址对应同一个物理地址，但这三个空间在同一时间有且只有一个空间有效。ZGC之所以设置三个虚拟地址空间，是因为它使用“空间换时间”思想，去降低GC停顿时间。“空间换时间”中的空间是虚拟空间，而不是真正的物理空间。

与上述地址空间划分相对应，ZGC实际仅使用64位地址空间的第0-41位，而第42-45位存储元数据，第47-63位固定位0。

![image-20230417114248169](https://github.com/chou401/pic-md/raw/master/img/image-20230417114248169.png)

每个对象有一个64位指针，这64位被分为：

- **18位**：预留给以后使用；
- **1位**：Finalizable标识，此位与并发引用处理有关，它标识这个对象只能通过finalizable才能访问（finalizable：object基类的一个空方法，如果被重写则会在GC之前调用该方法，该方法会且只会被调用一次）；
- **1位**：Remapped标识，设置此位的值后，对象未指向relocation set中（relocation set表示需要GC的Region集合）；
- **1位**：Marked1标识；
- **1位**：Marked0标识，和上面的Marked1都是标记对象用于辅助GC；
- **42位**：对象的地址（所以它可以支持2^42=4T内存）。

ZGC将对象存活信息存储在42-45位中，这与传统的垃圾回收并将对象存活信息放在对象头中完全不同。

在GC的过程中，指针中用于存储额外信息的bit位是变化的，也就是说对象的引用地址一直是在变化的，这怎么可能是物理空间的地址呢，对程序来说，只要这个对象没有被移动，那么它的物理地址空间就一定是不变的，那么ZGC的对象引用地址一直在变，这里指的是对象的虚拟地址，JVM怎么知道真实的物理地址空间？

这里面就涉及到了虚拟地址空间与物理地址空间的映射，不同层次的虚拟内存到物理内存的转换关系可以在硬件层面、操作系统层面以及软件进程层面来实现，如何完成地址转换，是一对一、多对一还是一对多的映射，也可以根据实际需要来设计。

Linux/x86-64平台上的ZGC使用的是多重映射（Multi-Mapping），即将多个不同的虚拟内存地址映射到同一个物理内存地址上，这种多对一映射，意味着ZGC在虚拟地址中看到的地址空间要比实际的堆空间容量更大，因为在虚拟空间中，ZGC的对象占45bit，而在物理内存空间中，只有其中的41bit表示内存地址。

有个多重映射，ZGC在GC过程中，不论M0、M1和Remapped对应的bit位怎么变化，它们对应的具体对象的内存空间都是同一个。

Linux多视图映射，Linux中主要通过系统函数mmap完成视图映射。多个视图映射就是多次调用mmap函数，多次调用的返回结果就是不同的虚拟地址。

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <stdint.h>

int main()
{
    //创建一个共享内存的文件描述符
    int fd = shm_open("/example", O_RDWR | O_CREAT | O_EXCL, 0600);
    if (fd == -1) return 0;
    //防止资源泄露,需要删除。执行之后共享对象仍然存活,但是不能通过名字访问
    shm_unlink("/example");

    //将共享内存对象的大小设置为4字节
    size_t size = sizeof(uint32_t);
    ftruncate(fd, size);

    //3次调用mmap,把一个共享内存对象映射到3个虚拟地址上
    int prot = PROT_READ | PROT_WRITE;
    uint32_t *remapped = mmap(NULL, size, prot, MAP_SHARED, fd, 0);
    uint32_t *m0 = mmap(NULL, size, prot, MAP_SHARED, fd, 0);
    uint32_t *m1 = mmap(NULL, size, prot, MAP_SHARED, fd, 0);


    //关闭文件描述符
    close(fd);

    //测试,通过一个虚拟地址设置数据,3个虚拟地址得到相同的数据
    *remapped = 0xdeafbeef;
    printf("48bit of remapped is: %p, value of 32bit is: 0x%x\n", remapped, *remapped);
    printf("48bit of m0 is: %p, value of 32bit is: 0x%x\n", m0, *m0);
    printf("48bit of m1 is: %p, value of 32bit is: 0x%x\n", m1, *m1);


    return 0;
}
```

**三大优势**：

- 一旦某个Region的存活对象被移走后，这个Region立即就能够释放和重用掉，而不必等待整个堆中所有指向该Region的引用都被修改后才能清理，这使得理论上只要还有一个空闲Region，ZGC就能完成收集。
- 着色指针可以大幅减少在垃圾收集过程中内存屏障的使用数量，ZGC只使用了读屏障。
- 着色指针具备强大的扩展性，因为还要18位未使用，它可以作为一种可扩展的存储结构用来记录更多与对象标记、重定位过程相关的数据，以便日后进一步提高性能。

##### 读屏障

并发标记阶段会进行对象的重定位以及删除对应的转发表，在ZGC中是通过读屏障实现的。

读屏障就是JVM向应用程序代码中插入一小段代码的技术，当应用程序线程需要从堆中读取对象引用时，就会执行这段代码。

```java
Object o = obj.FieldA; // 从堆中读取引用，需要加入屏障
Object p = o; // 无需加入屏障，因为不是从堆中读取引用
o.dosomething(); // 无需加入屏障，因为不是从堆中读取引用
int i =  obj.FieldB; // 无需加入屏障，因为不是对象引用
```

ZGC采用读屏障的方式来修正指针引用，由于ZGC采用的是复制整理的方式进行GC，很有可能在对象的位置改表之后指针位置尚未更新时程序调用了该对象，那么此时在程序需要并行的获取该对象的引用时，ZGC就会对该对象的指针进行读取，判断Remapped标识，如果标识为该对象位于背刺需要清理的Region区中，该对象则会有内存地址变化，会在指针中将新的引用地址替换原有对象的引用地址，然后再进行返回。如此，使用读屏障便解决了并发GC的对象读取问题。

##### 垃圾收集流程

ZGC一次垃圾回收周期中地址视图的切换过程：

**初始化**：ZGC初始化之后，整个内存空间的地址被设置为Remapped。程序正常运行，在内存分配对象，满足一定条件后垃圾回收启动，此时进入阶段。

**并发标记阶段**：第一次进入标记阶段时视图为M0，如果对象被GC标记线程或者应用线程访问过，那么就将对象的地址视图从Remapped调整为M0。所以在标记阶段结束之后，对象的地址要么是M0视图，要么是Remapped。如果对象的地址是M0视图，那么说明对象时活跃的；如果对象的地址是Remapped视图，说明对象是不活跃的。

**并发转移阶段**：标记结束后就进入转移阶段，此时地址视图再次被设置为Remapped。如果对象被GC转移线程或者应用线程访问过，那么就将对象的地址视图从M0调整为Remapped。

其实，再标记阶段存在两个地址视图M0和M1，上面的过程显示只用了一个地址视图，之所以设计成两个，是为了区别前一次标记和当前标记。也即，第一次进入并发标记阶段后，地址视图调整为M1，而非M0。

着色指针和读屏障技术不仅应用在并发转移阶段，还应用在并发标记阶段：将对象设置为已标记，传统的垃圾回收器需要进行一次内存访问，并将对象存活信息放在对象头中；而在ZGC中，只需要设置指针地址的第42-45位即可，并且因为是寄存器访问，所以速度比访问内存更快。

![image-20230423160203752](https://github.com/chou401/pic-md/raw/master/img/image-20230423160203752.png)

- **标记**

  从GC Roots出发，标记活跃对象，此时内存中存在或与对象和垃圾对象。

  在标记阶段，与G1基本相同，唯一的不同在于，在初始标记的时候，G1只能单线程进行，而ZGC为了追求性能，使用多线程（多个GC线程）进行。

  ZGC的再标记过程与G1的最终标记是一样，都是为了处理漏标对象，也是通过**原始快照**（STAB）算法解决，因为内存布局都是基于Region/Page。

  染色指针，通过M0、M1和Remapped来标记指针的状态，其中M0和M1主要用于区分连续的两次GC，ZGC的整个GC过程涉及到两次连续的GC，并不是一次GC就把所有工作都做了。

  我们给M0、M1和Remapped三种状态赋予不同的颜色，方便理解：

  ![image-20230423150650176](https://github.com/chou401/pic-md/raw/master/img/image-20230423150650176.png)

  假设现在有四个对象，都在小页面（对象大小<2MB）A中，因为此时还没有开始进行GC，所以所有引用的指针都处于Remapped状态：

  ![image-20230423153653755](https://github.com/chou401/pic-md/raw/master/img/image-20230423153653755.png)

  经过初次标记阶段厚，对象A被GC Root 引用，对象D被任何对象引用，A对象引用的指针颜色发生变化：

  ![image-20230423154450713](https://github.com/chou401/pic-md/raw/master/img/image-20230423154450713.png)

  然后进行并发标记，并发标记过程中，将所有存活对象的指针状态全部改为M0，由于D对象不可达，所以它的指针颜色还是蓝色，但在并发标记阶段，B对象又引用了一个新的对象E，因为是新的对象还没有被GC扫描过，所以指针颜色是蓝色：

  ![image-20230423154608727](https://github.com/chou401/pic-md/raw/master/img/image-20230423154608727.png)

  而在重新标记阶段，根据原始快照（STAB）会从B对象继续开始扫描，然后把E对象的指针变为绿色：

  ![image-20230423154643033](https://github.com/chou401/pic-md/raw/master/img/image-20230423154643033.png)

  至此标记阶段就完成了，剩下的对象指针为Remapped的都是垃圾对象，转移阶段进行回收。

  

- **转移**

  将活跃对象转移（复制）到新的内存上，原来的内存空间可回收，在并发转移阶段准备阶段，如果发现某个页全部都是垃圾对象，直接在该过程就把这些区域全部回收了。

  在并发转移阶段，会分析最有价值的GC分页，这个过程类似于G1筛选回收阶段的澄碧于收益比分析。

  在初始转移阶段，只会转移初始标记的存活对象，同时做对象的重定位，假设小页面A的对象都要转移到小页面B中，经过初始转移后，对象的位置及指针颜色如下：

  ![image-20230423163652891](https://github.com/chou401/pic-md/raw/master/img/image-20230423163652891.png)

  因为A对象转移到新的页面时进行了重定位，所以A对象的指针状态变为了Remapped蓝色。但对B对象的引用指针还是绿色。

  然后进行并发转移，把B、C、E对象也都转移到小页面B中：

  ![image-20230423165819998](https://github.com/chou401/pic-md/raw/master/img/image-20230423165819998.png)

  在并发转移阶段，只会把B、C、E对象转移到新的小页面B中，并不会修改它们对应的引用指针，也就是B对C的引用还指向原来小页面中的旧地址，并没有对转移对象做重定位。

  但对象既然转移了，那肯定需要根据之前旧的地址找到新的地址，所以每个页面都会维护一张**转发表**，这个转发表就记录了指针旧地址到新地址的映射。

  **注：对象转移与转发表插入记录这是一个原子操作，要么都成功，要么都失败。**

- **重定位**

  因为活跃对象的地址发生了变化，所以所有指向对象老地址的指针需要调整到新的对象地址上。

  在第一次GC的时候，只是用了M0，在第二次GC的时候，就会用到M1，这两个标志位就是为了区分两次不同的垃圾回收的。

  在第一次GC完成和第二次GC开始的间隙，A对象又引用了一个新的对象F，如下图所示：

  ![image-20230423165706646](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20230423165706646.png)

  因为是一个新创建的对象，所以指针状态为Remapped。

  当第二次GC开始，由于上一次GC使用M0来表示存活对象，那么这一次就采用M1来标识存活对象，然后经过初始标记后，对象A的引用就变成了红色：

  ![image-20230423165640436](https://github.com/chou401/pic-md/raw/master/img/image-20230423165640436.png)

  然后进行并发标记，在并发标记的过程中，因为F对象的指针为蓝色，就将其直接改为红色。当对象A扫描引用B时，发现它的指针颜色为绿色，状态为M0，它就会到原来的小页面A的转发表取到对象B的新地址进行重定位，然后再从转发表删除B指针的记录，重定位和删除转发表同样也是原子操作。C对象和E对象的操作一样，经过并发标记厚，新的对象布局如下：

  ![image-20230423170212287](https://github.com/chou401/pic-md/raw/master/img/image-20230423170212287.png)

  然后依次类推，下一次GC做标记时，在使用M0来标记存活对象。

  为什么要把重定向放到下一次GC的并发标记过程来做呢？

  重定向需要遍历所有存活对象，而下一次GC并发标记的时候也需要遍历所有存活，直接利用并发标记过程中遍历对象顺带做重定位减少一次扫描全部存活对象的开销，这样可以非常显著地提高垃圾收集性能。

##### GC时机

- **预热规则**

  服务刚启动时出现，一般不需要关注。日志中关键字是“Warmup”。

  JVM启动预热，如果从来没有发生过GC，则在堆内存使用超过**10%、20%、30%**时，分别触发一次GC，以收集GC数据。

- **基于分配速率的自适应算法**

  最主要的GC触发方式（默认方式），其算法原理可简单描述为**ZGC根据近期的对象分配速率以及GC时间，计算出当内存占用达到什么阈值时触发下一次GC**。通过 ZAllocationSpikeTolerance 参数控制阈值大小，该参数默认2，数值越大，越早的触发GC。日志中关键字 **Allocation Rate**。

- **基于固定时间间隔**

  通过 ZCollectionInterval 控制，适合应对突增流量场景。

  流量平稳变化时，自适应算法可能在堆使用率达到95%以上才触发GC。流量突增时，自适应算法触发的时机可能会过晚，导致部分线程阻塞。我们通过调整此参数解决流量突增场景的问题，比如定时任务、秒杀场景。

- **主动触发**

  类似于固定间隔规则，但时间间隔不固定，是ZGC自行算出来的时机，服务因为已经加了基于固定时间间隔的触发机制，所以通过 -ZProactive 参数将该功能关闭，以免GC频繁，影响服务可用性。

- **阻塞内存分配请求触发**

  当垃圾来不及回收，垃圾将堆占满时，会导致部分线程阻塞。我们应当避免出现这种触发方式。日志中关键字是 **Allocation Stall**。

- **外部触发**

  代码中显式调用 System.gc() 触发。日志中关键字是 **System.gc()**。

- **元数据分配触发**

  元数据区不足时导致，一般不需要关注。日志中关键字是 **Metadata GC Threshold**。

##### 参数设置

ZGC优势不仅在于其超低的STW停顿，也在于其参数的简单，绝大部分生产场景都可以自适应。当然，极端情况下，还是有可能需要对ZGC个别参数做个调整，大致可以分为三类：

- **堆大小**

  Xms。当分配速率过高，超过回收速率，造成堆内存不够时，会触发**Allocation Stall**，这类 Stall 会减缓当前的用户线程。因此，当我们在GC日志中看到 **Allocation Stall**，通常可以认为堆空间偏下或者 concurrent gc threads 数偏小。

- **GC 触发时机**

  ZAllocationSpikeTolerance，ZCollectionInterval。

  ZAllocationSpikeTolerance 用来估算当前的堆内存分配速率，在当前剩余的堆内存下，ZAllocationSpikeTolerance 越大，估算的达到OOM的时间越快，ZGC就会更早地进行触发GC。ZCollectionInterval 用来指定GC发生的间隔，以秒为单位触发 GC。

- **GC线程**

  ParallelGCThreads，ConcGCThreads。ParallelGCThreads是设置STW任务的GC线程数目，默认为CPU个数的60%。

  ConcGCThreads 是并发阶段GC线程的数目，默认为CPU个数的12.5%。增加GC线程数目，可以加快GC完成任务，减少各个阶段的时间，但也会增加CPU的抢占开销，可根据生产情况调整。

#### 如何选择垃圾回收器

- 单CPU或小内存，单机程序

  > -XX:+UseSerialGC

- 多CPU，需要最大吞吐量，如后台计算型应用

  > -XX:+UseParallelGC或者
  >
  > -XX:+UseParallelOldGC

- 多CPU，追求低停顿时间，需快速响应如互联网应用

  > -XX:+UseConcMarkSweepGC
  >
  > -XX:+ParNewGC



## 分布式

## MQ

### 概述

![image-20230428172550591](https://github.com/chou401/pic-md/raw/master/img/image-20230428172550591.png)

#### 介绍

消息队列是典型的：生产者、消费者模型。生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入，这样就实现了生产者和消费者的解耦。

本质上说消息队列就是一个队列结构的中间件，也就是说消息放入这个中间件之后就可以直接返回，并不需要系统立即处理，而另外会有一个程序读取这些数据，并按顺序进行逐次处理。

也就是说当你遇到一个并发特别大并且耗时特别长同时还不需要立即返回处理结果，使用消息队列可以解决这类问题。

消息队列主要解决了应用耦合、异步处理、流量削锋等问题。

当前使用较多的消息队列有 RabbitMQ、RocketMQ、ActiveMQ、Kafka、ZeroMQ、MetaMQ等，而部分数据库如 Redis、Mysql以及PhxSQL也可实现消息队列的功能。

#### 应用场景

- **数据冗余：**比如订单系统，后续需要严格的进行数据转换和记录，消息队列可以把这些数据持久化的存储在队列中，然后有订单，后续处理程序进行获取，后续处理完之后在把这条记录进行删除来保证每一条记录都能够处理完成。
- **系统解耦：**使用消息系统之后，入队系统和出队系统是分开的，也就说只要一天崩溃了，不会影响另外一台系统正常运转。
- **流量削锋：**例如秒杀和抢购，我们可以配合缓存来使用消息队列，能够有效的顶住瞬间访问量，防止服务器承受不住导致崩溃。
- **异步通信：**消息本身使用入队之后可以直接返回。
- **扩展性：**例如订单队列，不仅可以处理订单，还可以给其他业务使用。
- **排序保证：**有些场景需要按照产品的顺序进行处理比如单进单出从而保证数据按照一定的顺序处理，使用消息队列是可以的。

以上都是消息队列常见的使用场景，消息队列只是一个中间件，可以配合其他产品进行使用。

- **消息通讯：**消息通讯是指，消息队列一般都内置了搞笑的通信机制，因此也可以用作消息通讯。比如实现点对点消息队列，或者聊天室等。

![image-20230428140353787](https://github.com/chou401/pic-md/raw/master/img/image-20230428140353787.png)

以上实际是消息队列的两种消息模式，点对点或发布订阅模式。

**消息队列的缺点**

- 降低系统的可用性：系统引入的外部依赖越多，越容易挂掉。
- 系统复杂度提高：使用MQ后可能需要保证消息没有被重复消费、处理消息丢失的情况、保证消息传递的顺序性等等问题。
- 一致性问题：A系统处理完了直接返回成功了，但问题是：要是B、C、D三个系统那里，B和D两个系统写库成功了，结果C系统写库失败了，就造成了数据不一致了。

**如何保证消息队列的顺序性**

- 生产者有序的情况下，单线程消费来保证消息的顺序性。
- 生产者无序的情况下，对消息进行编号，消费者处理时根据编号判断顺序。多个消费者，可以考虑增加分布式锁。

**如何保证消息不被重复消费/如何保证消息的幂等性**

- 生产者发送每条数据的时候，里面添加一个全局唯一的id，消费者消费到消息后，先根据消息id去redis中查询，如果redis不存在，就处理消息，然后将消息id写入redis。如果redis中存在，说明消息已经消费过，就不用处理。
- 基于数据库的唯一键，保证重复数据不会重复插入。因为有唯一键的约束，所以重复数据只会插入报错，不会导致数据库中出现脏数据。



#### AMQP 和 JMS

##### AMQP

###### 简介

AMQP，即Advanced Message Queuing Protocol（高级消息队列协议），一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计，基于此协议的客户端与消息中间件传递消息，不受客户端/中间件不同产品、不同开发语言等条件的限制。该协议是一种二进制协议，提供客户端应用于消息中间件之间异步、安全、高效的交互。相对于我们常见的REST API，AMQP更容易实现，可以降低开销，同时灵活性高，可以轻松的添加负载平衡和高可用性的功能，并保证消息传递，在性能上AMQP协议也相对更好一些。

通俗来说，在异步通讯中，消息不会立刻到达接收方，而是被存放到一个容器中，当满足一定的条件之后，消息会被容器发送给接收方，这个容器即消息队列，而完成这个功能需要双方和容器以及其中的各个组件遵守统一的约定和规则，AMQP就是这样的一种协议，消息发送与接收的双方遵守这个协议可以实现异步通讯。这个协议约定了消息的格式和工作方式。

###### 核心组成

- **消息（Message）**：即客户端与消息中间件传送的数据。
- **生产者（Producer）**：消息生产者。
- **消费者（Consumer）**：消息消费者。
- **连接（Connection）**：一个网络连接，比如TCP/IP连接。AMQP连接通常是长连接，当一个应用不再需要连接到AMQP代理的时候，需要释放掉 AMQP 连接，而不是直接将TCP连接关闭。
- **信道（Channel）**：网络信道，是建立在Connection连接之上的一种轻量级的连接，可以创建多个信道。
- **交换机（Exchange）**：接收消息，并将消息路由转发给消息队列。
- **虚拟主机（Virtual Host）**：进行逻辑隔离，一个虚拟主机可以创建若干个交换机和队列。
- **绑定（Binding）**：交换机和队列之间的虚拟连接。
- **路由键（Routing Key）**：路由规则，虚拟机可以用来确定如何路由一个特定的消息。
- **队列（Queue）**：存储即将被消费者消费掉的消息。
- **中间件（Broker ）**：实现AMQP实体服务，比如常见的RabbitMQ、Azure Service Bus等。

###### 工作过程

1. 生产者发布消息，经由交换机。
2. 交换机根据路由规则将收到的消息分发给与该交换机绑定的队列。
3. 最后消息中间件会将消息投递给订阅了此队列的消费者，或者消费者按照需求自行获取。

##### JMS

###### 简介

JMS即Java消息服务（Java Message Service）应用程序接口，是一个Java平台中关于面向消息中间件（MOM）的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。Java消息服务是一个与具体平台无关的API，绝大多数MOM提供商都对JMS提供支持（百度百科给出的概述）。我们可以简单的理解：两个应用程序之间需要进行通信，我们使用一个JMS服务，进行中间的转发，通过JMS 的使用，我们可以解除两个程序之间的耦合。

###### 优势

1. Asynchronous（异步）

   JMS is asynchronous by default. So to receive a message, the client is not required to send the request. The message will arrive automatically to the client as they become available.（JMS 原本就是一个异步的消息服务，客户端获取消息的时候，不需要主动发送请求，消息会自动发送给可用的客户端）。

2. Reliable（可靠）

   JMS provides the facility of assurance that the message will delivered once and only once. You know that duplicate messages create problems. JMS helps you avoiding such problems.（JMS保证消息只会递送一次。大家都遇到过重复创建消息问题，而JMS能帮你避免该问题。）

###### JMS 的消息模型

JMS具有两种通信模式：

1. Point-to-Point Messaging Domain （点对点）
2. Publish/Subscribe Messaging Domain （发布/订阅模式）

在JMS API出现之前，大部分产品使用“点对点”和“发布/订阅”中的任一方式来进行消息通讯。JMS定义了这两种消息发送模型的规范，它们相互独立。任何JMS的提供者可以实现其中的一种或两种模型，这是它们自己的选择。JMS规范提供了通用接口保证我们基于JMS API编写的程序适用于任何一种模型。

**Point-to-Point Messaging Domain（点对点通信模型）**

- 在点对点通信模式中，应用程序由消息队列，发送方，接收方组成。每个消息都被发送到一个特定的队列，接收者从队列中获取消息。队列保留着消息，直到他们被消费或超时

- **特点**
  - 每个消息只要一个消费者
  - 发送者和接收者在时间上是没有时间的约束，也就是说发送者在发送完消息之后，不管接收者有没有接受消息，都不会影响发送方发送消息到消息队列中。
  - 发送方不管是否在发送消息，接收方都可以从消息队列中去到消息（The receiver can fetch message whether it is running or not when the sender sends the message）
  - 接收方在接收完消息之后，需要向消息队列应答成功

**Publish/Subscribe Messaging Domain（发布/订阅通信模型）**

- 在发布/订阅消息模型中，发布者发布一个消息，该消息通过topic传递给所有的客户端。该模式下，发布者与订阅者都是匿名的，即发布者与订阅者都不知道对方是谁。并且可以动态的发布与订阅Topic。Topic主要用于保存和传递消息，且会一直保存消息直到消息被传递给客户端。
- **特点**
  - 一个消息可以传递个多个订阅者（即：一个消息可以有多个接受方）
  - 发布者与订阅者具有时间约束，针对某个主题（Topic）的订阅者，它必须创建一个订阅者之后，才能消费发布者的消息，而且为了消费消息，订阅者必须保持运行的状态。
  - 为了缓和这样严格的时间相关性，JMS允许订阅者创建一个可持久化的订阅。这样，即使订阅者没有被激活（运行），它也能接收到发布者的消息。

###### JMS接收消息

在JMS中，消息的产生和消息是异步的。对于消费来说，JMS的消息者可以通过两种方式来消费消息。

1. 同步（Synchronous）

   在同步消费信息模式模式中，订阅者/接收方通过调用 receive（）方法来接收消息。在receive（）方法中，线程会阻塞直到消息到达或者到指定时间后消息仍未到达。

2. 异步（Asynchronous）

   使用异步方式接收消息的话，消息订阅者需注册一个消息监听者，类似于事件监听器，只要消息到达，JMS服务提供者会通过调用监听器的onMessage()递送消息。

###### JMS编程模型

1. 管理对象（Administered objects）-连接工厂（Connection Factories）和目的地（Destination）

   - Connection Factories

     创建Connection对象的工厂，针对两种不同的jms消息模型，分别有QueueConnectionFactory和TopicConnectionFactory两种。可以通过JNDI来查找ConnectionFactory对象。客户端使用一个连接工厂对象连接到JMS服务提供者，它创建了JMS服务提供者和客户端之间的连接。JMS客户端（如发送者或接受者）会在JNDI名字空间中搜索并获取该连接。使用该连接，客户端能够与目的地通讯，往队列或话题发送/接收消息。

     ```java
     QueueConnectionFactory queueConnFactory = (QueueConnectionFactory) initialCtx.lookup ("primaryQCF");
     Queue purchaseQueue = (Queue) initialCtx.lookup ("Purchase_Queue");
     Queue returnQueue = (Queue) initialCtx.lookup ("Return_Queue");
     ```

   - Destination

     目的地指明消息被发送的目的地以及客户端接收消息的来源。JMS使用两种目的地，队列和话题。如下代码指定了一个队列和话题：

     - 创建一个队列Session：

       ```java
       QueueSession ses = con.createQueueSession (false, Session.AUTO_ACKNOWLEDGE);  //get the Queue object  
       Queue t = (Queue) ctx.lookup ("myQueue");  //create QueueReceiver  
       QueueReceiver receiver = ses.createReceiver(t); 
       ```

     - 创建一个Topic Session：

       ```java
       QueueSession ses = con.createQueueSession (false, Session.AUTO_ACKNOWLEDGE);  //get the Queue object  
       Queue t = (Queue) ctx.lookup ("myQueue");  //create QueueReceiver  
       QueueReceiver receiver = ses.createReceiver(t);
       ```

2. 连接对象（Connections）

   Connection表示在客户端和JMS系统之间建立的链接（对TCP/IP socket的包装）。Connection可以产生一个或多个Session。跟ConnectionFactory一样，Connection也有两种类型：QueueConnection和TopicConnection。连接对象封装了与JMS提供者之间的虚拟连接，如果我们有一个ConnectionFactory对象，可以使用它来创建一个连接。

   ```java
   Connection connection = connectionFactory.createConnection();
   ```

3. 会话（Sessions）

   Session 是我们对消息进行操作的接口，可以通过session创建生产者、消费者、消息等。Session 提供了事务的功能，如果需要使用session发送/接收多个消息时，可以将这些发送/接收动作放到一个事务中。

   我们可以在连接创建完成之后创建session：

   ```java
   Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
   ```

4. 消息生产者（Message Producers）

   消息生产者由Session创建，用于往目的地发送消息。生产者实现MessageProducer接口，我们可以为目的地、队列或话题创建生产者。

   ```java
   MessageProducer producer = session.createProducer(dest);
   MessageProducer producer = session.createProducer(queue);
   MessageProducer producer = session.createProducer(topic);
   ```

5. 消息消费者（Message Consumers）

   消息消费者由Session创建，用于接收被发送到Destination的消息。

   ```java
   MessageConsumer consumer = session.createConsumer(dest);
   MessageConsumer consumer = session.createConsumer(queue);
   MessageConsumer consumer = session.createConsumer(topic);
   ```

6. 消息监听者（Message Listeners）

   消息监听器。如果注册了消息监听器，一旦消息到达，将自动调用监听器的onMessage方法。EJB中的MDB（Message-Driven Bean）就是一种MessageListener。



### 常见MQ

现在比较常见的MQ产品主要是ActiveMQ、RabbitMQ、ZeroMQ、Kafka、MetaMQ、RocketMQ等。

常见的消息队列有以下几种：  

1. Apache ActiveMQ：基于JMS的消息队列，支持多种配置。  
2. RabbitMQ：一个可靠的开源消息队列系统，支持多种编程语言和操作系统。  
3. Apache Kafka：一种高吞吐量、分布式发布订阅消息系统，可以处理大量数据并保持持久性。  
4. RocketMQ：阿里巴巴开源的低延迟、高可靠、可伸缩的消息队列服务。  
5. ZeroMQ：一种高性能、高可用的封装层，可以在不同的消息队列上运行。  
6. NATS：一种基于云计算的消息队列系统，具有高性能和可扩展性。  
7. Redis：一种内存数据库，支持多种数据结构和发布订阅功能，可以实现消息队列的功能。



#### ZeroMQ

ZeroMQ 号称是“史上最快的消息队列”，基于 C 语言开发，可以在任何平台通过任何代码连接，通过 inproc、IPC、TCP、TIPC、多播传送消息，支持发布-订阅、推-拉、共享队列等模式，高速异步 I/O 引擎。

根据官方的说法，ZeroMQ 是一个简单好用的传输层，像框架一样的可嵌入的 Socket 类库，使 Socket 编程更加简单、简洁、性能更高，是专门为高吞吐量/低延迟的场景开发。**ZeroMQ 与其他 MQ 有着本质的区别，它根本不是消息队列服务器，更类似于一个底层网络通讯库**，对原有 Socket API 进行封装，在使用引入对应的jar包即可，可谓是相当灵活。

同时，因为它的简单灵活，如果我们想作为消息队列使用的话，需要开发大量代码。而且，ZeroMQ 不支持消息持久化，其定位并不是安全可靠的消息传输，所以还需要自己编码保证可靠性。简而言之，ZeroMQ 很强大，但是想用好需要自己实现。

#### RabbitMQ

RabbitMQ 2007年发布，是一个在 AMQP 基础上完成的，可复用的企业消息系统，是当前最主流的消息中间件之一。

##### 代理效应

代理和负载平衡器在客户端与其目标节点之间引入了额外的网络跃点（甚至多个）。中介也可以成为网络争用点：它们的吞吐量将成为整个系统的限制因素。因此，代理和负载平衡器的网络带宽超额配置和吞吐量监视非常重要。

当中间商在一段时间内没有任何活动时，它们也可以终止“空闲” TCP连接。在大多数情况下，这是不可取的。此类事件将导致服务器端的突然关闭连接日志消息以及客户端的I / O异常。

在连接上启用心跳后，将导致周期性的轻型网络流量。因此，心跳具有保护客户端连接的副作用，该客户端连接可能会闲置一段时间，以防止代理和负载平衡器过早关闭。

从10到30秒的心跳超时将经常产生周期性的网络流量（大约每5到15秒一次），以满足大多数代理工具和负载均衡器的默认设置。太低的值将产生误报。



##### 主要特性

1. **可靠性：**提供了多种技术可以让你在性能和可靠性质检进行权衡。这些技术包括持久性机制、投递确认、发布者证实和高可用性机制；
2. **灵活的路由：**消息在到达队列前是通过交换机进行路由的。RabbitMQ 为典型的路由逻辑提供了多钟内置交换机类型。如果你有更复杂的路由需求，可以将这些交换机组合起来使用，你甚至可以实现自己的交换机类型，并且当做 RabbitMQ 的插件使用；
3. **消息集群：**在相同局域网中的多个 RabbitMQ 服务器可以聚合在一起，作为一个独立的逻辑代理来使用；
4. **队列高可用：**队列可以在集群中的机器上进行镜像，以确保在硬件问题下还保证消息安全；
5. **多种协议的支持：**支持多种消息队列协议；
6. **多语言支持：**服务器端用 Erlang 语言编写，支持只要是你能想到的所有编程语言；
7. **管理界面：**RabbitMQ 有一个易用的用户界面，使得用户可以监控和管理消息 Broker 的许多方面；
8. **跟踪机制：**如果消息异常，RabbitMQ 提供消息跟踪机制，使用者可以找出发生了什么；
9. **插件机制：**提供了许多插件，来从多方面进行扩展，也可以编写自己的插件。

使用 RabbitMQ 需要：

- Erlang 语言包
- RabbitMQ 安装包

RabbitMQ 可以运行在 Erlang 语言所支持的平台上：

- Solaris
- BSD
- Linux
- MacOSX
- TRU64
- Windows NT/2000/XP/Vista/Windows 7/Windows 8
- Windows Server 2003/2008/2012
- Windows 95, 98
- VxWorks

##### 优点

1. 由于 Erlang 语言的特性，MQ 性能较好，高并发；
2. 健壮、稳定、易用、跨平台、支持多种语言、文档齐全；
3. 有消息确认机制和持久化机制，可靠性高；
4. 高度可定制的路由；
5. 管理界面较丰富，在互联网公司也有较大规模的应用；
6. 社区活跃度高。

##### 缺点

1. 尽管结合 Erlang 语言本身的并发优势，性能较好，但是不利于做二次开发和维护；
2. 实现了代理架构，意味着消息在发送到客户端之前可以在中央节点上排队，此特性使得 RabbitMQ 易于使用和部署，但是使得其运行速度较慢，因为中央节点增加了延迟，消息封装后也比较大；
3. 需要学习比较复杂的接口和协议，学习和维护成本较高。

##### 工作原理

![image-20230517155538862](https://github.com/chou401/pic-md/raw/master/img/image-20230517155538862.png)

- **Broker**：接收和分发消息的应用，RabbitMQ Server就是 Message Broker。
- **Virtual Host**：出于多租户和安全因素设计的，把 AMQP 的基本组件划分到一个虚拟的分组中，类似于网络中的 namespace 概念。当多个不同的用户使用同一个 RabbitMQ server 提供的服务时，可以划分出多个vhost，每个用户在自己的 vhost 创建 exchange／queue 等。
- **Connection**：publisher／consumer 和 broker 之间的 TCP 连接。
- **Channel**：如果每一次访问 RabbitMQ 都建立一个 Connection，在消息量大的时候建立 TCP Connection的开销将是巨大的，效率也较低。Channel 是在 connection 内部建立的逻辑连接，如果应用程序支持多线程，通常每个thread创建单独的 channel 进行通讯，AMQP method 包含了channel id 帮助客户端和message broker 识别 channel，所以 channel 之间是完全隔离的。Channel 作为轻量级的 Connection 极大减少了操作系统建立 TCP connection 的开销。
- **Exchange**：message 到达 broker 的第一站，根据分发规则，匹配查询表中的 routing key，分发消息到queue 中去。常用的类型有：direct (point-to-point), topic (publish-subscribe) and fanout (multicast)。
- **Queue**：消息最终被送到这里等待 consumer 取走。
- **Binding**：exchange 和 queue 之间的虚拟连接，binding 中可以包含 routing key。Binding 信息被保存到 exchange 中的查询表中，用于 message 的分发依据。



##### 如何实现生产者和消费者



##### 交换机类型

- **Direct Exchange（直连交换机）**

  路由键与队列完全匹配交换机。此种类型交换机，通过 RoutingKey 路由键将交换机和队列进行绑定，消息被发送到 exchange 时，需要根据消息的 RoutingKey 进行匹配，只将消息发送到完全匹配到此 RoutingKey 的队列（如果匹配了多个队列，则每个队列都会收到相同的消息）。

  比如：如果一个队列绑定到交换机要求路由键为 “key”，则只转发 RoutingKey 标记为 “key” 的消息，不会转发 “key1”，也不会转发 “key2” 等等。它是完全匹配、单播的模式。

  ![image-20230519161049311](https://github.com/chou401/pic-md/raw/master/img/image-20230519161049311.png)

- **Fanout Exchange（扇型交换机）**

  Fanout，此种交换机，会将消息分发给所有绑定了次交换机的队列，此时 RoutingKey 参数无效。这个模式类似于广播。它是所有交换机速度最快的。

  Fanout 类型交换机下发送消息一条，无论 RoutingKey 是什么，queue1，queue2，queue3，queue4 都可以收到消息。

  

  ![image-20230519161221269](https://github.com/chou401/pic-md/raw/master/img/image-20230519161221269.png)

- **Topic Exchange（主题交换机）**

  应用范围最广的交换机类型，消息队列通过消息主题与交换机绑定。一个队列可以通过多个主题与交换机绑定，多个消息队列也可以通过相同消息主题和交换机绑定。并且可以通过通配符（*或者#）进行多个消息主题的适配。

  Topic，主题类型交换机，此种交换机与 Direct 类似，也是需要通过 RoutingKey 路由键进行匹配分发，区别在于 Topic 可以进行模糊匹配，Direct 是完全匹配。

  1. Topic 中，将 RoutingKey 通过 “.” 来分成多个部分。
  2. “*” 代表一个部分。
  3. “#” 代表0个或多个部分（如果绑定的路由键为 “#” 时，则接收所有消息，路由键所有都匹配）。

  ![image-20230519162427816](https://github.com/chou401/pic-md/raw/master/img/image-20230519162427816.png)

  然后发送一条消息，RoutingKey 为 “key1.key2.key3.key4”，那么根据 “.” 将这个路由键分为了四个部分，此条路由键将会匹配：

  1. key1.key2.key3.*：成功匹配，因为 * 可以代表一个部分。
  2. key1.#：成功匹配，因为 # 可以代表0或多个部分。
  3. *.key3.*.key4：成功匹配，因为第一和第三部分分别为key1和key3，且为四个部分，刚好匹配。
  4. #.key3.key4：成功匹配，#可以代表多个部分，正好匹配中了key1和key2。

  如果发送消息 RoutingKey 为 “key1”，那么将只能匹配中 key1.#，#可以代表0个部分。

  

- **Headers Exchange（头交换机）**

  与 RoutingKey 无关，匹配机制是匹配消息头中的属性信息。在绑定消息队列与交换机之前声明一个map键值对，通过这个map对象实现消息队列和交换机的绑定。当消息发送到 RabbitMQ 时会取到该消息的 headers 与 exchange 绑定时指定的键值对进行匹配，如果完全匹配则消息会路由到该队列，否则不会路由到该队列。

  匹配规则 x-match 有下列两种类型：

  - x-match = all：表示所有的键值对都匹配才能接收到消息。
  - x-match = any：表示只要有键值对匹配就能接收到消息。

![](https://github.com/chou401/pic-md/raw/master/img/image-20230523095354400.png)

##### 镜像队列

###### 背景

单节点的 RabbitMQ 存在性能上限，可以通过垂直或者水平扩容的方式增加 RabbitMQ的吞吐量。垂直扩容指的是提高 CPU 和内存的规格；水平扩容指部署 RabbitMQ 集群。

通过将单个节点的队列相对平均地分配到集群的不同节点，单节点的压力被分散，RabbitMQ 可以充分利用多个节点的计算和存储资源，以提升消息的吞吐量。

但是多节点的集群并不意味着有更好的可靠性--每个队列仍只存在于一个节点，当这个节点故障，这个节点上的所有队列都不再可用。

在 3.8 以前的版本，RabbitMQ 通过镜像队列（Classic Queue Mirroring）来提供高可用性。但镜像队列存在很大的局限性，在 3.8 之后的版本 RabbitMQ 推出了 Quorum queues 来替代镜像队列，在之后的版本中镜像队列将被移除。

镜像队列通过将一个队列镜像（消息广播）到其他节点的方式来提升消息的高可用性。当主节点宕机，从节点会提升为主节点继续向外提供服务。

###### 描述

RabbitMQ 以队列维度提供高可用的解决方案——镜像队列。

配置镜像队列规则后，新创建的队列按照规则成为镜像队列。每个镜像队列都包含一个主节点（Leader）和若干个从节点（Follower），其中只有主节点向外提供服务（生产消息和消费消息），从节点仅仅接收主节点发送的消息。

从节点会准确地按照主节点执行命令的顺序执行动作，所以从节点的状态与主节点应是一致的。

###### 配置方法

使用策略（Policy）来配置镜像策略，策略使用正则表达式来配置需要应用镜像策略的队列名称，以及在参数中配置镜像队列的具体参数。

按此步骤创建镜像策略，该策略为所有 `mirror_` 开头的队列创建 3 副本镜像。

![image-20230523112017966](https://github.com/chou401/pic-md/raw/master/img/image-20230523112017966.png)

创建完的策略如下图显示：

![image-20230523112042677](https://github.com/chou401/pic-md/raw/master/img/image-20230523112042677.png)

**参数解释：**

- Name: policy的名称，用户自定义。
- Pattern: queue的匹配模式（正则表达式）。`^`表示所有队列都是镜像队列。
- Definition: 镜像定义，包括三个部分ha-sync-mode、ha-mode、ha-params。
  - ha-mode: 指明镜像队列的模式，有效取值范围为all/exactly/nodes。
    - all：表示在集群所有的代理上进行镜像。
    - exactly：表示在指定个数的代理上进行镜像，代理的个数由ha-params指定。
    - nodes：表示在指定的代理上进行镜像，代理名称通过ha-params指定。
  - ha-params: ha-mode模式需要用到的参数。
  - ha-sync-mode: 表示镜像队列中消息的同步方式，有效取值范围为：automatic，manually。
    - automatic：表示自动向master同步数据。
    - manually：表示手动向master同步数据。
- Priority: 可选参数， policy的优先级。

###### 配置规则

配置完 Policy 后，创建新的队列，或者原有的的队列，如果队列名称符合 Policy 的匹配规则，则该队列会自动创建为镜像队列。

下图中 `mirror_queue` 匹配之前创建的镜像策略，为镜像队列。`normal_queue` 为普通队列。

![image-20230523112755735](https://github.com/chou401/pic-md/raw/master/img/image-20230523112755735.png)

镜像队列显示的蓝色 `+2` 表示同步副本数为 2 个。此处如果用红色显示，则表示为同步副本数

显示的 `mirror-policy` 为该队列应用的镜像策略。

点击队列名称可以进入查看队列详细信息，从中可以看出队列的主节点、从节点和镜像策略。

![image-20230523112856113](https://github.com/chou401/pic-md/raw/master/img/image-20230523112856113.png)

###### 配置参数

**镜像策略**

| **ha-mode** | **ha-params** | **结果**                                                     |
| ----------- | ------------- | ------------------------------------------------------------ |
| exactly     | count         | 集群中队列副本的数量（主队列加上镜像）。count值为1表示一个副本：只有主节点。如果主节点不可用，则其行为取决于队列是否持久化。count值为2表示两个副本：一个队列主队列和一个队列镜像。换句话说:“镜像数=节点数-1”。如果运行队列主服务器的节点变得不可用，队列镜像将根据配置的镜像提升策略自动提升到主服务器。如果集群中的可用节点数少于count，则将队列镜像到所有节点。如果集群中有多个计数节点，并且一个包含镜像的节点宕机，那么将在另一个节点上创建一个新镜像。使用’ exactly ‘模式和’ ha-promot-on-shutdown ': ’ always '可能是危险的，因为队列可以跨集群迁移，并在停机时变得不同步。 |
| all         | 不设置        | 队列跨集群中的所有节点镜像。当一个新节点被添加到集群中时，队列将被镜像到该节点。这个设置非常保守。建议设置的副本值为大多数节点`N / 2 + 1`。镜像到所有节点会给所有集群节点带来额外的负担，包括网络I/O、磁盘I/O和磁盘空间的使用。 |
| nodes       | 节点名称      | 队列被镜像到节点名中列出的节点。节点名是在rabbitmqctl cluster_status中出现的Erlang节点名；它们的形式通常是“rabbit@hostname”。如果这些节点名中有任何一个不是集群的一部分，则不构成错误。如果在声明队列时列表中的节点都不在线，则将在声明客户机连接的节点上创建队列。 |

**新镜像同步策略**

| **ha-sync-mode** | **说明**                                                     |
| ---------------- | ------------------------------------------------------------ |
| manual           | 这是默认模式。新队列镜像将不接收现有消息，它只接收新消息。一旦使用者耗尽了仅存在于主服务器上的消息，新的队列镜像将随着时间的推移成为主服务器的精确副本。如果主队列在所有未同步的消息耗尽之前失败，则这些消息将丢失。您可以手动完全同步队列，详情请参阅未同步的镜像部分。 |
| automatic        | 当新镜像加入时，队列将自动同步。值得重申的是，队列同步是一个阻塞操作。如果队列很小，或者您在RabbitMQ节点和ha-sync-batch-size之间有一个快速的网络，那么这是一个很好的选择。 |

**从节点晋升策略**

镜像队列主节点出现故障时，最老的从节点会被提升为新的主节点。如果新提升为主节点的这个副本与原有的主节点并未完成数据的同步，那么就会出现数据的丢失，而实际应用中，出现数据丢失可能会导致出现严重后果。

rabbitmq 提供了 ha-promote-on-shutdown，ha-promote-on-failure 两个参数让用户决策是保证队列的可用性，还是保证队列的一致性；两个参数分别控制正常关闭、异常故障情况下从节点是否提升为主节点，其可设置的值为 when-synced 和 always。

| **ha-promote-on-shutdown/ha-promote-on-failure** | **说明**                                       |
| ------------------------------------------------ | ---------------------------------------------- |
| when-synced                                      | 从节点与主节点完成数据同步，才会被提升为主节点 |
| always                                           | 无论什么情况下从节点都将被提升为主节点         |

> 这里要注意的是ha-promote-on-failure设置为always，插拔网线模拟网络异常的两个测试场景：当网络恢复后，其中一个会重新变为mirror，具体是哪个变为mirror，受cluster_partition_handling处理策略的影响。

> 例如两台节点A，B组成集群，并且cluster_partition_handling设置为autoheal，队列的master位于节点A上，具有全量数据，mirror位于节点B上，并且还未完成消息的同步，此时出现网络异常，网络异常后两个节点交互决策：如果节点A节点成为赢家，此时B节点内部会重启，这样数据全部保留不会丢失；相反如果B节点成为赢家，A需要重启，那么由于ha-prromote-on-failure设置为always，B节点上的mirror提升为master，这样就出现了数据丢失。

**主队列选择策略**

RabbitMQ中的每个队列都有一个主队列。该节点称为队列主服务器。所有队列操作首先经过主队列，然后复制到镜像。这对于保证消息的FIFO排序是必要的。

通过在策略中设置 `queue-master-locator` 键的方法可以定义主队列选择策略，这是常用的方法。

![image-20230523144411570](https://github.com/chou401/pic-md/raw/master/img/image-20230523144411570.png)

此外，也可以用队列参数 `x-queue-master-locator` 或配置文件中定义 `queue_master_locator` 的方式指定，此处不再赘述。

下面是该策略的可选参数列表：

| **queue-master-locator** | **说明**                       |
| ------------------------ | ------------------------------ |
| min-masters              | 选择承载最小绑定主机数量的节点 |
| client-local             | 选择客户机声明队列连接到的节点 |
| min-masters              | 随机选择一个节点               |



##### 负载均衡-HAProxy

对 RabbitMQ集群中的节点做负载均衡：

- 客户端负载均衡
- HAProxy实现负载均衡

###### 客户端负载均衡

要实现一个完整的负载均衡主要是实现以下功能：

- 请求需要按照规则打散到各个集群的节点。

- 节点的宕机需要负载均衡器自我感知并且进行剔除，这样就避免节点都宕掉了还在向宕掉的节点发送请求，导致大量的请求失败。

- 节点的新增其实还好，可以自我感知并上线，也可以手动配置。

- haproxy ip-hash

  整型的Hash算法使用的是Thomas Wang's 32 Bit / 64 Bit Mix Function ，这是一种基于位移运算的散列方法。基于移位的散列是使用Key值进行移位操作。通常是结合左移和右移。每个移位过程的结果进行累加，最后移位的结果作为最终结果。这种方法的好处是避免了乘法运算，从 

如果实现将请求打散到各个节点，负载均衡器需要遵循一定得规则，规则主要是一下几种：

- 轮询：将请求轮流到发送到后端的机器，不关系节点的实际连接数和负载能力。
- 加权轮询：对轮询的优化，考虑每个节点的性能，配置高的机器分配较高的权重，配置低的机器分配较低的权重，并将请求按照权重分配到后端节点。
- 随机法：通过随机算法，在众多节点中随机挑选一个进行请求。随着客户端调用服务端的次数增多，其实际效果越接近轮询。
- 加权随机法：对随机的优化，根据机器性能分配权重，按照权重访问后端节点。
- 源地址哈希法：根据客户端的IP地址，通过hash函数获取一个数值，用这个数值对后端节点数进行取模，这样在后端节点数保持不变的情况下，同一个客户端访问的 后端节点也是同一个。
- 最小连接数：根据后端节点的连接情况，动态选举一个连接积压最小的节点进行访问，尽可能的提高节点的利用率。

###### HAProxy实现负载均衡

**简介**

1. HAProxy 是一款提供高可用性、负载均衡以及基于TCP（第四层）和HTTP（第七层）应用的代理软件，支持虚拟主机，它是免费、快速并且可靠的一种解决方案。 HAProxy特别适用于那些负载特大的web站点，这些站点通常又需要会话保持或七层处理。HAProxy运行在时下的硬件上，完全可以支持数以万计的 并发连接。并且它的运行模式使得它可以很简单安全的整合进您当前的架构中， 同时可以保护你的web服务器不被暴露到网络上。
2. HAProxy 实现了一种事件驱动、单一进程模型，此模型支持非常大的并发连接数。多进程或多线程模型受内存限制 、系统调度器限制以及无处不在的锁限制，很少能处理数千并发连接。事件驱动模型因为在有更好的资源和时间管理的用户端(User-Space) 实现所有这些任务，所以没有这些问题。此模型的弊端是，在多核系统上，这些程序通常扩展性较差。这就是为什么他们必须进行优化以 使每个CPU时间片(Cycle)做更多的工作。
3. HAProxy 支持连接拒绝 : 因为维护一个连接的打开的开销是很低的，有时我们很需要限制攻击蠕虫（attack bots），也就是说限制它们的连接打开从而限制它们的危害。 这个已经为一个陷于小型DDoS攻击的网站开发了而且已经拯救了很多站点，这个优点也是其它负载均衡器没有的。
4. HAProxy 支持全透明代理（已具备硬件防火墙的典型特点）: 可以用客户端IP地址或者任何其他地址来连接后端服务器，这个特性仅在 Linux 2.4/2.6内核打了cttproxy补丁后才可以使用，这个特性也使得为某特殊服务器处理部分流量同时又不修改服务器的地址成为可能。

**四层负载均衡与七层负载均衡**

- **四层负载均衡**

  以常见的 TCP 应用为例，负载均衡器在接收到第一个来自客户端的 SYN 请求时，会通过设定的负载均衡算法选择一个最佳的后端服务器，同时将报文中目标 IP 地址修改为后端服务器 IP，然后直接转发给该后端服务器，这样一个负载均衡请求就完成了。从这个过程来看，一个 TCP 连接是客户端和服务器直接建立的，而负载均衡器只不过完成了一个类似路由器的转发动作。在某些负载均衡策略中，为保证后端服务器返回的报文可以正确传递给负载均衡器，在转发报文的同时可能还会对报文原来的源地址进行修改。

- **七层负载均衡**

  这里仍以常见的 TCP 应用为例，由于负载均衡器要获取到报文的内容，因此只能先代替后端服务器和客户端建立连接，接着，才能收到客户端发送过来的报文内容，然后再根据该报文中特定字段加上负载均衡器中设置的负载均衡算法来决定最终选择的内部服务器。纵观整个过程，七层负载均衡器在这种情况下类似于一个代理服务器。

  对比四层负载均衡和七层负载均衡运行的整个过程，可以看出，在七层负载均衡模式下， 负载均衡器与客户端及后端的服务器会分别建立一次 TCP 连接，而在四层负载均衡模式下， 仅建立一次 TCP 连接。由此可知，七层负载均衡对负载均衡设备的要求更高，而七层负载均衡的处理能力也必然低于四层模式的负载均衡。

**HAProxy配置详解**

HAProxy 配置文件根据功能和用途，主要有 5 个部分组成，但有些部分并不是必须的， 可以根据需要选择相应的部分进行配置。

- **global** 部分

  用来设定全局配置参数，属于进程级的配置，通常和操作系统配置有关。

  - **log**：全局的日志配置，local0 是日志设备，info 表示日志级别。其中日志级别有err、warning、info、debug 四种可选。这个配置表示使用 127.0.0.1 上的 rsyslog 服务中的local0 日志设备，记录日志等级为info。
  - **maxconn**：设定每个 haproxy 进程可接受的最大并发连接数，此选项等同于 Linux命令行选项“ulimit -n”。
  - **user/ group**：设置运行 haproxy 进程的用户和组，也可使用用户和组的 uid 和gid 值来替代。
  - **daemon**：设置 HAProxy 进程进入后台运行。这是推荐的运行模式。
  - **nbproc**：设置 HAProxy 启动时可创建的进程数，此参数要求将HAProxy 运行模式设置为“daemon”，默认只启动一个进程。根据使用经验，该值的设置应该小于服务器的 CPU 核数。创建多个进程，能够减少每个进程的任务队列，但是过多的进程可能会导致进程的崩溃。
  - **pidfile**：指定 HAProxy 进程的 pid 文件。启动进程的用户必须有访问此文件的权限。

- **defaults** 部分

  默认参数的配置部分。在此部分设置的参数值，默认会自动被引用到下面的 frontend、backend 和 listen 部分中，因此，如果某些参数属于公用的配置，只需在 defaults 部分添加一次即可。而如果在 frontend、backend 和 listen 部分中也配置了与 defaults 部分一样的参数，那么defaults 部分参数对应的值自动被覆盖。

  - **mode**：设置 HAProxy 实例默认的运行模式，有 tcp、http、health 三个可选值。

    - **tcp** 模式

      在此模式下，客户端和服务器端之间将建立一个全双工的连接，不会对七层报文做任何类型的检查，默认为 tcp 模式，经常用于 SSL、SSH、SMTP 等应用。

    - **http** 模式

       在此模式下，客户端请求在转发至后端服务器之前将会被深度分析，所有不与 RFC 格式兼容的请求都会被拒绝。

    - **health** 模式

      目前此模式基本已经废弃，不在多说。

  - **retries**：设置连接后端服务器的失败重试次数，连接失败的次数如果超过这里设置的值，HAProxy 会将对应的后端服务器标记为不可用。此参数也可在后面部分进行设置。

  - **timeout connect**：设置成功连接到一台服务器的最长等待时间，默认单位是毫秒，但也可以使用其他的时间单位后缀。

  - **timeout client**：设置连接客户端发送数据时最长等待时间，默认单位是毫秒，也可以使用其他的时间单位后缀。

  - **timeout server**：设置服务器端回应客户度数据发送的最长等待时间，默认单位是毫秒，也可以使用其他的时间单位后缀。

  - **timeout check**：设置对后端服务器的检测超时时间，默认单位是毫秒，也可以使用其他的时间单位后缀。

- **frontend** 部分

  此部分用于设置接收用户请求的前端虚拟节点。frontend 是在 HAProxy1.3 版本之后才引入的一个组件，同时引入的还有 backend 组件。通过引入这些组件，在很大程度上简化了 HAProxy 配置文件的复杂性。frontend 可以根据 ACL 规则直接指定要使用的后端。

  - **bind**：此选项只能在 frontend 和 listen 部分进行定义，用于定义一个或几个监听的套接字。bind 的使用格式为:bind [<address>:<port_range>] interface <interface>其中，address 为可选选项，其可以为主机名或IP 地址，如果将其设置为“*”或“0.0.0.0”，将监听当前系统的所有 IPv4 地址。port_range 可以是一个特定的 TCP 端口，也可是一个端口范围，小于 1024 的端口需要有特定权限的用户才能使用。interface 为可选选项，用来指定网络接口的名称，只能在 Linux 系统上使用。
  - **option httplog**：在默认情况下，haproxy 日志是不记录 HTTP 请求的，这样很不方便 HAProxy 问题的排查与监控。通过此选项可以启用日志记录 HTTP 请求。
  - **option forwardfor**：如果后端服务器需要获得客户端的真实  IP，就需要配置此参数。由于 HAProxy 工作于反向代理模式，因此发往后端真实服务器的请求中的客户端 IP 均为 HAProxy 主机的 IP，而非真正访问客户端的地址，这就导致真实服务器端无法记录客户端真正请求来源的 IP，而“X-Forwarded-For”则可用于解决此问题。通过使用“forwardfor”选项，HAProxy 就可以向每个发往后端真实服务器的请求添加“X-Forwarded-For”记录，这样后端真实服务器日志可以通过“X-Forwarded-For”信息来记录客户端来源 IP。
  - **option httpclose**：此选项表示在客户端和服务器端完成一次连接请求后，HAProxy 将主动关闭此 TCP 连接。这是对性能非常有帮助的一个参数。
  - **log global**：表示使用全局的日志配置，这里的“ global”表示引用在HAProxy 配置文件 global 部分中定义的 log 选项配置格式。
  - **default_backend**：#指定默认的后端服务器池，也就是指定一组后端真实服务器，而这些真实服务器组将在 backend 段进行定义。这里的htmpool 就是一个后端服务器组。

- **backend** 部分

  此部分用于设置集群后端服务集群的配置，也就是用来添加一组真实服务器，以处理前端用户的请求。添加的真实服务器类似于 LVS 中的real server 节点。

  - option redispatch：此参数用于 cookie 保持的环境中。在默认情况下，HAProxy会将其请求的后端服务器的 serverID 插入到 cookie 中，以保证会话的 SESSION 持久性。而如果后端的服务器出现故障，客户端的 cookie 是不会刷新的，这就出现了问题。此时，如果设置此参数，就会将客户的请求强制定向到另外一个健康的后端服务器上，以保证服务的正常。

  - option abortonclose：如果设置了此参数，可以在服务器负载很高的情况下， 自动结束掉当前队列中处理时间比较长的链接。

  - balance：此关键字用来定义负载均衡算法。目前 HAProxy 支持多种负载均衡算法，常用的有如下几种：

    | 算法         | 描述                                                         |
    | ------------ | ------------------------------------------------------------ |
    | roundrobin   | 是基于权重进行轮询调度的算法，在服务器的性能分布比较均匀的时候，这是一种最公平的、最合理的算法。此算法经常使用。 |
    | static-rr    | 也是基于权重进行轮询调度的算法，不过此算法为静态方法，在运行时调整其服务器严重不会生效。 |
    | source       | 是基于请求源IP的算法。此算法先对请求的源IP 进行hash 运算，然后将结果与后端服务器的权重总数相除后转发至某个匹配的后端服务器。这种方式可以使同一客户端 IP的请求始终被转发到某特定的后端服务器。 |
    | leastconn    | 此算法会将新的连接请求转发到具有最少连接数目的后端服务器。在会话时间较长的场景中推荐使用此算法，例如数据库复杂均衡等。此算法不适合会话时间比较短的环境中，例如基于 HTTP 的应用。 |
    | url          | 此算法会对部分或整个 URL 进行 hash 运算，再经过与服务器的总权重相除，最后转发到某台匹配的后端服务器上。 |
    | url_param    | 此算法会根据 URL 路径中的参数进行转发，这样可保证在后端真实服务器数量不变时，同一个用户的请求始终分发到同一机器上。 |
    | hdr(<name>): | 此算法根据 http 头进行转发，如果指定的 http 头名称不存在，则使用 roundrobin 算法进行策略转发。 |

  - cookie：表示允许向 cookie 插入 SERVERID，每台服务器的 SERVERID 可在下面的 server 关键字中使用 cookie 关键字定义。

  - option httpchk：此选项表示启用 HTTP 的服务状态检测功能。HAProxy 作为一款专业的负载均衡器，它支持对 backend 部分指定的后端服务节点的健康检查，以保证在后端 backend 中某个节点不能服务时，把从 frotend 端进来的客户端请求分配至 backend 中其他健康节点上，从而保证整体服务的可用性。“option httpchk”的用法如下：

    | 参数    | 描述                                                         |
    | ------- | ------------------------------------------------------------ |
    | method  | 表示 http 请求的方式，常用的有 OPTIONS、GET、HEAD 几种方式， 一般的健康检查可以使用 HEAD 方式进行，而不是采用 GET 方式，这是因为 HEAD 方式没有数据返回，仅检查 response 的 HEAD 是不是 200 状态。因此相对 GET 方式，HEAD 方式更快，更简单。 |
    | uri     | 表示要检测的 URL 地址，通过执行此 URL ，可以获取后端服务器的运行状态。在正常情况下将返回状态码 200，返回其他状态码均为异常状态。 |
    | version | 指定心跳检测时的 http 版本号。                               |

  - server：这个关键字用来定义多个后端真实服务器，不能用于 defaults 和frontend部分。使用格式为：server <name> <address>[:port] [param*] 其中，每个参数含义如下：

    - check：表示启用对此后端服务器执行健康状态检查。

    - inter：设置健康状态检查的时间间隔，单位为毫秒。

    - rise：设置从故障状态转换至正常状态需要成功检查的次数，例如。“rise 2”表示 2 次检查正确就认为此服务器可用。

    - fall：设置后端服务器从正常状态转换为不可用状态需要检查的次数，例如，“fall 3”表示 3 次检查失败就认为此服务器不可用。

    - cookie：为指定的后端服务器设定 cookie 值，此处指定的值将在请求入站时被检查，第一次为此值挑选的后端服务器将在后

      <table>
      <tr>
      	<td>name</td>
      	<td colspan="2">为后端真实服务器指定一个内部名称，随便定义一个即可。</td>
      </tr>
      <tr>
      	<td>address</td>
      	<td colspan="2">后端真实服务器的 IP 地址或主机名。</td>
      </tr>
      <tr>
      	<td>port</td>
        <td colspan="2">指定连接请求发往真实服务器时的目标端口。在未设定时，将使用客户端请求时的同一端口。</td>
      </tr>
        <td rowspan="8">[params*]</td>
        <td colspan="2">为后端服务器设定的一系列参数，可用参数较多，仅介绍常用的参数：</td>
      </tr>
      </tr>
        <td>check</td>
      	<td>表示启用对此后端服务器执行健康状态检查。</td>
      </tr>
      </tr>
        <td>inter</td>
      	<td>设置健康检查的时间间隔，单位为毫秒。</td>
      </tr>
      </tr>
        <td>rise</td>
      	<td>设置从故障状态转化为正常状态需要成功检查的次数，例如：“rise：2”表示两次检查正确就认为此服务可用。</td>
      </tr>
      </tr>
        <td>fall</td>
      	<td>设置后端服务器从正常状态转换为不可用状态需要检查的次数，例如：“fall：3”表示三次检查失败就认为此服务器不可用。</td>
      </tr>
      </tr>
        <td>cookie</td>
      	<td>为指定的后端服务器设置 cookie 值，此处指定的值将在请求入站时被检查，第一次为此值挑选的后端服务器将在后续的请求中一直被选中，其目的在于实现持久连接的功能。上面的“cookie server1” 表示 web1 的 serverid 为 server1。同理，“cookie server2” 表示 web2 的 serverid 为 server2。</td>
      </tr>
      </tr>
        <td>weight</td>
      	<td>设置后端真实服务器的权重，默认为 1，最大值为 256。设置为 0 表示不参与负载均衡。</td>
      </tr>
      </tr>
        <td>backup</td>
      	<td>设置后端真实服务器的备份服务器，仅仅在后端真实服务器均不可用时才会启用。</td>
      </tr>
      </table>

- **listen** 部分

  此部分是 frontend 部分和 backend 部分的结合体。在 HAProxy1.3 版本之前，HAProxy 的所有配置选项都在这个部分中设置。为了保持兼容性，HAProxy 新的版本仍然保留了 listen 组件的配置方式。目前在 HAProxy 中，两种配置方式任选其一即可。

  这个部分通过listen 关键字定义了一个名为“admin_stats”的实例，其实就是定义了一个 HAProxy 的监控页面，每个选项的含义如下：

  - stats refresh：设置 HAProxy 监控统计页面自动刷新的时间。
  - stats uri：设置 HAProxy 监控统计页面的URL 路径，可随意指定。例如、指定“stats uri /haproxy-status”，就可以过 http://IP:9188/haproxy-status  查看。
  - stats realm：设置登录 HAProxy 统计页面时密码框上的文本提示信息。
  - stats auth：设置登录 HAProxy 统计页面的用户名和密码。用户名和密码通过冒号分割。可为监控页面设置多个用户名和密码，每行一个。
  - stats hide-version：用来隐藏统计页面上 HAProxy 的版本信息。
  - stats admin if TRUE：通过设置此选项，可以在监控页面上手工启用或禁用后端真实服务器，仅在 haproxy1.4.9 以后版本有效。

**一份完整的配置**

```c
global
    log 127.0.0.1 local0 info 
    maxconn 4096
    user nobody 
    group nobody 
    daemon 
    nbproc 1
    pidfile /usr/local/haproxy/logs/haproxy.pid
defaults
    mode http 
    retries 3
    timeout connect 10s 
    timeout client 20s 
    timeout server 30s
    timeout check 5s
frontend www
    bind *:80 
    mode	http
    option	httplog 
    option	forwardfor
    option	httpclose 
    log	global
    #acl host_www	hdr_dom(host)	-i	www.zb.com
    #acl host_img	hdr_dom(host)	-i	img.zb.com
 
    #use_backend htmpool	if	host_www 
    #use_backend imgpool	if	host_img 
    default_backend	htmpool
backend htmpool
    mode http 
    option	redispatch
    option	abortonclose 
    balance  static-rr 
    cookie	SERVERID
    option	httpchk GET /index.jsp
    server	237server 192.168.81.237:8080 cookie server1 weight 6 check inter 2000 rise 2 fall 3
    server	iivey234 192.168.81.234:8080 cookie server2 weight 3 check inter 2000 rise 2 fall 3
backend imgpool
    mode		http 
    option	redispatch
    option	abortonclose
     balance  static-rr 
    cookie  SERVERID
    option	httpchk GET /index.jsp
    server	host236 192.168.81.236:8080 cookie server1 weight 6 check inter 2000 rise 2 fall 3
 
listen admin_stats
    bind 0.0.0.0:9188
    mode http
    log 127.0.0.1 local0 err 
    stats refresh 30s
    stats uri /haproxy-status
    stats realm welcome login\ Haproxy 
    stats auth admin:admin123
    stats hide-version 
    stats admin if TRUE
   
```

**安装HAProxy**

下载HAProxy相关版本，这里下载haproxy-1.8.12.tar.gz，之后准备安装。

安装之前查看内核版本，根据内核版本选择编译参数

```c
uname -r
```

解压HAProxy，并安装：

```c
tar xf haproxy-1.8.12.tar.gz
cd haproxy-1.7.5
make TARGET=linux2628 PREFIX=/usr/local/haproxy
make install PREFIX=/usr/local/haproxy
```

安装成功之后，查看版本

```c
/usr/local/haproxy/sbin/haproxy -v
```

**配置HAProxy**

配置启动文件，复制haproxy文件到/usr/sbin下 ，复制haproxy脚本，到/etc/init.d下

```c
cp /usr/local/haproxy/sbin/haproxy /usr/sbin/
cp ./examples/haproxy.init /etc/init.d/haproxy
chmod 755 /etc/init.d/haproxy
```

创建系统账号

```c
useradd -r haproxy
```

创建配置文件

```c
mkdir /etc/haproxy
vi /etc/haproxy/haproxy.cfg
```

更改配置文件

```c
#全局配置
global
    #设置日志
    log 127.0.0.1 local0 info
    #当前工作目录
    chroot /usr/local/haproxy
    #用户与用户组
    user haproxy
    group haproxy
    #运行进程ID
    uid 99
    gid 99
    #守护进程启动
    daemon
    #最大连接数
    maxconn 4096

#默认配置
defaults
    #应用全局的日志配置
    log global
    #默认的模式mode {tcp|http|health}
    #TCP是4层，HTTP是7层，health只返回OK
    mode tcp
    #日志类别tcplog
    option tcplog
    #不记录健康检查日志信息
    option dontlognull
    #3次失败则认为服务不可用
    retries 3
    #每个进程可用的最大连接数
    maxconn 2000
    #连接超时
    timeout connect 5s
    #客户端超时
    timeout client 120s
    #服务端超时
    timeout server 120s

#绑定配置
listen rabbitmq_cluster 
        bind 0.0.0.0:5671
        #配置TCP模式
        mode tcp
        #简单的轮询
        balance roundrobin
        #RabbitMQ集群节点配置
        server rmq_node1 10.110.8.34:5672 check inter 5000 rise 2 fall 3 weight 1
        server rmq_node2 10.110.8.38:5672 check inter 5000 rise 2 fall 3 weight 1

#haproxy监控页面地址
listen monitor 
        bind 0.0.0.0:8100
        mode http
        option httplog
        stats enable
        stats uri /stats
        stats refresh 5s
```

**启动haproxy**

```c
service haproxy start
```

Haproxy 解决集群 session 共享问题，二种方法保持客户端 session 一致。

- 用户 IP 识别

  Haproxy 将用户 IP 经过 hash 计算后 指定到固定的真实服务器上（类似于 nginx 的 IP hash 指令）。

  配置指令： balance source。

  ```c
  backend htmpool
          mode http
          option redispatch
          option abortonclose
          balance source
          cookie SERVERID
          option httpchk GET /index.jsp
          server 237server 192.168.81.237:8080 cookie server1 weight 6 check inter 2000 rise 2 fall 3
          server iivey234 192.168.81.234:8080 cookie server2 weight 3 check inter 2000 rise 2 fall 3
  ```

- cookie 识别

  haproxy 将WEB 服务端发送给客户端的 cookie 中插入(或添加加前缀)haproxy 定义的后端的服务器COOKIE ID。

  配置指令例举 cookie SESSION_COOKIE insert indirect nocache。

  ```c
  backend htmpool
          mode http
          option    redispatch
          option    abortonclose
          balance  static-rr
          cookie    SERVERID   #cookie参数
          option    httpchk GET /index.jsp
          server    237server 192.168.81.237:8080 cookie server1 weight 6 check inter 2000 rise 2 fall 3   #server里面的cookie参数
          server    iivey234 192.168.81.234:8080 cookie server2 weight 3 check inter 2000 rise 2 fall 3   #server里面的cookie参数
  ```

  

#### ActiveMQ

ActiveMQ 介于 ZeroMQ 和 RabbitMQ 之间。类似于 ZeroMQ，它可以部署于代理模式和 P2P（点对点）模式。类似于 RabbitMQ，它易于实现高级场景，而且只需付出低消耗，被誉为消息中间件的“瑞士军刀”。

支持 OpenWire、Stomp、AMQP v1.0、MQTT v3.1、REST、Ajax、Webservice 等多种协议，完全支持 JMS1.1 和 J2EE 1.4规范（事务、持久化、XA消息），支持持久化到数据库。但是 ActiveMQ 不够轻巧，而且对于队列较多的情况支持不好，据说还有丢消息的情况。

#### Apollo

Apache 称 Apollo 为最快、最强健的 STOMP（简单“流”文本定向消息协议，它提供了一个可互操作的连接模式，允许 STOMP 客户端与任意 STOMP 消息代理（Broker）进行交互。STOMP 协议由于设计简单，易于开发客户端，因此在多种语言和多种平台上得到广泛地应用）服务器。支持 STOMP、AMQP、MQTT、OpenWire 协议，支持 Topic、Queue、持久订阅等消费形式，支持对消息的多种处理，支持安全性处理，支持 REST 管理 API。

#### Kafka

##### 简介

###### Kafka是什么

Kafka是一种高吞吐量的分布式发布订阅消息系统（消息引擎系统），它可以处理消费者在网站中的所有动作流数据。 这种动作（网页浏览，搜索和其他用户的行动）是在现代网络上的许多社会功能的一个关键因素。 这些数据通常是由于吞吐量的要求而通过处理日志和日志聚合来解决。 对于像Hadoop一样的日志数据和离线分析系统，但又要求实时处理的限制，这是一个可行的解决方案。Kafka的目的是通过Hadoop的并行加载机制来统一线上和离线的消息处理，也是为了通过集群来提供实时的消息。

其实我们简单点理解就是系统A发送消息给kafka（消息引擎系统），系统B从kafka中读取A发送的消息。而kafka就是个中间商。

###### 消息系统简介

一个消息系统负责将数据从一个应用传递到另外一个应用，应用只需关注于数据，无需关注数据在两个或多个应用间是如何传递的。分布式消息传递基于可靠的消息队列，在客户端应用和消息系统之间异步传递消息。有两种主要的消息传递模式：**点对点传递模式、发布-订阅模式。大部分的消息系统选用发布-订阅模式**。**Kafka就是一种发布-订阅模式**。

**点对点消息传递模式**

在点对点消息系统中，消息持久化到一个队列中。此时，将有一个或多个消费者消费队列中的数据。但是一条消息只能被消费一次。当一个消费者消费了队列中的某条数据之后，该条数据则从消息队列中删除。该模式即使有多个消费者同时消费数据，也能保证数据处理的顺序。这种架构描述示意图如下：

![img](https://github.com/chou401/pic-md/raw/master/27da57a28c192b5f1b8eed58fd8765af.png)

**生产者发送一条消息到queue，只有一个消费者能收到**。

**发布-订阅消息传递模式**

在发布-订阅消息系统中，消息被持久化到一个topic中。与点对点消息系统不同的是，消费者可以订阅一个或多个topic，消费者可以消费该topic中所有的数据，同一条数据可以被多个消费者消费，数据被消费后不会立马删除。在发布-订阅消息系统中，消息的生产者称为发布者，消费者称为订阅者。该模式的示例图如下：

![img](https://github.com/chou401/pic-md/raw/master/7de02b63e52a56b2e6b5288f5b249fca.png)

**发布者发送到topic的消息，只有订阅了topic的订阅者才会收到消息**。

###### Kafka 简单理解

上面我们提到kafka是个中间商，我们为什么不能去掉这个中间商呢，凭着我们的想象也会觉得去掉这些消息引擎系统会更好吧，那我们来谈谈消息引擎系统存在的意义：

原因就是“**削峰填谷**”。这四个字简直比消息引擎本身还要有名气。

所谓的“削峰填谷”就是指缓冲上下游瞬时突发流量，使其更平滑。特别是对于那种发送能力很强的上游系统，如果没有消息引擎的保护，“脆弱”的下游系统可能会直接被压垮导致全链路服务“雪崩”。但是，一旦有了消息引擎，它能够有效地对抗上游的流量冲击，真正做到将上游的“峰”填满到“谷”中，避免了流量的震荡。消息引擎系统的另一大好处在于发送方和接收方的松耦合，这也在一定程度上简化了应用的开发，减少了系统间不必要的交互。

我们想象一下在双11期间我们购物的情景来形象的理解一下削峰填谷，感受一下Kafka在这中间是怎么去“抗”峰值流量的吧：

当我们点击某个商品以后进入付费页面，可是这个简单的流程中就可能包含多个子服务，比如点击购买按钮会调用订单系统生成对应的订单，而处理该订单会依次调用下游的多个子系统服务 ，比如调用支付宝和微信支付的接口、查询你的登录信息、验证商品信息等。显然上游的订单操作比较简单，所以它的 TPS（每秒事务处理量） 要远高于处理订单的下游服务，因此如果上下游系统直接对接，势必会出现下游服务无法及时处理上游订单从而造成订单堆积的情形。特别是当出现类似于秒杀这样的业务时，上游订单流量会瞬时增加，可能出现的结果就是直接压跨下游子系统服务。

解决此问题的一个常见做法是我们对上游系统进行限速，但这种做法对上游系统而言显然是不合理的，毕竟问题并不出现在它那里。所以更常见的办法是引入像 Kafka 这样的消息引擎系统来对抗这种上下游系统 TPS 的错配以及瞬时峰值流量。

还是这个例子，当引入了 Kafka 之后。上游订单服务不再直接与下游子服务进行交互。当新订单生成后它仅仅是向 Kafka Broker 发送一条订单消息即可。类似地，下游的各个子服务订阅 Kafka 中的对应主题，并实时从该主题的各自分区（Partition）中获取到订单消息进行处理，从而实现了上游订单服务与下游订单处理服务的解耦。这样当出现秒杀业务时，Kafka 能够将瞬时增加的订单流量全部以消息形式保存在对应的主题中，既不影响上游服务的 TPS，同时也给下游子服务留出了充足的时间去消费它们。这就是 Kafka 这类消息引擎系统的最大意义所在。

###### Kafka 的优点特点

- 解耦

  在项目启动之初来预测将来项目会碰到什么需求，是极其困难的。消息系统在处理过程中间插入了一个隐含的、基于数据的接口层，两边的处理过程都要实现这一接口。这允许你独立的扩展或修改两边的处理过程，只要确保它们遵守同样的接口约束。

- 冗余（副本）

  有些情况下，处理数据的过程会失败。除非数据被持久化，否则将造成丢失。消息队列把数据进行持久化直到它们已经被完全处理，通过这一方式规避了数据丢失风险。许多消息队列所采用的"插入-获取-删除"范式中，在把一个消息从队列中删除之前，需要你的处理系统明确的指出该消息已经被处理完毕，从而确保你的数据被安全的保存直到你使用完毕。

- 扩展性

  因为消息队列解耦了你的处理过程，所以增大消息入队和处理的频率是很容易的，只要另外增加处理过程即可。不需要改变代码、不需要调节参数。扩展就像调大电力按钮一样简单。

- 灵活性&峰值处理能力

  在访问量剧增的情况下，应用仍然需要继续发挥作用，但是这样的突发流量并不常见；如果为以能处理这类峰值访问为标准来投入资源随时待命无疑是巨大的浪费。使用消息队列能够使关键组件顶住突发的访问压力，而不会因为突发的超负荷的请求而完全崩溃。

- 可恢复性

  系统的一部分组件失效时，不会影响到整个系统。消息队列降低了进程间的耦合度，所以即使一个处理消息的进程挂掉，加入队列中的消息仍然可以在系统恢复后被处理。

- 顺序保证

  在大多使用场景下，数据处理的顺序都很重要。大部分消息队列本来就是排序的，并且能保证数据会按照特定的顺序来处理。Kafka保证一个Partition内的消息的有序性。

- 缓冲

  在任何重要的系统中，都会有需要不同的处理时间的元素。例如，加载一张图片比应用过滤器花费更少的时间。消息队列通过一个缓冲层来帮助任务最高效率的执行———写入队列的处理会尽可能的快速。该缓冲有助于控制和优化数据流经过系统的速度。

- 异步通信

  很多时候，用户不想也不需要立即处理消息。消息队列提供了异步处理机制，允许用户把一个消息放入队列，但并不立即处理它。想向队列中放入多少消息就放多少，然后在需要的时候再去处理它们。

##### Kafka中的术语解释概述

下图展示了Kafka的相关术语以及之间的关系：

![dfef98a36bb160ddd779634de5e6c4b4](https://github.com/chou401/pic-md/raw/master/dfef98a36bb160ddd779634de5e6c4b4.png)

- 上图中一个topic配置了3个partition。Partition1有两个offset：0和1。Partition2有4个offset。Partition3有1个offset。副本的id和副本所在的机器的id恰好相同。
- 如果一个topic的副本数为3，那么Kafka将在集群中为每个partition创建3个相同的副本。集群中的每个broker存储一个或多个partition。多个producer和consumer可同时生产和消费数据。

###### broker

- Kafka 集群包含一个或多个服务器，服务器节点称为broker。
- broker存储topic的数据。如果某topic有N个partition，集群有N个broker，那么每个broker存储该topic的一个partition。
- 如果某topic有N个partition，集群有(N+M)个broker，那么其中有N个broker存储该topic的一个partition，剩下的M个broker不存储该topic的partition数据。
- 如果某topic有N个partition，集群中broker数目少于N个，那么一个broker存储该topic的一个或多个partition。在实际生产环境中，尽量避免这种情况的发生，这种情况容易导致Kafka集群数据不均衡。

###### Topic

- 每条发布到Kafka集群的消息都有一个类别，这个类别被称为Topic。（物理上不同Topic的消息分开存储，逻辑上一个Topic的消息虽然保存于一个或多个broker上但用户只需指定消息的Topic即可生产或消费数据而不必关心数据存于何处）。
- 类似于数据库的表名。

###### Partition

- topic中的数据分割为一个或多个partition。每个topic至少有一个partition。每个partition中的数据使用多个segment文件存储。partition中的数据是有序的，不同partition间的数据丢失了数据的顺序。如果topic有多个partition，消费数据时就不能保证数据的顺序。在需要严格保证消息的消费顺序的场景下，需要将partition数目设为1。

###### Producer

- 生产者即数据的发布者，该角色将消息发布到Kafka的topic中。broker接收到生产者发送的消息后，broker将该消息**追加**到当前用于追加数据的segment文件中。生产者发送的消息，存储到一个partition中，生产者也可以指定数据存储的partition。

###### Consumer

- 消费者可以从broker中读取数据。消费者可以消费多个topic中的数据。

###### Consumer Group

- 每个Consumer属于一个特定的Consumer Group（可为每个Consumer指定group name，若不指定group name则属于默认的group）。

###### Leader 

- 每个partition有多个副本，其中有且仅有一个作为Leader，Leader是当前负责数据的读写的partition。

###### Follower

- Follower跟随Leader，所有写请求都通过Leader路由，数据变更会广播给所有Follower，Follower与Leader保持数据同步。如果Leader失效，则从Follower中选举出一个新的Leader。当Follower与Leader挂掉、卡住或者同步太慢，leader会把这个follower从“in sync replicas”（ISR）列表中删除，重新创建一个Follower。


##### Kafka架构

![img](https://github.com/chou401/pic-md/raw/master/cb62ef93fb5ef2f406a1ed0fe3a079a8.png)

如上图所示，一个典型的Kafka集群中包含若干Producer（可以是web前端产生的Page View，或者是服务器日志，系统CPU、Memory等），若干broker（Kafka支持水平扩展，一般broker数量越多，集群吞吐率越高），若干Consumer Group，以及一个Zookeeper集群。Kafka通过Zookeeper管理集群配置，选举leader，以及在Consumer Group发生变化时进行rebalance。Producer使用push模式将消息发布到broker，Consumer使用pull模式从broker订阅并消费消息。

###### Topics和Partition

Topic在逻辑上可以被认为是一个queue，每条消费都必须指定它的Topic，可以简单理解为必须指明把这条消息放进哪个queue里。为了使得Kafka的吞吐率可以线性提高，物理上把Topic分成一个或多个Partition，每个Partition在物理上对应一个文件夹，该文件夹下存储这个Partition的所有消息和索引文件。创建一个topic时，同时可以指定分区数目，分区数越多，其吞吐量也越大，但是需要的资源也越多，同时也会导致更高的不可用性，kafka在接收到生产者发送的消息之后，会根据均衡策略将消息存储到不同的分区中。**因为每条消息都被append到该Partition中，属于顺序写磁盘**，因此效率非常高（经验证，顺序写磁盘效率比随机写内存还要高，这是Kafka高吞吐率的一个很重要的保证）。

![img](https://github.com/chou401/pic-md/raw/master/ce47715713754188bd90492eaec2b67c.png)

对于传统的message queue而言，一般会删除已经被消费的消息，而Kafka集群会保留所有的消息，无论其被消费与否。当然，因为磁盘限制，不可能永久保留所有数据（实际上也没必要），因此Kafka提供两种策略删除旧数据。一是基于时间，二是基于Partition文件大小。例如可以通过配置$KAFKA_HOME/config/server.properties，让Kafka删除一周前的数据，也可在Partition文件超过1GB时删除旧数据，配置如下所示：

```properties
# 符合删除条件的日志文件的最小时间
log.retention.hours=168
# 日志段文件的最大大小。当达到这个大小时，将创建一个新的日志段。
log.segment.bytes=1073741824
# 检查日志段的时间间隔，以确定它们是否可以根据保留策略被删除
log.retention.check.interval.ms=300000
# 如果设置了log.cleaner.enable =true，则清理器将被启用，然后可以为日志压缩标记单个日志。
log.cleaner.enable=f
```

因为Kafka读取特定消息的**时间复杂度为O(1)**，即与文件大小无关，所以这里删除过期文件与提高Kafka性能无关。选择怎样的删除策略只与磁盘以及具体的需求有关。另外，Kafka会为每一个Consumer Group保留一些metadata信息——当前消费的消息的position，也即offset。这个offset由Consumer控制。正常情况下Consumer会在消费完一条消息后递增该offset。当然，Consumer也可将offset设成一个较小的值，重新消费一些消息。因为offset由Consumer控制，所以Kafka broker是无状态的，它不需要标记哪些消息被哪些消费过，也不需要通过broker去保证同一个Consumer Group只有一个Consumer能消费某一条消息，因此也就不需要锁机制，这也为Kafka的高吞吐率提供了有力保障。

###### Producer消息路由

Producer发送消息到broker时，会根据Paritition机制选择将其存储到哪一个Partition。如果Partition机制设置合理，所有消息可以均匀分布到不同的Partition里，这样就实现了负载均衡。如果一个Topic对应一个文件，那这个文件所在的机器I/O将会成为这个Topic的性能瓶颈，而有了Partition后，不同的消息可以并行写入不同broker的不同Partition里，极大的提高了吞吐率。可以在$KAFKA_HOME/config/server.properties中通过配置项num.partitions来指定新建Topic的默认Partition数量，也可在创建Topic时通过参数指定，同时也可以在Topic创建之后通过Kafka提供的工具修改。

在发送一条消息时，可以指定这条消息的key，Producer根据这个key和Partition机制来判断应该将这条消息发送到哪个Partition。Paritition机制可以通过指定Producer的paritition.class这一参数来指定，该class必须实现kafka.producer.Partitioner接口。

###### Consumer Group

使用Consumer high level API时，同一Topic的一条消息只能被同一个Consumer Group内的一个Consumer消费，但多个Consumer Group可同时消费这一消息。

![img](https://github.com/chou401/pic-md/raw/master/edd5bf39ad20cf985f45988c75446af7.png)

这是**Kafka用来实现一个Topic消息的广播（发给所有的Consumer）和单播（发给某一个Consumer）的手段**。一个Topic可以对应多个Consumer Group。如果需要实现广播，只要每个Consumer有一个独立的Group就可以了。要实现单播只要所有的Consumer在同一个Group里。用Consumer Group还可以将Consumer进行自由的分组而不需要多次发送消息到不同的Topic。

实际上，**Kafka的设计理念之一就是同时提供离线处理和实时处理**。根据这一特性，可以使用Storm这种实时流处理系统对消息进行实时在线处理，同时使用Hadoop这种批处理系统进行离线处理，还可以同时将数据实时备份到另一个数据中心，只需要保证这三个操作所使用的Consumer属于不同的Consumer Group即可。

###### Push与Pull

作为一个消息系统，Kafka遵循了传统的方式，选择由**Producer向broker push消息并由Consumer从broker pull消息**。一些logging-centric system，比如Facebook的Scribe和Cloudera的Flume，采用push模式。事实上，push模式和pull模式各有优劣。

push模式很难适应消费速率不同的消费者，因为消息发送速率是由broker决定的。push模式的目标是尽可能以最快速度传递消息，但是这样很容易造成Consumer来不及处理消息，典型的表现就是拒绝服务以及网络拥塞。而pull模式则可以根据Consumer的消费能力以适当的速率消费消息。

**对于Kafka而言，pull模式更合适**。pull模式可简化broker的设计，Consumer可自主控制消费消息的速率，同时Consumer可以自己控制消费方式——**即可批量消费也可逐条消费**，同时还能选择不同的提交方式从而实现不同的传输语义。

###### Kafka delivery guarantee

有这么几种可能的delivery guarantee：

> At most once 　　消息可能会丢，但绝不会重复传输
>
> At least one 　　  消息绝不会丢，但可能会重复传输
>
> Exactly once 　　 每条消息肯定会被传输一次且仅传输一次，很多时候这是用户所想要的。

当Producer向broker发送消息时，一旦这条消息被commit，因为replication的存在，它就不会丢。但是如果Producer发送数据给broker后，遇到网络问题而造成通信中断，那Producer就无法判断该条消息是否已经commit。虽然Kafka无法确定网络故障期间发生了什么，但是Producer可以生成一种类似于主键的东西，发生故障时幂等性的重试多次，这样就做到了Exactly once。

接下来讨论的是消息从broker到Consumer的delivery guarantee语义。（仅针对Kafka consumer high level API）。Consumer在从broker读取消息后，可以选择commit，该操作会在Zookeeper中保存该Consumer在该Partition中读取的消息的offset。该Consumer下一次再读该Partition时会从下一条开始读取。如未commit，下一次读取的开始位置会跟上一次commit之后的开始位置相同。当然可以将Consumer设置为autocommit，即Consumer一旦读到数据立即自动commit。如果只讨论这一读取消息的过程，那Kafka是确保了Exactly once。但实际使用中应用程序并非在Consumer读取完数据就结束了，而是要进行进一步处理，而数据处理与commit的顺序在很大程度上决定了消息从broker和consumer的delivery guarantee semantic。

**Kafka默认保证At least once**，并且允许通过设置Producer异步提交来实现At most once。而Exactly once要求与外部存储系统协作，幸运的是Kafka提供的offset可以非常直接非常容易的使用这种方式。

###### ack 机制

producer端设置`request.required.acks`。

- **request.required.acks = 0**：只要请求已发送出去，就算是发送完了，不关心有没有写成功。性能很好，如果是对一些日志进行分析，可以承受丢数据的情况，用这个参数，性能会很好。吞吐量高。
- **request.required.acks = 1（默认）**：发送一条消息，当leader partition写入成功以后，才算写入成功。不过这种方式也有丢数据的可能。
- **request.required.acks = -1/all**：需要ISR列表里面，所有 replica 都写完以后，这条消息才算写入成功。这才是 ISR 的正确应用场景，可靠性最高。

**ISR 的最坏情况**

排除所有 replica 全部故障，ISR 的最坏情况就是 ISR 中只剩 leader 自己一个了。退化成 ack=1 的情况了，甚至还不如 ack=1。ack=1，说的是 producer 不等服务器完全同步完 ISR，只要 leader 写入成功就行了，但是可没说不进行同步了。该有的同步过程还是会进行的，但凡能同步，kafka 肯定会同步的，而 ack=1 的最坏情况，也是 ISR 只剩下 leader 了。坏疽话说，producer 为了提高吞吐量，没等 ISR 全部同步，但是心里还是希望接口同步完成的。而这种 leader 孤家寡人的最坏情况，书上说“退化成 ack=1”，不足以说明问题的严重性。

ISR 的最坏情况，会使 ack=-1 退化成 ack=1 的最坏情况，完全背离我们设置-1 的初衷（因为特定是同步不了了）。

数据不丢失的方案：

1. 分区副本 >= 2
2. acks = -1
3. min.insync.replicas >= 2

下面给出此时leader出现故障的情况，可以看出，此时数据可能重复。

![img](https://github.com/chou401/pic-md/raw/master/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5ZSP5ZmX,size_20,color_FFFFFF,t_70,g_se,x_16.jpeg)

Leader维护了⼀个动态的 in-sync replica set（ISR）：和 Leader 保持同步的 Follower 集合。当 ISR 集合中的 Follower 完成数据的同步之后，Leader 就会给 Follower 发送 ACK。如果 Follower ⻓时间未向 Leader 同步数据，则该 Follower 将被踢出 ISR 集合，该时间阈值由replica.lag.time.max.ms 参数设定。Leader 发⽣故障后，就会从 ISR 中选举出新的 Leader。
kafka服务端中min.insync.replicas。 如果我们不设置的话，默认这个值是1。一个leader partition会维护一个ISR列表，这个值就是限制ISR列表里面 至少得有几个副本，比如这个值是2，那么当ISR列表里面只有一个副本的时候，往这个分区插入数据的时候会报错。

##### Kafka高可用

###### 高可用的由来

**为何需要Replication**

- Kafka在0.8以前的版本中，是没有Replication的，一旦某一个Broker宕机，则其上所有的Partition数据都不可被消费，这与Kafka数据持久性及Delivery Guarantee的设计目标相悖。同时Producer都不能再将数据存于这些Partition中。
- 如果Producer使用同步模式则Producer会在尝试重新发送message.send.max.retries（默认值为3）次后抛出Exception，用户可以选择停止发送后续数据也可选择继续选择发送。而前者会造成数据的阻塞，后者会造成本应发往该Broker的数据的丢失。
- 如果Producer使用异步模式，则Producer会尝试重新发送message.send.max.retries（默认值为3）次后记录该异常并继续发送后续数据，这会造成数据丢失并且用户只能通过日志发现该问题。同时，Kafka的Producer并未对异步模式提供callback接口。
- 由此可见，在没有Replication的情况下，一旦某机器宕机或者某个Broker停止工作则会造成整个系统的可用性降低。随着集群规模的增加，整个集群中出现该类异常的几率大大增加，因此对于生产系统而言Replication机制的引入非常重要。

**Leader Election（选举机制）**

- 引入Replication之后，同一个Partition可能会有多个Replica，而这时需要在这些Replication之间选出一个Leader，Producer和Consumer只与这个Leader交互，其它Replica作为Follower从Leader中复制数据。
- 因为需要保证同一个Partition的多个Replica之间的数据一致性（其中一个宕机后其它Replica必须要能继续服务并且即不能造成数据重复也不能造成数据丢失）。如果没有一个Leader，所有Replica都可同时读/写数据，那就需要保证多个Replica之间互相（N×N条通路）同步数据，数据的一致性和有序性非常难保证，大大增加了Replication实现的复杂性，同时也增加了出现异常的几率。而引入Leader后，只有Leader负责数据读写，Follower只向Leader顺序Fetch数据（N条通路），系统更加简单且高效。


######  Kafka HA设计解析

**如何将所有Replica均匀分布到整个集群**

为了更好的做负载均衡，Kafka尽量将所有的Partition均匀分配到整个集群上。一个典型的部署方式是一个Topic的Partition数量大于Broker的数量。同时为了提高Kafka的容错能力，也需要将同一个Partition的Replica尽量分散到不同的机器。实际上，如果所有的Replica都在同一个Broker上，那一旦该Broker宕机，该Partition的所有Replica都无法工作，也就达不到HA的效果。同时，如果某个Broker宕机了，需要保证它上面的负载可以被均匀的分配到其它幸存的所有Broker上。

Kafka分配Replica的算法如下：

1. 将所有Broker（假设共n个Broker）和待分配的Partition排序。
2. 将第i个Partition分配到第（i mod n）个Broker上。
3. 将第i个Partition的第j个Replica分配到第（(i + j) mode n）个Broker上。

**Data Replication（副本策略）**

Kafka的高可靠性的保障来源于其健壮的副本（replication）策略。 

**1.消息传递同步策略**

Producer在发布消息到某个Partition时，先通过ZooKeeper找到该Partition的Leader，然后无论该Topic的Replication Factor为多少，Producer只将该消息发送到该Partition的Leader。Leader会将该消息写入其本地Log。每个Follower都从Leader pull数据。这种方式上，Follower存储的数据顺序与Leader保持一致。Follower在收到该消息并写入其Log后，向Leader发送ACK。一旦Leader收到了ISR中的所有Replica的ACK，该消息就被认为已经commit了，Leader将增加HW并且向Producer发送ACK。

为了提高性能，**每个Follower在接收到数据后就立马向Leader发送ACK，而非等到数据写入Log中**。因此，对于已经commit的消息，Kafka只能保证它被存于多个Replica的内存中，而不能保证它们被持久化到磁盘中，也就不能完全保证异常发生后该条消息一定能被Consumer消费。

Consumer读消息也是从Leader读取，**只有被commit过的消息才会暴露给Consumer**。

Kafka Replication的数据流如下图所示：

![image-20230703112926417](https://github.com/chou401/pic-md/raw/master/image-20230703112926417.png)

**2.ACK前需要保证有多少个备份**

对于Kafka而言，定义一个Broker是否“活着”包含两个条件：

- 一是它必须维护与ZooKeeper的session（这个通过ZooKeeper的Heartbeat机制来实现）。
- 二是Follower必须能够及时将Leader的消息复制过来，不能“落后太多”。

Leader会跟踪与其保持同步的Replica列表，该列表称为ISR（即in-sync Replica）。如果一个Follower宕机，或者落后太多，Leader将把它从ISR中移除。这里所描述的“落后太多”指Follower复制的消息落后于Leader后的条数超过预定值（该值可在\$KAFKA_HOME/config/server.properties中通过replica.lag.max.messages配置，其默认值是4000）或者Follower超过一定时间（该值可在\$KAFKA_HOME/config/server.properties中通过replica.lag.time.max.ms来配置，其默认值是10000）未向Leader发送fetch请求。

Kafka的复制机制既不是完全的同步复制，也不是单纯的异步复制。事实上，完全同步复制要求所有能工作的Follower都复制完，这条消息才会被认为commit，这种复制方式极大的影响了吞吐率（高吞吐率是Kafka非常重要的一个特性）。而异步复制方式下，Follower异步的从Leader复制数据，数据只要被Leader写入log就被认为已经commit，这种情况下如果Follower都复制完都落后于Leader，而如果Leader突然宕机，则会丢失数据。而Kafka的这种使用ISR的方式则很好的均衡了确保数据不丢失以及吞吐率。Follower可以批量的从Leader复制数据，这样极大的提高复制性能（批量写磁盘），极大减少了Follower与Leader的差距。

需要说明的是，Kafka只解决fail/recover，不处理“Byzantine”（“拜占庭”）问题。一条消息只有被ISR里的所有Follower都从Leader复制过去才会被认为已提交。这样就避免了部分数据被写进了Leader，还没来得及被任何Follower复制就宕机了，而造成数据丢失（Consumer无法消费这些数据）。而对于Producer而言，它可以选择是否等待消息commit，这可以通过request.required.acks来设置。这种机制确保了只要ISR有一个或以上的Follower，一条被commit的消息就不会丢失。

**3.Leader Election算法**

Leader选举本质上是一个分布式锁，有两种方式实现基于ZooKeeper的分布式锁：

- 节点名称唯一性：多个客户端创建一个节点，只有成功创建节点的客户端才能获得锁。
- 临时顺序节点：所有客户端在某个目录下创建自己的临时顺序节点，只有序号最小的才获得锁。

一种非常常用的选举leader的方式是“Majority Vote”（“少数服从多数”），但Kafka并未采用这种方式。这种模式下，如果我们有2f+1个Replica（包含Leader和Follower），那在commit之前必须保证有f+1个Replica复制完消息，为了保证正确选出新的Leader，fail的Replica不能超过f个。因为在剩下的任意f+1个Replica里，至少有一个Replica包含有最新的所有消息。这种方式有个很大的优势，系统的latency只取决于最快的几个Broker，而非最慢那个。Majority Vote也有一些劣势，为了保证Leader Election的正常进行，它所能容忍的fail的follower个数比较少。如果要容忍1个follower挂掉，必须要有3个以上的Replica，如果要容忍2个Follower挂掉，必须要有5个以上的Replica。也就是说，在生产环境下为了保证较高的容错程度，必须要有大量的Replica，而大量的Replica又会在大数据量下导致性能的急剧下降。这就是这种算法更多用在ZooKeeper这种共享集群配置的系统中而很少在需要存储大量数据的系统中使用的原因。例如HDFS的HA Feature是基于majority-vote-based journal，但是它的数据存储并没有使用这种方式。

Kafka在ZooKeeper中动态维护了一个ISR（in-sync replicas），这个ISR里的所有Replica都跟上了leader，只有ISR里的成员才有被选为Leader的可能。在这种模式下，对于f+1个Replica，一个Partition能在保证不丢失已经commit的消息的前提下容忍f个Replica的失败。在大多数使用场景中，这种模式是非常有利的。事实上，为了容忍f个Replica的失败，Majority Vote和ISR在commit前需要等待的Replica数量是一样的，但是ISR需要的总的Replica的个数几乎是Majority Vote的一半。

虽然Majority Vote与ISR相比有不需等待最慢的Broker这一优势，但是Kafka作者认为Kafka可以通过Producer选择是否被commit阻塞来改善这一问题，并且节省下来的Replica和磁盘使得ISR模式仍然值得。

**4.如何处理所有Replica都不工作**

在ISR中至少有一个follower时，Kafka可以确保已经commit的数据不丢失，但如果某个Partition的所有Replica都宕机了，就无法保证数据不丢失了。这种情况下有两种可行的方案：

1. 等待ISR中的任一个Replica“活”过来，并且选它作为Leader
2. 选择第一个“活”过来的Replica（不一定是ISR中的）作为Leader

这就需要在可用性和一致性当中作出一个简单的折衷。如果一定要等待ISR中的Replica“活”过来，那不可用的时间就可能会相对较长。而且如果ISR中的所有Replica都无法“活”过来了，或者数据都丢失了，这个Partition将永远不可用。选择第一个“活”过来的Replica作为Leader，而这个Replica不是ISR中的Replica，那即使它并不保证已经包含了所有已commit的消息，它也会成为Leader而作为consumer的数据源（前文有说明，所有读写都由Leader完成）。Kafka0.8.*使用了第二种方式。根据Kafka的文档，在以后的版本中，Kafka支持用户通过配置选择这两种方式中的一种，从而根据不同的使用场景选择高可用性还是强一致性。

**5.选举Leader**

最简单最直观的方案是，所有Follower都在ZooKeeper上设置一个Watch，一旦Leader宕机，其对应的ephemeral znode会自动删除，此时所有Follower都尝试创建该节点，而创建成功者（ZooKeeper保证只有一个能创建成功）即是新的Leader，其它Replica即为Follower。

但是该方法会有3个问题：

- **split-brain**： 这是由ZooKeeper的特性引起的，虽然ZooKeeper能保证所有Watch按顺序触发，但并不能保证同一时刻所有Replica“看”到的状态是一样的，这就可能造成不同Replica的响应不一致
- **herd effect**： 如果宕机的那个Broker上的Partition比较多，会造成多个Watch被触发，造成集群内大量的调整
- **ZooKeeper负载过重**： 每个Replica都要为此在ZooKeeper上注册一个Watch，当集群规模增加到几千个Partition时ZooKeeper负载会过重。

Kafka 0.8.*的**Leader Election**方案解决了上述问题，它在所有broker中选出一个controller，所有Partition的Leader选举都由controller决定。controller会将Leader的改变直接通过RPC的方式（比ZooKeeper Queue的方式更高效）通知需为此作为响应的Broker。同时controller也负责增删Topic以及Replica的重新分配。

###### AR、ISR、LEO、HW

- **AR**：  Assigned Replicas的缩写，是每个partition下所有副本（replicas）的统称；
- **ISR**： 副本同步队列（In-Sync Replicas）的缩写，是指副本同步队列，ISR是AR中的一个子集；
- **LEO**：日志末端位移（Log End Offset）的缩写，表示每个partition的log最后一条Message的位置。新消息写入时将分配的偏移量（Offset）值，从0开始，随着消息不断写入递增。
- **HW**： 高水位（High Watermark）的缩写，是指consumer能够看到的此partition的位置。 取一个partition对应的ISR中最小的LEO作为HW，consumer最多只能消费到HW所在的位置。

kafka 中为了防止 log 文件过大导致数据定位效率低下而采取了分片和索引机制，将每个物理上的 partition 分为多个 segment。每个 segment 对应两个文件--“.index"文件和”.log“文件。”.index“ 文件存储大量的索引信息，”.log" 文件存储大量的数据，索引文件中的元数据指向对应数据文件中 message 的物理偏移地址。

但是对于上层应用来说，可以将partition看成最小的存储单元（一个由多个segment文件拼接而成的“巨型”文件），每个partition都由一些列有序的、不可变的消息组成，这些消息被连续的追加到partition中。

![image-20230703112608060](https://github.com/chou401/pic-md/raw/master/image-20230703112608060.png)

**ISR和AR**

ISR (In-Sync Replicas)，这个是指副本同步队列。副本数对Kafka的吞吐率是有一定的影响，但极大的增强了可用性。默认情况下Kafka的replica数量为1，即每个partition都有一个唯一的leader，为了确保消息的可靠性，通常应用中将其值(由broker的参数offsets.topic.replication.factor指定)大小设置为大于1，比如3。 所有的副本（replicas）统称为Assigned Replicas，即AR。ISR是AR中的一个子集，由leader维护ISR列表，follower从leader同步数据有一些延迟（包括延迟时间replica.lag.time.max.ms和延迟条数replica.lag.max.messages两个维度, 从 0.9.0.0 版本后中只支持replica.lag.time.max.ms这个维度），任意一个超过阈值都会把follower剔除出ISR, 存入OSR（Outof-Sync Replicas）列表，新加入的follower也会先存放在OSR中。**AR=ISR+OSR**。

![image-20210918150431039](https://github.com/chou401/pic-md/raw/master/image202304051925825.png)

**为什么在Kafka 0.9.0.0版本后移除了replica.lag.max.messages参数而只保留了replica.lag.time.max.ms作为ISR中副本管理的参数呢？**

replica.lag.max.messages表示当前某个副本落后leader的消息数量超过了这个参数的值，那么leader就会把follower从ISR中删除。假设设置replica.lag.max.messages=4，那么如果producer一次传送至broker的消息数量都小于4条时，因为在leader接受到producer发送的消息之后而follower副本开始拉取这些消息之前，follower落后leader的消息数不会超过4条消息，故此没有follower移出ISR，所以这时候replica.lag.max.message的设置似乎是合理的。但是producer发起瞬时高峰流量，producer一次发送的消息超过4条时，也就是超过replica.lag.max.messages，此时follower都会被认为是与leader副本不同步了，从而被踢出了ISR。但实际上这些follower都是存活状态的且没有性能问题。那么在之后追上leader,并被重新加入了ISR。于是就会出现它们不断地剔出ISR然后重新回归ISR，这无疑增加了无谓的性能损耗。而且这个参数是broker全局的。设置太大了，影响真正“落后”follower的移除；设置的太小了，导致follower的频繁进出。无法给定一个合适的replica.lag.max.messages的值，故此，新版本的Kafka移除了这个参数。

**HW和LEO**

上面有简单说到HW是HighWatermark的缩写，是指consumer能够看到的此partition的位置；而LEO是LogEndOffset的缩写，表示每个partition的log最后一条Message的位置。也就是，我们取一个partition对应的ISR中最小的LEO作为HW，consumer最多只能消费到HW所在的位置。**消费者能消费的数据 = [LW，HW）**。

每个replica都有自己的HW，leader和follower各自负责更新自己的HW的状态。对于leader新写入的消息，consumer不能立刻消费，leader会等待该消息被所有ISR中的replicas同步后更新HW，此时消息才能被consumer消费。这样就保证了如果leader所在的broker失效，该消息仍然可以从新选举的leader中获取。对于来自内部broker的读取请求，没有HW的限制。

![img](https://github.com/chou401/pic-md/raw/master/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzIzMDY4Mg==,size_16,color_FFFFFF,t_70.png)

**由此可见，Kafka的复制机制既不是完全的同步复制，也不是单纯的异步复制。**

事实上，同步复制要求所有能工作的follower都复制完，这条消息才会被commit，这种复制方式极大的影响了吞吐率。而异步复制方式下，follower异步的从leader复制数据，数据只要被leader写入log就被认为已经commit，这种情况下如果follower都还没有复制完，落后于leader时，突然leader宕机，则会丢失数据。而Kafka的这种使用ISR的方式则很好的均衡了确保数据不丢失以及吞吐率。

Kafka的ISR的管理最终都会反馈到Zookeeper节点上。具体位置为：**/brokers/topics/[topic]/partitions/[partition]/state**

**目前有两个地方会对这个Zookeeper的节点进行维护：**

1. Controller来维护：Kafka集群中的其中一个Broker会被选举为Controller，主要负责Partition管理和副本状态管理，也会执行类似于重分配partition之类的管理任务。在符合某些特定条件下，Controller下的LeaderSelector会选举新的leader，ISR和新的leader_epoch及controller_epoch写入Zookeeper的相关节点中。同时发起LeaderAndIsrRequest通知所有的replicas。
2. leader来维护：leader有单独的线程定期检测ISR中follower是否脱离ISR, 如果发现ISR变化，则会将新的ISR的信息返回到Zookeeper的相关节点中。



##### HA相关ZooKeeper结构

![img](https://github.com/chou401/pic-md/raw/master/c9a549eafe1ef71c22152b41c728bb58.png)

###### admin

- 该目录下znode只有在有相关操作时才会存在，操作结束时会将其删除
- /admin/reassign_partitions用于将一些Partition分配到不同的broker集合上。对于每个待重新分配的Partition，Kafka会在该znode上存储其所有的Replica和相应的Broker id。该znode由管理进程创建并且一旦重新分配成功它将会被自动移除。

###### broker

- 即/brokers/ids/[brokerId]）存储“活着”的broker信息。
- topic注册信息（/brokers/topics/[topic]），存储该topic的所有partition的所有replica所在的broker id，第一个replica即为preferred replica，对一个给定的partition，它在同一个broker上最多只有一个replica,因此broker id可作为replica id。

###### controller

- /controller -> int (broker id of the controller)存储当前controller的信息
- /controller_epoch -> int (epoch)直接以整数形式存储controller epoch，而非像其它znode一样以JSON字符串形式存储。

##### producer发布消息

###### 写入方式

producer 采用 push 模式将消息发布到 broker，每条消息都被 append 到 patition 中，属于顺序写磁盘（顺序写磁盘效率比随机写内存要高，保障 kafka 吞吐率）。

###### 消息路由

producer 发送消息到 broker 时，会根据分区算法选择将其存储到哪一个 partition。其路由机制为：

- 指定了 patition，则直接使用；
- 未指定 patition 但指定 key，通过对 key 的 value 进行hash 选出一个
- patition 和 key 都未指定，使用轮询选出一个 patition。

###### 写入流程

producer 写入消息序列图如下所示：

![img](https://github.com/chou401/pic-md/raw/master/3b45ec04ecea8ba94ca092b4de195fa5.png)

流程说明：

- producer 先从 zookeeper 的 "/brokers/.../state" 节点找到该 partition 的 leader。
- producer 将消息发送给该 leader。
- leader 将消息写入本地 log。
- followers 从 leader pull 消息，写入本地 log 后 leader 发送 ACK。
- leader 收到所有 ISR 中的 replica 的 ACK 后，增加 HW（high watermark，最后 commit 的 offset） 并向 producer 发送 ACK。

##### broker保存消息

###### 存储方式

物理上把 topic 分成一个或多个 patition（对应 server.properties 中的 num.partitions=3 配置），每个 patition 物理上对应一个文件夹（该文件夹存储该 patition 的所有消息和索引文件），如下：

![img](https://github.com/chou401/pic-md/raw/master/90540631f5560f08b643d73401e9e73e.png)

###### 存储策略

无论消息是否被消费，kafka 都会保留所有消息。有两种策略可以删除旧数据：

> 基于时间：log.retention.hours=168
>
> 基于大小：log.retention.bytes=1073741824

##### Topic的创建和删除

###### 创建topic

创建 topic 的序列图如下所示：

![img](https://github.com/chou401/pic-md/raw/master/83e7411c3ebbb7c7c0d84a758d22e184.png)

流程说明：

- controller 在 ZooKeeper 的 /brokers/topics 节点上注册 watcher，当 topic 被创建，则 controller 会通过 watch 得到该 topic 的 partition/replica 分配。
- controller从 /brokers/ids 读取当前所有可用的 broker 列表，对于 set_p 中的每一个 partition：
  - 从分配给该 partition 的所有 replica（称为AR）中任选一个可用的 broker 作为新的 leader，并将AR设置为新的 ISR
  - 将新的 leader 和 ISR 写入 /brokers/topics/[topic]/partitions/[partition]/state
-  controller 通过 RPC 向相关的 broker 发送 LeaderAndISRRequest

######  删除topic

删除 topic 的序列图如下所示：

![img](https://github.com/chou401/pic-md/raw/master/e0d44eb05a13c1e59b00b7d39ecee0c8.png)

流程说明：

- controller 在 zooKeeper 的 /brokers/topics 节点上注册 watcher，当 topic 被删除，则 controller 会通过 watch 得到该 topic 的 partition/replica 分配。
- 若 delete.topic.enable=false，结束；否则 controller 注册在 /admin/delete_topics 上的 watch 被 fire，controller 通过回调向对应的 broker 发送 StopReplicaRequest。

##### broker failover

kafka broker failover 序列图如下所示：

![img](https://github.com/chou401/pic-md/raw/master/6725b09c54e9183071e778ba1805928b.png)

流程说明：

- controller 在 zookeeper 的 /brokers/ids/[brokerId] 节点注册 Watcher，当 broker 宕机时 zookeeper 会 fire watch
- controller 从 /brokers/ids 节点读取可用broker
- controller决定set_p，该集合包含宕机 broker 上的所有 partition
- 对 set_p 中的每一个 partition
  - 从/brokers/topics/[topic]/partitions/[partition]/state 节点读取 ISR
  - 决定新 leader
  - 将新 leader、ISR、controller_epoch 和 leader_epoch 等信息写入 state 节点
- 通过 RPC 向相关 broker 发送 leaderAndISRRequest 命令

##### controller failover

当 controller 宕机时会触发 controller failover。每个 broker 都会在 zookeeper 的 "/controller" 节点注册 watcher，当 controller 宕机时 zookeeper 中的临时节点消失，所有存活的 broker 收到 fire 的通知，每个 broker 都尝试创建新的 controller path，只有一个竞选成功并当选为 controller。

当新的 controller 当选时，会触发 KafkaController.onControllerFailover 方法，在该方法中完成如下操作：

1. 读取并增加 Controller Epoch。
2. 在 reassignedPartitions Patch(/admin/reassign_partitions) 上注册 watcher。
3. 在 preferredReplicaElection Path(/admin/preferred_replica_election) 上注册 watcher。
4. 通过 partitionStateMachine 在 broker Topics Patch(/brokers/topics) 上注册 watcher。
5. 若 delete.topic.enable=true（默认值是 false），则 partitionStateMachine 在 Delete Topic Patch(/admin/delete_topics) 上注册 watcher。
6. 通过 replicaStateMachine在 Broker Ids Patch(/brokers/ids)上注册Watch。
7. 初始化 ControllerContext 对象，设置当前所有 topic，“活”着的 broker 列表，所有 partition 的 leader 及 ISR等。
8. 启动 replicaStateMachine 和 partitionStateMachine。
9. 将 brokerState 状态设置为 RunningAsController。
10. 将每个 partition 的 Leadership 信息发送给所有“活”着的 broker。
11. 若 auto.leader.rebalance.enable=true（默认值是true），则启动 partition-rebalance 线程。 
12. 若 delete.topic.enable=true 且Delete Topic Patch(/admin/delete_topics)中有值，则删除相应的Topic。

##### Kafka在zookeeper中存储结构图

![img](https://github.com/chou401/pic-md/raw/master/e577bb59a08c0b336dac332039d5bb6f.png)

###### topic注册信息

- /brokers/topics/[topicName] :
- 存储某个topic的partitions所有分配信息。

 我们输入zkCli.sh进入zookeeper客户端。

使用：get /brokers/topics/topic-test，可以看到某个topic的存储信息。

###### partition状态信息

- /brokers/topics/[topicName]/partitions/[0...N]  其中[0..N]表示partition索引号
- /brokers/topics/[topicName]/partitions/[partitionId]/state

>  "controller_epoch": 表示kafka集群中的中央控制器选举次数,
>
> "leader": 表示该partition选举leader的brokerId,
>
> "version": 版本编号默认为1,
>
> "leader_epoch": 该partition leader选举次数,
>
> "isr": [同步副本组brokerId列表]

###### Broker注册信息

- /brokers/ids/[0...N]
- 每个broker的配置文件中都需要指定一个数字类型的id(全局不可重复),此节点为临时znode(EPHEMERAL)

>  "jmx_port": jmx端口号,
>
> "timestamp": kafka broker初始启动时的时间戳,
>
> "host": 主机名或ip地址,
>
> "version": 版本编号默认为1,
>
> "port": kafka broker的服务端端口号,由server.properties中参数port确定
>

###### Controller epoch

- /controller_epoch --> int (epoch)  
- 此值为一个数字,kafka集群中第一个broker第一次启动时为1，以后只要集群中center controller中央控制器所在broker变更或挂掉，就会重新选举新的center controller，每次center controller变更controller_epoch值就会 + 1;


######  Controller注册信息

- /controller -> int (broker id of the controller)  存储center controller中央控制器所在kafka broker的信息

>  "version": 版本编号默认为1,
> "brokerid": kafka集群中broker唯一编号,
> "timestamp": kafka broker中央控制器变更时的时间戳



#### RocketMQ

RocketMQ 是阿里巴巴在 2012 年开源的消息队列产品，用**纯 Java 语言**实现，在设计时参考了 Kafka，并做出了自己的一些改进，后来捐赠给 Apache 软件基金会，2017 正式毕业，成为 Apache 的顶级项目。RocketMQ 在阿里内部被广泛应用在订单，交易，充值，流计算，消息推送，日志流式处理，Binglog 分发等场景。经历过多次双十一考验，它的性能、稳定性和可靠性都是值得信赖的。

RocketMQ 有着不错的性能，稳定性和可靠性，具备一个现代的消息队列应该有的几乎全部功能和特性，并且它还在持续的成长中。

RocketMQ 有非常活跃的中文社区，大多数问题可以找到中文的答案。RocketMQ 使用 Java 语言开发，源代码相对比较容易读懂，容易对 RocketMQ 进行扩展或者二次开发。

RocketMQ 对在线业务的响应时延做了很多的优化，大多数情况下可以做到毫秒级的响应，如果你的应用场景很在意响应时延，那应该选择使用 RocketMQ。

RocketMQ 的性能比 RabbitMQ 要高一个数量级，每秒钟大概能处理几十万条消息。

RocketMQ 的劣势是与周边生态系统的集成和兼容程度不够。

##### 基础概念

- **Producer**： 消息生产者，负责产生消息，一般由业务系统负责产生消息。
- **Producer Group**：消息生产者组，简单来说就是多个发送同一类消息的生产者称之为一个生产者。
- **Consumer**：消息消费者，负责消费消息，一般是后台系统负责异步消费。
- **Consumer Group**：消费者组，和生产者类似，消费同一类消息的多个 Consumer 实例组成一个消费者组。
- **Topic**：主题，用于将消息按主题做划分，Producer将消息发往指定的Topic，Consumer订阅该Topic就可以收到这条消息。
- **Message**：消息，每个message必须指定一个topic，Message 还有一个可选的 Tag 设置，以便消费端可以基于 Tag 进行过滤消息。
- **Tag**：标签，子主题（二级分类）对topic的进一步细化,用于区分同一个主题下的不同业务的消息。
- **Broker**：Broker是RocketMQ的核心模块，负责接收并存储消息，同时提供Push/Pull接口来将消息发送给Consumer。Broker同时提供消息查询的功能，可以通过MessageID和MessageKey来查询消息。Borker会将自己的Topic配置信息实时同步到NameServer。
- **Queue**：Topic和Queue是1对多的关系，一个Topic下可以包含多个Queue，主要用于负载均衡，Queue数量设置建议不要比消费者数少。发送消息时，用户只指定Topic，Producer会根据Topic的路由信息选择具体发到哪个Queue上。Consumer订阅消息时，会根据负载均衡策略决定订阅哪些Queue的消息。
- **Offset**：RocketMQ在存储消息时会为每个Topic下的每个Queue生成一个消息的索引文件，每个Queue都对应一个Offset记录当前Queue中消息条数。
- **NameServer**：NameServer可以看作是RocketMQ的注册中心，它管理两部分数据：集群的Topic-Queue的路由配置；Broker的实时配置信息。其它模块通过Nameserv提供的接口获取最新的Topic配置和路由信息；各 NameServer 之间不会互相通信， 各 NameServer 都有完整的路由信息，即无状态。
  - **Producer/Consumer** ：通过查询接口获取Topic对应的Broker的地址信息和Topic-Queue的路由配置
  - **Broker** ： 注册配置信息到NameServer， 实时更新Topic信息到NameServer

##### RocketMQ 消费模式

###### 广播模式

一条消息被多个Consumer消费，即使这些Consumer属于同一个Consumer Group，消息也会被Consumer Group中的每一个Consumer都消费一次。

```java
// 设置广播模式
consumer.setMessageModel(MessageModel.BROADCASTING);
```

###### 集群模式

一个Consumer Group中的所有Consumer平均分摊消费消息(组内负载均衡)。

```java
// 设置集群模式 也就是负载均衡模式
consumer.setMessageModel(MessageModel.CLUSTERING);
```

##### 基础架构

RocketMq 使用轻量级的NameServer服务进行服务的协调和治理工作，NameServer多节点部署时相互独立互不干扰。每一个rocketMq服务节点（broker节点）启动时都会遍历配置的NameServer列表并建立长链接，broker节点每30秒向NameServer发送一次心跳信息、NameServer每10秒会检查一次连接的broker是否存活。消费者和生产者会随机选择一个NameServer建立长连接，通过定期轮训更新的方式获取最新的服务信息。架构简图如下：

![img](https://img-blog.csdnimg.cn/b6c77636a91a422893e3be52a1cdb111.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA572X5b-X5a6P,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

1. **NameServer**：启动，监听端口，等待producer，consumer，broker连接上来。
2. **Broker**：启动，与nameserver保持长链接，定期向nameserver发送心跳信息，包含broker的ip，端口，当前broker上topic的信息。
3. **producer**：启动，随机选择一个NameServer建立长连接，拿到broker的信息，然后就可以给broker发送消息了。
4. **consumer**：启动，随机选择一个NameServer建立长连接，拿到broker的信息，然后就可以建立通道，消费消息。

###### Broker 的存储结构

RocketMQ 存储用的是**本地文件存储系统**，将所有topic的消息全部写入同一个文件中（commit log），这样保证了IO写入的绝对顺序性，最大限度利用IO系统顺序读写带来的优势提升写入速度。

由于消息混合存储在一起，需要将每个消费者组消费topic最后的**偏移量**记录下来。这个文件就是consumer queue（索引文件）。所以消息在写入commit log 文件的同时还需将偏移量信息写入consumer queue文件。在索引文件中会记录消息的物理位置、偏移量offse，消息size等，消费者消费时根据上述信息就可以从commit log文件中快速找到消息信息。

Broker 存储结构如下：

![img](https://img-blog.csdnimg.cn/4d4f6c0165fc40e1866af2f46d112a74.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA572X5b-X5a6P,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

###### 存储文件简介

- **Commit log**：消息存储文件，RocketMQ会对commit log文件进行分割（默认大小1GB），新文件以消息最后一条消息的偏移量命名。（比如 00000000000000000000 代表了第一个文件，第二个文件名就是 00000000001073741824，表明起始偏移量为 1073741824）。
- **Consumer queue**：消息消费队列（也是个文件），可以根据消费者数量设置多个，一个Topic 下的某个 Queue，每个文件约 5.72M，由 30w 条数据组成；ConsumeQueue 存储的条目是固定大小，只会存储 8 字节的 commitlog 物理偏移量，4 字节的消息长度和 8 字节 Tag 的哈希值，固定 20 字节；消费者是先从 ConsumeQueue 来得到消息真实的物理地址，然后再去 CommitLog 获取消息。
- **IndexFile**：索引文件，是额外提供查找消息的手段，通过 Key 或者时间区间来查询对应的消息。

**整个流程简介**

Producer 使用轮询的方式分别向每个 Queue 中发送消息。

Consumer 启动的时候会在 Topic，Consumer group 维度发生负载均衡，为每个客户端分配需要处理的 Queue。负载均衡过程中每个客户端都获取到全部的的 ConsumerID 和所有 Queue 并进行排序，每个客户端使用相同负责均衡算法，例如平均分配的算法，这样每个客户端都会计算出自己需要消费那些 Queue，每当 Consumer 增加或减少就会触发负载均衡，所以我们可以通过 RocketMQ 负载均衡机制实现动态扩容，提升客户端收发消息能力。客户端负责均衡为客户端分配好 Queue 后，客户端会不断向 Broker 拉取消息，在客户端进行消费。

**可以一直增加客户端的数量提升消费能力吗**？

当然不可以，因为 Queue 数量有限，客户端数量一旦达到 Queue 数量，再扩容新节点无法提升消费能力，因为会有节点分配不到 Queue 而无法消费。

###### Consumer 端的负载均衡机制

topic 在创建之处可以设置 comsumer queue数量。而 comsumer 在启动时会和comsumer queue绑定，这个绑定策略是咋样的？

![img](https://img-blog.csdnimg.cn/0466d1dbc23440d4bffb60adb710bad1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA572X5b-X5a6P,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

- **默认策略**：
  - queue 个数大于 Consumer个数， 那么 Consumer 会平均分配 queue，不够平均，会根据clientId排序来拿取余数
  - queue个数小于Consumer个数，那么会有Consumer闲置，就是浪费掉了，其余Consumer平均分配到queue
- **一致性hash算法**
- **就近元则，离的近的消费**
- **每个消费者依次消费一个queue，环状**
- **自定义方式**

**天然弊端**：

RocketMQ 采用一个 consumer 绑定一个或者多个 Queue 模式，假如某个消费者服务器挂了，则会造成部分Queue消息堆积。

###### 消息刷盘机制

- **同步刷盘**：当消息持久化完成后，Broker才会返回给Producer一个ACK响应，可以保证消息的可靠性，但是性能较低。
- **异步刷盘**：只要消息写入PageCache即可将成功的ACK返回给Producer端。消息刷盘采用后台异步线程提交的方式进行，降低了读写延迟，提高了RocketMQ的性能和吞吐量。

###### Mmap + pageCache

RocketMQ 底层对 commitLog、consumeQueue 之类的磁盘文件的读写操作都采用了 mmap 技术。

**传统 IO**

传统 I/O 的工作方式是，数据读取和写入是从用户空间到内核空间来回复制，而内核空间的数据是通过操作系统层面的 I/O 接口从磁盘读取或写入。

传统IO**发生了 4 次用户态与内核态的上下文切换**，因为发生了两次系统调用，一次是 `read()` ，一次是 `write()`，每次系统调用都得先从用户态切换到内核态，等内核完成任务后，再从内核态切换回用户态。

其次，还**发生了 4 次数据拷贝**，其中两次是 DMA 的拷贝，另外两次则是通过 CPU 拷贝的。

**传统IO，write() 过程是怎样？**

wirte() 写请求 和 read()，需要先写入用户缓存区，然后通过系统调用，CPU 拷贝数据从用户缓存区到内核缓存区，再从内核缓存区拷贝到磁盘文件！

- 第一次拷贝，把磁盘上的数据拷贝到操作系统内核的缓冲区里，这个拷贝的过程是通过 DMA 搬运的。
- 第二次拷贝，把内核缓冲区的数据拷贝到用户的缓冲区里，于是我们应用程序就可以使用这部分数据了，这个拷贝到过程是由 CPU 完成的（用户态不能直接操作内核态缓存区，所以需要拷贝到用户态才能使用）。
- 第三次拷贝，把刚才拷贝到用户的缓冲区里的数据，再拷贝到内核的 socket 的缓冲区里，这个过程依然还是由 CPU 搬运的。
- 第四次拷贝，把内核的 socket 缓冲区里的数据，拷贝到网卡的缓冲区里，这个过程又是由 DMA 搬运的。

我们回过头看这个文件传输的过程，我们只是搬运一份数据，结果却搬运了 4 次，过多的数据拷贝无疑会消耗 CPU 资源，大大降低了系统性能。

这种简单又传统的文件传输方式，存在冗余的上文切换和数据拷贝，在高并发系统里是非常糟糕的，多了很多不必要的开销，会严重影响系统性能。

所以，要想提高文件传输的性能，就需要**减少「用户态与内核态的上下文切换」和「内存拷贝」的次数**。

**如何优化文件传输的性能？**
先来看看，如何减少「用户态与内核态的上下文切换」的次数呢？

读取磁盘数据的时候，之所以要发生上下文切换，这是因为用户空间没有权限操作磁盘或网卡，内核的权限最高，这些操作设备的过程都需要交由操作系统内核来完成，所以一般要通过内核去完成某些任务的时候，就需要使用操作系统提供的系统调用函数。

而一次系统调用必然会发生 2 次上下文切换：首先从用户态切换到内核态，当内核执行完任务后，再切换回用户态交由进程代码执行。

所以，要想减少上下文切换到次数，就要减少系统调用的次数。

**再来看看，如何减少「数据拷贝」的次数？**

在前面我们知道了，传统的文件传输方式会历经 4 次数据拷贝，而且这里面，「从内核的读缓冲区拷贝到用户的缓冲区里，再从用户的缓冲区里拷贝到 socket 的缓冲区里」，这个过程是没有必要的。

因为文件传输的应用场景中，在用户空间我们并不会对数据「再加工」，所以数据实际上可以不用搬运到用户空间，因此**用户的缓冲区是没有必要存在的**。

**Mmap（内存映射）**

`read()` 系统调用的过程中会把内核缓冲区的数据拷贝到用户的缓冲区里，于是为了减少这一步开销，我们可以用 `mmap()` 替换 `read()` 系统调用函数。

`mmap()` 系统调用函数会把文件磁盘地址「**映射**」到内核缓存区（page cache），而内核缓存区会 「**映射**」到用户空间（虚拟地址）。这样，操作系统内核与用户空间就不需要再进行任何的数据拷贝。

> 注意，这里用户空间（虚拟地址）不是直接映射到文件磁盘地址，而是文件对应的 page cache，用户虚拟地址一般是和用户内存地址「映射」的，如果使用内存映射技术，则用户虚拟地址可以和内核内存地址「映射」。
> 根据维基百科给出的定义：在大多数操作系统中，映射的内存区域实际上是内核的page cache，这意味着不需要在用户空间创建副本。多个进程之间也可以通过同时映射 page cache，来进行进程通信）

**mmap() 函数简介**

```java
void * mmap(void *start, size_t length, int prot , int flags, int fd, off_t offset)
```

- **start**：要映射到的内存区域的起始地址，通常都是用NULL（NULL即为0）。NULL表示由内核来指定该内存地址
- **offset**：以文件开始处的偏移量, 必须是分页大小的整数倍, 通常为0, 表示从文件头开始映射
- **length**：将文件的多大长度映射到内存（每次创建新 commitlog 会默认指定长度 1GB）
- **prot**： 映射区的保护方式（PROT_EXEC: 映射区可被执行、PROT_READ: 映射区可被读取、PROT_WRITE: 映射区可被写入、PROT_NONE: 映射区不能存取）
- **flags**： 映射区的特性
- **fd**：文件描述符（由open函数返回）

从磁盘拷贝到内核空间的页缓存 (page cache)，然后将用户空间的虚拟地址映射到内核的page cache，这样不需要再将页面从内核空间拷贝到用户空间了。

![img](https://img-blog.csdnimg.cn/img_convert/6e9223b86e2fad24b14e5a31df1879e0.png)

简述上述过程

1. 应用进程调用了 mmap() 后，DMA 会把磁盘的数据拷贝到内核的缓冲区里。接着，应用进程跟操作系统内核「共享」这个缓冲区；
2. 应用进程再调用 write()，操作系统直接将内核缓冲区的数据拷贝到 socket 缓冲区中，这一切都发生在内核态，由 CPU 来搬运数据；
3. 应用进程再调用 write()，操作系统直接将内核缓冲区的数据拷贝到 socket 缓冲区中，这一切都发生在内核态，由 CPU 来搬运数据；
4. 最后，把内核的 socket 缓冲区里的数据，拷贝到网卡的缓冲区里，这个过程是由 DMA 搬运的。

**使用 mmap() 写数据到磁盘文件会怎样？**

mmap() 将用户虚拟地址映射内核缓存区（内存物理地址）后，写数据直接将数据写入内核缓存区，只需要经过一次CPU拷贝，将数据从内核缓存区拷贝到磁盘文件；比传统 IO 的 write() 操作少了一次数据拷贝的过程！

但这还不是最理想的零拷贝，因为仍然需要通过 CPU 把内核缓冲区的数据拷贝到 socket 缓冲区里，而且仍然需要 4 次上下文切换，因为系统调用还是 2 次。

**pageCache**

在传统IO过程中，其中第一步都是先需要先把磁盘文件数据拷贝「内核缓冲区」里，这个「内核缓冲区」实际上是**磁盘高速缓存（PageCache）**。

**预映射机制 + 文件预热机制**

Broker针对上述的磁盘文件高性能读写机制做的一些优化：

- 内存预映射机制：Broker 会针对磁盘上的各种 CommitLog、ConsumeQueue 文件预先分配好MappedFile，也就是提前对一些可能接下来要读写的磁盘文件，提前使用 MappedByteBuffer 执行 mmap() 函数完成内存映射，这样后续读写文件的时候，就可以直接执行了（减少一次 CPU 拷贝）。


- 文件预热：在提前对一些文件完成内存映射之后，因为内存映射不会直接将数据从磁盘加载到内存里来，那么后续在读，取尤其是 CommitLog、ConsumeQueue 文件时候，其实有可能会频繁的从磁盘里加载数据到内存中去。所以，在执行完 mmap() 函数之后，还会进行 madvise() 系统调用，就是提前尽可能将磁盘文件加载到内存里去。(读磁盘 -> 读内存)。

###### Topic 分片

为了突破单个机器容量上限和单个机器读写性能，RocketMQ 支持 topic 数据分片。

架构图如下：

![img](https://img-blog.csdnimg.cn/6dda18d3001c4db5b2fc0f84f7a8ba88.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA572X5b-X5a6P,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

###### 查缺补漏

**消息的全局顺序和局部顺序**

- **全局顺序**：一个 Topic 一个队列，Producer 和 Consuemr 的并发都为一。
- **局部顺序**：某个队列消息是顺序的。

**零拷贝（Zero-copy）**

sendfile：

在 Linux 内核版本 2.1 中，提供了一个专门发送文件的系统调用函数 `sendfile()`，函数形式如下：

```cpp
#include <sys/socket.h>
ssize_t sendfile(int out_fd, int in_fd, off_t *offset, size_t count);
```

它的前两个参数分别是目的端和源端的文件描述符，后面两个参数是源端的偏移量和复制数据的长度，返回值是实际复制数据的长度。

首先，它可以替代前面的 read() 和 write() 这两个系统调用，这样就可以减少一次系统调用，也就减少了 2 次上下文切换的开销。

其次，该系统调用，可以直接把内核缓冲区里的数据拷贝到 socket 缓冲区里，不再拷贝到用户态，这样就只有 2 次上下文切换，和 3 次数据拷贝。如下图：

![img](https://img-blog.csdnimg.cn/img_convert/87c6530cba656a8139b6def9b0db570b.png)

但是这还不是真正的零拷贝技术，如果网卡支持 SG-DMA（The Scatter-Gather Direct Memory Access）技术（和普通的 DMA 有所不同），我们可以进一步减少通过 CPU 把内核缓冲区里的数据拷贝到 socket 缓冲区的过程。

你可以在你的 Linux 系统通过下面这个命令，查看网卡是否支持 scatter-gather 特性：

```cobol
$ ethtool -k eth0 | grep scatter-gather
scatter-gather: on
```

于是，从 Linux 内核 2.4 版本开始起，对于支持网卡支持 SG-DMA 技术的情况下， sendfile() 系统调用的过程发生了点变化，具体过程如下：

1. 第一步，通过 DMA 将磁盘上的数据拷贝到内核缓冲区里；
2. 第二步，缓冲区描述符和数据长度传到 socket 缓冲区，这样网卡的 SG-DMA 控制器就可以直接将内核缓存中的数据拷贝到网卡的缓冲区里，此过程不需要将数据从操作系统内核缓冲区拷贝到 socket 缓冲区中，这样就减少了一次数据拷贝；

所以，这个过程之中，只进行了 2 次数据拷贝，如下图：

![img](https://img-blog.csdnimg.cn/img_convert/971767ef19a202042cdefcd3be6b4fa7.png)

这就是所谓的**零拷贝（Zero-copy）技术，因为我们没有在内存层面去拷贝数据，也就是说全程没有通过 CPU 来搬运数据，所有的数据都是通过 DMA 来进行传输的**。

零拷贝技术的文件传输方式相比传统文件传输的方式，减少了 2 次上下文切换和数据拷贝次数，**只需要 2 次上下文切换和数据拷贝次数，就可以完成文件的传输，而且 2 次的数据拷贝过程，都不需要通过 CPU，2 次都是由 DMA 来搬运**。

所以，总体来看，**零拷贝技术可以把文件传输的性能提高至少一倍以上**。

**使用零拷贝技术的项目**

事实上，Kafka 这个开源项目，就利用了「零拷贝」技术，从而大幅提升了 I/O 的吞吐率，这也是 Kafka 在处理海量数据为什么这么快的原因之一。

如果你追溯 Kafka 文件传输的代码，你会发现，最终它调用了 Java NIO 库里的 `transferTo`方法：

```java
@Overridepublic 
long transferFrom(FileChannel fileChannel, long position, long count) throws IOException { 
    return fileChannel.transferTo(position, count, socketChannel);
}
```

如果 Linux 系统支持 `sendfile()` 系统调用，那么 `transferTo()` 实际上最后就会使用到 `sendfile()` 系统调用函数。

另外，Nginx 也支持零拷贝技术，一般默认是开启零拷贝技术，这样有利于提高文件传输的效率，是否开启零拷贝技术的配置如下：

```cobol
http {
...
    sendfile on
...
}
```

sendfile 配置的具体意思: 

- 设置为 on 表示，使用零拷贝技术来传输文件：sendfile ，这样只需要 2 次上下文切换，和 2 次数据拷贝。
- 设置为 off 表示，使用传统的文件传输技术：read + write，这时就需要 4 次上下文切换，和 4 次数据拷贝。

当然，要使用 sendfile，Linux 内核版本必须要 2.1 以上的版本。

**PageCache 有什么作用？**

回顾前面说道文件传输过程，其中第一步都是先需要先把磁盘文件数据拷贝「内核缓冲区」里，这个「内核缓冲区」实际上是磁盘高速缓存（PageCache）。

由于零拷贝使用了 PageCache 技术，可以使得零拷贝进一步提升了性能，我们接下来看看 PageCache 是如何做到这一点的。

读写磁盘相比读写内存的速度慢太多了，所以我们应该想办法把「读写磁盘」替换成「读写内存」。于是，我们会通过 DMA 把磁盘里的数据搬运到内存里，这样就可以用读内存替换读磁盘。

但是，内存空间远比磁盘要小，内存注定只能拷贝磁盘里的一小部分数据。

那问题来了，选择哪些磁盘数据拷贝到内存呢？

我们都知道程序运行的时候，具有「局部性」，所以通常，刚被访问的数据在短时间内再次被访问的概率很高，于是我们可以用 PageCache 来缓存最近被访问的数据，当空间不足时淘汰最久未被访问的缓存。

所以，读磁盘数据的时候，优先在 PageCache 找，如果数据存在则可以直接返回；如果没有，则从磁盘中读取，然后缓存 PageCache 中。

还有一点，读取磁盘数据的时候，需要找到数据所在的位置，但是对于机械磁盘来说，就是通过磁头旋转到数据所在的扇区，再开始「顺序」读取数据，但是旋转磁头这个物理动作是非常耗时的，为了降低它的影响，PageCache 使用了「预读功能」。

比如，假设 read 方法每次只会读 32 KB 的字节，虽然 read 刚开始只会读 0 ～ 32 KB 的字节，但内核会把其后面的 32～64 KB 也读取到 PageCache，这样后面读取 32～64 KB 的成本就很低，如果在 32～64 KB 淘汰出 PageCache 前，进程读取到它了，收益就非常大。

所以，PageCache 的优点主要是两个：

- 缓存最近被访问的数据；
- 预读功能；

这两个做法，将大大提高读写磁盘的性能。

但是，在传输大文件（GB 级别的文件）的时候，PageCache 会不起作用，那就白白浪费多做的一次数据拷贝，造成性能的降低，即使使用了 PageCache 的零拷贝也会损失性能

这是因为如果你有很多 GB 级别文件需要传输，每当用户访问这些大文件的时候，内核就会把它们载入 PageCache 中，于是 PageCache 空间很快被这些大文件占满。

另外，由于文件太大，可能某些部分的文件数据被再次访问的概率比较低，这样就会带来 2 个问题：

- PageCache 由于长时间被大文件占据，其他「热点」的小文件可能就无法充分使用到 PageCache，于是这样磁盘读写的性能就会下降了；
- PageCache 中的大文件数据，由于没有享受到缓存带来的好处，但却耗费 DMA 多拷贝到 PageCache 一次；

所以，针对大文件的传输，不应该使用 PageCache，也就是说不应该使用零拷贝技术，因为可能由于 PageCache 被大文件占据，而导致「热点」小文件无法利用到 PageCache，这样在高并发的环境下，会带来严重的性能问题。

**大文件传输用什么方式实现？**

那针对大文件的传输，我们应该使用什么方式呢？

我们先来看看最初的例子，当调用 read 方法读取文件时，进程实际上会阻塞在 read 方法调用，因为要等待磁盘数据的返回，如下图：

![img](https://img-blog.csdnimg.cn/img_convert/de0ebc5fe89fb2999683504f3cca815b.png)

具体过程：

- 当调用 read 方法时，会阻塞着，此时内核会向磁盘发起 I/O 请求，磁盘收到请求后，便会寻址，当磁盘数据准备好后，就会向内核发起 I/O 中断，告知内核磁盘数据已经准备好；
- 内核收到 I/O 中断后，就将数据从磁盘控制器缓冲区拷贝到 PageCache 里；
- 最后，内核再把 PageCache 中的数据拷贝到用户缓冲区，于是 read 调用就正常返回了。

对于阻塞的问题，可以用异步 I/O 来解决，它工作方式如下图：

![img](https://img-blog.csdnimg.cn/img_convert/138296dc824bf3e5ab8576bdfb0be80b.png)

 它把读操作分为两部分：

- 前半部分，内核向磁盘发起读请求，但是可以**不等待数据就位就可以返回**，于是进程此时可以处理其他任务；
- 后半部分，当内核将磁盘中的数据拷贝到进程缓冲区后，进程将接收到内核的**通知**，再去处理数据；

而且，我们可以发现，异步 I/O 并没有涉及到 PageCache，所以使用异步 I/O 就意味着要绕开 PageCache。

绕开 PageCache 的 I/O 叫直接 I/O，使用 PageCache 的 I/O 则叫缓存 I/O。通常，对于磁盘，异步 I/O 只支持直接 I/O。

前面也提到，大文件的传输不应该使用 PageCache，因为可能由于 PageCache 被大文件占据，而导致「热点」小文件无法利用到 PageCache。

于是，在高并发的场景下，针对大文件的传输的方式，应该使用「异步 I/O + 直接 I/O」来替代零拷贝技术。

直接 I/O 应用场景常见的两种：

- 应用程序已经实现了磁盘数据的缓存，那么可以不需要 PageCache 再次缓存，减少额外的性能损耗。在 MySQL 数据库中，可以通过参数设置开启直接 I/O，默认是不开启；
- 传输大文件的时候，由于大文件难以命中 PageCache 缓存，而且会占满 PageCache 导致「热点」文件无法充分利用缓存，从而增大了性能开销，因此，这时应该使用直接 I/O。

另外，由于直接 I/O 绕过了 PageCache，就无法享受内核的这两点的优化：

- 内核的 I/O 调度算法会缓存尽可能多的 I/O 请求在 PageCache 中，最后「**合并**」成一个更大的 I/O 请求再发给磁盘，这样做是为了减少磁盘的寻址操作；
- 内核也会「**预读**」后续的 I/O 请求放在 PageCache 中，一样是为了减少对磁盘的操作；

于是，传输大文件的时候，使用「异步 I/O + 直接 I/O」了，就可以无阻塞地读取文件了。

所以，传输文件的时候，我们要根据文件的大小来使用不同的方式：

- 传输大文件的时候，使用「异步 I/O + 直接 I/O」；
- 传输小文件的时候，则使用「零拷贝技术」；

 在 nginx 中，我们可以用如下配置，来根据文件的大小来使用不同的方式：

```cobol
location /video/ { 
    sendfile on; 
    aio on; 
    directio 1024m; 
}
```

当文件大小大于 `directio` 值后，使用「异步 I/O + 直接 I/O」，否则使用「零拷贝技术」。

###### 总结

早期 I/O 操作，内存与磁盘的数据传输的工作都是由 CPU 完成的，而此时 CPU 不能执行其他任务，会特别浪费 CPU 资源。

于是，为了解决这一问题，DMA 技术就出现了，每个 I/O 设备都有自己的 DMA 控制器，通过这个 DMA 控制器，CPU 只需要告诉 DMA 控制器，我们要传输什么数据，从哪里来，到哪里去，就可以放心离开了。后续的实际数据传输工作，都会由 DMA 控制器来完成，CPU 不需要参与数据传输的工作。

传统 IO 的工作方式，从硬盘读取数据，然后再通过网卡向外发送，我们需要进行 4 上下文切换，和 4 次数据拷贝，其中 2 次数据拷贝发生在内存里的缓冲区和对应的硬件设备之间，这个是由 DMA 完成，另外 2 次则发生在内核态和用户态之间，这个数据搬移工作是由 CPU 完成的。

为了提高文件传输的性能，于是就出现了零拷贝技术，它通过一次系统调用（sendfile 方法）合并了磁盘读取与网络发送两个操作，降低了上下文切换次数。另外，拷贝数据都是发生在内核中的，天然就降低了数据拷贝的次数。

Kafka 和 Nginx 都有实现零拷贝技术，这将大大提高文件传输的性能。

零拷贝技术是基于 PageCache 的，PageCache 会缓存最近访问的数据，提升了访问缓存数据的性能，同时，为了解决机械硬盘寻址慢的问题，它还协助 I/O 调度算法实现了 IO 合并与预读，这也是顺序读比随机读性能好的原因。这些优势，进一步提升了零拷贝的性能。

需要注意的是，零拷贝技术是不允许进程对文件内容作进一步的加工的，比如压缩数据再发送。

另外，当传输大文件时，不能使用零拷贝，因为可能由于 PageCache 被大文件占据，而导致「热点」小文件无法利用到 PageCache，并且大文件的缓存命中率不高，这时就需要使用「异步 IO + 直接 IO 」的方式。

在 Nginx 里，可以通过配置，设定一个文件大小阈值，针对大文件使用异步 IO 和直接 IO，而对小文件使用零拷贝。

**Kafka为什么那么快？**

- 顺序读写+mmap，使用linux内核提供的mmap api，mmap将磁盘文件（设备内存）映射到内存，支持读和写，对内存的操作就会反映在磁盘文件上，但这不是实时写入磁盘的，操作系统会在程序主动调用flush的时候才真正的把数据写到硬盘（kafka也提供了一个参数——producer.type来控制是不是主动flush，如果kafaka写入到mmap之后立即flush然后返回producer叫同步，写入mmap之后立即返回producer叫异步）。
- 零拷贝，使用linux内核提供的sendfile api，sendfile是将读到内核空间的数据，直接转到socket buffer，进行网络发送。 

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYwMTE0MTk1MDMzNTA5?x-oss-process=image/format,png)

**什么事mmap？**

mmap是操作这些设备的一种方法，所谓操作设备，比如IO端口（点亮一个LED）、LCD控制器、磁盘控制器，实际上就是往设备的物理地址读写数据。

但是，由于应用程序不能直接操作设备硬件地址，所以操作系统提供了这样的一种机制——内存映射，把设备地址映射到进程虚拟地址，mmap就是实现内存映射的接口。

mmap的好处是，mmap把设备内存映射到虚拟内存，则用户操作虚拟内存相当于直接操作设备了，省去了用户空间到内核空间的复制过程，相对IO操作来说，增加了数据的吞吐量。

**什么是内存映射？**

既然mmap是实现内存映射的接口，那么内存映射是什么呢？看下图：

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYwMTE0MjAwMTMxODE1?x-oss-process=image/format,png)

每个进程都有独立的进程地址空间，通过页表和MMU，可将虚拟地址转换为物理地址，每个进程都有独立的页表数据，这可解释为什么两个不同进程相同的虚拟地址，却对应不同的物理地址。

**什么是虚拟地址空间？**

每个进程都有4G的虚拟地址空间，其中3G用户空间，1G内核空间（linux），每个进程共享内核空间，独立的用户空间，下图形象地表达了这点

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYwMTE0MjAwNzU5MTc0?x-oss-process=image/format,png)

驱动程序运行在内核空间，所以驱动程序是面向所有进程的。

用户空间切换到内核空间有两种方法：

（1）系统调用，即软中断

（2）硬件中断

**虚拟地址空间里面是什么？**

了解了什么是虚拟地址空间，那么虚拟地址空间里面装的是什么？看下图

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYwMTE0MjAxMzAzMzkx?x-oss-process=image/format,png)

虚拟空间装的大概是上面那些数据了，内存映射大概就是把设备地址映射到上图的红色段了，暂且称其为“内存映射段”，至于映射到哪个地址，是由操作系统分配的，操作系统会把进程空间划分为三个部分：

（1）未分配的，即进程还未使用的地址

（2）缓存的，缓存在ram中的页

（3）未缓存的，没有缓存在ram中

操作系统会在未分配的地址空间分配一段虚拟地址，用来和设备地址建立映射。

页缓存是linux内核一种重要的磁盘高速缓存，它通过软件机制实现。但页缓存和硬件cache的原理基本相同，将容量大而低速设备中的部分数据存放到容量小而快速的设备中，这样速度快的设备将作为低速设备的缓存，当访问低速设备中的数据时，可以直接从缓存中获取数据而不需再访问低速设备，从而节省了整体的访问时间。

页缓存以页为大小进行数据缓存，它将磁盘中最常用和最重要的数据存放到部分物理内存中，使得系统访问块设备时可以直接从主存中获取块设备数据，而不需从磁盘中获取数据。

在大多数情况下，内核在读写磁盘时都会使用页缓存。内核在读文件时，首先在已有的页缓存中查找所读取的数据是否已经存在。如果该页缓存不存在，则一个新的页将被添加到高速缓存中，然后用从磁盘读取的数据填充它。如果当前物理内存足够空闲，那么该页将长期保留在高速缓存中，使得其他进程再使用该页中的数据时不再访问磁盘。写操作与读操作时类似，直接在页缓存中修改数据，但是页缓存中修改的数据(该页此时被称为Dirty Page)并不是马上就被写入磁盘，而是延迟几秒钟，以防止进程对该页缓存中的数据再次修改。

### 主流消息中间件的对比 

| **特性**                 | ActiveMQ                              | **RabbitMQ**                                       | **RocketMQ**                                                 | Kafka                                                        |
| ------------------------ | ------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 单机吞吐量               | 万级，比 RocketMQ、Kafka 低一个数量级 | 同 ActiveMQ                                        | 10 万级，支撑高吞吐                                          | 10 万级，高吞吐，一般配合大数据类的系统来进行实时数据计算、日志采集等场景 |
| topic 数量对吞吐量的影响 |                                       |                                                    | topic 可以达到几百/几千的级别，吞吐量会有较小幅度的下降，这是 RocketMQ 的一大优势，在同等机器下，可以支撑大量的 topic | topic 从几十到几百个时候，吞吐量会大幅度下降，在同等机器下，Kafka 尽量保证 topic 数量不要过多，如果要支撑大规模的 topic，需要增加更多的机器资源 |
| 时效性                   | ms 级                                 | 微秒级，这是 RabbitMQ 的一大特点，延迟最低         | ms 级                                                        | 延迟在 ms 级以内                                             |
| 可用性                   | 高，基于主从架构实现高可用            | 同 ActiveMQ                                        | 非常高，分布式架构                                           | 非常高，分布式，一个数据多个副本，少数机器宕机，不会丢失数据，不会导致不可用 |
| 消息可靠性               | 有较低的概率丢失数据                  | 基本不丢                                           | 经过参数优化配置，可以做到 0 丢失                            | 同 RocketMQ                                                  |
| 功能支持                 | MQ 领域的功能极其完备                 | 基于 erlang 开发，并发能力很强，性能极好，延时很低 | MQ 功能较为完善，还是分布式的，扩展性好                      | 功能较为简单，主要支持简单的 MQ 功能，在大数据领域的实时计算以及日志采集被大规模使用 |





## 缓存



## 疑问

### **守护线程和用户线程有什么区别？**

- 用户（User）线程：运行在前台，执行具体的任务，如程序的主线程、连接网络的子线程等都是用户线程。
- 守护（Darmon）线程：运行在后台，为其他前台线程服务。也可以说守护线程是JVM中非守护线程的“佣人”。一旦所有用户线程都结束运行，守护线程会随JVM一起结束工作。

main函数所在的线程就是一个用户线程，main函数启动的同时在JVM内部同时启动了好多守护线程，比如垃圾回收线程。比较明显的区别之一就是用户线程结束，JVM退出，不管这个时候有没有守护线程运行。而守护线程不会影响JVM的退出。

**注意事项**

1. setDaemon（true）必须在start（）方法前执行，否则会抛出 IllegalThreadStateException 异常。
2. 在守护线程中产生的新线程也是守护线程。
3. 不是所有的任务都可以分配给守护线程来执行，比如读写操作或者计算逻辑。
4. 守护（Darmon）线程中不能依靠 finally 块的内容来确保执行关闭或清理资源的逻辑。因为我们上面说过了一旦所有用户线程都结束运行，守护线程会随JVM一起结束工作，所有守护（Daemon）线程中的finally 语句块可能无法被执行。

### MySQL5.6：Specified key was too long； max key length is 767 bytes

在数据库中，索引的字段设置太长了，导致不支持。**【根本原因：5.6版本的innodb大长度前缀默认是关闭的】**

mysql 建立索引时，数据库计算key的长度是累加所有index用到的字段的char长度，按照一定的比例乘起来不能超过限定的key长度767。

- latin 1 = 1 byte = 1character
- uft8 = 3 byte = 1 character
- utf8mb4 = 4byte = 1character
- gbk = 2 byte = 1 character

```mysql
CREATE TABLE `xxl_job_registry` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `registry_group` varchar(50) NOT NULL,
  `registry_key` varchar(190) NOT NULL,
  `registry_value` varchar(250) NOT NULL,
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `i_g_k_v` (`registry_key`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
 
 
registry_key 190 * 4 = 760因此创建成功
 
若将registry_key的字节数改成192，则195 * 4 = 780 则创建不成功
```

如果是联合索引时，应该是两个索引的字节加起来，然后折算成字节数。

```mysql
CREATE TABLE `xxl_job_registry` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `registry_group` varchar(50) NOT NULL,
  `registry_key` varchar(190) NOT NULL,
  `registry_value` varchar(110) NOT NULL,
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `i_g_k_v` (`registry_key`, `registry_value`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
 
那么索引需要的字节数是：（190 + 110） * 4 = 1200
创建不成功
 
 
但是实际上呢，是能创建成功。
在创建索引的时候进行了优化，取字节数最长的那个 190 * 4 = 760因此能创建成功。
```

**解决方法**

1. 修改索引的varchar字符，只要让字符 * 字节数 < 767 即可。但是有时某个字段的字符数是一定要足够大的，那就用第二种方式。

2. ```mysql
   // 查看
    
   show variables like "innodb_large_prefix";
    
   show variables like "innodb_file_format";
    
   //修改最大索引长度限制
   set global innodb_large_prefix=1;
   或
   set global innodb_large_prefix=on;
    
   set global innodb_file_format=BARRACUDA;
   ```

### 项目开发阶段，有一个关于下单发货的需求：如果今天下午 3 点前进行下单，那么发货时间是明天，如果今天下午 3 点后进行下单，那么发货时间是后天，如果被确定的时间是周日，那么在此时间上再加 1 天为发货时间

```java

final DateTime DISTRIBUTION_TIME_SPLIT_TIME = new DateTime().withTime(15,0,0,0);
private Date calculateDistributionTimeByOrderCreateTime(Date orderCreateTime){
    DateTime orderCreateDateTime = new DateTime(orderCreateTime);
    Date tomorrow = orderCreateDateTime.plusDays(1).toDate();
    Date theDayAfterTomorrow = orderCreateDateTime.plusDays(2).toDate();
    return orderCreateDateTime.isAfter(DISTRIBUTION_TIME_SPLIT_TIME) ? wrapDistributionTime(theDayAfterTomorrow) : wrapDistributionTime(tomorrow);
}
private Date wrapDistributionTime(Date distributionTime){
    DateTime currentDistributionDateTime = new DateTime(distributionTime);
    DateTime plusOneDay = currentDistributionDateTime.plusDays(1);
    boolean isSunday = (DateTimeConstants.SUNDAY == currentDistributionDateTime.getDayOfWeek());
    return isSunday ? plusOneDay.toDate() : currentDistributionDateTime.toDate() ;
}
```

### nacos 多个 ip 挂载 无法下线 did not find the Leader node

删除 nacos 目录下的 protocol，之后重启 nacos

```c
rm -rf protocol
```

关闭nacos 服务

```c
sh shutdown.sh
```

切换到bin目录，执行命令：

```c
sh startup.sh -m standalone
```

后台运行

```c
nohup sh startup.sh -m standalone &
```



