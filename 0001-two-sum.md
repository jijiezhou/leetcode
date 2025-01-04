### 01 Description

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums + nums == 9, we return [0, 1].
```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

**Constraints:**

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`
- **Only one valid answer exists.**

## 02 Idea

- **Why think of using a Hash Table, and why a map?**  
  We want to know if we've seen the element before (and where); we need a structure with key-value pairs of element and index. A map fits well because it stores both the element and its index.

- **Why not use a two-pointer (left and right) approach?**
    1. A typical two-pointer solution depends on sorting. Once sorted, the original indices are changed, but this problem requires returning the *original* indices.
    2. We can still sort if we keep a record of the original indices, but that requires extra data structure that might be more complex than a straightforward map approach.

## 03 Approach

- The map’s key stores each element’s value, while the value stores its index.
- If you try a two-pass strategy, you first store all elements in the map, then check if the complement of the current element exists.
- You can reduce two-pass to one-pass by checking for the complement before you insert the current element into the map.


1. **Brute Force**
    - Loop through each element x, then use a nested loop to find if there is another value that equals `target - x`.

2. **Two-Pass Hash Table**
    - In the first pass, record all elements and their indices in a map.
    - In the second pass, for each element, check if `target - nums[i]` exists in the map. Also ensure you’re not using the same element twice by verifying indices are different.

3. **One-Pass Hash Table**
    - While iterating through the array, check if `target - nums[i]` is already in the map. If it is, return the pair of indices immediately.
    - If not, put the current element and its index into the map.

4. **Two Pointers**
    - Sort the array with a copy of the original indices, then use two pointers from both ends. Depending on the sum, move left or right pointers until you find the target pair. Then retrieve original indices from your record.

3. Follow up: Two Pointers
   - clone original, left and right pointer, compare with target
   - if hit, then find corresponding index from original array
   - deduplicate

### 04 Answer

```
//1. Brute Force
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length - 1; i++){
            for (int j = i + 1; j < nums.length; j++){
                if (nums[i] + nums[j] == target)
                    return new int[]{i, j};
            }
        }
        return null;
    }
}

//2. Two-pass Hash Table
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++){
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; i++){
            int need = target - nums[i];
            if (map.containsKey(need) && map.get(need) != i)
                return new int[]{i, map.get(need)};
        }
        return null;
    }
}

//3. One-pass Hash Table
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++){
            int need = target - nums[i];
            if (map.containsKey(need)){
                return new int[]{i, map.get(need)};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}

//4. Two Pointers
to do: implement
```

### 05 Complexity
1. time complexity: O(n)
2. space complexity: O(n)
