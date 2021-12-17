

# Backtracking

## \77. Combinations

Given two integers `n` and `k`, return *all possible combinations of* `k` *numbers out of the range* `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```



Only the LinkedList class have the removeLast method.

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        back(n,k,1);
        return res;
    }
    
    public void back(int n,int k, int start){
        if(tmp.size() == k){
            res.add(new ArrayList<>(tmp));
            return;
        }
        
        for(int i = start;i<=n-(k-tmp.size())+1;i++){
            tmp.add(i);
            back(n,k,i+1);
            tmp.removeLast();
        }
    }
}
```





## \216. Combination Sum III

Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return *a list of all possible valid combinations*. The list must not contain the same combination twice, and the combinations may be returned in any order.

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
Explanation:
1 + 2 + 4 = 7
There are no other valid combinations.
```



same idea with the last one.

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        back(k,n,1,0);
        return res;
    }
    
    public void back(int k, int n, int start,int cur){
        if(cur>n) return ;
        if(tmp.size() == k){
            if(cur == n){
                res.add(new ArrayList<>(tmp));
            }
            return;
        }
        
        for(int i = start;i<=9-(k-tmp.size())+1;i++){
            tmp.add(i);
            back(k,n,i+1,cur+i);
            tmp.removeLast();
        }
    }
}
```







## \17. Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

 

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```



```java
class Solution {
    List<String> res = new ArrayList<>();
    String tmp = "";
    char[][] two = {{'a','b','c'},{'d','e','f'},{'g','h','i'},{'j','k','l'},{'m','n','o'},{'p','q','r','s'},{'t','u','v'},{'w','x','y','z'}};
    
    
    public List<String> letterCombinations(String digits) {
        if(digits.equals("")) return new ArrayList<>();
        back(digits,-1,digits.length());
        return res;
    }
    
    public void back(String digits, int depth, int len){
        depth++;
        if(depth>=len){
            res.add(tmp);
            return;
        }
        
        char [] cur;
        if(digits.charAt(depth) == '2') cur = two[0];
        else if(digits.charAt(depth) == '3') cur = two[1];
        else if(digits.charAt(depth) == '4') cur = two[2];
        else if(digits.charAt(depth) == '5') cur = two[3];
        else if(digits.charAt(depth) == '6') cur = two[4];
        else if(digits.charAt(depth) == '7') cur = two[5];
        else if(digits.charAt(depth) == '8') cur = two[6];
        else  cur = two[7];
        
        for(int i = 0;i<cur.length;i++){
            tmp+=cur[i];
            back(digits,depth,len);
            tmp = tmp.substring(0,tmp.length()-1);
        }
    }
}
```





## \39. Combination Sum

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```



The different between this one and the last one is we can use one number repeatedly. So every time we invoke the backtracking function, we will not update the` start` parameter.

Besides, we can optimize this code by adding a judgement in for loop. After doing this, programme will not go to the deeper recursion.

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    
    public List<List<Integer>> combinationSum(int[] candidates, int target) {       
        Arrays.sort(candidates);
        back(candidates,target,0,0);
        return res;
    }
    
    public void back(int[] candidate, int target, int sum,int start){
        if(sum>target){
            return;
        }else if(sum == target){
            res.add(new ArrayList<>(tmp));
            return;
        }
        
        for(int i = start;i<candidate.length && sum+candidate[i]<=target;i++){
            int cur = candidate[i];
            tmp.add(cur);
            sum+=cur;
            back(candidate,target,sum,i);
            tmp.removeLast();
            sum-=cur;
        }
    }
}
```





## \40. Combination Sum II

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```





The same number cannot be selected in the same tree layer, but can be selected in the same branch

![image-20211202103716981](/Users/lizhihan/Library/Application Support/typora-user-images/image-20211202103716981.png)

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        boolean[] flag = new boolean[candidates.length];
        Arrays.sort(candidates);
        back(candidates,target,0,0,flag);
        return res;
    }

    public void back(int[] candidates, int target, int start,int sum,boolean [] flag){
        if(sum>target) return ;
        if(sum == target){
            res.add(new ArrayList<Integer> (tmp));
        }

        for(int i = start;i<candidates.length;i++){
            if(i>=1 && flag[i-1] == false && candidates[i] == candidates[i-1]){
                continue;	// continue means we forgive current branch, move to the same layer's next number
            }

            tmp.add(candidates[i]);
            sum+=candidates[i];
            flag[i] = true;
            back(candidates,target,i+1,sum,flag);
            tmp.removeLast();
            sum-=candidates[i];
            flag[i] = false;
        }
    }
}
```





