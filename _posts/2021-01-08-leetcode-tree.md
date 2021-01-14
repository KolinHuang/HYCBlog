---
title: Leetcode：树专题整理
author: Kol Huang
date: 2021-01-08 16:21:00 +0800
categories: [Blogging, leetcode]
tags: [算法题解]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/leetcode_cover.jpg
---







## 二叉树遍历***

**114.二叉树展开为链表**

给定一个二叉树，原地将它展开为一个单链表。

**404.左叶子之和**

计算给定二叉树的所有左叶子之和。

<br>



第一题采用逆后序遍历，用一个指针指向上一个节点，然后将当前节点的right指向上一个节点，并把left置空。

```java
class Solution {
    TreeNode pre = null;
    public void flatten(TreeNode root) {
        if(root != null){
            flatten(root.right);
            flatten(root.left);
            if(pre != null){
                root.right = pre;
                root.left = null;
            }
            pre = root;
        }
    }
}
```



第二题无论采用何种遍历都行，只需要在访问左子树的时候，给一个标记即可。

```java
class Solution {
    int res = 0;
    public int sumOfLeftLeaves(TreeNode root) {
        preOrder(root, false);

        return res;
    }

    void preOrder(TreeNode root, boolean isLeft){
        if(root == null)    return;

        if(root.left == null && root.right == null && isLeft){
            res += root.val;
        }
        preOrder(root.left, true);
        preOrder(root.right, false);
    }
}
```





第三题：







### 前序遍历-迭代

用一个栈来模拟：先将根结点入栈，然后访问出栈1个节点，再将右子节点和左子节点依次入栈，这是为了优先访问左子树。

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)    return res;
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        while(!stack.isEmpty()){
            TreeNode tmp = stack.pop();
            res.add(tmp.val);
            if(tmp.right != null){
                stack.push(tmp.right);
            }
            if(tmp.left != null){
                stack.push(tmp.left);
            }
        }

        return res;
    }
}
```



### 中序遍历-迭代

还是利用栈进行操作：用一个指针指向父节点，每次将左子节点入栈，再将指针移向左子节点，持续直到无左子节点为止，再从栈中弹出一个元素进行访问，再将指针移向右子节点，重复上述操作。

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)    return res;

        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        //初始化一个指针指向根结点的左子树
        TreeNode cur = root.left;
        while(cur != null || !stack.isEmpty()){
            //若左子树还有节点，就持续地将左子树压入栈
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            //弹出一个节点进行访问，此时访问的一定是某个父节点
            TreeNode tmp = stack.pop();
            res.add(tmp.val);
            //指针转向此父节点的右子树
            cur = tmp.right;
        }
        return res;
    }
}
```







### 后序遍历-迭代

还是利用栈操作：用一个指针指向父节点，每次将左子节点入栈，再将指针移向左子节点，持续直到无左子节点为止，再从栈中弹出一个元素，判断此结点的右子树是否为空或者是否被访问过，如果为空或者被访问过了，就访问这个结点，然后将指针置空（为了下次循环不会重复将此结点的左子树入栈，因为已经访问过了）；如果不为空，且没有被访问过，那就将这个结点重新放入栈，将指针转向右子树。

```java
public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)    return res;

        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        TreeNode cur = root.left;
        //用于记录上一次被访问的节点
        TreeNode pre = null;
        while(cur != null || !stack.isEmpty()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }

            cur = stack.pop();
            //如果右子树为空，或者右子树已经被访问过了
            if(cur.right == null || pre == cur.right){
                res.add(cur.val);
                pre = cur;
                cur = null;
            }else{
                //右子树不为空，且没有被访问过，就入栈
                stack.push(cur);
                cur = cur.right;
            }
        }

        return res;
    }
```



### 小结

以上三种遍历的迭代形式都是利用栈进行操作：

* 前序遍历很简单，每次出栈一个元素并访问，然后将右子树入栈，再左子树入栈。
* 中序遍历需要优先将左子树入栈，直到没有左子树，就弹出一个节点访问，再将指针转向右子树，重复上述操作。
* 后序遍历也是优先将左子树入栈，直到没有左子树，弹出一个元素后，先不访问，先判断右子树的情况（是否为空、是否被访问过（用一个变量存放上一次访问的节点）），如果已经访问过了或者为空，就访问此结点，并将指针置空；否则就将节点重新入栈，指针转向右子树。



