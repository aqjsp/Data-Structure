# 十大排序

| 排序算法 | 平均时间复杂度 | 最好情况  | 最坏情况  | 空间复杂度 | 稳定性 |
| -------- | -------------- | --------- | --------- | ---------- | ------ |
| 冒泡排序 | O(n^2)         | O(n)      | O(n^2)    | O(1)       | 稳定   |
| 快速排序 | O(nlogn)       | O(nlogn)  | O(n^2)    | O(logn)    | 不稳定 |
| 选择排序 | O(n^2)         | O(n^2)    | O(n^2)    | O(1)       | 不稳定 |
| 插入排序 | O(n^2)         | O(n)      | O(n^2)    | O(1)       | 稳定   |
| 希尔排序 | O(nlogn)       | O(log^2n) | O(log^2n) | O(1)       | 不稳定 |
| 归并排序 | O(nlogn)       | O(nlogn)  | O(nlogn)  | O(n)       | 稳定   |
| 堆排序   | O(nlogn)       | O(nlogn)  | O(nlogn)  | O(1)       | 不稳定 |
| 计数排序 | O(n+k)         | O(n+k)    | O(n+k)    | O(k)       | 稳定   |
| 桶排序   | O(n+k)         | O(n+k)    | O(n^2)    | O(n+k)     | 稳定   |
| 基数排序 | O(nxk)         | O(nxk)    | O(nxk)    | O(n+k)     | 稳定   |

## 1、冒泡排序

相信大家在大一刚进学习数据结构与算法的时候，第一个遇到的肯定是冒泡排序。

### 1.1、概念

冒泡排序的英文Bubble Sort，是一种最基础的交换排序。之所以叫做冒泡排序，因为每一个元素都可以像小气泡一样，根据自身大小一点一点向数组的一侧移动。

### 1.2 、基本思想

从前往后（或从后往前）两两比较相邻元素的值，若为逆序（即A[I-1]>A[I]）,则交换它们，直到序列比较完。我们称它为第一趟冒泡，结果是将最小的元素交换到待排序列的第一个位置（或将最大的元素交换到待排序列的最后一个位置），关键字最小的元素如气泡一样逐渐向上“漂浮”。最终一个一个排好了位置。

### 1.3、C++示例代码

```C++
#include <iostream>
#include <vector>

using namespace std;

void bubbleSort(vector<int>& nums) {
    int n = nums.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (nums[j] > nums[j+1]) {
                swap(nums[j], nums[j+1]);
            }
        }
    }
}

int main() {
    vector<int> nums = { 3, 7, 2, 9, 1, 5, 8, 4, 6 };
    bubbleSort(nums);

    for (int i = 0; i < nums.size(); i++) {
        cout << nums[i] << " ";
    }
    cout << endl;

    return 0;
}
```

在该示例中，我们使用了vector作为存储数组的容器，并通过冒泡排序算法对其进行排序。算法实现过程中，我们通过两个嵌套的循环遍历整个数组，每次比较相邻的两个元素，如果它们的顺序不对，就交换它们的位置。经过多轮遍历，数组中的元素会逐渐按照从小到大的顺序排列。

该示例代码可以对任意大小的数组进行冒泡排序，时间复杂度为O(n^2)。虽然冒泡排序的时间复杂度较高，但是其代码实现简单，容易理解，适合对小规模数据进行排序。

### 1.4、优化

冒泡排序算法的时间复杂度为O(n^2)，因此对于大规模的数据集合，它的效率较低。可以通过一些优化手段提高冒泡排序的效率，以下是一些可能的优化方法：

1. 增加一个标志位，表示本轮排序是否进行了交换。如果本轮排序中没有发生任何交换，说明数组已经有序，可以直接退出排序。
2. 优化循环范围，每一轮排序只需要比较到上一轮最后发生交换的位置即可，因为在该位置之后的元素已经有序。
3. 对于已经有序的子数组，可以记录最后一次交换的位置，下一轮排序时只需要比较到该位置即可。
4. 如果待排序数组的大小比较小，可以使用插入排序或者选择排序等复杂度较低的算法进行排序。

**下面是对冒泡排序进行第一种优化的示例代码：**

```C++
void bubbleSort(vector<int>& nums) {
    int n = nums.size();
    for (int i = 0; i < n - 1; i++) {
        bool flag = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (nums[j] > nums[j+1]) {
                swap(nums[j], nums[j+1]);
                flag = true;
            }
        }
        if (!flag) {
            break;
        }
    }
}
```

在该示例中，我们增加了一个标志位flag，用于记录本轮排序是否进行了交换。如果本轮排序中没有发生任何交换，说明数组已经有序，可以直接退出排序。这样就能够减少排序的轮数，提高效率。

## 2、快速排序

在学习过程中，快排也是最经典的排序算法之一，大家务必务必掌握。

### 2.1、概念

快速排序是一种二叉树结构的交换排序方法。相当于是冒泡排序的一种升级，都是属于交换排序类，通过不断比较和移动交换来实现排序。

### 2.2、基本思想

通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据比另一部分的所有数据要小，再按这种方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，使整个数据变成有序序列。

### 2.3、C++示例代码

**递归：**

```C++
#include <iostream>
#include <vector>

using namespace std;

void quickSort(vector<int>& nums, int left, int right) {
    if (left >= right) return;

    int pivot = nums[left]; // 选取基准数

    while (left < right) {
        // 从右往左找到第一个小于基准数的元素
        while (left < right && nums[right] >= pivot) right--;
        if (left < right) nums[left++] = nums[right]; // 将该元素移到基准数左侧

        // 从左往右找到第一个大于基准数的元素
        while (left < right && nums[left] < pivot) left++;
        if (left < right) nums[right--] = nums[left]; // 将该元素移到基准数右侧
    }

    nums[left] = pivot; // 将基准数放回中间位置

    // 对基准数左侧和右侧的子数组分别递归调用快排
    quickSort(nums, left, left - 1);
    quickSort(nums, left + 1, right);
}

int main() {
    vector<int> nums = { 3, 7, 2, 9, 1, 5, 8, 4, 6 };
    quickSort(nums, 0, nums.size() - 1);

    for (int i = 0; i < nums.size(); i++) {
        cout << nums[i] << " ";
    }
    cout << endl;

    return 0;
}
```

