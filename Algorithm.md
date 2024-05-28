# Algorithm Note

Tags:  `algorithm`, `data structure`

[TOC]

## 前言

算法与数据结构，首先要理解数据结构，才能理解算法。虽然名字只有算法笔记，但是会记下所有的数据结构的实现，再去写相关的算法，以及stl标准库的应用。

## 复杂度

理解复杂度的概念，复杂度分为时间与空间的复杂度。时间上，又分为最好，最坏和平均的复杂度。一般考虑平均和最坏的情况。空间上，主要看存储用了什么数据结构。
复杂度只和输入数据的量n挂钩，不会考虑其他参数。

* O(1):复杂度为常量，时间上只进行了有限次操作，空间上只存储了几个变量
* O(logn):对数级别，通常用了二分法，二叉树等算法，可以达到这个复杂度级别。想要用二分法，通常要求传入数据已经是有序的。
* O(n):线性级别，通常经历了一次或者有限次遍历，空间上可能存储了n个变量。O(n)是对无序数组，也包括字符串通常能达到的最好水平，因为需要完整遍历一次数据才能得到最优解。
* O(nlogn):一般是对数据进行了排序操作，或者进行了n次二分查找或者二叉树。
* O(n^2):指数级别，一般用了双重嵌套for循环，空间上，可能存储了二维数组。
* O(2^n):回溯法，dfs等算法。只能在数据量小的情况使用，否则必定超时。

复杂度只会考虑最大的那个，比如O(nlogn+n) = O(nlogn)。

## 数据结构

用来存储数据的结构，不仅自己会写，也要会用stl提供的标准库。
常用的数据结构包括链表，栈，队列，优先级队列，树，平衡树，哈希表，图等等。会不断进行补充。

### 链表

链表一般都是单链表。头插尾插都是O(1)级别，中间插入删除搜索都是O(N)。一般用结点存数据，如果用数组，需要考虑扩充上限的问题。

```cpp
template <typename T>
struct ListNode {
    T data;
    ListNode *next;
};
ListNode head;
```

使用链表，一般用stl标准库里的`std::vector`，它的实现方式其实是数组，但是可以自动扩充，因此使用时不需要考虑内存问题。
可以使用迭代器访问数组中元素，也可以直接用下标访问。
`vector`常用的功能包括尾接，尾删，查看大小，和判空。

```cpp
#include <vector>
std::vector<int> vec{1,2,3,4,5};
for(auto iter = vec.begin(); iter != vec.end(); iter++) {
    cout << *iter << endl ;// 迭代器的解引用
    cout << vec[i] << endl;// 等价
}
vec.push_back(0);
vec.pop_back();
int n = vec.size();
if (vec.empty()) {
    return;
}
```

### 栈

栈的存储是和链表类似的，但是栈只能访问顶部的数据。满足先进后出原则，入栈和出栈都在顶部。只提供一个始终指向最顶部数据的指针。

```cpp
template <typename T>
struct Stack {
    T data;
    Stack *next;
};
Stack *top;
```

栈一般不需要自己实现。同样用标准库提供的`std::stack`即可。
`stack`常用功能包括入栈，出栈，获取栈顶，判空等。

```cpp
#include <stack>;
std::stack<int> st;
st.push(1);
if (!st.empty()) {
    int i = st.top();
    st.pop();
}
```

### 队列

队列也类似于链表，满足先进后出原则。入队数据在队尾，而出队数据出在队首。使用队列，需要保存指向队首和队尾的两个指针。

```cpp
template <typename T>
struct Queue {
    T data;
    Queue *next;
};
Queue *front, *end;
```

队列同样不需要自己写实现，一般用标准库提供的`std::queue`就可以了。
`queue`提供入队出队，判空，取队首队尾的操作。

```cpp
#include <queue>;
std::queue<int> que;
que.push({1,0});
if (!que.empty()) {
    int i = que.front();
    que.pop();
}
```

### 优先级队列

也叫二叉堆，它的特点是队首始终为数据里最大或者最小的。以最小堆为例，它的子节点会大于它本身。
实现二叉堆，一般用动态数组，不用结点的原因是，i/2刚好是i的父结点，比较好处理。对于优先级队列，需要考虑插入后，维持堆特性的过程，称之为向下过滤。

```cpp
//用最小堆存储最大的k组数据。
template <typename T>
class Priority_queue {
private:
    T *data;
    int count;
public:
    Priority_queue(int k) {
        data = new T[k+1];//会从data[1]开始存数据。
        count = 0;
    }
    void push(T i) {
        if (count < k) {
            ++count;
            int t = count;
            while (t > 1 && i < data[t/2]) {
                data[t] = data[t/2];
                t /= 2;
            }
            data[t] = i;
        } else if (i > data[1]) {
            int t = 1;
            while (i > que[t]) {
                t *= 2;
                if (t > k) {
                    break;
                } else if (t < k && que[t] > que[t + 1]) {
                    ++t;
                }
                if (i > que[t]) {
                    que[t / 2] = que[t];
                } else {
                    break;
                }
            }
            que[t / 2] = i;
        }
    }
};
```

自己写优先级队列很困难，最好还是用标准库提供的`std::priority_queue`实现。
`priority_queue`可以指定为大小堆，提供入堆，出堆，判空，取顶操作。

