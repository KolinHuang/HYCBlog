---
title: Leetcode题解:101~200
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

[106.从中序与后序遍历序列构造二叉树](#jump106)

[107.二叉树的层次遍历 II](#jump107)

[108. 将有序数组转换为二叉搜索树](#jump108)

[109.  有序链表转换二叉搜索树](#jump109)

[110.平衡二叉树](#jump110)

[111. 二叉树的最小深度](#jump111)

[112. 路径总和](#jump112)

[113.路径总和 II](#jump113)

[116.填充每个节点的下一个右侧节点指针](#jump116)

[117.填充每个节点的下一个右侧节点指针 II](#jump117)

[120.三角形最小路径和](#jump120)

[130.被围绕的区域](#jump130)

[133.克隆图](#jump133)

[139.单词拆分](#jump139)

[141.环形链表](#jump141)

[142.环形链表II](#jump142)

[143.重排链表](#jump143)

[145.二叉树的后序遍历](#jump145)





<span id = "jump106"></span>

## 106.从中序与后序遍历序列构造二叉树



根据一棵树的中序遍历与后序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

```java
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
```


返回如下的二叉树：

    		3
       / \
      9  20
        /  \
       15   7



与剑指offer.07题类似，那个是前序加中序，这个是中序加后序。

几个注意点：

* 要对中序序列的每个元素建立哈希索引，使得能够直接找到根节点位置。
* 然后计算左右子树的规模。
* 后序序列中，根节点的左子树应从序列的最左端开始统计，即`[po_low,po_low+leftNum-1]`，右子树应为根节点往左统计的`rightNum`个元素，即`[po_high-rightNum,po_high-1]`。

重点就是在递归过程中，将对应的左右子树区间对应好。

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {

        Map<Integer,Integer> inMap = new HashMap<>();
        //为中序遍历的每个元素建立哈希索引
        for(int i = 0; i < inorder.length; ++i){
            inMap.put(inorder[i],i);
        }
        TreeNode root = build(inorder, 0,inorder.length-1, postorder,0,postorder.length-1, inMap);
        return root;
    }


    TreeNode build(int[] inorder, int in_low, int in_high, int[] postorder, int po_low, int po_high, Map<Integer, Integer> inMap){

        if( po_low > po_high){
            return null;
        }
        //此时，后序遍历区间的最后一个元素是根节点
        TreeNode root = new TreeNode(postorder[po_high]);
        if(po_low == po_high){
            return root;
        }
        //需要在中序遍历区间内找到它的左右子树
        
        int index = inMap.get(postorder[po_high]);
        //计算左右子树的规模
        int leftNum = index - in_low;
        int rightNum =  in_high - index;

        //递归重建左右子树
        root.left = build(inorder,in_low, index-1, postorder, po_low, po_low + leftNum-1, inMap);
        root.right = build(inorder, index+1, in_high, postorder, po_high - rightNum, po_high - 1, inMap);
        
        return root;
    }
}
```





<span id = "jump107"></span>

## 107. 二叉树的层次遍历 II

给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

例如：
给定二叉树 [3,9,20,null,null,15,7],

    		3
       / \
      9  20
        /  \
       15   7


返回其自底向上的层次遍历为：

```java
[
  [15,7],
  [9,20],
  [3]
]
```



还是利用队列，只是在遍历完一层后，将这一层的遍历结果添加在结果列表的最前面。

想使用LinkedList的offer和poll方法，就要实现相应的Queue接口。同理，想使用push和pop方法就要实现Stack接口。

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null){
            //res.add(new ArrayList<>());
            return res;
        }    
        Queue<TreeNode> queue = new LinkedList<>();
      	//根节点入队
        queue.offer(root);
      	//层序遍历
        while(!queue.isEmpty()){
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            for(int i = 0; i < size; ++i){
                TreeNode tmp = queue.poll();
              	//子节点入队
                if(tmp.left != null)
                    queue.offer(tmp.left);
                if(tmp.right != null)
                    queue.offer(tmp.right);
                list.add(tmp.val);
            }
            res.add(0,list);
        }
        return res;
    }
}
```







<span id="jump108"></span>

## 108. 将有序数组转换为二叉搜索树

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:



    给定有序数组: [-10,-3,0,5,9],
    
    一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树： 		
          0
    		 / \
        -3  9
       /   /
     -10  5

BST的中序遍历是升序，所以本题等同于根据中序遍历的序列恢复二叉搜索树。若是普通的二叉搜索树，只需要选择序列中的任意一元素作为根节点，以该元素左边的升序序列构建左子树，右边的升序序列构建右子树，这样得到的就是一颗二叉搜索树了。

本题要求建立平衡二叉搜索树，因此每次需要选择升序序列的中间元素作为根节点。

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
    public TreeNode sortedArrayToBST(int[] nums) {
        return dfs(nums, 0, nums.length-1);
    }

    public TreeNode dfs(int[] nums, int l, int h){
        if(l > h) return null;
        int mid = l + (h-l)/2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = dfs(nums, l, mid-1);
        root.right = dfs(nums, mid+1,h);
        return root;

    }
}
```

<span id="jump109"></span>

## 109.  有序链表转换二叉搜索树

给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:



    给定的有序链表： [-10, -3, 0, 5, 9],
    
    一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：
    			 0
          / \
        -3   9
       /   /
     -10  5

具体地，设当前链表的左端点为left，右端点 right，包含关系为「左闭右开」，即left 包含在链表中而 right 不包含在链表中。我们希望快速地找出链表的中位数节点 mid。

```markdown
为什么要设定「左闭右开」的关系？由于题目中给定的链表为单向链表，访问后继元素十分容易，但无法直接访问前驱元素。因此在找出链表的中位数节点mid 之后，如果设定「左闭右开」的关系，我们就可以直接用 (left,mid) 以及 (mid.next,right) 来表示左右子树对应的列表了。并且，初始的列表也可以用(head,null) 方便地进行表示，其中null 表示空节点。
```

找出链表中位数节点的方法多种多样，其中较为简单的一种是「快慢指针法」。初始时，快指针 fast 和慢指针slow 均指向链表的左端点 left。我们将快指针fast 向右移动两次的同时，将慢指针slow 向右移动一次，直到快指针到达边界（即快指针到达右端点或快指针的下一个节点是右端点）。此时，慢指针对应的元素就是中位数。

在找出了中位数节点之后，我们将其作为当前根节点的元素，并递归地构造其左侧部分的链表对应的左子树，以及右侧部分的链表对应的右子树。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        return buildTree(head,null);
    }

    TreeNode buildTree(ListNode left, ListNode right){
        if(left == right)
            return null;
        ListNode mid = getMidian(left,right);
        TreeNode root = new TreeNode(mid.val);
        root.left = buildTree(left,mid);
        root.right = buildTree(mid.next,right);
        return root;
    }

    ListNode getMidian(ListNode left, ListNode right){
        if(left == right)
            return null;
        ListNode fast = left;
        ListNode slow = left;
        while(fast != right && fast.next != right){
            fast = fast.next;
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```



<span id="jump110"></span>

## 110.平衡二叉树

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

示例 1:

给定二叉树 [3,9,20,null,null,15,7]

    		3
       / \
      9  20
        /  \
       15   7

返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]

           1
          / \
         2   2
        / \
       3   3
      / \
     4   4

返回 false 。



### 自顶向下的递归

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
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        if(root.left == null && root.right == null)
            return true;
      	//当左右子树深度差大于1时，返回false
        if(Math.abs(getDepth(root.left) - getDepth(root.right)) > 1)
            return false;
      	//继续判断左右子树
        return isBalanced(root.left) && isBalanced(root.right);
    }
		//获取二叉树深度
    int getDepth(TreeNode root){
        if(root == null)
            return 0;
        return Math.max(getDepth(root.left),getDepth(root.right))+1;
    }
}
```



### 自底向上的递归

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) >= 0;
    }

    public int height(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
        if (leftHeight == -1 || rightHeight == -1 || Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }
}

```



<span id="jump111"></span>

## 111. 二叉树的最小深度

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明: 叶子节点是指没有子节点的节点。

示例:

给定二叉树 `[3,9,20,null,null,15,7]`,

    		3
       / \
      9  20
        /  \
       15   7

返回它的最小深度  2.



### 深度优先

递归判断，当前节点为空时返回0；当前节点不为空，遍历到叶节点时返回1，遍历到非叶节点时，返回左右子树的最小深度。

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
    public int minDepth(TreeNode root) {
        if(root == null)    return 0;
        if(root.left == null && root.right == null)
            return 1;
        int left = Integer.MAX_VALUE,right = Integer.MAX_VALUE;
        if(root.left != null)   left = minDepth(root.left);
        if(root.right != null)  right = minDepth(root.right);
        return Math.min(left,right)+1;

}
```

### 广度优先

当我们找到一个叶子节点时，直接返回这个叶子节点的深度。广度优先搜索的性质保证了最先搜索到的叶子节点的深度一定最小。

```java
class Solution {
    class QueueNode {
        TreeNode node;
        int depth;

        public QueueNode(TreeNode node, int depth) {
            this.node = node;
            this.depth = depth;
        }
    }

    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }

        Queue<QueueNode> queue = new LinkedList<QueueNode>();
        queue.offer(new QueueNode(root, 1));
        while (!queue.isEmpty()) {
            QueueNode nodeDepth = queue.poll();
            TreeNode node = nodeDepth.node;
            int depth = nodeDepth.depth;
            if (node.left == null && node.right == null) {
                return depth;
            }
            if (node.left != null) {
                queue.offer(new QueueNode(node.left, depth + 1));
            }
            if (node.right != null) {
                queue.offer(new QueueNode(node.right, depth + 1));
            }
        }

        return 0;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/solution/er-cha-shu-de-zui-xiao-shen-du-by-leetcode-solutio/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



<span id="jump112"></span>

## 112. 路径总和

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

说明: 叶子节点是指没有子节点的节点。

示例: 
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。



### 先序遍历

用二叉树的前序遍历，每遍历到一个节点，`sum-=node.val`，当遍历到叶节点时，判断sum是否为0，若为0则存在路径，否则继续遍历。

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
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) return false;
        return preOrderTraversal(root,sum);

    }

    //树的先序遍历
    public Boolean preOrderTraversal(TreeNode root, int sum){

        if(root == null) return false;
        //是叶节点
        if(root.left == null && root.right==null){
            return sum-root.val == 0;
        }
        int tmp = sum - root.val;
        return (preOrderTraversal(root.left,tmp) || preOrderTraversal(root.right,tmp));
    }
}
```





<span id = "jump113"></span>

## 113.路径总和 II

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

说明: 叶子节点是指没有子节点的节点。

示例:
给定如下二叉树，以及目标和 sum = 22，

```java
          5
         / \
        4   8
       /   / \
      11  13  4
     /  \    / \
    7    2  5   1
```

返回:

```java
[
   [5,4,11,2],
   [5,8,4,5]
]
```




回溯法，先序遍历二叉树，当遇到了叶节点时，判断是否该路径和是否等于目标值。在一轮递归结束，需要返回到父节点时进行回溯，删除路径列表中本节点。

```java
class Solution {
    List<List<Integer>> res;
    int sum;
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        res = new ArrayList<>();
        this.sum = sum;
        if(root == null){
            return res;
        }
        dfs(root, 0, new ArrayList<Integer>());
        return res;
    }
    //先序遍历
    void dfs(TreeNode root, int cnt, List<Integer> list){
        if(root == null)
            return;
        //找到了叶节点
        if(root.left == null && root.right == null){
            if(cnt + root.val == sum){
                list.add(root.val);
                res.add(new ArrayList<Integer>(list));
                list.remove(list.size()-1);
            }
            return;
        }
        //当前不是叶节点，就把他加到路径中
        cnt += root.val;
        //不能像这样剪枝，节点值可能为负
        /*if(cnt >= sum){
            return;
        }*/
        list.add(root.val);
        dfs(root.left,cnt,list);
        dfs(root.right,cnt,list);
        list.remove(list.size()-1);
    }
}
```







<span id = "jump116"></span>

## 116.填充每个节点的下一个右侧节点指针

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

初始状态下，所有 next 指针都被设置为 NULL。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/15/116_sample.png)

**提示：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。



不能用层序遍历，假设我们即将为第i+1行建立next关系，那么由于我们已经对第i行成功建立了next关系，所以我们可以用第i行的next指针遍历第i行，而第i+1行所有的节点都是第i行的子节点，所以也能直接被遍历到。具体地：

```markdown
使用一个nextStart指针指向下一行的起点。例如图中的1、2、4节点。
在建立第i+1行关系时，迭代第i行的节点cur，如2，
					令2.left.next = 2.right;
					令2.right.next = null or 2.next.left;	//当2.next为空时，2.right.next就为空。
然后移动指针cur，指向3。
在每行的开始，更新nextStart指针，使其指向第一个节点的left。每行结束时，更新迭代指针cur为nextStart，开始遍历下一行。
```

```java
class Solution {
    public Node connect(Node root) {
        if(root == null)    return root;
        root.next = null;
        //下一行的开头指针
        Node nextStart = root;
        while(nextStart != null){
            //开始遍历这一行
            Node cur = nextStart;
            //更新nextStart指针，为下一次迭代作准备
            nextStart = cur.left;
            while(cur != null && cur.left != null){
                cur.left.next = cur.right;
                if(cur.next == null){
                    cur.right.next = null;
                }else{
                    cur.right.next = cur.next.left;
                }
                cur = cur.next;
            }
        }
        return root;
    }
}
```











<span id = "jump117"></span>

## 117.填充每个节点的下一个右侧节点指针 II

给定一个二叉树

```c++
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```


填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。

初始状态下，所有 next 指针都被设置为 NULL。

进阶：

* 你只能使用常量级额外空间。

* 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。



层序遍历，在遍历到每层最后一个节点之前，让每个节点的next都指向队首元素。最后一个节点的next指向null。

```java
class Solution {
    public Node connect(Node root) {
        if(root == null){
            return root;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; ++i){
                
                Node tmp = queue.poll();
                //是否遍历到每一层的最后一个节点
                tmp.next = i <= size-2 ? queue.peek() : null;
                
                if(tmp.left != null)   queue.offer(tmp.left);
                if(tmp.right != null)   queue.offer(tmp.right);
            }
        }
        return root;
    }
}
```

进阶：

假设某一层已经建立好了next指针关系，那么我们就可以通过遍历这一层来建立下一层的next指针。具体的：

```markdown
使用一个指针nextStart指向下一次遍历需要访问的节点，在这次遍历访问第一个节点时进行赋值。
再使用一个指针pre指向遍历这一层的某个节点时，它的前一个节点。
遍历这一层时，通过next指针，遍历该层所有节点，为这些节点的子节点建立next指针关系。
每遍历完一层，用nextStart指针跳向下一层的开头继续遍历。
```

```java
class Solution {
    Node pre = null;
    Node nextStart = null;
    public Node connect(Node root) {
        Node start = root;
        while(start != null){
            pre = null;
            nextStart = null;
          	//遍历每一层
            for(Node p = start; p != null; p = p.next){
                if(p.left != null){
                    doConnect(p.left);
                }
                if(p.right != null){
                    doConnect(p.right);
                }
            }
          	//移向下一层
            start = nextStart;
        }
        return root;
    }

    void doConnect(Node p){
        //如果不是每层的第一个节点，就直接把p给pre.next
        if(pre != null){
            pre.next = p;
        }
        //如果是每层的开头，就记录这个节点，为下次遍历作准备
        if(nextStart == null){
            nextStart = p;
        }
        //让pre移向下一个节点
        pre = p;
    }
}
```









<span id="jump120"></span>

## 120. 三角形最小路径和

给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。

 

例如，给定三角形：

```markdown
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```


自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

 

说明：

如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题，那么你的算法会很加分。



### 动态规划

从倒数第二层开始，将每个节点的值加上子节点的最小值，最终根节点的值即为最小路径和。



```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int l = triangle.size();

        if(l == 0) return 0;

        if(l == 1) return triangle.get(0).get(0);

        int min = 0;

        int[][] res = new int[l][l];

        for(int i = 0; i < l; ++i)
            for(int j = 0; j <=i; ++j){
                res[i][j] = triangle.get(i).get(j);
            }

        for(int i = l-2; i >=0; --i)
            for(int j = 0; j <= l-2; ++j){
                res[i][j] += Math.min(res[i+1][j],res[i+1][j+1]);
            }

        return res[0][0];
    }
}
```



```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] f = new int[n][n];
        f[0][0] = triangle.get(0).get(0);
        for (int i = 1; i < n; ++i) {
            f[i][0] = f[i - 1][0] + triangle.get(i).get(0);
            for (int j = 1; j < i; ++j) {
                f[i][j] = Math.min(f[i - 1][j - 1], f[i - 1][j]) + triangle.get(i).get(j);
            }
            f[i][i] = f[i - 1][i - 1] + triangle.get(i).get(i);
        }
        int minTotal = f[n - 1][0];
        for (int i = 1; i < n; ++i) {
            minTotal = Math.min(minTotal, f[n - 1][i]);
        }
        return minTotal;
    }
}
```



```java
//空间优化
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] f = new int[n];
        f[0] = triangle.get(0).get(0);
        for (int i = 1; i < n; ++i) {
            f[i] = f[i - 1] + triangle.get(i).get(i);
            for (int j = i - 1; j > 0; --j) {
                f[j] = Math.min(f[j - 1], f[j]) + triangle.get(i).get(j);
            }
            f[0] += triangle.get(i).get(0);
        }
        int minTotal = f[0];
        for (int i = 1; i < n; ++i) {
            minTotal = Math.min(minTotal, f[i]);
        }
        return minTotal;
    }
}
```



<span id="jump130"></span>

## 130. 被围绕的区域



给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

示例:

```java
X X X X
X O O X
X X O X
X O X X
```


运行你的函数后，矩阵变为：

```java
X X X X
X X X X
X X X X
X O X X
```


解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。



 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。

遍历所有边界上的'O'，将与他相连的'O'染色。

最后遍历所有元素，被染色的恢复为'O'，未被染色的'O'修改为‘X’



```java
class Solution {
    public void solve(char[][] board) {
        if(board.length == 0 || board[0].length == 0) return;
        int row = board.length;
        int col = board[0].length;

        if(row <= 2 || col <= 2) return;
        //标记
        for(int i = 0; i < row; ++i){
            for(int j = 0; j < col; ++j){
                //在边界上
                if((i == 0 || i == row-1 || j == 0 || j == col-1) && board[i][j] == 'O'){
                    //染色
                  	dfs(board,i,j);

                }
            }
        }
        for(int i = 0; i < row; ++i){
            for(int j = 0; j < col; ++j){
                if(board[i][j] == 'O')
                    board[i][j] = 'X';
                if(board[i][j] == 'M')
                    board[i][j] = 'O';
            }
        }

    }
    void dfs(char[][] board, int x, int y){
        int row = board.length;
        int col = board[0].length;
        board[x][y] = 'M';
        if(x + 1 < row && board[x+1][y] == 'O')
            dfs(board,x+1,y);
        if(x - 1 >= 0 && board[x-1][y] == 'O')
            dfs(board,x-1,y);
        if(y + 1 < col && board[x][y+1] == 'O')
            dfs(board,x,y+1);
        if(y - 1 >= 0 && board[x][y-1] == 'O')
            dfs(board,x,y-1);
    }
  
