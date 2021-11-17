# 🔥Leetcode🔥 UK🇬🇧 



## \1. Two Sum

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.





an O(nlogn) method, first, use the sort algorithm ,make the original array to a ascending array. Then use the double pointer, left one point to 0, right one point to the end of the array, check wheather the sum of two numbers are target. If the sum is less than target, left++， otherwise, right--

```java
import java.util.Arrays;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int [2];
        if(nums.length == 0){return res;}
        
        int left = 0;
        int right = nums.length-1;
        
        int[] copy = new int[nums.length];
        for(int i = 0;i<nums.length;i++){
            copy[i] = nums[i];
        }
        Arrays.sort(copy);
        while(left<right && copy[left]+copy[right]!=target){
            if(copy[left]+copy[right] <target){
                left++;
            }else{
                right--;
            }
        }
        for(int i = 0;i<nums.length;i++){
            if(nums[i] == copy[left]){
                res[0] = i;
                break;
            }
        }
        
        for(int i = 0;i<nums.length;i++){
            if(nums[i] == copy[right] && i!=res[0]){
                res[1] = i;
                break;
            }
        }
        return res;
    }
}
```



O(n) method, use the HashMap , It only go through the list once

```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<Integer,Integer>();    
        for(int i = 0;i<nums.length;i++){
            if(map.containsKey(target-nums[i])){
                return new int[]{i,map.get(target-nums[i])};
            }
            map.put(nums[i],i);
        }
        return new int[]{0,0};
    }
}
```





## \2. Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.



Example

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```



take a look at the example, like usual, we start summing two numbers from the least digit number to the highest. So we build a loop to iterate whole l1 and l2, if the digit of l1 plus the digit of l2 plus up larger than 10, we put the variation up to 1, and put current digit to sum-10, iterate whole list, then solve the problem.

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode index = res;
        ListNode p1 = l1;
        ListNode p2 = l2;
        
        int up = 0;
        while(p1 != null || p2 != null || up != 0){
            int a = 0;
            int b = 0;
            if(p1 != null){
                a = p1.val;
            }
            if(p2!=null){
                b = p2.val;
            }
            int sum = (a+b+up)%10;
            up = (a+b+up)/10;
            
            ListNode tmp = new ListNode(sum);
            index.next = tmp;
            index = tmp;
            
            if(p1!=null){
                p1 = p1.next;
            }
            if(p2!=null){
                p2 = p2.next;
            }
        }
        
        return res.next;
       
    }
}
```





## \3. Longest Substring Without Repeating **Characters**

Given a string `s`, find the length of the **longest substring** without repeating characters.



at first, i use the HashMap to solve this, but i don't know how to efficiently update the HashMap, so i delete all elements in HashMap when i need to update the slide window. from the analytics, my method's time complexity is O(n^2), but in fact, it runs so slow(even slower than brute force).

```java
import java.util.*;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        int max = 0;
        
        HashMap<Character,Integer> house =  new HashMap<Character,Integer>();
        int p = 0;
        for(int j = 0;j<s.length();){

            int tmp = j;
            if(house.containsKey(s.charAt(j))){
                // find index of not repeating number's next one
                for(int k = 0;k<j;k++){
                    if(s.charAt(k) == s.charAt(j)){
                        tmp = k+1;
                    }
                }
                house.clear();
                p = 0;
                for(int m = tmp;m<=j;m++){
                    house.put(s.charAt(m),p);
                    p++;
                }
                j++;

            }else{
                house.put(s.charAt(j),p);
                if(house.size()>max){
                    max = house.size();
                }
                p++;
                j++;
            }


        }
        return max;
    }


}
```





use the slide window, but not use the hash map,  the neat thing about this method is that it use an array to store the element which is in the window. if a character appears more than once in a window, the array will exist an element = 2. through this method we could check if a string have repeating characters.

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] chars = new int[128];

        int left = 0;
        int right = 0;

        int res = 0;
        while (right < s.length()) {
            char r = s.charAt(right);
            chars[r]++;

            while (chars[r] > 1) {
                char l = s.charAt(left);
                chars[l]--;
                left++;
            }

            res = Math.max(res, right - left + 1);

            right++;
        }
        return res;
    }
}
```



besides, use the HashMap could reduce the complexity of time, but it needs O(n) space complexity, use the key-value to update the slide window, check if there is a repeating number in it. very clever solution!

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        
        return ans;
    }
}
```







## \4. Median of Two Sorted Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.



I can't reach the time complexity of O(log(m+n)), but I think this method is also cool. the time complexity is O((m+n)/2), space complexity is O(1). You can write it easily.

To calculate the median, we should find the middlest one or two elements(it depends on the whole array is odd or even). So there are two situations, first, whole array is odd, then we just need to find the `(nums1.length+nums2.length)/2`element, that is the median of two arrays. Second, whole array is even, we need to find the `(nums1.length+nums2.length)/2`,`(nums1.length+nums2.length)/2-1`,these two elements of array, sum it and divided by 2, then we got the answer.

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int index1 = 0,index2 = 0;
        int num1 = 0,num2 = 0;
        
        for(int i = 0;i<=(nums1.length+nums2.length)/2;i++){
            num1 = num2;
            if(index1>=nums1.length){	// Determine if it is in range
                num2 = nums2[index2];
                index2++;
            }else if(index2>=nums2.length){  // Determine if it is in range
                num2 = nums1[index1];
                index1++;
            }else if(nums1[index1]<nums2[index2]){  // compare
                num2 = nums1[index1];
                index1++;
            }else{
                num2 = nums2[index2];
                index2++;
            }
        }
        return (nums1.length+nums2.length)%2 == 0?(double)(num1+num2)/2:num2;
    }
}
```







## \5. Longest Palindromic Substring

Given a string `s`, return *the longest palindromic substring* in `s`.



The palindromic substring means a string is the same whether it is read forward or backward. So follow this definition, we can know that if we have a palindromic string which length is 3, we just need to check if the string which length = 4 and length = 5.



For friends who are confused about the key idea to check only new palindrome with length = current length +2 or +1, I add some more explanation here.

```java
Example: "xxxbcbxxxxxa", (x is random character, not all x are equal) now we 
          are dealing with the last character 'a'. The current longest palindrome
          is "bcb" with length 3.
1. check "xxxxa" so if it is palindrome we could get a new palindrome of length 5.
2. check "xxxa" so if it is palindrome we could get a new palindrome of length 4.
3. do NOT check "xxa" or any shorter string since the length of the new string is 
   no bigger than current longest length.
4. do NOT check "xxxxxa" or any longer string because if "xxxxxa" is palindrome 
   then "xxxx" got  from cutting off the head and tail is also palindrom. It has 
   length > 3 which is impossible.'
```

 

```java
public class Solution {
    public String longestPalindrome(String s) {
        char[] array = s.toCharArray();
        int len = 0;
        int start = 0;
        for(int i = 0;i<array.length;i++){
            if(isPalindrome(array,i-len-1,i)){
                start = i-len-1;
                len+=2;
            }else if(isPalindrome(array,i-len,i)){
                start = i-len;
                len+=1;
            }
        }
        return new String(array,start,len);

    }
    private boolean isPalindrome(char[] array, int start, int end) {
        if(start<0){
            return false;
        }

        while(start<end){
            if(array[start++]!=array[end--]){
                return false;
            }
        }
        return true;
    }
}
```





## \6. ZigZag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows); 
```

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```





Using stringBuffer, you can pass in a number n and generate n lines of string.

Rows are traversed in order 1-2-3-4-3-2-1-2-3-4-....，so we can use this sequence, insert sequently every element to every row.

```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1){return s;}
        StringBuilder[] sb = new StringBuilder[numRows];
        
        for(int i = 0; i<numRows;i++){
            sb[i] = new StringBuilder("");
        }
            
        int row = 0;
        int cur = 1;
        char [] tmp = s.toCharArray();
        for (int j = 0; j<s.length();j++){
            sb[row].append(tmp[j]);
            row+=cur;
            if(row == 0 && cur == -1){
                cur = 1;
            }else if(row == numRows-1 && cur == 1){
                cur = -1;
            }   
        }
        String res = "";
        for (int k = 0;k<numRows;k++){
            res+=sb[k];
        }
        return res;    
    }
}
```



## \7. Reverse Integer

Given a signed 32-bit integer `x`, return `x` *with its digits reversed*. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-231, 231 - 1]`, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).** 

