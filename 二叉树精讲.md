# 二叉树

## 定义

二叉树是一种树形结构，其中每个节点最多有两个子节点，分别称为左子节点和右子节点。

## 特点

1. 每个节点最多有两个子节点，分别称为左子节点和右子节点。
2. 左子节点和右子节点可以为空。
3. 二叉树的每个节点都包含一个值。
4. 通过递归的方式，可以将二叉树定义为一个根节点，以及左子树和右子树，其中左子树和右子树也都是二叉树。

### 图示

下边给大家画一下二叉树的图。

![image-20240225000848216](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260017488.png)

## 节点定义

```
class TreeNode {
public:
    int val;
    TreeNode* left;  // 左节点
    TreeNode* right; // 右节点

    TreeNode(int val) : val(val), left(nullptr), right(nullptr) {}
};
```

## 存储方式

### 顺式存储

将二叉树的节点按照从上到下、从左到右的顺序依次存储在一个数组中。具体存储方式如下：

- 对于第i个节点，其左子节点存储在2*i位置，右子节点存储在2*i+1位置。
- 根节点存储在数组的第一个位置（即下标为1的位置）。
- 如果某个位置没有节点，则数组中对应位置存储一个特定值（如0或null）。

![image-20240225225903726](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260017580.png)

### 链式存储

每个节点通过指针（或引用）连接其左右子节点。具体方式如下：

- 每个节点包含数据域和指向左右子节点的指针。
- 根节点通过一个指针指向树的根。
- 每个节点的左右子节点分别通过指针指向。

![image-20240225230306093](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260017374.png)

## 遍历方式

### 前序遍历

#### 遍历顺序

1. 先访问根节点。
2. 再遍历左子树。
3. 最后遍历右子树。

#### 参考代码

```
#include <iostream>
#include <stack>
using namespace std;

// 二叉树节点定义
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

// 递归实现前序遍历
void preorderTraversalRecursive(TreeNode* root) {
    if (root == NULL) return;
    cout << root->val << " "; // 访问当前节点
    preorderTraversalRecursive(root->left); // 递归遍历左子树
    preorderTraversalRecursive(root->right); // 递归遍历右子树
}

// 非递归实现前序遍历
void preorderTraversalIterative(TreeNode* root) {
    if (root == NULL) return;
    stack<TreeNode*> s;
    s.push(root);
    while (!s.empty()) {
        TreeNode* node = s.top();
        s.pop();
        cout << node->val << " "; // 访问当前节点
        // 因为栈是先进后出的结构，所以先将右子树压入栈中，再将左子树压入栈中
        if (node->right) s.push(node->right);
        if (node->left) s.push(node->left);
    }
}

int main() {
    // 创建二叉树
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);

    cout << "递归遍历结果：" << endl;
    preorderTraversalRecursive(root);
    cout << endl;

    cout << "非递归遍历结果：" << endl;
    preorderTraversalIterative(root);
    cout << endl;

    return 0;
}
```

### 中序遍历

#### 遍历顺序

1. 先遍历左子树
2. 再访问根节点
3. 最后遍历右子树

#### 参考代码

```
#include <iostream>
#include <stack>
using namespace std;

// 二叉树节点的定义
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// 递归实现中序遍历
void inorderTraversalRecursive(TreeNode* root) {
    if (root == nullptr) return; // 如果节点为空，直接返回
    inorderTraversalRecursive(root->left); // 先递归遍历左子树
    cout << root->val << " "; // 访问当前节点
    inorderTraversalRecursive(root->right); // 再递归遍历右子树
}

// 非递归实现中序遍历
void inorderTraversal(TreeNode* root) {
    stack<TreeNode*> s; // 辅助栈
    TreeNode* curr = root; // 当前节点
    while (curr != nullptr || !s.empty()) {
        // 遍历左子树并入栈
        while (curr != nullptr) {
            s.push(curr);
            curr = curr->left;
        }
        // 左子树遍历完成，访问栈顶节点
        curr = s.top();
        s.pop();
        cout << curr->val << " ";
        // 遍历右子树
        curr = curr->right;
    }
}

int main() {
    // 创建示例二叉树
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->right = new TreeNode(4);
    root->right->left = new TreeNode(5);

    cout << "递归遍历结果：" << endl;
    inorderTraversalRecursive(root);
    cout << endl;

    cout << "非递归遍历结果：" << endl;
    inorderTraversal(root);

    return 0;
}
```

### 后序遍历

#### 遍历顺序

1. 先遍历左子树
2. 再遍历右子树
3. 访问根节点

#### 参考代码

