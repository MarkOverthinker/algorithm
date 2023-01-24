# 组合总和

> 给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 __ **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。
>
> `candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。&#x20;
>
> 对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

这题限制了输入一定为正数，所以回溯终点很好找，即传入的target为负时。

回溯的方法很好想到，最关键的点在于避免选出重复的组合。

这里回溯时采用的传入一个startidx的方式，表示：找到所有包含了nums\[startidx]且不包含num\[m(m\<startidx)]的组合。这样就能找到全部组合且不重复。

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        vector<int> tmp;
        bt(ans, tmp, target, candidates, 0);
        return ans;
    }

    // startidx 用来限制找出所有包含idx == startidx的组合
    // 之后，再找出不包含idx == startidx的数的所有组合
    // 这些组合里又有包含idx == startidx+1和不包含idx == startidx+1的组合
    // 以此递推找出全部组合，同时避免出现重复的组合
    void bt(vector<vector<int>> &ans, vector<int> &tmp, int target, vector<int> &candidates, int startidx) {
        if (target < 0) {
            return ;
        }
        for (int i = startidx; i < candidates.size(); i++) {
            int val = candidates[i];
            tmp.emplace_back(val);
            if (val == target) {
                ans.emplace_back(tmp);
            } else {
                bt(ans, tmp, target-val, candidates, i);
            }
            tmp.pop_back();
        }
    }
};
```

