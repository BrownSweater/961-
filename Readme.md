# 961数据结构

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
void Traverse1(LinkList L){
    LNode *p = L->next;  //头结点
    p->next = NULL;
    while (p!=NULL){
        r = p->next; //保存后继结点
        p->next = L->next; //插入的结点p指向链表中的第一个结点
        L->next = p; // 令p为链表的第一个结点    
        p = r; // 待插入结点p后移一位
    }
}

// 带有头结点单链表，链表翻转2
void Traverse2(LinkList L){
  LNode *pre, *p, *r;
  p = L->next; // 待插入的结点
  r = p->next;  // 待插入结点的下一个结点
  p->next = NULL; // 初始时，待插入结点为第一个结点，因此前面无结点
  while (p!=NULL){
      pre = p;
      p = r;
      r= r->next;
      p->next = pre;
  }
    L-next=p
    return L
}

// 单链表排序 O(n^2)
void Sort(LinkList &L){
    LNode *p = L->next->next; // 待排序结点，从第二个结点开始
  	L->next->next = NULL;
  	while (p!=NULL){
      	LNode *pre = L;
      	LNode *r = pre->next;
    		while(r!=NULL){
        		if (r->data > p->data){ //插入
            		p->next=r;
              	pre->next=p;
            }
        r->next = p; // 大于链表所有值插入到末尾
        p = p->next; // p向后移一位
        r->next->next = NULL; 
        }
    }
}

