# Day 1: Number of Valid Subsequences (29/6/25)

## Problem Statement
Given an array `nums` and integer `target`, count the number of non-empty subsequences where the sum of the minimum and maximum elements ≤ target. Return count modulo 10⁹ + 7.

**Constraints:**
- 1 ≤ nums.length ≤ 10⁵
- 1 ≤ nums[i] ≤ 10⁶
- 1 ≤ target ≤ 10⁶

## Examples

Input: nums = [3,5,6,7], target = 9 → Output: 4
Input: nums = [3,3,6,8], target = 10 → Output: 6
Input: nums = [2,3,3,4,6,7], target = 12 → Output: 61

```python
class Solution:
    def numSubseq(self, nums: List[int], target: int) -> int:
        MOD = 10**9 + 7
        nums.sort()
        n = len(nums)
        res = 0
        left, right = 0, n - 1
        
        # Precompute powers of 2 modulo MOD
        power = [1] * (n + 1)
        for i in range(1, n + 1):
            power[i] = (power[i - 1] * 2) % MOD
        
        while left <= right:
            if nums[left] + nums[right] <= target:
                res = (res + power[right - left]) % MOD
                left += 1
            else:
                right -= 1
        return res

```

# Day 2: Longest Harmonious Subsequence (30/6/25)

## Problem Statement
Find the length of the longest subsequence where the difference between maximum and minimum values is exactly 1.

**Constraints:**
- 1 ≤ nums.length ≤ 2 × 10⁴
- -10⁹ ≤ nums[i] ≤ 10⁹

## Examples

Input: [1,3,2,2,5,2,3,7] → Output: 5 (subsequence [3,2,2,2,3])
Input: [1,2,3,4] → Output: 2 (subsequences [1,2], [2,3], or [3,4])
Input: [1,1,1,1] → Output: 0 (no valid subsequence)

```python
from collections import defaultdict

class Solution:
    def findLHS(self, nums: List[int]) -> int:
        freq = defaultdict(int)
        max_length = 0
        
        # Count frequencies
        for num in nums:
            freq[num] += 1
        
        # Check pairs
        for num in freq:
            if num + 1 in freq:
                max_length = max(max_length, freq[num] + freq[num + 1])
        
        return max_length


```
# Day 3: Find the Original Typed String I (1/7/25)

## Problem Statement
Given a string `word` where Alice might have pressed one key too long (at most once), return the number of possible original strings she might have intended to type.

**Constraints:**
- 1 ≤ word.length ≤ 100
- word consists of lowercase English letters

## Examples

Input: "abbcccc" → Output: 5
Possible originals: "abbcccc", "abbccc", "abbcc", "abbc", "abcccc"

Input: "abcd" → Output: 1
Only possible: "abcd"

Input: "aaaa" → Output: 4
Possible originals: "aaaa", "aaa", "aa", "a"


```python
class Solution:
    def possibleStringCount(self, word: str) -> int:
        if not word:
            return 0
        
        count = 0
        n = len(word)
        i = 0
        
        while i < n:
            j = i
            while j < n and word[j] == word[i]:
                j += 1
            run_length = j - i
            if run_length > 1:
                count += run_length - 1
            i = j
        
        # Always include the original string, plus each possible single reduction
        return count + 1

```
## Day-4  (2/7/25)
# 💰 Best Time to Buy and Sell Stock - Simple Solution

## 🎯 The Problem
Find the best day to buy and sell a stock to make maximum profit.

**Example:** `[7,1,5,3,6,4]` → Answer: `5` (buy at 1, sell at 6)

---

## 💡 Simple Strategy
1. **Remember the cheapest price** we've seen
2. **Check profit** if we sell today
3. **Keep track** of the best profit

---

## 🐍 Python Code

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = prices[0]  # Cheapest price so far
        max_profit = 0         # Best profit so far
        
        for price in prices[1:]:
            min_price = min(min_price, price)      # Update cheapest
            profit = price - min_price             # Today's profit
            max_profit = max(max_profit, profit)   # Update best profit
        
        return max_profit
