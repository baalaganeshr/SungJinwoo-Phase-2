# Day-28 | 26/7/25  
LeetCode 45. Jump Game II (Medium)

---

## 🎯 Problem in one line
Compute the **minimum number of jumps** required to reach the **last index** starting from index `0`, where `nums[i]` is the **maximum** jump length allowed from `i`.

---

## ✅ Greedy O(n) Solution

```python
class Solution:
    def jump(self, nums: list[int]) -> int:
        n = len(nums)
        jumps = 0
        cur_end = 0     # farthest we can reach with current jumps
        farthest = 0    # global farthest reachable so far

        for i in range(n - 1):          # no need to jump from last index
            farthest = max(farthest, i + nums[i])
            
            # when we reach the boundary provided by the previous jump
            if i == cur_end:
                jumps += 1
                cur_end = farthest
                if cur_end >= n - 1:    # early exit
                    break

        return jumps