```cpp
#include <queue>;
std::priority_queue<int> que;//默认最大堆
std::priority_queue<int, std::vector<int>, std::greater<int>> que;//最小堆
que.push(1);
if (!que.empty()) {
    int i = que.top();
    que.pop();
}
```

### 树

树的结构非常多，普通的有二叉树，多叉树等。对于普通二叉树，需要掌握其遍历方式，包括前序，中序，后序，层次遍历四种遍历方法。对于搜索树，包括二叉搜索树，AVL树，红黑树，AA树等。对于这些树，需要掌握插入和删除时，如何维持树的平衡性。子结点不为2个时，变为B树和B+树。

stl标准库里没有树的结构，但是有用红黑树原理实现的映射`std::unordered_map`。
因此需要自己实现树的相关算法。

#### 二叉树

二叉树每个结点存一个数据，并且有2个指向左右子节点的指针。只存储根节点的指针用来调用。

```cpp
template <typename T>
struct TreeNode {
    T data;
    TreeNode *left, *right;
};
TreeNode *root;
```

##### 前序遍历

前序遍历的顺序是先根结点，再左右子结点。遍历通常用递归函数实现，为了避免函数栈溢出，也可以用栈实现。

```cpp
void preorder(TreeNode *p) {
    if (p == nullptr) {
        return;
    }
    std::cout << p->data << '\t';
    preorder(p->left);
    preorder(p->right);
}

void preorder(TreeNode *root) {
    if (root == nullptr) {
        return;
    }
    std::stack<TreeNode*> st;
    TreeNode *cur = root;
    st.push(cur);
    while (!st.empty()) {
        cur = st.top();
        std::cout << cur->data << '\t';
        st.pop();
        if (cur->right) {
            st.push(cur->right);
        }
        if (cur->left) {
            st.push(cur->left);
        }
    }
}
```

##### 中序遍历

中序遍历的顺序是先遍历左结点，然后是父结点，最后是右结点。中序遍历同样可以用递归实现，但是也可以用栈实现。相比于前序遍历，栈实现会相对复杂。

```cpp
void midorder(TreeNode *p) {
    if (p == nullptr) {
        return;
    }
    midorder(p->left);
    std::cout << p->data << '\t';
    midorder(p->right);
}

void midorder(TreeNode *root) {
    if (root == nullptr) {
        return;
    }
    std::stack<TreeNode*> st;
    TreeNode *cur = root;
    while (!st.empty() || cur != nullptr) {
        while (cur != nullptr) {
            st.push(cur);
            cur = cur->left;
        }
        cur = st.top();
        std::cout << cur->data << '\t';
        st.pop();
        cur = cur->right;//这一步，避免cur一直在重复的左链里
    }
}
```

##### 后续遍历

后续遍历的顺序是先左右结点，再遍历父结点。这种遍历方法使用非递归实现是最为复杂的。

```cpp
void postorder(TreeNode *p) {
    if (p == nullptr) {
        return;
    }
    postorder(p->left);
    postorder(p->right);
    std::cout << p->data << '\t';
}

void postorder(TreeNode *root) {
    if (root == nullptr) {
        return;
    }
    std::stack<TreeNode*> st;
    TreeNode *cur = root, *pre = nullptr;
    while (!st.empty() || cur != nullptr) {
        while (cur != nullptr) {
            st.push(cur);
            cur = cur->left;
        }
        cur = st.top();
        if (cur->right == nullptr || cur->right == pre) {
            st.pop();
            std::cout << cur->data << '\t';
            pre = cur;
            cur = nullptr;
        } else {
            cur = cur->right;
        }
    }
}
```

##### 层次遍历

层次遍历会按照树的层数进行遍历，很显然这是用队列实现的。

```cpp
void levelorder(TreeNode *root) {
    if (root == nullptr) {
        return;
    }
    std::queue<TreeNode*> que;
    TreeNode *p = root;
    que.push(p);
    while(!que.empty()) {
        p = que.front();
        std::cout << p->data << '\t';
        que.pop();
        if (p->left) {
            que.push(p->left);
        }
        if (p->right) {
            que.push(p->right);
        }
    }
}
```

#### 二叉搜索树

让二叉树实现搜索的功能，需要有序存储数据。原理就是，比当前结点大的数存到右结点，小的数存到左节点。这样的搜索树的搜索性能是O(logn)的。
对二叉搜索树进行中序遍历，就可以以顺序输出存储的数据。
二叉树的删除最为复杂，需要考虑删除的结点有几个子结点的问题。如果有2个子结点，需要把他右子树的最左结点替换删除。删除通过栈实现。

```cpp
void insert(T i) {
    TreeNode *p = root;
    if (p == nullptr) {
        root = new TreeNode(i);
        return;
    }
    while (true) {
        if (p->data == i) {
            return;
        } else if (p->data < i) {
            if (p->right == nullptr) {
                p->right = new TreeNode(i);
                return;
            }
            p = p->right;
        } else {
            if (p->left == nullptr) {
                p->left = new TreeNode(i);
                return;
            }
            p = p->left;
        }
    }
}

TreeNode* find(T i) {
    TreeNode *p = root;
    while (p != nullptr) {
        if (p->data == i) {
            break;
        } else if (p->data < i) {
            p = p->right;
        } else {
            p = p->left;
        }
    }
    return p;
}

void remove(T i) {

}
```