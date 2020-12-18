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

[514.自由之路](#jump514)

[529.扫雷游戏](#jump529)

[530.二叉搜索树的最小绝对差](#jump530)

[538.将二叉搜索树转换为累加树](#jump538)

[543.二叉树的直径](#jump543)

[546.移除盒子](#jump546)

[557.反转字符串中的单词 III](#jump557)

[617.合并二叉树](#jump617)

[621.任务调度器](#jump621)

[637.二叉树的层平均值](#jump637)

[647.回文子串](#jump647)

[649. Dota2 参议院](#jump649)

[657.机器人能否返回原点](#jump657)

[659.分割数组为连续子序列](#jump659)

[679.24点游戏](#jump679)

[685.冗余连接2](#jump685)

[696.计数二进制子串](#jump696)

[701.二叉搜索树中的插入操作tag](jump701)

[711.宝石和石头](#jump711)

[714. 买卖股票的最佳时机含手续费](#jump714)

[718.最长公共连续子数组](#jump718)

[733.图像渲染](#jump733)

[763.划分字母区间](#jump763)

[767.重构字符串](#jump767)

[834.树中距离之和tag](#jump834)

[841.钥匙和房间](#jump841)

[842. 将数组拆分成斐波那契序列](#jump842)

[844.比较含退格的字符串](#jump844)

[845.数组中的最长山脉](#jump845)

[860. 柠檬水找零](#jump860)

[861.翻转矩阵后的得分](#jump861)

[922.按奇偶排序数组 II](#jump922)

[925.长按键入](jump925)

[941.有效的山脉数组](#jump941)

[968.监控二叉树](#jump968)

[973.最接近原点的 K 个点](#jump973)

[976.三角形的最大周长](#jump976)

[977.有序数组的平方](#jump977)

[1002.查找常用字符](#jump1002)

[1024.视频拼接](#jump1024)

[1030.距离顺序排列矩阵单元格](#jump1030)

[1207.独一无二的出现次数](#jump1207)

[1356.根据数字二进制下 1 的数目排序](#jump1356)

[1365.有多少小于当前数字的数字](#jump1365)

[1370.上升下降字符串](#jump1370)

[LCP 19.秋叶收藏集](#jumplcp19)



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









<span id = "jump514"></span>

## 514.自由之路

视频游戏“辐射4”中，任务“通向自由”要求玩家到达名为“Freedom Trail Ring”的金属表盘，并使用表盘拼写特定关键词才能开门。

给定一个字符串 ring，表示刻在外环上的编码；给定另一个字符串 key，表示需要拼写的关键词。您需要算出能够拼写关键词中所有字符的最少步数。

最初，ring 的第一个字符与12:00方向对齐。您需要顺时针或逆时针旋转 ring 以使 key 的一个字符在 12:00 方向对齐，然后按下中心按钮，以此逐个拼写完 key 中的所有字符。

旋转 ring 拼出 key 字符 key[i] 的阶段中：

* 您可以将 ring 顺时针或逆时针旋转一个位置，计为1步。旋转的最终目的是将字符串 ring 的一个字符与 12:00 方向对齐，并且这个字符必须等于字符 key[i] 。
* 如果字符 key[i] 已经对齐到12:00方向，您需要按下中心按钮进行拼写，这也将算作 1 步。按完之后，您可以开始拼写 key 的下一个字符（下一阶段）, 直至完成所有拼写。

```java
输入: ring = "godding", key = "gd"
输出: 4
解释:
 对于 key 的第一个字符 'g'，已经在正确的位置, 我们只需要1步来拼写这个字符。 
 对于 key 的第二个字符 'd'，我们需要逆时针旋转 ring "godding" 2步使它变成 "ddinggo"。
 当然, 我们还需要1步进行拼写。
 因此最终的输出是 4。
```


提示：

* ring 和 key 的字符串长度取值范围均为 1 至 100；
* 两个字符串中都只有小写字符，并且均可能存在重复字符；
* 字符串 key 一定可以由字符串 ring 旋转拼出。



```java
class Solution {
    //用dp[i][j]表示从游戏开始到拼接完成key[0:i]在ring[j] == key[i]时的最小步数
    //因为存在重复字符串，所以我们把所有重复的位置都考虑到。
    //无论顺时针还是逆时针，同一个字符出现的所有位置都有可能是最小的步数
    //所以干脆将每个字符出现的位置放入一个哈希表中，用列表维护某个字符的所有出现位置
    //状态转移：枚举上一次与12:00方向对齐的位置k，
    //计算顺时针从k转到j和逆时针从k转到j的最小步数，即：min{abs(j - k), n - abs(j - k)} + 1
    //加上之前走的步数dp[i-1][k]
    //再将上面的值跟dp[i][j]比较，取最小值:
    //dp[i][j] = min{dp[i][j], dp[i-1][k] + min{abs(j - k), n - abs(j - k)} + 1}
    public int findRotateSteps(String ring, String key) {

        int n = ring.length(), m = key.length();
        int[][] dp = new int[m][n];

        for(int i = 0; i < m; ++i){
            Arrays.fill(dp[i],0x3f3f3f);
        }

        //维护一个map，存放每个字符在ring中出现的位置集合
        Map<Character, List<Integer>> map = new HashMap<>();

        for(int i = 0; i < n; ++i){
            List<Integer> list = map.getOrDefault(ring.charAt(i), new ArrayList<>());
            list.add(i);
            map.put(ring.charAt(i), list);
        }
        //边界初始化：即使第一个字符就对应好了，也不一定是最优解，因为很有可能其他位置的才是更优的
        for(int i : map.get(key.charAt(0))){
            dp[0][i] = Math.min(i, n - i) + 1;
        }

        for(int i = 1; i < m; ++i){
            for(int j : map.get(key.charAt(i))){
                for(int k : map.get(key.charAt(i - 1))){
                    dp[i][j] = Math.min(dp[i][j], dp[i - 1][k] + Math.min(Math.abs(j - k), n - Math.abs(j - k)) + 1);
                }
            }
        }

        int res = dp[m - 1][0];
        for(int i = 1; i < n; ++i){
            res = Math.min(res, dp[m-1][i]);
        }
        return res;
        
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







<span id = "jump543"></span>

## 543.二叉树的直径

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

 

示例 :
给定二叉树

          1
         / \
        2   3
       / \     
      4   5    

返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

注意：两结点之间的路径长度是以它们之间边的数目表示。



```java
class Solution {
    int res = 0;
    //实际上是要找出左右子树深度和最大的结点
    public int diameterOfBinaryTree(TreeNode root) {
        getDepth(root);
        return res;
    }

    int getDepth(TreeNode root){
        if(root == null){
            return 0;
        }
        int left = getDepth(root.left);
        int right = getDepth(root.right);

        res = Math.max(left+right, res);

        return Math.max(left,right)+1;
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





<span id = "jump649"></span>

## 649. Dota2 参议院

Dota2 的世界里有两个阵营：Radiant(天辉)和 Dire(夜魇)

Dota2 参议院由来自两派的参议员组成。现在参议院希望对一个 Dota2 游戏里的改变作出决定。他们以一个基于轮为过程的投票进行。在每一轮中，每一位参议员都可以行使两项权利中的一项：

**禁止一名参议员的权利：**

参议员可以让另一位参议员在这一轮和随后的几轮中丧失所有的权利。

**宣布胜利：**

如果参议员发现有权利投票的参议员都是同一个阵营的，他可以宣布胜利并决定在游戏中的有关变化。

 


给定一个字符串代表每个参议员的阵营。字母 “R” 和 “D” 分别代表了 Radiant（天辉）和 Dire（夜魇）。然后，如果有 n 个参议员，给定字符串的大小将是 n。

以轮为基础的过程从给定顺序的第一个参议员开始到最后一个参议员结束。这一过程将持续到投票结束。所有失去权利的参议员将在过程中被跳过。

假设每一位参议员都足够聪明，会为自己的政党做出最好的策略，你需要预测哪一方最终会宣布胜利并在 Dota2 游戏中决定改变。输出应该是 Radiant 或 Dire。

 

示例 1：

```java
输入："RD"
输出："Radiant"
解释：第一个参议员来自 Radiant 阵营并且他可以使用第一项权利让第二个参议员失去权力，因此第二个参议员将被跳过因为他没有任何权利。然后在第二轮的时候，第一个参议员可以宣布胜利，因为他是唯一一个有投票权的人
```


示例 2：

```java
输入："RDD"
输出："Dire"
解释：
第一轮中,第一个来自 Radiant 阵营的参议员可以使用第一项权利禁止第二个参议员的权利
第二个来自 Dire 阵营的参议员会被跳过因为他的权利被禁止
第三个来自 Dire 阵营的参议员可以使用他的第一项权利禁止第一个参议员的权利
因此在第二轮只剩下第三个参议员拥有投票的权利,于是他可以宣布胜利
```






提示：

* 给定字符串的长度在 [1, 10,000] 之间.





```java
class Solution {
    public String predictPartyVictory(String senate) {
        int n = senate.length();
        boolean[] visited = new boolean[n];
        //将R和D的索引入队，再依次弹出比较，索引较小的放在队尾，较大的放弃
        Queue<Integer> radiant = new LinkedList<>();
        Queue<Integer> dire = new LinkedList<>();

        for(int i = 0; i < n; ++i){
            if(senate.charAt(i) == 'R'){
                radiant.offer(i);
            }else{
                dire.offer(i);
            }
        }

        while(!radiant.isEmpty() && !dire.isEmpty()){
            int r = radiant.poll(), d = dire.poll();
            if(r < d){
                radiant.offer(r + n);
            }else{
                dire.offer(r + n);
            }
        }
        return !dire.isEmpty() ? "Dire" : "Radiant";
    }
}
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









<span id = "jump659"></span>

## 659.分割数组为连续子序列

给你一个按升序排序的整数数组 num（可能包含重复数字），请你将它们分割成一个或多个子序列，其中每个子序列都由连续整数组成且长度至少为 3 。

如果可以完成上述分割，则返回 true ；否则，返回 false 。

示例 1：

```java
输入: [1,2,3,3,4,5]
输出: True
解释:
你可以分割出这样两个连续子序列 : 
1, 2, 3
3, 4, 5
```

示例 2：

```java
输入: [1,2,3,3,4,4,5,5]
输出: True
解释:
你可以分割出这样两个连续子序列 : 
1, 2, 3, 4, 5
3, 4, 5
```

示例 3：

```java
输入: [1,2,3,4,4,5]
输出: False
```


提示：

* 输入的数组长度范围为 [1, 10000]

```java
//贪心
//由于序列是升序，所以在遍历过程中，不会过度增长子序列
//因为只会把较小的每个数字都安排完以后，才会去处理更大的数字
class Solution {
    public boolean isPossible(int[] nums) {
        //记录nums中数字的个数
        Map<Integer, Integer> count = new HashMap<>();
        //记录以数字key结尾的子序列个数
        Map<Integer, Integer> tail = new HashMap<>();

        for(int i : nums){
            count.put(i, count.getOrDefault(i, 0) + 1);
        }

        for(int num : nums){
            if(count.get(num) == 0) continue;
            //当前数字可以接在以num-1为结尾的子序列后
            if(tail.get(num - 1) != null && tail.get(num - 1) > 0){
                tail.put(num,tail.getOrDefault(num,0) + 1);
                //消耗掉一个以num-1为结尾的子序列
                tail.put(num - 1, tail.get(num - 1) - 1);
                //消耗掉一个num
                count.put(num, count.get(num) - 1);
            //当前数字只能自成一个序列
            }else if(count.get(num+1) != null && count.get(num+2) != null
                        && count.get(num+1) > 0 && count.get(num+2) > 0){
                tail.put(num+2, tail.getOrDefault(num+2,0) + 1);
                count.put(num, count.get(num) - 1);
                count.put(num + 1, count.get(num + 1) - 1);
                count.put(num + 2, count.get(num + 2) - 1);
            }else{
                return false;
            }
        }
        return true;
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





<span id = "jump714"></span>

## 714. 买卖股票的最佳时机含手续费

给定一个整数数组 prices，其中第 i 个元素代表了第 i 天的股票价格 ；非负整数 fee 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

示例 1:

```java
输入: prices = [1, 3, 2, 8, 4, 9], fee = 2
输出: 8
解释: 能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```


注意:

* 0 < prices.length <= 50000.
* 0 < prices[i] < 50000.
* 0 <= fee < 50000.



```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if(prices.length < 2)   return 0;
        //dp[i]表示对第i只股票操作后能够得到的最大收益，
        //dp[i][0]表示持有一只股票，dp[i][1]表示不持有一只股票
        int[][] dp = new int[prices.length][2];

        dp[0][0] = -prices[0];
        dp[0][1] = 0;
        for(int i = 1; i < prices.length; ++i){
            //第i轮过后持有一只股票
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] - prices[i]);
            //第i轮过后不持有一只股票
            dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] + prices[i] - fee);
        }

        return Math.max(dp[prices.length - 1][0],dp[prices.length - 1][1]);
        
    }
}
```



空间优化

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if(prices.length < 2)   return 0;
        //dp[i]表示对第i只股票操作后能够得到的最大收益，
        //dp[i][0]表示持有一只股票，dp[i][1]表示不持有一只股票
        int hold = -prices[0], selt = 0;

        for(int i = 1; i < prices.length; ++i){
            //第i轮过后持有一只股票
            int tmp_hold = Math.max(hold, selt - prices[i]);
            //第i轮过后不持有一只股票
            selt = Math.max(selt, hold + prices[i] - fee);
            hold = tmp_hold;
        }

        return selt;
        
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





<span id = "jump763"></span>

## 763.划分字母区间

字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。

 示例 1：

```java
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
```


提示：

* S的长度在[1, 500]之间。
* S只包含小写字母 'a' 到 'z' 。

```java
class Solution {
    int start = 0;
    int[] hashTable = new int[26];
    List<Integer> res = new ArrayList<>();
    public List<Integer> partitionLabels(String S) {
        while(start < S.length()){
            dfs(S);
        }
        return res;
    }

    void dfs(String S){
      	//以start为起点
        char c = S.charAt(start);
        int lastIndex = start;
      	//找出字符串中start的最后出现位置
        for(int i = S.length() - 1; i > start; --i){
            if(S.charAt(i) == c){
                lastIndex = i;
                break;
            }
        }
        //遍历[start, lastIndex]区间内的字符，找出区间内每个字符在S中出现的最后位置，取最大值
        for(int i = start+1; i < lastIndex; ++i){
          	//避免重复搜索同一个字符
            if(hashTable[S.charAt(i) - 'a'] == 1)   continue;
            hashTable[S.charAt(i) - 'a'] = 1;
            for(int j = S.length() - 1; j > i; --j){
                if(S.charAt(j) == S.charAt(i)){
                  	//更新lastIndex，精髓在这，lastIndex可能会变大，这样循环能够包含到扩大后的字符
                    lastIndex = Math.max(lastIndex,j);
                }
            }
        }
      	//记录结果
        res.add(lastIndex - start + 1);
      	//更新start指针
        start = lastIndex + 1;
    }
}
```







<span id = "jump767"></span>

## 767.重构字符串

给定一个字符串S，检查是否能重新排布其中的字母，使得两相邻的字符不同。

若可行，输出任意可行的结果。若不可行，返回空字符串。

示例 1:

```java
输入: S = "aab"
输出: "aba"
```

示例 2:

```java
输入: S = "aaab"
输出: ""
```


注意:

* S 只包含小写字母并且长度在[1, 500]区间内。

```java
//贪心插空填字母

class Solution {
    public String reorganizeString(String S) {
        int n = S.length();
        if(n <= 1)  return S;

        char[] res = new char[n];

        int[][] hash = new int[26][2];
        //阈值，某个字母出现的次数大于这个值，说明无法重排
        int threshold = n % 2 == 0 ? n / 2 : n / 2 + 1;

        //哈希表：Hash[i][0]代表字符编号，Hash[i][1]代表字符出现次数
        for(int i = 0; i < n; ++i){
            hash[S.charAt(i)-'a'][0] = S.charAt(i) - 'a';
            hash[S.charAt(i) - 'a'][1]++;
            if(hash[S.charAt(i) - 'a'][1] > threshold)   return "";
        }
        //按字符出现次数升序排序
        Arrays.sort(hash, (a,b)->{
            if(a[1] > b[1]){
                return -1;
            }else{
                return 1;
            }
        });
        //插空填
        //先排偶数位
        int cur = 0;
        while(cur < n){
            for(int i = 0; i < 26; ++i){
                while(cur < n && hash[i][1] > 0){
                    res[cur] = (char) (hash[i][0] + 'a');
                    cur+=2;
                    hash[i][1]--;
                }
            }
        }
        //再排奇数位
        cur = 1;
        while(cur < n){
            for(int i = 0; i < 26; ++i){
                while(cur < n && hash[i][1] > 0){
                    res[cur] = (char) (hash[i][0] + 'a');
                    cur+=2;
                    hash[i][1]--;
                }
            }
        }
        return new String(res);

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





<span id = "jump842"></span>

## 842. 将数组拆分成斐波那契序列

给定一个数字字符串 S，比如 S = "123456579"，我们可以将它分成斐波那契式的序列 [123, 456, 579]。

形式上，斐波那契式序列是一个非负整数列表 F，且满足：

0 <= F[i] <= 2^31 - 1，（也就是说，每个整数都符合 32 位有符号整数类型）；
F.length >= 3；
对于所有的0 <= i < F.length - 2，都有 F[i] + F[i+1] = F[i+2] 成立。
另外，请注意，将字符串拆分成小块时，每个块的数字一定不要以零开头，除非这个块是数字 0 本身。

返回从 S 拆分出来的任意一组斐波那契式的序列块，如果不能拆分则返回 []。

 

示例 1：

```java
输入："123456579"
输出：[123,456,579]
```

示例 2：

```java
输入: "11235813"
输出: [1,1,2,3,5,8,13]
```

示例 3：

```java
输入: "112358130"
输出: []
解释: 这项任务无法完成。
```

示例 4：

```java
输入："0123"
输出：[]
解释：每个块的数字不能以零开头，因此 "01"，"2"，"3" 不是有效答案。
```

示例 5：

```java
输入: "1101111"
输出: [110, 1, 111]
解释: 输出 [11,0,11,11] 也同样被接受。
```


提示：

* 1 <= S.length <= 200
* 字符串 S 中只含有数字。

```java
class Solution {
    //使用列表存储拆分出的数，回溯过程中维护该列表的元素，列表初始为空。
    //遍历字符串的所有可能的前缀，作为当前被拆分出的数，然后对剩余部分继续拆分，直到整个字符串拆分完毕。
    public List<Integer> splitIntoFibonacci(String S) {
        List<Integer> res = new ArrayList<>();
        dfs(res, S, 0, 0, 0);
        return res;
    }

    boolean dfs(List<Integer> res, String S, int index, int sum, int pre){

        if(index == S.length()){
            return res.size() >= 3;
        }
        long curLong = 0L;
        for(int i = index; i < S.length(); ++i){
            
            //除了数字0本身以外，其他情况不能以0开头
            if(i > index && S.charAt(index) == '0'){
                break;
            }
            curLong = curLong * 10 + (S.charAt(i) - '0');

            if(curLong > Integer.MAX_VALUE){
                break;
            }
            int cur = (int) curLong;
            //只有当列表长度大于等于2时才开始比较
            if(res.size() >= 2){
                if(cur < sum){
                    continue;
                }else if(cur > sum){
                    break;
                }
            }
            
            res.add(cur);
            if(dfs(res, S, i+1, pre + cur, cur)){
                return true;
            }else{
                //回溯
                res.remove(res.size() - 1);
            }

        }
        return false;    
    }

}
```











<span id = "844"></span>

## 844.比较含退格的字符串

给定 S 和 T 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 # 代表退格字符。

注意：如果对空文本输入退格字符，文本继续为空。

 

示例 1：

```java
输入：S = "ab#c", T = "ad#c"
输出：true
解释：S 和 T 都会变成 “ac”。
```

示例 2：

```java
输入：S = "ab##", T = "c#d#"
输出：true
解释：S 和 T 都会变成 “”。
```

示例 3：

```java
输入：S = "a##c", T = "#a#c"
输出：true
解释：S 和 T 都会变成 “c”。
```

示例 4：

```java
输入：S = "a#c", T = "b"
输出：false
解释：S 会变成 “c”，但 T 仍然是 “b”。
```


提示：

* 1 <= S.length <= 200
* 1 <= T.length <= 200
* S 和 T 只含有小写字母以及字符 '#'。


进阶：

* 你可以用 O(N) 的时间复杂度和 O(1) 的空间复杂度解决该问题吗？



从后往前移动，依次消除退格

```java
/*
执行用时：1 ms, 在所有 Java 提交中击败了96.52%的用户
内存消耗：36.3 MB, 在所有 Java 提交中击败了99.82%的用户
*/
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int iS = S.length() - 1;
        int iT = T.length() - 1;
        int cS = 0;
        int cT = 0;
        //从后往前移动
        while(iS >= 0 || iT >= 0){
            //如果当前字符为退格，就计数
            if(iS >= 0 && S.charAt(iS) == '#'){
                cS++;
                iS--;
                continue;
            }
            if(iT >= 0 && T.charAt(iT) == '#'){
                cT++;
                iT--;
                continue;
            }
            //把退格消耗掉，一个一个消耗，因为可能会继续有退格出现导致cS\cT变大
            if(cS > 0){
                cS--;
                iS--;
                continue;
            }
            if(cT > 0){
                cT--;
                iT--;
                continue;
            }
            //iS和iT都合法，就判断当前字符是否相等，不相等返回flase
            if(iS >= 0 && S.charAt(iS) != '#' 
            && iT >= 0 && T.charAt(iT) != '#' 
            && S.charAt(iS) != T.charAt(iT)){
                return false;
            }
            //iS和iT有一者非法，返回false
            if((iS >= 0 && iT < 0) || (iS < 0 && iT >= 0)){
                return false;
            }
            //二者都合法，并且当前字符相等，就递减
            --iS;
            --iT;
        }
        //循环结束，没有出现不相等的情况，就返回true
        return true;

    }
}
```



<span id = "jump845"></span>

## 845.数组中的最长山脉

我们把数组 A 中符合下列属性的任意连续子数组 B 称为 “山脉”：

B.length >= 3
存在 0 < i < B.length - 1 使得 B[0] < B[1] < ... B[i-1] < B[i] > B[i+1] > ... > B[B.length - 1]
（注意：B 可以是 A 的任意子数组，包括整个数组 A。）

给出一个整数数组 A，返回最长 “山脉” 的长度。

如果不含有 “山脉” 则返回 0。

示例 1：

```java
输入：[2,1,4,7,3,2,5]
输出：5
解释：最长的 “山脉” 是 [1,4,7,3,2]，长度为 5。
```

示例 2：

```java
输入：[2,2,2]
输出：0
解释：不含 “山脉”。
```


提示：

* 0 <= A.length <= 10000
* 0 <= A[i] <= 10000

找到山顶，再中心扩展。

```java
/*
执行用时：2 ms, 在所有 Java 提交中击败了99.84%的用户
内存消耗：39.5 MB, 在所有 Java 提交中击败了87.43%的用户
*/
class Solution {
    public int longestMountain(int[] A) {
        int index = 1;
        int max = 0;
        while(index < A.length-1){
            int cur = index+1;
            //找到一个符合要求的山顶
            if(A[index] > A[index - 1] && A[index] > A[index + 1]){
                //中心扩展
                int cnt = 3;
              	//向左扩展山脉
                cur = index-1;
                while(cur > 0){
                    if(A[cur] <= A[cur - 1])
                        break;
                    cnt++;
                    cur--;
                }
              	//向右扩展山脉
                cur = index + 1;
                while(cur < A.length - 1){
                    if(A[cur] <= A[cur + 1])
                        break;
                    cnt++;
                    cur++;
                }
                max = Math.max(cnt,max);
            }
          	//更新index到这座山的右边山脚，减少迭代过程
            index = cur;
        }
        return max;
    }
}
```





<span id = "jump860"></span>

## 860. 柠檬水找零

在柠檬水摊上，每一杯柠檬水的售价为 5 美元。

顾客排队购买你的产品，（按账单 bills 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 5 美元、10 美元或 20 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 5 美元。

注意，一开始你手头没有任何零钱。

如果你能给每位顾客正确找零，返回 true ，否则返回 false 。

示例 1：

```java
输入：[5,5,5,10,20]
输出：true
解释：
前 3 位顾客那里，我们按顺序收取 3 张 5 美元的钞票。
第 4 位顾客那里，我们收取一张 10 美元的钞票，并返还 5 美元。
第 5 位顾客那里，我们找还一张 10 美元的钞票和一张 5 美元的钞票。
由于所有客户都得到了正确的找零，所以我们输出 true。
```

示例 2：

```java
输入：[5,5,10]
输出：true
```

示例 3：

```java
输入：[10,10]
输出：false
```

示例 4：

```java
输入：[5,5,10,10,20]
输出：false
解释：
前 2 位顾客那里，我们按顺序收取 2 张 5 美元的钞票。
对于接下来的 2 位顾客，我们收取一张 10 美元的钞票，然后返还 5 美元。
对于最后一位顾客，我们无法退回 15 美元，因为我们现在只有两张 10 美元的钞票。
由于不是每位顾客都得到了正确的找零，所以答案是 false。
```


提示：

* 0 <= bills.length <= 10000
* bills[i] 不是 5 就是 10 或是 20 

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        if(bills[0] != 5)    return false;

        int five = 0;
        int ten = 0;
        for(int i = 0; i < bills.length; ++i){

            if(bills[i] == 5){
                five++;
            }
            if(bills[i] == 10){
                if(five > 0){
                    five--;
                    ten++;
                }else{
                    return false;
                }
            }

            if(bills[i] == 20){
                if(five > 0 && ten > 0){
                    ten--;
                    five--;
                }else if(ten == 0 && five >= 3){
                    five -= 3;
                }else{
                    return false;
                }
            }
        }

        return true;
    }
}
```







<span id = "jump861"></span>

## 861.翻转矩阵后的得分

有一个二维矩阵 A 其中每个元素的值为 0 或 1 。

移动是指选择任一行或列，并转换该行或列中的每一个值：将所有 0 都更改为 1，将所有 1 都更改为 0。

在做出任意次数的移动后，将该矩阵的每一行都按照二进制数来解释，矩阵的得分就是这些数字的总和。

返回尽可能高的分数。

 示例：

```java
输入：[[0,0,1,1],[1,0,1,0],[1,1,0,0]]
输出：39
解释：
转换为 [[1,1,1,1],[1,0,0,1],[1,1,1,1]]
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39
```


提示：

* 1 <= A.length <= 20
* 1 <= A[0].length <= 20
* `A[i][j]` 是 0 或 1



贪心策略，最高位全部为1，剩余情况1越多越好

```java
class Solution {
    public int matrixScore(int[][] A) {
        int m = A.length;
        int n = A[0].length;

        for(int i = 0; i < m; ++i){
            //最高位为0的行都翻转一遍
            if(A[i][0] == 0){
                for(int j = 0; j < n; ++j){
                    A[i][j] = A[i][j] == 0 ? 1 : 0;
                }
            }
        }

        int threshold = m % 2 == 0 ? (m/2) : (m/2 + 1);
        //从第二列开始，遍历每一列，0比1多的列都翻转一遍
        for(int j = 1; j < n; ++j){
            int cnt = 0;
            for(int i = 0; i < m; ++i){
                cnt += A[i][j] == 1 ? 1 : 0;
            }
            if(cnt >= threshold)    continue;
            //翻转此列
            for(int i = 0; i < m; ++i){
                A[i][j] = A[i][j] == 0 ? 1 : 0;
            }
        }

        int res = 0;
        //读取数字
        for(int i = 0; i < m; ++i){
            int num = 0;
            for(int j = 0; j < n; ++j){
                num <<= 1;
                if(A[i][j] == 1)    num += 1;
            }
            res += num;
        }
        return res;
    }
}
```











<span id = "jump922"></span>

## 922.按奇偶排序数组 II

给定一个非负整数数组 A， A 中一半整数是奇数，一半整数是偶数。

对数组进行排序，以便当 A[i] 为奇数时，i 也是奇数；当 A[i] 为偶数时， i 也是偶数。

你可以返回任何满足上述条件的数组作为答案。

示例：

```java
输入：[4,2,5,7]
输出：[4,5,2,7]
解释：[4,7,2,5]，[2,5,4,7]，[2,7,4,5] 也会被接受。
```


提示：

* 2 <= A.length <= 20000
* A.length % 2 == 0
* 0 <= A[i] <= 1000



双指针：

```java
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        int j = 1;
        //每找到一个偶数位上的奇数，就移动指针j，使其找到奇数位上的第一个偶数，交换
        for(int i = 0; i < A.length; i += 2){
            if(A[i] % 2 != 0){
                while(j < A.length && A[j] % 2 != 0){
                    j += 2;
                }
                swap(A, i,j);
            }
        }
        return A;
    }

    void swap(int[] A, int i, int j){
        if(i != j){
            int tmp = A[i];
            A[i] = A[j];
            A[j] = tmp;   
        }
    }
}
```

辅助数组：

```java
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        int[] res = new int[A.length];
        int cur_o = 1;
        int cur_e = 0;
        for(int i = 0; i < A.length; ++i){
            if(A[i] % 2 != 0){
                res[cur_o] = A[i];
                cur_o += 2;
            }else{
                res[cur_e] = A[i];
                cur_e += 2;
            }
        }
        return res;
    }
}
```



Golang:

```go
func sortArrayByParityII(A []int) []int {
    j := 1
    for i := 0; i < len(A); i += 2 {
        if A[i] % 2 != 0 {
            for j < len(A) && A[j] % 2 != 0 {
                j += 2;
            }
            A[i], A[j] = A[j], A[i]
        }
    }
    return A
}
```









<span id = "jump925"></span>

## 925.长按键入

你的朋友正在使用键盘输入他的名字 name。偶尔，在键入字符 c 时，按键可能会被长按，而字符可能被输入 1 次或多次。

你将会检查键盘输入的字符 typed。如果它对应的可能是你的朋友的名字（其中一些字符可能被长按），那么就返回 True。

 示例 1：

```java
输入：name = "alex", typed = "aaleex"
输出：true
解释：'alex' 中的 'a' 和 'e' 被长按。
```

示例 2：

```java
输入：name = "saeed", typed = "ssaaedd"
输出：false
解释：'e' 一定需要被键入两次，但在 typed 的输出中不是这样。
```

示例 3：

```java
输入：name = "leelee", typed = "lleeelee"
输出：true
```

示例 4：

```java
输入：name = "laiden", typed = "laiden"
输出：true
解释：长按名字中的字符并不是必要的。
```


提示：

* name.length <= 1000
* typed.length <= 1000
* name 和 typed 的字符都是小写字母。



逆序遍历判断即可，出现不相等的字符，就判断是否是长按造成的，注意边界判断。

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {

        int ptr1 = name.length() - 1;
        int ptr2 = typed.length() - 1;
        if(ptr1 < 0 && ptr2 >= 0)   return false;
        if(ptr1 < 0 && ptr2 < 0)  return true;
        //从后往前遍历
        while(ptr1 >= 0 && ptr2 >= 0){
            if(name.charAt(ptr1) == typed.charAt(ptr2)){
                --ptr1;
                --ptr2;
                continue;
            }else{
                //最后一个字符就不相等了，肯定不可能是长按造成的
                if(ptr1 == name.length() - 1)
                    return false;
                //不相等的不是最后一个字符，那么需要判断这个字符是否被长按了
                //与后面那个字符相等，说明有可能是长按了
                if(typed.charAt(ptr2) == typed.charAt(ptr2 + 1)){
                    ptr2--;
                    continue;
                }else{
                    //如果不是长按多出的字符，那么就不匹配
                    return false;
                }
            }
        }
        //匹配结束，如果ptr1<0，则判断ptr2剩余字符是否都是与name.charAt(0)相同的字符，如果是，就返回true，否则就false
        if(ptr1 < 0){
            while(ptr2 >= 0){
                if(name.charAt(0) != typed.charAt(ptr2)){
                    return false;
                }
                ptr2--;
            }
            return true;
        }
        return false;
    }
}
```













<span id = "jump941"></span>

## 941.有效的山脉数组

给定一个整数数组 A，如果它是有效的山脉数组就返回 true，否则返回 false。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：

* A.length >= 3
* 在 0 < i < A.length - 1 条件下，存在 i 使得：
  * A[0] < A[1] < ... A[i-1] < A[i]
  * A[i] > A[i+1] > ... > A[A.length - 1]

示例 1：

```java
输入：[2,1]
输出：false
```

示例 2：

```java
输入：[3,5,5]
输出：false
```

示例 3：

```java
输入：[0,3,2,1]
输出：true
```


提示：

* 0 <= A.length <= 10000
* 0 <= A[i] <= 10000 



```java
class Solution {
    public boolean validMountainArray(int[] A) {
        boolean change = false;
        if(A.length < 3)    return false;
        int i = 0;
      	//上山
        while(i + 1 < A.length &&  A[i] < A[i + 1]) ++i;
      	//判断为何停止上山了
        if(i == 0 || i == A.length - 1 || A[i] == A[i + 1]) return false;
      	//下山
        while(i + 1 < A.length && A[i] > A[i + 1])  ++i;
      	//判断是否能够到山脚
        return i == A.length - 1;
    }
}
```

```java
class Solution {
    public boolean validMountainArray(int[] A) {
        boolean change = false;
        if(A.length < 3)    return false;
        if(A[0] > A[1]) return false;
        for(int i = 0; i < A.length - 1; ++i){
            if(A[i] == A[i + 1])    return false;
            if(A[i] > A[i + 1] && change == false){
                change = true;
            }
            if(A[i] < A[i+1] && change == true) return false;
        }
        return change;
    }
}
```











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







<span id = "jump973"></span>

## 973.最接近原点的 K 个点

我们有一个由平面上的点组成的列表 points。需要从中找出 K 个距离原点 (0, 0) 最近的点。

（这里，平面上两点之间的距离是欧几里德距离。）

你可以按任何顺序返回答案。除了点坐标的顺序之外，答案确保是唯一的。

示例 1：

```java
输入：points = [[1,3],[-2,2]], K = 1
输出：[[-2,2]]
解释： 
(1, 3) 和原点之间的距离为 sqrt(10)，
(-2, 2) 和原点之间的距离为 sqrt(8)，
由于 sqrt(8) < sqrt(10)，(-2, 2) 离原点更近。
我们只需要距离原点最近的 K = 1 个点，所以答案就是 [[-2,2]]。
```

示例 2：

```java
输入：points = [[3,3],[5,-1],[-2,4]], K = 2
输出：[[3,3],[-2,4]]
（答案 [[-2,4],[3,3]] 也会被接受。）
```


提示：

* `1 <= K <= points.length <= 10000`
* `-10000 < points[i][0] < 10000`
* `-10000 < points[i][1] < 10000`

优先队列：

```java
 class Solution {
    public int[][] kClosest(int[][] points, int K) {
        int[][] res = new int[K][2];

        PriorityQueue<Entry> pq = new PriorityQueue<>(
                (a,b) -> {
                    if(a.getValue() > b.getValue()){
                        return 1;
                    }else{
                        return -1;
                    }
                }
        );
        for(int i = 0; i < points.length; ++i){
            int key = i;
            Double value = Math.sqrt(points[i][0]*points[i][0] + points[i][1]*points[i][1]);
            Entry entry = new Entry(key, value);
            pq.offer(entry);
        }

        for(int i = 0; i < K; ++i){
            Entry entry = pq.poll();
            int key = entry.getKey();
            res[i][0] = points[key][0];
            res[i][1] = points[key][1];
        }
        return res;

    }

    class Entry{
        int key;
        double value;
        Entry(int key, double value){
            this.key = key;
            this.value = value;
        }
        int getKey(){
            return key;
        }
        double getValue(){
            return value;
        }
    }
}
```

数组排序：

```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        int[][] res = new int[K][2];

        Arrays.sort(points, (a,b) -> {
            double value1 = Math.sqrt(a[0]*a[0] + a[1]*a[1]);
            double value2 = Math.sqrt(b[0]*b[0] + b[1]*b[1]);
            if(value1 > value2){
                return 1;
            }else{
                return -1;
            }
        });
        for(int i = 0; i < K; ++i){
            res[i] = points[i];
        }
        return res;
    }
}
```

快速排序partition:

```java
class Solution {
    Random rand = new Random();

    public int[][] kClosest(int[][] points, int K) {
        int n = points.length;
        random_select(points, 0, n - 1, K);
        return Arrays.copyOfRange(points, 0, K);
    }

    public void random_select(int[][] points, int left, int right, int K) {
        int pivotId = left + rand.nextInt(right - left + 1);
        int pivot = points[pivotId][0] * points[pivotId][0] + points[pivotId][1] * points[pivotId][1];
        swap(points, right, pivotId);
        int i = left - 1;
        for (int j = left; j < right; ++j) {
            int dist = points[j][0] * points[j][0] + points[j][1] * points[j][1];
            if (dist <= pivot) {
                ++i;
                swap(points, i, j);
            }
        }
        ++i;
        swap(points, i, right);
        // [left, i-1] 都小于等于 pivot, [i+1, right] 都大于 pivot
        if (K < i - left + 1) {
            random_select(points, left, i - 1, K);
        } else if (K > i - left + 1) {
            random_select(points, i + 1, right, K - (i - left + 1));
        }
    }

    public void swap(int[][] points, int index1, int index2) {
        int[] temp = points[index1];
        points[index1] = points[index2];
        points[index2] = temp;
    }
}
```







<span id = "jump976"></span>

## 976.三角形的最大周长

给定由一些正数（代表长度）组成的数组 A，返回由其中三个长度组成的、面积不为零的三角形的最大周长。

如果不能形成任何面积不为零的三角形，返回 0。

 

示例 1：

```java
输入：[2,1,2]
输出：5
```

示例 2：

```java
输入：[1,2,1]
输出：0
```

示例 3：

```java
输入：[3,2,3,4]
输出：10
```

示例 4：

```java
输入：[3,6,2,3]
输出：8
```


提示：

* 3 <= A.length <= 10000
* 1 <= A[i] <= 10^6

```java
class Solution {
    public int largestPerimeter(int[] A) {
        int n = A.length;
        Arrays.sort(A);

        int p1 = n-1;
        int p2 = n-2;

        for(int i = n-3; i >= 0; --i){
            //两边之差A[p1] - A[p2]
            int dec = A[p1] - A[p2];
            if(dec < A[i]){
                return A[p1] + A[p2] + A[i];
            }else{
                p1 = p2;
                p2--;
            }
        }

        return 0;
    }
}
```











<span id = "jump977"></span>

## 977.有序数组的平方

给定一个按非递减顺序排序的整数数组 A，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。

示例 1：

```java
输入：[-4,-1,0,3,10]
输出：[0,1,9,16,100]
```

示例 2：

```java
输入：[-7,-3,2,3,11]
输出：[4,9,9,49,121]
```


提示：

* 1 <= A.length <= 10000
* -10000 <= A[i] <= 10000
* A 已按非递减顺序排序。



双指针左右移动，比较绝对值的较小值，取平方依次填入新数组。

```java
class Solution {
    public int[] sortedSquares(int[] A) {
        //找到绝对值最小值的位置，双指针左右移
        
        //如果全部是非正数，那么绝对值最小值就是最后一个元素
        int index = A.length - 1;
        //当两个相邻元素的绝对值之差为负数时，说明找到了绝对值最小值
        for(int i = 0; i < A.length - 1; ++i){
            if(Math.abs(A[i]) < Math.abs(A[i+1])){
                index = i;
                break;
            }
        }
        int[] res = new int[A.length];
        int cur = 0;
        //双指针，初始时都在index处
        int l = index, r = index+1;
        while(l >= 0 && r < A.length){
            if(Math.abs(A[l]) < Math.abs(A[r])){
                res[cur++] = A[l] * A[l];
                l--;
            }else{
                res[cur++] = A[r] * A[r];
                ++r;
            }
        }
        while(l >= 0){
            res[cur++] = A[l] * A[l];
            l--;
        }
        while(r < A.length){
            res[cur++] = A[r] * A[r];
            ++r;
        }
        return res;
    }
}
```









<span id = "jump1002"></span>

## 1002.查找常用字符

给定仅有小写字母组成的字符串数组 A，返回列表中的每个字符串中都显示的全部字符（包括重复字符）组成的列表。例如，如果一个字符在每个字符串中出现 3 次，但不是 4 次，则需要在最终答案中包含该字符 3 次。

你可以按任意顺序返回答案。

示例 1：

```java
输入：["bella","label","roller"]
输出：["e","l","l"]
```


示例 2：

```java
输入：["cool","lock","cook"]
输出：["c","o"]
```


提示：

* 1 <= A.length <= 100
* 1 <= A[i].length <= 100
* `A[i][j] `是小写字母

哈希表，建立数组`counts[26]`，先预存第一个字符串每个字母的哈希，统计出现次数。

再遍历剩余字符串，统计每个字符串中每个字符出现的次数，放入哈希数组`tmp[26]`中，再按如下规则更新`counts`:

```java
counts[i] = Math.min(count[i], tmp[i]);
```

即统计各字符出现的最小次数。

最后将`counts[i]`映射到字符串即可。

```java
class Solution {
    public List<String> commonChars(String[] A) {
        List<String> res = new ArrayList<>();
        if(A.length == 0)   return res;

        int[] counts = new int[26];
        //预存第一个字符串每个字母的哈希，统计出现次数。
        for(int i = 0; i < A[0].length(); ++i){
            char c = A[0].charAt(i);
            int index = c - 'a';
            counts[index]++;
        }
        //遍历剩余字符串，统计每个字符串中每个字符出现的次数，放入哈希数组tmp[26]中
        for(int i = 1; i < A.length; ++i){
            int[] tmp = new int[26];
            for(int j = 0; j < A[i].length(); ++j){
                char c = A[i].charAt(j);
                int index = c - 'a';
                tmp[index]++;
            }
          	//取最小次数
            for(int j = 0; j < 26; ++j){
                counts[j] = Math.min(counts[j],tmp[j]);
            }
        }

        for(int i = 0; i < 26; ++i){
            if(counts[i] == 0)  continue;
            char c = (char)(i+97);
            for(int j = 0; j < counts[i]; ++j){
                res.add(String.valueOf(c));
            }
        }
        return res;
    }
}
```





<span id = "jump1024"></span>

## 1024.视频拼接

你将会获得一系列视频片段，这些片段来自于一项持续时长为 T 秒的体育赛事。这些片段可能有所重叠，也可能长度不一。

视频片段 clips[i] 都用区间进行表示：开始于 clips[i][0] 并于 clips[i][1] 结束。我们甚至可以对这些片段自由地再剪辑，例如片段 [0, 7] 可以剪切成 [0, 1] + [1, 3] + [3, 7] 三部分。

我们需要将这些片段进行再剪辑，并将剪辑后的内容拼接成覆盖整个运动过程的片段（[0, T]）。返回所需片段的最小数目，如果无法完成该任务，则返回 -1 。

示例 1：

```java
输入：clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], T = 10
输出：3
解释：
我们选中 [0,2], [8,10], [1,9] 这三个片段。
然后，按下面的方案重制比赛片段：
将 [1,9] 再剪辑为 [1,2] + [2,8] + [8,9] 。
现在我们手上有 [0,2] + [2,8] + [8,10]，而这些涵盖了整场比赛 [0, 10]。
```


示例 2：

```java
输入：clips = [[0,1],[1,2]], T = 5
输出：-1
解释：
我们无法只用 [0,1] 和 [1,2] 覆盖 [0,5] 的整个过程。
```


示例 3：

```java
输入：clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], T = 9
输出：3
解释： 
我们选取片段 [0,4], [4,7] 和 [6,9] 。
```


示例 4：

```java
输入：clips = [[0,4],[2,8]], T = 5
输出：2
解释：
注意，你可能录制超过比赛结束时间的视频。
```


提示：

* 1 <= clips.length <= 100
* 0 <= clips[i][0] <= clips[i][1] <= 100
* 0 <= T <= 100

先将片段按结束时间降序排序，再贪心选择有效持续时间最长的片段。

```java
class Solution {
    public int videoStitching(int[][] clips, int T) {
        int res = 0;
        //对数组按结束时间降序排序
        Arrays.sort(clips,(a,b) -> {
            if(a[1] >= b[1]){
                return -1;
            }else {
                return 1;
            }
        });

        //指向当前视频片段覆盖的区域开头，为T表示未被覆盖
        int cur = T;
        boolean[] visited = new boolean[clips.length];
        //按结束时间先选择片段，贪心地选择持续时间最长的
        //但是结束时间可能超过T
        //所以选择有效持续时间最长的，先判断当前片段的结束时间是否大于cur，如果成立再计算有效持续时间才有意义
        //有效持续时间为：cur - 开始时间
        while(cur > 0){
            int max = 0;
            int index = clips.length;
            for(int i = 0; i < clips.length; ++i){
                if(clips[i][1] < cur)   break;
                if(visited[i] == true)  continue;
                //当前片段的结束时间大于cur，且开始时间小于cur
                if(clips[i][1] >= cur && clips[i][0] < cur){
                    if(cur - clips[i][0] > max){
                        max = cur - clips[i][0];
                        index = i;
                    }
                }
            }
            //没有片段符合此次覆盖要求
            if(index == clips.length){
                return -1;
            }else{
                res++;
                cur = clips[index][0];
                visited[index] = true;
            }
        }
        return res;
        

    }
}
```





<span id = "jump1030"></span>

## 1030.距离顺序排列矩阵单元格

给出 R 行 C 列的矩阵，其中的单元格的整数坐标为 (r, c)，满足 0 <= r < R 且 0 <= c < C。

另外，我们在该矩阵中给出了一个坐标为 (r0, c0) 的单元格。

返回矩阵中的所有单元格的坐标，并按到 (r0, c0) 的距离从最小到最大的顺序排，其中，两单元格(r1, c1) 和 (r2, c2) 之间的距离是曼哈顿距离，|r1 - r2| + |c1 - c2|。（你可以按任何满足此条件的顺序返回答案。）

 

示例 1：

```java
输入：R = 1, C = 2, r0 = 0, c0 = 0
输出：[[0,0],[0,1]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1]
```

示例 2：

```java
输入：R = 2, C = 2, r0 = 0, c0 = 1
输出：[[0,1],[0,0],[1,1],[1,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2]
[[0,1],[1,1],[0,0],[1,0]] 也会被视作正确答案。
```

示例 3：

```java
输入：R = 2, C = 3, r0 = 1, c0 = 2
输出：[[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
解释：从 (r0, c0) 到其他单元格的距离为：[0,1,1,2,2,3]
其他满足题目要求的答案也会被视为正确，例如 [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]]。
```


提示：

* 1 <= R <= 100
* 1 <= C <= 100
* 0 <= r0 < R
* 0 <= c0 < C



直接排序：

```java
class Solution {
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {
        int[][] res = new int[R*C][2];
        List<int[]> list = new ArrayList<>();

        for(int i = 0; i < R; ++i){
            for(int j = 0; j < C; ++j){
                list.add(new int[]{i,j});
            }
        }

        Collections.sort(list, (a,b)->{
            int v1 = Math.abs(a[0] - r0) + Math.abs(a[1] - c0);
            int v2 = Math.abs(b[0] - r0) + Math.abs(b[1] - c0);
            return v1 - v2;
        });
        for(int i = 0; i < list.size(); ++i){
            res[i] = list.get(i);
        }
        return res;
    }
}
```

桶排序：

```java
class Solution {
    //桶排序，由于各个节点到(r0,c0)的曼哈顿距离都是整数
    //所以分桶，枚举每个节点，将其填入各个桶中，最后按序读取桶即可
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {
        int maxDis = Math.max(r0, R - 1 - r0) + Math.max(c0, C - 1 - c0);
        List<List<int[]>> buckets = new ArrayList<>();

        for(int i = 0; i <= maxDis; ++i){
            buckets.add(new ArrayList<int[]>());
        }

        for(int i = 0; i < R; ++i){
            for(int j = 0; j < C; ++j){
                int dist = Math.abs(i - r0) + Math.abs(j - c0);
                buckets.get(dist).add(new int[]{i,j});
            }
        }
        int[][] res = new int[R*C][2];
        int cur = 0;
        for(int i = 0; i <= maxDis; ++i){
            List<int[]> list = buckets.get(i);
            for(int[] a : list){
                res[cur++] = a;
            }
        }
        return res;
    }

    
}
```









<span id = "jump1207"></span>

## 1207.独一无二的出现次数

给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数。

如果每个数的出现次数都是独一无二的，就返回 true；否则返回 false。

示例 1：

```java
输入：arr = [1,2,2,1,1,3]
输出：true
解释：在该数组中，1 出现了 3 次，2 出现了 2 次，3 只出现了 1 次。没有两个数的出现次数相同。
```

示例 2：

```java
输入：arr = [1,2]
输出：false
```

示例 3：

```java
输入：arr = [-3,0,1,-3,1,1,1,-3,10,0]
输出：true
```


提示：

* 1 <= arr.length <= 1000
* -1000 <= arr[i] <= 1000

先排序，再统计出现个数放入set，出现重复即返回false.

```java
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        if(arr.length <= 1) return true;
        Arrays.sort(arr);
        Set<Integer> set = new HashSet<>();
        int cnt = 0;
        for(int i = 0; i < arr.length - 1; ++i){
            cnt++;
            if(arr[i] != arr[i+1]){
                if(cnt != 0 && set.contains(cnt))
                    return false;
                set.add(cnt);
                cnt = 0;
            }
        }
        if(arr[arr.length - 1] != arr[arr.length - 2] && set.contains(1))
            return false;
        return true;
    }
}
```







<span id = "jump1356"></span>

## 1356.根据数字二进制下 1 的数目排序

给你一个整数数组 arr 。请你将数组中的元素按照其二进制表示中数字 1 的数目升序排序。

如果存在多个数字二进制中 1 的数目相同，则必须将它们按照数值大小升序排列。

请你返回排序后的数组。

 

示例 1：

```java
输入：arr = [0,1,2,3,4,5,6,7,8]
输出：[0,1,2,4,8,3,5,6,7]
解释：[0] 是唯一一个有 0 个 1 的数。
[1,2,4,8] 都有 1 个 1 。
[3,5,6] 有 2 个 1 。
[7] 有 3 个 1 。
按照 1 的个数排序得到的结果数组为 [0,1,2,4,8,3,5,6,7]
```

示例 2：

```java
输入：arr = [1024,512,256,128,64,32,16,8,4,2,1]
输出：[1,2,4,8,16,32,64,128,256,512,1024]
解释：数组中所有整数二进制下都只有 1 个 1 ，所以你需要按照数值大小将它们排序。
```


示例 3：

```java
输入：arr = [10000,10000]
输出：[10000,10000]
```

示例 4：

```java
输入：arr = [2,3,5,7,11,13,17,19]
输出：[2,3,5,17,7,11,13,19]
```

示例 5：

```java
输入：arr = [10,100,1000,10000]
输出：[10,100,10000,1000]
```


提示：

* 1 <= arr.length <= 500
* 0 <= arr[i] <= 10^4



快速排序：

```java
class Solution {
    public int[] sortByBits(int[] arr) {
        quickSort(arr, 0, arr.length - 1);
        return arr;
    }

    void quickSort(int[] arr, int low, int high){
        if(low < high){
            int pos = partition(arr, low, high);
            quickSort(arr, low, pos - 1);
            quickSort(arr, pos + 1, high);
        }

    }

    int partition(int[] arr, int low ,int high){
        int pivot = arr[low];
        int pivotCount = count(arr[low]);
        while(low < high){
          	//当二进制计数大于pivotCount时，--high
          	//当二进制计数等于pivotCount，且 arr[high] >= pivot，--high
            while(low < high &&
                (count(arr[high]) > pivotCount ||
                (count(arr[high]) == pivotCount && arr[high] >= pivot)))  --high;
            arr[low] = arr[high];
          	//同理
            while(low < high &&
                (count(arr[low]) < pivotCount ||
                (count(arr[low]) == pivotCount && arr[low] <= pivot))) ++low;
            arr[high] = arr[low];
        }
        arr[low] = pivot;
        return low;
    }
		//统计二进制个数
    static int count(int a){
        int cnt = 0;
        while(a != 0){
            cnt += a & 1;
            a >>>= 1;
        }
        return cnt;
    }
}
```

改写库函数：

```java
class Solution {
    public int[] sortByBits(int[] arr) {
        Integer[] arrI = new Integer[arr.length];
      	//因为Arrays.sort()只支持T类型对象自定义比较器，不支持诸如int[]的对象。
        for(int i = 0; i < arr.length; ++i){
            arrI[i] = arr[i];
        }
        Arrays.sort(arrI, (a,b) -> {
            int ca = count((int)a);
            int cb = count((int)b);
            if(ca > cb)
                return 1;
            else if(ca == cb){
                return (int)a - (int)b;
            }else{
                return -1;
            }
        });
        for(int i = 0; i < arr.length; ++i){
            arr[i] = arrI[i];
        }
        return arr;
    }

    int count(int a){
        int cnt = 0;
        while(a != 0){
            cnt += a & 1;
            a >>>= 1;
        }
        return cnt;
    }
}
```









<span id = "jump1365"></span>

## 1365.有多少小于当前数字的数字

给你一个数组 nums，对于其中每个元素 nums[i]，请你统计数组中比它小的所有数字的数目。

换而言之，对于每个 nums[i] 你必须计算出有效的 j 的数量，其中 j 满足 j != i 且 nums[j] < nums[i] 。

以数组形式返回答案。

 示例 1：

```java
输入：nums = [8,1,2,2,3]
输出：[4,0,1,1,3]
解释： 
对于 nums[0]=8 存在四个比它小的数字：（1，2，2 和 3）。 
对于 nums[1]=1 不存在比它小的数字。
对于 nums[2]=2 存在一个比它小的数字：（1）。 
对于 nums[3]=2 存在一个比它小的数字：（1）。 
对于 nums[4]=3 存在三个比它小的数字：（1，2 和 2）。
```

示例 2：

```java
输入：nums = [6,5,4,8]
输出：[2,1,0,3]
```

示例 3：

```java
输入：nums = [7,7,7,7]
输出：[0,0,0,0]
```


提示：

* 2 <= nums.length <= 500
* 0 <= nums[i] <= 100

```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] res = new int[nums.length];
        //将计算过的数放入哈希表中
        //拷贝一份数组，排序，动态规划，放入哈希表，直接读取
        res = Arrays.copyOf(nums, nums.length);
        Arrays.sort(res);
        Map<Integer, Integer> map = new HashMap<>();
        int x = 0;
        for(int i = 0; i < res.length; ++i){
            if(map.containsKey(res[i])){
                x++;
            }else{
                map.put(res[i],x++);    
            }
            
        }
        for(int i = 0; i < nums.length; ++i){
            res[i] = map.get(nums[i]);
        }
        return res;
    }
}
```





<span id = "jump1370"></span>

## 1370.上升下降字符串

给你一个字符串 s ，请你根据下面的算法重新构造字符串：

从 s 中选出 最小 的字符，将它 接在 结果字符串的后面。
从 s 剩余字符中选出 最小 的字符，且该字符比上一个添加的字符大，将它 接在 结果字符串后面。
重复步骤 2 ，直到你没法从 s 中选择字符。
从 s 中选出 最大 的字符，将它 接在 结果字符串的后面。
从 s 剩余字符中选出 最大 的字符，且该字符比上一个添加的字符小，将它 接在 结果字符串后面。
重复步骤 5 ，直到你没法从 s 中选择字符。
重复步骤 1 到 6 ，直到 s 中所有字符都已经被选过。
在任何一步中，如果最小或者最大字符不止一个 ，你可以选择其中任意一个，并将其添加到结果字符串。

请你返回将 s 中字符重新排序后的 结果字符串 。

 

示例 1：

```java
输入：s = "aaaabbbbcccc"
输出："abccbaabccba"
解释：第一轮的步骤 1，2，3 后，结果字符串为 result = "abc"
第一轮的步骤 4，5，6 后，结果字符串为 result = "abccba"
第一轮结束，现在 s = "aabbcc" ，我们再次回到步骤 1
第二轮的步骤 1，2，3 后，结果字符串为 result = "abccbaabc"
第二轮的步骤 4，5，6 后，结果字符串为 result = "abccbaabccba"
```

示例 2：

```java
输入：s = "rat"
输出："art"
解释：单词 "rat" 在上述算法重排序以后变成 "art"
```




示例 3：

```java
输入：s = "leetcode"
输出："cdelotee"
```




示例 4：

```java
输入：s = "ggggggg"
输出："ggggggg"
```




示例 5：

```java
输入：s = "spo"
输出："ops"
```


提示：

* 1 <= s.length <= 500
* s 只包含小写英文字母。

```java
class Solution {
    public String sortString(String s) {
        int n = s.length();
        if(n <= 1)  return s;

        StringBuilder sb = new StringBuilder();
        
        //桶计数，很有用的思路，以后排序字符就可以用这个
        int[] counts = new int[26];

        for(int i = 0; i < n; ++i){
            counts[s.charAt(i) - 'a']++;
        }

        while(sb.length() < n){
            for(int i = 0; i < counts.length; ++i){
                if(counts[i] > 0){
                    sb.append((char)(i + 'a'));
                    counts[i]--;
                }
            }

            for(int i = counts.length - 1; i >= 0; --i){
                if(counts[i] > 0){
                    sb.append((char)(i + 'a'));
                    counts[i]--;
                }
            }
        }
        return sb.toString();

    }
}
```












<span id = "jumplcp19"></span>

## LCP 19. 秋叶收藏集

小扣出去秋游，途中收集了一些红叶和黄叶，他利用这些叶子初步整理了一份秋叶收藏集 leaves， 字符串 leaves 仅包含小写字符 r 和 y， 其中字符 r 表示一片红叶，字符 y 表示一片黄叶。
出于美观整齐的考虑，小扣想要将收藏集中树叶的排列调整成「红、黄、红」三部分。每部分树叶数量可以不相等，但均需大于等于 1。每次调整操作，小扣可以将一片红叶替换成黄叶或者将一片黄叶替换成红叶。请问小扣最少需要多少次调整操作才能将秋叶收藏集调整完毕。

示例 1：

```java
输入：leaves = "rrryyyrryyyrr"

输出：2

解释：调整两次，将中间的两片红叶替换成黄叶，得到 "rrryyyyyyyyrr"
```

示例 2：

```java
输入：leaves = "ryr"

输出：0

解释：已符合要求，不需要额外操作
```

提示：

* 3 <= leaves.length <= 10^5
* leaves 中只包含字符 'r' 和字符 'y'



动态规划：

```java
class Solution {
    public int minimumOperations(String leaves) {
        int n = leaves.length();
        //dp[i][j]表示对0~i的叶子进行调整操作，并且第i片叶子的颜色为j的最小调整操作
        //j有三个状态，0:前面的红色，1：中间的黄色，2:后面的红色
        //叶子一共有n片
        int[][] dp = new int[n][3];

        //初始条件
        //对第0片叶子进行调整，第0片叶子为1的最小调整次数
        dp[0][0] = leaves.charAt(0) == 'y' ? 1 : 0;
        dp[0][1] = Integer.MAX_VALUE;
     //由于i必须大于等于2，所以dp[0][2],dp[1][2]是不可能的值，为了不影响后面的结果，就将其初始化为极大值
        dp[0][2] = Integer.MAX_VALUE;
        dp[1][2] = Integer.MAX_VALUE;

      //状态转移：
      //  状态dp[i][0]必须从dp[i-1][0]转化而来，所以第i片叶子为红色就无需调整，若为黄色就需要调整为红色
      //
      //  状态dp[i][1]可能由dp[i-1][0]转化而来，所以第i片叶子为黄色就无需调整，若为红色就需要调整为黄色
      //            也可能由dp[i-1][1]转化而来，所以第i片叶子为黄色就无需调整，若为红色就需要调整为黄色
      //            不可能从dp[i-1][2]转化而来
      //  状态dp[i][2]可能由dp[i-1][1]转化而来，所以第i片叶子为红色就无需调整，若为黄色就需要调整为红色
      //            也可能从dp[i-1][2]转化而来，所以第i片叶子为红色就无需调整，若为黄色就需要调整为红色

        for(int i = 1; i < n; ++i){
            dp[i][0] = dp[i-1][0] + (leaves.charAt(i) == 'r' ? 0 : 1);
            dp[i][1] = Math.min(dp[i-1][0],dp[i-1][1]) + (leaves.charAt(i) == 'y' ? 0 : 1);
            //i必须大于等于2,dp[i][2]才有意义
            if(i >= 2){
                dp[i][2] = Math.min(dp[i-1][1],dp[i-1][2]) + (leaves.charAt(i) == 'r' ? 0 : 1);
            } 
        }
        return dp[n-1][2];

    }
}
```





