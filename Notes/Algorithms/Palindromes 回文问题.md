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

   ![Demo](https://raw.githubusercontent.com/DuskPiper/ProjChengdu-Coder-Notes/master/Illustration/ManachersAlgorithmDemo1.png)

   观察上图中，理论上互相对称的绿色框区间，那么就有下面的结论：

   ```pseudocode
   一、对任意 i < mx 有：
   P[i] >= min(
   	p[2 * id - i], // 如P[j]回文未超过P[id]回文左边界，则i有一个对称回文不超过右边界（对称性）
       mx - i // 如果P[j]回文超过P[id]回文左边界，则P[i]所取对称回文只能截取mx以内的部分
   )
   // 值得一提的是，示意图里对应第二种情况，所以截取了P[j]回文的绿框部分
   
   二、对于 i >= mx 则：
   我们不了解任何有效信息，只能按部就班从P[i] = 1开始扫描计算，也就是：
   P[i] >= 1
   ```

   **所以算法的精髓在：**对给定`P[i]`，知道一个最小值，于是节省扫描其最大回文的时间（赢在起跑线）

### 实现

```java
String question = readQuestion();
List s = new ArrayList<Character>();
// Now rebuild
s.add("$");
s.add("#")
for (int i = 0; i < question.length; i ++) {
	s.add(question.charAt(i));
    s.add("#")
}
// Now find ans
List p = new ArrayList<Integer>(){1, 1}; // 第一个1是$对应的占位。第二个1是string第一项。
int id = 1;
int mx = 2;
int maxlen = 0;
for (int i = 2; i < s.size(); i ++) {
    p[i] = mx >= i ? min(p[2 * id - i], mx - i) : 1; // 得到P[i]的最小值
    while (s[i + p[i]] == s[i - p[i]]) p[i] ++; // 暴力向两边扩张
    // 此时已经得到了p[i]的正确值
    if (i + p[i] > mx) { // 检测并更新“已知的、右边界最大的回文子串”
        mx = i + p[i];
        id = i;
    }
    if (p[i] - 1 > maxlen) {
        maxlen = p[i] - 1;
    }
}
// 此时maxlen即是答案
```



## 参考资料

- [Manacher's ALGORITHM: O(n)时间求字符串的最长回文子串 ](https://www.felix021.com/blog/read.php?2040)