### 层序遍历

**剑指 Offer 32 - I. 从上到下打印二叉树**

 从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

**剑指 Offer 32 - II. 从上到下打印二叉树 II**

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

**剑指 Offer 32 - III. 从上到下打印二叉树 III**

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

**107. 二叉树的层次遍历 II**

给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

**116.填充每个节点的下一个右侧节点指针**

给定一个完美二叉树，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```c
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```


填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。

**117.填充每个节点的下一个右侧节点指针 II**

和上题相比，就多了以下两个条件：

* 只是普通的二叉树
* 你只能使用常量级额外空间。
* 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

**222.完全二叉树的节点个数**

给出一个完全二叉树，求出该树的节点个数。

**637. 二叉树的层平均值**

给定一个非空二叉树, 返回一个由每层节点平均值组成的数组。





第一题、第二题、第四题就是层序模版题；第三题可以用一个变量表示当前的层数，如果是奇数层就正序打印，如果是偶数层就反序打印。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null)    return res;
        
        int dir = 1;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            List<Integer> list = new ArrayList<>();
            int size = queue.size();
            for(int i = 0; i < size; ++i){
                TreeNode tmp = queue.poll();
              	//奇数表示从左到右，偶数表示从右到左
                if(dir%2 == 1)  list.add(tmp.val);
                else list.add(0,tmp.val);
                if(tmp.left != null) queue.offer(tmp.left);
                if(tmp.right != null) queue.offer(tmp.right);
                
            }
            dir++;
            res.add(list);
        }
        return res;
    }
}
```



第五题：根据此树结构的特殊性，可以根据next指针和start指针实现层序遍历，利用已经填充好的部分来链接其子树。

```java
class Solution {
    //层序遍历：利用已经填充好的部分来链接其子树
    public Node connect(Node root) {
        if(root == null)    return null;
        Node start = root;

        while(start.left != null){
            Node cur = start;
            start = cur.left;

            while(cur != null){
                cur.left.next = cur.right;
                if(cur.next != null)
                    cur.right.next = cur.next.left;
                cur = cur.next;
            }
        }

        return root;
    }
}
```



第六题：和上一题十分相似，但是是更加一般的情况，所以要转换思路，用一个指针指向上一个被访问的子节点，让上一个节点来链接当前节点，就无需判断下一个节点是否存在的复杂情况。

```java
class Solution {
    Node pre = null;
    Node nextStart = null;
    //层序遍历：利用已经填充好的部分来链接其子树
    public Node connect(Node root) {
        if(root == null)    return null;

        Queue<Node> queue = new LinkedList<>();
        Node start = root;

        while(start != null){
            pre = null;
            nextStart = null;
            //遍历当前层的所有节点，遇到一个有效子节点，就将pre指向它
            for(Node cur = start; cur != null; cur = cur.next){
                if(cur.left != null){
                    doConnect(cur.left);
                }
                if(cur.right != null){
                    doConnect(cur.right);
                }
            }

            start = nextStart;
        }
        return root;
    }

    void doConnect(Node cur){
        if(pre != null){
            pre.next = cur;
        }
        if(nextStart == null){
            nextStart = cur;
        }
        pre = cur;
    }
}
```





第七题：层序遍历，由于是完全二叉树，其节点个数范围可计算为：

```markdown
2^(depth-1) - 1 ~ 2^depth - 1
```

所以可以先求深度，再求叶节点的个数：遍历到倒数第二层节点后，统计第二层节点的子节点数目。



第八题：层序遍历的模版题。







## 重建二叉树***

1. 利用前序和中序遍历的结果重建二叉树。
2. 利用中序和后序遍历的结果重建二叉树。



思路都相同：

* 将中序结果放入哈希表，形成`值:索引`的映射。

* 递归建立子树

  * 将中序区间和后/前序区间对应好。

  * 前序区间的第一个元素是根结点，后序区间的最后一个元素是根结点

  * 前序序列和后序序列中，都是左边部分的区间属于左子树，右边部分的区间属于右子树

    ```java
    //中序+前序
    root.left = rebuild(pS + 1, pS + idx - iS, iS, idx-1);
    root.right = rebuild(pS+(idx - iS) + 1, pE, idx + 1, iE);
    //中序+后序
    root.left = rebuild(pS, pE - (iE - idx) - 1, iS, idx-1);
    root.right = rebuild(pE - (iE - idx), pE - 1, idx + 1, iE);
    ```

完整代码：

```java
//中+前
class Solution {
    int[] pre;
    int[] in;
    Map<Integer, Integer> map = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        pre = preorder;
        in = inorder;
        //为中序序列建立哈希表
        for(int i = 0; i < inorder.length; ++i){
            map.put(inorder[i], i);
        }

