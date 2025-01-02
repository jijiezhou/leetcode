### 01 Desc + Example

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: true
```

**Example 2:**

```
Input: nums = [1,2,3,4]
Output: false
```

**Example 3:**

```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

**Constraints:**

-   `1 <= nums.length <= 105`
-   `109 <= nums[i] <= 109`

### 02 Idea

-   hashtable
-   sorting
-   stream

### 03 Approach

1.  Hashtable
    1.  Hashmap: check frequency
    2.  Hashset
2.  Sorting
    1.  step 1: sorting
    2.  step 2: check if neighbor equals
3.  stream
    1.  Arrays.stream(int[]): convert int array into IntStream
    2.  use distinct(), count() method
    3.  check if the number of distinct less than total length

### 04 Code

```
//1.Hash Table
//1.A HashMap
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int num: nums){
            if (map.containsKey(num))
                return true;
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        return false;
    }
}

//1.B HashSet
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int num: nums){
            if (set.contains(num))
                return true;
            set.add(num);
        }
        return false;
    }
}

//2.Sorting
class Solution {
    public boolean containsDuplicate(int[] nums) {
        int n = nums.length;
        if (n == 1)
            return false;
        Arrays.sort(nums);

        for (int i = 1; i < nums.length; i++){
            if (nums[i] == nums[i - 1])
                return true;
        }
        return false;
    }
}

//3.Stream
class Solution {
    public boolean containsDuplicate(int[] nums) {
        return Arrays.stream(nums).distinct().count() < nums.length;
    }
}
```

### 05 Complexity

1.  time complexity: O(n)/ O(nlogn)
2.  space complexity: O(n)/ O(1)