```

---

## 📊 How It Works

**Array:** `[7, 1, 5, 3, 6, 4]`

| Day | Price | Cheapest | Profit | Best Profit |
|-----|-------|----------|--------|-------------|
| 0   | 7     | 7        | -      | 0           |
| 1   | 1     | 1        | 0      | 0           |
| 2   | 5     | 1        | 4      | 4           |
| 3   | 3     | 1        | 2      | 4           |
| 4   | 6     | 1        | **5**  | **5**       |
| 5   | 4     | 1        | 3      | 5           |

**Result:** Buy at $1, sell at $6 = $5 profit! 🎉

---

## ⚡ Why This Works
- We can only sell **after** we buy
- So we track the **lowest price** up to today
- Then check: "What if I sell today?"
- Keep the **best profit** we find

**Time:** O(n) - one pass  
**Space:** O(1) - just two variables


# Day 5 - July 3, 2025

## 125. Valid Palindrome

**Difficulty:** Easy  
**Topics:** Two Pointers, String  
**Companies:** Facebook, Microsoft, Amazon, Google, Apple

---

## 🎯 Problem Statement

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward.

Given a string `s`, return `true` if it is a **palindrome**, or `false` otherwise.

### Examples

**Example 1:**
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**
```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**
```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

### Constraints
- `1 <= s.length <= 2 * 10^5`
- `s` consists only of printable ASCII characters.

---

## 💡 Simple Strategy

1. **Clean the string**: Remove non-alphanumeric characters and convert to lowercase
2. **Two pointers**: Compare characters from both ends moving inward
3. **Check match**: If all characters match, it's a palindrome!

---

## 🐍 Python Solutions

### Solution 1: Simple and Clean
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Step 1: Clean the string - keep only alphanumeric and lowercase
        cleaned = ''.join(char.lower() for char in s if char.isalnum())
        
        # Step 2: Check if cleaned string equals its reverse
        return cleaned == cleaned[::-1]
```

### Solution 2: Two Pointers (Space Efficient)
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Two pointers from both ends
        left, right = 0, len(s) - 1
        
        while left < right:
            # Skip non-alphanumeric characters from left
            while left < right and not s[left].isalnum():
                left += 1
            
            # Skip non-alphanumeric characters from right
            while left < right and not s[right].isalnum():
                right -= 1
            
            # Compare characters (case-insensitive)
            if s[left].lower() != s[right].lower():
                return False
            
            left += 1
            right -= 1
        
        return True
```

### Solution 3: Step-by-Step (For Understanding)
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Step 1: Convert to lowercase
        s = s.lower()
        
        # Step 2: Filter only alphanumeric characters
        alphanumeric_only = ""
        for char in s:
            if char.isalnum():  # Keep letters and numbers only
                alphanumeric_only += char
        
        # Step 3: Check if string reads same forwards and backwards
        return alphanumeric_only == alphanumeric_only[::-1]
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `"A man, a plan, a canal: Panama"`

### Method 1: Clean First
```
Original: "A man, a plan, a canal: Panama"
Step 1: Convert to lowercase → "a man, a plan, a canal: panama"
Step 2: Keep only alphanumeric → "amanaplanacanalpanama"
Step 3: Check if palindrome → "amanaplanacanalpanama" == "amanaplanacanalpanama"
Result: True ✅
```

### Method 2: Two Pointers
```
Original: "A man, a plan, a canal: Panama"
         ↑                            ↑
       left                        right

