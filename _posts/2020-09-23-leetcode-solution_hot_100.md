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

[5.最长回文子串](#jump5)

[17.电话号码的字母组合](#jump17)

[19.删除链表的倒数第N个节点](#jump19)

[20.有效的括号](#jump20)

[39.组合总和](#jump39)

[62.不同路径](jump62)

[75.颜色分类](jump75)

[78.子集](jump78)

[79.单词搜索](#jump79)

[94.二叉树的中序遍历](#jump94)

[96. 不同的二叉搜索树](#jump96)

[98.验证二叉搜索树](#jump98)

[101.对称二叉树](#jump101)

[136.只出现一次的数字](#jump136)

[141.环形链表](#jump141)

[142.环形链表II](#jump142)

[148.排序链表](#jump148)

[215.数组中的第k大元素](#jump215)

[226.翻转二叉树](#jump226)

[234.回文链表](jump234)

[239.滑动窗口最大值](#jump239)

[309.最佳买卖股票时机含冷冻期](#jump309)

[337. 打家劫舍 3](#jump337)

[347.前 K 个高频元素](#jump347)

[416.分割等和子集](jump416)

[494.目标和](jump494)

[538.把二叉搜索树转换为累加树](#jump538)

[560.和为K的子数组](#jump560)

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











<span id="jump5"></span>

## 5.最长回文子串

[点这里跳转](HYCBlog/posts/algorithm-manacher)



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



