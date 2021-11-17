# Arrays

## \704. Binary Search

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```



```java
class Solution {
    public int search(int[] nums, int target) {
        return searching(nums,target,0,nums.length-1);
    }
    
    public int searching(int[] nums, int target, int left,int right){
        if(left>right) return -1;
        
        int middle = (left+right)/2;
        if(nums[middle]<target){
            return searching(nums,target,middle+1,right);
        }else if(nums[middle]>target){
            return searching(nums,target,left,middle-1);
        }else{
            return middle;
        }
    }
}
```



```java
class Solution{
    public int search(int[] nums, int target){
        int left = 0;
        int right = nums.length-1;
        
        while(left<=right){
            int middle = left+(right-left)/2;
            if(nums[middle] == target){
                return middle;
            }else if(nums[middle]<target){
                left = middle+1;
            }else{
                right = middle-1;
            }
        }
        
        return -1;
    }
}
```





## \27. Remove Element

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The relative order of the elements may be changed.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

Do **not** allocate extra space for another array. You must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```



intuititon

use the double pointers approach, the slow pointer indicate the index which should be updated, the fast pointer iterate the whole array.

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int label = 0;
        for(int i = 0;i<nums.length;i++){
            if(nums[i] != val){
                nums[label] = nums[i];
                label++;
            }
        }
        return label;
    }
}
```





## \977. Squares of a Sorted Array

Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

**Example 1:**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```



Brute force?

At first, use a for loop change all negative numbers to positive. Then sort them, at last, calculate every number's Square.

Time complexity: O(nlogn), Space complexity: O(1)

```java
import java.util.Arrays;
class Solution {
    public int[] sortedSquares(int[] nums) {
        for(int i = 0; i<nums.length;i++){
            nums[i] = Math.abs(nums[i]);
        }
        Arrays.sort(nums);
        
        for(int i = 0;i<nums.length;i++){
            nums[i] = nums[i]*nums[i];
        }
        return nums;
    }
}
```



Use double pointers, new array could get the current highest number from the leftmost number in old array or the rightmost number in old array. So we just need to compare these two number's square, and locate the larger one at the correct location of new array, we can get the final array. 

This approach need a space complexity of O(n) ,but the time complexity is O(n).

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length-1;
        int [] res = new int[nums.length];
        int index = nums.length-1;
        
        for(;index>=0;index--){
            if(left == right){
                res[0] = nums[left];
            }
            if(Math.abs(nums[left])<=Math.abs(nums[right])){
                res[index] = nums[right] * nums[right];
                right--;
            }else{
                res[index] = nums[left] * nums[left];
                left++;
            }
        }
        
        return res;
    }
}
```







## \209. Minimum Size Subarray Sum

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a **contiguous subarray** `[numsl, numsl+1, ..., numsr-1, numsr]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead. 

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```



intuition

We could move the starting index of the current subarray as soon as we know that no better could be done with this index as the starting index. We could keep 2 pointer,one for the start and another for the end of the current subarray, and make optimal moves so as to keep the \text{sum}sum greater than s*s* as well as maintain the lowest size possible.

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int res = Integer.MAX_VALUE;
        int left = 0;
        int right = 0;
        int sum = 0;
        for(int i = 0; i<nums.length; i++){
            sum+=nums[i];   
            while(sum>=target){
                res = Math.min(res,i-left+1);
                sum-=nums[left];
                left++;   
            }
        }
        return res == Integer.MAX_VALUE?0:res;
    }
}
```

Time complexity: O(n), Each element can be visited atmost twice, once by the right pointer(i) and once by the left pointer.



## \59. Spiral Matrix **II**

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```



A circle traverser around the matrix, we call it layer, and we know that we have (n+1)/2 layers. 

In every layer, we divide the iteration to 4 parts. In the first part, the start = layer-1, and the end = length-1. By using the start and end, we could get the answer.

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        int tmp = 1;
        int layer = (n+1)/2;
        for(int i = 1;i<=layer;i++){
            for(int a = i-1;a<=n-i;a++){
                res[i-1][a] = tmp;
                tmp++;
            }
            for(int b = i;b<=n-i;b++){
                res[b][n-i] = tmp;
                tmp++;
            }
            for(int c = n-i-1;c>=i-1;c--){
                res[n-i][c] = tmp;
                tmp++;
            }
            for(int d = n-i-1;d>i-1;d--){
                res[d][i-1] = tmp;
                tmp++;
            }
        }
        return res;
    }
}
```

