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



### 

### 集合

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





### 优化空间复杂度

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



### 遍历

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


### 

### 递归



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



### 迭代

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



维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，我们每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第二个栈，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。如果为空，我们将第一个栈里的元素一个个弹出插入到第二个栈里，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。

### 

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





## 剑指 Offer 10- I. 斐波那契数列

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

```java
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```



斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

```java
class Solution {
    public int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;
        int a = 0;
        int b = 1;
        for(int i = 2; i <=n; ++i){
            int tmp = b;
            b = (a+b) % 1000000007;
            a = tmp;
        }
        return b;
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



## 剑指 Offer 11. 旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：

```java
输入：[3,4,5,1,2]
输出：1
```


示例 2：

```java
输入：[2,2,2,0,1]
输出：0
```



### 遍历

```java
class Solution {
    public int minArray(int[] numbers) {
      	//遍历整个数组，若第i+1个元素小于第i个元素，返回第i+1个元素。
        for(int i = 0; i < numbers.length-1; ++i){
            if(numbers[i+1] < numbers[i])
                return numbers[i+1];
        }
      	//否则返回最初元素
        return numbers[0];
    }
}
```



### 二分

用high判断

```java
class Solution {
    public int minArray(int[] numbers) {
        int low = 0;
        int high = numbers.length - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (numbers[pivot] < numbers[high]) {
                high = pivot;
            } else if (numbers[pivot] > numbers[high]) {
                low = pivot + 1;
            } else {
                high -= 1;
            }
        }
        return numbers[low];
    }
}
```

用low判断

```java
class Solution {
    public int minArray(int[] numbers) {
        int l = 0;
        int r = numbers.length-1;
        int mid = (l+r)/2;
        //本就是有序数组
        if(numbers[r] > numbers[l]) return numbers[l];

        while(l <= r){
            //如果二分后的数组是有序数组，则返回最左元素，即为最小
            if(numbers[r] > numbers[l]) return numbers[l];
            //若最左小于mid元素，则最左到mid是严格递增的，那么最小元素必定在mid之后
            if(numbers[l] < numbers[mid]){
                l = mid+1;
                mid = (l+r)/2;
            }
            //若最左大于mid元素，则最小元素必定在最左到mid之间(不包括最左，因为最左已经大于mid)
            else if(numbers[l] > numbers[mid]){
                r = mid;
                l++;
                mid = (l+r)/2;
            }
            //若二者相等，则最小元素必定在l+1到r之间，因为l和mid相等，故可以去除
            else{
                l++;
                mid = (l+r)/2;
            }
        }
        return numbers[mid];
    }
}
```





## 剑指 Offer 12. 矩阵中的路径

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","**b**","c","e"],
["s","**f**","**c**","s"],
["a","d","**e**","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。 

示例 1：

```java
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```


示例 2：

```java
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```


提示：

```java
1 <= board.length <= 200
1 <= board[i].length <= 200
```



### 深度优先搜索

深度优先搜索，每遇到一个起始点，就深度搜索各个邻接点。标记已访问节点，但是在回溯前，需要将已访问节点取消标记。



```java
import java.awt.Point;
class Solution {

    private char[][] b;
    private String w;
    private Set<Point> points = new HashSet<>();

    public boolean exist(char[][] board, String word) {
        b = board;
        w = word;
        if(b.length * b[0].length < w.length()) return false;
        
        Character c = w.charAt(0);
        for(int i = 0; i < b.length; ++i){
            for(int j = 0; j < b[0].length; ++j){
                if(b[i][j] == c){
                    if(dfs(i,j,1))
                        return true;
                }
            }
        }

        return false;
    }

