# 合法二叉搜索树

实现一个函数，检查一棵二叉树是否为二叉搜索树。

中序遍历，检验当前遍历到的数是否比上一个数大即可。

忽然发现两个函数可以合成一个函数。

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
    bool dfs(TreeNode* root) {
        if (!root) return true;
        if (!dfs(root->left) || (pre >= root->val && flag)) {
            return false;
        }
        pre = root->val;
        if (!flag) flag = true;
        return dfs(root->right);
    }

    bool isValidBST(TreeNode* root) {
        pre = INT_MIN;
        flag = false;
        return dfs(root);
    }

private:
    int pre;
    // 用一个flag判断是不是第一个遇到的节点
    // 主要用于解决用例中会有节点的值为INT_MIN的问题
    // 为了方便其实也可以用LONG_MIN
    bool flag;
};
```

还有种方法是给子树划分值域，递归遍历即可。之前想的时候比较局限的只思考遍历时大于多少或小于多少，其实给他们划分值域就好了，既要大于一个值，也要小于一个值。实现也较简单。

不过都要考虑节点中可能有INT\_MAX和INT\_MIN的存在。

```
class Solution {
public:
    bool helper(TreeNode* root, long long lower, long long upper) {
        if (root == nullptr) {
            return true;
        }
        if (root -> val <= lower || root -> val >= upper) {
            return false;
        }
        return helper(root -> left, lower, root -> val) && helper(root -> right, root -> val, upper);
    }
    bool isValidBST(TreeNode* root) {
        return helper(root, LONG_MIN, LONG_MAX);
    }
};

```
