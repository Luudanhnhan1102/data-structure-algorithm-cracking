# Topics:
#array, #binary-search

# Intuition
The goal is to find the position where a target value should be inserted into a sorted array to maintain its order. If the target is already present in the array, we return its index. Otherwise, we return the index where it would be inserted.

---
# Approach: Binary Search
The problem can be efficiently solved using a binary search approach due to the sorted nature of the array. Binary search allows us to quickly narrow down the possible insertion point by repeatedly dividing the search interval in half.

## Explanation:
1. **Initialization**:
    - Start with two pointers, `low` and `high`, representing the current search interval within the array.

2. **Binary Search**:
    - Calculate the middle index `mid` of the current interval.
    - If the middle element is equal to the target, return `mid` as the target is found.
    - If the middle element is less than the target, adjust the `low` pointer to `mid + 1` to search the right half.
    - If the middle element is greater than the target, adjust the `high` pointer to `mid - 1` to search the left half.

3. **Termination**:
    - If the search interval is exhausted (`low > high`), the target is not in the array. The `low` pointer will be at the position where the target should be inserted to maintain the sorted order.

## Complexity
- Time complexity: `O(log⁡n)`, where n is the number of elements in the array. This is because binary search halves the search interval with each step.
- Space complexity: `O(log⁡n)`  due to the recursion stack in the worst case. However, it can be optimized to `O(1)` using an iterative approach.

---
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
