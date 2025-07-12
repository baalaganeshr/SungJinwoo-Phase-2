# Day 14: Excel Sheet Column Number (12/7/25)

## Problem Statement
Convert an Excel column title (e.g., "A", "AB", "ZY") to its corresponding column number.

**Constraints:**
- 1 ≤ columnTitle.length ≤ 7
- "A" ≤ columnTitle ≤ "FXSHRXW" (max Excel column)
- Uppercase English letters only

## Examples
```python
Input: "A" → 1
Input: "AB" → 28
Input: "ZY" → 701


class Solution:
    def titleToNumber(self, columnTitle: str) -> int:
        result = 0
        for char in columnTitle:
            result = result * 26 + (ord(char) - ord('A') + 1)
        return result