**Example 1:**

```
Input: x = 123
Output: 321
```



Converts a string to an char array. After flipping the array, converts it to an int
We use try and catch to see if we're outside the scope of an int. the time complexity is O(n) and the space complexity is O(n).

```java
class Solution {
    public int reverse(int x) {
        if(x == 0)return 0;
        
        String x_ = String.valueOf(x);
        char [] tmp = x_.toCharArray();
        
        Boolean flag = true;
        int left = 0;
        int right = tmp.length-1;

        if(tmp[0] == '-')left++;
        while(tmp[right] == '0'){
            right--;
        }
        int res_right = right;
        
        while(left<=right){
            char t = tmp[left];
            tmp[left] = tmp[right];
            tmp[right] = t;
            left++;
            right--;
        }
        if(flag == true){
            try {
                String result = String.valueOf(String.valueOf(Arrays.copyOfRange(tmp,0,res_right+1)));
                return Integer.parseInt(result);
            } catch(Exception e) {
                return 0;
            }
        }else{
            try {
                String result = String.valueOf(String.valueOf(Arrays.copyOfRange(tmp,1,res_right+1)));
                return -1 * Integer.parseInt(result);
            } catch(Exception e) {
                return 0;
            }
        }
    }
}
```





An excellent solution, the boundary condition problem is solved ingeniously. We know the int_max = 2147483647, 

```java
class Solution {
    public int reverse(int x) {  

        int ans = 0;
        while (x != 0)
        {
            if (ans > 214748364 || ans < -214748364)
            {
                return 0;
            }
            
            ans = ans * 10 + x % 10;
            x /= 10;
        }  
        return ans;
    }
}
```









## \8. String to Integer (atoi)

Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

The algorithm for `myAtoi(string s)` is as follows:

1. Read in and ignore any leading whitespace.
2. Check if the next character (if not already at the end of the string) is `'-'` or `'+'`. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
3. Read in next the characters until the next non-digit charcter or the end of the input is reached. The rest of the string is ignored.
4. Convert these digits into an integer (i.e. `"123" -> 123`, `"0032" -> 32`). If no digits were read, then the integer is `0`. Change the sign as necessary (from step 2).
5. If the integer is out of the 32-bit signed integer range `[-231, 231 - 1]`, then clamp the integer so that it remains in the range. Specifically, integers less than `-231` should be clamped to `-231`, and integers greater than `231 - 1` should be clamped to `231 - 1`.
6. Return the integer as the final result.

**Note:**

- Only the space character `' '` is considered a whitespace character.
- **Do not ignore** any characters other than the leading whitespace or the rest of the string after the digits.





Summarize the topic requirement, we just should finish 4 steps.

1. check if it's a null input
2. Check the leading whitespace
3. Check positive or negative.
4. check the numbers

We should ignore the whole leading whitespaces, check only one symbol of number and so many numbers, and if there are still some elements after the numbers, we will ignore them. After understanding what we will gonna do, solution will be simple.

```java
class Solution {
    public int myAtoi(String s) {
        //step1.check null input
        if(s.length() == 0)return 0;
        
        int res = 0;
        int index = 0;
        int flag = 1;
        
        // step2.check the space
        // we do not use the code below, cause if index is already equal to s.length, 
        // try to access s.charAt(index) will lead to error,if we reverse these two judgement, 
        // when index is equal to s.length, because of the characteristic of &&, we will not run s.charAt(index) == ' '
        //
        //
        // while(s.charAt(index) == ' ' && index<s.length()){
        //     index++;
        // }
        
        while(index < s.length() && s.charAt(index) == ' ')
            index++;
        if(index == s.length()){return 0;}

        //step3.check if it's negative
        if(s.charAt(index) == '+' || s.charAt(index) == '-'){
            if(s.charAt(index) == '-'){
                flag = -1;
            }
            index++;
        }
        
        //step4.check numbers
        while(index<s.length()){
            if(checkDigit(s.charAt(index)) != true){
                    break;
            }
            
            int cur = s.charAt(index)-'0';
            //check if it's out of bound
            if(res>Integer.MAX_VALUE/10 || res == Integer.MAX_VALUE/10 && cur>7){
                return flag == 1?Integer.MAX_VALUE:Integer.MIN_VALUE;
            }
            res = res*10+cur;
            index++;
        }
        return res*flag;
    }




    public Boolean checkDigit(char a){
        if(a >= '0' && a<='9'){
            return true;
        }else{return false;}
    }
}
```









## \9. Palindrome Number

Given an integer `x`, return `true` if `x` is palindrome integer.

An integer is a **palindrome** when it reads the same backward as forward. For example, `121` is palindrome while `123` is not.

**Example 1:**

```
Input: x = 121
Output: true
```





We should notice that if we reverse the int, it may lead to overflow. So we can just reverse half of this integer or use some other methods to avoid overflow.

Here are 2 interesting methods.



- Don't care about the overflow, because if a integer larger than the MAX_INT, it will become negative, this situation will cause return false, it suit our need.

```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0)return false;
        int y = x;
        int tmp = 0;
        while(y!=0){
            tmp = tmp*10+y%10;
            y/=10;
        }
        return tmp == x;
    }
}
```

- Set the limit of p, and do not let the q reach the MAX_INT

```java
class Solution {
public boolean isPalindrome(int x) {
    
    if (x < 0) return false;

    int p = x; 
    int q = 0; 
    
    while (p >= 10){
        q *=10; 
        q += p%10; 
        p /=10; 
    }
    
    return q == x / 10 && p == x % 10;
}
}
```



# Biweekly Contest

## \2053. Kth Distinct String in an Array

A **distinct string** is a string that is present only **once** in an array.

Given an array of strings `arr`, and an integer `k`, return *the* `kth` ***distinct string** present in* `arr`. If there are **fewer** than `k` distinct strings, return *an **empty string*** `""`.

Note that the strings are considered in the **order in which they appear** in the array.

**Example 1:**

```
Input: arr = ["d","b","c","b","c","a"], k = 2
Output: "a"
Explanation:
The only distinct strings in arr are "d" and "a".
"d" appears 1st, so it is the 1st distinct string.
"a" appears 2nd, so it is the 2nd distinct string.
Since k == 2, "a" is returned. 
```



It worth noting that we can use getOrDefault function to conveniencely increase the number of Map.

```java
import java.util.*;

class Solution {
    public String kthDistinct(String[] arr, int k) {
        HashMap<String,Integer> a = new HashMap<String,Integer>();
        for(int i = 0;i<arr.length;i++){
            a.put(arr[i],a.getOrDefault(arr[i],0)+1);
        }

        int symbol = 0;	//use symbol to record if given k larger than the number of distinct String
        String res = "";
        for(String i : arr){
            if(a.get(i)==1 && symbol != k){
                res = i;
                symbol++;
            }
        }
        return symbol == k?res:"";
    }
}
```





## \2055. Plates Between Candles

There is a long table with a line of plates and candles arranged on top of it. You are given a **0-indexed** string `s` consisting of characters `'*'` and `'|'` only, where a `'*'` represents a **plate** and a `'|'` represents a **candle**.

You are also given a **0-indexed** 2D integer array `queries` where `queries[i] = [lefti, righti]` denotes the **substring** `s[lefti...righti]` (**inclusive**). For each query, you need to find the **number** of plates **between candles** that are **in the substring**. A plate is considered **between candles** if there is at least one candle to its left **and** at least one candle to its right **in the substring**.

- For example, `s = "||**||**|*"`, and a query `[3, 8]` denotes the substring `"*||***\***|"`. The number of plates between candles in this substring is `2`, as each of the two plates has at least one candle **in the substring** to its left **and** right.

Return *an integer array* `answer` *where* `answer[i]` *is the answer to the* `ith` *query*.

**Example 1:**