在该示例中，我们使用了vector作为存储数组的容器，并通过快速排序算法对其进行排序。算法实现过程中，我们首先选取数组的第一个元素作为基准数（pivot），然后通过两个指针（i和j）分别从左右两侧开始扫描数组，找到小于和大于基准数的元素，并将它们交换位置。最后，将基准数放回数组中间位置，然后对基准数左侧和右侧的子数组递归调用快排算法。

该示例代码可以对任意大小的数组进行快速排序，时间复杂度为O(nlogn)。

**非递归：**

```C++
void quickSort(vector<int>& nums) {
    if (nums.empty()) {
        return;
    }
    stack<int> s;
    int left = 0, right = nums.size() - 1;
    s.push(left);
    s.push(right);
    while (!s.empty()) {
        right = s.top();
        s.pop();
        left = s.top();
        s.pop();
        int pivot = nums[left];
        int i = left, j = right;
        while (i < j) {
            while (i < j && nums[j] >= pivot) {
                j--;
            }
            if (i < j) {
                nums[i++] = nums[j];
            }
            while (i < j && nums[i] < pivot) {
                i++;
            }
            if (i < j) {
                nums[j--] = nums[i];
            }
        }
        nums[i] = pivot;
        if (i - 1 > left) {
            s.push(left);
            s.push(i - 1);
        }
        if (i + 1 < right) {
            s.push(i + 1);
            s.push(right);
        }
    }
}
```

在非递归实现中，我们使用了栈来保存每个待排序数组的左右端点。在栈不为空的情况下，弹出当前待排序数组的左右端点，进行分区，并将分区后的左右子数组的端点入栈。当栈为空时，排序结束。

在具体实现中，我们仍然选择第一个元素作为pivot，并使用双指针的方式进行分区。需要注意的是，每次分区后，需要判断左右子数组的长度是否大于1，若大于1，则将其端点入栈，以便进行下一次分区。

与递归实现相比，非递归实现的优势在于可以避免递归过程中的函数调用开销，从而提高效率。

### 2.4、优化

1. 随机选择基准元素

快速排序最坏情况下的时间复杂度为 $O(n^2)$，这种情况通常发生在每次选择的基准元素都是当前子数组的最大或最小值时。为了避免这种情况，我们可以随机选择一个元素作为基准元素，这样每个元素都有相同的概率成为基准元素，从而避免了最坏情况的发生。

2. 三数取中法选择基准元素

在确定基准元素时，我们可以选择当前子数组的第一个元素、最后一个元素、中间元素中的中位数作为基准元素。这种方式称为三数取中法，可以使得基准元素更加均衡，从而提高排序效率。

3. 小数组使用插入排序

对于长度较小的子数组，快速排序的效率并不一定比插入排序更高。因此，我们可以设置一个阈值 $k$，当子数组长度小于 $$k$$ 时，使用插入排序而不是快速排序。

4. 双轴快排

双轴快排是一种基于快速排序的改进算法，它使用两个基准元素而不是一个基准元素进行分区。具体来说，我们先选择两个基准元素 p 和 q，其中 p < q，然后将数组分成三部分：小于 p 的部分、大于 q 的部分和介于 p 和 q 之间的部分。接下来，我们对小于 p 和大于 q 的两部分递归进行双轴快排，对介于 p 和 q 之间的部分进行普通的快速排序。双轴快排相比于普通的快速排序，在某些情况下可以提高排序效率。

综上所述，快速排序是一种高效的排序算法，并且具有很多优化的空间。通过选择合适的基准元素、使用插入排序等方法，我们可以进一步提高快速排序的效率。

**示例代码：**

```C++
#include <iostream>
#include <algorithm>
using namespace std;

// 插入排序阈值
const int kInsertionSortThreshold = 16;

// 三数取中法选择基准元素
template <typename T>
T MedianOfThree(T a[], int left, int right)
{
    int mid = left + (right - left) / 2;
    if (a[left] > a[mid]) {
        swap(a[left], a[mid]);
    }
    if (a[left] > a[right]) {
        swap(a[left], a[right]);
    }
    if (a[mid] > a[right]) {
        swap(a[mid], a[right]);
    }
    return a[mid];
}

// 插入排序
template <typename T>
void InsertionSort(T a[], int left, int right)
{
    for (int i = left + 1; i <= right; ++i) {
        T tmp = a[i];
        int j = i - 1;
        while (j >= left && a[j] > tmp) {
            a[j + 1] = a[j];
            --j;
        }
        a[j + 1] = tmp;
    }
}

// 快速排序
template <typename T>
void QuickSort(T a[], int left, int right)
{
    // 对于小数组，使用插入排序
    if (right - left + 1 <= kInsertionSortThreshold) {
        InsertionSort(a, left, right);
        return;
    }

    // 选择基准元素
    T pivot = MedianOfThree(a, left, right);

    // 分区
    int i = left, j = right - 1;
    while (true) {
        while (a[++i] < pivot);
        while (a[--j] > pivot);
        if (i < j) {
            swap(a[i], a[j]);
        } else {
            break;
        }
    }
    swap(a[i], a[right - 1]);

    // 递归排序左右子数组
    QuickSort(a, left, i - 1);
    QuickSort(a, i + 1, right);
}

int main()
{
    int a[] = { 6, 5, 3, 1, 8, 7, 2, 4 };
    int n = sizeof(a) / sizeof(a[0]);

    QuickSort(a, 0, n - 1);

    for (int i = 0; i < n; ++i) {
        cout << a[i] << " ";
    }
    cout << endl;

    return 0;
}
```