```
#include <iostream>
#include <stack>
using namespace std;

// 二叉树节点的定义
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// 递归实现后序遍历
void postorderTraversalRecursive(TreeNode* root) {
    if (root == nullptr) return; // 如果节点为空，直接返回
    postorderTraversalRecursive(root->left); // 先递归遍历左子树
    postorderTraversalRecursive(root->right); // 再递归遍历右子树
    cout << root->val << " "; // 访问当前节点
}

// 非递归实现后序遍历
void postorderTraversal(TreeNode* root) {
    stack<TreeNode*> s; // 辅助栈
    TreeNode* lastVisited = nullptr; // 上一个访问过的节点
    TreeNode* curr = root; // 当前节点
    while (curr != nullptr || !s.empty()) {
        // 遍历左子树并入栈
        while (curr != nullptr) {
            s.push(curr);
            curr = curr->left;
        }
        // 查看栈顶节点，但不出栈
        curr = s.top();
        // 如果当前节点的右子树为空或者已经访问过
        if (curr->right == nullptr || curr->right == lastVisited) {
            cout << curr->val << " "; // 访问当前节点
            s.pop(); // 出栈
            lastVisited = curr; // 更新上一个访问过的节点
            curr = nullptr; // 将当前节点置空，下一轮循环将访问栈中下一个节点
        } else {
            // 否则，遍历右子树
            curr = curr->right;
        }
    }
}

int main() {
    // 创建示例二叉树
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);

    cout << "递归遍历结果：" << endl;
    postorderTraversalRecursive(root);
    cout << endl;

    cout << "非递归遍历结果：" << endl;
    postorderTraversal(root);

    return 0;
}
```

## 分类

### 完全二叉树

#### 定义

除了最后一层外，每一层都被完全填满，并且所有节点都保持向左对齐的二叉树。

#### 特点

除了最后一层，其他各层节点数都达到最大值，且最后一层的节点都集中在左边。

#### 性质

如果一个节点的编号为i，则其左子节点的编号为2i，右子节点的编号为2i+1。反之，对于编号为i的节点，其父节点的编号为i/2（向下取整）。

#### 图示

![image-20240225003521409](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260017587.png)

但是需要注意的是，如果没有节点6和7，那么就不是完全二叉树；如果没有节点8，也不是完全二叉树。

### 满二叉树

#### 定义

除最后一层无任何子节点外，每一层上的所有结点都有两个子结点的二叉树。

#### 特点

1. 每个节点要么是叶子节点，要么有两个子节点。
2. 所有叶子节点都在同一层，即最底层，除叶子节点外，每个节点都有两个子节点。

#### 性质

1. 如果满二叉树的高度为 h，则节点总数为 2^h - 1。其中，h 为树的高度。
2. 树的高度 h 可以通过节点数量计算得到：h = log2(n+1)，其中 n 为节点数量。
3. 是一种特殊的完全二叉树，每一层都被完全填满，没有缺失的节点。
4. 具有相同高度的满二叉树具有相同数量的节点，并且结构相同。

#### 图示

![image-20240225204601362](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260017519.png)

### 二叉搜索树（BST）

#### 定义

二叉搜索树（BST）是一种特殊的二叉树，每个顶点最多可以有两个子节点。这种结构遵循**BST属性**，规定给定顶点的左子树中的每个顶点的值必须小于给定顶点的值，右子树中的每个顶点的值必须大于给定顶点的值。

#### 特点

1. 对于任意节点，其左子树上的所有节点的值都小于该节点的值，右子树上的所有节点的值都大于该节点的值。
2. 中序遍历二叉搜索树可以得到一个递增（或递减）的有序序列。

#### 性质

1. 最左下角的节点包含树中最小的元素，最右下角的节点包含树中最大的元素。
2. 可以通过比较节点的值，按照二叉搜索树的性质，在 O(log n) 的时间内找到目标值。
3. 插入和删除节点时，需要保持二叉搜索树的性质，但在最坏情况下可能需要 O(n) 的时间复杂度。

#### 图示

![image-20240225210218536](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260018181.png)

### 平衡二叉树（AVL）

#### 定义

它的左右子树的高度差不超过 1，也就是任意节点的左右子树高度差的绝对值不超过 1。

#### 特点

1. 每个节点的左右子树高度差不超过 1。
2. 对于任意节点，它的左右子树也都是平衡二叉树。

#### 性质

1. 在平衡二叉树中，查找、插入和删除操作的时间复杂度都是 O(log n)，其中 n 是树中节点的个数。
2. 平衡二叉树的高度近似 log₂(n+1)，其中 n 是树中节点的个数。

