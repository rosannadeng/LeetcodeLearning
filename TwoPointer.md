双指针
===
## 数组
### 快慢指针
通常使用于原地修改数组和通过扩大或缩小窗口来解决的问题
- 26. 删除有序数组中的重复项 -> 返回最新数组长度
    - 当快指针指向不同元素时 移动慢指针 并对位置进行数值覆盖
    - 否则移动快指针
```python
def remoeDuplicate(self,nums):
    if len(nums)==0:
        return 0
    i = 0 
    j = 1
    while j<len(nums):
        if nums[j]!=nums[i]:
            i+=1
            nums[i]=nums[j]
        j+=1
    return i+1
```
- 283. 移动零 -> None
    - 思路一致，增加的是将slow指针后的元素改为0
    ```python
    def moveZero(self,nums):
        if len(nums)==0:
            return 0
        i = 0 
        j = 1
        while j<len(nums):
            if nums[j]!=0:
                nums[i]=nums[j]
                i+=1
            j+=1
        for index in range(i,len(nums)):
            nums[index]=0
    ```
    
### 左右指针
#### 二分查找
#### nSum
- 167. 两数之和 -> 返回index
```python
def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        left = 0
        right = len(numbers)-1
        while left<right:
            if numbers[left]+numbers[right]>target:
                right-=1
            elif numbers[left]+numbers[right]<target:
                left+=1
            else:
                return [left+1,right+1]
        return None
```

 **如果把结果改为 不重复的value呢？**
- 排序+左右指针

*通过修改while loop 跳过所有重复元素即可*

```python
def twoSumTarget(nums, target):
    # nums必须有序
    nums.sort()
    lo, hi = 0, len(nums) - 1
    res = []
    while lo < hi:
        s = nums[lo] + nums[hi]
        left, right = nums[lo], nums[hi]
        if s < target:
            while lo < hi and nums[lo] == left:
                lo += 1
        elif s > target:
            while lo < hi and nums[hi] == right:
                hi -= 1
        else:
            res.append([left, right])
            while lo < hi and nums[lo] == left:
                lo += 1
            while lo < hi and nums[hi] == right:
                hi -= 1
    return res
```
由于排序算法时间复杂度更新为 -> O(NlogN)
- Nsum问题的本质就是不断对指针开始位置进行更新并对target进行迭代 从target-> target-nums[i] -> target-nums[i]-nums[j]->...
```python
def nSumTarget(nums,n,start,target):
    sz = len(nums)
    res = []
    # 至少是 2Sum，且数组大小不应该小于 n
    if n < 2 or sz < n:
        return res
    # 2Sum 是 base case
    if n == 2:
        # 双指针那一套操作
        lo, hi = start, sz - 1
        while lo < hi:
            sum = nums[lo] + nums[hi]
            left, right = nums[lo], nums[hi]
            if sum < target:
                while lo < hi and nums[lo] == left:
                    lo += 1
            elif sum > target:
                while lo < hi and nums[hi] == right:
                    hi -= 1
            else:
                res.append([left, right])
                while lo < hi and nums[lo] == left:
                    lo += 1
                while lo < hi and nums[hi] == right:
                    hi -= 1
    else:
        # n > 2 时，递归计算 (n-1)Sum 的结果
        for i in range(start, sz):
            sub = nSumTarget(nums, n - 1, i + 1, target - nums[i])
            for arr in sub:
                # (n-1)Sum 加上 nums[i] 就是 nSum
                arr.append(nums[i])
                res.append(arr)
            while i < sz - 1 and nums[i] == nums[i + 1]:
                i += 1
    return res
```
#### 反转数组

#### 回文串判断 
- 5. 最长回文串 -> 字符串
    - 最长问题先从最短找起，目标字符串的长度不确定时 可能是奇数也可能是偶数，因此左右指针不再是从两头找起，而是从中心扩散
    ```python
    def longestPlaindrome(s):
        res = ""
        for i in range(len(s)):
            s1 = helper(s,i,i) //odd
            s2 = helper(s,i,i+1) //even
            res = res if len(res)>len(s1) else s1
            res = res if len(res)>len(s2) else s2
        return res
    def helper(s,cen1,cen2):
        while cen1>=0 and cen2<len(s) and s[cen1]==s[cen2]:
            cen1-=1
            cen2+=1
        return s[cen1+1:cen2]
    ```
    
---

## 链表
### 快慢指针
- 83.删除排序链表中的重复元素 -> 去重后的链表
    - 和26题思路一致，区别是使用pointer替换赋值
    ```python
    def deleteDuplicate(self,head):
        if not head:
            return None
        slow,fast = head,head
        while fast:
            if fast.val!=slow.val:
                slow.next = fast
                slow = slow.next
            fast = fast.next
        slow.next = None #去尾，断开与后面元素的连接
        return head
    ```