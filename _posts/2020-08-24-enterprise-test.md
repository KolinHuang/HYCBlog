---
title: 2020秋招企业笔试题
author: Kol Huang
date: 2020-08-24 21:10:00 +0800
categories: [Blogging, 企业笔试]
tags: [算法题解]
comments: true
math: true
image: /HYCBlog/assets/img/leetcode/ptoffer_cover.jpg
pin: true
---



## 京东-2020-8-6



### 统计在N到M之间的回文素数有多少个

0<=N<M<=1000000

把在N和M之间的数去掉一个数字后，是否还是回文素数？

是的话统计它的总数



#### 解法

```java
import java.util.Scanner;
public class Main {
    final static int maxn = 1000000 + 10;
    // 素数
    static boolean[] isp = new boolean[maxn];
    static int[] pow = new int[10];
    static void init() {
        // 打表，备忘录，全部初始化给 false
        for (int i = 0; i < maxn; i++) {
            isp[i] = true;
        }
        isp[0] = false;
        isp[1] = false;

        // 素数的倍数就不是素数，一并处理
        for (int i = 2; i < maxn; i++) {
            if (isp[i] == true) {
                for (int j = i + i; j < maxn; j += i) {
                    isp[j] = false;
                }
            }
          	
        }
      
      	
      	
      
      	//设置除数1,10,100,...,1000000用于抠数
        pow[0] = 1;
        for (int i = 1; i < 8; i++) {
            pow[i] = pow[i - 1] * 10;
        }
    }
    // 扣掉某个位置的数
    static int solve(int i, int j) {
        // 123456 要扣掉 4 即 i = 2的时候，pow = 100，12356 = (56 = 123456 % 100) + (12300 = 123456 / 1000 * 100)
        int ans = 0;
        ans += i % pow[j];
        ans += i / pow[j + 1] * pow[j];
        return ans;
    }
    // 回文检测，将数字转换为字符数组，双指针遍历
    static boolean isr(String num) {
        boolean res = true;
        int length = num.length();
      
      	//偶数长度的回文数中，除了11都不是素数
      	if(num.equals("11"))
          	return true;
      	if(length % 2 == 0)
          	return false;
      
      
        for (int i = 0; i < length / 2; i++) {
            if (num.charAt(i) != num.charAt(length - i - 1)) {
                res = false;
                break;
            }
        }
        return res;
    }
    public static void main(String[] args) {
        init();
        int n, m;
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        m = sc.nextInt();
        int ans = 0;
        for (int i = n; i <= m; i++) {
            // 长度
            int length = String.valueOf(i).length();
            // 从 最后一个位置开始扣
            for (int j = 0; j < length; j++) {
                int buf = solve(i, j);
                // 处理后的数既要是素数也要是回文
                if (isp[buf] && isr(String.valueOf(buf))) {
                    ans++;
                    break;
                }
            }
        }
        System.out.println(ans);
        sc.close();
    }
}
```

#### 回文数和素数的若干规律

* 回文数的生成，可由类似这种公式来生成10001 * i + 1010 * j + 100 * k；
* 判断n是否为素数，只需要考虑2到n的开平方后最大的整数之间的数是否能整除i就可以；
* 数学结论：除了11以外，所有回文素数的位数都是奇数；
* 从与N同位数的回文数开始遍历，列表解析式筛选出大于N-1的回文数；
* 还有一个结论，所有大于3的素数都在6的倍数附近(6倍原理)，基于此结论可以编写最高效率的素数遍历



#### 回文数的最优判别和统计

```java
public boolean isPalindrome(int x) {
        // 特殊情况：
        // 如上所述，当 x < 0 时，x 不是回文数。
        // 同样地，如果数字的最后一位是 0，为了使该数字为回文，
        // 则其第一位数字也应该是 0
        // 只有 0 满足这一属性
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }
        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }
        // 当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
        // 例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12，revertedNumber = 123，
        // 由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
        return x == revertedNumber || x == revertedNumber / 10;
    }
```