        return rebuild(0,pre.length-1, 0, in.length-1);
    }

    //pS是根结点
    TreeNode rebuild(int pS, int pE, int iS, int iE){
        if(pS > pE){
            return null;
        }
        TreeNode root = new TreeNode(pre[pS]);
        if(pS == pE){
            return root;
        }

        int idx = map.get(root.val);

        root.left = rebuild(pS + 1, pS + idx - iS, iS, idx-1);
        root.right = rebuild(pS+(idx - iS) + 1, pE, idx + 1, iE);

        return root;
    }
}
```



```java
//中+后
class Solution {
    int[] post;
    int[] in;
    Map<Integer, Integer> map = new HashMap<>();
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        post = postorder;
        in = inorder;
        //为中序序列建立哈希表
        for(int i = 0; i < inorder.length; ++i){
            map.put(inorder[i], i);
        }

        return rebuild(0,post.length-1, 0, in.length-1);
    }

    TreeNode rebuild(int pS, int pE, int iS, int iE){
        if(pS > pE){
            return null;
        }
        TreeNode root = new TreeNode(post[pE]);
        if(pS == pE){
            return root;
        }

        int idx = map.get(root.val);

        root.left = rebuild(pS, pE - (iE - idx) - 1, iS, idx-1);
        root.right = rebuild(pE - (iE - idx), pE - 1, idx + 1, iE);

        return root;
    }
}
```







## 二叉树的路径**

**112.路径总和**

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

**113.路径总和 II**

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

**124.二叉树中的最大路径和**

给定一个非空二叉树，返回其最大路径和。本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。

**129.求根到叶子节点数字之和**

给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。例如，从根到叶子节点路径 1->2->3 代表数字 123。计算从根到叶子节点生成的所有数字之和。

**437.路径总和 III**

给定一个二叉树，它的每个结点都存放着一个整数值。找出路径和等于给定数值的路径总数。路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

**543.二叉树的直径**

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。



第一题只需要判断是否存在这样的路径即可，而第二题需要找出所有这样的路径。要找出所有这样的路径，只需要遍历所有路径，判断路径和是否符合要求。我这里用了先序遍历，当还未到达叶节点时，就将本节点的值累加到cnt上，并记录本节点到list中，然后再遍历左右子树；当到达叶节点时，比较cnt和target，如果相等就将list中的路径放入结果集，如果不相等就返回。在一层递归结束时，需要将本层的节点从list中删除。

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    int target;
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if(root == null)    return res;
        target = sum;
        dfs(root, 0, new ArrayList<Integer>());
        return res;
    }

    void dfs(TreeNode root, int cnt, List<Integer> list){
        if(root == null)    return;
        //到达了叶节点
        if(root.left == null && root.right == null){
            if(cnt + root.val == target){
                list.add(root.val);
                res.add(new ArrayList<>(list));
                //回溯
                list.remove(list.size() - 1);
            }
            return;
        }
        list.add(root.val);
        //未到达叶节点
        dfs(root.left, cnt + root.val, list);
        dfs(root.right, cnt + root.val, list);
        //回溯
        list.remove(list.size() - 1);
    }
}
```





第3题：找出一条任意长度（不为0）的路径，其和最大。考虑每个节点作为父节点时，能获得的最大路径和，左子节点的贡献+右子节点的贡献+当前节点值 = 以当前节点为父节点的最大路径，当左右节点的贡献为负数时就取贡献为0，表示不经过左/右节点。

