# Day 9 - July 8, 2025

## 144. Binary Tree Preorder Traversal

**Difficulty:** Easy  
**Topics:** Stack, Tree, Depth-First Search, Binary Tree  
**Companies:** Microsoft, Amazon, Google, Facebook, Apple

---

## 🎯 Problem Statement

Given the `root` of a binary tree, return *the preorder traversal of its nodes' values*.

**Preorder Traversal:** Root → Left → Right

### Examples

**Example 1:**
```
Input: root = [1,null,2,3]
Output: [1,2,3]

Tree:
   1
    \
     2
    /
   3
```

**Example 2:**
```
Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]
Output: [1,2,4,5,6,7,3,8,9]

Tree:
       1
      / \
     2   3
    / \   \
   4   5   8
      / \   \
     6   7   9
```

**Example 3:**
```
Input: root = []
Output: []
```

**Example 4:**
```
Input: root = [1]
Output: [1]
```

### Constraints
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

**Follow up:** Recursive solution is trivial, could you do it iteratively?

---

## 💡 Understanding Preorder Traversal

**Order:** Root → Left Subtree → Right Subtree

**Memory Trick:** **"Pre"** means we process the root **before** its children!

```
For any node:
1. Process current node (add to result)
2. Traverse left subtree
3. Traverse right subtree
```

---

## 🐍 Python Solutions

### Solution 1: Recursive (Simple & Clean)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        
        def dfs(node):
            if not node:
                return
            
            result.append(node.val)    # Root
            dfs(node.left)             # Left
            dfs(node.right)            # Right
        
        dfs(root)
        return result
```

### Solution 2: Iterative with Stack (Optimal)
```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        result = []
        stack = [root]
        
        while stack:
            node = stack.pop()
            result.append(node.val)
            
            # Push right first, then left (stack is LIFO)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        
        return result
```

### Solution 3: Iterative with Explicit State
```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        result = []
        stack = [(root, False)]  # (node, processed)
        
        while stack:
            node, processed = stack.pop()
            
            if processed:
                result.append(node.val)
            else:
                # Push in reverse order: right, left, root
                if node.right:
                    stack.append((node.right, False))
                if node.left:
                    stack.append((node.left, False))
                stack.append((node, True))  # Mark as processed
        
        return result
```

### Solution 4: Morris Traversal (Advanced - O(1) Space)
```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        current = root
        
        while current:
            if not current.left:
                # No left child, process current and go right
                result.append(current.val)
                current = current.right
            else:
                # Find inorder predecessor
                predecessor = current.left
                while predecessor.right and predecessor.right != current:
                    predecessor = predecessor.right
                
                if not predecessor.right:
                    # Make current the right child of predecessor
                    result.append(current.val)  # Process before going left
                    predecessor.right = current
                    current = current.left
                else:
                    # Revert the changes
                    predecessor.right = None
                    current = current.right
        
        return result
```

---

## 📊 Step-by-Step Walkthrough

**Example:** Tree `[1,2,3,4,5]`

```
    1
   / \
  2   3
 / \
4   5
```

### Recursive Approach:
```
1. Visit 1 → result = [1]
2. Go left to 2 → result = [1,2]
3. Go left to 4 → result = [1,2,4]
4. 4 has no children, return
5. Go right to 5 → result = [1,2,4,5]
6. 5 has no children, return
7. Go right to 3 → result = [1,2,4,5,3]
8. 3 has no children, return

Final: [1,2,4,5,3]
```

### Iterative Approach:
```
Stack: [1]
Pop 1, add to result: [1]
Push right(3), left(2): Stack = [3,2]

Stack: [3,2]
Pop 2, add to result: [1,2]
Push right(5), left(4): Stack = [3,5,4]

Stack: [3,5,4]
Pop 4, add to result: [1,2,4]
No children: Stack = [3,5]

Stack: [3,5]
Pop 5, add to result: [1,2,4,5]
No children: Stack = [3]

Stack: [3]
Pop 3, add to result: [1,2,4,5,3]
No children: Stack = []

Final: [1,2,4,5,3]
```

---

## 🔍 Traversal Comparison

| Type | Order | Example Tree Result |
|------|-------|-------------------|
| **Preorder** | Root → Left → Right | [1,2,4,5,3] |
| **Inorder** | Left → Root → Right | [4,2,5,1,3] |
| **Postorder** | Left → Right → Root | [4,5,2,3,1] |

**Memory Trick:**
- **Pre**: Process root **before** children
- **In**: Process root **in between** children  
- **Post**: Process root **after** children

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Recursive | O(n) | O(h) - call stack |
| Iterative (Stack) | O(n) | O(h) - explicit stack |
| Morris Traversal | O(n) | O(1) ✅ |

**Where h = height of tree**
- Best case: O(log n) for balanced tree
- Worst case: O(n) for skewed tree

---

## 🎯 Key Takeaways

1. **Preorder = Root first:** Process current node before its children
2. **Recursive is natural:** Matches the definition perfectly
3. **Stack simulates recursion:** LIFO order, push right then left
4. **Morris is advanced:** O(1) space using tree restructuring
5. **Foundation for DFS:** Many tree problems use preorder traversal

---

## 🧠 Visual Memory Aid

```
    ROOT
   /    \
LEFT   RIGHT

Preorder: "I process myself FIRST, then my children"
1. Process ROOT  ← "Pre" = before children
2. Process LEFT
3. Process RIGHT
```

---

## 🔍 Edge Cases

```python
# Empty tree
root = None → []

# Single node
root = [1] → [1]

# Only left children (linked list)
root = [1,[2,[3,null,null],null],null] → [1,2,3]

# Only right children
root = [1,null,[2,null,[3,null,null]]] → [1,2,3]

# Complete binary tree
root = [1,2,3,4,5,6,7] → [1,2,4,5,3,6,7]
```

---

## 💭 Common Use Cases

1. **Tree copying:** Create a copy of the tree
2. **Expression trees:** Evaluate prefix expressions
3. **Directory traversal:** Process folder before contents
4. **Tree serialization:** Convert tree to string format
5. **Syntax tree parsing:** Process operators before operands

---

## 🔗 Related Problems
- 94. Binary Tree Inorder Traversal
- 145. Binary Tree Postorder Traversal
- 102. Binary Tree Level Order Traversal
- 589. N-ary Tree Preorder Traversal

---

## 🏆 Interview Tips

1. **Start with recursive** - it's the most intuitive
2. **Explain the order** - Root, Left, Right
3. **Draw the tree** - visual helps with explanation
4. **Mention iterative** - shows you understand stack simulation
5. **Discuss space optimization** - Morris traversal for bonus points

**Preorder is the foundation of tree traversal! 🚀**
