+++
author = "MOLO"
title = "C/C++排序算法"
date = "2025-04-19"
description = "c/c++排序算法"
tags = [
    "算法",
    "笔记"
]
categories = [
    "算法学习"
]
series = ["C/C++ 排序算法"]
aliases = ["c/c++排序算法"]
image = "px.png"
+++

c/c++排序算法有关笔记。 

<!--more-->

# 排序算法介绍

排序算法（sorting algorithm）用于对一组数据按照特定顺序进行排列。排序算法有着广泛的应用，因为有序数据通常能够被更高效地查找、分析和处理。

排序算法中的数据类型可以是整数、浮点数、字符或字符串等。排序的判断规则可根据需求设定，如数字大小、字符 ASCII 码顺序或自定义规则。

## 评价维度
1. **运行效率**：我们期望排序算法的时间复杂度尽量低，且总体操作数量较少（时间复杂度中的常数项变小）。对于大数据量的情况，运行效率显得尤为重要。
2. **就地性**：顾名思义，原地排序通过在原数组上直接操作实现排序，无须借助额外的辅助数组，从而节省内存。通常情况下，原地排序的数据搬运操作较少，运行速度也更快。
3. **稳定性**：稳定排序在完成排序后，相等元素在数组中的相对顺序不发生改变。
4. **自适应性**：自适应排序能够利用输入数据已有的顺序信息来减少计算量，达到更优的时间效率。自适应排序算法的最佳时间复杂度通常优于平均时间复杂度。
5. **是否基于比较**

## 具体排序算法
### 选择排序
开启一个循环，每轮从未排序区间选择最小/最大的元素，将其放到已排序区间的末尾。

#### 流程
1. 初始状态下，所有元素未排序，即未排序（索引）区间为 [0, n - 1] 。
2. 选取区间 [0, n - 1] 中的最小元素，将其与索引 0 处的元素交换。完成后，数组前 1 个元素已排序。
3. 选取区间 [1, n  - 1] 中的最小元素，将其与索引 1 处的元素交换。完成后，数组前 2 个元素已排序。
4. 以此类推。经过 n - 1 轮选择与交换后，数组前 n - 1 个元素已排序。
5. 仅剩的一个元素必定是最大元素，无须排序，因此数组排序完成。

#### 代码实现
```cpp
/* 选择排序 */
void selectionSort(vector<int> &nums) {
    int n = nums.size();
    // 外循环：未排序区间为 [i, n-1]
    for (int i = 0; i < n - 1; i++) {
        // 内循环：找到未排序区间内的最小元素
        int k = i;
        for (int j = i + 1; j < n; j++) {
            if (nums[j] < nums[k])
                k = j; // 记录最小元素的索引
        }
        // 将该最小元素与未排序区间的首个元素交换
        swap(nums[i], nums[k]);
    }
}
```

#### 特性
- **时间复杂度**：$O(n^2)$
- **空间复杂度**：$O(1)$
- **稳定性**：非稳定排序

### 冒泡排序
通过连续地比较与交换相邻元素实现排序。这个过程就像气泡从底部升到顶部一样，因此得名冒泡排序。

#### 流程
设数组的长度为 n
1. 首先，对 n 个元素执行“冒泡”，将数组的最大元素交换至正确位置。
2. 接下来，对剩余 n - 1 个元素执行“冒泡”，将第二大元素交换至正确位置。
3. 以此类推，经过 n - 1 轮“冒泡”后，前 n - 1 大的元素都被交换至正确位置。
4. 仅剩的一个元素必定是最小元素，无须排序，因此数组排序完成。

#### 代码实现（标志优化）
```cpp
/* 冒泡排序（标志优化）*/
void bubbleSortWithFlag(vector<int> &nums) {
    // 外循环：未排序区间为 [0, i]
    for (int i = nums.size() - 1; i > 0; i--) {
        bool flag = false; // 初始化标志位
        // 内循环：将未排序区间 [0, i] 中的最大元素交换至该区间的最右端
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                // 交换 nums[j] 与 nums[j + 1]
                // 这里使用了 std::swap() 函数
                swap(nums[j], nums[j + 1]);
                flag = true; // 记录交换元素
            }
        }
        if (!flag)
            break; // 此轮“冒泡”未交换任何元素，直接跳出
    }
}
```

