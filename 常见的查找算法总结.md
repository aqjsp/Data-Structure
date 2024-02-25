# 常见的查找算法总结

查找算法，顾名思义就是在一堆数据中查找到你想要的那个数据。以下就介绍几种常用的查找算法，帮助大家更好的了解其原理和使用场景。

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260021287.png)

## 1、线性查找

### 1.1、基本概念及适用场景

线性查找（Linear Search），也叫顺序查找，是一种简单的查找算法，适用于无序数组或链表中的元素查找。线性查找的原理是按顺序依次扫描待查找的元素，直到找到目标元素或扫描完所有元素。

具体实现时，从数组的第一个元素开始逐个比较，如果找到目标元素，则返回其下标，否则返回未找到的标记（如-1）。如果数组中存在多个目标元素，则只会找到第一个。

线性查找的时间复杂度为O(n)，其中n是待查找元素的个数，最坏情况下需要扫描整个数组或链表。

线性查找适用于以下情况：

- 待查找的数据规模较小，或数据无序，或需要查询的数据在数组或链表的末尾。
- 数据存储在单向链表中，没有下标的概念。

**需要注意的是，线性查找的效率比较低，不适用于大规模的数据查询。**

### 1.2、代码示例

```C
#include <stdio.h>

int linearSearch(int arr[], int n, int x) {
    int i;
    for (i = 0; i < n; i++) {
        if (arr[i] == x) {
            return i;   // 找到了，返回元素下标
        }
    }
    return -1;  // 没找到，返回-1
}

int main() {
    int arr[] = {10, 20, 30, 40, 50, 60};
    int n = sizeof(arr) / sizeof(arr[0]);
    int x = 40;
    int result = linearSearch(arr, n, x);
    if (result == -1) {
        printf("元素 %d 不存在\n", x);
    } else {
        printf("元素 %d 的下标是 %d\n", x, result);
    }
    return 0;
}
```

在上述示例中，`linearSearch()`函数用于进行线性查找，参数 `arr` 表示要查找的数组，`n` 表示数组的长度，`x` 表示要查找的元素值。函数通过遍历数组中的每个元素，找到目标元素就返回其下标，若未找到则返回 `-1`。在 `main()` 函数中，声明数组并调用 `linearSearch()` 函数进行查找，最终输出查找结果。

## 2、二分查找

### 2.1、基本原理及注意事项

二分查找（Binary Search），也称折半查找，是一种常见的查找算法。它的**基本原理是在有序数组中查找目标元素，通过将目标元素与有序数组的中间元素进行比较，可以排除一半的元素，从而提高查找效率**。

二分查找适用于有序数组中的查找，可以用于查找具有单调性质的数据集合。其时间复杂度为 O(log n)，相对于线性查找的 O(n)，效率更高。但是，二分查找的前提是必须有序，如果需要频繁的插入和删除操作，那么维护有序性就需要额外的操作，会降低效率。

在使用二分查找时需要注意以下几点：

1. **数组必须有序。**
2. **二分查找只适用于静态查找，即目标数组不经常变化。**
3. **目标元素必须是可比较的，可以使用小于或大于操作符进行比较。**
4. **二分查找的效率高于线性查找，但在小数据量的查找中，可能没有线性查找快。**

二分查找的应用场景包括查找有序数组中的特定元素、查找第一个大于或小于给定值的元素等。

需要注意的是，虽然二分查找是一种高效的查找算法，但是在实际开发中，有时候使用哈希表等其他数据结构也能达到更高的效率，所以需要根据具体的问题场景选择合适的算法。

### 2.2、代码示例

当在一个有序数组查找某个元素时，二分查找是一个很高效的算法。

```C
#include <stdio.h>

int binarySearch(int arr[], int l, int r, int x) {
    if (r >= l) {
        int mid = l + (r - l) / 2;

        if (arr[mid] == x) {
            return mid;
        }
        if (arr[mid] > x) {
            return binarySearch(arr, l, mid - 1, x);
        }
        return binarySearch(arr, mid + 1, r, x);
    }
    return -1;
}

int main() {
    int arr[] = {1, 3, 5, 7, 9, 11, 13};
    int n = sizeof(arr) / sizeof(arr[0]);
    int x = 7;
    int result = binarySearch(arr, 0, n - 1, x);
    if (result == -1) {
        printf("元素不在数组中");
    } else {
        printf("元素在数组中的索引为：%d", result);
    }
    return 0;
}
```

该代码实现了一个名为`binarySearch`的函数，该函数用于在一个有序数组中查找给定的元素。`binarySearch`函数的参数为待查找数组`arr`，数组左边界`l`，数组右边界`r`以及要查找的元素`x`。

在函数内部，首先通过计算中间元素的下标来将待查找区间分为两部分。如果中间元素等于要查找的元素，则直接返回中间元素的下标。如果中间元素大于要查找的元素，则在左半部分进行递归查找。否则，在右半部分进行递归查找。

在`main`函数中，先定义了一个有序数组`arr`，以及要查找的元素`x`。然后，调用`binarySearch`函数查找给定元素，并将返回值保存在变量`result`中。如果`result`的值为`-1`，则说明要查找的元素不在数组中；否则，输出要查找的元素在数组中的索引。

需要注意的是，二分查找算法要求待查找的数组必须是有序的。因此，在使用二分查找算法前，需要保证待查找的数组已经排好序。

## 3、插值查找

### 3.1、基本概念

插值查找是一种基于二分查找算法的优化，它的基本原理与二分查找类似，只不过插值查找根据查找键值与查找范围内值的分布情况，通过插值来确定下一步查找的位置。与二分查找相比，它可以提供更快的查找速度，尤其是数据比较分散的情况下，比如数据集中在数组的前面或后面。

插值查找的具体实现步骤如下：

1. 确定查找范围，初始化起始位置left为0，结束位置right为n-1，其中n为数组长度。
2. 计算中间位置mid，mid的值为 (key - arr[left]) / (arr[right] - arr[left]) * (right - left) + left，其中key为查找关键字，arr为待查找的有序数组。
3. 如果arr[mid]等于key，则返回mid。
4. 如果arr[mid]小于key，则在[mid+1, right]范围内查找。
5. 如果arr[mid]大于key，则在[left, mid-1]范围内查找。
6. 重复2-5步，直到查找到目标值或查找范围为空，查找失败。

