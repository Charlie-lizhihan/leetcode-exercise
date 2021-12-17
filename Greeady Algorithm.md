# Greedy algorithm

## \455. Assign Cookies

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child `i` has a greed factor `g[i]`, which is the minimum size of a cookie that the child will be content with; and each cookie `j` has a size `s[j]`. If `s[j] >= g[i]`, we can assign the cookie `j` to the child `i`, and the child `i` will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Example 1:**

```
Input: g = [1,2,3], s = [1,1]
Output: 1
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
```



```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int i = 0;
        for(int j = 0;i<g.length && j<s.length;j++){
            if(g[i]<=s[j]) i++;
        }
        return i;
    }
}
```





## \376. Wiggle Subsequence

A **wiggle sequence** is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

- For example, `[1, 7, 4, 9, 2, 5]` is a **wiggle sequence** because the differences `(6, -3, 5, -7, 3)` alternate between positive and negative.
- In contrast, `[1, 4, 7, 2, 5]` and `[1, 7, 4, 5, 5]` are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.

A **subsequence** is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array `nums`, return *the length of the longest **wiggle subsequence** of* `nums`.

**Example 1:**

```
Input: nums = [1,7,4,9,2,5]
Output: 6
Explanation: The entire sequence is a wiggle sequence with differences (6, -3, 5, -7, 3).
```



At first, we need to calculate the gap between every two adjacent numbers, If two adjacent numbers are equal we skip them. If the difference is positive, we take 1, and if the difference is negative, we take -1.

In a DIff array, if adjacent elements have different signs, the result is incremented by one

```java
class Solution {
    public int wiggleMaxLength(int[] nums){
        ArrayList<Integer> diff = new ArrayList<>();
        for(int i = 0;i<nums.length-1;i++){
            if(nums[i+1]-nums[i] != 0){
                diff.add(nums[i+1]-nums[i]>0 ? 1 : -1);
            }
        }
        if(diff.size() == 0) return 1;
        
        int res = 1;
        for(int i = 1;i<diff.size(); i++){
            if(diff.get(i)+diff.get(i-1) == 0){
                res++;
            }
        }
        return res+1;
    }
}
```





## \53. Maximum Subarray

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return *its sum*.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```



Intuition: Iterate the whole array, if current sum is negative, we need to set it as 0. Because 0 must larger than negative number.

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int cur = 0;
        int res = Integer.MIN_VALUE;
        for(int i = 0;i<nums.length;i++){
            if(cur<=0){
                cur = 0;
            }
            cur+=nums[i];
            res = Math.max(cur,res);
        }
        return res;
    }
}
```



divide and conquer can also solve this problem. 

```java
class Solution {
    public int maxSubArray(int[] nums) {
        return helper(nums,0,nums.length-1);
    }
    
    public int helper(int nums[],int i,int j){

        if(i==j){
            return nums[i];
        }
                          
        int mid  =  (i+j)/2;
        int sum = 0,leftMax = Integer.MIN_VALUE;
        
        for(int l =  mid;l>=i;l--){
            sum+=nums[l];
            if(sum>leftMax){
                leftMax =  sum;
            }                                    
        }
        
        int rightMax = Integer.MIN_VALUE;
        sum = 0;    // reset sum to 0
        for (int l = mid + 1; l <=j; l++)
        {
            sum += nums[l];
            if (sum > rightMax) {
                rightMax = sum;
            }
        }
        
       int maxLeftRight = Math.max(helper(nums, i, mid), helper(nums, mid + 1, j ));
        return Math.max(maxLeftRight, leftMax + rightMax);
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



Using the greedy approach, we can get all local optimality, and then global optimality.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int cur = 0;
        int res = 0;
        for(int i = 1;i<prices.length;i++){
            cur = prices[i]-prices[i-1];
            if(cur>0){
                res+=cur;
            }
        }
        return res;
    }
}
```





## \55. Jump Game

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` *if you can reach the last index, or* `false` *otherwise*.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```



the point is that whether the range we can reach could cover the current index.

