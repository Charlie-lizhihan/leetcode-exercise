# Dynamic Programming

## \10. Regular Expression Matching

https://leetcode-cn.com/problems/regular-expression-matching/solution/shou-hui-tu-jie-wo-tai-nan-liao-by-hyj8/



```java
class Solution_Q10 {
    public boolean isMatch(String s, String p) {
        char[] cs = s.toCharArray();
        char[] cp = p.toCharArray();

        // dp[i][j]:表示s的前i个字符，p的前j个字符是否能够匹配
        boolean[][] dp = new boolean[cs.length + 1][cp.length + 1];

        // 初期值
        // s为空，p为空，能匹配上
        dp[0][0] = true;
        // p为空，s不为空，必为false(boolean数组默认值为false，无需处理)

        // s为空，p不为空，由于*可以匹配0个字符，所以有可能为true
        for (int j = 1; j <= cp.length; j++) {
            if (cp[j - 1] == '*') {
                dp[0][j] = dp[0][j - 2];
            }
        }

        // 填格子
        for (int i = 1; i <= cs.length; i++) {
            for (int j = 1; j <= cp.length; j++) {
                // 文本串和模式串末位字符能匹配上
                if (cs[i - 1] == cp[j - 1] || cp[j - 1] == '.') {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (cp[j - 1] == '*') { // 模式串末位是*
                    // 模式串*的前一个字符能够跟文本串的末位匹配上
                    if (cs[i - 1] == cp[j - 2] || cp[j - 2] == '.') {
                        dp[i][j] = dp[i][j - 2]      // *匹配0次的情况
                                || dp[i - 1][j];     // *匹配1次或多次的情况
                    } else { // 模式串*的前一个字符不能够跟文本串的末位匹配
                        dp[i][j] = dp[i][j - 2];     // *只能匹配0次
                    }
                }
            }
        }
        return dp[cs.length][cp.length];
    }
}

```









## \22. Generate Parentheses

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```
Input: n = 1
Output: ["()"]
```



The permulation of parentheses always obey the rules below:

- the amount of left parenthesis larger than right parenthesis

So we can create 2 parameters, left and right, respectively describe how many parenthesis we still do not put in the String, then we know that 

1. if left == right, we just can put the `(` in,
2. if left != right, left>0, we can put `(`and`)`  

the details of recursion steps are below

<img src="/Users/lizhihan/Downloads/各种照片/IMG_C687D58A31ED-1.jpeg" alt="IMG_C687D58A31ED-1" style="zoom:33%;" />

```java
import java.util.*;
class Solution {
    ArrayList<String> res = new ArrayList<String>();
    
    
    public List<String> generateParenthesis(int n) {
        if(n<=0) return res;
        generating("",n,n);
        return res;
    }
    
    
    public void generating(String str, int left,int right){
        if(left == 0 && right == 0) {
            res.add(str);
            return ;
        }
        
        if(left == right){
            generating(str+"(",left-1,right);
        }else{
            if(left>0){
                generating(str+"(",left-1,right);
            }
            generating(str+")",left,right-1);
        }
    }
}

```



## \32. Longest Valid **Parentheses**

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring. 

**Example 1:**

```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```

**Example 2:**

```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
```



<img src="/Users/lizhihan/Library/Application Support/typora-user-images/image-20211017103656122.png" alt="image-20211017103656122" style="zoom:100%;" />

```java
class Solution {
    public int longestValidParentheses(String s) {
        if(s.length()<=1) return 0;

        int max = 0;
        int [] dp = new int[s.length()];
        dp[0] = 0;
        dp[1] = 0;

        if(s.charAt(0) == '(' && s.charAt(1) == ')'){
            dp[1] = 2;
            max = dp[1];
        }
        

        for(int i = 2;i<s.length();i++){
            if(s.charAt(i) == '('){
                dp[i] = 0;
            }else{
                if(s.charAt(i-1) == '('){
                    dp[i] = dp[i-2]+2;
                }else if(dp[i-1]!=0 && i-dp[i-1]-1>=0 && s.charAt(i-dp[i-1]-1) == '('){
                    try{
                        dp[i] = dp[i-1]+dp[i-dp[i-1]-2]+2;
                    }catch(Exception e){
                        dp[i] = dp[i-1]+2;
                    }
                }
            }
            max = Math.max(max,dp[i]);
        }
    
        return max;
    }
}
```