// 找两个单链表的重合部分
void GetSame(LinkList L1, LinkList L2){
    int L1Length =0;
  	int L2Length =0;
  	// 分别求两个单链表的长度
  	LNode *p = L1->next; // 首结点
  	while(p!=NULL){
      	L1Length++;
    }
    p = L2->next; // 首结点
  	while(p!=NULL){
      	L2Length++;
    }
  
    
// 链表归并
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



## 树

### 1、二叉树的顺序存储和链式存储

#### 顺序存储

适合完全二叉树和满二叉树，树中的结点序号可以反映结点之间的逻辑关系。

#### 链式存储

含有n个结点的二叉链表中，有n+1个空链域。

```c++
typedef struct BiTNode{
    ElemType data;
    BiTNode *lchild, *rchild;
}BiTNode, *BiTree;
```



### 2、二叉树的遍历

- 二叉树的先序和中序可以确定一棵树
- 二叉树的中序和后序可以确定一棵树
- 二叉树的层次和中序可以确定一棵树

#### 前序遍历

```c++
// 递归
void PreOrder(BiTree T){
    if (T!=NULL){
        visit(T);
        PreOrder(T->lchild);
        PreOrder(T->rchild);
    }
}

// 非递归
void PreOrder2(BiTree T){
    BiTNode *p;
    p = T;
    InitStack(S); // 初始化一个辅助栈
    while (p || !IsEmpty(S)){
        if (p!=NULL){
            visit(p);
            Push(S, p);
            p = p->lchild;
        }
        else{
            Pop(S, p);
            p = p->rchild;
        }
    }
}
```



#### 中序遍历

```c++
// 递归实现
void InOrder(BiTree T){
    while (T!=NULL){
        InOrder(T->lchild);
        visit(T);
        InOrder(T->rchild);
    }
}

// 非递归实现
void InOrder2(BiTree T){
    BiTNode *p = T;
    InitStack(S);
    while (p || !IsEmpty(S)){ // p不空或者栈不空时循环
        if (p!=null){
            Push(S, p);  // p不为空一直访问左子树
            p = p->next; 
        }
        else{
            Pop(S, p); // p空就出栈访问结点
            visit(p);
            p=p->rchild;
        }
    }
}
```



#### 后序遍历

```c++
// 递归
void PostOrder(BiTree T){
    while (T!=null){
        PostOrder(T->lchild);
        PostOrder(T->rchild);
        visit(T);
    }
}

// 非递归
void PostOrder(BiTree T){
    BiTNode *r, *p=T;
    InitStack(S);
    while (p || !IsEmpty(S)){
        if (p){
            Push(S, p); // p不空就入栈
            p = p->next; //往左走
        }
        else{
            GetTop(S, p); // 拿出最左边的结点
            if (p->rchild && p->rchild!=r){ // 右结点没被访问过
                p = p->rchild;
                Push(S, p); //右结点入栈
                p = p->lchild; // 往左边走
            }
            else{
                Pop(S, p);  
                visit(p);  //访问结点
                r=p;  // 标记此结点已经访问过
                p=null; // 后序遍历只要访问了的结点就是局部子树的根节点，没有左右结点了
                 
            }
        }
    }
}
```



#### 层次遍历

```c++
// 借助辅助队列进行遍历
void LevelOrder(BiTree T){
    BiTNode *p = T;
    InitQueue(Q);
    EnQueue(Q, s); // 队列里的元素才能访问
    while (p || !IsEmpty(Q)){
        DeQueue(Q, p);
        visit(p);
        if (p->lchild!=null){  // 不空才能入队，因为队列里的所有元素都需要访问
            EnQueue(Q, p-lchild);
        }
	    if (p->rchild!=null){
            EnQueue(Q, p->rchild);
        } 
    }
}
```



### 3、和二叉树遍历相关的题目

- 先序遍历和后序遍历正好相反的树的形态是一层只有一个结点
- 先序遍历和后序遍历正好相同的树的形态是只有一个根节点

```c++
// 非递归算法求树的高度
// 使用层次遍历，记录下每一层最右边的结点，通过一个变量是否等于最右边结点来判断是否遍历完了一层
int GetTreeHeight(BiTNode T){
    InitQueue(Q);
    BiTNode *p = T, last; // last是每层最右边的结点
    last = p;
    int level =0; // 树的高度
    EnQueue(Q, p);
    while (p!=null || !IsEmpty(Q)){// p不空或者队列不空进入循环
        DeQueue(Q, p);
        if (p->lchild!=null){ //左子树不空入队
            EnQueue(Q, p->lchild);
        }
        if (p->rchild!null){ //右子数不空入队
            EnQueue(Q, p->rchild);
        }
        if (last == p){ //如果p=last，最右边的结点，表示该层已经全部遍历完
            level++;
            if (!IsEmpty(Q)){
                GetBottom(Q, last) // 如果队列不为空，更新last结点为队列底部的元素
            }
        }
    }
    return level
}

// 递归算法求树的高度（可扩展到求最大宽度，每层节点数)
int GetTreeHeight(BiTree T){
    int level=0;
    if (T!null){
        level++;
    }
    lh = GetTreeHeight(T->lchild);
    rh = GetTreeHeight(T->rchild);
    if (lh > rh){
        level += lh;
    }
    else{
        level += rh;
    }
    return level;
}


// 求二叉树所有双分支结点个数
int TwoChildNodeNum(BiTree T){
    num = 0;
    if (T!=null){
        if (T-lchild!=null && T->rchild!=null){
            num++;
        }
        num += TwoChildNodeNum(T->lchild);
        num += TwoChildNodeNum(T->rchild);
    {
    return num;
}

// 根据先序和中序构建二叉树, 先序遍历确定根节点，中序遍历切分左右子树


// 判断是否是完全二叉树，层次遍历，当一个结点为空后，是否还有元素进队
bool IsComplete(BiTree T){
    BiTNode *p = T;
    InitQueue(Q);
    EnQueue(Q, p);
    flag = true // 设置一个状态，如果一个结点的子树为空，就设置为false，如果后续还有结点入队，就不是完全树
    if (p==null){  // 空树
        return false
    }
    while (!IsEmpty(Q)){
        DeQueue(Q, p);
        if (p->lchild!null){
            if (!flag){
                return false;
            }
            EnQueue(Q, p);
        }
        else{
            if (flag){
                flag = false;
            }
        }
        if (p->rchild!=null){
            if (!flag){
                return false;
            }
            EnQueue(Q, p);
        }
        else{
            if (flag){
                flag = false;
            }
        }
    }
    return true;
}

        
// 求非空二叉树的宽度
        
// 交换左右子树
        
```

### 4、树

#### 存储结构

- 双亲表示法：`data`和`parent`两个数组，分别存放结点的值和双亲结点的序号，根节点为-1。
- 孩子表示法：所有结点存在一个数组中，数组的元素是一个链表，存放该结点的孩子结点在数组中的序号。
- **孩子兄弟表示法（二叉树表示法）**：用二叉链表进行存储，每个结点由三部分构成。左孩子右兄弟。
  - 结点值
  - 指向结点第一个孩子结点的指针
  - 指向结点下一个兄弟结点的指针

```c++
// 孩子兄弟表示法的存储结构
typedef struct CSNode{
    ElemType data;
    struct CSNode *firstchild, *nextsibling;
}CSNode, *CSTree
```

#### 树和二叉树的转换

左孩子右兄弟。



### 5、二叉排序树（二叉搜索树）

#### 定义

中序遍历是一个递增的序列

- 如果左子树非空，则所有左子树上的值小于根节点的值
- 如果右子树非空，则所有右子树上的值大于根节点的值
- 左右子树均为二叉排序树

#### 查找

```c++
BSTNode *BST_Search(BSTree T, ElemType key){
    BSTNode *p = T;
    while (p!null || key=!p->data){
        if (key>p.data){
            p = p->rchild;
        }
        else{
            p = p->lchild;
        }
    }
    if (p.data != key){ //查找不成功
        return null;
    }
    return p;
}
```

#### 插入

插入的结点一定是一个新添加的叶子结点。

```c++
// 插入
void BST_Insert()
```



#### 删除

删除结点z：

- 1、z为叶子结点直接删除
- 2、如果z只有一棵子树，z的子树代替z为z父结点的子树
- 3、如果z有左右子树，中序第一个子女代替

#### 查找效率分析

查找效率主要取决于树的高度。

- 查找过程和二分查找类似，但是二分查找的判定唯一，而二叉排序树的查找不唯一，相同的关键字插入顺序不同生成的二叉排序树不同。
- **当有序表是静态查找表时，选择顺序存储作为其存储结构；使用二分查找实现其查找操作；当有序表是动态查找表时，选择二叉排序树作为其逻辑存储结构。**
- **二叉排序树的插入和删除只需要修改指针，平均执行时间$O(nlog_{2^n})$；二分查找需要移动结点，代价是$O(n)$**



### 6、平衡二叉树



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







# 历年考点

|      | 线性表                                  | 树                       | 排序         | 查找       | 图               |
| ---- | --------------------------------------- | ------------------------ | ------------ | ---------- | ---------------- |
| 2017 | 向量和数组的优缺点对比                  | 遍历二叉树叶子结点       | 堆排序       | 二叉排序树 |                  |
| 2018 | 栈用单链表和数组哪个好；实现栈的pop函数 | 哈夫曼树                 | 希尔排序     | 哈希冲突   |                  |
| 2019 | 循环单链表                              | 统计有两个非空子树的结点 | 二叉堆；快排 |            | Dijkstra算法填空 |
| 2020 |                                         | 统计有一个非空子树的结点 | 快排；快排   | 二叉搜索树 | floyd            |
|      |                                         |                          |              |            |                  |



