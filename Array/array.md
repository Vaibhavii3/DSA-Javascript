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