## \42. Trapping Rain Water

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```



- brute force

intuition:

Find the largest height first, check the gap between currently largest numbers. Iterate this process, sum these gap, we can get the answer.

Take example 1 as an example, at first we know that 3 is the largest, but we just have 1 number, so the gap equals to 0; Next we find 2 is the largest, we know the 3rd, 7th, 8th,10th all reached the height of 2, gap = (7-3-1)+(8-7-1)+(10-8-1) = 4; Finally, we find largest number is 1 and 1st, 3rd, 4th, 6-11th all larger or equal 1, so the gap = (3-1-1)+(6-4-1)+......  = 2, output = sum(gap) = 6.



Time complexity: O(height.max*height.size)

Space complexity: O(height.size)

```java
import java.util.*;
class Solution {
    public int trap(int[] height) {
        int max = 0;
        for(int i = 0; i< height.length;i++){
            if(height[i]>max){
                max = height[i];
            }
        }
        if(max == 0) return 0;
        
        int res = 0;
        
        for(int j = max;j>=1;j--){
            ArrayList<Integer> tmp = new ArrayList<Integer>(); 
            for(int i = 0;i<height.length;i++){
                if(height[i] >= j){
                    tmp.add(i);
                }
            }
            
            for(int k = 0;k<tmp.size()-1;k++){
                res+=tmp.get(k+1)-tmp.get(k)-1;
            }
        }
        
        return res;
    }
}
```





- dynamic programming

Honestly I have no idea why this approach named dp. in my words, this is like a "cut approach" ....maybe?

intuition：

One picture could describe how to solve this problem, we just need to calculate the number of shadow of blocks. Create 2 arrays, one start from left, one from right.

![image-20211018122040172](/Users/lizhihan/Library/Application Support/typora-user-images/image-20211018122040172.png)

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        if (n == 0) {
            return 0;
        }
        
        int [] left = new int[n];
        int [] right = new int[n];        
        
        left[0] = height[0];
        for(int i = 1;i<n;i++){
            left[i] = Math.max(left[i-1],height[i]);
        }
        
        right[n-1] = height[n-1];
        for(int j = n-2;j >= 0;j--){
            right[j] = Math.max(right[j+1],height[j]);
        }
        
        int res = 0;
        for(int i = 0;i<n;i++){
            res += Math.min(left[i],right[i])-height[i];
        }
        return res;
    }
}

```







## \746. Min Cost Climbing Stairs

You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return *the minimum cost to reach the top of the floor*.

**Example 1:**

```
Input: cost = [10,15,20]
Output: 15
Explanation: Cheapest is: start on cost[1], pay that cost, and go to the top.
```

**Example 2:**

```
Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: Cheapest is: start on cost[0], and only step on 1s, skipping cost[3].
```



Using the dynamic programming could easily solve this problem. We know that we can jump 1 or 2 steps, so we just need to figure out min(dp[last], dp[last-1]), and use a for loop to calculate the dp array, we could get the answer.

BTW, in every loop, we just need to use the last 2 elements in dp array. So we can just use 2 parameters to describe a dp array, it could decrease the space complexity from O(n) to O(1). And we just use 1 for loop, time complexity is O(n)

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        if(cost.length == 0 || cost.length == 1) return 0;
             
        int First = cost[0];
        int Second = cost[1];
        
        for(int i = 2 ;i<cost.length;i++){
            int tmp = First;
            First = Second;
            Second = Math.min(Second,tmp) + cost[i];
        }
        
        return Math.min(First,Second);
    }
}
```





## \62. Unique Paths

A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
Input: m = 3, n = 7
Output: 28
```

