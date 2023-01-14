# 首个公共祖先

> 设计并实现一个算法，找出二叉树中某两个节点的第一个共同祖先。不得将其他的节点存储在另外的数据结构中。注意：这不一定是二叉搜索树。

* 判断是第一个公共祖先的方法：左子树和右子树都包含对应节点，或者自己是对应节点且左子树或右子树包含对应节点
* “包含”这个点很重要，之前老想着各种情况弄混了，什么左边有P右边有q，左边有q右边有p，一颗子树有q也有p,实际上只要使用包含其中任意个取判断就可以了。

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
    // dfs搜索root中是否包含p,q中的一个或多个
    bool dfs(TreeNode *root, TreeNode *p, TreeNode *q) {
        if (!root) return false;
        bool l = dfs(root->left, p, q);
        bool r = dfs(root->right, p, q);
        // 只有第一个祖先节点满足
        if ((l && r) || (root->val == p->val || root->val == q->val) && (l || r)) {
            ans = root;
        }
        return l || r || root->val == p->val || root->val == q->val;
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        dfs(root, p, q);
        return ans;
    }

private:
    TreeNode *ans;

};
```
