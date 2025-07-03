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
