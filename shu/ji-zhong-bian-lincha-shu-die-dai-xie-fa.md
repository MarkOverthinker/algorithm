# 几种遍历N叉树迭代写法

### 前序遍历N叉树

用存放树节点的栈实现

对N叉树的每一个非空节点，从右往左入栈。

初始时，若根节点不为空，将跟节点入栈

当栈中有元素时，循环进行：

弹出栈顶节点，将该节点的值存入遍历结果中，然后将对N叉树子节点按规则入栈。

```
// 前序遍历
class Solution {
public:
    vector<int> preorder(Node* root) {
        if (nullptr == root) return {};
        vector<int> ret;
        stack<Node *> stk;
        stk.push(root);        
        while (stk.size() > 0) {
            Node *node = stk.top();
            stk.pop();
            ret.push_back(node->val);
            for (int i = node->children.size()-1; i >= 0; --i) {
                if (nullptr != node->children[i]) {
                    stk.push(node->children[i]);
                }
            }
        }
        return ret;
    }
};
```



### 中序遍历二叉树

对一个根节点，向下遍历它的全部左子节点并入栈。没弹出栈一个节点后，获取它的右子节点，将对右子树采取同样的操作。即可保证完成中序遍历。

```
// 中序遍历
 vector<int> inorderTraversal(TreeNode* root) {
        if (nullptr == root) return {};
        vector<int> ret;
        stack<TreeNode*> stk;
        TreeNode *node = root;
        while (node != nullptr || !stk.empty()) {
            while (nullptr != node) {
                stk.push(node);
                node = node->left;
            }
            node = stk.top();
            stk.pop();
            ret.push_back(node->val);
            node = node->right;
        }
        return ret;
    }

```



### 后序遍历N叉树

迭代方法1（栈+set）：

按子节点从右往左->根节点的顺序将节点入栈，并用unordered\_set标记root，表示root的子节点已入栈。

当栈不为空时，若栈顶已标记，则出栈并存入结果集中。若未标记，则将该节点的子节点按从右往左的顺序入栈并标记该节点。重复上述步骤即可。

迭代方法二 (用类似前序遍历实现中右左的遍历，再将结果反转即得后序遍历)：

方法1需要用set标记的根本原因在于根节点放在后面输出，即上层节点是后于下层节点输出的，栈顶的节点要被访问两次才会输出，即第一次访问时将它的子节点入栈，第二次访问时说明子节点已遍历完此时才输出该节点。而前序遍历由于上层节点先输出，入栈顺序为上层先于下层，每个栈顶节点只需要被访问一次就能出栈，然后将他的子节点入栈，所以无需用set进行标记，每次直接栈顶出栈然后子节点入栈就行了。

因此，采取类似于前序遍历（中左右）的实现方式，实现中右左的遍历，之后将遍历结果反转即可得到后序遍历的结果（左右中）

```
// 迭代方法2
class Solution {
public:
    vector<int> postorder(Node* root) {
        if (nullptr == root) return {};
        vector<int> ret;
        stack<Node*> stk;
        stk.push(root);
        while (!stk.empty()) {
            Node *node = stk.top();
            ret.push_back(node->val);
            stk.pop();
            for (auto &n : node->children) {
                stk.push(n);
            }
        }
        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```

### 中序也只需要使用栈的思考

中序在输出一个节点后，立刻就能找到他的右儿子，此时中->右的顺序确定了。而一开始一直循环向下寻找左子节点压入，左->中的顺序也确定了。
