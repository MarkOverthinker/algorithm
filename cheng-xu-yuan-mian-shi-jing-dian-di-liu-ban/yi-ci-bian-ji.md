# 一次编辑

> 字符串有三种编辑操作:插入一个英文字符、删除一个英文字符或者替换一个英文字符。 给定两个字符串，编写一个函数判定它们是否只需要一次(或者零次)编辑就变为相等字符串。

* 先排除字符创长度差大于1的，肯定无法匹配。下面的判断都是针对字符串长度<= 1的，并对两者长度差为-1, 0, 1的三种情况分别判断。-1和1同一为插入和删除的判断函数，0则使用替换字符的判断方式即可。
* 双指针，一个指长字符串，一个指短字符串。只是在三种操作判断下情况可能略有不同。对插入和删除，若遇到不同的，则只移动一次长字符串的指针即可，若相同则两个都移动。若两个指针距离大于2说明1次操作无法使两个字符串相同；而对于相等操作，遇到不同时则两个都移动即可，若这样的次数大于1说明无法匹配。
* 插入和删除操作本质上其实是一样的，就是多/少一个字符的区别，所以可以统一
* 插入和删除的0次操作就是相等，可以统一到替换0次字符的情况里处理

```cpp
class Solution {
public:
    bool insertj(string longer, string shorter) {
        int length1 = longer.size(), length2 = shorter.size();
        int idx1 = 0, idx2 = 0;
        while (idx1 < length1 && idx2 < length2) {
            if (longer[idx1] == shorter[idx2]) {
                idx2++;
            }
            idx1++;
            if (idx1 - idx2 > 1) {
                return false;
            }
        }
        return true;
    }

    bool oneEditAway(string first, string second) {
        int m = first.size(), n = second.size();
        if (m - n == 1) {
            return insertj(first, second);
        } else if (n - m == 1) {
            return insertj(second, first);
        } else if (n == m) {
            int cnt = 0;
            for (int i = 0; i <m; i++) {
                if (first[i] == second[i]) {
                    continue;
                }
                if (++cnt > 1) {
                    return false;
                }
            }
            return true;
        }
        return false;
    }
};
```
