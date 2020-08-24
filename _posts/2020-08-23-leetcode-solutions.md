---
title: Leetcode题解
author: Kolin Huang
date: 2020-08-23 20:44:00 +0800
categories: [Blogging, leetcode]
tags: [算法题解]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/leetcode_cover.jpg
---



 

## 201.数字范围按位与

给定范围 [m, n]，其中 0 <= m <= n <= 2147483647，返回此范围内所有数字的按位与（包含 m, n 两端点）。

示例 1: 

```java
输入: [5,7]
输出: 4
```

示例 2:

```java
输入: [0,1]
输出: 0
```



### 答案

通过分析，若m和n转为二进制字符串后，长度不一致，则在这个范围内按位与后，结果必为0。例如：

```markdown
m = 11110011
n = 101101111
在范围[m,n]中，肯定会出现100000000的情况，所以有：
100000000 & 101101111 = 100000000,且100000000 & 11111111 = 000000000
所以结果为0
```

长度一致的情况下，结果就是m和n的最长公共前缀。

```markdown
m = 101101101
n = 101101111
最长前缀为1011011，最后两位需要归0，因为在范围[m,n]中，会出现数字101101100使得最终结果的两位归0。
```



```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        if(m == n)  return m;

        String s1 = Integer.toBinaryString(m);
        String s2 = Integer.toBinaryString(n);
        if(s1.length() != s2.length())
            return 0;

        int index = 0;

        //找到两个字符串第一个不同的位置
        for(int i = 0; i < s1.length(); ++i){
            if(s1.charAt(i) != s2.charAt(i)){
                index = i;
                break;
            }
        }
        //取出其公共前缀，剩余位都替换为0
        String sub1 = s1.substring(0,index);
        String sub2 = s1.substring(index,s1.length());
        sub2 = sub2.replaceAll("1","0");
        String res = sub1 + sub2;
        return Integer.parseUnsignedInt(res,2);
    }
}
```

## 459.重复的子字符串

给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。

示例 1:

```java
输入: "abab"
输出: True
解释: 可由子字符串 "ab" 重复两次构成。
```


示例 2:

```java
输入: "aba"
输出: False
```


示例 3:

```java
输入: "abcabcabcabc"
输出: True
解释: 可由子字符串 "abc" 重复四次构成。 (或者子字符串 "abcabc" 重复两次构成。)
```

### 答案

#### 枚举法

若存在这种形式的字符串，那么必有：

```java
sub.length() * k = s.length()	(k!=1)
```

循环添加sub k-1次，最终结果若与s相等，则返回true。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        for(int i = 1; i <= s.length()/2; ++i){
            if(s.length() % i != 0)   continue;
            int p = s.length() / i;
            String res = s.substring(0,i);
            String subS = s.substring(0,i);
            for(int j = 2; j <= p; ++j){
                res+=subS;
                if(i * j <= s.length() && !res.equals(s.substring(0,i*j)))
                    break;
            }
            if(res.equals(s))
                return true;
        }
        return false;
    }
}
```



#### 字符串匹配

![leetcode_repeated_substring_pattern](/HYCBlog/assets/img/leetcode/leetcode_repeated_substring_pattern.png)

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).indexOf(s, 1) != s.length();
    }
}
作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/repeated-substring-pattern/solution/zhong-fu-de-zi-zi-fu-chuan-by-leetcode-solution/
```



#### KMP算法

![leetcode_重复的子字符串_字符串匹配_KMP_1](/HYCBlog/assets/img/leetcode/leetcode_repeated_substring_pattern_KMP_1.png)

![leetcode_重复的子字符串_字符串匹配_KMP_2](/HYCBlog/assets/img/leetcode/leetcode_repeated_substring_pattern_KMP_2.png)

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return kmp(s + s, s);
    }

    public boolean kmp(String query, String pattern) {
        int n = query.length();
        int m = pattern.length();
        int[] fail = new int[m];
        Arrays.fill(fail, -1);
        for (int i = 1; i < m; ++i) {
            int j = fail[i - 1];
            while (j != -1 && pattern.charAt(j + 1) != pattern.charAt(i)) {
                j = fail[j];
            }
            if (pattern.charAt(j + 1) == pattern.charAt(i)) {
                fail[i] = j + 1;
            }
        }
        int match = -1;
        for (int i = 1; i < n - 1; ++i) {
            while (match != -1 && pattern.charAt(match + 1) != query.charAt(i)) {
                match = fail[match];
            }
            if (pattern.charAt(match + 1) == query.charAt(i)) {
                ++match;
                if (match == m - 1) {
                    return true;
                }
            }
        }
        return false;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/repeated-substring-pattern/solution/zhong-fu-de-zi-zi-fu-chuan-by-leetcode-solution/
```

