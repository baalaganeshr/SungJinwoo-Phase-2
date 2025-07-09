# Day 11 - September 7, 2025

## Reschedule Meetings for Maximum Free Time I

**Difficulty:** Medium  
**Topics:** Array, Sliding Window, Greedy  
**Companies:** Google, Microsoft, Amazon

---

## 🎯 Problem Statement

Given an event duration `eventTime`, meetings schedule (`startTime`, `endTime`), and `k` allowed reschedules, maximize the longest continuous free time by moving at most `k` meetings while maintaining their order and non-overlapping nature.

**Constraints:**
- 1 ≤ eventTime ≤ 10⁹
- 2 ≤ n ≤ 10⁵
- 1 ≤ k ≤ n
- Meetings are initially non-overlapping and ordered

### Examples

**Example 1:**
```
Input: eventTime=5, k=1, startTime=[1,3], endTime=[2,5]
Output: 2
Explanation: Move first meeting to [2,3], creating free time [0,2]
```

**Example 2:**
```
Input: eventTime=10, k=1, startTime=[0,2,9], endTime=[1,4,10]
Output: 6
Explanation: Move second meeting to [1,3], creating free time [3,9]
```

---

## 💡 Sliding Window Strategy

**Key Insight:** By rescheduling at most `k` meetings, we can merge up to `k+1` consecutive gaps between meetings.

**Why?** When we move a meeting, we eliminate the gap after it and potentially create a larger continuous gap by merging adjacent free time periods.

### Algorithm Steps:
1. **Calculate all gaps** between meetings (including before first and after last)
2. **Use sliding window** to find the maximum sum of `k+1` consecutive gaps
3. **Return the maximum** free time achievable

---

## 🐍 Python Solution

### Optimal Solution: Sliding Window
```python
from typing import List

class Solution:
    def maxFreeTime(self, eventTime: int, k: int, startTime: List[int], endTime: List[int]) -> int:
        n = len(startTime)
        
        # Calculate all gaps
        gaps = []
        # Gap before first meeting
        gaps.append(startTime[0] - 0)
        # Gaps between meetings
        for i in range(1, n):
            gaps.append(startTime[i] - endTime[i-1])
        # Gap after last meeting
        gaps.append(eventTime - endTime[-1])

        # We can merge up to k gaps → sum of (k+1) consecutive gaps is candidate
        window_size = k + 1
        
        # Sliding window to find max sum of window_size consecutive gaps
        max_free_time = 0
        window_sum = sum(gaps[:window_size])
        max_free_time = window_sum
        
        for i in range(window_size, len(gaps)):
            window_sum += gaps[i] - gaps[i - window_size]
            if window_sum > max_free_time:
                max_free_time = window_sum
        
        return max_free_time
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `eventTime=10, k=1, startTime=[0,2,9], endTime=[1,4,10]`

### Step 1: Calculate gaps
```
Meetings: [0,1], [2,4], [9,10]
Gap before first: 0 - 0 = 0
Gap between 1st and 2nd: 2 - 1 = 1  
Gap between 2nd and 3rd: 9 - 4 = 5
Gap after last: 10 - 10 = 0
Gaps: [0, 1, 5, 0]
```

### Step 2: Sliding window (k+1 = 2 consecutive gaps)
```
Window [0, 1]: sum = 1
Window [1, 5]: sum = 6  ← Maximum!
Window [5, 0]: sum = 5
```

### Step 3: Result
```
Maximum free time = 6
Achieved by moving the second meeting to create gap [3,9]
```

---

## 🔍 Why This Works

**Intuition:** When we reschedule `k` meetings, we can eliminate `k` gaps and merge the remaining gaps around them.

**Key Points:**
1. **Gap elimination:** Moving a meeting removes the gap after it
2. **Gap merging:** Adjacent gaps can be combined into larger continuous free time
3. **Optimal choice:** We want to merge the largest possible gaps

---

## ⚡ Complexity Analysis

- **Time Complexity:** O(n)
  - O(n) to calculate gaps
  - O(n) for sliding window
- **Space Complexity:** O(n)
  - O(n) for storing gaps array

---

## 🎯 Key Takeaways

1. **Gap thinking:** Convert meeting scheduling to gap maximization problem
2. **Sliding window:** Efficiently find maximum sum of consecutive elements
3. **Greedy insight:** Merging largest gaps gives optimal result
4. **Edge cases:** Handle meetings at boundaries (start/end of event time)

---

## 🔍 Edge Cases

```python
# Only two meetings
eventTime=5, k=1, startTime=[0,3], endTime=[1,4] → Output: 2

# Can reschedule all meetings  
eventTime=10, k=3, startTime=[1,3,5], endTime=[2,4,6] → Output: 10

# No rescheduling needed
eventTime=10, k=0, startTime=[0,5], endTime=[1,6] → Output: 4

# Single large gap
eventTime=20, k=1, startTime=[0,15], endTime=[5,20] → Output: 15
```

---

## 🔗 Related Problems

- 435. Non-overlapping Intervals
- 452. Minimum Number of Arrows to Burst Balloons  
- 1326. Minimum Number of Taps to Open to Water a Garden
- 253. Meeting Rooms II

---

## 🏆 Interview Tips

1. **Start with gap analysis** - explain how rescheduling affects gaps
2. **Mention sliding window** - efficient way to find maximum sum
3. **Walk through example** - show gap calculation and window movement
4. **Discuss edge cases** - boundary meetings, extreme k values
5. **Optimize space** - could avoid storing all gaps if needed

**This is a clever combination of scheduling and sliding window techniques! 🚀**