需要注意的是，插值查找要求待查找的数组是有序的，否则无法保证查找结果的正确性。此外，当数据分布较为均匀时，插值查找可以快速定位到目标值，但当数据分布不均时，可能会导致查找效率的降低。

### 3.2、代码示例

```C
#include <stdio.h>

// 插值查找函数，array为待查找数组，n为数组长度，target为目标值
int interpolationSearch(int array[], int n, int target) {
    int low = 0, high = n - 1;
    while (low <= high && target >= array[low] && target <= array[high]) {
        int pos = low + ((double)(target - array[low]) / (array[high] - array[low])) * (high - low);
        if (array[pos] == target) {
            return pos;
        } else if (array[pos] < target) {
            low = pos + 1;
        } else {
            high = pos - 1;
        }
    }
    return -1;
}

// 测试
int main() {
    int array[] = { 2, 4, 6, 8, 10, 12, 14, 16, 18, 20 };
    int n = sizeof(array) / sizeof(int);
    int target = 12;
    int index = interpolationSearch(array, n, target);
    if (index != -1) {
        printf("目标值%d在数组中的位置是%d\n", target, index);
    } else {
        printf("目标值%d在数组中不存在\n", target);
    }
    return 0;
}
```

在上面的代码中，`interpolationSearch()` 函数采用了插值查找算法的实现，其中 `array` 为待查找数组，`n` 表示数组长度，`target` 表示目标值。在查找过程中，我们首先计算出目标值所在的估计位置 `pos`，然后根据 `array[pos]` 的值与目标值 `target` 的大小进行比较，并更新查找的范围，直到找到目标值或者确定目标值不存在于数组中。最终，该函数返回目标值在数组中的位置，如果不存在则返回 -1。

在 `main()` 函数中，我们定义了一个数组 `array` 和目标值 `target`，并调用 `interpolationSearch()` 函数进行查找，将结果输出到控制台。

## 4、斐波那契查找

### 4.1、基本原理

斐波那契查找算法（Fibonacci Search Algorithm）是一种利用斐波那契数列长度的有序数组进行查找的算法。与二分查找不同，斐波那契查找每次的查找范围是斐波那契数列中的某一段，从而更加高效地进行查找。

具体来说，假设待查找的数组为arr，数组长度为n。斐波那契查找首先要确定一个斐波那契数列fib，满足fib[k] >= n，且fib[k-1] < n。然后根据当前查找范围的左右端点计算mid = left + fib[k-1]，即查找范围中点的位置。如果arr[mid] == target，则查找成功；如果arr[mid] < target，则在mid的右侧继续查找；如果arr[mid] > target，则在mid的左侧继续查找。查找的过程类似于二分查找，只不过查找范围不是一半，而是根据斐波那契数列划分的一段。

斐波那契查找的优点是可以在O(log n)的时间内完成查找，比一般的线性查找O(n)和二分查找O(log n)更快。但是需要注意的是，斐波那契查找算法只适用于元素数量比较大、分布均匀的数组。对于元素数量较少或分布不均的数组，效率并不一定比其他算法高。

### 4.2、代码示例

#### 方法一：

假设需要查找的元素为key，数组为arr，数组长度为n

```C
#include <stdio.h>
#include <stdlib.h>

// 斐波那契查找算法
int fib_search(int arr[], int n, int key) {
    // 初始化斐波那契数列
    int fib1 = 0;
    int fib2 = 1;
    int fibn = fib1 + fib2;

    // 找到斐波那契数列中第一个大于等于n的数
    while (fibn < n) {
        fib1 = fib2;
        fib2 = fibn;
        fibn = fib1 + fib2;
    }

    // 对数组进行扩展，使其长度为斐波那契数列中的某个数
    int *tmp = (int *)malloc(fibn * sizeof(int));
    for (int i = 0; i < n; i++) {
        tmp[i] = arr[i];
    }
    for (int i = n; i < fibn; i++) {
        tmp[i] = arr[n - 1];
    }

    // 开始查找
    int low = 0;
    int high = fibn - 1;
    int mid;
    while (low <= high) {
        // 计算当前中间位置
        mid = low + fib1 - 1;
        // 如果key比当前位置的值小，则在左侧继续查找
        if (key < tmp[mid]) {
            high = mid - 1;
            fibn = fib1;
            fib1 = fib2 - fib1;
            fib2 = fibn - fib1;
        }
        // 如果key比当前位置的值大，则在右侧继续查找
        else if (key > tmp[mid]) {
            low = mid + 1;
            fibn = fib2;
            fib1 = fib1 - fib2;
            fib2 = fibn - fib1;
        }
        // 找到了key
        else {
            // 如果当前位置小于n，则返回该位置，否则返回n-1
            if (mid < n) {
                return mid;
            } else {
                return n - 1;
            }
        }
    }

    // 没有找到key
    free(tmp);
    return -1;
}

// 测试代码
int main() {
    int arr[] = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
    int n = sizeof(arr) / sizeof(arr[0]);
    int key = 7;
    int index = fib_search(arr, n, key);
    if (index == -1) {
        printf("The key %d is not found.\n", key);
    } else {
        printf("The key %d is found at index %d.\n", key, index);
    }
    return 0;
}
```

注意：上述代码假设数组中的元素是按照从小到大的顺序排列的。如果数组中的元素是无序的，则需要先对数组进行排序，然后再进行斐波那契查找。

#### 方法二：

