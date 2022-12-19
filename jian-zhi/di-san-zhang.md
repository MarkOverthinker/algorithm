# 第三章

> 题16. 快速幂

{% embed url="https://leetcode.cn/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/solutions/1398793/shu-zhi-de-zheng-shu-ci-fang-by-leetcode-yoqr/" %}

```
// 迭代写法
class Solution {
public:
    double quickMul(double x, long long N) {
        double ans = 1.0;
        // 贡献的初始值为 x
        double x_contribute = x;
        // 在对 N 进行二进制拆分的同时计算答案
        while (N > 0) {
            if (N % 2 == 1) {
                // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
                ans *= x_contribute;
            }
            // 将贡献不断地平方
            x_contribute *= x_contribute;
            // 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
            N /= 2;
        }
        return ans;
    }

    double myPow(double x, int n) {
        long long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
};
```

```
// 递归写法
class Solution {
public:
    double quickMul(double x, long long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }

    double myPow(double x, int n) {
        long long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }
};

```

> 题17. 打印从1到n位的数

考虑到n位可能很大，超过int,long之类的限制。所以用字符串来模拟。

````
```cpp
#include "string"
#include "iostream"
#include "vector"

const int N = 4;

// 生成所有长度为length的数
void printnum(std::string &s, int cnt , int length) {
    if (cnt == length) {
        std::cout << s << std::endl;
        return ;
    }
    // 不生成0开头的数
    int startidx = cnt == 0 ? 1 : 0;
    // dfs + 回溯
    for (int i = startidx; i < 10; ++i) {
        s.push_back('0'+i);
        printnum(s, cnt+1, length);
        s.pop_back();
    }
}

int main() {
    std::string s;
    for (int i = 1; i <= N; ++i) {
        printnum(s, 0, i);
    }
}
```
````

> 删除链表中的节点。要求O(1)，给出了头结点和被删除节点的指针。

为了在O(1)时间内完成，可以采用将被删除节点的下一个节点复制到该节点再删除下一个节点的方法，必须要求被删除的节点不是尾节点。否则只能遍历到倒数第二个节点进行删除。还要考虑只有一个结点的情况。

> 题19. 正则表达式匹配
>
> 请实现一个函数用来匹配包含`'. '`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但与`"aa.a"`和`"ab*a"`均不匹配。
>
> 示例：
>
> <pre><code><strong>输入:
> </strong>s = "aa"
> p = "a*"
> <strong>输出: true
> </strong><strong>解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。
> </strong><strong>因此，字符串 "aa" 可被视为 'a' 重复了一次。
> </strong></code></pre>

{% embed url="https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solutions/521347/zheng-ze-biao-da-shi-pi-pei-by-leetcode-s3jgn/" %}

```
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();

        auto match = [&] (int i, int j) {
            // 假设s为空时不匹配
            if (i == 0) {
                return false;
            }
            if (p[j-1] == '.') {
                return true;
            }
            return s[i-1] == p[j-1];
        };
        vector<vector<int>> f(m+1, vector<int>(n+1));
        f[0][0] = true;
        for (int i = 0; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (p[j-1] == '*') {
                    f[i][j] |= f[i][j-2];
                    if (match(i, j-1)) {
                        f[i][j] |= f[i-1][j];
                    }
                } else {
                    if (match(i, j)) {
                        f[i][j] |= f[i-1][j-1];
                    }
                }
            }
        }
        return f[m][n];
    }
};
```



> 题21. 把数组中奇数放到偶数前面。

用双指针找到偶数和奇数进行交换，将奇数交换到前面，偶数交换到后面。

可以抽象成通用的方法，传入一个函数指针用于判断某位的数，将满足xx条件的数放到不满足xx条件的数的前面或后面。

````cpp
```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int i = 0, j = nums.size()-1;
        while (i < j) {
            while (i < j && nums[i] % 2 == 1) {
                ++i;
            }
            while (i < j && nums[j] % 2 == 0) {
                --j;
            }
            // 相等则未找到
            if (i < j) {
                swap(nums[i], nums[j]);
            }
        }
        return nums;
    }
};
```
````

