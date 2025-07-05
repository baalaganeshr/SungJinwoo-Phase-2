# Day 7 - July 5, 2025

## 1394. Find Lucky Integer in an Array

**Difficulty:** Easy  
**Topics:** Array, Hash Table, Counting  
**Companies:** Amazon, Microsoft, Google

---

## 🎯 Problem Statement

Given an array of integers `arr`, a **lucky integer** is an integer that has a frequency in the array equal to its value.

Return *the largest ****lucky integer**** in the array*. If there is no **lucky integer** return `-1`.

### Examples

**Example 1:**
```
Input: arr = [2,2,3,4]
Output: 2
Explanation: The only lucky number in the array is 2 because frequency[2] == 2.
```

**Example 2:**
```
Input: arr = [1,2,2,3,3,3]
Output: 3
Explanation: 1, 2 and 3 are all lucky numbers, return the largest of them.
```

**Example 3:**
```
Input: arr = [2,2,2,3,3]
Output: -1
Explanation: There are no lucky numbers in the array.
```

### Constraints
- `1 <= arr.length <= 500`
- `1 <= arr[i] <= 500`

---

## 💡 Simple Strategy

1. **Count frequency** of each number
2. **Check if number equals its frequency** (lucky condition)
3. **Return the largest** lucky number found

**Lucky condition:** `number == frequency[number]`

---

## 🐍 Python Solutions

### Solution 1: Simple and Clean
```python
class Solution:
    def findLucky(self, arr: List[int]) -> int:
        from collections import Counter
        
        # Count frequency of each number
        freq = Counter(arr)
        
        # Find all lucky numbers and return the largest
        lucky_numbers = [num for num, count in freq.items() if num == count]
        
        return max(lucky_numbers) if lucky_numbers else -1
```

### Solution 2: One Pass Solution
```python
class Solution:
    def findLucky(self, arr: List[int]) -> int:
        from collections import Counter
        
        freq = Counter(arr)
        max_lucky = -1
        
        # Check each number if it's lucky
        for num, count in freq.items():
            if num == count:  # Lucky condition
                max_lucky = max(max_lucky, num)
        
        return max_lucky
```

### Solution 3: Manual Counting (No Imports)
```python
class Solution:
    def findLucky(self, arr: List[int]) -> int:
        # Manual frequency counting
        freq = {}
        for num in arr:
            freq[num] = freq.get(num, 0) + 1
        
        # Find largest lucky number
        max_lucky = -1
        for num, count in freq.items():
            if num == count:
                max_lucky = max(max_lucky, num)
        
        return max_lucky
```

### Solution 4: Using Dictionary with Default
```python
class Solution:
    def findLucky(self, arr: List[int]) -> int:
        from collections import defaultdict
        
        freq = defaultdict(int)
        for num in arr:
            freq[num] += 1
        
        # Find the largest lucky number
        result = -1
        for num in freq:
            if num == freq[num]:
                result = max(result, num)
        
        return result
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `[1,2,2,3,3,3]`

### Step 1: Count Frequencies
```
Number | Frequency
-------|----------
   1   |    1     ← Lucky! (1 == 1)
   2   |    2     ← Lucky! (2 == 2)  
   3   |    3     ← Lucky! (3 == 3)
```

### Step 2: Check Lucky Condition
```
1: frequency = 1 → 1 == 1 ✅ Lucky!
2: frequency = 2 → 2 == 2 ✅ Lucky!
3: frequency = 3 → 3 == 3 ✅ Lucky!
```

### Step 3: Return Largest
```
Lucky numbers: [1, 2, 3]
Largest: 3 ✅
```

---

## 🔍 More Examples

**Example:** `[2,2,2,3,3]`

| Number | Frequency | Lucky? | Why? |
|--------|-----------|--------|------|
| 2      | 3         | ❌     | 2 ≠ 3 |
| 3      | 2         | ❌     | 3 ≠ 2 |

**Result:** -1 (no lucky numbers)

---

**Example:** `[7,7,7,7,7,7,7]`

| Number | Frequency | Lucky? | Why? |
|--------|-----------|--------|------|
| 7      | 7         | ✅     | 7 == 7 |

**Result:** 7 (the only lucky number)

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Counter + List | O(n) | O(n) |
| One Pass | O(n) | O(n) |
| Manual Counting | O(n) | O(n) |

**All solutions are equally efficient!** Choose based on readability preference.

---

## 🎯 Key Takeaways

1. **Lucky Definition:** `number == frequency[number]`
2. **Two-step process:** Count frequencies, then check lucky condition
3. **Edge case:** Return -1 if no lucky numbers exist
4. **Multiple solutions:** Counter, manual counting, or one-pass approach

---

## 🧠 Mental Model

Think of it as a **matching game**:
- Count how many times each number appears
- Check if the number "matches" its count
- Pick the biggest match!

```
Number 3 appears 3 times → Perfect match! ✅
Number 2 appears 5 times → No match ❌
```

---

## 🔗 Related Problems
- 387. First Unique Character in a String
- 448. Find All Numbers Disappeared in an Array
- 1512. Number of Good Pairs
- 1365. How Many Numbers Are Smaller Than the Current Number

---

## 💭 Alternative Approaches

```python
# Using sorted() for cleaner max finding
def findLucky(arr):
    from collections import Counter
    freq = Counter(arr)
    lucky = [num for num, count in freq.items() if num == count]
    return max(lucky) if lucky else -1

# Using filter() for functional approach
def findLucky(arr):
    from collections import Counter
    freq = Counter(arr)
    lucky = list(filter(lambda x: x[0] == x[1], freq.items()))
    return max(lucky)[0] if lucky else -1
```

**Simple is best! 🚀**