Compare: 'A' vs 'a' → same (case-insensitive) ✅
Move pointers inward, skip non-alphanumeric...
Continue until pointers meet
Result: True ✅
```

---

## 🔍 Edge Cases

| Input | Cleaned | Result | Explanation |
|-------|---------|--------|-------------|
| `""` | `""` | `True` | Empty string is palindrome |
| `" "` | `""` | `True` | Only spaces → empty after cleaning |
| `"a"` | `"a"` | `True` | Single character is palindrome |
| `"Aa"` | `"aa"` | `True` | Case-insensitive |
| `"ab"` | `"ab"` | `False` | Different characters |

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Clean First | O(n) | O(n) |
| Two Pointers | O(n) | O(1) |
| Step-by-Step | O(n) | O(n) |

**Best Choice:** Two Pointers method for O(1) space complexity!

---

## 🎯 Key Takeaways

1. **Preprocessing**: Clean the string by removing non-alphanumeric characters
2. **Case-insensitive**: Convert to lowercase for comparison
3. **Two approaches**: 
   - Clean first, then compare (simple but uses extra space)
   - Two pointers (more efficient, constant space)
4. **Edge cases**: Empty strings and single characters are palindromes

---

## 🔗 Related Problems
- 9. Palindrome Number
- 234. Palindrome Linked List  
- 680. Valid Palindrome II
- 1332. Remove Palindromic Subsequences
  1333. 




  
# Day 6 - July 4, 2025

## 136. Single Number

**Difficulty:** Easy  
**Topics:** Array, Bit Manipulation  
**Companies:** Amazon, Microsoft, Google, Apple, Facebook

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


# Day 9 - July 7, 2025

## 1353. Maximum Number of Events That Can Be Attended

**Difficulty:** Medium  
**Topics:** Array, Greedy, Sorting, Heap (Priority Queue)  
**Companies:** Google, Microsoft, Amazon

---

## 🎯 Problem Statement

You are given an array of events where `events[i] = [startDay, endDay]`. Every event i starts at `startDay` and ends at `endDay`.

You can attend an event i at any day d where `startDay <= d <= endDay`. You can only attend one event at any time d.

Return the maximum number of events you can attend.

### Examples

**Example 1:**
```
Input: events = [[1,2],[2,3],[3,4]]
Output: 3
Explanation: You can attend all the three events.
One way to attend them all is as shown.
Attend the first event on day 1.
Attend the second event on day 2.
Attend the third event on day 3.
```

**Example 2:**
```
Input: events = [[1,2],[2,3],[3,4],[1,2]]
Output: 4
Explanation: You can attend all four events by attending them optimally.
```

### Constraints
- `1 <= events.length <= 10^5`
- `events[i].length == 2`
- `1 <= startDay <= endDay <= 10^5`

---

## 💡 Greedy Strategy

**Key Insight:** Always attend the event that ends earliest among available events on each day.

**Why?** Events that end sooner have fewer opportunities to be attended later, so we should prioritize them.

### Algorithm Steps:
1. **Sort events** by start day
2. **For each day**, collect all events that start on that day
3. **Remove expired events** from consideration
4. **Attend the event that ends earliest** (greedy choice)

---

## 🐍 Python Solutions

### Solution 1: Greedy with Heap (Optimal)
```python
import heapq

class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        # Sort events by start day
        events.sort()
        
        min_heap = []  # Store end days of available events
        day = 1
        event_idx = 0
        attended = 0
        
        # Continue until no more events to process
        while event_idx < len(events) or min_heap:
            # Add all events that start on current day
            while event_idx < len(events) and events[event_idx][0] == day:
                heapq.heappush(min_heap, events[event_idx][1])  # Add end day
                event_idx += 1
            
            # Remove expired events (ended before current day)
            while min_heap and min_heap[0] < day:
                heapq.heappop(min_heap)
            
            # Attend the event that ends earliest
            if min_heap:
                heapq.heappop(min_heap)  # Attend this event
                attended += 1
            
            day += 1
        
        return attended
```

### Solution 2: Cleaner Implementation
```python
import heapq

class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        events.sort()  # Sort by start day
        heap = []
        i = attended = day = 0
        
        while i < len(events) or heap:
            # If no events available today, jump to next event start
            if not heap:
                day = events[i][0]
            
            # Add all events starting today
            while i < len(events) and events[i][0] == day:
                heapq.heappush(heap, events[i][1])
                i += 1
            
            # Remove expired events
            while heap and heap[0] < day:
                heapq.heappop(heap)
            
            # Attend earliest ending event
            if heap:
                heapq.heappop(heap)
                attended += 1
            
            day += 1
        
        return attended
```

### Solution 3: Alternative with Set (Less Efficient)
```python
class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        # Sort events by end day (earliest deadline first)
        events.sort(key=lambda x: x[1])
        
        attended_days = set()
        
        for start, end in events:
            # Try to attend on the earliest available day
            for day in range(start, end + 1):
                if day not in attended_days:
                    attended_days.add(day)
                    break
        
        return len(attended_days)
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `events = [[1,2],[2,3],[3,4],[1,2]]`

### Step 1: Sort by start day
```
Original: [[1,2],[2,3],[3,4],[1,2]]
Sorted:   [[1,2],[1,2],[2,3],[3,4]]
```

