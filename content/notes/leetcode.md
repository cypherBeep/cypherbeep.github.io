---
title: LeetCode Notes
date: 2021-02-24
author: Sanad Kadu
---
# Table of Contents

1.  [Flow](#orgc329df9)
2.  [Core Data Structures](#orgb914c25)
3.  [Core Algorithms](#org809fa2a)
4.  [Patterns](#org04f3695)
    1.  [DFS](#org67db91d)
        1.  [Problems](#org0f066d6)
    2.  [Two Pointer](#org2d4a619)



<a id="orgc329df9"></a>

# Flow

1.  Ask for the runtime of the required solution.
2.  Filter out the data structures and algorithms that obviously aren’t relevant to the problem at hand. This will eliminate most of the list and you will usually be left with 2-3 data structures and algorithms.
    1.  You can filter out data structures that are too slow. If you need to solve a problem in O(1) time, it’s impossible to use a binary tree in the solution, because a binary tree will always take at least O(log n) time.
    2.  You can filter out algorithms if they are impossible to use for the given problem. If there isn’t a graph in the problem, you know that depth-first search can’t be relevant.
3.  Go through the use cases for the remaining data structures and see which ones are relevant to the problem at hand. The solution for the problem is going to be a combination of these use cases. All you need to do is piece together these different use cases and you’ll come up with the solution to the problem.


<a id="orgb914c25"></a>

# Core Data Structures

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Structure</th>
<th scope="col" class="org-left">Runtime</th>
<th scope="col" class="org-left">Use Case</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">- O(1) Deletion</td>
<td class="org-left">- to partition a list of objects distinct groups.</td>
</tr>


<tr>
<td class="org-left">Hash Tables</td>
<td class="org-left">- O(1) Insertion</td>
<td class="org-left">- to store and lookup objects.</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">- O(1) Lookup</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">- O(1) Lookup</td>
<td class="org-left">- mostly used as stack or queue in interviews</td>
</tr>


<tr>
<td class="org-left">Linked List</td>
<td class="org-left">- O(1) Insertion</td>
<td class="org-left">- as it maintians the relative ordering of elements.</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">- O(1) Deletion</td>
<td class="org-left">* this is only if we have pointer to the location.</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">- O(log n) Deletion</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">Binary Trees</td>
<td class="org-left">- O(log n) Insertion</td>
<td class="org-left">- when you need to keep your data in sorted order</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">- O(log n) Lookup</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org809fa2a"></a>

# Core Algorithms

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Algorithm</th>
<th scope="col" class="org-left">Runtime</th>
<th scope="col" class="org-left">Use Case</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">- to traverse an entire graph.</td>
</tr>


<tr>
<td class="org-left">DFS</td>
<td class="org-left">- O(n)</td>
<td class="org-left">- to find an element in graph.</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">- to find connected components of graph.</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">- to process elements in certain order</td>
</tr>


<tr>
<td class="org-left">Sorting</td>
<td class="org-left">- O(n log n)</td>
<td class="org-left">- to sort an array beforehand for binary search.</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">- to find a number in sorted array.</td>
</tr>


<tr>
<td class="org-left">Binary Search</td>
<td class="org-left">- O(log n)</td>
<td class="org-left">- to find the smallest number in a sorted array.</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">- to check if array is sorted</td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">- to find the largest number in a sorted array.</td>
</tr>
</tbody>
</table>


<a id="org04f3695"></a>

# Patterns


<a id="org67db91d"></a>

## DFS

-   We use recursion to keep track of all previous (parent) nodes.
-   Complexity is O(n) where n is the height of the tree.


<a id="org0f066d6"></a>

### Problems

1.  Binary Tree Sum Path Pattern

    1.  [Path Sum](https://leetcode.com/problems/path-sum/)
    
        -   A good intro problem to DFS. We need to return a t/f value if we have a path whose sum is equal to given target sum.
        -   So, we make need to make two recursive calls on each node to get path sums. We substract the value of the node from targetSum and if targetSum in the end becomes 0 then we have a path whose sum of values in equal to target sum.
            
                use std::rc::Rc;
                use std::cell::RefCell;
                impl Solution {
                    pub fn has_path_sum(root: Option<Rc<RefCell<TreeNode>>>, target_sum: i32) -> bool {
                        if root.is_none() {
                            return false;
                        }
                        match root {
                            Some(x) => {
                                let node = x.borrow();
                                if target_sum - node.val == 0 && node.left.is_none() && node.right.is_none() {
                                    return true;
                                }
                                return Self::has_path_sum(node.left.clone(), target_sum-node.val) || Self::has_path_sum(node.right.clone(), target_sum-node.val)
                            },
                            _ => return false,
                        }
                    }
                }
    
    2.  [All Paths](https://leetcode.com/problems/path-sum-ii/solution/)
    
        -   Same pattern as previous problem but this time we need to return all the paths from root to leaf whose values add up to the target sum.
        -   Caveat in my opinion is to remove the value of node from current path after all of its children have been checked. Why? Because to clear the call stack (the results of previous recursive calls) so as to get back to the parents and free up the &ldquo;current&rdquo; list.
            
                use std::rc::Rc;
                use std::cell::RefCell;
                impl Solution {
                    pub fn path_sum(root: Option<Rc<RefCell<TreeNode>>>, target_sum: i32) -> Vec<Vec<i32>> {
                        let mut all: Vec<Vec<i32>> = vec![];
                        let mut current_path = vec![];
                        Self::find_path(root, target_sum, &mut all, &mut current_path);
                        all
                    }
                    pub fn find_path(node: Option<Rc<RefCell<TreeNode>>>, target_sum: i32, all: &mut Vec<Vec<i32>>, current: &mut Vec<i32>) {
                        match node {
                            Some(x) => {
                                let cur = x.borrow();
                                current.push(cur.val);
                                if cur.val == target_sum && cur.left.is_none() && cur.right.is_none(){
                                    all.push(current.to_vec());
                                } else {
                                    Self::find_path(cur.right.clone(), target_sum - cur.val, all, current);
                                    Self::find_path(cur.left.clone(), target_sum - cur.val, all, current);
                                }
                                // This part.
                                current.remove(current.len()-1);
                            },
                            _ => return,
                        }
                    }
                
                }
    
    3.  [Path with given sequence](https://leetcode.com/problems/check-if-a-string-is-a-valid-sequence-from-root-to-leaves-path-in-a-binary-tree/)
    
        -   Need to check if any path from root to leaf matches the given sequence.
        -   We use the same pattern as previous problems i.e check every node and return false when its value does not match the sequence.
            
                use std::rc::Rc;
                use std::cell::RefCell;
                #[allow(dead_code)]
                impl Solution {
                    pub fn is_valid_sequence(root: Option<Rc<RefCell<TreeNode>>>, arr: Vec<i32>) -> bool {
                        if root.is_none() {
                            return false;
                        }
                        Self::check(&root, &arr, 0)
                    }
                    pub fn check(node: &Option<Rc<RefCell<TreeNode>>>, seq: &Vec<i32>, seq_idx: usize) -> bool {
                        if let Some(node) = node {
                            let node = node.borrow();
                            let left = &node.left;
                            let right = &node.right;
                            if seq_idx >= seq.len() || node.val != seq[seq_idx]{
                                return false;
                            }
                            if left.is_none() && right.is_none() && seq_idx == seq.len() - 1 && node.val == seq[seq_idx] {
                                return true;
                            }
                            Self::check(&node.left, seq, seq_idx+1) || Self::check(&node.right, seq, seq_idx+1)
                        } else {
                            false
                        }
                    }
                }
    
    4.  TODO [Number of Paths with given sum](https://leetcode.com/problems/path-sum-iii/solution/)
    
    5.  TODO [Tree Diameter](https://leetcode.com/problems/tree-diameter/)
    
        -   Need to find the diameter of a tree which is the number of edges in the longest path in that tree.


<a id="org2d4a619"></a>

## Two Pointer

-   Mainly used for **sorted** array and linked list problems where we need to find elements that fullfill certain conditions.

1.  [Two Sum || Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

    -   We use beg and end pointers and increment beg if sum if lower than target and decrement end if sum is greater than target.
        
            impl Solution {
                pub fn two_sum(numbers: Vec<i32>, target: i32) -> Vec<i32> {
                    let mut ans: Vec<i32> = vec![];
                    let mut beg = 0;
                    let mut end = numbers.len() - 1;
            
                    while beg < end {
                        let sum = numbers[beg] + numbers[end];
                        if sum == target {
                            ans.push((beg+1) as i32);
                            ans.push((end+1) as i32);
                            return ans;
                        }
                        if sum < target {
                            beg += 1;
                        }
                        if sum > target {
                            end -= 1;
                        }
                    }
                    ans
                }
            }

2.  [Remove duplicates from sorted array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

    -   So we need to do this in-place, one way to do this is to shift the elements left whenever we encounter duplicates. In other words, we will keep one pointer for iterating the array and one pointer for placing the next non-duplicate number. Thus, the value of next<sub>dup</sub> would be length of sorted non duplicate array.
        
            impl Solution {
                pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
                    let mut next_dup = 1;
                    for i in 1..nums.len() {
                        if nums[next_dup - 1] != nums[i] {
                            nums[next_dup] = nums[i];
                            next_dup += 1;
                        }
                    }
                    next_dup as i32
                }
            }

3.  [Squares of a sorted array](https://leetcode.com/problems/squares-of-a-sorted-array/)

    -   We are given a sorted array with negative and positive numbers and we need to return an array of squares of elements in sorted order in O(n).
    -   So we take two pointers, beg and end, then the logic is to check squares of numbers at positions beg and end. As we need to return an array in sorted order we create a new array and add elements after comparing at beg and end like, if square of beg is > square of end then we add square of beg at end of new array and move beg forward as its greater than square of biggest number in given sorted array. If however square of beg smaller that square of end then we add square of end and move end backwards.
        
            class Solution {
                public int[] sortedSquares(int[] nums) {
                    int[] res = new int[nums.length];
                    int idx = nums.length - 1;
                    int beg = 0;
                    int end = nums.length - 1;
                    while(beg <= end) {
                        int left_sqr = nums[beg] * nums[beg];
                        int right_sqr = nums[end] * nums[end];
                        if(left_sqr > right_sqr) {
                            res[idx--] = left_sqr;
                            beg++;
                        } else {
                            res[idx--] = right_sqr;
                            end--;
                        }
                    }
                    return res;
                }
            }

4.  TODO [3 Sum](https://leetcode.com/problems/3sum/)

    ****\*****

