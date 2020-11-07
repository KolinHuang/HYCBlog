---
title: Leetcode题解:200~500
author: Kol Huang
date: 2020-09-23 08:29:00 +0800
categories: [Blogging, leetcode]
tags: [算法题解]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/leetcode_cover.jpg
pin: true
---

## 目录

[201.数字范围按位与](#jump201)

[206.反转链表](#jump206)

[209.寻找长度最小的子数组](#jump209)

[214.最短回文串](#jump214)

[216.组合总和3](#jump216)

[221.最大正方形](#jump221)

[235.二叉树的最近公共祖先](#jump235)

[257.二叉树的所有路径](#jump257)

[327.区间和的个数[tag]](#jump327)

[332.重新安排行程](#jump332)

[336.回文对](#jump336)

[349.两个数组的交集](#jump349)

[378.有序矩阵中第K小元素](#jump378)

[381.O(1) 时间插入、删除和获取随机元素 - 允许重复](#jump381)

[344.反转字符串](#jumo344)

[404.左叶子之和](#jump404)

[416.分割等和子集](#jump416)

[459.重复的子字符串](#jump459)

[463.岛屿的周长](#jump463)

[486.预测赢家](#jump486)





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







<span id = "jump206"></span>

## 206.反转链表

反转一个单链表。

示例:

```java
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

进阶:

* 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

迭代：

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode p = head;
        while(p != null){
            ListNode q = p.next;
            p.next = pre;
            pre = p;
            p = q;
        }
        return pre;
    }
}
```

递归:

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        
        return reverse(null, head);
    }

    ListNode reverse(ListNode pre, ListNode p){
        if(p == null){
            return pre;
        }
        ListNode q = p.next;
        p.next = pre;
        pre = p;
        p = q;
        return reverse(pre, p);
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











<span id = "jump216"></span>

## 216.组合总和 III

找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

说明：

* 所有数字都是正整数。
* 解集不能包含重复的组合。 

示例 1:

```java
输入: k = 3, n = 7
输出: [[1,2,4]]
```


示例 2:

```java
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
```



这题比这个系列的前两题简单，几个条件：

* 只含有1-9的整数
* 组合不存在重复数字。

这两个条件简化了许多操作，可以创建一个数组存放待取数字，`{1,2,3,4,5,6,7,8,9}`，每次固定一位，一共取k位数字。

```java
class Solution {
    int[] nums;
    List<List<Integer>> res;
    public List<List<Integer>> combinationSum3(int k, int n) {
        res = new ArrayList<>();
        if(k <= 0 || n <= 0){
            res.add(new ArrayList<>());
            return res;
        }
        //待取数字
        nums = new int[]{1,2,3,4,5,6,7,8,9};
        dfs(0,k,n,0,new ArrayList<Integer>(),0);
        return res;
    }
    void dfs(int cnt,int k,int n,int cur, List<Integer> list, int sum){
        if(cnt > k || sum > n){
            return;
        }
        if(sum == n && cnt == k){
            res.add(new ArrayList<>(list));
            return;
        }
        for(int i = cur; i < nums.length; ++i){
            //剪枝
            if(nums[i] > n) return;
            cnt++;
            sum += nums[i];
            list.add(nums[i]);
            dfs(cnt,k,n,i+1,list,sum);
            cnt--;
            sum-= nums[i];
            list.remove(list.size()-1);
        }
    }
}
```





<span id = "jump221"></span>

## 221.最大正方形

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

示例:

```java
输入: 
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```



动态规划，`dp[i][j]`表示以`(i,j)`为右下角的正方形的最大边长。

```java
class Solution {
    public int maximalSquare(char[][] matrix) {

        if(matrix.length == 0 || matrix[0].length == 0)    return 0;
        //dp[i][j]表示以(i,j)为右下角的正方形的最大边长
        //dp[i][j]的值由其左方，上方，左上方的dp值得到。

        int[][] dp = new int[matrix.length][matrix[0].length];
        int max = 0;
        //边界初始化
        for(int i = 0; i < matrix.length; ++i){
            dp[i][0] = matrix[i][0] == '1' ? 1 : 0;
            max = Math.max(dp[i][0], max);
        }
        for(int j = 0; j < matrix[0].length; ++j){
            dp[0][j] = matrix[0][j] == '1' ? 1 : 0;
            max = Math.max(dp[0][j], max);
        }
        for(int i = 1; i < matrix.length; ++i){
            for(int j = 1; j < matrix[0].length; ++j){
                if(matrix[i][j] == '0')
                    dp[i][j] = 0;
                else
                    dp[i][j] = Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1]) + 1;
                max = Math.max(dp[i][j], max);
            }
        }
        return max * max;
        
    }
}
```











<span id = "jump235"></span>

## 235.二叉搜索树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

示例 1:

```java
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```


示例 2:

```java
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```


说明:

* 所有节点的值都是唯一的。

* p、q 为不同节点且均存在于给定的二叉树中。



思路：

后序遍历，从树的底部开始向上回溯，这样可以保证搜索到的祖先是离p,q最近的。

* 如果p和q分别是root的左右节点，那么root就是我们要找的最近公共祖先
* 如果p和q都是root的左节点，那么返回lowestCommonAncestor(root.left,p,q)
* 如果p和q都是root的右节点，那么返回lowestCommonAncestor(root.right,p,q)



边界条件：

* 如果root是null，则说明我们已经找到最底了，返回null表示没找到
* 如果root与p相等或者与q相等，则返回root
* 如果左子树没找到，递归函数返回null，证明p和q同在root的右侧，那么最终的公共祖先就是右子树找到的结点
* 如果右子树没找到，递归函数返回null，证明p和q同在root的左侧，那么最终的公共祖先就是左子树找到的结点

```java
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q)  return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //有一者为空，那么p和q在另一侧，由于是后序遍历，所以该侧指向最近的公共祖先
        if(left == null)    return right;
        if(right == null)   return left;
        //p,q在两侧
        return root;   
    }   
}
```









<span id = "jump257"></span>

## 257.二叉树的所有路径

给定一个二叉树，返回所有从根节点到叶子节点的路径。

说明: 叶子节点是指没有子节点的节点。

示例:

```java
输入:
   1
 /   \
2     3
 \
  5

输出: ["1->2->5", "1->3"]
解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
```



### 回溯法

```java
class Solution {
    StringBuffer path;
    List<String> res;
    public List<String> binaryTreePaths(TreeNode root) {
        if(root == null)    return new ArrayList<>();
        path = new StringBuffer();
        res = new ArrayList<>();
        getPath(root);
        return res;
    }

    void getPath(TreeNode root){
        //到达叶节点
        if(root.left == null && root.right == null){
            path.append(root.val);
            res.add(path.toString());
            //回溯
            int len = String.valueOf(root.val).length();
            path.delete(path.length()-len,path.length());
            return;
            
        }
        path.append(root.val);
        path.append("->");
        if(root.left != null)   getPath(root.left);
        if(root.right != null)  getPath(root.right);
        //回溯，这题比较奇葩，root.val长度是变化的，所以在删除字符的时候需要计算root.val的长度
        int len = String.valueOf(root.val).length() + 2;
        path.delete(path.length()-len,path.length());

    }
}
```







<span id = "jump327"></span>

## 327.区间和的个数[tag]

给定一个整数数组 nums，返回区间和在 [lower, upper] 之间的个数，包含 lower 和 upper。
区间和 S(i, j) 表示在 nums 中，位置从 i 到 j 的元素之和，包含 i 和 j (i ≤ j)。

说明:
最直观的算法复杂度是 O(n2) ，请在此基础上优化你的算法。

示例:

```java
输入: nums = [-2,5,-1], lower = -2, upper = 2,
输出: 3 
解释: 3个区间分别是: [0,0], [2,2], [0,2]，它们表示的和分别为: -2, -1, 2。
```





归并排序是从最小子数组开始统计的，我们是在归并之前统计下标对，之后再归并，所以不影响。

```java
class Solution {
    //目的是为了统计符合
    //pre[j] - pre[i] >= lower && pre[j] - pre[i] <= upper
    //的下标（i,j）的个数
    //归并排序
    //让排好序的，待归并的两个子数组作为pre
    //计算从左边数组下标i到右边数组下标j的和是否满足条件
    public int countRangeSum(int[] nums, int lower, int upper) {
        long s = 0;
        long[] sum = new long[nums.length + 1];
        //计算[0,i]的前缀和
        for(int i = 0; i < nums.length; ++i){
            s += nums[i];
            sum[i + 1] = s;
        }
        return count(0, sum.length - 1, sum, lower, upper);
    }

    int count(int left, int right, long[] sum, int lower, int upper){
        if(left == right){
            return 0;
        }else{
            int mid = (right - left) / 2 + left;
            //归并
            int n1 = count(left, mid, sum, lower, upper);
            int n2 = count(mid + 1, right, sum, lower, upper);
            int res = n1 + n2;

            //统计下标对的数量

            //枚举左边数组的每一位
            int i = left;
            
            //让l和r指向右边数组的开头
            //让l向右移动，直到sum[l] - sum[i] >= lower
            //让r向右移动，直到sum[r] - sum[i] > upper
            //这样 r - l就是下标对的数量了
             int l = mid + 1;
             int r = mid + 1;
             while(i <= mid){
                 while(l <= right && sum[l] - sum[i] < lower){
                     l++;
                 }
                 while(r <= right && sum[r] - sum[i] <= upper){
                     r++;
                 }
                 res += r - l;
                 ++i;
             }
             //执行左右数组合并
             int[] arr = new int[right - left + 1];
             int ptr1 = left, ptr2 = mid + 1;
             int ptr = 0;

             while(ptr1 <= mid || ptr2 <= right){
                 if(ptr1 > mid){
                     arr[ptr++] = (int) sum[ptr2++];
                 }else if(ptr2 > right){
                     arr[ptr++] = (int) sum[ptr1++];
                 }else{
                     if(sum[ptr1] < sum[ptr2]){
                         arr[ptr++] = (int) sum[ptr1++];
                     }else{
                         arr[ptr++] = (int) sum[ptr2++];
                     }
                 }
             }

             //将归并好的数组放入前缀和数组中
             for(int k = 0; k < arr.length; ++k){
                 sum[left + k] = arr[k];
             }
             return res;

        }
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





<span id = "jump344"></span>

## 344.反转字符串

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

 

示例 1：

```java
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```


示例 2：

```java
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```



```java
class Solution {
    public void reverseString(char[] s) {
        int low = 0;
        int high = s.length - 1;
        while(low < high){
            if(s[low] != s[high]){
                char tmp = s[low];
                s[low] = s[high];
                s[high] = tmp;
            }
            ++low;
            --high;
        }

    }
}
```







<span id = "jump349"></span>

## 349.两个数组的交集

给定两个数组，编写一个函数来计算它们的交集。

 

示例 1：

```java
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

示例 2：

```java
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
```


说明：

* 输出结果中的每个元素一定是唯一的。
* 我们可以不考虑输出结果的顺序。



哈希表：

```java
class Solution {
    
    public int[] intersection(int[] nums1, int[] nums2) {
        //哈希表
        List<Integer> list = new ArrayList<>();
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums1.length; ++i){
            set.add(nums1[i]);
        }
        for(int j = 0; j < nums2.length; ++j){
            if(set.contains(nums2[j]) && !list.contains(nums2[j])){
                list.add(nums2[j]);
            }
        }
        int[] res = new int[list.size()];
        for(int i = 0; i < list.size(); ++i){
            res[i] = list.get(i);
        }
        return res;
    }
}
```

都放入set中，再遍历较小的set：

```java
class Solution {
    
    public int[] intersection(int[] nums1, int[] nums2) {
        //哈希表
        List<Integer> list = new ArrayList<>();
        Set<Integer> set1 = new HashSet<>();
        for(int i = 0; i < nums1.length; ++i){
            set1.add(nums1[i]);
        }
        Set<Integer> set2 = new HashSet<>();
        for(int i = 0; i < nums2.length; ++i){
            set2.add(nums2[i]);
        }
        
        return compareSet(set1, set2);
    }

    int[] compareSet(Set<Integer> set1, Set<Integer> set2){
      	//这个很有趣，可以避免重复代码
        if(set1.size() > set2.size()){
            return compareSet(set2,set1);
        }

        Set<Integer> resSet = new HashSet<>();
        for(Integer num : set1){
            if(set2.contains(num)){
                resSet.add(num);
            }
        }

        int[] res = new int[resSet.size()];

        int index = 0;

        for(Integer num : resSet){
            res[index++] = num;
        }

        return res;

    }
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





<span id = "jump381"></span>

## 381.O(1) 时间插入、删除和获取随机元素 - 允许重复

设计一个支持在平均 时间复杂度 O(1) 下， 执行以下操作的数据结构。

注意: 允许出现重复元素。

* insert(val)：向集合中插入元素 val。
* remove(val)：当 val 存在时，从集合中移除一个 val。
* getRandom：从现有集合中随机获取一个元素。每个元素被返回的概率应该与其在集合中的数量呈线性相关。

示例:

```java
// 初始化一个空的集合。
RandomizedCollection collection = new RandomizedCollection();

// 向集合中插入 1 。返回 true 表示集合不包含 1 。
collection.insert(1);

// 向集合中插入另一个 1 。返回 false 表示集合包含 1 。集合现在包含 [1,1] 。
collection.insert(1);

// 向集合中插入 2 ，返回 true 。集合现在包含 [1,1,2] 。
collection.insert(2);

// getRandom 应当有 2/3 的概率返回 1 ，1/3 的概率返回 2 。
collection.getRandom();

// 从集合中删除 1 ，返回 true 。集合现在包含 [1,2] 。
collection.remove(1);

// getRandom 应有相同概率返回 1 和 2 。
collection.getRandom();
```



用一个列表存放元素，再用一个map存放元素与下标的对应。

插入时，直接将元素插入到列表的最后，然后在map中更新元素索引。

删除时，从map中取出对应元素的索引，再在列表中将此元素与列表最后一个元素交换，最后删除列表最后一个元素即可。

需要注意的是：此元素可能与列表最后一个元素相等，那么就需要同步更新set和hashMap。

```java
class RandomizedCollection {

    List<Integer> nums;
    Map<Integer, Set<Integer>> hashIndex;
    /** Initialize your data structure here. */
    public RandomizedCollection() {
        nums = new ArrayList<>();
        hashIndex = new HashMap<>();
    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    public boolean insert(int val) {
        nums.add(val);
        Set<Integer> set = hashIndex.getOrDefault(val, new HashSet<>());
        set.add(nums.size() - 1);
        hashIndex.put(val, set);
        //size==1说明当前插入的元素是唯一的，没有重复
        return set.size() == 1;

    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    public boolean remove(int val) {
        Set<Integer> set = hashIndex.get(val);
        if(set == null) return false;
        //根据迭代器获取val在set中的索引
        Iterator<Integer> itr = set.iterator();
        int idx = 0;
        if(itr.hasNext()){
            idx = itr.next();
        }

        Set<Integer> lastSet = hashIndex.get(nums.get(nums.size() - 1));
      	//当需要删除的元素和列表最后一个元素不相等时，才需要分别更新两个集合
      	//因为从set中删除一个元素，需要更新索引
      	//从lastSet中删除并插入一个元素，需要更新索引
        if(set != lastSet){
            lastSet.remove(nums.size() - 1);
            lastSet.add(idx);
            set.remove(idx);
        }else{
          	//如果是同一个元素，那么直接删除set最后一个的值即可
            set.remove(nums.size() - 1);
        }
      	//当set为空时，就清出map
        if(set.size() == 0) hashIndex.remove(val);
        //最后处理列表：将找到的索引与最后一个元素交换，然后删除
        nums.set(idx, nums.get(nums.size() - 1));
        nums.remove(nums.size() - 1);
        return true;
    }
    
    /** Get a random element from the collection. */
    public int getRandom() {
        Random random = new Random();
        int idx = random.nextInt(nums.size());
        return nums.get(idx);
    }
}
```







<span id = "jump404"></span>

## 404.左叶子之和

计算给定二叉树的所有左叶子之和。

示例：

```java
		3
   / \
  9  20
    /  \
   15   7
```



递归即可，当访问左子树时，做一次标记，这样可以在访问到左叶子的时候计算和。

```java
class Solution {
    int sum;
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null)    return 0;
        sum = 0;
        traversal(root,false);
        return sum;
    }

    void traversal(TreeNode root, boolean isLeft){
        if(root.left == null && root.right == null && isLeft){
            sum += root.val;
            return;
        }
        if(root.left != null)   traversal(root.left, true);
        if(root.right != null)  traversal(root.right,false);
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





<span id = "jump463"></span>

## 463.岛屿的周长

给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

 

示例 :

```java
输入:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

输出: 16
```



```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        if(grid.length == 0)    return 0;
        if(grid.length == 1 && grid[0].length == 1) return grid[0][0] == 1 ? 4 : 0;
        int res = 0;
        for(int i = 0; i < grid.length; ++i){
            for(int j = 0; j < grid[0].length; ++j){
                if(grid[i][j] == 0) continue;
                //只有一行
                if(grid.length == 1){
                    //左边界
                    if(j == 0){
                        res += grid[i][j + 1] == 0 ? 1 : 0;
                        res += 3;
                    }else if(j == grid[0].length - 1){
                        res += grid[i][j - 1] == 0 ? 1 : 0;
                        res += 3;
                    }else{
                        res += grid[i][j - 1] == 0 ? 1 : 0;
                        res += grid[i][j + 1] == 0 ? 1 : 0;
                        res += 2;
                    }
                    continue;
                //只有一列
                }else if(grid[0].length == 1){
                    if(i == 0){
                        res += grid[i+1][j] == 0 ? 1 : 0;
                        res += 3;
                    }else if(i == grid.length - 1){
                        res += grid[i - 1][j] == 0 ? 1 : 0;
                        res += 3;
                    }else{
                        res += grid[i-1][j] == 0 ? 1 : 0;
                        res += grid[i+1][j] == 0 ? 1 : 0;
                        res += 2;
                    }
                    continue;
                }


                //边界和四个对角特别判断

                //1.在上边界
                if(i == 0){
                    //在左上角
                    if(j == 0){
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += 2;
                    //在右上角
                    }else if(j == grid[0].length - 1){
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += 2;
                    //上边界的边上，不包含角
                    }else{
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += 1;
                    }
                    res += grid[i+1][j] == 0 ? 1 : 0;
                //2.在下边界
                }else if( i == grid.length - 1){
                    //在左下角
                    if(j == 0){
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += 2;
                    //在右下角
                    }else if(j == grid[0].length - 1){
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += 2;
                    //下边界的边上，不包含角
                    }else{
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += 1;
                    }
                    res += grid[i-1][j] == 0 ? 1 : 0;
                //3.在左边界，不包含角
                }else if(j == 0 && i != 0 && i != grid.length - 1){
                    res += grid[i-1][j] == 0 ? 1 : 0;
                    res += grid[i+1][j] == 0 ? 1 : 0;
                    res += grid[i][j+1] == 0 ? 1 : 0;
                    res += 1;
                //4.在右边界，不包含角
                }else if(j == grid[0].length-1 && i != 0 && i != grid.length - 1){
                    res += grid[i-1][j] == 0 ? 1 : 0;
                    res += grid[i+1][j] == 0 ? 1 : 0;
                    res += grid[i][j-1] == 0 ? 1 : 0;
                    res += 1;
                //5.其他情况
                }else{
                    if(i - 1 >= 0)  res += grid[i-1][j] == 0 ? 1 : 0;
                    if(i + 1 < grid.length) res += grid[i+1][j] == 0 ? 1 : 0;
                    if(j - 1 >= 0)  res += grid[i][j-1] == 0 ? 1 : 0;
                    if(j + 1 < grid[0].length) res += grid[i][j+1] == 0 ? 1 : 0;
                }

            }
            
        }
        return res;
    }
}
```



```java
class Solution {
    static int[] dx = {0, 1, 0, -1};
    static int[] dy = {1, 0, -1, 0};

    public int islandPerimeter(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (grid[i][j] == 1) {
                    int cnt = 0; 
                    for (int k = 0; k < 4; ++k) {
                        int tx = i + dx[k];
                        int ty = j + dy[k];
                        if (tx < 0 || tx >= n || ty < 0 || ty >= m || grid[tx][ty] == 0) {
                            cnt += 1;
                        }
                    }
                    ans += cnt;
                }
            }
        }
        return ans;
    }
}

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









