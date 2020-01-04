---
title: "LeetCode.547 朋友圈"
date: 2020-01-04T20:18:21+08:00
draft: false
categories: ["刷题记录"]
---

班上有 N 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 N * N 的矩阵 M，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个学生互为朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。

**示例 1:**

输入: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]  

输出: 2 

说明：已知学生0和学生1互为朋友，他们在一个朋友圈。
第2个学生自己在一个朋友圈。所以返回2。

**示例 2:**

输入: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]

输出: 1

说明：已知学生0和学生1互为朋友，学生1和学生2互为朋友，所以学生0和学生2也是朋友，所以他们三个在一个朋友圈，返回1。

注意：

N 在[1,200]的范围内。
对于所有学生，有M[i][i] = 1。
如果有M[i][j] = 1，则有M[j][i] = 1。

### 并查集解法

#### 思路：
1. 遍历下三角，因为是对称矩阵。  
2. 若i，j是朋友，则看看他俩是不是一个圈子，若不是则合并。
3. 初始化Set数组各元素值均为自身，即Set[i]=i; 经过一番合并后，遍历Set，Set[i]=i的个数即为朋友圈数。

```c
执行用时 :32 ms, 在所有 C 提交中击败了92.68%的用户  
内存消耗 :8.3 MB, 在所有 C 提交中击败了100.00%的用户  

int Find(int set[], int x) {
    for(;set[x]!=x;x=set[x]);
    return x;
}

执行用时 :28 ms, 在所有 C 提交中击败了99.29%的用户
内存消耗 :8.2 MB, 在所有 C 提交中击败了100.00%的用户

// 路径压缩
int Find2(int set[], int x) {
    if(set[x] == x)
        return x;
    else return set[x] = Find(set, set[x]);
}

// 按秩归并
void Union(int* set, int* size, int r1, int r2) {
    if(size[r1] > size[r2]) {
        set[r2] = r1;
        size[r1] += size[r2];
    } else {
        set[r1] = r2;
        size[r2] += size[r1];
    }
}

int findCircleNum(int** M, int MSize, int* MColSize){
    int set[MSize], size[MSize], i, j, count=0, root1, root2;
    // 初始化并查集
    for(i=0;i<MSize;i++) {
        set[i]=i;
        size[i] = 1;
    }
    for(i=1;i<MSize;i++) {
        for(j=0;j<MSize && j<i;j++) {
            // 只遍历下三角
            if(M[i][j] == 1) {
                // 看看他俩分别是哪个圈子的
                root1 = Find2(set, i);
                root2 = Find2(set, j);
                // 圈子不同，合并！
                if(root1 != root2) {
                    Union(&set, &size, root1, root2);
                }
            }
        }
    }
    for(i=0;i<MSize;i++) {
        if(set[i] == i) count++;
    }
    return count;
}
```
