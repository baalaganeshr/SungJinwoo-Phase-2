# Day 6 - July 4, 2025

## 136. Single Number

**Difficulty:** Easy  
**Topics:** Array, Bit Manipulation  
**Companies:** Amazon,  Microsoft, Google, Apple, Facebook

---

## 🎯 Problem Statement

Given a **non-empty** array of integers `nums`, every element appears *twice* except for one. Find that single one.

**Important:** You must implement a solution with linear runtime complexity and use only constant extra space.

### Examples

**Example 1:**
```
Input: nums = [2,2,1]
Output: 1
```

**Example 2:**
```
Input: nums = [4,1,2,1,2]
Output: 4
```

**Example 3:**
```
Input: nums = [1]
Output: 1
```

### Constraints
- `1 <= nums.length <= 3 * 10^4`
- `-3 * 10^4 <= nums[i] <= 3 * 10^4`
- Each element in the array appears twice except for one element which appears only once.

---

## 💡 The Magic Solution: XOR

**Key Insight:** XOR has a special property:
- `a ^ a = 0` (any number XOR with itself = 0)
- `a ^ 0 = a` (any number XOR with 0 = itself)
- XOR is commutative: `a ^ b = b ^ a`

So if we XOR all numbers together, the duplicates cancel out! 🎉

---

## 🐍 Python Solutions

### Solution 1: XOR Magic (Optimal)
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        result = 0
        for num in nums:
            result ^= num  # XOR with each number
        return result
```

### Solution 2: One-liner XOR
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        return reduce(lambda x, y: x ^ y, nums)
```

### Solution 3: Using Python's Built-in
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        from functools import reduce
        import operator
        return reduce(operator.xor, nums)
```

### Solution 4: Set Approach (Less Efficient)
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        seen = set()
        for num in nums:
            if num in seen:
                seen.remove(num)  # Remove if seen before
            else:
                seen.add(num)     # Add if first time
        return seen.pop()         # Return the only remaining element
```

---

## 🔍 How XOR Works

**Example:** `[4,1,2,1,2]`

```
Step by step XOR:
result = 0
result = 0 ^ 4 = 4
result = 4 ^ 1 = 5
result = 5 ^ 2 = 7
result = 7 ^ 1 = 6
result = 6 ^ 2 = 4

Final result: 4 ✅
```

**Why it works:**
```
4 ^ 1 ^ 2 ^ 1 ^ 2
= 4 ^ (1 ^ 1) ^ (2 ^ 2)  # Group duplicates
= 4 ^ 0 ^ 0              # Duplicates cancel out
= 4                      # Single number remains!
```

---

## 📊 Visual Explanation

**Array:** `[2,2,1]`

| Step | Current | XOR Operation | Result |
|------|---------|---------------|---------|
| 1    | 0       | 0 ^ 2         | 2       |
| 2    | 2       | 2 ^ 2         | 0       |
| 3    | 0       | 0 ^ 1         | 1       |

**Final Answer:** 1 🎯

---

## 🧠 XOR Truth Table

| A | B | A ^ B |
|---|---|-------|
| 0 | 0 | 0     |
| 0 | 1 | 1     |
| 1 | 0 | 1     |
| 1 | 1 | 0     |

**Key Property:** `A ^ A = 0` (same bits cancel out)

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| XOR Magic | O(n) | O(1) ✅ |
| Set Approach | O(n) | O(n) |
| Sorting + Check | O(n log n) | O(1) |

**Winner:** XOR approach - meets all requirements perfectly!

---

## 🎯 Key Takeaways

1. **XOR Properties:** 
   - `a ^ a = 0` (duplicates cancel)
   - `a ^ 0 = a` (identity)
   - Commutative and associative

2. **Perfect for this problem:**
   - Linear time O(n)
   - Constant space O(1)
   - Works with negative numbers too!

3. **Real-world application:** Error detection, cryptography, finding differences

---

## 🔗 Related Problems
- 137. Single Number II
- 260. Single Number III
- 268. Missing Number
- 287. Find the Duplicate Number

---

## 💭 Alternative Approaches (Less Efficient)

```python
# Using Counter (O(n) space)
from collections import Counter
def singleNumber(nums):
    count = Counter(nums)
    for num, freq in count.items():
        if freq == 1:
            return num

# Using Sum Math (O(n) space)  
def singleNumber(nums):
    return 2 * sum(set(nums)) - sum(nums)
```

**But XOR is the cleanest and most efficient! 🚀**
