### 01 Description

Given a string `s` of zeros and ones, *return the maximum score after splitting the string into two **non-empty** substrings* (i.e. **left** substring and **right** substring).

The score after splitting a string is the number of **zeros** in the **left** substring plus the number of **ones** in the **right** substring.

**Example 1:**

```
Input: s = "011101"
Output: 5
Explanation:
All possible ways of splitting s into two non-empty substrings are:
left = "0" and right = "11101", score = 1 + 4 = 5
left = "01" and right = "1101", score = 1 + 3 = 4
left = "011" and right = "101", score = 1 + 2 = 3
left = "0111" and right = "01", score = 1 + 1 = 2
left = "01110" and right = "1", score = 2 + 1 = 3
```

**Example 2:**

```
Input: s = "00111"
Output: 5
Explanation: When left = "00" and right = "111", we get the maximum score = 2 + 3 = 5
```

**Example 3:**

```
Input: s = "1111"
Output: 3
```

**Constraints:**

-   `2 <= s.length <= 500`
-   The string `s` consists of characters `'0'` and `'1'` only.

### 02 Idea

-   prefix sum

### 03 Approach

1.  Intuition
2.  Brute Force
    1.  For every possible split, count front zeros and end ones separately in nested loop
3.  Prefix Sum
    1.  O(n)
        1.  prefix sum template
    2.  O(1)
        1.  Core: for every split move(i → i + 1), we just change single character, don't need to traverse all prefix/ left-side and suffix/ right-side, can change zeros and ones depends on that character
        2.  Algo
            1.  ones: # of 1s in s[i..n) right part
                1.  initialize: i = 0 at first = # of 1s in total
            2.  zeros: # of 0s in s[0..i] left part
            3.  for every index i: previously in left part, then join right part
                1.  s[i] = '1': ones---
                2.  s[i] = '0': zeros++
4.  One Pass
    1.  Intuition
        1.  score = zeroLeft + oneRight
        2.  oneRight = oneTotal - oneLeft
        3.  score = zeroLeft + oneTotal(fixed) - oneLeft
        4.  find max{zeroLeft - oneLeft}, all dependent on left part substring
        5.  res = max{zeroLeft - oneLeft} + oneTotal
    2.  Algo
        1.  zeros: # of zeros on s[0..i] left part
        2.  ones: # of ones on s[0..i] left part
        3.  record max = {zeros - ones}
        4.  at last, ones: # of ones on s[0..n) left part = oneTotal
            1.  check if last digit '1'
        5.  res = max + ones

### 04 Code

```
//1.Brute Force

//2.Prefix Sum
//2.A O(n)
class Solution {
    public int maxScore(String s) {
        if (s == null || s.length() == 0)
            return 0;
        int n = s.length();
        //frontZeros[i]: # of zeros in s[0..i]
        int[] frontZeros = new int[n];
        //endOnes[i]: # of ones in s(i..n)
        int[] endOnes = new int[n];
        int zeros = 0, ones = 0;
        for (int i = 0; i < n; i++){
            zeros += s.charAt(i) == '0' ? 1 : 0;
            frontZeros[i] = zeros;
        }

        for (int i = n - 1; i >= 0; i--){
            ones += s.charAt(i) == '1' ? 1 : 0;
            endOnes[i] = ones;
        }

        int res = 0;
        for (int i = 0; i < n - 1; i++){
            res = Math.max(res, frontZeros[i] + endOnes[i + 1]);
        }
        return res;
    }
}

//2.B O(1)
class Solution {
    public int maxScore(String s) {
        if (s == null || s.length() == 0)
            return 0;
        int n = s.length();
        int ones = 0;
        for (int i = n - 1; i >= 0; i--)
            ones += s.charAt(i) == '1' ? 1 : 0;

        int zeros = 0;
        int res = 0;
        for (int i = 0; i < n - 1; i++){
            zeros += s.charAt(i) == '0' ? 1 : 0;
            ones -= s.charAt(i) == '1' ? 1 : 0;
            res = Math.max(res, zeros + ones);
        }

        return res;
    }
}

//3.One Pass
class Solution {
    public int maxScore(String s) {
        //# of zeros on s[0..i] left part
        int zeros = 0;
        //# of ones on s[0..i] left part
        int ones = 0;
        int max = Integer.MIN_VALUE;
        int n = s.length();
        for (int i = 0; i < n - 1; i++){
            if (s.charAt(i) == '0')
                zeros++;
            else
                ones++;
            max = Math.max(max, zeros - ones);
        }

        ones += s.charAt(n - 1) == '1' ? 1 : 0;
        return max + ones;
    }
}
```

### 05 Complexity

1.  Time Complexity: O(n^2) → O(2n) → O(n)
2.  Space Complexity: O(n) → O(1)