![ex-1](https://assets.leetcode.com/uploads/2021/10/04/ex-1.png)

```
Input: s = "**|**|***|", queries = [[2,5],[5,9]]
Output: [2,3]
Explanation:
- queries[0] has two plates between candles.
- queries[1] has three plates between candles.
```







brute force

create a function to iterate every substring. this method need many repeated calculations.

```java
class Solution {
    public int[] platesBetweenCandles(String s, int[][] queries) {
        int [] res = new int [queries.length];
        for(int i = 0;i<queries.length;i++){
            res[i] = cal(s.substring(queries[i][0],queries[i][1]+1));
        }
        return res;
    }
    
    public static int cal(String in){
        char[] inn = in.toCharArray();
        int label = 0;
        int start = 0;
        int end = 0;
        int res = 0;

        for(int i = 0;i<inn.length;i++){
            if(label == 0 && inn[i] == '|'){
                start = i;
                label++;
            }
            if(inn[i] == '|'){
                end = i;
            }
        }

        for(int j = start+1;j<end;j++){
            if(inn[j] == '*'){
                res++;
            }
        }
        return res;

    }
}
```





Intuition

Iterate through the string only once, creating a sum array to store the number of * in each position.

Then create a left array and right array respectively store the label. The left label and right label are from far left to far right and from far right to far left, indicate the true index we will need when we calculate the sum

```java
class Solution {
    public static int[] platesBetweenCandles(String s, int[][] queries){
        // create sum array, whatever where '|' is
        char[] ss = s.toCharArray();
        int [] sum = new int [ss.length];
        if(ss[0] == '*') sum[0] = 1;
        for(int i = 1;i<ss.length;i++){
            if(ss[i] == '*'){
                sum[i] = sum[i-1]+1;
            }else{
                sum[i] = sum[i-1];
            }
        }

        // create left array and right array to help us determine there is the true position of left and right
        int[] left = new int [s.length()];
        int[] right = new int [s.length()];
        if(ss[0] == '|'){
            left[0] = 0;
        }else{
            left[0] = -1;
        }
        for(int i = 1;i<ss.length;i++){
            if(ss[i] == '|'){
                left[i] = i;
            }else{
                left[i] = left[i-1];
            }
        }
        if(ss[ss.length-1] == '|'){
            right[ss.length-1] = ss.length-1;
        }else{
            right[ss.length-1] = ss.length;
        }
        for(int j = s.length()-2;j>=0;j--){
            if(ss[j] == '|'){
                right[j] = j;
            }else{
                right[j] = right[j+1];
            }
        }

        // create final result
        int[] res = new int[queries.length];
        for(int a = 0;a<res.length;a++){
            int r = left[queries[a][1]];
            int l = right[queries[a][0]];

            if(l>=r || l == -1 || r == ss.length){
                res[a] = 0;
            }else {
                res[a] = sum[r] - sum[l];
            }
        }
        return res;
    }
}

```



```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        dfs(root,"",res);
        return res;
    }
    

    public void dfs(TreeNode root, String cur, List<String> res){
        if(root == null) return ;
        if(root.left == null&& root.right == null){
            res.add(cur+root.val);
        }
        
        dfs(root.left,cur+root.val+"->",res);
        dfs(root.right,cur+root.val+"->",res);
    }

}


```







## \2069. Walking Robot Simulation II

The neat point of this problem is that we need to set the 0,0 location's direction is east. otherwise we can not get the accepted.

besides, note that the robot want to walk like a circle, but we could process it by change the circle to a line. Then use the % operation to get the current location and current direction. This could reduce the code complexity.



```java
class Robot {
    int cur;
    int b,c,d,a;
    int X,Y;

    public Robot(int width, int height) {
        X = width-1;
        Y = height-1;
        b = X;
        c = X+Y;
        d = X+X+Y;
        a = X+X+Y+Y;
        cur = 0;
    }

    public void move(int num) {
        cur+=num;
    }

    public int[] getPos() {
        int yu = cur%a;
        if(yu>=0 && yu<=b){
            return new int[] {yu,0};
        }else if(yu>b && yu<=c){
            return new int[] {X,yu-b};
        }else if(yu>c && yu<=d){
            return new int[] {X-(yu-c),Y};
        }else{
            return new int[] {0,a-yu};
        }
    }

    public String getDir() {
        
        if(cur == 0) return "East";
        int yu = cur%a;
        
        
        if(yu>0 && yu<=b){
            return "East";
        }else if(yu>b && yu<=c){
            return "North";
        }else if(yu>c && yu<=d){
            return "West";
        }else{
            return "South";
        }

    }
}
```



## \2074. Reverse Nodes in Even Length Groups

You are given the `head` of a linked list.

The nodes in the linked list are **sequentially** assigned to **non-empty** groups whose lengths form the sequence of the natural numbers (`1, 2, 3, 4, ...`). The **length** of a group is the number of nodes assigned to it. In other words,

- The `1st` node is assigned to the first group.
- The `2nd` and the `3rd` nodes are assigned to the second group.
- The `4th`, `5th`, and `6th` nodes are assigned to the third group, and so on.

Note that the length of the last group may be less than or equal to `1 + the length of the second to last group`.

**Reverse** the nodes in each group with an **even** length, and return *the* `head` *of the modified linked list*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/10/25/eg1.png)

```
Input: head = [5,2,6,3,9,1,7,3,8,4]
Output: [5,6,2,3,9,1,4,8,3,7]
Explanation:
- The length of the first group is 1, which is odd, hence no reversal occurrs.
- The length of the second group is 2, which is even, hence the nodes are reversed.
- The length of the third group is 3, which is odd, hence no reversal occurrs.
- The length of the last group is 4, which is even, hence the nodes are reversed.
```





We can use 2 pointers approach. One used to iteratre the list, another one used to update the ndoes.

Since we need to reverse part of the linkedlist, we can use a stack to store the numbers we need to reverse(cause stack is FILO), then use the help pointer(another pointer) to update the original list.

Note that the last group also could be even numbers, though it's an odd number group it contains an even number of numbers. This scenario we still need to reverse it.

```java
class Solution {
    public ListNode reverseEvenLengthGroups(ListNode head) {
        int current_level = 1;
        ListNode tmp = head;
        while(tmp!=null){
            ListNode help = tmp;
            Stack<Integer> s = new Stack<>();
            int count = 0;
            while(count<current_level && tmp!=null){
                s.push(tmp.val);
                count++;
                tmp = tmp.next;
            }    
            
            if(count%2 == 0){
                while(count!=0){
                    help.val = s.pop();
                    help = help.next;
                    count--;
                }
            }
            current_level++;
        }
        return head;
    }
}
```





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



# LinkedList

## \203. Remove Linked List Elements

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return *the new head*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```





The first head node could be null or equal to val, so we create a preFirst node pointing to head, then some special situation could be handled easily.

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
    public ListNode removeElements(ListNode head, int val) {
        ListNode preFirst = new ListNode(0);
        ListNode pre = preFirst;
        ListNode cur = head;
        pre.next = cur;
        
        while(cur!=null){
            if(cur.val == val){
                pre.next = cur.next; 
                cur = pre.next;
            }else{
                cur = cur.next;
                pre = pre.next;
            }
        }
        return preFirst.next;
    }
}
```





## \707. Design Linked List

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are **0-indexed**.

Implement the `MyLinkedList` class:

- `MyLinkedList()` Initializes the `MyLinkedList` object.
- `int get(int index)` Get the value of the `indexth` node in the linked list. If the index is invalid, return `-1`.
- `void addAtHead(int val)` Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
- `void addAtTail(int val)` Append a node of value `val` as the last element of the linked list.
- `void addAtIndex(int index, int val)` Add a node of value `val` before the `indexth` node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node **will not be inserted**.
- `void deleteAtIndex(int index)` Delete the `indexth` node in the linked list, if the index is valid.

**Example 1:**

```
Input
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
Output
[null, null, null, null, 2, null, 3]

Explanation
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
myLinkedList.get(1);              // return 2
myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
myLinkedList.get(1);              // return 3
```



