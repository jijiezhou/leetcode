### 01 Desc & Example

You are given a **0-indexed** array of strings `words` and a 2D array of integers `queries`.

Each query `queries[i] = [li, ri]` asks us to find the number of strings present in the range `li` to `ri` (both **inclusive**) of `words` that start and end with a vowel.

Return *an array* `ans` *of size* `queries.length`*, where* `ans[i]` *is the answer to the* `i`th *query*.

**Note** that the vowel letters are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**

```
Input: words = ["aba","bcb","ece","aa","e"], queries = [[0,2],[1,4],[1,1]]
Output: [2,3,0]
Explanation: The strings starting and ending with a vowel are "aba", "ece", "aa" and "e".
The answer to the query [0,2] is 2 (strings "aba" and "ece").
to query [1,4] is 3 (strings "ece", "aa", "e").
to query [1,1] is 0.
We return [2,3,0].
```

**Example 2:**

```
Input: words = ["a","e","i"], queries = [[0,2],[0,1],[2,2]]
Output: [3,2,1]
Explanation: Every string satisfies the conditions, so we return [3,2,1].
```

**Constraints:**

-   `1 <= words.length <= 105`
-   `1 <= words[i].length <= 40`
-   `words[i]` consists only of lowercase English letters.
-   `sum(words[i].length) <= 3 * 105`
-   `1 <= queries.length <= 105`
-   `0 <= li <= ri < words.length`

### 02 Idea

-   prefix sum

### 03 Approach

1.  Prefix Sum
    1.  Core: record # of vowel strings in range [i..j]
    2.  Template
        1.  Relation
            1.  words[i] isVowel: preSum[i] = preSum[i - 1] + 1
            2.  words[i] !isVowel: preSum[i] = preSum[i - 1]
        2.  sum[i..j] = preSum[j + 1] - preSum[i]
            1.  preSum[i] = sum[0..i)
            2.  preSum[j + 1] = sum[0..j + 1)

### 04 Code

```
//prefix sum
class Solution {
    public int[] vowelStrings(String[] words, int[][] queries) {
        //prefix sum
        int qLen = queries.length;
        int[] res = new int[qLen];
        int wLen = words.length;

        int[] preSum = new int[wLen + 1];
        for (int i = 1; i <= wLen; i++){
            String cur = words[i - 1];
            preSum[i] = preSum[i - 1] + (isVowel(cur.charAt(0)) && isVowel(cur.charAt(cur.length() - 1)) ? 1 : 0);
        }

        for (int i = 0; i < qLen; i++){
            int[] query = queries[i];
            res[i] = preSum[query[1] + 1] - preSum[query[0]];
        }

        return res;
    }

    private boolean isVowel(char c){
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
}
```

### 05 Complexity

1.  time complexity: O(n)
2.  space complexity: O(n)