```C
#include <stdio.h>

// 获取斐波那契数列，n表示数列中第n个元素的值
int getFibonacci(int n) {
    if (n <= 0) return 0;
    if (n == 1) return 1;
    return getFibonacci(n-1) + getFibonacci(n-2);
}

// 斐波那契查找算法，a为有序数组，n为数组长度，key为要查找的元素
int fibonacciSearch(int a[], int n, int key) {
    int low = 0, high = n-1, k = 0, mid = 0;

    // 计算斐波那契数列中第k个数刚好大于n
    while (n > getFibonacci(k)-1) {
        k++;
    }

    // 将数组扩展到斐波那契数列中第k个数的长度
    int *temp = new int[getFibonacci(k)];
    for (int i = 0; i < n; i++) {
        temp[i] = a[i];
    }
    for (int i = n; i < getFibonacci(k); i++) {
        temp[i] = a[n-1];
    }

    // 二分查找
    while (low <= high) {
        mid = low + getFibonacci(k-1) - 1;
        if (key < temp[mid]) {
            high = mid - 1;
            k--;
        } else if (key > temp[mid]) {
            low = mid + 1;
            k -= 2;
        } else {
            if (mid < n) {
                return mid;
            } else {
                return n - 1;
            }
        }
    }
    delete [] temp;
    return -1;
}

int main() {
    int a[] = {1, 3, 5, 7, 9, 11, 13};
    int n = sizeof(a) / sizeof(a[0]);
    int key = 11;
    int pos = fibonacciSearch(a, n, key);
    if (pos >= 0) {
        printf("元素 %d 在数组中的位置为 %d\n", key, pos);
    } else {
        printf("元素 %d 在数组中不存在\n", key);
    }
    return 0;
}
```

在这段代码中，我们首先使用 `getFibonacci` 函数获取斐波那契数列中第k个数，然后将数组 `a` 扩展到斐波那契数列中第k个数的长度，这样做是为了保证数组长度大于等于斐波那契数列中第k个数，从而可以用斐波那契数列中的数作为分割点进行查找。最后，我们使用二分查找的方式进行查找，最终返回查找结果的下标或者-1表示查找失败。

## 5、哈希查找

### 5.1、基本原理

哈希查找是一种常用的查找算法，它的基本思想是将数据元素映射到一个固定的存储位置，从而实现快速的查找。哈希查找的基本原理是利用哈希函数将关键字映射到存储位置，当需要查找一个元素时，先通过哈希函数计算出该元素对应的存储位置，然后在该位置进行查找。

哈希函数是哈希查找的关键，它将关键字映射到一个存储位置。哈希函数应该具有良好的映射性能，能够均匀地将关键字分布到不同的存储位置上，这样可以避免冲突。冲突是指多个关键字映射到同一个存储位置上，这种情况下需要解决冲突。

哈希查找的注意事项包括以下几点：

1. 哈希函数的设计应该具有良好的均匀性，能够尽可能避免冲突。
2. 解决冲突的方法有很多，比如拉链法、开放地址法等。选择合适的冲突解决方法对哈希查找的效率影响很大。
3. 哈希查找的效率取决于哈希函数的设计、冲突解决方法的选择以及负载因子等因素。
4. 哈希表的大小应该合适，过大会造成空间浪费，过小会造成冲突增加。
5. 哈希表的扩容和缩容是一个比较复杂的问题，需要根据实际情况进行考虑。

### 5.2、代码示例

下面是一个简单的哈希查找算法的代码示例，使用了线性探测法解决冲突。

```C
#include <stdio.h>
#include <stdlib.h>

#define TABLE_SIZE 13

typedef struct HashNode {
    int key;
    int value;
} HashNode;

typedef struct HashTable {
    HashNode *table[TABLE_SIZE];
} HashTable;

int hash(int key) {
    return key % TABLE_SIZE;
}

HashTable* createHashTable() {
    HashTable *hashTable = (HashTable*)malloc(sizeof(HashTable));
    for (int i = 0; i < TABLE_SIZE; i++) {
        hashTable->table[i] = NULL;
    }
    return hashTable;
}

void insert(HashTable *hashTable, int key, int value) {
    HashNode *node = (HashNode*)malloc(sizeof(HashNode));
    node->key = key;
    node->value = value;

    int index = hash(key);
    while (hashTable->table[index] != NULL) {
        index = (index + 1) % TABLE_SIZE;
    }
    hashTable->table[index] = node;
}

int search(HashTable *hashTable, int key) {
    int index = hash(key);
    while (hashTable->table[index] != NULL && hashTable->table[index]->key != key) {
        index = (index + 1) % TABLE_SIZE;
    }

    if (hashTable->table[index] == NULL) {
        return -1;
    } else {
        return hashTable->table[index]->value;
    }
}

void delete(HashTable *hashTable, int key) {
    int index = hash(key);
    while (hashTable->table[index] != NULL && hashTable->table[index]->key != key) {
        index = (index + 1) % TABLE_SIZE;
    }

    if (hashTable->table[index] != NULL) {
        free(hashTable->table[index]);
        hashTable->table[index] = NULL;
    }
}

int main() {
    HashTable *hashTable = createHashTable();
    insert(hashTable, 3, 100);
    insert(hashTable, 9, 200);
    insert(hashTable, 6, 300);
    insert(hashTable, 12, 400);

    int value = search(hashTable, 9);
    printf("Value: %d\n", value);

    delete(hashTable, 9);

    value = search(hashTable, 9);
    printf("Value: %d\n", value);

    return 0;
}
```

该代码实现了一个基于线性探测法的哈希查找算法，其中 `HashTable` 表示哈希表，`HashNode` 表示哈希表中的一个节点，包括键 `key` 和值 `value`，`hash` 函数用于计算哈希值，`createHashTable` 函数用于创建一个新的哈希表，`insert` 函数用于在哈希表中插入一个节点，`search` 函数用于在哈希表中查找指定的键，并返回对应的值，`delete` 函数用于从哈希表中删除指定的键。

## 6、树表查找

### 6.1、基本原理

树表查找（Tree-based Search）通常是一种利用有序树结构进行查找的算法，基于二叉搜索树（BST）或其它平衡二叉搜索树（如AVL树、红黑树）等数据结构实现的查找算法。其基本原理是将查找值与树中的某个节点进行比较，根据比较结果，沿着树的某个分支继续向下查找，直到查找到目标节点或者发现目标节点不存在为止。

树表查找的优点是查找效率高，时间复杂度为 O(log n)，其中 n 是节点的总数。它的主要缺点是需要维护树的平衡性，增加和删除节点会导致树的平衡性被破坏，需要进行旋转操作来恢复平衡，这样会增加操作的时间复杂度。