```java
class MyLinkedList {
    int size;
    TreeNode head;

    public MyLinkedList() {
        size = 0;
        head = new TreeNode(0);
    }
    
    public int get(int index) {
        if(index>=size) return -1;
        
        TreeNode tmp = head;
        for (int i = 0; i <= index; i++){
            tmp = tmp.next;
        }
        return tmp.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0,val);
        
    }
    
    public void addAtTail(int val) {
        addAtIndex(size,val);
    }
    
    public void addAtIndex(int index, int val) {
        if(index>size) return;
        
        size++;
        TreeNode n = new TreeNode(val);
        TreeNode tmp = head;
        for (int i = 0; i < index; i++) {
            tmp = tmp.next;
        }
        
        n.next = tmp.next;
        tmp.next = n;a
    }
    
    public void deleteAtIndex(int index) {
        if(index>size-1) return;
        
        size--;
        TreeNode tmp = head;
        while(index>0){
            tmp = tmp.next;
            index--;
        }
        tmp.next = tmp.next.next;
    }
}

class TreeNode{
    int val;
    TreeNode next;
    TreeNode(){};
    TreeNode(int val){
        this.val = val;
    }
}



/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```







## \206. Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```





double pointer:

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null) return null;
        ListNode reverse = new ListNode(0);
        ListNode tmp = head;
        ListNode cur = head.next;
        while(tmp!=null){
            cur = tmp.next;
            tmp.next = reverse.next;
            reverse.next = tmp;
            tmp = cur;
        }
        return reverse.next;
    }
}
```



Recursive:

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode res = reverse(null,head);
        return res;
    }
    
    public ListNode reverse(ListNode pre,ListNode cur){
        if(cur == null){
            return pre;
        }
        ListNode tmp = cur.next;
        cur.next = pre;
        return reverse(cur,tmp);
    }
}
```





## \24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.) 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```



The only thing worth to mention is that do not let the cur reach the null.next, it will lead to NullPointerException

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;   
        
        ListNode cur = head.next;
        ListNode pre = head;
        while(cur!=null && pre!=null){
            int tmp = cur.val;
            cur.val = pre.val;
            pre.val = tmp;
            if(cur.next != null){
                cur = cur.next.next;
                pre = pre.next.next;
            }else{
                cur = cur.next;
            }
        }
        return head;
    }
}
```





## \19. Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```



We can calculate the length of linked list, use the n-length to get the position of the previous node of the node to be deleted. 

Time complexity: O(n)

Space complexity: O(1)

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // calculate the length of LinkedList
        ListNode tmp = head;
        int count = 0;
        while(tmp != null){
            count++;
            tmp = tmp.next;
        }
        if(count == n) return head.next;    // otherwise, when we run the tmp.next = tmp.next.next; it will throw exception
        
        //get the pointer point to the node before the node which need to be deleted
        tmp = head;
        for(int i = 0;i<count-n-1;i++){
            tmp = tmp.next;
        }
        
        tmp.next = tmp.next.next;
        return head;
    }
}
```



Also, we can use double pointer, their gap is n, and when the fast pointer reach the end of linkedlist, the slow will reach the position of previous node of the node to be deleted.

Time complexity: O(n)

Space complexity: O(1)

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fast = head;
        ListNode slow = head;
        
        for(int i = 0;i<n;i++){
            fast = fast.next;
        }
        if(fast == null) return head.next;
        
        while(fast.next!=null){
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```



## \142. Linked List Cycle II

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```



Create 2 pointers, slow and fast. Both start at head node, fast is twice as fast as slow. If fast pointer reaches the end means there is no cycle, otherwise it will eventually catch up to slow pointer somewhere in the cycle.

The slow pointer take A+B steps, A indicates the distance between head and entry of cycle. N indicates the length of cycle. So we get `A+B+N=2A+2B`, then we know if we create a new pointer at head node, move the new pointer and the slow pointer, when they meet each other, the node they point is the entry of cycle.

```java
import java.util.*;
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null) return null;
        if(head.next.next == head) return head;
        if(head.next.next == null) return null; 
        
        ListNode s = head;
        ListNode l = head;
        int count = 0;
        while(l.next!=null && l.next.next!=null){
            s = s.next;
            l = l.next.next;
            count++;
            if(s == l){
                ListNode h = head;
                while(h!=s){
                    s = s.next;
                    h = h.next;
                }
                return s;
            } 
        }
        return null;
    }
}
```











## [面试题 02.07. Intersection of Two Linked Lists](https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/)

Given two (singly) linked lists, determine if the two lists intersect. Return the inter­ secting node. Note that the intersection is defined based on reference, not value. That is, if the kth node of the first linked list is the exact same node (by reference) as the jth node of the second linked list, then they are intersecting.

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png" alt="img" style="zoom:67%;" />



 **Example 1:**

<img src="https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png" alt="img" style="zoom:67%;" />

Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
Output: Reference of the node with value = 8
Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,0,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.





This is a similar idea to the last problem, first we calculate the difference of two given list's length. Then move the longer list, let two lists could move synchronously, if they reach the same node(the same node or the end), we get the intersaction node.

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) return null;

        ListNode tmp1 = headA;
        ListNode tmp2 = headB;

        int count1 = 0;
        while(tmp1!=null){
            tmp1 = tmp1.next;
            count1++;
        }
        int count2 = 0;
        while(tmp2!=null){
            tmp2 = tmp2.next;
            count2++;
        }

        tmp1 = headA;
        tmp2 = headB;
        for(int i = 0;i<Math.abs(count2-count1);i++){
            if(count1>count2){
                tmp1 = tmp1.next;
            }else{
                tmp2 = tmp2.next;
            }
        }

        while(tmp1 != tmp2){
            tmp1 = tmp1.next;
            tmp2 = tmp2.next;
        }

        if(tmp1 == null) return null;
        return tmp1;
    }
}
```



We just simply use two temprory pointer, one start from A, the other start from B. 

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png" alt="img" style="zoom:67%;" />

take this picture as an example, the length of A is 5, the length of B is 6. and the length of front part of A is a1,a2 = 2, in a similar way the length of front part of B is 3.

then we get 5+3 = 6+2, so we can iterate whole A list, and iterate the front part of B. Meanwhile we iterate B list and the front part of A. Finally, these two iteration will intersact at the same node(or null), this node is what we need.

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode tmpA = headA;
        ListNode tmpB = headB;

        while(tmpA!=tmpB){
            tmpA = tmpA == null?headB:tmpA.next;
            tmpB = tmpB == null?headA:tmpB.next;
        }

        return tmpA;
    }
}
```





# HashMap

## \1002. Find Common Characters

Given a string array `words`, return *an array of all characters that show up in all strings within the* `words` *(including duplicates)*. You may return the answer in **any order**.

**Example 1:**

```
Input: words = ["bella","label","roller"]
Output: ["e","l","l"]
```



Notice that we can use an array of length of 26th to indicate what character we already have. The index of this array could described as `char-'a'`.

```java
class Solution {
    public List<String> commonChars(String[] words) {
        // final statistical result
        int[] stan = new int[26];
        // initialize
        for(int i = 0;i<words[0].length();i++){
            stan[words[0].charAt(i)-'a']++;
        }
        
        //iterate words array and update stan array
        for(int j = 1;j<words.length;j++){
            int[] cur = new int[26];
            for(int k = 0;k<words[j].length();k++){
                cur[words[j].charAt(k)-'a']++;    
            }
            
            // update
            for(int a = 0; a<26;a++){
                stan[a] = cur[a]<stan[a]?cur[a]:stan[a];
            }
        }
        
        // transform to the form we need
        List<String> res = new ArrayList<String>();
        for(int q = 0;q<26;q++){
            for(int p = 0;p<stan[q];p++){
                res.add(String.valueOf((char)('a'+q)));
            }
        }
        return res;
    }
}
```





## \242. Valid Anagram

Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```



This question is just a application of 26th length array, we just need to check after the comparison if the final array all 0;