```java
class Solution {
    int res = Integer.MIN_VALUE;
    //考虑每个节点作为父节点时，能获得的最大路径和
    public int maxPathSum(TreeNode root) {
        postOrder(root);
        return res;
    }

    //后序遍历
    int postOrder(TreeNode root){
        if(root == null){
            return 0;
        }
        //左子节点的贡献
        int left = Math.max(postOrder(root.left), 0);
        //右子节点的贡献
        int right = Math.max(postOrder(root.right), 0);
        //左子节点的贡献+右子节点的贡献+当前节点值 = 以当前节点为父节点的最大路径
        int value = left + right + root.val;
        //更新结果
        res = Math.max(res, value);
        //返回此节点的最大贡献 = max(左子树的贡献，右子树的贡献) + 当前节点值
        return root.val + Math.max(left, right);
    }
}
```





第4题：无非就是遍历每一条路径，将数字累加，非常经典的回溯题。只不过在处理字符组合为数字时，可以使用*10的方法，而不是用处理字符串的方法，这样会节省空间和时间。



第5题：遍历二叉树，抵达当前节点(即B节点)后，将前缀和累加，然后查找在前缀和上，有没有前缀和currSum-target的节点(即A节点)，存在即表示从A到B有一条路径之和满足条件的情况。

```java
class Solution {
    int sum;
    int res = 0;
    //用于记录每个节点的前缀和，用前缀和-sum来判断路径是否存在
    Map<Integer, Integer> map = new HashMap<>();
    public int pathSum(TreeNode root, int sum) {
        this.sum = sum;
        map.put(0, 1);
        dfs(root, 0);
        return res;
    }

    void dfs(TreeNode root, int preSum){
        if(root == null)    return;

        preSum += root.val;
        //此节点的前缀和 - sum是否存在于哈希表中，如果存在，说和为sum的路径存在
        if(map.containsKey(preSum - sum)){
            res += map.get(preSum - sum);
        }
        map.put(preSum, map.getOrDefault(preSum, 0) + 1);
        
        dfs(root.left, preSum);
        dfs(root.right, preSum);
        //回溯
        map.put(preSum, map.get(preSum) - 1);
    }
}
```





第六题：实际上就是找左右子树深度最大的节点。但是，直径指的是边的个数，而不是节点的个数。所以在按节点数求最大深度之后，需要将结果-1表示边数的最大值。

```java
class Solution {
    int res = 0;
    //实际上是要找出左右子树深度和最大的结点
    public int diameterOfBinaryTree(TreeNode root) {
        getDepth(root);
        return res - 1;
    }

    int getDepth(TreeNode root){
        if(root == null){
            return 0;
        }
        int left = getDepth(root.left);
        int right = getDepth(root.right);

        res = Math.max(left + right + 1, res);

        return Math.max(left,right) + 1;
    }
}
```





## 树形动态规划***

**834. 树中距离之和**

给定一个无向、连通的树。树中有 N 个标记为 0...N-1 的节点以及 N-1 条边 。第 i 条边连接节点 `edges[i][0]` 和 `edges[i][1]` 。返回一个表示节点 i 与其他所有节点距离之和的列表 ans。

**968.监控二叉树**

给定一个二叉树，我们在树的节点上安装摄像头。节点上的每个摄影头都可以监视**其父对象、自身及其直接子对象。**计算监控树的所有节点所需的最小摄像头数量。





第一题：将每个节点的距离和分为：（1）以本节点为根结点的子树所有子节点到本节点的距离和；（2）本节点到子树外的所有节点的距离和。

