# Trie 字典树
(基础知识)[https://www.geeksforgeeks.org/introduction-to-trie-data-structure-and-algorithm-tutorials/]
字典树是一种字符串的HashMap
* Java实现
(208. 实现 Trie (前缀树))[https://leetcode.cn/problems/implement-trie-prefix-tree/]
```Java
class Trie {
    public Trie[] children;
    int wordCount;

    public Trie() {
        children = new Trie[26];
        wordCount = 0;
    }
    
    public void insert(String word) {
        int n = word.length();
        Trie cur = this;
        for (int i = 0; i<n; i++){
            int index = word.charAt(i) - 'a';
            if (cur.children[index] == null){
                cur.children[index] = new Trie();
            }
            cur = cur.children[index];
        }
        cur.wordCount++;
    }
    
    public boolean search(String word) {
        int n = word.length();
        Trie cur = this;
        for (int i = 0; i<n; i++){
            int index = word.charAt(i) - 'a';
            if (cur.children[index] == null){
                return false;
            }
            cur = cur.children[index];
        }
        return cur.wordCount > 0;
    }
    
    public boolean startsWith(String prefix) {
        int n = prefix.length();
        Trie cur = this;
        for (int i = 0; i<n; i++){
            int index = prefix.charAt(i) - 'a';
            if (cur.children[index] == null){
                return false;
            }
            cur = cur.children[index];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```
[1698. 字符串的不同子字符串个数](https://leetcode.cn/problems/number-of-distinct-substrings-in-a-string/)
```java
class Solution {
    public class TrieNode{
        public TrieNode[] next;
        public TrieNode(){
            next = new TrieNode[26];
        }
    }
    public int countDistinct(String s) {
        int res = 0;
        int n = s.length();
        TrieNode root = new TrieNode();
        for (int i = 0; i<n; i++){
            TrieNode cur = root;
            for (int j = i; j<n; j++){
                int index = s.charAt(j) - 'a';
                if (cur.next[index] == null){
                    cur.next[index] = new TrieNode();
                    res += 1;
                }
                cur = cur.next[index];
            }
        }
        return res;
    }
}

```
[1804. 实现 Trie （前缀树） II](https://leetcode.cn/problems/implement-trie-ii-prefix-tree/)
```java

```