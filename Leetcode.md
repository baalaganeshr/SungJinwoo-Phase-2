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