实现树表查找算法，需要定义一个树结构，每个节点包括一个键和一个值。键用于比较大小，值是存储的数据。具体实现可以使用递归或者迭代的方式，对于每个节点进行比较，并根据比较结果决定向左子树或者右子树继续查找，直到找到目标节点或者发现目标节点不存在为止。

### 6.2、代码示例

#### 方法一：基于BST实现

```C
#include <stdio.h>
#include <stdlib.h>

// 定义二叉搜索树节点结构体
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
};

// BST的插入操作
struct TreeNode* insert(struct TreeNode* root, int val) {
    if (root == NULL) {
        struct TreeNode* new_node = (struct TreeNode*)malloc(sizeof(struct TreeNode));
        new_node->val = val;
        new_node->left = NULL;
        new_node->right = NULL;
        return new_node;
    } else {
        if (val < root->val) {
            root->left = insert(root->left, val);
        } else if (val > root->val) {
            root->right = insert(root->right, val);
        }
        return root;
    }
}

// BST的查找操作
struct TreeNode* search(struct TreeNode* root, int val) {
    if (root == NULL || root->val == val) {
        return root;
    } else if (val < root->val) {
        return search(root->left, val);
    } else {
        return search(root->right, val);
    }
}

// 中序遍历BST（用于验证BST的正确性）
void inorderTraversal(struct TreeNode* root) {
    if (root != NULL) {
        inorderTraversal(root->left);
        printf("%d ", root->val);
        inorderTraversal(root->right);
    }
}

int main() {
    struct TreeNode* root = NULL;
    root = insert(root, 5);
    root = insert(root, 3);
    root = insert(root, 7);
    root = insert(root, 2);
    root = insert(root, 4);
    root = insert(root, 6);
    root = insert(root, 8);

    inorderTraversal(root); // 输出：2 3 4 5 6 7 8

    struct TreeNode* node = search(root, 6);
    if (node != NULL) {
        printf("找到了：%d\n", node->val); // 输出：找到了：6
    } else {
        printf("未找到\n");
    }

    return 0;
}
```

该代码定义了一个二叉搜索树的结构体`TreeNode`，包含了该节点的值`val`、左子节点指针`left`、右子节点指针`right`。同时，实现了BST的插入和查找操作，其中插入操作是递归实现的，查找操作也是递归实现的。最后，利用中序遍历函数`inorderTraversal`验证了BST的正确性，以及利用查找函数`search`查找了节点6。

#### 方法二：基于红黑树

基于红黑树实现的树表查找，也称为红黑树查找，是一种高效的查找算法。红黑树是一种自平衡二叉查找树，它具有以下特点：

1. 每个节点都有颜色，红色或黑色；
2. 根节点和叶子节点都是黑色；
3. 如果一个节点是红色，则它的子节点必须是黑色；
4. 任何一条从根节点到叶子节点的路径上，黑色节点的个数必须相同。

基于红黑树实现的树表查找的实现过程如下：

1. 构建红黑树，将要查找的元素插入到红黑树中；
2. 对红黑树进行遍历，查找需要的元素；
3. 如果查找成功，返回该元素的位置；
4. 如果查找失败，返回空指针。

红黑树的插入和删除操作都会改变树的结构，因此在进行插入和删除操作时需要对红黑树进行重新平衡。具体的平衡方法包括左旋、右旋、变色等操作，这些操作的目的是保持红黑树的平衡性和有序性。在进行查找操作时，根据红黑树的特点可以快速定位到目标元素，从而实现高效的查找。

**rbtree.h**

```C
#ifndef _RED_BLACK_TREE_H_
#define _RED_BLACK_TREE_H_

#define RED        0    // 红色节点
#define BLACK    1    // 黑色节点

typedef int Type;

// 红黑树的节点
typedef struct RBTreeNode{
    unsigned char color;        // 颜色(RED 或 BLACK)
    Type   key;                    // 关键字(键值)
    struct RBTreeNode *left;    // 左孩子
    struct RBTreeNode *right;    // 右孩子
    struct RBTreeNode *parent;    // 父结点
}Node, *RBTree;

// 红黑树的根
typedef struct rb_root{
    Node *node;
}RBRoot;

// 创建红黑树，返回"红黑树的根"！
RBRoot* create_rbtree();

// 销毁红黑树
void destroy_rbtree(RBRoot *root);

// 将结点插入到红黑树中。插入成功，返回0；失败返回-1。
int insert_rbtree(RBRoot *root, Type key);

// 删除结点(key为节点的值)
void delete_rbtree(RBRoot *root, Type key);


// 前序遍历"红黑树"
void preorder_rbtree(RBRoot *root);
// 中序遍历"红黑树"
void inorder_rbtree(RBRoot *root);
// 后序遍历"红黑树"
void postorder_rbtree(RBRoot *root);

// (递归实现)查找"红黑树"中键值为key的节点。找到的话，返回0；否则，返回-1。
int rbtree_search(RBRoot *root, Type key);
// (非递归实现)查找"红黑树"中键值为key的节点。找到的话，返回0；否则，返回-1。
int iterative_rbtree_search(RBRoot *root, Type key);

// 返回最小结点的值(将值保存到val中)。找到的话，返回0；否则返回-1。
int rbtree_minimum(RBRoot *root, int *val);
// 返回最大结点的值(将值保存到val中)。找到的话，返回0；否则返回-1。
int rbtree_maximum(RBRoot *root, int *val);

// 打印红黑树
void print_rbtree(RBRoot *root);

#endif
```

**rbtree.c**