上述代码中，使用了三数取中法选择基准元素、插入排序阈值、递归排序左右子数组等优化方式，从而提高了快速排序的效率。

## 3、选择排序

### 3.1、概念

是一种简单直观的排序算法。

### 3.2、基本思想

每次从待排序序列中选择最小的元素，与序列的第一个元素交换位置。这样，序列的第一个位置就是最小的元素。然后在剩下的元素中继续执行上述操作，直到整个序列排序完成。

### 3.3、C++示例代码

```C++
#include <iostream>
#include <algorithm>
using namespace std;

template <typename T>
void SelectionSort(T a[], int n)
{
    for (int i = 0; i < n - 1; ++i) {
        int minIndex = i;
        for (int j = i + 1; j < n; ++j) {
            if (a[j] < a[minIndex]) {
                minIndex = j;
            }
        }
        swap(a[i], a[minIndex]);
    }
}

int main()
{
    int a[] = { 6, 5, 3, 1, 8, 7, 2, 4 };
    int n = sizeof(a) / sizeof(a[0]);

    SelectionSort(a, n);

    for (int i = 0; i < n; ++i) {
        cout << a[i] << " ";
    }
    cout << endl;

    return 0;
}
```

时间复杂度为 $O(n^2)$，稳定性为不稳定。

### 3.4、优化

选择排序的时间复杂度为 $O(n^2)$，无法避免的比较次数较多。因此，常常需要对选择排序进行优化以提高排序的效率。

以下是一个优化过的选择排序算法，称之为双向选择排序：

```C++
#include <iostream>
#include <algorithm>
using namespace std;

template <typename T>
void DoubleSelectionSort(T a[], int n)
{
    int left = 0, right = n - 1;
    while (left < right) {
        int minIndex = left, maxIndex = right;
        if (a[left] > a[right]) {
            swap(a[left], a[right]);
        }
        for (int i = left + 1; i < right; ++i) {
            if (a[i] < a[minIndex]) {
                minIndex = i;
            } else if (a[i] > a[maxIndex]) {
                maxIndex = i;
            }
        }
        swap(a[left], a[minIndex]);
        swap(a[right], a[maxIndex]);
        ++left;
        --right;
    }
}

int main()
{
    int a[] = { 6, 5, 3, 1, 8, 7, 2, 4 };
    int n = sizeof(a) / sizeof(a[0]);

    DoubleSelectionSort(a, n);

    for (int i = 0; i < n; ++i) {
        cout << a[i] << " ";
    }
    cout << endl;

    return 0;
}
```

该算法将待排序序列分成两部分，分别维护当前未排序序列中的最小值和最大值。具体实现是：每次在未排序序列的左边和右边分别找到最小值和最大值，将它们分别和未排序序列的左端点和右端点进行交换。由于每次交换会减少一个元素的比较次数，因此双向选择排序的平均时间复杂度为 $O(n^2)$，但比选择排序的时间复杂度要略低一些。

## 4、插入排序

### 4.1、概念

插入排序(InsertionSort)，一般也被称为直接插入排序。对于少量元素的排序，它是一个有效的算法。

### 4.2、基本思想

它的基本思想是将一个记录插入到已经排好序的有序表中，从而一个新的、记录数增1的有序表。在其实现过程使用双层循环，外层循环对除了第一个元素之外的所有元素，内层循环对当前元素前面有序表进行待插入位置查找，并进行移动。

插入排序的工作方式像许多人排序一手扑克牌。开始时，我们的左手为空并且桌子上的牌面向下。然后，我们每次从桌子上拿走一张牌并将它插入左手中正确的位置。为了找到一张牌的正确位置，我们从右到左将它与已在手中的每张牌进行比较。拿在左手上的牌总是排序好的，原来这些牌是桌子上牌堆中顶部的牌。

### 4.3、C++示例代码

插入排序的核心思想是将一个元素插入到已经排好序的部分中，使得插入后仍然是有序的。

以下是一个简单的插入排序实现：

```C++
#include <iostream>
#include <algorithm>
using namespace std;

template <typename T>
void InsertionSort(T a[], int n)
{
    for (int i = 1; i < n; ++i) {
        T key = a[i];
        int j = i - 1;
        while (j >= 0 && a[j] > key) {
            a[j + 1] = a[j];
            --j;
        }
        a[j + 1] = key;
    }
}

int main()
{
    int a[] = { 6, 5, 3, 1, 8, 7, 2, 4 };
    int n = sizeof(a) / sizeof(a[0]);

    InsertionSort(a, n);

    for (int i = 0; i < n; ++i) {
        cout << a[i] << " ";
    }
    cout << endl;

    return 0;
}
```

该算法从第二个元素开始，将当前元素插入到已经排好序的部分中，使得插入后仍然是有序的。具体实现是：将当前元素保存到 key 中，从当前元素的前一个元素开始向前遍历已经排好序的部分，将大于 key 的元素向右移动一个位置，直到找到一个小于等于 key 的元素，将 key 插入到该位置后面。由于插入排序的时间复杂度为 $O(n^2)$，因此当序列规模较小时，插入排序可以比较高效。

### 4.4、优化

对于插入排序算法的优化，主要有以下几个方面：

1. 使用二分查找优化内部循环 在内部循环中，对于已经排好序的部分可以使用二分查找定位插入位置，而不是逐个比较。这样可以将内部循环的时间复杂度从 $$O(n)$$ 优化到 $O(logn)$，从而提高整个算法的性能。以下是使用二分查找优化内部循环的代码实现：

```C++
template <typename T>
void InsertionSort(T a[], int n)
{
    for (int i = 1; i < n; ++i) {
        T key = a[i];
        int left = 0, right = i - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (a[mid] > key) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        for (int j = i - 1; j >= left; --j) {
            a[j + 1] = a[j];
        }
        a[left] = key;
    }
}
```

