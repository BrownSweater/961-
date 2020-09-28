# 数据结构自定义类型

## 线性表

### 1、线性表的顺序存储

顺序表的特点是逻辑顺序与物理顺序相同

```c++
// 静态分配
typedef struct{
    ElemType data[MaxSize];
    int length;
}SqList;

// 动态分配
typedef struct{
    ElemType *data;
    int MaxSize, length;
} SeqList;
L.data = (ElemType *)malloc(sizeof(ElemType)* InitSize)
    
// 插入
bool ListInsert(SqList &L, int i, ElemType e){  //将e插入L的i位置
    if(i<1 || i >L.length+1){
        return false;
    }
    if L.length > L.MaxSize{
        return false;
    }
    for(int j=L.length; j>=i; j--){
        L.data[j] = L.data[j-1];
    }
    L.data[i-1] = e;
    L.length++;
    return true;
}

// 删除
bool ListDel(SqList &L, int i, ElemType &e){  //删除L中第i个元素
    if(i<1 || i> L.length){
        return false;
    }
    if(L.length==0){
        return false;
    }
    e = L.data[i-1]
    for(int j=i; j<L.length; j++){
        L.data[j-1] = L.data[j];
    }
    L.length--;
    return true;
}

//按值查找
int ListLoc(SqList L, ElemType e){
    for(int i=0; i<L.length; i++){
        if (L.data[0] == e){
            return i+1;
        }
    }
    return -1;
}

// 翻转
void Reverse(SqList &L){
    ElemType tmp;
    for(i=0; i<L.length/2;i++){
        tmp = L.data[i];
        L.data[i] = L.data[L.length-i];
        L.data[L.length-i]=tmp;
    }
}

// 删除顺序表中为所有值为x的元素
void Deleted_X(SqList L, ElemType x){
    int k=0;
    for(i=0;i<L.length;i++){
        if (L.data[i]==x){
            k++;
        }
        else{
            L.data[i-k] = L.data[i];
        }
    }
    L.length = L.length - k;
}


// 两个有序顺序表合成一个有序顺序表
SqList Contact_Two_SqList(SqList L1, SqList L2){
    // 如果有一个为空，直接返回另一个
    if (L1.length==0){
        return L2
    }
    if (L2.length==0){
        return L1    
    }
    int i=0;
    int j=0;
    // 初始化 SqList
    SqList L3;
    L3.data = (ElemType *)malloc(sizeof(int)*(L1.length+L2.length));
    L3.MaxSize = L1.length+L2.length;
    L3.length = 0;
    while(!(i==L1.length && j==L2.length)){
        if (L1.data[i]<L2.data[j]){
            L3.data[L3.length] = L1.data[i];
            L3.length+=1;
            if (i<L1.length){
                i++;
            }
        else{
            L3.data[L3.length] = L2.data[j];
            L3.length+=1;
            if (j<L2.length){
                j++;
            }
        }
    }
    return L3;      
}
    
// 两个有序顺序表合成一个有序顺序表， 王道书上的方法
bool Merge(SqList L1, SqList L2, SqList &L3){
    if((L1.length + L2.length)>L3.MaxSize){
        return false;
    }
    int i=0;
    int j=0;
    int k=0;
    while (i<L1.length && j<L2.length){
        if (L1.data[i] < L2.data[j]){
            L3.data[k]=L1.data[i]
            k++;
            i++;
        }
        else{
            L3.data[k]=L2.data[j];
            k++;
            j++;
        }
    }
    while (i<L1.length){
        L3.data[k]=L1.data[i];
        k++;
        i++
    }
    while (j<L2.length){
        L3.data[k]=L2.data[j];
        k++;
        j++;
    }
    L3.length = k;
    return true;
}
```

### 2、线性表的链式存储

头结点和头指针的区分：不管带不带头结点，头指针始终指向链表的第一个结点，头结点不是一个有效的结点，计算链表长度时不包括在内。

引入头结点（哨兵结点）的好处：

- 1、第一个数据结点的位置被存放在头结点的指针域中，因此在链表的第一个位置上的操作和在链表其他位置上的操作一致，无需特殊处理。
- 2、无论链表是否为空，其头指针都指向头结点的非空指针。因此空表和非空表的处理也一致。

##### 单链表

```c++
typedef struct LNode{
    ElemType data;    //数据域
    struct LNode *next; // 指针域
}LNode, *LinkList;

// 按序号查找链表的值
LNode *GetElem(LinkList L, int i){
    int j=1;
    LNode *p = L->next; //头结点
    if(i==0){
        return p; // 0返回头结点
    }
    if(i<1){
        return NULL; // 无效
    }
    while(p&&j<i){
        p=p->next;
        j++;
    }
    return p;
}

// 按值查找链表
LNode *LocLinkList(LinkList L, ElemType e){
    LNode *p = L->next; //头结点
    while(p&&p->next!=e){
        p=p->next;
    }
    return p;
}

// 插入，后插
bool Insert(&LinkList L, int i, ElemType e){
    LNode *p = GetElem(L, i-1);
    if(p){
        LNode tmp;
        tmp.data = e;
        tmp->next = p->next;
        p->next = tmp;
        return true;
    }
    else{
        return false;
    }
}

// 删除
bool Deleted(&LinkList L, int i){
    LNode *p = GetElem(L, i-1);
    LNode *tmp = p->next;
    p->next = tmp->next;
}

// 求表长, 头结点不算
int GetLength(LinkList L){
    LNode *p = L->next; //头结点
    int i=0;
    while(p->next!=NULL){
        i++;
    }
    return i;
}


// 带有头结点单链表，链表翻转1
void Traverse1(LinkList &L){
    LNode *p = L->next;  //头结点
    LNode *r = p->next; //第一个结点
    while (r!=NULL){
        p = L->next->next; // 第一个结点
        p->next = r;    
        r = r->next;
        p->next->next = 
        r= r->next;
    }
}

// 带有头结点单链表，链表翻转2
void Traverse1(LinkList &L){
    
}

// 单链表排序
void Sort(LinkList &L){
    
}

// 找两个单链表的重合部分
void Sort(LinkList &L1){
    
}
```

##### 双链表

```c++
// 数据结构
typedef struct DNode{
    ElemType data;
    struct DNode *prior, *next; //前驱指针、后继指针
}DNode, *DLinkList;

// 插入
bool Insert(DLinkList &L, ElemType e, int i){
    DNode *p=GetElem(L, i-1); // 找到插入位置i的前一个结点
    if (p!=NULL){
        DNode tmp;  // 中间缓存
        tmp.data = e;
        tmp->next=p->next;
        tmp->prior = p;
        p->next->prior = tmp;
        p->next = tmp;
        return true;
    }
    else{
        return false;
    }
}

// 删除
bool Deleted(DLinkList &L, int i){
    DNode *p=GetElem(L, i); // 删除位置的结点的指针
    if(p!=NULL){
        p->prior->next = p->next;
        p->next->prior = p->prior;
        free(p);
        return true;
    }
    else{
        return false;
    }
}
```

##### 循环链表

- 循环单链表：表中的最后一个结点的指针的值不为`NULL`，而是指向头结点，形成一个环。
  - 1、**没有指针域为`NULL`的结点。因此判断链表为空：`头结点的指针=头指针`**
  - 2、**可以不设置头指针而是设置尾指针，使得头尾操作效率更高。`r->next=头指针`**
- 循环双链表：头结点的`prior`指向尾节点
  - 1、判断链表为空：**头结点的`next`和`prior`都等于头指针。**

### 3、顺序表和链表对比

|            | 顺序表（访问多）               | 链表（插入、删除、长度不定） |
| ---------- | ------------------------------ | ---------------------------- |
| 按序号查找 | $O(1)$                         | $O(N)$                       |
| 按值查找   | 无序$O(N)$；有序$O(log_{2^n})$ | $O(N)$                       |
| 插入、删除 | $O(N)$，需要移动               | $O(N)$，主要是比较操作       |
| 空间分配   | 无法扩充                       | 动态扩充                     |

- 基于存储考虑：无法估计线性表的长度和规模时，用链表。
- 基于运算考虑：顺序表访问快；但是进行插入删除操作，链表优于线性表。
- 基于环境的考虑：顺序表简单，链表需要用指针相对复杂。

## 图

### 1、图的存储结构

#### 邻接矩阵

若涉及邻接矩阵的问题，从：边表、顶点表为出发点回答

```c++
#define MaxVertexNum 100 //顶点数目的最大值
typedef char VertexType;
typedef int EdgeType;
typedef struct {
    VertexType Vex[MaxVertexNum];  //邻接矩阵的顶点表
    EdgeType Edge[MaxVertexNum][MaxVertexNum]; // 邻接矩阵的边表
    int vexnum, arcnum; //顶点数和弧数
}MGraph;
```

#### 邻接表 

若涉及邻接表的问题，从：边表结点、顶点表结点为出发点回答。

``` c++
#define MaxVertexNum 100
typedef struct ArcNode{  //边表顶点
    int adjvex; // 该弧指向的顶点
    ArcNode *next; //下一个边表结点的指针
}
typedef struct VNode{ //顶点表结点
    VertexType data; 
    ArcNone *first; //指向的第一个边表结点
}VNode, AdjList[MaxVertexNum];
typedef struct{
    AdjList vertices; // 邻接表
    int vexnum, arcnum;
}ALGraph  //邻接表
```

#### 邻接表转换成邻接矩阵

```c++
// 将邻接表G1转换为邻接矩阵表G2
for(int i=0; i<G1.vexnum; ++i)
{	
    p = G1.vertices[i].first;
    while (p)
    {	
        G2.Edge[i][p->adjvex] = 1
        p = p->next
    }
}
G2.vexnum=G1.vexnum
G2.arcnum=G1.vexnum
```

#### 对比

|                  | 邻接矩阵         | 邻接表                           |
| ---------------- | ---------------- | -------------------------------- |
| 适合存储的场景   | 稠密图           | 稀疏图                           |
| 顶点有多少邻边   | 慢               | 快                               |
| 顶点间是否有邻边 | 快               | 慢                               |
| 出度             | 慢、访问整行元素 | 快、顶点表结点对应的边表结点个数 |
| 入度             | 快，访问整列元素 | 慢、访问整张表                   |





