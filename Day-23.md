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
