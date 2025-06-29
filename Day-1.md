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
