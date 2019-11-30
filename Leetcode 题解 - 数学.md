<!-- GFM-TOC -->
* [反转一个整数](#-反转一个整数)

<!-- GFM-TOC -->
# 反转一个整数
  直接反转会导致溢出，所以要做出溢出保护。要注意signed int的最大正数和最小负数的绝对值不相同
  
  ```cpp
      if(result > INT_MAX / 10 || (result == INT_MAX / 10 && tmp > 7)) {
          return 0;
      }
      if(result < INT_MIN / 10 || (result == INT_MIN / 10 && tmp < -8)) {
          return 0;
      }
  ```