**Example 2:**

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```



Intuition: 

We know the final end is equal to the number of left block + the number of up block. we just can move down and right, so the first row and the left column is always 1. After that, iterate the rest of blocks, we can get the answer.

```java
class Solution {
    public int uniquePaths(int m, int n) {
        if(m<=0||n<=0) return 0;
        
        int [][] dp = new int[m][n];
        dp[0][0] = 0;
        
        for(int i = 0;i<m;i++){
            dp[i][0] = 1;
        }
        for(int j = 0;j<n;j++){
            dp[0][j] = 1;
        }
        
        for(int i = 1;i<n;i++){
            for(int j = 1;j<m;j++){
                dp[j][i] = dp[j-1][i]+dp[j][i-1]; 
            }
        }
        
        return dp[m-1][n-1];
    }
}
```



The method mentioned above time complexity is O(m*n), space complexity is O(m\*n), we can simplify the code to reduce the space complexity from O(m\*n) to O(n).

 Cause we don't need store every row's numbers, what we need is the number of row before current row, so we could just create a one dimensional array and update it continuously.

```java
public class Solution {
    public int uniquePaths(int m, int n) {
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                arr[j] = arr[j] + arr[j-1];
            }
        }
        return arr[n-1];
    }
}
```





## \63. Unique Paths II

A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as `1` and `0` respectively in the grid.



**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```



Intuition:

First we should set the 1st row and 1st column, we set every element in dp is 1, but if we meet a 1 in the row or column, elements after 1 in dp array will set as 0

Then use the same dynamic approach like before. Only need to mention is that we should set the "obstacle" element as 0 in dp array. Time complexity O(m*n), space complexity O(m\*n).

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] ob) {
        if(ob.length == 0 || ob[0].length == 0) return 0;
        if(ob[0][0] == 1) return 0;
        if(ob.length == 1 && ob[0].length == 1){
            return 1;
        }
        
        int i = 0;
        for(; i<ob[0].length;i++){
            if(ob[0][i] == 1){
                break;
            }
            ob[0][i] = 1;
        }
        while(i<ob[0].length){
            ob[0][i] = 0;
            i++;
        }
        
        int j = 1;
        for(; j<ob.length;j++){
            if(ob[j][0] == 1){
                break;
            }
            ob[j][0] = 1;
        }
        while(j<ob.length){
            ob[j][0] = 0;
            j++;
        }
        
        for(int m = 1;m<ob.length;m++){
            for(int n = 1; n<ob[0].length; n++){
                if(ob[m][n] == 1){
                    ob[m][n] = 0;
                }else{
                    ob[m][n] = ob[m-1][n]+ob[m][n-1];
                }
            }
        }
        
        return ob[j-1][i-1];    
    }
}
```



Change the dp 2 dimentional array to a 1 dimentional array, principle is what we use in the last problem.

This could decrease the space complexity from O(m*n) to O(m).

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] ob) {
        int rows = ob[0].length;
        int [] dp = new int [rows];

        dp[0] = 1;
        for(int[] row:ob){
            for(int i = 0;i<rows;i++){
                if(row[i] == 1){
                    dp[i] = 0;
                }else{
                    dp[i] += i-1 == -1?0:dp[i-1];
                }
            }
        }

        return dp[rows-1];
    }
}
```







## \343. Integer Break

Given an integer `n`, break it into the sum of `k` **positive integers**, where `k >= 2`, and maximize the product of those integers.

Return *the maximum product you can get*.

**Example 1:**

```
Input: n = 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
```

**Example 2:**

```
Input: n = 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```



Intuition:

Use 2 loops, first is used to update the dp array, second is used to try to find the biggest product. Why the second loop just iterate until i/2? cause if j>i/2, we will do the same calculation. For example, i = 4, j = 1: we will need to calculate (1,3);j = 2:we will need to calculate (2,2), if j = 3, we need to figure out the (3,1), the result is same to (1,3). 

```java
class Solution {
    public int integerBreak(int n) {
        int [] dp = new int[n+1];
        dp[1] = 1;
        dp[2] = 1;
        
        for(int i = 3;i<n+1;i++){
            for(int j = 1;j<=i/2;j++){
                dp[i] = Math.max(dp[i],Math.max(j,dp[j])*Math.max(i-j,dp[i-j]));
            }
        }
        return dp[n];
    }
}
```



## \96. Unique Binary Search Trees