```java
class Solution {
    //树形动态规划
    List<List<Integer>> graph = new ArrayList<>();
    //node[i]表示以i为根结点的子树中节点的总数
    int[] node;
    //dist[i]表示以i为根结点的子树中，所有子节点到节点i的距离之和
    int[] dist;
    public int[] sumOfDistancesInTree(int N, int[][] edges) {
        //建立邻接表
        for(int i = 0; i < N; ++i){
            graph.add(new ArrayList<>());
        }

        //建立双向邻接关系
        for(int i = 0; i < edges.length; ++i){
            int x = edges[i][0];
            int y = edges[i][1];

            graph.get(x).add(y);
            graph.get(y).add(x);
        }

        node = new int[N];
        dist = new int[N];
        Arrays.fill(node, 1);
        //后序遍历计算子树的距离和
        postOrder(0, -1);
        //先序遍历计算除子树之外的距离和
        //由于根结点的距离和我们已经知道了，所以可以根据根结点的距离和来推出每个子节点的距离和
        preOrder(0, -1);

        return dist;
    }

    void postOrder(int root, int parent){
        List<Integer> children = graph.get(root);

        for(Integer child : children){
            if(child == parent) continue;
            //后序遍历
            postOrder(child, root);

            //计算子树节点总数
            node[root] += node[child];
            //计算子树节点到根结点root的距离和
            //比如根结点有子节点1，2，3，子节点有子节点4
            //那么节点1，2，3，4到节点1的距离分别为0，1，1，2
            //特别注意：边1-3被经过了两次，因为以3为根结点的子树，节点数目为2，所以必须经过两次才能到达所有节点。
            //所以根结点的距离和为所有子节点的自身的dist之和，再加上子节点到根节点的边被经过了几次，也就是子树的节点个数！
            dist[root] += node[child] + dist[child];
        }
    }

    void preOrder(int root, int parent){
        List<Integer> children = graph.get(root);
        for(Integer child : children){
            if(child == parent) continue;
            //计算每个子节点的距离和，子节点的子树内距离和我们已经得到了，就是dist[child]
            //我们要计算的就是子树外的距离和
            //观察dist[root]和dist[child]，如何通过dist[root]推出dist[child]呢？
            //首先，root和child之间有一条边，在dist[root]中，root经过这条边访问了child子树的所有节点
            //  但是在dist[child]中，我们是不需要以上这些路径的，所以这部分可以减掉：dist[root] - node[child]
            //其次，还是root和child之间的这条边，在dist[child]中，child需要经过这条边访问child子树外的所有节点
            //  所以这部分还需要加上 N - node[child]
            dist[child] = dist[root] + graph.size() - 2 * node[child];
            preOrder(child, root);
        }
    }

}
```





第二题：

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







## 树的结构**

**剑指 Offer 26. 树的子结构**

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

**100.相同的树**

给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**617.合并二叉树**

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。





第一题：判断（1）当前树A是否包含树B（2）树A的左子树是否包含树B（3）树A的右子树是否包含树B。三个条件，有一者满足要求即可。判断树是否包含树B，可以不断地比较两颗树，直到树B中的节点都比较成功了，就返回true。

```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(A == null || B == null)  return false;
        return isCompare(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B);
    }

    boolean isCompare(TreeNode A, TreeNode B){
        //B为空时，说明匹配成功
        if(B == null)   return true;
        if(A == null || A.val != B.val){
            return false;
        }

        return isCompare(A.right, B.right) && isCompare(A.left, B.left);

    }
}
```





第二题就是第一题的一个子问题，相当简单：

```java
func isSameTree(p *TreeNode, q *TreeNode) bool {
    if p == nil && q == nil {
        return true
    }
    if p == nil || q == nil {
        return false
    }

    return p.Val == q.Val && isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```





第三题：合并一棵树需要以下步骤：（1）合并当前节点。（2）合并当前节点的左子树，并作为当前节点的新左子树。（3）合并当前节点的右子树，并作为当前节点的新右子树。

```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null)  return t2;
        if(t2 == null)  return t1;

        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
}
```









## 二叉搜索树**

**剑指 Offer 33. 二叉搜索树的后序遍历序列**

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

**剑指 Offer 36. 二叉搜索树与双向链表**

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

**剑指 Offer 54. 二叉搜索树的第k大节点**

给定一棵二叉搜索树，请找出其中第k大的节点。

**剑指 Offer 68 - I. 二叉搜索树的最近公共祖先**

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

**96.不同的二叉搜索树**

给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？

**98.验证二叉搜索树**

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

**108.将有序数组转换为二叉搜索树**

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

**109.  有序链表转换二叉搜索树**

给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。本题中，一个高度平衡二叉树是指一个二叉树每个节点的左右两个子树的高度差的绝对值不超过 1。

**501.二叉搜索树中的众数**

给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

**530.二叉搜索树的最小绝对差**

给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。

**538.把二叉搜索树转换为累加树**

给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

**701.二叉搜索树中的插入操作**







二叉搜索树比较重要的特性：

