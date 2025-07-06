# Day 8 - July 6, 2025

## 141. Linked List Cycle

**Difficulty:** Easy  
**Topics:** Hash Table, Linked List, Two Pointers  
**Companies:** Amazon, Microsoft, Google, Facebook, Apple

---

## 🎯 Problem Statement

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

### Examples

**Example 1:**
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**
```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**
```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

### Constraints
- The number of the nodes in the list is in the range `[0, 10^4]`.
- `-10^5 <= Node.val <= 10^5`
- `pos` is `-1` or a **valid index** in the linked-list.

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

---

## 💡 The Classic Solution: Floyd's Cycle Detection (Tortoise and Hare)

**Key Insight:** Use two pointers moving at different speeds:
- **Slow pointer** (tortoise): moves 1 step at a time
- **Fast pointer** (hare): moves 2 steps at a time

If there's a cycle, the fast pointer will eventually catch up to the slow pointer! 🐢🐰

---

## 🐍 Python Solutions

### Solution 1: Floyd's Algorithm (Optimal)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        if not head or not head.next:
            return False
        
        # Two pointers: slow (1 step) and fast (2 steps)
        slow = head
        fast = head.next
        
        while fast and fast.next:
            if slow == fast:  # They meet = cycle detected!
                return True
            
            slow = slow.next        # Move 1 step
            fast = fast.next.next   # Move 2 steps
        
        return False  # Reached end = no cycle
```

### Solution 2: Cleaner Floyd's Algorithm
```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = fast = head
        
        while fast and fast.next:
            slow = slow.next        # 1 step
            fast = fast.next.next   # 2 steps
            
            if slow == fast:        # Meeting point = cycle!
                return True
        
        return False
```

### Solution 3: Using Hash Set (Less Efficient)
```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        visited = set()
        
        current = head
        while current:
            if current in visited:  # Already seen this node
                return True
            
            visited.add(current)
            current = current.next
        
        return False
```

### Solution 4: Node Modification (Tricky)
```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        while head:
            if head.val == float('inf'):  # Already visited
                return True
            
            head.val = float('inf')  # Mark as visited
            head = head.next
        
        return False
```

---

## 🔍 How Floyd's Algorithm Works

**Visual Example:** `[3,2,0,-4]` with cycle at position 1

```
Initial: 3 → 2 → 0 → -4
              ↑_______|

Step 1: slow=3, fast=2
Step 2: slow=2, fast=-4  
Step 3: slow=0, fast=2
Step 4: slow=-4, fast=0
Step 5: slow=2, fast=-4
Step 6: slow=0, fast=2
Step 7: slow=-4, fast=0
Step 8: slow=2, fast=-4
Step 9: slow=0, fast=2   ← They meet! Cycle detected! 🎯
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `[1,2]` with pos = 0 (cycle: 1 → 2 → 1)

| Step | Slow | Fast | Meet? |
|------|------|------|-------|
| 0    | 1    | 1    | Same start |
| 1    | 2    | 2    | ✅ Meet! |

**Result:** True (cycle detected)

---

## 🧠 Why Floyd's Algorithm Works

**Mathematical Proof:**
- If there's a cycle, both pointers will eventually enter it
- Inside the cycle, fast pointer gains 1 position per step on slow pointer
- Since cycle is finite, fast will eventually catch slow
- If no cycle, fast pointer reaches end (null)

**Analogy:** Think of a circular race track 🏁
- Slow runner: 1 lap per hour
- Fast runner: 2 laps per hour
- Eventually, fast runner will "lap" the slow runner!

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Floyd's Algorithm | O(n) | O(1) ✅ |
| Hash Set | O(n) | O(n) |
| Node Modification | O(n) | O(1) |

**Winner:** Floyd's Algorithm - optimal in both time and space!

---

## 🎯 Key Takeaways

1. **Two-pointer technique:** Classic approach for cycle detection
2. **Speed difference:** Fast pointer moves 2x faster than slow
3. **Meeting point:** If they meet, there's definitely a cycle
4. **Edge cases:** Empty list, single node, no cycle
5. **Space efficient:** O(1) space complexity

---

## 🔍 Edge Cases

```python
# Empty list
head = None → False

# Single node, no cycle
head = [1] → False

# Single node, self-cycle
head = [1] → True (if pos = 0)

# Two nodes, no cycle
head = [1,2] → False

# Two nodes, cycle
head = [1,2] with pos = 0 → True
```

---

## 💭 Alternative Approaches

```python
# Using try-catch (creative but not recommended)
def hasCycle(head):
    try:
        slow = head
        fast = head.next
        while slow is not fast:
            slow = slow.next
            fast = fast.next.next
        return True
    except:
        return False

# Using recursion (not space efficient)
def hasCycle(head, visited=None):
    if not head:
        return False
    if visited is None:
        visited = set()
    if head in visited:
        return True
    visited.add(head)
    return hasCycle(head.next, visited)
```

---

## 🔗 Related Problems
- 142. Linked List Cycle II (Find where cycle begins)
- 287. Find the Duplicate Number
- 202. Happy Number
- 876. Middle of the Linked List

---

## 🏆 Interview Tips

1. **Start with Floyd's Algorithm** - it's the expected solution
2. **Explain the intuition** - tortoise and hare analogy
3. **Handle edge cases** - empty list, single node
4. **Mention alternatives** - hash set approach
5. **Time/space complexity** - O(n) time, O(1) space

**Floyd's Cycle Detection is a must-know algorithm! 🚀**
