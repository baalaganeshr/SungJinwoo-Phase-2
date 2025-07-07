# Day 8 - July 7, 2025

## 1353. Maximum Number of Events That Can Be Attended

**Difficulty:** Medium  
**Topics:** Array, Greedy, Sorting, Heap (Priority Queue)  
**Companies:** Google, Microsoft, Amazon

---

## 🎯 Problem Statement

You are given an array of events where `events[i] = [startDay, endDay]`. Every event i starts at `startDay` and ends at `endDay`.

You can attend an event i at any day d where `startDay <= d <= endDay`. You can only attend one event at any time d.

Return the maximum number of events you can attend.

### Examples

**Example 1:**
```
Input: events = [[1,2],[2,3],[3,4]]
Output: 3
Explanation: You can attend all the three events.
One way to attend them all is as shown.
Attend the first event on day 1.
Attend the second event on day 2.
Attend the third event on day 3.
```

**Example 2:**
```
Input: events = [[1,2],[2,3],[3,4],[1,2]]
Output: 4
Explanation: You can attend all four events by attending them optimally.
```

### Constraints
- `1 <= events.length <= 10^5`
- `events[i].length == 2`
- `1 <= startDay <= endDay <= 10^5`

---

## 💡 Greedy Strategy

**Key Insight:** Always attend the event that ends earliest among available events on each day.

**Why?** Events that end sooner have fewer opportunities to be attended later, so we should prioritize them.

### Algorithm Steps:
1. **Sort events** by start day
2. **For each day**, collect all events that start on that day
3. **Remove expired events** from consideration
4. **Attend the event that ends earliest** (greedy choice)

---

## 🐍 Python Solutions

### Solution 1: Greedy with Heap (Optimal)
```python
import heapq

class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        # Sort events by start day
        events.sort()
        
        min_heap = []  # Store end days of available events
        day = 1
        event_idx = 0
        attended = 0
        
        # Continue until no more events to process
        while event_idx < len(events) or min_heap:
            # Add all events that start on current day
            while event_idx < len(events) and events[event_idx][0] == day:
                heapq.heappush(min_heap, events[event_idx][1])  # Add end day
                event_idx += 1
            
            # Remove expired events (ended before current day)
            while min_heap and min_heap[0] < day:
                heapq.heappop(min_heap)
            
            # Attend the event that ends earliest
            if min_heap:
                heapq.heappop(min_heap)  # Attend this event
                attended += 1
            
            day += 1
        
        return attended
```

### Solution 2: Cleaner Implementation
```python
import heapq

class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        events.sort()  # Sort by start day
        heap = []
        i = attended = day = 0
        
        while i < len(events) or heap:
            # If no events available today, jump to next event start
            if not heap:
                day = events[i][0]
            
            # Add all events starting today
            while i < len(events) and events[i][0] == day:
                heapq.heappush(heap, events[i][1])
                i += 1
            
            # Remove expired events
            while heap and heap[0] < day:
                heapq.heappop(heap)
            
            # Attend earliest ending event
            if heap:
                heapq.heappop(heap)
                attended += 1
            
            day += 1
        
        return attended
```

### Solution 3: Alternative with Set (Less Efficient)
```python
class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        # Sort events by end day (earliest deadline first)
        events.sort(key=lambda x: x[1])
        
        attended_days = set()
        
        for start, end in events:
            # Try to attend on the earliest available day
            for day in range(start, end + 1):
                if day not in attended_days:
                    attended_days.add(day)
                    break
        
        return len(attended_days)
```

---

## 📊 Step-by-Step Walkthrough

**Example:** `events = [[1,2],[2,3],[3,4],[1,2]]`

### Step 1: Sort by start day
```
Original: [[1,2],[2,3],[3,4],[1,2]]
Sorted:   [[1,2],[1,2],[2,3],[3,4]]
```

### Step 2: Process day by day

| Day | Events Starting | Available Events (heap) | Action | Attended |
|-----|-----------------|------------------------|---------|----------|
| 1   | [1,2], [1,2]   | [2, 2]                | Attend event ending day 2 | 1 |
| 2   | [2,3]          | [2, 3]                | Attend event ending day 2 | 2 |
| 3   | [3,4]          | [3, 4]                | Attend event ending day 3 | 3 |
| 4   | -              | [4]                   | Attend event ending day 4 | 4 |

**Result:** 4 events attended! ✅

---

## 🔍 Why Greedy Works

**Intuition:** Events with earlier end dates have fewer opportunities to be attended.

**Example:** Consider events `[1,2]` and `[1,5]`
- Event `[1,2]` can only be attended on day 1 or 2
- Event `[1,5]` can be attended on days 1, 2, 3, 4, or 5
- **Greedy choice:** Attend `[1,2]` first (more urgent)

**Proof:** If we can attend event A that ends earlier instead of event B that ends later, this choice never makes the solution worse.

---

## 🧠 Visual Example

```
Events: [[1,3],[2,4],[3,5]]

Day 1: Available [1,3] → Attend [1,3]
Day 2: Available [2,4] → Attend [2,4]  
Day 3: Available [3,5] → Attend [3,5]

Result: 3 events attended
```

---

## ⚡ Complexity Analysis

| Solution | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| Greedy + Heap | O(n log n) | O(n) |
| Sort by End Day | O(n²) | O(n) |
| Brute Force | O(n × max_day) | O(n) |

**Winner:** Greedy with Heap - optimal performance!

---

## 🎯 Key Takeaways

1. **Greedy Strategy:** Always choose the event that ends earliest
2. **Heap Usage:** Efficiently find the earliest ending event
3. **Day-by-day Processing:** Simulate attending events chronologically
4. **Remove Expired:** Clean up events that can no longer be attended
5. **Sorting:** Process events in chronological order of start times

---

## 🔍 Edge Cases

```python
# Single event
events = [[1,1]] → 1

# Non-overlapping events
events = [[1,1],[2,2],[3,3]] → 3

# All events same period
events = [[1,2],[1,2],[1,2]] → 2 (can only attend 2 days)

# No events
events = [] → 0
```

---

## 💭 Alternative Approaches

```python
# Sort by end day first (simpler but less efficient)
def maxEvents(events):
    events.sort(key=lambda x: x[1])  # Sort by end day
    used_days = set()
    
    for start, end in events:
        for day in range(start, end + 1):
            if day not in used_days:
                used_days.add(day)
                break
    
    return len(used_days)

# Using interval scheduling (classic algorithm)
def maxEvents(events):
    events.sort(key=lambda x: x[1])  # Sort by end time
    count = 0
    last_day = 0
    
    for start, end in events:
        if start > last_day:
            count += 1
            last_day = end
    
    return count  # Note: This doesn't work for this problem!
```

---

## 🔗 Related Problems
- 435. Non-overlapping Intervals
- 452. Minimum Number of Arrows to Burst Balloons
- 1326. Minimum Number of Taps to Open to Water a Garden
- 630. Course Schedule III

---

## 🏆 Interview Tips

1. **Start with greedy intuition** - explain why earliest deadline first
2. **Mention heap for efficiency** - O(log n) for min operations
3. **Walk through example** - show day-by-day simulation
4. **Discuss edge cases** - single event, no overlaps
5. **Compare approaches** - heap vs simple sorting

**This is a classic greedy scheduling problem! 🚀**