### Step 2: Process day by day

| Day | Events Starting | Available Events (heap) | Action | Attended |
|-----|-----------------|------------------------|---------|----------|
| 1   | [1,2], [1,2]   | [2, 2]                | Attend event ending day 2 | 1 |
| 2   | [2,3]          | [2, 3]                | Attend event ending day 2 | 2 |
| 3   | [3,4]          | [3, 4]                | Attend event ending day 3 | 3 |
| 4   | -              | [4]                   | Attend event ending day 4 | 4 |

**Result:** 4 events attended! ✅

---

## 🔍 Why Greedy Works

**Intuition:** Events with earlier end dates have fewer opportunities to be attended.

**Example:** Consider events `[1,2]` and `[1,5]`
- Event `[1,2]` can only be attended on day 1 or 2
- Event `[1,5]` can be attended on days 1, 2, 3, 4, or 5
- **Greedy choice:** Attend `[1,2]` first (more urgent)

**Proof:** If we can attend event A that ends earlier instead of event B that ends later, this choice never makes the solution worse.

---

## 🧠 Visual Example

```
Events: [[1,3],[2,4],[3,5]]

Day 1: Available [1,3] → Attend [1,3]
Day 2: Available [2,4] → Attend [2,4]  
Day 3: Available [3,5] → Attend [3,5]

Result: 3 events attended
```

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Greedy + Heap | O(n log n) | O(n) |
| Sort by End Day | O(n²) | O(n) |
| Brute Force | O(n × max_day) | O(n) |

**Winner:** Greedy with Heap - optimal performance!

---

## 🎯 Key Takeaways

1. **Greedy Strategy:** Always choose the event that ends earliest
2. **Heap Usage:** Efficiently find the earliest ending event
3. **Day-by-day Processing:** Simulate attending events chronologically
4. **Remove Expired:** Clean up events that can no longer be attended
5. **Sorting:** Process events in chronological order of start times

---

## 🔍 Edge Cases

```python
# Single event
events = [[1,1]] → 1

# Non-overlapping events
events = [[1,1],[2,2],[3,3]] → 3

# All events same period
events = [[1,2],[1,2],[1,2]] → 2 (can only attend 2 days)

# No events
events = [] → 0
```

---

## 💭 Alternative Approaches

```python
# Sort by end day first (simpler but less efficient)
def maxEvents(events):
    events.sort(key=lambda x: x[1])  # Sort by end day
    used_days = set()
    
    for start, end in events:
        for day in range(start, end + 1):
            if day not in used_days:
                used_days.add(day)
                break
    
    return len(used_days)

# Using interval scheduling (classic algorithm)
def maxEvents(events):
    events.sort(key=lambda x: x[1])  # Sort by end time
    count = 0
    last_day = 0
    
    for start, end in events:
        if start > last_day:
            count += 1
            last_day = end
    
    return count  # Note: This doesn't work for this problem!
```

---

## 🔗 Related Problems
- 435. Non-overlapping Intervals
- 452. Minimum Number of Arrows to Burst Balloons
- 1326. Minimum Number of Taps to Open to Water a Garden
- 630. Course Schedule III

---

## 🏆 Interview Tips

1. **Start with greedy intuition** - explain why earliest deadline first
2. **Mention heap for efficiency** - O(log n) for min operations
3. **Walk through example** - show day-by-day simulation
4. **Discuss edge cases** - single event, no overlaps
5. **Compare approaches** - heap vs simple sorting

**This is a classic greedy scheduling problem! 🚀**




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



# Day 11 - September 7, 2025

## Reschedule Meetings for Maximum Free Time I

**Difficulty:** Medium  
**Topics:** Array, Sliding Window, Greedy  
**Companies:** Google, Microsoft, Amazon

---

## 🎯 Problem Statement

Given an event duration `eventTime`, meetings schedule (`startTime`, `endTime`), and `k` allowed reschedules, maximize the longest continuous free time by moving at most `k` meetings while maintaining their order and non-overlapping nature.

**Constraints:**
- 1 ≤ eventTime ≤ 10⁹
- 2 ≤ n ≤ 10⁵
- 1 ≤ k ≤ n
- Meetings are initially non-overlapping and ordered

