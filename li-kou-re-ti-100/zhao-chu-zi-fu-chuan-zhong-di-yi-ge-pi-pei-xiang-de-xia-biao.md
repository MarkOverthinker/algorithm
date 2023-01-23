# 找出字符串中第一个匹配项的下标

> 给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回  `-1` ****&#x20;

KMP算法。注意next数组的建立方法。

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.length(), n = needle.length();
        if (n == 0) {
            return 0;
        }
        vector<int> next(n);
        for (int i = 1, j = 0; i < n; i++) {
            // 优先找最大的。找所有可能匹配的串
            while (j > 0 && needle[i] != needle[j]) {
                j = next[j-1];
            }
            // 排除j == 0退出的while循环的情况
            if (needle[i] == needle[j]) {
                j++;
            }
            next[i] = j;
        }
        for (int i = 0, j = 0; i < m; i++) {
            while (j > 0 && haystack[i] != needle[j]) {
                j = next[j-1];
            }
            if (haystack[i] == needle[j]) {
                j++;
            }
            // i是匹配子串的末尾
            if (j == n) {
                return i - n + 1;
            }
        }
        return -1;
    }
};
```
