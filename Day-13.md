# Day 13: Excel Sheet Column Title (11/7/25)

## Problem Statement
Convert a given integer to its corresponding Excel column title (1 → "A", 28 → "AB", etc.).

**Constraints:**
- 1 ≤ columnNumber ≤ 2³¹ - 1

## Examples

Input: 1 → Output: "A"
Input: 28 → Output: "AB"
Input: 701 → Output: "ZY"

## Solution Approach
Base-26 Conversion with Adjustment
- Treat the numbering as base-26 but with A=1 instead of A=0
- Repeatedly divide by 26 and get remainders
- Adjust remainders to map to A-Z (1-26)
- Build the string in reverse order

```python
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        result = []
        while columnNumber > 0:
            columnNumber -= 1  # Adjust to 0-based
            remainder = columnNumber % 26
            result.append(chr(65 + remainder))  # 65 = 'A'
            columnNumber //= 26
        return ''.join(reversed(result))
```