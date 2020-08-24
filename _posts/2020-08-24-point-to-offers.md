---
title: 剑指offer题解
author: Kolin Huang
date: 2020-08-24 15:00:00 +0800
categories: [Blogging, 剑指offer]
tags: [算法题解]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/ptoffer_cover.jpg
---



 

## 剑指 Offer 56 - I. 数组中数字出现的次数

一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

 

示例 1：

```java
输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
```

示例 2：

```java
输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]
```

限制：

```java
2 <= nums.length <= 10000
```



### 答案

```java
class Solution {
    public int[] singleNumbers(int[] nums) {
        int x = 0;
        //计算所有数字的异或值，出现两次的数字会在异或过程中被消除
        //最终x是只出现1次的两个数的异或值。
        for(int num : nums){
            x ^= num;
        }
        //把这两个分开，按照异或的性质，计算时相同的位置0，不同的位置1。
        //我们只要找到x中任意一位为1的位置，就能把这两个数分开
        //找到最低的一位1
        int i = 1;
        while((x & i) == 0)
            i <<= 1;
        int a = 0;
        int b = 0;
        for(int num : nums){
            if((num & i) == 0)
                a ^= num;
            else
                b ^= num;
        }
        return new int[]{a,b};
    }
}
```



[扩展](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/solution/zhi-chu-xian-yi-ci-de-shu-xi-lie-wei-yun-suan-by-a/)

