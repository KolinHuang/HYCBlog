---
title: Manacher算法：线性时间内找到最大回文子串
author: Kolin Huang
date: 2020-08-29 13:49:00 +0800
categories: [Blogging, leetcode]
tags: [算法题解]
comments: true
math: true
---



## 5.最长回文子串

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

```java
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

示例 2：

```java
输入: "cbbd"
输出: "bb"
```



求解这个问题的方法有很多：动态规划、中心扩散、KMP等。

今天主要分析**Manacher算法。**

Manacher算法本质上还是中心扩散法，但是它使用了类似于KMP算法的技巧，可以减少重复判断。

### 第一步：添加分隔符

对原始字符串进行预处理，在首尾以及每个字符之间插入分隔符（这个分隔符不能在原字符串中出现过）。插入了分隔符之后，新字符串有如下性质：

```markdown
1. 新字符串中的任意一个回文子串在原始字符串中的一定能找到唯一的一个回文子串与之对应，因此对新字符串的回文子串的研究就能得到原始字符串的回文子串；
2. 新字符串的回文子串的长度一定是奇数；
3. 原字符串中的字符在新字符串的索引为：oldIndex * 2 + 1 = newIndex，同理，新字符串中的字符在旧字符串中的索引为：newIndex / 2(向下取整) = oldIndex。
```

```java
private String addBoundaries(String s, char divide) {
        int len = s.length();
        if (len == 0) {
            return "";
        }
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < len; i++) {
            stringBuilder.append(divide);
            stringBuilder.append(s.charAt(i));
        }
        stringBuilder.append(divide);
        return stringBuilder.toString();
    }