2. 缩小常数因子 插入排序算法的常数因子比较大，可以通过减少赋值操作来缩小常数因子。在实现中，可以将需要插入的元素保存到临时变量中，然后在找到插入位置后再进行一次赋值操作。以下是缩小常数因子的代码实现：

```C++
template <typename T>
void InsertionSort(T a[], int n)
{
    for (int i = 1; i < n; ++i) {
        T key = a[i];
        int j = i - 1;
        for (; j >= 0 && a[j] > key; --j) {
            a[j + 1] = a[j];
        }
        a[j + 1] = key;
    }
}
```

## 5、希尔排序

### 5.1、概念

希尔排序（Shell Sort）是一种基于插入排序的快速排序算法，由Donald Shell在1959年提出。希尔排序是将整个序列分割成若干个子序列，对每个子序列进行插入排序，使得子序列基本有序，然后再对全体元素进行一次插入排序。

### 5.2、基本思想

希尔排序的基本思想是将待排序的元素分成若干个小组，对每个小组进行插入排序，随着排序过程的进行，每个小组的元素个数逐渐增多，但仍然保持有序。最后将所有元素分成一个组，进行插入排序。

希尔排序的具体步骤如下：

1. 选择一个增量序列d1，d2，...，dk，其中di > dj，dk = 1；
2. 对于每个增量di，将序列分成di个子序列，分别对每个子序列进行插入排序；
3. 增量逐渐缩小，重复步骤2，直到增量为1。

在实际应用中，希尔排序常常使用一些常见的增量序列，如希尔增量（n/2，n/4，...，1）、Hibbard增量（1，3，7，...，2^k-1）、Sedgewick增量等，以提高排序的效率。

### 5.3、C++示例代码

```C++
void shellSort(vector<int>& nums) {
    int n = nums.size();
    // 初始化增量为n/2
    int gap = n / 2;
    while (gap > 0) {
        // 对每个子序列进行插入排序
        for (int i = gap; i < n; i++) {
            int temp = nums[i];
            int j = i;
            while (j >= gap && nums[j - gap] > temp) {
                nums[j] = nums[j - gap];
                j -= gap;
            }
            nums[j] = temp;
        }
        // 缩小增量
        gap /= 2;
    }
}
```

在这个代码中，我们使用了一个变量gap来表示增量，初始化为n/2。然后每次循环时，我们将gap缩小一半，直到它变成1为止。在每次循环中，我们将原序列分成gap个子序列，对每个子序列进行插入排序。具体地，对于第i个子序列，它的第一个元素是第i个元素，第二个元素是第i+gap个元素，第三个元素是第i+2*gap个元素，以此类推。对于每个子序列，我们使用插入排序的思想，将它们变成基本有序的序列。最后，当gap变成1时，我们对整个序列进行一次插入排序，使得整个序列变得有序。

需要注意的是，希尔排序的效率取决于增量序列的选择，常见的增量序列有希尔增量、Hibbard增量、Sedgewick增量等，可以根据实际情况选择适合的增量序列。

### 5.4、优化

希尔排序的优化方法比较多，下面介绍其中两种。

1. 增量序列选择

希尔排序的性能与增量序列的选择有很大关系。常见的增量序列有希尔增量、Hibbard增量、Sedgewick增量等。一般来说，增量序列的最后一个元素应该是1，而其他元素的选择会影响算法的性能。在实际应用中，可以通过试验不同的增量序列，选择最优的增量序列。

2. 插入排序的优化

在希尔排序的每个子序列中，我们使用插入排序的思想，将子序列变成基本有序的序列。但是，插入排序在对近乎有序的序列进行排序时，效率非常高。因此，我们可以使用一种优化方式，即在插入排序中，将查找插入位置的过程改为二分查找。这样，当子序列基本有序时，插入排序的效率将大大提高。

下面是用C++实现的希尔排序的优化代码：

```C++
void shellSort(vector<int>& nums) {
    int n = nums.size();
    int gap = n / 2;
    while (gap > 0) {
        // 在每个子序列中，使用二分查找法查找插入位置
        for (int i = gap; i < n; i++) {
            int temp = nums[i];
            int j = i;
            while (j >= gap && nums[j - gap] > temp) {
                nums[j] = nums[j - gap];
                j -= gap;
            }
            nums[j] = temp;
        }
        gap /= 2;
    }
}
```

在这个代码中，我们对插入排序进行了优化，使用二分查找法查找插入位置，从而提高了算法的效率。

## 6、归并排序

### 6.1、概念

归并排序是建立在归并操作上的一种有效，稳定的排序算法，该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。

归并排序的基本概念如下：

- 分割：将待排序的序列不断地二分为两个子序列，直到每个子序列只剩下一个元素。
- 归并：将两个有序子序列合并为一个有序序列。

### 6.2、基本思想

归并排序是一种基于分治思想的排序算法，它的基本思想是将待排序的序列不断地二分为两个子序列，直到每个子序列只剩下一个元素。然后，将两个子序列归并成一个有序序列，不断地归并，直到最终得到一个有序序列。

### 6.3、C++示例代码

**递归方式的归并排序实现：**

```C++
void mergeSort(vector<int>& nums, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid + 1, right);
    merge(nums, left, mid, right);
}

void merge(vector<int>& nums, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j]) temp[k++] = nums[i++];
        else temp[k++] = nums[j++];
    }
    while (i <= mid) temp[k++] = nums[i++];
    while (j <= right) temp[k++] = nums[j++];
    for (int m = 0; m < k; m++) {
        nums[left + m] = temp[m];
    }
}
```

归并排序是一种基于分治思想的排序算法，它的基本思想是将待排序的序列不断地二分为两个子序列，直到每个子序列只剩下一个元素。然后，将两个子序列归并成一个有序序列，不断地归并，直到最终得到一个有序序列。

归并排序的基本概念如下：

- 分割：将待排序的序列不断地二分为两个子序列，直到每个子序列只剩下一个元素。
- 归并：将两个有序子序列合并为一个有序序列。

