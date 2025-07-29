# Day-31 | 29/7/25  
LeetCode 238. Product of Array Except Self (Medium)

---

## 🎯 Problem in one line  
Return an array where each element is the **product of every other element** in `nums`, **without division** and in **O(n)** time.

---

## ✅ Prefix-Suffix Product (O(n) time, O(1) extra space)

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        n = len(nums)
        res = [1] * n

        # 1. left products
        left = 1
        for i in range(n):
            res[i] = left
            left *= nums[i]

        # 2. right products
        right = 1
        for i in range(n - 1, -1, -1):
            res[i] *= right
            right *= nums[i]

        return res