#### 特性
- **时间复杂度**：$O(n^2)$
- **空间复杂度**：$O(1)$
- **稳定性**：稳定排序

### 插入排序
它的工作原理与手动整理一副牌的过程非常相似。

#### 流程
1. 初始状态下，数组的第 1 个元素已完成排序。
2. 选取数组的第 2 个元素作为 base ，将其插入到正确位置后，数组的前 2 个元素已排序。
3. 选取第 3 个元素作为 base ，将其插入到正确位置后，数组的前 3 个元素已排序。
4. 以此类推，在最后一轮中，选取最后一个元素作为 base ，将其插入到正确位置后，所有元素均已排序。

#### 代码实现
```cpp
/* 插入排序 */
void insertionSort(vector<int> &nums) {
    // 外循环：已排序区间为 [0, i-1]
    for (int i = 1; i < nums.size(); i++) {
        int base = nums[i], j = i - 1;
        // 内循环：将 base 插入到已排序区间 [0, i-1] 中的正确位置
        while (j >= 0 && nums[j] > base) {
            nums[j + 1] = nums[j]; // 将 nums[j] 向右移动一位
            j--;
        }
        nums[j + 1] = base; // 将 base 赋值到正确位置
    }
}
```

#### 特性
- **时间复杂度**：$O(n^2)$（平均和最坏情况），$O(n)$（最好情况，数组已排序）
- **空间复杂度**：$O(1)$
- **稳定性**：稳定排序

### 快速排序
是一种基于分治策略的排序算法，运行高效，应用广泛。

#### 核心操作（哨兵划分）
选择数组中的某个元素作为“基准数”，将所有小于基准数的元素移到其左侧，而大于基准数的元素移到其右侧。

1. 选取数组最左端元素作为基准数，初始化两个指针 i 和 j 分别指向数组的两端。
2. 设置一个循环，在每轮中使用 i（j）分别寻找第一个比基准数大（小）的元素，然后交换这两个元素。
3. 循环执行步骤 2. ，直到 i 和 j 相遇时停止，最后将基准数交换至两个子数组的分界线。

#### 流程
1. 首先，对原数组执行一次“哨兵划分”，得到未排序的左子数组和右子数组。
2. 然后，对左子数组和右子数组分别递归执行“哨兵划分”。
3. 持续递归，直至子数组长度为 1 时终止，从而完成整个数组的排序。

#### 代码实现
```cpp
/* 哨兵划分 */
int partition(vector<int> &nums, int left, int right) {
    // 以 nums[left] 为基准数
    int i = left, j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left])
            j--;                // 从右向左找首个小于基准数的元素
        while (i < j && nums[i] <= nums[left])
            i++;                // 从左向右找首个大于基准数的元素
        swap(nums[i], nums[j]); // 交换这两个元素
    }
    swap(nums[i], nums[left]);  // 将基准数交换至两子数组的分界线
    return i;                   // 返回基准数的索引
}

/* 快速排序 */
void quickSort(vector<int> &nums, int left, int right) {
    // 子数组长度为 1 时终止递归
    if (left >= right)
        return;
    // 哨兵划分
    int pivot = partition(nums, left, right);
    // 递归左子数组、右子数组
    quickSort(nums, left, pivot - 1);
    quickSort(nums, pivot + 1, right);
}
```

#### 特性
- **时间复杂度**：$O(nlogn)$（平均情况），$O(n^2)$（最坏情况，如数组完全倒序）
- **空间复杂度**：$O(logn)$（平均情况，递归调用栈空间），$O(n)$（最坏情况）
- **稳定性**：非稳定排序

#### 优化
1. **基准数优化**：快速排序在某些输入下的时间效率可能降低。举一个极端例子，假设输入数组是完全倒序的，由于我们选择最左端元素作为基准数，那么在哨兵划分完成后，基准数被交换至数组最右端，导致左子数组长度为 n - 1、右子数组长度为 0 。如此递归下去，每轮哨兵划分后都有一个子数组的长度为 0 ，分治策略失效，快速排序退化为“冒泡排序”的近似形式。

我们可以在数组中选取三个候选元素（通常为数组的首、尾、中点元素），并将这三个候选元素的中位数作为基准数。这样一来，基准数“既不太小也不太大”的概率将大幅提升。当然，我们还可以选取更多候选元素，以进一步提高算法的稳健性。采用这种方法后，时间复杂度劣化至 O(n^2) 的概率大大降低。

