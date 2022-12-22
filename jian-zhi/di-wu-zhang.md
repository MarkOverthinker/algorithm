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