    private boolean dfs(int i, int j, int k){
        if(k >= w.length())
            return true;
      	//标记此顶点为已访问
        points.add(new Point(i,j));
        Character c = w.charAt(k);
        boolean res = false;
      	//依次判断上下左右是否可访问
        if(i - 1 >= 0 && !points.contains(new Point(i-1,j)) && b[i-1][j] == c)
            res = res || dfs(i-1,j,k+1);
        if(i + 1 < b.length && !points.contains(new Point(i+1,j)) && b[i+1][j] == c)
            res = res || dfs(i+1,j,k+1);
        if(j - 1 >= 0 && !points.contains(new Point(i,j-1)) && b[i][j-1] == c)
            res = res || dfs(i,j-1,k+1);
        if(j + 1 < b[0].length && !points.contains(new Point(i,j+1)) && b[i][j+1] == c)
            res = res || dfs(i,j+1,k+1);
      	//此次搜索失败，回溯，取消此顶点标记
        if(!res)
            points.remove(new Point(i,j));
        return res;
    }
}
```



## 剑指 Offer 13. 机器人的运动范围

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

示例 1：

```java
输入：m = 2, n = 3, k = 1
输出：3
```

示例 2：

```java
输入：m = 3, n = 1, k = 0
输出：1
```

提示：

```java
1 <= n,m <= 100
0 <= k <= 20
```







### 我的答案-DFS

深度优先遍历。

```java
import java.awt.*;
class Solution {
    int m;
    int n;
    int k;
    int count;
  	//不要用这种方法来判断是否已访问了，创建对象的代价太大，用数组快很多
    //Set<Point> set = new HashSet<>();
    boolean[][] visited;
    public int movingCount(int m, int n, int k) {
        this.m = m;
        this.n = n;
        this.k = k;
        visited = new boolean[m][n];
        //if(sum(m-1) + sum(n-1) <= k) return (m*n);
        count = 0;
        dfs(0,0);

        return count;
    }
    void dfs(int x, int y){
        if(visited[x][y] || (sum(x) + sum(y) > k))
            return;
        visited[x][y] = true;
        count++;
        if(x - 1 >= 0) dfs(x-1,y);
        if(x + 1 < m) dfs(x+1,y);
        if(y - 1 >= 0) dfs(x,y-1);
        if(y + 1 < n) dfs(x,y+1);
    }
    int sum(int num){
        if(num < 10) return num;
        int res = 0;
        while(num > 0){
            res += num % 10;
            num /= 10;
        }
        return res;
    }
}
```

### 优化

![leetcode_机器人的运动范围](/HYCBlog/assets/img/leetcode/leetcode_机器人的运动范围.png)

仅通过向右和向下移动，可以访问所有可达解。

### BFS

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited = new boolean[m][n];
        int res = 0;
        Queue<int[]> queue= new LinkedList<int[]>();
        queue.add(new int[] { 0, 0, 0, 0 });
        while(queue.size() > 0) {
            int[] x = queue.poll();
            int i = x[0], j = x[1], si = x[2], sj = x[3];
            if(i >= m || j >= n || k < si + sj || visited[i][j]) continue;
            visited[i][j] = true;
            res ++;
            queue.add(new int[] { i + 1, j, (i + 1) % 10 != 0 ? si + 1 : si - 8, sj });
            queue.add(new int[] { i, j + 1, si, (j + 1) % 10 != 0 ? sj + 1 : sj - 8 });
        }
        return res;
    }
}

作者：jyd
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/solution/mian-shi-ti-13-ji-qi-ren-de-yun-dong-fan-wei-dfs-b/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





## 剑指 Offer 16. 数值的整数次方

实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

 

示例 1:

```java
输入: 2.00000, 10
输出: 1024.00000
```

示例 2:

```java
输入: 2.10000, 3
输出: 9.26100
```


示例 3:

```java
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```


说明:

* -100.0 < x < 100.0
* n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。



这题在数据精度上卡了一会，观察测试样例，结果的小数均保留最后5位。也就是说，小于等于+0.000009...9的数和大于-0.000009...9的数都返回0。

其他有意思的地方:

```java
x = -1.00000, n = -2147483648输出的结果是1
x = -1.00000, n = -2147483647输出的结果是-1
```

```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0 && n < 0) return Double.POSITIVE_INFINITY+5;
        if(n == 0)  return 1;
        if(x == 0)  return 0;
        if(x == -1 && n == -2147483648) return 1;
        if(x == -1 || x == 1)   return x; 
        if(n > 0){
            return pow(x,n);
        }else{
            return pow(1/x,-1*n);
        }
    }

    double pow(double x, int n){
        double res = 1;
        while(n > 0 && (res >= 0.000001 || res < -0.000001)){
            res *= x;
            n--;
        }
        return n == 0 ? res : 0;
    }
}
```

虽然ac了，但是感觉不是很妥，因为会超时，所以把数据精度缩减了，在这题会通过，但是下一次就没那么幸运了。看看[大佬的快速幂解法](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/solution/mian-shi-ti-16-shu-zhi-de-zheng-shu-ci-fang-kuai-s/)，学习一下，下回用这个解：

```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0) return 0;
        long b = n;
        double res = 1.0;
        if(b < 0) {
            x = 1 / x;
            b = -b;
        }
        while(b > 0) {
            if((b & 1) == 1) res *= x;
            x *= x;
            b >>= 1;
        }
        return res;
    }
}
```







## 剑指 Offer 20. 表示数值的字符串



请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"、"-1E-16"及"12e+5.4"都不是。



### 模式匹配

表示数值的字符串遵循模式：`A[.[B]][e|EC]`或`.[B][e|EC]`

其中，A表示整数部分，首位可为“+”或”-“，剩余位需为0~9间的数字。若数值为小数，此部分可省略。

B表示该数的小数部分，在"."之后，并且所有位均需在0~9之间。

e或E表示阶10，C表示阶数，其中C的要求与A相同，但不可省略。



```java
class Solution {
  	//全局指针，用以指向当前遍历的字符位置
    private int ptr = 0;
    public boolean isNumber(String s) {
				//字符串前后可存在空格，因此先将其去掉
        s = s.trim();
     	 	if(s.length() == 0) return false;
      	//首先判断A部分
        boolean numeric = scanInteger(s);
				//是否遍历完毕
        if(ptr > s.length()-1)
            return numeric;
        else{
          	//小数判断，即B部分
            if(s.charAt(ptr) == '.'){
                ptr++;
             //小数部分可为空，但是若情况为.1时，需要给numeric赋值，因为在第一轮判断A时赋值为false。
                numeric = scanUnsignedInteger(s) || numeric;
            }
          //是否遍历完毕
            if(ptr > s.length()-1)  return numeric;
            if(s.charAt(ptr) == 'e' || s.charAt(ptr) == 'E'){
                ptr++;
                numeric = numeric && scanInteger(s);
            }
          	//是否遍历完毕，若没有遍历完毕就返回了，则返回false。
            return numeric && ptr == s.length();
        }

    }
    boolean scanUnsignedInteger(String s){
        if(s.length() <= ptr) return false;
        int start = ptr;
        int i = start;
      	//将指针移出小数部分
        for(; i <= s.length()-1 && s.charAt(i) >= '0' && s.charAt(i) <= '9'; i++);

        ptr = i;

        return i > start;
    }
    boolean scanInteger(String s){
        if(s.length() <= ptr) return false;
        if(s.charAt(ptr) == '+' || s.charAt(ptr) == '-'){
            ptr++;
        }
        return scanUnsignedInteger(s);
    }
}
```





## 剑指 Offer 21. 调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

 

示例：

```java
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

提示：

```java
1 <= nums.length <= 50000
1 <= nums[i] <= 10000
```



### 用列表转存

