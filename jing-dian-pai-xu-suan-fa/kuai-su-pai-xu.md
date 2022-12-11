# 快速排序

{% embed url="https://blog.csdn.net/liuchen1206/article/details/6954074" %}
解释的还可以
{% endembed %}

随机选取的数可以固定为某个数。

采用挖坑填数的方法，把比第0个数大的数填到第0个位置，然后这个数的位置又成了新坑用来填比第0个数小的数。反复填即可。

```cpp
#include "iostream"
#include "vector"

void quicksort(std::vector<int> &nums, int l, int r) {
    int i = l, j = r;
    int x = nums[l];
    // if 不能剩。否则最后面两句递归终止不了，即每次函数都一定会执行最后两句
    if (i < j) {
        while (i < j) {
            for ( ; j > i; j--) {
                if (nums[j] < x) {
                    nums[i++] = nums[j];
                    break;
                }
            }
            for ( ; i < j; i++) {
                if (nums[i] > x) {
                    nums[j--] = nums[i];
                    break;
                }
            }
        }
        // j+1很重要，两个区间不能相交.
        // 否则会出现类似于分治为[0, 0], [0,4]这种无限递归的情况
        quicksort(nums, l, i);
        quicksort(nums, j+1, r);
    }
}

int main() {
    std::vector<int> nums{5, 1, 2, 7, 3, 4, 9, 8, 0, 12};
    auto sz = nums.size();
    quicksort(nums, 0, sz-1);
    for (auto i = 0; i < sz; ++i) {
        std::cout << nums[i] << "\t";
    }
    std::cout << std::endl;
    return 0;
}
```
