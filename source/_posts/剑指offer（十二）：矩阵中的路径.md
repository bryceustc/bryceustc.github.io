---
title: 剑指offer（十二）：矩阵中的路径
date: 2023-10-08 20:24:50
tags: 剑指Offer
categories: 剑指Offer
mathjax: true
---
# 题目描述：
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","**b**","c","e"],
["s","**f**","**c**","s"],
["a","d","**e**","e"]]

<!--more-->

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。
  
**示例：**
```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```
  
# 解题思路:
使用深度优先遍历DFS：
1. 遍历二维字符网格board，寻找和单词word第一个字母相同的字符，即board[i][j]==word[0]。
2. 对于board[i][j]等于word第一个字母的情况，进行深度优先搜索，从当前字符board[i][j]出发，向上、下、左、右四个方向进行搜索。
3. 在每个方向上，判断下一个字符是否等于word中的下一个字符，如果相等，则继续递归搜索，否则返回上一层。
4. 如果搜索到word的最后一个字符，则说明存在这个单词，返回true；否则，返回false。
# 时间复杂度：
O(n^2)
# 空间复杂度
 O(n)

# 代码
## C++
```c++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if (board.empty()) return false;
        int rows = board.size();
        int cols = board[0].size();
        // 初始化visited 数组
        vector<vector<int>> visited (rows, vector<int>(cols, 0));
        for (int i=0;i<rows;i++) {
            for(int j =0;j<cols;j++) {
                if (board[i][j] == word[0]) { // 如果当前字符匹配到了单词中的第一个字符
                    if (dfs(board,rows, cols, i, j, word, 0, visited)) {
                        return true; // 如果找到符合条件的单词，就返回true
                    }
                }
            }
        }
        return false; // 如果遍历了整个矩阵都没有找到符合条件的单词，就返回false
    }
    bool dfs(vector<vector<char>>& board, int rows, int cols, int i, int j, string word, int k, vector<vector<int>> &visited) {
        // 如果已经找到了单词的最后一个字符就返回true
        if (k == word.size()){
            return true;
        }
        // 如果当前位置越界、已经被访问过、或者当前位置的字符与单词中下一个要搜索的字符不匹配，就返回false
        if (i < 0|| i >= rows || j < 0 || j >= cols || board[i][j]!=word[k] || visited[i][j]==1) {
            return false;
        }
        // 标记当前位置已经访问过
        visited[i][j]=1;
        // 从当前位置的上、下、左、右四个方向递归地搜索下一个字符，如果在这些方向中任意一个方向找到符合条件的单词，就返回true
        if (dfs(board,rows, cols, i-1, j, word, k+1, visited)||
        dfs(board,rows, cols, i+1, j, word, k+1, visited)||
        dfs(board,rows, cols, i, j-1, word, k+1, visited)||
        dfs(board,rows, cols, i, j+1, word, k+1, visited)) {
            return true;
        }
        // 如果在所有方向中都没有找到符合条件的单词，就回溯到上一个字符位置，并将其标记为未访问过
        visited[i][j]=0;
        return false;
    }
};
```
## Python
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        if board == None:
            return False
        rows = len(board)
        cols = len(board[0])
        visited = [[0 for x in range(cols)] for y in range(rows)]
        for i in range(rows):
            for j in range(cols):
                if self.dfs(board, rows,  cols, i, j, word, 0, visited):
                    return True
        return False
    def dfs(self, board, rows, cols, i, j, word, k, visited):
        if i<0 or i>=rows or j<0 or j>=cols or board[i][j]!=word[k] or visited[i][j]==1:
            return False
        if k==len(word)-1:
            return True
        visited[i][j]=1
        if self.dfs(board, rows, cols, i-1, j, word, k+1, visited):
            return True
        if self.dfs(board, rows, cols, i+1, j, word, k+1, visited):
            return True
        if self.dfs(board, rows, cols, i, j-1, word, k+1, visited):
            return True
        if self.dfs(board, rows, cols, i, j+1, word, k+1, visited):
            return True
        visited[i][j]=0
        return False
```
