+++
author = "MOLO"
title = "C/C++二分查找算法"
date = "2025-04-16"
description = "c/c++二分查找"
tags = [
    "算法",
    "笔记"
]
categories = [
    "算法学习"
]
series = ["C/C++ 二分查找算法"]
aliases = ["c/c++二分查找"]
image = "erfen.png"
+++

c/c++二分查找算法有关笔记。 

<!--more-->

# 二分查找

## 二分法

### 定义

二分查找（binary search），用于在一个有序数组中高效地查找某一个元素。相较于顺序查找的$O(n)$时间复杂度，二分查找能将时间复杂度降低至$O(\log n)$，极大提升了查找效率。

### 过程（升序为例）

**初始区间设定**：设定初始查找区间为数组的整个范围，即左边界`left`为数组起始位置（索引为 0），右边界`right`为数组末尾位置（索引为数组长度减 1）。

**中间元素计算**：在每一轮循环中，计算当前区间的中间位置`mid`，通常通过公式`mid = left + (right - left) / 2`计算得到，这样可防止`left + right`时可能出现的溢出情况。

**元素比较与区间更新**：

若中间元素`arr[mid]`刚好等于所查找的值`target`，则直接返回`mid`，查找成功结束。

若`arr[mid]`小于`target`，说明目标值在中间元素右侧，更新左边界`left = mid + 1`，缩小查找区间为`[mid + 1, right]`。

若`arr[mid]`大于`target`，说明目标值在中间元素左侧，更新右边界`right = mid - 1`，缩小查找区间为`[left, mid - 1]`。

**循环终止条件**：当`left`超过`right`时，说明在数组中未找到目标值，查找失败，返回特定标识（如 - 1）。

### 时间复杂度

二分查找每次将查找区间缩小一半，因此时间复杂度为$O(\log n)$。这里的$n$是数组的长度。与顺序查找的$O(n)$时间复杂度相比，随着数组规模$n$的增大，二分查找的效率优势愈发明显。例如，当$n = 1024$时，顺序查找平均需要比较 512 次，而二分查找最多只需比较 10 次（因为$2^{10}=1024$）。

### 实现

#### 实现方式 1：基本 while 循环实现

```cpp
int binarySearch(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1; // 未找到目标值
}
```

#### 实现方式 2：递归实现

```cpp
int binarySearchRecursive(int arr[], int left, int right, int target) {
    if (left > right) {
        return -1; // 未找到目标值
    }
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        return binarySearchRecursive(arr, mid + 1, right, target);
    } else {
        return binarySearchRecursive(arr, left, mid - 1, target);
    }
}
```

#### 实现方式 3：（红蓝染色法）

红蓝染色法是一种更形象化理解二分的方式。假设数组中的元素可分为 “红”（不满足条件）和 “蓝”（满足条件）两类，且红元素都在蓝元素左侧（或右侧）。通过二分不断调整边界，找到红蓝元素的分界点。以查找满足某条件的最小元素为例：

```cpp
int binarySearchByColor(int arr[], int n) {
    int left = 0, right = n - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (isBlue(arr[mid])) {  // isBlue函数判断元素是否满足条件（蓝色）
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;  // 返回满足条件的最小索引
}
```

## 最大值最小化

### 广义有序的理解

这里的有序是广义的有序。若一个数组中的左侧或者右侧都满足某一种条件，而另一侧都不满足这种条件，也可看作是一种有序。比如把满足条件看做 1，不满足看做 0，至少对于这个条件的这一维度是有序的。例如，在一个任务分配场景中，数组元素表示不同任务的耗时，我们希望将这些任务分配给若干个机器，使得完成所有任务的最大耗时最小。对于每个可能的最大耗时值`x`，可以判断能否按照此最大耗时将任务合理分配到机器上，若能分配成功可视为满足条件（标记为 1），否则视为不满足（标记为 0）。在这个条件维度下，存在这样一种广义有序性：若存在一个值`x1`能满足任务分配条件，那么大于`x1`的值`x2`必然也能满足任务分配条件。

### 需要满足的条件

**答案在一个固定的区间内**：例如在上述任务分配例子中，最大耗时的取值范围在任务的最大耗时（所有任务由一台机器完成的情况）和所有任务耗时总和（每个任务由一台单独机器完成的情况）之间。

**判断某个值是否符合条件相对容易**：对于给定的一个可能的最大耗时值，编写一个函数来判断能否在该最大耗时限制下完成任务分配，其实现难度相对不大。

**可行解对于区间满足一定的单调性**：即如果`x`是符合条件的，那么有`x + 1`或者`x - 1`也符合条件（取决于具体问题是求最大值最小化还是最小值最大化）。在最大值最小化问题中，如果`x`能满足任务分配条件，那么更大的`x + 1`必然也能满足，这就保证了在这个区间内使用二分查找的可行性。

## STL 的二分查找

### 调用前元素必须有序

在使用 C++ STL 中的二分查找函数前，必须确保数组或容器中的元素是有序的。否则，结果将是未定义的。

### 头文件

相关二分查找函数定义在`<algorithm>`头文件中，使用时需包含此头文件。

### std::lower_bound

`std::lower_bound(begin, end, value)`函数用于在有序范围`[begin, end)`内查找第一个大于或等于`value`的元素的位置。返回一个迭代器指向该位置。例如：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> v = {1, 3, 5, 7, 9};
    auto it = std::lower_bound(v.begin(), v.end(), 4);
    if (it != v.end()) {
        std::cout << "第一个大于或等于4的元素是: " << *it << std::endl;
    }
    return 0;
}
```

上述代码中，`std::lower_bound`返回指向元素`5`的迭代器。

### std::upper_bound

`std::upper_bound(begin, end, value)`函数用于在有序范围`[begin, end)`内查找第一个大于`value`的元素的位置。同样返回一个迭代器指向该位置。例如：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> v = {1, 3, 5, 7, 9};
    auto it = std::upper_bound(v.begin(), v.end(), 4);
    if (it != v.end()) {
        std::cout << "第一个大于4的元素是: " << *it << std::endl;
    }
    return 0;
}
```

上述代码中，`std::upper_bound`也返回指向元素`5`的迭代器，因为`5`是第一个大于`4`的元素。