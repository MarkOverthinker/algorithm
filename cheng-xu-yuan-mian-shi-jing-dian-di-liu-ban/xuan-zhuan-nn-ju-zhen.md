# 旋转n\*n矩阵

顺时针旋转90度的情况：

````
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        // 每次交换四个，所以把矩阵四等分，遍历其中一个分区就行了
        // 对于分区的边界，只取一个边界上的就行，所以上面是(n+1)/2，下面是n/2。
        // 也就是有一个n+1就行。下面n+1上面n也可以
        for (int i = 0; i < (n+1) / 2; ++i) {
            for (int j = 0; j < n / 2; ++j) {
                int temp = matrix[i][j];
                // 第一个为基础公式。因为是旋转90度，观察矩阵可知道一定能形成一个圈(旋转一定能形成圈)
                // 所以反复套用第一个公式一定能形成循环
                // 因为是90度，所以一次交换360/90 = 4 个。（每套用一次公式相当于旋转90度）
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n-j-1][i] = matrix[n-i-1][n-j-1];
                matrix[n-i-1][n-j-1] = matrix[j][n-i-1];
                matrix[j][n-i-1] = temp;
            }
        }
    }
};
```
````
