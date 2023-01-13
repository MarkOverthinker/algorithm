# 面试题 04.03. 特定深度节点链表

给定一棵二叉树，设计一个算法，创建含有某一深度上所有节点的链表（比如，若一棵树的深度为 `D`，则会创建出 `D` 个链表）。返回一个包含所有深度的链表的数组。



主要是为了记录这种分辨层序遍历每一层的写法

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
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<ListNode*> listOfDepth(TreeNode* tree) {
        queue<TreeNode*> que;
        int curr = 1, next = 1;
        que.push(tree);
        vector<ListNode*> ret;
        // 每次循环遍历一层
        while(!que.empty()) {
            // curr 记录当前层节点数
            // next 记录下一层
            curr = next;
            next = 0;
            ListNode *dummy = new ListNode(-1);
            ListNode *tail = dummy;
            // 进行一层遍历
            while(curr) {
                TreeNode *node = que.front();
                que.pop();
                curr--;
                tail->next = new ListNode(node->val);
                tail = tail->next;
                if (node->left) {
                    next++;
                    que.push(node->left);
                }
                if (node->right) {
                    next++;
                    que.push(node->right);
                }
            }
            if (dummy->next) ret.push_back(dummy->next);
        }
        return ret;
    }
};
```
````