```java
class Solution{
    public boolean canJump(int[] nums) {       
        int max = 0;
        for(int i = 0;i<=nums.length-1;i++){
            if(i>max) return false;
            max = Math.max(max,i+nums[i]);
        }
        return true;
    }
}
```







## \45. Jump Game II

Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```



Greedy approach, `far` describe that in current range, which index we can reach farest. `cur` describes next index we need to record.

We need to judge if we can reach the end, if yes, we should break the loop.

```java
class Solution {
    public int jump(int[] nums) {
        if(nums.length == 1) return 0;
        
        int cur = 0;
        int far = 0;
        int jumps = 0;
        for(int i = 0;i<nums.length;i++){
            far = Math.max(far,i+nums[i]);
            if(far>=nums.length-1){
                jumps++;
                break;
            }
            if(i == cur){
                jumps++;
                cur = far;
            }
        }
        return jumps;
    }
}
```











## \1005. Maximize Sum Of Array After K Negations

Given an integer array `nums` and an integer `k`, modify the array in the following way:

- choose an index `i` and replace `nums[i]` with `-nums[i]`.

You should apply this process exactly `k` times. You may choose the same index `i` multiple times.

Return *the largest possible sum of the array after modifying it in this way*.

**Example 1:**

```
Input: nums = [4,2,3], k = 1
Output: 5
Explanation: Choose index 1 and nums becomes [4,-2,3].
```



Intuition:

1. try if we can put every negative number to positive
2. if yes, we still need to judge if the `k-neg.size` is odd or not, odd means we need to subtract a smallest number, then we can get the answer.

```java
class Solution{
    public int largestSumAfterKNegations(int[] nums, int k) {
        ArrayList<Integer> neg = new ArrayList<>();
        ArrayList<Integer> pos = new ArrayList<>();
        for(int i = 0;i<nums.length;i++){
            if(nums[i]<0){
                neg.add(nums[i]);
            } else{
                pos.add(nums[i]);
            }
        }

        Collections.sort(neg);
        Collections.sort(pos);

        int res = 0;
        if(k<=neg.size()){
            for(int j = 0;j<neg.size();j++){
                if(j<k) res-=neg.get(j);
                else res+=neg.get(j);
            }
            for (Integer po : pos) {
                res += po;
            }
            return res;
        }else{
            for (int num : nums) {
                res += Math.abs(num);
            }
            if((k-neg.size())%2 == 0){
                return res;
            }else{
                int min;
                if(neg.size() == 0){
                    min = pos.get(0);
                }else{
                    min = pos.size() == 0 ? -neg.get(neg.size()-1) : Math.min(pos.get(0),-neg.get(neg.size()-1));
                }
                return res-2*min;
            }
        }
    }
}
```





Of course we can simplify it.

sort it first, then we don't have to build 2 arrays.

```java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        Arrays.sort(nums);
        for(int i = 0;nums[i]<0 && k>0 && i<nums.length-1 ;i++,k--){
            nums[i] = -nums[i];
        }
        int res = 0;
        int min = Integer.MAX_VALUE;
        for(int i = 0;i<nums.length;i++){
            res+=nums[i];
            min = Math.min(min,nums[i]);
        }
        return res-(k%2)*2*min;
    }
}
```



## \134. Gas Station

There are `n` gas stations along a circular route, where the amount of gas at the `ith` station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return *the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return* `-1`. If there exists a solution, it is **guaranteed** to be **unique**

**Example 1:**

```
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```



We can calculate the sum of `gas[i]-cost[i]`, if this sum<0, we know the car can not start from the range what sum cover. 

For example, if we calculate 0,1,2 then we find the sum<0, we can not choose 0,1,2 as the start. Obviously we can not start from 0, and if we choose 1 as start, the start gas will not larger than if we start from 0. And we already know start from 0 is impossible. 

So, if the sum of range [i,j] less then 0, we will chosse j+1 as the start. if the latter part doesn't occur sum<0, we will know that j+1 is the start point.

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int len = gas.length;
        int gasSum = 0;
        int costSum = 0;
        
        for(int i = 0; i<len; i++){
            gasSum+=gas[i];
            costSum+=cost[i];
        }
        if(gasSum<costSum) return -1;
        
        int cur = 0;
        int start = 0;
        for(int j = 0; j<len; j++){
            cur += gas[j]-cost[j];
            if(cur<0){
                start = j+1;
                cur = 0;
            }
        }
        
        return start;
    }
}
```





