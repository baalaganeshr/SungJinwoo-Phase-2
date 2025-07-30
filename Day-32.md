# Day-32 | 30/7/25  
LeetCode 134. Gas Station (Medium)

---

## 🎯 Problem in one line
Return the **starting index** from which you can complete a full clockwise circuit of the circular gas-station route, or **-1** if impossible.

---

## ✅ Greedy O(n) Solution

```python
class Solution:
    def canCompleteCircuit(self, gas: list[int], cost: list[int]) -> int:
        n = len(gas)
        total_tank = 0        # global gas - cost
        curr_tank  = 0        # gas - cost from current start
        start = 0

        for i in range(n):
            diff = gas[i] - cost[i]
            total_tank += diff
            curr_tank  += diff

            # if current segment fails, move start right
            if curr_tank < 0:
                start = i + 1
                curr_tank = 0

        return start if total_tank >= 0 else -1