```C
/**
 * C语言实现的红黑树(Red Black Tree)
 *
 * @author skywang
 * @date 2013/11/18
 */

#include <stdio.h>
#include <stdlib.h>
#include "rbtree.h"

#define rb_parent(r)   ((r)->parent)
#define rb_color(r) ((r)->color)
#define rb_is_red(r)   ((r)->color==RED)
#define rb_is_black(r)  ((r)->color==BLACK)
#define rb_set_black(r)  do { (r)->color = BLACK; } while (0)
#define rb_set_red(r)  do { (r)->color = RED; } while (0)
#define rb_set_parent(r,p)  do { (r)->parent = (p); } while (0)
#define rb_set_color(r,c)  do { (r)->color = (c); } while (0)

/*
 * 创建红黑树，返回"红黑树的根"！
 */
RBRoot* create_rbtree()
{
    RBRoot *root = (RBRoot *)malloc(sizeof(RBRoot));
    root->node = NULL;

    return root;
}

/*
 * 前序遍历"红黑树"
 */
static void preorder(RBTree tree)
{
    if(tree != NULL)
    {
        printf("%d ", tree->key);
        preorder(tree->left);
        preorder(tree->right);
    }
}
void preorder_rbtree(RBRoot *root)
{
    if (root)
        preorder(root->node);
}

/*
 * 中序遍历"红黑树"
 */
static void inorder(RBTree tree)
{
    if(tree != NULL)
    {
        inorder(tree->left);
        printf("%d ", tree->key);
        inorder(tree->right);
    }
}

void inorder_rbtree(RBRoot *root)
{
    if (root)
        inorder(root->node);
}

/*
 * 后序遍历"红黑树"
 */
static void postorder(RBTree tree)
{
    if(tree != NULL)
    {
        postorder(tree->left);
        postorder(tree->right);
        printf("%d ", tree->key);
    }
}

void postorder_rbtree(RBRoot *root)
{
    if (root)
        postorder(root->node);
}

/*
 * (递归实现)查找"红黑树x"中键值为key的节点
 */
static Node* search(RBTree x, Type key)
{
    if (x==NULL || x->key==key)
        return x;

    if (key < x->key)
        return search(x->left, key);
    else
        return search(x->right, key);
}
int rbtree_search(RBRoot *root, Type key)
{
    if (root)
        return search(root->node, key)? 0 : -1;
}

/*
 * (非递归实现)查找"红黑树x"中键值为key的节点
 */
static Node* iterative_search(RBTree x, Type key)
{
    while ((x!=NULL) && (x->key!=key))
    {
        if (key < x->key)
            x = x->left;
        else
            x = x->right;
    }

    return x;
}
int iterative_rbtree_search(RBRoot *root, Type key)
{
    if (root)
        return iterative_search(root->node, key) ? 0 : -1;
}

/*
 * 查找最小结点：返回tree为根结点的红黑树的最小结点。
 */
static Node* minimum(RBTree tree)
{
    if (tree == NULL)
        return NULL;

    while(tree->left != NULL)
        tree = tree->left;
    return tree;
}

int rbtree_minimum(RBRoot *root, int *val)
{
    Node *node;

    if (root)
        node = minimum(root->node);

    if (node == NULL)
        return -1;

    *val = node->key;
    return 0;
}

/*
 * 查找最大结点：返回tree为根结点的红黑树的最大结点。
 */
static Node* maximum(RBTree tree)
{
    if (tree == NULL)
        return NULL;

    while(tree->right != NULL)
        tree = tree->right;
    return tree;
}

int rbtree_maximum(RBRoot *root, int *val)
{
    Node *node;

    if (root)
        node = maximum(root->node);

    if (node == NULL)
        return -1;

    *val = node->key;
    return 0;
}

/*
 * 找结点(x)的后继结点。即，查找"红黑树中数据值大于该结点"的"最小结点"。
 */
static Node* rbtree_successor(RBTree x)
{
    // 如果x存在右孩子，则"x的后继结点"为 "以其右孩子为根的子树的最小结点"。
    if (x->right != NULL)
        return minimum(x->right);

    // 如果x没有右孩子。则x有以下两种可能：
    // (01) x是"一个左孩子"，则"x的后继结点"为 "它的父结点"。
    // (02) x是"一个右孩子"，则查找"x的最低的父结点，并且该父结点要具有左孩子"，找到的这个"最低的父结点"就是"x的后继结点"。
    Node* y = x->parent;
    while ((y!=NULL) && (x==y->right))
    {
        x = y;
        y = y->parent;
    }

    return y;
}

/*
 * 找结点(x)的前驱结点。即，查找"红黑树中数据值小于该结点"的"最大结点"。
 */
static Node* rbtree_predecessor(RBTree x)
{
    // 如果x存在左孩子，则"x的前驱结点"为 "以其左孩子为根的子树的最大结点"。
    if (x->left != NULL)
        return maximum(x->left);

    // 如果x没有左孩子。则x有以下两种可能：
    // (01) x是"一个右孩子"，则"x的前驱结点"为 "它的父结点"。
    // (01) x是"一个左孩子"，则查找"x的最低的父结点，并且该父结点要具有右孩子"，找到的这个"最低的父结点"就是"x的前驱结点"。
    Node* y = x->parent;
    while ((y!=NULL) && (x==y->left))
    {
        x = y;
        y = y->parent;
    }

    return y;
}

/*
 * 对红黑树的节点(x)进行左旋转
 *
 * 左旋示意图(对节点x进行左旋)：
 *      px                              px
 *     /                               /
 *    x                               y
 *   /  \      --(左旋)-->           / \                #
 *  lx   y                          x  ry
 *     /   \                       /  \
 *    ly   ry                     lx  ly
 *
 *
 */
static void rbtree_left_rotate(RBRoot *root, Node *x)
{
    // 设置x的右孩子为y
    Node *y = x->right;

    // 将 “y的左孩子” 设为 “x的右孩子”；
    // 如果y的左孩子非空，将 “x” 设为 “y的左孩子的父亲”
    x->right = y->left;
    if (y->left != NULL)
        y->left->parent = x;

    // 将 “x的父亲” 设为 “y的父亲”
    y->parent = x->parent;

    if (x->parent == NULL)
    {
        //tree = y;            // 如果 “x的父亲” 是空节点，则将y设为根节点
        root->node = y;            // 如果 “x的父亲” 是空节点，则将y设为根节点
    }
    else
    {
        if (x->parent->left == x)
            x->parent->left = y;    // 如果 x是它父节点的左孩子，则将y设为“x的父节点的左孩子”
        else
            x->parent->right = y;    // 如果 x是它父节点的左孩子，则将y设为“x的父节点的左孩子”
    }

    // 将 “x” 设为 “y的左孩子”
    y->left = x;
    // 将 “x的父节点” 设为 “y”
    x->parent = y;
}

/*
 * 对红黑树的节点(y)进行右旋转
 *
 * 右旋示意图(对节点y进行左旋)：
 *            py                               py
 *           /                                /
 *          y                                x
 *         /  \      --(右旋)-->            /  \                     #
 *        x   ry                           lx   y
 *       / \                                   / \                   #
 *      lx  rx                                rx  ry
 *
 */
static void rbtree_right_rotate(RBRoot *root, Node *y)
{
    // 设置x是当前节点的左孩子。
    Node *x = y->left;

    // 将 “x的右孩子” 设为 “y的左孩子”；
    // 如果"x的右孩子"不为空的话，将 “y” 设为 “x的右孩子的父亲”
    y->left = x->right;
    if (x->right != NULL)
        x->right->parent = y;

    // 将 “y的父亲” 设为 “x的父亲”
    x->parent = y->parent;

    if (y->parent == NULL)
    {
        //tree = x;            // 如果 “y的父亲” 是空节点，则将x设为根节点
        root->node = x;            // 如果 “y的父亲” 是空节点，则将x设为根节点
    }
    else
    {
        if (y == y->parent->right)
            y->parent->right = x;    // 如果 y是它父节点的右孩子，则将x设为“y的父节点的右孩子”
        else
            y->parent->left = x;    // (y是它父节点的左孩子) 将x设为“x的父节点的左孩子”
    }

    // 将 “y” 设为 “x的右孩子”
    x->right = y;

    // 将 “y的父节点” 设为 “x”
    y->parent = x;
}

/*
 * 红黑树插入修正函数
 *
 * 在向红黑树中插入节点之后(失去平衡)，再调用该函数；
 * 目的是将它重新塑造成一颗红黑树。
 *
 * 参数说明：
 *     root 红黑树的根
 *     node 插入的结点        // 对应《算法导论》中的z
 */
static void rbtree_insert_fixup(RBRoot *root, Node *node)
{
    Node *parent, *gparent;

    // 若“父节点存在，并且父节点的颜色是红色”
    while ((parent = rb_parent(node)) && rb_is_red(parent))
    {
        gparent = rb_parent(parent);

        //若“父节点”是“祖父节点的左孩子”
        if (parent == gparent->left)
        {
            // Case 1条件：叔叔节点是红色
            {
                Node *uncle = gparent->right;
                if (uncle && rb_is_red(uncle))
                {
                    rb_set_black(uncle);
                    rb_set_black(parent);
                    rb_set_red(gparent);
                    node = gparent;
                    continue;
                }
            }

            // Case 2条件：叔叔是黑色，且当前节点是右孩子
            if (parent->right == node)
            {
                Node *tmp;
                rbtree_left_rotate(root, parent);
                tmp = parent;
                parent = node;
                node = tmp;
            }

            // Case 3条件：叔叔是黑色，且当前节点是左孩子。
            rb_set_black(parent);
            rb_set_red(gparent);
            rbtree_right_rotate(root, gparent);
        }
        else//若“z的父节点”是“z的祖父节点的右孩子”
        {
            // Case 1条件：叔叔节点是红色
            {
                Node *uncle = gparent->left;
                if (uncle && rb_is_red(uncle))
                {
                    rb_set_black(uncle);
                    rb_set_black(parent);
                    rb_set_red(gparent);
                    node = gparent;
                    continue;
                }
            }

            // Case 2条件：叔叔是黑色，且当前节点是左孩子
            if (parent->left == node)
            {
                Node *tmp;
                rbtree_right_rotate(root, parent);
                tmp = parent;
                parent = node;
                node = tmp;
            }

            // Case 3条件：叔叔是黑色，且当前节点是右孩子。
            rb_set_black(parent);
            rb_set_red(gparent);
            rbtree_left_rotate(root, gparent);
        }
    }

    // 将根节点设为黑色
    rb_set_black(root->node);
}

/*
 * 添加节点：将节点(node)插入到红黑树中
 *
 * 参数说明：
 *     root 红黑树的根
 *     node 插入的结点        // 对应《算法导论》中的z
 */
static void rbtree_insert(RBRoot *root, Node *node)
{
    Node *y = NULL;
    Node *x = root->node;

    // 1. 将红黑树当作一颗二叉查找树，将节点添加到二叉查找树中。
    while (x != NULL)
    {
        y = x;
        if (node->key < x->key)
            x = x->left;
        else
            x = x->right;
    }
    rb_parent(node) = y;

    if (y != NULL)
    {
        if (node->key < y->key)
            y->left = node;                // 情况2：若“node所包含的值” < “y所包含的值”，则将node设为“y的左孩子”
        else
            y->right = node;            // 情况3：(“node所包含的值” >= “y所包含的值”)将node设为“y的右孩子”
    }
    else
    {
        root->node = node;                // 情况1：若y是空节点，则将node设为根
    }

    // 2. 设置节点的颜色为红色
    node->color = RED;

    // 3. 将它重新修正为一颗二叉查找树
    rbtree_insert_fixup(root, node);
}

/*
 * 创建结点
 *
 * 参数说明：
 *     key 是键值。
 *     parent 是父结点。
 *     left 是左孩子。
 *     right 是右孩子。
 */
static Node* create_rbtree_node(Type key, Node *parent, Node *left, Node* right)
{
    Node* p;

    if ((p = (Node *)malloc(sizeof(Node))) == NULL)
        return NULL;
    p->key = key;
    p->left = left;
    p->right = right;
    p->parent = parent;
    p->color = BLACK; // 默认为黑色

    return p;
}

/*
 * 新建结点(节点键值为key)，并将其插入到红黑树中
 *
 * 参数说明：
 *     root 红黑树的根
 *     key 插入结点的键值
 * 返回值：
 *     0，插入成功
 *     -1，插入失败
 */
int insert_rbtree(RBRoot *root, Type key)
{
    Node *node;    // 新建结点

    // 不允许插入相同键值的节点。
    // (若想允许插入相同键值的节点，注释掉下面两句话即可！)
    if (search(root->node, key) != NULL)
        return -1;

    // 如果新建结点失败，则返回。
    if ((node=create_rbtree_node(key, NULL, NULL, NULL)) == NULL)
        return -1;

    rbtree_insert(root, node);

    return 0;
}

/*
 * 红黑树删除修正函数
 *
 * 在从红黑树中删除插入节点之后(红黑树失去平衡)，再调用该函数；
 * 目的是将它重新塑造成一颗红黑树。
 *
 * 参数说明：
 *     root 红黑树的根
 *     node 待修正的节点
 */
static void rbtree_delete_fixup(RBRoot *root, Node *node, Node *parent)
{
    Node *other;

    while ((!node || rb_is_black(node)) && node != root->node)
    {
        if (parent->left == node)
        {
            other = parent->right;
            if (rb_is_red(other))
            {
                // Case 1: x的兄弟w是红色的
                rb_set_black(other);
                rb_set_red(parent);
                rbtree_left_rotate(root, parent);
                other = parent->right;
            }
            if ((!other->left || rb_is_black(other->left)) &&
                (!other->right || rb_is_black(other->right)))
            {
                // Case 2: x的兄弟w是黑色，且w的俩个孩子也都是黑色的
                rb_set_red(other);
                node = parent;
                parent = rb_parent(node);
            }
            else
            {
                if (!other->right || rb_is_black(other->right))
                {
                    // Case 3: x的兄弟w是黑色的，并且w的左孩子是红色，右孩子为黑色。
                    rb_set_black(other->left);
                    rb_set_red(other);
                    rbtree_right_rotate(root, other);
                    other = parent->right;
                }
                // Case 4: x的兄弟w是黑色的；并且w的右孩子是红色的，左孩子任意颜色。
                rb_set_color(other, rb_color(parent));
                rb_set_black(parent);
                rb_set_black(other->right);
                rbtree_left_rotate(root, parent);
                node = root->node;
                break;
            }
        }
        else
        {
            other = parent->left;
            if (rb_is_red(other))
            {
                // Case 1: x的兄弟w是红色的
                rb_set_black(other);
                rb_set_red(parent);
                rbtree_right_rotate(root, parent);
                other = parent->left;
            }
            if ((!other->left || rb_is_black(other->left)) &&
                (!other->right || rb_is_black(other->right)))
            {
                // Case 2: x的兄弟w是黑色，且w的俩个孩子也都是黑色的
                rb_set_red(other);
                node = parent;
                parent = rb_parent(node);
            }
            else
            {
                if (!other->left || rb_is_black(other->left))
                {
                    // Case 3: x的兄弟w是黑色的，并且w的左孩子是红色，右孩子为黑色。
                    rb_set_black(other->right);
                    rb_set_red(other);
                    rbtree_left_rotate(root, other);
                    other = parent->left;
                }
                // Case 4: x的兄弟w是黑色的；并且w的右孩子是红色的，左孩子任意颜色。
                rb_set_color(other, rb_color(parent));
                rb_set_black(parent);
                rb_set_black(other->left);
                rbtree_right_rotate(root, parent);
                node = root->node;
                break;
            }
        }
    }
    if (node)
        rb_set_black(node);
}

/*
 * 删除结点
 *
 * 参数说明：
 *     tree 红黑树的根结点
 *     node 删除的结点
 */
void rbtree_delete(RBRoot *root, Node *node)
{
    Node *child, *parent;
    int color;

    // 被删除节点的"左右孩子都不为空"的情况。
    if ( (node->left!=NULL) && (node->right!=NULL) )
    {
        // 被删节点的后继节点。(称为"取代节点")
        // 用它来取代"被删节点"的位置，然后再将"被删节点"去掉。
        Node *replace = node;

        // 获取后继节点
        replace = replace->right;
        while (replace->left != NULL)
            replace = replace->left;

        // "node节点"不是根节点(只有根节点不存在父节点)
        if (rb_parent(node))
        {
            if (rb_parent(node)->left == node)
                rb_parent(node)->left = replace;
            else
                rb_parent(node)->right = replace;
        }
        else
            // "node节点"是根节点，更新根节点。
            root->node = replace;

        // child是"取代节点"的右孩子，也是需要"调整的节点"。
        // "取代节点"肯定不存在左孩子！因为它是一个后继节点。
        child = replace->right;
        parent = rb_parent(replace);
        // 保存"取代节点"的颜色
        color = rb_color(replace);

        // "被删除节点"是"它的后继节点的父节点"
        if (parent == node)
        {
            parent = replace;
        }
        else
        {
            // child不为空
            if (child)
                rb_set_parent(child, parent);
            parent->left = child;

            replace->right = node->right;
            rb_set_parent(node->right, replace);
        }

        replace->parent = node->parent;
        replace->color = node->color;
        replace->left = node->left;
        node->left->parent = replace;

        if (color == BLACK)
            rbtree_delete_fixup(root, child, parent);
        free(node);

        return ;
    }

    if (node->left !=NULL)
        child = node->left;
    else
        child = node->right;

    parent = node->parent;
    // 保存"取代节点"的颜色
    color = node->color;

    if (child)
        child->parent = parent;

    // "node节点"不是根节点
    if (parent)
    {
        if (parent->left == node)
            parent->left = child;
        else
            parent->right = child;
    }
    else
        root->node = child;

    if (color == BLACK)
        rbtree_delete_fixup(root, child, parent);
    free(node);
}

/*
 * 删除键值为key的结点
 *
 * 参数说明：
 *     tree 红黑树的根结点
 *     key 键值
 */
void delete_rbtree(RBRoot *root, Type key)
{
    Node *z, *node;

    if ((z = search(root->node, key)) != NULL)
        rbtree_delete(root, z);
}

/*
 * 销毁红黑树
 */
static void rbtree_destroy(RBTree tree)
{
    if (tree==NULL)
        return ;

    if (tree->left != NULL)
        rbtree_destroy(tree->left);
    if (tree->right != NULL)
        rbtree_destroy(tree->right);

    free(tree);
}

void destroy_rbtree(RBRoot *root)
{
    if (root != NULL)
        rbtree_destroy(root->node);

    free(root);
}

/*
 * 打印"红黑树"
 *
 * tree       -- 红黑树的节点
 * key        -- 节点的键值
 * direction  --  0，表示该节点是根节点;
 *               -1，表示该节点是它的父结点的左孩子;
 *                1，表示该节点是它的父结点的右孩子。
 */
static void rbtree_print(RBTree tree, Type key, int direction)
{
    if(tree != NULL)
    {
        if(direction==0)    // tree是根节点
            printf("%2d(B) is root\n", tree->key);
        else                // tree是分支节点
            printf("%2d(%s) is %2d's %6s child\n", tree->key, rb_is_red(tree)?"R":"B", key, direction==1?"right" : "left");

        rbtree_print(tree->left, tree->key, -1);
        rbtree_print(tree->right,tree->key,  1);
    }
}

void print_rbtree(RBRoot *root)
{
    if (root!=NULL && root->node!=NULL)
        rbtree_print(root->node, root->node->key, 0);
}
```

