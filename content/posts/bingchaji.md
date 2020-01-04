---
title: "并查集"
date: 2020-01-04T14:56:19+08:00
draft: false
categories: ['数据结构与算法']
---

> 并查集是一个用树结构表示的集合，来处理一些不相交集的合并及查询问题。
其实并查集就像朋友圈，我们之前都是一个个的个体，后来因为某些共同兴趣而走到了一起，建立了一个小圈子。随着志同道合的人越来越多，这个圈子也越来越大，我们也完全可以通过两个人将两个圈子融合到一起。

并查集的主要操作有两个：查询某个元素属于哪个集合，合并两个集合。

### 并查集的表示

并查集是一个树结构，根结点用来代表集合，集合中的其他元素都指向根结点。即我们每个朋友圈都有个代表人物，我们只要报出各自的代表人物的名字，就可以知道我们是不是同一个圈子的人。

```c
#define MAXSIZE 10
typedef int SetName;
typedef int Set[MAXSIZE];   // Set数组里面有好多集合
// Set[i] 表示 i 的所属圈子的大佬
```

### 初始化朋友圈

```c
void Init() {
    int i;
    for(i=0;i<MAXSIZE;i++)
        Set[i] = i; // 一开始，每个人的圈子只有自己
}
```

### 你哪个圈子的？lo娘?！

```c
void Find(Set s[], int x) {
    // 直到s[x]=x时循环结束
    for(;s[x]!=x;x=s[x]);
    return x;   // 返回圈内大佬
}
```

### 老妹儿啊~咱俩挺投机滴，拉我进圈儿呗~

```c
void Union(Set s[], int a, int b) {
    int dalao1 = Find(s, a);
    int dalao2 = Find(s, b);
    // 若他们的大佬是同一个人，那他俩就属于同一个圈子
    if(dalao1 == dalao2) return;
    // 否则，将a的圈子并入b的圈子
    s[dalao1] = dalao2; // dalao1 认 dalao2 为大佬
    // 也可以反过来
}
```

**dalao1：“我TM不服！为什么我要认他做大哥！”**

那好，你们谁手下多谁当老大好吧......

```c
// 于是我们增加了一个数组size，用来存储各个圈子的大小。
typedef int Size[MAXSIZE];
// 初始化时 Size[i] = 1;
void Union(Set s[], int a, int b) {
    int dalao1 = Find(s, a);
    int dalao2 = Find(s, b);
    if(dalao1 == dalao2) return;
    if(Size[dalao1] > Size[dalao2]) {
        Size[dalao1] += Size[dalao2];
        Set[dalao2] = dalao1;
    } else {
        Size[dalao2] += Size[dalao1];
        Set[dalao1] = dalao2;
    }
}
```

**Q：**  上面为什么要用大小来决定哪个集合合并到哪个集合呢？  
**A：**  因为集合是个树结构，树是有高度的，而且这个高度和Find的效率息息相关。若一直是高树合并到矮树，那么这个树会越来越高，极端情况就是这个集合是一个链表，Find的时间复杂度会达到O(n)。而我们将矮树合并到高树，整个集合树的高度并不会增加，不会给Find带来影响。

### 按秩归并

按秩归并就是上面那个思想，按秩归并有两种方法。

1、按规模（见朋友圈合并）  
2、按树高

```c
// size数组变成height数组，存储各元素所在集合的高度
void Union(Set s[], int a,int b) {
    int dalao1 = Find(s, a);
    int dalao2 = Find(s, b);
    if(dalao1 == dalao2) return;
    if(height[dalao1] > height[dalao2]) {
        Set[dalao2] = dalao1;
    } else {
        if(height[dalao1] == height[dalao2]) height[dalao2]++;
        Set[dalao1] = dalao2;
    }
}
```

### 路径压缩

若我们每次都查询Set集合的最后一个元素的圈内大佬，查了n次，那最坏情况下时间复杂度为O(n^2)。  
所以我们需要压缩一下，是集合树尽量扁平化。

```c
void Find(Set s[], int x) {
    if(s[x] == x) {
        return x;
    } else {
        return s[x] = Find(s, s[x]);
    }
}
// 这样，当我们一层层查到了x的圈内大佬是谁，  
// 就将这一查找路径上的所有人的上级直接指向圈内大佬（即都是大佬的直属小弟了），  
// 那么下一次查找时就不用一层层的找了，节省了时间。
```

![](/img/posts/bcj2.png)

---

小结：并查集有两种不同的表示形式

```c
// 基本形式（结构体数组）
typedef struct Node {
    ElemType data;
    int parent;
}Set[MAXSIZE];

// 精简形式（普通数组）
typedef int Set[MAXSIZE];
// 集合中n个元素可以用下标0~n-1表示(即第一个元素为0，该元素所属圈子的大佬的下标为Set[0])。

/*
   还有一种方式是 Set[根下标] 不再为根下标本身，而是负的树高或负的集合大小。
*/
```

当然，这两种形式的Find和Union的实现方式也不一样。

![](/img/posts/bcj.png)
