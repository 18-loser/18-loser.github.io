---
layout: post
title: "binary sort tree"
date: 2021-08-11 20:47:30 UTC+8
category: Algorithm
---

# 二叉排序树

## 1.1 二叉排序树的定义

中序遍历有序的二叉树为二叉排序树（简称BST）

定义一个二叉树：

```c++
struct TreeNode
{
    int val;
    TreeNode *left, *right;
    TreeNode(int _val): val(_val), left(NULL), right(NULL) {}
}*root;
```

支持的操作：插入、删除、查找

三个操作都是`O(h)`

## 2.1 二叉排序树的操作

### 2.1.1 二叉排序树的插入操作

每次插入操作必然都是往树的叶结点插入新的节点，所以只要找到待插入结点的位置插入就成功完成二叉排序树的插入操作。

这是插入操作的具体代码

```c++
void insert(TreeNode* &root, int x)
{
    /*
        1. 递归找到x的待插入的位置
        2. 如果x < 当前root就递归到左子树，反之，递归到右子树。
    */ 
    if (!root) root = new TreeNode(x);
    // 如果发现数值相同的话就判断一下;
    if (root->val == x) return;
    else if (x < root->val) insert(root->left, X);
    else insert(root->right, x);
}
```

### 2.1.2 二叉排序树的删除操作

每次删除操作一定存在三种情况：

1. 待删除的结点只有左子树。立即可推出，左子树上的结点都是小于待删除的结点的，我们只需要把待删除结点删了然后左子树接上待删除结点的父节点就可以了。
2. 待删除的结点只有右子树。立即可推出，右子树上的结点都是大于待删除的结点的，我们只需要把待删除结点删了然后右子树接上待删除结点的父节点就可以了。
3. 待删除的结点既有左子树又有右子树。这个比上两个情况麻烦一点，但也不麻烦，需要读者理解的是，怎么能删除结点后还不改变中序遍历的结果，并且操作代价最小，显而易见，我们根据待删除结点的左子树可以得到最右下角的最后结点满足$<x$并且最接近x的结点，把这个结点覆盖待删除的结点，然后把这个结点删除，就完成了我们的操作。

删除操作的具体代码

```c++
void remove(TreeNode* &root, int x)
{
    // 如果不存在直接return
    if (!root) return;
    // 递归找到待删除的root
    if (x < root->val) remove(root->left, x);
    else if (x > root->val) remove(root->right, x);
    else
    {
        // 四种情况
        // 1.左右都为NULL 则直接删除
        // 2.左为空，删除结点，父连接子
        // 3.右为空，删除结点，父连接子
        // 4.左右都不为空，找到左子树离x最近的结点(大小/左子树最右下)然后拿来覆盖root，然后删除这个左子树离x最近的结点
        if (!root->left && !root->right) root = NULL;
        else if (!root->left) root = root->right;
        else if (!root->right) root = root->left;
        else
        {
            auto p = root->left;
            while (p->right) p = p->right;
            root->val = p->val;
            remove(root->left, p->val);
        }
    }
}
```

### 2.1.3 二叉排序树的查找操作

查找操作非常的简单，类似二分，递归查询。

我们来看这样两个高级查找

1. 输出数值 xx 的前驱(前驱定义为现有所有数中小于 xx 的最大的数)。
2. 输出数值 xx 的后继(后继定义为现有所有数中大于 xx 的最小的数)。

这是结点的前驱后继，是不是更高级了一点。

找x的前驱前提条件不为空，如果是空的，则特判一下就ok，我们递归找找到第一个$ <x $的结点然后递归右子树求小于$ x $的最大的数，因为x的前驱一定是x的左子树的最右下角那个结点。

找x的后继同理，反过来。

查找x的前驱后继的代码实现：
```c++
// INF我定义为了2e9 可以定义为0x3f3f3f3f,感兴趣可以搜一下为什么要这样定义
const int INF = 2e9;

// 输出数值 x 的前驱(前驱定义为现有所有数中小于 x 的最大的数)。
int get_pre(TreeNode* root, int x)
{
    if (!root) return -INF;
    if (root->val >= x) return get_pre(root->left, x);
    else return max(root->val, get_pre(root->right, x));
}

// 输出数值 x 的后继(后继定义为现有所有数中大于 x 的最小的数)。
int get_suc(TreeNode* root, int x)
{
    if (!root) return INF;
    if (root->val <= x) return get_suc(root->right, x);
    else return min(root->val, get_suc(root->left, x));
}
```

题解代码

```c++
#include <bits/stdc++.h>

using namespace std;

const int INF = 2e9;

struct TreeNode
{
    int val;
    TreeNode *left, *right;
    TreeNode(int _val): val(_val), left(NULL), right(NULL) {}
}*root;

// 插入操作
void insert(TreeNode* &root, int x)
{
    /*
        1. 递归找到x的待插入的位置
        2. 如果x < 当前root就递归到左子树，反之，递归到右子树。
    */ 
    if (!root) root = new TreeNode(x);
    // 如果发现数值相同的话就判断一下;
    if (root->val == x) return;
    else if (x < root->val) insert(root->left, x);
    else insert(root->right, x);
}

void remove(TreeNode* &root, int x)
{
    /*
        1. 待删除的结点只有左子树。立即可推出，左子树上的结点都是小于待删除的结点的，我们只需要把待删除结点删了然后左子树接上待删除结点的父节点就可以了。
        2. 待删除的结点只有右子树。立即可推出，右子树上的结点都是大于待删除的结点的，我们只需要把待删除结点删了然后右子树接上待删除结点的父节点就可以了。
        3. 待删除的结点既有左子树又有右子树。这个比上两个情况麻烦一点，但也不麻烦，需要读者理解的是，怎么能删除结点后还不改变中序遍历的结果，并且操作代价最小，显而易见，我们根据待删除结点的左子树可以得到最右下角的最后结点满足$<x$并且最接近x的结点，把这个结点覆盖待删除的结点，然后把这个结点删除，就完成了我们的操作。
    */
    
    // 如果不存在直接return
    if (!root) return;
    if (x < root->val) remove(root->left, x);
    else if (x > root->val) remove(root->right, x);
    else
    {
        if (!root->left && !root->right) root = NULL;
        else if (!root->left) root = root->right;
        else if (!root->right) root = root->left;
        else
        {
            auto p = root->left;
            while (p->right) p = p->right;
            root->val = p->val;
            remove(root->left, p->val);
        }
    }
}

// 输出数值 x 的前驱(前驱定义为现有所有数中小于 x 的最大的数)。
int get_pre(TreeNode* root, int x)
{
    if (!root) return -INF;
    if (root->val >= x) return get_pre(root->left, x);
    else return max(root->val, get_pre(root->right, x));
}

// 输出数值 x 的后继(后继定义为现有所有数中大于 x 的最小的数)。
int get_suc(TreeNode* root, int x)
{
    if (!root) return INF;
    if (root->val <= x) return get_suc(root->right, x);
    else return min(root->val, get_suc(root->left, x));
}

int main()
{
    int n;
    cin >> n;
    while (n--)
    {
        int t, x;
        cin >> t >> x;
        if (t == 1) insert(root, x);
        else if (t == 2) remove(root, x);
        else if (t == 3) cout << get_pre(root, x) << endl;
        else cout << get_suc(root, x) << endl;
    }
    return 0;
}
```