---

title: Leetcode Log
date: 2021-02-24
author: Sanad Kadu

---


# Leetcode Patterns

## Tree BFS 
- What is BFS? #spaced 
- BFS Common Pattern

1. [Average of Levels in BT](https://leetcode.com/problems/average-of-levels-in-binary-tree) 


## Tree DFS
- What is DFS?
- DFS Common Pattern

1. [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
	- Imagine you are standing at the root of the binary tree and you need to find the shortest path from root to leaf. You move to either of the children of root and then again move to either of the children of children of the root and so on. All the while you remember the depth you're currently at and ...
		```java
		class Solution {
		    public int minDepth(TreeNode root) {
		        return dfs(root);
		    }
		    public int dfs(TreeNode root) {
		        if (root == null) {
		            return 0;
		        }
		        if (root.left == null && root.right == null) {
		            return 1;
		        }
		        int minDepth = Integer.MAX_VALUE;
		        if (root.right != null) minDepth = Math.min(minDepth, dfs(root.right));
		        if(root.left != null) minDepth = Math.min(minDepth, dfs(root.left));
		        return minDepth + 1;
		    }
		}
		```
