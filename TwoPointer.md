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
- 167. 两数之和
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