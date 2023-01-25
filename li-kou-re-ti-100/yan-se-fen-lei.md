# 颜色分类

> 给定一个包含红色、白色和蓝色、共 `n` __ 个元素的数组 `nums` ，[**原地**](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
>
> 我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。
>
> *
>
> 必须在不使用库内置的 sort 函数的情况下解决这个问题。

遍历一遍，0和2分别往前和后放。

元素种类少，也可遍历一遍用计数排序。

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int p0 = 0, p2 = nums.size()-1;
        for (int i = 0; i <= p2; i++) {
            while (i <= p2 && nums[i] == 2) {
                swap(nums[i], nums[p2]);
                p2--;
            }
            if (nums[i] == 0) {
                swap(nums[i], nums[p0]);
                p0++;
            }
        }
    }
};
```

