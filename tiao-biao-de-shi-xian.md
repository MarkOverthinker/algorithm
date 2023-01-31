# 跳表的实现

原理介绍：[https://www.jianshu.com/p/9d8296562806](https://www.jianshu.com/p/9d8296562806)

代码要点：

* 每个节点不需要存储指向下方的节点，因为每一列节点的值是一样的，不一样只有他们指向的下一个节点。所以Skiplistnode中只存了该节点值和指向下一个节点的数组forward。实际上一个节点代表的是一列节点。使用forward\[i]可以访问第i层节点的下一个节点。
* 使用了哨兵节点，编译操作。
* 在下一个节点大于当前节点的时候才进入下一个节点，等于的时候也不跳转。
* 添加节点的层数是由概率决定的，每加一层的概率变为1/2，因此在大数据量下，可以做到索引比较均匀，而且上一层的节点数量是下一层的一半。每次向下一层，需要搜索的节点数量就减半（可看原理介绍中的图片理解）。所以跳表其实是一种实现了二分查找的链表。

````
```cpp
class Skiplist {
public:

    #define MAX_LEVEL  32
    #define P_FACTOR  0.5

    class Skiplistnode {
        public:
        int val;
        vector<Skiplistnode *> forward;
        Skiplistnode(int v, int level = MAX_LEVEL) {
            val = v;
            forward = vector<Skiplistnode *>(level, nullptr);
        }
    };

    Skiplistnode *head;
    int top;
    mt19937 gen{random_device{}()};
    uniform_real_distribution<double> dis;

    Skiplist(): head(new Skiplistnode(-1)), top(0), dis(0,1) {}
    
    bool search(int target) {
        Skiplistnode *curr = head;
        for (int i = top; i >= 0; --i) {
            while (curr->forward[i] && curr->forward[i]->val < target) {
                curr = curr->forward[i];
            }
        }
        curr = curr->forward[0];
        if (curr && curr->val == target) {
            return true;
        }
        return false;
    }
    
    void add(int num) {
        vector<Skiplistnode *> update(MAX_LEVEL, this->head);
        Skiplistnode *curr = this->head;
        for (int i = top-1; i >= 0; --i) {
            while (curr->forward[i] && curr->forward[i]->val < num) {
                curr = curr->forward[i];
            }
            update[i] = curr;
        }
        int lv = random_level();
        top = max(lv, top);
        Skiplistnode *newnode = new Skiplistnode(num, lv+1);
        // for (int i = lv; i >= 0; i--) {
        for (int i = 0; i <= lv; i++) {
            newnode->forward[i] = update[i]->forward[i];
            update[i]->forward[i] = newnode;
        }
    }

    bool erase(int num) {
        vector<Skiplistnode *> update(MAX_LEVEL, head);
        Skiplistnode *curr = head;
        for (int i = top; i >= 0; --i) {
            while (curr->forward[i] && curr->forward[i]->val < num) {
                curr = curr->forward[i];
            }
            update[i] = curr;
        }
        curr = curr->forward[0];
        if (!curr || curr->val != num) {
            return false;
        }
        for (int i = 0; i < top; ++i) {
            if (update[i]->forward[i] == curr) {
                update[i] = curr->forward[i];
            }
        }
        return true;
    }

    int random_level() {
        int lv = 0;
        while (dis(gen) < P_FACTOR && lv < MAX_LEVEL) {
            lv++;
        }
        return lv;
    }
};

/**
 * Your Skiplist object will be instantiated and called as such:
 * Skiplist* obj = new Skiplist();
 * bool param_1 = obj->search(target);
 * obj->add(num);
 * bool param_3 = obj->erase(num);
 */
```
````
