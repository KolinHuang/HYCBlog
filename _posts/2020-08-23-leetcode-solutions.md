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

## 目录

[5.最长回文子串](#jump5)

[16.最接近三数之和](#jump16)

[17.电话号码的字母组合](#jump17)

[43.字符串相乘](#jump43)

[51.N 皇后](#jump51)

[63.含障碍物网格中的不同路径](#jump63)

[67.二进制求和](#jump67)

[93. 恢复IP地址](#jump93)

[96. 不同的二叉搜索树](#jump96)

[100. 相同的树](#jump100)

[108. 将有序数组转换为二叉搜索树](#jump108)

[109.  有序链表转换二叉搜索树](#jump109)

[110.平衡二叉树](#jump110)

[111. 二叉树的最小深度](#jump111)

[112. 路径总和](#jump112)

[120.三角形最小路径和](#jump120)

[130.被围绕的区域](#jump130)

[133.克隆图](#jump133)

[139.单词拆分](#jump139)

[201.数字范围按位与](#jump201)

[209.寻找长度最小的子数组](#jump209)

[214.最短回文串](#jump214)

[215.数组中的第K大元素](#jump215)

[309.最佳买卖股票时机含冷冻期](#jump309)

[332.重新安排行程](#jump332)

[336.回文对](#jump336)

[337.打家劫舍3](#jump337)

[378.有序矩阵中第K小元素](#jump378)

[459.重复的子字符串](#jump459)

[486.预测赢家](#jump486)

[491.递增子序列](#jump491)

[529.扫雷游戏](#jump529)

[546.移除盒子](#jump546)

[557.反转字符串中的单词 III](#jump557)

[647.回文子串](#jump647)

[657.机器人能否返回原点](#jump657)

[679.24点游戏](#jump679)

[696.计数二进制子串](#jump696)

[718.最长公共连续子数组](#jump718)

[733.图像渲染](#jump733)

[841.钥匙和房间](#jump841)





<span id="jump5"></span>

## 5.最长回文子串

[点这里跳转](/posts/algorithm-manacher)

<span id="jump16"></span>


##  16.最接近三数之和

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。



示例：

```markdown
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

提示：

```markdown
3 <= nums.length <= 10^3
-10^3 <= nums[i] <= 10^3
-10^4 <= target <= 10^4
```



### 暴力解法

固定一个数，遍历其他两个数，找出`target - sum`最小的`sum`作为结果。



```java
int min = Integer.MAX_VALUE;
        int x=0,y=0,z=0;
        for(int i = 0; i < nums.length; ++i)
            for(int j = i + 1; j < nums.length; ++j)
                for(int k = j + 1; k < nums.length; ++k){
                    int tmp = (nums[i] + nums[j] + nums[k]);
                    int m = min;
                    min = Math.min(Math.abs(target - tmp), min);
                    if(m != min){
                        x = i;
                        y = j;
                        z = k;
                    }
                }
        return nums[x]+nums[y]+nums[z];
```



### 排序+双指针

我们首先考虑枚举第一个元素 a，对于剩下的两个元素 b 和 c，我们希望它们的和最接近 `target - a`。对于 b 和 c，如果它们在原数组中枚举的范围（既包括下标的范围，也包括元素值的范围）没有任何规律可言，那么我们还是只能使用两重循环来枚举所有的可能情况。因此，我们可以考虑对整个数组进行升序排序，借助双指针，我们就可以对枚举的过程进行优化。

```markdown
如果a+b+c≥target，那么就将 p_c向左移动一个位置；
如果a+b+c<target，那么就将 p_b向右移动一个位置。
```

#### 

```java
        Arrays.sort(nums);
        int best = 10000;
        int x=0,y=0,z=0;
        for(int i = 0; i < nums.length-2; ++i){
          //保证此次第i个数与前一个不同
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
          //维护两个指针，l和r，分别代表b和c
            int l = i+1,r = nums.length-1;
            while(l < r){
              //计算当前的和
                int sum = nums[i]+nums[l]+nums[r];
                if(sum == target)
                    return sum;
              //若当前的和跟target更接近，则更新距离最小值
                if(Math.abs(sum - target) < Math.abs(best -target)){
                    best = sum;
                }
              //移动指针l
                if(sum < target){
                    int l0 = l;
                  //保证移动后，与前一个数字不同
                    while(l0 < r && nums[l0] == nums[l])
                        ++l0;
                    l = l0;
                }
              //移动指针r
                if(sum > target){
                    int r0 = r;
                  //保证移动后，与前一个数字不同
                    while(l < r0 && nums[r0] == nums[r])
                        --r0;
                    r = r0;
                }
            }
        }
        return best;
```



<span id="jump17"></span>

## 17.电话号码的字母组合

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。



示例:

```java
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。



### 回溯法

```java
class Solution {
    Map<Integer,String> map = new HashMap<>();
    String s;
    List<String> res;
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0)    return new ArrayList<>();
        //先建立数字和字符串的映射关系
        map.put(2,"abc");
        map.put(3,"def");
        map.put(4,"ghi");
        map.put(5,"jkl");
        map.put(6,"mno");
        map.put(7,"pqrs");
        map.put(8,"tuv");
        map.put(9,"wxyz");
        s = digits;
        res = new ArrayList<>();
        //取出第一个数字对应的字符串
        int num = s.charAt(0) - '0';
        String s1 = map.get(num);
        //以字符串的第i位字符作为根节点，开始回溯
        for(int i = 0; i < s1.length();++i){
            StringBuffer sb = new StringBuffer();
            sb.append(s1.charAt(i));
            backtrack(sb,1);
        }
        return res;
    }

    void backtrack(StringBuffer sb, int cur){
        //若已访问到根节点，就将这个字符串记录到res中
        if(sb.length() == s.length())
            res.add(sb.toString());
        for(int i = cur; i < s.length(); ++i){
            int num = s.charAt(i) - '0';
            String s1 = map.get(num);
            //回溯
            for(int j = 0; j < s1.length(); ++j){
                sb.append(s1.charAt(j));
                backtrack(sb,i+1);
                sb.delete(sb.length()-1,sb.length());
            }
        }
    }
}
```



<span id="jump43"></span>

## 43.字符串相乘

给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

示例 1:

```java
输入: num1 = "2", num2 = "3"
输出: "6"
```


示例 2:

```java
输入: num1 = "123", num2 = "456"
输出: "56088"
```


说明：

```java
num1 和 num2 的长度小于110。
num1 和 num2 只包含数字 0-9。
num1 和 num2 均不以零开头，除非是数字 0 本身。
不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。
```



![leetcode_字符串相乘](/HYCBlog/assets/img/leetcode/leetcode_字符串相乘.png)

```java
class Solution {

    public String multiply(String num1, String num2) {
        if(num1.equals("0") || num2.equals("0")) return "0";

        int n = num1.length()-1,m = num2.length()-1;

        //存放结果
        String res = "";
        for(int i = m; i >= 0; --i){
            StringBuffer sb = new StringBuffer();
            //进位标志
            int add = 0;
            //十位补1个0，百位补2个0，以此类推，便于累加计算每位的乘积
            for(int j = m; j > i; --j){
                sb.append("0");
            }
            
            int y = num2.charAt(i) - '0';
            //遍历num1的每一位
            for(int j = n; j >= 0; --j){
                int x = num1.charAt(j) - '0';
                int product = x * y + add;
                sb.append(product % 10);
                //乘积大于9，进位
                add = product / 10;
            }
            //最后一次计算是否有进位未处理
            if (add != 0) {
                sb.append(add % 10);
            }
            res = addString(res,sb.reverse().toString());
            
        }

        return res;
    }
    String addString(String s1, String s2){
        int n = s1.length()-1, m = s2.length() -1, add = 0;
        StringBuffer res = new StringBuffer();
        while(n >= 0 || m >= 0 || add != 0){
            int x = n >= 0 ? s1.charAt(n) - '0' : 0;
            int y = m >= 0 ? s2.charAt(m) - '0' : 0;
            int sum = x + y + add;
            res.append(sum % 10);
            add = sum / 10;
            --n;
            --m;
        }
        return res.reverse().toString();
    }
}
```



<span id = "jump51"></span>

## 51.N 皇后

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

示例：

```java
输入：4
输出：[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。

```


提示：

* 皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。



n*n的棋盘，放置n个皇后，且每个皇后不能处于同一条横行、纵行或斜线上，说明每一行都有一个皇后。所以我们只要遍历每一行，在合适的位置放置一个皇后即可。合适的位置判断如下：

```markdown
不能在同一列。用一个集合存放已访问的列。
不能在同一正对角线。同一个正对角线上，i-j的值是相等的，因此用一个集合放置已访问过的i-j的值。
不能在同一负对角线。同一个负对角线上，i+j的值是相等的，因此用一个集合放置已访问过的i+j的值。
```

若每个皇后都顺利放置完毕，就记录此时棋盘的状态。

```java
class Solution {
  List<List<String>> res;
  int n;
  int cnt;
  //标识某一列已经有皇后了
  Set<Integer> col;
  //标识某一正对角线已经有皇后了，这条线上，i-j的值是唯一的
  Set<Integer> pos_dia;
  //标识某一负对角线已经有皇后了，这条线上，i+j的值是唯一的
  Set<Integer> neg_dia;
  public List<List<String>> solveNQueens(int n) {
    res = new ArrayList<>();
    if(n == 0){
      res.add(new ArrayList<>());
      return res;
    }
    col = new HashSet<>();
    pos_dia = new HashSet<>();
    neg_dia = new HashSet<>();
    char[][] board = new char[n][n];
    cnt = n;
    this.n = n;
    for(int i = 0; i < n; ++i){
      for(int j = 0; j < n; ++j){
        board[i][j] = '.';
      }
    }
    //按行填充皇后，n个皇后一定分布在不同行，而且每行都会有一个皇后
    backTrack(board, 0);
    return res;
  }

  void backTrack(char[][] board,int row){
    //找到了一种答案
    if(cnt == 0){
      //将棋盘状态记录到列表中
      List<String> list = new ArrayList<>();
      for(int i = 0; i < n; ++i){
        StringBuffer sb = new StringBuffer();
        for(int j = 0; j < n; ++j){
          sb.append(board[i][j]);
        }
        list.add(sb.toString());
      }
      res.add(list);
    }
    //在列上填充
    for(int i = 0; i < n; ++i){
      if(!col.contains(i)){
        //若此位置所在的两条对角线没有被访问过
        if(!pos_dia.contains(i-row) && !neg_dia.contains(i+row)) {
          board[row][i] = 'Q';
          col.add(i);
          pos_dia.add(i - row);
          neg_dia.add(i + row);
          cnt--;
          backTrack(board, row + 1);
          //回溯
          cnt++;
          board[row][i] = '.';
          col.remove(i);
          pos_dia.remove(i - row);
          neg_dia.remove(i + row);
        }
      }
    }
  }

}
```

* 时间复杂度为O(n!)，n为皇后的数量。
* 空间复杂度为O(n)，n为皇后的数量。空间复杂度主要取决于递归调用层数、记录每行放置的皇后的列下标的数组以及三个集合，递归调用层数不会超过 N，数组的长度为 N，每个集合的元素个数都不会超过 N。



回顾这道题，拿到这道题的时候，其实我们很容易看出需要使用枚举的方法来求解这个问题，当我们不知道用什么办法来枚举是最优的时候，可以从下面三个方向考虑：

* 子集枚举：可以把问题转化成「从n^2^个格子中选一个子集，使得子集中恰好有 n 个格子，且任意选出两个都不在同行、同列或者同对角线」，这里枚举的规模是 2^n^
* 组合枚举：可以把问题转化成「从 n^2^个格子中选择 n 个，且任意选出两个都不在同行、同列或者同对角线」，这里的枚举规模是 n~n~^2^;
* 排列枚举：因为这里每行只能放置一个皇后，而所有行中皇后的列号正好构成一个 1 到 n 的排列，所以我们可以把问题转化为一个排列枚举，规模是 n!。

带入一些 nn 进这三种方法验证，就可以知道那种方法的枚举规模是最小的，这里我们发现第三种方法的枚举规模最小。这道题给出的两个方法其实和排列枚举的本质是类似的。







<span id="jump63"></span>

## 63.含障碍物网格中的不同路径

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

**说明：** *m*和 *n* 的值均不超过 100。

示例：

```markdown
输入:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
输出: 2
解释:
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```



### 动态规划

设`c[i][j]`代表从`(0,0)`网格走到`(i,j)`网格有几种走法，其状态方程为：

```markdown
c[i][j] = 0 											if obstacleGrid[i][j] == 1,
c[i][j] = c[i-1][j] + c[i][j-1] 	if obstacleGrid[i][j] == 0
```



```java
class Solution {

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] c = new int[m][n];
      //边界判断
        c[0][0] = obstacleGrid[0][0] == 1 ? 0 : 1;
        for(int i = 1; i < n; ++i){
            c[0][i] = obstacleGrid[0][i] == 1 ? 0 : c[0][i-1];
        }
        for(int i = 1; i < m; ++i){
            c[i][0] = obstacleGrid[i][0] == 1 ? 0 : c[i-1][0];
        }
			//状态转移
        for(int i = 1; i < m; ++i)
            for(int j = 1; j < n; ++j){
                c[i][j] = obstacleGrid[i][j] == 1 ? 0 : c[i-1][j] + c[i][j-1];
            }

        return c[m-1][n-1];
    }

    public static void main(String[] args) {
        int[][] m = new int[][]{
                {0,1}
        };
        int res = new Solution().uniquePathsWithObstacles(m);
        System.out.println(res);
    }
}
```

<span id="jump67"></span>

## 67.二进制求和

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`

示例 1:

```markdown
输入: a = "11", b = "1"
输出: "100"
```


示例 2:

```markdown
输入: a = "1010", b = "1011"
输出: "10101"
```

提示：

```markdown
每个字符串仅由字符 '0' 或 '1' 组成。
1 <= a.length, b.length <= 10^4
字符串如果不是 "0" ，就都不含前导零。
```



```java
public class Solution {
    public String addBinary(String a, String b) {
        //如果a的长度小于b，则交换a和b，保证a的长度总是最长的
        if (b.length() > a.length()) {
            String tmp = a;
            a = b;
            b = tmp;
        }
        //填充b
        String pz = "";
        for (int i = 0; i < a.length() - b.length(); ++i) {
            pz += "0";
        }
        b = pz + b;
        //进位标志
        int c = 0;
        String res = "";
        for (int i = a.length()-1; i >= 0; --i) {
            int ca = (int) a.charAt(i) - 48;
            int cb = (int) b.charAt(i) - 48;

            switch (ca + cb + c){
                case 0:
                    res = "0" + res;
                    break;
                case 1:
                    res = "1" + res;
                    c = 0;
                    break;
                case 2:
                    res = "0" + res;
                    c = 1;
                    break;
                case 3:
                    res = "1" + res;
                    c = 1;
                    break;
            }
        }
        if(c == 1)
            res = "1"+res;
        //System.out.println(b);
        return res;
    }
    public static void main(String[] args){
        String a = "1010";
        String b = "1011";
        String res = new Solution().addBinary(a,b);
        System.out.println(res);
    }

}
```



```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuffer ans = new StringBuffer();

        int n = Math.max(a.length(), b.length()), carry = 0;
        for (int i = 0; i < n; ++i) {
            carry += i < a.length() ? (a.charAt(a.length() - 1 - i) - '0') : 0;
            carry += i < b.length() ? (b.charAt(b.length() - 1 - i) - '0') : 0;
            ans.append((char) (carry % 2 + '0'));
            carry /= 2;
        }

        if (carry > 0) {
            ans.append('1');
        }
        ans.reverse();

        return ans.toString();
    }
}
```

<span id="jump93"></span>

## 93. 恢复IP地址

给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址正好由四个整数（每个整数位于 0 到 255 之间组成），整数之间用 '.' 分隔。

示例:

```java
输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]
```



![leetcode_恢复Ip地址](/Users/huangyucai/Documents/code/git_depositorys/github_KolinHuang/HYCBlog/assets/img/leetcode/leetcode_恢复Ip地址.png)

```java
package leetcode;
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("all")
class RestoreIpAddresses {
    private int SEG_COUNT = 4;
    private List<String> res = new ArrayList<String>();
    private int[] segment = new int[SEG_COUNT];

    public List<String> solution(String s) {
        dfs(s,0,0);
        return res;
    }
    private void dfs(String s, int segId, int segStart){
        //找到了第四段地址
        if(segId == SEG_COUNT){
            //已经遍历完字符串
            if(segStart == s.length()){
                //确定一种答案
                StringBuffer sb = new StringBuffer();
                for(int i = 0; i < SEG_COUNT; ++i){
                    sb.append(segment[i]);
                    if(i != SEG_COUNT-1)
                        sb.append(".");
                }
                res.add(sb.toString());
            }
            return;
        }
        //如果未找到第四段地址，字符串就已经遍历完，则回溯
        if(segStart == s.length())
            return;

        //如果有前导0，那么这个位置的segment只能为0
        if(s.charAt(segStart)=='0'){
            segment[segId] = 0;
            dfs(s,segId+1,segStart+1);
        }
        //一般情况
        int addr = 0;
        for(int segEnd = segStart; segEnd < s.length(); ++segEnd){
            addr = addr * 10 + (s.charAt(segEnd) - '0');
            if(addr>0 && addr <= 0xFF){
                segment[segId] = addr;
                dfs(s, segId + 1, segEnd + 1);
            }else{
                break;
            }
        }
    }
    public static void main(String[] args) {
        String s = "25525511135";
        List<String> res = new RestoreIpAddresses().solution(s);
        System.out.println(res.toString());
    }
}
```



* 如果需要字符串拼接，定义StringBuffer会好很多。
* 但需要递归函数返回值时，最好不要直接用函数返回，定义一个全局变量来直接记录会好很多。
* 思考递归时，不要把问题划分的太细，要一次递归能够处理一个子问题。递归树的每条路径对应一种解法。像这题，我一开始想着从最后一个地址段开始，遍历所有情况，然后用返回值把字符串拼接起来，这种思维非常混乱。
* 写递归函数，先写满足要求的情况，即递归结束条件；再写特殊情况，最后写一般情况。

<span id="jump96"></span>

## 96. 不同的二叉搜索树

给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？

示例:

输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

```markdown
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```



### 分治递归

```java
public int numTrees(int n) {

        if(n <= 1) return 1;
        if(n == 2) return 2;

        int num = 0;
        for(int i = 1; i <= n; ++i){
            num += numTrees(i-1) * numTrees(n-i);
        }

        return num;
    }
```

### 不用递归的解法

```java
class Solution {
    public int numTrees(int n) {
        int[] G = new int[n + 1];
        G[0] = 1;
        G[1] = 1;

        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= i; ++j) {
                G[i] += G[j - 1] * G[i - j];
            }
        }
        return G[n];
    }
}
```



### 卡特兰数

```java
class Solution {
    public int numTrees(int n) {
        // 提示：我们在这里需要用 long 类型防止计算过程中的溢出
        long C = 1;
        for (int i = 0; i < n; ++i) {
            C = C * 2 * (2 * i + 1) / (i + 2);
        }
        return (int) C;
    }
}
```

<span id="jump100"></span>

## 100. 相同的树



给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

示例 1:

    输入:       1         1
              / \       / \
             2   3     2   3
    				[1,2,3],   [1,2,3]
    输出: true


示例 2:

    输入:      1          1
              /           \
             2             2
        [1,2],     [1,null,2]
    输出: false


示例 3:

    输入:       1         1
              / \       / \
              2   1     1   2
       		 [1,2,1],   [1,1,2]
    输出: false

### 深度优先

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null && q == null) return true;
        if(p == null || q == null) return false;

        if(p.val != q.val)
            return false;
        else
            return isSameTree(p.left,q.left)&&isSameTree(p.right,q.right);
    }
}
```



### 广度优先

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        } else if (p == null || q == null) {
            return false;
        }
        Queue<TreeNode> queue1 = new LinkedList<TreeNode>();
        Queue<TreeNode> queue2 = new LinkedList<TreeNode>();
        queue1.offer(p);
        queue2.offer(q);
        while (!queue1.isEmpty() && !queue2.isEmpty()) {
            TreeNode node1 = queue1.poll();
            TreeNode node2 = queue2.poll();
            if (node1.val != node2.val) {
                return false;
            }
            TreeNode left1 = node1.left, right1 = node1.right, left2 = node2.left, right2 = node2.right;
            if (left1 == null ^ left2 == null) {
                return false;
            }
            if (right1 == null ^ right2 == null) {
                return false;
            }
            if (left1 != null) {
                queue1.offer(left1);
            }
            if (right1 != null) {
                queue1.offer(right1);
            }
            if (left2 != null) {
                queue2.offer(left2);
            }
            if (right2 != null) {
                queue2.offer(right2);
            }
        }
        return queue1.isEmpty() && queue2.isEmpty();
    }
}
```

<span id="jump108"></span>

## 108. 将有序数组转换为二叉搜索树

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:



    给定有序数组: [-10,-3,0,5,9],
    
    一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树： 		
          0
    		 / \
        -3  9
       /   /
     -10  5

BST的中序遍历是升序，所以本题等同于根据中序遍历的序列恢复二叉搜索树。若是普通的二叉搜索树，只需要选择序列中的任意一元素作为根节点，以该元素左边的升序序列构建左子树，右边的升序序列构建右子树，这样得到的就是一颗二叉搜索树了。

本题要求建立平衡二叉搜索树，因此每次需要选择升序序列的中间元素作为根节点。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return dfs(nums, 0, nums.length-1);
    }

    public TreeNode dfs(int[] nums, int l, int h){
        if(l > h) return null;
        int mid = l + (h-l)/2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = dfs(nums, l, mid-1);
        root.right = dfs(nums, mid+1,h);
        return root;

    }
}
```

<span id="jump109"></span>

## 109.  有序链表转换二叉搜索树

给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:



    给定的有序链表： [-10, -3, 0, 5, 9],
    
    一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：
    			 0
          / \
        -3   9
       /   /
     -10  5

具体地，设当前链表的左端点为left，右端点 right，包含关系为「左闭右开」，即left 包含在链表中而 right 不包含在链表中。我们希望快速地找出链表的中位数节点 mid。

```markdown
为什么要设定「左闭右开」的关系？由于题目中给定的链表为单向链表，访问后继元素十分容易，但无法直接访问前驱元素。因此在找出链表的中位数节点mid 之后，如果设定「左闭右开」的关系，我们就可以直接用 (left,mid) 以及 (mid.next,right) 来表示左右子树对应的列表了。并且，初始的列表也可以用(head,null) 方便地进行表示，其中null 表示空节点。
```

找出链表中位数节点的方法多种多样，其中较为简单的一种是「快慢指针法」。初始时，快指针 fast 和慢指针slow 均指向链表的左端点 left。我们将快指针fast 向右移动两次的同时，将慢指针slow 向右移动一次，直到快指针到达边界（即快指针到达右端点或快指针的下一个节点是右端点）。此时，慢指针对应的元素就是中位数。

在找出了中位数节点之后，我们将其作为当前根节点的元素，并递归地构造其左侧部分的链表对应的左子树，以及右侧部分的链表对应的右子树。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        return buildTree(head,null);
    }

    TreeNode buildTree(ListNode left, ListNode right){
        if(left == right)
            return null;
        ListNode mid = getMidian(left,right);
        TreeNode root = new TreeNode(mid.val);
        root.left = buildTree(left,mid);
        root.right = buildTree(mid.next,right);
        return root;
    }

    ListNode getMidian(ListNode left, ListNode right){
        if(left == right)
            return null;
        ListNode fast = left;
        ListNode slow = left;
        while(fast != right && fast.next != right){
            fast = fast.next;
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```



<span id="jump110"></span>

## 110.平衡二叉树

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

示例 1:

给定二叉树 [3,9,20,null,null,15,7]

    		3
       / \
      9  20
        /  \
       15   7

返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]

           1
          / \
         2   2
        / \
       3   3
      / \
     4   4

返回 false 。



### 自顶向下的递归

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        if(root.left == null && root.right == null)
            return true;
      	//当左右子树深度差大于1时，返回false
        if(Math.abs(getDepth(root.left) - getDepth(root.right)) > 1)
            return false;
      	//继续判断左右子树
        return isBalanced(root.left) && isBalanced(root.right);
    }
		//获取二叉树深度
    int getDepth(TreeNode root){
        if(root == null)
            return 0;
        return Math.max(getDepth(root.left),getDepth(root.right))+1;
    }
}
```



### 自底向上的递归

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) >= 0;
    }

    public int height(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
        if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }
}

```



<span id="jump111"></span>

## 111. 二叉树的最小深度

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明: 叶子节点是指没有子节点的节点。

示例:

给定二叉树 `[3,9,20,null,null,15,7]`,

    		3
       / \
      9  20
        /  \
       15   7

返回它的最小深度  2.



### 深度优先

递归判断，当前节点为空时返回0；当前节点不为空，遍历到叶节点时返回1，遍历到非叶节点时，返回左右子树的最小深度。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null)    return 0;
        if(root.left == null && root.right == null)
            return 1;
        int left = Integer.MAX_VALUE,right = Integer.MAX_VALUE;
        if(root.left != null)   left = minDepth(root.left);
        if(root.right != null)  right = minDepth(root.right);
        return Math.min(left,right)+1;

}
```

### 广度优先

当我们找到一个叶子节点时，直接返回这个叶子节点的深度。广度优先搜索的性质保证了最先搜索到的叶子节点的深度一定最小。

```java
class Solution {
    class QueueNode {
        TreeNode node;
        int depth;

        public QueueNode(TreeNode node, int depth) {
            this.node = node;
            this.depth = depth;
        }
    }

    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        Queue<QueueNode> queue = new LinkedList<QueueNode>();
        queue.offer(new QueueNode(root, 1));
        while (!queue.isEmpty()) {
            QueueNode nodeDepth = queue.poll();
            TreeNode node = nodeDepth.node;
            int depth = nodeDepth.depth;
            if (node.left == null && node.right == null) {
                return depth;
            }
            if (node.left != null) {
                queue.offer(new QueueNode(node.left, depth + 1));
            }
            if (node.right != null) {
                queue.offer(new QueueNode(node.right, depth + 1));
            }
        }

        return 0;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/solution/er-cha-shu-de-zui-xiao-shen-du-by-leetcode-solutio/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



<span id="jump112"></span>

## 112. 路径总和

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

说明: 叶子节点是指没有子节点的节点。

示例: 
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。



### 先序遍历

用二叉树的前序遍历，每遍历到一个节点，`sum-=node.val`，当遍历到叶节点时，判断sum是否为0，若为0则存在路径，否则继续遍历。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) return false;
        return preOrderTraversal(root,sum);

    }

    //树的先序遍历
    public Boolean preOrderTraversal(TreeNode root, int sum){

        if(root == null) return false;
        //是叶节点
        if(root.left == null && root.right==null){
            return sum-root.val == 0;
        }
        int tmp = sum - root.val;
        return (preOrderTraversal(root.left,tmp) || preOrderTraversal(root.right,tmp));
    }
}
```



<span id="jump120"></span>

## 120. 三角形最小路径和

给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。

 

例如，给定三角形：

```markdown
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```


自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

 

说明：

如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题，那么你的算法会很加分。



### 动态规划

从倒数第二层开始，将每个节点的值加上子节点的最小值，最终根节点的值即为最小路径和。



```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int l = triangle.size();

        if(l == 0) return 0;

        if(l == 1) return triangle.get(0).get(0);

        int min = 0;

        int[][] res = new int[l][l];

        for(int i = 0; i < l; ++i)
            for(int j = 0; j <=i; ++j){
                res[i][j] = triangle.get(i).get(j);
            }

        for(int i = l-2; i >=0; --i)
            for(int j = 0; j <= l-2; ++j){
                res[i][j] += Math.min(res[i+1][j],res[i+1][j+1]);
            }

        return res[0][0];
    }
}
```



```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] f = new int[n][n];
        f[0][0] = triangle.get(0).get(0);
        for (int i = 1; i < n; ++i) {
            f[i][0] = f[i - 1][0] + triangle.get(i).get(0);
            for (int j = 1; j < i; ++j) {
                f[i][j] = Math.min(f[i - 1][j - 1], f[i - 1][j]) + triangle.get(i).get(j);
            }
            f[i][i] = f[i - 1][i - 1] + triangle.get(i).get(i);
        }
        int minTotal = f[n - 1][0];
        for (int i = 1; i < n; ++i) {
            minTotal = Math.min(minTotal, f[n - 1][i]);
        }
        return minTotal;
    }
}
```



```java
//空间优化
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] f = new int[n];
        f[0] = triangle.get(0).get(0);
        for (int i = 1; i < n; ++i) {
            f[i] = f[i - 1] + triangle.get(i).get(i);
            for (int j = i - 1; j > 0; --j) {
                f[j] = Math.min(f[j - 1], f[j]) + triangle.get(i).get(j);
            }
            f[0] += triangle.get(i).get(0);
        }
        int minTotal = f[0];
        for (int i = 1; i < n; ++i) {
            minTotal = Math.min(minTotal, f[i]);
        }
        return minTotal;
    }
}
```



<span id="jump130"></span>

## 130. 被围绕的区域



给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

示例:

```java
X X X X
X O O X
X X O X
X O X X
```


运行你的函数后，矩阵变为：

```java
X X X X
X X X X
X X X X
X O X X
```


解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。



 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。

遍历所有边界上的'O'，将与他相连的'O'染色。

最后遍历所有元素，被染色的恢复为'O'，未被染色的'O'修改为‘X’



```java
class Solution {
    public void solve(char[][] board) {
        if(board.length == 0 || board[0].length == 0) return;
        int row = board.length;
        int col = board[0].length;

        if(row <= 2 || col <= 2) return;
        //标记
        for(int i = 0; i < row; ++i){
            for(int j = 0; j < col; ++j){
                //在边界上
                if((i == 0 || i == row-1 || j == 0 || j == col-1) && board[i][j] == 'O'){
                    //染色
                  	dfs(board,i,j);

                }
            }
        }
        for(int i = 0; i < row; ++i){
            for(int j = 0; j < col; ++j){
                if(board[i][j] == 'O')
                    board[i][j] = 'X';
                if(board[i][j] == 'M')
                    board[i][j] = 'O';
            }
        }

    }
    void dfs(char[][] board, int x, int y){
        int row = board.length;
        int col = board[0].length;
        board[x][y] = 'M';
        if(x + 1 < row && board[x+1][y] == 'O')
            dfs(board,x+1,y);
        if(x - 1 >= 0 && board[x-1][y] == 'O')
            dfs(board,x-1,y);
        if(y + 1 < col && board[x][y+1] == 'O')
            dfs(board,x,y+1);
        if(y - 1 >= 0 && board[x][y-1] == 'O')
            dfs(board,x,y-1);
    }
  
  /*void dfs(vector<vector<char>>& board, int x, int y) {
        if (x < 0 || x >= n || y < 0 || y >= m || board[x][y] != 'O') {
            return;
        }
        board[x][y] = 'A';
        dfs(board, x + 1, y);
        dfs(board, x - 1, y);
        dfs(board, x, y + 1);
        dfs(board, x, y - 1);
    }*/


}
```



**参考**

https://leetcode-cn.com/problems/surrounded-regions/solution/bfsdi-gui-dfsfei-di-gui-dfsbing-cha-ji-by-ac_pipe/



<span id="jump133"></span>

## 133. 克隆图

给你无向连通图中一个节点的引用，请你返回该图的深拷贝（克隆）。

图中的每个节点都包含它的值 val（int） 和其邻居的列表（list[Node]）。

```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```




测试用例格式：

简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（`val = 1`），第二个节点值为 2（`val = 2`），以此类推。该图在测试用例中使用邻接列表表示。

邻接列表 是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。

给定节点将始终是图中的第一个节点（值为 1）。你必须将 给定节点的拷贝 作为对克隆图的引用返回。

```java
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
解释：
图中有 4 个节点。
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。
```

```java
输入：adjList = [[]]
输出：[[]]
解释：输入包含一个空列表。该图仅仅只有一个值为 1 的节点，它没有任何邻居。
```

```java
输入：adjList = []
输出：[]
解释：这个图是空的，它不含任何节点。
```

```java
输入：adjList = [[2],[1]]
输出：[[2],[1]]
```



提示：

* 节点数不超过 100 。
* 每个节点值 Node.val 都是唯一的，1 <= Node.val <= 100。
* 无向图是一个简单图，这意味着图中没有重复的边，也没有自环。
* 由于图是无向的，如果节点 p 是节点 q 的邻居，那么节点 q 也必须是节点 p 的邻居。
* 图是连通图，你可以从给定节点访问到所有节点。



### 深搜

用一个哈希表来存储已访问顶点，键是原图中的顶点，值是拷贝图中对应的顶点。

从原图的某个顶点开始遍历，判断是否以拷贝，若是，则返回哈希表中的值；否则，拷贝该顶点，并放入哈希表中，随即遍历该顶点的邻居，执行深度优先搜索，并将返回值赋给拷贝顶点的邻接表。



```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    private HashMap<Node,Node> visited = new HashMap<>();
    public Node cloneGraph(Node node) {
        if(node == null) return node;
        return dfs(node);
    }

    Node dfs(Node node){
        if(visited.containsKey(node))
            return visited.get(node);
        Node myNode = new Node(node.val);
        visited.put(node,myNode);
        for(Node n : node.neighbors){
            myNode.neighbors.add(dfs(n));
        }
        return myNode;
    }
}
```



### 广搜



```java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return node;
        }

        HashMap<Node, Node> visited = new HashMap();

        // 将题目给定的节点添加到队列
        LinkedList<Node> queue = new LinkedList<Node> ();
        queue.add(node);
        // 克隆第一个节点并存储到哈希表中
        visited.put(node, new Node(node.val, new ArrayList()));

        // 广度优先搜索
        while (!queue.isEmpty()) {
            // 取出队列的头节点
            Node n = queue.remove();
            // 遍历该节点的邻居
            for (Node neighbor: n.neighbors) {
                if (!visited.containsKey(neighbor)) {
                    // 如果没有被访问过，就克隆并存储在哈希表中
                    visited.put(neighbor, new Node(neighbor.val, new ArrayList()));
                    // 将邻居节点加入队列中
                    queue.add(neighbor);
                }
                // 更新当前节点的邻居列表
                visited.get(n).neighbors.add(visited.get(neighbor));
            }
        }

        return visited.get(node);
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/clone-graph/solution/ke-long-tu-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



<span id="jump139"></span>

## 139.单词拆分



给定一个**非空**字符串 *s* 和一个包含**非空**单词列表的字典 *wordDict*，判定 *s* 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

示例 1：

```markdown
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
```

示例 2：

```markdown
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
```

示例 3：

```markdown
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

### 动态规划

设`dp[i]`为字符串前i个字符组成的单词拆分后，能否被字典表示。令`dp[0]=true`，表示空字符串能够被表示。

对于字符串前`i`个字符，判断子串`[j,i-1]`能否被字典表示。目前我们已经确定了`dp[i-1]`的值，接下来要确定`dp[i]`的值，从左向右移动指针`j`,判断是否有这么一种情况出现：即`d[j]=true`，并且子串`[j,i-1]`能够被字典表示。这样一来，字符串的前`i`个字符组成的单词拆分后也能够字典表示，令`dp[i]=true`。

状态方程：

```markdown
						dp[i] = dp[j] && isMatch(s.substring(j,i-1));
```

### 答案

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        //dp[i]表示字符串s的前i个字符串是否能被字典表示
        //初始化dp[0]为true 
        for(int i = 1; i <= s.length(); ++i){
            for(int j = 0; j < i; ++j){
                if(dp[j] && wordDict.contains(s.substring(j,i))){
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```



<span id="jump201"></span>

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



### 分析法-最长公共前缀

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



<span id="jump209"></span>

## 209.寻找长度最小的子数组



给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的连续子数组，并返回其长度。如果不存在符合条件的连续子数组，返回 0。



示例: 

```markdown
输入: s = 7, nums = [2,3,1,2,4,3]
输出: 2
解释: 子数组 [4,3] 是该条件下的长度最小的连续子数组。
```

进阶:

```markdown
如果你已经完成了O(n) 时间复杂度的解法, 请尝试 O(n log n) 时间复杂度的解法。
```

### 动态规划

dp[i]表示数组nums前i个元素中，符合条件的最短数组长度，为0时表示不符合条件。

状态方程：

```markdown
	dp[i] = min(f(i-1), f(i))
```



```java
public int minSubArrayLen(int s, int[] nums) {

        //dp[i]表示数组nums前i个元素中，符合条件的最短数组长度，为0时表示不符合条件
        int[] dp = new int[nums.length];
        if(nums.length == 0) return 0;
        boolean yes = false;
        if(nums[0] >= s)
            dp[0] = 1;
        else
            dp[0] = nums.length+1;

        for(int i = 1; i < dp.length; ++i){
            int tmp = 0;
            int sum = 0;
            for(int j = i; j >= 0; --j){
                sum += nums[j];
                ++tmp;
                if(sum >= s){
                    yes = true;
                    break;
                }
            }
            dp[i] = sum >= s ? Math.min(dp[i - 1], tmp) : dp[i-1];
        }
        return yes ? dp[nums.length-1] : 0;
    }
```



### 暴力法

暴力法是最直观的方法。初始化子数组的最小长度为无穷大，枚举数组`nums` 中的每个下标作为子数组的开始下标，对于每个开始下标 `i`，需要找到大于或等于 `i`的最小下标 `j`，使得从`nums[i]` 到`nums[j] `的元素和大于或等于`s`，并更新子数组的最小长度（此时子数组的长度是 `j-i+1`。



```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum >= s) {
                    ans = Math.min(ans, j - i + 1);
                    break;
                }
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```



### 前缀和+二分查找

为了使用二分查找，需要额外创建一个数组 `sums` 用于存储数组` nums` 的前缀和，其中 `sums[i]` 表示从`nums[0]` 到`nums[i−1]` 的元素和。得到前缀和之后，对于每个开始下标`i`，可通过二分查找得到大于或等于`i`的最小下标 `bound`，使得`sums[bound]−sums[i−1]≥s`，并更新子数组的最小长度（此时子数组的长度是 `bound−(i−1)`）。

因为这道题保证了数组中每个元素都为正，所以前缀和一定是递增的，这一点保证了二分的正确性。如果题目没有说明数组中每个元素都为正，这里就不能使用二分来查找这个位置了。



```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int[] sums = new int[n + 1]; 
        // 为了方便计算，令 size = n + 1 
        // sums[0] = 0 意味着前 0 个元素的前缀和为 0
        // sums[1] = A[0] 前 1 个元素的前缀和为 A[0]
        // 以此类推
        for (int i = 1; i <= n; i++) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
        for (int i = 1; i <= n; i++) {
            int target = s + sums[i - 1];
            int bound = Arrays.binarySearch(sums, target);
            if (bound < 0) {
                bound = -bound - 1;
            }
            if (bound <= n) {
                ans = Math.min(ans, bound - (i - 1));
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```



### 双指针

![leetcode_长度最小的子数组](/HYCBlog/assets/img/leetcode/leetcode_长度最小的子数组.png)

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int start = 0, end = 0;
        int sum = 0;
        while (end < n) {
            sum += nums[end];
            while (sum >= s) {
                ans = Math.min(ans, end - start + 1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```



<span id="jump214"></span>

## 214. 最短回文串

给定一个字符串 s，你可以通过在字符串前面添加字符将其转换为回文串。找到并返回可以用这种方式转换的最短回文串。

示例 1:

```java
输入: "aacecaaa"
输出: "aaacecaaa"
```


示例 2:

```java
输入: "abcd"
输出: "dcbabcd"
```



不难分析，“在字符串前添加字符，将其转换为回文串”，并且要求的回文串最短，说明只需要找到以第一个字符为起始的最长回文子串s1，然后将s-s1剩余的子串翻转放到s的前面即可。

找最长回文子串的方法有很多，动态规划、KMP、中心扩展等。[参考第5题。](#jump5)

本题用暴力判断+中心扩展法超时了，所以需要考虑KMP或Manacher算法。

另外，还有一个条件就是最长回文子串必须是以0为起始位置。

```java
class Solution {
    public String shortestPalindrome(String s) {
        if(s.length() == 0) return "";
        if(s.length() == 1) return s;
        //找到以第一个元素为起始的最大回文子串的结尾位置
        int index = longestPalindrome(s);

        StringBuffer sb = new StringBuffer();
        for(int i = s.length()-1; i >= index;--i){
            sb.append(s.charAt(i));
        }
        return sb.toString() + s;

    }
    public int longestPalindrome(String s) {

        int len = s.length();

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

            if ( p[i] > maxLen) {
                // 记录最长回文子串的长度和相应它在原始字符串中的起点
                if((i - p[i]) / 2 == 0){
                    maxLen = p[i];
                    start = (i - maxLen) / 2;
                }

            }
        }
        return start+maxLen;
    }


    /**
     * 创建预处理字符串
     */
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

}
```









<span id="jump215"></span>

## 215.数组中的第k大元素

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。



示例 1:

```markdown
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

示例 2:

```markdown
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

说明:	你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

### 基于快速排序的选择方法

每完成一轮快速排序，就有一个元素被放在正确的位置上，所以要找到数组第k大的元素，只需在第k大的元素被放置在正确的位置上时，返回其值即可。为了提高快速排序的性能，pivot的选择采用随机数的方式，这样可以避免每次都将数组分为1和n-1的极端情况。

```java
import java.util.*;

class Solution {
        Random random = new Random();

        public int findKthLargest(int[] nums, int k) {
            return quickSelect(nums, 0, nums.length - 1, nums.length - k);
        }
				//快速选择算法
        public int quickSelect(int[] a, int l, int r, int index) {
            int q = randomPartition(a, l, r);
            if (q == index) {
                return a[q];
            } else {
                return q < index ? quickSelect(a, q + 1, r, index) : quickSelect(a, l, q - 1, index);
            }
        }
			//随机选择pivot
       public int randomPartition(int[] a, int l, int r){
            int i = random.nextInt(r-l+1) + l;
            swap(a,i,l);
            return partition(a,l,r);
       }

       public int partition(int[] a, int l, int r){
            //取第l个元素作为基准，此时已经在randomPartition中随机选择了一个数放在了l位置
            int x = a[l];
						
            while(l < r){
                while(l < r && x <= a[r]) --r;
                a[l] = a[r];
                while(l < r && x >= a[l]) ++l;
                a[r] = a[l];
            }
            a[l] = x;
            return l;
       }

        public void swap(int[] a, int i, int j) {
            int temp = a[i];
            a[i] = a[j];
            a[j] = temp;
        }
}
```



### 基于堆排序的选择方法

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int heapSize = nums.length;
        buildMaxHeap(nums, heapSize);
        //排序出数组的后k-1个元素
        for (int i = nums.length - 1; i >= nums.length - k + 1; --i) {
            swap(nums, 0, i);
            --heapSize;
            maxHeapify(nums, 0, heapSize);
        }
        //前k-1大的元素已经被排序到数组的后方，则最大堆的根节点就是第k大元素
        return nums[0];
    }

    public void buildMaxHeap(int[] a, int heapSize) {
        for (int i = heapSize / 2; i >= 0; --i) {
            maxHeapify(a, i, heapSize);
        }
    }

    public void maxHeapify(int[] a, int i, int heapSize){
        int tmp = a[i];
        for(int j = i*2 +1; j < heapSize; j =2 * j +1){
//            若右孩子大于左孩子
            if(a[j+1] > a[j] && j+1 < heapSize)
                ++j;
            //如果当前值已经是最大值，则不做操作
            if(tmp > a[j])
                break;
            //若父节点的值小于其子节点最大值，则将子节点的值赋给父节点
            a[i] = a[j];
            //记录原来父节点值应该放置的位置
            i = j;
        }
        //放置父节点
        a[i] = tmp;
    }

    public void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```



<span id="jump309"></span>

## 309.最佳买卖股票时机含冷冻期

给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
示例:

```markdown
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```



### 动态规划

设`dp[i]`为第`i`天结束后，最大的累积收益。第`i`天结束后有3种状态：

```markdown
1. 持有一支股票，对应的累积最大收益为`dp[i][0]`;
2. 不持有任何股票，并处于冷冻期，累计最大收益为`dp[i][1]`;
3. 不持有任何股票，并不处于冷冻期，累计最大收益为`dp[i][2]`;
```

第`i`天的状态由第`i-1`天状态转移而来，针对以上三种状态，分别有如下可能：

```markdown
1. 当出现状态1时，说明第i天结束后持有一支股票，股票来源可能是
		第i-1天就持有的，那么dp[i][0] = dp[i-1][0];
		第i天买入的，那么第i-1天结束后就不可能持有股票，且不可能处于冷冻期，则
								dp[i][0]=dp[i-1][2]-prices[i];
		综上：dp[i][0] = max(dp[i-1][0], dp[i-1][2]-prices[i]);

2. 当出现状态2时，说明第i天结束后不持有股票，并处于冷冻期，说明第i天卖出了股票，则只有一种可能，即第i-1天持有一支股票：
		dp[i][1] = dp[i-1][0];
3. 当出现状态3时，说明第i天结束后不持有股票，并不处于冷冻期，说明在第i天未买入也未卖出股票，则有两种可能，即第i-1天不持有股票，并处于冷冻期；或第i-1天不持有股票，并不处于冷冻期：
		dp[i][1] = max(dp[i-1][1], dp[i-1][2]);
```



初始条件：`dp[0][0]`表示第1天结束后，持有一支股票，则`dp[0][0] = -prices[0]`，`dp[0][1]`和`dp[0][2]`均为0；

```java
class Solution {
    public int maxProfit(int[] prices) {

        if(prices.length == 0) return 0;

        int l = prices.length;
        int[][] dp = new int[l+1][3];
        dp[0][0] = -prices[0];


        for(int i = 1; i < l; ++i){
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][2]-prices[i]);
            dp[i][1] = dp[i-1][0] + prices[i];
            dp[i][2] = Math.max(dp[i-1][1],dp[i-1][2]);
        }

        int res = dp[l-1][0] > dp[l-1][1] ? dp[l-1][0] : dp[l-1][1];
        res = res > dp[l-1][2] ? res : dp[l-1][2];

        return res;
    }
}
```



### 空间优化

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) {
            return 0;
        }

        int n = prices.length;
        int f0 = -prices[0];
        int f1 = 0;
        int f2 = 0;
        for (int i = 1; i < n; ++i) {
            int newf0 = Math.max(f0, f2 - prices[i]);
            int newf1 = f0 + prices[i];
            int newf2 = Math.max(f1, f2);
            f0 = newf0;
            f1 = newf1;
            f2 = newf2;
        }

        return Math.max(f1, f2);
    }
}
```



<span id="jump332"></span>

## 332. 重新安排行程

给定一个机票的字符串二维数组 [from, to]，子数组中的两个成员分别表示飞机出发和降落的机场地点，对该行程进行重新规划排序。所有这些机票都属于一个从 JFK（肯尼迪国际机场）出发的先生，所以该行程必须从 JFK 开始。

说明:

1. 如果存在多种有效的行程，你可以按字符自然排序返回最小的行程组合。例如，行程 ["JFK", "LGA"] 与 ["JFK", "LGB"] 相比就更小，排序更靠前
2. 所有的机场都用三个大写字母表示（机场代码）。
3. 假定所有机票至少存在一种合理的行程。

示例 1:

```markdown
输入: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
输出: ["JFK", "MUC", "LHR", "SFO", "SJC"]
```


示例 2:

```markdown
输入: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
输出: ["JFK","ATL","JFK","SFO","ATL","SFO"]
解释: 另一种有效的行程是 ["JFK","SFO","ATL","JFK","ATL","SFO"]。但是它自然排序更大更靠后。
```



化简题意：

```markdown
给定一个n个点m条边的图，要求从指定顶点出发，经过所有边恰好一次，并且路径的字典序最小。
```

这种「一笔画」问题与欧拉图或者半欧拉图有着紧密的联系：

```markdown
*	通过图中所有边恰好一次且行遍所有顶点的通路称为欧拉通路。
*	通过图中所有边恰好一次且行遍所有顶点的回路称为欧拉回路。
*	具有欧拉回路的无向图称为欧拉图。
*	具有欧拉通路但不具有欧拉回路的无向图称为半欧拉图。
```

因为本题保证至少存在一种合理的路径，也就告诉了我们，这张图是一个欧拉图或者半欧拉图。

![leetcode_重新安排行程_1](/HYCBlog/assets/img/leetcode/leetcode_重新安排行程_1.png)

![leetcode_重新安排行程_2](/HYCBlog/assets/img/leetcode/leetcode_重新安排行程_2.png)



### 排序

```java
public List<String> findItinerary(List<List<String>> tickets) {
        // 因为逆序插入，所以用链表
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0)
            return res;
        Map<String, List<String>> graph = new HashMap<>();
        //将tickets中的所有边和点都对应存入graph中
        for (List<String> pair : tickets) {
            // 因为涉及删除操作，我们用链表
            //当key不存在时，向map中插入(key=value);当key存在时，返回旧值
//            List<String> nbr = graph.computeIfAbsent(pair.get(0), k -> new LinkedList<>());
            //所以理论上还可以这样写
            if(!graph.containsKey(pair.get(0))){//不存在此key
                //新建一项
                graph.put(pair.get(0),new LinkedList<>());
            }
            //获取邻居列表，并添加项
            List<String> nbr = graph.get(pair.get(0));
            nbr.add(pair.get(1));
        }
        // 将每个邻居节点列表按字典序排序
        graph.values().forEach(x -> x.sort(String::compareTo));
        visit(graph, "JFK", res);
        return res;
    }
    // DFS方式遍历图，当走到不能走为止，再将节点加入到答案
    private void visit(Map<String, List<String>> graph, String src, List<String> res) {
        //获取src的邻居列表
        List<String> nbr = graph.get(src);
        //nbr为空时，说明这个顶点从始至终都没有邻居，nbr.size等于0时，说明这个顶点的邻居已经被访问完了
        while (nbr != null && nbr.size() > 0) {
            //访问一个邻居
            String dest = nbr.remove(0);
            //DFS
            visit(graph, dest, res);
        }
        res.add(0, src); // 逆序插入
    }
```

### 最小堆/优先队列代替排序：

```java
public List<String> findItinerary(List<List<String>> tickets) {
        // 因为逆序插入，所以用链表
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0)
            return res;
        Map<String, PriorityQueue<String>> graph = new HashMap<>();
        //将tickets中的所有边和点都对应存入graph中
        for (List<String> pair : tickets) {
            // 因为涉及删除操作，我们用链表
            //当key不存在时，向map中插入(key=value);当key存在时，返回旧值
//            List<String> nbr = graph.computeIfAbsent(pair.get(0), k -> new LinkedList<>());
            //所以理论上还可以这样写
            if(!graph.containsKey(pair.get(0))){//不存在此key
                //新建一项
                graph.put(pair.get(0),new PriorityQueue<>());
            }
            //获取邻居列表，并添加项
            PriorityQueue<String> nbr = graph.get(pair.get(0));
            nbr.add(pair.get(1));
        }
        // 将每个邻居节点列表按字典序排序
//        graph.values().forEach(x -> x.sort(String::compareTo));
        visit(graph, "JFK", res);
        return res;
    }
    // DFS方式遍历图，当走到不能走为止，再将节点加入到答案
    private void visit(Map<String, PriorityQueue<String>> graph, String src, List<String> res) {
        //获取src的邻居列表
        PriorityQueue<String> nbr = graph.get(src);
        //nbr为空时，说明这个顶点从始至终都没有邻居，nbr.size等于0时，说明这个顶点的邻居已经被访问完了
        while (nbr != null && nbr.size() > 0) {
            //访问一个邻居
            String dest = nbr.poll();
            //DFS
            visit(graph, dest, res);
        }
        res.add(0, src); // 逆序插入
    }
```

### 用stack实现迭代代替递归

```java
public List<String> findItinerary(List<List<String>> tickets) {
        // 因为逆序插入，所以用链表
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0)
            return res;
        Map<String, PriorityQueue<String>> graph = new HashMap<>();
        //将tickets中的所有边和点都对应存入graph中
        for (List<String> pair : tickets) {
            // 因为涉及删除操作，我们用链表
            //当key不存在时，向map中插入(key=value);当key存在时，返回旧值
//            List<String> nbr = graph.computeIfAbsent(pair.get(0), k -> new LinkedList<>());
            //所以理论上还可以这样写
            if(!graph.containsKey(pair.get(0))){//不存在此key
                //新建一项
                graph.put(pair.get(0),new PriorityQueue<>());
            }
            //获取邻居列表，并添加项
            PriorityQueue<String> nbr = graph.get(pair.get(0));
            nbr.add(pair.get(1));
        }
        // 将每个邻居节点列表按字典序排序
//        graph.values().forEach(x -> x.sort(String::compareTo));
        visit(graph, "JFK", res);
        return res;
    }
    // DFS方式遍历图，当走到不能走为止，再将节点加入到答案
    private void visit(Map<String, PriorityQueue<String>> graph, String src, List<String> res) {
        Stack<String> stack = new Stack<>();

        stack.push(src);

        while (!stack.isEmpty()) {
            PriorityQueue<String> nbr;

            while ((nbr = graph.get(stack.peek())) != null &&
                    nbr.size() > 0) {
                stack.push(nbr.poll());
            }
            res.add(0, stack.pop());
        }

    }
```



```markdown
如果没有保证至少存在一种合理的路径，我们需要判别这张图是否是欧拉图或者半欧拉图，具体地：

* 对于无向图 GG，GG 是欧拉图当且仅当 GG 是连通的且没有奇度顶点。
* 对于无向图 GG，GG 是半欧拉图当且仅当 GG 是连通的且 GG 中恰有 22 个奇度顶点。
* 对于有向图 GG，GG 是欧拉图当且仅当 GG 的所有顶点属于同一个强连通分量且每个顶点的入度和出度相同。
* 对于有向图 GG，GG 是半欧拉图当且仅当 GG 的所有顶点属于同一个强连通分量且
* 恰有一个顶点的出度与入度差为 11；
* 恰有一个顶点的入度与出度差为 11；
* 所有其他顶点的入度和出度相同。
```







<span id="jump336"></span>

## 336. 回文对

给定一组 互不相同 的单词， 找出所有不同 的索引对(i, j)，使得列表中的两个单词， words[i] + words[j] ，可拼接成回文串。

 

示例 1：

```markdown
输入：["abcd","dcba","lls","s","sssll"]
输出：[[0,1],[1,0],[3,2],[2,4]] 
解释：可拼接成的回文串为 ["dcbaabcd","abcddcba","slls","llssssll"]
```

示例 2：

```markdown
输入：["bat","tab","cat"]
输出：[[0,1],[1,0]] 
解释：可拼接成的回文串为 ["battab","tabbat"]
```

### 暴力遍历(超出时间限制)



```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        if(words.length <= 1) return null;
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        
        //遍历所有的索引对，若拼接后的字符串是回文串则将索引加入list
        for(int i = 0; i < words.length; ++i)
            for(int j = i+1; j < words.length;++j){
                ArrayList<Integer> tmp = new ArrayList<Integer>();
                ArrayList<Integer> tmp2 = new ArrayList<Integer>();
                String s1 = words[i] + words[j];
                //String s2 = words[j] + words[i];
                if(isHWC(s1)){
                    tmp.add(i);
                    tmp.add(j);
                    res.add(tmp);
                    
                }
                if(isHWC(s2)){
                    tmp2.add(j);
                    tmp2.add(i);
                    res.add(tmp2);
                }  
            }
        return res;         
    }

    public boolean isHWC(String s){
        if(s.length() < 1) return false;
        for(int i = 0,j = s.length()-1; i <=j;){
            if(!(s.charAt(i) == s.charAt(j)))
                return false;       
            i++;
            j--;
        }
        return true;
    }
}
```

### 对暴力优化

![leetcode_回文对](/HYCBlog/assets/img/leetcode/leetcode_回文对.png)

### 字典树解法

```java
class Solution {
    class Node {
        int[] ch = new int[26];
        int flag;

        public Node() {
            flag = -1;
        }
    }

    List<Node> tree = new ArrayList<Node>();

    public List<List<Integer>> palindromePairs(String[] words) {
        tree.add(new Node());
        int n = words.length;
        for (int i = 0; i < n; i++) {
            insert(words[i], i);
        }
        List<List<Integer>> ret = new ArrayList<List<Integer>>();
        for (int i = 0; i < n; i++) {
            int m = words[i].length();
            for (int j = 0; j <= m; j++) {
                if (isPalindrome(words[i], j, m - 1)) {
                    int leftId = findWord(words[i], 0, j - 1);
                    if (leftId != -1 && leftId != i) {
                        ret.add(Arrays.asList(i, leftId));
                    }
                }
                if (j != 0 && isPalindrome(words[i], 0, j - 1)) {
                    int rightId = findWord(words[i], j, m - 1);
                    if (rightId != -1 && rightId != i) {
                        ret.add(Arrays.asList(rightId, i));
                    }
                }
            }
        }
        return ret;
    }

    public void insert(String s, int id) {
        int len = s.length(), add = 0;
        for (int i = 0; i < len; i++) {
            int x = s.charAt(i) - 'a';
            if (tree.get(add).ch[x] == 0) {
                tree.add(new Node());
                tree.get(add).ch[x] = tree.size() - 1;
            }
            add = tree.get(add).ch[x];
        }
        tree.get(add).flag = id;
    }

    public boolean isPalindrome(String s, int left, int right) {
        int len = right - left + 1;
        for (int i = 0; i < len / 2; i++) {
            if (s.charAt(left + i) != s.charAt(right - i)) {
                return false;
            }
        }
        return true;
    }

    public int findWord(String s, int left, int right) {
        int add = 0;
        for (int i = right; i >= left; i--) {
            int x = s.charAt(i) - 'a';
            if (tree.get(add).ch[x] == 0) {
                return -1;
            }
            add = tree.get(add).ch[x];
        }
        return tree.get(add).flag;
    }
}
```



### 哈希表解法

```java
class Solution {
    List<String> wordsRev = new ArrayList<String>();
    Map<String, Integer> indices = new HashMap<String, Integer>();

    public List<List<Integer>> palindromePairs(String[] words) {
        int n = words.length;
        for (String word: words) {
            wordsRev.add(new StringBuffer(word).reverse().toString());
        }
        for (int i = 0; i < n; ++i) {
            indices.put(wordsRev.get(i), i);
        }

        List<List<Integer>> ret = new ArrayList<List<Integer>>();
        for (int i = 0; i < n; i++) {
            String word = words[i];
            int m = words[i].length();
            if (m == 0) {
                continue;
            }
            for (int j = 0; j <= m; j++) {
                if (isPalindrome(word, j, m - 1)) {
                    int leftId = findWord(word, 0, j - 1);
                    if (leftId != -1 && leftId != i) {
                        ret.add(Arrays.asList(i, leftId));
                    }
                }
                if (j != 0 && isPalindrome(word, 0, j - 1)) {
                    int rightId = findWord(word, j, m - 1);
                    if (rightId != -1 && rightId != i) {
                        ret.add(Arrays.asList(rightId, i));
                    }
                }
            }
        }
        return ret;
    }

    public boolean isPalindrome(String s, int left, int right) {
        int len = right - left + 1;
        for (int i = 0; i < len / 2; i++) {
            if (s.charAt(left + i) != s.charAt(right - i)) {
                return false;
            }
        }
        return true;
    }

    public int findWord(String s, int left, int right) {
        return indices.getOrDefault(s.substring(left, right + 1), -1);
    }
}
```



<span id="jump337"></span>

## 337. 打家劫舍 3

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

示例 1:

    输入: [3,2,3,null,3,null,1]
     3
    / \
    2  3
    \   \ 
     3   1
     输出: 7 
    解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.

示例 2:

     输入: [3,4,5,1,3,null,1]
         3
        / \
       4   5
      / \   \ 
     1   3   1
     
     输出: 9
    解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.

### 暴力递归

**4 个孙子偷的钱 + 爷爷的钱 VS 两个儿子偷的钱 哪个组合钱多，就当做当前节点能偷的最大钱数。这就是动态规划里面的最优子结构**

由于是二叉树，这里可以选择计算所有子节点

4 个孙子投的钱加上爷爷的钱如下
`int method1 = root.val + rob(root.left.left) + rob(root.left.right) + rob(root.right.left) + rob(root.right.right)`
两个儿子偷的钱如下
`int method2 = rob(root.left) + rob(root.right);`
挑选一个钱数多的方案则
`int result = Math.max(method1, method2);`



```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int rob(TreeNode root) {
        if(root == null) return 0;

        int money = root.val;
        if(root.left != null)
            money += rob(root.left.left) + rob(root.left.right);
        if(root.right != null)
            money += rob(root.right.left) + rob(root.right.right);
        
        return Math.max(money,rob(root.left) + rob(root.right));
    }

}
```

### 记忆化-解决重叠问题

针对解法一种速度太慢的问题，经过分析其实现，我们发现爷爷在计算自己能偷多少钱的时候，同时计算了 4 个孙子能偷多少钱，也计算了 2 个儿子能偷多少钱。这样在儿子当爷爷时，就会产生重复计算一遍孙子节点。

我们这一步针对重复子问题进行优化，我们在做斐波那契数列时，使用的优化方案是记忆化，但是之前的问题都是使用数组解决的，把每次计算的结果都存起来，下次如果再来计算，就从缓存中取，不再计算了，这样就保证每个数字只计算一次。
由于二叉树不适合拿数组当缓存，我们这次使用哈希表来存储结果，TreeNode 当做 key，能偷的钱当做 value

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int rob(TreeNode root) {
        HashMap<TreeNode, Integer> memo = new HashMap<>();
        return memRob(root,memo);
    }

    public int memRob(TreeNode root, HashMap<TreeNode, Integer> memo){
        if(root == null) return 0;
        if(memo.containsKey(root)) return memo.get(root);

        int money = root.val;
        if(root.left != null)
            money += memRob(root.left.left,memo) + memRob(root.left.right,memo);
        if(root.right != null)
            money += memRob(root.right.left,memo) + memRob(root.right.right,memo);

        int res = Math.max(money,memRob(root.left,memo) + memRob(root.right,memo));
        
        memo.put(root,res);

        return res;
    }
}
```





每个节点可选择偷或者不偷两种状态，根据题目意思，相连节点不能一起偷

当前节点选择偷时，那么两个孩子节点就不能选择偷了
当前节点选择不偷时，两个孩子节点只需要拿最多的钱出来就行(两个孩子节点偷不偷没关系)
我们使用一个大小为 2 的数组来表示 `int[] res = new int[2]` 0 代表不偷，1 代表偷
**任何一个节点能偷到的最大钱的状态可以定义为**

1. 当前节点选择不偷：当前节点能偷到的最大钱数 = 左孩子能偷到的钱 + 右孩子能偷到的钱
2. 当前节点选择偷：当前节点能偷到的最大钱数 = 左孩子选择自己不偷时能得到的钱 + 右孩子选择不偷时能得到的钱 + 当前节点的钱数

表示为公式如下:

```markdown
root[0] = Math.max(rob(root.left)[0], rob(root.left)[1]) + Math.max(rob(root.right)[0], rob(root.right)[1])
root[1] = rob(root.left)[0] + rob(root.right)[0] + root.val;
```



### 规划+递归

```java
public int rob(TreeNode root) {
    int[] result = robInternal(root);
    return Math.max(result[0], result[1]);
}

public int[] robInternal(TreeNode root) {
    if (root == null) return new int[2];
    int[] result = new int[2];

    int[] left = robInternal(root.left);
    int[] right = robInternal(root.right);

    result[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    result[1] = left[0] + right[0] + root.val;

    return result;
}

```





<span id="jump378"></span>

## 378. 有序矩阵中第k小元素

给定一个 *`n x n`* 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 `k` 小的元素。
请注意，它是排序后的第 `k` 小元素，而不是第 `k` 个不同的元素。



**示例：**

```markdown
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。
```

**提示：**
你可以假设 k 的值永远是有效的，1 ≤ k ≤ n2 。

### 直接排序

将这个二维数组另存为一维数组，并对该一维数组进行排序。最后返回第k个数，即为答案。

```java
class Solution{
	public int kthSmallest(int[][] matrix, int k) {
        int[] v = new int[matrix.length*matrix.length];
        int c = 0;
        for(int i = 0; i < matrix.length; ++i){
            for(int j = 0; j < matrix.length; ++j){
                v[c++] = matrix[i][j];
            }
        }

        Arrays.sort(v);
        return v[k-1];
    } 
}
```



### 归并排序(没看懂)



```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[0] - b[0];
            }
        });
        int n = matrix.length;
        for (int i = 0; i < n; i++) {
            pq.offer(new int[]{matrix[i][0], i, 0});
        }
        for (int i = 0; i < k - 1; i++) {
            int[] now = pq.poll();
            if (now[2] != n - 1) {
                pq.offer(new int[]{matrix[now[1]][now[2] + 1], now[1], now[2] + 1});
            }
        }
        return pq.poll()[0];
    }
}
```



### 二分查找(还没看)

由题目给出的性质可知，这个矩阵内的元素是从左上到右下递增的（假设矩阵左上角为 matrix[0][0]*m**a**t**r**i**x*[0][0]）。以下图为例：

![leetcode_有序矩阵中的第k小元素](/HYCBlog/assets/img/leetcode/leetcode_有序矩阵中的第k小元素.png)



我们知道整个二维数组中 `matrix[0][0]` 为最小值，`matrix[n−1][n−1]` 为最大值，现在我们将其分别记作` l`和` r`。

可以发现一个性质：任取一个数 `mid` 满足 `l≤mid≤r`，那么矩阵中不大于 `mid` 的数，肯定全部分布在矩阵的左上角。

例如上图，取 `mid=8`，我们可以看到，矩阵中大于 `mid` 的数就和不大于 `mid` 的数分别形成了两个板块，沿着一条锯齿线将这个矩形分开。其中左上角板块的大小即为矩阵中不大于 `mid` 的数的数量。



初始位置在 `matrix[n−1][0]`（即左下角）；

设当前位置为 `matrix[i][j]`。若`matrix[i][j]≤mid`，则将当前所在列的不大于 `mid` 的数的数量（即 `i+1`）累加到答案中，并向右移动，否则向上移动；

不断移动直到走出格子为止。

我们发现这样的走法时间复杂度为 O(n)，即我们可以线性计算对于任意一个 `mid`，矩阵中有多少数不大于它。这满足了二分查找的性质。

不妨假设答案为 `x`，那么可以知道`l≤x≤r`，这样就确定了二分查找的上下界。

每次对于「猜测」的答案` mid`，计算矩阵中有多少数不大于 `mid` ：

如果数量不少于 `k`，那么说明最终答案 `x` 不大于 `mid`；
如果数量少于 `k`，那么说明最终答案 `x` 大于`mid`。



```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0];
        int right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (check(matrix, mid, k, n)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    public boolean check(int[][] matrix, int mid, int k, int n) {
        int i = n - 1;
        int j = 0;
        int num = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] <= mid) {
                num += i + 1;
                j++;
            } else {
                i--;
            }
        }
        return num >= k;
    }
}
```





<span id="jump459"></span>

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

### 枚举法

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



### 字符串匹配

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



### KMP算法

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



<span id = "jump486"></span>

## 486.预测赢家

给定一个表示分数的非负整数数组。 玩家 1 从数组任意一端拿取一个分数，随后玩家 2 继续从剩余数组任意一端拿取分数，然后玩家 1 拿，…… 。每次一个玩家只能拿取一个分数，分数被拿取之后不再可取。直到没有剩余分数可取时游戏结束。最终获得分数总和最多的玩家获胜。

给定一个表示分数的数组，预测玩家1是否会成为赢家。你可以假设每个玩家的玩法都会使他的分数最大化。

 

示例 1：

```java
输入：[1, 5, 2]
输出：False
解释：一开始，玩家1可以从1和2中进行选择。
如果他选择 2（或者 1 ），那么玩家 2 可以从 1（或者 2 ）和 5 中进行选择。如果玩家 2 选择了 5 ，那么玩家 1 则只剩下 1（或者 2 ）可选。
所以，玩家 1 的最终分数为 1 + 2 = 3，而玩家 2 为 5 。
因此，玩家 1 永远不会成为赢家，返回 False 。
```




示例 2：

```java
输入：[1, 5, 233, 7]
输出：True
解释：玩家 1 一开始选择 1 。然后玩家 2 必须从 5 和 7 中进行选择。无论玩家 2 选择了哪个，玩家 1 都可以选择 233 。
最终，玩家 1（234 分）比玩家 2（12 分）获得更多的分数，所以返回 True，表示玩家 1 可以成为赢家。
```




提示：

* 1 <= 给定的数组长度 <= 20.
* 数组里所有分数都为非负数且不会大于 10000000 。
* 如果最终两个玩家的分数相等，那么玩家 1 仍为赢家。



### 动态规划

对于偶数个数字的数组，玩家1一定获胜。因为如果玩家1选择拿法A，玩家2选择拿法B，玩家1输了。则玩家1换一种拿法选择拿法B，因为玩家1是先手，所以玩家1一定可以获胜。

对于奇数个数字的数组，利用动态规划（dynamic programming）计算。 首先证明最优子结构性质。对于数组`[1..n]`中的子数组`[i..j]`，假设玩家1在子数组`[i..j]`中的拿法是最优的，即拿的分数比玩家2多出最多。假设玩家1拿了`i`，则`[i+1..j]`中玩家1拿的方法也一定是最优的。利用反证法证明：如果玩家1在`[i+1..j]`中有更优的拿法，即玩家1在`[i+1...j]`可以拿到更多的分数，则玩家在`[i..j]`中拿到的分数就会比假设的最优拿法拿到的分数更多，显然矛盾。如果玩家1拿了j，同理可证矛盾。 所以当前问题的最优解包含的子问题的解一定也是子问题的最优解。

对于只有一个数字的子数组,即`i=j，dp[i][i] = num[i]`，因为玩家1先手拿了这一个分数，玩家2就没得拿了，所以是最优拿法。 对于两个数字的子数组,即`j-i=1，dp[i][j]=abs(num[i]-num[j])`,玩家1先手拿两个数中大的一个，所以玩家1一定比玩家2多两个数字差的绝对值，为最优拿法。 对于`j-i>1`的子数组，如果玩家1先手拿了`i`，则玩家1手里有`num[i]`分，则玩家2一定会按照`[i+1..j]`这个子数组中的最优拿法去拿，于是玩家2此时手里相当于有`dp[i+1][j]`分，于是玩家1比玩家2多`num[i]-dp[i+1][j]`分。如果玩家1先手拿了`j`，则玩家1手里有`num[j]`分，则玩家2一定会按照`[i..j-1]`这个子数组中的最优拿法去拿，于是玩家2此时手里相当于有`dp[i][j-1]`分，于是玩家1比玩家2多`num[j]-dp[i][j-1]`分。数组的填充方向是从下往上，从左到右，最后填充的是`dp[1][n]`。

```java
class Solution {
    int player1;
    int player2;
    boolean flag;
    public boolean PredictTheWinner(int[] nums) {
        if(nums.length == 1)    return true;
        if(nums.length % 2 == 0)    return true;
        int n = nums.length;
        int[][] dp = new int[n][n];
      	//先填充对角线
        for(int i = 0; i < n; ++i){
            dp[i][i] = nums[i];
        }
     //从对角线开始迭代，从下往上，因为最小的子结构为i=j，最终的结果是[0,n]，所以dp的结果是dp[0][n-1]
        for(int i = n - 1; i >= 0; --i){
            for(int j = i + 1 ; j < n; ++j){
              	//记录得分差，若当前玩家选择了i，那么下一个玩家肯定会选择[i+1,j]的最优拿法
                dp[i][j] = Math.max(nums[i] - dp[i+1][j], nums[j] - dp[i][j-1]);
            }
        }
        return dp[0][n-1] >= 0;

    }

    
}
```









<span id="jump491"></span>

## 491.递增子序列

给定一个整型数组, 你的任务是找到所有该数组的递增子序列，递增子序列的长度至少是2。

示例:

```java
输入: [4, 6, 7, 7]
输出: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
```




说明:

1. 给定数组的长度不会超过15。
2. 数组中的整数范围是 [-100,100]。
3. 给定数组中可能包含重复数字，相等的数字应该被视为递增的一种情况。



### 回溯法

光标cur从0开始移动，找出所有以nums[cur]为起始的递增子序列，用回溯法遍历所有情况。详见注释

```java
class Solution {
    int[] nums;
    Set<List<Integer>> res;
    public List<List<Integer>> findSubsequences(int[] nums) {
        this.nums = nums;
        res = new HashSet<>();
        backtrack(new ArrayList<>(),0);
        return new ArrayList<>(res);
    }

    void backtrack(List<Integer> list, int cur){
      	//当list代表的递增子序列长度大于等于2时，添加入结果集合中
        if(list.size() >= 2)
            res.add(new ArrayList<>(list));
      	//遍历从cur开始的所有情况
        for(int i = cur; i < nums.length; ++i){
          	//特殊判断list为空的情况，然后一般判断当光标所指元素是否大于等于现有递增子序列的最大值
          	//若成立，则将此元素加入list
            if(list.size() == 0 || list.get(list.size() - 1) <= nums[i]){
                list.add(nums[i]);
              	//递归继续判断
                backtrack(list,i+1);
              	//回溯，移动光标至下一点
                list.remove(list.size() - 1);
            }
        }
    }
}
```



相关知识：

[回溯算法入门级详解 + 典型问题列表（持续更新）](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)

[扒一扒回溯算法的裤子](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-xiang-jie-by-labuladong-2/)

参考：

https://leetcode-cn.com/problems/increasing-subsequences/solution/java-di-gui-jie-fa-by-don-vito-corleone-3/



<span id="jump529"></span>

## 529. 扫雷游戏

让我们一起来玩扫雷游戏！

给定一个代表游戏板的二维字符矩阵。 'M' 代表一个未挖出的地雷，'E' 代表一个未挖出的空方块，'B' 代表没有相邻（上，下，左，右，和所有4个对角线）地雷的已挖出的空白方块，数字（'1' 到 '8'）表示有多少地雷与这块已挖出的方块相邻，'X' 则表示一个已挖出的地雷。

现在给出在所有未挖出的方块中（'M'或者'E'）的下一个点击位置（行和列索引），根据以下规则，返回相应位置被点击后对应的面板：

1. 如果一个地雷（'M'）被挖出，游戏就结束了- 把它改为 'X'。
2. 如果一个没有相邻地雷的空方块（'E'）被挖出，修改它为（'B'），并且所有和其相邻的未挖出方块都应该被递归地揭露。
3. 如果一个至少与一个地雷相邻的空方块（'E'）被挖出，修改它为数字（'1'到'8'），表示相邻地雷的数量
4. 如果在此次点击中，若无更多方块可被揭露，则返回面板。


示例 1：

输入: 

```java
[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]

Click : [3,0]
输出: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```



示例 2：

```java
输入: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Click : [1,2]

输出: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

 

注意：

1. 输入矩阵的宽和高的范围为 [1,50]。
2. 点击的位置只能是未被挖出的方块 ('M' 或者 'E')，这也意味着面板至少包含一个可点击的方块。
3. 输入面板不会是游戏结束的状态（即有地雷已被挖出）。
4. 简单起见，未提及的规则在这个问题中可被忽略。例如，当游戏结束时你不需要挖出所有地雷，考虑所有你可能赢得游戏或标记方块的情况。

### 深度优先

八个方向都是相邻节点。

```java
class Solution {
    int m;
    int n;
    char[][] board;
    boolean[][] visited;
    public char[][] updateBoard(char[][] board, int[] click) {
        if(board[click[0]][click[1]] == 'M'){
            board[click[0]][click[1]] = 'X';
            return board;
        }
        this.board = board;
        m = board.length;
        n = board[0].length;
        visited = new boolean[m][n];
        dfs(click[0],click[1]);
        return board;
    }

    void dfs(int x, int y){
        if(visited[x][y])   return;
        int cnt = count(x,y);
        if(cnt > 0){
            board[x][y] = (char)(cnt+48);
            visited[x][y] = true;
            return;
        }
        board[x][y] = 'B';
        visited[x][y] = true;
        if(x-1 >= 0)    dfs(x-1,y);
        if(x + 1 < m)   dfs(x+1,y);
        if(y-1 >= 0)    dfs(x,y-1);
        if(y+1 < n)     dfs(x,y+1);
        if(x+1 < m && y-1 >= 0)   dfs(x+1,y-1);
        if(x-1 >= 0 && y-1 >= 0)   dfs(x-1,y-1);
        if(x-1 >= 0 && y+1 < n)   dfs(x-1,y+1);
        if(x+1 < m && y+1 < n)   dfs(x+1,y+1);
    }

    int count(int x, int y){
        int res = 0;
        if(x-1 >= 0 && board[x-1][y] == 'M')   res++;
        if(x + 1 < m && board[x+1][y] == 'M')   res++;
        if(y-1 >= 0 && board[x][y-1] == 'M')   res++;
        if(y+1 < n && board[x][y+1] == 'M')   res++;
        if(x+1 < m && y-1 >= 0 && board[x+1][y-1] == 'M')   res++;
        if(x-1 >= 0 && y-1 >= 0 && board[x-1][y-1] == 'M')   res++;
        if(x-1 >= 0 && y+1 < n && board[x-1][y+1] == 'M')   res++;
        if(x+1 < m && y+1 < n && board[x+1][y+1] == 'M')   res++;
        return res;
    }
}
```

### 广度优先

同样地，我们也可以将深度优先搜索改为广度优先搜索来模拟，我们只要在\textit{cnt}cnt 为零的时候，将当前点相邻的未挖出的方块加入广度优先搜索的队列里即可，其他情况不加入队列，这里不再赘述。

```java
class Solution {
    int[] dirX = {0, 1, 0, -1, 1, 1, -1, -1};
    int[] dirY = {1, 0, -1, 0, 1, -1, 1, -1};

    public char[][] updateBoard(char[][] board, int[] click) {
        int x = click[0], y = click[1];
        if (board[x][y] == 'M') {
            // 规则 1
            board[x][y] = 'X';
        } else{
            bfs(board, x, y);
        }
        return board;
    }

    public void bfs(char[][] board, int sx, int sy) {
        Queue<int[]> queue = new LinkedList<int[]>();
        boolean[][] vis = new boolean[board.length][board[0].length];
        queue.offer(new int[]{sx, sy});
        vis[sx][sy] = true;
        while (!queue.isEmpty()) {
            int[] pos = queue.poll();
            int cnt = 0, x = pos[0], y = pos[1];
            for (int i = 0; i < 8; ++i) {
                int tx = x + dirX[i];
                int ty = y + dirY[i];
                if (tx < 0 || tx >= board.length || ty < 0 || ty >= board[0].length) {
                    continue;
                }
                // 不用判断 M，因为如果有 M 的话游戏已经结束了
                if (board[tx][ty] == 'M') {
                    ++cnt;
                }
            }
            if (cnt > 0) {
                // 规则 3
                board[x][y] = (char) (cnt + '0');
            } else {
                // 规则 2
                board[x][y] = 'B';
                for (int i = 0; i < 8; ++i) {
                    int tx = x + dirX[i];
                    int ty = y + dirY[i];
                    // 这里不需要在存在 B 的时候继续扩展，因为 B 之前被点击的时候已经被扩展过了
                    if (tx < 0 || tx >= board.length || ty < 0 || ty >= board[0].length || board[tx][ty] != 'E' || vis[tx][ty]) {
                        continue;
                    }
                    queue.offer(new int[]{tx, ty});
                    vis[tx][ty] = true;
                }
            }
        }
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/minesweeper/solution/sao-lei-you-xi-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



<span id="jump546"></span>

## 546.移除盒子

给出一些不同颜色的盒子，盒子的颜色由数字表示，即不同的数字表示不同的颜色。
你将经过若干轮操作去去掉盒子，直到所有的盒子都去掉为止。每一轮你可以移除具有相同颜色的连续 k 个盒子（k >= 1），这样一轮之后你将得到 k*k 个积分。
当你将所有盒子都去掉之后，求你能获得的最大积分和。

 示例：

```java
输入：boxes = [1,3,2,2,2,3,4,3,1]
输出：23
解释：
[1, 3, 2, 2, 2, 3, 4, 3, 1] 
----> [1, 3, 3, 4, 3, 1] (3*3=9 分) 
----> [1, 3, 3, 3, 1] (1*1=1 分) 
----> [1, 1] (3*3=9 分) 
----> [] (2*2=4 分)
```

提示：

```java
1 <= boxes.length <= 100
1 <= boxes[i] <= 100
```



![leetcode_移除盒子_1](/HYCBlog/assets/img/leetcode/leetcode_移除盒子_1.png)

![leetcode_移除盒子_2](/HYCBlog/assets/img/leetcode/leetcode_移除盒子_2.gif)

```java
class Solution {
    public int removeBoxes(int[] boxes) {
        int[][][] dp = new int[100][100][100];
        return calculatePoints(boxes, dp, 0, boxes.length - 1, 0);
    }

    public int calculatePoints(int[] boxes, int[][][] dp, int l, int r, int k) {
        if (l > r) return 0;
        if (dp[l][r][k] != 0) return dp[l][r][k];
        while (r > l && boxes[r] == boxes[r - 1]) {
            r--;
            k++;
        }
        dp[l][r][k] = calculatePoints(boxes, dp, l, r - 1, 0) + (k + 1) * (k + 1);
        for (int i = l; i < r; i++) {
            if (boxes[i] == boxes[r]) {
                dp[l][r][k] = Math.max(dp[l][r][k], calculatePoints(boxes, dp, l, i, k + 1) + calculatePoints(boxes, dp, i + 1, r - 1, 0));
            }
        }
        return dp[l][r][k];
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/remove-boxes/solution/yi-chu-he-zi-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



<span id="jump557"></span>

## 557.反转字符串中的单词 III

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

 

示例：

```java
输入："Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
```


提示:

在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。



```java
class Solution {
    public String reverseWords(String s) {
        if(s.length() < 2)    return s;
        StringBuffer res = new StringBuffer(); 
        String[] arr = s.split(" ");
        
        for(int i = 0; i < arr.length; ++i){
            if(i == 0)
                res.append(reverseString(arr[i]));
            else
                res.append( " " + reverseString(arr[i]));

        }
        return res.toString();

    }

    String reverseString(String s){
        if(s.length() < 2)  return s;
        StringBuffer sb = new StringBuffer();
        for(int i = s.length()-1; i >= 0;--i){
            sb.append(s.charAt(i));
        }
        return sb.toString();
    }
}
```

发现字符串直接相加的效率比StringBuffer的append慢多了，以后尽量不用字符串直接相加。



<span id="jump647"></span>

## 647. 回文子串

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

 

示例 1：

```java
输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

示例 2：

```java
输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

提示：

```java
输入的字符串长度不会超过 1000 。
```



### 暴力法

遍历所有子串

```java
class Solution {
    public int countSubstrings(String s) {
        int cnt = s.length();
        for(int i = 0; i < s.length(); ++i){
            for(int j = i+1; j <= s.length(); ++j){
                cnt += isPlindromeString(s.substring(i,j));
            }
        }
        return cnt;
    }

    int isPlindromeString(String s){
        if(s.length() <= 1) return 0;
        int l = 0;
        int r = s.length() - 1;
        while(l <= r){
            if(s.charAt(l) != s.charAt(r))
                return 0;
            ++l;
            --r;
        }
        return 1;
    }
}
```

剪枝

```java
class Solution {
    public int countSubstrings(String s) {
        int cnt = s.length();
        char c = s.charAt(0);
        boolean flag = true;
      	//------------剪枝------------------
        for(int i = 1; i < s.length(); ++i){
            if(s.charAt(i) != c){
                flag = false;
                break;
            }
        }
        if(flag) return (s.length()*(s.length() + 1))/2;
      	//------------剪枝------------------

        for(int i = 0; i < s.length(); ++i){
            for(int j = i+1; j <= s.length(); ++j){
                cnt += isPlindromeString(s.substring(i,j));
            }
        }
        return cnt;
    }

    int isPlindromeString(String s){
        if(s.length() <= 1) return 0;
        int l = 0;
        int r = s.length() - 1;
        while(l <= r){
            if(s.charAt(l) != s.charAt(r))
                return 0;
            ++l;
            --r;
        }
        return 1;
    }
}
```



### 回文中心法

![leetcode_回文子串](/HYCBlog/assets/img/leetcode/leetcode_回文子串.png)

```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length(), ans = 0;
        for (int i = 0; i < 2 * n - 1; ++i) {
            int l = i / 2, r = i / 2 + i % 2;
            while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                --l;
                ++r;
                ++ans;
            }
        }
        return ans;
    }
}
```

### Manacher算法

![leetcode_回文子串_Manacher算法_1](/HYCBlog/assets/img/leetcode/leetcode_回文子串_Manacher算法_1.png)

![leetcode_回文子串_Manacher算法_2](/HYCBlog/assets/img/leetcode/leetcode_回文子串_Manacher算法_2.png)

```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        StringBuffer t = new StringBuffer("$#");
        for (int i = 0; i < n; ++i) {
            t.append(s.charAt(i));
            t.append('#');
        }
        n = t.length();
        t.append('!');

        int[] f = new int[n];
        int iMax = 0, rMax = 0, ans = 0;
        for (int i = 1; i < n; ++i) {
            // 初始化 f[i]
            f[i] = i <= rMax ? Math.min(rMax - i + 1, f[2 * iMax - i]) : 1;
            // 中心拓展
            while (t.charAt(i + f[i]) == t.charAt(i - f[i])) {
                ++f[i];
            }
            // 动态维护 iMax 和 rMax
            if (i + f[i] - 1 > rMax) {
                iMax = i;
                rMax = i + f[i] - 1;
            }
            // 统计答案, 当前贡献为 (f[i] - 1) / 2 上取整
            ans += f[i] / 2;
        }

        return ans;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/palindromic-substrings/solution/hui-wen-zi-chuan-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





<span id="jump657"></span>

## 657. 机器人能否返回原点

在二维平面上，有一个机器人从原点 (0, 0) 开始。给出它的移动顺序，判断这个机器人在完成移动后是否在 (0, 0) 处结束。

移动顺序由字符串表示。字符 move[i] 表示其第 i 次移动。机器人的有效动作有 R（右），L（左），U（上）和 D（下）。如果机器人在完成所有动作后返回原点，则返回 true。否则，返回 false。

注意：机器人“面朝”的方向无关紧要。 “R” 将始终使机器人向右移动一次，“L” 将始终向左移动等。此外，假设每次移动机器人的移动幅度相同。

示例 1:

```java
输入: "UD"
输出: true
解释：机器人向上移动一次，然后向下移动一次。所有动作都具有相同的幅度，因此它最终回到它开始的原点。因此，我们返回 true。
```


示例 2:

```java
输入: "LL"
输出: false
解释：机器人向左移动两次。它最终位于原点的左侧，距原点有两次 “移动” 的距离。我们返回 false，因为它在移动结束时没有返回原点。
```



```java
class Solution {
    public boolean judgeCircle(String moves) {
        int x = 0;
        int y = 0;
        for(int i = 0; i < moves.length(); ++i){
            char c = moves.charAt(i);
            switch (c){
                case 'R':
                    x++;
                    break;
                case 'L':
                    x--;
                    break;
                case 'U':
                    y--;
                    break;
                case 'D':
                    y++;
                    break;
            }
        }
        if(x == 0 && y == 0)    return true;
        return false;
    }
}
```





<span id="jump679"></span>

## 679. 24点游戏



给出4个1-10的数字，通过加减乘除，得到数字为24就算胜利



```java
class Solution {
    public boolean judgePoint24(int[] nums) {
        ArrayList A = new ArrayList<Double>();
        for (int v: nums) A.add((double) v);
        return solve(A);
    }
    private boolean solve(ArrayList<Double> nums) {
        if (nums.size() == 0) return false;
        if (nums.size() == 1) return Math.abs(nums.get(0) - 24) < 1e-6;

        for (int i = 0; i < nums.size(); i++) {
            for (int j = 0; j < nums.size(); j++) {
                if (i != j) {
                    ArrayList<Double> nums2 = new ArrayList<Double>();
                    for (int k = 0; k < nums.size(); k++) if (k != i && k != j) {
                        nums2.add(nums.get(k));
                    }
                    for (int k = 0; k < 4; k++) {
                        if (k < 2 && j > i) continue;
                        if (k == 0) nums2.add(nums.get(i) + nums.get(j));
                        if (k == 1) nums2.add(nums.get(i) * nums.get(j));
                        if (k == 2) nums2.add(nums.get(i) - nums.get(j));
                        if (k == 3) {
                            if (nums.get(j) != 0) {
                                nums2.add(nums.get(i) / nums.get(j));
                            } else {
                                continue;
                            }
                        }
                        if (solve(nums2)) return true;
                        nums2.remove(nums2.size() - 1);
                    }
                }
            }
        }
        return false;
    }
}
```



```java
class Solution {
    public boolean judgePoint24(int[] nums) {
        List<Double> numbers = new ArrayList<>();
        for(int num : nums)
            numbers.add((double)num);
        return solve(numbers);
    }

    boolean solve(List<Double> numbers){
        //若list为空，返回false
        if(numbers.size() == 0) return false;
        //若list长度为1，则说明计算到了最终结果，比较
        if(numbers.size() == 1)
            return Math.abs((numbers.get(0) - 24*1.0)) < 1e-6;
        
        //从其中取出2个不同的数
        for(int i = 0; i < numbers.size(); ++i){
            for(int j = 0; j < numbers.size(); ++j){
                if(i != j){
                    //新建一个数组，避免将取出的数插入到原来的位置
                    List<Double> nums = new ArrayList<>();
                    //将原数组的剩余的数插入到新数组中
                    for(int k = 0; k < numbers.size(); ++k){
                        if(k != i && k != j)
                            nums.add(numbers.get(k));
                    }
                    //计算i,j两个数的结果集，也加入nums数组中
                    Set<Double> resultSet = cal(numbers.get(i), numbers.get(j));
                    //回溯
                    for(Double res : resultSet){
                        nums.add(res);
                        if(solve(nums))
                            return true;
                        nums.remove(nums.size()-1);
                    }
                }
            }
        }
        return false;
    }

    Set<Double> cal(Double a, Double b){
        Set<Double> set = new HashSet<>();
        set.add(a-b);
        set.add(b-a);
        set.add(a+b);
        set.add(a*b);
        if(a != 0)
            set.add(b/a);
        if(b != 0)
            set.add(a/b);
        return set;
    }


}
```





<span id="jump696"></span>

## 696. 计数二进制子串

给定一个字符串 s，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。

重复出现的子串要计算它们出现的次数。

示例 1 :

```java
输入: "00110011"
输出: 6
解释: 有6个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。
请注意，一些重复出现的子串要计算它们出现的次数。
另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。
```


示例 2 :

```java
输入: "10101"
输出: 4
解释: 有4个子串：“10”，“01”，“10”，“01”，它们具有相同数量的连续1和0。
```


注意：

* s.length 在1到50,000之间。
* s 只包含“0”或“1”字符。

### 按字符分组

将字符串串 s 按照 00 和 11 的连续段分组，存在counts 数组中，例如 s=00111011，可以得到这样的counts 数组：counts={2,3,1,2}。

只要遍历这个数组，相邻元素的最小值即为第i次的贡献。一共需遍历n-1次

```java
public class CountBinarySubString {
    public int countBinarySubstrings(String s) {
        List<Integer> counts = new ArrayList<>();
        for(int i = 0,j = 0; i < s.length();){
            while(s.charAt(i) == s.charAt(j)){
                if(++j > s.length()-1) break;
            }
            counts.add(j-i);
            i = j;
        }
        int sum = 0;
        for (int i = 0; i < counts.size()-1; i++) {
            sum += Math.min(counts.get(i),counts.get(i+1));
        }
        return sum;
    }
    public static void main(String[] args) {
        new CountBinarySubString().countBinarySubstrings("00110011");
    }
}
```



### 左右扩展

每遍历到一个相邻互异对，则左右扩展，统计数量。

```java
class Solution {
    private int count = 0;
    public int countBinarySubstrings(String s) {
        
        if(s.length() == 1) return 0;

        for(int i = 0; i < s.length()-1; ++i){
            if(s.charAt(i) != s.charAt(i+1))
                subCounts(s,i);
        }

        return count;
    }

    void subCounts(String s, int i){
        int left = i;
        int right = i+1;
        while(left >= 0 && right <= s.length()-1 
        && s.charAt(left) == s.charAt(i) && s.charAt(right) == s.charAt(i+1)
        ){
            right++;
            left--;
            count++;
        }
    }
}
```



<span id="jump718"></span>

## 718.最长公共连续子数组

给两个整数数组 A 和 B ，返回两个数组中公共的、长度最长的子数组的长度。

示例 1:

```markdown
输入:
A: [1,2,3,2,1]
B: [3,2,1,4,7]
输出: 3

解释: 
长度最长的公共子数组是 [3, 2, 1]。
```

说明:

```markdown
1 <= len(A), len(B) <= 1000
0 <= A[i], B[i] < 100
```



### 动态规划

这个题目是最长公共子序列的变化形式，要求子序列连续。

最长公共子序列解法：设`c[i][j]`为序列a的前`i`个元素和序列b前`j`个元素的最长公共子序列长度（可不连续，只要求找出的子序列下标递增）。那么

```markdown
当`a[i] == b[j]`时，`c[i][j] = c[i-1][j-1]+1`。
当`a[i] != b[j]`时，`c[i][j] = max(c[i-1][j],c[i][j-1])`
```

只需返回`c[a.length][b.length]`就能得到最终的结果。

但是这个题目要求子数组必须连续，则当`a[i] != b[j]`时，要令`c[i][j]=0`，最后返回数组c的最大值就能得到最终结果。



```java
class Solution {
    public int findLength(int[] A, int[] B) {

        //c[i][j]表示A数组的前i个元素和B数组的前i个元素的最长公共子数组长度
        int[][] c = new int[A.length+1][B.length+1];

        for(int i = 0; i < A.length+1;++i){
            c[i][0] = 0;
        }
        for(int i = 0; i < B.length+1;++i){
            c[0][i] = 0;
        }
        int max = 0;
        for(int i = 1; i <= A.length; ++i)
            for(int j = 1; j <= B.length; ++j){
                if(A[i-1] == B[j-1]){
                    c[i][j] = c[i-1][j-1] + 1;
                    if(c[i][j] >= max)
                        max = c[i][j];
                }
                else
                    c[i][j] = 0;
            }


        return max;
    }

    public static void main(String[] args) {
        int[] A = new int[]{1,2,3,2,1};
        int[] B = new int[]{3,2,1,4,7};
        int res = new Solution().findLength(A,B);
        System.out.println(res);
    }
}
```



```java
class Solution {
    public int findLength(int[] A, int[] B) {
        int n = A.length, m = B.length;
        int[][] dp = new int[n + 1][m + 1];
        int ans = 0;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                dp[i][j] = A[i] == B[j] ? dp[i + 1][j + 1] + 1 : 0;
                ans = Math.max(ans, dp[i][j]);
            }
        }
        return ans;
    }
}
```

### 滑动窗口

待补充

### 二分法+哈希

待补充





<span id="jump733"></span>

## 733. 图像渲染

有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。

给你一个坐标 (sr, sc) 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 newColor，让你重新上色这幅图像。

为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。

最后返回经过上色渲染后的图像。

示例 1:

```java
输入: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
输出: [[2,2,2],[2,2,0],[2,0,1]]
解析: 
在图像的正中间，(坐标(sr,sc)=(1,1)),
在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，
因为它不是在上下左右四个方向上与初始点相连的像素点。
```

注意:

```java
image 和 image[0] 的长度在范围 [1, 50] 内。
给出的初始点将满足 0 <= sr < image.length 和 0 <= sc < image[0].length。
image[i][j] 和 newColor 表示的颜色值在范围 [0, 65535]内。
```



### 深搜

```java
import java.awt.Point;

class Solution {
    int[][] images;
    int srcColor;
    int color;
    Set<Point> set;
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if(image[sr][sc] == newColor) return image;
        set = new HashSet<>();
        images = image;
        color = newColor;
        srcColor = images[sr][sc];
        dfs(sr,sc);
        return images;
    }

    void dfs(int sr,int sc){
        if(sr < 0 || sr >= images.length || sc < 0 || sc >= images[0].length)
            return;
        if(images[sr][sc] != srcColor)
            return;
        if(set.contains(new Point(sr,sc)))
            return;
        set.add(new Point(sr,sc));
        images[sr][sc] = color;
        dfs(sr + 1,sc);
        dfs(sr - 1,sc);
        dfs(sr,sc + 1);
        dfs(sr,sc - 1);    
    }
}
```



### 广搜

```java
class Solution {
    int[] dx = {1, 0, 0, -1};
    int[] dy = {0, 1, -1, 0};

    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        int currColor = image[sr][sc];
        if (currColor == newColor) {
            return image;
        }
        int n = image.length, m = image[0].length;
        Queue<int[]> queue = new LinkedList<int[]>();
        queue.offer(new int[]{sr, sc});
        image[sr][sc] = newColor;
        while (!queue.isEmpty()) {
            int[] cell = queue.poll();
            int x = cell[0], y = cell[1];
            for (int i = 0; i < 4; i++) {
                int mx = x + dx[i], my = y + dy[i];
                if (mx >= 0 && mx < n && my >= 0 && my < m && image[mx][my] == currColor) {
                    queue.offer(new int[]{mx, my});
                    image[mx][my] = newColor;
                }
            }
        }
        return image;
    }
}
```



<span id="jump841"></span>

## 841.钥匙和房间

有` N `个房间，开始时你位于 0 号房间。每个房间有不同的号码：`0，1，2，...，N-1`，并且房间里可能有一些钥匙能使你进入下一个房间。

在形式上，对于每个房间 i 都有一个钥匙列表 rooms[i]，每个钥匙 `rooms[i][j] `由 `[0,1，...，N-1] `中的一个整数表示，其中` N = rooms.length`。 钥匙` rooms[i][j] = v `可以打开编号为` v `的房间。

最初，除 0 号房间外的其余所有房间都被锁住。

你可以自由地在房间之间来回走动。

如果能进入每个房间返回 true，否则返回 false。

示例 1：

```java
输入: [[1],[2],[3],[]]
输出: true
解释:  
我们从 0 号房间开始，拿到钥匙 1。
之后我们去 1 号房间，拿到钥匙 2。
然后我们去 2 号房间，拿到钥匙 3。
最后我们去了 3 号房间。
由于我们能够进入每个房间，我们返回 true。
```




示例 2：

```java
输入：[[1,3],[3,0,1],[2],[0]]
输出：false
解释：我们不能进入 2 号房间。
```




提示：

* 1 <= rooms.length <= 1000
* 0 <= rooms[i].length <= 1000
* 所有房间中的钥匙数量总计不超过 3000。



很常规的深搜题，最近回溯做多了，导致每次递归回来都要把已访问的标记取消掉，然而这题仅仅只是一题遍历题而已，只要把所有点访问一遍就可以返回了。广搜也行。

```java
	class Solution {
    Set<Integer> visited;
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        if(rooms.size() < 2)    return true;
        visited = new HashSet<>();
        visited.add(0);
        dfs(rooms,0);
        return visited.size() == rooms.size();
    }

    void dfs(List<List<Integer>> rooms, int index){
        if(visited.size() == rooms.size())
            return;

        List<Integer> nbrs = rooms.get(index);
        for(int i = 0; i < nbrs.size(); ++i){
            int nbr = nbrs.get(i);
            if(!visited.contains(nbr)){
                visited.add(nbr);
                dfs(rooms,nbr);
            }
        }
    }
```

### 空间换时间

```java
class Solution {
    boolean[] visited;
    int cnt;
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        if(rooms.size() < 2)    return true;
        visited = new boolean[rooms.size()];
        cnt = 0;
        dfs(rooms,0);
        return cnt == rooms.size();
    }

    void dfs(List<List<Integer>> rooms, int index){
        visited[index] = true;
        cnt++;
        List<Integer> nbrs = rooms.get(index);
        for(int i = 0; i < nbrs.size(); ++i){
            int nbr = nbrs.get(i);
            if(!visited[nbr]){
                dfs(rooms,nbr);
            }
        }  
    }
}
```



有时候不能理解leetcode的难度分类标准，有些简单题都需要费很多时间，像这种模版题却是中等题。



