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
