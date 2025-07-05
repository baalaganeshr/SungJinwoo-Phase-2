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