#### 素数的最优判别和统计

```java
public boolean isPrime(int N) {
        if (N < 2) return false;
        int R = (int) Math.sqrt(N);
        for (int d = 2; d <= R; ++d)
            if (N % d == 0) return false;
        return true;
}

```







## 华为-2020-8-12



### 1. 购物管理

在某咖啡店，每杯咖啡的售价为5元。

每位顾客只买一杯咖啡，客户支付面额为：5、10、20元。购物之后，根据面额给客户找钱。

开始备用零钱为0。

根据客户购买顺序，返回是否可以正确找零钱及当前客户顺序号。

说明：如果是能正确找零，则返回最后一个客户的顺序号，如果不能找零，则返回当前客户的顺序号；

客户编号从1开始；

约束：顾客数量限制在100以内；

输入描述：

```java
输入一串数字序列，表示每次顾客支付的金额
```

输出描述：

```java
输出：true,顺序号或者false,顺序号
若成功找零，则返回true,顺序号；否则返回false,顺序号
```

示例

```java
输入：5,5,5,10
输出：true,4
```

示例

```java
输入：10,10
输出：false,1
```

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;

public class Main{
    public String solution(int[] nums){
        if(nums[0] != 5)
            return "false,1";
        List<Integer> list = new ArrayList<>();
        for(int i = 0; i < nums.length; ++i){
            if(nums[i] == 5)
                list.add(nums[i]);
            else {
                boolean yes = false;
                for (Integer integer : list) {
                    nums[i] -= integer;
                    if(nums[i] == 0)
                        yes = true;
                }
                if(!yes)
                    return "false," + (i+1);
            }
        }
        return "true,"+nums.length;

    }
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        String[] arr = input.nextLine().split(",");

        int[] num = new int[arr.length];
        for(int i=0;i<arr.length;i++){
            num[i] = Integer.parseInt(arr[i]);
        }
        String res = new Main().solution(num);
        System.out.println(res);

    }
}
```









### 2. 走地砖



小明有一个爱好，走路喜欢踩在地砖格上，并且按照固定的步长走，某天，小明要通过以未完工的矩形路面，只有部分区域铺设了地砖格，尝试判断小明是否能够按照固定的步长N通过该区域。



**输入描述**

第一行为步长S（S>0）

第二行为矩阵的行列M,N（0 < M,N <= 100）

第三行开始为矩阵输入，0表示没有地砖，1表示有地砖。

小明从左上角出发，到右下角。这两个点保证为1。



**输出描述**

可以通过返回1，否则返回0

#### 答案

```java
package test;

import java.awt.*;
import java.util.*;
import java.util.List;

public class Main{
    int row;
    int col;
    int step;
    Set<Point> visited = new HashSet<>();
    public int solution(int[][] nums,int S){
        row = nums.length;
        col = nums[0].length;
        step = S;
        return dfs(nums,0,0);
    }

    int dfs(int[][] nums, int x, int y){
        if(nums[x][y] != 1)
            return 0;
        if(x == row - 1 && y == col - 1)
            return 1;
        visited.add(new Point(x,y));
        int res = 0;
        if(x - step >= 0 && !visited.contains(new Point(x - step,y)))
            res+=dfs(nums,x - step,y);
        if(y - step >= 0 && !visited.contains(new Point(x,y-step)))
            res+=dfs(nums,x,y-step);
        if(x + step < row && !visited.contains(new Point(x + step,y)))
            res+=dfs(nums,x + step,y);
        if(y + step < col && !visited.contains(new Point(x,y + step)))
            res+=dfs(nums,x,y + step);

        return res > 0 ? 1 : 0;
    }

