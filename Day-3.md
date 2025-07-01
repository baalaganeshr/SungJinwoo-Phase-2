# Day 3: Find the Original Typed String I (1/7/25)

## Problem Statement
Given a string `word` where Alice might have pressed one key too long (at most once), return the number of possible original strings she might have intended to type.

**Constraints:**
- 1 ≤ word.length ≤ 100
- word consists of lowercase English letters

## Examples

Input: "abbcccc" → Output: 5
Possible originals: "abbcccc", "abbccc", "abbcc", "abbc", "abcccc"

Input: "abcd" → Output: 1
Only possible: "abcd"

Input: "aaaa" → Output: 4
Possible originals: "aaaa", "aaa", "aa", "a"


```python
class Solution:
    def possibleStringCount(self, word: str) -> int:
        if not word:
            return 0
        
        count = 0
        n = len(word)
        i = 0
        
        while i < n:
            j = i
            while j < n and word[j] == word[i]:
                j += 1
            run_length = j - i
            if run_length > 1:
                count += run_length - 1
            i = j
        
        # Always include the original string, plus each possible single reduction
        return count + 1