Given an integer `n`, return *the number of structurally unique **BST'**s (binary search trees) which has exactly* `n` *nodes of unique values from* `1` *to* `n`. 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
Input: n = 3
Output: 5
```



Intuition:

The problem mentioned that trees are BSTs, so we know every tree consists of root and left and right. So the quantity of how many BST's could express as root, it left tree's possibilities multiple right tree's possibilities, sum it all, we will get the answer.

 to construct a Binary Search Tree (BST), we enumerate every number in the sequence, use every number as the root. The number of BST that can be formed by each root node can be expressed as the number of left subtrees times the number of right subtrees.



```java
class Solution {
    public int numTrees(int n) {      
        int [] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        
        for(int i = 2;i<n+1;i++){
            int sum = 0;
            for(int j = 1;j<=i;j++){
                int left = j-1;
                int right = i-left-1;
                sum += dp[left] * dp[right];
            }
            dp[i] = sum;
        }
        
        return dp[n];
    }
}
```



## \416. Partition Equal Subset Sum

Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Example 1:**

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```



Intuition:

Use a 1 dimentional array to store the number, which describe the current target number and the practical reachable number, if these two number is equal, we can know that this array could be partitioned into two arrays, and the sum of both two arrays are equal.

```java
class Solution {
    public boolean canPartition(int[] num) {
        // calculate the sum/2
        int sum = 0;
        for(int i : num){
            sum+=i;
        }
        if(sum%2 == 1) return false;	//sum belongs to odd is not acceptable
        sum/=2;
        
        // create and initialize
        int [] dp = new int[sum+1];
        
        //update
        for(int i = 0;i<num.length;i++){	
            for(int j = sum;j>=num[i];j--){
                dp[j] = Math.max(dp[j],dp[j-num[i]]+num[i]);
            }
        }
        
        return dp[sum] == sum;
    }
}
```







## \518. Coin Change 2

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```





```java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int [coins.length+1][amount+1];
        for(int i =1;i<amount+1;i++){
            dp[0][i] = 0;
        }
        for(int i = 0;i<coins.length+1;i++){
            dp[i][0] = 1;
        }

        for(int m = 1;m<coins.length+1;m++){
            for(int n = 1;n<amount+1;n++){
                dp[m][n] = dp[m-1][n]+ (n>=coins[m-1]?dp[m][n-coins[m-1]]:0);
            }
        }
        return dp[coins.length][amount];
    }
}
```

Optimize the space complexity from O(m*n) to O(m)

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int [amount+1];
        dp[0] = 1;

        for(int m = 1;m<coins.length+1;m++){
            for(int n = 1;n<amount+1;n++){
                dp[n] += (n>=coins[m-1]?dp[n-coins[m-1]]:0);
            }
        }
        return dp[amount];
    }
}
```







## \377. Combination Sum IV

Given an array of **distinct** integers `nums` and a target integer `target`, return *the number of possible combinations that add up to* `target`.

The answer is **guaranteed** to fit in a **32-bit** integer.

**Example 1:**

```
Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```



Intuition:

it should be noticed that the sequence of loops, the first for loop is indicates the backpack capacity, and the inner loop is indicates the index of nums(iterate nums array). This sequence is fixed, why? Because the problem says (1,3) and (3,1) are the different situations, if we change the sequence of loop, we will set some situations(like (1,3) and (3,1)) as a same situation.



Let's take some examples:

The picture below indicates the true iterate sequence, red line means how algorithm works. we can see the outer loop is target, inner loop is nums. Cells in the table which contains ''+''  means it updated at current loop.

As we can see, in this traversal method, two cases (1,3) and (3,1) are counted respectively, that is what we need.

<img src="/Users/lizhihan/Downloads/IMG_57D755B6419C-1.jpeg" alt="IMG_57D755B6419C-1" style="zoom:50%;" />





This picture indicates the false iterate sequence, the outer loop is nums and the inner loop is target. let's take a look of the buttom row, when we arrive the 2+1 cell, we just hava (3),(1,2),(1,1,1), so we lose the element(2,1). This method is only suitable for statistical methods that do not care about the order of elementss, like Leetcode 518.

<img src="/Users/lizhihan/Downloads/IMG_21D68B01DDF2-1.jpeg" alt="IMG_21D68B01DDF2-1" style="zoom:50%;" />