    public static void main(String[] args) {
//        Scanner sc = new Scanner(System.in);
//        int s = sc.nextInt();
//        int m = sc.nextInt();
//        int n = sc.nextInt();
//
//        int[][] nums = new int[m][n];
//
//        for(int i = 0; i < m; ++i){
//            for (int j = 0; j < n; ++i){
//                nums[i][j] = sc.nextInt();
//            }
//        }

        int[][] nums = {
                {1,1,1,0,0},
                {0,1,1,0,1},
                {0,0,1,0,1}
        };
        int S = 1;
        int res = new Main().solution(nums,S);
        System.out.println(res);

    }
}
```







### 3. 按列输出字符串

给定一个仅由大写字符组成的字符串以及一个指定的奇列数，按从左到右，从上到下的顺序将给定的字符串以大X字形排列晨指定的列数。

例如，给定字符串为EVERYTHINGGOESWELL，指定列数为3，则大X字形排列为

```java
E  V
 E
R  Y
 T
H  I
 N
G  G
 O
E  S
 W
E  L
 L
```

然后从上到下逐列读取上面的大X字形排列，产生一个新的字符串，如读取上面的大X字形排列可得目标字符串为：ERHGEEETNOWLVYIGSL



**输入描述**

```java
任意输入一个仅由大写字母组成的字符串STRING，以及一个指定的奇列数，两者用半角都好分隔。
 其中（1 < STRING <2000）,(3 <= 1000)
```



#### 答案



将每个字母都用空格分割，再将重新组成的字符串填入数组arr中，按列读取即可

```java
public class Main{

    String solution(String s, int k){

        StringBuffer exeString = new StringBuffer();
        for(int i = 0; i < s.length(); ++i){
            exeString.append(s.charAt(i));
            if(i != s.length()-1)
                exeString.append(" ");
        }

        String exes = exeString.toString();

        int row = (exeString.length() / k) + 1;
        int col = k;
        char[][] arr = new char[row][col];
        for (char[] strings : arr) {
            Arrays.fill(strings,' ');
        }
        int x = 0;
        for(int i = 0; i < row; ++i){
            for(int j = 0; j < col; ++j){
                if(x >= exes.length())
                    break;
                arr[i][j] = exes.charAt(x++);
            }
        }
        StringBuffer res = new StringBuffer();
        for(int i = 0; i < col; ++i){
            for(int j = 0; j < row; ++j){
                if(arr[j][i] != ' ')
                    res.append(arr[j][i]);
            }
        }
        return res.toString();
    }
    public static void main(String[] args) {
//        Scanner sc = new Scanner(System.in);
//        String[] para = sc.nextLine().split(",");
//        String s = para[0];
//        Integer k = Integer.parseInt(para[1]);

        String s = "EVERYTHINGGOESWELL";
        int k = 37;
        String res = new Main().solution(s, k);
        System.out.println(res);
    }
}
```





## Bilibili-2020-8-13





### 1. 算24点



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

#### 我的答案

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



### 2. 有效括号

给定一个字包含括号的字符串，判断字符串是否有效。其中，括号种类包含

`'{','}','(',')','[',']'`，有效字符串需满足：（1）左括号必须用相同类型的右括号闭合。（2）左括号必须以正确的顺序闭合，注意空字符串可被认为是有效字符串。

#### 答案

##### 用栈


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

##### 用map+栈
以下代码需要放入

​        

```java
class Solution {
    private static final Map<Character,Character> map = new HashMap<Character,Character>()\{\{      put('{','}'); put('[',']'); put('(',')'); put('?','?');\}\};
    public boolean isValid(String s) {
        if(s.length() > 0 && !map.containsKey(s.charAt(0))) return false;
        LinkedList<Character> stack = new LinkedList<Character>() {{ add('?'); }};
        for(Character c : s.toCharArray()){
            if(map.containsKey(c)) stack.addLast(c);
            else if(map.get(stack.removeLast()) != c) return false;
        }
        return stack.size() == 1;
    }
}
```




### 3. 最少硬币

面值1元、4元、16元、64元共计4种硬币，以及面值1024元的纸币，现在小Y使用1024元的纸币购买了一件价值为N（0<N<=1024）的商品，请问最少他会收到多少硬币。



```java
	Scanner sc = new Scanner(System.in);
  int n = sc.nextInt();
  int cnum1,cnum2,cnum3,cnum4;
  cnum1=(1024-n)/64;                          //64元硬币的数量
  cnum2=((1024-n)%64)/16;                     //16元硬币的数量
  cnum3=(((1024-n)%64)%16)/4;                 //4元硬币的数量
  cnum4=(1024-n)-(cnum1*64+cnum2*16+cnum3*4); //1元硬币的数量
  System.out.println(cnum1+cnum2+cnum3+cnum4);
