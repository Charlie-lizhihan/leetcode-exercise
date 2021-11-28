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

## \2086. Minimum Number of Buckets Required to Collect Rainwater from Houses

You are given a **0-index****ed** string `street`. Each character in `street` is either `'H'` representing a house or `'.'` representing an empty space.

You can place buckets on the **empty spaces** to collect rainwater that falls from the adjacent houses. The rainwater from a house at index `i` is collected if a bucket is placed at index `i - 1` **and/or** index `i + 1`. A single bucket, if placed adjacent to two houses, can collect the rainwater from **both** houses.

Return *the **minimum** number of buckets needed so that for **every** house, there is **at least** one bucket collecting rainwater from it, or* `-1` *if it is impossible.*

**Example 1:**

```
Input: street = "H..H"
Output: 2
Explanation:
We can put buckets at index 1 and index 2.
"H..H" -> "HBBH" ('B' denotes where a bucket is placed).
The house at index 0 has a bucket to its right, and the house at index 3 has a bucket to its left.
Thus, for every house, there is at least one bucket collecting rainwater from it.
```



The key of this problem is we need splite the question to 2 parts.

1. check if we can collect water from every houses.
2. place buckets.

the 2nd part we need to place the bucket behind the house, this could make judgement after easier



```java
class Solution_W3 {
    public int minimumBuckets(String street) {
        char[] c = street.toCharArray();
        for(int i = 0;i<c.length;i++){
            if(c[i] == '.') continue;
            if(i-1>=0 && c[i] == 'H' && c[i-1] == '.') continue;
            if(i+1<c.length && c[i] == 'H' && c[i+1] == '.') continue;
            return -1;
        }

        int res = 0;
        for(int j = 0;j<c.length;j++){
            if(j-1>=0 && c[j] == 'H' && c[j-1] == 'a') continue;
            if(j+1<c.length && c[j] == 'H' && c[j+1] == '.'){
                c[j+1] = 'a';
                res++;
            }else if(j-1>=0 && c[j-1] == '.' && c[j] == 'H') {
                c[j-1] = 'a';
                res++;
            }
        }
        return res;
    }
}
```





