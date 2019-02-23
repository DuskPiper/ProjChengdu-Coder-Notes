# Palindromes 回文问题

- 回文串：一个正读和反读都一样的字符串，比如“level”或者“noon”等等就是回文串。
- 回文子串，顾名思义，即字符串中满足回文性质的子串。如"google"有回文子串为"goog"。

## Manacher's Algorithm 曼彻斯特算法

### 简述

用O(n)时间复杂度得到给定字符串中最长回文子串的长度。

### 算法

1. 为解决奇偶算法不同的问题，对给定string进行增补，使用string中不会出现的特殊符号。string开头、任意字符之间和结尾都进行增补。这样的结果是新string一定是奇数长度。最后在新string开头增补一位特殊符号，方便地避免之后遍历时index溢出。最终的string一定是偶数长度。

   ```pseudocode
   "google" -> "#g#o#o#g#l#e#" -> "$#g#o#o#g#l#e#"
   "apple" -> "#a#p#p#l#e#" -> "$#a#p#p#l#e#"
   ```

2. 定义P[i]：新string长度-1长的array，每一项数字对应`string[1:]`一项。数字的大小是string中对应项为中心向两边张开的最大回文长度。

   ```pseudocode
   S: $  #  1  #  2  #  2  #  1  #  2  #  3  #  2  #  1  #
   P[]:  1  2  1  2  5  2  1  4  1  2  1  6  1  2  1  2  1
   // P长度是S长度-1，因为S第一位只是增补用以防止溢出的，我们不需要考虑
   ```

   可见两点：

   - 要求出整个问题的答案，就应该得到P由此得到P的最大值`Pmax`。
   - `Pmax`和答案存在关系：`ans = Pmax - 1`。

3. `string[1:]`从左到右开始获取`P[i]`，显然第一项为1，不过对于非第一项都有一面的捷径：

4. **算法核心**。对于`P[i]`我们定义以下变量

   - `id`：已知的、右边界最大的回文子串的中心index。
   - `mx`：已知的、右边界最大的回文子串的右边界。
   - `my`：已知的、右边界最大的回文子串的左边界。
   - `j`：`i`关于`id`的对称点。

   ![Demo](https://www.felix021.com/blog/attachment.php?fid=448)

## 参考资料

- [Manacher's ALGORITHM: O(n)时间求字符串的最长回文子串 ](https://www.felix021.com/blog/read.php?2040)