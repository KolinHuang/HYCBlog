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



## 剑指 Offer 03. 数组中重复的数字

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

```java
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

限制：

```java
2 <= n <= 100000
```



### 答案

#### 集合

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums.length; ++i){
            if(set.contains(nums[i]))
                return nums[i];
            set.add(nums[i]);
        }
        return 0;
    }
}
```





#### 优化空间复杂度

以值作为下标，依次访问各个元素，若出现访问统一下标2次的情况，就返回下标。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int n = nums.length;
        for(int i = 0; i < nums.length; ++i){
            //标记以nums[i]为下标的元素，若出现重复访问，就返回nums[i]
            int k = nums[i];
            if(k < 0) k += n;
            //若再次访问了此元素，就返回其下标
            if(nums[k] < 0) return k;
            //标记第一次访问以nums[i]为下标的元素
            nums[k] -= n;
        }
        return 0;
    }
}
```





## 剑指 Offer 05. 替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

 

示例 1：

```java
输入：s = "We are happy."
输出："We%20are%20happy."
```

限制：

```java
0 <= s 的长度 <= 10000
```



### 我的答案

```java
class Solution {
    public String replaceSpace(String s) {
        if(s.length() == 0) return s;
        if(s.indexOf(" ") == -1)    return s;
        StringBuffer res = new StringBuffer();
        
        for(int i = 0; i < s.length(); ++i){
            if(s.charAt(i) == ' ')
                res.append("%20");
            else
                res.append(s.charAt(i));
        }
        return res.toString();
    }
}
```



### 参考答案

```java
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
      	//用字符数组构造字符串
        String newStr = new String(array, 0, size);
        return newStr;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/solution/mian-shi-ti-05-ti-huan-kong-ge-by-leetcode-solutio/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





## 剑指 Offer 07. 重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

 

例如，给出

```markdown
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```


返回如下的二叉树：

    		3
       / \
      9  20
        /  \
       15   7


### 答案

#### 递归



![leetcode_重建二叉树递归解法](/HYCBlog/assets/img/leetcode/leetcode_重建二叉树递归解法.png)

```java
package leetcode;


import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

class RebuildBTree {

    class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;

        TreeNode(int x) {
            val = x;
        }
    }


    public TreeNode buildTree(int[] preorder, int[] inorder) {
        //边界判断
        if(preorder.length == 0 || inorder.length == 0) return null;
        if(preorder.length != inorder.length) return null;

        //为inorder中的每个数建立哈希索引
        Map<Integer,Integer> inMap = new HashMap<>();
        for(int i = 0; i < inorder.length; ++i){
            inMap.put(inorder[i],i);
        }

        //递归建立树
        return buildTree(preorder,0,preorder.length-1,inorder,0,inorder.length-1,inMap);

    }

    public TreeNode buildTree(int[] preorder, int pS, int pE, int[] inorder, int iS, int iE, Map<Integer, Integer> inMap) {
        if(pS > pE)
            return null;
        TreeNode root = new TreeNode(preorder[pS]);
        if(pS == pE)
            return root;

        int index = inMap.get(preorder[pS]);
        //当前节点左右子树节点总数
        int lnum = index - iS, rnum = iE - index;
        //左子树和右子树在前序序列和中序序列中的序列要一致，范围要对应好
        root.left = buildTree(preorder,pS+1,pS+lnum,inorder,iS,index-1,inMap);
        root.right = buildTree(preorder,pE-rnum+1,pE,inorder,index+1,iE,inMap);

        return root;
    }

    public static void main(String[] args) {
        int[] a = {3,9,20,15,7};
        int[] b = {9,3,15,20,7};

        TreeNode treeNode = new RebuildBTree().buildTree(a, b);
        System.out.println(treeNode);
    }
}
```



#### 迭代

![leetcode_重建二叉树迭代解法_1](/HYCBlog/assets/img/leetcode/leetcode_重建二叉树迭代解法_1.png)

![leetcode_重建二叉树迭代解法_2](/HYCBlog/assets/img/leetcode/leetcode_重建二叉树迭代解法_2.png)

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || preorder.length == 0) {
            return null;
        }
        TreeNode root = new TreeNode(preorder[0]);
        int length = preorder.length;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        int inorderIndex = 0;
        for (int i = 1; i < length; i++) {
            int preorderVal = preorder[i];
            TreeNode node = stack.peek();
            if (node.val != inorder[inorderIndex]) {
                node.left = new TreeNode(preorderVal);
                stack.push(node.left);
            } else {
                while (!stack.isEmpty() && stack.peek().val == inorder[inorderIndex]) {
                    node = stack.pop();
                    inorderIndex++;
                }
                node.right = new TreeNode(preorderVal);
                stack.push(node.right);
            }
        }
        return root;
    }
}
```



## 剑指offer 09.用两个栈模拟队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

示例 1：

```markdown
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

示例 2：

```markdown
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

提示：

```markdown
1 <= values <= 10000
最多会对 appendTail、deleteHead 进行 10000 次调用
```



### 思路和方法

维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，我们每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第二个栈，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。如果为空，我们将第一个栈里的元素一个个弹出插入到第二个栈里，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。

#### 我的答案

```java
class CQueue {
    private class Stack{
        int top;
        int[] nums;
        Stack(){}

        void push(int x){
            nums[++top] = x;
        }
        int pop(){
            return nums[top--];
        }
    }

    Stack s1 = new Stack();
    Stack s2 = new Stack();

    public CQueue() {
        s1.top = -1;
        s1.nums = new int[10000];
        s2.top = -1;
        s2.nums = new int[10000];
    }

    public void appendTail(int value) {
        s1.nums[++s1.top] = value;
    }

    public int deleteHead() {
        if(s1.top == -1) return -1;

        while(s1.top != 0){
            s2.push(s1.pop());
        }
        int tmp = s1.pop();

        while(s2.top != -1){
            s1.push(s2.pop());
        }
        return tmp;
    }

    public static void main(String[] args) {
        CQueue obj = new CQueue();
        obj.appendTail(5);
        obj.appendTail(2);

        int res = obj.deleteHead();
        System.out.println(res);

        int res2 = obj.deleteHead();

        System.out.println(res2);
//
//        int res3 = obj.deleteHead();
//        System.out.println(res3);

    }
}

```



#### 参考答案

```java
class CQueue {
    Deque<Integer> stack1;
    Deque<Integer> stack2;
    
    public CQueue() {
        stack1 = new LinkedList<Integer>();
        stack2 = new LinkedList<Integer>();
    }
    
    public void appendTail(int value) {
        stack1.push(value);
    }
    
    public int deleteHead() {
        // 如果第二个栈为空
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        } 
        if (stack2.isEmpty()) {
            return -1;
        } else {
            int deleteItem = stack2.pop();
            return deleteItem;
        }
    }
}
```



##  剑指 Offer 10- II. 青蛙跳台阶问题



一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。



```java
class Solution {
    public int numWays(int n) {
        if(n == 0) return 1;
        if(n == 1) return 1;
        if(n == 2) return 2;

        int a = 1;
        int b = 2;
        int sum = 0;
        for(int i = 3; i <= n; ++i){
            sum = (a + b)% 1000000007;
            a = b;
            b = sum;
        }
        return b;
    }
}
```



```java
class Solution {
    public int numWays(int n) {
        if(n == 0) return 1;
        if(n == 1) return 1;
        if(n == 2) return 2;

        int[] res = new int[n+1];
        res[1] = 1;
        res[2] = 2;
        for(int i = 3; i <= n; ++i)
            res[i] = (res[i-1] + res[i-2])% 1000000007;
        
        return res[n];
    }
}
```





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