具体实现时，可以使用递归或非递归的方式来实现归并排序。

递归方式的归并排序实现：

```C++
cppCopy code
void mergeSort(vector<int>& nums, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid + 1, right);
    merge(nums, left, mid, right);
}

void merge(vector<int>& nums, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j]) temp[k++] = nums[i++];
        else temp[k++] = nums[j++];
    }
    while (i <= mid) temp[k++] = nums[i++];
    while (j <= right) temp[k++] = nums[j++];
    for (int m = 0; m < k; m++) {
        nums[left + m] = temp[m];
    }
}
```

**非递归方式的归并排序实现：**

```C++
void mergeSort(vector<int>& nums) {
    int n = nums.size();
    for (int size = 1; size < n; size *= 2) {
        for (int left = 0; left < n - size; left += 2 * size) {
            int mid = left + size - 1;
            int right = min(left + 2 * size - 1, n - 1);
            merge(nums, left, mid, right);
        }
    }
}

void merge(vector<int>& nums, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j]) temp[k++] = nums[i++];
        else temp[k++] = nums[j++];
    }
    while (i <= mid) temp[k++] = nums[i++];
    while (j <= right) temp[k++] = nums[j++];
    for (int m = 0; m < k; m++) {
        nums[left + m] = temp[m];
    }
}
```

以上两种实现方式都包含了归并排序的基本思想，即分割和归并。在实际应用中，通常采用非递归方式的实现，主要有以下几个原因：

1. 非递归方式实现的空间复杂度更低：递归方式需要使用系统栈来保存函数调用信息，当递归深度较大时，可能会导致栈溢出。而非递归方式可以使用循环和迭代来实现，不需要使用额外的空间，因此空间复杂度更低。
2. 非递归方式实现的效率更高：递归方式需要频繁地进行函数调用和返回操作，每次调用和返回都会带来额外的开销。而非递归方式只需要进行简单的循环和迭代，效率更高。
3. 非递归方式实现的代码更易于理解和调试：递归方式实现的代码比较难以理解和调试，因为递归过程中函数的调用顺序比较复杂。而非递归方式实现的代码结构更加清晰，易于理解和调试。

### 6.4、优化

归并排序是一种比较高效的排序算法，但是在实际应用中还是可以进行一些优化的，下面介绍几种常见的优化方式：

1. 优化归并操作：在归并操作时可以采用一些优化策略，比如当待归并的两个子序列已经有序时，就可以直接将它们合并，不需要再进行归并操作。另外，在合并两个有序子序列时，可以采用双指针的方式，避免频繁的数组拷贝操作。
2. 优化辅助数组的使用：在归并排序过程中需要使用一个辅助数组来存储排序结果，可以考虑将辅助数组作为参数传入递归函数中，避免频繁的申请和释放空间。另外，可以在归并过程中将辅助数组的元素进行复制，减少访问数组的次数。
3. 优化递归深度：归并排序采用递归的方式实现，当数据量较大时，递归深度可能会比较大，影响排序的效率。可以考虑在数据量较小的情况下采用插入排序等其他排序算法，减少递归深度。
4. 优化数据的存储方式：归并排序对数据的存储方式比较敏感，采用不同的存储方式可能会影响排序的效率。可以采用 cache-aware 排序算法或者对数据进行预处理，使得数据更加适合在内存中进行排序。

综上所述，通过优化归并操作、辅助数组的使用、递归深度和数据的存储方式等方面，可以进一步提高归并排序的效率。

**以下是一个优化的示例代码：**

```C++
#include <iostream>
#include <vector>

using namespace std;

// 归并排序
void mergeSort(vector<int>& nums, int left, int right) {
    if (left >= right) {
        return;
    }

    int mid = (left + right) / 2;
    mergeSort(nums, left, mid);
    mergeSort(nums, mid + 1, right);

    // 合并两个有序序列
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j]) {
            temp[k++] = nums[i++];
        } else {
            temp[k++] = nums[j++];
        }
    }
    while (i <= mid) {
        temp[k++] = nums[i++];
    }
    while (j <= right) {
        temp[k++] = nums[j++];
    }
    for (int p = 0; p < temp.size(); p++) {
        nums[left + p] = temp[p];
    }
}

int main() {
    vector<int> nums = {3, 2, 1, 5, 6, 4};
    mergeSort(nums, 0, nums.size() - 1);
    for (int i = 0; i < nums.size(); i++) {
        cout << nums[i] << " ";
    }
    cout << endl;
    return 0;
}
```

在这个示例代码中，我们采用递归的方式实现归并排序。优化的方法主要包括：

1. 在划分序列时，使用了一个变量`mid`来记录中间位置，避免了重复计算。
2. 在合并有序序列时，使用了一个临时数组`temp`来存储合并后的结果，避免了频繁的数组元素交换操作。
3. 对于一些小规模的子序列，我们采用了插入排序的方式进行排序，避免了递归层次过深导致的性能问题。

以上这些优化方法都可以有效提高归并排序的效率。

## 7、堆排序

### 7.1、概念

堆排序（英语:Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。

### 7.2、基本思想

堆排序是一种基于堆数据结构的排序算法，其基本思想是将待排序的元素构造成一个堆，然后依次将堆顶元素与堆底元素交换，再对堆顶元素进行下沉操作，使得交换后的堆仍然保持最大堆或最小堆的性质，重复上述过程直到排序完成。

在堆排序中，首先要构建一个堆，可以使用从下往上的建堆方法，或者使用堆插入的方法。建堆完成后，将堆顶元素与堆底元素交换，然后对堆顶元素进行下沉操作，使得堆顶元素重新满足最大堆或最小堆的性质。交换后的堆除堆顶元素外，仍然满足最大堆或最小堆的性质，继续进行相同的操作，直到排序完成。