```java
import java.util.*;

class Solution {
    public boolean isAnagram(String s, String t) {
        int[] alpha = new int[26];
        for(int i = 0;i<s.length();i++){
            alpha[s.charAt(i)-'a']++;
        }
        for(int j = 0;j<t.length();j++){
            alpha[t.charAt(j)-'a']--;
            // use this if could sometimes avoid the 3rd for loop
            if(alpha[t.charAt(j)-'a']<0){
                return false;
            }
        }
        for(int k = 0;k<alpha.length;k++){
            if(alpha[k]!=0){
                return false;
            }
        }
        return true;
    }
}
```



## \349. Intersection of Two Arrays

Given two integer arrays `nums1` and `nums2`, return *an array of their intersection*. Each element in the result must be **unique** and you may return the result in **any order**.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```



This problem is mainly about the application of the set feature. We should care that add same element into a set will not have any effect. Cause set can not have same element.

```java
import java.util.*;
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> store = new HashSet<Integer>();
        HashSet<Integer> res = new HashSet<Integer>();
        
        for(int i = 0;i<nums1.length;i++){
            store.add(nums1[i]);
        }
        for(int j = 0;j<nums2.length;j++){
            if(store.contains(nums2[j])){
                res.add(nums2[j]);
            }
        }
        
        ArrayList<Integer> r = new ArrayList<Integer>();
        for (Integer i : res) {	// 迭代输出
            r.add(i);
        }
        int[] re = new int [r.size()];
        for(int i = 0;i<re.length;i++){
            re[i] = r.get(i);
        }
        return re;
                
    }
}
```



if the given 2 arrays are sorted, we can use different approach.

We still could use set, but just use it to record the intersection.

```java
import java.util.*;
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        
        
        Set<Integer> res = new HashSet<Integer>();
        int length1 = nums1.length;
        int length2 = nums2.length;
        while(length1 != 0 && length2 != 0){
            if(nums1[length1-1]<nums2[length2-1]){
                length2--;
            }else if(nums1[length1-1]>nums2[length2-1]){
                length1--;
            }else{
                res.add(nums1[length1-1]);
                length1--;
                length2--;
            }
        }
        
        ArrayList<Integer> r = new ArrayList<Integer>();
        for (Integer i : res) {	// 迭代输出
            r.add(i);
        }
        int[] re = new int [r.size()];
        for(int i = 0;i<re.length;i++){
            re[i] = r.get(i);
        }
        return re;
    }
}
```







## \454. 4Sum II

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

**Example 1:**

```
Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
Output: 2
Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```





Create 2 HashMap, one store the sum of nums1 array and nums2 array, the other store the sum of nums3 array and nums4 array. It worth mention that HashMap 's key should be the sum, and the value is numbers how many this sum occurr.

After creating, iterate 2 HashMaps, if two numbers could sum as 0, this two number's occurance number's product should be added to res.

Time complexity: O(n.length^2)

Space complexity: O(n.length^2) (wothest situation: all numbers in this hash map is different)

```java
import java.util.*;
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        HashMap<Integer,Integer> l1 = new HashMap<Integer,Integer>();
        HashMap<Integer,Integer> l2 = new HashMap<Integer,Integer>();
        for(int i = 0;i<nums1.length;i++){
            for(int j = 0;j<nums2.length;j++){
                int tmp1 = nums1[i]+nums2[j];
                if(l1.containsKey(tmp1)){
                    l1.put(tmp1,l1.get(tmp1)+1);
                }else{
                    l1.put(tmp1,1);
                }
            }
        }
        
        for(int i = 0;i<nums3.length;i++){
            for(int j = 0;j<nums4.length;j++){
                int tmp2 = nums3[i]+nums4[j];
                if(l2.containsKey(tmp2)){
                    l2.put(tmp2,l2.get(tmp2)+1);
                }else{
                    l2.put(tmp2,1);
                }
            }
        }
        
        int res = 0;
        for(int i : l1.keySet()){
            if(l2.containsKey(0-i)) {
                res+=l2.get(0-i)*l1.get(i);
            }
        }
        return res; 
    }
}
```



We also can just use 1 hash map and use the getOrDefault function to simplify the code.

```java
import java.util.*;
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        HashMap<Integer,Integer> l1 = new HashMap<Integer,Integer>();
        int res = 0;
        for(int i = 0;i<nums1.length;i++){
            for(int j = 0;j<nums2.length;j++){
                int tmp1 = nums1[i]+nums2[j];
                l1.put(tmp1,l1.getOrDefault(tmp1,0)+1);
            }
        }
        
        for(int i = 0;i<nums3.length;i++){
            for(int j = 0;j<nums4.length;j++){
                res+=l1.getOrDefault(-nums3[i]-nums4[j],0);
            }
        }
        return res;
    }
}
```





## \383. Ransom Note

Given two stings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`. 

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```



Use hashmap to store the character of ransomNote, judge if magazine could cover whole characters.

```java
import java.util.*;
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        HashMap<Character,Integer> map = new HashMap<Character,Integer>();
        for(int i = 0;i<ransomNote.length();i++){
            map.put(ransomNote.charAt(i),map.getOrDefault(ransomNote.charAt(i),0)+1);
        }
        
        for(int j = 0;j<magazine.length();j++){
            if(map.containsKey(magazine.charAt(j))){
                map.put(magazine.charAt(j),map.get(magazine.charAt(j))-1);
            }
        }
        
        for(int i : map.values()){
            if(i>0){
                return false;
            }
        }
        return true;
    }
}
```



Or use a array, because we just have 26 characters need to judge. If use array, we do not need to create a hashmap function. Even though the time complexity will not decrease, but practical speed will increase.

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int [] map = new int [26];
        for(int i = 0;i<ransomNote.length();i++){
            map[ransomNote.charAt(i)-'a'] += 1;
        }
        
        for(int j = 0;j<magazine.length();j++){
            map[magazine.charAt(j)-'a'] -= 1;
        }
        
        for(int i : map){
            if(i>0){
                return false;
            }
        }
        return true;
    }
}
```



## \15. 3sum

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```



the difficult point in this question is to find a suitable data structure. The question require not contain duplicate triplets, and the index of 3 numbers can not be the same.

Use the hashset cache to store the 3rd number, and use another hashset check to check if there is a duplicate. To check the duplication, we need to sort the original array first, and the cache should be updated every time after the first iteration

```java
import java.util.*;
class Solution {
    public List<List<Integer>> threeSum(int[] num) {
        Arrays.sort(num);
        List<List<Integer>> out = new ArrayList();
        HashSet<Integer> cache = new HashSet<>();
        HashSet<List<Integer>> check = new HashSet<>();
        
        for(int i = 0;i<num.length;i++){
            for(int j = i+1;j<num.length;j++){
                List<Integer> tmp = new ArrayList<>();
                if(cache.contains(-num[i]-num[j])){
                    tmp.add(num[i]);
                    tmp.add(num[j]);
                    tmp.add(0-num[i]-num[j]);       
                    if(!check.contains(tmp)){
                        out.add(tmp);
                        check.add(tmp);
                    }
                }
            }
            cache.add(num[i]);
        }
        return out;
    }
}
```



We also can iterate the first number, and use 2 pointers to find which 3 are satisfied sum is 0.

Still care about the occurance of duplication.

```java
import java.util.*;
class Solution {
    public List<List<Integer>> threeSum(int[] num) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(num);
        for(int i = 0;i<num.length-2;i++){
            if(i>0 && num[i] == num[i-1]) continue;	// skip same result
            int j = i+1;
            int k = num.length-1;
            while(j<k){
                if(num[j]+num[k] == -num[i]){
                    res.add(Arrays.asList(num[i],num[j],num[k]));
                    j++;
                    k--;
                    while(j<k && num[j] == num[j-1]) j++;	// skip same result
                    while(j<k && num[k] == num[k+1]) k--;	// skip same result
                }else if(num[j]+num[k]>-num[i]) k--;
                 else j++;
            }
        }
        return res;
    }
}
```





## \18. 4Sum

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**. 

**Example 1:**

```
Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```



The difference between 3sum and 4sum is 4sum has one more loop than 3sum.

```java
import java.util.*;
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0;i<nums.length;i++){	// one more loop
            if(i!=0 && nums[i] == nums[i-1]) continue;
            for(int j = i+1;j<nums.length;j++){
                if(j!=i+1 && nums[j] == nums[j-1]) continue;
                int need = target-nums[i]-nums[j];
                int l = j+1;
                int r = nums.length-1;
                while(l<r){
                    if(nums[l]+nums[r] == need){
                        res.add(Arrays.asList(nums[i],nums[j],nums[l],nums[r]));
                        l++;
                        r--;
                        while(l<r && nums[l] == nums[l-1]) l++;
                        while(l<r && nums[r] == nums[r+1]) r--;
                    }else if(nums[l]+nums[r] < need){
                        l++;
                    }else{
                        r--;
                    }
                }
            }
        }
        return res;
    }
}
```







# Binaray Tree



## \144. Binary Tree Preorder Traversal

recurtion:

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> arr = new ArrayList<>();
        pre(root,arr);
        return arr;
    }
    
    public void pre(TreeNode root, List<Integer> arr){
        if(root == null) return;
        arr.add(root.val);
        pre(root.left,arr);
        pre(root.right,arr);
    }
}
```

