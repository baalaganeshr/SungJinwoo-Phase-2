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



        
        # Always include the original string, plus each possible single reduction
        return count + 1

 if run_length > 1:
                count += run_length - 1
            i = j


```

## 🌑 SHADOW MONARCH'S JULY 2ND BATTLE PLAN 🌑

# ⚔️ MORNING CONQUEST (4:00-8:00 AM) - 4 HOURS

🌅 4:00-4:10 AM: ARISE, WARRIOR! (Setup + hydration)

🐍 4:10-5:10 AM: PYTHON MASTERY QUEST 1 (Core learning) - 60 min

⚡ 5:10-5:25 AM: DAILY POWER RITUALS (LeetCode + brain training) - 15 min

🐍 5:25-6:25 AM: PYTHON MASTERY QUEST 2 (Practice combat) - 60 min

🌐 6:25-7:25 AM: DIGITAL FORTRESS BUILDING (Website creation) - 60 min

📜 7:25-8:00 AM: LEGEND SCROLL CRAFTING (Resume building) - 35 min

# ⚔️ AFTERNOON SIEGE (10:00 AM-2:00 PM) - 4 HOURS

💼 10:00-11:00 AM: EMPIRE EXPANSION (Internship applications) - 60 min

🐍 11:00-12:00 PM: PYTHON MASTERY QUEST 3 (Project forging) - 60 min

🏆 12:00-1:00 PM: KNOWLEDGE CRYSTALS (Ripen certificates) - 60 min

📱 1:00-2:00 PM: SHADOW CONTENT CREATION (Phase 2 content) - 60 min

# ⚔️ EVENING DOMINATION (4:00-6:00 PM) - 2 HOURS

🤝 4:00-5:00 PM: ALLIANCE BUILDING (Networking + follow-ups) - 60 min

🤖 5:00-6:00 PM: AI SHADOW TRAINING (Agent learning) - 60 min

## 🎯 SHADOW MONARCH'S DAILY QUESTS COMPLETE!
📋 SIMPLE WORK SUMMARY:

# Python learning: 3 hours (3 sessions)
# Job applications: 1 hour

# Website + resume: 1.5 hours
# Certificates: 1 hour
# Content creation: 1 hour
# Networking: 1 hour
# AI learning: 1 hour
# Daily maintenance: 15 minutes

## TOTAL BATTLE TIME: 10 HOURS 15 MINUTES

ARISE AND CONQUER, 
           