```cpp
/* 选取三个候选元素的中位数 */
int medianThree(vector<int> &nums, int left, int mid, int right) {
    int l = nums[left], m = nums[mid], r = nums[right];
    if ((l <= m && m <= r) || (r <= m && m <= l))
        return mid; // m 在 l 和 r 之间
    if ((m <= l && l <= r) || (r <= l && l <= m))
        return left; // l 在 m 和 r 之间
    return right;
}

/* 哨兵划分（三数取中值） */
int partition(vector<int> &nums, int left, int right) {
    // 选取三个候选元素的中位数
    int med = medianThree(nums, left, (left + right) / 2, right);
    // 将中位数交换至数组最左端
    swap(nums[left], nums[med]);
    // 以 nums[left] 为基准数
    int i = left, j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left])
            j--;                // 从右向左找首个小于基准数的元素
        while (i < j && nums[i] <= nums[left])
            i++;                // 从左向右找首个大于基准数的元素
        swap(nums[i], nums[j]); // 交换这两个元素
    }
    swap(nums[i], nums[left]);  // 将基准数交换至两子数组的分界线
    return i;                   // 返回基准数的索引
}
```

2. **尾递归优化**
```cpp
/* 快速排序（尾递归优化） */
void quickSort(vector<int> &nums, int left, int right) {
    // 子数组长度为 1 时终止
    while (left < right) {
        // 哨兵划分操作
        int pivot = partition(nums, left, right);
        // 对两个子数组中较短的那个执行快速排序
        if (pivot - left < right - pivot) {
            quickSort(nums, left, pivot - 1); // 递归排序左子数组
            left = pivot + 1;                 // 剩余未排序区间为 [pivot + 1, right]
        } else {
            quickSort(nums, pivot + 1, right); // 递归排序右子数组
            right = pivot - 1;                 // 剩余未排序区间为 [left, pivot - 1]
        }
    }
}
```

### 归并排序
是一种基于分治策略的排序算法，包含“划分”和“合并”阶段。