> 题22. 链表中倒数第k个节点。
>
> 可用双指针，前一个指针先走k-1步即可。

鲁棒性问题：

* 头指针可能为空
* 节点总数可能小于k
* k可能为0.

> 题23. 找出链表中环的入口节点。

方式1：一直往前走，哈希

方式2：用快慢指针，一个一次走1步一个一次走2两步，等两个指针相遇后，再用一个指针和慢指针一起一次走一步直到相遇，相遇点即为入口。推导可见力扣官方题解；

{% embed url="https://leetcode.cn/problems/c32eOV/solutions/1037744/lian-biao-zhong-huan-de-ru-kou-jie-dian-vvofe/" %}

````
```cpp
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
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;
        while (fast != nullptr) {
            // 已判断fast != nullptr，所以可以认为slow != nullptr
            slow = slow->next;
            if (fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;
            if (fast == slow) {
                ListNode *ret = head;
                while (ret != slow) {
                    ret = ret->next;
                    slow = slow->next;
                }
                return ret;
            }
        }
        return fast;
    }
};
```
````

> 题24. 反转链表
>
> 给定单链表的头节点 `head` ，请反转链表，并返回反转后的链表的头节点

要注意需要保存当前节点、前一个节点、和下一个节点。不然反转过程中容易断开。

````cpp
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *prev = nullptr;
        ListNode *newhead = nullptr;
        ListNode *curr = head;
        while (curr) {
            if (curr->next == nullptr) {
                newhead = curr;
                curr->next = prev;
                break;
            }
            // 保存下一个
            ListNode *tmp = curr->next;
            // 反指
            curr->next = prev;
            prev = curr;
            curr = tmp;
        }
        return newhead;
    }
};
```
````

> 题25. 合并两个升序链表

递归写法：找出较小的那个，然后令其next指向为合并结果；

````
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (list1 == nullptr) {
            return list2;
        }
        if (list2 == nullptr) {
            return list1;
        }
        if (list1->val < list2->val) {
            list1->next = mergeTwoLists(list1->next, list2);
            return list1;
        }
        list2->next = mergeTwoLists(list1, list2->next);
        return list2;
    }
};
```
````

> 题26. 树的子结构。输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)
>
> B是A的子结构， 即 A中有出现和B相同的结构和节点值。

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
    // isSub只用来判断A是否严格的包含B。即以A为根节点的树的上层是否和B完全一样。
    // 而不是用来判断A是否包含B.
    // 这两个功能的区分很关键
    bool isSub(TreeNode *A, TreeNode *B) {
        if (B == nullptr) {
            return true;
        }
        if (A == nullptr || A->val != B->val) {
            return false;
        }
        return isSub(A->left, B->left) && isSub(A->right, B->right);
    }

    // 用来判断是否A包含子结构B
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (B == nullptr || A == nullptr) return false;
        // 要么A=B，要么的子节点中有A = B
        return isSub(A, B) || isSubStructure(A->left, B) || isSubStructure(A->right, B);
    }
};
```
````

> 题26、树的子结构
>
> 输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)
>
> B是A的子结构， 即 A中有出现和B相同的结构和节点值。

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
    // isSub只用来判断A是否严格的包含B。即以A为根节点的树的上层是否和B完全一样。
    // 而不是用来判断A是否包含B.
    // 这两个功能的区分很关键
    bool isSub(TreeNode *A, TreeNode *B) {
        if (B == nullptr) {
            return true;
        }
        if (A == nullptr || A->val != B->val) {
            return false;
        }
        return isSub(A->left, B->left) && isSub(A->right, B->right);
    }

    // 用来判断是否A包含子结构B
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (B == nullptr || A == nullptr) return false;
        // 要么A=B，要么的子节点中有A = B
        return isSub(A, B) || isSubStructure(A->left, B) || isSubStructure(A->right, B);
    }
};
```
````
