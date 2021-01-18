---
title: Leetcode题解
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

[5.最长回文子串](#jump5)

[16.最接近三数之和](#jump16)

[17.电话号码的字母组合](#jump17)

[18.四数之和](jump18)

[19.删除链表的倒数第N个节点](#jump19)

[24.两两交换链表中的节点](#jump24)

[37.解数独](#jump37)

[39.组合总和1](#jump39)

[40.组合总和2](#jump40)

[43.字符串相乘](#jump43)

[47.全排列](#jump47)

[50.Pow(x, n)](#jump50)

[51.N 皇后](#jump51)

[52.N皇后 II[tag]](#jump52)

[57.插入区间](#jump57)

[60.第k个排列[tag]](#jump60)

[62.不同路径](#jump62)

[63.含障碍物网格中的不同路径](#jump63)

[67.二进制求和](#jump67)

[75.颜色分类](#jump75)

[77.组合[tag:字典序]](#jump77)

[79.单词搜索](#jump79)

[93. 恢复IP地址](#jump93)

[94.二叉树的中序遍历](#jump94)

[96. 不同的二叉搜索树](#jump96)

[98.验证二叉搜索树](#jump98)

[100. 相同的树](#jump100)

[103. 二叉树的锯齿形层序遍历](#jump103)

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

[127.单词接龙](#jump127)

[129.求根到叶子节点数字之和](#jump129)

[130.被围绕的区域](#jump130)

[133.克隆图](#jump133)

[135. 分发糖果](#jump135)

[139.单词拆分](#jump139)

[141.环形链表](#jump141)

[142.环形链表II](#jump142)

[143.重排链表](#jump143)

[144.二叉树的前序遍历](#jump144)

[145.二叉树的后序遍历](#jump145)

[147.对链表进行插入排序](#jump147)

[152.乘积最大子数组](#jump152)

[164.最大间距](#jump164)

[189. 旋转数组](#jump189)

[201.数字范围按位与](#jump201)

[204.计数质数](#jump204)

[206.反转链表](#jump206)

[209.寻找长度最小的子数组](#jump209)

[214.最短回文串](#jump214)

[216.组合总和3](#jump216)

[221.最大正方形](#jump221)

[222.完全二叉树的节点个数](#jump222)

[228. 汇总区间](#jump228)

[235.二叉树的最近公共祖先](#jump235)

[242.有效的字母异位词](#jump242)

[257.二叉树的所有路径](#jump257)

[316. 去除重复字母](#jump316)

[321.拼接最大数[tag]](#jump321)

[327.区间和的个数[tag]](#jump327)

[328.奇偶链表](#jump328)

[332.重新安排行程](#jump332)

[336.回文对](#jump336)

[349.两个数组的交集](#jump349)

[376. 摆动序列](#jump376)

[378.有序矩阵中第K小元素](#jump378)

[381.O(1) 时间插入、删除和获取随机元素 - 允许重复](#jump381)

[387. 字符串中的第一个唯一字符](#jump387)

[389. 找不同](#jump389)

[399. 除法求值](#jump399)

[344.反转字符串](#jumo344)

[402.移掉K位数字](#jump402)

[404.左叶子之和](#jump404)

[416.分割等和子集](#jump416)

[435. 无重叠区间](#jump435)

[437.路径总和 III](#jump437)

[438.找到字符串中所有字母异位词](#jump438)

[452.用最少数量的箭引爆气球](#jump452)

[455. 分发饼干](#jump455)

[459.重复的子字符串](#jump459)

[463.岛屿的周长](#jump463)

[486.预测赢家](#jump486)

[493.翻转对](#jump493)

[501.二叉搜索树中的众数](#jump501)

[509. 斐波那契数](#jump509)

[514.自由之路](#jump514)

[529.扫雷游戏](#jump529)

[530.二叉搜索树的最小绝对差](#jump530)

[538.将二叉搜索树转换为累加树](#jump538)

[543.二叉树的直径](#jump543)

[546.移除盒子](#jump546)

[547. 省份数量](#jump547)

[557.反转字符串中的单词 III](#jump557)

[605. 种花问题](#jump605)

[617.合并二叉树](#jump617)

[621.任务调度器](#jump621)

[637.二叉树的层平均值](#jump637)

[647.回文子串](#jump647)

[649. Dota2 参议院](#jump649)

[657.机器人能否返回原点](#jump657)

[659.分割数组为连续子序列](#jump659)

[679.24点游戏](#jump679)

[684. 冗余连接](#jump684)

[685.冗余连接2](#jump685)

[696.计数二进制子串](#jump696)

[701.二叉搜索树中的插入操作tag](jump701)

[711.宝石和石头](#jump711)

[714. 买卖股票的最佳时机含手续费](#jump714)

[718.最长公共连续子数组](#jump718)

[721. 账户合并](#jump721)

[733.图像渲染](#jump733)

[763.划分字母区间](#jump763)

[767.重构字符串](#jump767)

[830. 较大分组的位置](#jump830)

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

[947. 移除最多的同行或同列石头](#jump947)

[968.监控二叉树](#jump968)

[973.最接近原点的 K 个点](#jump973)

[976.三角形的最大周长](#jump976)

[977.有序数组的平方](#jump977)

[1002.查找常用字符](#jump1002)

[1018. 可被 5 整除的二进制前缀](#jump1018)

[1024.视频拼接](#jump1024)

[1030.距离顺序排列矩阵单元格](#jump1030)

[1202. 交换字符串中的元素](#jump1202)

[1203. 项目管理](#jump1203)

[1207.独一无二的出现次数](#jump1207)

[1356.根据数字二进制下 1 的数目排序](#jump1356)

[1365.有多少小于当前数字的数字](#jump1365)

[1370.上升下降字符串](#jump1370)

[LCP 19.秋叶收藏集](#jumplcp19)















<span id="jump16"></span>


##  16.最接近三数之和

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。



示例：

```markdown
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

提示：

```markdown
3 <= nums.length <= 10^3
-10^3 <= nums[i] <= 10^3
-10^4 <= target <= 10^4
```



### 暴力解法

固定一个数，遍历其他两个数，找出`target - sum`最小的`sum`作为结果。



```java
int min = Integer.MAX_VALUE;
        int x=0,y=0,z=0;
        for(int i = 0; i < nums.length; ++i)
            for(int j = i + 1; j < nums.length; ++j)
                for(int k = j + 1; k < nums.length; ++k){
                    int tmp = (nums[i] + nums[j] + nums[k]);
                    int m = min;
                    min = Math.min(Math.abs(target - tmp), min);
                    if(m != min){
                        x = i;
                        y = j;
                        z = k;
                    }
                }
        return nums[x]+nums[y]+nums[z];
```



### 排序+双指针

我们首先考虑枚举第一个元素 a，对于剩下的两个元素 b 和 c，我们希望它们的和最接近 `target - a`。对于 b 和 c，如果它们在原数组中枚举的范围（既包括下标的范围，也包括元素值的范围）没有任何规律可言，那么我们还是只能使用两重循环来枚举所有的可能情况。因此，我们可以考虑对整个数组进行升序排序，借助双指针，我们就可以对枚举的过程进行优化。

```markdown
如果a+b+c≥target，那么就将 p_c向左移动一个位置；
如果a+b+c<target，那么就将 p_b向右移动一个位置。
```

#### 

```java
        Arrays.sort(nums);
        int best = 10000;
        int x=0,y=0,z=0;
        for(int i = 0; i < nums.length-2; ++i){
          //保证此次第i个数与前一个不同
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
          //维护两个指针，l和r，分别代表b和c
            int l = i+1,r = nums.length-1;
            while(l < r){
              //计算当前的和
                int sum = nums[i]+nums[l]+nums[r];
                if(sum == target)
                    return sum;
              //若当前的和跟target更接近，则更新距离最小值
                if(Math.abs(sum - target) < Math.abs(best -target)){
                    best = sum;
                }
              //移动指针l
                if(sum < target){
                    int l0 = l;
                  //保证移动后，与前一个数字不同
                    while(l0 < r && nums[l0] == nums[l])
                        ++l0;
                    l = l0;
                }
              //移动指针r
                if(sum > target){
                    int r0 = r;
                  //保证移动后，与前一个数字不同
                    while(l < r0 && nums[r0] == nums[r])
                        --r0;
                    r = r0;
                }
            }
        }
        return best;
```









<span id = "jump18"></span>

## 18.四数之和

给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：

答案中不可以包含重复的四元组。

示例：

```java
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```



回溯法：

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> fourSum(int[] nums, int target) {
        
        res = new ArrayList<>();
        if(nums.length == 0)    return res;
        Arrays.sort(nums);
        dfs(target, nums, 0, 0, 0, new ArrayList<Integer>());
        return res;
    }


    void dfs(int target, int[] nums, int cur, int size, int sum, List<Integer> list){

        if(list.size() == 4){
            if(sum == target){
                res.add(new ArrayList<>(list));
            }
            return;
        }

        Set<Integer> set = new HashSet<>();
        for(int i = cur; i < nums.length; ++i){
            //去重
            if(set.contains(nums[i]))   continue;
            //剪枝：当剩余的元素个数不满足所需个数时，返回
            if(nums.length - i < (4-size)) return;
            //剪枝：当剩余所需元素都用后续最小元素来填充时，还是超过了target，就返回
            if(i < nums.length-1 && sum + nums[i] + (3-size) * nums[i+1] > target) return;
            //剪枝：当剩余所需元素都用后续最大元素来填充时，还是比target小时，就跳过此次添加，去添加更大的元素，期望逼近target
            if(i < nums.length-1 && sum + nums[i] + (3-size) * nums[nums.length-1] < target) continue;
            //回溯
            set.add(nums[i]);
            list.add(nums[i]);
            dfs(target, nums, i+1, size + 1, sum + nums[i], list);
            list.remove(list.size()-1);
        }
    }
}
```



排序+双指针：

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> quadruplets = new ArrayList<List<Integer>>();
        if (nums == null || nums.length < 4) {
            return quadruplets;
        }
        Arrays.sort(nums);
        int length = nums.length;
        for (int i = 0; i < length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            if (nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
                break;
            }
            if (nums[i] + nums[length - 3] + nums[length - 2] + nums[length - 1] < target) {
                continue;
            }
            for (int j = i + 1; j < length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                if (nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target) {
                    break;
                }
                if (nums[i] + nums[j] + nums[length - 2] + nums[length - 1] < target) {
                    continue;
                }
                int left = j + 1, right = length - 1;
                while (left < right) {
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum == target) {
                        quadruplets.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        left++;
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        return quadruplets;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/4sum/solution/si-shu-zhi-he-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```











<span id = "jump24"></span>

## 24.两两交换链表中的节点

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例:

```java
给定 1->2->3->4, 你应该返回 2->1->4->3.
```



```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode fake_head = new ListNode(-1);
        fake_head.next = head;
        //建立一个伪头节点
        ListNode p = fake_head;
        ListNode q = head;
        //存在成对的未交换节点才继续
        while(q != null && q.next != null){
            //交换节点
            p.next = q.next;
            q.next = p.next.next;
            p.next.next = q;
            //交换完毕，移向下一组
            p = q;
            q = q.next;
        }
        //返回头节点
        return fake_head.next;
    }
}
```









<span id = "jump37"></span>

## 37.解数独

编写一个程序，通过已填充的空格来解决数独问题。

一个数独的解法需遵循如下规则：

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
空白格用 '.' 表示。

**Note:**

- 给定的数独序列只包含数字 `1-9` 和字符 `'.'` 。
- 你可以假设给定的数独只有唯一解。
- 给定数独永远是 `9x9` 形式的。



回溯法，这题的难点在于（1）方块区域的访问判断；（2）下一步往哪儿递归；（3）什么条件下回溯。

```java
class Solution {
    boolean[][] row_visited;
    boolean[][] col_visited;
    boolean[][][] block_visited;
    public void solveSudoku(char[][] board) {
        //判断某行是否访问过k元素,1<=k<=9
        row_visited = new boolean[9][10];
        //判断某列是否访问过k元素,1<=k<=9
        col_visited = new boolean[9][10];
        //判断某3*3方块区域是否访问过k元素,1<=k<=9
        block_visited = new boolean[3][3][10];
        //预填充，自带的数字先加入访问数组中
        for(int i = 0; i < 9; ++i){
            for(int j = 0; j < 9; ++j){
                if(board[i][j] != '.'){
                    int k = board[i][j] - '0';
                    row_visited[i][k] = true;
                    col_visited[j][k] = true;
                    block_visited[i/3][j/3][k] = true;
                }
            }
        }

        dfs(board,0,0);

    }

    boolean dfs(char[][] board, int x, int y){
        //若首次超出了边界，即x == 8,y == 9，就说明找到了答案
        if(x == 8 && y == 9)
            return true;
        //若当前不是可填充区域，就向右移动，若到达了右边界，就x+1，从下一行的开头开始填充
        if(board[x][y] != '.'){
            int new_x = x;
            int new_y = y+1;
            if(y == 8 && x != 8){
                new_x++;
                new_y = 0;
            }
            return dfs(board,new_x,new_y);
        }
				//当前是可填充区域
        for(int k = 1; k <= 9; ++k){
            //若当前数字目前来看是合法的
            if(!row_visited[x][k] && !col_visited[y][k] && !block_visited[x/3][y/3][k]){
                board[x][y] = (char)(k+48);
                row_visited[x][k] = true;
                col_visited[y][k] = true;
                block_visited[x/3][y/3][k] = true;
                //向右移动，若到达了右边界，就x+1，从下一行的开头开始填充
                int new_x = x;
                int new_y = y+1;
                if(y == 8 && x != 8){
                    new_x++;
                    new_y = 0;
                }
                //在后边的遍历过程中，发现当前数字不合法，回溯，修改数字
                if(!dfs(board,new_x,new_y)){
                    board[x][y] = '.';
                    row_visited[x][k] = false;
                    col_visited[y][k] = false;
                    block_visited[x/3][y/3][k] = false;
                }
            }
        }
        //填充当前的x,y区域，发现所有数字都无法填充进去，那么就要求回溯
        if(board[x][y] == '.')
            return false;
        
        return true;
    }
}
```











<span id="jump40"></span>

## 40.组合总和 II

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

说明：

* 所有数字（包括目标数）都是正整数。
* 解集不能包含重复的组合。 

示例 1:

```java
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```


示例 2:

```java
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```



### 回溯法

```java
class Solution {

    int target;
    List<List<Integer>> res;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        res = new ArrayList<>();
        if(candidates.length==0) return res;
        this.target = target;
        //先对数组排序，方便剪枝，遇到第一个大于target的元素，后面的元素就都不用判断了
        Arrays.sort(candidates);
        dfs(candidates,new ArrayList<>(),0,0);
        return res;
    }

    void dfs(int[] candidates, List<Integer> list, int sum, int cur){
        if(sum > target || cur > candidates.length){
            return;
        }
        if(sum == target){
            if(!res.contains(list))
                res.add(new ArrayList<>(list));
            return;
        }
        Set<Integer> set = new HashSet<>();
        for(int i = cur; i <candidates.length; ++i){
            //剪枝
            if(candidates[i] > target) return;
          	//重复的起点无需再次判断
            if(!set.contains(candidates[i])){
                set.add(candidates[i]);
                //遍历回溯
                list.add(candidates[i]);
                dfs(candidates, list, sum + candidates[i], i+1);
                list.remove(list.size()-1);
            }
        }
      
      	//也可以不用set,速度快了一些
      	//if (i > cur && candidates[i] == candidates[i - 1]) continue;
        ////遍历回溯
        //list.add(candidates[i]);
        //dfs(candidates, list, sum + candidates[i], i+1);
        //list.remove(list.size()-1);

    }
}
```









<span id="jump43"></span>

## 43.字符串相乘

给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

示例 1:

```java
输入: num1 = "2", num2 = "3"
输出: "6"
```


示例 2:

```java
输入: num1 = "123", num2 = "456"
输出: "56088"
```


说明：

```java
num1 和 num2 的长度小于110。
num1 和 num2 只包含数字 0-9。
num1 和 num2 均不以零开头，除非是数字 0 本身。
不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。
```



![leetcode_字符串相乘](/HYCBlog/assets/img/leetcode/leetcode_字符串相乘.png)

```java
class Solution {

    public String multiply(String num1, String num2) {
        if(num1.equals("0") || num2.equals("0")) return "0";

        int n = num1.length()-1,m = num2.length()-1;

        //存放结果
        String res = "";
        for(int i = m; i >= 0; --i){
            StringBuffer sb = new StringBuffer();
            //进位标志
            int add = 0;
            //十位补1个0，百位补2个0，以此类推，便于累加计算每位的乘积
            for(int j = m; j > i; --j){
                sb.append("0");
            }
            
            int y = num2.charAt(i) - '0';
            //遍历num1的每一位
            for(int j = n; j >= 0; --j){
                int x = num1.charAt(j) - '0';
                int product = x * y + add;
                sb.append(product % 10);
                //乘积大于9，进位
                add = product / 10;
            }
            //最后一次计算是否有进位未处理
            if (add != 0) {
                sb.append(add % 10);
            }
            res = addString(res,sb.reverse().toString());
            
        }

        return res;
    }
    String addString(String s1, String s2){
        int n = s1.length()-1, m = s2.length() -1, add = 0;
        StringBuffer res = new StringBuffer();
        while(n >= 0 || m >= 0 || add != 0){
            int x = n >= 0 ? s1.charAt(n) - '0' : 0;
            int y = m >= 0 ? s2.charAt(m) - '0' : 0;
            int sum = x + y + add;
            res.append(sum % 10);
            add = sum / 10;
            --n;
            --m;
        }
        return res.reverse().toString();
    }
}
```







<span id = "jump47"></span>

## 47.全排列 II

给定一个可包含重复数字的序列，返回所有不重复的全排列。

示例:

```java
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```



回溯法。往n个格子里填数字，每个数字只能填一次，且不能出现重复排列。

每个数字只能填一次。这个容易实现，用一个`visited[i]`数组标记数字`i`是否被访问过。

不能出现重复排列。例如在填第`i`个格子（从0开始计数）的时候，我们需要尝试填`n-i-1`个数字，这`n-i-1`个数字如果有重复的数字，那么我们就会重复地往第`i`个格子里填同一个数字，导致出现重复序列。所以，在填第`i`个格子的时候，用一个集合来存放已经填过的数字，保证下一次尝试不会填入重复的数字，即可去重。

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> permuteUnique(int[] nums) {
        res = new ArrayList<>();
        List<Integer> tmp = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];

        if(nums.length == 0){
            res.add(new ArrayList<>());
            return res;
        }
        backTrack(0,nums,tmp,visited);

        return res;
    }
		//往第cur个格子里填数字，从0开始
    void backTrack(int cur, int[] nums,List<Integer> tmp,boolean[] visited){
			//当所有格子都填满的时候，记录结果
      if(cur == nums.length){
            res.add(new ArrayList<>(tmp));
            return;
        }
      	//存放已经填过的数字
        Set<Integer> set = new HashSet<>();
      	//尝试每一种填法
        for (int i = 0; i < nums.length; ++i){
						//只有当前数字没有被访问，且没在集合中
            if(visited[i] || set.contains(nums[i]))
                continue;
            tmp.add(nums[i]);
            set.add(nums[i]);
            visited[i] = true;
            backTrack(cur+1,nums,tmp,visited);
          	//回溯
            visited[i] = false;
            tmp.remove(cur);
        }

    }
}
```





<span id = "jump50"></span>

## 50.Pow(x, n)

实现 pow(x, n) ，即计算 x 的 n 次幂函数。

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

快速幂解法：

```markdown
n = 1*b1 + 2*b2 + 4*b3 + ... + 2^m-1 * bm
x^n = x^(b1 + 2*b2 + 4*b3 + ... + 2^m-1 * bm)
= x^b1 * x^2b2 * x^4b3 * ... x^2^m-1*bm
分两步：
求x^1, x^2, x^4...x^2^m-1	-> x *= x即可
求b1,b2,b3,...,bm					-> b_i = n & 1; n >>= 1;
```



```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0.0f)   return 0.0d;
        long b = n;
        double res = 1.0;
        if(b < 0){
            x = 1 / x;
            b = -b;
        }
        double y = x;
        while(b > 0){
            if((b & 1) == 1)  res *= y;
            b >>= 1;
            y *= y;
        }
        return res;
    }
}
```













<span id = "jump51"></span>

## 51.N 皇后

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

示例：

```java
输入：4
输出：[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。

```


提示：

* 皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。



n*n的棋盘，放置n个皇后，且每个皇后不能处于同一条横行、纵行或斜线上，说明每一行都有一个皇后。所以我们只要遍历每一行，在合适的位置放置一个皇后即可。合适的位置判断如下：

```markdown
不能在同一列。用一个集合存放已访问的列。
不能在同一正对角线。同一个正对角线上，i-j的值是相等的，因此用一个集合放置已访问过的i-j的值。
不能在同一负对角线。同一个负对角线上，i+j的值是相等的，因此用一个集合放置已访问过的i+j的值。
```

若每个皇后都顺利放置完毕，就记录此时棋盘的状态。

```java
class Solution {
  List<List<String>> res;
  int n;
  int cnt;
  //标识某一列已经有皇后了
  Set<Integer> col;
  //标识某一正对角线已经有皇后了，这条线上，i-j的值是唯一的
  Set<Integer> pos_dia;
  //标识某一负对角线已经有皇后了，这条线上，i+j的值是唯一的
  Set<Integer> neg_dia;
  public List<List<String>> solveNQueens(int n) {
    res = new ArrayList<>();
    if(n == 0){
      res.add(new ArrayList<>());
      return res;
    }
    col = new HashSet<>();
    pos_dia = new HashSet<>();
    neg_dia = new HashSet<>();
    char[][] board = new char[n][n];
    cnt = n;
    this.n = n;
    for(int i = 0; i < n; ++i){
      for(int j = 0; j < n; ++j){
        board[i][j] = '.';
      }
    }
    //按行填充皇后，n个皇后一定分布在不同行，而且每行都会有一个皇后
    backTrack(board, 0);
    return res;
  }

  void backTrack(char[][] board,int row){
    //找到了一种答案
    if(cnt == 0){
      //将棋盘状态记录到列表中
      List<String> list = new ArrayList<>();
      for(int i = 0; i < n; ++i){
        StringBuffer sb = new StringBuffer();
        for(int j = 0; j < n; ++j){
          sb.append(board[i][j]);
        }
        list.add(sb.toString());
      }
      res.add(list);
    }
    //在列上填充
    for(int i = 0; i < n; ++i){
      if(!col.contains(i)){
        //若此位置所在的两条对角线没有被访问过
        if(!pos_dia.contains(i-row) && !neg_dia.contains(i+row)) {
          board[row][i] = 'Q';
          col.add(i);
          pos_dia.add(i - row);
          neg_dia.add(i + row);
          cnt--;
          backTrack(board, row + 1);
          //回溯
          cnt++;
          board[row][i] = '.';
          col.remove(i);
          pos_dia.remove(i - row);
          neg_dia.remove(i + row);
        }
      }
    }
  }

}
```

* 时间复杂度为O(n!)，n为皇后的数量。
* 空间复杂度为O(n)，n为皇后的数量。空间复杂度主要取决于递归调用层数、记录每行放置的皇后的列下标的数组以及三个集合，递归调用层数不会超过 N，数组的长度为 N，每个集合的元素个数都不会超过 N。



回顾这道题，拿到这道题的时候，其实我们很容易看出需要使用枚举的方法来求解这个问题，当我们不知道用什么办法来枚举是最优的时候，可以从下面三个方向考虑：

* 子集枚举：可以把问题转化成「从n^2^个格子中选一个子集，使得子集中恰好有 n 个格子，且任意选出两个都不在同行、同列或者同对角线」，这里枚举的规模是 2^n^
* 组合枚举：可以把问题转化成「从 n^2^个格子中选择 n 个，且任意选出两个都不在同行、同列或者同对角线」，这里的枚举规模是 n~n~^2^;
* 排列枚举：因为这里每行只能放置一个皇后，而所有行中皇后的列号正好构成一个 1 到 n 的排列，所以我们可以把问题转化为一个排列枚举，规模是 n!。

带入一些 nn 进这三种方法验证，就可以知道那种方法的枚举规模是最小的，这里我们发现第三种方法的枚举规模最小。这道题给出的两个方法其实和排列枚举的本质是类似的。







<span id = "jump52"></span>

## 52.N皇后 II[tag]

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

示例：

```java
输入：4
输出：[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。

```


提示：

* 皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。



n*n的棋盘，放置n个皇后，且每个皇后不能处于同一条横行、纵行或斜线上，说明每一行都有一个皇后。所以我们只要遍历每一行，在合适的位置放置一个皇后即可。合适的位置判断如下：

```markdown
不能在同一列。用一个集合存放已访问的列。
不能在同一正对角线。同一个正对角线上，i-j的值是相等的，因此用一个集合放置已访问过的i-j的值。
不能在同一负对角线。同一个负对角线上，i+j的值是相等的，因此用一个集合放置已访问过的i+j的值。
```

若每个皇后都顺利放置完毕，就计数+1。

```java
class Solution {
    int n;
    int cnt = 0;
    //不能在同一列
    Set<Integer> col = new HashSet<>();
    //不能在同一正对角线
    Set<Integer> pos_dia = new HashSet<>();
    //不能在同一负对角线
    Set<Integer> neg_dia = new HashSet<>();
    public int totalNQueens(int n) {
        this.n = n;
        backTrack(0);
        return cnt;
    }

    void backTrack(int row){
        //填充完毕，找到了一种答案
        if(row == n){
            cnt++;
        }

        //遍历这一行的每一列
        for(int j = 0; j < n; ++j){
            if(!col.contains(j) && !pos_dia.contains(row - j) && !neg_dia.contains(row + j)){
                col.add(j);
                pos_dia.add(row - j);
                neg_dia.add(row + j);

                backTrack(row + 1);
                //回溯
                col.remove(j);
                pos_dia.remove(row - j);
                neg_dia.remove(row + j);
            }
        }
    }
}
```

位运算解法：

```java
class Solution {
    public int totalNQueens(int n) {
        return solve(n, 0, 0, 0, 0);
    }

    public int solve(int n, int row, int columns, int diagonals1, int diagonals2) {
        if (row == n) {
            return 1;
        } else {
            int count = 0;
            int availablePositions = ((1 << n) - 1) & (~(columns | diagonals1 | diagonals2));
            while (availablePositions != 0) {
                int position = availablePositions & (-availablePositions);
                availablePositions = availablePositions & (availablePositions - 1);
                count += solve(n, row + 1, columns | position, (diagonals1 | position) << 1, (diagonals2 | position) >> 1);
            }
            return count;
        }
    }
}
```







<span id = "jump57"></span>

## 57.插入区间

给出一个无重叠的 ，按照区间起始端点排序的区间列表。

在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

 

示例 1：

```java
输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]
```


示例 2：

```java
输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```



无非是边界判断和区间合并，繁琐了点而已，没啥难度。

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> res = new LinkedList<>();
        int i = 0;
        //让newInterval的起点和每个intervals的终点比较，搜索插入的起点。
        for(; i < intervals.length; ++i){
            //所有终点小于newInterval起点的区间都加入结果集
            if(newInterval[0] > intervals[i][1]){
                res.add(intervals[i]);
            //出现了终点大于等于newInterval起点的区间
            }else{
                //如果newInterval的终点小于intervals的起点，就没有往后搜索的意义了
                //newInterval直接插入在最前面

                //否则就继续搜索
                if(newInterval[1] > intervals[i][0]){
                    //如果newInterval的起点要大于搜索到的位置的起点，就更新newInterval的起点
                    if(newInterval[0] > intervals[i][0]){
                        newInterval[0] = intervals[i][0];
                    }
                    //如果newInterval的终点要小于搜索到的位置的终点，就更新newInterval的终点
                    //将这个位置区间包裹在内
                    if(newInterval[1] < intervals[i][1]){
                        newInterval[1] = intervals[i][1];
                    }
                }
                break;
            }
        }
        //让newInterval的终点和每个intevals的起点比较，搜索插入的终点。
        for(; i <intervals.length; ++i){
            //如果newInterval的终点某个区间的起点重合
            if(newInterval[1] == intervals[i][0]){
                //直接将这个区间包裹进来
                newInterval[1] = intervals[i][1];
                i++;
                break;
            }
            //如果newInterval的终点大于某个区间的起点，并且终点小于这个区间的终点
            if(newInterval[1] > intervals[i][0] && newInterval[1] < intervals[i][1]){
                //包裹这个区间
                newInterval[1] = intervals[i][1];
                i++;
                break;
            }
            //如果newInterval的终点小于某个区间的起点，那么就和前一个区间比较
            if(newInterval[1] < intervals[i][0]){
                //前一个区间的起点肯定是大于newInterval的终点的，如果newInterval的终点小于这个区间，就更新
                //把这个区间包裹进来
                if(i - 1 >= 0 && intervals[i-1][1] > newInterval[1]){
                    newInterval[1] = intervals[i-1][1];
                }
                break;
            }
        }
        res.add(newInterval);
        //将剩余的区间加入结果集
        for(; i < intervals.length; ++i){
            res.add(intervals[i]);
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```
















<span id = "jump60"></span>

## 60. 第k个排列[tag]

给出集合 [1,2,3,…,n]，其所有元素共有 n! 种排列。

按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：

```java
"123"
"132"
"213"
"231"
"312"
"321"
```


给定 n 和 k，返回第 k 个排列。

说明：

* 给定 n 的范围是 [1, 9]。
* 给定 k 的范围是[1,  n!]。

示例 1:

```java
输入: n = 3, k = 3
输出: "213"
```


示例 2:

```java
输入: n = 4, k = 9
输出: "2314"
```

容易得知`[1,n]`中的每个元素为开头的序列有`n-1!`个。



```markdown
假设n = 4, k = 9，先排列第一个元素。
1开头的排列有3! = 6种，
2开头的排列有3! = 6种
6< 9 < 12，所以第一个元素为2。
所以可以得到映射关系为 x = 向下取整[(k-1) / (n-1)!] + 1。则所求元素为[1,n]中第x小的未使用元素
接下来需要更新k
因为第k个排列落在以元素2为开头的序列集合中，所以需要找到第k个排列在这个序列集合中的位置。
故 k = ((k-1) % (n-1)!) + 1;

至此,问题已经转换为 n = 3, k = 3，可排列集合为{1,3,4}的情况下，找到第k个排列。
以此类推，当n == 0时，代表排列完成。

现在没搞懂的是，为什么要用k-1，然后向下取整+1呢？直接向上取整反而不能通过，改天好好研究一下。打个tag吧

知道啦！
不妨设分子为 k，那么得到的公式可能是这样的：
    ai =  ⌊k / (n-1)!⌋ + 1

尝试使用以上公式计算 a1:
    （1）当 k < (n-1)! 时，a1 = ⌊k / (n-1)!⌋ + 1 = 1，正确
    （2）当 k = (n-1)! 时，a1 = ⌊k / (n-1)!⌋ + 1 = 2，错误

而使用 ai =  ⌊(k-1) / (n-1)!⌋ + 1 却能正确处理这种情况
```



```java
class Solution {
  public String getPermutation(int n, int k) {
    List<Integer> list = new ArrayList<>();
    int m = n;
    while(n > 0){
      //第k次排列的开头元素为第x小的未使用元素
      int x = (int)Math.floor((k-1)*1.0 / levelMul(n-1)) + 1;

      int index = 0;
      //找到第x小的未使用元素index
      for(int i = 1; i <= m; ++i){
        if (!list.contains(i)){
          x--;
        }
        if(x <= 0){
          index = i;
          break;
        }
      }

      list.add(index);
      //缩小规模
      k = ((k-1) % levelMul(n-1)) + 1;
      n--;
    }
    StringBuffer res = new StringBuffer();
    for(Integer i : list){
      res.append(i);
    }
    return res.toString();
  }

  int levelMul(int n){
    if(n > 0){
      return n * levelMul(n-1);
    }
    return 1;
  }
}
```

拓展：

[康托展开和逆康托展开](https://blog.csdn.net/wbin233/article/details/72998375)
















<span id="jump63"></span>

## 63.含障碍物网格中的不同路径

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

**说明：** *m*和 *n* 的值均不超过 100。

示例：

```markdown
输入:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
输出: 2
解释:
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```



### 动态规划

设`c[i][j]`代表从`(0,0)`网格走到`(i,j)`网格有几种走法，其状态方程为：

```markdown
c[i][j] = 0 											if obstacleGrid[i][j] == 1,
c[i][j] = c[i-1][j] + c[i][j-1] 	if obstacleGrid[i][j] == 0
```



```java
class Solution {

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] c = new int[m][n];
      //边界判断
        c[0][0] = obstacleGrid[0][0] == 1 ? 0 : 1;
        for(int i = 1; i < n; ++i){
            c[0][i] = obstacleGrid[0][i] == 1 ? 0 : c[0][i-1];
        }
        for(int i = 1; i < m; ++i){
            c[i][0] = obstacleGrid[i][0] == 1 ? 0 : c[i-1][0];
        }
			//状态转移
        for(int i = 1; i < m; ++i)
            for(int j = 1; j < n; ++j){
                c[i][j] = obstacleGrid[i][j] == 1 ? 0 : c[i-1][j] + c[i][j-1];
            }

        return c[m-1][n-1];
    }

    public static void main(String[] args) {
        int[][] m = new int[][]{
                {0,1}
        };
        int res = new Solution().uniquePathsWithObstacles(m);
        System.out.println(res);
    }
}
```

<span id="jump67"></span>

## 67.二进制求和

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`

示例 1:

```markdown
输入: a = "11", b = "1"
输出: "100"
```


示例 2:

```markdown
输入: a = "1010", b = "1011"
输出: "10101"
```

提示：

```markdown
每个字符串仅由字符 '0' 或 '1' 组成。
1 <= a.length, b.length <= 10^4
字符串如果不是 "0" ，就都不含前导零。
```



```java
public class Solution {
    public String addBinary(String a, String b) {
        //如果a的长度小于b，则交换a和b，保证a的长度总是最长的
        if (b.length() > a.length()) {
            String tmp = a;
            a = b;
            b = tmp;
        }
        //填充b
        String pz = "";
        for (int i = 0; i < a.length() - b.length(); ++i) {
            pz += "0";
        }
        b = pz + b;
        //进位标志
        int c = 0;
        String res = "";
        for (int i = a.length()-1; i >= 0; --i) {
            int ca = (int) a.charAt(i) - 48;
            int cb = (int) b.charAt(i) - 48;

            switch (ca + cb + c){
                case 0:
                    res = "0" + res;
                    break;
                case 1:
                    res = "1" + res;
                    c = 0;
                    break;
                case 2:
                    res = "0" + res;
                    c = 1;
                    break;
                case 3:
                    res = "1" + res;
                    c = 1;
                    break;
            }
        }
        if(c == 1)
            res = "1"+res;
        //System.out.println(b);
        return res;
    }
    public static void main(String[] args){
        String a = "1010";
        String b = "1011";
        String res = new Solution().addBinary(a,b);
        System.out.println(res);
    }

}
```



```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuffer ans = new StringBuffer();

        int n = Math.max(a.length(), b.length()), carry = 0;
        for (int i = 0; i < n; ++i) {
            carry += i < a.length() ? (a.charAt(a.length() - 1 - i) - '0') : 0;
            carry += i < b.length() ? (b.charAt(b.length() - 1 - i) - '0') : 0;
            ans.append((char) (carry % 2 + '0'));
            carry /= 2;
        }

        if (carry > 0) {
            ans.append('1');
        }
        ans.reverse();

        return ans.toString();
    }
}
```













<span id = "jump77"></span>

## 77.组合[tag:字典序]

给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

示例:

```java
输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```



### 回溯法

先固定第i位，然后将i+1~n位的数都与其交换位置，这里不是数组，所以直接+1。

```java
class Solution {
    List<List<Integer>> res;
    int[] x;
    int n;
    int k;
    int cnt;
    public List<List<Integer>> combine(int n, int k) {
        res = new ArrayList<>();
        if(k > n || n <= 0  || k < 0){
            return res;
        }
        this.n = n;
        this.k = k;
        x = new int[k];
        int cnt = 0;
        if(k == 0){
            List<Integer> list = new ArrayList<>();
            res.add(list);
            return res;
        }
        backTrack(1);
        return res;
    }

    //先固定第i位，然后将i+1~n位的数都与其交换位置，这里不是数组，所以直接+1
    void backTrack(int cur){
        if(cnt == k){
            List<Integer> tmp = new ArrayList<>();
            for(int i = 0; i < cnt; ++i){
                tmp.add(x[i]);
            }
            res.add(tmp);
            return;
        }
        for(int i = cur; i<=n; ++i){
            x[cnt++] = i;
            backTrack(i+1);
            x[--cnt] = 0;
        }
    }
}
```

在记录结果的时候，以下代码为什么会更慢呢？

```java
List<Integer> tmp = new ArrayList<>();
Collections.addAll(tmp,x);
res.add(tmp);
```

按理来说，库函数的执行效率会比我自己实现的逐个添加会高一些的。但是这里却慢了许多。

[字典序解法](https://leetcode-cn.com/problems/combinations/solution/zu-he-by-leetcode-solution/)

字典序出现好多次了，还是没搞懂，下回得好好弄弄。























<span id="jump93"></span>

## 93. 恢复IP地址

给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址正好由四个整数（每个整数位于 0 到 255 之间组成），整数之间用 '.' 分隔。

示例:

```java
输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]
```



![leetcode_恢复Ip地址](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/leetcode_恢复Ip地址.png)

```java
package leetcode;
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("all")
class RestoreIpAddresses {
    private int SEG_COUNT = 4;
    private List<String> res = new ArrayList<String>();
    private int[] segment = new int[SEG_COUNT];

    public List<String> solution(String s) {
        dfs(s,0,0);
        return res;
    }
    private void dfs(String s, int segId, int segStart){
        //找到了第四段地址
        if(segId == SEG_COUNT){
            //已经遍历完字符串
            if(segStart == s.length()){
                //确定一种答案
                StringBuffer sb = new StringBuffer();
                for(int i = 0; i < SEG_COUNT; ++i){
                    sb.append(segment[i]);
                    if(i != SEG_COUNT-1)
                        sb.append(".");
                }
                res.add(sb.toString());
            }
            return;
        }
        //如果未找到第四段地址，字符串就已经遍历完，则回溯
        if(segStart == s.length())
            return;

        //如果有前导0，那么这个位置的segment只能为0
        if(s.charAt(segStart)=='0'){
            segment[segId] = 0;
            dfs(s,segId+1,segStart+1);
        }
        //一般情况
        int addr = 0;
        for(int segEnd = segStart; segEnd < s.length(); ++segEnd){
            addr = addr * 10 + (s.charAt(segEnd) - '0');
            if(addr>0 && addr <= 0xFF){
                segment[segId] = addr;
                dfs(s, segId + 1, segEnd + 1);
            }else{
                break;
            }
        }
    }
    public static void main(String[] args) {
        String s = "25525511135";
        List<String> res = new RestoreIpAddresses().solution(s);
        System.out.println(res.toString());
    }
}
```



* 如果需要字符串拼接，定义StringBuffer会好很多。
* 但需要递归函数返回值时，最好不要直接用函数返回，定义一个全局变量来直接记录会好很多。
* 思考递归时，不要把问题划分的太细，要一次递归能够处理一个子问题。递归树的每条路径对应一种解法。像这题，我一开始想着从最后一个地址段开始，遍历所有情况，然后用返回值把字符串拼接起来，这种思维非常混乱。
* 写递归函数，先写满足要求的情况，即递归结束条件；再写特殊情况，最后写一般情况。

















<span id="jump100"></span>

## 100. 相同的树



给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

示例 1:

    输入:       1         1
              / \       / \
             2   3     2   3
    				[1,2,3],   [1,2,3]
    输出: true


示例 2:

    输入:      1          1
              /           \
             2             2
        [1,2],     [1,null,2]
    输出: false


示例 3:

    输入:       1         1
              / \       / \
              2   1     1   2
       		 [1,2,1],   [1,1,2]
    输出: false

### 深度优先

```java
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null && q == null) return true;
        if(p == null || q == null) return false;

        if(p.val != q.val)
            return false;
        else
            return isSameTree(p.left,q.left)&&isSameTree(p.right,q.right);
    }
}
```



### 广度优先

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        } else if (p == null || q == null) {
            return false;
        }
        Queue<TreeNode> queue1 = new LinkedList<TreeNode>();
        Queue<TreeNode> queue2 = new LinkedList<TreeNode>();
        queue1.offer(p);
        queue2.offer(q);
        while (!queue1.isEmpty() && !queue2.isEmpty()) {
            TreeNode node1 = queue1.poll();
            TreeNode node2 = queue2.poll();
            if (node1.val != node2.val) {
                return false;
            }
            TreeNode left1 = node1.left, right1 = node1.right, left2 = node2.left, right2 = node2.right;
            if (left1 == null ^ left2 == null) {
                return false;
            }
            if (right1 == null ^ right2 == null) {
                return false;
            }
            if (left1 != null) {
                queue1.offer(left1);
            }
            if (right1 != null) {
                queue1.offer(right1);
            }
            if (left2 != null) {
                queue2.offer(left2);
            }
            if (right2 != null) {
                queue2.offer(right2);
            }
        }
        return queue1.isEmpty() && queue2.isEmpty();
    }
}
```



















<span id = "jump103"></span>

## 103. 二叉树的锯齿形层序遍历

给定一个二叉树，返回其节点值的锯齿形层序遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 [3,9,20,null,null,15,7],

    		3
       / \
      9  20
        /  \
       15   7


返回锯齿形层序遍历如下：

```java
[
  [3],
  [20,9],
  [15,7]
]
```





```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null)    return res;
        //程序遍历，增加一个布尔变量用于判断当前遍历的是奇数层还是偶数层
        //如果是奇数层，那么正序遍历，如果是偶数层，则逆序遍历
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        boolean even = false;
        while(!queue.isEmpty()){
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            for(int i = 0; i < size; ++i){
                TreeNode tmp = queue.poll();
                if(!even){
                    //正序添加
                    list.add(tmp.val);
                }else{
                    //逆序添加
                    list.add(0, tmp.val);
                }
                if(tmp.left != null)    queue.offer(tmp.left);
                if(tmp.right != null)   queue.offer(tmp.right);
            }
            res.add(list);
            //奇偶替换
            even = !even;
        }
        return res;
    }
}
```











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







<span id ="jump127"></span>

## 127.单词接龙

给定两个单词（beginWord 和 endWord）和一个字典，找到从 beginWord 到 endWord 的最短转换序列的长度。转换需遵循如下规则：

每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。
说明:

* 如果不存在这样的转换序列，返回 0。
* 所有单词具有相同的长度。
* 所有单词只由小写字母组成。
* 字典中不存在重复的单词。
* 你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

示例 1:

```java
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
```


示例 2:

```java
输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。
```



深度优先遍历超时了。

广度优先遍历：

1. 用一个集合存放字典元素，便于判断某个单词是否在字典内。
2. 广度优先：startWord开始，尝试修改每一位字符，将修改后，存在于字典中的新单词全部放入队列。
3. 每一次广度优先尝试step+1。
4. 若在一次广度优先尝试中，有一个单词修改一个字母后，变成了endWord，就返回step+1。



```java
class Solution {

    public int ladderLength(String beginWord, String endWord, List<String> wordList) {

        if(wordList.size() == 0 || !wordList.contains(endWord)) return 0;

        //将wordList放入集合中，便于查询某个单词是否在字典中
        Set<String> wordSet = new HashSet<>();
        for(String word : wordList){
            wordSet.add(word);
        }

        wordSet.remove(beginWord);

        //队列，用于广度优先遍历
        Queue<String> queue = new LinkedList<>();
        queue.offer(beginWord);
        //标记某个单词是否被访问过
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        int step = 1;
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; ++i){
                String w = queue.poll();
                if(modifyEachLetter(w, endWord, queue, visited, wordSet)){
                    return step+1;
                }
            }
            step++;
        }
        return 0;
    }

    boolean modifyEachLetter(String w, String endWord, Queue<String> queue, Set<String> visited, Set<String> wordSet){

        char[] wordChar = w.toCharArray();
        //修改每个位置的字符
        for(int i = 0; i < w.length(); ++i){
            char orignChar = wordChar[i];
            //每个位置遍历26种情况
            for(char c = 'a'; c <= 'z'; ++c){
                if(orignChar == c)    continue;
                wordChar[i] = c;
                String new_w = new String(wordChar);
                if(!wordSet.contains(new_w))    continue;
                //字典内存在这个新单词
                if(new_w.equals(endWord)){
                    return true;
                }else{
                    if(!visited.contains(new_w)){
                        queue.offer(new_w);
                        visited.add(new_w);
                    }
                }
            }
            wordChar[i] = orignChar;
            
        }
        return false;
        

    }

}
```









<span id = "jump129"></span>

## 129.求根到叶子节点数字之和

给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径 1->2->3 代表数字 123。

计算从根到叶子节点生成的所有数字之和。

说明: 叶子节点是指没有子节点的节点。

示例 1:

```java
输入: [1,2,3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
```

示例 2:

```java
输入: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
输出: 1026
解释:
从根到叶子节点路径 4->9->5 代表数字 495.
从根到叶子节点路径 4->9->1 代表数字 491.
从根到叶子节点路径 4->0 代表数字 40.
因此，数字总和 = 495 + 491 + 40 = 1026.
```





前序遍历+回溯

```java
class Solution {
    int sum = 0;
    public int sumNumbers(TreeNode root) {
        //前序遍历加回溯
        preOderBackTrack(root, 0);
        return sum;
    }

    void preOderBackTrack(TreeNode root, int base){
        if(root != null){
            base = base * 10 + root.val;
            preOderBackTrack(root.left, base);
            preOderBackTrack(root.right, base);
            //如果到了叶节点
            if(root.left == null && root.right == null)
                sum += base;
        }
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











<span id = "jump135"></span>

## 135. 分发糖果

老师想给孩子们分发糖果，有 N 个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。

你需要按照以下要求，帮助老师给这些孩子分发糖果：

每个孩子至少分配到 1 个糖果。
相邻的孩子中，评分高的孩子必须获得更多的糖果。
那么这样下来，老师至少需要准备多少颗糖果呢？

示例 1:

```java
输入: [1,0,2]
输出: 5
解释: 你可以分别给这三个孩子分发 2、1、2 颗糖果。
```

示例 2:

```java
输入: [1,2,2]
输出: 4
解释: 你可以分别给这三个孩子分发 1、2、1 颗糖果。
     第三个孩子只得到 1 颗糖果，这已满足上述两个条件。
```



```java
class Solution {
    public int candy(int[] ratings) {
        int res = 0;
        //统计连续下降的子序列长度，按照这个长度分配糖果
        //假设有这么一个序列「44，13，3，2，3」，连续下降长度为4，所以分配糖果数为4+3+2+1+...
        int n = ratings.length;
        if(n == 0)  return 0;
        int[] counts = new int[ratings.length];

        int l = 0, r = -1;
        while(l < n && r < n){
            ++r;
            while(r < n-1 && ratings[r] > ratings[r+1]){
                ++r;
            }
            //说明找到了一个长度大于等于2的下降子序列
            if(r > l){
                if(l == 0 || (l > 0 && ratings[l] == ratings[l - 1])){
                    for(int i = l, j = r - l + 1; i <= r; ++i){
                        counts[i] = j--;
                    }
                }else{//需要保证这个子序列的首元素分配的糖果数不小于上一个
                    if(ratings[l] > ratings[l-1]){
                        counts[l] = Math.max(counts[l - 1] + 1, r - l + 1);
                        for(int i = l + 1, j = r - l; i <= r; ++i){
                            counts[i] = j--;
                        }
                    }
                }

            }else{//r == l
                //与上一个元素相等，且下一个元素等于自己
                if(r - 1 >= 0 && ratings[r] == ratings[r - 1]){
                    //给1个糖果就行了
                    counts[r] = 1;
                }else if(r == 0){
                    counts[r] = 1;
                }else if(ratings[r] > ratings[r - 1]){//比上一个元素大，那就必须给多一个糖果
                    counts[r] = counts[r-1] + 1;
                }
            }
            l = r + 1;

        }

        for(int i = 0; i <counts.length; ++i){
            res += counts[i];
        }
        return res;
    }


}
```



![image-20201224094150566](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20201224094150566.png)

```java
class Solution {
    public int candy(int[] ratings) {
        int[] left = new int[ratings.length];
        int[] right = new int[ratings.length];
        Arrays.fill(left, 1);
        Arrays.fill(right, 1);
        for(int i = 1; i < ratings.length; i++)
            if(ratings[i] > ratings[i - 1]) left[i] = left[i - 1] + 1;
        int count = left[ratings.length - 1];
        for(int i = ratings.length - 2; i >= 0; i--) {
            if(ratings[i] > ratings[i + 1]) right[i] = right[i + 1] + 1;
            count += Math.max(left[i], right[i]);
        }
        return count;
    }
}
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









<span id = "jump114"></span>

## 144.二叉树的前序遍历

给定一个二叉树，返回它的 前序 遍历。

 示例:

```java
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```


进阶: 递归算法很简单，你可以通过迭代算法完成吗？

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if(root == null)    return res;
        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode node = root;
        while(!stack.isEmpty() || node != null){
            //访问当前节点以及左子树节点，并入栈
            while(node != null){
                res.add(node.val);
                stack.push(node);
                node = node.left;
            }
            //到叶节点了，出栈，转向右子树
            node = stack.pop();
            node = node.right;
        }
        return res;

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









<span id = "jump147"></span>

## 147.对链表进行插入排序




插入排序算法：

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。

示例 1：

```java
输入: 4->2->1->3
输出: 1->2->3->4
```

示例 2：

```java
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```



```java
class Solution {

    public ListNode insertionSortList(ListNode head) {
        if(head == null)    return null;
        //新链表
        ListNode fakeHead = new ListNode(0);

        fakeHead.next = head;
        ListNode cur = head;

        ListNode lastSort = head;

        while(cur.next != null){
            //剪枝，指向当前已排序链表的最后一个元素，若cur.next比这个元素都大，那就不用插入了
            if(cur.next.val >= lastSort.val){
                lastSort = lastSort.next;
                cur = cur.next;
                continue;
            }

            ListNode ptr = fakeHead;
            while(ptr.next != null && ptr.next.val < cur.next.val){
                ptr = ptr.next;
            }

            ListNode tmp = cur.next;
            cur.next = tmp.next;
            tmp.next = ptr.next;
            ptr.next = tmp;

        }

        return fakeHead.next;
    }

    
}
```







<span id = "jump152"></span>

## 152.乘积最大子数组

给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

 

示例 1:

```java
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

示例 2:

```java
输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```



动态规划：

考虑`max[i]`为以第i个元素为结尾的最大子数组乘积，转移方程：

```markdown
max[i] = Math.max(max[i-1] * nums[i], nums[i]);
```

这样考虑有一个问题，就是如果数字为：3,-4,5,-2，我们计算max[3]的时候，会得出5这样的结果，实际上应该是前面所有数字的乘积，这是因为我们没有考虑负数的情况。

所以为了将负数纳入考虑范围，我们再维护一个`min[i]`,表示以第i个元素为结尾的最小子数组乘积，这个`min[i]`，我们期望获得最小的乘积子数组，因此负数会被考虑在内。

所以当`nums[i]`为正数时，转移方程为：

```java
max[i] = Math.max(max[i-1] * nums[i], nums[i]);
```

当`nums[i]`为负数时，转移方程为：

```java
max[i] = Math.max(min[i-1] * nums[i], nums[i]);
```

合起来就是：

```java
max[i] = max{max[i-1] * nums[i], min[i-1]*nums[i], nums[i]}
```

同时`min[i]`也需要更新：

```java
min[i] = min{max[i-1] * nums[i], min[i-1]*nums[i], nums[i]}
```



```java
class Solution {
    public int maxProduct(int[] nums) {
        if(nums.length == 0) return 0;
        int n = nums.length;
        int[] max = new int[n];
        int[] min = new int[n];

        System.arraycopy(nums, 0, max, 0, n);
        System.arraycopy(nums, 0, min, 0, n);

        for(int i = 1; i < n; ++i){
            max[i] = Math.max(max[i-1] * nums[i], Math.max(nums[i], min[i-1] * nums[i]));
            min[i] = Math.min(min[i-1] * nums[i], Math.min(nums[i], max[i-1] * nums[i]));
        }

        int res = Integer.MIN_VALUE;

        for( int i : max){
            res = Math.max(res, i);
        }

        return res;
    
    }
}
```



空间优化：

```java
class Solution {
    public int maxProduct(int[] nums) {
      
        if(nums.length == 0) return 0;
      
        int n = nums.length;
        int max = nums[0];
        int min = nums[0];
        int res = nums[0];
      
        for(int i = 1; i < n; ++i){
            int mx = max, mn = min;
            max = Math.max(mx * nums[i], Math.max(nums[i], mn * nums[i]));
            min = Math.min(mn * nums[i], Math.min(nums[i], mx * nums[i]));
            res = Math.max(res, max);
        }    
        return res;
    }
}
```









<span id = "jump164"></span>

## 164.最大间距

给定一个无序的数组，找出数组在排序之后，相邻元素之间最大的差值。

如果数组元素个数小于 2，则返回 0。

示例 1:

```java
输入: [3,6,9,1]
输出: 3
解释: 排序后的数组是 [1,3,6,9], 其中相邻元素 (3,6) 和 (6,9) 之间都存在最大差值 3。
```

示例 2:

```java
输入: [10]
输出: 0
解释: 数组元素个数小于 2，因此返回 0。
```


说明:

* 你可以假设数组中所有元素都是非负整数，且数值在 32 位有符号整数范围内。
* 请尝试在线性时间复杂度和空间复杂度的条件下解决此问题。

```java
class Solution {
    public int maximumGap(int[] nums) {

        if(nums.length < 2) return 0;
        int n = nums.length;
        //基数排序
        int maxVal = Arrays.stream(nums).max().getAsInt();
        long exp = 1;

        int[] buf = new int[n];

        while(maxVal >= exp){
            int[] cnt = new int[10];
            //统计这一位上各个数字出现的次数
            for(int i = 0; i < n; ++i){
                int digit = (nums[i] / (int)exp) % 10;
                cnt[digit]++;
            }
            //让cnt内的值有序
            for(int i = 1; i < 10; ++i){
                cnt[i] += cnt[i-1];
            }
            //倒着填回去
            //如果不是倒着填
            for(int i = n-1; i >= 0; --i){
                int digit = (nums[i] / (int)exp) % 10;
                buf[cnt[digit] - 1] = nums[i];
                cnt[digit]--;
            }

            System.arraycopy(buf, 0, nums, 0, n);
            exp *= 10;
        }

        int max = 0;
        for(int i = 0; i < n-1; ++i){
            max = Math.max(nums[i+1] - nums[i], max);
        }
        return max;
    }
}
```

因此，我们可以选取整数 
$$
d = \lfloor (\textit{max}-\textit{min}) / (N-1) \rfloor < \lceil (\textit{max}-\textit{min}) / (N-1)
$$
。随后，我们将整个区间划分为若干个大小为 dd 的桶，并找出每个整数所在的桶。根据前面的结论，能够知道，元素之间的最大间距一定不会出现在某个桶的内部，而一定会出现在不同桶当中。

因此，在找出每个元素所在的桶之后，我们可以维护每个桶内元素的最大值与最小值。随后，只需从前到后不断比较相邻的桶，用后一个桶的最小值与前一个桶的最大值之差作为两个桶的间距，最终就能得到所求的答案。



<span id = "jump189"></span>

## 189. 旋转数组

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

示例 1:

```java
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

示例 2:

```java
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```


说明:

* 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
* 要求使用空间复杂度为 O(1) 的 原地 算法。



```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k %= n;

        //1,2,3,4,5,6,7
        reverse(nums, 0, n-1);//7,6,5,4,3,2,1
        reverse(nums, 0, k-1);//5,6,7,4,3,2,1
        reverse(nums, k, n-1);//5,6,7,1,2,3,4

    }

    void reverse(int[] nums, int l, int h){
        if(l >= h)  return;
        while(l < h){
            int tmp = nums[l];
            nums[l] = nums[h];
            nums[h] = tmp;
            l++;
            h--;
        }
    }
}
```







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







<span id = "jump204"></span>

## 204.计数质数

统计所有小于非负整数 n 的质数的数量。

 

示例 1：

```java
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

示例 2：

```java
输入：n = 0
输出：0
```

示例 3：

```java
输入：n = 1
输出：0
```


提示：

* 0 <= n <= 5 * 106

```java
class Solution {
    //要判断数字x是不是质数，需要判断[2,x-1]中的每个数是否都不是x的因数
    //考虑到如果y是x的因数，那么x/y肯定也是x的因数是，所以只需要判断y或者x/y是不是x的因数即可
    //而y和x/y的较小值肯定落在[2,√x]中，所以只要枚举[2,√x]的值即可
    public int countPrimes(int n) {
        int res = 0;

        for(int i = 2; i < n; ++i){
            res += isPrime(i) ? 1 : 0;
        }

        return res;
    }

    public boolean isPrime(int x){
        for(int i = 2; i * i <= x; ++i){
            if(x % i == 0){
                return false;
            }
        }
        return true;
    }
}
```

```java
class Solution {
    //埃式筛：由希腊数学家厄拉多塞（Eratosthenes）提出
    //如果x是质数，那么2x,3x,...等肯定不是质数
    //所以只要用一个数组，记录下标对应的数字是不是质数即可
    //遇到一个质数，就将其所有的倍数下标标记为合数
    //优化：应该从x*x开始标记，因为2x,3x,...这些数一定在x之前就被其他质数的倍数标记了，例如2的所有倍数
    public int countPrimes(int n) {
        int[] isPrime = new int[n];

        Arrays.fill(isPrime,1);
        int res = 0;
        for(int i = 2; i < n; ++i){
            if(isPrime[i] == 1){
                res += 1;

                if((long) i * i < n){
                    for(int j = i * i; j < n; j += i){
                        isPrime[j] = 0;
                    }
                }
            }
        }

        return res;
    }
}
```









<span id = "jump206"></span>

## 206.反转链表

反转一个单链表。

示例:

```java
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

进阶:

* 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

迭代：

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode p = head;
        while(p != null){
            ListNode q = p.next;
            p.next = pre;
            pre = p;
            p = q;
        }
        return pre;
    }
}
```

递归:

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        
        return reverse(null, head);
    }

    ListNode reverse(ListNode pre, ListNode p){
        if(p == null){
            return pre;
        }
        ListNode q = p.next;
        p.next = pre;
        pre = p;
        p = q;
        return reverse(pre, p);
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





<span id = "jump221"></span>

## 221.最大正方形

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

示例:

```java
输入: 
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```



动态规划，`dp[i][j]`表示以`(i,j)`为右下角的正方形的最大边长。

```java
class Solution {
    public int maximalSquare(char[][] matrix) {

        if(matrix.length == 0 || matrix[0].length == 0)    return 0;
        //dp[i][j]表示以(i,j)为右下角的正方形的最大边长
        //dp[i][j]的值由其左方，上方，左上方的dp值得到。

        int[][] dp = new int[matrix.length][matrix[0].length];
        int max = 0;
        //边界初始化
        for(int i = 0; i < matrix.length; ++i){
            dp[i][0] = matrix[i][0] == '1' ? 1 : 0;
            max = Math.max(dp[i][0], max);
        }
        for(int j = 0; j < matrix[0].length; ++j){
            dp[0][j] = matrix[0][j] == '1' ? 1 : 0;
            max = Math.max(dp[0][j], max);
        }
        for(int i = 1; i < matrix.length; ++i){
            for(int j = 1; j < matrix[0].length; ++j){
                if(matrix[i][j] == '0')
                    dp[i][j] = 0;
                else
                    dp[i][j] = Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1]) + 1;
                max = Math.max(dp[i][j], max);
            }
        }
        return max * max;
        
    }
}
```







<span id = "jump222"></span>

## 222.完全二叉树的节点个数

给出一个完全二叉树，求出该树的节点个数。

说明：

完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

示例:

```java
输入: 
    1
   / \
  2   3
 / \  /
4  5 6

输出: 6
```



层序遍历：

```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null)    return 0;
        if(root.left == null)   return 1;
        //首先求出二叉树的深度，再遍历倒数第二层的节点，统计最后一层的节点个数
        //由于是完全二叉树，所以只要持续探索左节点，就能得到深度
        TreeNode cur = root;
        int depth = 0;
        while(cur != null){
            depth++;
            cur = cur.left;
        }
        //层序遍历，直到到达第depth-1层
        int cnt = depth;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty() && cnt > 2){
            cnt--;
            int size = queue.size();
            for(int i = 0; i < size; ++i){
                TreeNode tmp = queue.poll();
                if(tmp.left != null)    queue.offer(tmp.left);
                if(tmp.right != null)    queue.offer(tmp.right);
            }
        }
        //此时queue中的节点都是倒数第二层的节点
        int res = (int)Math.pow(2,depth-1) - 1;
        int size = queue.size();
        for(int i = 0; i < size; ++i){
            TreeNode tmp = queue.poll();
            if(tmp.left != null && tmp.right != null){
                res+=2;
            }else if(tmp.left != null && tmp.right == null){
                res+=1;
                break;
            }else{
                break;
            }
        }
        return res;
    }
}
```



二分+位运算

```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null)    return 0;
        if(root.left == null)   return 1;
        //首先求出二叉树的深度
        //由于是完全二叉树，所以只要持续探索左节点，就能得到深度
        TreeNode cur = root;
        int depth = 0;
        while(cur.left != null){
            depth++;
            cur = cur.left;
        }
        //知道了深度，就知道了此二叉树的节点数范围：2^depth~2^(depth+1)-1
        //可以利用二分搜索确定节点数
        // 1 00000 ～ 1 11111
        int low = 1 << depth, high = (1 << (depth + 1)) - 1;
        while(low < high){
            int mid = (high - low + 1) / 2 + low;
            System.out.println("mid:" + mid);
            //判断二叉树是否有mid个节点
            if(exist(root, depth, mid)){
                low = mid;
            }else{
                high = mid - 1;
            }
        }
        return low;
    }

    //根据节点编号寻找路径，最后判断节点是否为空，即可知道第mid个节点是否存在了
    //比如第mid个节点是第6个节点，那么编号为110，则1 0 表示二叉树先往右子树移动再往左子树移动，即可到达mid节点
    boolean exist(TreeNode root, int depth, int mid){
        //取出 11111，即最后一层的最大节点数
        int bits = 1 << (depth - 1);
        System.out.println("mid:" + mid + "; bits :" + bits);
        TreeNode node = root;
        while(node != null && bits > 0){
            if((bits & mid) == 0){
                node = node.left;
            }else{
                node = node.right;
            }
            bits >>= 1;
        }

        return node != null;
    }
}
```





<span id = "jump228"></span>

## 228. 汇总区间

给定一个无重复元素的有序整数数组 nums 。

返回 恰好覆盖数组中所有数字 的 最小有序 区间范围列表。也就是说，nums 的每个元素都恰好被某个区间范围所覆盖，并且不存在属于某个范围但不属于 nums 的数字 x 。

列表中的每个区间范围 [a,b] 应该按如下格式输出：

"a->b" ，如果 a != b
"a" ，如果 a == b

示例 1：

```java
输入：nums = [0,1,2,4,5,7]
输出：["0->2","4->5","7"]
解释：区间范围是：
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

示例 2：

```java
输入：nums = [0,2,3,4,6,8,9]
输出：["0","2->4","6","8->9"]
解释：区间范围是：
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```


提示：

* 0 <= nums.length <= 20
* -231 <= nums[i] <= 231 - 1
* nums 中的所有值都 互不相同
* nums 按升序排列





双指针

```java
class Solution {
    public List<String> summaryRanges(int[] nums) {

        List<String> res = new ArrayList<>();
        int n = nums.length;

        if(n == 0)  return res;
        int idx = 0;

        while(idx < n){
            int low = idx;
            idx++;
            while(idx < n && nums[idx] == nums[idx-1] + 1){
                idx++;
            }
            int high = idx - 1;
            StringBuilder sb = new StringBuilder();
            sb.append(Integer.toString(nums[low]));
            if(low < high){
                sb.append("->");
                sb.append(Integer.toString(nums[high]));
            }
            res.add(sb.toString());
        }
        return res;
    }
}
```










<span id = "jump235"></span>

## 235.二叉搜索树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/binarytree.png)

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





<span id= "jump242"></span>

## 242.有效的字母异位词

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1:

```java
输入: s = "anagram", t = "nagaram"
输出: true
```


示例 2:

```java
输入: s = "rat", t = "car"
输出: false
```

说明:
你可以假设字符串只包含小写字母。

进阶:
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？



排序比较：

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length())    return false;

        char[] char1 = s.toCharArray();
        char[] char2 = t.toCharArray();

        Arrays.sort(char1);
        Arrays.sort(char2);

        return Arrays.equals(char1,char2);
    }
}
```

哈希表：

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length())    return false;

        Map<Character, Integer> table = new HashMap<>();

        for(int i = 0; i < s.length(); ++i){
            char c = s.charAt(i);
            table.put(c,table.getOrDefault(c, 0) + 1);
        }

        for(int i = 0; i < t.length(); ++i){
            char c = t.charAt(i);
            table.put(c,table.getOrDefault(c, 0) - 1);
            if(table.get(c) < 0){
                return false;
            }
        }
        return true;

        
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







<span id = "jump316"></span>

## 316. 去除重复字母

给你一个字符串 s ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

注意：该题与 1081 https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters 相同

 

示例 1：

```java
输入：s = "bcabc"
输出："abc"
```




示例 2：

```java
输入：s = "cbacdcbc"
输出："acdb"
```






提示：

* 1 <= s.length <= 104
* s 由小写英文字母组成



单调栈

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        int[] count = new int[26];
        int n = s.length();

        for(int i = 0; i < n; ++i){
            count[s.charAt(i) - 'a']++;
        }
        StringBuilder sb = new StringBuilder();

        boolean[] visited = new boolean[26];
        //维护一个单调栈，如果当前字母小于栈顶字母，就将栈顶字母出栈
        //  但是如果当前栈顶字母的计数已经为0了，那就不能出栈了
      	//	如果当前字母已经在栈中了，没必要访问了，计数-1
        for(int i = 0; i < n; ++i){
            char cur = s.charAt(i);
            if(!visited[cur - 'a']){
                while(sb.length() > 0 && sb.charAt(sb.length() - 1) > cur){
                    if(count[sb.charAt(sb.length() - 1) - 'a'] > 0){
                        visited[sb.charAt(sb.length() - 1) - 'a'] = false;
                        sb.deleteCharAt(sb.length() - 1);
                    }else{
                        break;
                    }

                }
                sb.append(cur);
                visited[cur - 'a'] = true;
            }

            count[cur - 'a']--;
        }

        return sb.toString();
    }
}
```











<span id = "jump321"></span>

## 321.拼接最大数[tag]

给定长度分别为 m 和 n 的两个数组，其元素由 0-9 构成，表示两个自然数各位上的数字。现在从这两个数组中选出 k (k <= m + n) 个数字拼接成一个新的数，要求从同一个数组中取出的数字保持其在原数组中的相对顺序。

求满足该条件的最大数。结果返回一个表示该最大数的长度为 k 的数组。

说明: 请尽可能地优化你算法的时间和空间复杂度。

示例 1:

```java
输入:
nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
输出:
[9, 8, 6, 5, 3]
```

示例 2:

```java
输入:
nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
输出:
[6, 7, 6, 0, 4]
```


示例 3:

```java
输入:
nums1 = [3, 9]
nums2 = [8, 9]
k = 3
输出:
[9, 8, 9]
```

与删除k个数字类似：

```java
class Solution {
    //单调栈：保留k个数相当于移除nums1.length+nums2.length - k个数
    //从两个数组中取k个数，相当于从nums1中取k1个数，从nums2中取k2个数
    //k1 + k2 = k
    //也就是:
  	//1.从nums1中删除n1 - k1个数，使得子序列最大
    //2.从nums2中删除n2 - k2个数，使得子序列最大
    //若k < n1，那么nums1可以删除1、2、3、...、k个数
    //但是k有可能大于n1或n2，所以需要遍历每一种情况
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int m = nums1.length, n = nums2.length;
        int[] maxSubsequence = new int[k];
        int start = Math.max(0, k - n), end = Math.min(k, m);
        for (int i = start; i <= end; i++) {
            int[] subsequence1 = maxSubsequence(nums1, i);
            int[] subsequence2 = maxSubsequence(nums2, k - i);
            int[] curMaxSubsequence = merge(subsequence1, subsequence2);
            if (compare(curMaxSubsequence, 0, maxSubsequence, 0) > 0) {
                System.arraycopy(curMaxSubsequence, 0, maxSubsequence, 0, k);
            }
        }
        return maxSubsequence;
    }
    //单调栈，找到长度为leng-k的最大子序列
    public int[] maxSubsequence(int[] nums, int k) {
        int length = nums.length;
        int[] stack = new int[k];
        int top = -1;
        int remain = length - k;
        for (int i = 0; i < length; i++) {
            int num = nums[i];
            while (top >= 0 && stack[top] < num && remain > 0) {
                top--;
                remain--;
            }
            if (top < k - 1) {
                stack[++top] = num;
            } else {
                remain--;
            }
        }
        return stack;
    }
    //合并子序列
    public int[] merge(int[] subsequence1, int[] subsequence2) {
        int x = subsequence1.length, y = subsequence2.length;
        if (x == 0) {
            return subsequence2;
        }
        if (y == 0) {
            return subsequence1;
        }
        int mergeLength = x + y;
        int[] merged = new int[mergeLength];
        int index1 = 0, index2 = 0;
        for (int i = 0; i < mergeLength; i++) {
            if (compare(subsequence1, index1, subsequence2, index2) > 0) {
                merged[i] = subsequence1[index1++];
            } else {
                merged[i] = subsequence2[index2++];
            }
        }
        return merged;
    }
    //自定义比较方法。首先比较两个子序列的当前元素，
  	//如果两个当前元素不同，则选其中较大的元素作为下一个合并的元素，
    //否则需要比较后面的所有元素才能决定选哪个元素作为下一个合并的元素。
    public int compare(int[] subsequence1, int index1, int[] subsequence2, int index2) {
        int x = subsequence1.length, y = subsequence2.length;
        while (index1 < x && index2 < y) {
            int difference = subsequence1[index1] - subsequence2[index2];
            if (difference != 0) {
                return difference;
            }
            index1++;
            index2++;
        }
        return (x - index1) - (y - index2);
    }
}
```













<span id = "jump327"></span>

## 327.区间和的个数[tag]

给定一个整数数组 nums，返回区间和在 [lower, upper] 之间的个数，包含 lower 和 upper。
区间和 S(i, j) 表示在 nums 中，位置从 i 到 j 的元素之和，包含 i 和 j (i ≤ j)。

说明:
最直观的算法复杂度是 O(n2) ，请在此基础上优化你的算法。

示例:

```java
输入: nums = [-2,5,-1], lower = -2, upper = 2,
输出: 3 
解释: 3个区间分别是: [0,0], [2,2], [0,2]，它们表示的和分别为: -2, -1, 2。
```





归并排序是从最小子数组开始统计的，我们是在归并之前统计下标对，之后再归并，所以不影响。

```java
class Solution {
    //目的是为了统计符合
    //pre[j] - pre[i] >= lower && pre[j] - pre[i] <= upper
    //的下标（i,j）的个数
    //归并排序
    //让排好序的，待归并的两个子数组作为pre
    //计算从左边数组下标i到右边数组下标j的和是否满足条件
    public int countRangeSum(int[] nums, int lower, int upper) {
        long s = 0;
        long[] sum = new long[nums.length + 1];
        //计算[0,i]的前缀和
        for(int i = 0; i < nums.length; ++i){
            s += nums[i];
            sum[i + 1] = s;
        }
        return count(0, sum.length - 1, sum, lower, upper);
    }

    int count(int left, int right, long[] sum, int lower, int upper){
        if(left == right){
            return 0;
        }else{
            int mid = (right - left) / 2 + left;
            //归并
            int n1 = count(left, mid, sum, lower, upper);
            int n2 = count(mid + 1, right, sum, lower, upper);
            int res = n1 + n2;

            //统计下标对的数量

            //枚举左边数组的每一位
            int i = left;
            
            //让l和r指向右边数组的开头
            //让l向右移动，直到sum[l] - sum[i] >= lower
            //让r向右移动，直到sum[r] - sum[i] > upper
            //这样 r - l就是下标对的数量了
             int l = mid + 1;
             int r = mid + 1;
             while(i <= mid){
                 while(l <= right && sum[l] - sum[i] < lower){
                     l++;
                 }
                 while(r <= right && sum[r] - sum[i] <= upper){
                     r++;
                 }
                 res += r - l;
                 ++i;
             }
             //执行左右数组合并
             int[] arr = new int[right - left + 1];
             int ptr1 = left, ptr2 = mid + 1;
             int ptr = 0;

             while(ptr1 <= mid || ptr2 <= right){
                 if(ptr1 > mid){
                     arr[ptr++] = (int) sum[ptr2++];
                 }else if(ptr2 > right){
                     arr[ptr++] = (int) sum[ptr1++];
                 }else{
                     if(sum[ptr1] < sum[ptr2]){
                         arr[ptr++] = (int) sum[ptr1++];
                     }else{
                         arr[ptr++] = (int) sum[ptr2++];
                     }
                 }
             }

             //将归并好的数组放入前缀和数组中
             for(int k = 0; k < arr.length; ++k){
                 sum[left + k] = arr[k];
             }
             return res;

        }
    }
}
```







<span id = "jump328"></span>

## 328.奇偶链表

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

示例 1:

```java
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
```

示例 2:

```java
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```


说明:

* 应当保持奇数节点和偶数节点的相对顺序。
* 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null)    return head;
        //双指针
        ListNode pto = head;
        ListNode pte = head.next;
        while(pte != null){
            //移动节点
            ListNode o = pte.next;
            if(o == null)   break;
            ListNode tmp = o.next;
            o.next = pto.next;
            pto.next = o;
            pte.next = tmp;
            //让这两个指针分别指向奇数子链表和偶数子链表的末端
            pte = pte.next;
            pto = pto.next;
        }
        return head;
    }
}
```

```go
func oddEvenList(head *ListNode) *ListNode {
    if head == nil{
        return head
    }
    var pto *ListNode = head;
    var pte *ListNode = head.Next;
    for pte != nil {
        var o *ListNode = pte.Next;
        if o == nil {
            break;
        }
        var tmp *ListNode = o.Next;
        o.Next = pto.Next;
        pto.Next = o;
        pte.Next = tmp;
        pte = pte.Next;
        pto = pto.Next;
    }
    return head;
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







<span id = "jump349"></span>

## 349.两个数组的交集

给定两个数组，编写一个函数来计算它们的交集。

 

示例 1：

```java
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

示例 2：

```java
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
```


说明：

* 输出结果中的每个元素一定是唯一的。
* 我们可以不考虑输出结果的顺序。



哈希表：

```java
class Solution {
    
    public int[] intersection(int[] nums1, int[] nums2) {
        //哈希表
        List<Integer> list = new ArrayList<>();
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums1.length; ++i){
            set.add(nums1[i]);
        }
        for(int j = 0; j < nums2.length; ++j){
            if(set.contains(nums2[j]) && !list.contains(nums2[j])){
                list.add(nums2[j]);
            }
        }
        int[] res = new int[list.size()];
        for(int i = 0; i < list.size(); ++i){
            res[i] = list.get(i);
        }
        return res;
    }
}
```

都放入set中，再遍历较小的set：

```java
class Solution {
    
    public int[] intersection(int[] nums1, int[] nums2) {
        //哈希表
        List<Integer> list = new ArrayList<>();
        Set<Integer> set1 = new HashSet<>();
        for(int i = 0; i < nums1.length; ++i){
            set1.add(nums1[i]);
        }
        Set<Integer> set2 = new HashSet<>();
        for(int i = 0; i < nums2.length; ++i){
            set2.add(nums2[i]);
        }
        
        return compareSet(set1, set2);
    }

    int[] compareSet(Set<Integer> set1, Set<Integer> set2){
      	//这个很有趣，可以避免重复代码
        if(set1.size() > set2.size()){
            return compareSet(set2,set1);
        }

        Set<Integer> resSet = new HashSet<>();
        for(Integer num : set1){
            if(set2.contains(num)){
                resSet.add(num);
            }
        }

        int[] res = new int[resSet.size()];

        int index = 0;

        for(Integer num : resSet){
            res[index++] = num;
        }

        return res;

    }
}
```





<span id = "jump376"></span>

## 376. 摆动序列

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。

例如， [1,7,4,9,2,5] 是一个摆动序列，因为差值 (6,-3,5,-7,3) 是正负交替出现的。相反, [1,4,7,2,5] 和 [1,7,4,5,5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。

示例 1:

```java
输入: [1,7,4,9,2,5]
输出: 6 
解释: 整个序列均为摆动序列。
```

示例 2:

```java
输入: [1,17,5,10,13,15,10,5,16,8]
输出: 7
解释: 这个序列包含几个长度为 7 摆动序列，其中一个可为[1,17,10,13,10,16,8]。
```

示例 3:

```java
输入: [1,2,3,4,5,6,7,8,9]
输出: 2
```

进阶:

* 你能否用 O(n) 时间复杂度完成此题?



```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if(nums.length < 2) return nums.length;

        if(nums.length == 2){
            return nums[1] == nums[0] ? 1 : 2;
        }
        int pre = 0;
        int cnt = nums.length;
        //遍历序列，遇到连续上升序列时，保留峰值，遇到连续下降序列时，保留谷值
        for(int i = 0; i < nums.length - 1; ++i){
            int cur = nums[i] - nums[i+1];
            if(cur == 0){//相邻两个数字相等，需要去掉一个
                cnt--;
            }else{
                if(pre == 0){
                    pre = cur;
                }else{
                    if(pre * cur > 0){//相邻两个差值同号，需要去掉一个数字
                        cnt--;
                    }else{
                        pre = cur;
                    }
                }
            }
        }

        return cnt;
    }
}
```

```java
func wiggleMaxLength(nums []int) int {
    n := len(nums)
    if n < 2 {
        return n
    }

    pre := 0
    cnt := n
    //遍历序列，遇到连续上升序列时，保留峰值，遇到连续下降序列时，保留谷值
    for i := 0; i < n-1; i++ {
        cur := nums[i] - nums[i+1]
        if cur == 0 {//相邻两个数字相等，需要去掉一个
            cnt--
        }else{
            if pre == 0 {
                pre = cur
            }else{
                if pre * cur > 0 {//相邻两个差值同号，需要去掉一个数字
                    cnt--
                }else{
                    pre = cur
                }
            }
        }
    }

    return cnt
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





<span id = "jump381"></span>

## 381.O(1) 时间插入、删除和获取随机元素 - 允许重复

设计一个支持在平均 时间复杂度 O(1) 下， 执行以下操作的数据结构。

注意: 允许出现重复元素。

* insert(val)：向集合中插入元素 val。
* remove(val)：当 val 存在时，从集合中移除一个 val。
* getRandom：从现有集合中随机获取一个元素。每个元素被返回的概率应该与其在集合中的数量呈线性相关。

示例:

```java
// 初始化一个空的集合。
RandomizedCollection collection = new RandomizedCollection();

// 向集合中插入 1 。返回 true 表示集合不包含 1 。
collection.insert(1);

// 向集合中插入另一个 1 。返回 false 表示集合包含 1 。集合现在包含 [1,1] 。
collection.insert(1);

// 向集合中插入 2 ，返回 true 。集合现在包含 [1,1,2] 。
collection.insert(2);

// getRandom 应当有 2/3 的概率返回 1 ，1/3 的概率返回 2 。
collection.getRandom();

// 从集合中删除 1 ，返回 true 。集合现在包含 [1,2] 。
collection.remove(1);

// getRandom 应有相同概率返回 1 和 2 。
collection.getRandom();
```



用一个列表存放元素，再用一个map存放元素与下标的对应。

插入时，直接将元素插入到列表的最后，然后在map中更新元素索引。

删除时，从map中取出对应元素的索引，再在列表中将此元素与列表最后一个元素交换，最后删除列表最后一个元素即可。

需要注意的是：此元素可能与列表最后一个元素相等，那么就需要同步更新set和hashMap。

```java
class RandomizedCollection {

    List<Integer> nums;
    Map<Integer, Set<Integer>> hashIndex;
    /** Initialize your data structure here. */
    public RandomizedCollection() {
        nums = new ArrayList<>();
        hashIndex = new HashMap<>();
    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    public boolean insert(int val) {
        nums.add(val);
        Set<Integer> set = hashIndex.getOrDefault(val, new HashSet<>());
        set.add(nums.size() - 1);
        hashIndex.put(val, set);
        //size==1说明当前插入的元素是唯一的，没有重复
        return set.size() == 1;

    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    public boolean remove(int val) {
        Set<Integer> set = hashIndex.get(val);
        if(set == null) return false;
        //根据迭代器获取val在set中的索引
        Iterator<Integer> itr = set.iterator();
        int idx = 0;
        if(itr.hasNext()){
            idx = itr.next();
        }

        Set<Integer> lastSet = hashIndex.get(nums.get(nums.size() - 1));
      	//当需要删除的元素和列表最后一个元素不相等时，才需要分别更新两个集合
      	//因为从set中删除一个元素，需要更新索引
      	//从lastSet中删除并插入一个元素，需要更新索引
        if(set != lastSet){
            lastSet.remove(nums.size() - 1);
            lastSet.add(idx);
            set.remove(idx);
        }else{
          	//如果是同一个元素，那么直接删除set最后一个的值即可
            set.remove(nums.size() - 1);
        }
      	//当set为空时，就清出map
        if(set.size() == 0) hashIndex.remove(val);
        //最后处理列表：将找到的索引与最后一个元素交换，然后删除
        nums.set(idx, nums.get(nums.size() - 1));
        nums.remove(nums.size() - 1);
        return true;
    }
    
    /** Get a random element from the collection. */
    public int getRandom() {
        Random random = new Random();
        int idx = random.nextInt(nums.size());
        return nums.get(idx);
    }
}
```







<span id = "jump387"></span>

## 387. 字符串中的第一个唯一字符

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

 

示例：

```java
s = "leetcode"
返回 0

s = "loveleetcode"
返回 2
```






提示：你可以假定该字符串只包含小写字母。



```java
class Solution {
    public int firstUniqChar(String s) {
        int[] count = new int[26];

        int n = s.length();

        for(int i = 0; i < n; ++i){
            count[s.charAt(i) - 'a']++;
        }
        for(int i = 0; i < n; ++i){
            if(count[s.charAt(i) - 'a'] == 1){
                return i;
            }
        }
        return -1;
    }
}
```











<span id = "jump389"></span>

## 389. 找不同

给定两个字符串 s 和 t，它们只包含小写字母。

字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母。

请找出在 t 中被添加的字母。

 

示例 1：

```java
输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。
```

示例 2：

```java
输入：s = "", t = "y"
输出："y"
```

示例 3：

```java
输入：s = "a", t = "aa"
输出："a"
```

示例 4：

```java
输入：s = "ae", t = "aea"
输出："a"
```


提示：

* 0 <= s.length <= 1000
* t.length == s.length + 1
* s 和 t 只包含小写字母



位运算：

```java
class Solution {
    public char findTheDifference(String s, String t) {        
        char res = 0;
        int sl = s.length();
        int tl = t.length();

        for(int i = 0; i < sl; ++i){
            res ^= s.charAt(i);
        }
        for(int i = 0; i < tl; ++i){
            res ^= t.charAt(i);
        }
        return res;
    }
}
```



```go
func findTheDifference(s string, t string) byte {
    len, ch := len(s), t[len(s)]

    for i := 0; i < len; i++ {
        ch ^= s[i]
        ch ^= t[i]
    }

    return ch
}
```





<span id = "jump399"></span>

## 399. 除法求值

给你一个变量对数组 equations 和一个实数值数组 values 作为已知条件，其中 equations[i] = [Ai, Bi] 和 values[i] 共同表示等式 Ai / Bi = values[i] 。每个 Ai 或 Bi 是一个表示单个变量的字符串。

另有一些以数组 queries 表示的问题，其中 queries[j] = [Cj, Dj] 表示第 j 个问题，请你根据已知条件找出 Cj / Dj = ? 的结果作为答案。

返回 所有问题的答案 。如果存在某个无法确定的答案，则用 -1.0 替代这个答案。

 

注意：输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

 

示例 1：

```java
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
```




示例 2：

```java
输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]
示例 3：

输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]
```




提示：

* 1 <= equations.length <= 20
* equations[i].length == 2
* 1 <= Ai.length, Bi.length <= 5
* values.length == equations.length
* 0.0 < values[i] <= 20.0
* 1 <= queries.length <= 20
* queries[i].length == 2
* 1 <= Cj.length, Dj.length <= 5
* Ai, Bi, Cj, Dj 由小写英文字母与数字组成



并查集：

a / b = 2.0 说明 a = 2b， a 和 b 在一个集合中；

b / c = 3.0 说明 b = 3c ，b 和 c 在一个集合中。

![image-20210106100326546](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210106100326546.png)

![image-20210106100432917](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210106100432917.png)

```java
class Solution {
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        int e_size = equations.size();
        UnionFind unionFind = new UnionFind(2 * e_size);
        //预处理，将变量值与id进行映射
        Map<String, Integer> map = new HashMap<>(2 * e_size);
        int id = 0;

        for(int i = 0; i < e_size; ++i){
            List<String> equation = equations.get(i);
            String s1 = equation.get(0);
            String s2 = equation.get(1);

            if(!map.containsKey(s1)){
                map.put(s1, id);
                id++;
            }
            if(!map.containsKey(s2)){
                map.put(s2, id);
                id++;
            }

            unionFind.union(map.get(s1), map.get(s2), values[i]);
        }
        //做查询
        int q_size = queries.size();
        double[] res = new double[q_size];

        for(int i = 0; i < q_size; ++i){
            String s1 = queries.get(i).get(0);
            String s2 = queries.get(i).get(1);

            Integer id1 = map.get(s1);
            Integer id2 = map.get(s2);

            if(id1 == null || id2 == null){
                res[i] = -1.0d;
            }else{
                res[i] = unionFind.isConnected(id1, id2);
            }
        }

        return res;
        
    }
    private class UnionFind{
        private int[] parent;
        //指向父节点的权值
        private double[] weight;

        public UnionFind(int n){
            this.parent = new int[n];
            this.weight = new double[n];
            for(int i = 0; i < n; ++i){
                parent[i] = i;
                weight[i] = 1.0d;
            }
        }

        public void union(int x, int y, double value){
            //找到x和y的根结点
            int rootX = find(x);
            int rootY = find(y);
            if(rootX == rootY){
                //如果根结点相同，直接返回
                return;
            }
            //如果根结点不相同，就指定x的根结点作为rootY的根结点
            parent[rootX] = rootY;
            weight[rootX] = weight[y] * value / weight[x];
        }
        //查找根结点，同时进行路径压缩
        public int find(int x){
            if(x != parent[x]){
                int origin = parent[x];
                parent[x] = find(parent[x]);
                weight[x] *= weight[origin];
            }
            return parent[x];
        }

        public double isConnected(int x, int y){
            int rootX = find(x);
            int rootY = find(y);
            if(rootX == rootY){
                return weight[x] / weight[y];
            }else{
                return -1.0d;
            }
        }


    }
}
```







<span id = "jump402"></span>

## 402.移掉K位数字

给定一个以字符串表示的非负整数 num，移除这个数中的 k 位数字，使得剩下的数字最小。

注意:

* num 的长度小于 10002 且 ≥ k。
* num 不会包含任何前导零。

示例 1 :

```java
输入: num = "1432219", k = 3
输出: "1219"
解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。
```

示例 2 :

```java
输入: num = "10200", k = 1
输出: "200"
解释: 移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。
```

示例 3 :

```java
输入: num = "10", k = 2
输出: "0"
解释: 从原数字移除所有的数字，剩余为空就是0。
```



逐个删除数字期望开头的数字越小越好。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        if(num.length() <= k) return "0";
        //双端队列
        Deque<Character> deque = new LinkedList<>();
        //将所有元素依次入栈
        for(int i = 0; i < num.length(); ++i){
            char c = num.charAt(i);
            //当当前元素比栈顶元素小时，且k>0时，将栈顶弹出
            while(!deque.isEmpty() && k > 0 && deque.peekLast() > c){
                deque.pollLast();
                k--;
            }
            deque.offerLast(c);
        }
        //若序列都已经单调不降了，但是还有没删干净的数字，那么就将最后几个数字删了
        while(k > 0){
            deque.pollLast();
            k--;
        }
        //去除前导0
        while(deque.peekFirst() != null && deque.peekFirst() == '0'){
            deque.pollFirst();
        }
        //读取数字
        StringBuffer sb = new StringBuffer();
        while(!deque.isEmpty()){
            sb.append(deque.pollFirst());
        }
        //队列为空，就返回0
        if(sb.length() == 0){
            return "0";
        }
        return sb.toString();
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







<span id = "jump435"></span>

## 435. 无重叠区间

给定一个区间的集合，找到需要移除区间的最小数量，使剩余区间互不重叠。

注意:

可以认为区间的终点总是大于它的起点。
区间 [1,2] 和 [2,3] 的边界相互“接触”，但没有相互重叠。
示例 1:

```java
输入: [ [1,2], [2,3], [3,4], [1,3] ]

输出: 1

解释: 移除 [1,3] 后，剩下的区间没有重叠。
```

示例 2:

```java
输入: [ [1,2], [1,2], [1,2] ]

输出: 2

解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
```

示例 3:

```java
输入: [ [1,2], [2,3] ]

输出: 0

解释: 你不需要移除任何区间，因为它们已经是无重叠的了。
```

贪心：

* 区间相交，就把后一个区间删除
* 区间重合，就把前一个区间删除



```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if(intervals.length == 0)   return 0;
        int n = intervals.length;
        //按区间的开始位置排序
        Arrays.sort(intervals, (a,b) -> {
            if(a[0] > b[0]){
                return 1;
            }else if(a[0] == b[0]){
                return a[1] - b[1];
            }else{
                return -1;
            }
        });
        //需要删除的区间个数
        int res = 0;
        //遍历区间
        int l = intervals[0][0], r = intervals[0][1];
        for(int i = 1; i < n; ++i){
            //如果当前区间和前一个区间无重叠， 就更新l和r
            if(intervals[i][0] >= r){
                l = intervals[i][0];
                r = intervals[i][1];
            }//如果当前区间被前一个区间包裹住了，那就把前一个区间删除
            else if(intervals[i][1] <= r){
                l = intervals[i][0];
                r = intervals[i][1];
                res++;

            }else if(intervals[i][1] > r){//如果两个区间只是相交，那就把后一个区间删除
                res++;
            }

        }
        return res;
    }
}
```

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if(intervals.length == 0)   return 0;
        int n = intervals.length;
        //按区间的结束位置排序
        Arrays.sort(intervals, (a,b) -> {
            return a[1] - b[1];
        });
        //可留下的区间个数
        int res = 1;
        //遍历区间
        int r = intervals[0][1];
        for(int i = 1; i < n; ++i){
            //如果当前区间和前一个区间无重叠， 就更新r
            if(intervals[i][0] >= r){
                res++;
                r = intervals[i][1];
            }
        

        }
        return n - res;
    }
}
```









<span id = "jump437"></span>

## 437.路径总和 III

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

示例：

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

          10
         /  \
        5   -3
       / \    \
      3   2   11
     / \   \
    3  -2   1
    
    返回 3。和等于 8 的路径有:
    
     5 -> 3
     5 -> 2 -> 1
    -3 -> 11

和第560题类似，只不过这里是多个数组。

```java
class Solution {
    int cnt = 0;
    public int pathSum(TreeNode root, int sum) {
        //统计每个节点的前缀和pre，放入哈希表中
        //再遍历每条路径，统计pre - sum出现的个数
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0,1);
        preOrder(map, root,0, sum);
        return cnt;
    }

    void preOrder(Map<Integer, Integer> map, TreeNode root, int pre, int k){
        if(root != null){
            pre += root.val;
            if(map.containsKey(pre - k)){
                cnt += map.get(pre - k);
            }
            map.put(pre, map.getOrDefault(pre, 0) + 1);
            preOrder(map, root.left, pre, k);
            preOrder(map, root.right, pre, k);
            //回溯，这样在每个时刻，map中的节点都是同一条路径上的
            map.put(pre, map.get(pre) - 1);
        }
    }
}
```





<span id = "jump438"></span>

## 438.找到字符串中所有字母异位词

给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

说明：

字母异位词指字母相同，但排列不同的字符串。
不考虑答案输出的顺序。
示例 1:

```java
输入:
s: "cbaebabacd" p: "abc"
输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```


 示例 2:

```java
输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。
```

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int np = p.length();
        int ns = s.length();
        List<Integer> res = new ArrayList<>();
        if(np == 0 || ns == 0)  return  res;

        //现将p中的字符打到表中
        int[] table = new int[26];
        for(int i = 0; i < np; ++i){
            table[p.charAt(i) - 'a']++;
        }

        //初始化滑动窗口，窗口大小为np
        int l = 0, r = 0;
        char[] arr = s.toCharArray();

        //记录窗口状态的表
        int[] cur = new int[26];
        while(l <= (ns-np)){
            //窗口增长期
            if(r == l){
                Arrays.fill(cur,0);
                boolean tag = true;
                for(int i = l; i < l+np; ++i){
                    //table表中没有这个字符
                    if(table[arr[i] - 'a'] == 0){
                        l = i+1;
                        tag = false;
                        break;
                    }
                    //table表中有这个字符

                    //这个字符出现的次数超过了table表
                    if(table[arr[i] - 'a'] < cur[arr[i] - 'a']+1){
                        l++;
                        tag = false;
                        break;
                    }
                    cur[arr[i] - 'a']++;
                }
                //是由于异常才结束循环的
                if(tag == false){
                    r = l;
                    continue;
                }
                //正常结束循环，说明全部比较完毕了，都没有出现异常
                res.add(l);
                r = l + np;
            }else{
                //窗口滑动期
                //指针l和r同时向右滑动一个单位
                r++;
                //超出了边界
                if(r-1 == ns){
                    break;
                }
                //新加入的字符和移出去的字符相同
                if(arr[r-1] == arr[l]){
                    l++;
                    res.add(l);
                }else{
                    //回到窗口增长期
                    l++;
                    r = l;
                }

            }


        }
        return res;

    }
}
```













<span id = "jump452"></span>

## 452.用最少数量的箭引爆气球

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。

一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

给你一个数组 points ，其中 points [i] = [xstart,xend] ，返回引爆所有气球所必须射出的最小弓箭数。

示例 1：

```java
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：对于该样例，x = 6 可以射爆 [2,8],[1,6] 两个气球，以及 x = 11 射爆另外两个气球
```

示例 2：

```java
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
```

示例 3：

```java
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
```

示例 4：

```java
输入：points = [[1,2]]
输出：1
```

示例 5：

```java
输入：points = [[2,3],[2,3]]
输出：1
```






提示：

* 0 <= points.length <= 104
* points[i].length == 2
* -231 <= xstart < xend <= 231 - 1



贪心左端点排序：

```java
class Solution {
    
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0)  return 0;
        //区间排序：先按开始坐标升序排序，再按结束坐标升序排序
        Arrays.sort(points, (a,b)->{
            if(a[0] > b[0]){
                return 1;
            }else if(a[0] == b[0]){
                return a[1] - b[1];
            }else{
                return -1;
            }
        });

        int[] cur = points[0];
        int res = 0;
        //贪心地覆盖一个区间，使其包括后序与此区间相交区间，并更新此区间为二者的交集
        for(int i = 1; i < points.length; ++i){
            //cur的结束大于等于下一个区间的开始，说明可以一起戳破
            if(cur[1] >= points[i][0]){
                //更新cur
                cur[0] = Math.max(cur[0], points[i][0]);
                cur[1] = Math.min(cur[1], points[i][1]);
            }else{
                //cur的结束小于下一个区间的开始，说明没办法一起戳破了
                //弓箭数+1，移动cur
                res++;
                cur[0] = points[i][0];
                cur[1] = points[i][1];
            }
        }

        return res+1;
    }
}
```

贪心右端点排序，只需要知道最远可射到的位置即可

```java
class Solution {
    
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0)  return 0;
        //区间排序：先按开始坐标升序排序，再按结束坐标升序排序
        Arrays.sort(points, (a,b)->{
            if(a[1] > b[1]){
                return 1;
            }else{
                return -1;
            }
        });

        int cur = points[0][1];
        int res = 0;
        //贪心地覆盖一个区间，使其包括后序与此区间相交区间，并更新此区间为二者的交集
        for(int i = 1; i < points.length; ++i){
            //cur的结束大于等于下一个区间的开始，说明可以一起戳破
            if(cur < points[i][0]){
                res++;
                cur = points[i][1];
            }
        }

        return res+1;
    }
}
```





<span id = "jump455"></span>

## 455. 分发饼干

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

示例 1:

```java
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```




示例 2:

```java
输入: g = [1,2], s = [1,2,3]
输出: 2
解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
```






提示：

* 1 <= g.length <= 3 * 104
* 0 <= s.length <= 3 * 104
* 1 <= g[i], s[j] <= 231 - 1



排序+双指针

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Map<Integer, Integer> map = new HashMap<>();

        Arrays.sort(g);
        Arrays.sort(s);
        
        int p1 = 0, p2 = 0;
        int res = 0;
        while(p1 < g.length && p2 < s.length){
            if(g[p1] <= s[p2]){
                res++;
                p1++;
                p2++;
            }else{
                p2++;
            }
        }

        return res;
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





<span id = "jump463"></span>

## 463.岛屿的周长

给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

 

示例 :

```java
输入:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

输出: 16
```



```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        if(grid.length == 0)    return 0;
        if(grid.length == 1 && grid[0].length == 1) return grid[0][0] == 1 ? 4 : 0;
        int res = 0;
        for(int i = 0; i < grid.length; ++i){
            for(int j = 0; j < grid[0].length; ++j){
                if(grid[i][j] == 0) continue;
                //只有一行
                if(grid.length == 1){
                    //左边界
                    if(j == 0){
                        res += grid[i][j + 1] == 0 ? 1 : 0;
                        res += 3;
                    }else if(j == grid[0].length - 1){
                        res += grid[i][j - 1] == 0 ? 1 : 0;
                        res += 3;
                    }else{
                        res += grid[i][j - 1] == 0 ? 1 : 0;
                        res += grid[i][j + 1] == 0 ? 1 : 0;
                        res += 2;
                    }
                    continue;
                //只有一列
                }else if(grid[0].length == 1){
                    if(i == 0){
                        res += grid[i+1][j] == 0 ? 1 : 0;
                        res += 3;
                    }else if(i == grid.length - 1){
                        res += grid[i - 1][j] == 0 ? 1 : 0;
                        res += 3;
                    }else{
                        res += grid[i-1][j] == 0 ? 1 : 0;
                        res += grid[i+1][j] == 0 ? 1 : 0;
                        res += 2;
                    }
                    continue;
                }


                //边界和四个对角特别判断

                //1.在上边界
                if(i == 0){
                    //在左上角
                    if(j == 0){
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += 2;
                    //在右上角
                    }else if(j == grid[0].length - 1){
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += 2;
                    //上边界的边上，不包含角
                    }else{
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += 1;
                    }
                    res += grid[i+1][j] == 0 ? 1 : 0;
                //2.在下边界
                }else if( i == grid.length - 1){
                    //在左下角
                    if(j == 0){
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += 2;
                    //在右下角
                    }else if(j == grid[0].length - 1){
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += 2;
                    //下边界的边上，不包含角
                    }else{
                        res += grid[i][j+1] == 0 ? 1 : 0;
                        res += grid[i][j-1] == 0 ? 1 : 0;
                        res += 1;
                    }
                    res += grid[i-1][j] == 0 ? 1 : 0;
                //3.在左边界，不包含角
                }else if(j == 0 && i != 0 && i != grid.length - 1){
                    res += grid[i-1][j] == 0 ? 1 : 0;
                    res += grid[i+1][j] == 0 ? 1 : 0;
                    res += grid[i][j+1] == 0 ? 1 : 0;
                    res += 1;
                //4.在右边界，不包含角
                }else if(j == grid[0].length-1 && i != 0 && i != grid.length - 1){
                    res += grid[i-1][j] == 0 ? 1 : 0;
                    res += grid[i+1][j] == 0 ? 1 : 0;
                    res += grid[i][j-1] == 0 ? 1 : 0;
                    res += 1;
                //5.其他情况
                }else{
                    if(i - 1 >= 0)  res += grid[i-1][j] == 0 ? 1 : 0;
                    if(i + 1 < grid.length) res += grid[i+1][j] == 0 ? 1 : 0;
                    if(j - 1 >= 0)  res += grid[i][j-1] == 0 ? 1 : 0;
                    if(j + 1 < grid[0].length) res += grid[i][j+1] == 0 ? 1 : 0;
                }

            }
            
        }
        return res;
    }
}
```



```java
class Solution {
    static int[] dx = {0, 1, 0, -1};
    static int[] dy = {1, 0, -1, 0};

    public int islandPerimeter(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (grid[i][j] == 1) {
                    int cnt = 0; 
                    for (int k = 0; k < 4; ++k) {
                        int tx = i + dx[k];
                        int ty = j + dy[k];
                        if (tx < 0 || tx >= n || ty < 0 || ty >= m || grid[tx][ty] == 0) {
                            cnt += 1;
                        }
                    }
                    ans += cnt;
                }
            }
        }
        return ans;
    }
}

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







<span id = "jump493"></span>

## 493.翻转对

给定一个数组 nums ，如果 i < j 且 nums[i] > 2*nums[j] 我们就将 (i, j) 称作一个重要翻转对。

你需要返回给定数组中的重要翻转对的数量。

示例 1:

```java
输入: [1,3,2,3,1]
输出: 2
```

示例 2:

```java
输入: [2,4,3,5,1]
输出: 3
```


注意:

* 给定数组的长度不会超过50000。
* 输入数组中的所有数字都在32位整数的表示范围内。

```java
class Solution {
    //归并排序，对于数组nums[l...r]
    //我们已经分别求出了子数组nums[l...m]与nums[m+1...r]的翻转对数量
    //并已经将两个子数组分别排好序(归并排序的特点)
    //则nums[l...r]中的翻转对数量就是两个子数组翻转对数量之和
    //加上左右端点分别位于两个子数组的翻转对数量
    public int reversePairs(int[] nums) {
        if(nums.length == 0)    return 0;
        return reversePair(nums, 0, nums.length - 1);
    }

    public int reversePair(int[] nums, int left, int right){
        if(left == right){
            return 0;
        }
        
        int mid = (right + left) / 2;
        int n1 = reversePair(nums, left, mid);
        int n2 = reversePair(nums, mid+1, right);

        int res = n1 + n2;

        //统计下标对的数量
        int i = left;
        int j = mid + 1;

        //遍历每一个i
        while(i <= mid){
            //移动j,直到找到nums[i] <= 2 * nums[j]的位置，那么后续的nums[j]就没必要遍历了
            while(j <= right && (long) nums[i] > 2 * (long) nums[j]){
                ++j;
            }
            //记录符合要求的下标对
            res += j - mid - 1;
            i++;
        }

        //合并数组
        int[] sorted = new int[right - left + 1];
        int p1 = left, p2 = mid + 1;
        int p = 0;

        while(p1 <= mid || p2 <= right){
            if(p1 > mid){
                sorted[p++] = nums[p2++];
            }else if(p2 > right){
                sorted[p++] = nums[p1++];
            }
            else{
                if(nums[p1] < nums[p2]){
                    sorted[p++] = nums[p1++];
                }else{
                    sorted[p++] = nums[p2++];
                }
            }
        }

        for(int k = 0; k < sorted.length; ++k){
            nums[left + k] = sorted[k];
        }
        return res;
    }
}
```





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





<span id = "jump509"></span>

## 509. 斐波那契数

斐波那契数，通常用 F(n) 表示，形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
给你 n ，请计算 F(n) 。

 提示：

* 0 <= n <= 30



```java
class Solution {

    public int fib(int n) {

        int a = 0;
        int b = 1;

        for(int i = 2; i <= n; ++i){
            int tmp = a + b;
            a = b;
            b = tmp;
        }

        return n == 0 ? a : b;
    }
}
```

![image-20210104090303916](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/image-20210104090303916.png)

```java
class Solution {
    public int fib(int n) {
        double sqrt5 = Math.sqrt(5);
        double fibN = Math.pow((1 + sqrt5) / 2, n) - Math.pow((1 - sqrt5) / 2, n);
        return (int) Math.round(fibN / sqrt5);
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



<span id = "jump547"></span>

## 547. 省份数量

有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中` isConnected[i][j] = 1` 表示第 i 个城市和第 j 个城市直接相连，而 `isConnected[i][j] = 0` 表示二者不直接相连。

返回矩阵中 省份 的数量。

示例 1：

```java
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```


示例 2：

```java
输入：isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出：3
```


提示：

* 1 <= n <= 200
* n == isConnected.length
* n == isConnected[i].length
* `isConnected[i][j]` 为 1 或 0
* `isConnected[i][i] == 1`
* `isConnected[i][j] == isConnected[j][i]`





深度优先搜索着色

```java
class Solution {
    boolean[] visited;
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        visited = new boolean[n];
        int cnt = 0;
        //统计需要着色几次才能全都着上色
        for(int i = 0; i < n; ++i){
            if(!visited[i]){
                dfs(isConnected, i);
                cnt++;
            }
        }
        return cnt;
    }
    //着色，将这一轮dfs访问的城市都着色
    void dfs(int[][] isConnected, int k){
        visited[k] = true;
        for(int j = 0; j < isConnected.length; ++j){
            if(isConnected[k][j] == 1 && !visited[j]){
                dfs(isConnected, j);
            }
        }
    }
}
```



并查集

```java
class Solution {
    //并查集
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;

        int[] parent = new int[n];
        for(int i = 0; i < n; ++i){
            parent[i] = i;
        }

        for(int i = 0; i < n; ++i){
            for(int j = i + 1; j < n; ++j){
                if(isConnected[i][j] == 1){
                    //i和j之间有连接，就将其合并
                    union(parent, i, j);
                }
            }
        }

        int res = 0;
        for(int i = 0; i < n; ++i){
            res += parent[i] == i ? 1 : 0;
        }
        return res;
    }
    //合并的过程，就是找出各自的祖先，然后将其合并
    void union(int[] parent, int x, int y){
        int px = find(parent, x);
        int py = find(parent, y);
        if(px == py)    return;
        parent[px] = py;
    }
    //在找祖先的时候，执行路径压缩
    int find(int[] parent, int idx){
        if(parent[idx] != idx){
            parent[idx] = find(parent, parent[idx]);
        }
        return parent[idx];
    }
}
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







<span id = "jump605"></span>

## 605. 种花问题

假设你有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花卉不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给定一个花坛（表示为一个数组包含0和1，其中0表示没种植花，1表示种植了花），和一个数 n 。能否在不打破种植规则的情况下种入 n 朵花？能则返回True，不能则返回False。

示例 1:

```java
输入: flowerbed = [1,0,0,0,1], n = 1
输出: True
```

示例 2:

```java
输入: flowerbed = [1,0,0,0,1], n = 2
输出: False
```


注意:

* 数组内已种好的花不会违反种植规则。
* 输入的数组长度范围为 [1, 20000]。
* n 是非负整数，且不会超过输入数组的大小。



```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        if(flowerbed.length == 1){
            if(n == 0)  return true;
            return flowerbed[0] == 0;
        }
        for(int i = 0; i < flowerbed.length; ++i){
            if(flowerbed[i] == 1)   continue;
            if((i == 0  && flowerbed[i+1] == 0) 
            || (i == flowerbed.length - 1  && flowerbed[i-1] == 0)
            || (i > 0 && i < flowerbed.length - 1 && flowerbed[i-1] == 0 && flowerbed[i+1] == 0)){
                flowerbed[i] = 1;
                n--;
            }
            if(n <= 0){
                return true;
            }
        }

        return false;
    }
}
```



```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int prev = -1;
        int count = 0;
        //在两个已经种了花的位置[i,j]之间，可以种花的数量为(j - i - 3 + 1) / 2
        //双指针找这样的区间，计算可种花的数量
        for(int i = 0; i < flowerbed.length; ++i){
            if(flowerbed[i] == 1){
                if(prev < 0){
                    count += i / 2;
                }else{
                    count += (i - prev - 2) / 2;
                }
                prev = i;
            }
            if(n <= count)  return true;
        }
        if(prev < 0){
            //全空
            count += (flowerbed.length + 1) / 2;
        }else{
            //到了最后一个位置都没有花，就假设位置flowerbed.length + 1 处有花
            count += (flowerbed.length + 1 - 2 - prev) / 2;
        }
        return n <= count;
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





<span id = "jump684"></span>

## 684. 冗余连接

在本问题中, 树指的是一个连通且无环的无向图。

输入一个图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。每一个边的元素是一对[u, v] ，满足 u < v，表示连接顶点u 和v的无向图的边。

返回一条可以删去的边，使得结果图是一个有着N个节点的树。如果有多个答案，则返回二维数组中最后出现的边。答案边 [u, v] 应满足相同的格式 u < v。

示例 1：

```java
输入: [[1,2], [1,3], [2,3]]
输出: [2,3]
解释: 给定的无向图为:
  1
 / \
2 - 3
```

示例 2：

```java
输入: [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
解释: 给定的无向图为:
5 - 1 - 2
    |   |
    4 - 3
```


注意:

* 输入的二维数组大小在 3 到 1000。
* 二维数组中的整数在1到N之间，其中N是输入数组的大小。



```java
class Solution {
    //并查集：
    //在一棵树上加1条边，使其形成环
    //初始时让每个节点都属于不同的连通分量
    //遍历每一条边，若当前边的两个顶点属于不同的连通分量，则这条边不会导致环路出现
    //若当前边的两个顶点已经属于同一个连通分量，说明这条边加上后会导致环路出现

    int[] parent;
    public int[] findRedundantConnection(int[][] edges) {
        int N = edges.length;//图中顶点的个数
        parent = new int[N+1];

        for(int i = 1; i <= N; ++i){
            parent[i] = i;
        }

        for(int i = 0; i < N; ++i){
            if(union(parent, edges[i][0], edges[i][1])){
                return new int[]{edges[i][0], edges[i][1]};
            }
        }
        return new int[]{};
    }

    boolean union(int[] parent, int x, int y){
        int px = find(parent, x);
        int py = find(parent, y);
        if(px == py)    return true;
        parent[px] = py;
        return false;
    }

    int find(int[] parent, int idx){
        if(parent[idx] != idx){
            parent[idx] = find(parent, parent[idx]);
        }
        return parent[idx];
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
* J 中的字符不重复。



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



<span id = "jump721"></span>

## 721. 账户合并

给定一个列表 accounts，每个元素 accounts[i] 是一个字符串列表，其中第一个元素 accounts[i][0] 是 名称 (name)，其余元素是 emails 表示该账户的邮箱地址。

现在，我们想合并这些账户。如果两个账户都有一些共同的邮箱地址，则两个账户必定属于同一个人。请注意，即使两个账户具有相同的名称，它们也可能属于不同的人，因为人们可能具有相同的名称。一个人最初可以拥有任意数量的账户，但其所有账户都具有相同的名称。

合并账户后，按以下格式返回账户：每个账户的第一个元素是名称，其余元素是按顺序排列的邮箱地址。账户本身可以以任意顺序返回。

 

示例 1：

```java
输入：
accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
输出：
[["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
解释：
第一个和第三个 John 是同一个人，因为他们有共同的邮箱地址 "johnsmith@mail.com"。 
第二个 John 和 Mary 是不同的人，因为他们的邮箱地址没有被其他帐户使用。
可以以任何顺序返回这些列表，例如答案 [['Mary'，'mary@mail.com']，['John'，'johnnybravo@mail.com']，
['John'，'john00@mail.com'，'john_newyork@mail.com'，'johnsmith@mail.com']] 也是正确的。
```


提示：

* accounts的长度将在[1，1000]的范围内。
* accounts[i]的长度将在[1，10]的范围内。
* `accounts[i][j]`的长度将在[1，30]的范围内。



并查集+哈希表，将邮件地址作为顶点进行编号，然后将同一个账户的邮件地址通过并查集合并，最后将同一个账户的邮件地址进行排序。

需要邮件地址->编号的映射，在合并邮件时，需要读取邮件的编号。

需要邮件地址->账户名的映射，最终记录结果时，需要将账户名放在列表首位。

需要parent->邮件列表的映射，同一个账户的邮件地址放入同一个列表中。

所以需要三个哈希表。



```java
class Solution {
    //并查集，将每个邮件地址作为顶点，若同属于一个账户，就进行合并
    int[] parent;
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        //两个哈希表
        //邮件对编号
        Map<String, Integer> emailToIndex = new HashMap<>();
        //邮件对名称
        Map<String, String> emailToName = new HashMap<>();
        //对邮件进行编号
        int emailCount = 0;
        //遍历所有邮件
        for(List<String> account : accounts){
            String name = account.get(0);
            int size = account.size();
            for(int i = 1; i < size; ++i){
                String email = account.get(i);
                //建立邮件对编号、邮件对账户名的映射
                //即使在不同列表中的邮件地址，只要邮件地址相同，编号就一样
                if(!emailToIndex.containsKey(email)){
                    emailToIndex.put(email, emailCount++);
                    emailToName.put(email, name);
                }
            }
        }

        //并查集
        parent = new int[emailCount];
        for(int i = 0; i < emailCount; ++i){
            parent[i] = i;
        }
        //将同个账户的邮件编号在并查集中合并
        for(List<String> account : accounts){
            String firstEmail = account.get(1);
            int firstIndex = emailToIndex.get(firstEmail);
            int size = account.size();
            for(int i = 2; i < size; ++i){
                String nextEmail = account.get(i);
                int nextIndex = emailToIndex.get(nextEmail);
                //将邮箱地址进行合并
                union(firstIndex, nextIndex);
            }
        }
        //将parent作为key，邮件列表作为value
        Map<Integer, List<String>> indexToEmails = new HashMap<>();
        //遍历所有邮件，将相同parent的邮件地址放入同一个列表中
        for(String email : emailToIndex.keySet()){
            //找到每个邮件地址的parent，将邮件地址放入哈希表中
            int index = find(emailToIndex.get(email));
            List<String> account = indexToEmails.getOrDefault(index, new ArrayList<String>());
            account.add(email);
            indexToEmails.put(index, account);
        }


        List<List<String>> merged = new ArrayList<>();
        for(List<String> emails : indexToEmails.values()){
            //将邮件地址排序
            Collections.sort(emails);
            String name = emailToName.get(emails.get(0));
            List<String> account = new ArrayList<>();
            //组装放入account
            account.add(name);
            account.addAll(emails);

            //放入结果集
            merged.add(account);
        }

        return merged;


    }

    void union(int x, int y){
        int px = find(x);
        int py = find(y);

        if(px == py) return;

        parent[py] = px;
    }

    int find(int idx){
        if(parent[idx] != idx){
            parent[idx] = find(parent[idx]);
        }

        return parent[idx];
    }

}
```





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





<span id = "jump830"></span>

## 830. 较大分组的位置

在一个由小写字母构成的字符串 s 中，包含由一些连续的相同字符所构成的分组。

例如，在字符串 s = "abbxxxxzyy" 中，就含有 "a", "bb", "xxxx", "z" 和 "yy" 这样的一些分组。

分组可以用区间 [start, end] 表示，其中 start 和 end 分别表示该分组的起始和终止位置的下标。上例中的 "xxxx" 分组用区间表示为 [3,6] 。

我们称所有包含大于或等于三个连续字符的分组为 较大分组 。

找到每一个 较大分组 的区间，按起始位置下标递增顺序排序后，返回结果。

 

示例 1：

```java
输入：s = "abbxxxxzzy"
输出：[[3,6]]
解释："xxxx" 是一个起始于 3 且终止于 6 的较大分组。
```

示例 2：

```java
输入：s = "abc"
输出：[]
解释："a","b" 和 "c" 均不是符合要求的较大分组。
```


示例 3：

```java
输入：s = "abcdddeeeeaabbbcd"
输出：[[3,5],[6,9],[12,14]]
解释：较大分组为 "ddd", "eeee" 和 "bbb"
```

提示：

* 1 <= s.length <= 1000
* s 仅含小写英文字母



双指针

```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String s) {
        List<List<Integer>> res = new ArrayList<>();
        if(s.length() == 0) return res;
        
        char[] arr = s.toCharArray();

        int l = 0, r = 0;

        while(r < arr.length && l < arr.length){

            int cnt = 0;
            //固定左指针，移动右指针找相同字符的最大个数
            while(r < arr.length && arr[r] == arr[l]){
                cnt++;
                r++;
            }
            //如果最大个数大于等于3，说明属于一个分组
            if(cnt >= 3){
                List<Integer> list = new ArrayList<>();
                list.add(l);
                list.add(r - 1);
                res.add(list);
            }
            
            if(r >= arr.length) break;
            l = r;
        }

        return res;
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





<span id = "jump947"></span>

## 947. 移除最多的同行或同列石头

n 块石头放置在二维平面中的一些整数坐标点上。每个坐标点上最多只能有一块石头。

如果一块石头的 同行或者同列 上有其他石头存在，那么就可以移除这块石头。

给你一个长度为 n 的数组 stones ，其中 stones[i] = [xi, yi] 表示第 i 块石头的位置，返回 可以移除的石子 的最大数量。

 

示例 1：

```java
输入：stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
输出：5
解释：一种移除 5 块石头的方法如下所示：

1. 移除石头 [2,2] ，因为它和 [2,1] 同行。
2. 移除石头 [2,1] ，因为它和 [0,1] 同列。
3. 移除石头 [1,2] ，因为它和 [1,0] 同行。
4. 移除石头 [1,0] ，因为它和 [0,0] 同列。
5. 移除石头 [0,1] ，因为它和 [0,0] 同行。
石头 [0,0] 不能移除，因为它没有与另一块石头同行/列。
示例 2：
```




提示：

* 1 <= stones.length <= 1000
* 0 <= xi, yi <= 104
* 不会有两块石头放在同一个坐标点上



连通在一起的石头可以被移除到只剩一个，因此这题是计算连通分量的个数。

将每个石头视作一个顶点，两个石头的坐标中x或y相等，则这两个顶点之间有边。因此可以根据此建立并查集计算连通分量。

```java
class Solution {
    //并查集
    //计算连通分量的个数
    int[] parent;
    public int removeStones(int[][] stones) {
        int n = stones.length;
        parent = new int[n];
        for(int i = 0; i < n; ++i){
            parent[i] = i;
        }
        //n块石头视为图的n个顶点，顶点间的边关系由坐标得到，x相等或者y相等的视为两点之间有边
        for(int i = 0; i < n; ++i){
            for(int j = i+1; j < n; ++j){
                if(stones[i][0] == stones[j][0]){
                    union(parent, i, j);
                }else if(stones[i][1] == stones[j][1]){
                    union(parent, i, j);
                }
            }
        }
        int res = 0;
        for(int i = 0; i < n; ++i){
            if(parent[i] == i){
                res++;
            }
        }
        return n - res;
    }
    void union(int[] parent, int x, int y){
        int px = find(parent, x);
        int py = find(parent, y);

        if(px == py)    return;

        parent[py] = px;
    }

    int find(int[] parent, int idx){
        if(parent[idx] != idx){
            parent[idx] = find(parent, parent[idx]);
        }
        return parent[idx];
    }
}
```



优化：遍历每块石头与其他石头的关系时间消耗太大，我们可以将处于同一行或者同一列的石头视为连通的，因此我们可以将行数或者列数作为parent。

但是行数和列数可能会重叠，如坐标(2,2)行数和列数是相同的。观察到坐标范围在1到10000之间，所以我们可以将x或y的坐标映射到其他区间内，这样就能错开行列了。

同时行和列可能是稀疏分布的，因此要用Map替代数组实现并查集。

```java
class Solution {
    //并查集
    //计算连通分量的个数
    int count = 0;
    Map<Integer, Integer> parent = new HashMap<>();
    public int removeStones(int[][] stones) {
        int n = stones.length;

        //n块石头视为图的n个顶点，顶点间的边关系由坐标得到，x相等或者y相等的视为两点之间有边
        for(int i = 0; i < n; ++i){
            union(parent, stones[i][0], stones[i][1] + 10001);
        }

        return n - count;
    }

    void union(Map<Integer, Integer> parent, int x, int y){
        int px = find(parent, x);
        int py = find(parent, y);

        if(px == py)    return;
        //两个节点需要合并，那么count需要减1
        parent.put(py, px);
        count--;
    }

    int find(Map<Integer, Integer> parent, int idx){
        if(!parent.containsKey(idx)){//并查集中没有这个节点，就计数并放入并查集
            parent.put(idx, idx);
            count++;
        }
        if(parent.get(idx) != idx){
            parent.put(idx, find(parent, parent.get(idx)));
        }
        return parent.get(idx);
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





<span id = "jump1018"></span>

## 1018. 可被 5 整除的二进制前缀

给定由若干 0 和 1 组成的数组 A。我们定义 N_i：从 A[0] 到 A[i] 的第 i 个子数组被解释为一个二进制数（从最高有效位到最低有效位）。

返回布尔值列表 answer，只有当 N_i 可以被 5 整除时，答案 answer[i] 为 true，否则为 false。

 

示例 1：

```java
输入：[0,1,1]
输出：[true,false,false]
解释：
输入数字为 0, 01, 011；也就是十进制中的 0, 1, 3 。只有第一个数可以被 5 整除，因此 answer[0] 为真。
```

示例 2：

```java
输入：[1,1,1]
输出：[false,false,false]
```


提示：

* 1 <= A.length <= 30000
* A[i] 为 0 或 1



循环左移，低位补A[i]，再循环取模。

```java
class Solution {
    public List<Boolean> prefixesDivBy5(int[] A) {
        List<Boolean> res = new ArrayList<>(A.length);
        int num = 0;
        for(int i = 0; i < A.length; ++i){
            num = ((num << 1) + A[i]) % 5;
            if(num == 0){
                res.add(Boolean.TRUE);
            }else{
                res.add(Boolean.FALSE);
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





<span id = "jump1202"></span>

## 1202. 交换字符串中的元素

给你一个字符串 s，以及该字符串中的一些「索引对」数组 pairs，其中 pairs[i] = [a, b] 表示字符串中的两个索引（编号从 0 开始）。

你可以 任意多次交换 在 pairs 中任意一对索引处的字符。

返回在经过若干次交换后，s 可以变成的按字典序最小的字符串。

 

示例 1:

```java
输入：s = "dcab", pairs = [[0,3],[1,2]]
输出："bacd"
解释： 
交换 s[0] 和 s[3], s = "bcad"
交换 s[1] 和 s[2], s = "bacd"
```

示例 2：

```java
输入：s = "dcab", pairs = [[0,3],[1,2],[0,2]]
输出："abcd"
解释：
交换 s[0] 和 s[3], s = "bcad"
交换 s[0] 和 s[2], s = "acbd"
交换 s[1] 和 s[2], s = "abcd"
```

示例 3：

```java
输入：s = "cba", pairs = [[0,1],[1,2]]
输出："abc"
解释：
交换 s[0] 和 s[1], s = "bca"
交换 s[1] 和 s[2], s = "bac"
交换 s[0] 和 s[1], s = "abc"
```






提示：

* 1 <= s.length <= 10^5
* 0 <= pairs.length <= 10^5
* 0 <= `pairs[i][0]`,` pairs[i][1]` < s.length
* s 中只含有小写英文字母



并查集：一开始想的是将每个连通分量的字符序列进行排序，再依次填回去，发现超时了。

可以将排序用优先队列替换，将每个连通分量的代表元放入哈希表，将优先队列与其对应，直接查询能够大大减少时间复杂度。

```java
class Solution {
    int[] parent;
    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        int len = s.length();
        char[] arr = s.toCharArray();
        parent = new int[len];

        for(int i = 0; i < len; ++i){
            parent[i] = i;
        }

        //对每个连通分量内的索引代表的字符进行排序，并且填入arr中
        for(List<Integer> pair : pairs){
            union(parent, pair.get(0), pair.get(1));
        }
        //遍历每个连通分量的代表元，将子元素都放入代表元下的优先队列中，用哈希表优化映射，直接搜索代表元的子元素会超时
        HashMap<Integer, PriorityQueue<Character>> hashMap = new HashMap<>();
        for(int i = 0; i < len; ++i){
            int root = find(parent, i);
            if(hashMap.containsKey(root)){
                hashMap.get(root).offer(arr[i]);
            }else{
                hashMap.computeIfAbsent(root, key -> new PriorityQueue<>()).offer(arr[i]);
            }
        }

        //重组字符串
        StringBuilder sb = new StringBuilder();

        for(int i = 0; i < len; ++i){
            int root = find(parent, i);
            sb.append(hashMap.get(root).poll());
        }
        return sb.toString();

    }


    void union(int[] parent, int x, int y){
        int px = find(parent, x);
        int py = find(parent, y);
        if(px == py)    return;
        parent[px] = py;
    }

    int find(int[] parent, int idx){
        if(parent[idx] != idx){
            parent[idx] = find(parent, parent[idx]);
        }
        return parent[idx];
    }
}
```



<span id = "jump1203"></span>

## 1203. 项目管理

公司共有 n 个项目和  m 个小组，每个项目要不无人接手，要不就由 m 个小组之一负责。

group[i] 表示第 i 个项目所属的小组，如果这个项目目前无人接手，那么 group[i] 就等于 -1。（项目和小组都是从零开始编号的）小组可能存在没有接手任何项目的情况。

请你帮忙按要求安排这些项目的进度，并返回排序后的项目列表：

同一小组的项目，排序后在列表中彼此相邻。
项目之间存在一定的依赖关系，我们用一个列表 beforeItems 来表示，其中 beforeItems[i] 表示在进行第 i 个项目前（位于第 i 个项目左侧）应该完成的所有项目。
如果存在多个解决方案，只需要返回其中任意一个即可。如果没有合适的解决方案，就请返回一个 空列表 。

**示例 1：**

![img](https://hyc-pic.oss-cn-hangzhou.aliyuncs.com/1359_ex1-20210112100308782.png)

```java
输入：n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3,6],[],[],[]]
输出：[6,3,4,1,5,2,0,7]
```

示例 2：

```java
输入：n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3],[],[4],[]]
输出：[]
解释：与示例 1 大致相同，但是在排序后的列表中，4 必须放在 6 的前面。
```


提示：

* 1 <= m <= n <= 3 * 104
* group.length == beforeItems.length == n
* -1 <= group[i] <= m - 1
* 0 <= beforeItems[i].length <= n - 1
* 0 <= `beforeItems[i][j]` <= n - 1
* i != `beforeItems[i][j]`
* beforeItems[i] 不含重复元素



拓扑排序，根据组内依赖关系建立组内拓扑图、根据组间依赖关系建立组间拓扑图。在建图时将group[i]==-1的项目从m开始编号。

分别对组间、组内拓扑图执行拓扑排序。

```java
class Solution {
    //有几个关键点：
    //（1）相同小组的项目要相邻
    //（2）进行第i个项目前，需要beforeItem[i]中的项目都以完成
    //（3）beforeItem[i]为空的项目可以较为自由地安排，但需受限于关键点（1）
    //利用拓扑排序，首先解决组间依赖，再解决组内依赖
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        List<List<Integer>> groupItem = new ArrayList<>();
        for(int i = 0; i < n + m; ++i){
            groupItem.add(new ArrayList<>());
        }
        //建图
        //组间依赖图
        List<List<Integer>> groupGraph = new ArrayList<>();
        for(int i = 0; i < n + m; ++i){
            groupGraph.add(new ArrayList<>());
        }
        //组内依赖图
        List<List<Integer>> itemGraph = new ArrayList<>();
        for(int i = 0; i < n + m; ++i){
            itemGraph.add(new ArrayList<>());
        }
        //组间和组内入度数组
        int[] groupDeg = new int[n + m];
        int[] itemDeg = new int[n + m];
        //对group[i]=-1的项目进行编号
        List<Integer> id = new ArrayList<>();
        for(int i = 0; i < n + m; ++i){
            id.add(i);
        }
        int leftId = m;
        //将项目分组
        for(int i = 0; i < n; ++i){
            if(group[i] == -1){
                group[i] = leftId++;
            }
            groupItem.get(group[i]).add(i);
        }
        //遍历所有项目，
        for(int i = 0; i < n; ++i){
            int curGroupId = group[i];
            //遍历第i个项目的条件
            for(int item : beforeItems.get(i)){
                int beforeGroupId = group[item];
                //如果第i个项目的条件与第i个项目属于同一个小组，就建立组内依赖关系
                if(beforeGroupId == curGroupId){
                    itemDeg[i]++;
                    itemGraph.get(item).add(i);
                }else{
                    //否则就建立组间依赖关系
                    groupDeg[curGroupId]++;
                    groupGraph.get(beforeGroupId).add(curGroupId);
                }
            }
        }

        //组间拓扑排序
        List<Integer> groupTopSort = topSort(groupDeg, groupGraph, id);
        if(groupTopSort.size() == 0){//组间不存在拓扑排序，就返回空数组
            return new int[0];
        }
        int[] res = new int[n];
        int idx = 0;
        //组内拓扑排序
        for(int curGroupId : groupTopSort){
            int size = groupItem.get(curGroupId).size();
            if(size == 0){
                continue;
            }
            List<Integer> list = topSort(itemDeg, itemGraph, groupItem.get(curGroupId));
            if(list.size() == 0){
                return new int[0];
            }

            for(int item : list){
                res[idx++] = item;
            }
        }
        return res;
    }
    //拓扑排序
    public List<Integer> topSort(int[] deg, List<List<Integer>> graph, List<Integer> items){
        Queue<Integer> queue = new LinkedList<>();
        //将入度为0的项目入队
        for(int item : items){
            if(deg[item] == 0){
                queue.offer(item);
            }
        }
        List<Integer> res = new ArrayList<>();
        while(!queue.isEmpty()){
            int u = queue.poll();
            res.add(u);
            //遍历顶点u的所有邻接点
            for(int v : graph.get(u)){
                //入度-1
                if(--deg[v] == 0){
                    queue.offer(v);
                }
            }
        }
        return res.size() == items.size() ? res : new ArrayList<Integer>();
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