## \93. Restore IP Addresses

A **valid IP address** consists of exactly four integers separated by single dots. Each integer is between `0` and `255` (**inclusive**) and cannot have leading zeros.

- For example, `"0.1.2.201"` and `"192.168.1.1"` are **valid** IP addresses, but `"0.011.255.245"`, `"192.168.1.312"` and `"192.168@1.1"` are **invalid** IP addresses.

Given a string `s` containing only digits, return *all possible valid IP addresses that can be formed by inserting dots into* `s`. You are **not** allowed to reorder or remove any digits in `s`. You may return the valid IP addresses in **any** order.

**Example 1:**

```
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
```



Boundary conditions are hard to judge,

```java
class Solution {
    List<String> res = new ArrayList<>();

    public List<String> restoreIpAddresses(String s) {
        backTrack(s,0,0);
        return res;
    }


    private void backTrack(String s, int startIndex, int pointNum) {
        if(pointNum == 3){
            if(isValid(s,startIndex,s.length()-1) == true){
                res.add(s);
                return;
            }
            return;
        }

        for(int i = startIndex;i<s.length();i++){
            if(isValid(s,startIndex,i)){
                s = s.substring(0,i+1)+"."+s.substring(i+1);
                pointNum++;
                backTrack(s,i+2,pointNum);
                s = s.substring(0,i+1)+s.substring(i+2);
                pointNum--;
            }else{
                break;
            }
        }
    }


    private Boolean isValid(String s, int start, int end) {
        if (start > end) {
            return false;
        }
        if (s.charAt(start) == '0' && start != end) { // 0开头的数字不合法
            return false;
        }
        int num = 0;
        for (int i = start; i <= end; i++) {
            if (s.charAt(i) > '9' || s.charAt(i) < '0') { // 遇到⾮数字字符不合法
                return false;
            }
            num = num * 10 + (s.charAt(i) - '0');
            if (num > 255) { // 如果⼤于255了不合法
                return false;
            }
        }
        return true;
    }
}

```





## \78. Subsets

Given an integer array `nums` of **unique** elements, return *all possible subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```



We can use the backTracking to record every node in the backtracking tree.

<img src="/Users/lizhihan/Library/Application Support/typora-user-images/image-20211203230928724.png" alt="image-20211203230928724" style="zoom:50%;" />

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> subsets(int[] nums) {
        back(nums,0);
        return res;
    }
    public void back(int[] nums, int start){
        res.add(new ArrayList<>(tmp));
        for(int i = start;i<nums.length;i++){
            tmp.add(nums[i]);
            back(nums,i+1);
            tmp.removeLast();
        }
    }
}
```



bit manipulation also could solve this, because we have 2^nums.length possibilies, we can iterate all.

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0;i<(1<<nums.length);i++){  // There are 2^nums.length possibilities of subsets, itrate them all
            List<Integer> tmp = new ArrayList<>();
            for(int j = 0;j<nums.length;j++){
                if(((i>>j) & 1) != 0){  
                // judge every bit is equal to 1 or 0. The result is 1, 
                // which means we want to take the number of the current position 
                    tmp.add(nums[j]);
                }
            }
            res.add(tmp);
        }
        return res;
    }
}
```







## \90. Subsets II

Given an integer array `nums` that may contain duplicates, return *all possible subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```



we need to use the `used` array to record which element we already used. Remember to sort the initial array first.

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        boolean[] used = new boolean[nums.length];
        LinkedList<Integer> tmp = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        back(nums,0,used,tmp,res);
        return res;
    }
    
    public void back(int[] nums, int start,boolean[] used,LinkedList<Integer> tmp,List<List<Integer>> res){
        res.add(new ArrayList<>(tmp));
        if(start >= nums.length) return ;
        
        for(int i = start;i<nums.length;i++){
            if(i>=1 && used[i-1] == false && nums[i] == nums[i-1]){
                continue;
            }else{
                used[i] = true;
                tmp.add(nums[i]);
                back(nums,i+1,used,tmp,res);
                used[i] = false;
                tmp.removeLast();
            }
        }
    }
}
```







## \491. Increasing Subsequences

Given an integer array `nums`, return all the different possible increasing subsequences of the given array with **at least two elements**. You may return the answer in **any order**.

The given array may contain duplicates, and two equal integers should also be considered a special case of increasing sequence.

**Example 1:**

```
Input: nums = [4,6,7,7]
Output: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```



The idea is like the 78, we need to record the process of recursion, so we do not need to write a return.

Besides we also need to judge if there are 2 elements and to avoid repeated sets, we can use the set to store the result. Because set can not store the same elements.

```java
class Solution {
    Set<List<Integer>> res = new HashSet<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        back(nums,0);
        return new ArrayList<>(res);
    }
    
    public void back(int[] nums, int start){
        if(tmp.size()>=2) res.add(new ArrayList<>(tmp));
        
        for(int i = start;i<nums.length;i++){
            if(tmp.size()==0 || i>=1 && tmp.get(tmp.size()-1) <= nums[i]){
                tmp.add(nums[i]);
                back(nums,i+1);
                tmp.removeLast();
            }
        }
    }
}
```







## \46. Permutations

Given an array `nums` of distinct integers, return *all the possible permutations*. You can return the answer in **any order**.

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```



