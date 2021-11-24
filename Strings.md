# Strings

## \344. Reverse String

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.

**Example 1:**

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```



```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while (l < r) {
            s[l] ^= s[r];  
            s[r] ^= s[l];  
            s[l++] ^= s[r--];  
        }
    }
}
```





## \541. Reverse String II

Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and left the other as original.

**Example 1:**

```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```





There are 2 complex subquestions, one is the surplus part of array is less than k, how to process it. another is how to update the iterate index.

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] tmp = s.toCharArray();
        int len = tmp.length;
        int index = 0;
        while(index<len){
            int l = index;
            int r = Math.min(l+k-1,len-1);  // so neat process method
            swap(tmp,l,r);
            index+=2*k;	// just update the index, use l and r to swap
        }
        return String.valueOf(tmp);
        
    }
    
    public void swap(char[] arr, int l, int r){
        while(l<r){
            char t = arr[l];
            arr[l++] = arr[r];
            arr[r--] = t;
        }
    }
}
```



## \151. Reverse Words in a String

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces. 

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```



First remove any extra Spaces (including those at the beginning and end), and then iterate backwards, adding the part you just walked through to a new string.

This solution is intuitive, but we need to create a new String to help process, so can we operate characters on the original String?

```java
class Solution {
    public String reverseWords(String s) {
        if(s.length() == 0 || s.length() == 1) return s;

        StringBuilder sb = new StringBuilder(s);
        int len = s.length();
        for(int i = 0;i<len;i++){
            if(i == 0 && sb.charAt(i) == ' '){
                sb.delete(i,i+1);
                len--;
                i--;
            }else if(i>=0 && sb.charAt(i) == ' ' && sb.charAt(i-1)==' '){
                if(i == len-1){
                    sb.delete(i-1,i+1);
                }else{
                    sb.delete(i,i+1);
                    len--;
                    i--;
                }   
            }
        }

        int index = sb.length()-1;
        int label = sb.length()-1;
        String res = "";

        for(;index>=0;index--){
            if(sb.charAt(index) == ' ' ){
                res+=sb.substring(index+1,label+1);
                res+=" ";
                label = index-1;
            }
            if(index == 0){
                res+=sb.substring(index,label+1);
            }
        }
        return res;
    }
}
```





We can reverse the whole String, then reverse every substring which divided by ' '. Then the space complexity is O(1)

```java
class Solution {
    public String reverseWords(String s) {
        if(s.length() == 0 || s.length() == 1) return s;

        StringBuilder sb = new StringBuilder(s);
        int len = s.length();
        for(int i = 0;i<len;i++){
            if(i == 0 && sb.charAt(i) == ' '){
                sb.delete(i,i+1);
                len--;
                i--;
            }else if(i>=0 && sb.charAt(i) == ' ' && sb.charAt(i-1)==' '){
                if(i == len-1){
                    sb.delete(i-1,i+1);
                }else{
                    sb.delete(i,i+1);
                    len--;
                    i--;
                }   
            }
        }
        reverse(sb,0,sb.length()-1);
        int index = 0;
        int label = 0;
        for(;index<sb.length();index++){
            if(sb.charAt(index) == ' ' ){
                reverse(sb,label,index-1);
                label = index+1;
            }else if(index == sb.length()-1){
                reverse(sb,label,index);
            }
        }
        return sb.toString();
    }
    
    public void reverse(StringBuilder sb,int l,int r){
        while(l<r){
            char tmp = sb.charAt(l);
            sb.replace(l,l+1,String.valueOf(sb.charAt(r)));
            sb.replace(r,r+1,String.valueOf(tmp));
            l++;
            r--;
        }
    }
}
```





## 剑指 Offer 05. 替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

示例 1：

输入：s = "We are happy."
输出："We%20are%20happy."



```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder sb = new StringBuilder();
        int left = s.length()-1;
        for(int i = 0;i<=left;i++){
            if(s.charAt(i) == ' '){
                sb.append("  ");
            }
        }
        int right = left+sb.length();

        s = s+sb.toString();
        char[] res = s.toCharArray();
        for(;left>=0;left--,right--){
            if(res[left] ==  ' '){
                res[right--] = '0';
                res[right--] = '2';
                res[right] = '%';
            }else{
                res[right] = res[left];
            }
        }
        return String.valueOf(res);
    }
}
```





## \151. Reverse Words in a String

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```