Iteration:

we need the sequence: middle->left->right, so the sequence of pushing is middle->right->left. With this order and the character of stack of first in last out. We can get the preorder traversal.

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> arr = new ArrayList<>();
        if(root == null) return arr;
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while(s.isEmpty() == false){
            TreeNode cur = s.pop();
            arr.add(cur.val);
            if(cur.right!=null){
                s.push(cur.right);
            }
            if(cur.left!=null){
                s.push(cur.left);
            }
        }
        return arr;
    }
}
```



## \145. Binary Tree Postorder Traversal





recursion:

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> arr = new ArrayList<>();
        pre(root,arr);
        return arr;
    }
    
    public void pre(TreeNode root, List<Integer> arr){
        if(root == null) return;
        pre(root.left,arr);
        pre(root.right,arr);
        arr.add(root.val);
    }
}
```



literation:

the sequence of postorder traversal is left->right->middle, we can use the preorder traversal to help solve this.

Preorder traversal is middle->left->right, and the push sequence is middle->right->left. So we just need change the push sequence of preorder traversal, from m->r->l to m->l->r, then the final array  sequence is m->r->l, reverse this array, then we get the postorder traversal.

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> arr = new ArrayList<>();
        if(root == null) return arr;
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while(!s.isEmpty()){
            TreeNode cur = s.pop();
            arr.add(cur.val);
            if(cur.left!=null){
                s.push(cur.left);
            }
            if(cur.right!=null){
                s.push(cur.right);
            }
        }
        Collections.reverse(arr);
        return arr;
    }
}
```





## \94. Binary Tree Inorder Traversal

Recursion:

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> arr = new ArrayList<>();
        pre(root,arr);
        return arr;
    }
    
    public void pre(TreeNode root, List<Integer> arr){
        if(root == null) return;  
        pre(root.left,arr);
        arr.add(root.val);
        pre(root.right,arr);
    }
}
```



Literation:

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root; 
        while(cur!=null || !stack.isEmpty()){
            if(cur!=null){
                stack.push(cur);
                cur = cur.left;
            }else{
                cur =  stack.pop();
                res.add(cur.val);
                cur = cur.right;
            }
        }
        return res;
    }
}
```





## \102. Binary Tree Level Order Traversal

Recursion:

uses the idea of dfs

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> levelOrder(TreeNode root) {
        order(root,0);
        return res;   
    }
    
    public void order(TreeNode root, Integer level){
        if(root == null) return;
        level++;
        
        if(res.size()<level){
            List<Integer> in = new ArrayList<>();
            res.add(in);
        }
        res.get(level-1).add(root.val);
        
        order(root.left,level);
        order(root.right,level);
    }
}
```



Iteration:

use the idea of bfs.

```java
import java.util.*;
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> levelOrder(TreeNode root) {
        order(root);
        return res;   
    }
    
    public void order(TreeNode root){
        if(root == null) return;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while(!queue.isEmpty()){
            List<Integer> a = new ArrayList<>();
            int len = queue.size();
            while(len>0){
                len--;
                TreeNode tmp = queue.poll();
              	//	populate the queue to satisfy the next iteration
                if(tmp.left!=null) queue.offer(tmp.left);
                if(tmp.right!=null) queue.offer(tmp.right);
                a.add(tmp.val);
            }
            res.add(a);
        }
    }
}
```





## \226. Invert Binary Tree

The idea of this question is preorder traversal, we just need to write a preorder traversal and change part of the code, replace the output portion with a swap child node, we get the answer.



recursion

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        invert(root);
        return root;
    }
    
    public void invert(TreeNode root){
        if(root == null) return;
        
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        
        invert(root.left);
        invert(root.right);
    }
}
```



iteration

```java
import java.util.*;
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root == null) return null;
        Stack<TreeNode> s = new Stack<>();
        s.push(root);
        while(!s.isEmpty()){
            TreeNode tmp = s.pop();
            TreeNode help = tmp.left;
            tmp.left = tmp.right;
            tmp.right = help;
            
            // Use iteration to implement preorder traversal, in general, we should push right first then left(character of stack:FILO)
            // but here we invert the 2 child nodes, so we push left first.
            if(tmp.left!=null){
                s.push(tmp.left);
            }
            if(tmp.right!=null){
                s.push(tmp.right);
            }
        }
        return root;
    }
}
```



Also, level order traversal could solve this question, because this traversal also iterate all nodes in binary tree.

```java
import java.util.*;
class Solution {
    public TreeNode invertTree(TreeNode root) {
        order(root);
        return root;  
    }
    
    public void order(TreeNode root){
        if(root == null) return;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while(!queue.isEmpty()){
            int len = queue.size();
            while(len>0){
                len--;
                TreeNode tmp = queue.poll();
              	TreeNode help = tmp.left;
                tmp.left = tmp.right;
                tmp.right = help;
                
                if(tmp.left!=null) queue.offer(tmp.left);
                if(tmp.right!=null) queue.offer(tmp.right);
                
            }
        }
    }
}
```





## \101. Symmetric Tree

Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true
```



It should be mentioned that we just need to compare the values left.left and right.right left.right and right.left.

recursive version 

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return false;
        return define(root.left,root.right);
    }
    
    public boolean define(TreeNode l,TreeNode r){
        if(l == null || r == null) return l == r;        
        if(l.val != r.val) return false;
        return define(l.left,r.right) && define(l.right,r.left);
    }
}
```

Non recursive version

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root.left);
        q.add(root.right);
        
        while(q.size()>0){
            TreeNode l = q.poll();
            TreeNode r = q.poll();
            
            if(l == null || r == null){
                if(l == r){
                    continue;    
                } else{
                    return false;
                }
            }
            if(l.val!=r.val) return false;
            
            q.add(l.left);
            q.add(r.right);
            q.add(l.right);
            q.add(r.left);
        }
        
        return true;
    }
}
```



## \104. Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node. 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```





easy recursive approach

```java
class Solution {
    public int maxDepth(TreeNode r) {
        if(r == null) return 0;
        return 1+Math.max(maxDepth(r.left),maxDepth(r.right)); 
    }
}
```







Level order traversal method

recursive DFS

```java
class Solution {
    int level;
    public int maxDepth(TreeNode root) {
        upup(root,0);
        return level;
    }
    
    public void upup(TreeNode r, int cur){
        if(r==null) return;
        cur++;
        if(cur>level) level = cur;
        
        upup(r.left,cur);
        upup(r.right,cur);
    } 
}
```



iteration of BFS

```java
class Solution {
    int level = 0;
    public int maxDepth(TreeNode root) {
        upup(root);
        return level;
    }
    
    public void upup(TreeNode r){
        if(r == null) return ;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(r);
        
        while(!q.isEmpty()){
            level++;
            int len = q.size();
            while(len>0){
                len--;
                TreeNode tmp = q.poll();
                if(tmp.left!=null) q.add(tmp.left);
                if(tmp.right!=null) q.add(tmp.right);
            }
        }
    } 
}
```



