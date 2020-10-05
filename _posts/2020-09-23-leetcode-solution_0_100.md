---
title: Leetcode题解:1~100
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

[1.两数之和](#jump1)

[5.最长回文子串](#jump5)

[16.最接近三数之和](#jump16)

[17.电话号码的字母组合](#jump17)

[18.四数之和](jump18)

[37.解数独](#jump37)

[39.组合总和1](#jump39)

[40.组合总和2](#jump40)

[43.字符串相乘](#jump43)

[47.全排列](#jump47)

[51.N 皇后](#jump51)

[60.第k个排列[tag]](#jump60)

[63.含障碍物网格中的不同路径](#jump63)

[67.二进制求和](#jump67)

[77.组合[tag:字典序]](#jump77)

[79.单词搜索](#jump79)

[93. 恢复IP地址](#jump93)

[94.二叉树的中序遍历](#jump94)

[96. 不同的二叉搜索树](#jump96)

[100. 相同的树](#jump100)





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









<span id="jump93"></span>

## 93. 恢复IP地址

给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址正好由四个整数（每个整数位于 0 到 255 之间组成），整数之间用 '.' 分隔。

示例:

```java
输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]
```



![leetcode_恢复Ip地址](/Users/huangyucai/Documents/code/git_depositorys/github_KolinHuang/HYCBlog/assets/img/leetcode/leetcode_恢复Ip地址.png)

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