Time complexity O(n), Space complexity O(n)

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder sb = new StringBuilder(s);
        return sb.substring(n)+sb.substring(0,n);
    }
}
```



Time complexity O(n), Space complexity O(1)

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder sb = new StringBuilder(s);
        reverse(sb,0,s.length()-1);
        reverse(sb,0,s.length()-n-1);
        reverse(sb,s.length()-n,s.length()-1);
        return sb.toString();
    }

    public void reverse(StringBuilder sb,int left,int right){
        while(left<right){
            char tmp = sb.charAt(right);
            sb.setCharAt(right,sb.charAt(left));
            sb.setCharAt(left,tmp);
            left++;
            right--;
        }
    }
}
```







## \28. Implement strStr()

Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).

Return the index of the first occurrence of needle in haystack, or `-1` if `needle` is not part of `haystack`.

**Clarification:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's [strstr()](http://www.cplusplus.com/reference/cstring/strstr/) and Java's [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)).

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```





KMP algorithm could reach time complexity O(m+n)

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        int[] next = new int[needle.length()];
        getNext(next,needle);

        int j = 0;
        for(int k = 0;k<haystack.length();k++){
            while(j>0 && haystack.charAt(k) != needle.charAt(j)){
                j = next[j-1];
            }
            if(haystack.charAt(k) == needle.charAt(j)){
                j++;
            }
            if(j == needle.length()) {
                return (k-needle.length()+1);
            }
        }
        return -1;


    }

    public void getNext(int[] next,String s){
        next[0] = 0;
        int j = 0; // left pointer

        for(int i = 1;i<s.length();i++){
            while(j>0 && s.charAt(i)!=s.charAt(j)){
                j = next[j-1];
            }
            if(s.charAt(i) == s.charAt(j)){
                j++;
            }
            next[i] = j;
        }
    }
}

```



## \459. Repeated Substring Pattern

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

**Example 1:**

```
Input: s = "abab"
Output: true
Explanation: It is the substring "ab" twice.
```

**Example 2:**

```
Input: s = "aba"
Output: false
```



Use the kmp algorithm, we can get a `next` array. The last element of `next` is the biggest  one, judge if the last element exist and `len % (len - next[len-1]) == 0)`

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int len = s.length();
        if(len == 0) return false;
        if(len == 1) return false;
            
            
        int [] next = new int[len];
        getNext(next,s);
        
        if (next[len-1] > 0 && len % (len - next[len-1]) == 0) {
            return true;
        }
        return false;
    }

    public void getNext(int[] next, String s){
        int i = 0;
        next[0] = 0;
        for(int j = 1;j<s.length();j++){
            while(i>0 && s.charAt(j)!=s.charAt(i)){
                i = next[i-1];
            }
            if(s.charAt(i) == s.charAt(j)){
                i++;
            }
            next[j] = i;
        }
    }
}
```



If your string S contains a repeating substring, then this means you can "shift and wrap around" your string some number of times and have it match the original string.
Example: abcabc
Shift once: **c**abcab
Shift twice: **bc**abca
Shift three times: **abc**abc
Now the string matches the original string, so you know there is a repeated substring.



To avoid doing this weird wraparound and using modulo, you can just create a new string SS that is the original string concatenated with itself, and check if this new string contains the original string (shifted some number of times). However, you don't want it to match with the first half (S) and the second half (S), so you remove the first and last characters.

```java
class Solution {
    public boolean repeatedSubstringPattern(String str) {
        String s = str + str;
        return s.substring(1, s.length() - 1).contains(str);
    }
}
```