### Examples

**Example 1:**
```
Input: eventTime=5, k=1, startTime=[1,3], endTime=[2,5]
Output: 2
Explanation: Move first meeting to [2,3], creating free time [0,2]
```

**Example 2:**
```
Input: eventTime=10, k=1, startTime=[0,2,9], endTime=[1,4,10]
Output: 6
Explanation: Move second meeting to [1,3], creating free time [3,9]
```

---

## 💡 Sliding Window Strategy

**Key Insight:** By rescheduling at most `k` meetings, we can merge up to `k+1` consecutive gaps between meetings.

**Why?** When we move a meeting, we eliminate the gap after it and potentially create a larger continuous gap by merging adjacent free time periods.

### Algorithm Steps:
1. **Calculate all gaps** between meetings (including before first and after last)
2. **Use sliding window** to find the maximum sum of `k+1` consecutive gaps
3. **Return the maximum** free time achievable

---

## 🐍 Python Solution

### Optimal Solution: Sliding Window
```python
from typing import List

class Solution:
    def maxFreeTime(self, eventTime: int, k: int, startTime: List[int], endTime: List[int]) -> int:
        n = len(startTime)
        
        # Calculate all gaps
        gaps = []
        # Gap before first meeting
        gaps.append(startTime[0] - 0)
        # Gaps between meetings
        for i in range(1, n):
            gaps.append(startTime[i] - endTime[i-1])
        # Gap after last meeting
        gaps.append(eventTime - endTime[-1])

        # We can merge up to k gaps → sum of (k+1) consecutive gaps is candidate
        window_size = k + 1
        
        # Sliding window to find max sum of window_size consecutive gaps
        max_free_time = 0
        window_sum = sum(gaps[:window_size])
        max_free_time = window_sum
        
        for i in range(window_size, len(gaps)):
            window_sum += gaps[i] - gaps[i - window_size]
            if window_sum > max_free_time:
                max_free_time = window_sum
        
        return max_free_time
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `eventTime=10, k=1, startTime=[0,2,9], endTime=[1,4,10]`

### Step 1: Calculate gaps
```
Meetings: [0,1], [2,4], [9,10]
Gap before first: 0 - 0 = 0
Gap between 1st and 2nd: 2 - 1 = 1  
Gap between 2nd and 3rd: 9 - 4 = 5
Gap after last: 10 - 10 = 0
Gaps: [0, 1, 5, 0]
```

### Step 2: Sliding window (k+1 = 2 consecutive gaps)
```
Window [0, 1]: sum = 1
Window [1, 5]: sum = 6  ← Maximum!
Window [5, 0]: sum = 5
```

### Step 3: Result
```
Maximum free time = 6
Achieved by moving the second meeting to create gap [3,9]
```

---

## 🔍 Why This Works

**Intuition:** When we reschedule `k` meetings, we can eliminate `k` gaps and merge the remaining gaps around them.

**Key Points:**
1. **Gap elimination:** Moving a meeting removes the gap after it
2. **Gap merging:** Adjacent gaps can be combined into larger continuous free time
3. **Optimal choice:** We want to merge the largest possible gaps

---

## ⚡ Complexity Analysis

- **Time Complexity:** O(n)
  - O(n) to calculate gaps
  - O(n) for sliding window
- **Space Complexity:** O(n)
  - O(n) for storing gaps array

---

## 🎯 Key Takeaways

1. **Gap thinking:** Convert meeting scheduling to gap maximization problem
2. **Sliding window:** Efficiently find maximum sum of consecutive elements
3. **Greedy insight:** Merging largest gaps gives optimal result
4. **Edge cases:** Handle meetings at boundaries (start/end of event time)

---

## 🔍 Edge Cases

```python
# Only two meetings
eventTime=5, k=1, startTime=[0,3], endTime=[1,4] → Output: 2

# Can reschedule all meetings  
eventTime=10, k=3, startTime=[1,3,5], endTime=[2,4,6] → Output: 10

# No rescheduling needed
eventTime=10, k=0, startTime=[0,5], endTime=[1,6] → Output: 4

