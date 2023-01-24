# 搜索旋转排序数组

> 整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。
>
> 在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。
>
> 给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。
>
> 你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

二分搜索

关键的地方在于二分出的两部分一部分有序而一部分无序，可以根据和nums\[0]比较找出哪一个是有序的哪一个是无序的，然后在有序的这部分很容易判断target是否在这段有序的数里面，从而可以判断target是在左还是在右

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        if (!n) return -1;
        if (n == 1) return nums[0] == target ? 0 : -1;
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r)/2;
            // 这句判断必须有，否则mid为target的情况会被跳过
            if (nums[mid] == target) return mid;
            // 找到的有序的那部分，利用这部分可以判断target在哪一边
            if (nums[0] <= nums[mid]) {
                // 在有序的这部分
                if (nums[0] <= target && target <= nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                // 在有序的这部分
                if (nums[mid] <= target && target <= nums[n-1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return -1;
    }
};

```
