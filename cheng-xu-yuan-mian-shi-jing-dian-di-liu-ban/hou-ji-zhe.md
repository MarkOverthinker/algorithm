# 后继者

> 设计一个算法，找出二叉搜索树中指定节点的“下一个”节点（也即中序后继）。
>
> 如果指定节点没有对应的“下一个”节点，则返回`null`。

可以中序遍历到指定节点，再输出下一个节点。

也可以利用二叉搜索树的性质，搜索最小的大于指定节点的节点。

```cpp
// 二叉搜索的方法
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
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        TreeNode *succ = nullptr;
        // 优化，有右子树直接找右子树最左节点
        if (p->right) {
            succ = p->right;
            while (succ->left) {
                succ = succ->left;
            }
            return succ;
        }
        // 否则从根节点开始二叉搜索
        TreeNode *node = root;
        while (node) {
            // 在树里搜索记录最后一个大于p的节点即可
            if (node->val > p->val) {
                succ = node;
                node = node->left;
            } else {
                node = node->right;
            }
        }
        return succ;
    }
};
```

```
// 中序遍历也可。（以下为用栈实现的中序遍历）
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        stack<TreeNode*> st;
        TreeNode *prev = nullptr, *curr = root;
        while (!st.empty() || curr != nullptr) {
            while (curr != nullptr) {
                st.emplace(curr);
                curr = curr->left;
            }
            curr = st.top();
            st.pop();
            if (prev == p) {
                return curr;
            }
            prev = curr;
            curr = curr->right;
        }
        return nullptr;
    }
};
```

