二分查找
===
* [一. 寻找一个书](#寻找一个数)

* [二. 寻找左侧边界](#寻找左侧边界)

* [三. 寻找右侧边界](#寻找右侧边界)

* 代表题：LC 34、 LC 704、 LCR53

## 寻找一个数
```python
def binarySearch(nums,target):
    left = 0
    right = len(nums)-1
    while left<=right:
        mid = left+(right-left)//2 #防止溢出
        if nums[mid]==target:
            return mid
        elif nums[mid]<target:
            left = mid+1
        elif nums[mid]>target:
            right = mid-1
```
**为什么不用right=len(nums) while left<right?**

while left<right 的终止条件为left $==$ right，写成区间形式就是[right,right],区间非空right index会被直接跳过返回-1 

## 寻找左侧边界
```python
def SearchLeft(nums,target):
    left = 0
    right = len(nums)
    while left<right:
        mid = left+(right-left)//2 #防止溢出
        if nums[mid]==target:
            right = mid
        elif nums[mid]<target:
            left = mid+1
        elif nums[mid]>target:
            right = mid
    return left
```
**为什么可以找到左侧边界**
找到target时不立即返回，缩小上界不断向左收缩达到左侧边界的目的

## 寻找右侧边界
```python
def SearchRight(nums,target):
    left = 0
    right = len(nums)
    while left<right:
        mid = left+(right-left)//2 #防止溢出
        if nums[mid]==target:
            left = mid+1
        elif nums[mid]<target:
            left = mid+1
        elif nums[mid]>target:
            right = mid
    return left-1
```

总结
===
找到元素==直接返回、左侧边界==收紧右界、右侧边界==收紧左界+返回减一