**test.c**

```C
/**
 * C语言实现的红黑树(Red Black Tree)
 *
 * @author skywang
 * @date 2013/11/18
 */

#include <stdio.h>
#include "rbtree.h"

#define CHECK_INSERT 0    // "插入"动作的检测开关(0，关闭；1，打开)
#define CHECK_DELETE 0    // "删除"动作的检测开关(0，关闭；1，打开)
#define LENGTH(a) ( (sizeof(a)) / (sizeof(a[0])) )

void main()
{
    int a[] = {10, 40, 30, 60, 90, 70, 20, 50, 80};
    int i, ilen=LENGTH(a);
    RBRoot *root=NULL;

    root = create_rbtree();
    printf("== 原始数据: ");
    for(i=0; i<ilen; i++)
        printf("%d ", a[i]);
    printf("\n");

    for(i=0; i<ilen; i++)
    {
        insert_rbtree(root, a[i]);
#if CHECK_INSERT
        printf("== 添加节点: %d\n", a[i]);
        printf("== 树的详细信息: \n");
        print_rbtree(root);
        printf("\n");
#endif
    }

    printf("== 前序遍历: ");
    preorder_rbtree(root);

    printf("\n== 中序遍历: ");
    inorder_rbtree(root);

    printf("\n== 后序遍历: ");
    postorder_rbtree(root);
    printf("\n");

    if (rbtree_minimum(root, &i)==0)
        printf("== 最小值: %d\n", i);
    if (rbtree_maximum(root, &i)==0)
        printf("== 最大值: %d\n", i);
    printf("== 树的详细信息: \n");
    print_rbtree(root);
    printf("\n");

#if CHECK_DELETE
    for(i=0; i<ilen; i++)
    {
        delete_rbtree(root, a[i]);

        printf("== 删除节点: %d\n", a[i]);
        if (root)
        {
            printf("== 树的详细信息: \n");
            print_rbtree(root);
            printf("\n");
        }
    }
#endif

    destroy_rbtree(root);
}
```