```





## 美团-2020-8-15

### 车辆调度

某个时刻A和B两地都向该公司提交了租车订单，分别需要a和b辆汽车。此时，公司的所有车辆都在外运营，通过北斗定位，可以得到所有车辆的位置，分别计算了每辆车前往A地和B地完成订单的利润。作为一名精明的调度师，当然是想让公司的利润最大化。

请你帮他分别选择a辆车完成A地任务，选择b辆车完成B地任务。使得公司获利最大，每辆车最多只能完成一地的任务。



输入描述

```java
输入第一行包含3个整数n,a,b，分别表示公司的车辆数量和A,B两地订单所需数量，保证a+b<=n(1<=n<=2000)
  接下来有n行，每行两个正整数x,y，分别表示该车完成A地任务的利润和B地任务的利润。
```



输出最大利润和



#### 我的答案

将每辆车到A的利润减去到B的利润，选择去A比去B贡献大，且最大的a辆车给A地，剩余n-a辆车中选择利润最大的b辆给B地。

```java
import com.sun.javafx.collections.MappingChange;

import javax.imageio.ImageTranscoder;
import java.lang.reflect.Array;
import java.util.*;
import java.util.stream.Collectors;

public class Main {


    public static int solution(int[] nums1, int[] nums2, int a, int b){
        int n = nums1.length;
        int res = 0;
//        int sum  = 0;
//        for(int i = 0; i < nums1.length; ++i){
//            sum += nums1[i];
//        }

        int[] diff = new int[n];

        Map<Integer,Integer> map = new HashMap<>();

      	//计算贡献差，并将其和下标绑定，便于在nums1和nums2中读取选择的车的利润
        for(int i = 0; i < n; ++i){
            diff[i] = nums1[i] - nums2[i];
            map.put(i,diff[i]);
        }

        Arrays.sort(diff);

        Set<Integer> set = new HashSet<>();
        for(int i = n-1; i >= 0; --i){
            for(int j = 0; j < map.size(); ++j){
                if(map.get(j) == diff[i] && !set.contains(j)){
                    if(a == 0) break;
                    a--;
                    set.add(j);
                    res+=nums1[j];
                }
            }
            if(a == 0) break;
        }

        for(int i = 0; i < b; ++i){
            res+= getMax(nums2,set);
        }

        return res;
    }
    static int getMax(int[] nums, Set<Integer> set){
        if(nums.length == 0) return 0;
        if(set.size() == nums.length) return 0;
        int max = Integer.MIN_VALUE;
        int pos = -1;
        for (int i = 0; i < nums.length; ++i){
            if(!set.contains(i) && max < nums[i]){
                max = nums[i];
                pos = i;
            }
        }
        if(pos != -1)
            set.add(pos);

        return max;
    }


    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int a = sc.nextInt();
        int b = sc.nextInt();
        int[] nums1 = new int[n];
        int[] nums2 = new int[n];
        for(int i = 0; i < n; ++i){
            int x = sc.nextInt();
            int y = sc.nextInt();
            nums1[i] = x;
            nums2[i] = y;
        }
        solution(nums1,nums2,a,b);
    }
}
```





## 字节跳动-2020-8-16





### 1. 编码协议

小明在学习一种编码协议，二进制数据可以用一串16进制的字符来表示（0-9，A-F），其中“0010”是这个协议的特殊字符串，不允许出现；

现在给出一串16进制的字符串s，问：

最少去掉几个字符，使得剩下的字符串不存在'0010'。



输入描述：

```java
第一行 t （1<=t <= 1000），表示接下来有t个样例