```java
class Solution {
    public int[] exchange(int[] nums) {
        List<Integer> res = new ArrayList<>();
        for(int i = 0; i < nums.length; ++i){
            //is ood
            if(nums[i] % 2 != 0){
                res.add(nums[i]);
            }
        }
        for(int i = 0; i < nums.length; ++i){
            //is even
            if(nums[i] % 2 == 0){
                res.add(nums[i]);
            }
        }
        int j = 0;
        for(Integer i : res){
            nums[j++] = i;
        }
        return nums;
    }
}
```



### 双指针

left从左向右移动，遇到一个偶数就停止；

right从右向左移动，遇到一个奇数就停止；

若此时left < right，交换nums[left]和nums[right]。

若left == right，停止，输出结果。



```java
class Solution {
    public int[] exchange(int[] nums) {  
        int left = 0, right = nums.length-1;
        while(left < right){
            while(nums[left] % 2 != 0 && left < right) ++left;
            while(nums[right] % 2 == 0 && left < right) --right;
            if(left < right){
                int tmp = nums[left];
                nums[left] = nums[right];
                nums[right] = tmp;
            }                
        }     
        return nums;
    }
}
```





## 剑指 Offer 22. 链表中倒数第k个节点

双指针移动即可。特别注意边界判断！



```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        //链表为空，返回null
        //k值非正，返回null
        if(k <= 0) return null;
        if(head == null) return null;
        //初始化两个指针p,q，但q走了k步之后，p再开始走，q到达末尾后，返回p
        ListNode p,q;
        p = q = head;
        //边界判断，长度没有k，则返回null
        for(int i = 1; i < k; ++i)
            if(q.next != null)
                q = q.next;
            else
                return null;
        while(q.next != null){
            p = p.next;
            q = q.next;
        }
        return p;
    }
}
```





## 剑指 Offer 26. 树的子结构



输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

         3
        / \
       4   5
      / \
     1   2

给定的树 B：

```java
   4 
  /
 1
```




返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

示例 1：

```java
输入：A = [1,2,3], B = [3,1]
输出：false
```

示例 2：

```java
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```

限制：

```java
0 <= 节点个数 <= 10000
```

### 递归

若A或B有一者为空，则返回false。

判断以A树的当前节点为根节点时，是否存在子结构树B

判断以A树的左孩子为根节点时，是否存在子结构树B

判断以A树的右孩子为根节点时，是否存在子结构树B

三者有一种情况为true，则返回true

```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) { 
        if(B == null || A == null) return false;
        return isCompare(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B);
    }

    boolean isCompare(TreeNode A, TreeNode B){
        if(A == null)
            return false;
        if(A.val != B.val)
            return false;
        boolean res = true;
        if(B.left != null)
            res = isCompare(A.left,B.left);
        if(B.right != null)
            res = res && isCompare(A.right,B.right);
        return res;
    }
}
```



## 剑指 Offer 29. 顺时针打印矩阵



输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

 

示例 1：

```markdown
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

示例 2：

```markdown
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

限制：

```markdown
0 <= matrix.length <= 100
0 <= matrix[i].length <= 100
```



### 边界收缩

![leetcode_顺时针打印矩阵](/HYCBlog/assets/img/leetcode/leetcode_顺时针打印矩阵.png)

```java
public class Solution {
    public static int[] spiralOrder(int[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0) return new int[0];
				//初始化四个边界
        int left = 0,top = 0, right = matrix[0].length-1,bol = matrix.length-1,x=0;
      	//存放结果
        int[] res = new int[matrix.length * matrix[0].length];
        for(;;){
          	//从左到右
            for(int i = left; i <= right; ++i)
                res[x++] = matrix[top][i];
						//左边界是否超出右边界
            if(++top > bol) break;
						
          	//从上到下
            for(int i = top; i <= bol; ++i)
                res[x++] = matrix[i][right];
						//上边界是否超出下边界
            if(--right < left) break;
						
          	//从右到左
            for(int i = right; i >= left; --i)
                res[x++] = matrix[bol][i];
						
          	//右边界是否超出左边界
            if(--bol < top) break;
						//从下到上
            for(int i = bol; i >= top; --i)
                res[x++] = matrix[i][left];
						//下边界是否超出上边界
            if(++left > right)  break;
        }

        return res;

    }
    public static void main(String[] args) {
        int[][] matrix = new int[][]\{\{1,2,3},{4,5,6},{7,8,9\}\};
        spiralOrder(matrix);
    }
}
```



### 模拟

可以模拟打印矩阵的路径。初始位置是矩阵的左上角，初始方向是向右，当路径超出界限或者进入之前访问过的位置时，则顺时针旋转，进入下一个方向。

判断路径是否进入之前访问过的位置需要使用一个与输入矩阵大小相同的辅助矩阵 visited，其中的每个元素表示该位置是否被访问过。当一个元素被访问时，将visited 中的对应位置的元素设为已访问。

如何判断路径是否结束？由于矩阵中的每个元素都被访问一次，因此路径的长度即为矩阵中的元素数量，当路径的长度达到矩阵中的元素数量时即为完整路径，将该路径返回。

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return new int[0];
        }
        int rows = matrix.length, columns = matrix[0].length;
        boolean[][] visited = new boolean[rows][columns];
        int total = rows * columns;
        int[] order = new int[total];
        int row = 0, column = 0;
        int[][] directions = \{\{0, 1}, {1, 0}, {0, -1}, {-1, 0\}\};
        int directionIndex = 0;
        for (int i = 0; i < total; i++) {
            order[i] = matrix[row][column];
            visited[row][column] = true;
            int nextRow = row + directions[directionIndex][0], nextColumn = column + directions[directionIndex][1];
            if (nextRow < 0 || nextRow >= rows || nextColumn < 0 || nextColumn >= columns || visited[nextRow][nextColumn]) {
                directionIndex = (directionIndex + 1) % 4;
            }
            row += directions[directionIndex][0];
            column += directions[directionIndex][1];
        }
        return order;
    }
}
```



## 剑指 Offer 31. 栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

 

示例 1：

```java
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```


示例 2：

```java
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