* 中序遍历序列是有序的。
* 每个节点的左子树均小于本节点，右子树均大于本节点。





根据这两个性质，判断一个后序遍历序列是否属于某个二叉树，可以找出后序序列的父结点，再找出左子树和右子树的位置（左子树和右子树的位置可以通过比较元素和父节点的大小得出：一个连续的，值均小于父节点的序列，就是左子树；一个连续的，值均大于父节点的序列，就是右子树）只要不断地递归判断左右子树序列是否符合上述特征，直到只剩单个节点，就可以认为这是符合要求的二叉搜索树。

```go
func verifyPostorder(postorder []int) bool {
    n := len(postorder)
    if n == 0 {
        return true
    }
    return recur(postorder, 0, n - 1)
}

func recur(postorder []int, l int, h int) bool {
    if l >= h{
        return true
    }
    //postorder[h]是父结点
    //先从l开始找左子树
    p := l
    for postorder[p] < postorder[h] {
        p++
    }
    q := p
    for postorder[q] > postorder[h] {
        q++
    }
    return q == h && recur(postorder, l, p - 1) && recur(postorder, p, q - 1)
}
```

第二题：利用中序遍历的性质，初始化一个pre指针记录上一个节点，在访问当前节点的时候将上一个节点的right指向当前节点，将当前节点的left指向上一个节点即可，最后还需要把首尾节点连接起来。



```java
class Solution {
    Node pre = null;
    Node head = null;
    public Node treeToDoublyList(Node root) {
        if(root == null)    return null;
        inOrder(root);
        //首尾还需相连
        head.left = pre;
        pre.right = head;
        return head;
    }

    void inOrder(Node cur){
        if(cur != null){
            inOrder(cur.left);
            if(pre == null){
                head = cur;
            }else{
                pre.right = cur;
            }
            cur.left = pre;
            pre = cur;

            inOrder(cur.right);
        }
    }
}
```

第三题比较简单：为了不去统计节点个数，我们可以采用逆中序遍历，当遍历到第k个节点时，就是我们要找的第k大节点。如果题目要求是找到第k小的节点，就正中序遍历即可。

```java
class Solution {
    int res = 0;
    int k;
    public int kthLargest(TreeNode root, int k) {
        this.k = k;
        reverseInOrder(root);
        return res;
    }

    void reverseInOrder(TreeNode root){
        if(root != null){
            reverseInOrder(root.right);
            k--;
            if(k == 0){
                res = root.val;
                return;
            }
            reverseInOrder(root.left);
        }
    }
}
```



第四题：由于有二叉搜索树这个条件，这题就相对比较简单了，对于两个确定的值，我们可以使用二分搜索来找到他们的公共祖先。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null)    return null;
        TreeNode cur = root;

        while(cur != null){
          //二分搜索
            if(cur.val > p.val && cur.val > q.val){
                cur = cur.left;
            }else if(cur.val < p.val && cur.val < q.val){
                cur = cur.right;
            }else{
                break;
            }
        }
        return cur;
    }
}
```

还有一题扩展题：**剑指 Offer 68 - II. 二叉树的最近公共祖先**，这题没规定是二叉树，是一般情况下的最近公共祖先问题，无法用值去判断。

思路：p和q的最近公共祖先有三种情况：（1）公共祖先就是p（2）公共祖先就是q（3）公共祖先是p和q的某一者父节点。所以只要不断地递归判断上面三种情况即可。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || p == root || q == root)  return root;
        //找出左子树中的最近公共祖先
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        //找出右子树中的最近公共祖先
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //由于是后序遍历，肯定是从最底层节点开始判断的，所以肯定是指向最近的公共祖先
        //如果左子树没找到，那说明最近公共祖先是右子树
        if(left == null)    return right;
        //反之就是左子树
        else if(right == null)  return left;
        //左右子树都没找到，那公共祖先就是root
        return root;
    }
}
```



第五题：给定一个有序序列1...n，为了构建一棵二叉搜索树，我们可以选择某个数字作为根结点，再将其左边的序列作为左子树，右边的序列作为右子树。

![image-20210106161205601](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210106161205601.png)

![image-20210106161219966](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210106161219966.png)

![image-20210106161428163](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210106161428163.png)



