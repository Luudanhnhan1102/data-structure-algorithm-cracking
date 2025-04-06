# Topics:
#two-pointers

# Goal:
- The goal is to find the next permutation of an array of integers in lexicographical order.


# Key Challenges:
- Understanding the mathematical pattern behind permutations
- Finding the pivot point where the sequence decreases
- Swapping and reversing elements efficiently
- Handling the case where the array is in descending order (last permutation)
- Handling the case when there are duplicate elements

---
# 1. Brute Force Approach
## Intuition
- Generate all possible permutations of the array
- Sort them lexicographically
- Find the given permutation in the sorted list
- Return the next permutation in the list, or the first one if at the end

**Edge cases:**
- Already last permutation case handled by reversing entire array


## Code

```java
class Solution {
    public void nextPermutation(int[] nums) {
        List<List<Integer>> permutations = generatePermutations(nums);
        List<Integer> current = toList(nums);
        List<Integer> next = findNextPermutation(permutations, current);
        
        for (int i = 0; i < nums.length; i++) {
            nums[i] = next.get(i);
        }
    }
    
    private List<List<Integer>> generatePermutations(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), nums);
        Collections.sort(result, (a, b) -> {
            for (int i = 0; i < a.size(); i++) {
                if (!a.get(i).equals(b.get(i))) {
                    return a.get(i) - b.get(i);
                }
            }
            return 0;
        });
        return result;
    }
    
    private void backtrack(List<List<Integer>> result, List<Integer> temp, int[] nums) {
        if (temp.size() == nums.length) {
            result.add(new ArrayList<>(temp));
            return;
        }
        for (int num : nums) {
            if (temp.contains(num)) continue; // This doesn't handle duplicates well
            temp.add(num);
            backtrack(result, temp, nums);
            temp.remove(temp.size() - 1);
        }
    }
    
    private List<Integer> toList(int[] nums) {
        List<Integer> list = new ArrayList<>();
        for (int num : nums) {
            list.add(num);
        }
        return list;
    }
    
    private List<Integer> findNextPermutation(List<List<Integer>> permutations, List<Integer> current) {
        int index = -1;
        for (int i = 0; i < permutations.size(); i++) {
            if (permutations.get(i).equals(current)) {
                index = i;
                break;
            }
        }
        if (index == permutations.size() - 1) {
            return permutations.get(0);
        }
        return permutations.get(index + 1);
    }
}
```

## Time and Space Complexity
#### Time complexity: $O(n! * n log n)$
- Generating all permutations is O(n!)
- Sorting them is O(n! log n!)

#### Space complexity: $O(n!)$
- Need to store all permutations

## Explanation:
### Implementation Details
1. Generate Permutations *(generatePermutations)*:
	- Compute every possible ordering (permutation) of the elements in the array.
	- It uses a recursive backtracking method (backtrack) to generate all possible permutations.
	- Sorting: sorts them lexicographically. This means it orders the lists as if they were numbers in ascending order based on their digits.

2. Convert to Array *(toList)*:
	- Converts the current permutation from a list to an array.

3. Find Next Permutation *(findNextPermutation)*:
	- Find the next permutation in the sorted array.
	- If the current permutation is the last one, return the first one.

---
# 2. Optimal Approach
## Intuition
- **Identify Pivot:** Traverse from the end to find the first element smaller than its successor.
- **Find Successor:** If a pivot exists, find the smallest element larger than it in the suffix.
- **Swap and Reverse:** Swap the pivot with the successor and reverse the suffix to get the smallest permutation.

## Code

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int pivotPointIndex = findPivotPointIndex(nums);
        if (pivotPointIndex != -1) {
            int targetIndex = findSmallestNumGreaterThanPivotIndex(nums, pivotPointIndex);
            swapPivotNumWithTargetNum(nums, pivotPointIndex, targetIndex);
        }

        reverseArray(nums, pivotPointIndex + 1);
    }

    public int findPivotPointIndex(int[] nums) {
        for (int i = nums.length - 2; i >= 0; i--) {
            if (nums[i] < nums[i + 1]) {
                return i;
            }
        }

        return -1; // this means the initial array has descending order => reverse the whole array
    }

    public int findSmallestNumGreaterThanPivotIndex(int[] nums, int pivotPointIndex) {
        for (int i = nums.length - 1; i > pivotPointIndex; i--) {
            if (nums[i] > nums[pivotPointIndex]) {
                return i;
            }
        }

        return -1;
    }

    public void swapPivotNumWithTargetNum(int[] nums, int pivotPointIndex, int targetIndex) {
        int temp = nums[pivotPointIndex];
        nums[pivotPointIndex] = nums[targetIndex];
        nums[targetIndex] = temp;
    }

    public void reverseArray(int[] nums, int startIndex) {
        int i = startIndex, j = nums.length - 1;
        while (i < j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
            i++;
            j--;
        }
    }
}
```

## Explanation:
### Key Insights
- The algorithm uses a two-pointer approach to find the pivot point and swap the pivot with the smallest number greater than it.

### Complexity
- Time complexity: $O(n)$
- Space complexity: $O(1)$ (in-place modifications)

### Implementation Details
1. **Initialization**:
    - Find the pivot point where the sequence decreases
    - If no pivot point is found, reverse the array

2. **Two Pointers**:
    - If a pivot is found (pivotPointIndex != -1), it then locates the rightmost element greater than the pivot using findSmallestNumGreaterThanPivotIndex(nums, pivotPointIndex).
    - Swap the pivot with this target using swapPivotNumWithTargetNum(nums, pivotPointIndex, targetIndex).
    - Reverse the suffix after the pivot point

3. **Termination**:
    - Regardless of whether a pivot was found or not (which covers the edge case of the array being completely non-increasing), the sub-array to the right of the pivot is reversed via reverseArray(nums, pivotPointIndex + 1).
	- This reversal ensures that the tail is reset to its smallest possible order.



---
# Conclusion
## Key Takeaways
- **Key Insight:** Use pivot to determine where the permutation can be increased and reverse the suffix for minimal increase.
- **Efficiency:** Optimal solution runs in linear time with constant space, making it feasible for large inputs.

---
# Further Improvements?
## Approach:
- Idea: 
- Trade-offs:
