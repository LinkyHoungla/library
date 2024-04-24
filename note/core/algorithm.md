# 搜索

## 广度优先算法

广度优先搜索 (Breadth-First Search, BFS)是一种用于图形数据结构的基本算法，用于逐层地扩展和探索图中的节点。

它从根节点开始，沿着树或图的宽度遍历所有相邻节点，然后逐层向外扩展，直到找到目标节点或遍历完整个图。该算法类似于树的层序遍历，通常借助队列实现，确保按照层级顺序逐个访问节点。

### 基本原理

BFS 使用队列来实现节点的遍历和探索。具体步骤如下：

1. **初始化**：将起始节点放入队列中，并标记为已访问。
2. **循环**：从队列中取出一个节点，探索其所有相邻节点。
3. **标记和入队**：对于每个未访问过的相邻节点，标记为已访问并放入队列。
4. **重复**：重复步骤2和步骤3，直到队列为空。

BFS 保证在访问所有当前层级节点后才会进入下一层级，因此可以用来求解最短路径或最短步数等问题。

### 代码实现

**伪代码**

```c
// 访问标记数组
int visited[Maxsize];

// 对图 G 进行广度优先遍历
void BFSTraverse(Graph G) {
    // 初始化访问标记数组
    for(int i = 0; i < G.vexnum; i ++) 
        visited[i]=0;
    // 初始化辅助队列
    InitQueue(Q);
    for(int i = 0; i < G.vexnum; i ++)
        // 对每个连通分量调用一次 BFS
        if(!visited[i])
            BFS(G,i);
}

void BFS(Graph G, int v) {
    // 从顶点 v 出发，广度优先遍历图 G
    // 访问初始顶点 v
    visit(v);
    // 对 v 做已访问标记
    visited[v] = 1;
    // 顶点 v 入队
    EnQueue(Q,v);
    while(!isempty(Q)) {
        // 队头顶点出队
        DeQueue(Q,v);
        // 将 v 的所有未访问的邻接顶点访问并入队
        for(w = FirstNeighbor; w >= 0; w = NextNeighbor) {
            // 此时 w 为 v 的未访问的邻接顶点
            if(!visited[w]) {
                // 访问顶点 w
                visit(w);
                // 对 w 做已访问标记
                visited[w] = 1;
                // 将顶点 w 入队
                EnQueue(Q,w);
            }
        }
    }
}
```

**Java 实现**

```java
import java.util.*;

class Graph {
    private int V; // 节点数
    private LinkedList<Integer> adj[]; // 邻接表

    // 构造函数
    Graph(int v) {
        V = v;
        adj = new LinkedList[v];
        for (int i = 0; i < v; ++i)
            adj[i] = new LinkedList();
    }

    // 添加边
    void addEdge(int v, int w) {
        adj[v].add(w);
    }

    // 广度优先搜索
    void BFS(int s) {
        boolean visited[] = new boolean[V]; // 标记节点是否访问过
        LinkedList<Integer> queue = new LinkedList<>();

        visited[s] = true;
        queue.add(s);

        while (queue.size() != 0) {
            s = queue.poll();
            System.out.print(s + " ");

            Iterator<Integer> i = adj[s].listIterator();
            while (i.hasNext()) {
                int n = i.next();
                if (!visited[n]) {
                    visited[n] = true;
                    queue.add(n);
                }
            }
        }
    }
}
```
**C++ 实现**

```C++
#include <iostream>
#include <list>
#include <queue>

using namespace std;

class Graph {
    int V; // 节点数
    list<int> *adj; // 邻接表

public:
    Graph(int V); // 构造函数
    void addEdge(int v, int w); // 添加边
    void BFS(int s); // 广度优先搜索
};

Graph::Graph(int V) {
    this->V = V;
    adj = new list<int>[V];
}

void Graph::addEdge(int v, int w) {
    adj[v].push_back(w);
}

void Graph::BFS(int s) {
    bool *visited = new bool[V];
    for (int i = 0; i < V; i++)
        visited[i] = false;

    queue<int> queue;
    visited[s] = true;
    queue.push(s);

    while (!queue.empty()) {
        s = queue.front();
        cout << s << " ";
        queue.pop();

        for (auto i = adj[s].begin(); i != adj[s].end(); ++i) {
            if (!visited[*i]) {
                visited[*i] = true;
                queue.push(*i);
            }
        }
    }
}
```

