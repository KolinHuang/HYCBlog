---
title: Leetcode题解:Hot 100!
author: Kol Huang
date: 2020-10-23 13:51:00 +0800
categories: [Blogging, leetcode]
tags: [算法题解]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/leetcode_cover.jpg
pin: true

---





## 目录



[1.两数之和](#jump1)

[2.两数相加](#jump2)

[4. 寻找两个正序数组的中位数](#jump4)

[5.最长回文子串](#jump5)

[10. 正则表达式匹配](#jump10)

[11.盛最多水的容器](#jump11)

[15.三数之和](#jump15)

[17.电话号码的字母组合](#jump17)

[19.删除链表的倒数第N个节点](#jump19)

[20.有效的括号](#jump20)

[21.合并两个有序链表](#jump21)

[22.括号生成](#jump22)

[31.下一个排列](#jump31)

[33.搜索旋转排序数组](#jump33)

[34.[重要]在排序数组中查找元素的第一个和最后一个位置](#jump34)

[39.组合总和](#jump39)

[46. 全排列](#jump46)

[48.旋转图像](#jump48)

[49.字母异位词分组](#jump49)

[53.最大子序和](#jump53)

[55. 跳跃游戏](#jump55)

[62.不同路径](jump62)

[64.最小路径和](#jump64)

[70. 爬楼梯](#jump70)

[75.颜色分类](jump75)

[78.子集](jump78)

[79.单词搜索](#jump79)

[86.分隔链表](#jump86)

[94.二叉树的中序遍历](#jump94)

[96. 不同的二叉搜索树](#jump96)

[98.验证二叉搜索树](#jump98)

[101.对称二叉树](#jump101)

[105. 从前序与中序遍历序列构造二叉树](#jump105)

[114.二叉树展开为链表](#jump114)

[121.买卖股票的最佳时机](#jump121)

[122.买卖股票的最佳时机 II](#jump122)

[136.只出现一次的数字](#jump136)

[141.环形链表](#jump141)

[142.环形链表II](#jump142)

[146.LRU 缓存机制](#jump146)

[148.排序链表](#jump148)

[200.岛屿数量](#jump200)

[215.数组中的第k大元素](#jump215)

[226.翻转二叉树](#jump226)

[234.回文链表](jump234)

[238.除自身以外数组的乘积](#jump238)

[239.滑动窗口最大值](#jump239)

[240.搜索二维矩阵 II](#jump240)

[279.完全平方数](#jump279)

[283.移动零](#283)

[287.寻找重复数](#jump287)

[300.最长上升子序列](#jump300)

[309.最佳买卖股票时机含冷冻期](#jump309)

[337. 打家劫舍 3](#jump337)

[347.前 K 个高频元素](#jump347)

[394.字符串解码](#jump394)

[406.根据身高重建队列](#jump406)

[416.分割等和子集](jump416)

[448.找到所有数组中消失的数字](#jump448)

[494.目标和](jump494)

[538.把二叉搜索树转换为累加树](#jump538)

[560.和为K的子数组](#jump560)

[581.最短无序连续子数组](#jump581)

[617.合并二叉树](#jump617)

[621.任务调度器](#jump621)

[647.回文子串](#jump647)

<span id = "jump1"></span>

## 1.两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

 

示例:

```java
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



哈希法

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {

        Map<Integer, Integer> hashTable = new HashMap<>();

        //遍历每个nums[i]
        for(int i = 0; i < nums.length; ++i){
            //查询哈希表中是否存在target-nums[i]项，如果存在就找到了答案
            if(hashTable.containsKey(target - nums[i])){
                return new int[]{hashTable.get(target - nums[i]),i};
            }
            //否则将nums[i]放入哈希表
            hashTable.put(nums[i],i);
        }
        return new int[2];
    }
}
```







## 2.两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

```java
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```



```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    
        int carry = 0;

        ListNode head = new ListNode(0);
        ListNode cur = head;

        //当两个链表都还没到尾部时，按位相加
        while(l1 != null && l2 != null){
            ListNode node = new ListNode();
            //有进位
            if(l1.val + l2.val + carry > 9){
                node.val = l1.val + l2.val + carry - 10;
                carry = 1;
            }else{
                //无进位
                node.val = l1.val + l2.val + carry;
                carry = 0;
            }
            cur.next = node;
            cur = cur.next;
            l1 = l1.next;
            l2 = l2.next;
        }
        //当l1还有剩余的位时
        while(l1 != null){
            ListNode node = new ListNode();
            //如果有进位
            if(carry == 1){
                if(l1.val + carry > 9){
                    
                    node.val = 0;
                    carry = 1;
                }else{
                    
                    node.val = l1.val + carry;
                    carry = 0;
                }
            //如果没有进位
            }else{
                node.val = l1.val;
            }
            cur.next = node;
            cur = cur.next;
            l1 = l1.next;
        }

        //同样处理l2
        while(l2 != null){
            ListNode node = new ListNode();
            if(carry == 1){
                if(l2.val + carry > 9){
                    
                    node.val = 0;
                    carry = 1;
                }else{
                    
                    node.val = l2.val + carry;
                    carry = 0;
                }
            }
            else{
                node.val = l2.val;
            }
            cur.next = node;
            cur = cur.next;
            l2 = l2.next;
        }

        //最后还有一个进位，就增加一个节点，值为1
        if(carry == 1){
            ListNode node = new ListNode(1);
            cur.next = node;
        }

        return head.next;


    }
}
```



官方的简便写法：

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = null, tail = null;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            int sum = n1 + n2 + carry;
            if (head == null) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail.next = new ListNode(sum % 10);
                tail = tail.next;
            }
            carry = sum / 10;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        if (carry > 0) {
            tail.next = new ListNode(carry);
        }
        return head;
    }
}
```









## 3.无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

```java
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例 2:

```java
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例 3:

```java
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```



滑动窗口：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        char[] arr = s.toCharArray();
        int n = arr.length;
        if(n <= 1) return n;

        Set<Character> set = new HashSet<>();
        int res = 0;
        int start = 0;
        int end = 0;
        //滑动窗口，探索每个以start开始的子串
        //假设我们找到了当前最长的某个无重复子串：
        //即[start,end)都没有重复字符，那么[start+1,end)肯定也是无重复字符的
        //所以可以在此基础上继续增大end，探索更长的无重复子串
        while(start < n && end < n){
            while(end < n && !set.contains(arr[end])){
                set.add(arr[end]);
                end++;
            }
            res = Math.max(res, end - start);
            set.remove(arr[start]);
            start++;
            
        }

        return res;
    }
}
```

```go
func lengthOfLongestSubstring(s string) int {
    n := len(s);
    if n < 2 {
        return n;
    }
    //用map定义一个set
    set := map[byte]int{}
    start := 0
    end := 0
    res := 0
    
    for start < n && end < n {
        //判断set中有没有s[end]这个元素
        for end < n && set[s[end]] == 0 {
            //把s[end]放入set
            set[s[end]]++
            end++
        }
        res = max(res, end - start)
        //从set中删除s[start]
        delete(set, s[start])
        start++
    }
    return res
}

func max(a int, b int) int {
    if a > b {
        return a
    }else{
        return b
    }
}
```







<span id = "jump4"></span>

## 4. 寻找两个正序数组的中位数

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。

进阶：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？

 

示例 1：

```java
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

示例 2：

```java
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

示例 3：

```java
输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000
```

示例 4：

```java
输入：nums1 = [], nums2 = [1]
输出：1.00000
```

示例 5：

```java
输入：nums1 = [2], nums2 = []
输出：2.00000
```


提示：

* nums1.length == m
* nums2.length == n
* 0 <= m <= 1000
* 0 <= n <= 1000
* 1 <= m + n <= 2000
* -106 <= nums1[i], nums2[i] <= 106





用二分才能达到这样的时间复杂度

```java
/* 主要思路：要找到第 k (k>1) 小的元素，那么就取 pivot1 = nums1[k/2-1] 和 pivot2 = nums2[k/2-1] 进行比较
* 这里的 "/" 表示整除
* nums1 中小于等于 pivot1 的元素有 nums1[0 .. k/2-2] 共计 k/2-1 个
* nums2 中小于等于 pivot2 的元素有 nums2[0 .. k/2-2] 共计 k/2-1 个
* 取 pivot = min(pivot1, pivot2)，两个数组中小于等于 pivot 的元素共计不会超过 (k/2-1) + (k/2-1) <= k-2 个
* 这样 pivot 本身最大也只能是第 k-1 小的元素
* 如果 pivot = pivot1，那么 nums1[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums1 数组
* 如果 pivot = pivot2，那么 nums2[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums2 数组
* 由于我们 "删除" 了一些元素（这些元素都比第 k 小的元素要小），因此需要修改 k 的值，减去删除的数的个数
*/
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        
        int len = m + n;
        //总长为奇数
        if(len % 2 == 1){
            //中位数在合并数组中的的索引为mid
            int mid = len / 2;
            //找到两个数组中第mid+1小的数就是中位数
            return getKthElement(nums1, nums2, mid + 1);
        }else{
            int mid1 = len / 2 -1, mid2 = len / 2;
            return (getKthElement(nums1, nums2, mid1+1) + getKthElement(nums1, nums2, mid2+1))*1.0/2;
        }
    }
    //找到两个数组中第k小的数
    public int getKthElement(int[] nums1, int[] nums2, int k){
        int m = nums1.length;
        int n = nums2.length;

        int cur1 = 0, cur2 = 0;


        while(true){
            //超出数组边界了，就从另外一个数组中找第k小的数直接返回
            if(cur1 == m){
                return nums2[cur2 + k - 1];
            }
            if(cur2 == n){
                return nums1[cur1 + k - 1];
            }
            //两个都没有超出边界，且k == 1就从两个数组的当前首索引找出一个最小值返回
            if(k == 1){
                return Math.min(nums1[cur1], nums2[cur2]);
            }

            //k > 1，且两个数组都未越界
            int half = k / 2;
            //本来是比较 nums1[k/2-1]和nums2[k/2-1]的值的
            //但是k/2-1可能会越界，如果越界了就取数组最后一个元素比较
            int newCur1 = Math.min(cur1 + half, m) - 1;
            int newCur2 = Math.min(cur2 + half, n) - 1;

            if(nums1[newCur1] <= nums2[newCur2]){
                //更新k，排除了多少个数，就把k缩小多少
                k -= (newCur1 - cur1 + 1);
                cur1 = newCur1 + 1;
            }else{
                k -= (newCur2 - cur2 + 1);
                cur2 = newCur2 + 1;
            }

        }
    }
}
```









<span id="jump5"></span>

## 5.最长回文子串

[点这里跳转](HYCBlog/posts/algorithm-manacher)





<span id = "jump10"></span>

## 10. 正则表达式匹配

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。

示例 1：

```java
输入：s = "aa" p = "a"
输出：false
解释："a" 无法匹配 "aa" 整个字符串。
```


示例 2:

```java
输入：s = "aa" p = "a*"
输出：true
解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

示例 3：

```java
输入：s = "ab" p = ".*"
输出：true
解释：".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

示例 4：

```java
输入：s = "aab" p = "c*a*b"
输出：true
解释：因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```

示例 5：

```java
输入：s = "mississippi" p = "mis*is*p*."
输出：false
```

提示：

* 0 <= s.length <= 20
* 0 <= p.length <= 30
* s 可能为空，且只包含从 a-z 的小写字母。
* p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。
* 保证每次出现字符 * 时，前面都匹配到有效的字符



```java
class Solution {
    public boolean isMatch(String s, String p) {
        //dp[i][j]标识s的前i个字符和p的前j个字符是否能够匹配
        //  1.如果p的第j个字符是一个小写字母，那么必须在s中匹配一个相同的小写字母
        //  2.如果p的第j个字符是'.'，那么p[j]一定匹配成功s中的任意一个小写字母
        //  3.如果p的第j个字符是'*'，那么就可以对p的第j-1个字符匹配任意次
        //      字母+*的组合在匹配过程中本质只有两种情况：
                    //1.匹配s末尾的一个字符，将该字符丢掉，而该组合还可以继续匹配
                    //2.不匹配字符，将该组合扔掉，不再进行匹配
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m+1][n+1];
        //空配空
        dp[0][0] = true;
        //边界情况：dp[0]表示用p匹配空字符串的情况
        for(int j = 1; j <= n; ++j){
            if(p.charAt(j-1) == '*')    dp[0][j] = dp[0][j-2];
        }

        for(int i = 1; i <= m; ++i){
            for(int j = 1; j <= n; ++j){
                //如果当前p字符是'*'
                if(p.charAt(j-1) == '*'){
                    //如果s[i] = p[j-1]
                    if(matches(s, p, i, j-1)){
                        //不妨把类似 'a*', 'b*' 等的当成整体看待。
                        //可以只把 i 前移一位，而不丢弃 'b*', 转化为子问题 dp[i-1][j]
                        //反过来说，就是只要dp[i-1][j]成立，那么dp[i][j]就成立，因为s[i] = p[j-1]
                        //当然也可以丢弃'a*'这样的整体，就需要把 j 前移2位，转化为子问题dp[i][j-2]
                        //反过来说，就是只要dp[i][j-2]成立，那么dp[i][j]就成立，忽略'a*'整体
                        dp[i][j] = dp[i][j-2] || dp[i-1][j];
                    }else{
                        //s[i] != p[j-1]只能选择不匹配
                        dp[i][j] = dp[i][j-2];
                    }
                }else{
                    if(matches(s, p, i, j)){
                        dp[i][j] = dp[i-1][j-1];
                    }
                }
            }
        }
        return dp[m][n];
        
    }

    //判断对应位置上的字符是否匹配
    boolean matches(String s, String p, int i, int j){
        if(p.charAt(j-1) == '.'){
            return true;
        }
        return s.charAt(i-1) == p.charAt(j-1);
    }
}
```









<span id = "jump11"></span>

## 11.盛最多水的容器

给你 n 个非负整数 `a1，a2，...，an`，每个数代表坐标中的一个点 `(i, ai)` 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 `(i, ai)` 和` (i, 0)` 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。

暴力枚举+剪枝优化：

```java
class Solution {
    public int maxArea(int[] height) {
        int max = 0;
        int max_l = 0;
        int max_h = 0;
        for(int i = 0; i <height.length; ++i){
            max_h = Math.max(max_h, height[i]);
        }

        for(int i = 0; i < height.length-1; ++i){
            if((height.length - i) * max_h <= max){
                break;
            }
            //剪枝（1）
            if(max_l < height[i]){
                int max_r = 0;
                for(int j = height.length - 1; j > i; --j){
                    //剪枝（2）
                    if((j - i) * height[i] <= max){
                        break;
                    }
                    //剪枝（3）
                    if(max_r < height[j]){
                        max = Math.max(max, Math.min(height[i],height[j]) * (j - i));
                        max_r = height[j];
                    }
                    
                }
                max_l = height[i];
            }
            
        }
        return max;
    }
}
```

双指针内缩短板，一句话理解：只有不断克服短板，才能增加成功的可能性

```java
class Solution {
    public int maxArea(int[] height) {
        int res = 0;
        //双指针指向两端，不断将短板内缩，因为只有将短板内缩才有可能把短板增大，容量才有可能增大
        int l = 0, r = height.length - 1;
        while(l < r){
            res = Math.max(res, Math.min(height[l],height[r]) * (r - l));
            if(height[l] >= height[r]){
                r--;
            }else{
                l++;
            }
        }
        return res;
    }
}
```



golang

```go
func maxArea(height []int) int {
    res := 0

    l,r := 0, len(height) - 1
    for l < r {
        res = max(res, min(height[l],height[r]) * (r - l))
        if height[l] >= height[r] {
            r--
        }else{
            l++
        }
    }
    return res
}


func max(a int, b int) int {
    if a >= b {
        return a
    }else{
        return b
    }
}

func min(a int, b int) int {
    if a >= b {
        return b
    }else{
        return a
    }
}
```









<span id = "jump15"></span>

## 15.三数之和

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

示例：

```java
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```



```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        List<List<Integer>> res = new ArrayList<>();
        if(n <= 2)  return res;

        Arrays.sort(nums);

        for(int i = 0; i < n-2; ++i){
            //剪枝：nums[i]太小了，即使最大的数字*2加起来，都没办法使其大于等于0
            if(nums[i] + nums[n-1]*2 < 0)   continue;
            //剪枝：nums[i]大于0了，后面再加肯定都是大于0的
            if(nums[i] > 0) break;
            //去重
            if(i > 0 && nums[i] == nums[i-1])   continue;
            //双指针
            //让指针right指向最右端
            int r = n-1;
            int target = -nums[i];

            for(int l = i + 1; l < n - 1; ++l){
                if(nums[i]+nums[l]>0)   break;
                //去重
                if(l > i + 1 && nums[l] == nums[l-1])   continue;
                //右指针向左移动,缩小nums[l] + nums[r]
                while(l < r && nums[l] + nums[r] > target){
                    --r;
                }
                //指针重合了，那么l继续增加也没有意义了，因为最小的nums[l] + nums[r]都大于target了
                if(l == r){
                    break;
                }

                if(nums[l] + nums[r] == target){
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[l]);
                    list.add(nums[r]);
                    res.add(list);
                }
            }
        }

        return res;
    }
}
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







<span id = "jump19"></span>

## 19.删除链表的倒数第N个节点

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

```java
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```


说明：

* 给定的 n 保证是有效的。

进阶：

* 你能尝试使用一趟扫描实现吗？

双指针移动，先让一个指针移动n位，再让两个指针同时移动，这样当后一个指针到头时，前一个指针就指向倒数第n个节点。

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head == null)    return null;
        if(n == 0)  return head;
      	//用一个伪头节点降低代码复杂度
        ListNode fake_head = new ListNode(-1);
        fake_head.next = head;
      	//指向需要被删除节点的前一个节点
        ListNode pre = fake_head;
      	//指向需要被删除的节点
        ListNode left = head;
        ListNode right = head;
        while(right != null){
            if(n != 0){
              	//先让right指针移动n位
                right = right.next;
                n--;
            }else{
                pre = pre.next;
                left = left.next;
                right = right.next;
            }
        }
      	//删除节点
        pre.next = left.next;
      	//help GC 
        left.next = null;
        return fake_head.next;
    }
}
```









<span id = "jump20"></span>

## 20.有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

```java
输入: "()"
输出: true
```


示例 2:

```java
输入: "()[]{}"
输出: true
```

示例 3:

```java
输入: "(]"
输出: false
```

示例 4:

```java
输入: "([)]"
输出: false
```

示例 5:

```java
输入: "{[]}"
输出: true
```

```java
class Solution {
    public boolean isValid(String s) {
        if(s.length() == 0) return true;

        if(s.length() % 2 != 0) return false;

        Stack<Character> stack = new Stack<>();
        for(int i = 0; i < s.length(); ++i){
            if(s.charAt(i) == '(' || s.charAt(i) == '{' || s.charAt(i) == '[')
                stack.push(s.charAt(i));
            else{
                if(stack.size() == 0)
                    return false;
                Character c = stack.pop();
                if(s.charAt(i) == ')' && c != '(')
                    return false;
                else if(s.charAt(i) == '}' && c != '{')
                    return false;
                else if(s.charAt(i) == ']' && c != '[')
                    return false;
            }
        }
        return stack.size()==0;
    }
}
```







<span id = "jump21"></span>

## 21.合并两个有序链表

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

示例：

```java
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode cur = head;
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
                cur.next = l1;
                l1 = l1.next;
            }else{
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
      	//直接接在后面即可
        cur.next = l1 == null ? l2 : l1;
        return head.next;
    }
}
```







<span id = "jump22"></span>

## 22.括号生成

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

 

示例：

```java
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

回溯法，判断是排左括号还是右括号，当左右括号都排完了，就记录。

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        if(n == 0)  return res;
        dfs(new StringBuilder(), n, n);
        return res;
    }

    void dfs(StringBuilder sb, int left, int right){
        //左右括号都排完了
        if(left == 0 && right == 0){
            res.add(sb.toString());
            return;
        }
        //左右括号数量相等，只能排左括号
        if(left == right){
            sb.append('(');
            dfs(sb, left - 1, right);
          	//回溯
            sb.deleteCharAt(sb.length()-1);
        }else{
            //左括号排完了，只能排右括号
            if(left == 0){
                sb.append(')');
                dfs(sb, left, right - 1);
              	//回溯
                sb.deleteCharAt(sb.length()-1);
            }else{
                //左右括号都可以排
                sb.append('(');
                dfs(sb, left - 1, right);
                //回溯
                sb.deleteCharAt(sb.length()-1);
                sb.append(')');
                dfs(sb, left, right - 1);
	              //回溯
                sb.deleteCharAt(sb.length()-1);
            }
        }
    }
}
```









## 23. 合并K个升序链表

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

示例 1：

```java
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

示例 2：

```java
输入：lists = []
输出：[]
示例 3：

输入：lists = [[]]
输出：[]
```


提示：

* k == lists.length
* 0 <= k <= 10^4
* 0 <= lists[i].length <= 500
* -10^4 <= `lists[i][j]` <= 10^4
* lists[i] 按 升序 排列
* lists[i].length 的总和不超过 10^4



链表数组的归并排序

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0)   return null;
        return merge_sort(lists,0 ,lists.length - 1);
    }
    //归并排序
    ListNode merge_sort(ListNode[] lists, int l, int r){
        if(r == l){
            return lists[l];
        }

        int mid = ((r - l) >> 1) + l;
        
        ListNode left = merge_sort(lists, l, mid);
        ListNode right = merge_sort(lists, mid + 1, r);

        //合并left链表和right链表
        ListNode cur_l = left;
        ListNode cur_r = right;
        ListNode mList = new ListNode();
        ListNode cur = mList;
        while(cur_l != null && cur_r != null){
            if(cur_l.val < cur_r.val){
                cur.next = cur_l;
                cur_l = cur_l.next;
            }else{
                cur.next = cur_r;
                cur_r = cur_r.next;
            }
            cur = cur.next;
        }
        
        cur.next = cur_l != null ? cur_l : cur_r;

        return mList.next;
    }
}
```











<span id = "jump31"></span>

## 31.下一个排列

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。

```java
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```



观察变化规律，每次都是从数组结尾开始找到一个逆序对，即`nums[i] <= nums[i+1]`的元素，然后再从数组结尾开始找到第一个比`nums[i]`大的，二者交换，再反转`[i+1,end]`

```java
1,2,3,4,5
1,2,3,5,4
1,2,4,3,5
1,2,4,5,3
1,2,5,3,4
1,2,5,4,3
1,3,2,4,5
...
```





```java
class Solution {
    public void nextPermutation(int[] nums) {
        if(nums.length == 1)    return;        
        int l = 0;
        int r = nums.length - 2;
        //让r指针从右往左走，直到下一个元素比num[r]小，这个位置的元素需要换到后面来增大字典序
        //所以还需要从后面找到一个比这个位置元素大的元素，二者交换
        while(r >= 0 && nums[r] >= nums[r + 1]){
            r--;
        }
        //如果r == -1，就直接reverse全部
        if(r >= 0){
            //从后面的序列中，找到第一个比nums[r]大的元素，换到前面去
            int cur = nums.length - 1;
            while(cur >= 0 && nums[cur] <= nums[r]){
                --cur;
            }
            swap(nums, cur, r);
        }
        //将逆序序列变为正序
        reverse(nums, r + 1);
        
    }

    void reverse(int[] nums, int s){
        int i = s, j = nums.length - 1;
        while(i < j){
            swap(nums, i ,j);
            ++i;
            --j;
        }
    }

    void swap(int[] nums, int i, int j){
        if(i != j){
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
    }

    
}
```











<span id = "jump33"></span>

## 33.搜索旋转排序数组

给你一个升序排列的整数数组 nums ，和一个整数 target 。

假设按照升序排序的数组在预先未知的某个点上进行了旋转。（例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] ）。

请你在数组中搜索 target ，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

示例 1：

```java
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

示例 2：

```java
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

示例 3：

```java
输入：nums = [1], target = 0
输出：-1
```


提示：

* 1 <= nums.length <= 5000
* -10^4 <= nums[i] <= 10^4
* nums 中的每个值都 独一无二
* nums 肯定会在某个点上旋转
* -10^4 <= target <= 10^4

二分法，判断target和nums[mid]在分段函数的位置来更新l和r指针。

```java
class Solution {
    public int search(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        while(l <= r){
            int mid = (r - l) / 2 + l;
            if(nums[mid] == target) return mid;
            if(nums[mid] < target){
              	//二者都在左分段
                if(nums[mid] >= nums[0]){
                    l = mid+1;
                }else{
                  	//二者都在右分段
                    if(target < nums[0]){
                        l = mid + 1;    
                    //mid在右分段，target在左分段
                    }else{
                        r = mid-1;
                    }
                }
            }else{
                if(nums[mid] < nums[nums.length - 1]){
                    r = mid-1;
                }else{
                    if(target >= nums[0]){
                        r = mid - 1;
                    }else{
                        l = mid + 1;
                    }
                }
            }
        }
        return -1;
    }
}
```









<span id = "jump34"></span>

## 34.[重要]在排序数组中查找元素的第一个和最后一个位置

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

* 你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

示例 1：

```java
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

示例 2：

```java
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

示例 3：

```java
输入：nums = [], target = 0
输出：[-1,-1]
```


提示：

* 0 <= nums.length <= 105
* -109 <= nums[i] <= 109
* nums 是一个非递减数组
* -109 <= target <= 109



```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        //二分查找左边界和右边界的问题
        int n = nums.length;
        if(n == 0)  return new int[]{-1,-1};
        //查找左边界
        int lower = binarySearch(nums,target,0,n-1,true);
        

        //查找右边界
        int upper = binarySearch(nums,target, 0,n-1, false) - 1;
        
        if(lower <= upper && upper < nums.length && nums[lower] == target && nums[upper] == target){
            return new int[]{lower,upper};
        }

        return new int[]{-1,-1};
    }
    //lower==true表示找到左边界，否则就是右边界
    public int binarySearch(int[] nums, int target, int l, int r, boolean lower){
        int ans = nums.length;
        while(l <= r){
            int mid = (l + r) / 2;

            if(nums[mid] > target || (lower && nums[mid] >= target)){
                r = mid-1;
                ans = mid;
            }else{
                l = mid+1;
            }
        }
        return ans;
    }
}
```







<span id = "jump39"></span>

## 39.组合总和

给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。

说明：

* 所有数字（包括 target）都是正整数。

* 解集不能包含重复的组合。 

  

示例 1：

```java
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
```


示例 2：

```java
输入：candidates = [2,3,5], target = 8,
所求解集为：
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```



### 回溯

```java
class Solution {
    int target;
    List<List<Integer>> res;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        res = new ArrayList<>();
        if(candidates.length==0) return res;
        this.target = target;
        dfs(candidates,new ArrayList<>(),0,0);
        return res;
    }

    void dfs(int[] candidates, List<Integer> list, int sum,int cur){
        if(sum > target){
            return;
        }
        if(sum == target){
            List<Integer> ans = new ArrayList<>(list);
            if(!res.contains(ans))
                res.add(ans);
            return;
        }

        for(int i = cur; i < candidates.length; ++i){
            list.add(candidates[i]);
            dfs(candidates,list,sum + candidates[i],i);
            list.remove(list.size()-1);
        }
    }
}
```







<span id = "jump46"></span>

## 46. 全排列

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

```java
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```



回溯交换：

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> permute(int[] nums) {
        res = new ArrayList<>();
        backTrack(nums, 0);
        return res;
    }

    void backTrack(int[] nums,int cur){
        if(cur == nums.length){
            List<Integer> list = new ArrayList<>();
            for(int k : nums){
                list.add(k);
            }
            res.add(list);
            return;
        }   
        for(int i = cur; i < nums.length; ++i){
            swap(nums, cur, i);
            backTrack(nums, cur + 1);
            swap(nums, cur, i);
        }
    }
    void swap(int[] nums, int i, int j){
        if(i != j){
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
    }
}
```







<span id = "jump48"></span>

## 48.旋转图像

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

示例 1:

```java
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```


示例 2:

```java
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```



```java
class Solution {
    public void rotate(int[][] matrix) {
        //先转置，再按列交换
        int m = matrix.length;
        int n = matrix[0].length;
        for(int i = 0; i < m; ++i){
            for(int j = i; j < n; ++j){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
            }
        }

        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n / 2; ++j){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[i][n - j - 1];
                matrix[i][n - j - 1] = tmp;
            }
        }

    }
}
```







<span id = "jump49"></span>

## 49.字母异位词分组

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

示例:

```java
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```


说明：

* 所有输入均为小写字母。
* 不考虑答案输出的顺序。

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        //用哈希法找异位词
        //两个互为异位词，则哈希数组肯定相同
        //我们可以把哈希数组存为一个key值，然后再遍历每个字符串
        Map<String, List> map = new HashMap<>();

        int[] counts = new int[26];
        for(int i = 0; i < strs.length; ++i){
            char[] arr = strs[i].toCharArray();
            Arrays.fill(counts,0);

            for(char c : arr){
                counts[c - 'a']++;
            }

            StringBuilder sb = new StringBuilder();

            for(int j = 0; j < 26; ++j){
                //用分隔符隔开，这样可以防止11和1、1混在一起
                sb.append('*');
                sb.append(counts[j]);
            }
            String key = sb.toString();
            if(!map.containsKey(key))   map.put(key, new ArrayList<>());
            map.get(key).add(strs[i]);
        }

        return new ArrayList(map.values());
        
    }

    
}
```







<span id = "jump53"></span>

## 53.最大子序和

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

```java
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```


进阶:

* 如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。



```java
class Solution {
  	//dp[i]表示以第i个元素为结尾的最大子序列和
  	//dp[i] = dp[i-1] > 0 ? dp[i-1] + nums[i] : nums[i];
    public int maxSubArray(int[] nums) {
        if(nums.length == 0)    return 0;

        if(nums.length == 1)    return nums[0];
        int dp = nums[0];
        int res = nums[0];
        for(int i = 1; i < nums.length; ++i){
            dp = dp > 0 ? nums[i] + dp : nums[i];
            res = Math.max(res, dp);
        }

        return res;
    }
}
```



分治：

```java
class Solution {
    //线段树解法：
    /*对于一个区间 [l, r][l,r]，我们可以维护四个量：
        lSum 表示 [l, r][l,r] 内以 ll 为左端点的最大子段和
        rSum 表示 [l, r][l,r] 内以 rr 为右端点的最大子段和
        mSum 表示 [l, r][l,r] 内的最大子段和
        iSum 表示 [l, r][l,r] 的区间和*/

    public class Status{
        public int lSum, rSum, mSum, iSum;

        public Status(int _lSum, int _rSum, int _mSum, int _iSum){
            lSum = _lSum;
            rSum = _rSum;
            mSum = _mSum;
            iSum = _iSum;
        }

        public int getMSum(){
            return this.mSum;
        }
    }
    
    public int maxSubArray(int[] nums) {
        return getInfo(nums, 0, nums.length-1).getMSum();
    }

    public Status getInfo(int[] nums, int l, int r){
        if(l == r){
            return new Status(nums[l],nums[l],nums[l],nums[l]);
        }

        int m = ((r - l) >> 1) + l;
        Status left = getInfo(nums, l, m);
        Status right = getInfo(nums, m+1, r);

        return pushUp(left, right);
    }

    public Status pushUp(Status s1, Status s2){
        
        int ls = Math.max(s1.lSum, s1.iSum + s2.lSum);
        
        int rs = Math.max(s2.rSum, s1.rSum + s2.iSum);

        int is = s1.iSum + s2.iSum;
        //合并区间的最大子序和可能是左区间的最大子序和或者右区间的最大子序和，
        //也可能跨越分界点m：左区间以右端点为结尾的的最大子序和+右区间以左端点为起始的最大子序和
        int ms = Math.max(Math.max(s1.mSum, s2.mSum), s1.rSum + s2.lSum);


        return new Status(ls, rs, ms, is);
    }



}
```

不仅可以解决区间` [0,n−1]`，还可以用于解决任意的子区间 `[l, r]` 的问题。如果我们把`[0,n−1] `分治下去出现的所有子区间的信息都用堆式存储的方式记忆化下来，即建成一颗真正的树之后，我们就可以在O(logn) 的时间内求到任意区间内的答案，我们甚至可以修改序列中的值，做一些简单的维护，之后仍然可以在O(logn) 的时间内求到任意区间内的答案，对于大规模查询的情况下，这种方法的优势便体现了出来。这棵树就是上文提及的一种神奇的数据结构——线段树。





<span id = "jump55"></span>

## 55. 跳跃游戏

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

示例 1:

```java
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```

示例 2:

```java
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```



```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;

        if(n <= 1)  return true;
        //维护一个maxStep，表示当前能够跳过的最大长度
        //只要这个小于等于连续0序列的长度，就没办法越过
        int maxStep = 0;
        int i = 0;
        while(i < n){
            maxStep = Math.max(nums[i], maxStep);

            if(nums[i] != 0){
                maxStep--;
                i++;
                continue;
            }
            //统计连续0序列的长度
            int cur = i;
            while(cur < n && nums[cur] == 0)
                cur++;
          	//到达了最后一个序列，那么最大长度可以只需大于等于连续0序列长度-1即可，最后一个0无所谓
            if(cur == n){
                if(maxStep >= (cur-i-1)){
                    return true;
                }else{
                    return false;
                }
            }
            if(maxStep < (cur - i))
                return false;
            maxStep -= (cur - i);
            i = cur;
        }

        return true;
    }
}
```

```java
public class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int rightmost = 0;
        for (int i = 0; i < n; ++i) {
            if (i <= rightmost) {
                rightmost = Math.max(rightmost, i + nums[i]);
                if (rightmost >= n - 1) {
                    return true;
                }
            }
        }
        return false;
    }
}
```









<span id = "jump62"></span>

## 62.不同路径

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

示例 1:

```java
输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。

1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
```

示例 2:

```java
输入: m = 7, n = 3
输出: 28
```


提示：

* 1 <= m, n <= 100
* 题目数据保证答案小于等于 2 * 10 ^ 9



简单的动态规划

```java
class Solution {
    public int uniquePaths(int m, int n) {
        //dp[i][j]表示，走到第(i,j)个网格有几种方法
        int[][] dp = new int[m+1][n+1];
      
        //初始条件
        //当j = 1时，只能由dp[i-1][j]向下走一步而来
        for(int j = 1; j <= n; ++j){
            dp[1][j] = 1;
        }
        //当i = 1时，只能由dp[i][j-1]向右走一步而来
        for(int i = 1; i <= m; ++i){
            dp[i][1] = 1;
        }
        //dp[i][j]的值可能由dp[i-1][j]向下走一步，或者dp[i][j-1]向右走一步而来
        for(int i = 2; i <= m; ++i){
            for(int j = 2; j <= n; ++j){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m][n];
    }
}
```





<span id = "jump64"></span>

## 64.最小路径和

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

```java
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

示例 2：

```java
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```


提示：

* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 200`
* `0 <= grid[i][j] <= 100`

```go
func minPathSum(grid [][]int) int {
    
    var m int = len(grid)
    var n int = len(grid[0])
  	//动态数组声明，太麻烦了吧
    dp := make([][]int, m)
    for i := 0; i < m; i++ {
        dp[i] = make([]int, n)
    }
    //边界初始化
    dp[0][0] = grid[0][0]
    for i := 1; i < m; i++ {
        dp[i][0] = dp[i-1][0] + grid[i][0]
    }

    for j := 1; j < n; j++ {
        dp[0][j] = dp[0][j-1] + grid[0][j]
    }

    for i := 1; i < m; i++ {
        for j:= 1; j < n; j++ {
            dp[i][j] = mymin(dp[i-1][j], dp[i][j-1]) + grid[i][j]
        }
    }
    return dp[m-1][n-1]
}
//还得自己写min，因为不支持函数重载，所以math包里只支持float64的min和max，美其名曰保持简洁干净？
func mymin(a int, b int) int{
    if a < b {
        return a
    }else{
        return b
    }
}
```

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        //dp[i][j]表示从grid[0][0]走到grid[i][j]的最小路径和
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        //边界初始化
        for(int j = 1; j < n; ++j){
            dp[0][j] = dp[0][j-1] + grid[0][j];
        }

        for(int i = 1; i < m; ++i){
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }

        for(int i = 1; i < m; ++i){
            for(int j = 1; j < n; ++j){
                dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }

        return dp[m-1][n-1];

    }
}
```



<span id = "jump70"></span>

## 70. 爬楼梯

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

```java
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。

1.  1 阶 + 1 阶
2.  2 阶
```

示例 2：

```java
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。

1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```



```java
class Solution {
    public int climbStairs(int n) {
        //斐波那契数列

        int a = 1, b = 1;
        for(int i = 2; i <= n; ++i){
            int tmp = a + b;
            a = b;
            b = tmp;
        }

        return b;
    }
}
```



进阶：不能连续爬两步，即有一次跳了2步，下一次就不能跳2步了

```java
维护dp[i][1]和dp[i][2]，表示
第i次爬楼跳1步可以从上一次爬楼跳1步而来，也可以从上一次爬楼跳2步而来
而第i次爬楼跳2步只能从上一次爬楼跳1步而来
dp[i][1] = dp[i-1][1] + dp[i-1][2];
dp[i][2] = dp[i-1][1]
```





<span id = "jump75"></span>

## 75.颜色分类

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

注意:
不能使用代码库中的排序函数来解决这道题。

示例:

```java
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```



进阶：

* 一个直观的解决方案是使用计数排序的两趟扫描算法。
  首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
* 你能想出一个仅使用常数空间的一趟扫描算法吗？





计数排序

```java
class Solution {
    public void sortColors(int[] nums) {
        int a = 0;
        int b = 0;
        int c = 0;
        for(int i = 0; i < nums.length; ++i){
            if(nums[i] == 0)    a++;
            if(nums[i] == 1)    b++;
            if(nums[i] == 2)    c++;
        }

        for(int i = 0; i < nums.length; ++i){
            if(a != 0){
                nums[i] = 0;
                a--;
            }else if(b != 0){
                nums[i] = 1;
                b--;
            }else{
                nums[i] = 2;
                c--;
            }
        }
    }    
}
```







一趟partition，把0都放到左边，1都放到右边，数字2放完之后，这个位置还需要被遍历一次，所以`i--`。

```java
class Solution {
    public void sortColors(int[] nums) {
        int low = 0;
        int high = nums.length-1;
        

        for(int i = 0; i < nums.length; ++i){
            if(i >= low && i <= high){
                if(nums[i] == 0){
                nums[i] = nums[low];
                nums[low++] = 0;
                }else if(nums[i] == 2){
                  	//交换过来的数字不确定，所以需要再判断一次这个位置
                    nums[i--] = nums[high];
                    nums[high--] = 2;
                }
            }   
        }
    }
}
```







<span id = "jump78"></span>

## 78.子集

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

输入: nums = [1,2,3]
输出:

```java
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

回溯的典型题。只需要遍历所有长度的子集，然后回溯长度为`len`的子集的所有可能性即可。

```java
class Solution {

    List<List<Integer>> res;
    public List<List<Integer>> subsets(int[] nums) {
        res = new ArrayList<>();
        //控制子集的长度
        for(int len = 0; len <= nums.length; ++len){
            dfs(nums,len,0,new ArrayList<Integer>());
        }
        return res;
    }


    void dfs(int[] nums,int len, int cur,List<Integer> list){
        if(cur == len){
            res.add(new ArrayList<>(list));
            return;
        }

        //回溯添加长度为len的子集
        for(int i = cur; i < nums.length; ++i){
            list.add(nums[i]);
            dfs(nums,len,i+1,list);
            list.remove(list.size()-1);
        }
    }
}
```







<span id = "jump79"></span>

## 79.单词搜索



给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 示例:

```java
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
```


提示：

* board 和 word 中只包含大写和小写英文字母。
* 1 <= board.length <= 200
* 1 <= board[i].length <= 200
* 1 <= word.length <= 10^3



深搜

```java
class Solution {
    //boolean flag;
    boolean[][] visited;
    public boolean exist(char[][] board, String word) {
        visited = new boolean[board.length][board[0].length];
        for(int i = 0; i < board.length; ++i){
            for(int j = 0; j < board[0].length; ++j){
                if(board[i][j] == word.charAt(0)){
                    if(dfs(board,i,j,0,word)){
                        return true;
                    }
                  //if(flag)	return true;
                }
                    
            }
        }
        return false;
    }
    boolean dfs(char[][] board, int x, int y, int cur ,String word){
        //
        if(board[x][y] != word.charAt(cur)){
            return false;
        }
        if(cur == word.length()-1){
            return true;
          	//flag = true;
        }
        visited[x][y] = true;
        int[][] direction = {\{0,1},{0,-1},{1,0},{-1,0}\};

        //向四个方向搜索，若找到答案就返回true
        for(int i = 0; i < 4; ++i){
            int new_x = x + direction[i][0];
            int new_y = y + direction[i][1];
            
            //新位置的合法性判别
            if(new_x < board.length && new_y <board[0].length && new_x >= 0 && new_y >= 0
            && !visited[new_x][new_y]){
                if(dfs(board,new_x,new_y,cur+1,word)){
                    return true;
                }
              	//dfs(board,new_x,new_y,cur+1,word);
              	//if(flag) break;
            }
                
        }
        //回溯
        visited[x][y] = false;
        return false;
    }
}
```





<span id = "jump86"></span>

## 86.分隔链表

给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

 

示例:

```java
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
```



```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode fakeHead = new ListNode(0);
        fakeHead.next = head;
        //pre用于指向小于x节点的子链表的尾部，cur用于指向当前节点的前一个节点
        ListNode pre = fakeHead, cur = fakeHead;

        while(cur != null && pre != null){
            //越过所有大于等于x的节点
            while(cur.next != null && cur.next.val >= x){
                cur = cur.next;
            }
            //到达结尾了，结束遍历
            if(cur.next == null){
                break;
            }

            //越过一开始就在正确位置的小于x的节点
            if(cur == pre){
                cur = cur.next;
                pre = pre.next;
                continue;
            }
            //将小于x的节点插到前面去
            ListNode tmp = cur.next;
            cur.next = tmp.next;
            tmp.next = pre.next;
            pre.next = tmp;

            //向后移动指针
            pre = pre.next;

        }

        return fakeHead.next;
        
    }
}
```











<span id = "jump94"></span>

## 94.二叉树的中序遍历

给定一个二叉树，返回它的中序 遍历。

示例:

```java
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```


进阶: 递归算法很简单，你可以通过迭代算法完成吗？



```java
public List<Integer> inorderTraversal(TreeNode root) {
  	List<Integer> res = new ArrayList<>();
  	if(root == null)	return null;
  	//Stack<TreeNode> stack = new Stack<>();	用Deque比Stack快多了
  	Deque<TreeNode> stack = new LinkedList<>();
  	TreeNode cur = root;
  	while( cur != null || !stack.isEmpty()){
      	while(cur != null){
          	stack.push(cur);
          	cur = cur.left;
        }
      	cur = stack.pop();
      	res.add(cur.val);
      	cur = cur.right;
    }
  	return res;
}
```



[Morris中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/er-cha-shu-de-zhong-xu-bian-li-by-leetcode-solutio/)







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







<span id = "jump98"></span>

## 98.验证二叉搜索树

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
示例 1:

输入:

```java
    2
   / \
  1   3
输出: true
```


示例 2:

```java
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```



中序遍历，序列有序则符合要求。

```java
class Solution {
    TreeNode pre;
    boolean flag = true;
    public boolean isValidBST(TreeNode root) {
        pre = null;
        inOrder(root);

        return flag;
    }

    void inOrder(TreeNode root){
        if(root != null && flag == true){
            inOrder(root.left);
            if(pre != null && flag == true){
                flag = pre.val < root.val ? true : false;
                if(flag == false)
                    return;
            }
            pre = root;
            inOrder(root.right);
        }
    }
}
```





<span id = "jump105"></span>

## 105. 从前序与中序遍历序列构造二叉树

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

    		3
       / \
      9  20
        /  \
       15   7



可以解决有重复元素的树：

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length == 0)    return null;
        return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    TreeNode build(int[] preorder, int pS, int pE, int[] inorder, int iS, int iE){
        
        if(pE < pS){
            return null;
        }

        if(pS == pE){
            return new TreeNode(preorder[pS]);
        }
        
        TreeNode root = new TreeNode(preorder[pS]);
        
        int idx = iS;
        while(inorder[idx] != root.val){
            ++idx;
        }
        
        TreeNode left = build(preorder, pS + 1, pS + (idx - iS), inorder, iS, idx - 1);
        TreeNode right = build(preorder, pS + (idx - iS) + 1, pE, inorder, idx + 1, iE);

        root.left = left;
        root.right = right;
        return root;
    }
}
```

```java
class Solution {
    Map<Integer, Integer> inMap = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        //由于树中无重复元素，所以可以为每个元素建立哈希索引，后面可以直接在中序遍历序列中定位根节点
        for(int i = 0; i < inorder.length; ++i){
            inMap.put(inorder[i],i);
        }

        return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    TreeNode build(int[] preorder, int pS, int pE, int[] inorder, int iS, int iE){
        
        if(pE < pS){
            return null;
        }

        if(pS == pE){
            return new TreeNode(preorder[pS]);
        }
        
        TreeNode root = new TreeNode(preorder[pS]);
        
        int idx = inMap.get(root.val);
        
        TreeNode left = build(preorder, pS + 1, pS + (idx - iS), inorder, iS, idx - 1);
        TreeNode right = build(preorder, pS + (idx - iS) + 1, pE, inorder, idx + 1, iE);

        root.left = left;
        root.right = right;
        return root;
    }
}
```











<span id = "jump101"></span>

## 101.对称二叉树

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    		1
       / \
      2   2
     / \ / \
    3  4 4  3


但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    		1
       / \
      2   2
       \   \
       3    3


进阶：

你可以运用递归和迭代两种方法解决这个问题吗？

递归：

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return compare(root, root);
    }

    boolean compare(TreeNode root1, TreeNode root2){
        if(root1 == null && root2 == null)  return true;
        if(root1 == null || root2 == null)  return false;
        return root1.val == root2.val
        && compare(root1.left,root2.right)
        && compare(root1.right,root2.left);
    }
}
```

迭代：

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        //初始化队列
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        queue.offer(root);
      	//每次取两个节点出来比较
        while(!queue.isEmpty()){
            TreeNode node1 = queue.poll();
            TreeNode node2 = queue.poll();
          	//如果这两个节点都为空，那么就继续比较
            if(node1 == null && node2 == null)
                continue;
          	//如果有一者为空，或者两个节点值不等，就返回false
            if((node1 == null || node2 == null) || node1.val != node2.val)
                return false;
						//按比较顺序入队
            queue.offer(node1.left);
            queue.offer(node2.right);
          
            queue.offer(node1.right);
            queue.offer(node2.left);

        }
        return true;
    }

}
```









<span id= "jump114"></span>

## 114.二叉树展开为链表

给定一个二叉树，原地将它展开为一个单链表。

 

例如，给定二叉树

    		1
       / \
      2   5
     / \   \
    3   4   6


将其展开为：

```java
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```



```java
class Solution {
    TreeNode post;
    public void flatten(TreeNode root) {
        post = null;
        postOrder(root);
    }
    //逆后序遍历，一个一个接上即可
    void postOrder(TreeNode root){
        if(root != null){
            postOrder(root.right);
            postOrder(root.left);
            if(post != null){
                root.left = null;
                root.right = post;
            }
            post = root;
        }
    }

}
```







<span id = "jump121"></span>

## 121.买卖股票的最佳时机

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。

 

示例 1:

```java
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

示例 2:

```java
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

```java
class Solution {
    //一次遍历，不断更新最低价格，这样后面的price[i]可以不用管前面比minPrice更高的价格
    //直接跟minPrice相减，收益肯定是当前最大的。
    public int maxProfit(int[] prices) {

        int minPrice = Integer.MAX_VALUE;
        int res = 0;
        for(int i = 0; i < prices.length; ++i){
            if(prices[i] < minPrice){
                minPrice = prices[i];
            }else{
                res = Math.max(res, prices[i] - minPrice);
            }
        }
        return res;
    }
}
```















<span id = "jump122"></span>

## 122.买卖股票的最佳时机 II

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:

```java
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

示例 2:

```java
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

示例 3:

```java
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```


提示：

* 1 <= prices.length <= 3 * 10 ^ 4
* 0 <= prices[i] <= 10 ^ 4



动态规划：

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length - 1;
        //dp[i]表示第i天结束后，最大的累积收益
        //dp[i][0]持有一支股票->hs
        //dp[i][1]不持有任何股票->nhs
        //边界
        int hs = -prices[0];
        int nhs = 0;

        for(int i = 1; i <= n; ++i){
            hs = Math.max(hs, nhs - prices[i]);
            nhs = Math.max(nhs, hs + prices[i]);
        }

        return nhs;
    }
}
```

贪心：

```java
class Solution {
    public int maxProfit(int[] prices) {

        int res = 0;
        //涨了就卖，跌了就不卖
        //不管到底有没有买入，只要涨了，我就认为我买入了，直接卖即可，贪心
        for(int i = 1; i < prices.length; ++i){
            if(prices[i] - prices[i-1] > 0){
                res += prices[i] - prices[i-1];
            }
        }
        return res;
    }
}
```





<span id = "jump136"></span>

## 136.只出现一次的数字

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

```java
输入: [2,2,1]
输出: 1
```

示例 2:

```java
输入: [4,1,2,1,2]
输出: 4
```

利用异或的性质，当一个数被另一个数异或两次时，会变回自身。

```java
class Solution {
    public int singleNumber(int[] nums) {
        int targe = nums[0];
        for(int i = 1; i < nums.length; ++i){
            targe ^= nums[i];
        }
        return targe;
    }
}
```













<span id = "jump141"></span>

## 141.环形链表

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

 

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```java
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```



![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```java
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```



![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```java
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

```java
public class Solution {
    public boolean hasCycle(ListNode head) {

        int cnt = 0;

        while(head != null){
            if(cnt++ > 10000){
                return true;
            }
            head = head.next;
        }

        return false;
    }
}
```

快慢指针：

一个指针移动地慢，一个指针移动地快，如果有环路存在，那么快的指针肯定会追上慢的指针（套圈）。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null)   return false;
        ListNode slow = head;
        ListNode fast = head.next;
        while(fast != null && slow != null){
            if(slow == fast)
                return true;
            slow = slow.next;
            fast = fast.next;
            if(fast == null)    return false;
            fast = fast.next;
        }

        return false;
    }
}
```







<span id = "jump142"></span>

## 142.环形链表II

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

**说明：**不允许修改给定的链表。

 

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```java
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```



![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```java
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```



![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```java
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

快慢指针：

初始时，slow和fast都位于链表的头部，随后slow每次向后移动一个位置，fast向后移动两个位置。二者会在环中的某点相遇。如下图所示：

![](https://assets.leetcode-cn.com/solution-static/142/142_fig1.png)

b是相遇时slow指针在环内走过的距离，所以此时fast指针走过的距离为：`a+n(b+c)+b = a+(n+1)b+nc`。任意时刻，fast指针走过的距离都为slow指针的两倍，所以有：`b = a+(n+1)b+nc`，即：`a = c+(n-1)(b+c)`，所以从相遇点到入环点的距离加上 n-1 圈的环长，恰好等于从链表头部到入环点的距离。因此，当发现 slow 与 fast 相遇时，我们再额外使用一个指针 ptr。起始，它指向链表头部；随后，它和 slow 每次向后移动一个位置。最终，它们会在入环点相遇。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {

        if(head == null || head.next == null)   return null;
        ListNode slow = head, fast = head, ptr = head;

        while(fast != null){
            slow = slow.next;
            fast = fast.next;
            if(fast == null)    return null;
            fast = fast.next;
            if(slow == fast){
                while(slow != ptr){
                    slow = slow.next;
                    ptr = ptr.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```





<span id = "jump146"></span>

## 146.LRU 缓存机制

运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制 。
实现 LRUCache 类：

LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。


进阶：你是否可以在 O(1) 时间复杂度内完成这两种操作？

 

示例：

```java
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```






提示：

* 1 <= capacity <= 3000
* 0 <= key <= 3000
* 0 <= value <= 104
* 最多调用 3 * 104 次 get 和 put



双链表的删除或者移动都是O(1)，只有查询不是，所以用一个哈希表来存放双链表节点即可实现全O(1)

自己实现一个双链表，再用一个哈希表存储链表节点和key的对应关系，方便直接取出节点。

```java
class LRUCache {
    DLinkedNode head;
    DLinkedNode tail;
    Map<Integer, DLinkedNode> cache = new HashMap<>();
    int capacity;
    int size;
    public LRUCache(int capacity) {
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.pre = head;
        this.capacity = capacity;
        size = 0;
    }

    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if(node == null){
            return -1;
        }
        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        //cache中不存在key
        if(node == null){
            //cache满了吗？
            if(cache.size() == capacity){
                //删除尾巴的一个元素
                cache.remove(tail.pre.key);
                removeNode(tail.pre);
            }
            DLinkedNode newNode = new DLinkedNode(key, value);
            addHead(newNode);
            cache.put(key, newNode);
        }else{
            node.value = value;
            cache.put(key, node);
            moveToHead(node);
        }

    }

    private void moveToHead(DLinkedNode node){
        removeNode(node);
        head.next.pre = node;
        node.next = head.next;
        node.pre = head;
        head.next = node;
    }



    private void addHead(DLinkedNode node){
        head.next.pre = node;
        node.next = head.next;
        node.pre = head;
        head.next = node;
    }

    private void removeNode(DLinkedNode node){
        node.pre.next = node.next;
        node.next.pre = node.pre;
    }
}

class DLinkedNode{
    int key;
    int value;
    DLinkedNode pre;
    DLinkedNode next;
    public DLinkedNode(){}
    public DLinkedNode(int _key, int _value){
        key = _key;
        value = _value;
    }


}
```











<span id = "jump148"></span>

## 148.排序链表

给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

进阶：

你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

```java
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

```java
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

**提示：**

- 链表中节点的数目在范围 `[0, 5 * 104]` 内
- `-105 <= Node.val <= 105`



暴力遍历插入：

```java
class Solution {
    public ListNode sortList(ListNode head) {
        //制造一个伪头节点，用于执行插入操作
        ListNode fake_head = new ListNode(-1);
        fake_head.next = head;
        //遍历节点，出现逆序对，就把后一个节点插入到合适的位置
        ListNode ptr = head;
        while(ptr != null && ptr.next != null){
            if(ptr.val > ptr.next.val){
                //将此节点从原链表中取出
                ListNode tmp = ptr.next;
                ptr.next = tmp.next;
                ListNode cur = fake_head;
                //找合适的位置
                while(cur.next != null && cur.next.val < tmp.val){
                    cur = cur.next;
                }
                //把该节点插入到合适的位置
                tmp.next = cur.next;
                cur.next = tmp;
            }else {
                ptr = ptr.next;
            }

        }
        return fake_head.next;
    }
}
```

小顶堆：

```java
class Solution {
    public ListNode sortList(ListNode head) {
        //小顶堆
        PriorityQueue<ListNode> queue = new PriorityQueue<>((a,b)->{
            if(a.val > b.val){
                return 1;
            }else{
                return -1;
            }
        });
        ListNode ptr = head;
        while(ptr != null){
            queue.offer(ptr);
            ptr = ptr.next;
        }
        ListNode newHead = new ListNode(-1);
        ptr = newHead;
        while(!queue.isEmpty()){
            ListNode tmp = queue.poll();
            tmp.next = null;
            ptr.next = tmp;
            ptr = tmp;
        }
        ptr = null;
        return newHead.next;
    }
}
```

递归归并排序：

```java
class Solution {
    //归并排序链表
    //1.先用快慢指针把链表分割为两部分
    //2.再递归分割直至只剩一个节点，即head.next = null
    //3.接着对两个有序链表进行归并
    //4.用一个指针来串
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null)
            return head;
        ListNode slow = head;
        ListNode fast = head;
        while(slow.next != null && fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next;
            fast = fast.next;
        }
        //保留后半段的引用
        ListNode tmp = slow.next;
        //分割链表
        slow.next = null;
        //递归分割
        ListNode left = sortList(head);
        ListNode right = sortList(tmp);

        ListNode res = new ListNode(-1);
        ListNode cur = res;
        //执行归并
        while(left != null && right != null){
            if(left.val < right.val){
                cur.next = left;
                left = left.next;
            }else{
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        cur.next = left != null ? left : right;
        return res.next;

    }
}
```

迭代归并排序（不是我写的，待复习！）：

```java
class Solution {
    public ListNode sortList(ListNode head) {
        int length = getLength(head);
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
       
        for(int step = 1; step < length; step*=2){ //依次将链表分成1块，2块，4块...
            //每次变换步长，pre指针和cur指针都初始化在链表头
            ListNode pre = dummy; 
            ListNode cur = dummy.next;
            while(cur!=null){
                ListNode h1 = cur; //第一部分头 （第二次循环之后，cur为剩余部分头，不断往后把链表按照步长step分成一块一块...）
                ListNode h2 = split(h1,step);  //第二部分头
                cur = split(h2,step); //剩余部分的头
                ListNode temp = merge(h1,h2); //将一二部分排序合并
                pre.next = temp; //将前面的部分与排序好的部分连接
                while(pre.next!=null){
                    pre = pre.next; //把pre指针移动到排序好的部分的末尾
                }
            }
        }
        return dummy.next;
    }
    public int getLength(ListNode head){
    //获取链表长度
        int count = 0;
        while(head!=null){
            count++;
            head=head.next;
        }
        return count;
    }
    public ListNode split(ListNode head,int step){
        //断链操作 返回第二部分链表头
        if(head==null)  return null;
        ListNode cur = head;
        for(int i=1; i<step && cur.next!=null; i++){
            cur = cur.next;
        }
        ListNode right = cur.next;
        cur.next = null; //切断连接
        return right;
    }
    public ListNode merge(ListNode h1, ListNode h2){
    //合并两个有序链表
        ListNode head = new ListNode(-1);
        ListNode p = head;
        while(h1!=null && h2!=null){
            if(h1.val < h2.val){
                p.next = h1;
                h1 = h1.next;
            }
            else{
                p.next = h2;
                h2 = h2.next;
            }
            p = p.next;           
        }
        if(h1!=null)    p.next = h1;
        if(h2!=null)    p.next = h2;

        return head.next;     
    }
}

作者：cherry-n1
链接：https://leetcode-cn.com/problems/sort-list/solution/pai-xu-lian-biao-di-gui-die-dai-xiang-jie-by-cherr/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





<span id = "jump200"></span>

## 200.岛屿数量

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1：

```java
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

示例 2：

```java
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```


提示：

* m == grid.length
* n == grid[i].length
* 1 <= m, n <= 300
* grid[i][j] 的值为 '0' 或 '1'



非常简单的深搜题：

```java
class Solution {
    boolean[][] visited;

    public int numIslands(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        visited = new boolean[m][n];
        int cnt = 0;
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(grid[i][j] == '1' && visited[i][j] == false){
                    dfs(i, j, grid);
                    cnt++;
                }
            }
        }
        return cnt;
    }


    void dfs(int i, int j, char[][] grid){
        if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length){
            return;
        }
        if(grid[i][j] == '0' || visited[i][j] == true){
            return;
        }
        visited[i][j] = true;
        dfs(i + 1, j, grid);
        dfs(i, j + 1, grid);
        dfs(i - 1, j, grid);
        dfs(i, j - 1, grid);
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















<span id = "jump234"></span>

## 234.回文链表

请判断一个链表是否为回文链表。

示例 1:

```java
输入: 1->2
输出: false
```

示例 2:

```java
输入: 1->2->2->1
输出: true
```

进阶：

* 你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

用栈实现逆序比对。

```java
class Solution {
    public boolean isPalindrome(ListNode head) {

        //统计节点个数
        int num = 0;
        ListNode ptr = head;
        while(ptr != null){
            num++;
            ptr = ptr.next;
        }
        ptr = head;
        int cnt = num / 2;
        //将前一半节点入栈
        Stack<ListNode> stack = new Stack<>();
        while(cnt > 0){
            stack.push(ptr);
            ptr = ptr.next;
            cnt--;
        }
        //如果是奇数，就需要继续向后移动一个节点，越过中间节点
        if(num % 2 != 0)    ptr = ptr.next;
        //出栈比对
        while(!stack.isEmpty()){
            ListNode tmp = stack.pop();
            if(tmp.val != ptr.val)
                return false;
            ptr = ptr.next;
        }
        return true;
    }
}
```



进阶：将后一半链表翻转，再双指针遍历比较，结束后将链表恢复原样

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head == null)    return true;
        //统计节点个数
        int num = 0;
        ListNode ptr = head;
        while(ptr != null){
            num++;
            ptr = ptr.next;
        }
        ptr = head;
        int cnt = num / 2;
        while(cnt > 0){
            ptr = ptr.next;
            cnt--;
        }
        //将后一半链表翻转
        ListNode revStart = reverseListNode(ptr);
        ListNode p1 = head;
        ListNode p2 = revStart;
        cnt = num / 2;
        while(cnt > 0){
            if(p1.val != p2.val){
                ptr.next = reverseListNode(revStart);
                return false;
            }
            cnt--;
            p1 = p1.next;
            p2 = p2.next;
        }
        ptr.next = reverseListNode(revStart);
        return true;
    }

    ListNode reverseListNode(ListNode head){
        ListNode cur = head;
        ListNode pre = null;

        while(cur != null){
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;            
        }
        return pre;
    }
}
```







<span id = "jump238"></span>

## 238.除自身以外数组的乘积

给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

 

示例:

```java
输入: [1,2,3,4]
输出: [24,12,8,6]
```


提示：题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。

说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

进阶：
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）





```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if(nums.length == 0)    return new int[]{};
        //pre[i]表示[0,i-1]元素的乘积
        //post[i]表示[i+1,nums.length-1]元素的乘积
        int[] pre = new int[nums.length];
        int[] post = new int[nums.length];

        pre[0] = 1;
        for(int i = 1 ;i < nums.length; ++i){
            pre[i] = pre[i-1] * nums[i-1];
        }

        post[nums.length - 1] = 1;
        for(int i = nums.length - 2; i >= 0; --i){
            post[i] = post[i+1] * nums[i+1];
        }

        int[] output = new int[nums.length];
        for(int i = 0; i < nums.length; ++i){
            output[i] = pre[i] * post[i];
        }
        return output;
    }
}
```

线性空间：

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if(nums.length == 0)    return new int[]{};
        //pre[i]表示[0,i-1]元素的乘积
        //post[i]表示[i+1,nums.length-1]元素的乘积
        int[] output = new int[nums.length];
        output[0] = 1;
        for(int i = 1 ;i < nums.length; ++i){
            output[i] = output[i-1] * nums[i-1];
        }
        int post = 1;
        for(int i = nums.length - 2; i >= 0; --i){
            post = post * nums[i+1];
            output[i] = post * output[i];
        }
        return output;
    }
}
```









<span id = "jump239"></span>

## 239.滑动窗口最大值

给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

进阶：

* 你能在线性时间复杂度内解决此题吗？



示例:

```java
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值

---------------               -----

[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```


提示：

* 1 <= nums.length <= 10^5
* -10^4 <= nums[i] <= 10^4
* 1 <= k <= nums.length
* 通过次数83,092提交次数168,914

暴力法：

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0)    return new int[0];
        //最终结果的数组
        int[] res = new int[nums.length - k + 1];
        int max_i = 0;
        int max = Integer.MIN_VALUE;
        int cur = 0;
        for(int i = 0; i < nums.length; ++i){
            //还在第一个窗口内
            if(i < k){
                if(nums[i] >= max){
                    max = nums[i];
                    max_i = i;
                }
                if(i == k-1)
                    res[cur++] = max;
            }else{
                //开始滑动窗口
                //去除的左边界就是最大值
                if(i-k == max_i){
                    max = Integer.MIN_VALUE;
                    max_i = i - k + 1;
                    //重新搜索最大值
                    for(int j = i - k + 1; j <= i; ++j){
                        if(nums[j] >= max){
                            max = nums[j];
                            max_i = j;
                        }
                    }
                }else{
                    //去除的左边界不是最大值
                    //比较加入的右边界是否比当前最大值大
                    if(nums[i] >= max){
                        max = nums[i];
                        max_i = i;
                    }
                }
                res[cur++] = max;
            }
        }
        return res;
    }
}
```

双向队列：

```java
class Solution {
    //建立一个双向队列
    //实现：遍历每一个元素，从右边删除（出队）所有小于等于该元素的队列元素
    //这样就能保证队列首元素为最大值
    ArrayDeque<Integer> deque = new ArrayDeque<>();
    int[] nums;
    int k;
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0)    return new int[0];
        //最终结果的数组
        int[] res = new int[nums.length - k + 1];
        int cur = 0;
        int max_i = 0;
        this.nums = nums;
        this.k = k;
        //先遍历前k个元素，初始化队列
        for(int i = 0; i < k; ++i){
            clean_deq(i);
            deque.offerLast(i);
        }
        res[cur++] = nums[deque.getFirst()];
        //遍历剩余元素
        for(int i = k; i < nums.length; ++i){
            clean_deq(i);
            deque.offerLast(i);
            res[cur++] = nums[deque.getFirst()];
        }
        return res;
    }

    void clean_deq(int i){
        //如果当前最大值为左边界
        if(!deque.isEmpty() && deque.getFirst() == i - k){
            //删除左边界
            deque.pollFirst();
        }
        //把nums[i]放入队列
        while(!deque.isEmpty() && nums[i] >= nums[deque.getLast()]) deque.pollLast();
        
    }
}
```

动态规划（别人写的，需要看！）：

```java
class Solution {
  public int[] maxSlidingWindow(int[] nums, int k) {
    int n = nums.length;
    if (n * k == 0) return new int[0];
    if (k == 1) return nums;

    int [] left = new int[n];
    left[0] = nums[0];
    int [] right = new int[n];
    right[n - 1] = nums[n - 1];
    for (int i = 1; i < n; i++) {
      // from left to right
      if (i % k == 0) left[i] = nums[i];  // block_start
      else left[i] = Math.max(left[i - 1], nums[i]);

      // from right to left
      int j = n - i - 1;
      if ((j + 1) % k == 0) right[j] = nums[j];  // block_end
      else right[j] = Math.max(right[j + 1], nums[j]);
    }

    int [] output = new int[n - k + 1];
    for (int i = 0; i < n - k + 1; i++)
      output[i] = Math.max(left[i + k - 1], right[i]);

    return output;
  }
}

作者：LeetCode
链接：https://leetcode-cn.com/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-zui-da-zhi-by-leetcode-3/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





<span id = "jump240"></span>

## 240.搜索二维矩阵 II

编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。
示例:

现有矩阵 matrix 如下：

```java
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```



* 给定 target = 5，返回 true。
* 给定 target = 20，返回 false。

深度优先搜索，从左上角开始搜索：

```java
class Solution {

    boolean[][] visited;
    int[][] matrix;
    int target;
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix.length == 0)  return false;
        visited = new boolean[matrix.length][matrix[0].length];
        this.matrix = matrix;
        this.target = target;
        return dfs(0,0);
    }

    boolean dfs(int x, int y){
        //超出边界
        if( x < 0 || x >= matrix.length || y < 0 || y >= matrix[0].length)
            return false;
        //找到了target
        if(matrix[x][y] == target)
            return true;
        //记录已访问元素
        visited[x][y] = true;
        //当前元素小于target
        if(matrix[x][y] < target){
            //如果在右边界了，就往下走
            if(y == matrix[0].length - 1){
                return dfs(x+1, y);
            }else{
                //如果还没在右边界，并且(x,y+1)没有被访问过
                if(visited[x][y+1] == false){
                    //向右边走
                    return dfs(x, y +1);
                }else{
                    //右边已经访问过了，就往下走
                    return dfs(x + 1, y);
                }
            }
        }
        else{
            //如果当前元素大于target，就往左走
            return dfs(x, y - 1);
        }

    }
}
```



从左下角开始搜索：

```java
class Solution {

    public boolean searchMatrix(int[][] matrix, int target) {
        int x = matrix.length - 1, y = 0;
        while(x >= 0 && y < matrix[0].length){
            if(matrix[x][y] > target){
                x--;
            }else if(matrix[x][y] < target){
                y++;
            }else{
                return true;
            }         
        }
        return false;   
    }
}
```







<span id = "jump279"></span>

## 279.完全平方数

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

示例 1:

```java
输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
```

示例 2:

```java
输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```



动态规划：

```java
class Solution {

    public int numSquares(int n) {
        if(n == 1)  return 1;
        //dp[i]表示组成数字i最少需要的完全平方数个数
        //dp[n]就是答案
        int[] dp = new int[n+1];
        //那么dp[i]为min(dp[i - 可用的每个完全平方数] + 1)
        Arrays.fill(dp, 4);
        //边界处理
        dp[0] = 0;
        //找出可用的完全平方数
        int max_s_i = (int)Math.sqrt(n);
        int[] square_nums = new int[max_s_i + 1];
        for(int i = 1; i <= max_s_i; ++i){
            square_nums[i] = i * i;
        }

        //遍历1～n所有的数
        for(int i = 1; i <= n; ++i){
            //遍历所有的完全平方数
            for(int s = 1; s <= max_s_i; ++s){
                if(i < square_nums[s])  break;
                dp[i] = Math.min(dp[i], dp[i - square_nums[s]] + 1);
            }
        }
        return dp[n];
    }

}
```

数学方法：

```java
class Solution {

  protected boolean isSquare(int n) {
    int sq = (int) Math.sqrt(n);
    return n == sq * sq;
  }

  //拉格朗日四平方数定理：所有的自然数都可以被表示为4个整数的平方和
  public int numSquares(int n) {
    while (n % 4 == 0)
      n /= 4;
    if (n % 8 == 7)
      return 4;
		//只要n ！= 4^k (8m+7)，那么这个正整数可以表示为3个平方
    //但是可以更少时，我们取更少
    //即当前数字就是个完全平方数
    if (this.isSquare(n))
      return 1;
    //当前数字可以被分解为两个完全平方数
    for (int i = 1; i * i <= n; ++i) {
      if (this.isSquare(n - i * i))
        return 2;
    }
    //不可以分解为1个或2个完全平方数，那么根据Adrien-Marie-Legendre的三平方定理只能是3个完全平方数了
    return 3;
  }
}
```











<span id = "jump283"></span>

## 283.移动零

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

```java
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```


说明:

* 必须在原数组上操作，不能拷贝额外的数组。
* 尽量减少操作次数。

冒泡法：

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums.length == 0)    return;
        int  left = 0, right = 0;
   
        while(left < nums.length){
            while(left < nums.length && nums[left] != 0) ++left;
            if(left >= nums.length) return;
            right = left + 1;
            while(right < nums.length && nums[right] == 0)  ++right;
            if(right >= nums.length)    return;
            swap(nums, left, right);
            left++;
        }
    }

    void swap(int[] nums, int i, int j){
        if(i != j){
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
    }
}
```

先排列所有非0元素，再填充0:

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int ptr = 0;
        for(int i = 0; i < nums.length; ++i){
            if(nums[i] != 0){
                nums[ptr++] = nums[i];
            }
        }
        for(int i = ptr; i < nums.length; ++i){
            nums[i] = 0;
        }   
    }
}
```

快慢指针，快指针正常遍历，慢指针始终指向第一个0。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int ptr = 0;
        for(int i = 0; i < nums.length; ++i){
            if(nums[i] != 0){
                swap(nums, ptr, i);
                ++ptr;
            }
        }
    }

    void swap(int[] nums, int i, int j){
        if(i != j){
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
    }
}
```





<span id = "jump287"></span>

## 287.寻找重复数

给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

示例 1:

```java
输入: [1,3,4,2,2]
输出: 2
```

示例 2:

```java
输入: [3,1,3,4,2]
输出: 3
```


说明：

* 不能更改原数组（假设数组是只读的）。
* 只能使用额外的 O(1) 的空间。
* 时间复杂度小于 O(n2) 。
* 数组中只有一个重复的数字，但它可能不止重复出现一次。



数字分布在1～n之间，如果重复的数字为k：

1. 那么在[1,k-1]范围内的数字nums[i]有：数组中小于等于nums[i]的数字个数为不会超过nums[i]个，例如1，小于等于1的数字有1个
2. 在[k,n]范围内的数字nums[i]有：小于等于nums[i]数字个数均大于nums[i]，例如3，小于等于3的数字有4个。

二分法，搜索单调性变化的边界即可。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        //二分法：在1，2，3，4，...,n之间搜索
        //我们发现：如果重复的数字为k，
        //1、那么在[1,k-1]范围内的数字nums[i]有：
        //      数组中小于等于nums[i]的数字个数为不会超过nums[i]个，例如1，小于等于1的数字有1个
        //2、在[k,max]范围内的数字nums[i]有：
        //      小于等于nums[i]数字个数均大于nums[i]，例如3，小于等于3的数字有4个。
        int l = 1, r = nums.length - 1;
        int res = -1;
        while(l <= r){
            int mid = (l + r) >> 1;
            int cnt = 0;
            for(int i = 0; i < nums.length; ++i){
                if(nums[i] <= mid){
                    cnt++;
                }
            }

            if(cnt <= mid){
                l = mid + 1;
            }else{
                r = mid - 1;
                res = mid;
            }
        }
        return res;

    }
}
```







<span id ="jump300"></span>

## 300.最长上升子序列

给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

```java
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```


说明:

* 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
* 你算法的时间复杂度应该为 O(n2) 。

进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?



动态规划：

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        if(n == 0)  return 0;
        //dp[i]表示[0,i]的最长上升子序列长度
        int[] dp = new int[n];
        //初始化边界
        Arrays.fill(dp,1);
        int max = 1;
        for(int i = 1; i < n; ++i){
            for(int j = i - 1; j >= 0; --j){
                if(nums[i] > nums[j]){
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                }
            }
            max = Math.max(dp[i], max);
        }

        return max;
    }
}
```



贪心：

设计思路：

新的状态定义：
我们考虑维护一个列表 tails，其中每个元素 tails[k] 的值代表 长度为k+1 的子序列尾部元素的值。
如 `[1,4,6]` 序列，长度为 `1,2,3` 的子序列尾部元素值分别为 tails=`[1,4,6]`。
状态转移设计：
设常量数字 N，和随机数字 x，我们可以容易推出：当 N 越小时，N<x 的几率越大。例如： N=0 肯定比 N=1000 更可能满足 N<x。
在遍历计算每个 tails[k]，不断更新长度为` [1,k] `的子序列尾部元素值，始终保持每个尾部元素值最小 （例如 `[1,5,3]`， 遍历到元素 5 时，长度为 2 的子序列尾部元素值为 5；当遍历到元素 3 时，尾部元素值应更新至 3，因为 3 遇到比它大的数字的几率更大）。
tails 列表一定是严格递增的： 即当尽可能使每个子序列尾部元素值最小的前提下，子序列越长，其序列尾部元素值一定更大。

既然严格递增，每轮计算 tails[k] 时就可以使用二分法查找需要更新的尾部元素值的对应索引 i。

```java
class Solution {
	
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        if(n == 0)  return 0;
	      //其中每个元素 tails[k] 的值代表 长度为k+1 的子序列尾部元素的值。
        int[] tail = new int[n];
        int res = 0;
        for(int num : nums){
            int i = 0, j = res;
            //计算 tails[k] 时就可以使用二分法查找需要更新的尾部元素值的对应索引 i
            while(i < j){
                int m = (j - i) / 2 + i;
                if(tail[m] < num){
                    i = m + 1;
                }else{
                    j = m;
                }
            }
            tail[i] = num;
            if(j == res) res++;
        }
        return res;
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









<span id = "394"></span>

## 394.字符串解码

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

 

示例 1：

```java
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

示例 2：

```java
输入：s = "3[a2[c]]"
输出："accaccacc"
```

示例 3：

```java
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

示例 4：

```java
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```



```java
class Solution {
    public String decodeString(String s) {

        Deque<String> chars = new LinkedList<>();
        Deque<Integer> times = new LinkedList<>();

        char[] arr = s.toCharArray();
        //遍历字符数组，把数字放入time栈，把字符放入chars栈
        //当遇到"]"时，出栈chars元素，直到碰到"["，再出栈一个times元素
        //将组成的字符串重复多次，再放入chars
        //当读取完了数组，就将chars字符串拼接并且reverse
        for(int i = 0; i < arr.length; ++i){
            if(arr[i] == ']'){
                //出栈
                StringBuffer sb = new StringBuffer();
                while(!chars.peek().equals("[")){
                    String tmp = chars.pop();
                    sb.append(tmp);
                }
                //删除"]"
                chars.pop();
                int time = times.pop();
                //重复字符串time-1次
                String str = sb.toString();
                for(int j = 0; j < time-1; ++j){
                    sb.append(str);
                }
                chars.push(sb.toString());
            }else if(Character.isDigit(arr[i])){
                //连续读取数字如'1','0','0'读为100
                StringBuffer digit = new StringBuffer();
                digit.append(arr[i]);
                while(i + 1 < arr.length && Character.isDigit(arr[i+1])){
                    digit.append(arr[i+1]);
                    i++;
                }
                times.push(Integer.parseInt(digit.toString()));
            }else{
                chars.push(arr[i] + "");
            }
        }

        StringBuffer res = new StringBuffer();
        while(!chars.isEmpty()){
            res.append(chars.pop());
        }
        return res.reverse().toString();

    }
}
```











<span id = "jump406"></span>

## 406.根据身高重建队列

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对(h, k)表示，其中h是这个人的身高，k是排在这个人前面且身高大于或等于h的人数。 编写一个算法来重建这个队列。

注意：
总人数少于1100人。

示例

```java
输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

观察数组，如果将数组按`people[i][0]`逆序排序，然后逐个插入到新数组中，发现`people[i][1]`就是此元素需要在数组插入的位置，向后移动数组，再进行插入即可。

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        //将数组按people[i][0]逆序排序
        Arrays.sort(people, (a,b)->{
            if(a[0] > b[0])
                return -1;
            else if(a[0] == b[0]){
                if(a[1] > b[1])
                    return 1;
                else
                    return -1;
            }else
                return 1;
        });

        int[][] res = new int[people.length][2];
        for(int i = 0; i < people.length; ++i){
            insert(res,people[i][0], people[i][1], i);
        }
        return res;
    }

    void insert(int[][] res, int h, int k, int tail){
        //k是此元素在数组中需要被插入的位置
        //先把k以后的元素都往后挪，腾出第k个位置
        for(int i = tail; i > k; --i){
            res[i][0] = res[i - 1][0];
            res[i][1] = res[i - 1][1];
        }
        res[k][0] = h;
        res[k][1] = k;
    }
}
```

用list代替数组来提高效率，实际上用的方法是一样的，只不过上面这种方法实现了下面list的底层思想，效率没它高罢了：

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        //将数组按people[i][0]逆序排序
        Arrays.sort(people, (a,b)->{
            if(a[0] > b[0])
                return -1;
            else if(a[0] == b[0]){
                if(a[1] > b[1])
                    return 1;
                else
                    return -1;
            }else
                return 1;
        });

        List<int[]> list = new LinkedList<>();
        for(int i = 0; i < people.length; ++i){
            list.add(people[i][1],people[i]);
        }
        return list.toArray(new int[people.length][2]);
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







<span id = "jump448"></span>

## 448.找到所有数组中消失的数字

给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

示例:

```java
输入:
[4,3,2,7,8,2,3,1]
输出:
[5,6]
```

另类哈希，这样可以处理哈希值是二值的情况！

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList<>();
    
        //遍历数组每个元素，将|nums[i]| - 1处的数字标记为负数
        //已经是负数的就不处理
        //-1是为了让1~n映射到0~n-1
        for(int i = 0; i < nums.length; ++i){
            int idx = Math.abs(nums[i]) - 1;
            if(nums[idx] > 0){
                nums[idx] *= -1;
            }
        }

        //遍历处理过后的nums，
        //值为负数的位置对应的索引值就是出现在数组中的数字，
        //非负数就是没出现在其中的数字
        for(int i = 0; i  < nums.length; ++i){
            if(nums[i] > 0){
                res.add(i+1);
            }
        }

        return res;
    }
}
```







<span id = "jump494"></span>

## 494.目标和

给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

示例：

```java
输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5
解释：

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
```


提示：

* 数组非空，且长度不会超过 20 。
* 初始的数组的和不会超过 1000 。
* 保证返回的最终结果能被 32 位整数存下。



深搜回溯：

```java
class Solution {
    int cnt = 0;
    int target;
    int[] nums;
    public int findTargetSumWays(int[] nums, int S) {
        this.nums = nums;
        target = S;
        dfs(0,0);
        return cnt;
    }

    void dfs(int cur, int sum){
        if(cur == nums.length){
            if(sum == target)    cnt++;
            return;
        }
        dfs(cur+1, sum + nums[cur]);
        dfs(cur+1, sum - nums[cur]);
    }
}
```



动态规划：

这道题也是一个常见的背包问题，我们可以用类似求解背包问题的方法来求出可能的方法数。

我们用 `dp[i][j] `表示用数组中的前 i 个元素，组成和为 j 的方案数。考虑第 i 个数 `nums[i]`，它可以被添加 + 或 -，因此状态转移方程如下：

```java
dp[i][j] = dp[i - 1][j - nums[i]] + dp[i - 1][j + nums[i]]
```


也可以写成递推的形式：

```java
dp[i][j + nums[i]] += dp[i - 1][j]
dp[i][j - nums[i]] += dp[i - 1][j]
```


由于数组中所有数的和不超过 1000，那么 j 的最小值可以达到 -1000。在很多语言中，是不允许数组的下标为负数的，因此我们需要给 dp[i][j] 的第二维预先增加 1000，即：

```java
dp[i][j + nums[i] + 1000] += dp[i - 1][j + 1000]
dp[i][j - nums[i] + 1000] += dp[i - 1]
```



```java
public class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int[][] dp = new int[nums.length][2001];
        dp[0][nums[0] + 1000] = 1;
        dp[0][-nums[0] + 1000] += 1;
        for (int i = 1; i < nums.length; i++) {
            for (int sum = -1000; sum <= 1000; sum++) {
                if (dp[i - 1][sum + 1000] > 0) {
                    dp[i][sum + nums[i] + 1000] += dp[i - 1][sum + 1000];
                    dp[i][sum - nums[i] + 1000] += dp[i - 1][sum + 1000];
                }
            }
        }
        return S > 1000 ? 0 : dp[nums.length - 1][S + 1000];
    }
}
```







<span id = "jump538"></span>

## 538.把二叉搜索树转换为累加树

给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

 例如：

```java
输入: 原始二叉搜索树:
              5
            /   \
           2     13

输出: 转换为累加树:
             18
            /   \
          20     13
```

递归反中序遍历，可以得到二叉搜索树的降序排序节点。将较大值加到前一个较小值即可。

```java
class Solution {

    List<TreeNode> nodes;
    public TreeNode convertBST(TreeNode root) {
        if(root == null)    return null;
        nodes = new ArrayList<>();
        inOrder(root);
      	//将节点值累加到较小节点
        for(int i = 0; i < nodes.size()-1; ++i){
            nodes.get(i+1).val += nodes.get(i).val;
        }
        return root;
    }
		//逆中序遍历，将节点添加到list中
    void inOrder(TreeNode root){
        if(root == null)
            return;
        
        inOrder(root.right);
        nodes.add(root);
        inOrder(root.left);
    }
}
```

或：

```java
class Solution {

    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root != null){
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }   
        return root;
    }    
}
```





<span id = "jump560"></span>

## 560.和为K的子数组

给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

示例 1 :

```java
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```


说明 :

* 数组的长度为 [1, 20,000]。
* 数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。

暴力遍历子数组：

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        
        int cnt = 0;
        for(int i = 0; i < nums.length; ++i){
            int sum = 0;
            for(int j = i; j >= 0; --j){
                sum += nums[j];
                if(sum == k){
                    cnt++;
                }
            }
        }
        return cnt;
    }
}
```

记录前缀和，统计`pre[j] = pre[i] - k`的个数。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int cnt = 0;
        //pre[i]表示从[0,i]的元素之和，那么我们只需要找到所有符合条件的下标j
        //使得pre[j] == pre[i] - k即可，这样的j可能有多个，所以我们把所有元素的前缀和pre[i]都放入一个哈希表中
        //再从左往右遍历数组，统计pre[j]出现的次数
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0,1);
        int pre = 0;
        for(int i = 0; i < nums.length; ++i){
            pre += nums[i];
            if(map.containsKey(pre - k)){
               cnt += map.get(pre - k); 
            }
            map.put(pre, map.getOrDefault(pre, 0)+1);
        }
        return cnt;
    }
}
```





<span id = "jump581"></span>

## 581.最短无序连续子数组

给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是最短的，请输出它的长度。

示例 1:

```java
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```


说明 :

* 输入的数组长度范围在 [1, 10,000]。
* 输入的数组可能包含重复元素 ，所以升序的意思是<=。

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        //有可能这个子数组长度为0
        //初始化指针，[start, end]表示需要排序的子数组
        int start = nums.length - 1, end = nums.length - 1;
        //max记录[0,i]的最大值
        int max = nums[0];
        for(int i = 1; i < nums.length; ++i){
            //每找到一个逆序对，即nums[i] < max，就从[0, start]中搜索第一个比nums[i]大的数，更新start
            if(nums[i] < max){
                for(int j = 0; j < start; ++j){
                    if(nums[j] > nums[i]){
                        //往前更新start
                        start = j;
                        break;
                    }
                }
                //更新end
                end = i;
            }
            max = Math.max(max, nums[i]);
        }
        return end == start ? 0 : end - start + 1;
    }
}
```









<span id="jump617"></span>

## 617.合并二叉树

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

示例 1:

```java
输入: 
				Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```


注意: 合并必须从两个树的根节点开始。



以Tree 2为参照，同时遍历两颗树：

* 如果Tree2的左子节点不为空，就在Tree1的左子节点上合并这个节点，如果此时Tree1的左子节点为空，就新建一个值为0的节点，再合并。
* 如果Tree2的右子节点不为空，就在Tree1的右子节点上合并这个节点，如果此时Tree1的右子节点为空，就新建一个值为0的节点，再合并。
* Tree2子树为空的情况无需处理

最终返回Tree1，即为答案。

```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null)  return t2;
        if(t2 == null)  return t1;
        merge(t1,t2);
        return t1;
    }
    void merge(TreeNode t1, TreeNode t2){
        //传进来的节点肯定不为空
        t1.val += t2.val;
        if(t2.left != null){
            if(t1.left == null){
                t1.left = new TreeNode(0);
            }
            //合并左子树
            merge(t1.left,t2.left);
        }
        if(t2.right != null){
            if(t1.right == null){
                t1.right = new TreeNode(0);
            }
            //合并右子树
            merge(t1.right,t2.right);
        }
    }
}
```

或：

```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null)  return t2;
        if(t2 == null)  return t1;

        TreeNode merge = new TreeNode(t1.val + t2.val);
        merge.left = mergeTrees(t1.left,t2.left);
        merge.right = mergeTrees(t1.right,t2.right);

        return merge;
    }
}
```









<span id = "jump621"></span>

## 621.任务调度器

给定一个用字符数组表示的 CPU 需要执行的任务列表。其中包含使用大写的 A - Z 字母表示的26 种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。CPU 在任何一个单位时间内都可以执行一个任务，或者在待命状态。

然而，两个相同种类的任务之间必须有长度为 n 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的最短时间。

示例 ：

```java
输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B.
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 
```


提示：

* 任务的总个数为 [1, 10000]。
* n 的取值范围为 [0, 100]。



每次从任务列表中选取剩余次数最多的n+1个任务执行，如果剩余任务没有超过n+1个，则选取剩余所有任务执行，多余的时间待命。

只要每次选取剩余次数最多的n+1个任务，那么下一轮选取的时候，每个任务都是处于可选择状态。

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int rest = tasks.length;
        int res = 0;
        int[] counts = new int[26];
				//记录所有任务的次数
        for(int i = 0; i < tasks.length; ++i){
            int index = tasks[i] - 65;
            counts[index]++;
        }

        Arrays.sort(counts);

        while(counts[25] > 0){
          	//选取剩余次数最多的n+1个任务
            for(int i = 0; i <= n; ++i){
              	//剩余次数最多的任务，次数都为0了，那么就执行完毕了
                if(counts[25] == 0)
                    break;
                if( 25 - i >= 0 && counts[25 - i] > 0)
                    counts[25-i]--;
                res++;
            }
          	//选取完后，需要重新排序
            Arrays.sort(counts);
        }
        return res;
    }
}
```

最小堆解法：

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {

        int res = 0;
        int[] counts = new int[26];

        for(int i = 0; i < tasks.length; ++i){
            int index = tasks[i] - 65;
            counts[index]++;
        }

        PriorityQueue<Integer> queue = new PriorityQueue<>(26, Collections.reverseOrder());

        for(int i : counts){
            if(i > 0)
                queue.add(i);
        }


        while(!queue.isEmpty()){
            //为了避免重复选取任务，每次取出一个任务时，先放入list中，最后再统一插入到最小堆
            List<Integer> list = new ArrayList<>();
            for(int i = 0; i <= n; ++i){
                if(!queue.isEmpty()){
                    if(queue.peek() > 1)
                        list.add(queue.poll() - 1);
                    else
                        queue.poll();
                }
                res++;
                if(queue.isEmpty() && list.size() == 0)
                    break;
            }
            for(int e : list){
                queue.add(e);
            }
        }
        return res;
    }
}
```





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



