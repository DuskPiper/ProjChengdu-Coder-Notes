# Trie 字典树

在[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)中，**trie**，又称**前缀树**或**字典树**，是一种有序[树](https://zh.wikipedia.org/wiki/%E6%A0%91_(%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84))，用于保存[关联数组](https://zh.wikipedia.org/wiki/%E5%85%B3%E8%81%94%E6%95%B0%E7%BB%84)，其中的键通常是[字符串](https://zh.wikipedia.org/wiki/%E5%AD%97%E7%AC%A6%E4%B8%B2)。与[二叉查找树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91)不同，键不是直接保存在节点中，而是由节点在树中的位置决定。一个节点的所有子孙都有相同的[前缀](https://zh.wikipedia.org/wiki/%E5%89%8D%E7%BC%80)，也就是这个节点对应的字符串，而根节点对应[空字符串](https://zh.wikipedia.org/wiki/%E7%A9%BA%E5%AD%97%E7%AC%A6%E4%B8%B2)。一般情况下，不是所有的节点都有对应的值，只有叶子节点和部分内部节点所对应的键才有相关的值。

##特点与性能

trie树常用于搜索提示。如当输入一个网址，可以自动搜索出可能的选择。当没有完全匹配的搜索结果，可以返回前缀最相似的可能。

实现高效在字典中查找(serach)某个字符串的操作，实现高效构造(insert)字典操作，实现高效的查找某个字符串是否是前缀(prefix)的操作。

## 实现

```java
class Trie {
    private static final int SIZE = 26;
    public Node root;
    class Node {
        Node[] children;
        boolean isStr;
        public Node() {
            children = new Node[SIZE];
            isStr = false;
        }
    }
    
    public Trie() {
        root = new Node();
    }
    
    /**Insertion**/
    public void insert(String word) {
        if (word == null || word.length() == 0)
            return;
        Node ptr = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word.charAt(i) - 'a';
            if (ptr.children[index] == null) // No node found
                ptr.children[index] = new Node(); // Create new node to insert char
            ptr = ptr.children[index];
        }
        ptr.isStr = true;
    }
    
    /**Search**/
    public boolean search(String word) {
        Node ptr = root;
        for (int i = 0; i < word.length(); i++) {
            int index = word.charAt(i) - 'a';
            if (ptr.children[index] == null || 
               (i + 1 == word.length() && ptr.children[index].isStr == false)) // char sequence found but not a word
                return false;
            ptr = ptr.children[index];
        }
        return true;
    }
    
    /**Search if any words has given prefix**/
    public boolean prefixSearch(String prefix) {
        Node ptr = root;
        for (int i = 0; i < prefix.length(); i++) {
            int index = prefix.charAt(i) - 'a';
            if (ptr.children[index] == null)
                return false;
            ptr = ptr.children[index];
        }
        return true;
    }
}
```



## References

- [Wikipedia](https://zh.wikipedia.org/wiki/Trie)
- [学会Trie](http://www.yikanggao.com/blog/2017/09/%E5%AD%A6%E4%BC%9ATrie.html)
- [LeetCode 208 Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)