## \135. Candy

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return *the minimum number of candies you need to have to distribute the candies to the children*.

**Example 1:**

```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```



The greedy algorithm's strategy is to update the array from back to front.

We can not consider 2 directions in one loop, so we consider left to right first, then consider about right to left.

```java
class Solution {
    public int candy(int[] ratings) {
        int len = ratings.length;
        int[] res = new int[len];
        Arrays.fill(res, 1);
        
        for(int i = 1; i<len; i++){
            if(ratings[i]>ratings[i-1]){
                res[i] = res[i-1]+1;
            }
        }
        
        for(int j = len-2; j>=0; j--){
            if(ratings[j]>ratings[j+1]){
                res[j] = Math.max(res[j],res[j+1]+1);
            }
        }
        
        int r = 0;
        for(int m = 0; m<len; m++){
            r += res[m];
        }
        return r;
    }
}
```





## \860. Lemonade Change

At a lemonade stand, each lemonade costs `$5`. Customers are standing in a queue to buy from you, and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a `$5`, `$10`, or `$20` bill. You must provide the correct change to each customer so that the net transaction is that the customer pays `$5`.

Note that you don't have any change in hand at first.

Given an integer array `bills` where `bills[i]` is the bill the `ith` customer pays, return `true` *if you can provide every customer with correct change, or* `false` *otherwise*.

**Example 1:**

```
Input: bills = [5,5,5,10,20]
Output: true
Explanation: 
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.
```



The greedy point is when we get a $20, we can give change of 10+5 or 5+5+5, but 5 could use for giving change for \$10 or \$20. So we choose to give change of 10+5 first.

```java
class Solution{
    public boolean lemonadeChange(int[] bills) {
        int map5 = 0;
        int map10 = 0;

        for (int i = 0; i < bills.length; i++) {
            if (bills[i] == 5) {
                map5++;
            } else if (bills[i] == 10) {
                map10++;
            }

            if (bills[i] == 10) {
                if (map5 <= 0) {
                    return false;
                } else {
                    map5--;
                }
            }

            if (bills[i] == 20) {
                if (map10>=1 && map5 >= 1) {
                    map10--;
                    map5--;
                } else if (map5 >= 3) {
                    map5 = map5-3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
}

```



## \406. Queue Reconstruction by Height

You are given an array of people, `people`, which are the attributes of some people in a queue (not necessarily in order). Each `people[i] = [hi, ki]` represents the `ith` person of height `hi` with **exactly** `ki` other people in front who have a height greater than or equal to `hi`.

Reconstruct and return *the queue that is represented by the input array* `people`. The returned queue should be formatted as an array `queue`, where `queue[j] = [hj, kj]` is the attributes of the `jth` person in the queue (`queue[0]` is the person at the front of the queue).

**Example 1:**

```
Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
Explanation:
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.
```



In this question, we need to consider how to arrange those 2 elements. Ie:h and k.

Since we can not set this 2 element at right location simultaneously, we choose to process the height first. Because if we order height from highest to lowest, and if height is the same, we order K from lowest to high. After this, we can select element from the sorted array one by one. In this case, insert will not bother the right sequence.

For example, after we sorted it the first time, we will get:

 [[7,0], [7,1], [6,1], [5,0], [5,2]，[4,4]]

The process of insert : 

- insert[7,0]：[[7,0]]
- inset[7,1]：[[7,0],[7,1]]
- insert[6,1]：[[7,0],[6,1],[7,1]]
- insert[5,0]：[[5,0],[7,0],[6,1],[7,1]]
- insert[5,2]：[[5,0],[7,0],[5,2],[6,1],[7,1]]
- insert[4,4]：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]

