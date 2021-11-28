# Stack & Queue

## \232. Implement Queue using Stacks

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.



```java
class MyQueue {
    Stack<Integer> s1;
    Stack<Integer> s2;
    

    public MyQueue() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }
    
    public void push(int x) {
        s1.push(x);
    }
    
    public int pop() {
        if(s2.size()!=0){
            return s2.pop();
        }else{
            while(s1.size() != 1){
                s2.push(s1.pop());
            }
            return s1.pop();
        }
        
    }
    
    public int peek() {
        if(s2.size()==0){
            while(s1.size()!=0){
                s2.push(s1.pop());
            }   
        }
        return s2.peek();
    }
    
    public boolean empty() {
        if(s1.empty() == true && s2.empty() == true){
            return true;
        }else{
            return false;
        }
    }
}
```



## \225. Implement Stack using Queues

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.



```java
class MyStack {
    Queue<Integer> q = new LinkedList<>();
    
    public void push(int x) {
        q.offer(x);
        for(int i = 0;i<q.size()-1;i++){
            q.offer(q.poll());
        }
    }
    
    public int pop() {
        return q.poll();
    }
    
    public int top() {
        return q.peek();
    }
    
    public boolean empty() {
        return q.size()!=0 ? false : true;
    }
}
```





## \20. Valid Parentheses

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

 

**Example 1:**

```
Input: s = "()"
Output: true
```



Just using the stack! If you want to use switch case, remenber write break in each case.

```java
class Solution {
    public boolean isValid(String str) {
        Stack<Character> s = new Stack<>();
        for(int i = 0;i<str.length();i++){
            switch (str.charAt(i)){
                case '(':
                    s.push(')');
                    break;
                case '[':
                    s.push(']');
                    break;
                case '{':
                    s.push('}');
                    break;
                default:
                    if(s.size()==0 || str.charAt(i) != s.pop()){
                        return false;
                    }
            }
        }
        return s.size() == 0;
    }
}
```





## \1047. Remove All Adjacent Duplicates In String

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

**Example 1:**

```
Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```



Besides, this question could also be handled by using double pointer.

```java
class Solution {
    public String removeDuplicates(String s) {
        char[] c = s.toCharArray();

        Stack<Character> stack = new Stack<>();
        stack.push(c[0]);
        for(int i = 1;i<c.length;i++){
            if( stack.isEmpty() || c[i] != stack.peek()){
                stack.push(c[i]);
            }else{
                stack.pop();
            }
        }

        char[] res = new char[stack.size()];
        for(int i = stack.size()-1;i>=0;i--){
            res[i] = stack.pop();
        }
        return String.valueOf(res);
    }
}
```









## \150. Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression.

**Note** that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

**Example 1:**

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```



Stack, stack, stack!!!

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> s = new Stack<>();
        for(int i = 0;i<tokens.length;i++){
            if(!tokens[i].equals("+")  && !tokens[i].equals("-")  && !tokens[i].equals("*")  && !tokens[i].equals("/") ){
                s.push(Integer.parseInt(tokens[i]));
            }else{
                int tmp1 = s.pop();
                int tmp2 = s.pop();
                switch(tokens[i]){
                    case "+":
                        s.push(tmp1+tmp2);
                        break;
                    case "-":
                        s.push(tmp2-tmp1);
                        break;

                    case "*":
                        s.push(tmp2*tmp1);
                        break;

                    case "/":
                        s.push(tmp2/tmp1);
                        break;
                }
            }
        }
        return s.pop();
    }
}
```





## \239. Sliding Window Maximum

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```



The idea of this question is create a new class Queue. In this Queue, everytime you want to add a new element, elements which less than this new element will be removed. poll only will remove the first element(in this queue, the first element must be the largest one). Getmax will return the first element.

Using this new Queue, we can easily find the sliding window's max number. To more easier to say, we simplify the process of finding the max with brute froce by creating a add function of new queue.

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int [] res = new int[nums.length-k+1];

        MyQueue239 myQueue239 = new MyQueue239();
        for(int i = 0;i<k;i++){
            myQueue239.add(nums[i]);
        }
        res[0] = myQueue239.getMax();

        for(int j = k;j<nums.length;j++){
            if(nums[j-k] == myQueue239.getMax()){
                myQueue239.poll();
            }
            myQueue239.add(nums[j]);
            res[j-k+1] = myQueue239.getMax();
        }
        return res;
    }
}

class MyQueue239{
    Deque<Integer> q = new LinkedList<>();
    int getMax(){
        return q.getFirst();
    }

    void add(int input){
        while(!q.isEmpty() && input>q.getLast()){
            q.removeLast();
        }
        q.addLast(input);
    }

    void poll(){
        q.removeFirst();
    }
}
```





## \347. Top K Frequent Elements

Given an integer array `nums` and an integer `k`, return *the* `k` *most frequent elements*. You may return the answer in **any order**.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```



The nice thing about this solution is using a List array, it's quite good in this question.

`List<Integer>[] arr = new List[nums.length + 1];`

Use a hashmap to count how many times every number exist, and use this List array to sort the frequency.

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i<nums.length;i++){
            map.put(nums[i],map.getOrDefault(nums[i],0)+1);
        }

        List<Integer>[] arr = new List[nums.length + 1];

        for(int key : map.keySet()){
            int frequency = map.get(key);
            if(arr[frequency]==null){
                arr[frequency] = new ArrayList<>();
            }
            arr[frequency].add(key);
        }

        int[] res= new int[k];
        int index = 0;
        for(int j = arr.length-1;j>=0 && index<k;j--){
            if(arr[j]!=null){
                for(int m = 0;m<arr[j].size() && index<k;m++){
                    res[index++] = arr[j].get(m);
                }
            }
        }
        return res;
    }
}
```

