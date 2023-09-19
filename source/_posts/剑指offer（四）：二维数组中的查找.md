---
title: 剑指offer（四）：二维数组中的查找
date: 2023-09-16 21:50:54
tags: 剑指Offer
categories: 剑指Offer
mathjax: true
---
# 题目描述:

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，
判断数组中是否含有该整数

 <!--more--> 
# 解题思路:
  1). 直接暴力遍历二维数组所有元素，时间复杂度为O(m\*n)
  
  2). 对每一行使用一次二分查找，时间复杂度为O(m\*logn)
  
  3). 根据简单的例子寻找规律，从右上角开始寻找，时间复杂度为O(m+n)

# 代码

[C++](https://github.com/bryceustc/CodingInterviews/blob/master/FindInPartiallySortedMatrix/FindInPartiallySortedMatrix.cpp)

# C++:
## 方法一：暴力遍历
```c++
class Solution{
    public:
        bool findNumberIn2DArray(vector<vector<int>> &matrix,int target)
        {
            if (matrix.empty()) return false;
            int m = matrix.size();       
            int n = matrix[0].size();
            for (int i=0;i<m;i++)
            {
                for (int j=0;j<n;j++)
                {
                    if (matrix[i][j]==target)
                    {
                        return true;
                    }                    
                }
            }
            return false;          
        }
};
```

## 方法二：遍历行+二分查找：
```c++
class Solution{
    public:
        bool findNumberIn2DArray(vector<vector<int>> &matrix,int target)
        {
            if (matrix.empty()) return false;
            int n = matrix.size();       
            int m = matrix[0].size();
            for (int i=0;i<n;i++)
            {
                int start = 0;
                int end = m-1;
                while (end>=start)
                {
                    int mid = start + (end-start)/2;
                    if (matrix[i][mid]==target)
                        return true;
                    if (matrix[i][mid]<target)
                        start = mid+1;
                    if (matrix[i][mid]>target)
                        end = mid-1;
                }
            }
            return false;          
        }
};
```

## 方法三：从右上角或者左下角开始查找：
```c++
// 右上角开始查找
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.empty()) return false;
        int m = matrix.size();
        int n = matrix[0].size();
        int i = 0;
        int j = n - 1;
        while (i < m && j >= 0) {
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] < target) {
                i++;
            } else if (matrix[i][j] > target) {
                j--;
            }
        }
        return false;
    }
};

// 左下角开始查找
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.empty()) return false;
        int m = matrix.size();
        int n = matrix[0].size();
        int i = m-1;
        int j = 0;
        while (i >=0 && j < n) {
            if (matrix[i][j] == target) {
                return true;
            } else if (matrix[i][j] < target) {
                j++;
            } else if (matrix[i][j] > target) {
                i--;
            }
        }
        return false;
    }
};
```