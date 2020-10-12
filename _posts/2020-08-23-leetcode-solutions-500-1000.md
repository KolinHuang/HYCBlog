---
title: Leetcode题解:500-1000
author: Kol Huang
date: 2020-08-23 20:44:00 +0800
categories: [Blogging, leetcode]
tags: [算法题解]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/leetcode_cover.jpg
pin: true
---

## 目录

[501.二叉搜索树中的众数](#jump501)

[529.扫雷游戏](#jump529)

[530.二叉搜索树的最小绝对差](#jump530)

[538.将二叉搜索树转换为累加树](#jump538)

[546.移除盒子](#jump546)

[557.反转字符串中的单词 III](#jump557)

[617.合并二叉树](#jump617)

[637.二叉树的层平均值](#jump637)

[647.回文子串](#jump647)

[657.机器人能否返回原点](#jump657)

[679.24点游戏](#jump679)

[685.冗余连接2](#jump685)

[696.计数二进制子串](#jump696)

[701.二叉搜索树中的插入操作tag](jump701)

[711.宝石和石头](#jump711)

[718.最长公共连续子数组](#jump718)

[733.图像渲染](#jump733)

[834.树中距离之和tag](#jump834)

[841.钥匙和房间](#jump841)

[968.监控二叉树](#jump968)





<span id = "jump501"></span>

## 501.二叉搜索树中的众数

给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

假定 BST 有如下定义：

* 结点左子树中所含结点的值小于等于当前结点的值
* 结点右子树中所含结点的值大于等于当前结点的值
* 左子树和右子树都是二叉搜索树

例如：

给定 BST [1,null,2,2],

```java
   1
    \
     2
    /
   2
```


返回[2].

提示：如果众数超过1个，不需考虑输出顺序

进阶：你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）



二叉搜索树的中序遍历是非严格升序，所以先用中序遍历一遍，拿到升序数组，再用哈希表找到数组中的众数。

```java
class Solution {
    int max_times = 0;
    public int[] findMode(TreeNode root) {
        Map<Integer,Integer> map = new HashMap<>();
        List<Integer> list = new ArrayList<>();

        inOrder(root,list);
        for(int i = 0; i < list.size(); ++i){
            if(!map.containsKey(list.get(i)))
                map.put(list.get(i),0);
            map.put(list.get(i),map.get(list.get(i)) + 1);
        }
        list.clear();
        for(Integer key : map.keySet()){
            if(map.get(key) > max_times){
                max_times = map.get(key);
                list.clear();
                list.add(key);
            }else if(map.get(key) == max_times){
                list.add(key);
            }
        }
        int[] res = new int[list.size()];
        for(int i = 0; i < list.size(); ++i){
            res[i] = list.get(i);
        }
        return res;
    }

    void inOrder(TreeNode root, List<Integer> list){
        if(root != null){
            inOrder(root.left,list);

            list.add(root.val);
            
            inOrder(root.right,list);
        }
    } 
}
```

进阶：直接在中序遍历过程中统计众数，为了不使用额外空间，遍历树两次，第一次记录结果集的长度，第二次记录结果集。

```java
class Solution {
    int max_times = 0;	//众数的出现次数
    int times = 0;	//当前数字的出现次数
    int res_length = 0;	//结果集的长度
    int[] res;	//结果集，第一次遍历时设空
    TreeNode pre = null;	//上一个节点
    public int[] findMode(TreeNode root) {
        if(root == null)    return new int[0];
        inOrder(root);
        res = new int[res_length];
        pre = null;
        times = 0;
        res_length = 0;
        inOrder(root);
        
        return res;
    }

    void inOrder(TreeNode root){
        if(root != null){
            inOrder(root.left);

            if(pre != null && pre.val == root.val)
                times++;
            else
                times = 1;
            
            if(times > max_times){
                max_times = times;
                res_length = 1;
            //max_times在第一次遍历中会记录最大的次数，在第二次遍历中用于判断哪些数字是众数
            }else if(times == max_times){
              	//当前是第二次遍历的话，就记录结果集
                if(res != null){
                    res[res_length] = root.val;
                }
                res_length++;
            }
            pre = root;
            inOrder(root.right);
        }
    } 
}
```











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





<span id = "jump530"></span>

## 530.二叉搜索树的最小绝对差

给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。

 

示例：

```java
输入：

   1
    \
     3
    /
   2

输出：
1

解释：
最小绝对差为 1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
```


提示：

* 树中至少有 2 个节点。



中序遍历：

```java
class Solution {
    TreeNode pre;
    int min = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        if(root == null)    return min;
        inOrder(root);
        return min;
    }

    void inOrder(TreeNode root){
        if(root != null){
            inOrder(root.left);
            if(pre != null){
                min = Math.min(min, Math.abs(root.val - pre.val));
            }
            pre = root;
            inOrder(root.right);
        }
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













<span id = "jump637"></span>

## 637.二叉树的层平均值

给定一个非空二叉树, 返回一个由每层节点平均值组成的数组。

 

示例 1：

```java
输入：
    3
   / \
  9  20
    /  \
   15   7
输出：[3, 14.5, 11]
解释：
第 0 层的平均值是 3 ,  第1层是 14.5 , 第2层是 11 。因此返回 [3, 14.5, 11] 。
```


提示：

* 节点值的范围在32位有符号整数范围内。



层序遍历

```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<>();
        if(root == null)    return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            double sum = 0;
            for(int i = 0; i < size; ++i){
                TreeNode tmp = queue.poll();
                sum += tmp.val;
                if(tmp.left != null)    queue.offer(tmp.left);
                if(tmp.right != null)    queue.offer(tmp.right);
            }
            res.add(sum/size);
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







<span id = "jump685"></span>

## 685.冗余连接 II

在本问题中，有根树指满足以下条件的有向图。该树只有一个根节点，所有其他节点都是该根节点的后继。每一个节点只有一个父节点，除了根节点没有父节点。

输入一个有向图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。 每一个边 的元素是一对 [u, v]，用以表示有向图中连接顶点 u 和顶点 v 的边，其中 u 是 v 的一个父节点。

返回一条能删除的边，使得剩下的图是有N个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

示例 1:

```java
输入: [[1,2], [1,3], [2,3]]
输出: [2,3]
解释: 给定的有向图如下:
  1
 / \
v   v
2-->3
```


示例 2:

```java
输入: [[1,2], [2,3], [3,4], [4,1], [1,5]]
输出: [4,1]
解释: 给定的有向图如下:
5 <- 1 -> 2
     ^    |
     |    v
     4 <- 3
```


注意:

* 二维数组大小的在3到1000范围内。
* 二维数组中的每个整数在1到N之间，其中 N 是二维数组的大小。


原始图有是由一颗有根树添加一条附加边而来，那么这个图中的边数等于图的顶点数。

树中的每个节点都有一个父节点，除了根节点没有父节点。在多了一条附加的边之后，可能有以下两种情况：

* 附加的边指向**根节点**，则包括根节点在内的每个节点都有一个父节点，此时图中一定有环路；
* 附加的边指向**非根节点**，则恰好有一个节点（即被附加的边指向的节点）有两个父节点，此时图中可能有环路也可能没有环路。

要找到附加的边，需要遍历图中的所有的边构建出一棵树，在构建树的过程中寻找导致**冲突（即导致一个节点有两个父节点）**的边以及**导致环路**出现的边。

具体做法是，使用数组`parent `记录每个节点的父节点，初始时对于任何 `1≤i≤N `都有`parent[i]=i`，另外创建并查集，初始时并查集中的每个节点都是一个连通分支，该连通分支的根节点就是该节点本身。遍历每条边的过程中，维护导致冲突的边和导致环路出现的边，由于只有一条附加的边，因此**最多有一条导致冲突的边和一条导致环路出现的边。**

当访问到边 `[u,v]` 时，进行如下操作：

* 如果此时已经有`parent[v] !=v`，说明 v 有两个父节点，将当前的边 `[u,v]` 记为导致冲突的边；
* 否则，令`parent[v]=u`，然后在并查集中分别找到`u `和 `v` 的祖先（即各自的连通分支中的根节点），如果祖先相同，说明这条边导致环路出现，将当前的边 `[u,v]`记为导致环路出现的边，如果祖先不同，则在并查集中将 `u` 和 `v `进行合并。
* 根据上述操作，同一条边不可能同时被记为导致冲突的边和导致环路出现的边。



遍历所有边之后，根据是否存在导致冲突的边和导致环路的边，来得到附加的边。

* 如果没有导致冲突的边，说明附加的边一定导致环路出现，而且是在环路中的最后一条被访问到的边，因此附加的边即为导致环路出现的边。
* 如果有导致冲突的边，记这条边为 `[u,v]`，则有两条边指向 `v`，另一条边为 `[parent[v],v]`，需要通过判断是否有导致环路的边决定哪条边是附加的边。
  * 如果有导致环路的边，则附加的边不可能是 `[u,v]`（因为 `[u,v]` 已经被记为导致冲突的边，不可能被记为导致环路出现的边），因此附加的边是 `[parent[v],v]`。
  * 如果没有导致环路的边，则附加的边是后被访问到的指向` v` 的边，因此附加的边是 `[u,v]`。

```java
class Solution {
    public int[] findRedundantDirectedConnection(int[][] edges) {
        int[] res = new int[2];
        int N = edges.length;
        UnionFind uf = new UnionFind(N+1);
        int[] parent = new int[N+1];
        for(int i = 1;i <= N; ++i){
            parent[i] = i;
        }
        //导致冲突的边
        int conflict = -1;
        //导致环路的边
        int cycle = -1;
        //遍历所有边
        for(int i = 0; i < N; ++i){
            int u = edges[i][0];
            int v = edges[i][1];
            //若此时这条边的子节点的父节点早就记录过了，所以v除了u之外，还有一个父节点，记录为导致冲突的边
            if(parent[v] != v){
                conflict = i;
            }else {
                //否则就更新v的父节点
                parent[v] = u;
                //如果u和v的父节点相同，则说明这条边导致了环路
                if(uf.find(u) == uf.find(v)){
                    cycle = i;
                }else {
                    //将这条边加入并查集中
                    uf.union(u,v);
                }

            }
        }
        //没有发生冲突
        if(conflict < 0){
            //那么附加的边一定是导致环路出现的边，而且是最后一条导致环路的边
            res[0] = edges[cycle][0];
            res[1] = edges[cycle][1];
            return res;
        }else {
            //有发生了冲突的边(u,v)，说明节点v有两个父节点，分别是u和parent[v]
            //那么附加边就是(u,v)和(parent[v],v)的其中一条

            //有导致环路的边，那么附加的边不可能是(u,v),因为(u,v)已经被记为导致冲突的边了，不可能被记为导致环路的边
            if(cycle >= 0){
                res[0] = parent[edges[conflict][1]];
                res[1] = edges[conflict][1];
            }else {
                //没有导致环路的边，那么附加的边就是导致冲突的变
                res[0] = edges[conflict][0];
                res[1] = edges[conflict][1];
            }
            return res;
        }
    }
}
class UnionFind{
    int[] pre;
    public UnionFind(int n){
        pre = new int[n];
        for(int i = 0; i < n; ++i){
            pre[i] = i;
        }
    }

    //顺带做了路径压缩，最后把x的父节点修改为了根节点
    public int find(int x){
        if(pre[x] != x){
            pre[x] = find(pre[x]);
        }
        return pre[x];
    }

    public void union(int a, int b){
        pre[find(a)] = find(b);
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





<span id = "jump701"></span>

## 701.二叉搜索树中的插入操作[tag]

给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据保证，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回任意有效的结果。

 

例如, 

给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和 插入的值: 5
你可以返回这个二叉搜索树:

         4
       /   \
      2     7
     / \   /
    1   3 5

或者这个树也是有效的:

         5
       /   \
      2     7
     / \   
    1   3
         \
          4


提示：

* 给定的树上的节点数介于 0 和 10^4 之间
* 每个节点都有一个唯一整数值，取值范围从 0 到 10^8
* -10^8 <= val <= 10^8
* 新值和原始二叉搜索树中的任意节点值都不同





递归插入即可。如果说是插入到一个平衡的二叉搜索树，就没这么简单了！打个tag，改天试试！

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        
        if(root == null){
            TreeNode node = new TreeNode(val);
            return node;
        }
        insert(root,val);
        return root;

    }

    void insert(TreeNode root, int val){
        if(root != null){
            if(root.val > val){
                if(root.left == null){
                    TreeNode node = new TreeNode(val);
                    root.left = node;
                }else{
                    insert(root.left,val);
                }
            }
            else{
                if(root.right == null){
                    TreeNode node = new TreeNode(val);
                    root.right = node;
                }else{
                    insert(root.right,val);
                }
            }
                
        }
    }
}
```







<span id = "jump771"></span>

## 771.宝石与石头

给定字符串J 代表石头中宝石的类型，和字符串 S代表你拥有的石头。 S 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

J 中的字母不重复，J 和 S中的所有字符都是字母。字母区分大小写，因此"a"和"A"是不同类型的石头。

示例 1:

```java
输入: J = "aA", S = "aAAbbbb"
输出: 3
```




示例 2:

```java
输入: J = "z", S = "ZZ"
输出: 0
```


注意:

* S 和 J 最多含有50个字母。
*  J 中的字符不重复。



```java
class Solution {
    public int numJewelsInStones(String J, String S) {
        int lenS = S.length();
        if(lenS == 0 || J.length() == 0)   return 0;
        int cnt = 0;
        for(int i = 0; i < lenS; ++i){
            Character c = S.charAt(i);
            if(J.indexOf(c) != -1){
                cnt++;
            }
        }
        return cnt;
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





<span id = "jump834"></span>

## 834.树中距离之和[tag]

给定一个无向、连通的树。树中有 N 个标记为 0...N-1 的节点以及 N-1 条边 。

第 i 条边连接节点 `edges[i][0]` 和` edges[i][1]` 。

返回一个表示节点 i 与其他所有节点距离之和的列表 ans。

示例 1:

输入: N = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
输出: [8,12,6,10,10,10]
解释: 
如下为给定的树的示意图：

```java
  0
 / \
1   2
   /|\
  3 4 5
```

我们可以计算出 dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5) 
也就是 1 + 1 + 2 + 2 + 2 = 8。 因此，answer[0] = 8，以此类推。

* 说明: 1 <= N <= 10000



树形DP，[参考](https://leetcode-cn.com/problems/sum-of-distances-in-tree/)

```java
class Solution {

    List<List<Integer>> graph = new ArrayList<>();
    //nodeSum[i]表示以i为根节点的子树的子节点个数
    int[] nodeSum;
    //distSum[i]表示以i为根节点的子树的距离之和
    int[] distSum;
    public int[] sumOfDistancesInTree(int N, int[][] edges) {

        //建立邻接表
        for(int i = 0; i < N; ++i){
            graph.add(new ArrayList<Integer>());
        }
        for(int i = 0; i < edges.length; ++i){
            int src = edges[i][0];
            int dist = edges[i][1];
            graph.get(src).add(dist);
            graph.get(dist).add(src);
        }

        nodeSum = new int[N];
        distSum = new int[N];
        //填充1
        Arrays.fill(nodeSum,1);
        //后序遍历计算子树距离之和
        postOrder(0,-1);
        //先序遍历计算总距离之和
        preOrder(0,-1);

        return distSum;
        
    }

    void postOrder(int root, int parent){
        //获取root的所有子节点
        List<Integer> children = graph.get(root);
        //遍历子节点，计算距离之和
        for(Integer child : children){
            //访问到邻接表中父节点，跳过
            if(child == parent) continue;

            //后序遍历
            postOrder(child,root);

            //访问到有效的子节点
            //先计算nodeSum,统计子节点的个数
            nodeSum[root] += nodeSum[child];
            //根据nodeSum计算distSum
            distSum[root] += nodeSum[child] + distSum[child];
        }
    }

    void preOrder(int root, int parent){
        List<Integer> children = graph.get(root);

        for(Integer child : children){
            if(child == parent) continue;

            distSum[child] = distSum[root] - nodeSum[child] + (graph.size() - nodeSum[child]);
            preOrder(child,root);
        }
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





<span id = "jump968"></span>

## 968.监控二叉树

给定一个二叉树，我们在树的节点上安装摄像头。

节点上的每个摄影头都可以监视**其父对象、自身及其直接子对象。**

计算监控树的所有节点所需的最小摄像头数量。



示例1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/bst_cameras_01.png)

```java
输入：[0,0,null,0,0]
输出：1
解释：如图所示，一台摄像头足以监控所有节点。
```





示例2:

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/bst_cameras_02.png)



```java
输入：[0,0,null,0,null,0,null,null,0]
输出：2
解释：需要至少两个摄像头来监视树的所有节点。 上图显示了摄像头放置的有效位置之一。
```

**提示：**

1. 给定树的节点数的范围是 `[1, 1000]`。
2. 每个节点的值都是 0。





思路：

假设节点有3种状态：1.节点已被覆盖，2.节点未被覆盖，3.节点上有相机。每个节点的状态由两个子节点的状态推出。

```markdown
1. 若两个节点至少有一个未被覆盖，当前节点必须装相机。
2. 若两个节点都已经被覆盖，则当前节点就不一定需要装相机，就设置为为覆盖，交由父节点处理。
3. 若其中一个子节点有相机，就设置当前节点为已覆盖
```

由于需要知道两个子节点的状态才能决定当前节点的状态，所以采用后序遍历。

```java
class Solution {
    int res = 0;
    public int minCameraCover(TreeNode root) {
         if(backOrder(root) == 2){
            res++;
         }
         return res;
    }

    //节点有三种状态：1.节点已被覆盖，2.节点未被覆盖，3.节点上有相机
    int backOrder(TreeNode root){
        
        if(root == null){
            return 1;
        }
        int left = backOrder(root.left);
        int right = backOrder(root.right);
        //两个节点至少有一个没有被覆盖
        if(left == 2 || right == 2){
            res++;
            return 3;
        }
        //两个节点已经被覆盖，那这个节点就不一定需要装相机了
        else if(left == 1 && right == 1){
            return 2;
        }
        else if(left == 3 || right == 3){
            return 1;
        }
        return 0;
    }
}
```











