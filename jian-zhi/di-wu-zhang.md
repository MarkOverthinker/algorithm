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
