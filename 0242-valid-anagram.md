### 01 Desc + Example

Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

**Constraints:**

-   `1 <= s.length, t.length <= 5 * 104`
-   `s` and `t` consist of lowercase English letters.

### 02 Idea

-   Sorting
-   Hashmap

### 03 Approach

1.  Core: every characters frequency should be same
2.  Sorting
    1.  after sorting should be equal
3.  Frequency Count
    1.  use int array to record frequencies, and each should be same
    2.  2 counter
    3.  1 counter: one plus and one minus to all 0s
4.  Follow up: what if unicode
    1.  Hash table instead of fixed size counter
5.  Note: sort String
    1.  since String is immutable in Java, first convert to char array, use Arrays.sort()

### 04 Code

```
//1.Sorting
class Solution {
    public boolean isAnagram(String s, String t) {
        char[] s1 = s.toCharArray();
        char[] t1 = t.toCharArray();
        Arrays.sort(s1);
        Arrays.sort(t1);
        return Arrays.equals(s1, t1);
    }
}

//2.Count Frequency
//2.A 2 Counter
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] smap = new int[26];
        int[] tmap = new int[26];
        if (s.length() != t.length())
            return false;
        for (int i = 0; i < s.length(); i++){
            smap[s.charAt(i) - 'a']++;
            tmap[t.charAt(i) - 'a']++;
        }
        for (int i = 0; i < 26; i++){
            if (smap[i] != tmap[i])
                return false;
        }
        return true;
    }
}

//2.B 1 Counter
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] record = new int[26];
        for (int i = 0; i < s.length(); i++)
            record[s.charAt(i) - 'a']++;
        for (int j = 0; j < t.length(); j++)
            record[t.charAt(j) - 'a']--;
        for (int k: record){
            if (k != 0)
                return false;
        }
        return true;
    }
}

//2.C HashMap
class Solution {
    public boolean isAnagram(String s, String t) {
        HashMap<Character, Integer> smap = new HashMap<>();
        HashMap<Character, Integer> tmap = new HashMap<>();

        if (s.length() != t.length())
            return false;
        for (int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            smap.put(c, smap.getOrDefault(c, 0) + 1);
            char d = t.charAt(i);
            tmap.put(d, tmap.getOrDefault(d, 0) + 1);
        }

        for (Map.Entry<Character, Integer> entry: smap.entrySet()){
            Character key = entry.getKey();
            Integer val = entry.getValue();
            // System.out.println(key + " " + entry.getValue());
            if (!tmap.containsKey(key))
                return false;
            if (!val.equals(tmap.get(key)))
                return false;
        }

        return true;
    }
}
```

### 05 Complexity

1.  Time Complexity: O(nlogn) → O(2n) → O(n)
2.  Space Complexity: O(n)
