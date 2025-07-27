# Day-29 | 27/7/25  
LeetCode 274. H-Index (Medium)

---

## 🎯 Problem in one line
Find the **largest** integer `h` such that **at least `h`** papers have **≥ h** citations each.

---

## ✅ Counting-Sort / Bucket O(n) Solution

```python
class Solution:
    def hIndex(self, citations: list[int]) -> int:
        n = len(citations)
        bucket = [0] * (n + 1)  # indices 0..n

        # populate bucket: index = citation count
        for c in citations:
            bucket[min(c, n)] += 1

        # running sum of papers from the tail
        papers = 0
        for h in range(n, -1, -1):
            papers += bucket[h]
            if papers >= h:       # h papers each cited >= h times
                return h
        return 0