每个样例一行字符串s，s的长度不大于10^5
```



输入

```java
2
0123456789ABCDEF
001023456789ABCDEF00  
```

输出

```java
0
1
```



### 2. 二叉树的叶节点数



给定一颗二叉树，二叉树每个节点都有一个唯一的正整数值代表节点，在遍历时，我们使用节点的整数值作为标记。

输入：二叉树的节点个数，前序和中序遍历结果，分别是第一行、第二行与第三行

输出：二叉树叶节点的个数

```java
第一行 输入二叉树节点个数N，其中0<n<30000

第二行与第三行，分别输入二叉树的前序和中序遍历结果，每个节点对应唯一整数值
```

输入

```java
3
1 3 4
3 1 4
```

输出

```java
2
```







### 3. 广告插入

某视频网站上有N个视频，每个视频时长为L_i秒。

产品经理找到你，希望你帮忙在其中插入M个广告。

一个视频里的两个广告必须间隔一段时间（间隔时间可以为0）

考虑到用户体验，他希望这个间隔尽可能长。

为方便实现，间隔时间是一个整数



请你帮忙计算一下，这个间隔时间最大可以设置为多少秒呢？

如果不能插入M条广告，输出0

输入描述：

```java
第一行有两个整数 N,M (1 <= N <= 100000, N < M <= 5000000)
第二行有N个整数 L_i，表示视频的时长（1 <= L_i <= 1000000000）
```

输出描述：

```java
一个整数，表示最大的时间间隔
```

示例

```java
3 9
90 100 120
```



输出

```java
45
```



说明

```java
最长广告间隔为45秒
第一个视频时长90秒，可以在第0秒，第45秒，第90秒分别插入广告，总共3个广告。
```





### 寻觅

在n个正整数中，任意挑选K个（不可重复挑选，0 <= k <=n）数字的和记为sum，另有一个正整数m，请问sun % m最大为多少?

输入描述：

```java
第一行两个正整数，分别是n,m
第二行为n个正整数
```

输出描述：

```java
一个数x，x表示sum%m的最大值
```



示例

```java
5 5
1 2 3 4 5
```

输出

```java
4
```

备注：

```java
数据范围
2<=m <= 1e9 + 7
30%的数据(1 <= n <= 15)
40%的数据(1 <= n <= 25)
40%的数据(1 <= n <= 35)
```





## 滴滴-2020-8-21





### 逆序地顺时针打印斐波那契数

输入一个正整数n，表示矩阵的行列。用逆序的斐波那契数顺时针填入该矩阵。



```java
import java.util.Scanner;

