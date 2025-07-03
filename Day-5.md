# Day 5 - July 3, 2025

## 125. Valid Palindrome

**Difficulty:** Easy  
**Topics:** Two Pointers, String  
**Companies:** Facebook, Microsoft, Amazon, Google, Apple

---

## 🎯 Problem Statement

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward.

Given a string `s`, return `true` if it is a **palindrome**, or `false` otherwise.

### Examples

**Example 1:**
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**
```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**
```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

### Constraints
- `1 <= s.length <= 2 * 10^5`
- `s` consists only of printable ASCII characters.

---

## 💡 Simple Strategy

1. **Clean the string**: Remove non-alphanumeric characters and convert to lowercase
2. **Two pointers**: Compare characters from both ends moving inward
3. **Check match**: If all characters match, it's a palindrome!

---

## 🐍 Python Solutions

### Solution 1: Simple and Clean
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Step 1: Clean the string - keep only alphanumeric and lowercase
        cleaned = ''.join(char.lower() for char in s if char.isalnum())
        
        # Step 2: Check if cleaned string equals its reverse
        return cleaned == cleaned[::-1]
```

### Solution 2: Two Pointers (Space Efficient)
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Two pointers from both ends
        left, right = 0, len(s) - 1
        
        while left < right:
            # Skip non-alphanumeric characters from left
            while left < right and not s[left].isalnum():
                left += 1
            
            # Skip non-alphanumeric characters from right
            while left < right and not s[right].isalnum():
                right -= 1
            
            # Compare characters (case-insensitive)
            if s[left].lower() != s[right].lower():
                return False
            
            left += 1
            right -= 1
        
        return True
```

### Solution 3: Step-by-Step (For Understanding)
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Step 1: Convert to lowercase
        s = s.lower()
        
        # Step 2: Filter only alphanumeric characters
        alphanumeric_only = ""
        for char in s:
            if char.isalnum():  # Keep letters and numbers only
                alphanumeric_only += char
        
        # Step 3: Check if string reads same forwards and backwards
        return alphanumeric_only == alphanumeric_only[::-1]
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `"A man, a plan, a canal: Panama"`

### Method 1: Clean First
```
Original: "A man, a plan, a canal: Panama"
Step 1: Convert to lowercase → "a man, a plan, a canal: panama"
Step 2: Keep only alphanumeric → "amanaplanacanalpanama"
Step 3: Check if palindrome → "amanaplanacanalpanama" == "amanaplanacanalpanama"
Result: True ✅
```

### Method 2: Two Pointers
```
Original: "A man, a plan, a canal: Panama"
         ↑                            ↑
       left                        right

Compare: 'A' vs 'a' → same (case-insensitive) ✅
Move pointers inward, skip non-alphanumeric...
Continue until pointers meet
Result: True ✅
```

---

## 🔍 Edge Cases

| Input | Cleaned | Result | Explanation |
|-------|---------|--------|-------------|
| `""` | `""` | `True` | Empty string is palindrome |
| `" "` | `""` | `True` | Only spaces → empty after cleaning |
| `"a"` | `"a"` | `True` | Single character is palindrome |
| `"Aa"` | `"aa"` | `True` | Case-insensitive |
| `"ab"` | `"ab"` | `False` | Different characters |

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Clean First | O(n) | O(n) |
| Two Pointers | O(n) | O(1) |
| Step-by-Step | O(n) | O(n) |

**Best Choice:** Two Pointers method for O(1) space complexity!

---

## 🎯 Key Takeaways

1. **Preprocessing**: Clean the string by removing non-alphanumeric characters
2. **Case-insensitive**: Convert to lowercase for comparison
3. **Two approaches**: 
   - Clean first, then compare (simple but uses extra space)
   - Two pointers (more efficient, constant space)
4. **Edge cases**: Empty strings and single characters are palindromes

---

## 🔗 Related Problems
- 9. Palindrome Number
- 234. Palindrome Linked List  
- 680. Valid Palindrome II
- 1332. Remove Palindromic Subsequences
