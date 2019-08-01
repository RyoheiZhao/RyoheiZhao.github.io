---
title: 数据结构和算法
categories:
    - 笔记总结
tags:
    - 算法
    - 数据结构
    - 程序员内功
---

## 算法

### 冒泡排序

冒泡排序原理:

每一趟只能将一个数归位, 如果有n个数进行排序,只需将n-1个数归位, 也就是说要进行n-1趟操作(已经归位的数不用再比较)
N个数字要排序完成，总共进行N-1趟排序，每i趟的排序次数为(N-i)次
冒泡排序缺点，效率低.

``` python
nums = [1,13,9,5]
n = len(nums)
for i in range(0,n):
    for j in range(0,n-i-1):
        if nums[j] > nums[j+1]:
            nums[j],nums[j+1] = nums[j+1],nums[j]
print(nums)
```