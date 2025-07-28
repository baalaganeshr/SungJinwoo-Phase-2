# Day-30 | 28/7/25  
LeetCode 380. Insert Delete GetRandom O(1) (Medium)

---

## 🎯 Problem in one line
Design a set that supports **insert**, **remove**, and **getRandom** in **average O(1)** time.

---

## ✅ HashMap + Dynamic Array Solution

```python
import random

class RandomizedSet:
    def __init__(self):
        self.vals = []          # underlying list for O(1) random access
        self.idx_map = {}       # val -> its index in vals

    def insert(self, val: int) -> bool:
        if val in self.idx_map:
            return False
        self.idx_map[val] = len(self.vals)
        self.vals.append(val)
        return True

    def remove(self, val: int) -> bool:
        if val not in self.idx_map:
            return False
        # move last element into the hole left by val
        last, idx = self.vals[-1], self.idx_map[val]
        self.vals[idx] = last
        self.idx_map[last] = idx
        self.vals.pop()
        del self.idx_map[val]
        return True

    def getRandom(self) -> int:
        return random.choice(self.vals)
