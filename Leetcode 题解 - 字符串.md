<!-- GFM-TOC -->
* [1. 最长回文子串](#1-字符串循环移位包含)
* [2. 最长回文子序列](#2-最长回文子序列)
* [3. 正则表达式匹配](#3-正则表达式匹配)
<!-- GFM-TOC -->

# 1. 最长回文子串

中心扩散法，为了减小计算量，使用标识位，不要中间赋值。
马拉车算法，这是一种时间复杂度很小的算法O(n), 在更新pos和mx方面，我个人倾心于以下的写法。有些程序更新pos和mx时，程序段如下
```cpp
  if(i + len[i] > mx) {
      pos = i;
      mx = i + len[i];
  }
```
```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int length = s.size();
        if(length < 1) {
            return "";
        }
        string traArr = "$#";
        for(int i = 0; i < length; i++) {
            traArr += s[i];
            traArr += '#';
        }
        int pos = 0, mx = 0;
        int length_2 = traArr.size();
        int start = 0, maxLen = 0;
        vector<int> len(length_2,0);
        for(int i = 1; i < length_2; i++) {
            len[i] = i < mx ? min(len[2 * pos - i], mx - i) : 1;
            while(i + len[i] < length_2 && i - len[i] > 0 && traArr[i + len[i]] == traArr[i - len[i]]) {
                len[i]++;
            }
            if(i + len[i] - 1> mx ) {
                pos = i;
                mx = i + len[i] - 1;
            }
            if(len[i] - 1 > maxLen) {
                start = (i - len[i]) / 2;
                maxLen = len[i] - 1;
            }
        }
        return s.substr(start, maxLen); 
    }
};
```

# 2. 最长回文子序列
子序列与子串不同，子串是原字符串某一段连续的子字符串，子序列是原字符串某个排列顺序不变的子集。
这道题采用题解中一套C的做法，算法通过对一个数组(dp[i]记录以str[i]为左边界的最长子序列长度)的不断刷新，得到子序列长度数组。

```cpp
       for(i = 0; i < length; i++) {
            Max = 0;
            dp[i] = 1;
            for(j = i - 1; j >= 0; j--) {
                int tmp = dp[j];
                if(s[j] == s[i]) {
                    dp[j] = Max + 2;
                }
                Max = max(tmp,Max);
            }
        }
```

# 3. 正则表达式匹配
这道题与牛客网《剑指offer》上的52题十分的相似，我的解题思路也是按照牛客网上的来的：
    递归，分解：字符的后边是否跟有'*', 如有，则分为'*'匹配0个，1个或者是多个。如没有，直接匹配str和pattern的下一个字符。
```cpp
  class Solution {
  public:
      bool isMatch(string s, string p) {
          int lenS = s.size();
          int lenP = p.size();
          if(lenS == 0 && lenP == 0) {
              return true;
          }
          if(lenS != 0 && lenP == 0) {
              return false;
          }
          if(p[1] != '*') {
              if(s[0] == p[0] || (p[0] == '.' && lenS != 0)) {
                  return isMatch(s.substr(1, lenS - 1), p.substr(1, lenP - 1));
              }
              else {
                  return false;
              }
          }
          else {
              if(s[0] == p[0] || (p[0] == '.' && lenS != 0)) {
                  return isMatch(s, p.substr(2, lenP - 2)) || isMatch(s.substr(1,lenS - 1), p);
              }
              else {
                  return isMatch(s, p.substr(2,lenP - 2));
              }
          }
          return false;
      }
  };
```
但是该算法的时间复杂度和内存开销比较大，以下是对该算法的一个改进。改进对极大的降低了内存的开销。
其中：c_str()函数是获取字符串的首指针；auto：变量的自动类型推断。
```cpp
  class Solution {
  public:
      bool isMatch(string s, string p) {
          return isMatch(s.c_str(), p.c_str());
      }

      bool isMatch(const char* s, const char* p) {
          if(*p == 0) return *s == 0;

          auto first_match = *s && (*s == *p || *p == '.');

          if(*(p+1) == '*'){
              return isMatch(s, p+2) || (first_match && isMatch(++s, p));
          }
          else{
              return first_match && isMatch(++s, ++p);
          }
      }
  };
```
