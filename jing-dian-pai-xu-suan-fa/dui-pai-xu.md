# 堆排序

用数组表示堆排序的完全二叉树，则每个节点左字节为2n+1, 右子节点为2n+2。最后一个非叶子节点的编号为(最后一个节点的编号-1) / 2。

堆排序即不断取出最大堆/最小堆的对顶然后重新构造最大堆/最小堆。



取大数据量中最大的n个数，可以维护一个大小为n的最小堆，每次取一个数和堆顶比较，比他小就丢弃，比他大就去掉堆顶，把这个数加入最小堆。重复执行即可

<pre><code><strong>```cpp
</strong>#include "iostream"
#include "vector"
#include "algorithm"

void adjust(std::vector&#x3C;int> &#x26;nums, size_t n) {
    auto end = (n-1)/2;
    // 这里要用int i，而不能auto i 推导成size_t i，因为size_t i 会在i = 0的时候i--成很大的正数
    // 而不是变成负数退出循环
    for (int i = end; i >= 0; i--) {
        if (i*2+1 &#x3C;= n &#x26;&#x26; nums[i*2+1] > nums[i]) {
            std::swap(nums[i], nums[i*2+1]);
        }
        if (i*2+2 &#x3C;= n &#x26;&#x26; nums[i*2+2] > nums[i]) {
            std::swap(nums[i], nums[i*2+2]);
        }
    }
}

void heapsort(std::vector&#x3C;int> &#x26;nums, size_t n) {
    auto maxidx = n-1;
    // 有n个数，循环n次。每次排出一个最大的然后提取出来
    for (int i = n; i > 0; --i) {
        adjust(nums, maxidx);
        std::swap(nums[0], nums[maxidx--]);
    }
}

int main() {
    std::vector&#x3C;int> nums{2, 4, 1, 5, 6, 2, 9, 8, 7, 0, 4};
    auto sz = nums.size();
    heapsort(nums, sz);
    for (auto i = 0; i &#x3C; sz; ++i) {
        std::cout &#x3C;&#x3C; nums[i] &#x3C;&#x3C; "\t";
    }
    std::cout &#x3C;&#x3C; std::endl;
    return 0;
}
```
</code></pre>
