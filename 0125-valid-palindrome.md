### 01 Desc + Example

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a **palindrome**, or* `false` *otherwise*.

**Example 1:**

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2:**

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3:**

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

**Constraints:**

-   `1 <= s.length <= 2 * 105`
-   `s` consists only of printable ASCII characters.

### 02 Idea

-   two pointers
-   string manipulation

### 03 Approach

1.  Process String

    1.  s to lower case: s = s.toLowerCase()
    2.  delete heading and tailing blank: s = s.trim()
    3.  **Alphanumeric: letters + numbers**
        1.  delete all special characters
2.  Check if Palindrome

    1.  two pointers

        1.  each time left and right should be same
    2.  also can compare the first half and second half

        1.  `s.charAt(i) != s.charAt(s.length() - i - 1)`
    3.  compare if reverse string equals to original string

        ```
        StringBuilder sb = new StringBuilder(s);
        return s.equals(sb.reverse().toString());
        ```

3.  Easy for mistakes

    1.  `s = s.toLowerCase()` rather then simply s.toLowerCase()
4.  Notice

    1.  `Character.toLowerCase(c)`
    2.  `Character.isLetterOrDigits() <-> (c >= 'a' && c <= 'z') || (c >= '0' && c <= '9'))`
    3.  regex
        1.  `s = s.replaceAll("[^a-z0-9A-Z]", "").toLowerCase();`

### 04 Code

```
//1st self implement
class Solution {
    public boolean isPalindrome(String s) {
        s = s.toLowerCase();
        StringBuilder sb = new StringBuilder();
        for (char c: s.toCharArray()){
            if ((c >= 'a' && c <= 'z') || (c >= '0' && c <= '9'))
                sb.append(c);
        }
        return isP(sb.toString());
    }

    public boolean isP(String s){
        int lo = 0;
        int hi = s.length() - 1;
        while (lo < hi){
            if (s.charAt(lo) != s.charAt(hi))
                return false;
            lo++;
            hi--;
        }
        return true;
    }
}

//2nd self implement
class Solution {
    public boolean isPalindrome(String s) {
        s = filter(s);
        return isPalin(s);
    }

    public String filter(String s){
        s = s.toLowerCase();
        s = s.trim();
        int n = s.length();
        StringBuilder sb = new StringBuilder();
        for (char c: s.toCharArray()){
            if (isDigit(c) || isCharacter(c))
                sb.append(c);
        }

        return sb.toString();
    }

    public boolean isDigit(char c){
        return c >= '0' && c <= '9';
    }

    public boolean isCharacter(char c){
        return c >= 'a' && c <= 'z';
    }

    public boolean isPalin(String s){
        int l = 0;
        int r = s.length() - 1;
        while (l < r){
            if (s.charAt(l) != s.charAt(r))
                return false;
            l++;
            r--;
        }
        return true;
    }
}

//regex
class Solution {
    public boolean isPalindrome(String s) {
        s = s.replaceAll("[^a-z0-9A-Z]", "").toLowerCase();
        for (int i = 0; i < s.length() / 2; i++){
            if (s.charAt(i) != s.charAt(s.length() - i - 1))
                return false;
        }
        return true;
    }
}
```

### 05 Complexity

1.  time complexity: O(n)
2.  space complexity: O(1)
