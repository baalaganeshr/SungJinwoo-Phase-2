# Day-25 | 23/7/25  
LeetCode 122. Best Time to Buy and Sell Stock II (Medium)

---

## 🎯 Problem in one line
Maximize profit by buying & selling a stock any number of times, **holding at most one share at a time**.

---

## ✅ Greedy O(n) Solution

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        max_profit = 0
        for i in range(1, len(prices)):
            # capture every uphill segment
            if prices[i] > prices[i - 1]:
                max_profit += prices[i] - prices[i - 1]
        return max_profit
