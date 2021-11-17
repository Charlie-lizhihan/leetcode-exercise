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

