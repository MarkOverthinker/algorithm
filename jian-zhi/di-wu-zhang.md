# 第五章

> 题39.找数组中出现次数超过一半的数字。

> ```cpp
> 投票算法
> class Solution {
> public:
>     int majorityElement(vector<int>& nums) {
>         int candidate;
>         int n = nums.size();
>         int cnt = 0;
>         for (int i = 0; i < n; i++) {
>             if (cnt == 0) {
>                 candidate = nums[i];
>                 cnt++;
>             } else {
>                 if (candidate == nums[i]) {
>                     cnt++;
>                 } else {
>                     cnt--;
>                 }
>             }
>         }
>         return candidate;
>     }
> };
> ```
>
>

```
随机选一个然后验证
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        while (true) {
            int candidate = nums[rand() % nums.size()];
            int count = 0;
            for (int num : nums)
                if (num == candidate)
                    ++count;
            if (count > nums.size() / 2)
                return candidate;
        }
        return -1;
    }
};

作者：力扣官方题解
链接：https://leetcode.cn/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/solutions/832356/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-pvh8/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

> 题40. 找出数组中最小的k个数

找最小要用最大堆，找最大要用最小堆。

另一种方法是用快速选择（类似于快排），选择一个数，将小于他的放在左边大于它的放在右边，该数最后下标若为k，则找完了。否则递归进行。时间复杂度期望为O(n)，最坏为O(n^2)。此方法只适用于较少的数据。

````
用堆复杂度为nlog(k)。n为遍历一遍数组的消耗，log(k)为维护大小为k的复杂度（插入删除都为log(k)）。
```cpp
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int> ans;
        ans.resize(k);
        if (k == 0) {
            return {};
        }
        priority_queue<int> q;
        for (int i = 0; i < k; ++i) {
            q.push(arr[i]);
        }
        int sz = arr.size();
        for (int i = k; i < sz; ++i) {
            if (arr[i] < q.top()) {
                q.pop();
                q.push(arr[i]);
            }
        }
        for (int i = 0; i < k; ++i) {
            ans[i] = q.top();
            q.pop();
        }
        return ans;
    }
};
```
````

> 题42.连续子数组的最大和
>
> 输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
>
> 要求时间复杂度为O(n)。

基于动态规划，设dp\[i]为以i元素结尾的子数组的最大和，则dp\[i] = max(dp\[i-1] + nums\[i], nums\[i]),可以基于dp\[i-1]是否为正判断。

每次计算只需要上一次的结果，所以只用了一个curr记录。&#x20;

````
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max = INT_MIN, curr = 0;
        for (int n : nums) {
            // 前面最大和大于0则加上前面
            curr = curr > 0 ? n+curr : n;
            if (curr > max) max = curr;
        }
        return max;
    }
};
```
````

> 剑指 Offer 44. 数字序列中某一位的数字
>
> 数字以123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。
>
> 请写一个函数，求任意第n位对应的数字。

力扣题有修改，一位数去掉了0，做起来更简单。否则需要考虑上一位数中包含0的情况，则一位数为10个。

````
```cpp
class Solution {
public:
    int findNthDigit(int n) {
        long long bits = 1;
        long long nums = 9;
        // 每位数有 9 * 10 ^ i个。
        while (n > bits*nums) {
            n -= bits*nums;
            nums *= 10;
            bits++;
        }
        // 找出要求的位在bits位数中的第几个数
        int p = (n-1) / bits;
        int ans = pow(10, bits-1) + p;
        // bits-b-1表示要求ans的第几位。
        int b = (n-1) % bits;
        for (b = bits-b-1; b > 0; b--) {
            ans /= 10;
        }
        return ans % 10;
    }
};
```
````

> 面试题45. 把数组排成最小的数
>
> 输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

优先选“小”的排在高位即可。

大小的判断，将比较的两个数一前一后组合看那种组合出来的数更小。

````
```cpp
class Solution {
public:
    string minNumber(vector<int>& nums) {
        vector<string> numss;
        for (auto s : nums) {
            numss.push_back(to_string(s));
        }
        quicksort(numss, 0, nums.size()-1);
        string ret;
        for (auto s : numss) {
            ret += s;
        }
        return ret;
    }

private:
    void quicksort(vector<string> &nums, int l, int r) {
        if (l > r) return;
        int i = l, j = r;
        while (i < j) {
            while (nums[j] + nums[l] >= nums[l] + nums[j] && i < j) j--;
            while (nums[i] + nums[l] <= nums[l] + nums[i] && i < j) i++;
            swap(nums[i], nums[j]);
        }
        swap(nums[i], nums[l]);
        // 递归不加入i
        quicksort(nums, l, i-1);
        quicksort(nums, i+1, r);
    }
};
```
````

> 剑指 Offer 46. 把数字翻译成字符串
>
> 给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

动态规划。可以以f\[i]为结尾为i的翻译方法数规划，也可以以之为以i开始的翻译方法数规划。

````
```cpp
class Solution {
public:
    int translateNum(int num) {
        string src = to_string(num);
        int p = 0, q = 0, r = 1;
        for (int i = 0; i < src.size(); ++i) {
            p = q;
            q = r;
            // 优化掉这两句效果一样
            // r = 0;
            // r += q;
            if (i == 0) continue;;
            auto tmp = src.substr(i-1, 2);
            if (tmp >= "10" && tmp <= "25") {
                r += p;
            }
        }
        return r;
    }
};
```
````

> 剑指 Offer 47. 礼物的最大价值
>
> 在一个 m\*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

动态规划。

````
```cpp
class Solution {
public:
    int translateNum(int num) {
        string src = to_string(num);
        int p = 0, q = 0, r = 1;
        for (int i = 0; i < src.size(); ++i) {
            p = q;
            q = r;
            // 优化掉这两句效果一样
            // r = 0;
            // r += q;
            if (i == 0) continue;;
            auto tmp = src.substr(i-1, 2);
            if (tmp >= "10" && tmp <= "25") {
                r += p;
            }
        }
        return r;
    }
};
```
````

> 剑指 Offer 48. 最长不含重复字符的子字符串
>
> 请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

双指针，遇到重复字符时利用哈希表记录的字符位置直接移动左指针。

````
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> charidx;
        int l = 0, r = 0;
        int ans = 0;
        while (r < s.length()) {
            // 字符串中已有该字符
            if (charidx.find(s[r]) != charidx.end() && charidx[s[r]] >= l) {
                l = charidx[s[r]]+1;
            }
            // 每次更新新加入字符的位置
            charidx[s[r]] = r;
            ans = max(ans, r-l+1);
            r++;
        }
        return ans;
    }
};
```
````
