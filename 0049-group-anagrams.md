### 01 Desc + Example

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**Example 2:**

```
Input: strs = [""]
Output: [[""]]
```

**Example 3:**

```
Input: strs = ["a"]
Output: [["a"]]
```

**Constraints:**

-   `1 <= strs.length <= 104`
-   `0 <= strs[i].length <= 100`
-   `strs[i]` consists of lowercase English letters.

### 02 Idea

-   hashmap: frequency count
-   sorting

### 03 Approach
1.  Core

    1.  How to know two words is anagram

        1.  **Categorize By Sorting**: after sorting should be same String
        2.  **Categorize By Frequency Count**: Have same occurrences for same character
    2.  use **char[26]** to record frequencies → why char[] not int[]

        1.  use char is unique → ASCII table

            1.  ex: (char) 0 = null, (char) 1 = start of heading, (char) 48 = 0, (char) 65 = A, (char) 97 = a
            2.  range: **unsigned 16-digit/ 2 bytes, ['\u0000', '\uffff']**
        2.  use int may cause ambiguity: following string has same freq table

            ```
            //string: bdddddddddd, id:010100000000000000000000000
            //string: bbbbbbbbbbc, id:010100000000000000000000000
            ```
        3.  if use int[], append ‘#’

    3.  Map<String, List<String>>

        1.  Need to quickly decide if key is already in → put(key, value) O(1) hashing key in bucket to target index
    4.  Map Operations

        ```
        if (!map.containsKey(code)){
        		map.put(code, new ArrayList<>());
        }
        map.get(code).add(s);
        ==
        map.putIfAbsent(code, new ArrayList<>());
        map.get(code).add(str);
        ==
        map.computeIfAbsent(key, v -> new ArrayList<>()).add(value); //return list

        new ArrayList<>(map.values());
        ```

2.  Sorting

    1.  Core
        1.  since all characters frequencies same, then after sort, it will becomes to same
        2.  Sort a single String
            1.  cannot sort directly
            2.  convert to char array: char[] arr = str.toCharArray()
            3.  sort the char array: Arrays.sort(arr)
            4.  get the String: String str = new String(arr)
3.  Counting frequency + encode

### 04 Code

```
//1. Sorting
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<>();
        for (String str: strs){
            char[] arr = str.toCharArray();
            Arrays.sort(arr);
            String code = new String(arr);
            if (!map.containsKey(code)){
                map.put(code, new ArrayList<>());
            }
            map.get(code).add(str);
        }
        List<List<String>> res = new LinkedList<>();
        for (List<String> list : map.values()) {
            res.add(list);
        }
        return res;
    }
}

//2. counting + encode
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
		    //<code, anagrams in a group>
        HashMap<String, List<String>> map = new HashMap<>();
        for (String s : strs) {
            String code = encode(s);
            //if (!map.containsKey(code)){
            //    map.put(code, new ArrayList<>());
            //}
            map.putIfAbsent(code, new ArrayList<>());
            map.get(code).add(s);
        }

        List<List<String>> res = new LinkedList<>();
        for (List<String> list : map.values()) {
            res.add(list);
        }
        return res;
    }
    
    //char[]
    String encode(String s) {
        char[] count = new char[26];
        for (char c : s.toCharArray()) {
            int delta = c - 'a';
            count[delta]++;
        }
        return new String(count);
    }
    
    //int[] + '#'
    private String encode(String str){
        int[] count = new int[26];
        StringBuilder sb = new StringBuilder();
        for (char c: str.toCharArray()){
            count[c - 'a']++;
        }
        for (int i = 0; i < 26; i++){
            sb.append(count[i]);
            sb.append('#');
        }
        return sb.toString();
    }
}

//Wrong when self implement
//string: bdddddddddd, id:010100000000000000000000000
//string: bbbbbbbbbbc, id:010100000000000000000000000
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        //<key: id, val: index>
        Map<String, Integer> map = new HashMap<>();
        List<List<String>> res = new ArrayList<>();
        int num = 0;

        for (String str: strs){
            String id = getFreq(str);
            System.out.println("string: " + str + ", id:" + id);

            if (!map.containsKey(id)){
                map.put(id, num);
                res.add(new ArrayList<>());
                res.get(map.get(id)).add(str);
                num++;
            }else{
              res.get(map.get(id)).add(str);
            }
        }

        return res;
    }

    public String getFreq(String str){
        int[] freq = new int[26];
        for (char c: str.toCharArray()){
            freq[c - 'a']++;
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 26; i++){
            sb.append(freq[i]);
        }
        return sb.toString();
    }
}
```

### 05 Complexity

-   Sorting
    1.  time complexity: O(NKlogK)
    2.  space complexity: O(NK)
-   Count
    1.  time complexity: O(nk)
    2.  space complexity: O(n)