```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int [] dp = new int [target+1];
        dp[0] = 1;  // initailize
        
        for(int i = 0;i<dp.length;i++){     // i indicates the backpack capacity
            for(int j = 0;j<nums.length;j++){   // j indicates index of nums
                if(i>=nums[j]){
                    dp[i] += dp[i-nums[j]];
                }
            }
        }
        return dp[target];
    }
}
```



## \70. Climbing Stairs

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```



```java
class Solution {
    public int climbStairs(int n) {
        int [] dp = new int [n+1];
        dp[0] = 1;
        
        for(int i = 0;i<dp.length;i++){
            for(int j = 1;j<=2;j++){
                if(i>=j){
                    dp[i] += dp[i-j];
                }
            }
        }
        
        return dp[n];
    }
}
```





## \322. Coin Change

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```



Hint:

1. we don't care about the sequence of combinations, so the sequence of 2 for loop are all acceptable.
2. the second loop is start from coins[i], because elements less than coins[i] will not update the dp array.
3. the if in the second loop means if we can not update current dp array, we skip it. Otherwise, the dp element will be updated by INTEGER.MIN_VALUE(max+1).
4. After the iteration the final element of dp array still never changed, we know that there is no conbination can build the amount, so we return -1.

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        dp[0] = 0;

        for(int i = 1; i<dp.length;i++){
            dp[i] = Integer.MAX_VALUE;
        }

        for(int i = 0;i<coins.length;i++){
            for(int j = coins[i];j<=dp.length-1;j++){   
                // j start from coins[i], only if j larger than coins[i], the dp array should be updated 
                if(dp[j-coins[i]]!=Integer.MAX_VALUE){
                    dp[j] = Math.min(dp[j],dp[j-coins[i]]+1);
                }
            }
        }

        return dp[amount] == Integer.MAX_VALUE?-1:dp[amount];
    }
}

```



## \198. House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```





It should be mentioned that the robber do not need to rob as many house as he can, he just need to rob the whole value is largest. So he could skip 2 houses without robbing, or 3 houses, 4houses.



The first idea is to check every situation, but the time complexity is O(n^2), space complexity is O(n).

```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 1) return nums[0];
        
        int [] dp = new int [nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0],nums[1]);
        
        for (int i = 2;i<nums.length;i++){
            for(int j = 2;i>=j;j++){	// skip 2houses or more
                dp[i] = Math.max(nums[i]+dp[i-j],dp[i]);
            }
        }
        
        return Math.max(dp[nums.length-1],dp[nums.length-2]);
        
    }
}
```



We can optimize it by updating with i-1 th element and i-2 th element, dp array update or not is depend skip current house or not, if we update using dp[i-1], this not mean we will rob the i-1 th house, it is possible to skip n-1 th house too. So this method avoid 2 loops, the time complexity is O(n), space complexity is O(n).

```java
class Solution {
	public int rob(int[] nums) {
		if (nums == null || nums.length == 0) return 0;
		if (nums.length == 1) return nums[0];

		int[] dp = new int[nums.length];
		dp[0] = nums[0];
		dp[1] = Math.max(dp[0], nums[1]);
		for (int i = 2; i < nums.length; i++) {
			dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
		}

		return dp[nums.length - 1];
	}
}
```



Can we continue to optimize? we notice that what parameter we need are just i-1 th element and i-2 th element. So instead of using an array, we use two variables to update the final result.

Time complexity O(n), Space complexity O(1)

```java
class Solution {
	public int rob(int[] nums) {
		if (nums == null || nums.length == 0) return 0;
		if (nums.length == 1) return nums[0];

		int a = nums[0];
		int b = Math.max(a, nums[1]);
		for (int i = 2; i < nums.length; i++) {
            int tmp = b;
			b = Math.max(b, a + nums[i]);
            a = tmp;
		}

		return b;
	}
}
```





