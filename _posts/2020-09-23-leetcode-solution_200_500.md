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

[209.寻找长度最小的子数组](#jump209)

[214.最短回文串](#jump214)

[215.数组中的第K大元素](#jump215)

[216.组合总和3](#jump216)

[226.翻转二叉树](#jump226)

[235.二叉树的最近公共祖先](#jump235)

[257.二叉树的所有路径](#jump257)

[309.最佳买卖股票时机含冷冻期](#jump309)

[332.重新安排行程](#jump332)

[336.回文对](#jump336)

[337.打家劫舍3](#jump337)

[347.前K个高频元素](#jump347)

[378.有序矩阵中第K小元素](#jump378)

[404.左叶子之和](#jump404)

[416.分割等和子集](#jump416)

[459.重复的子字符串](#jump459)

[486.预测赢家](#jump486)

[491.递增子序列](#jump491)







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





<span id = "jump226"></span>



## 226.翻转二叉树

翻转一棵二叉树。

示例：

输入：

```java
		 4
   /   \
  2     7
 / \   / \
1   3 6   9
```


输出：

```java
		 4
   /   \
  7     2
 / \   / \
9   6 3   1
```



备注:
这个问题是受到 Max Howell 的 原问题 启发的 ：

> 谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。

立即推--->白板撸代码无用！

递归交换：

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null)    return null;
        invert(root);
        return root;
    }

    void invert(TreeNode root){
        if(root == null)
            return;
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        invert(root.left);
        invert(root.right);
    }
}
```

迭代交换：

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null)    return null;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
      	//层序遍历
        while(!queue.isEmpty()){
            int size = queue.size();

            for(int i = 0; i < size; ++i){
                TreeNode cur = queue.poll();
                //交换子节点
                TreeNode tmp = cur.left;
                cur.left = cur.right;
                cur.right = tmp;
                if(cur.right != null)   queue.offer(cur.right);
                if(cur.left != null)   queue.offer(cur.left);
            }
        }
        return root;
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









<span id="jump347"></span>

## 347.前 K 个高频元素

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

 

示例 1:

```java
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

示例 2:

```java
输入: nums = [1], k = 1
输出: [1]
```


提示：

* 你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
* 你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
* 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
* 你可以按任意顺序返回答案。



先统计所有数出现的次数，放到一个map里，再创建一个有序集合，将map中的key按value的大小降序放入，取出有序集合的前k个元素，即为答案。

需要注意的是，定制TreeSet比较器的时候，当两个元素相等时，不能返回0，否则不会将相等的元素放入，要返回1，就默认后加入的元素放在了前面（因为是降序排序的，升序排序就会放在后面）。

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();

        //先统计所有数出现的次数，放到一个map里
        for(int i = 0; i < nums.length; ++i){
            map.put(nums[i].getOrDefault(nums[i],0)+1)
        }
        //再创建一个有序集合，定制比较器。
        Set<Integer> ts = new TreeSet(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
              	//相等时返回1，其实返回-1也是可以的
                if(map.get(o1) == map.get(o2)){
                    return 1;
                }
                return map.get(o2) - map.get(o1);
            }
        });
	      //将map中的key按value的大小降序放入
        ts.addAll(map.keySet());
        Iterator<Integer> iterator = ts.iterator();
        int[] res = new int[k];
        int n = 0;
        //取出有序集合的前k个元素，即为答案
        while(iterator.hasNext() && n < k){
            res[n++] = iterator.next();
        }
        return res;
    }
}
```

也可以用一个数组来替代有序集合：

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();

        //先统计所有数出现的次数，放到一个map里
        for(int i = 0; i < nums.length; ++i){
            map.put(nums[i],map.getOrDefault(nums[i],0)+1);
        }
        int len = map.size();
      	int[][] num = new int[len][2];
      	int n = 0;
      	for(int key : map.keySet()){
          	num[n][0] = key;
          	num[n++][1] = map.get(key);
        }
      	Arrays.sort(num,(a,b) -> b[1]-a[1]);
	      
        int[] res = new int[k];
        for(int i = 0; i < k;++i){
          res[i] = num[i][0];
        }
        return res;
    }
}
```

用map+堆：

在小顶堆中存放目前为止堆元素出现次数为前k大的。如果小顶堆元素个数小于k，就直接插入，否则插入时判断出现次数是否大于堆顶元素，大于就删除堆顶元素，然后执行插入。

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> occurrences = new HashMap<Integer, Integer>();
        for (int num : nums) {
            occurrences.put(num, occurrences.getOrDefault(num, 0) + 1);
        }

        // int[] 的第一个元素代表数组的值，第二个元素代表了该值出现的次数
        PriorityQueue<int[]> queue = new PriorityQueue<int[]>(new Comparator<int[]>() {
            public int compare(int[] m, int[] n) {
                return m[1] - n[1];
            }
        });
        for (Map.Entry<Integer, Integer> entry : occurrences.entrySet()) {
            int num = entry.getKey(), count = entry.getValue();
            if (queue.size() == k) {
                if (queue.peek()[1] < count) {
                    queue.poll();
                    queue.offer(new int[]{num, count});
                }
            } else {
                queue.offer(new int[]{num, count});
            }
        }
        int[] ret = new int[k];
        for (int i = 0; i < k; ++i) {
            ret[i] = queue.poll()[0];
        }
        return ret;
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





<span id = "jump416"></span>

## 416.分割等和子集

给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意:

每个数组中的元素不会超过 100
数组的大小不会超过 200
示例 1:

```java
输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
```

示例 2:

```java
输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.
```





动态规划：

```java
class Solution {
    public boolean canPartition(int[] nums) {

        if(nums.length == 1)    return false;
        int sum  = 0;
        int maxNum = nums[0];

        for(int i = 0; i < nums.length; ++i){
            sum += nums[i];
            maxNum = Math.max(maxNum,nums[i]);
        }

        //和不能被2整除，肯定false
        if(sum % 2 != 0)    return false;
        int target = sum  /2;
        //最大元素比总和的一半都大，肯定不能平分
        if(maxNum > target) return false;

        boolean[][] dp = new boolean[nums.length][target + 1];

        //dp[i][j] 表示是否能够从下标为0~i的数组元素中选取若干个元素时，和为j。

        
        for(int i = 0; i < nums.length; ++i){
            dp[i][0] = true;
        }
        dp[0][nums[0]] = true;

        for(int i = 1; i < nums.length; ++i){
            for(int j = 1; j <= target; ++j){
                
                if(j < nums[i]){
                    //当j < nums[i]时，肯定不能选nums[i]，所以dp[i][j]只能由dp[i-1][j]决定
                    dp[i][j] = dp[i-1][j];
                }else{
                    //当j >= nums[i]时，nums[i]可选可不选
                    //如果选了，就取决于dp[i-1][j-nums[i]]是否为true
                    //如果不选，就取决于d[i-1][j]
                    dp[i][j] = dp[i-1][j] || dp[i-1][j - nums[i]];
                }
            }
        }

        return dp[nums.length - 1][target];
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



