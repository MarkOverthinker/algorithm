# 检查平衡性

实现一个函数，检查二叉树是否平衡。在这个问题中，平衡树的定义如下：任意一个节点，其两棵子树的高度差不超过 1。



这道题本质上不难，主要是没想到怎么写把计算高度和判定平衡结合起来（自底向上递归），也一下没想到怎么把计算高度和判定平衡分离成两个函数的写法（自顶向下，其实很经典的写法，一看就懂）

```
// 自顶向下。一次只判断一个树的左右子节点的深度。所以存在重复的遍历，O(n^2)
class Solution {
public:
    int height(TreeNode* root) {
        if (root == NULL) {
            return 0;
        } else {
            return max(height(root->left), height(root->right)) + 1;
        }
    }

    bool isBalanced(TreeNode* root) {
        if (root == NULL) {
            return true;
        } else {
            // 这里一次判定一个节点然后再向下递归判定后续的节点。
            // 很经典的写法，居然没想到
            return abs(height(root->left) - height(root->right)) <= 1 && isBalanced(root->left) && isBalanced(root->right);
        }
    }
};
```

````
// 自底向上。效率更高，O(n)，每个节点只遍历一次。用负数的高度表示不平衡。
// 计算高度和判定平衡放在一个函数里实现了。
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
    int height(TreeNode* root) {
        if (!root) return 0;
        int leftheight = height(root->left);
        int rightheight = height(root->right);
        if (leftheight == -1 || rightheight == -1 || abs(leftheight-rightheight) > 1) {
            return -1;
        }
        return max(leftheight, rightheight) + 1;
    }

    bool isBalanced(TreeNode* root) {
        return height(root) >= 0;
    }
};
```
````