## 7、分块查找

### 7.1、基本原理

分块查找（Block Search）是一种基于分块思想的查找算法。它将待查找的数据集合分成多个块（Block），每个块内的数据按照某种方式有序排列。这样就可以在查找时快速缩小查找范围，从而提高查找效率。

分块查找的基本原理如下：

1. 将待查找的数据分成若干块，并将每块内的数据按照某种方式有序排列。
2. 确定各块的范围和关键字，用一张索引表来存放各块的关键字和起始地址。
3. 查找时，先在索引表中二分查找到关键字所在的块，然后在该块内进行线性查找，直到找到目标数据。

分块查找的注意事项和应用场景如下：

1. 数据分块要尽量均匀，使每块数据量大致相等。
2. 对每个块内的数据要按照某种方式有序排列，以便进行二分查找。
3. 适合数据集合变动较少的情况，如果数据频繁变动，需要不断重构索引表，效率较低。
4. 分块查找适合于数据量较大，但又不适合全部加载到内存中的情况，比如外部文件查找。

### 7.2、代码示例

```C
#include <stdio.h>

// 定义块的大小
#define BLOCK_SIZE 3

// 分块查找函数
int blockSearch(int arr[], int n, int key)
{
    // 计算块的数量
    int blockCount = (n + BLOCK_SIZE - 1) / BLOCK_SIZE;

    // 创建一个块数组，存储每个块的最大值
    int blockMax[blockCount];
    for (int i = 0; i < blockCount; i++) {
        int max = arr[i * BLOCK_SIZE];
        for (int j = 1; j < BLOCK_SIZE && i * BLOCK_SIZE + j < n; j++) {
            if (arr[i * BLOCK_SIZE + j] > max) {
                max = arr[i * BLOCK_SIZE + j];
            }
        }
        blockMax[i] = max;
    }

    // 在块数组中查找key所在的块
    int blockIndex = 0;
    while (blockIndex < blockCount && blockMax[blockIndex] < key) {
        blockIndex++;
    }

    // 在块中进行线性查找
    int startIndex = blockIndex * BLOCK_SIZE;
    int endIndex = startIndex + BLOCK_SIZE;
    for (int i = startIndex; i < endIndex && i < n; i++) {
        if (arr[i] == key) {
            return i;
        }
    }

    return -1;
}

int main()
{
    int arr[] = {2, 4, 6, 8, 10, 12, 14, 16, 18, 20};
    int n = sizeof(arr) / sizeof(arr[0]);

    int key = 12;
    int index = blockSearch(arr, n, key);
    if (index == -1) {
        printf("%d not found\n", key);
    } else {
        printf("%d found at index %d\n", key, index);
    }

    return 0;
}
```

该程序定义了一个 `BLOCK_SIZE` 常量，表示块的大小。在分块查找函数中，首先计算块的数量，然后创建一个块数组，存储每个块的最大值。接下来，在块数组中查找key所在的块，并在该块中进行线性查找。如果找到了key，则返回其下标，否则返回-1。最后，程序使用一个示例数组来测试分块查找函数，查找数组中的一个元素并输出结果。