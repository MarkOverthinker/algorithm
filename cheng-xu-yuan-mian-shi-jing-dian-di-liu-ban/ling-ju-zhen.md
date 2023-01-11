# 零矩阵

> 编写一种算法，若M × N矩阵中某个元素为0，则将其所在的行与列清零。

其实就是遍历一遍数组记录有哪些列和行需要置0，然后遍历一遍数组并根据记录选择是否将遍历到的元素置零。

优化的点在于可以使用第一列和第一行来记录有哪些列和行可以置0，降低了空间复杂度。

````
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        if (matrix.empty()) return ;
        int m = matrix.size();
        int n = matrix[0].size();
        bool isrowzero = false, iscolzero = false;
        for (int i = 0; i < n; ++i) {
            if (matrix[0][i] == 0) isrowzero = true;
        }
        for (int j = 0; j < m; ++j) {
            if (matrix[j][0] == 0) iscolzero = true;
        }
        // 置0表示这列/行有0，不为0说明该行/列没有0
        for (int i = 1; i < n; ++i) {
            for (int j = 1; j < m; ++j) {
                if (matrix[j][i] == 0) {
                    matrix[0][i] = 0;
                    break;
                }
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    break;
                }
            }
        }
        // 直接遍历每一个元素然后根据标记判断是否需要置0
        // 而不是遍历标记再逐行逐列置0
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[0][j] == 0 || matrix[i][0] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        if (isrowzero) {
            for (int i = 0; i < n; ++i) matrix[0][i] = 0;
        }

        if (iscolzero) {
            for (int i = 0; i < m; ++i) matrix[i][0] = 0;
        }
    }
};
```
````