class Solution {
    public static long[][] spiralOrder(long[] res, int n) {
        long[][] matrix = new long[n][n];
        if (matrix.length == 0) return new long[0][0];
        int l = 0, r = matrix[0].length - 1, t = 0, b = matrix.length - 1, x = res.length - 1;
        while (true) {
            for (int i = l; i <= r; i++) matrix[t][i] = res[x--];
            if (++t > b) break;
            for (int i = t; i <= b; i++) matrix[i][r] = res[x--];
            if (l > --r) break;
            for (int i = r; i >= l; i--) matrix[b][i] = res[x--];
            if (t > --b) break;
            for (int i = b; i >= t; i--) matrix[i][l] = res[x--];
            if (++l > r) break;
        }
        return matrix;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        if (n <= 1) {
            System.out.println(n);
            return;
        }
        // 顺时针填
        long[] f = new long[n * n];
        f[0] = 1;
        f[1] = 1;
        f[2] = 2;
        for (int i = 3; i < n * n; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }
        // 逆时针填回去
        long[][] ma = spiralOrder(f, n);
        for (int i = 0; i < ma.length; i++) {
            for (int j = 0; j < ma[i].length; j++) {
                System.out.print(ma[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```





### 熟悉的A+B

设a,b,c是0到9之间的整数（其中a,b,c互不相同），其中abc和acc是两个不同的三位数，现给定一正整数n，问有多少对abc和acc能满足abc+acc=n（a!=0）？

输入描述：

```java
一个正整数n（100<n<2000）
```

输出描述：

```java
第一行输出有多少对满足要求的数字。
接下来每一行输出一对abc和acc，以空格分隔。如果没有一对abc和acc的话。则直接输出0即可。如果有多对，请按照abc升序的次序输出。
```



样例输入

```java
1068
1
524 544
```



#### 答案

```java
import java.util.*;

public class Main1 {


    public static void main(String[] args) {


//        Scanner sc = new Scanner(System.in);
        int cnt = 0;
        for(int n = 100; n <=1000; ++n) {


            for (int i = 100; i < 999; i++) {
                for (int j = 100; j < 999; j++) {
                    if (i + j == n && i != n) {
                        boolean flag = true;
                        String a = String.valueOf(i);
                        if (a.charAt(0) == a.charAt(1) || a.charAt(0) == a.charAt(2) || a.charAt(2) == a.charAt(1)) {
                            flag = false;
                            continue;
                        }
                        String b = String.valueOf(j);
                        if (b.charAt(0) == b.charAt(1) || b.charAt(0) == b.charAt(2) || b.charAt(2) != b.charAt(1)) {
                            flag = false;
                            continue;
                        }

                        char[] Achars = a.toCharArray();
                        char[] Bchars = b.toCharArray();
                        if (Achars[2] != Bchars[2] || Achars[1] == Bchars[1] || Achars[0] != Bchars[0]) {
                            flag = false;
                        }

                        if (flag) {
//                            System.out.println(i + " " + j);
                            cnt++;
                        }
                    }
                }
            }
        }
        System.out.println(cnt);
    }
}
```







## 阿里巴巴-2020-8-24



### x和y的最大乘积

小强想要从[1,A]中选出一个整数x，从[1,B]中选出 一个整数y，使得满足x/y = a/b的同时且x和y的乘积最大。如果不存在这样的x和y，请输出“0 0”



输入描述：

```java
输入一行包含四个整数A,B,a和b
1<=A,B,a,b<=2e9
```

输出描述：输出两个整数表示满足条件的x和y，若不存在，则输出“0 0”

测试样例：

```java
1 1 2 1
0 0
  
1000 500 4 2
1000 500

1000 500 3 1
999 333
```



#### 答案



```java
1. 先将a和b按比例缩小到最小。gcd算法
2. 再找出区间[1,A]中a的最大倍数，即
		argmax(a * n <= A)
  		n
		同理再找出区间[1,B]中b的最大倍数
		argmax(b * m <= B)
		  m
3. 求 p = min(n,m)，此时p为a和b在满足成比例的情况下，能够扩大的最大倍数
		x = a * p
		y = b * p
4. x,y即为区间[1,A]和[1,B]内能取到的最大值，乘积也自然最大
```



```java
public class solution {

    static int A=0;
    static int B=0;
    static int a=0;
    static int b=0;
    static int x=0;
    static int y=0;

  	//找出a,b的最大公约数
    public  static int gcd(){
        int r;
        while (b > 0){
            r = a%b;
            a = b;
            b =r;
        }
        return a;
    }
    public static void exec(){
        int n;
      	// A/a 表示在[1,A]范围内，a的最大倍数，B/b表示在[1,B]范围内，b的最大倍数
      	// 二者取最小值，就是a和b能扩大的最大倍数；若取了最大值，a或b必有一者会超出区间限制
        n = Math.min( A/a, B/b);
      	//扩大
        x = n * a;
        y = n * b;
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        A = sc.nextInt();
        B = sc.nextInt();
        a = sc.nextInt();
        b = sc.nextInt();
        int g = gcd();
      	//比例缩小a,b的值
        a = a/g;
        b = b/g; 
        exec(A,B,a,b);

        System.out.println(x + " " +y);
    }
}
```

#### gcd算法

最大公约数(GCD, Greatest Common Divisor，为简便下文都使用GCD表示最大公约数)：指某几个整数共有约数中最大的一个。



1. 辗转相除法，又称欧几里得算法

定理：

```java
gcd(a,b) = acd(b,a mod b)
```

证明：

```markdown
(1)充分性：
设g是a,b的公约数，则a,b可由g来表示：	a = xg, b = yg (x,y为整数)

又a可由b表示（任意一个数都可以由另一个数来表示）：
a = kb + r (k为整数，r为a除以b所得余数)=> r = a - kb = xg - kyg = (x - ky)g
即，g也是r的约数。这样，g就是(b, r)即(b, a mod b)的公约数。


(2)必要性：
设g是(b, a mod b)的公约数，类似于(1)，同样可以推出g也是(a, b)的公约数。
```

```java
辗转相除的两种写法
      // 辗转相除法（递归写法）
    public static int gcd_division_recursive(int a, int b){
        if (b == 0) {
            return a;
        }
        return gcd_division_recursive(b, a % b);
    }

    // 辗转相除法（迭代写法）
    public static int gcd_division_iteration(int a, int b){
        while(b != 0){// 为什么用b判断呢？因为b就是用来存a%b的，即上面算法步骤里的r的
            int t = b;
            b = a % b;
            a = t;
        }
        return a;
    }
```

其实，只要知道了a,b的最大公约数，那最小公倍数不就是a * b / gcd(a, b)吗？



参考链接：

[最大公约数GCD的三种求法](https://www.jianshu.com/p/25d0ca88a4a1)





## 京东-2020-8-27



### 逆序五进制

编写一个程序，首先将一个十进制正整数逆序【需要去掉前导0】，然后转换成五进制正整数，最后输出该五进制正整数。



输入描述

```java
单组输入。
每组测试数据的输入占一行，输入一个十进制正整数n。（n <= 100000）
```

输出描述

```java
每组测试数据的输出占一行，输出转换后所得的五进制正整数。
```

样例：

```java
1000	1
77267	4420102
```



```java
public class Main {
    
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        StringBuilder sb = new StringBuilder(String.valueOf(n));
        int n1 = Integer.parseInt(sb.reverse().toString());
        sb.delete(0, sb.length());
        while (n1 != 0) {
            sb.append(n1 % 5);
            n1 /= 5;
        }
        System.out.println(Integer.valueOf(sb.reverse().toString()));
    }
}
```



还有一题智障题，就不贴了。





## 美团-2020-8-29





### 1.最长的被加密字符串

对一个字符串加密，规则如下：

```markdown
1. 添加一个首部。首部至少包含一个“MT”子序列，且以“T”结尾。
2. 添加一个尾部。尾部至少包含一个“MT”子序列，且以“M”开头。
3. 首部和尾部要尽量短。
```

现在给定一个加密后的字符串，请找出被加密的字符串S。



输入描述：

```markdown
输入第一行是一个正整数n，标识加密后的字符串总长度。（1<=n<=100000）
输入第二行是一个长度为n的仅由大写字母组成的字符串T。
```

输出描述：

```markdown
输出仅包含一个字符串S。
```

样例：

```markdown
10
MMATSATMMT

SATM
```

暴力法，果然超时了...我太菜了。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {


    static String solution(String s){
        //找出最短头部
        int pos_T = 0;
        for(int i = 0; i < s.length();){
            pos_T = s.indexOf('T');
            //判断在i到posT的之中是否有M
            String sub = s.substring(i,pos_T+1);
            if(sub.indexOf('M') != -1){
                //找到了头部的结尾
                break;
            }
            i = pos_T+1;
        }
        StringBuffer sb = new StringBuffer();
        for(int i = 0; i < s.length();++i){
            sb.append(s.charAt(i));
        }
				//问题应该出在这里了，字符串长度为100000时，reverse是很恐怖的..
      	//后面再改改吧，应该可以有其他办法改进
        String reverse = sb.reverse().toString();
        //找出最短尾部
        int pos_M = 0;
        for(int i = 0; i < reverse.length();){
            pos_M = s.indexOf('M');
            //判断在i到posT的之中是否有M
            if(reverse.substring(i,pos_M+1).indexOf('T') != -1){
                //找到了头部的结尾
                break;
            }
            i = pos_M+1;
        }
        return s.substring(pos_T+1,s.length()-pos_M-pos_T+1);
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String s = sc.next();
//        String s = "MMATSATMMT";
        String res = new Main().solution(s);
        System.out.println(res);
    }
}
```

正则表达式：

```java
import java.util.Scanner;
public class App {
		
    private static String reveString(String string) {
        return new StringBuilder(string).reverse().toString();
    }

  	
    public static String solve(String string) {
        String result = string.replaceFirst("^.*?M.*?T", "");
        result = reveString(result);
        result = result.replaceFirst("^.*?T.*?M", "");
        result = reveString(result);
        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        String s = sc.next();
        System.out.println(solve(s));
    }
}
```

为啥这边reverse不会有问题，难道我跟大佬的差距就是一个正则表达式？奥利给，明天肝正则！！！







### 2.小团的选调计划

美团打算把n个人分配到n个不同的业务区域，能者优先，n个人的业务能力排行为1～n。编号考前的人具有优先选择的权力。每个人填一个意向。这个意向是一个1～n的排列，表示一个人希望的业务区域顺序。有两个人同时希望去某一个业务区域则有限满足编号小的人。

输入：

```markdown
第一行n
n行，每行n个整数，即一个1～n的排列，第i行表示i-1号业务骨干的意向顺序。
```

输出：

```markdown
n个正整数，第i个表示第i号人去的区域编号。
```

样例：

```markdown
5
1 5 3 4 2
2 3 5 4 1
5 4 1 2 3
1 2 5 4 3
1 4 5 2 3

输出：1 2 5 4 3
```



### 3.树节点追逐

A追逐B。A和B在一颗有n个结点的树上，A位于x位置，B位于y位置，A和B每个单位时间内都可以选择不动活着向相邻的位置转移，假设A足够聪明，很显然B会无路可逃，请问最多经过多少个单位时间后，B会被追上？

输入：

```markdown
第一行包含三个整数n,x,y，分别表示树上的结点数量，A所在的位置和B所在的位置(1 <= n <= 50000)
接下来有n-1行，每行两个整数u,v，表示u号位置和v号位置之间有一条边，即u号位置和v号位置彼此相邻。
```

输出：

```markdown
一个整数，最多的单位时间。
```

样例：

```markdown
5 1 2
2 1
3 1
4 2
5 3

输出 2 
```



### 4.小团的默契游戏

有一个长度为n的序列，最大值不超过m。

A和B各自选择一个[1,m]之间的整数，设A选择的是l，B选择的是r，满足以下条件则为一个有效二元组：

```markdown
1. l <= r
2. 对于序列中的元素x, 如果0<x<l，或r < x < m+1，则x按其顺序保留下来，要求保留下来的子序列单调不下降。
```

请问这样的二元组有多少种？

输入：

```markdown
输入第一行，m,n，表示序列元素的最大值和序列的长度(1 <= n,m <= 100000)
第二行n个整数，表示序列。
```

输出：

```markdown
一个整数，符合要求的有效二元组数量。
```

样例：

```java
5 5
4 1 4 1 2

输出 10
```