### 2、图的遍历方法

#### 广度优先搜索（BFS）

类似于二叉树的层次遍历算法。类似于剥洋葱，每一个连通分量都是一个洋葱，用`BFSTraverse`找洋葱、`BFS`来剥洋葱。

- 空间复杂度：最坏情况下为$O(V)$
- 时间复杂度：
  - 邻接矩阵：$O(V^2)$
  - 邻接表：$O(V+E)$

```c++
bool visited[MaxVertexNum] //标记结点是否访问过
void BFSTraverse(Graph G){
    for(i=0; i<G.vexnum; ++i){
        visited[i] = false; // 初始化状态数组
    }
    InitQueue(Q); //初始化辅助队列
    for(i=0; i<G.vexnum; ++i){
        if (!visited[i]){
            BFS(G, i);  // 对于每个连通分量使用BFS
        }
    }
}
void BFS(Graph G, int v){
    visit(G, v); // 直接访问V
    visited[v] = true; //标记为已经访问
    EnQueue(Q, v); // 将结点入队
    while (!IsEmpty(Q)){  // 逐层遍历，每一次while循环表示一层
        DeQueue(Q, v); // 拿出一个结点
        for(w=FirstNerghbor(G,v); w>=0; w=NextNeighbor(G, v, w)){ // 遍历和v结点连接的所有结点
            if (!visited[w]){
                visit(w);
                visited[w] = true;
                EnQueue(Q, w);
            }
        }
    }
}
```

##### BFS求解单源最短路径

无权图顶点u到顶点v的最短路径`d(u,v)`最少的边数。

```c++
void BFSMinDisdance(Graph G, int u){
    int d[MaxVertexNum]; // 距离数组
    bool visited[MaxVertexNum]; // 状态数组
    for(i=0; i<G.vexnum; ++i){
        d[i] = ∞;
        visited[i]=false;
    }
    d[u]= 0;
    InitQueue(Q);
    EnQueue(Q, u);
    while (!IsEmpty(Q)){
        DeQueue(Q, u);
        for(w=FirstNerghbor(G,v); w>=0; w=NextNeighbor(G, v, w)){ //w是结点的编号
            if (!visited(w)){
                visited[w]=true;
                d[w]=d[u]+1;
                EnQueue(Q, w);
            }
        }
    }
}
```

##### 广度优先生成树

类似于二叉树的先序遍历算法。在BFS的过程中，可以得到一个遍历树，称为广度优先生成树。

#### 深度优先搜索（DFS）	

- 空间复杂度：最坏情况下为$O(V)$
- 时间复杂度：
  - 邻接矩阵：$O(V^2)$
  - 邻接表：$O(V+E)$

```c++
bool visited[MaxVertexNum];
void DFSTraverse(Graph G){
    for(i=0; i<G.vexnum; ++i){
        visited[i]=false;
    }
    for(i=0; i<G.vexnum; ++i){
        if !visited[i]{
            DFS(G, i);
        }
    }
}
void DFS(Graph G, int V){
    visit(v);
    visited[i]=true;
    for(w=FirstNerghbor(G,v); w>=0; w=NextNeighbor(G, v, w)){
         if (!visited(w)){
              DFS(G, i);
         }
    }
}
```

### 3、图的应用

#### 最小生成树

最小生成树是对于连通图而言。

- 当图中各边权值不相等时，最小生成树唯一。
- 最小生成树的边的权值之和总是唯一的。
- E = V-1，边数等于顶点数减一。

##### Prim算法

**通过顶点来生成最小生成树**，基本步骤：

1、将空树T中添加图中任一顶点u

2、循环从图中挑选出不属于T，对于T中顶点具有最小权值的边的顶点，加入T

3、时间复杂度：$O(V^2)$，适合求解稠密图



##### Kruskal算法

**通过边来生成最小生成树**，基本步骤：

1、假设每个顶点构成一颗独立的树

2、循环，按照G的边的权值递增顺序依次选择一条边，若这条边加入T后不构成回路，则加入否则舍弃，直到T中含有n-1条边

3、采用堆来存放边的集合，因此每次选择最小权值的边只需要$O(log{E})$的时

4、时间复杂度：$O(Elog{E})$，适合求解边稀疏而顶点较多的图



#### 最短路径

带权路径长度最短的路径称为最短路径。解决带权有向图的最短路径问题主要有两个方法：

##### Dijkstra（迪杰斯特拉）：单源最短路径

- 时间复杂度：$O(V^2)$，无论邻接表还是邻接矩阵时间复杂度相同，因为选择最小分量的时间不变。
- 求解源点到指定顶点的时间复杂度和其他所有点的时间复杂度相同
- 边上带有负权值时，方法不适用



##### Floyd（弗洛伊德）：每对顶点间的最短路径

- 时间复杂度：$O(V^3)$
- 允许图中有带负权值的边，但不允许包含带有负权值的边组成的回路



