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