提示：

```java
0 <= pushed.length == popped.length <= 1000
0 <= pushed[i], popped[i] < 1000
pushed 是 popped 的排列。
```



### 遍历出栈序列

遍历出栈序列的每个元素，将其在入栈序列中位于其前面的所有元素入辅助栈stack，每次判断栈顶元素是否等于第i个出栈序列元素，若不等，且该元素在栈中，则返回false

遍历结束则返回true

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        if(pushed.length <= 2)  return true;
        Stack<Integer> stack = new Stack<>();
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < popped.length; ++i){
          	//栈顶元素是否等于第i个出栈序列元素，若不等，且该元素在栈中，则返回false
            if(stack.search(popped[i]) != -1 && (stack.peek() != popped[i]))
                return false;
            for(int j = 0; pushed[j] != popped[i]; ++j){
              	//若此元素还未入栈，且也没有出栈，则将其放入栈中
                if(stack.search(pushed[j]) == -1 && !set.contains(pushed[j]))
                    stack.push(pushed[j]);
            }
            if(stack.search(popped[i]) != -1)
                stack.pop();
            set.add(popped[i]);
        }
        return true;
    }
}
```

### 遍历入栈序列

遍历入栈元素，将其依次入栈.每次入栈后，都循环判断栈顶元素是否等于出栈元素

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        if(pushed.length <= 2)  return true;
        Stack<Integer> stack = new Stack<>();
        int i = 0;
        for(int num : pushed){
            stack.push(num);
            
            while(!stack.isEmpty() && stack.peek() == popped[i]){
                stack.pop();
                ++i;
            }
            
        }
        return stack.isEmpty();
    }
}
```



## 剑指 Offer 39. 数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素.

示例 1:

```java
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```

限制：

```java
1 <= 数组长度 <= 50000
```



### 排序+双指针

```java
class Solution {
    public int majorityElement(int[] nums) {
        if(nums.length == 1) return nums[0];
      	//先排序
        Arrays.sort(nums);
      	//双指针搜索 长度大于数组长度一半 的重复子序列
        for(int low = 0, high =1; high < nums.length;){
            while(high < nums.length && nums[low] == nums[high]) ++high;
            if(high - low > (nums.length /2))
                return nums[low];
            low = high;
            high = low + 1;
        }
      	//随便return一个值
        return 0;
    }
}
```







## 剑指 Offer 40. 最小的k个数

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

示例 1：

```java
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

示例 2：

```java
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

限制：

```java
0 <= k <= arr.length <= 10000
0 <= arr[i] <= 10000
```



### 快速排序

```java
package leetcode;

public class SmallestK {
    public int[] getLeastNumbers(int[] arr, int k) {
        quickSort(arr,0,arr.length-1);
        int[] res = new int[k];
        for(int i = 0; i < k; ++i){
            res[i] = arr[i];
        }
        return res;
    }
    void quickSort(int[] arr, int low, int high){
        if(low < high){
            int pos = partition(arr,low,high);
            quickSort(arr,low,pos-1);
            quickSort(arr,pos+1,high);
        }

    }
    int partition(int[] arr, int low, int high){
        int pivot = arr[low];
        while(low < high){
            while(low < high && arr[high] >= pivot) high--;
            arr[low] = arr[high];
            while(low < high && arr[low] <= pivot) low++;
            arr[high] = arr[low];
        }
        arr[low] = pivot;
        return low;
    }
}

```



### 堆排序

```java
// 保持堆的大小为K，然后遍历数组中的数字，遍历的时候做如下判断：
// 1. 若目前堆的大小小于K，将当前数字放入堆中。
// 2. 否则判断当前数字与大根堆堆顶元素的大小关系，如果当前数字比大根堆堆顶还大，这个数就直接跳过；
//    反之如果当前数字比大根堆堆顶小，先poll掉堆顶，再将该数字放入堆中。
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (k == 0 || arr.length == 0) {
            return new int[0];
        }
        // 默认是小根堆，实现大根堆需要重写一下比较器。
        Queue<Integer> pq = new PriorityQueue<>((v1, v2) -> v2 - v1);
        for (int num: arr) {
            if (pq.size() < k) {
                pq.offer(num);
            } else if (num < pq.peek()) {
                pq.poll();
                pq.offer(num);
            }
        }
        
        // 返回堆中的元素
        int[] res = new int[pq.size()];
        int idx = 0;
        for(int num: pq) {
            res[idx++] = num;
        }
        return res;
    }
}

```



## 剑指 Offer 41. 数据流中的中位数



[点击跳转](/posts/priority-queue-src-code-analyze/)





## 剑指 Offer 42. 连续子数组的最大和

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

 

示例1:

```java
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

提示：

```java
1 <= arr.length <= 10^5
-100 <= arr[i] <= 100
```



### 动态规划

定义`dp[i]`为以第i个元素为末尾的最大子数组和。则当`dp[i-1]`为负数时，对`dp[i]`是负贡献，因此状态方程如下：

```java
dp[i] = 
					num[i] 						if dp[i-1] <= 0
  				dp[i-1] + num[i] 	if dp[i-1] > 0
```

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //dp[i]表示以第i个元素为结尾的最大子数组和
        if(nums.length == 0) return 0;
        if(nums.length == 1) return nums[0];

        int[] dp = new int[nums.length];
        int i = 1;
        dp[0] = nums[0];
        int max = dp[0];
        for(; i < dp.length; ++i){
            //若dp[i-1]<0则说明前i-1个元素对于第i个元素为结尾的子数组来说是负贡献，因此dp[i] = num[i]最佳
            if(dp[i-1] < 0)
                dp[i] = nums[i];
            else
                dp[i] = dp[i-1] + nums[i];
            max = max < dp[i] ? dp[i] : max;
        }
        return max;

    }
}
```