```



</br>

### 第二步：计算辅助数组p

辅助数组 `p` 记录了新字符串中以每个字符为中心的回文子串的长度。

使用中心扩散法：记录以当前字符为中心，向左右两边同时扩散，记录能够扩散的最大步数。

以`char='#'&&index=4`为例，以`'#'`为中心可以向两边扩散4个位置，所以`p[4]=4`

| char  | #    | a    | #    | b    | #     | b    | #    | a    | #    | b    | #    | b    | #    |
| :---: | ---- | ---- | ---- | ---- | ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| index | 0    | 1    | 2    | 3    | 4     | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   |
|   p   | 0    | 1    | 0    | 1    | **4** |      |      |      |      |      |      |      |      |

以此类推，得到所有以字符为中心的最大扩散步数：

| char  | #    | a    | #    | b    | #    | b    | #    | a    | #    | b    | #    | b    | #    |
| :---: | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| index | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   |
|   p   | 0    | 1    | 0    | 1    | 4    | 1    | 0    | 5    | 0    | 1    | 2    | 1    | 0    |

发现索引为7时，能扩散的步数最大，因此计算其在旧字符串中的索引，就得到了最大的回文子串。

值得注意的是：`p[7] = 5`，5就等于旧字符串中最大回文子串的长度。

借用[大佬](https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zhong-xin-kuo-san-dong-tai-gui-hua-by-liweiwei1419/)的图解，清晰明了：

![leetcode_最大回文子串_manacher_1](/HYCBlog/assets/img/leetcode/leetcode_最大回文子串_manacher_1.png)

![leetcode_最大回文子串_manacher_2](/HYCBlog/assets/img/leetcode/leetcode_最大回文子串_manacher_2.png)

```java
public class Solution {

    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }
        String str = addBoundaries(s, '#');
        int sLen = 2 * len + 1;
        int maxLen = 1;

        int start = 0;
        for (int i = 0; i < sLen; i++) {
            int curLen = centerSpread(str, i);
            if (curLen > maxLen) {
              	//记录当前最大回文子串的长度和在原字符串中的起始位置
                maxLen = curLen;
              	// start = i / 2(此回文串的中心字符在原始字符串的索引) - maxLen / 2（回文串长度的一半）;
                start = (i - maxLen) / 2;
            }
        }
        return s.substring(start, start + maxLen);
    }
		//中心扩散法，返回最大扩散步长
    private int centerSpread(String s, int center) {
        int len = s.length();
        int i = center - 1;
        int j = center + 1;
        int step = 0;
        while (i >= 0 && j < len && s.charAt(i) == s.charAt(j)) {
            i--;
            j++;
            step++;
        }
        return step;
    }
}
```

到此已经可以求解出最大的回文子串了，但是有一个问题：多次执行中心扩散，会有许多字符被重复访问，那么有什么方法能够使得这些字符最多只被访问一次呢？

计算机科学家 Manacher 就改进了这种算法，使得在填写新的辅助数组 p 的值的时候，能够参考已经填写过的辅助数组 p 的值，使得新字符串每个字符只访问了一次。

### Manacher的改进

在遍历过程中，额外记录两个变量`maxRight`和`center`，含义如下：

```markdown
maxRight：记录当前向右扩展的最远边界，即从开始到现在使用“中心扩散法”能得到的回文子串，它能延伸到的最右端的位置。
center：center 是与 maxRight 相关的一个变量，它是上述 maxRight 的回文中心的索引值。
```

`center`的形式化定义：
$$
center = argmax\{x+p[x] | 0 <=x < i\}
$$
`x`为回文中心，`p[x]`为最大扩散步长，所以`p+p[x]`就是以`x`为中心时探索的最右端点，这个函数的最大值就等于`maxRight`，对应的`x`就是`center`。

```markdown
maxRight 与 center 是一一对应的关系，即一个 center 的值唯一对应了一个 maxRight 的值；因此 maxRight 与 center 必须要同时更新。
```

下面分情况讨论循环变量`i`和`maxRight`的关系：

1. 当` i >= maxRight `的时候，这是在一开始或者刚刚把一个回文子串扫描完的情况，此时只能够根据“中心扩散法”一个一个扫描，逐渐扩大 `maxRight`；

2. 当` i < maxRight `的时候，根据新字符的回文子串的性质，循环变量关于 `center` 对称的那个索引（记为 `mirror`）的 `p` 值就很重要。（`mirror = 2 * center - i`）下面根据`p[mirror]`的值再分3种情况：

   1. `p[mirror]` 的数值比较小，不超过 `maxRight - i`。再借图：

      ![leetcode_最大回文子串_manacher_3](/HYCBlog/assets/img/leetcode/leetcode_最大回文子串_manacher_3.png)

      只要以`i`为中心的回文子串和以`i`的镜像为中心的回文子串都落在区间[2*center-maxRight ,maxRight]内，那么这两个回文子串就一定是相同的。这是因为以center为中心的回文串是对称的。所以结论如下

      ```java
      if(p[mirror] < maxRight-i)	p[i] = p[mirror];
      ```

   2. `p[mirror]` 的数值恰好等于 `maxRight - i`。继续借图：

      ![leetcode_最大回文子串_manacher_4](/HYCBlog/assets/img/leetcode/leetcode_最大回文子串_manacher_4.png)

      从上图可以看出，以`mirror`为中心的回文子串扩散步长最大为`maxRight-i`，已经不能扩散了，而以`i`为中心的回文子串还能继续扩散，扩散步长超过了`maxRight-i`。所以可以先把 `p[mirror]` 的值抄过来，然后继续对中心`i`执行“中心扩散法”，增加 `maxRight`。结论如下：

      ```java
      if(p[mirror] < maxRight-i)	p[i] = p[mirror] then do centerSprase(i);
      ```

      

   3. `p[mirror]` 的数值大于 `maxRight - i`。

      ![leetcode_最大回文子串_manacher_5](/HYCBlog/assets/img/leetcode/leetcode_最大回文子串_manacher_5.png)

      结论：

      ```java
      if(p[mirror] > maxRight-i)	p[i] = maxRight-i;
      ```

      证明过程如下：

      ![leetcode_最大回文子串_manacher_6](/HYCBlog/assets/img/leetcode/leetcode_最大回文子串_manacher_6.png)

      * 由于“以 `center` 为中心的回文子串”的对称性， 黄色箭头对应的字符 `c` 和 `e` 一定不相等；
      * 由于“以 `mirror` 为中心的回文子串”的对称性， 绿色箭头对应的字符 `c` 和 `c` 一定相等；
      * 又由于“以 `center` 为中心的回文子串”的对称性， 蓝色箭头对应的字符 `c` 和 `c` 一定相等；
      * 推出“以 `i` 为中心的回文子串”的对称性， 红色箭头对应的字符 `c` 和 `e` 一定不相等。

      所以，以`i`为中心的回文子串最大右边界不会达到红色箭头所指的`e`。

      

      综合上面3种情况，有：

      ```java
      p[i] = min(p[mirror], maxRight - i);
      ```

   

   代码实现：

   ```java
   public class Solution {
   
       public String longestPalindrome(String s) {
           // 特判
           int len = s.length();
           if (len < 2) {
               return s;
           }
   
           // 得到预处理字符串
           String str = addBoundaries(s, '#');
           // 新字符串的长度
           int sLen = 2 * len + 1;
   
           // 数组 p 记录了扫描过的回文子串的信息
           int[] p = new int[sLen];
   
           // 双指针，它们是一一对应的，须同时更新
           int maxRight = 0;
           int center = 0;
   
           // 当前遍历的中心最大扩散步数，其值等于原始字符串的最长回文子串的长度
           int maxLen = 1;
           // 原始字符串的最长回文子串的起始位置，与 maxLen 必须同时更新        
           int start = 0;
   
           for (int i = 0; i < sLen; i++) {
               if (i < maxRight) {
                   int mirror = 2 * center - i;
                   // 这一行代码是 Manacher 算法的关键所在，要结合图形来理解
                   p[i] = Math.min(maxRight - i, p[mirror]);
               }
   
               // 下一次尝试扩散的左右起点，能扩散的步数直接加到 p[i] 中
             	//就是为了处理p[mirror] == marRight - i的情况
               int left = i - (1 + p[i]);
               int right = i + (1 + p[i]);
   
               // left >= 0 && right < sLen 保证不越界
               // str.charAt(left) == str.charAt(right) 表示可以扩散 1 次
               while (left >= 0 && right < sLen && str.charAt(left) == str.charAt(right)) {
                   p[i]++;
                   left--;
                   right++;
               }
               // 根据 maxRight 的定义，它是遍历过的 i 的 i + p[i] 的最大者
               // 如果 maxRight 的值越大，进入上面 i < maxRight 的判断的可能性就越大，这样就可以重复利用之前判断过的回文信息了
             	//更新maxRight，如果当前遍历到的中心i所能达到的最右端大于maxRight，就更新它
               if (i + p[i] > maxRight) {
                   // maxRight 和 center 需要同时更新
                   maxRight = i + p[i];
                   center = i;
               }
               if (p[i] > maxLen) {
                   // 记录最长回文子串的长度和相应它在原始字符串中的起点
                   maxLen = p[i];
                   start = (i - maxLen) / 2;
               }
           }
           return s.substring(start, start + maxLen);
       }
   
   
       /**
        * 创建预处理字符串
        *
        * @param s      原始字符串
        * @param divide 分隔字符
        * @return 使用分隔字符处理以后得到的字符串
        */
       private String addBoundaries(String s, char divide) {
           int len = s.length();
           if (len == 0) {
               return "";
           }
           if (s.indexOf(divide) != -1) {
               throw new IllegalArgumentException("参数错误，您传递的分割字符，在输入字符串中存在！");
           }
           StringBuilder stringBuilder = new StringBuilder();
           for (int i = 0; i < len; i++) {
               stringBuilder.append(divide);
               stringBuilder.append(s.charAt(i));
           }
           stringBuilder.append(divide);
           return stringBuilder.toString();
       }
   }
   ```

   

   

   ### 扩展

   如果我只需要从特定位置出发的最大回文子串呢？例如我只需要找到字符串中以0为起始的最大回文子串，相关题目：[214. 最短回文串](https://leetcode-cn.com/problems/shortest-palindrome/)

   只需要在记录最大子串位置的时候判断一下是否`p[i] - maxLen == 0`，不等于0则不记录。

```java
public class Solution {