可以参考讲解[视频](https://www.bilibili.com/video/BV1Ma4y1n7E2)理解学习。

#### 流程
1. **划分阶段**：通过递归不断地将数组从中点处分开，将长数组的排序问题转换为短数组的排序问题。
    - 计算数组中点 mid ，递归划分左子数组（区间 [left, mid] ）和右子数组（区间 [mid + 1, right] ）。
    - 递归执行上一步，直至子数组区间长度为 1 时终止。
2. **合并阶段**：当子数组长度为 1 时终止划分，开始合并，持续地将左右两个较短的有序数组合并为一个较长的有序数组，直至结束。

#### 代码实现
```cpp
/* 合并左子数组和右子数组 */
void merge(vector<int> &nums, int left, int mid, int right) {
    // 左子数组区间为 [left, mid], 右子数组区间为 [mid+1, right]
    // 创建一个临时数组 tmp ，用于存放合并后的结果
    vector<int> tmp(right - left + 1);
    // 初始化左子数组和右子数组的起始索引
    int i = left, j = mid + 1, k = 0;
    // 当左右子数组都还有元素时，进行比较并将较小的元素复制到临时数组中
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j])
            tmp[k++] = nums[i++];
        else
            tmp[k++] = nums[j++];
    }
    // 将左子数组和右子数组的剩余元素复制到临时数组中
    while (i <= mid) {
        tmp[k++] = nums[i++];
    }
    while (j <= right) {
        tmp[k++] = nums[j++];
    }
    // 将临时数组 tmp 中的元素复制回原数组 nums 的对应区间
    for (k = 0; k < tmp.size(); k++) {
        nums[left + k] = tmp[k];
    }
}

/* 归并排序 */
void mergeSort(vector<int> &nums, int left, int right) {
    // 终止条件
    if (left >= right)
        return; // 当子数组长度为 1 时终止递归
    // 划分阶段
    int mid = left + (right - left) / 2;    // 计算中点
    mergeSort(nums, left, mid);      // 递归左子数组
    mergeSort(nums, mid + 1, right); // 递归右子数组
    // 合并阶段
    merge(nums, left, mid, right);
}
```

#### 特性
- **时间复杂度**：$O(nlogn)$
- **空间复杂度**：$O(n)$（需要额外的临时数组用于合并）
- **稳定性**：稳定排序

#### 应用
链表排序（因为链表只能线性访问），此外对于stl里的std::list容器有成员函数sort可调用（参见[菜鸟教程c++<list>](https://www.runoob.com/cplusplus/cpp-libs-list.html)），可减少工作量。

### 计数排序
通过统计元素数量来实现排序，通常应用于整数数组。（适用于数量大但范围小的场景）

#### 特性
- **时间复杂度**：$O(n + k)$（n为数组长度，k为数据范围）
- **空间复杂度**：$O(k)$（需要额外空间存储计数数组）
- **稳定性**：稳定排序

### 基数排序
核心思想与计数排序一致，也通过统计个数来实现排序。在此基础上，基数排序利用数字各位之间的递进关系，依次对每一位进行排序，从而得到最终的排序结果。（不断比较并按各个位数上数字的大小排序）

#### 流程
以学号数据为例，假设数字的最低位是第 1 位，最高位是第 8 位。
1. 初始化位数 k = 1 。
2. 对学号的第 k 位执行“计数排序”。完成后，数据会根据第 k 位从小到大排序。
3. 将 k 增加 1 ，然后返回步骤 2. 继续迭代，直到所有位都排序完成后结束。

#### 代码实现
```cpp
/* 获取元素 num 的第 k 位，其中 exp = 10^(k-1) */
int digit(int num, int exp) {
    // 传入 exp 而非 k 可以避免在此重复执行昂贵的次方计算
    return (num / exp) % 10;
}

/* 计数排序（根据 nums 第 k 位排序） */
void countingSortDigit(vector<int> &nums, int exp) {
    // 十进制的位范围为 0~9 ，因此需要长度为 10 的桶数组
    vector<int> counter(10, 0);
    int n = nums.size();
    // 统计 0~9 各数字的出现次数
    for (int i = 0; i < n; i++) {
        int d = digit(nums[i], exp); // 获取 nums[i] 第 k 位，记为 d
        counter[d]++;                // 统计数字 d 的出现次数
    }
    // 求前缀和，将“出现个数”转换为“数组索引”
    for (int i = 1; i < 10; i++) {
        counter[i] += counter[i - 1];
    }
    // 倒序遍历，根据桶内统计结果，将各元素填入 res
    vector<int> res(n, 0);
    for (int i = n - 1; i >= 0; i--) {
        int d = digit(nums[i], exp);
        int j = counter[d] - 1; // 获取 d 在数组中的索引 j
        res[j] = nums[i];       // 将当前元素填入索引 j
        counter[d]--;           // 将 d 的数量减 1
    }
    // 使用结果覆盖原数组 nums
    for (int i = 0; i < n; i++)
        nums[i] = res[i];
}

/* 基数排序 */
void radixSort(vector<int> &nums) {
    // 获取数组的最大元素，用于判断最大位数
    int m = *max_element(nums.begin(), nums.end());
    // 按照从低位到高位的顺序遍历
    for (int exp = 1; exp <= m; exp *= 10)
        // 对数组元素的第 k 位执行计数排序
        // k = 1 -> exp = 1
        // k = 2 -> exp = 10
        // 即 exp = 10^(k-1)
        countingSortDigit(nums, exp);
}
```

**为什么从最低位开始排序？**
在连续的排序轮次中，后一轮排序会覆盖前一轮排序的结果。从最低位开始排序可以保证在处理更高位时，相同高位数字的元素之间的相对顺序是基于低位已经排好序的结果，从而保证最终排序的正确性。

#### 特性
- **时间复杂度**：$O(d(n + b))$，其中 $d$ 是数字的最大位数，$n$ 是数组长度，$b$ 是基数（例如十进制 $b = 10$）
- **空间复杂度**：$O(n + b)$，需要额外的空间用于计数数组和临时存储结果
- **稳定性**：稳定排序

### 桶排序
可以看作计数排序的延伸，是分治策略的一个典型应用。它通过设置一些具有大小顺序的桶，每个桶对应一个数据范围，将数据平均分配到各个桶中；然后，在每个桶内部分别执行排序；最终按照桶的顺序将所有数据合并。

#### 流程
考虑一个长度为 $n$ 的数组，其元素是范围 $[0, 1)$ 内的浮点数。
1. 初始化 $k$ 个桶，将 $n$ 个元素分配到 $k$ 个桶中。
2. 对每个桶分别执行排序（这里采用编程语言的内置排序函数）。
3. 按照桶从小到大的顺序合并结果。

#### 代码实现
```cpp
/* 桶排序 */
void bucketSort(vector<float> &nums) {
    // 初始化 k = n/2 个桶，预期向每个桶分配 2 个元素
    int k = nums.size() / 2;
    vector<vector<float>> buckets(k);
    // 1. 将数组元素分配到各个桶中
    for (float num : nums) {
        // 输入数据范围为 [0, 1)，使用 num * k 映射到索引范围 [0, k-1]
        int i = num * k;
        // 将 num 添加进桶 bucket_idx
        buckets[i].push_back(num);
    }
    // 2. 对各个桶执行排序
    for (vector<float> &bucket : buckets) {
        // 使用内置排序函数，也可以替换成其他排序算法
        sort(bucket.begin(), bucket.end());
    }
    // 3. 遍历桶合并结果
    int i = 0;
    for (vector<float> &bucket : buckets) {
        for (float num : bucket) {
            nums[i++] = num;
        }
    }
}
```

#### 特性
- **时间复杂度**：如果数据均匀分布，时间复杂度为 $O(n + k \times (n/k \log(n/k)))$，化简后为 $O(n + n \log(n/k))$，当 $k$ 接近 $n$ 时，时间复杂度接近 $O(n)$；如果数据分布不均匀，时间复杂度可能退化为 $O(n^2)$
- **空间复杂度**：$O(n + k)$，需要额外的空间存储桶以及桶内数据
- **稳定性**：桶排序是否稳定取决于排序桶内元素的算法是否稳定，如果桶内使用的是稳定排序算法，则桶排序是稳定的

### 堆排序
堆排序是利用堆这种数据结构所设计的一种排序算法。堆是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。

可以参考[视频](https://www.bilibili.com/video/BV1HYtseiEQ8/)理解学习。

#### 流程
1. **建堆**：将初始待排序序列 $R[1..n]$ 构建成大顶堆（或小顶堆），此堆为初始的无序区。
2. **调整堆**：将堆顶元素 $R[1]$ 与最后一个元素 $R[n]$ 交换，此时得到新的无序区 $R[1..n-1]$ 和新的有序区 $R[n]$，且满足 $R[1..n-1].keys \leq R[n].key$。
3. **重复调整**：由于交换后新的堆顶 $R[1]$ 可能违反堆的性质，因此需要对当前无序区 $R[1..n-1]$ 调整为新堆，然后再次将 $R[1]$ 与无序区最后一个元素交换，得到新的无序区 $R[1..n-2]$ 和新的有序区 $R[n-1..n]$。不断重复此过程直到有序区的元素个数为 $n-1$，则整个排序过程完成。

#### 代码实现（以大顶堆为例）
```cpp
// 调整堆，将以 i 为根节点的子树调整为大顶堆
void heapify(vector<int>& nums, int n, int i) {
    int largest = i;  // 初始化最大元素为根节点
    int left = 2 * i + 1;  // 左子节点索引
    int right = 2 * i + 2;  // 右子节点索引

    // 如果左子节点比根节点大
    if (left < n && nums[left] > nums[largest])
        largest = left;

    // 如果右子节点比最大元素还大
    if (right < n && nums[right] > nums[largest])
        largest = right;

    // 如果最大元素不是根节点
    if (largest != i) {
        swap(nums[i], nums[largest]);

        // 递归地调整被交换的子树
        heapify(nums, n, largest);
    }
}

// 堆排序函数
void heapSort(vector<int>& nums) {
    int n = nums.size();

    // 建堆（从最后一个非叶子节点开始调整）
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(nums, n, i);

    // 一个一个地从堆顶取出元素
    for (int i = n - 1; i > 0; i--) {
        // 将堆顶元素移到数组末尾
        swap(nums[0], nums[i]);

        // 调用调整堆函数，将剩余的元素重新调整为大顶堆
        heapify(nums, i, 0);
    }
}
```

#### 特性
- **时间复杂度**：$O(n \log n)$，建堆的时间复杂度为 $O(n)$，调整堆的时间复杂度为 $O(\log n)$，总共需要调整 $n-1$ 次，所以总的时间复杂度为 $O(n \log n)$
- **空间复杂度**：$O(1)$，只需要常数级别的额外空间
- **稳定性**：非稳定排序