  /*void dfs(vector<vector<char>>& board, int x, int y) {
        if (x < 0 || x >= n || y < 0 || y >= m || board[x][y] != 'O') {
            return;
        }
        board[x][y] = 'A';
        dfs(board, x + 1, y);
        dfs(board, x - 1, y);
        dfs(board, x, y + 1);
        dfs(board, x, y - 1);
    }*/


}
```



**参考**

https://leetcode-cn.com/problems/surrounded-regions/solution/bfsdi-gui-dfsfei-di-gui-dfsbing-cha-ji-by-ac_pipe/



<span id="jump133"></span>

## 133. 克隆图

给你无向连通图中一个节点的引用，请你返回该图的深拷贝（克隆）。

图中的每个节点都包含它的值 val（int） 和其邻居的列表（list[Node]）。

```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```




测试用例格式：

简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（`val = 1`），第二个节点值为 2（`val = 2`），以此类推。该图在测试用例中使用邻接列表表示。

邻接列表 是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。

给定节点将始终是图中的第一个节点（值为 1）。你必须将 给定节点的拷贝 作为对克隆图的引用返回。

```java
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
解释：
图中有 4 个节点。
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。
```

```java
输入：adjList = [[]]
输出：[[]]
解释：输入包含一个空列表。该图仅仅只有一个值为 1 的节点，它没有任何邻居。
```

```java
输入：adjList = []
输出：[]
解释：这个图是空的，它不含任何节点。
```

```java
输入：adjList = [[2],[1]]
输出：[[2],[1]]
```



提示：

* 节点数不超过 100 。
* 每个节点值 Node.val 都是唯一的，1 <= Node.val <= 100。
* 无向图是一个简单图，这意味着图中没有重复的边，也没有自环。
* 由于图是无向的，如果节点 p 是节点 q 的邻居，那么节点 q 也必须是节点 p 的邻居。
* 图是连通图，你可以从给定节点访问到所有节点。



### 深搜

用一个哈希表来存储已访问顶点，键是原图中的顶点，值是拷贝图中对应的顶点。

从原图的某个顶点开始遍历，判断是否以拷贝，若是，则返回哈希表中的值；否则，拷贝该顶点，并放入哈希表中，随即遍历该顶点的邻居，执行深度优先搜索，并将返回值赋给拷贝顶点的邻接表。



```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    private HashMap<Node,Node> visited = new HashMap<>();
    public Node cloneGraph(Node node) {
        if(node == null) return node;
        return dfs(node);
    }

    Node dfs(Node node){
        if(visited.containsKey(node))
            return visited.get(node);
        Node myNode = new Node(node.val);
        visited.put(node,myNode);
        for(Node n : node.neighbors){
            myNode.neighbors.add(dfs(n));
        }
        return myNode;
    }
}
```



### 广搜



```java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return node;
        }

        HashMap<Node, Node> visited = new HashMap();

        // 将题目给定的节点添加到队列
        LinkedList<Node> queue = new LinkedList<Node> ();
        queue.add(node);
        // 克隆第一个节点并存储到哈希表中
        visited.put(node, new Node(node.val, new ArrayList()));

        // 广度优先搜索
        while (!queue.isEmpty()) {
            // 取出队列的头节点
            Node n = queue.remove();
            // 遍历该节点的邻居
            for (Node neighbor: n.neighbors) {
                if (!visited.containsKey(neighbor)) {
                    // 如果没有被访问过，就克隆并存储在哈希表中
                    visited.put(neighbor, new Node(neighbor.val, new ArrayList()));
                    // 将邻居节点加入队列中
                    queue.add(neighbor);
                }
                // 更新当前节点的邻居列表
                visited.get(n).neighbors.add(visited.get(neighbor));
            }
        }

        return visited.get(node);
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/clone-graph/solution/ke-long-tu-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



