# 全排列

> 给定一个不含重复数字的数组 `nums` ，返回其 _所有可能的全排列_ 。你可以 **按任意顺序** 返回答案。
>
> &#x20;



{% embed url="https://leetcode.cn/problems/permutations/description/?favorite=2cktkvj" %}

关键在于通过交换数组的位置来确定某个位置选哪个数，这样也刚好可以把数组分为前面一部分已选和后面一部分未选两部分，这样就不同使用专门的空间记录已选和未选，也不需要使用什么空间存储中间结果。

```cpp
class Solution {
public:
    vector<vector<int>> ans;

    vector<vector<int>> permute(vector<int>& nums) {
        bt(0, nums);
        return ans;
    }

    void bt(int first, vector<int> &nums) {
        // 最后一个不用选
        if (first == nums.size()-1) {
            ans.emplace_back(nums);
            return ;
        }
        // 不断为first位选择一个元素
        for (int i = first; i < nums.size(); i++) {
            swap(nums[i], nums[first]);
            bt(first+1, nums);
            swap(nums[i], nums[first]);
        }
    }
};

```