Everytime we want to insert, we just set the element at the k th location, this will not affect the right  sequence. Because the correctness of the queue is not affected by newly inserted elements.

```java
class Solution {
    public int[][] reconstructQueue(int[][] nums) {
        Arrays.sort(nums,(a,b)->{
            if(a[0] == b[0]){
                return a[1]-b[1];
            }
            return b[0]-a[0];
        });

        List<int[]> res = new ArrayList<>();
        for(int i = 0;i<nums.length;i++){
            res.add(nums[i][1],nums[i]);
        }
        return res.toArray(new int[0][0]);
    }
}
```





## \452. Minimum Number of Arrows to Burst Balloons

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [xstart, xend]` denotes a balloon whose **horizontal diameter** stretches between `xstart` and `xend`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up **directly vertically** (in the positive y-direction) from different points along the x-axis. A balloon with `xstart` and `xend` is **burst** by an arrow shot at `x` if `xstart <= x <= xend`. There is **no limit** to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return *the **minimum** number of arrows that must be shot to burst all balloons*.

**Example 1:**

```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
```



We need to make sure that every balloon is hit, so we sort the left edge of the balloon, and then judge it from left to right if the current end index could cover the next ballon's left edge. 

Here it should be noticed that current end index should be updated by `cur_end = Math.min(points[i][1],cur_end);` , this means the range we need to cover all balloons we already selected.

```java
class Solution  {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (o1, o2) -> Integer.compare(o1[0], o2[0]));
        int cur_end = points[0][1];
        int count = 1;
        for(int i = 1;i<points.length;i++){
            if(points[i][0] > cur_end){
                cur_end = points[i][1];
                count++;
            }else{
                cur_end = Math.min(points[i][1],cur_end);
            }
        }
        return count;
    }
}
```





## \435. Non-overlapping Intervals

Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping*.

**Example 1:**

```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```



Sort by the left margin of the array first. check current right margin, if there is a overlap occur. If no, use the next interval to update the cur_end, if yes, we should choose the min right margin among whole overlap intervals as the new cur_end.

```java
class Solution  {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals,(a,b)->{
            return a[0]-b[0];
        });

        int res = 0;
        int cur_end = intervals[0][1];
        for(int i = 1;i<intervals.length;i++){
            if(cur_end>intervals[i][0]){
                cur_end = Math.min(cur_end,intervals[i][1]);
                res++;
            }else{
                cur_end = intervals[i][1];
            }
        }
        return res;
    }
}
```





## \56. Merge Intervals

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```



