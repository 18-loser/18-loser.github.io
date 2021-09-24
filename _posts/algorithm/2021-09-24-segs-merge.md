---
layout: post
title: "区间合并"
date: 2021-09-24 22:49:22 UTC+8
category: algorithm
---
# 区间合并

![绘图1](https://gitee.com/shl1122/pic-bed/raw/master//img/202109242226444.png)

看以上这个区间图

数学里常用的一种思想方法，集合的方法，区间也同样是这个道理，区间合并只有三种情况：

1. 两区间相交
2. 两区间相离
3. 两区间呈包含关系

这就是区间合并的核心问题

- 如果相交或包含不改变左端点，右端点变为最大的；
- 如果相离上一个头和尾连续的区间就加入解集合里了；

## 代码实现[^1]

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;
typedef pair<int, int> PII;

vector<PII> segs;

void merge(vector<PII> &segs)
{
    vector<PII> res;
    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs) {
        if (seg.first > ed){
                if (st != -2e9) res.push_back({st, ed});
                st = seg.first, ed = seg.second;
        } else {
                ed = max(ed, seg.second);
        }
    }
    if (st != -2e9) res.push_back({st, ed});
    segs = res;
}

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int st, ed;
        cin >> st >> ed;
        segs.push_back({st, ed});
    }

    merge(segs);

    cout << segs.size() << endl;
    return 0;
}
```

[^1]: [ACWing 803. 区间合并](https://www.acwing.com/problem/content/805/)

