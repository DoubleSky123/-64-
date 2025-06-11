# DAY 1｜704.Binary Search27. Remove Element977. Squares of a Sorted Array
## 学习内容
[数组理论基础](https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
[704讲解](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html)
[27讲解](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)
[977讲解](https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html)
## 704.Binary Search
[Leetcode Link](https://leetcode.cn/problems/binary-search/description/)-Easy
### Description
>Given an array of integers nums which is sorted in ascending order, 
>and an integer target, write a function to search target in nums. 
>If target exists, then return its index. Otherwise, return -1.
>You must write an algorithm with O(log n) runtime complexity.
>> **Example 1:**
>> **Input:** nums = `[-1,0,3,5,9,12]`, target = 9
>> **Output:** 4
>> **Explanation:** 9 exists in nums and its index is 4
### Code
#### Method1 [左闭右开)
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left,right = 0,len(nums)
        while left < right:
            mid = left+(right-left)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid +1
            else: 
                right = mid
        return -1
```
> - Time: O(Logn)
> - Space: O(1)
#### Method2 [左闭右闭]
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left,right = 0,len(nums)-1
        while left <= right:
            mid = left+(right-left)//2
            if nums[mid] == target:
                return mid
            elif nums[mid]> target:
                right = mid-1
            else:
                left = mid+1
        return -1
```
> - Time: O(Logn)
> - Space: O(1)
## 27. Remove Element
[Leetcode Link](https://leetcode.cn/problems/remove-element/description/)-Easy
### Description
>Given an integer array nums and an integer val, remove all occurrences of val in nums in-place.
> The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.
>Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:
>Change the array nums such that the first k elements of nums contain the elements which are not equal to val.
>The remaining elements of nums are not important as well as the size of nums.
>Return k.
>> **Example 1:**
>> **Input:** nums = `[3,2,2,3]`, val = 3
>> **Output:** 2, nums = `[2,2,_,_]`
>> **Explanation:** Your function should return k = 2, with the first two elements of nums being 2.
>> It does not matter what you leave beyond the returned k (hence they are underscores).
### Code
#### Method1 暴力
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i, l = 0,len(nums)
        while i < l:
            if nums[i] == val:
                for j in range(i+1,l):
                    nums[j-1] = nums[j]
                l -= 1
                i -= 1
            i += 1
        return i
```
> - Time: O(N**2)
> - Space: O(1)
#### Method2 双指针
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow,fast = 0,0
        while fast < len(nums):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```
> - Time: O(N)
> - Space: O(1)
## 977. Squares of a Sorted Array
[Leetcode Link](https://leetcode.cn/problems/squares-of-a-sorted-array/description/)-Easy
### Description
>Given an integer array nums sorted in non-decreasing order,
>return an array of the squares of each number sorted in non-decreasing order.
>>**Example 1:**
>>**Input:** nums = `[-4,-1,0,3,10]`
>>**Output:** `[0,1,9,16,100]`
>>**Explanation:** After squaring, the array becomes `[16,1,0,9,100]`.
>>After sorting, it becomes `[0,1,9,16,100]`.
### Code
#### Method1 暴力
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        result = []
        for i in nums:
            result.append(i**2)
        result.sort()
        return result
```
> - Time: O(Nlogn)
> - Space: O(N)
#### Method2 双指针
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        l, r, k = 0,len(nums)-1,len(nums)-1
        result = [0] * (len(nums))

        while k >= 0:
            if nums[l] **2 <= nums[r] **2:
                result[k] = nums[r] **2
                r -= 1
            else:
                result[k] = nums[l] **2
                l += 1
            k -= 1
        

        return result
```
> - Time: O(N)
> - Space: O(N)

## 今日心得
- 其实是二刷代码随想录了，在参加训练营之前自己先过了一遍，所以二刷数组题目，思路还比较清晰，相当于复习一遍，有一些细节的地方较第一遍有遗忘再记录一下。
- 第一题：计算中间值的方法一开始写成了`mid = left+right//2 `，看了题解发现是`mid = left+(right-left)//2`。群里的录友帮我解答了他们的数学逻辑是相同的，所以在lc上跑是可以过的。但是如果数组长度较大，left+right有可能导致导致整数溢出从而导致mid错误。而right-left的值一定小于数组长度，因此更优。并且复习了左闭右开和左闭右闭两种写法。
- 第二题：一开始想到了用双指针，但是忘了双指针如何实现，尝试了半小时之后还是看了题解，发现思路和暴力类似。遍历数组的时候如果和目标值相同，就将数组往前平移进行替换，因此需要快慢指针。快指针进行比较，如果不同就和处在前面的慢指针替换，并且慢指针+1。暴力也是一样，遍历数组的同时，碰到相同就向前平移。但是平移的时候需要注意，数组在缩短，因此每次遍历都要从头开始遍历，不然每次缩短都会遗漏下一个下标。
- 第三题：暴力用python内置排序函数比较简单，值得注意的是时间复杂度为O(Nlogn)，因为包含了排序的时间，而双指针则是O(N)。`如果是第一次做肯定想不到双指针。`首先首尾的平方和一定是最大的，因为是有序数组。然后定义首尾两个指针，开始比较平方和，哪个大就放入新的结果数组，并且指针移动直到结果放满，因此只要遍历一遍数组，时间复杂度是O(N)。
- 总结：第二遍刷，还是有很多值得思考的地方，双指针的方法还是很难想到需要去看题解。第一天打卡完成！继续努力坚持！




