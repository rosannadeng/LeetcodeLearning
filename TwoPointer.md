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
- 167. 两数之和
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
- 83. 删除排序链表中的重复元素 -> 去重后的链表
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