### 01 Desc & Example

You are given a string `s` of lowercase English letters and a 2D integer array `shifts` where `shifts[i] = [starti, endi, directioni]`. For every `i`, **shift** the characters in `s` from the index `starti` to the index `endi` (**inclusive**) forward if `directioni = 1`, or shift the characters backward if `directioni = 0`.

Shifting a character **forward** means replacing it with the **next** letter in the alphabet (wrapping around so that `'z'` becomes `'a'`). Similarly, shifting a character **backward** means replacing it with the **previous** letter in the alphabet (wrapping around so that `'a'` becomes `'z'`).

Return *the final string after all such shifts to* `s` *are applied*.

**Example 1:**

```
Input: s = "abc", shifts = [[0,1,0],[1,2,1],[0,2,1]]
Output: "ace"
Explanation: Firstly, shift the characters from index 0 to index 1 backward. Now s = "zac".
Secondly, shift the characters from index 1 to index 2 forward. Now s = "zbd".
Finally, shift the characters from index 0 to index 2 forward. Now s = "ace".
```

**Example 2:**

```
Input: s = "dztz", shifts = [[0,0,0],[1,1,1]]
Output: "catz"
Explanation: Firstly, shift the characters from index 0 to index 0 backward. Now s = "cztz".
Finally, shift the characters from index 1 to index 1 forward. Now s = "catz".
```

**Constraints:**

-   `1 <= s.length, shifts.length <= 5 * 104`
-   `shifts[i].length == 3`
-   `0 <= starti <= endi < s.length`
-   `0 <= directioni <= 1`
-   `s` consists of lowercase English letters.

### 02 Idea

-   difference array

### 03 Approach

1.  Difference array
    1.  Core
        1.  frequently edit range → difference array
        2.  (char)('a' + s[i] - 'a' + shiftStep) % 26
    2.  Note
        1.  don't need to pre-calculate res, use shiftSteps sum up diff
        2.  make sure move step: [0, 25]
            1.  mod(%) 26
            2.  negative → +26
2.  Fenwick Tree/ BIT(Binary Indexed Tree): to do
3.  Segment Tree: to do

### 04 Code

```
//1.Difference Array
class Solution {
    public String shiftingLetters(String s, int[][] shifts) {
        int n = s.length();
        int[] diff = new int[n];
        for (int[] shift: shifts){
            int start = shift[0];
            int end = shift[1];
            if (shift[2] == 1){
                diff[start] += 1;
                if (end + 1 < n)
                    diff[end + 1] -= 1;
            }else{
                diff[start] -= 1;
                if (end + 1 < n)
                    diff[end + 1] += 1;
            }
        }

        // int[] res = new int[n];
        // res[0] = diff[0];
        // for (int i = 1; i < n; i++){
        //     res[i] = res[i - 1] + diff[i];
        // }
        int shiftStep = 0;

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++){
            shiftStep = (shiftStep + diff[i]) % 26;
            //int shiftStep = res[i] % 26;
            if (shiftStep < 0)
                shiftStep += 26;

            sb.append((char)('a' + (s.charAt(i) - 'a' + shiftStep) % 26));
        }

        return sb.toString();
    }
}

//2.Binary Indexed Tree
//to do

//3.Segment Tree
//to do
```

### 05 Complexity

1.  time complexity: O(n)
2.  space complexity: O(n)
