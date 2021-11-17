

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

