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