## **\213. House Robber II**

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```



this question is similar to the 198, and we just need to consider whether we need to rob the final house or not. So we have 2 situations. For example, we have 5 houses, 1st situation is 1-4, 2nd is 2-5, we just need to calculate Max(1st,2nd)

```java
class Solution {
	public int rob(int[] nums) {
		if (nums == null || nums.length == 0) return 0;
		if (nums.length == 1) return nums[0];
        if (nums.length == 2) return Math.max(nums[0],nums[1]);

        int res1 = cal(nums,0,nums.length-2);
        int res2 = cal(nums,1,nums.length-1);
        return res1>res2?res1:res2;
	}
    
    int cal(int[] arr,int start,int end){
        int a = arr[start];
		int b = Math.max(a, arr[start+1]);
		for (int i = start+2; i <= end; i++) {
            int tmp = b;
			b = Math.max(b, a + arr[i]);
            a = tmp;
		}
        return b;
    }
}
```





## \337. House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`.

Besides the `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if **two directly-linked houses were broken into on the same night**.

Given the `root` of the binary tree, return *the maximum amount of money the thief can rob **without alerting the police***.

**Example 1:**

<img src="https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg" alt="img" style="zoom:50%;" />

```
Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```



for each tree root, there are two scenarios: it is robbed or is not.

Let's relate `rob(root)` to `rob(root.left)` and `rob(root.right)...`, etc. For the 1st element of `rob(root)`, we only need to sum up the larger elements of `rob(root.left)` and `rob(root.right)`, respectively, since `root` is not robbed and we are free to rob its left and right subtrees. For the 2nd element of `rob(root)`, however, we only need to add up the 1st elements of `rob(root.left)` and `rob(root.right)`, respectively, plus the value robbed from `root` itself, since in this case it's guaranteed that we cannot rob the nodes of `root.left` and `root.right`.

As you can see, by keeping track of the information of both scenarios, we decoupled the subproblems and the solution essentially boiled down to a greedy one. Here is the program:

```java
class Solution {
    public int rob(TreeNode root) {
        int[] res = sob_help(root);
        return Math.max(res[1],res[0]);
    }
    
    int[] sob_help(TreeNode root){
        int[] res = new int[2];
        if(root == null) return res;
        
        int[] left = sob_help(root.left);
        int[] right = sob_help(root.right);
        
        res[0] = Math.max(left[1],left[0])+Math.max(right[1],right[0]);
        res[1] = left[0]+right[0]+root.val;
        
        return res;
    }
}
```





## \121. Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```



Find the smallest element before current iteration number, and use current number subtract it. judge if this difference is the largest, iterate and update until get the end of the array.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0;
        int cur_min = Integer.MAX_VALUE;
        
        for(int i = 0;i<prices.length;i++){
            cur_min = prices[i]<cur_min?prices[i]:cur_min;
            max = max<(prices[i]-cur_min)?(prices[i]-cur_min):max;
        }
        
        return max;
    }
}
```





## \122. Best Time to Buy and Sell Stock II

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return *the **maximum** profit you can achieve*.

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```



Intuition:

Create a 2 dimentional array, dp\[n][0] descibe the current date we do not hold stocks, dp\[n][1] describe the current date we hold stocks.

we hold stocks may updated from 2 scenarios

1. last day we already have stocks, today we still hold it.
2. last day we do not hold the stocks, but we buy the today's stocks.

So our recursion formula is `dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]+prices[i-1]);`



we do not hold stocks also have 2 scenarios

1. last day we hold stocks, but we seld out today
2. last day we do not hold stocks and today we still do not buy stocks

So our recursion formula is `dp[i][1] = Math.max(dp[i-1][0]-prices[i-1],dp[i-1][1]);`

```java
class Solution {
    public int maxProfit(int[] prices) {
        int [][] dp = new int[prices.length+1][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        
        for(int i = 1;i<dp.length;i++){
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]+prices[i-1]);
            dp[i][1] = Math.max(dp[i-1][0]-prices[i-1],dp[i-1][1]);
        }
        return dp[prices.length][0];
    }
}
```

Of course we can use the scrolling array to reduce the space complexity.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int no = 0;
        int yes = -prices[0];
        
        for(int i = 1;i<prices.length+1;i++){
            int tmp = no;
            no = Math.max(no,yes+prices[i-1]);
            yes = Math.max(tmp-prices[i-1],yes);
        }
        return no;
    }
}
```

