# 求和路径

> 给定一棵二叉树，其中每个节点都含有一个整数数值(该值或正或负)。设计一个算法，打印节点数值总和等于某个给定值的所有路径的数量。注意，路径不一定非得从二叉树的根节点或叶节点开始或结束，但是其方向必须向下(只能从父节点指向子节点方向)。

用了前缀和。dfs遍历每一条路径，在遍历到每个节点（记为p）时，记录他的前缀和。同时就可计算出root->p的路径和curr,用哈希表保存路径上root->每个节点的距离出现了多少次，此时路径上每个节点到p的路径和都是可以计算出来的，只需找存在多少个q，root->q = curr-sum，则q->p=sum。所以只需根据哈希表找pre\[curr-sum]的值即可，若不存在pre\[curr-sum]说明这条路径上不存在以p为终点的路径和为sum的路径。由于是dfs遍历了所有节点，所以相当于寻找了以所有节点为终点的路径。同时因为有哈希表记录以及每个节点只遍历一次，免去了重复计算。

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
    // 用curr记录root->父节点路径总长度
    // 用pre记录所有root->路径上各个点路径长度个数（可能不止一个）
    int dfs(TreeNode* root, int curr, int sum) {
        if (!root) return 0;
        // 将curr加上val表示root->本节点路径长度
        curr += root->val;
        int ret = 0;
        // 查看是否存在路径上一个节点到本节点长度为sum
        if (pre.count(curr-sum)) {
            // 是pre[curr-sum],不是count(curr-sum)
            // 之前下意识想着数curr-sum就用成count了
            ret = pre[curr-sum];
        }
        pre[curr]++;
        ret += dfs(root->left, curr, sum);
        ret += dfs(root->right, curr, sum);
        pre[curr]--;
        return ret;
    }

    int pathSum(TreeNode* root, int sum) {
        pre[0] = 1;
        return dfs(root, 0, sum);
    }

private:
    unordered_map<int, int> pre;
};
```
