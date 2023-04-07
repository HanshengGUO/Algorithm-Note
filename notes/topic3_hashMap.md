# HashMap
## 哈希表基础知识
A HashMap is a data structure that is used to store and retrieve values based on keys. HashMap has a function called hashing function that can generate hashCode to store the value with a given key.

Some of the key characteristics of a hashmap include:
* **Fast access time**: HashMaps provide constant time access to elements, which means that retrieval and insertion of elements is very fast, usually O(1) time complexity.
* **Uses hashing function**: HashMaps use a hash function to map keys to indices in an array. This allows for quick lookup of values based on keys.
* **Stores key-value pairs**: Each element in a HashMap consists of a key-value pair. The key is used to look up the associated value.
* **Supports null keys and values**: HashMaps allow for null values and keys. This means that a null key can be used to store a value, and a null value can be associated with a key.
* **Not ordered**: HashMaps are not ordered, which means that the order in which elements are added to the map is not preserved. However, LinkedHashMap is a variation of HashMap that preserves the insertion order.
* **Allows duplicates**: HashMaps allow for duplicate values, but not duplicate keys. If a duplicate key is added, the previous value associated with the key is overwritten.
* **Thread-unsafe**: HashMaps are not thread-safe, which means that if multiple threads access the same hashmap simultaneously, it can lead to data inconsistencies. If thread safety is required, ConcurrentHashMap can be used.
* **Capacity and load factor**: HashMaps have a capacity, which is the number of elements that it can hold, and a load factor, which is the measure of how full the hashmap can be before it is resized.

## HashMap in Java
```java
import java.util.HashMap;
 
public class ExampleHashMap {
   public static void main(String[] args) {
       
      // Create a HashMap
      HashMap<String, Integer> hashMap = new HashMap<>();
       
      // Add elements to the HashMap
      hashMap.put("John", 25);
      hashMap.put("Jane", 30);
      hashMap.put("Jim", 35);
       
      // Access elements in the HashMap
      System.out.println(hashMap.get("John")); // Output: 25
       
      // Remove an element from the HashMap
      hashMap.remove("Jim");
       
      // Check if an element is present in the HashMap
      System.out.println(hashMap.containsKey("Jim")); // Output: false
       
      // Get the size of the HashMap
      System.out.println(hashMap.size()); // Output: 2
   }
}
```
#### [242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)
多种解法：
1. 字符串排序，把字符串转成Char数组进行排序对比
2. 用长度为26的数组作为hashmap
3. 用一个hashmap来统计
```java
class Solution {
    public boolean isAnagram1(String s, String t) {
        char[] sChars = s.toCharArray();
        char[] tChars = t.toCharArray();
        Arrays.sort(sChars);
        Arrays.sort(tChars);
        return Arrays.equals(sChars, tChars);
    }

    public boolean isAnagram2(String s, String t) {
        Map<Character, Integer> map = new HashMap<>();
        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }
        for (char ch : t.toCharArray()) {
            Integer count = map.get(ch);
            if (count == null) {
                return false;
            } else if (count > 1) {
                map.put(ch, count - 1);
            } else {
                map.remove(ch);
            }
        }
        return map.isEmpty();
    }

    public boolean isAnagram3(String s, String t) {
        int[] sCounts = new int[26];
        int[] tCounts = new int[26];
        for (char ch : s.toCharArray()) {
            sCounts[ch - 'a']++;
        }
        for (char ch : t.toCharArray()) {
            tCounts[ch - 'a']++;
        }
        for (int i = 0; i < 26; i++) {
            if (sCounts[i] != tCounts[i]) {
                return false;
            }
        }
        return true;
    }

    public boolean isAnagram4(String s, String t) {
        int[] counts = new int[26];
        t.chars().forEach(tc -> counts[tc - 'a']++);
        s.chars().forEach(cs -> counts[cs - 'a']--);
        return Arrays.stream(counts).allMatch(c -> c == 0);
    }

    public boolean isAnagram(String s, String t) {
        return Arrays.equals(s.chars().sorted().toArray(), t.chars().sorted().toArray());
    }
}
```
#### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)
如果使用哈希集合存储元素，则可以在O(1)的时间内判断一个元素是否在集合中，从而降低时间复杂度。
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> set1 = new HashSet<Integer>();
        HashSet<Integer> set2 = new HashSet<Integer>();
        HashSet<Integer> res = new HashSet<Integer>();

        for (int num1 : nums1){
            set1.add(num1);
        }
        for (int num2 : nums2){
            set2.add(num2);
        }
        for (int num1 : nums1){
            if (set2.contains(num1)){
                res.add(num1);
            }
        }
        int[] ans = new int[res.size()];
        int i = 0;
        for (int num : res)
            ans[i++] = num;
        return ans;

    }
}
```
[202. 快乐数](https://leetcode.cn/problems/happy-number/description/)
主要是题意的理解和Set的使用
```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet<Integer>();
        while (n != 1 && (!set.contains(n))){
            set.add(n);
            n = digitSquareSum(n);
        }
        if (n == 1)
            return true;
        else return false;
        
    }
    
    private int digitSquareSum(int num){
        int sum = 0;
        while (num > 0){
            sum += (num % 10) * (num % 10);
            num = num / 10;
        }
        return sum;
    }
}
```
[1. 两数之和](https://leetcode.cn/problems/two-sum/)
用HashMap降低运算量，和之前用Set逻辑类似
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();

        for (int i = 0; i<n; i++){
            if (map.containsKey(nums[i])){
                return new int[]{map.get(nums[i]), i};
            }else{
                map.put(target - nums[i], i);
            }
        }
        return new int[]{-1, -1};
    }
}
```