# Single large gap
eventTime=20, k=1, startTime=[0,15], endTime=[5,20] → Output: 15
```

---

## 🔗 Related Problems

- 435. Non-overlapping Intervals
- 452. Minimum Number of Arrows to Burst Balloons  
- 1326. Minimum Number of Taps to Open to Water a Garden
- 253. Meeting Rooms II

---

## 🏆 Interview Tips

1. **Start with gap analysis** - explain how rescheduling affects gaps
2. **Mention sliding window** - efficient way to find maximum sum
3. **Walk through example** - show gap calculation and window movement
4. **Discuss edge cases** - boundary meetings, extreme k values
5. **Optimize space** - could avoid storing all gaps if needed

**This is a clever combination of scheduling and sliding window techniques! 🚀**



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

# Day 13: Excel Sheet Column Title (11/7/25)

## Problem Statement
Convert a given integer to its corresponding Excel column title (1 → "A", 28 → "AB", etc.).

**Constraints:**
- 1 ≤ columnNumber ≤ 2³¹ - 1

## Examples

Input: 1 → Output: "A"
Input: 28 → Output: "AB"
Input: 701 → Output: "ZY"

## Solution Approach
Base-26 Conversion with Adjustment
- Treat the numbering as base-26 but with A=1 instead of A=0
- Repeatedly divide by 26 and get remainders
- Adjust remainders to map to A-Z (1-26)
- Build the string in reverse order

```python
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        result = []
        while columnNumber > 0:
            columnNumber -= 1  # Adjust to 0-based
            remainder = columnNumber % 26
            result.append(chr(65 + remainder))  # 65 = 'A'
            columnNumber //= 26
        return ''.join(reversed(result))
```



# Day 14: Excel Sheet Column Number (12/7/25)

## Problem Statement
Convert an Excel column title (e.g., "A", "AB", "ZY") to its corresponding column number.

**Constraints:**
- 1 ≤ columnTitle.length ≤ 7
- "A" ≤ columnTitle ≤ "FXSHRXW" (max Excel column)
- Uppercase English letters only

## Examples
```python
Input: "A" → 1
Input: "AB" → 28
Input: "ZY" → 701


class Solution:
    def titleToNumber(self, columnTitle: str) -> int:
        result = 0
        for char in columnTitle:
            result = result * 26 + (ord(char) - ord('A') + 1)
        return result


```

# Day 15: Majority Element (13/7/25)

## Problem Statement
Given an array of size n, find the majority element that appears more than ⌊n/2⌋ times. The majority element always exists.

**Constraints:**
- 1 ≤ n ≤ 5 × 10⁴
- -10⁹ ≤ nums[i] ≤ 10⁹

## Examples
```python
Input: [3,2,3] → Output: 3
Input: [2,2,1,1,1,2,2] → Output: 2


class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None
        
        for num in nums:
            if count == 0:
                candidate = num
            count += (1 if num == candidate else -1)
        
        return candidate


```
# Day 16: Combine Two Tables (14/7/25)

## Problem Statement
Given two tables `Person` and `Address`, write a SQL query to report each person's first name, last name, city, and state. Include all persons from the Person table even if they don't have an address.

**Tables:**
```sql
Person (personId, lastName, firstName)
Address (addressId, personId, city, state)


SELECT 
    p.firstName,
    p.lastName,
    a.city,
    a.state
FROM 
    Person p
LEFT JOIN 
    Address a ON p.personId = a.personId

```
# Day-17 | 15/7/25  
LeetCode 181. Employees Earning More Than Their Managers (Easy)

---

## 🎯 Problem in one line
Return the **names** of employees whose salary is **strictly higher** than their **direct manager’s** salary.

---

## ✅ SQL Solution (self-join)

```sql
SELECT e1.name AS Employee
FROM Employee e1
JOIN Employee e2
  ON e1.managerId = e2.id
WHERE e1.salary > e2.salary;


```
# Day-18 | 16/7/25  
LeetCode 182. Duplicate Emails (Easy)

---

## 🎯 Problem in one line
Return every **email address** that appears **more than once** in the `Person` table.

---

## ✅ SQL Solution (GROUP BY + HAVING)

```sql
SELECT email AS Email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1;


