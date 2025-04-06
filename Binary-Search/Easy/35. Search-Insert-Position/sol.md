# Topics:
#binary-search

# Goal:
- The goal is to find the position where a target value should be inserted into a sorted array to maintain its order.
- If the target is already present in the array, we return its index.
- Otherwise, we return the index where it would be inserted.

# Key Challenges:
- Efficiently finding the insertion position in O(log n) time
- Handling edge cases where the target is at the start/end or not present.

---
# 1. Brute Force Approach
## Intuition
- Iterate through each element until finding the first element >= target, return that index.

**Edge cases:**
- Return 0 if all elements are greater.
- Return the array's length if all elements are smaller.


## Code

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] >= target) {
                return i;
            }
        }
        return nums.length;
    }
}
```

## Time and Space Complexity
#### Time complexity: $O(n)$ in the worst case (target inserted at end)
#### Space complexity: $O(1)$

## Explanation:
### Implementation Details
...

---
# 2. Optimal Approach(Binary Search)
# Intuition
- The problem can be efficiently solved using a binary search approach due to the sorted nature of the array.
- Binary search allows us to quickly narrow down the possible insertion point by repeatedly dividing the search interval in half.

## Code

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0, high = nums.length - 1;

        return recursiveSearch(nums, low, high, target);
    }

    public int recursiveSearch(int[] nums, int low, int high, int target) {
        if (low <= high) {
            int mid = low + (high - low) / 2;

            if (nums[mid] == target)
                return mid;
            if (nums[mid] < target)
                return recursiveSearch(nums, mid + 1, high, target);
            return recursiveSearch(nums, low, mid - 1, target);
        }

        return low;
    }
}
```

## Explanation:
### Key Insights
- ...

### Complexity
- Time complexity: `O(log⁡n)`, where n is the number of elements in the array. This is because binary search halves the search interval with each step.
- Space complexity: `O(1)`
  - **Variables Used:** Only a constant number of variables are needed (left, right, mid).
  - **No Recursion or Extra Memory:** The algorithm is implemented iteratively, avoiding recursion stack overhead.
  - **Input Independence:** The space used does not scale with the input size n.

### Implementation Details
1. **Initialization**:
    - Start with two pointers, `low` and `high`, representing the current search interval within the array.

2. **Binary Search**:
    - Calculate the middle index `mid` of the current interval.
    - If the middle element is equal to the target, return `mid` as the target is found.
    - If the middle element is less than the target, adjust the `low` pointer to `mid + 1` to search the right half.
    - If the middle element is greater than the target, adjust the `high` pointer to `mid - 1` to search the left half.

3. **Termination**:
    - If the search interval is exhausted (`low > high`), the target is not in the array. The `low` pointer will be at the position where the target should be inserted to maintain the sorted order.

---
# Conclusion
## Key Takeaways
- **Time Efficiency:** Binary search is optimal for sorted arrays, reducing the problem size exponentially.
- **Space Efficiency:** Uses constant space, making it suitable for large datasets.

---
# Further Improvements?
## Bitwise Approach:
- Idea: Use bitwise operations to find the insertion point.
- Trade-offs: May offer minor speed gains but reduce readability.