#### 图示

![image-20240225213656886](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260018543.png)

#### AVL的失衡与调整

这里先讲讲**平衡因子**。

在 AVL 树中，每个节点都会有一个平衡因子（Balance Factor），它表示节点的左子树高度减去右子树高度的结果。平衡因子可以是 -1、0 或 1，这取决于左右子树的高度关系。

具体来说：

- 如果一个节点的平衡因子为 -1，表示该节点的右子树比左子树高度高 1；
- 如果平衡因子为 0，表示左右子树高度相等；
- 如果平衡因子为 1，表示左子树比右子树高度高 1。

##### 左旋

给上图插入一个结点80

![image-20240225213853629](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260018573.png)

此时二叉树不平衡，需要左旋保持平衡。

步骤：

围绕根节点进行，将该节点的右子节点提升为新的根节点，原来的根节点则成为新根节点的左子节点。同时，新根节点的左子树成为原根节点的右子树。

![image-20240225214713430](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260018957.png)

##### 右旋

![image-20240225220341673](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260018506.png)

此时二叉树不平衡，需要右旋转保持平衡。

步骤：

围绕根节点进行，将该节点的左子节点提升为新的根节点，原来的根节点则成为新根节点的右子节点。同时，新根节点的右子树成为原根节点的左子树。

![image-20240225220516734](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260018474.png)

#### 节点定义

```
struct Node {
    int data;
    int height; // 节点高度
    Node* left;
    Node* right;

    Node(int value) : data(value), height(1), left(nullptr), right(nullptr) {}
};
```

### 红黑树（RBTree）

#### 定义

红黑树（Red-Black Tree）是一种自平衡的二叉搜索树，它在每个节点上增加了一个存储位来表示节点的颜色，可以是红色或黑色。

#### 特点

1. 红黑树是一种近似平衡的二叉搜索树，能够在最坏情况下保证基本动态操作的时间复杂度为 O(log n)。
2. 红黑树的高度最多是二叉树高度的两倍。

#### 性质

1. 每个节点要么是红色，要么是黑色。
2. 根节点是黑色的。
3. 每个叶子节点（NIL节点，即空节点）是黑色的。
4. 如果一个节点是红色的，则它的两个子节点都是黑色的。
5. 对于每个节点，从该节点到其所有后代叶子节点的简单路径上，均包含相同数量的黑色节点（即黑色节点的数量相同）。

#### 图示

![image-20240225223247904](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260018694.png)

#### 节点定义

```
enum Color { RED, BLACK };

struct Node {
    int data;
    Color color;
    Node* left;
    Node* right;
    Node* parent;

    Node(int value) : data(value), color(RED), left(nullptr), right(nullptr), parent(nullptr) {}
};
```

##### 左旋

```
#include <iostream>
using namespace std;

enum Color {RED, BLACK};

// 红黑树节点的结构体定义
struct Node {
    int data;           // 数据
    Node* left;         // 左子节点指针
    Node* right;        // 右子节点指针
    Node* parent;       // 父节点指针
    Color color;        // 节点颜色

    // 构造函数
    Node(int data) : data(data), left(nullptr), right(nullptr), parent(nullptr), color(RED) {}
};

// 获取节点的颜色
Color getColor(Node* node) {
    return (node == nullptr) ? BLACK : node->color;
}

// 红黑树左旋操作函数
void leftRotate(Node*& root, Node* x) {
    Node* y = x->right;   // 将y设为x的右子节点
    x->right = y->left;    // 将y的左子节点设为x的右子节点
    if (y->left != nullptr) {
        y->left->parent = x;
    }
    y->parent = x->parent; // 将y的父节点设为x的父节点
    if (x->parent == nullptr) {
        root = y;
    } else if (x == x->parent->left) {
        x->parent->left = y;
    } else {
        x->parent->right = y;
    }
    y->left = x;           // 将x设为y的左子节点
    x->parent = y;
}
```

##### 右旋

```
// 红黑树右旋操作函数
void rightRotate(Node*& root, Node* y) {
    Node* x = y->left;    // 将x设为y的左子节点
    y->left = x->right;    // 将x的右子节点设为y的左子节点
    if (x->right != nullptr) {
        x->right->parent = y;
    }
    x->parent = y->parent; // 将x的父节点设为y的父节点
    if (y->parent == nullptr) {
        root = x;
    } else if (y == y->parent->right) {
        y->parent->right = x;
    } else {
        y->parent->left = x;
    }
    x->right = y;          // 将y设为x的右子节点
    y->parent = x;
}
```

