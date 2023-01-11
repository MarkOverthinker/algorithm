# 链表求和

> 给定两个用链表表示的整数，每个节点包含一个数位。
>
> 这些数位是反向存放的，也就是个位排在链表首部。
>
> 编写函数对这两个整数求和，并用链表形式返回结果。

主要记录对于不同长度的链表的处理方式

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int last = 0;
        // 记得初始化为空指针
        ListNode *head = nullptr, *tail = nullptr;
        while (l1 || l2) {
            // 对于短的链表，可以用这种方式来补0（节点一直保持为空即可），而不是补0节点
            int v1 = l1 ? l1->val : 0;
            int v2 = l2 ? l2->val : 0;
            int sum = v1 + v2 + last;
            if (!head) {
               tail = head = new ListNode(sum%10);
            } else {
                tail->next = new ListNode(sum%10);
                tail = tail->next;
            }
            last = sum/10;
            // 短链表就不用动了
            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }
        // 最后可能有一位进位
        if (last > 0) {
            tail->next = new ListNode(last);
        }
        return head;
    }
};
```

对于正向记录数位的链表，可以用栈存数位，再依次弹出计算。需要辅助空间。

也可反转链表计算，这样不用辅助空间，但是麻烦一点。

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> s1, s2;
        while (l1) {
            s1.push(l1 -> val);
            l1 = l1 -> next;
        }
        while (l2) {
            s2.push(l2 -> val);
            l2 = l2 -> next;
        }
        int carry = 0;
        ListNode* ans = nullptr;
        while (!s1.empty() or !s2.empty() or carry != 0) {
            int a = s1.empty() ? 0 : s1.top();
            int b = s2.empty() ? 0 : s2.top();
            if (!s1.empty()) s1.pop();
            if (!s2.empty()) s2.pop();
            int cur = a + b + carry;
            carry = cur / 10;
            cur %= 10;
            auto curnode = new ListNode(cur);
            curnode -> next = ans;
            ans = curnode;
        }
        return ans;
    }
};
```