```

# Day-19 | 17/7/25  
LeetCode 202. Happy Number (Easy)

---

## 🎯 Problem in one line
Return **true** if repeatedly summing the squares of the digits of `n` eventually reaches **1**, else return **false**.

---

## ✅ Python Solution (Floyd Cycle Detection)

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_num(x):
            return sum(int(d) ** 2 for d in str(x))

        slow = n          # tortoise
        fast = next_num(n) # hare

        while fast != 1 and slow != fast:
            slow = next_num(slow)           # 1 step
            fast = next_num(next_num(fast)) # 2 steps

        return fast == 1


```

# Day-20 | 18/7/25  
LeetCode 231. Power of Two (Easy)

---

## 🎯 Problem in one line
Return **true** if the integer `n` is exactly **2^x** for some non-negative integer `x`, else return **false**.

---

## ✅ Optimal Solution (Bit-trick, no loops/recursion)

```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        return n > 0 and (n & (n - 1)) == 0


```
# Day-21 | 19/7/25  
LeetCode 88. Merge Sorted Array (Easy)

---

## 🎯 Problem in one line
Merge two **already-sorted** arrays `nums1` (length `m`) and `nums2` (length `n`) **in-place** into `nums1` so it contains all `m + n` elements in **ascending order**.

---

## ✅ Optimal O(m+n) Solution (Three-Pointer, Backward Merge)

```python
class Solution:
    def merge(self, nums1: list[int], m: int, nums2: list[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        i, j, k = m - 1, n - 1, m + n - 1
        
        while i >= 0 and j >= 0:
            if nums1[i] > nums2[j]:
                nums1[k] = nums1[i]
                i -= 1
            else:
                nums1[k] = nums2[j]
                j -= 1
            k -= 1
        
        # If any leftovers in nums2, copy them over
        nums1[:j + 1] = nums2[:j + 1]
```
# Day-22 | 20/7/25  
LeetCode 27. Remove Element (Easy)

---

## 🎯 Problem in one line
Delete **every occurrence** of `val` from array `nums` **in-place** and return the new **length `k`** of the remaining elements (order may change).

---

## ✅ Two-Pointer O(n) Solution

```python
class Solution:
    def removeElement(self, nums: list[int], val: int) -> int:
        k = 0                       # pointer for next non-val position
        for i in range(len(nums)):
            if nums[i] != val:
                nums[k] = nums[i]   # keep it
                k += 1
        return k
```

# Day-23 | 21/7/25  
LeetCode 80. Remove Duplicates from Sorted Array II (Medium)

---

## 🎯 Problem in one line
Keep **at most two copies** of every distinct value inside the **sorted** array `nums` **in-place**, and return the new length `k`.

---

## ✅ Two-Pointer O(n) / O(1) Solution

```python
class Solution:
    def removeDuplicates(self, nums: list[int]) -> int:
        if len(nums) <= 2:
            return len(nums)

        k = 2  # next index to place a kept element
        for i in range(2, len(nums)):
            # only keep nums[i] if it differs from the two previous kept ones
            if nums[i] != nums[k - 2]:
                nums[k] = nums[i]
                k += 1
        return k
```
# Day-24 | 22/7/25  
LeetCode 189. Rotate Array (Medium)

---

## 🎯 Problem in one line
Rotate the array `nums` **k** steps to the **right** **in-place** (extra space O(1)).

---

## ✅ Optimal O(n) / O(1) Solution (Three-Reversal Trick)

```python
class Solution:
    def rotate(self, nums: list[int], k: int) -> None:
        n = len(nums)
        k %= n                   # handle k ≥ n
        if k == 0:
            return

        def reverse(l: int, r: int) -> None:
            while l < r:
                nums[l], nums[r] = nums[r], nums[l]
                l += 1
                r -= 1

        # 1. reverse whole array
        reverse(0, n - 1)
        # 2. reverse first k elements
        reverse(0, k - 1)
        # 3. reverse the rest
        reverse(k, n - 1)
```
# Day-25 | 23/7/25  
LeetCode 122. Best Time to Buy and Sell Stock II (Medium)

---

## 🎯 Problem in one line
Maximize profit by buying & selling a stock any number of times, **holding at most one share at a time**.

---

## ✅ Greedy O(n) Solution

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        max_profit = 0
        for i in range(1, len(prices)):
            # capture every uphill segment
            if prices[i] > prices[i - 1]:
                max_profit += prices[i] - prices[i - 1]
        return max_profit
