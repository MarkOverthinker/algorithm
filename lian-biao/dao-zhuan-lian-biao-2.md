# 倒转链表2

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

思路易理解，运用头插法一次遍历即可。一个重要的点是哨兵节点的运用，存在left从第一个节点开始的情况，那么此时头插法的头就在第一个结点之前，于是可以设一个哨兵指向head,从这个哨兵起向后寻找头插法的头在哪，如果left为1那么就可以以这个哨兵为头进行头插法实现反转，省去了对left=1这种边界值的额外考虑。另外在返回结果时也要注意返回的是dummynode->next而不是head,同样是为了应对left=1这种情况。
