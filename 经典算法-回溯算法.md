# 经典算法-回溯算法

刷题网站相关题目：

> [77. 组合](https://leetcode.cn/problems/combinations) 216. 组合总和 III
>
> [39. 组合总和](https://leetcode.cn/problems/combination-sum)
>
> [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii)
>
> [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number)
>
> [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning)
>
> [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses)
>
> [78. 子集](https://leetcode.cn/problems/subsets)
>
> [90. 子集 II](https://leetcode.cn/problems/subsets-ii)
>
> [491. 递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences)
>
> [46. 全排列](https://leetcode.cn/problems/permutations)
>
> [47. 全排列 II](https://leetcode.cn/problems/permutations-ii)
>
> [51. N 皇后](https://leetcode.cn/problems/n-queens)
>
> [37. 解数独](https://leetcode.cn/problems/sudoku-solver)

...

# 一、N皇后问题

N皇后问题是一个著名的组合问题，目标是在一个N×N的棋盘上放置N个皇后，使得它们互相不能攻击到对方（即不在同一行、同一列或同一对角线上）

## 1.1、问题描述

给定一个N×N的棋盘，要求放置N个皇后，使得它们互相不能攻击到对方。每个皇后可以水平、垂直或对角线移动。

## 1.2、解题思路

使用回溯算法解决N皇后问题。回溯算法是一种试错的算法，它尝试每一种可能性，如果达不到目标，则回溯到上一步，直到找到解决方案或尝试了所有可能性。

1. 创建一个N×N的棋盘，初始化为0，其中0表示没有皇后，1表示有皇后。
2. 从第一行开始，尝试在每一列放置皇后。对于每一列，检查是否可以放置皇后（即不会受到攻击）。如果可以，将该位置标记为1，并继续放置下一行的皇后。
3. 如果在某一行无法找到一个合适的位置来放置皇后，回溯到上一行，将上一行的皇后移到下一个可行位置，并继续尝试。如果无法找到解决方案，回溯到更上一行，以此类推。
4. 当放置了N个皇后且它们互相不受攻击时，找到一个解决方案。
5. 继续尝试其他可能的放置方式，直到找到所有解决方案或尝试了所有可能性。

## 1.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

// 检查是否可以在棋盘board[row][col]放置皇后
bool isSafe(vector<vector<int>>& board, int row, int col, int N) {
    // 检查列是否安全
    for (int i = 0; i < row; ++i) {
        if (board[i][col] == 1) {
            return false;
        }
    }

    // 检查左上对角线是否安全
    for (int i = row, j = col; i >= 0 && j >= 0; --i, --j) {
        if (board[i][j] == 1) {
            return false;
        }
    }

    // 检查右上对角线是否安全
    for (int i = row, j = col; i >= 0 && j < N; --i, ++j) {
        if (board[i][j] == 1) {
            return false;
        }
    }

    return true;
}

// 使用回溯算法解决N皇后问题
void solveNQueens(vector<vector<string>>& solutions, vector<vector<int>>& board, int row, int N) {
    if (row == N) {
        // 找到一个解决方案，将其存储为字符串表示
        vector<string> solution;
        for (int i = 0; i < N; ++i) {
            string rowStr(N, '.');
            for (int j = 0; j < N; ++j) {
                if (board[i][j] == 1) {
                    rowStr[j] = 'Q';
                }
            }
            solution.push_back(rowStr);
        }
        solutions.push_back(solution);
        return;
    }

    for (int col = 0; col < N; ++col) {
        if (isSafe(board, row, col, N)) {
            // 在当前位置放置皇后
            board[row][col] = 1;
            // 继续放置下一行的皇后
            solveNQueens(solutions, board, row + 1, N);
            // 回溯，移除当前位置的皇后
            board[row][col] = 0;
        }
    }
}

// 解决N皇后问题的入口函数
vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> solutions;
    vector<vector<int>> board(n, vector<int>(n, 0));
    solveNQueens(solutions, board, 0, n);
    return solutions;
}

int main() {
    int N;
    cout << "Enter the value of N for N-Queens problem: ";
    cin >> N;

    vector<vector<string>> solutions = solveNQueens(N);

    if (solutions.empty()) {
        cout << "No solutions found for N=" << N << endl;
    } else {
        cout << "Solutions for N=" << N << ":" << endl;
        for (const auto& solution : solutions) {
            for (const string& row : solution) {
                cout << row << endl;
            }
            cout << endl;
        }
    }

    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036727.png)

# 二、01背包问题

## 2.1、问题描述

有一个背包，它的容量是C。有n个物品，每个物品的重量是w[i]，价值是v[i]。要求选择一些物品放入背包中，使得背包中物品的总重量不超过C，同时使得背包中物品的总价值最大。每个物品要么放入背包，要么不放入背包，不能将物品切割成小块。

## 2.2、解题思路

1. 创建一个数组 `selected`，用于记录每个物品是否被选择放入背包。初始时，所有物品都未被选择。
2. 从第一个物品开始，尝试将其放入背包。如果当前物品的重量不超过背包的容量，并且将其放入背包后，可以获得更多的总价值，就选择放入背包，并更新 `selected` 数组。
3. 递归尝试放入下一个物品，并继续判断是否满足背包容量限制和是否可以获得更多的总价值。
4. 递归过程中，如果发现无法放入下一个物品或者已经尝试了所有物品，就回溯到上一步，将上一个物品移出背包。
5. 重复上述步骤，直到遍历完所有物品或者无法继续放入物品为止。
6. 在递归过程中，不断更新记录的最优解，以便找到总价值最大的放置方式。

## 2.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

int maxTotalValue = 0; // 记录最大总价值
vector<int> selected; // 记录是否选择放入背包

// 使用回溯算法解决01背包问题
void solve01Knapsack(vector<int>& weights, vector<int>& values, int capacity, int currentWeight, int currentValue, int index) {
    if (index == weights.size() || currentWeight >= capacity) {
        // 递归结束条件：遍历完所有物品或背包已满
        if (currentValue > maxTotalValue) {
            maxTotalValue = currentValue;
            selected.assign(selected.begin(), selected.end()); // 更新最优解
        }
        return;
    }

    // 尝试放入当前物品
    if (currentWeight + weights[index] <= capacity) {
        selected[index] = 1;
        solve01Knapsack(weights, values, capacity, currentWeight + weights[index], currentValue + values[index], index + 1);
        selected[index] = 0; // 回溯，不放入当前物品
    }

    // 不放入当前物品
    solve01Knapsack(weights, values, capacity, currentWeight, currentValue, index + 1);
}

int main() {
    int n; // 物品数量
    cout << "Enter the number of items: ";
    cin >> n;

    vector<int> weights(n);
    vector<int> values(n);

    cout << "Enter the weights of items:" << endl;
    for (int i = 0; i < n; ++i) {
        cin >> weights[i];
    }

    cout << "Enter the values of items:" << endl;
    for (int i = 0; i < n; ++i) {
        cin >> values[i];
    }

    int capacity; // 背包容量
    cout << "Enter the capacity of knapsack: ";
    cin >> capacity;

    selected.resize(n, 0); // 初始化selected数组

    solve01Knapsack(weights, values, capacity, 0, 0, 0);

    cout << "Maximum total value: " << maxTotalValue << endl;
    cout << "Selected items: ";
    for (int i = 0; i < n; ++i) {
        if (selected[i]) {
            cout << i << " ";
        }
    }
    cout << endl;

    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036618.png)

# 三、排列和组合问题

排列组合问题是指给定一组元素，求所有可能的排列或组合方式。

## 3.1、问题描述

给定一组不重复的整数，要求找出它们的所有排列。

## 3.2、解题思路

1. 初始化一个空数组 `result`，用于存储所有排列的结果。
2. 创建一个辅助函数 `permuteHelper`，该函数将一个当前排列 `current` 作为参数。初始时，`current` 为空数组。
3. 在 `permuteHelper` 函数内，首先检查如果 `current` 的大小等于给定整数集合的大小，那么将 `current` 添加到 `result` 中，表示找到了一个排列。
4. 否则，遍历整数集合中的每个元素，检查它是否已经在 `current` 中。如果没有，将该元素添加到 `current`，然后递归调用 `permuteHelper`。
5. 在递归返回后，将刚添加到 `current` 的元素移除，以便尝试下一个元素。
6. 最终，`result` 中将包含所有排列的结果。

## 3.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

void permuteHelper(vector<int>& nums, vector<int>& current, vector<vector<int>>& result) {
    if (current.size() == nums.size()) {
        result.push_back(current);
        return;
    }

    for (int i = 0; i < nums.size(); ++i) {
        if (find(current.begin(), current.end(), nums[i]) == current.end()) {
            current.push_back(nums[i]);
            permuteHelper(nums, current, result);
            current.pop_back();
        }
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    permuteHelper(nums, current, result);
    return result;
}

int main() {
    vector<int> nums = {1, 2, 3};
    vector<vector<int>> permutations = permute(nums);

    cout << "All permutations:" << endl;
    for (const vector<int>& perm : permutations) {
        for (int num : perm) {
            cout << num << " ";
        }
        cout << endl;
    }

    return 0;
}
```

此代码将生成给定整数集合 `{1, 2, 3}` 的所有排列，并输出它们。

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036958.png)

# 四、数独问题

数独问题是一个经典的求解填充数独游戏的问题，规则是在一个9x9的网格中，每行、每列和每个3x3的子网格中都必须包含数字1到9，而且不能重复。

## 4.1、问题描述

给定一个部分填充的9x9数独棋盘，其中已填入一些数字（用数字0-9表示，0表示空格）。任务是找到一种填充方式，满足数独游戏的规则。

## 4.2、解题思路

1. 创建一个9x9的二维数组，表示数独棋盘。
2. 创建一个辅助函数 `solveSudoku`，该函数使用回溯算法来填充数独。
3. 在 `solveSudoku` 函数内，首先遍历数独棋盘，寻找一个未填充数字的位置（值为0的位置）。
4. 如果找到未填充数字的位置，尝试在该位置填入数字1到9，然后递归调用 `solveSudoku` 继续填充下一个位置。
5. 在递归返回后，如果数独已经成功填充完毕，返回 `true` 表示成功。
6. 如果在某一步填充数字时遇到冲突（即在同一行、同一列或同一个3x3子网格中已经存在相同的数字），则返回 `false` 表示填充失败，需要回溯到上一步重新尝试。
7. 最终，如果 `solveSudoku` 函数返回 `true`，表示成功填充了数独，否则表示无解。

## 4.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

const int N = 9;

bool isValid(vector<vector<char>>& board, int row, int col, char num) {
    for (int i = 0; i < N; ++i) {
        if (board[row][i] == num || board[i][col] == num || board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == num) {
            return false;
        }
    }
    return true;
}

bool solveSudoku(vector<vector<char>>& board) {
    for (int row = 0; row < N; ++row) {
        for (int col = 0; col < N; ++col) {
            if (board[row][col] == '.') {
                for (char num = '1'; num <= '9'; ++num) {
                    if (isValid(board, row, col, num)) {
                        board[row][col] = num;
                        if (solveSudoku(board)) {
                            return true;
                        }
                        board[row][col] = '.'; // Backtrack
                    }
                }
                return false; // No valid solution found
            }
        }
    }
    return true; // All cells filled
}

int main() {
    vector<vector<char>> board = {
        {'5', '3', '.', '.', '7', '.', '.', '.', '.'},
        {'6', '.', '.', '1', '9', '5', '.', '.', '.'},
        {'.', '9', '8', '.', '.', '.', '.', '6', '.'},
        {'8', '.', '.', '.', '6', '.', '.', '.', '3'},
        {'4', '.', '.', '8', '.', '3', '.', '.', '1'},
        {'7', '.', '.', '.', '2', '.', '.', '.', '6'},
        {'.', '6', '.', '.', '.', '.', '2', '8', '.'},
        {'.', '.', '.', '4', '1', '9', '.', '.', '5'},
        {'.', '.', '.', '.', '8', '.', '.', '7', '9'}
    };

    if (solveSudoku(board)) {
        cout << "Solved Sudoku:" << endl;
        for (int i = 0; i < N; ++i) {
            for (int j = 0; j < N; ++j) {
                cout << board[i][j] << " ";
            }
            cout << endl;
        }
    } else {
        cout << "No solution exists." << endl;
    }

    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036046.png)

# 五、全排列问题

## 5.1、问题描述

全排列问题是一个经典的组合学问题，要求给定一个包含不同元素的集合，找出这些元素的所有可能的排列方式。

## 5.2、解题思路

1. 创建一个空的结果集，用于存储所有的排列。
2. 创建一个辅助函数（例如 `permute`），该函数采用以下参数：
   - 当前正在生成的排列（初始为空）
   - 剩余可用的元素列表
   - 结果集
3. 在 `permute` 函数内，首先检查是否已经生成了一个完整的排列，如果是，则将其添加到结果集中。
4. 否则，遍历剩余可用的元素，依次将每个元素添加到当前排列中，并从剩余元素列表中移除。然后递归调用 `permute` 函数生成下一个排列。
5. 在递归返回后，需要将添加的元素从当前排列中移除，并将其重新添加到剩余元素列表中，以便生成其他排列。
6. 最终，`permute` 函数将生成所有可能的排列。

## 5.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

void permute(vector<int>& nums, vector<vector<int>>& result, vector<int>& current) {
    if (current.size() == nums.size()) {
        result.push_back(current); // 找到一个完整的排列，添加到结果集
        return;
    }
    
    for (int i = 0; i < nums.size(); ++i) {
        if (find(current.begin(), current.end(), nums[i]) == current.end()) {
            current.push_back(nums[i]); // 添加元素到当前排列
            permute(nums, result, current); // 递归生成下一个排列
            current.pop_back(); // 回溯，移除最后一个元素
        }
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    permute(nums, result, current);
    return result;
}

int main() {
    vector<int> nums = {1, 2, 3};
    vector<vector<int>> result = permute(nums);
    
    cout << "All permutations:" << endl;
    for (const vector<int>& perm : result) {
        for (int num : perm) {
            cout << num << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036985.png)

# 六、子集合问题

## 6.1、问题描述

子集合问题是一个经典的组合学问题，给定一个集合，要求找出这个集合的所有可能的子集合。

## 6.2、解题思路

1. 创建一个空的结果集，用于存储所有的子集合。
2. 创建一个辅助函数（例如 `subsets`），该函数采用以下参数：
   - 当前正在生成的子集合（初始为空）
   - 剩余可用的元素列表
   - 结果集
3. 在 `subsets` 函数内，首先将当前子集合添加到结果集中。
4. 遍历剩余可用的元素，依次将每个元素添加到当前子集合中，并从剩余元素列表中移除。然后递归调用 `subsets` 函数生成下一个子集合。
5. 在递归返回后，需要将添加的元素从当前子集合中移除，并将其重新添加到剩余元素列表中，以便生成其他子集合。
6. 最终，`subsets` 函数将生成所有可能的子集合。

## 6.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

void subsets(vector<int>& nums, vector<vector<int>>& result, vector<int>& current, int index) {
    result.push_back(current); // 添加当前子集合到结果集
    
    for (int i = index; i < nums.size(); ++i) {
        current.push_back(nums[i]); // 添加元素到当前子集合
        subsets(nums, result, current, i + 1); // 递归生成下一个子集合
        current.pop_back(); // 回溯，移除最后一个元素
    }
}

vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    subsets(nums, result, current, 0);
    return result;
}

int main() {
    vector<int> nums = {1, 2, 3};
    vector<vector<int>> result = subsets(nums);
    
    cout << "All subsets:" << endl;
    for (const vector<int>& subset : result) {
        for (int num : subset) {
            cout << num << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036270.png)

# 七、图的着色问题

## 7.1、问题描述

图的着色问题是指给定一个无向图，找到一种方法为图中的每个节点分配一种颜色，使得相邻的节点具有不同的颜色。这个问题的目标是使用尽量少的颜色来完成着色。

## 7.2、解题思路

1. 创建一个数组（或向量）`colors`，用于存储每个节点的颜色。初始时，所有节点都未被着色，可以使用一个特殊的值（如-1）表示未着色。
2. 创建一个函数（例如 `colorGraph`），该函数采用以下参数：
   - 当前节点的索引
   - 图的邻接表
   - 颜色数组
3. 在 `colorGraph` 函数内，首先检查是否所有节点都已着色。如果是，表示已找到一种着色方案，可以将颜色数组添加到结果集中。
4. 否则，遍历当前节点可用的颜色（通常从1到K，K表示允许的颜色数），并检查是否可以为当前节点着色。如果可以，将颜色赋给当前节点，然后递归调用 `colorGraph` 函数处理下一个节点。
5. 在递归返回后，需要撤销对当前节点的颜色分配，以便尝试其他颜色。这个过程称为回溯。
6. 最终，`colorGraph` 函数将生成所有可能的着色方案。

## 7.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

// 检查节点是否可以着色
bool isSafe(int node, int color, const vector<vector<int>>& graph, const vector<int>& colors) {
    for (int neighbor : graph[node]) {
        if (colors[neighbor] == color) {
            return false;
        }
    }
    return true;
}

void colorGraph(int node, const vector<vector<int>>& graph, vector<int>& colors, int numColors, vector<vector<int>>& result) {
    int numNodes = graph.size();
    
    // 如果所有节点都已着色，将当前着色方案添加到结果
    if (node == numNodes) {
        result.push_back(colors);
        return;
    }
    
    // 尝试为当前节点分配颜色
    for (int color = 1; color <= numColors; ++color) {
        if (isSafe(node, color, graph, colors)) {
            colors[node] = color;
            colorGraph(node + 1, graph, colors, numColors, result);
            colors[node] = -1; // 回溯，撤销颜色分配
        }
    }
}

vector<vector<int>> graphColoring(const vector<vector<int>>& graph, int numColors) {
    int numNodes = graph.size();
    vector<int> colors(numNodes, -1); // 初始化颜色数组
    vector<vector<int>> result;
    
    colorGraph(0, graph, colors, numColors, result);
    
    return result;
}

int main() {
    vector<vector<int>> graph = {
        {0, 1, 1, 0},
        {1, 0, 1, 1},
        {1, 1, 0, 1},
        {0, 1, 1, 0}
    };
    
    int numColors = 3; // 假设有3种颜色
    
    vector<vector<int>> colorSchemes = graphColoring(graph, numColors);
    
    cout << "Possible colorings:" << endl;
    for (const vector<int>& colors : colorSchemes) {
        for (int color : colors) {
            cout << color << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036665.png)

# 八、迷宫问题

## 8.1、问题描述

迷宫问题是指在一个迷宫地图中寻找从起点到达终点的路径，使得路径尽可能短且不穿越迷宫中的墙壁。通常，迷宫由格子组成，其中一些格子是墙壁，而其他格子是可通行的路径。问题是找到一条从起点到终点的路径，路径只能沿着通行的格子移动，不能穿越墙壁。

## 8.2、解题思路

1. 创建一个二维数组表示迷宫地图，通常使用0表示可通行的路径，1表示墙壁。还需要创建一个与地图相同大小的二维数组来记录路径。
2. 创建一个递归函数，该函数采用以下参数：
   - 当前所在的行和列
   - 目标终点的行和列
   - 迷宫地图
   - 记录路径的数组
3. 在递归函数内部，首先检查是否已到达目标终点。如果是，表示找到了一条路径，将路径添加到结果中。
4. 否则，尝试从当前位置向四个方向（上、下、左、右）移动。对于每个移动，需要检查是否可以前进（不是墙壁且不超出迷宫边界）。如果可以前进，将当前位置标记为已访问，然后递归调用函数前进到下一个位置。
5. 在递归返回后，需要撤销对当前位置的标记，以便尝试其他路径。这个过程称为回溯。
6. 最终，递归函数将生成所有可能的路径，将它们添加到结果中。

## 8.3、代码示例

```C
#include <iostream>
#include <vector>

using namespace std;

bool findPath(vector<vector<int>>& maze, int row, int col, const vector<vector<int>>& destination, vector<vector<int>>& path) {
    int numRows = maze.size();
    int numCols = maze[0].size();

    // 检查是否越界或撞墙
    if (row < 0 || col < 0 || row >= numRows || col >= numCols || maze[row][col] == 1) {
        return false;
    }

    // 检查是否到达终点
    if (row == destination[0][0] && col == destination[0][1]) {
        path.push_back({row, col});
        return true;
    }

    // 标记当前位置已访问
    maze[row][col] = 1;

    // 尝试向四个方向移动
    if (findPath(maze, row - 1, col, destination, path) ||  // 上
        findPath(maze, row + 1, col, destination, path) ||  // 下
        findPath(maze, row, col - 1, destination, path) ||  // 左
        findPath(maze, row, col + 1, destination, path)) {  // 右
        path.push_back({row, col}); // 添加当前位置到路径
        return true;
    }

    // 撤销标记，回溯
    maze[row][col] = 0;
    return false;
}

int main() {
    vector<vector<int>> maze = {
        {0, 1, 0, 0, 0},
        {0, 1, 1, 0, 0},
        {0, 0, 0, 1, 0},
        {0, 1, 1, 1, 0},
        {0, 0, 0, 0, 0}
    };

    vector<vector<int>> path;
    vector<vector<int>> destination = {{4, 4}}; // 终点坐标

    if (findPath(maze, 0, 0, destination, path)) {
        cout << "Path found:" << endl;
        for (int i = path.size() - 1; i >= 0; --i) {
            cout << "(" << path[i][0] << ", " << path[i][1] << ") ";
        }
        cout << endl;
    } else {
        cout << "No path found." << endl;
    }

    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260037023.png)

# 九、电话号码的字母组合问题

## 9.1、问题描述

给定一个包含数字 2-9 的字符串，表示电话按键上的数字与字母的映射关系。每个数字可以映射到 0 到多个字母之一。编写一个函数，返回所有可能的电话号码字母组合。

## 9.2、解题思路

1. 创建一个映射表，将数字与字母的映射关系存储起来，例如：

```Plain
map<char, string> digitToLetters = {
    {'2', "abc"},
    {'3', "def"},
    {'4', "ghi"},
    {'5', "jkl"},
    {'6', "mno"},
    {'7', "pqrs"},
    {'8', "tuv"},
    {'9', "wxyz"}
};
```

2. 创建一个结果集（可以是字符串向量），用于存储生成的电话号码组合。

3. 编写一个递归函数，该函数接受以下参数：

   - 当前电话号码的组合（初始为空）

   - 当前处理的数字索引

   - 输入的数字字符串

4. 在递归函数内部，首先检查当前数字索引是否等于输入数字字符串的长度。如果是，表示已经生成了一个完整的电话号码组合，将其添加到结果集中，然后返回。

5. 如果当前数字索引小于输入数字字符串的长度，就取出当前数字，并获取与之对应的字母集合。

6. 对于每个字母，将其添加到当前电话号码组合中，然后递归调用函数，传递更新后的电话号码组合和下一个数字索引。

7. 在递归返回后，需要撤销对当前电话号码组合的修改，以便尝试其他字母。

8. 最终，递归函数将生成所有可能的电话号码组合，存储在结果集中。

## 9.3、代码示例

```C
#include <iostream>
#include <vector>
#include <map>

using namespace std;

class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> result;
        if (digits.empty()) {
            return result;
        }
        
        map<char, string> digitToLetters = {
            {'2', "abc"},
            {'3', "def"},
            {'4', "ghi"},
            {'5', "jkl"},
            {'6', "mno"},
            {'7', "pqrs"},
            {'8', "tuv"},
            {'9', "wxyz"}
        };
        
        string current;
        backtrack(digits, 0, current, digitToLetters, result);
        return result;
    }
    
    void backtrack(const string& digits, int index, string& current, const map<char, string>& digitToLetters, vector<string>& result) {
        if (index == digits.length()) {
            result.push_back(current);
            return;
        }
        
        char digit = digits[index];
        string letters = digitToLetters.at(digit);
        
        for (char letter : letters) {
            current.push_back(letter);
            backtrack(digits, index + 1, current, digitToLetters, result);
            current.pop_back(); // 回溯
        }
    }
};

int main() {
    Solution solution;
    string digits = "23"; // 示例输入
    vector<string> result = solution.letterCombinations(digits);
    
    cout << "All possible combinations:" << endl;
    for (const string& combination : result) {
        cout << combination << " ";
    }
    cout << endl;
    
    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260037945.png)

# 十、单词搜索问题

## 10.1、问题描述

给定一个二维网格和一个单词，找出该单词是否存在于网格中。单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

## 10.2、解题思路

1. 对于二维网格中的每个单元格，都可以作为单词的起始位置进行搜索。
2. 编写一个递归的DFS函数，该函数接受以下参数：
   - 当前单元格的坐标 (i, j)。
   - 当前要匹配的单词中的字符索引 wordIndex。
3. 在DFS函数内部，首先检查边界情况：
   - 如果坐标 (i, j) 超出了网格范围，返回 false。
   - 如果当前单元格的字符与要匹配的单词字符不相同，返回 false。
4. 如果当前单元格的字符与要匹配的单词字符相同，将当前单元格标记为已访问，并递归地在上下左右四个方向搜索下一个字符。
5. 在递归函数内，如果 wordIndex 达到了单词的长度，表示已经匹配成功，返回 true。
6. 如果在上述搜索过程中没有找到匹配的路径，需要将当前单元格标记为未访问，并返回 false。
7. 在主函数中，遍历二维网格的每个单元格，将其作为搜索的起始点，调用DFS函数进行搜索。如果有任何一个起始点返回 true，则表示单词存在于网格中。

## 10.3、代码示例

```C
#include <iostream>
#include <vector>
#include <string>

using namespace std;

class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        rows = board.size();
        cols = board[0].size();
        
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                if (dfs(board, i, j, 0, word)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
private:
    int rows;
    int cols;
    
    bool dfs(vector<vector<char>>& board, int i, int j, int wordIndex, string& word) {
        if (i < 0 || i >= rows || j < 0 || j >= cols || board[i][j] != word[wordIndex]) {
            return false;
        }
        
        if (wordIndex == word.length() - 1) {
            return true;
        }
        
        char temp = board[i][j];
        board[i][j] = ' '; // 标记为已访问，防止重复访问
        
        bool found = dfs(board, i + 1, j, wordIndex + 1, word) ||
                     dfs(board, i - 1, j, wordIndex + 1, word) ||
                     dfs(board, i, j + 1, wordIndex + 1, word) ||
                     dfs(board, i, j - 1, wordIndex + 1, word);
        
        board[i][j] = temp; // 恢复为未访问状态
        
        return found;
    }
};

int main() {
    Solution solution;
    vector<vector<char>> board = {
        {'A', 'B', 'C', 'E'},
        {'S', 'F', 'C', 'S'},
        {'A', 'D', 'E', 'E'}
    };
    string word = "ABCCED"; // 示例输入
    
    bool result = solution.exist(board, word);
    
    if (result) {
        cout << "The word exists in the board." << endl;
    } else {
        cout << "The word does not exist in the board." << endl;
    }
    
    return 0;
}
```

结果输出：

![img](https://raw.githubusercontent.com/aqjsp/Pictures/main/202402260036193.png)