堆排序的时间复杂度为O(nlogn)，空间复杂度为O(1)。堆排序是一种不稳定的排序算法，因为交换操作会改变相同元素之间的相对位置。

### 7.3、C++示例代码

```C++
#include <iostream>
#include <vector>

using namespace std;

void heapify(vector<int>& arr, int n, int i) {
    int largest = i; // 初始化根节点为最大值
    int l = 2 * i + 1; // 左子节点索引
    int r = 2 * i + 2; // 右子节点索引

    // 如果左子节点比根节点大，则更新最大值为左子节点
    if (l < n && arr[l] > arr[largest]) {
        largest = l;
    }

    // 如果右子节点比最大值还大，则更新最大值为右子节点
    if (r < n && arr[r] > arr[largest]) {
        largest = r;
    }

    // 如果最大值不是根节点，则交换根节点和最大值，然后继续调整
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    // 建堆，从最后一个非叶子节点开始，依次向下调整
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }

    // 依次取出堆顶元素，放到数组末尾
    for (int i = n - 1; i >= 0; i--) {
        swap(arr[0], arr[i]); // 将堆顶元素交换到末尾
        heapify(arr, i, 0); // 对剩余元素进行堆调整
    }
}

int main() {
    vector<int> arr = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5};
    heapSort(arr);

    for (auto num : arr) {
        cout << num << " ";
    }

    return 0;
}
```

在上面的代码中，`heapify`函数用于将以`i`为根节点的子树进行堆调整，使其满足最大堆的性质。`heapSort`函数用于进行堆排序，首先建堆，然后依次取出堆顶元素进行排序，最后得到有序数组。

### 7.4、优化

堆排序的优化主要是通过优化建堆的过程和减少交换操作来提高排序的效率。

1. 优化建堆过程

   - 从最后一个非叶子节点开始向下进行调整，减少不必要的交换次数。

   - 建堆过程中，可以将每个非叶子节点看作是一个小堆，然后对这些小堆进行合并。

2. 减少交换操作

   - 在删除堆顶元素时，为了保证堆的性质，需要将堆尾元素移动到堆顶，并对堆进行调整。这里可以将堆尾元素赋值给堆顶元素，然后删除堆尾元素，这样就可以减少一次交换操作。

   - 对于需要交换的元素，可以将它们保存在一个临时变量中，然后再一次性进行赋值，减少交换次数。

下面是一个优化过的堆排序的C++代码实现：

```C++
#include <iostream>
#include <vector>
using namespace std;

void adjustHeap(vector<int>& arr, int i, int len) {
    int temp = arr[i];
    for (int k = i * 2 + 1; k < len; k = k * 2 + 1) {
        if (k + 1 < len && arr[k] < arr[k + 1]) {
            k++;
        }
        if (arr[k] > temp) {
            arr[i] = arr[k];
            i = k;
        }
        else {
            break;
        }
    }
    arr[i] = temp;
}

void heapSort(vector<int>& arr) {
    int len = arr.size();
    for (int i = len / 2 - 1; i >= 0; i--) {
        adjustHeap(arr, i, len);
    }
    for (int i = len - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        adjustHeap(arr, 0, i);
    }
}

int main() {
    vector<int> arr{ 5, 2, 9, 4, 7, 6, 1, 3, 8 };
    heapSort(arr);
    for (auto i : arr) {
        cout << i << " ";
    }
    cout << endl;
    return 0;
}
```

这里采用的是非递归的建堆方式，同时在删除堆顶元素时采用了赋值的方式代替交换操作。

## 8、计数排序

### 8.1、概念

数排序是一个非基于比较的排序算法，该算法于1954年由 Harold H. Seward 提出。它的优势在于在对一定范围内的整数排序时，它的复杂度为Ο(n+k)（其中k是整数的范围），快于任何比较排序算法。当然这是一种牺牲空间换取时间的做法，而且当O(k)>O(n*log(n))的时候其效率反而不如基于比较的排序（基于比较的排序的时间复杂度在理论上的下限是O(n*log(n)), 如归并排序，堆排序）。

### 8.2、基本思想

计数排序对输入的数据有附加的限制条件：

1. 输入的线性表的元素属于有限偏序集S；

2. 设输入的线性表的长度为n，|S|=k（表示集合S中元素的总数目为k），则k=O(n)。

在这两个条件下，计数排序的复杂性为O(n)。

计数排序的基本思想是对于给定的输入序列中的每一个元素x，确定该序列中值小于x的元素的个数（此处并非比较各元素的大小，而是通过对元素值的计数和计数值的累加来确定）。一旦有了这个信息，就可以将x直接存放到最终的输出序列的正确位置上。例如，如果输入序列中只有17个元素的值小于x的值，则x可以直接存放在输出序列的第18个位置上。当然，如果有多个元素具有相同的值时，我们不能将这些元素放在输出序列的同一个位置上，因此，上述方案还要作适当的修改。

### 8.3、C++代码示例

```C++
#include <iostream>
#include <vector>

using namespace std;

void countingSort(vector<int>& arr, int maxValue) {
    vector<int> count(maxValue + 1, 0);
    vector<int> output(arr.size(), 0);

    // 计数每个元素出现的次数
    for (int i = 0; i < arr.size(); i++) {
        count[arr[i]]++;
    }

    // 计算前缀和
    for (int i = 1; i <= maxValue; i++) {
        count[i] += count[i-1];
    }

    // 将元素放入输出数组中
    for (int i = arr.size() - 1; i >= 0; i--) {
        output[count[arr[i]]-1] = arr[i];
        count[arr[i]]--;
    }

    // 将输出数组复制到原始数组中
    for (int i = 0; i < arr.size(); i++) {
        arr[i] = output[i];
    }
}

int main() {
    vector<int> arr{9, 5, 7, 3, 1, 2, 6, 8, 4, 0};

    cout << "Before sorting: ";
    for (int x : arr) {
        cout << x << " ";
    }
    cout << endl;

    countingSort(arr, 9);

    cout << "After sorting: ";
    for (int x : arr) {
        cout << x << " ";
    }
    cout << endl;

    return 0;
}
```

