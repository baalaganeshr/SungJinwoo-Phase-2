# Day-34 | 31/7/25  
LeetCode 12. Integer to Roman (Medium)

---

## 🎯 Problem in one line  
Convert an integer `1 ≤ num ≤ 3999` into its **Roman numeral** string.

---

## ✅ Greedy Look-Up Table Solution (O(1) time & space)

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # value-symbol pairs in descending order
        value_symbol = [
            (1000, "M"), (900, "CM"), (500, "D"), (400, "CD"),
            (100,  "C"), (90,  "XC"), (50,  "L"), (40,  "XL"),
            (10,   "X"), (9,   "IX"), (5,   "V"), (4,   "IV"),
            (1,    "I")
        ]

        roman = []
        for v, s in value_symbol:
            if num == 0:
                break
            count, num = divmod(num, v)  # how many times v fits
            roman.append(s * count)
        return ''.join(roman)
