---
title: Leetcode题解
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







## 树的结构**

**剑指 Offer 26. 树的子结构**

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

**100.相同的树**

给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。







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

