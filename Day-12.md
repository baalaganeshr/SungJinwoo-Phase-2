Binary Tree Postorder Traversal
Date: 10/7/25
Day: 12

Problem Description
Given the root of a binary tree, return the postorder traversal of its nodes' values.

Postorder traversal means:

Traverse the left subtree

Traverse the right subtree

Visit the root node

Examples
Example 1:
Input: root = [1,null,2,3]
Output: [3, 2, 1]

Example 2:
Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]
Output: [4,6,7,5,2,9,8,3,1]

Example 3:
Input: root = []
Output: []

Example 4:
Input: root = [1]
Output: [1]

Constraints
The number of nodes in the tree is in the range [0, 100].

Node values are between -100 and 100.

Explanation
Postorder traversal means we visit nodes in the order: left child, right child, then the node itself.

A recursive solution is straightforward, but the problem asks if you can do it iteratively (without recursion).

Python Solutions
Recursive Solution (Simple)
python
Copy
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        return self.postorderTraversal(root.left) + self.postorderTraversal(root.right) + [root.val],
