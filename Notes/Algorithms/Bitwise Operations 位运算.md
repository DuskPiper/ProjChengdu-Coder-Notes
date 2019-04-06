#Bitwise Operations 位操作

- 1个字是2个字节 `1 word = 2 byte`

- 1个字节是8个比特 `1 byte = 8 bit`



## 原码 反码 补码

- 原码：Binary Number

- 反码：One's Complement

- 补码：Two's Complement

### 正数

正数的源码 = 反码 = 补码。

### 负数

负数的反码是源码除符号位以外**取反**，补码是反码**+1**.

```pseudocode
// -3
1000 0000 0000 0011 // 源码
1111 1111 1111 1100 // 反码
1111 1111 1111 1101 // 补码
```



## 基本位操作

- Java的位运算符有：左移、右移、无符号右移、位与、位或、位非、位异或。

- 位非是一元操作符，其它的都是二元操作符。

### 移位运算

- 左移 `<<`

  左移并低位补0。

- 右移 `>>`

  右移并补高位为符号位(正数左补0、负数左补1)。

- 无符号右移 `>>>`

  右移并高位补0，包括符号位。

### 逻辑位运算

- 位与 `&`

- 位或 `|`

- 位非 `~`

  对每一位都取反，包括符号位。

- 位异或 `^`



## 位操作技巧

###移位的应用

-  `n << 1` 相当于 `n *= 2` , `n >> 1` 相当于 `n /* 2` 。

- 同理，`n << m` 相当于 $n * 2^m​$ 。

- 获取integer极限值

  ```java
  (1 << 31) - 1 // Integer.MAX_VALUE
  ~ (1 << 31) // Integer.MAX_VALUE
  1 << 31 // Integer.MIN_VALUE
  ```

### 位与的应用

- 判断奇偶性

  `a & 1 = 0` 偶数
  `a & 1 = 1` 奇数

  ```java
  boolean isOdd(int n) {
  	return (n & 1) == 1;
  }
  ```

- 清零特定位

  `a & 255` 清空了a的高八位。

### 位异或的应用

- **交换变量**

  ```java
  void swap(int a, int b) {
    a ^= b;
    b ^= a;
    a ^= b;
  }
  ```

- 判断符号是否同（对 0 不适用）

  ```java
  boolean sameSign(int a, int b) {
    return (a ^ y) >= 0;
  }
  ```



## 进阶

### 格雷码 Gray Code

格雷码（循环二进制单位距离码）是任意两个相邻数的代码只有一位二进制数不同的编码，它与奇偶校验码同属可靠性编码。格雷码能避免讯号传送错误。

#### 格雷码与二进制数

- 二进制数转格雷码

  `G(n) = B(n+1) XOR B(n) `  即 `G(n) = B(n+1) + B(n)`

  ```c++
  unsigned int binaryToGray(unsigned int x) {
    return x ^ (x >> 1);
  }
  ```

- 格雷码转二进制数

  `B(n) = B(n+1) - G(n) `

  ```c++
  unsigned int grayToDecimal(unsigned int x) {
    unsigned int mask;
    for (mask = x >> 1; mask != 0; mask = mask >> 1) {
      x = x ^ mask;
    }
    return x;
  }
  ```

#### LeetCode

- [LeetCode 89. Gray Code](https://leetcode.com/problems/gray-code/)





## Reference

- [Bitwise Operation - WikiPedia](https://en.wikipedia.org/wiki/Bitwise_operation)
- [Gray Code - WikiPedia](https://en.wikipedia.org/wiki/Gray_code)
- [可能是最通俗易懂的 Java 位操作运算讲解](https://blog.csdn.net/briblue/article/details/70296326)
- [Java中带符号右移和无符号右移的区别](https://blog.csdn.net/zerolaw/article/details/81081823)
- [优秀程序员不得不知道的20个位运算技巧](https://blog.csdn.net/zmazon/article/details/8262185)
- [格雷码 - WikiPedia](https://zh.wikipedia.org/wiki/%E6%A0%BC%E9%9B%B7%E7%A0%81)
- [Convertion of grey code and binary 格雷码和二进制数之间的转换](http://www.cnblogs.com/grandyang/p/4315607.html) 