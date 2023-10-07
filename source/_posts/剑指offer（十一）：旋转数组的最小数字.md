---
title: 剑指offer（十一）：旋转数组的最小数字
date: 2023-10-07 20:00:02
tags: 剑指Offer
categories: 剑指Offer
mathjax: true
---


# 题目描述:

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
<!--more-->
# 本题考点：
  
  1). 数组非减排序
  
  2). 二分查找
  
# 解题思路:
  1). 直接暴力遍历数组所有元素，寻找最小元素，时间复杂度为O(n)
  
  2). 使用二分查找，时间复杂度为O(log<sub>2</sub>n)

  用二分法查找，需要始终将目标值（这里是最小值）套住，并不断收缩左边界或右边界。左、中、右三个位置的值相比较，有以下几种情况：

  a. 左值 < 中值, 中值 < 右值 ：没有旋转，最小值在最左边，可以**收缩右边界**

            右
        中
    左
  
  b. 左值 > 中值, 中值 < 右值 ：有旋转，最小值在左半边，可以**收缩右边界**

    左       右
        中  
  

  c. 左值 < 中值, 中值 > 右值 ：有旋转，最小值在右半边，可以**收缩左边界**
            
        中
    左      右

  d. 左值 > 中值, 中值 > 右值 ：单调递减，不可能出现

    右        
        中
            左

分析前面三种可能的情况，会发现情况1、2是一类，情况3是另一类。

如果中值 < 右值，则最小值在左半边，可以收缩右边界。 

如果中值 > 右值，则最小值在右半边，可以收缩左边界。 

通过比较中值与右值，可以确定最小值的位置范围，从而决定边界收缩的方向。

而情况1与情况3都是左值 < 中值，但是最小值位置范围却不同，这说明，如果只比较左值与中值，不能确定最小值的位置范围。

所以我们需要通过**比较中值与右值来确定最小值的位置范围**，进而确定边界收缩的方向。


# 代码

## C++:
### 方法一：暴力遍历
```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        if (nums.empty()) return 0;
        int n = nums.size();     
        int res = nums[0];
        for (int i=1;i<n;i++)
        {
            if (res>nums[i])
                res = nums[i];
        }
        return res;
    }
};
```

### 方法二：二分查找：
```c++
class Solution{
    public:
        int findMin(vector<int>&nums)
        {
            if (nums.empty()) return 0;
            int n = nums.size();     
            int start = 0;
            int end = n-1;
            while (end > start) {
                int mid = start + (end - start) / 2;
                if (nums[mid] > nums[end]) { /* 中值 > 右值，最小值在右半边，收缩左边界 */ 
                    start = mid + 1; /* 因为中值 > 右值，中值肯定不是最小值，左边界可以跨过mid */ 
                } else { /* 明确中值 < 右值，最小值在左半边，收缩右边界 */ 
                    end = mid; /* 因为中值 < 右值，中值也可能是最小值，右边界只能取到mid处 */ 
                }
            }
            return nums[start]; /* start == end，最小值输出nums[start]或nums[end]均可 */   
        }
};
/* 包含重复元素的情况
class Solution {
public:
    int findMin(vector<int>& nums) {
        if (nums.empty()) return 0;
        int n = nums.size();
        int start = 0;
        int end = n-1;
        while(end > start)
        {
            int mid = start + (end-start)/2;
            if (nums[mid] > nums[end]) {
                start = mid+1;
            }
            else if (nums[mid] < nums[end]) {
                end = mid;
            }
            else {
                end--;
            }
        }
        return nums[start];
    }
};
*/
```



## Python:
### 方法一：暴力遍历：
```python
# -*- coding:utf-8 -*-
class Solution:
    def findMin(self, nums: List[int]) -> int:
        n = len(nums)
        if n==0:
            return 0
        res = nums[0]
        for i in range (n):
            if res >nums[i]:
                res = nums[i]
        return res


if __name__ == '_ main__':
    nums = [3,4,5,1,2]
    res = Solution().findMin(nums)
    print(res)
```

### 方法二：二分查找：
```python
# -*- coding:utf-8 -*-
class Solution:
    def findMin(self, nums: List[int]) -> int:
        n = len(nums)
        if n==0:
            return 0
        res = nums[0]
        start = 0
        end = n-1
        while end>=start:
            mid = start+(end-start)//2
            if nums[mid]>=res:
                start=mid+1
            if nums[mid]<res:
                end = mid-1
            res = min(res,nums[mid])
        return res


if __name__ == '_ main__':
    nums = [3,4,5,1,2]
    res = Solution().findMin(nums)
    print(res)
```

# 参考：
-  [二分查找算法](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Find-First-And-Last-Position-Of-Element-In-Sorted-Array/BinarySearch.md)

-  [寻找旋转排序数组中的最小值](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Find-Minimum-In-Rotated-Sorted-Array/README.md)