### 扩展：最长单调递增子序列

```java
int maxSubSeq(int[] a){
  int n = a.length;
  int[] b = new int[n];
  for(int i = 0; i < n; ++i)
    b[i] = 1;
  
  int max = 1;
  //遍历所有子序列
  for(int i = 0; i < n ++i){
    for(int j = 0; j < i; ++j){
      if(a[i]> a[j])
        b[i] = (b[i-1] > b[j] + 1) > b[i-1] :(b[j] + 1);
      max = Math.max(max,b[i]);
    }
  }
  return max;
}
```





## 剑指 Offer 44. 数字序列中某一位的数字

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

 

示例 1：

```java
输入：n = 3
输出：3
```


示例 2：

```java
输入：n = 11
输出：0
```

限制：

```java
0 <= n < 2^31
```



```java
package leetcode;

/**
 *  0～9贡献了      10 * 1个字符   k = 0
 *  10～99贡献了    90 * 2个字符   k = 1
 *  100～999贡献了  900 * 3个字符  k = 2
 *  以此类推，容易推出第k个区间贡献的字符数：9 * 10^k * (k+1)
 *  目标就是将数字n映射到上面的某个区间内，那么就要找到以下几个值：
 *  k       标识n落在区间[10^k, 9..9(k+1个9)]
 *  blk_i   标识n这个数字对应于这个区间内的第几个数。（块索引）
 *  inner_i 标识第n个字符是在这个blk_i块（即一个数字）的第几位。（块内索引）
 */
public class NthDigitInSequence {
    public int findNthDigit(int n) {
        //前10个字符直接返回
        if(n < 10)  return n;
        //当前字符序列的序列长度
        int sum = 10;
        //用于标识目标数字所在区间，start元素的后缀0个数
        //例如[10~99]区间，start元素为10，后缀0个数为1
        int k = 1;
        //找到n所在的区间
        for(k = 1; sum < n; ++k){
            if(n - 9 * Math.pow(10,k)*(k+1) < 0)
                break;
            sum += 9 * Math.pow(10,k)*(k+1);
        }
        n -= sum;

        //块索引
        int blk_i = n / (k+1);
        //块内索引
        int inner_i = n % (k+1);
        //利用块索引找到目标数字
        int target = (int)Math.pow(10,k) + blk_i;
        String target_s = target + "";
        //利用块内索引返回序列的第n个字符
        return target_s.charAt(inner_i) - '0';

    }

    public static void main(String[] args) {
        int res = new NthDigitInSequence().findNthDigit(21121);
//        System.out.println(res);
    }
}

```





## 剑指 Offer 46. 把数字翻译成字符串

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

 

示例 1:

```java
输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```


提示：

0 <= num < 2^31



### dfs解法

遍历每一种有效情况。

```java
class Solution {
    int cnt;
    public int translateNum(int num) {
        if(num < 10)    return 1;
        cnt = 0;
        dfs(num+"", 0);
        return cnt;
    }
		//start为此次搜索的起点
    void dfs(String num, int start){
      	//搜索到结尾就计数
        if(start >= num.length()){
            cnt++;
            return;
        }
      	//1个字母肯定成立
        dfs(num,start+1);
      	//两个字母需要判断是否在区间[10,25]内，因为前导0不符合要求
        if(start + 1 < num.length() &&
            num.substring(start,start+2).compareTo("25") <= 0
            &&num.substring(start,start+2).compareTo("10") >= 0){
                dfs(num,start+2);
            }
    }
}
```

### 动态规划解法

```markdown
设x1x2...xi-2的翻译方案数量为f(i-2)
设x1x2...xi-1的翻译方案数量为f(i-1)
当整体翻译xi-1xi时，x1x2...xi-2xi-1xi的方案数为f(i-2)
当单独翻译xi时，x1x2...xi-2xi-1xi的方案数为f(i-1)
所以有如下状态方程：
			｜--  f(i-2) + f(i-1),	若xi-1xi可被整体翻译
f(i) =｜
			｜--  f(i-1)，若数字xi-1xi不可被翻译
```

```java
class Solution {

    public int translateNum(int num) {
        if(num < 10)    return 1;
        String s = num + "";
        int[] dp = new int[s.length()+1];
        dp[0] = 1;
        dp[1] = 1;
        
        for(int i = 2; i <= s.length(); ++i){
            if(s.substring(i-2,i).compareTo("25") <= 0
            &&s.substring(i-2,i).compareTo("10") >= 0){
                dp[i] = dp[i-1] + dp[i-2];
            }else{
                dp[i] = dp[i-1];
            }
        }
        return dp[s.length()];
        
    }
}
```

### 数字求余

```java
class Solution {
    public int translateNum(int num) {
        int a = 1, b = 1, x, y = num % 10;
        while(num != 0) {
            num /= 10;
            x = num % 10;
            int tmp = 10 * x + y;
            int c = (tmp >= 10 && tmp <= 25) ? a + b : a;
            b = a;
            a = c;
            y = x;
        }
        return a;
    }
}
```









## 剑指 Offer 47. 礼物的最大价值

在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

 

示例 1:

```java
输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

提示：

```java
0 < grid.length <= 200
0 < grid[0].length <= 200
```

### 动态规划



```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(i == 0 && j == 0)
                    continue;
                if(i == 0)
                    dp[i][j] = dp[i][j-1] + grid[i][j];
                else if(j == 0)
                    dp[i][j] = dp[i-1][j] + grid[i][j];
                else{
                    dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]) + grid[i][j];
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```



将i==0和j==1的情况提前输入，可减少判断次数



```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];

        for(int i = 1; i < n; ++i){
            dp[0][i] = dp[0][i-1] + grid[0][i];
        }
        for(int i = 1; i < m; ++i){
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }

        for(int i = 1; i < m; ++i){
            for(int j = 1; j < n; ++j){
                dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]) + grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
}
```





## 剑指 Offer 48. 最长不含重复字符的子字符串

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

 

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
```


