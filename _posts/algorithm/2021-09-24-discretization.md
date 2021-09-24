---
layout: post
title: "离散化"
date: 2021-09-24 22:49:22 UTC+8
category: Algorithm
---
# 离散化

## 数组离散化

当数组存值的时候，数据都非常的离散，这样非常浪费空间，我们想办法节省空间，且不失一般性。

1. 先把所有数位置存入数组中（离散化）
2. 排序去重
3. 再把所有数也对应位置数组的下标存个数组
4. 前缀和处理数数组
5. 查询区间和之间要先用二分查这个数离散化后的位置

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;
const int N = 300010;

int n, m;
int a[N], s[N];
vector<int> alls; // 所有x的位置
vector<PII> add, query;

int find(int x)
{
    int l = -1, r = alls.size();
    while (l + 1 != r)
    {
        int mid = l + r >> 1;
        if (alls[mid] <= x)
        {
            l = mid;
        } else {
            r = mid;
        }
    }
    return l + 1; // 因为要preSum，所以下标从1开始。
}

int main()
{
    cin >> n >> m;
    while (n--)
    {
        int x, c;
        cin >> x >> c;
        add.push_back({x,c});
        
        alls.push_back(x);
    }
    
    while (m--)
    {
        int l, r;
        cin >> l >> r;
        query.push_back({l, r});
        // 查询区间两端的位置也要映射到离散数组里
        alls.push_back(l);
        alls.push_back(r);
    }
    // 对vector经典的去重操作
    // 先排序 −109≤x≤109,
    sort(alls.begin(), alls.end());
    // 去重 删除迭代器中的元素 erase()
    // 去重后容器中不重复序列+原序列 unique()
    alls.erase(unique(alls.begin(), alls.end()), alls.end());
    
    for (auto item : add)
    {
        int x = find(item.first); // 找到离散后的位置下标从1开始
        a[x] += item.second;
    }
    
    // 预处理preSum
    for (int i = 1; i <= alls.size(); i++)
    {
        s[i] = s[i - 1] + a[i];
    }
    
    for (auto item : query)
    {
        int l = find(item.first), r = find(item.second);
        cout << s[r] - s[l - 1] << endl;
    }
    return 0;
}
```