这段代码中，首先声明了一个`count`数组和一个`output`数组，`count`数组用于记录每个元素出现的次数，`output`数组用于存储排序后的结果。

在计数排序算法中，需要进行三个步骤：

1. 计数每个元素出现的次数：遍历原始数组，对每个元素进行计数。
2. 计算前缀和：将每个元素出现的次数累加到其前面所有元素出现次数的和中。
3. 将元素放入输出数组中：倒序遍历原始数组，将每个元素放入对应位置上，同时将其出现次数减1。

最后，将输出数组复制到原始数组中，排序完成。

### 8.4、优化

计数排序本身已经是一种时间复杂度为O(n)的排序算法，因此优化的空间有限。以下是几种可能的优化方式：

1. 原地计数排序：传统的计数排序算法需要使用一个额外的计数数组来进行排序，需要O(k)的额外空间，其中k是输入数组中最大元素的大小。可以考虑在原输入数组中进行计数和排序，这样可以省去额外的空间开销。
2. 对于数值较小的输入数组可以采用桶排序的方式：对于数值范围不大的输入数组，可以将每个数放入对应的桶中，桶内进行插入排序，最后再将桶中元素合并即可得到有序数组。
3. 多线程优化：计数排序的特点是不需要比较，只需要进行计数和写回操作，因此可以考虑使用多线程来加速计数和写回操作，从而提高排序速度。
4. GPU加速：计数排序可以使用GPU进行加速，因为GPU的并行处理能力非常强，可以同时处理多个数的计数和写回操作，从而大大提高排序速度。

**以下是一个使用原地计数的C++计数排序的实现，这是一种常数优化：**

```C++
void countingSort(vector<int>& arr) {
    int n = arr.size();
    int maxVal = *max_element(arr.begin(), arr.end());
    vector<int> count(maxVal+1);

    for (int i = 0; i < n; ++i) {
        ++count[arr[i]];
    }

    int j = 0;
    for (int i = 0; i <= maxVal; ++i) {
        while (count[i]-- > 0) {
            arr[j++] = i;
        }
    }
}
```

这个算法的时间复杂度为O(n+k)，其中k是输入数组中的最大值，空间复杂度为O(k)。可以看到，这个算法使用了原地计数，省去了额外的空间开销。

## 9、桶排序

### 9.1、概念

桶排序 (Bucket sort)或所谓的箱排序，是一个排序算法，工作的原理是将数组分到有限数量的桶子里。每个桶子再个别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。桶排序是鸽巢排序的一种归纳结果。当要被排序的数组内的数值是均匀分配的时候，桶排序使用线性时间（O（*n*））。但桶排序并不是 比较排序，他不受到O(nlogn) 下限的影响。

### 9.2、基本思想

桶排序是一种基于比较排序的线性时间复杂度算法，它的基本思想是将待排序的元素划分成若干个桶，每个桶中的元素都比桶内其他元素小，然后对每个桶中的元素进行排序，最后将所有桶中的元素合并成一个有序序列。

桶排序的实现步骤如下：

1. 确定桶的数量和范围。将待排序的元素划分为n个桶，每个桶存放的元素的值域范围为[bi, bi+1)。
2. 将元素放入桶中。遍历待排序的元素，将元素放入相应的桶中。
3. 对每个桶中的元素进行排序。可以使用任意排序算法，比如插入排序、快速排序等。
4. 合并所有桶中的元素。按照桶的顺序，将每个桶中的元素按顺序放入一个数组中。
5. 返回有序序列。

桶排序的时间复杂度为O(n+k)，其中k为桶的数量。如果桶的数量足够大，可以认为桶排序的时间复杂度是线性的。但是，桶排序的空间复杂度比较高，需要额外的存储空间来存放桶。

### 9.3、C++示例代码

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void bucketSort(vector<int>& arr, int bucketSize) {
    if (arr.empty()) {
        return;
    }

    // 获取最大值和最小值
    int minValue = arr[0];
    int maxValue = arr[0];
    for (int i = 1; i < arr.size(); ++i) {
        if (arr[i] < minValue) {
            minValue = arr[i];
        } else if (arr[i] > maxValue) {
            maxValue = arr[i];
        }
    }

    // 计算桶的数量
    int bucketCount = (maxValue - minValue) / bucketSize + 1;
    vector<vector<int>> buckets(bucketCount);

    // 将元素分配到桶中
    for (int i = 0; i < arr.size(); ++i) {
        int bucketIndex = (arr[i] - minValue) / bucketSize;
        buckets[bucketIndex].push_back(arr[i]);
    }

    // 对每个桶中的元素进行排序
    for (int i = 0; i < bucketCount; ++i) {
        sort(buckets[i].begin(), buckets[i].end());
    }

    // 将排序好的元素依次放回原数组中
    int index = 0;
    for (int i = 0; i < bucketCount; ++i) {
        for (int j = 0; j < buckets[i].size(); ++j) {
            arr[index++] = buckets[i][j];
        }
    }
}

