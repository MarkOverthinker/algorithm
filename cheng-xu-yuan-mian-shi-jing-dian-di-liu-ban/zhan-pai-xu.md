# 栈排序

> 栈排序。 编写程序，对栈进行排序使最小元素位于栈顶。最多只能使用一个其他的临时栈存放数据，但不得将元素复制到别的数据结构（如数组）中。该栈支持如下操作：`push`、`pop`、`peek` 和 `isEmpty`。当栈为空时，`peek` 返回 -1。

```cpp
class SortedStack {
public:
    SortedStack() {
    }
    
    void push(int val) {
    // 把比val小的都弹到辅助栈中
        while (!stk1.empty() && stk1.top() < val) {
            stk2.push(stk1.top());
            stk1.pop();
        }
        stk1.push(val);
        while (!stk2.empty()) {
            stk1.push(stk2.top());
            stk2.pop();
        }
    }
    
    void pop() {
        // 为空不能pop
        if (stk1.empty()) return;
        stk1.pop();
    }
    
    int peek() {
        return stk1.empty() ? -1 : stk1.top();
    }
    
    bool isEmpty() {
        return stk1.empty();
    }

private:
    stack<int> stk1, stk2;
};

/**
 * Your SortedStack object will be instantiated and called as such:
 * SortedStack* obj = new SortedStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->isEmpty();
 */
```