```go
func numTrees(n int) int {
    G := make([]int, n+1)
    G[0] = 1
    G[1] = 1

    for i := 2; i <= n; i++ {
        for j := 1; j <= i; j++ {
            G[i] += G[j-1] * G[i-j];
        }
    }

    return G[n]
}
```

```go
func numTrees(n int) int {
    C := 1

    for i := 0; i < n; i++ {
        C = C * 2 * (2 * i + 1) / (i + 2)
    }
    return C
}
```



第六题：判断一个二叉树是否是二叉搜索树，只需要对其执行中序遍历，如果整个中序遍历序列都是有序的，那么就说明是符合要求的。

```go
var pre *TreeNode
var flag bool
func isValidBST(root *TreeNode) bool {
    if root == nil {
        return true
    }
    pre = nil
    flag = true
    inOrder(root)
    return flag
}


func inOrder(root *TreeNode){
    if root != nil {
        inOrder(root.Left)
        //当有一处不符要求，就直接返回吧，没必要判断下去了
        if flag == false{
            return
        }
        if pre != nil {
            if pre.Val < root.Val {
                flag = true
            }else{
                flag = false
            }
        }
        pre = root
        inOrder(root.Right)
    }
}
```



第七题很简单，一个有序数组要建成一个平衡二叉树，说明左右子树的深度需相差1以内，所以可以采用二分法建树，将数组不断的划分为长度相近的两部分，以mid为根结点建树。

```java
class Solution {
    //二分法建树
    public TreeNode sortedArrayToBST(int[] nums) {
        int n = nums.length;
        if(n == 0)  return null;
        
        return build(nums, 0, n-1);
    }

    TreeNode build(int[] nums, int l, int h){
        if(l > h)   return null;

        if(l == h){
            return new TreeNode(nums[l]);
        }

        int mid = ((h - l) >> 1) + l;
        TreeNode root = new TreeNode(nums[mid]);

        root.left = build(nums, l, mid - 1);
        root.right = build(nums, mid + 1, h);

        return root;
    }
}
```



第八题和上一题相似，只不过从数组变为了链表。需要采用快慢指针来寻找中点，并且需要将链表断开。

```java
class Solution {
    //还是二分法建树
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null)    return null;
        //当只剩一个节点时，直接以此节点创建树
        if(head.next == null)   return new TreeNode(head.val);
        //快慢指针找到中点
        ListNode fake_head = new ListNode();
        fake_head.next = head;
        ListNode p1 = fake_head, p2 = fake_head;
        //pre用于记录p1的前一个节点，因为p1将作为根结点，所以需要将链表从p1处断开
        ListNode pre = p1;
        //快慢指针移动
        while(p1 != null && p2 != null){
            pre = p1;
            p1 = p1.next;
            p2 = p2.next;
            if(p2 != null)  p2 = p2.next;
        }
        //以p1为根结点创建树
        TreeNode root = new TreeNode(p1.val);
        //记录p1的next作为右子树的链表
        ListNode tmp = p1.next;
        //断开链表，一分为二
        pre.next = null;
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(tmp);

        return root;
    }

}
```



第九题的解法比较有意思：还是利用二叉搜索树中序遍历是升序序列的特性，首先统计众数的个数以及众数出现的次数，其次将众数记录。这两个步骤放到同一个中序遍历函数中，用结果集是否为空来标识是哪个步骤，挺有想法的，这样可以避免写两个功能差不多的函数。

```java
class Solution {
    int maxTimes = 0;
    int times = 0;
    int resLen = 0;
    int[] res;
    TreeNode pre = null;
    public int[] findMode(TreeNode root) {
        if(root == null)    return new int[0];
        inOrder(root);
        res = new int[resLen];
        pre = null;
        times = 0;
        resLen = 0;
        inOrder(root);
        return res;
    }

    void inOrder(TreeNode root){
        if(root == null)    return;
        inOrder(root.left);
        
        if(pre != null && pre.val == root.val){
            times++;
        }else{
            times = 1;
        }

        if(times > maxTimes){
            maxTimes = times;
            resLen = 1;
        }else if(times == maxTimes){
            if(res != null){
                res[resLen] = root.val;
            }
            resLen++;
        }

        pre = root;
        inOrder(root.right);

    }
}
```