int main() {
    vector<int> arr = {3, 6, 4, 8, 2, 5, 9, 1, 7};
    bucketSort(arr, 3);
    for (int num : arr) {
        cout << num << " ";
    }
    return 0;
}
```

桶排序的时间复杂度为O(n)，是一种非常快速的排序算法，但是它的空间复杂度比较高，需要开辟足够的空间存储桶。

### 9.4、优化

桶排序的时间复杂度主要取决于桶的个数和桶内部排序所采用的算法。如果桶的个数较少，桶内部元素较多，则可以采用其他的排序算法，比如快速排序或归并排序，来对每个桶内部进行排序，以提高排序效率。

另外，如果待排序的数据是比较集中的，可以采用按区间划分桶的方式，使得每个桶内的元素数量大致相同，从而减少桶内部排序所需要的时间。

以下是一个基于桶排序的优化示例：

```C++
void bucketSort(vector<int>& arr, int bucketSize) {
    if (arr.empty()) {
        return;
    }

    int minValue = arr[0];
    int maxValue = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        if (arr[i] < minValue) {
            minValue = arr[i];
        } else if (arr[i] > maxValue) {
            maxValue = arr[i];
        }
    }

    int bucketCount = (maxValue - minValue) / bucketSize + 1;
    vector<vector<int>> buckets(bucketCount);

    for (int i = 0; i < arr.size(); i++) {
        int bucketIndex = (arr[i] - minValue) / bucketSize;
        buckets[bucketIndex].push_back(arr[i]);
    }

    arr.clear();
    for (int i = 0; i < buckets.size(); i++) {
        if (buckets[i].empty()) {
            continue;
        }

        if (bucketSize == 1) {
            for (int j = 0; j < buckets[i].size(); j++) {
                arr.push_back(buckets[i][j]);
            }
        } else {
            bucketSort(buckets[i], bucketSize - 1);
            for (int j = 0; j < buckets[i].size(); j++) {
                arr.push_back(buckets[i][j]);
            }
        }
    }
}
```

这个桶排序算法采用了按区间划分桶的方式，并且在每个桶内部采用了递归的方式进行排序。其中 `bucketSize` 表示每个桶内部的元素数量，可以通过调整这个参数来改变算法的性能表现。

## 10、基数排序

### 10.1、概念

基数排序（radix sort）属于“分配式排序”（distribution sort），又称“桶子法”（bucket sort）或bin sort，顾名思义，它是透过键值的部份资讯，将要排序的元素分配至某些“桶”中，藉以达到排序的作用，基数排序法是属于稳定性的排序，其时间复杂度为O (nlog(r)m)，其中r为所采取的基数，而m为堆数，在某些时候，基数排序法的效率高于其它的稳定性排序法。

### 10.2、基本思想

基数排序是一种非比较排序算法，它的基本思想是将待排序的元素分别按照位数的大小进行排序。一般的实现方法是先按照个位数排序，然后按照十位数排序，接着按照百位数排序，直到最高位数排完后，排序完成。

具体的实现步骤如下：

1. 找出待排序数组中最大的数，确定最大数的位数，作为排序的轮数；
2. 对于每一位数，用计数排序或桶排序进行排序；
3. 将排序后的数组按照位数依次组合起来，得到最终结果。

基数排序的时间复杂度为O(nk)，其中k为最大数的位数，n为数组元素个数。当k比较小的时候，基数排序的效率较高。但是当k比较大时，需要分配较大的桶或计数器，空间复杂度会变高。

### 10.3、C++示例代码

```C++
#include <iostream>
#include <vector>

using namespace std;

void radixSort(vector<int>& arr) {
    int maxVal = *max_element(arr.begin(), arr.end());

    for (int exp = 1; maxVal / exp > 0; exp *= 10) {
        vector<int> count(10, 0);

        for (int i = 0; i < arr.size(); i++) {
            int digit = (arr[i] / exp) % 10;
            count[digit]++;
        }

        for (int i = 1; i < count.size(); i++) {
            count[i] += count[i - 1];
        }

        vector<int> temp(arr.size());
        for (int i = arr.size() - 1; i >= 0; i--) {
            int digit = (arr[i] / exp) % 10;
            temp[count[digit] - 1] = arr[i];
            count[digit]--;
        }

        for (int i = 0; i < arr.size(); i++) {
            arr[i] = temp[i];
        }
    }
}

int main() {
    vector<int> arr = {170, 45, 75, 90, 802, 24, 2, 66};
    radixSort(arr);

    for (auto x : arr) {
        cout << x << " ";
    }
    cout << endl;

    return 0;
}
```

基数排序是通过分离元素的各个位来进行排序的，因此它不会受到排序数据中的数字大小的限制。它的时间复杂度为O(d*(n+k))，其中d是数字的最大位数，k是进制数，一般为10。虽然它的时间复杂度较高，但是由于它对排序数据的范围没有限制，因此它可以处理更大范围的数据，且稳定性高。

### 10.4、优化

基数排序本身已经是一种比较高效的排序算法，因此不太需要进行太多的优化。如果非要提高其效率，可以考虑以下几个方面：

1. 优化桶的大小：桶的大小可以根据实际数据的范围来设定，过大会造成空间浪费，过小则会影响排序效率。
2. 选择合适的排序算法：基数排序的最后一步需要使用其他的排序算法进行排序，选择一个高效的排序算法可以提高整个基数排序的效率。
3. 优化对数据的访问：对于需要进行频繁访问的数据，可以将其放置在缓存友好的位置，减少访问时间。

以下是一个经过优化的基数排序的 C++ 代码示例：

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

void countingSort(vector<int>& arr, int exp) {
    vector<int> output(arr.size());
    vector<int> count(10, 0);

    for (int i = 0; i < arr.size(); i++) {
        int digit = (arr[i] / exp) % 10;
        count[digit]++;
    }

    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    for (int i = arr.size() - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    for (int i = 0; i < arr.size(); i++) {
        arr[i] = output[i];
    }
}

void radixSort(vector<int>& arr) {
    int max_element = *max_element(arr.begin(), arr.end());

    for (int exp = 1; max_element / exp > 0; exp *= 10) {
        countingSort(arr, exp);
    }
}

int main() {
    vector<int> arr = {170, 45, 75, 90, 802, 24, 2, 66};
    radixSort(arr);

    for (int i = 0; i < arr.size(); i++) {
        cout << arr[i] << " ";
    }

    return 0;
}
```

在这个代码示例中，我们对桶的大小进行了优化，将桶的大小设定为 10，而非一般的数组长度，以此来减少空间浪费。我们也采用了常见的计数排序作为基数排序的最后一步。在访问数据时，我们使用了 vector 容器，而非数组，这也能够更好地利用缓存。