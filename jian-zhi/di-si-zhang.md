# 第四章

> 题28. 对称的二叉树。
>
> 判断一个二叉树是否是对称的。

用两个指针，一个一个向左走一个向右走。不断往下遍历判断。

````
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool check(TreeNode *p, TreeNode *q) {
        if (!p && !q) {
            return true;
        }
        return p && q && p->val == q->val && check(p->left, q->right) && check(p->right, q->left);
    }

    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return check(root->left, root->right);
    }
};
```
````

> 题29. 顺时针打印矩阵
>
> 将矩阵按从外向里以顺时针的顺序依次打印出来。

取一个矩形的四个端点，打印一周。再缩小矩阵。直至打印完所有数。

````
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return {};
        }
        int left = 0, right = matrix[0].size() - 1;
        int top = 0, bottom = matrix.size() - 1;
        vector<int> order;
        // 打印左列和底行的时候只打印不与上下两行相交的部分
        while (left <= right && top <= bottom) {
            // 打印这一圈的上层行
            for (int i = left; i <= right; i++) {
                order.push_back(matrix[top][i]);
            }
            // 打印这一圈的最右边一列
            for (int i = top+1; i <= bottom; ++i) {
                order.push_back(matrix[i][right]);
            }
            // 确认不是单行或者单列
            if (left < right && top < bottom) {
                // 打印最下面一行
                for (int i = right-1; i >= left; --i) {
                    order.push_back(matrix[bottom][i]);
                }
                // 打印左边一列.
                for (int i = bottom-1; i > top; --i) {
                    order.push_back(matrix[i][left]);
                }
            }
            // 收缩
            ++left;
            ++top;
            --right;
            --bottom;
        }
        return order;
    }
};
```
````

> 题30. 包含min函数的栈。实现一个包含min函数的栈。min函数返回栈中最小值。

````
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return {};
        }
        int left = 0, right = matrix[0].size() - 1;
        int top = 0, bottom = matrix.size() - 1;
        vector<int> order;
        // 打印左列和底行的时候只打印不与上下两行相交的部分
        while (left <= right && top <= bottom) {
            // 打印这一圈的上层行
            for (int i = left; i <= right; i++) {
                order.push_back(matrix[top][i]);
            }
            // 打印这一圈的最右边一列
            for (int i = top+1; i <= bottom; ++i) {
                order.push_back(matrix[i][right]);
            }
            // 确认不是单行或者单列
            if (left < right && top < bottom) {
                // 打印最下面一行
                for (int i = right-1; i >= left; --i) {
                    order.push_back(matrix[bottom][i]);
                }
                // 打印左边一列.
                for (int i = bottom-1; i > top; --i) {
                    order.push_back(matrix[i][left]);
                }
            }
            // 收缩
            ++left;
            ++top;
            --right;
            --bottom;
        }
        return order;
    }
};
```
````

```
```