第十题很简单，就是利用中序遍历的性质来计算两数之差。

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



第十一题：逆中序遍历，将后缀和不断加到当前节点上。

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



第十二题：二分查找插入叶节点。

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null)    return new TreeNode(val);
        TreeNode cur = root;
        //二分搜索插入的位置
        while(cur != null){
            if(cur.val > val){
                if(cur.left == null){
                    cur.left = new TreeNode(val);
                    break;
                }else{
                    cur = cur.left;
                }
            }else{
                if(cur.right == null){
                    cur.right = new TreeNode(val);
                    break;
                }else{
                    cur = cur.right;
                }
            }
        }

        return root;
    }
}
```











## 平衡二叉树*







## 二叉树的深度*

**剑指 Offer 55 - I. 二叉树的深度**

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

**剑指 Offer 55 - II. 平衡二叉树**

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

**111. 二叉树的最小深度**

给定一个二叉树，找出其最小深度。最小深度是从根节点到最近叶子节点的最短路径上的节点数量。





第一题无非是找出左右子树的最大深度，+1再返回，思路很简单。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null)    return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```



第二题还是需要求左右子树的深度，只不过需要左右子树深度相差不大于1

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null)    return true;
        return (Math.abs(depth(root.left) - depth(root.right)) <= 1) && isBalanced(root.left) && isBalanced(root.right);
    }

    int depth(TreeNode root){
        if(root == null)    return 0;
        return Math.max(depth(root.left), depth(root.right)) + 1;
    }
}
```



第三题可以用递归，也可以用迭代的方法做。

* 递归的话，无非就是取左右子树的最小值，但是只有当左右子树都存在时，取最小值才有意义，当左右子树只存在一个的话，那就返回其深度，都不存在就返回0。
* 还可以用层序遍历做，只要遍历到一个叶节点，立马返回当前的层数。

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null)    return 0;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int res = 0;
        while(!queue.isEmpty()){
            res++;
            int size = queue.size();

            for(int i = 0; i < size; ++i){
                TreeNode tmp = queue.poll();
                if(tmp.left == null && tmp.right == null){
                    return res;
                }
                if(tmp.left != null)    queue.offer(tmp.left);
                if(tmp.right != null)    queue.offer(tmp.right);
            }
        }

        return res;

    }
}
```





## 二叉树的镜像*

**剑指 Offer 27. 二叉树的镜像**

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

**101.对称二叉树**

给定一个二叉树，检查它是否是镜像对称的。





第一题有两种做法，一种是直接修改原树，一种是新建节点，在写之前问清楚用哪种方式。

思路很简单：递归地交换左右子树

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if(root != null){
            TreeNode tmp = root.left;
            root.left = root.right;
            root.right = tmp;
            mirrorTree(root.left);
            mirrorTree(root.right);
        }
        return root;
    }
}
```

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null)    return null;
        TreeNode new_root = new TreeNode(root.val);
        mirror(root, new_root);
        return new_root;
    }

    void mirror(TreeNode root, TreeNode new_root){
        if(root != null){
            if(root.left != null){
                new_root.right = new TreeNode(root.left.val);
            }
            if(root.right != null){
                new_root.left = new TreeNode(root.right.val);
            }
            mirror(root.left, new_root.right);
            mirror(root.right, new_root.left);
        }
    }
}
```



第二题：镜像题的衍生，只需要将一个镜像树画出来，就很容易得知规律：比较每对对称点，判断left和right、right和left是否相等，如果相等就递归判断左右子树，注意左右子树也需要对称判断。

```java
class Solution {
  public boolean isSymmetric(TreeNode root) {
    if(root == null)    return true;
    return dfs(root,root);
  }

  boolean dfs(TreeNode root1, TreeNode root2){
    //遍历到了叶节点
    if(root1 == null && root2 == null){
      return true;
    }
    //两个结点只有一个为空，则不对称
    if(root1 == null || root2 == null){
      return false;
    }
    //两个结点的值不相等，则不对称
    if(root1.val != root2.val){
      return false;
    }
    //递归判断
    return dfs(root1.left,root2.right) && dfs(root1.right,root2.left);
  }
}
```

迭代解法：

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