请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。


提示：

`s.length <= 40000`

### 动态规划+线性遍历

dp[i]表示以第i个字母为结尾的最长子串大小。用一个变量替代dp数组。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int len = s.length();
        if(len <= 1) return len;

        int max = 0;
        
        for(int i = 0; i < len; ++i){
            Set<Character> set = new HashSet<>();
            int j = i;
            while(j >= 0 && !set.contains(s.charAt(j))){
                set.add(s.charAt(j));
                --j;
            }
            max = Math.max(max,set.size());
        }
        return max;
    }
}
```

### 哈希表+双指针

遍历整个字符串，将每个元素依次存入哈希表中，以元素值为键，下标为值。

在遍历过程中，若该元素已在哈希表中出现，则更新左指针为上一次出现的位置。

维护右指针为当前遍历元素的下标。记录左右指针差最大的即为结果。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int len = s.length();
        if(len <= 1) return len;
        int max = 0;
        Map<Character,Integer> map = new HashMap<>();
        int i = -1;
        for(int j = 0; j < len; ++j){
            if(map.containsKey(s.charAt(j)))
                i = Math.max(map.get(s.charAt(j)),i);
            map.put(s.charAt(j),j);
            max = Math.max(max,j-i);
            
        }
        return max;
    }
}
```





## 剑指 Offer 50. 第一个只出现一次的字符

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

示例:

```java
s = "abaccdeff"
返回 "b"
s = "" 
返回 " "
```

限制：

```java
0 <= s 的长度 <= 50000
```



### 统计所有出现1次的字符

从头到尾遍历字符串，将所有的出现1次的字符依次放入列表中，最后返回列表头元素。

```java
class Solution {
    public char firstUniqChar(String s) {
        if(s.equals("")) return ' ';
        List<Integer> sl = new LinkedList<>();
        Set<Integer> ss = new HashSet<>();
        for(int i = 0; i < s.length(); ++i){
            if(!ss.contains(s.charAt(i) - 'a')){
                ss.add(s.charAt(i) - 'a');
                sl.add(s.charAt(i) - 'a');
            }else if(sl.contains(s.charAt(i)- 'a')){
                sl.remove(sl.indexOf(s.charAt(i)- 'a'));
            }
        }
        return (sl.size() == 0) ? ' ' : (char)(sl.get(0) + 'a');
    }
}
```



### 哈希表

```java
class Solution {
    public char firstUniqChar(String s) {
        if(s.equals("")) return ' ';
        
        int[] hash = new int[26];

        for(int i = 0; i < s.length() ; ++i){
            hash[s.charAt(i) - 'a']++;
        }
        //遍历字符串，找到第一个哈希值为1的字符返回它
        for(int i = 0; i < s.length() ; ++i){
            if(hash[s.charAt(i) - 'a'] == 1)
                return s.charAt(i);
        }
        return ' ';
    }
}
```



### 有序哈希表

```java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character, Boolean> dic = new LinkedHashMap<>();
        char[] sc = s.toCharArray();
        for(char c : sc)
            dic.put(c, !dic.containsKey(c));
        for(Map.Entry<Character, Boolean> d : dic.entrySet()){
           if(d.getValue()) return d.getKey();
        }
        return ' ';
    }
}

```





## 剑指 Offer 53 - II. 0～n-1中缺失的数字

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

 

示例 1:

```java
输入: [0,1,3]
输出: 2
```


示例 2:

```java
输入: [0,1,2,3,4,5,6,7,9]
输出: 8
```

限制：

```java
1 <= 数组长度 <= 10000
```



### 循环遍历

找到下标与值不对应的第一个数，返回；否则返回数组长度。



```java
class Solution {
    public int missingNumber(int[] nums) {
        for(int i = 0; i < nums.length; ++i){
            if(nums[i] != i)	return i;
        }
        return nums.length;
    }
}
```



### 二分

```java
class Solution {
    public int missingNumber(int[] nums) {
        int i = 0, j = nums.length - 1;
        while(i <= j) {
            int m = (i + j) / 2;
          	//判断数组元素和下标是否相等，若相等继续往后搜索
            if(nums[m] == m) i = m + 1;
          	//不相等就往前搜索
            else j = m - 1;
        }
        return i;
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



### 分组异或

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



## 剑指 Offer 56 - II. 数组中数字出现的次数 II

在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

 

示例 1：

```java
输入：nums = [3,4,3,3]
输出：4
```


示例 2：

```java
输入：nums = [9,1,7,9,7,9,7]
输出：1
```



限制：

```java
1 <= nums.length <= 10000
1 <= nums[i] < 2^31
```

### 暴力解法

```java
class Solution {
    public int singleNumber(int[] nums) {
        if(nums.length == 1) return nums[0];
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums.length; ++i){
            if(!set.contains(nums[i])){
                boolean flag = false;
                for(int j = i + 1; j < nums.length; ++j){
                    if(nums[i] == nums[j]){
                        set.add(nums[i]);
                        flag = true;
                        break;
                    }
                }
                if(!flag)
                    return nums[i];
            }
        }
        return nums[nums.length-1];
    }
}
```

### 有限状态自动机

考虑数字的二进制形式，对于出现三次的数字，各二进制位出现的次数都是**3的倍数**。

因此，统计所有数字的各二进制位中1出现次数，并**对3求余**，结果则为只出现一次的数字。

位运算是并行的，因此只需考虑一位。对于所有数字中的二进制位 1 的个数，存在 3 种状态，即对 3 余数0,1,2 。状态转移如下：

```markdown
输入1:
	0->1->2->0
