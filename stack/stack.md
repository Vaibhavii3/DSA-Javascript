# Stack Problems - Solutions in JavaScript

A comprehensive collection of stack-based problems solved using JavaScript, focusing on monotonic stack patterns and stack operations.

---

## Table of Contents
1. [Build an Array With Stack Operations](#q1-build-an-array-with-stack-operations)
2. [Final Prices With a Special Discount in a Shop](#q2-final-prices-with-a-special-discount-in-a-shop)
3. [Daily Temperatures](#q3-daily-temperatures)
4. [Largest Rectangle in Histogram](#q4-largest-rectangle-in-histogram)
5. [Exclusive Time of Functions](#q5-exclusive-time-of-functions)

---

## Q1. Build an Array With Stack Operations

**Difficulty:** Medium  
**Pattern:** Stack Simulation

### Problem Statement
You are given an integer array `target` and an integer `n`.

You have an empty stack with two operations:
- `"Push"`: pushes an integer to the top of the stack
- `"Pop"`: removes the integer on the top of the stack

You also have a stream of integers in the range `[1, n]` (in order: 1, 2, 3, ..., n).

Use the stack operations to make the stack equal to `target`. Return the operations needed.

### Rules
- If the stream number matches target, push it
- If the stream number doesn't match target, push then pop (to skip it)
- Stop when stack equals target (don't process remaining stream numbers)

### Examples
```javascript
Input: target = [1,3], n = 3
Output: ["Push","Push","Pop","Push"]
Explanation: 
- Read 1 from stream → in target → Push → stack: [1]
- Read 2 from stream → NOT in target → Push, Pop → stack: [1]
- Read 3 from stream → in target → Push → stack: [1,3]

Input: target = [1,2,3], n = 3
Output: ["Push","Push","Push"]
Explanation: All numbers 1,2,3 are in target, just push them all.

Input: target = [1,2], n = 4
Output: ["Push","Push"]
Explanation: After building [1,2], we stop. Don't read 3 and 4.

Input: target = [2,3,4], n = 5
Output: ["Push","Pop","Push","Push","Push"]
```

### Constraints
- `1 <= target.length <= 100`
- `1 <= n <= 100`
- `1 <= target[i] <= n`
- `target` is strictly increasing

### Logic Explanation

The key insight is that we process the stream sequentially (1, 2, 3, ...) and for each target element:

1. **Skip unwanted numbers:** For any stream number before the target number, we Push then Pop (to skip it)
2. **Add target numbers:** When stream number matches target element, just Push
3. **Stop when done:** Once we've added all target elements, stop (ignore remaining stream)

**Strategy:**
- Use a counter (`current`) to track which stream number we're at
- For each target element:
  - While current < target element: Push & Pop (skip these numbers)
  - When current == target element: Push (add it to stack)
  - Increment current

### Solution
```javascript
function buildArray(target, n) {
    const result = [];
    let current = 1;  // Current stream number (starts from 1)
    
    for (let i = 0; i < target.length; i++) {
        // Skip numbers that are not in target
        while (current < target[i]) {
            result.push("Push");  // Read from stream and push
            result.push("Pop");   // Not in target, so pop it
            current++;            // Move to next stream number
        }
        
        // Current number is in target, just push it
        result.push("Push");
        current++;  // Move to next stream number
    }
    
    return result;
}
```

**Time Complexity:** O(target's last element)  
**Space Complexity:** O(number of operations)

### Walkthrough Example

**Example:** `target = [1,3], n = 3`
```
Initial state: current = 1, result = []

Iteration 1 (target[0] = 1):
  - while (1 < 1)? NO, skip while loop
  - Push to result → result = ["Push"]
  - current = 2

Iteration 2 (target[1] = 3):
  - while (2 < 3)? YES
    - Push, Pop → result = ["Push", "Push", "Pop"]
    - current = 3
  - while (3 < 3)? NO, exit while loop
  - Push to result → result = ["Push", "Push", "Pop", "Push"]
  - current = 4

Return: ["Push", "Push", "Pop", "Push"]
```

**Visual representation:**
```
Stream: 1 → 2 → 3 → (4, not processed)
        ↓   ↓   ↓
Target: 1   X   3   (2 is not in target, so Push-Pop)
Ops:   Push Push Push
            Pop
```

### Key Insights
- Stream numbers come in sequential order (1, 2, 3, ...)
- If stream number is in target → just Push
- If stream number is NOT in target → Push then Pop (to skip it)
- Stop processing when all target elements are added
- Target is strictly increasing, so we process sequentially

---

## Q2. Final Prices With a Special Discount in a Shop

**Difficulty:** Easy  
**Pattern:** Next Smaller or Equal Element

### Problem Statement
You are given an integer array `prices` where `prices[i]` is the price of the `ith` item in a shop.

There is a special discount for items in the shop. If you buy the `ith` item, then you will receive a discount equivalent to `prices[j]` where `j` is the minimum index such that `j > i` and `prices[j] <= prices[i]`. Otherwise, you will not receive any discount at all.

Return an integer array `answer` where `answer[i]` is the final price you will pay for the `ith` item of the shop, considering the special discount.

### Examples
```javascript
Input: prices = [8,4,6,2,3]
Output: [4,2,4,2,3]
Explanation: 
- For item 0 (price=8): discount = 4, final = 8-4 = 4
- For item 1 (price=4): discount = 2, final = 4-2 = 2
- For item 2 (price=6): discount = 2, final = 6-2 = 4
- For items 3 and 4: no discount

Input: prices = [1,2,3,4,5]
Output: [1,2,3,4,5]
Explanation: No discounts (strictly increasing)

Input: prices = [10,1,1,6]
Output: [9,0,1,6]
```

### Constraints
- `1 <= prices.length <= 500`
- `1 <= prices[i] <= 1000`

### Logic Explanation

**Pattern Recognition:** This is a "Next Smaller or Equal Element" problem.

For each item, we need to find the first item to its right with price ≤ current price.

**Two Approaches:**

**1. Brute Force (O(n²))**
- For each item, scan all items to the right
- Find the first one with price ≤ current price
- Apply discount

**2. Monotonic Stack (O(n))**
- Use a stack to maintain indices in decreasing order of prices
- When current price can discount previous items, apply it
- Stack keeps track of items still waiting for a discount

### Solutions

**Approach 1: Brute Force**
```javascript
function finalPrices(prices) {
    const result = [...prices]; // Copy array
    
    for (let i = 0; i < prices.length; i++) {
        // Look for next item with price <= current price
        for (let j = i + 1; j < prices.length; j++) {
            if (prices[j] <= prices[i]) {
                result[i] = prices[i] - prices[j];
                break; // Found discount, stop searching
            }
        }
    }
    
    return result;
}
```

**Approach 2: Monotonic Stack (Optimal)**
```javascript
function finalPrices(prices) {
    const result = [...prices];
    const stack = []; // Stores indices
    
    for (let i = 0; i < prices.length; i++) {
        // While current price can be a discount for items in stack
        while (stack.length > 0 && prices[i] <= prices[stack[stack.length - 1]]) {
            const idx = stack.pop();
            result[idx] = prices[idx] - prices[i];
        }
        stack.push(i);
    }
    
    return result;
}
```

**Time Complexity:** 
- Brute Force: O(n²)
- Monotonic Stack: O(n)

**Space Complexity:** O(n) for stack

### Walkthrough Example

```
prices = [8,4,6,2,3]

i=0, price=8: Stack: [0]
i=1, price=4: 4<=8 → pop 0, result[0]=8-4=4
              Stack: [1]
i=2, price=6: 6>4, Stack: [1,2]
i=3, price=2: 2<=6 → pop 2, result[2]=6-2=4
              2<=4 → pop 1, result[1]=4-2=2
              Stack: [3]
i=4, price=3: 3>2, Stack: [3,4]

Items 3 and 4 remain → no discount
result = [4,2,4,2,3]
```

### Key Insights
- Stack maintains indices in **decreasing order of prices**
- When we find a smaller/equal price, it can discount all larger prices in stack
- Items remaining in stack have no discount

---

## Q3. Daily Temperatures

**Difficulty:** Medium  
**Pattern:** Next Greater Element

### Problem Statement
Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `ith` day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

### Examples
```javascript
Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]
Explanation:
- Day 0 (73°): Next warmer is day 1 (74°) → wait 1 day
- Day 1 (74°): Next warmer is day 2 (75°) → wait 1 day
- Day 2 (75°): Next warmer is day 6 (76°) → wait 4 days
- Day 3 (71°): Next warmer is day 5 (72°) → wait 2 days
- Day 4 (69°): Next warmer is day 5 (72°) → wait 1 day
- Day 5 (72°): Next warmer is day 6 (76°) → wait 1 day
- Days 6-7: No warmer days → 0

Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]

Input: temperatures = [30,60,90]
Output: [1,1,0]
```

### Constraints
- `1 <= temperatures.length <= 10^5`
- `30 <= temperatures[i] <= 100`

### Logic Explanation

**Pattern Recognition:** This is a "Next Greater Element" problem.

For each day, we need to find:
- The **first** day to the right with temperature > current temperature
- Return the **number of days** to wait (index difference)

**Key Insight:** When we encounter a warmer temperature, it answers all cooler temperatures waiting in the stack.

**Why Monotonic Stack?**
- Need to track temperatures waiting for a warmer day
- When we find a warmer temp, it answers multiple previous days
- Stack maintains decreasing order of temperatures

### Solutions

**Approach 1: Brute Force**
```javascript
function dailyTemperatures(temperatures) {
    const n = temperatures.length;
    const answer = new Array(n).fill(0);
    
    for (let i = 0; i < n; i++) {
        // Look for next warmer day
        for (let j = i + 1; j < n; j++) {
            if (temperatures[j] > temperatures[i]) {
                answer[i] = j - i;
                break;
            }
        }
    }
    
    return answer;
}
```

**Approach 2: Monotonic Stack (Optimal)**
```javascript
function dailyTemperatures(temperatures) {
    const n = temperatures.length;
    const answer = new Array(n).fill(0);
    const stack = []; // Stores indices
    
    for (let i = 0; i < n; i++) {
        // While current temp is warmer than temperatures in stack
        while (stack.length > 0 && temperatures[i] > temperatures[stack[stack.length - 1]]) {
            const prevIndex = stack.pop();
            answer[prevIndex] = i - prevIndex; // Days to wait
        }
        stack.push(i);
    }
    
    // Remaining indices in stack have no warmer day (already 0)
    return answer;
}
```

**Time Complexity:**
- Brute Force: O(n²)
- Monotonic Stack: O(n) - each element pushed/popped once

**Space Complexity:** O(n) for stack

### Walkthrough Example

```
temperatures = [73,74,75,71,69,72,76,73]

i=0, temp=73: Stack: [0]
i=1, temp=74: 74>73 → answer[0]=1, Stack: [1]
i=2, temp=75: 75>74 → answer[1]=1, Stack: [2]
i=3, temp=71: 71<75, Stack: [2,3]
i=4, temp=69: 69<71, Stack: [2,3,4]
i=5, temp=72: 72>69 → answer[4]=1
              72>71 → answer[3]=2
              72<75, Stack: [2,5]
i=6, temp=76: 76>72 → answer[5]=1
              76>75 → answer[2]=4
              Stack: [6]
i=7, temp=73: 73<76, Stack: [6,7]

Final: [1,1,4,2,1,1,0,0]
```

### Key Insights
- Stack maintains indices in **decreasing order of temperatures**
- When we find a warmer temp, it answers **all cooler temps** in stack
- Items remaining in stack have **no warmer day** → keep as 0

---

## Q4. Largest Rectangle in Histogram

**Difficulty:** Hard  
**Pattern:** Next Smaller Element (Both Sides)

### Problem Statement
Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

### Examples
```javascript
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The largest rectangle has height 5 and width 2 (indices 2-3)

        6     ┌─┐
        5   ┌─┤ │
        4   │ │ │
        3   │ │ │   ┌─┐
        2 ┌─┤ │ │ ┌─┤ │
        1 │ ├─┤ │ │ │ │
        0 └─┴─┴─┴─┴─┴─┘
          0 1 2 3 4 5

Input: heights = [2,4]
Output: 4
```

### Constraints
- `1 <= heights.length <= 10^5`
- `0 <= heights[i] <= 10^4`

### Logic Explanation

**Pattern Recognition:** For each bar, we need to find how far it can extend left and right while maintaining its height.

**Key Insight:**
- For each bar at index `i` with height `h`:
  - Find left boundary: how far left can we go with height ≥ h?
  - Find right boundary: how far right can we go with height ≥ h?
  - Area = h × width (between boundaries)

**When to Calculate Area?**
- When we encounter a **shorter bar**, it means taller bars can't extend further right
- We calculate area for all bars that can't extend anymore
- Stack helps us know the left boundary (previous smaller bar)

**Why Monotonic Stack?**
- Stack maintains indices in **increasing order of heights**
- When we find a shorter bar:
  - Bars in stack can't extend further right (right boundary found)
  - Previous element in stack gives left boundary
  - We have both boundaries → calculate area!

### Solutions

**Approach 1: Brute Force**
```javascript
function largestRectangleArea(heights) {
    let maxArea = 0;
    
    for (let i = 0; i < heights.length; i++) {
        let minHeight = heights[i];
        
        // Try all possible right boundaries
        for (let j = i; j < heights.length; j++) {
            minHeight = Math.min(minHeight, heights[j]);
            const width = j - i + 1;
            const area = minHeight * width;
            maxArea = Math.max(maxArea, area);
        }
    }
    
    return maxArea;
}
```

**Approach 2: Monotonic Stack (Optimal)**
```javascript
function largestRectangleArea(heights) {
    const stack = []; // Stores indices
    let maxArea = 0;
    
    for (let i = 0; i < heights.length; i++) {
        // When current bar is shorter, calculate areas for taller bars
        while (stack.length > 0 && heights[i] < heights[stack[stack.length - 1]]) {
            const heightIndex = stack.pop();
            const height = heights[heightIndex];
            
            // Width calculation:
            // - Right boundary: i (current position, exclusive)
            // - Left boundary: stack.top + 1 (or 0 if stack empty)
            const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1;
            
            maxArea = Math.max(maxArea, height * width);
        }
        stack.push(i);
    }
    
    // Process remaining bars in stack
    while (stack.length > 0) {
        const heightIndex = stack.pop();
        const height = heights[heightIndex];
        const width = stack.length === 0 
            ? heights.length 
            : heights.length - stack[stack.length - 1] - 1;
        maxArea = Math.max(maxArea, height * width);
    }
    
    return maxArea;
}
```

**Approach 3: Stack with Sentinel (Cleaner)**
```javascript
function largestRectangleArea(heights) {
    const stack = [];
    let maxArea = 0;
    heights.push(0); // Sentinel to flush remaining bars
    
    for (let i = 0; i < heights.length; i++) {
        while (stack.length > 0 && heights[i] < heights[stack[stack.length - 1]]) {
            const heightIndex = stack.pop();
            const height = heights[heightIndex];
            const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        stack.push(i);
    }
    
    heights.pop(); // Remove sentinel
    return maxArea;
}
```

**Time Complexity:**
- Brute Force: O(n²)
- Monotonic Stack: O(n) - each element pushed/popped once

**Space Complexity:** O(n) for stack

### Understanding Width Calculation

**The Tricky Part:**
```javascript
const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1;
```

**Why this formula?**
- **Right boundary:** `i` (where we found shorter bar, exclusive)
- **Left boundary:**
  - If stack empty: can extend all the way to index 0
  - Otherwise: `stack.top + 1` (one position after previous smaller bar)
- **Width:** `right - left = i - (stack.top + 1) = i - stack.top - 1`

**Example:**
```
heights = [2,1,5,6,2,3]
indices    0 1 2 3 4 5

When i=4 (height=2), popping index 3 (height=6):
- Right boundary: 4 (exclusive)
- Stack after popping 3: [1,2]
- Stack.top = 2
- Left boundary: 2 + 1 = 3 (the bar itself)
- Width = 4 - 2 - 1 = 1 (only index 3)
- Area = 6 × 1 = 6

When i=4, popping index 2 (height=5):
- Right boundary: 4 (exclusive)
- Stack after popping 2: [1]
- Stack.top = 1
- Left boundary: 1 + 1 = 2
- Width = 4 - 1 - 1 = 2 (indices 2,3)
- Area = 5 × 2 = 10 ✓
```

### Walkthrough Example

```
heights = [2,1,5,6,2,3]

i=0, h=2: Stack: [0]
i=1, h=1: 1<2 → pop 0, calculate area
          height=2, width=1, area=2
          Stack: [1]
i=2, h=5: Stack: [1,2]
i=3, h=6: Stack: [1,2,3]
i=4, h=2: 2<6 → pop 3, area=6×1=6
          2<5 → pop 2, area=5×2=10 ✓
          Stack: [1,4]
i=5, h=3: Stack: [1,4,5]

Process remaining:
- Pop 5: height=3, width=1, area=3
- Pop 4: height=2, width=4, area=8
- Pop 1: height=1, width=6, area=6

maxArea = 10
```

### Key Insights
- Stack maintains indices in **increasing order of heights**
- When we find a **shorter bar**, taller bars can't extend further
- We calculate area when we know both **left and right boundaries**
- Width calculation is the trickiest part - understand it well!

---

## Q5. Exclusive Time of Functions

**Difficulty:** Medium  
**Pattern:** Call Stack Simulation

### Problem Statement
On a single-threaded CPU, we execute a program containing `n` functions. Each function has a unique ID between `0` and `n-1`.

Function calls are stored in a call stack: when a function call starts, its ID is pushed onto the stack, and when a function call ends, its ID is popped off the stack. The function whose ID is at the top of the stack is the current function being executed.

You are given a list `logs`, where `logs[i]` represents the `ith` log message formatted as `"{function_id}:{"start" | "end"}:{timestamp}"`.

A function's **exclusive time** is the sum of execution times for all function calls in the program (excluding time spent in nested function calls).

Return the exclusive time of each function in an array.

### Examples
```javascript
Input: n = 2, logs = ["0:start:0","1:start:2","1:end:5","0:end:6"]
Output: [3,4]
Explanation:
Function 0: executes from 0-1 (2 units) and 6-6 (1 unit) = 3 units
Function 1: executes from 2-5 (4 units) = 4 units

Timeline:
Time: 0 1 2 3 4 5 6
F0:   [─────]       [─]
F1:       [─────────]

Input: n = 1, logs = ["0:start:0","0:start:2","0:end:5","0:start:6","0:end:6","0:end:7"]
Output: [8]
Explanation: Recursive calls, total time = 2+4+1+1 = 8

Input: n = 2, logs = ["0:start:0","0:start:2","0:end:5","1:start:6","1:end:6","0:end:7"]
Output: [7,1]
```

### Constraints
- `1 <= n <= 100`
- `2 <= logs.length <= 500`
- `0 <= function_id < n`
- `0 <= timestamp <= 10^9`
- No two start events at same timestamp
- No two end events at same timestamp
- Each function has "end" for each "start"

### Logic Explanation

**Key Concepts:**

1. **Exclusive Time:** Only counts time when function is actively executing (not waiting for nested calls)

2. **Timestamp Interpretation:**
   - **"start" at T:** Function begins at **beginning** of timestamp T
   - **"end" at T:** Function ends at **end** of timestamp T
   - Duration from start:2 to end:5 = 5-2+1 = 4 units

3. **Call Stack Behavior:**
   - When function starts → push to stack
   - When function ends → pop from stack
   - Top of stack = currently executing function

**Strategy:**

- Use a stack to track the call stack
- Track `prevTime` to know when last event occurred
- **For "start" events:**
  - Calculate time for currently running function (if any)
  - Push new function to stack
  - Update prevTime
- **For "end" events:**
  - Calculate time for ending function (including the end timestamp)
  - Pop function from stack
  - Update prevTime to after this timestamp

### Solution
```javascript
function exclusiveTime(n, logs) {
    const result = new Array(n).fill(0);
    const stack = [];
    let prevTime = 0;
    
    for (let log of logs) {
        const [id, type, time] = log.split(':');
        const funcId = parseInt(id);
        const timestamp = parseInt(time);
        
        if (type === 'start') {
            // If there's a function currently running, add its execution time
            if (stack.length > 0) {
                result[stack[stack.length - 1]] += timestamp - prevTime;
            }
            // Push new function to stack
            stack.push(funcId);
            prevTime = timestamp;
        } else {
            // 'end' event
            // Pop the function and add its execution time
            // +1 because 'end' includes the full timestamp
            result[stack.pop()] += timestamp - prevTime + 1;
            prevTime = timestamp + 1; // Next function starts after this
        }
    }
    
    return result;
}
```

**Time Complexity:** O(m) where m = logs.length  
**Space Complexity:** O(n) for result array + O(n) for stack

### Walkthrough Example

```
n = 2, logs = ["0:start:0","1:start:2","1:end:5","0:end:6"]

Log: "0:start:0"
  - Stack empty, no time to add
  - Push 0, prevTime = 0
  - Stack: [0]

Log: "1:start:2"
  - Function 0 was running from 0 to 2 → 2-0 = 2 units
  - result[0] += 2 → result = [2,0]
  - Push 1, prevTime = 2
  - Stack: [0,1]

Log: "1:end:5"
  - Function 1 ran from 2 to 5 → 5-2+1 = 4 units
  - result[1] += 4 → result = [2,4]
  - Pop 1, prevTime = 6
  - Stack: [0]

Log: "0:end:6"
  - Function 0 ran from 6 to 6 → 6-6+1 = 1 unit
  - result[0] += 1 → result = [3,4]
  - Pop 0, prevTime = 7
  - Stack: []

Output: [3,4] ✓
```

### Key Insights

**Why +1 for "end" events?**
```javascript
result[stack.pop()] += timestamp - prevTime + 1;
```
Because "end" at timestamp T means the function ran through the **entire** timestamp T.

Example: start:2, end:5 means:
- Started at beginning of 2
- Ended at end of 5
- Duration: timestamps 2,3,4,5 = 4 units = 5-2+1

**Why timestamp+1 for prevTime after "end"?**
```javascript
prevTime = timestamp + 1;
```
The next function (if resumed) starts **after** the current timestamp ends.

**Common Mistakes:**
- Forgetting the +1 for "end" timestamps
- Not updating prevTime correctly
- Confusing exclusive vs inclusive time

---

## Pattern Recognition Guide

### When to Use Monotonic Stack?

**Indicators:**
- ✅ Need to find next/previous greater/smaller element
- ✅ Only care about the **first** such element
- ✅ Processing array left-to-right or right-to-left
- ✅ Need O(n) solution for large inputs

### Common Monotonic Stack Patterns

| Pattern | Stack Order | While Condition | Example Problem |
|---------|-------------|-----------------|-----------------|
| **Next Greater** | Decreasing values | `arr[i] > arr[stack.top]` | Daily Temperatures |
| **Next Smaller** | Increasing values | `arr[i] < arr[stack.top]` | Largest Rectangle |
| **Next Smaller/Equal** | Increasing values | `arr[i] <= arr[stack.top]` | Final Prices |
| **Previous Greater** | Process right-to-left | Same as Next Greater | Stock Span |
| **Both Sides** | Two passes | Depends on problem | Largest Rectangle |

### Stack Problem Template

```javascript
function solveWithStack(arr) {
    const stack = [];
    const result = new Array(arr.length);
    
    for (let i = 0; i < arr.length; i++) {
        // While condition depends on pattern
        while (stack.length > 0 && CONDITION(arr[i], arr[stack[stack.length - 1]])) {
            const idx = stack.pop();
            // Process popped element
            result[idx] = CALCULATE(i, idx);
        }
        stack.push(i);
    }
    
    // Handle remaining elements in stack if needed
    while (stack.length > 0) {
        const idx = stack.pop();
        result[idx] = DEFAULT_VALUE;
    }
    
    return result;
}
```

### Time Complexity Analysis

**Why O(n) despite nested loops?**

Each element is:
- Pushed to stack **exactly once** → n operations
- Popped from stack **at most once** → at most n operations

Total: 2n = O(n)

This is called **amortized analysis**.

---

## Interview Tips

### Problem-Solving Strategy

1. **Recognize the Pattern**
   - "Next greater/smaller element" → Monotonic Stack
   - "Call stack simulation" → Regular Stack
   - "Boundaries for rectangles" → Monotonic Stack with width calculation

2. **Start with Brute Force**
   - Always mention O(n²) solution first
   - Shows you understand the problem

3. **Optimize with Stack**
   - Explain why stack helps
   - Describe the stack invariant

4. **Walk Through Example**
   - Show how stack evolves
   - Explain each push/pop

5. **Handle Edge Cases**
   - Empty array
   - Single element
   - All same values
   - Strictly increasing/decreasing

### Common Mistakes to Avoid

1. **Off-by-one errors** in width calculation
2. **Forgetting** to process remaining elements in stack
3. **Wrong condition** in while loop
4. **Not handling** the +1 for inclusive timestamps
5. **Confusing** indices vs values

### Related Problems

**Easy:**
- Valid Parentheses
- Min Stack
- Next Greater Element I
- Remove All Adjacent Duplicates In String

**Medium:**
- Next Greater Element II
- Asteroid Collision
- Online Stock Span
- Sum of Subarray Minimums
- Remove K Digits

**Hard:**
- Trapping Rain Water
- Maximal Rectangle
- Longest Valid Parentheses

---

## Summary

### Key Takeaways

1. **Monotonic Stack** maintains elements in increasing/decreasing order
2. **Next Greater/Smaller** problems are perfect for monotonic stacks
3. **Time Complexity** is O(n) due to each element being pushed/popped once
4. **Stack Invariant** is crucial - understand what order stack maintains
5. **Width Calculation** in rectangle problems needs careful attention
6. **Call Stack Simulation** uses regular stack to track execution state

### Pattern Recognition Checklist

- [ ] Does the problem ask for "next" or "previous" element?
- [ ] Do we need the "first" element that satisfies a condition?
- [ ] Is the condition based on comparison (greater/smaller)?
- [ ] Would a brute force solution be O(n²) with nested loops?
- [ ] Are we processing elements sequentially?

If you answered **yes** to most of these → **Use Monotonic Stack!**

### Quick Reference Table

| Problem Type | Pattern | Key Indicator |
|-------------|---------|---------------|
| Next warmer day | Next Greater | Find first larger to right |
| Discount pricing | Next Smaller/Equal | Find first ≤ to right |
| Rectangle area | Next Smaller (both) | Find boundaries |
| Function execution | Call stack | Track nested calls |
| Stream processing | Stack ops | Sequential operations |

---

**Happy Coding! 🚀**

For more DSA problems and solutions, check out my GitHub repository.