<span id="jump139"></span>

## 139.单词拆分



给定一个**非空**字符串 *s* 和一个包含**非空**单词列表的字典 *wordDict*，判定 *s* 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

示例 1：

```markdown
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
```

示例 2：

```markdown
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
```

示例 3：

```markdown
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

### 动态规划

设`dp[i]`为字符串前i个字符组成的单词拆分后，能否被字典表示。令`dp[0]=true`，表示空字符串能够被表示。

对于字符串前`i`个字符，判断子串`[j,i-1]`能否被字典表示。目前我们已经确定了`dp[i-1]`的值，接下来要确定`dp[i]`的值，从左向右移动指针`j`,判断是否有这么一种情况出现：即`d[j]=true`，并且子串`[j,i-1]`能够被字典表示。这样一来，字符串的前`i`个字符组成的单词拆分后也能够字典表示，令`dp[i]=true`。

状态方程：

```markdown
						dp[i] = dp[j] && isMatch(s.substring(j,i-1));
```

### 答案

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        //dp[i]表示字符串s的前i个字符串是否能被字典表示
        //初始化dp[0]为true 
        for(int i = 1; i <= s.length(); ++i){
            for(int j = 0; j < i; ++j){
                if(dp[j] && wordDict.contains(s.substring(j,i))){
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
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







<span id = "jump143"></span>

## 143.重排链表

给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例 1:

```java
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
```

示例 2:

```java
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
```



先统计结点个数，将后一半节点入栈，再遍历前一半节点，将栈中节点依次错位插入到其中。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reorderList(ListNode head) {
        ListNode new_head = new ListNode(-1);
        ListNode cur = head;
        //一次遍历统计节点个数
        int nums = 0;
        while(cur != null){
            nums++;
            cur = cur.next;
        }
        //将后一半的节点入栈
        Stack<ListNode> stack = new Stack<>();
        int index = (nums % 2 == 0) ? (nums / 2) : (nums / 2 + 1);
        cur = head;
        //定位到链表的中间节点
        while(index != 0){
            index--;
            cur = cur.next;
        }
        ListNode ptr = cur;
        while(cur != null){
            stack.push(cur);
            cur = cur.next;
        }
        //将栈中节点依次插入到链表中
        cur = head;
        while(!stack.isEmpty()){
            ListNode tmp = stack.pop();
            tmp.next = cur.next;
            cur.next = tmp;
            cur = tmp.next;
        }
        if(ptr != null && ptr.next != null)
           ptr.next.next = null;
        return head;
    }
}
```













<span id = "jump145"></span>

## 145.二叉树的后序遍历

给定一个二叉树，返回它的 后序 遍历。

示例:

```java
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
```

进阶: 递归算法很简单，你可以通过迭代算法完成吗？



1. 先将左子树依次入栈，直到叶节点。

2. 判断右子树是否为空，或者右子树是否访问过

   2.1 如果是，就可以访问根节点了。

   2.2 如果不是，就需要把根节点再次入栈，将指针转向右子树，跳到1.1继续判断。

可以用一个指针`pre`指向上一个已经访问过的节点。

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode pre = null;
        while(root != null || !stack.isEmpty()){
            while(root != null){
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();

            //当右子树为空时，就访问此节点；若右子树已经被访问过了，就访问此节点
            if(root.right == null || pre == root.right){
                res.add(root.val);
                pre = root;
                root = null;
            }else{
                //右子树不为空，就继续根节点入栈，转向右子树
                stack.push(root);
                root = root.right;
            }
        }
        return res;
    }

}
```