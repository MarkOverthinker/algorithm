# 无重复字符的最长子串

> 给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

思路，很容易想到，但是要注意细节，有坑

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> map;
        int l = 0;
        int max = 0;
        for (int r = 0; r < s.length(); r++) {
            if (map.count(s[r]) == 0) {
                map[s[r]] = r;
            } else {
                // 注意 map[s[r]]可能在窗口外，不能把l往左移了
                // 即随着l的移动map里的元素可能失效
                l = ::max(map[s[r]]+1, l);
                // 不要忘记更新map
                map[s[r]] = r;
            }
            if (r - l + 1 > max) max = r - l + 1;
        }
        return max;
    }
};
```
