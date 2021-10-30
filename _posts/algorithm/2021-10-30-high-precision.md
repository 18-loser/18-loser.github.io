---
layout: post
title: "高精度四则运算"
date: 2021-10-30 10:35:58 UTC+8
category: Algorithm
---

# 高精度四则运算

## 高精度加法

### 算法思路

正常模拟加法的过程，注意的话就是最后不要忘记要加上进位。

### 代码实现[^1]

```c++
#include <bits/stdc++.h>

using namespace std;

vector<int> add(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size() || i < B.size(); i++)
    {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) C.push_back(1);
    return C;
}

int main()
{
    string a, b;
    vector<int> A, B;

    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i--)
    {
        A.push_back(a[i] - '0');
    }
    for (int i = b.size() - 1; i >= 0; i--)
    {
        B.push_back(b[i] - '0');
    }

    auto C = add(A, B);

    for (int i = C.size() - 1; i >= 0; i--)
    {
        printf("%d", C[i]);
    }
    return 0;
}
```

[^1]: [ACWing 791. 高精度加法](https://www.acwing.com/problem/content/793/)

## 高精度减法

### 算法思路

因为结果可能存在负数，所以我们利于

$$
a-b=-(b-a)
$$

来解决负数的情况

### 代码实现[^2]

```c++
#include <bits/stdc++.h>

using namespace std;

bool cmp(vector<int> &A, vector<int> &B)
{
    if (A.size() != B.size())
        return A.size() > B.size();
    for (int i = A.size() - 1; i >= 0; i--)
    {
        if (A[i] != B[i])
        {
            return A[i] > B[i];
        }
    }
    return true;
}

vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i++)
    {
        t = A[i] - t; // 先减借位
        if (i < B.size())
        {
            t -= B[i]; // 位相减
        }
        C.push_back((t + 10) % 10); // 减不了就补1
        t = t < 0 ? 1 : 0;          // 确定借位是多少
    }
    // 去前导0
    while (C.size() > 1 && C.back() == 0)
        C.pop_back();

    return C;
}

int main()
{
    string a, b;
    vector<int> A, B;

    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i--)
    {
        A.push_back(a[i] - '0');
    }
    for (int i = b.size() - 1; i >= 0; i--)
    {
        B.push_back(b[i] - '0');
    }

    if (cmp(A, B))
    {
        auto C = sub(A, B);
        for (int i = C.size() - 1; i >= 0; i--)
        {
            printf("%d", C[i]);
        }
    }
    else
    {
        auto C = sub(B, A);
        printf("-");
        for (int i = C.size() - 1; i >= 0; i--)
        {
            printf("%d", C[i]);
        }
    }
    return 0;
}
```

[^2]: [ACWing 792. 高精度减法](https://www.acwing.com/problem/content/794/)

## 高精度乘法

### 算法思路

模拟乘法的竖列式

### 代码实现[^3]

```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size() || t; i++)
    {
        if (i < A.size()) t = A[i] * b + t;
        C.push_back(t % 10);
        t /= 10;
    }
    // 去前导0
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main()
{
    string a;
    int b;
    vector<int> A;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i--)
    {
        A.push_back(a[i] - '0');
    }
    vector<int> C = mul(A, b);
    for (int i = C.size() - 1; i >= 0; i--)
    {
        printf("%d", C[i]);
    }
    return 0;
}
```

[^3]: [ACWing 793. 高精度乘法](https://www.acwing.com/problem/content/795/)

## 高精度除法

### 算法思路

模拟除法竖列式

### 代码实现[^4]

```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> div(vector<int> A, int b, int r)
{
    vector<int> C;
    for (int i = A.size() - 1; i >= 0; i--)
    {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    // 去前导0
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main()
{
    string a;
    int b;
    vector<int> A;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i--)
    {
        A.push_back(a[i] - '0');
    }
    int r = 0; // 余数
    vector<int> C = div(A, b, r);
    
    for (int i = C.size() - 1; i >= 0; i--)
    {
        printf("%d", C[i]);
    }
    return 0;
}
```

[^4]: [ACWing 794. 高精度除法](https://www.acwing.com/problem/content/796/)

