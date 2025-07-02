# 💰 Best Time to Buy and Sell Stock - Simple Solution

## 🎯 The Problem
Find the best day to buy and sell a stock to make maximum profit.

**Example:** `[7,1,5,3,6,4]` → Answer: `5` (buy at 1, sell at 6)

---

## 💡 Simple Strategy
1. **Remember the cheapest price** we've seen
2. **Check profit** if we sell today
3. **Keep track** of the best profit

---

## 🐍 Python Code

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = prices[0]  # Cheapest price so far
        max_profit = 0         # Best profit so far
        
        for price in prices[1:]:
            min_price = min(min_price, price)      # Update cheapest
            profit = price - min_price             # Today's profit
            max_profit = max(max_profit, profit)   # Update best profit
        
        return max_profit
```

---

## 📊 How It Works

**Array:** `[7, 1, 5, 3, 6, 4]`

| Day | Price | Cheapest | Profit | Best Profit |
|-----|-------|----------|--------|-------------|
| 0   | 7     | 7        | -      | 0           |
| 1   | 1     | 1        | 0      | 0           |
| 2   | 5     | 1        | 4      | 4           |
| 3   | 3     | 1        | 2      | 4           |
| 4   | 6     | 1        | **5**  | **5**       |
| 5   | 4     | 1        | 3      | 5           |

**Result:** Buy at $1, sell at $6 = $5 profit! 🎉

---

## ⚡ Why This Works
- We can only sell **after** we buy
- So we track the **lowest price** up to today
- Then check: "What if I sell today?"
- Keep the **best profit** we find

**Time:** O(n) - one pass  
**Space:** O(1) - just two variables
