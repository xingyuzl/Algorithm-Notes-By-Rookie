# 菜鸟的算法笔记：寻找两个正序数组的中位数



## 1. 问题

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

![image-20210610003528326](typora-user-images/image-20210610003528326.png)

停下来思考下，你能想到什么解法？

## 2. 解决方法

```
中位数被用来将一个集合划分为两个长度相等的子集，其中一个子集中的元素总是大于另一个子集中的元素。
对一个长度为length的集合。
    如果length为偶数，中位数就是集合的第length/2 - 1 和 length/2个元素 
	如果length为奇数，中位数就是集合的length//2个元素
		length//2表示除2以后向下取整，比如3/2 = 1.5，向下取整就是1，所以3//2=1	
```

### 2.1. 暴力排序法

就是将两个数组合并，然后根据合并后数组的长度是奇数还是偶数，再返回中位数就可以了。

翻译成代码如下。

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
		num = nums1 + nums2
        num = sorted(num)
        length = len(num)
        return num[length//2] if length % 2 == 1 else (num[length // 2] + num[length // 2 - 1]) / 2
```

#### 2.1.1. 复杂度分析

##### 2.1.1.1. 时间复杂度

上述程序最耗时的是第4行。

排序算法根据时间复杂度主要分为三类：

-   $O(n^2)$：比如冒泡排序、插入排序、选择排序等
-   $O(n \log n)$：比如快速排序、归并排序、堆排序等
-   $O(n)$比：如桶排序、计数排序、基数排序等

最快的排序算法( $O(n)$算法 ）只适合某些特殊场景，并不普遍适用。而工业级排序算法根据需要排序的数据特征，一般采用插入排序、快速排序和堆排序。

一般可以认为常用语言中的排序算法时间复杂度为$O(n \log n)$。其中 $n$ 为需要排序的数组长度。

所以本程序时间复杂度为$O(n \log n)$，其中 $n$ 是 $num$ 数组的长度，即 $nums1$ 的长度和 $nums2$ 的长度之和。

##### 2.1.1.2. 空间复杂度

程序中最费内存的是第3行代码，它需要开辟新空间来存储 $num$ 。

所以本程序空间复杂度为$O(n)$，其中 $n$ 是 $num$ 数组的长度，即 $nums1$ 的长度和 $nums2$ 的长度之和。

#### 2.1.2. 执行

![image-20210610005040096](typora-user-images/image-20210610005040096.png)

### 2.2. 基于归并的遍历算法

名字是我胡乱编的。该算法借助了归并排序的合并数组思想。

其实很简单，每次取两个数组里的更小的元素弹出，一直弹出第 $length/2 - 1$ 和 $length/2$ 个元素（两个数组长度之和偶数）或第 $length//2$ 个元素（两个数组长度之和为奇数）。

图解如下：

![image-20210610013047032](typora-user-images/image-20210610013047032.png)

中位数索引为2，因为索引从0开始，所以要弹出第3个元素。

翻译成代码如下。

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
		m, n = len(nums1), len(nums2)
        length = m + n
        # 需要弹出的索引
        k = set([length // 2]) if length % 2 == 1 else set([length // 2 - 1, length // 2])
        # 总共弹出的次数，从0开始
        pop_index = -1
        sum = 0
        while k:
            if not nums1: # 如果nums1数组为空，弹出nums2的第一个元素
                pop_value = nums2.pop(0)
            elif not nums2: # 如果nums2数组为空，弹出nums1的第一个元素
                pop_value = nums1.pop(0)
            elif nums1[0] < nums2[0]: # 弹出较小的元素
                pop_value = nums1.pop(0)
            else: # 弹出较小的元素
                pop_value = nums2.pop(0)
            pop_index += 1
            # 弹出的索引是中位数的索引
            if pop_index in k:
                sum += pop_value
                k.remove(pop_index)
        return sum if length % 2 == 1 else sum / 2
```

#### 2.2.1. 复杂度分析

##### 2.2.1.1. 时间复杂度

本程序最多只需要遍历 $length/2$ 次，去掉常数项，时间复杂度为 $O(m + n)$。

需要特别说明的是，上述程序只是为了说明方法的原理而简写的程序。因为 $nums1.pop(0)$ 操作的时间复杂度为 $O(m)$， $nums2.pop(0)$ 操作的时间复杂度为 $O(n)$，实际编程时。可以设置两个指针 $left$ 和 $right$， $nums1.pop(0)$ 时 $left = left + 1$， $nums2.pop(0)$ 时 $right = right + 1$。$nums1[0]$替换为 $nums1[left]$， $nums2[0]$替换为 $nums2[right]$ 即可。

##### 2.2.1.2. 空间复杂度

本程序没有额外与 $m$ 或 $n$ 相关的申请空间，所以空间复杂度为 $O(1)$。

#### 2.2.2. 执行

![image-20210610021016712](typora-user-images/image-20210610021016712.png)

### 2.3.  二分查找法

时间复杂度为 $O(\log(m+n))$，空间复杂度为 $O(1)$

**待补充**

### 2.4. 划分数组法

时间复杂度为 $O(\log(min(m,n)))$，空间复杂度为 $O(1)$

**待补充**
