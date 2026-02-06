# Array Problems - Solutions in JavaScript

A comprehensive collection of array manipulation problems solved using JavaScript.

---

## Table of Contents
1. [Concatenation of Array](#q1-concatenation-of-array)
2. [Shuffle the Array](#q2-shuffle-the-array)
3. [Max Consecutive Ones](#q3-max-consecutive-ones)
4. [Set Mismatch](#q4-set-mismatch)
5. [How Many Numbers Are Smaller Than the Current Number](#q5-how-many-numbers-are-smaller-than-the-current-number)
6. [Find All Numbers Disappeared in an Array](#q6-find-all-numbers-disappeared-in-an-array)

---

## Q1. Concatenation of Array

**Difficulty:** Easy  
**Pattern:** Array Manipulation

### Problem Statement
Given an integer array `nums` of length `n`, you want to create an array `ans` of length `2n` where `ans[i] == nums[i]` and `ans[i + n] == nums[i]` for `0 <= i < n` (0-indexed).

Specifically, `ans` is the concatenation of two `nums` arrays.

Return the array `ans`.

### Examples
```javascript
Input: nums = [1,2,3]
Output: [1,2,3,1,2,3]

Input: nums = [1,3,2,1]
Output: [1,3,2,1,1,3,2,1]

Input: nums = [1,2,1]
Output: [1,2,1,1,2,1]
```

### Constraints
- `n == nums.length`
- `1 <= n <= 1000`
- `1 <= nums[i] <= 1000`

### Logic Explanation
- Take the original array and repeat it twice
- First half contains the original array
- Second half is a copy of the original array
- Simply concatenate the array with itself

### Solutions

**Approach 1: Using concat()**
```javascript
function getConcatenation(nums) {
    return nums.concat(nums);
}
```

**Approach 2: Using spread operator**
```javascript
function getConcatenation(nums) {
    return [...nums, ...nums];
}
```

**Approach 3: Manual loop**
```javascript
function getConcatenation(nums) {
    const result = [];
    const n = nums.length;
    
    for (let i = 0; i < n; i++) {
        result.push(nums[i]);
    }
    for (let i = 0; i < n; i++) {
        result.push(nums[i]);
    }
    
    return result;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n) - for the new array

---

## Q2. Shuffle the Array

**Difficulty:** Easy  
**Pattern:** Array Interleaving

### Problem Statement
Given the array `nums` consisting of `2n` elements in the form `[x1,x2,...,xn,y1,y2,...,yn]`.

Return the array in the form `[x1,y1,x2,y2,...,xn,yn]`.

### Examples
```javascript
Input: nums = [2,5,1,3,4,7], n = 3
Output: [2,3,5,4,1,7]
Explanation: x1=2, x2=5, x3=1, y1=3, y2=4, y3=7
Result: [x1,y1,x2,y2,x3,y3] = [2,3,5,4,1,7]

Input: nums = [1,2,3,4,4,3,2,1], n = 4
Output: [1,4,2,3,3,2,4,1]

Input: nums = [1,1,2,2], n = 2
Output: [1,2,1,2]
```

### Constraints
- `1 <= n <= 500`
- `nums.length == 2n`
- `1 <= nums[i] <= 10^3`

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
    const result = [];
    
    for (let i = 0; i < n; i++) {
        result.push(nums[i]);       // xi
        result.push(nums[i + n]);   // yi
    }
    
    return result;
}
```

**Alternative approach using single loop:**
```javascript
function shuffle(nums, n) {
    const result = new Array(2 * n);
    
    for (let i = 0; i < n; i++) {
        result[2 * i] = nums[i];        // x values at even indices
        result[2 * i + 1] = nums[i + n]; // y values at odd indices
    }
    
    return result;
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n) - for the result array

---

## Q3. Max Consecutive Ones

**Difficulty:** Easy  
**Pattern:** Sliding Window / Counting

### Problem Statement
Given a binary array `nums`, return the maximum number of consecutive `1`'s in the array.

### Examples
```javascript
Input: nums = [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s. 
The maximum number of consecutive 1s is 3.

Input: nums = [1,0,1,1,0,1]
Output: 2

Input: nums = [1,1,1,1,1]
Output: 5
```

### Constraints
- `1 <= nums.length <= 10^5`
- `nums[i]` is either `0` or `1`

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

**Walkthrough Example:**
```
nums = [1,1,0,1,1,1]

Index 0 (num=1): currentCount=1, maxCount=1
Index 1 (num=1): currentCount=2, maxCount=2
Index 2 (num=0): currentCount=0, maxCount=2
Index 3 (num=1): currentCount=1, maxCount=2
Index 4 (num=1): currentCount=2, maxCount=2
Index 5 (num=1): currentCount=3, maxCount=3

Output: 3
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

## Q4. Set Mismatch

**Difficulty:** Easy  
**Pattern:** Hash Set / Array

### Problem Statement
You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in repetition of one number and loss of another number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return them in the form of an array.

### Examples
```javascript
Input: nums = [1,2,2,4]
Output: [2,3]
Explanation: 2 appears twice, 3 is missing

Input: nums = [1,1]
Output: [1,2]

Input: nums = [3,2,3,4,6,5]
Output: [3,1]
```

### Constraints
- `2 <= nums.length <= 10^4`
- `1 <= nums[i] <= 10^4`

### Logic Explanation

**Using Set (Efficient Approach)**
- Use a Set to find the duplicate while iterating
- Then check which number from 1 to n is not in the Set

**Alternative: Using Math**
- Expected sum = 1+2+3+...+n = n*(n+1)/2
- Actual sum = sum of array
- Sum with duplicate = actual sum
- Missing number = expected - actual + duplicate

### Solutions

**Approach 1: Using Set**
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

**Approach 2: Using Frequency Array**
```javascript
function findErrorNums(nums) {
    const n = nums.length;
    const count = new Array(n + 1).fill(0);
    let duplicate = 0, missing = 0;
    
    // Count frequency of each number
    for (let num of nums) {
        count[num]++;
    }
    
    // Find duplicate and missing
    for (let i = 1; i <= n; i++) {
        if (count[i] === 2) duplicate = i;
        if (count[i] === 0) missing = i;
    }
    
    return [duplicate, missing];
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

## Q5. How Many Numbers Are Smaller Than the Current Number

**Difficulty:** Easy  
**Pattern:** Counting / Sorting

### Problem Statement
Given the array `nums`, for each `nums[i]` find out how many numbers in the array are smaller than it. That is, for each `nums[i]` you have to count the number of valid `j`'s such that `j != i` and `nums[j] < nums[i]`.

Return the answer in an array.

### Examples
```javascript
Input: nums = [8,1,2,2,3]
Output: [4,0,1,1,3]
Explanation: 
For nums[0]=8 there exist four smaller numbers (1, 2, 2, 3)
For nums[1]=1 there exist zero smaller numbers
For nums[2]=2 there exist one smaller number (1)
For nums[3]=2 there exist one smaller number (1)
For nums[4]=3 there exist three smaller numbers (1, 2, 2)

Input: nums = [6,5,4,8]
Output: [2,1,0,3]

Input: nums = [7,7,7,7]
Output: [0,0,0,0]
```

### Constraints
- `2 <= nums.length <= 500`
- `0 <= nums[i] <= 100`

### Logic Explanation

**Brute Force Approach:**
- For each element, count how many elements are smaller
- Compare with all other elements

**Optimized Approach:**
- Sort the array while keeping track of original indices
- Use the sorted position to determine count
- Handle duplicates carefully

### Solutions

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

**Approach 2: Sorting with Index Mapping**
```javascript
function smallerNumbersThanCurrent(nums) {
    // Create array of [value, originalIndex] pairs
    const sorted = nums.map((num, idx) => [num, idx]).sort((a, b) => a[0] - b[0]);
    const result = new Array(nums.length);
    
    for (let i = 0; i < sorted.length; i++) {
        const [value, originalIdx] = sorted[i];
        
        // Count how many numbers are smaller
        // Handle duplicates by finding first occurrence
        let count = i;
        while (count > 0 && sorted[count - 1][0] === value) {
            count--;
        }
        
        result[originalIdx] = count;
    }
    
    return result;
}
```

**Approach 3: Counting Sort (Best for small range)**
```javascript
function smallerNumbersThanCurrent(nums) {
    const count = new Array(101).fill(0);
    
    // Count frequency of each number
    for (let num of nums) {
        count[num]++;
    }
    
    // Convert to cumulative count
    for (let i = 1; i < 101; i++) {
        count[i] += count[i - 1];
    }
    
    // Build result
    const result = [];
    for (let num of nums) {
        result.push(num === 0 ? 0 : count[num - 1]);
    }
    
    return result;
}
```

**Time Complexity:** 
- Brute Force: O(n²)
- Sorting: O(n log n)
- Counting Sort: O(n + k) where k is range of values

**Space Complexity:** O(n) or O(k) depending on approach

---

## Q6. Find All Numbers Disappeared in an Array

**Difficulty:** Easy  
**Pattern:** Hash Set / In-place Modification

### Problem Statement
Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

### Examples
```javascript
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
Explanation: Numbers from 1 to 8 should be present. 5 and 6 are missing.

Input: nums = [1,1]
Output: [2]
Explanation: Numbers from 1 to 2 should be present. 2 is missing.

Input: nums = [1,2,3,4,5]
Output: []
```

### Constraints
- `n == nums.length`
- `1 <= n <= 10^5`
- `1 <= nums[i] <= n`

### Logic Explanation

**Using Set (Simple Approach):**
- Create a Set with all numbers from the array (removes duplicates automatically)
- Loop from 1 to n and check which numbers are not in the Set
- Those missing numbers are our answer

**In-place Approach (Space Optimized):**
- Mark visited numbers by making the value at that index negative
- Numbers at positive indices were never visited (missing)

### Solutions

**Approach 1: Using Set**
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

**Approach 2: In-place Marking (O(1) extra space)**
```javascript
function findDisappearedNumbers(nums) {
    // Mark visited numbers by negating values
    for (let i = 0; i < nums.length; i++) {
        const index = Math.abs(nums[i]) - 1;
        if (nums[index] > 0) {
            nums[index] = -nums[index];
        }
    }
    
    // Collect indices with positive values (missing numbers)
    const result = [];
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] > 0) {
            result.push(i + 1);
        }
    }
    
    return result;
}
```

**Walkthrough (In-place approach):**
```
nums = [4,3,2,7,8,2,3,1]

Step 1: Mark visited
i=0, num=4: Mark index 3 → [4,3,2,-7,8,2,3,1]
i=1, num=3: Mark index 2 → [4,3,-2,-7,8,2,3,1]
i=2, num=2: Mark index 1 → [4,-3,-2,-7,8,2,3,1]
i=3, num=7: Mark index 6 → [4,-3,-2,-7,8,2,-3,1]
i=4, num=8: Mark index 7 → [4,-3,-2,-7,8,2,-3,-1]
i=5, num=2: Already negative at index 1
i=6, num=3: Already negative at index 2
i=7, num=1: Mark index 0 → [-4,-3,-2,-7,8,2,-3,-1]

Step 2: Find positive indices
Index 4 is positive (8) → missing number = 5
Index 5 is positive (2) → missing number = 6

Output: [5,6]
```

**Time Complexity:** O(n)  
**Space Complexity:** 
- Set approach: O(n)
- In-place approach: O(1) excluding output

---

## Pattern Recognition Guide

### Array Problem Patterns

| Pattern | Characteristics | Example Problems |
|---------|----------------|------------------|
| **Two Pointers** | Process from both ends | Two Sum, Container With Most Water |
| **Sliding Window** | Consecutive subsequence | Max Consecutive Ones |
| **Hash Set/Map** | Fast lookups, duplicates | Set Mismatch, Disappeared Numbers |
| **In-place Modification** | Modify array to track state | Disappeared Numbers |
| **Sorting** | Order matters | Smaller Numbers Than Current |
| **Array Manipulation** | Direct operations | Concatenation, Shuffle |

### Common Techniques

1. **Using Set for uniqueness**
   - Remove duplicates
   - Fast O(1) lookups
   - Find missing/duplicate elements

2. **Two-pass approach**
   - First pass: collect information
   - Second pass: build result

3. **Index as hash key**
   - When values are in range [1, n]
   - Mark visited by negating values
   - Or swap to correct positions

4. **Counting/Frequency array**
   - When value range is small
   - Count occurrences
   - Build result from counts

---

## Interview Tips

### Problem-Solving Strategy

1. **Understand the problem**
   - Read carefully, note constraints
   - Ask clarifying questions

2. **Identify the pattern**
   - Is it about searching, sorting, or manipulation?
   - What's the constraint on time/space?

3. **Start with brute force**
   - Get a working solution first
   - Then optimize

4. **Optimize**
   - Can we use hash map/set?
   - Can we solve in-place?
   - Can we sort first?

5. **Test edge cases**
   - Empty array
   - Single element
   - All same elements
   - Minimum/maximum values

### Common Edge Cases

- Empty array `[]`
- Single element `[1]`
- All duplicates `[5,5,5,5]`
- Already sorted/reverse sorted
- Minimum/maximum constraint values

### Time Complexity Goals

| Array Size | Target Complexity |
|-----------|------------------|
| n ≤ 100 | O(n²) acceptable |
| n ≤ 10³ | O(n log n) preferred |
| n ≤ 10⁵ | O(n) or O(n log n) |
| n > 10⁵ | O(n) required |

---

## Related Problems

### Easy
- Remove Duplicates from Sorted Array
- Remove Element
- Search Insert Position
- Plus One
- Merge Sorted Array

### Medium
- Two Sum II
- 3Sum
- Product of Array Except Self
- Find All Duplicates in an Array
- Sort Colors

### Hard
- First Missing Positive
- Median of Two Sorted Arrays
- Trapping Rain Water

---

## Summary

### Key Takeaways

1. **Hash Set/Map** are powerful for O(1) lookups
2. **Two passes** often simplify complex problems
3. **In-place modification** can save space
4. **Counting arrays** work well for small value ranges
5. **Understand constraints** - they hint at the solution

### Quick Reference

**When to use Set:**
- Finding duplicates/missing elements
- Need fast lookups
- Uniqueness checking

**When to sort:**
- Need ordered data
- Can afford O(n log n)
- Finding pairs/triplets

**When to use frequency array:**
- Small value range (0-100)
- Need to count occurrences
- Can afford O(n+k) space

---

**Happy Coding! 🚀**

For more DSA problems and solutions, check out my GitHub repository.