## \559. Maximum Depth of N-ary Tree

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).*

**Example 1:**

<img src="https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png" alt="img" style="zoom:50%;" />

```
Input: root = [1,null,3,2,4,null,5,6]
Output: 3
```



recursion

```java
class Solution {
    public int maxDepth(Node root) {
        if(root == null) return 0;
        int level = 0;
        for(Node child : root.children){
            level = Math.max(level,maxDepth(child));
        }
        return level+1;
    }
}
```

or

```java
class Solution {
    public int maxDepth(Node root) {
        if(root == null) return 0;
        int level = 1;
        for(Node node : root.children){
            level = Math.max(level,1+maxDepth(node));
        }
        return level;    
     }
}
```







BFS

```java
class Solution {
    public int maxDepth(Node root) {
        if(root == null) return 0;
        
        int level = 0;
        Queue<Node> q = new LinkedList<>();
        q.add(root);
        
        while(q.size()>0){
            int len = q.size();
            for(int i = 0;i<len;i++){
                Node tmp = q.poll();
                for(Node node : tmp.children){
                    q.add(node);
                }
            }
            level++;
        }
        return level;
    }
}
```





## \111. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```



DFS

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        return dfs(root,1);
    }
    
    public int dfs(TreeNode root, int level){
        if(root.left == null && root.right == null){
             return level;
        }
        
        level++;
        
        if(root.left == null && root.right!=null){
            return dfs(root.right,level);
        }else if(root.left!=null && root.right == null){
            return dfs(root.left,level);
        }else{
            return Math.min(dfs(root.left,level),dfs(root.right,level));
        }
        
    }
}
```





BFS could gain higher performance, because when we reach the first terminal, we will return, but if we use DFS, we need to iterate the whole tree then we can get the answer.

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        int depth = 1;
        while(!q.isEmpty()){
            int len = q.size();
            for(int i = 0;i<len;i++){
                TreeNode tmp = q.poll();
                if(tmp.left == null && tmp.right == null){
                    return depth;
                }
                if(tmp.left != null){
                    q.add(tmp.left);
                }
                if(tmp.right != null){
                    q.add(tmp.right);
                }
            }
            depth++;
        }
        return depth;
    }
}
```



recursion

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int l = minDepth(root.left);
        int r = minDepth(root.right);
        
        if(root.left == null){
            return r+1;
        }
        if(root.right == null){
            return l+1;
        }
        return 1+Math.min(r,l);
    }
}
```





## \222. Count Complete Tree Nodes

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

```
Input: root = [1,2,3,4,5,6]
Output: 6
```



Use the BFS level order traversal. when iterate every node, update the res.

```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null) return 0;
        
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        int res = 0;
        
        while(!q.isEmpty()){
            int len = q.size();
            for(int i = 0;i<len;i++){
                res++;
                TreeNode tmp = q.poll();
                if(tmp.left!=null) q.add(tmp.left);
                if(tmp.right!=null) q.add(tmp.right);
            }
        }   
        return res;
    }
}
```



The problem said this is a complete binary tree, so we know that only the lowest level is not complete.

We also know a full tree have 2^n-1 nodes, so we can compare the height of left tree and the height of right tree, if these two tree have the same height, use the 2^n-1 formula. Otherwise, recursive the function, calculate the current node's left node and right node. finally sum them all, we could get the answer.

time complexity O(logn^2); space complexity O(logn1)

```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null) return 0;
        
        int left = getLeftHeight(root.left);
        int right = getRightHeight(root.right);
        
        if(left == right) return ((2<<left)-1);
        
        return countNodes(root.left)+countNodes(root.right)+1;
    }
    
    int getLeftHeight(TreeNode node){
        if(node == null) return 0;
        int res = 0;
        while(node != null){
            node = node.left;
            res++;
        }
        return res;
    }
    
    int getRightHeight(TreeNode node){
        if(node == null) return 0;
        int res = 0;
        while(node != null){
            node = node.right;
            res++;
        }
        return res;
    }
}
```



## \110. Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of *every* node differ in height by no more than 1.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```



Recursive approach

Since we are looking for height, we use the back-order traversal method. When the difference between left and right tree is larger than 1, we return the -1, which means this is not a balanced tree.

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return getH(root) == -1 ? false : true;     
    }
    
    public int getH(TreeNode node){
        if(node == null) return 0;
        
        int left = getH(node.left);
        if(left == -1) return -1;
        
        int right = getH(node.right);
        if(right == -1) return -1;
        
        // check
        if(Math.abs(left-right) >1) return -1;
      
        return Math.max(left,right)+1;
    }
}
```







## \257. Binary Tree Paths

Given the `root` of a binary tree, return *all root-to-leaf paths in **any order***.

A **leaf** is a node with no children. 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```



recursion

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        dfs(root,"",res);
        return res;
    }
    
    public void dfs(TreeNode root, String cur, List<String> res){
        if(root == null) return ;
        if(root.left == null&& root.right == null){
            res.add(cur+root.val);
        }
        
        dfs(root.left,cur+root.val+"->",res);
        dfs(root.right,cur+root.val+"->",res);
    }
}
```



BTW, we can also use the iteration to mimic the recursion.

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        if(root == null) return res;
        
        Queue<String> path = new LinkedList<>();
        Queue<TreeNode> node = new LinkedList<>();
        node.add(root);
        path.add(root.val+"");
        
        while(!node.isEmpty()){
            String item = path.poll();
            TreeNode tmp = node.poll();
            
            if(tmp.left == null && tmp.right == null){
                res.add(item);
            }    
            if(tmp.left!=null){
                node.add(tmp.left);
                path.add(item+"->"+tmp.left.val);
            }
            if(tmp.right!=null){
                node.add(tmp.right);
                path.add(item + "->"+tmp.right.val);
            }
        }
        return res;
    }
}
```



## \404. Sum of Left Leaves

Given the `root` of a binary tree, return the sum of all left leaves.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 24
Explanation: There are two left leaves in the binary tree, with values 9 and 15 respectively.
```



level order traversal approach.

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) return 0;
        
        int res = 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while(!q.isEmpty()){
            TreeNode tmp = q.poll();
            if(tmp.left!=null && tmp.left.left == null && tmp.left.right == null){
                res+=tmp.left.val;
            }
            if(tmp.left!=null) q.add(tmp.left);
            if(tmp.right!=null) q.add(tmp.right);
        }
        return res;
    }
}
```



Recursive

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        return recur(root,0);
    }
    
    public int recur(TreeNode node,int res){
        if(node == null) return res;
        
        if(node.left != null && node.left.left == null && node.left.right == null){
            res+=node.left.val;
        }  
        return recur(node.left,0)+recur(node.right,0)+res;
    }
}
```



## \513. Find Bottom Left Tree Value

Given the `root` of a binary tree, return the leftmost value in the last row of the tree.

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/14/tree2.jpg)

```
Input: root = [1,2,3,4,null,5,6,null,null,7]
Output: 7
```



We can use the level order traversal. The last row's leftest node is what we need, just return it.

```java
class Solution {
    ArrayList<ArrayList<Integer>> res = new ArrayList<>();
    public int findBottomLeftValue(TreeNode root) {
        bfs(root);
        return res.get(res.size()-1).get(0);
    }

    public void bfs(TreeNode root){
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);

        while(!q.isEmpty()){
            ArrayList<Integer> res1 = new ArrayList<>();
            int len = q.size();

            for(int i = 0;i<len;i++){
                root = q.poll();    // here we also can create a new TreeNode = q.poll(), but use the root could reduce the space
                if(root.left!=null) q.add(root.left);
                if(root.right!=null) q.add(root.right);
                res1.add(root.val);
            }
            res.add(res1);

        }
    }
}
```



Cause we only need the leftest node in the last row, we can change the left-to-right level order traversal to right-to-left. From this, we know that the last node we iterate is what we need, so we do not to need to create an Arraylist to store all node's value.

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while(!q.isEmpty()){
            int len = q.size();
            for(int i = 0;i<len;i++){
                root = q.poll();
                if(root.right!=null) q.add(root.right);
                if(root.left!=null) q.add(root.left);
            }
        }
        return root.val;
    }
}
```