Each layer is searched from 0 instead of startIndex
You need to determine the current TMP. TMP cannot contain existing digits

````java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    public List<List<Integer>> permute(int[] nums) {
        back(nums);
        return res;
    }

    public void back(int[] nums){
        if(tmp.size() == nums.length){
            res.add(new ArrayList<>(tmp));
            return;
        }

        for(int i = 0;i<nums.length;i++){
            if(tmp.contains(nums[i])) continue;
            tmp.add(nums[i]);
            back(nums);
            tmp.removeLast();
        }
    }
}
````





## \47. Permutations II

Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.* 

**Example 1:**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```



The question said we need to return unique permutations, so we can use the character of HashSet to de-emphasize it. But this method has low efficiency.

```java
class Solution {
    Set<List<Integer>> res = new HashSet<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    boolean [] used ;
    public List<List<Integer>> permuteUnique(int[] nums) {
        used = new boolean[nums.length];
        back(nums);
        return new ArrayList<>(res);
    }

    public void back(int[] nums){
        if(tmp.size() == nums.length) {
            res.add(new ArrayList<>(tmp));
            return;
        }

        for(int i = 0;i<nums.length;i++){
            if(used[i]) continue;
            tmp.add(nums[i]);
            used[i] = true;
            back(nums);
            tmp.removeLast();
            used[i] = false;
        }
    }
}

```



create a used array to store we use this element or not, if we use the different element but with same value, we will skip it. Besides, we also need to skip itself, since the result is a permutation

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> tmp = new LinkedList<>();
    boolean [] used ;
    public List<List<Integer>> permuteUnique(int[] nums) {
        used = new boolean[nums.length];
        Arrays.sort(nums);
        back(nums);
        return res;
    }

    public void back(int[] nums){
        if(tmp.size() == nums.length) {
            res.add(new ArrayList<>(tmp));
            return;
        }

        for(int i = 0;i<nums.length;i++){
            if(i>0 && used[i-1] == true && nums[i] == nums[i-1]) continue;
            if(used[i] == false){
                tmp.add(nums[i]);
                used[i] = true;
                back(nums);
                tmp.removeLast();
                used[i] = false;
            }
        }
    }
}
```





## \51. N-Queens

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```



it should notice that every time we update the parameter `row` of back function, we should use `row+1`. Which means we will consider the next row.

```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    public List<List<String>> solveNQueens(int n) {
        char[][] map = new char[n][n];
        for (char[] c : map) {
            Arrays.fill(c, '.');
        }
        back(n,0,map);
        return res;
    }

    public void back(int n, int row, char[][] map){
        if(row == n){
            res.add(convert(map));
            return;
        }

        for(int i = 0;i<n;i++){
            if(check(row, i, map)){
                map[row][i] = 'Q';
                back(n,row+1,map);
                map[row][i] = '.';
            }
        }
    }

    public List<String> convert(char[][] map){
        List<String> c = new ArrayList<>();
        for(char[] i : map){
            c.add(String.copyValueOf(i));
        }
        return c;
    }

    public boolean check(int cur,int i,char[][] map){
        // column
        for (char[] strings : map) {
            if (strings[i] == 'Q') return false;
        }

        // 45degree
        for(int m = cur,n = i; m>=0&&n>=0; m--,n--){
            if(map[m][n] == 'Q') return false;
        }

        // 135degree
        for(int m = cur,n = i; n<=map.length -1&&m>=0; m--,n++){
            if(map[m][n] == 'Q') return false;
        }

        return true;
    }
}

```

