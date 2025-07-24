# Day-27 | 25/7/25  
LeetCode 55. Jump Game (Medium)

---

## 🎯 Problem in one line
Return **true** if you can jump from the **first** index to the **last** index of `nums`, using each element as your **maximum** jump length.

---

## ✅ Greedy O(n) Solution

```python
class Solution:
    def canJump(self, nums: list[int]) -> bool:
        farthest = 0
        n = len(nums)
        
        for i in range(n):
            if i > farthest:          # can't get here
                return False
            farthest = max(farthest, i + nums[i])
            if farthest >= n - 1:     # early exit
                return True
        
        return farthest >= n - 1