This is going to be almost the same idea as the last problem. But we need to process the output.

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals,(a,b)->{
            return a[0]-b[0];
        });

        ArrayList<Integer> left = new ArrayList<>();
        ArrayList<Integer> right = new ArrayList<>();
        int count = 1;
        int cur_end = intervals[0][1];
        left.add(intervals[0][0]);
        right.add(intervals[0][1]);
        for(int i = 1;i< intervals.length;i++){
            if(cur_end<intervals[i][0]){
                count++;
                cur_end = intervals[i][1];
                left.add(intervals[i][0]);
                right.add(intervals[i][1]);
            }else{
                if(cur_end<intervals[i][1]){
                    cur_end = intervals[i][1];
                    right.remove(right.size()-1);
                    right.add(intervals[i][1]);
                }
            }
        }

        int [][] res  = new int [count][2];
        for(int i = 0;i<count;i++){
            res[i][0] = left.get(i);
            res[i][1] = right.get(i);
        }
        return res;
    }
}
```



simplify it, then will be...

```java
class Solution {
	public int[][] merge(int[][] intervals) {
		if (intervals.length <= 1)
			return intervals;

		// Sort by ascending starting point
		Arrays.sort(intervals, (i1, i2) -> Integer.compare(i1[0], i2[0]));

		List<int[]> result = new ArrayList<>();
		int[] newInterval = intervals[0];
		result.add(newInterval);
		for (int[] interval : intervals) {
			if (interval[0] <= newInterval[1]) // Overlapping intervals, move the end if needed
				newInterval[1] = Math.max(newInterval[1], interval[1]);
			else {                             // Disjoint intervals, add the new interval to the list
				newInterval = interval;
				result.add(newInterval);
			}
		}

		return result.toArray(new int[result.size()][]);
	}
}
```







## \763. Partition Labels

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Return *a list of integers representing the size of these parts*.

**Example 1:**

```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
```



1. We need to calculate the position of each letter closest to the right
2. iterate the String, keep updating the target, when target is reached, we get a partition

```java
class Solution{
    public List<Integer> partitionLabels(String s) {
        int [] label = new int[26];
        for(int i = 0; i<s.length(); i++){
            label[s.charAt(i)-'a'] = i;
        }

        int target = 0; // At least the position to which the condition is satisfied
        int cur = -1;   // record the start point
        ArrayList<Integer> res = new ArrayList<>();
        for(int i = 0; i<s.length(); i++){
            target = Math.max(target,label[s.charAt(i)-'a']);

            if(i == target){
                res.add(i-cur);
                cur = i;
                target = 0;
            }
        }
        return res;
    }
}
```





## \738. Monotone Increasing Digits

An integer has **monotone increasing digits** if and only if each pair of adjacent digits `x` and `y` satisfy `x <= y`.

Given an integer `n`, return *the largest number that is less than or equal to* `n` *with **monotone increasing digits***.

**Example 1:**

```
Input: n = 10
Output: 9
```

**Example 2:**

```
Input: n = 1234
Output: 1234
```



The case strNum[i - 1] > strNum[i] is encountered, let strNum[i - 1] --, then strNum[i] is given as 9, which ensures that these two become the largest monotonically increasing integer.

```java
class Solution {
    public int monotoneIncreasingDigits(int N) {
        char [] c = String.valueOf(N).toCharArray();
        int len = c.length;

        int index = len-1;
        for(int i = len-2;i>=0; i--){
            if(c[i+1]<c[i]){
                index = i;
                c[i]--;
            }
        }
        for(int i = index+1; i<len; i++){
            c[i] = '9';
        }

        return Integer.valueOf(String.valueOf(c));
    }
}
```





## \714. Best Time to Buy and Sell Stock with Transaction Fee

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```



There are 3 conditions we will face, so process them with different methods.

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int min = prices[0];
        int res = 0;
        for(int i : prices){
            if(i<min){  // situation 1: find the minist
                min = i;
            }
            
            if(i>=min && i<=min+fee) continue;  // situation 2: sell stocks means lose money, buy stock also means lose
            
            if(i>min+fee){  // situatoin3: sell could make profit
                res+=i-min-fee;
                min = i-fee;    // Prevent repeated calculations of transaction fee
            }   
        }
        return res;
    }
}
```



## \968. Binary Tree Cameras

You are given the `root` of a binary tree. We install cameras on the tree nodes where each camera at a node can monitor its parent, itself, and its immediate children.

Return *the minimum number of cameras needed to monitor all nodes of the tree*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png)

```
Input: root = [0,0,null,0,0]
Output: 1
Explanation: One camera is enough to monitor all nodes if placed as shown.
```



If we set a parent as camera, we will get more profit than leaf . So we use the postorder traversal, select as few nodes as possible as the camera

0: current node is not covered

2: current node is covered

1: current node is a camera 

We set the end condition of the sequence traversal as 2, because it will not affect the setting of the leaf node as 0. Only when the leaf node is set as 0, can the number of camera be minimum.



```java
class Solution {
    private int res = 0;
    public int minCameraCover(TreeNode root) {
        if(recur(root) == 0) res++;
        return res;
    }
    
    public int recur(TreeNode node){
        if(node == null) return 2;
        
        int left = recur(node.left);
        int right = recur(node.right);
      
      // three conditions we will process
        if(left == 2 && right == 2){
            return 0;
        }else if(left == 0 || right == 0){
            res++;
            return 1;
        }else if(left == 1 || right == 1){
            return 2;
        }
        return 0;
    }
}
```