输入0:
	0->0,1->1,2->2
以二进制代表上述3种状态：00，01，10
高位为high, 低位为low

计算low:
if high == 0:
	if in == 0:
		low = low
	if in == 1:
		low = ~low
if high == 1:
	low = 0

引入异或，简化为：
if high == 0:
	low = low ^ n
if high == 1:
	low = 0
引入与，简化为：
low = low ^ n & ~high

由于先计算了low，所以应该在新low的基础上计算high
计算high:
high = high ^ n & ~low
```

```java
class Solution {
    public int singleNumber(int[] nums) {
      	//一个数字与0异或，值不改变
        int low = 0, high = 0;
        for(int num : nums){
            low = low ^ num & ~high;
            high = high ^ num & ~low;
        }
        return low;
    }
}
```

###  遍历统计

![leetcode_数组中数字出现的次数_遍历统计](/HYCBlog/assets/img/leetcode/leetcode_数组中数字出现的次数_遍历统计.png)

```java
class Solution {
    public int singleNumber(int[] nums) {
        int[] counts = new int[32];
        for(int num : nums) {
            for(int j = 0; j < 32; j++) {
                counts[j] += num & 1;
                num >>>= 1;
            }
        }
        int res = 0, m = 3;
        for(int i = 0; i < 32; i++) {
            res <<= 1;
            res |= counts[31 - i] % m;
        }
        return res;
    }
}
```

[参考](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/solution/mian-shi-ti-56-ii-shu-zu-zhong-shu-zi-chu-xian-d-4/)






## 剑指 Offer 58 - I. 翻转单词顺序

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

 

示例 1：

```java
输入: "the sky is blue"
输出: "blue is sky the"
```

示例 2：

```java
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```


示例 3：

```java
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```




说明：

* 无空格字符构成一个单词。
* 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
* 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。



### 找到所有的空格替换

```java
package leetcode;

public class PartialReverseString {

    public String reverseWords(String s) {
        s = s.trim();
        if(s.length() == 0) return "";
        String reverse = "";
        for(int i = 0; i < s.length();){
            if(s.charAt(i) != ' '){
                int end = s.indexOf(" ",i);
                if(end == -1) end = s.length();
                reverse = s.substring(i,end) + " " + reverse;
                i = end;
            }else {
                ++i;
            }
        }
        return reverse.trim();
    }

}
```



### 双指针

```java
class Solution {
    public String reverseWords(String s) {
        s = s.trim(); // 删除首尾空格
        int j = s.length() - 1, i = j;
        StringBuilder res = new StringBuilder();
        while(i >= 0) {
            while(i >= 0 && s.charAt(i) != ' ') i--; // 搜索首个空格
            res.append(s.substring(i + 1, j + 1) + " "); // 添加单词
            while(i >= 0 && s.charAt(i) == ' ') i--; // 跳过单词间空格
            j = i; // j 指向下个单词的尾字符
        }
        return res.toString().trim(); // 转化为字符串并返回
    }
}
```



### 分割+倒序

```java
class Solution {
    public String reverseWords(String s) {
        String[] strs = s.trim().split(" "); // 删除首尾空格，分割字符串
        StringBuilder res = new StringBuilder();
        for(int i = strs.length - 1; i >= 0; i--) { // 倒序遍历单词列表
            if(strs[i].equals("")) continue; // 遇到空单词则跳过
            res.append(strs[i] + " "); // 将单词拼接至 StringBuilder
        }
        return res.toString().trim(); // 转化为字符串，删除尾部空格，并返回
    }
}
```



## 剑指 Offer 59 - I. 滑动窗口的最大值

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

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

```java
你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。
```



### 暴力法

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0)
            new int[0];
        List<Integer> list = new ArrayList<>();
        for(int i = 0, j = k-1; j < nums.length;){
            int max = nums[i];
            for(int m = i; m <= j; ++m)
                max = Math.max(nums[m],max);
            list.add(max);
            ++i;
            ++j;
        }
        int[] res = list.stream().mapToInt(Integer::valueOf).toArray();
        
        return res;
    }
}
```

### 双端队列

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0 || k == 0) return new int[0];
        Deque<Integer> deque = new LinkedList<>();
        int[] res = new int[nums.length - k + 1];
        for(int i = 0; i < k; i++) { // 未形成窗口
            while(!deque.isEmpty() && deque.peekLast() < nums[i])
                deque.removeLast();
            deque.addLast(nums[i]);
        }
      	//上面的循环结束时，已经形成了第一个窗口
        res[0] = deque.peekFirst();
        for(int i = k; i < nums.length; i++) { // 形成窗口后
          	//如果最大值在窗口的左边缘，则将其移除
            if(deque.peekFirst() == nums[i - k])
                deque.removeFirst();
          	//将队列中，小于nums[i]的元素都移除
            while(!deque.isEmpty() && deque.peekLast() < nums[i])
                deque.removeLast();
          	//存入队列
            deque.addLast(nums[i]);
          	//取出这轮的最大值，即队首元素
            res[i - k + 1] = deque.peekFirst();
        }
        return res;
    }
}

作者：jyd
链接：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/mian-shi-ti-59-i-hua-dong-chuang-kou-de-zui-da-1-6/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 剑指 Offer 59 - II. 队列的最大值



请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

若队列为空，pop_front 和 max_value 需要返回 -1

示例 1：

```markdown
输入: 
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
输出: [null,null,null,2,1,2]
```

示例 2：

```markdown
输入: 
["MaxQueue","pop_front","max_value"]
[[],[],[]]
输出: [null,-1,-1]
```

限制：

```markdown
1 <= push_back,pop_front,max_value的总操作数 <= 10000
1 <= value <= 10^5
```