    public String longestPalindrome(String s) {
        // 特判
        int len = s.length();
        if (len < 2) {
            return s;
        }

        // 得到预处理字符串
        String str = addBoundaries(s, '#');
        // 新字符串的长度
        int sLen = 2 * len + 1;

        // 数组 p 记录了扫描过的回文子串的信息
        int[] p = new int[sLen];

        // 双指针，它们是一一对应的，须同时更新
        int maxRight = 0;
        int center = 0;

        // 当前遍历的中心最大扩散步数，其值等于原始字符串的最长回文子串的长度
        int maxLen = 1;
        // 原始字符串的最长回文子串的起始位置，与 maxLen 必须同时更新        
        int start = 0;

        for (int i = 0; i < sLen; i++) {
            if (i < maxRight) {
                int mirror = 2 * center - i;
                // 这一行代码是 Manacher 算法的关键所在，要结合图形来理解
                p[i] = Math.min(maxRight - i, p[mirror]);
            }

            // 下一次尝试扩散的左右起点，能扩散的步数直接加到 p[i] 中
          	//就是为了处理p[mirror] == marRight - i的情况
            int left = i - (1 + p[i]);
            int right = i + (1 + p[i]);

            // left >= 0 && right < sLen 保证不越界
            // str.charAt(left) == str.charAt(right) 表示可以扩散 1 次
            while (left >= 0 && right < sLen && str.charAt(left) == str.charAt(right)) {
                p[i]++;
                left--;
                right++;
            }
            // 根据 maxRight 的定义，它是遍历过的 i 的 i + p[i] 的最大者
            // 如果 maxRight 的值越大，进入上面 i < maxRight 的判断的可能性就越大，这样就可以重复利用之前判断过的回文信息了
          	//更新maxRight，如果当前遍历到的中心i所能达到的最右端大于maxRight，就更新它
            if (i + p[i] > maxRight) {
                // maxRight 和 center 需要同时更新
                maxRight = i + p[i];
                center = i;
            }
            
            if (p[i] > maxLen) {
                // 记录最长回文子串的长度和相应它在原始字符串中的起点
              	//记录时，判断一下是否是以0为起始，如果不是，就不记录它
								if((p[i] - maxLen) == 0){
	                  maxLen = p[i];
                		start = (i - maxLen) / 2;
                }
                
            }
        }
        return s.substring(start, start + maxLen);
    }


    /**
     * 创建预处理字符串
     *
     * @param s      原始字符串
     * @param divide 分隔字符
     * @return 使用分隔字符处理以后得到的字符串
     */
    private String addBoundaries(String s, char divide) {
        int len = s.length();
        if (len == 0) {
            return "";
        }
        if (s.indexOf(divide) != -1) {
            throw new IllegalArgumentException("参数错误，您传递的分割字符，在输入字符串中存在！");
        }
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < len; i++) {
            stringBuilder.append(divide);
            stringBuilder.append(s.charAt(i));
        }
        stringBuilder.append(divide);
        return stringBuilder.toString();
    }
}
```

