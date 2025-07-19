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
