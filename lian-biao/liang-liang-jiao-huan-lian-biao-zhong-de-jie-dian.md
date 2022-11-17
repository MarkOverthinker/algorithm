# 两两交换链表中的节点

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

最开用的非递归，写着写着发现不对劲，链表head变了而原来的head还指着原来的节点。另外每次循环交换的时候交换到后面的节点需要指向next->next节点而不是next,写起来感觉代码安全上需要考虑的有点多，于是想到了用递归来做。

每次只需要反转两个节点的位置，并且返回反转后前面的节点的位置就可以了，而后面的节点指向的位置也可用递归函数的返回值来指定，写起来简单很多。

自己原来的非递归写法没保存。贴一波官方的：

```
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode pre = new ListNode(0);
        pre.next = head;
        ListNode temp = pre;
        while(temp.next != null && temp.next.next != null) {
            ListNode start = temp.next;
            ListNode end = temp.next.next;
            temp.next = end;
            start.next = end.next;
            end.next = start;
            temp = start;
        }
        return pre.next;
    }
}
```