```java
class MaxQueue {

    int[] queue;
    int front;
    int tail;
    int max_pos;


    public MaxQueue() {
        queue = new int[10001];
        front = 0;
        tail = -1;
        max_pos = 0;
    }

    public int max_value() {
        if(front > tail) return -1;
        int max = queue[front];
        max_pos = front;
        for(int i = front; i <= tail; ++i)
            if(max <= queue[i]){
                max = queue[i];
                max_pos = i;
            }
        return queue[max_pos];
    }

    public void push_back(int value) {
        tail = (tail + 1);
        queue[tail] = value;
    }

    public int pop_front() {
        if(front > tail) return -1;
        int res = queue[front];
        front = (front+1);
        return res;
    }

    public static void main(String[] args) {
        MaxQueue maxQueue = new MaxQueue();
        String[] instructions = {
                "max_value","pop_front","max_value","push_back",
                "max_value","pop_front","max_value","pop_front","push_back",
                "pop_front","pop_front", "pop_front","push_back","pop_front",
                "max_value","pop_front","max_value","push_back", "push_back",
                "max_value","push_back","max_value","max_value","max_value"};

        int[] values = {0,0,0,46,0,0,0,0,868,0,0,0,525,0,0,0,0,123,646,0,229,0,0,0};

        for(int i = 0; i < instructions.length; ++i){
            switch (instructions[i]){
                case "max_value":
                    System.out.print(maxQueue.max_value()+",");
                    break;
                case "pop_front":
                    System.out.print(maxQueue.pop_front()+",");
                    break;
                case "push_back":
                    System.out.print("null,");
                    maxQueue.push_back(values[i]);
                default:
                    break;
            }
        }
    }
}
```



## 剑指 Offer 63. 股票的最大利润

假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

 

示例 1:

```java
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```




示例 2:

```java
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```


限制：

`0 <= 数组长度 <= 10^5`

### 动态规划

dp[i]表示以第i次价格卖出的利润，若为负则归零.由于只卖出一次，用一个变量替代数组。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length <= 1) return 0;

        //profit表示以第i次价格卖出的利润，若为负则归零
        int profit = 0;
        int maxProfit = 0;
        for(int i = 1; i < prices.length; ++i){
            int min = prices[i];
            for(int j = 0; j < i; ++j){
                if(prices[j] <= prices[i]){
                    min = Math.min(prices[j],min);
                }
            }
            profit = prices[i] - min;
            maxProfit = Math.max(profit,maxProfit);
        }

        return maxProfit;
    }
}
```



### 动态规划优化

用一个变量记录这一天之前的最低价格，即可优化时间复杂度。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length <= 1) return 0;

        int maxProfit = 0;
        int minPrice = Integer.MAX_VALUE;
        for(int i = 0; i < prices.length; ++i){
            minPrice = Math.min(minPrice,prices[i]);
            maxProfit = Math.max(prices[i] - minPrice, maxProfit);
        }

        return maxProfit;
    }
}
```



## 剑指 Offer 64. 求1+2+…+n

求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

 

示例 1：

```java
输入: n = 3
输出: 6
```

示例 2：

```java
输入: n = 9
输出: 45
```

限制：

```java
1 <= n <= 10000
```



### 利用位运算符的短路性质

![leetcode_求1+2+...+n_1](/HYCBlog/assets/img/leetcode/leetcode_求1+2+...+n_1.png)

```java
class Solution {
    public int sumNums(int n) {
        boolean f = (n > 0) && (n += sumNums(n-1)) > 0;
        return n;
    }
}
```

或

```java
class Solution {
    public int sumNums(int n) {
        boolean f = (n == 0) || (n += sumNums(n-1)) < 0;
        return n;
    }
}
```



### 快速乘

![leetcode_求1+2+...+n_2](/HYCBlog/assets/img/leetcode/leetcode_求1+2+...+n_2.png)

```java
class Solution {
public:
    int sumNums(int n) {
        int ans = 0, A = n, B = n + 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        return ans >> 1;
    }
};
```



### 使用IntStream（JDK8）

```java
class Solution {
    public int sumNums(int n) {
        return IntStream.range(1,n+1).sum();
      	//或者
      	//return IntStream.rangeClosed(1,n).sum();
    }
}
```



## 剑指 Offer 66. 构建乘积数组

给定一个数组` A[0,1,…,n-1]`，请构建一个数组` B[0,1,…,n-1]`，其中 B 中的元素 `B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]`。不能使用除法。

 

示例:

```java
输入: [1,2,3,4,5]
输出: [120,60,40,30,24]
```

提示：

```java
所有元素乘积之和不会溢出 32 位整数
a.length <= 100000
```



### 暴力+记忆化

直接暴力计算会有很多重复计算的过程，因此将A0~Ai的乘积存放在`p_front[i]`中，将An-1~Ai的乘积存放在`p_tail[i]`中，有`b[i] = p_front[i-1] * p_tail[i+1]`。

```java
class Solution {
    public int[] constructArr(int[] a) {
        if(a.length <= 1) return a;
        int n = a.length;
        int[] p_front = new int[n];
        int[] p_tail = new int[n];
        //将A0到Ai的乘积放在数组p_front[i]中
        p_front[0] = a[0];
        for(int i = 1; i < n; ++i){
            p_front[i] = a[i] * p_front[i-1];
        }

        //将An-1到Ai的乘机放在数组p_tail[i]中
        p_tail[n-1] = a[n-1];
        for(int i = n-2; i >=0; --i){
            p_tail[i] = a[i] * p_tail[i+1];
        }

        //计算b[i]
        int[] b = new int[n];
        b[0] = p_tail[1];
        b[n-1] = p_front[n-2];
        for(int i = 1; i < n-1; ++i){
            b[i] = p_front[i-1] * p_tail[i+1];
        }
        return b;
    }
}
```



