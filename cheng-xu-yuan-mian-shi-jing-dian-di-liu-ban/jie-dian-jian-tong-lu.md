# 节点间通路

> 节点间通路。给定有向图，设计一个算法，找出两个节点之间是否存在一条路径。

方式一：邻接矩阵+dfs/bfs

```
class Solution {
public:

    bool dfs(int start, int target, vector<vector<int>> &dots, vector<bool> &vis) {
        // 访问start了，记录
        vis[start] = true;
        for (auto val : dots[start]) {
            // val未查找过则进行对其进行查找
            if (!vis[val] && val != start) {
                if (val == target || dfs(val, target, dots, vis)) return true;
            }
        }
        // 注释掉这句，因为上述查找已经证明以start为起点不可能到达target
        // 所以不将start还原为false
        // 使得vis还能用于记录那些start无法到达target，增加查找速度
        // vis[start] = false;
        return false;
    }

    bool findWhetherExistsPath(int n, vector<vector<int>>& graph, int start, int target) {
        vector<bool> vis(n, false);
        vector<vector<int>> dots(n, vector<int>());
        // 使用引用，减少拷贝，速度暴涨
        for (const auto &vec : graph) {
            dots[vec[0]].push_back(vec[1]);
        }
        return dfs(start, target, dots, vis);
    }
};
```

方式二：

一条边的起点可达，则终点也可达。

可以用这个方式进行搜索。

```cpp
class Solution {
public:
    bool findWhetherExistsPath(int n, vector<vector<int>>& graph, int start, int target) {
        //建立hash表，记录当下可以到达的结点，hash[i]为i是否可以到达
        vector<bool> hash(n);
        hash[start] = true;
        bool flag = true;
        // 由于路径最长为n，所以while循环最多进行n次
        while (flag) {
            flag = false;
            // size()最大为n^2级的(不考虑无限多的平行边的情况下)，所以最坏情况O(n^3)
            for(int i=0;i<graph.size();++i){
                // 每当一条路径的初始位置可以到达，那么这条路径的终点就可以到达
                // 对两个点都访问过的情况不要进行更新。否则死循环了。(if里第二个判断条件的作用)
                // 当路径已找到时直接return即可
                // 若一次循环中一次更新都没有，说明不存在这条路径
                if(hash[graph[i][0]] && !hash[graph[i][1]]){
                    hash[graph[i][1]] = true;
                    if (hash[target]) return true;
                    flag = true;
                }
            }
        }
        return hash[target];
    }
};
```

方式一建立邻接矩阵，可以排出平行边，而方式二每次都会访问平行边，这是方式二的一个缺点。