### 应用

1. 迷宫求解：BFS 可以帮助我们找到从起点到终点的最短路径，用于解决迷宫问题。

2. 社交网络分析：BFS 可以用于网络中的节点搜索和社交关系分析，发现网络中的关键人物或组织。

3. 游戏 AI：BFS 可以用于计算机游戏中的路径规划和敌人追踪，以及以智能方式移动角色。

4. 地图导航：BFS 可以帮助我们找到两个地点之间的最短路径，用于导航应用中的路线规划。

#### 最短路径

利用广度优先搜索（BFS）实现最短路径的查找是一种常见的应用场景，特别适用于无权图或权值相同的图。在广度优先搜索中，首次到达目标节点的路径长度即为最短路径长度。

##### 原理

最短路径查找算法基于广度优先搜索（BFS）。其核心思想是：

1. 使用 BFS 遍历图，从起始节点开始搜索。
2. 维护一个距离数组，记录每个节点到起始节点的最短距离。
3. 如果遍历到目标节点，即可得到从起始节点到目标节点的最短路径长度。

> 为了记录路径，我们需要维护一个 `parent` 数组，用于记录每个节点在 BFS 树中的父节点。这样我们可以从目标节点回溯到起始节点，重建出完整的路径。

##### 算法实现

**Java 实现**

```java
import java.util.*;

class Graph {
    private int V; // 节点数
    private List<List<Integer>> adj; // 邻接表

    // 构造函数
    public Graph(int V) {
        this.V = V;
        adj = new ArrayList<>(V);
        for (int i = 0; i < V; ++i)
            adj.add(new ArrayList<>());
    }

    // 添加边
    public void addEdge(int v, int w) {
        adj.get(v).add(w);
        adj.get(w).add(v); // 如果是无向图，需要添加反向边
    }

    // 使用 BFS 查找最短路径长度并获取完整路径
    public List<Integer> shortestPath(int src, int dest) {
        boolean[] visited = new boolean[V]; // 记录节点是否访问过
        int[] parent = new int[V]; // 记录节点的父节点
        Arrays.fill(parent, -1); // 初始化父节点为 -1 表示无父节点

        Queue<Integer> queue = new LinkedList<>();
        queue.add(src);
        visited[src] = true;

        while (!queue.isEmpty()) {
            int current = queue.poll();

            if (current == dest) {
                // 构建路径
                List<Integer> path = new ArrayList<>();
                int step = current;
                while (step != -1) {
                    path.add(step);
                    step = parent[step];
                }
                Collections.reverse(path);
                return path;
            }

            for (int neighbor : adj.get(current)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    parent[neighbor] = current;
                    queue.add(neighbor);
                }
            }
        }

        return new ArrayList<>(); // 如果未找到路径，返回空列表
    }

    public static void main(String[] args) {
        int V = 6; // 节点数
        Graph g = new Graph(V);

        // 添加边
        g.addEdge(0, 1);
        g.addEdge(0, 2);
        g.addEdge(1, 3);
        g.addEdge(2, 4);
        g.addEdge(3, 4);
        g.addEdge(3, 5);
        g.addEdge(4, 5);

        int src = 0; // 起始节点
        int dest = 5; // 目标节点

        List<Integer> shortestPath = g.shortestPath(src, dest);

        if (!shortestPath.isEmpty()) {
            System.out.println("最短路径: " + shortestPath);
          	// 路径长度为节点数减一
            System.out.println("最短路径长度: " + (shortestPath.size() - 1)); 
        } else {
            System.out.println("未找到路径");
        }
    }
}
```