similarly, dfs could solve this also.

```java
class Solution {
    ArrayList<ArrayList<Integer>> res = new ArrayList<>();
    public int findBottomLeftValue(TreeNode root) {
        dfs(root,0);
        return res.get(res.size()-1).get(0);
    }
    public void dfs(TreeNode root,int level){
        if(root == null) return ;
        
        level++;
        if(res.size()<level){
            ArrayList<Integer> tmp = new ArrayList<>();
            res.add(tmp);
        }
        
        res.get(level-1).add(root.val);
        dfs(root.left,level);
        dfs(root.right,level);
    } 
}
```



dfs also can have some optimisation. Since we just need the last row's leftest node's value, we can record the number when the depth update. From doing this, we do not need an array to store all node's value.

```java
public class Solution {
    int maxDepth = 0;
    int val = 0;

    private void dfs(TreeNode root, int depth) {
        if (root != null) {
            if (depth > maxDepth) {
                val = root.val;
                maxDepth = depth;
            }
            dfs(root.left, depth + 1);
            dfs(root.right, depth + 1);
        }
    }

    public int findBottomLeftValue(TreeNode root) {
        dfs(root, 1);
        return val;
    }
}
```





## \112. Path Sum

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
```



Just iterate it. After iterating all nodes, we get the answer.

```java
class Solution {
    Boolean label = false;
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;     
        dfs(root,0,targetSum);
        return label;
    }
    
    public void dfs(TreeNode node, int cur,int targetSum){
        if(node == null) return ;
        
        cur+=node.val;
        if(cur == targetSum && node.left == null && node.right == null){
            label = true;
        }
        
        dfs(node.left,cur,targetSum);
        dfs(node.right,cur,targetSum);
        
    }
}
```









## \113. Path Sum II

Given the `root` of a binary tree and an integer `targetSum`, return *all **root-to-leaf** paths where the sum of the node values in the path equals* `targetSum`*. Each path should be returned as a list of the node **values**, not node references*.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
Explanation: There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22
```



DFS recursion could help solve this, but we need to care about the back tracking. Since we need to get every paths, so after every recursion we need back track the dynamic array(cur_arr).

```java
class Solution {
    
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        if(root == null) return res;

        ArrayList<Integer> tmp = new ArrayList<>();
        dfs(root,tmp,targetSum);
        return res;
    }

    public void dfs(TreeNode node, ArrayList<Integer> cur_arr, int targetSum){
        if(node == null) return ;

        cur_arr.add(node.val);
        if(node.left == null && node.right == null && node.val == targetSum){
            res.add(new ArrayList<>(cur_arr));
        }else{
            dfs(node.left,cur_arr,targetSum-node.val);
            dfs(node.right,cur_arr,targetSum-node.val);
        }
        cur_arr.remove(cur_arr.size()-1);	// back tracking
    }
}
```



## \106. Construct Binary Tree from Inorder and Postorder Traversal

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return *the binary tree*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```



the last element of postorder array is the middlest node in this tree. So use this element we can get the left nodes and right nodes. This process is like level by level and divide it. recursion could handle this!

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {   
        return recur(postorder,inorder);
    }
    
    public TreeNode recur(int[] post,int[] in){
        if(post.length<1) return null;
        
        TreeNode root = new TreeNode(post[post.length-1]);
        
        int k = 0;
        for(;k<in.length;k++){
            if(in[k] == post[post.length-1]){
                break;
            }
        }
        root.left = recur(Arrays.copyOfRange(post,0,k),
                          Arrays.copyOfRange(in,0,k));
        root.right = recur(Arrays.copyOfRange(post,k,post.length-1),
                           Arrays.copyOfRange(in,k+1,post.length));
        return root;
    }
}
```



Of course! You can use a HashMap to optimize the code, because if we use for loop to search the middle node, it will repeat searching many times.

Also we can judge if current node is a leaf node or not, just to skip the process that add null node to leaf node.

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        HashMap<Integer, Integer> hm = new HashMap<Integer,Integer>();
        for (int i=0;i<inorder.length;++i)
            hm.put(inorder[i], i);
        return buildTreePostIn(inorder, 0, inorder.length-1, postorder, 0,
                postorder.length-1,hm);
    }

    private TreeNode buildTreePostIn(int[] inorder, int is, int ie, int[] postorder, int ps, int pe,
                                     HashMap<Integer,Integer> hm){
        if (ps>pe || is>ie) return null;
        TreeNode root = new TreeNode(postorder[pe]);
        if(is == ie) return root;   // if current node is a lead?
        
        int ri = hm.get(postorder[pe]);     
        TreeNode leftchild = buildTreePostIn(inorder, is, ri-1, postorder, ps, ps+ri-is-1, hm);
        TreeNode rightchild = buildTreePostIn(inorder,ri+1, ie, postorder, ps+ri-is, pe-1, hm);
        root.left = leftchild;
        root.right = rightchild;
        return root;
    }
}
```



## \105. Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```



The idea is pretty similar to the last one, what we need to care is the middle number, we set m as `map.get(pre[pl])-il` , this could make the parameters definition easier.

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {

        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i<inorder.length;i++){
            map.put(inorder[i],i);
        }
        return build(preorder,0,preorder.length-1,inorder,0,inorder.length-1,map);
    }

    public TreeNode build(int[] pre, int pl, int pr,int [] in, int il, int ir, HashMap<Integer,Integer> map){
        if(pl>pr) return null;
        TreeNode root = new TreeNode(pre[pl]);
        if(pl >= pr) return root;
        
        int m = map.get(pre[pl])-il;
        root.left = build(pre,pl+1,pl+m,in,il,il+m,map);
        root.right = build(pre,pl+m+1,pr,in,il+m+1,ir,map);

        return root;
    }
}
```



## \654. Maximum Binary Tree

You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:

1. Create a root node whose value is the maximum value in `nums`.
2. Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
3. Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.

Return *the **maximum binary tree** built from* `nums`. 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

```
Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.
```



The idea is similar to last 2 questions.

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i<nums.length;i++){
            map.put(nums[i],i);
        }
        return con (nums,0,nums.length-1,map);
    }
    
    public TreeNode con (int[] nums, int l, int r, HashMap<Integer,Integer> map){
        if(l == r+1) return null;
        
        int max = getMax(Arrays.copyOfRange(nums,l,r+1));
        int index = map.get(max);
        TreeNode root = new TreeNode(max);
        if(l>=r) return root;
        
        root.left = con(nums,l,index-1,map);
        root.right = con(nums,index+1,r,map);
        
        return root;
        
    }
    
    public int getMax(int[] nums){
        int max = 0;
        for(int i = 0;i<nums.length;i++){
            if(nums[i]>max) max = nums[i];
        }
        return max;
    }
}
```





## \617. Merge Two Binary Trees

You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return *the merged tree*.

**Note:** The merging process must start from the root nodes of both trees.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

```
Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
Output: [3,4,5,5,4,null,7]
```



An intuitive approach.

```java
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null) return null;
        
        int val = (t1 == null ? 0 : t1.val) + (t2 == null ? 0 : t2.val);
        TreeNode newNode = new TreeNode(val);
        
        newNode.left = mergeTrees(t1 == null ? null : t1.left, t2 == null ? null : t2.left);
        newNode.right = mergeTrees(t1 == null ? null : t1.right, t2 == null ? null : t2.right);
        
        return newNode;
    }
}
```



Concise and wonderful approach.

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if(root1 == null) return root2;
        if(root2 == null) return root1;
        
        TreeNode node = new TreeNode(root1.val+root2.val);
        node.left = mergeTrees(root1.left,root2.left);
        node.right = mergeTrees(root1.right,root2.right);
        return node;
    }
}
```





## \700. Search in a Binary Search Tree

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

```
Input: root = [4,2,7,1,3], val = 2
Output: [2,1,3]
```



Since the idea of BST, if we find root.val>val we keep searching the left, or we search the right tree.

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null || root.val == val ) return root;
        return root.val>val?searchBST(root.left,val) : searchBST(root.right,val);
    }
}
```







