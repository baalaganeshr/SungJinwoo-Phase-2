# Day-20 | 18/7/25  
LeetCode 231. Power of Two (Easy)

---

## 🎯 Problem in one line
Return **true** if the integer `n` is exactly **2^x** for some non-negative integer `x`, else return **false**.

---

## ✅ Optimal Solution (Bit-trick, no loops/recursion)

```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        return n > 0 and (n & (n - 1)) == 0
