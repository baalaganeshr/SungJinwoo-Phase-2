# Day-19 | 17/7/25  
LeetCode 202. Happy Number (Easy)

---

## 🎯 Problem in one line
Return **true** if repeatedly summing the squares of the digits of `n` eventually reaches **1**, else return **false**.

---

## ✅ Python Solution (Floyd Cycle Detection)

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def next_num(x):
            return sum(int(d) ** 2 for d in str(x))

        slow = n          # tortoise
        fast = next_num(n) # hare

        while fast != 1 and slow != fast:
            slow = next_num(slow)           # 1 step
            fast = next_num(next_num(fast)) # 2 steps

        return fast == 1
