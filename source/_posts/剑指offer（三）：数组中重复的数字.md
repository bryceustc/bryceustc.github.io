---
title: 剑指offer（三）：数组中重复的数字
date: 2023-09-16 21:11:41
tags: 剑指Offer
categories: 剑指Offer
mathjax: true
---
# 题目描述:数组中重复的数字

## 题目一：
在一个长度为n的数组里的所有数字都在0~n-1范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。
请找出数组中任意一个重复的数字。例如，如果输入长度为7的数组{2，3，1，0，2，5，3}，那么对应的输出是重复的数字2或者3
<!--more-->
# 本题考点：
  
  1). 一维数组的理解，一维数组在内存中占据连续空间，可以根据下标定位对应的元素
  
  2). 二分查找算法的理解，快速正确实现二分查找
  
  3). 时间复杂度与空间复杂度的理解
  
# 解题思路:
  1). 把输入的数组进行排序，排序后再判断有无重复数字，时间复杂度为O(nlogn)
  
  2). 使用哈希表来解决，时间复杂度为O(n)，但空间复杂度也为O(n)
  
  3). **交换位置重排法**，把每个数字放回对应位置的方法。如果出现一个数字无法放回（所在位置已经是对应数字了），那么说明该数字重复，**时间复杂度为O(n)，空间复杂度为O(1)**

# 代码

[C++](https://github.com/bryceustc/CodingInterviews/blob/master/DuplicationInArray/DuplicationInArray.cpp)


# C++
## 方法一：排序后直接查找
```c++
class Solution{
    public:
        bool Duplication1(vector<int> nums,int n, vector<int> &res){
            bool flag = false;
            if (nums.empty()||n<0) return false;
            for (int i = 0; i < n; i++)
            {
                if (nums[i] < 0 || nums[i] > n - 1)
                {
                    return false;
                }
            }
            sort(nums.begin(),nums.end());tem
            for (int i = 0; i < n-1; i++)
            {
                if (nums[i]==nums[i+1])
                {
                    res.push_back(nums[i]);
                    flag = true;
                }
            }
            return flag;
        }
};
```

## 方法二：利用哈希表
```c++
class Solution{
    public:
        bool Duplication2(vector<int> nums,int n, vector<int> &res){
            bool flag = false;
            if(nums.empty() || n < 0) return false;
            for (int i = 0; i < n; i++)
            {
                if (nums[i] < 0 || nums[i] > n-1)
                {
                    return false;
                } 
            }
            
            unordered_map<int,int> m;
            for (auto i : nums)
            {
                m[i]++;
                if(m[i]>1)
                {
                    res.push_back(i);
                    flag = true;
                }
            }
            return flag;
        }
};
```

## 方法三：交换位置重排
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            //只要当前数值与索引不等，就要一直替换
            while (nums[i] != i) {
                //一旦遇到当前的数值与另外一个换位的值相等，就停止，说明找到了重复的数字
                if (nums[i] == nums[nums[i]]) {
                    return nums[i];
                }
                int temp = nums[i];
                nums[i] = nums[temp];
                nums[temp] = temp;
            }
        }
        return -1;
    }
};
```

# 参考：
 - [循环结构中break、continue、return和exit的区别](https://blog.csdn.net/hunanchenxingyu/article/details/8101795)
 
 - [时间复杂度和空间复杂度](https://blog.csdn.net/zolalad/article/details/11848739)
 - [二分查找算法](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Find-First-And-Last-Position-Of-Element-In-Sorted-Array/BinarySearch.md)
