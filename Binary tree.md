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
class Solution {    public boolean isSymmetric(TreeNode root) {        if(root == null) return false;        return define(root.left,root.right);    }        public boolean define(TreeNode l,TreeNode r){        if(l == null || r == null) return l == r;                if(l.val != r.val) return false;        return define(l.left,r.right) && define(l.right,r.left);    }}
```

Non recursive version

```java
class Solution {    public boolean isSymmetric(TreeNode root) {        if(root == null) return true;        Queue<TreeNode> q = new LinkedList<>();        q.add(root.left);        q.add(root.right);                while(q.size()>0){            TreeNode l = q.poll();            TreeNode r = q.poll();                        if(l == null || r == null){                if(l == r){                    continue;                    } else{                    return false;                }            }            if(l.val!=r.val) return false;                        q.add(l.left);            q.add(r.right);            q.add(l.right);            q.add(r.left);        }                return true;    }}
```



## \104. Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node. 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]Output: 3
```





easy recursive approach

```java
class Solution {    public int maxDepth(TreeNode r) {        if(r == null) return 0;        return 1+Math.max(maxDepth(r.left),maxDepth(r.right));     }}
```







Level order traversal method

recursive DFS

```java
class Solution {    int level;    public int maxDepth(TreeNode root) {        upup(root,0);        return level;    }        public void upup(TreeNode r, int cur){        if(r==null) return;        cur++;        if(cur>level) level = cur;                upup(r.left,cur);        upup(r.right,cur);    } }
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
Input: root = [1,null,3,2,4,null,5,6]Output: 3
```



recursion

```java
class Solution {    public int maxDepth(Node root) {        if(root == null) return 0;        int level = 0;        for(Node child : root.children){            level = Math.max(level,maxDepth(child));        }        return level+1;    }}
```

or

```java
class Solution {    public int maxDepth(Node root) {        if(root == null) return 0;        int level = 1;        for(Node node : root.children){            level = Math.max(level,1+maxDepth(node));        }        return level;         }}
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
class Solution {    public int minDepth(TreeNode root) {        if(root == null) return 0;        return dfs(root,1);    }        public int dfs(TreeNode root, int level){        if(root.left == null && root.right == null){             return level;        }                level++;                if(root.left == null && root.right!=null){            return dfs(root.right,level);        }else if(root.left!=null && root.right == null){            return dfs(root.left,level);        }else{            return Math.min(dfs(root.left,level),dfs(root.right,level));        }            }}
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







