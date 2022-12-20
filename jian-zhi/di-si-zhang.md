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

> 题31. 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

用一个辅助栈进行模拟

````
```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        int n = pushed.size();
        stack<int> stk;
        for (int i = 0, j = 0; i < n; ++i) {
            // 在每次无法出栈后压入栈。最初的一次也统一在这里
            stk.push(pushed[i]);
            // 按popped中的出栈顺序出栈
            while (!stk.empty() && stk.top() == popped[j]) {
                stk.pop();
                j++;
            }
        }
        return stk.empty();
    }
};
```
````

> 题32.打印二叉树。
>
> 从上到下打印（层序遍历） ： 用队列实现即可
>
> 每一层打印到一行： 层序遍历时，用toBePrint记录当前层还有多少没打印，用nextLevel记录下一层节点数量，每次子节点进入队列则nextLevel++.
>
> 之字型打印：奇数层从左到右打印，偶数层从右往左打印。可以：打印奇数层时，把子节点从左向右压入栈，而打印偶数层时，把子节点从右往左压入栈。理解：上一层终点为下一层起点，也就是反转过来。所以用栈。因为上下层不能共用，所以要两个栈。

> 题33.输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

不只是二叉搜索树，任何有关到二叉树的遍历序列问题，都应该考虑到遍历序列中根节点的位置。

本题为二叉搜索树根节点为序列最后一个，则可根数字大小把序列中的数分为前一部分和后一部分，前一部分一定全部小于根节点，后一部分一定全部大于根节点。然后可递归进这两部分继续分。若不能这样分，说明不是二叉搜索树。

````
```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        int n = postorder.size();
        if (n == 0) {
            return true;
        }
        vector<int> left, right;
        int root = postorder[n-1];
        int i = 0;
        // 分出左部分
        while (i < n-1 && postorder[i] <= root) {
            left.emplace_back(postorder[i]);
            i++;
        }
        // 分出右部分。如果找到比根节点小的则说明不是二叉搜索树后序遍历序列。
        while (i < n-1) {
            if (postorder[i] > root) {
                right.emplace_back(postorder[i]);
            } else {
                return false;
            }
            ++i;
        }
        return verifyPostorder(left) && verifyPostorder(right);
    }
};
```
````

> 题34. 剑指 Offer 34. 二叉树中和为某一值的路径
>
> 给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。
>
> **叶子节点** 是指没有子节点的节点。

dfs + 回溯即可。注意注释中的几个细节。

````
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    // 也可以不传sum。而是改变target并传入
    void dfs(vector<vector<int>> &ans, vector<int> &tmp, TreeNode* root, int target, int sum) {
        if (!root) return ;
        sum += root->val;
        tmp.emplace_back(root->val);
        // 注意题目要求是为叶子结点。需要进行判断
        if (sum == target && root->left == nullptr && root->right == nullptr) {
            ans.emplace_back(tmp);
        } else { // 这里有坑，要用else，而不能else if (sum < target)，因为节点中可能有负数，目标值也可能有负数。
                // 不能想当然认为两者都为正数。
            dfs(ans, tmp, root->left, target, sum);
            dfs(ans, tmp, root->right, target, sum);
        }
        tmp.pop_back();
    }

    vector<vector<int>> pathSum(TreeNode* root, int target) {
        vector<vector<int>> ans;
        vector<int> tmp;
        dfs(ans, tmp, root, target, 0);
        return ans;
    }
};
```
````
