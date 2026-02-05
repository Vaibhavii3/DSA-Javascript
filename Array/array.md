# Array Problems - Solutions in JavaScript

A collection of array manipulation problems solved using JavaScript.

---

## Q1. Concatenation of Array

**Difficulty:** Easy

### Problem Statement
Given an integer array `nums` of length `n`, you want to create an array `ans` of length `2n` where `ans[i] == nums[i]` and `ans[i + n] == nums[i]` for `0 <= i < n` (0-indexed).

Specifically, `ans` is the concatenation of two `nums` arrays.

Return the array `ans`.

### Example
```
Input: nums = [1,2,3]
Output: [1,2,3,1,2,3]
```

### Logic Explanation
- Take the original array and repeat it twice
- First half contains the original array
- Second half is a copy of the original array
- Simply concatenate the array with itself

### Solution
```javascript
function getConcatenation(nums) {
    return nums.concat(nums);
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

## Q2. Shuffle the Array

**Difficulty:** Easy

### Problem Statement
Given the array `nums` consisting of `2n` elements in the form `[x1,x2,...,xn,y1,y2,...,yn]`.

Return the array in the form `[x1,y1,x2,y2,...,xn,yn]`.

### Example
```
Input: nums = [2,5,1,3,4,7], n = 3
Output: [2,3,5,4,1,7]
Explanation: x1=2, x2=5, x3=1, y1=3, y2=4, y3=7
Result: [x1,y1,x2,y2,x3,y3] = [2,3,5,4,1,7]
```

### Logic Explanation
- Array has two halves: first half (x values) and second half (y values)
- We need to interleave these two halves
- For each index `i` from 0 to n-1:
  - Take element from first half: `nums[i]`
  - Take element from second half: `nums[i + n]`
  - Add them consecutively to result

### Solution
```javascript
function shuffle(nums, n) {
    let result = [];
    for (let i = 0; i < n; i++) {
        result.push(nums[i]);       // xi
        result.push(nums[i + n]);   // yi
    }
    return result;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

## Q3. Max Consecutive Ones

**Difficulty:** Easy

### Problem Statement
Given a binary array `nums`, return the maximum number of consecutive `1`'s in the array.

### Example
```
Input: nums = [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s. 
The maximum number of consecutive 1s is 3.
```

### Logic Explanation
- Keep track of current streak of 1's
- Keep track of maximum streak seen so far
- When we see a 1: increment current count
- When we see a 0: reset current count to 0
- Always update maximum if current count is larger

### Solution
```javascript
function findMaxConsecutiveOnes(nums) {
    let maxCount = 0;
    let currentCount = 0;
    
    for (let num of nums) {
        if (num === 1) {
            currentCount++;
            maxCount = Math.max(maxCount, currentCount);
        } else {
            currentCount = 0;
        }
    }
    
    return maxCount;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

## Q4. Set Mismatch

**Difficulty:** Easy

### Problem Statement
You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in repetition of one number and loss of another number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return them in the form of an array.

### Example
```
Input: nums = [1,2,2,4]
Output: [2,3]
Explanation: 2 appears twice, 3 is missing
```

### Logic Explanation

**Using Set (More Efficient)**
- Use a Set to find the duplicate while iterating
- Then check which number from 1 to n is not in the Set

### Solution

```javascript
function findErrorNums(nums) {
    const n = nums.length;
    const seen = new Set();
    let duplicate = 0;
    
    // Find duplicate
    for (let num of nums) {
        if (seen.has(num)) {
            duplicate = num;
        }
        seen.add(num);
    }
    
    // Find missing
    let missing = 0;
    for (let i = 1; i <= n; i++) {
        if (!seen.has(i)) {
            missing = i;
            break;
        }
    }
    
    return [duplicate, missing];
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

## Q5. How Many Numbers Are Smaller Than the Current Number

**Difficulty:** Easy

### Problem Statement
Given the array `nums`, for each `nums[i]` find out how many numbers in the array are smaller than it. That is, for each `nums[i]` you have to count the number of valid `j`'s such that `j != i` and `nums[j] < nums[i]`.

Return the answer in an array.

### Example
```
Input: nums = [8,1,2,2,3]
Output: [4,0,1,1,3]
Explanation: 
For nums[0]=8 there exist four smaller numbers (1, 2, 2, 3)
For nums[1]=1 there exist zero smaller numbers
For nums[2]=2 there exist one smaller number (1)
For nums[3]=2 there exist one smaller number (1)
For nums[4]=3 there exist three smaller numbers (1, 2, 2)
```

### Logic Explanation

**Brute Force**
- For each element, count how many elements are smaller
- Compare with all other elements

### Solution

**Approach 1: Brute Force**
```javascript
function smallerNumbersThanCurrent(nums) {
    const result = [];
    
    for (let i = 0; i < nums.length; i++) {
        let count = 0;
        for (let j = 0; j < nums.length; j++) {
            if (nums[j] < nums[i]) {
                count++;
            }
        }
        result.push(count);
    }
    
    return result;
}
```

**Time Complexity:** O(n²)  
**Space Complexity:** O(n)

---

## Q6. Find All Numbers Disappeared in an Array

**Difficulty:** Easy

### Problem Statement
Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

### Example
```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
Explanation: Numbers from 1 to 8 should be present. 5 and 6 are missing.

Input: nums = [1,1]
Output: [2]
Explanation: Numbers from 1 to 2 should be present. 2 is missing.
```

### Constraints
- `n == nums.length`
- `1 <= n <= 10^5`
- `1 <= nums[i] <= n`

### Logic Explanation

**Using Set**
- Create a Set with all numbers from the array (removes duplicates automatically)
- Loop from 1 to n and check which numbers are not in the Set
- Those missing numbers are our answer


### Solution

**Using Set**
```javascript
function findDisappearedNumbers(nums) {
    const n = nums.length;
    const present = new Set(nums);
    const result = [];
    
    for (let i = 1; i <= n; i++) {
        if (!present.has(i)) {
            result.push(i);
        }
    }
    
    return result;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

## Q7. Build an Array With Stack Operations

**Difficulty:** Medium

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

### Example
```
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
**Space Complexity:** O(operations count)

### Code Walkthrough

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

### Key Points
- Stream numbers come in sequential order (1, 2, 3, ...)
- If stream number is in target → just Push
- If stream number is NOT in target → Push then Pop (to skip it)
- Stop processing when all target elements are added

---