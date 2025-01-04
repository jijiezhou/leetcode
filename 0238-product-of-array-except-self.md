### 01 Desc + Example

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

**Constraints:**

-   `2 <= nums.length <= 105`
-   `30 <= nums[i] <= 30`
-   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

### 02 Idea

-   prefix sum

### 03 Approach
1.  Brute Force: for every index i, calculate prefix and suffix product
2.  Prefix + Suffix
    1.  prefix[i]: prefix from index 0 to i
    2.  suffix[i]: suffix from index n - 1 to i
    3.  record a prefix and suffix array, **res[i] = prefix[i - 1] * suffix[i + 1]**
    4.  corner case:
        1.  res length n, but in index 0, we consider prefix[-1] and index n - 1, we consider suffix[n]
        2.  we can use suffix length n + 2 and a buffer array n + 2
3.  Optimization 1: deleting those corner cases → change definition
    1.  **res[i] = prefix[i] * suffix[i]**
        1.  prefix[i]: prefix from index 0 to i - 1
        2.  suffix[i]: suffix from index n - 1 to i + 1
    2.  prefix and suffix all equal length n, and don't need buffer
    3.  corner case
        1.  prefix[0]: omit the index 0, so that 1
        2.  prefix[n - 1]: omit the index n - 1, so preProduct(0 .. n-2)
        3.  suffix[n - 1]: omit the index n - 1, so that 1
        4.  suffix[0]: omit the index 0, so preProduct(1 .. n-1)
4.  Optimization 2: O(1)
    1.  Intuition:
        1.  output array doesn't count → take advantage of res array
        2.  **store prefix/ suffix in res at first, then use a variable to store suffix/ prefix**

### 04 Code

```
//1. Prefix Suffix
//1.A res[i] = pre[i-1] * suf[i+1] buffer n+2
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] prefix = new int[n + 1];
        int[] suffix = new int[n + 2];
        int[] buffer = new int[n + 2];
        int[] res = new int[n];
        prefix[0] = 1;
        suffix[n + 1] = 1;

        for (int i = 1; i <= n; i++){
            prefix[i] = prefix[i - 1] * nums[i - 1];
        }

        for (int i = n; i >= 1; i--){
            suffix[i] = suffix[i + 1] * nums[i - 1];
        }

        for (int i = 1; i <= n; i++){
            buffer[i] = prefix[i - 1] * suffix[i + 1];
        }

        for (int i = 1; i <= n; i++){
            res[i - 1] = buffer[i];
        }

        return res;
    }
}

//1.B res[i] = pre[i] * suf[i]
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] prefix = new int[n];
        int[] suffix = new int[n];
        int[] res = new int[n];
        prefix[0] = 1;
        suffix[n - 1] = 1;

        for (int i = 1; i < n; i++){
            prefix[i] = prefix[i - 1] * nums[i - 1];
        }

        for (int i = n - 2; i >= 0; i--){
            suffix[i] = suffix[i + 1] * nums[i + 1];
        }

        for (int i = 0; i < n; i++){
            res[i] = prefix[i] * suffix[i];
        }
        return res;
    }
}

//1.C O(1)
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        res[0] = 1;
        for (int i = 1; i < n; i++){
            res[i] = res[i - 1] * nums[i - 1];
        }
        int r = 1;
        for (int i = n - 1; i >= 0; i--){
            res[i] = res[i] * r;
            r *= nums[i];
        }
        return res;
    }
}

class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        res[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--){
            res[i] = res[i + 1] * nums[i + 1];
        }
        long prefix = 1;
        for (int i = 0; i < n; i++){
            res[i] *= prefix;
            prefix *= nums[i];
        }
        return res;
    }
}
```

### 05 Complexity

1.  time complexity: O(n)
2.  space complexity: O(n) / O(1)
