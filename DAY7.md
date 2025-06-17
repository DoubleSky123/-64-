
# DAY 6｜454. 4Sum II 383. Ransom Note 15. 3Sum 18. 4Sum
## 学习内容
[454讲解](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
[383讲解](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)
[15讲解](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)
[18讲解](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)
## 454. 4Sum II
[Leetcode Link](https://leetcode.cn/problems/4sum-ii/description/)-Medium
### Description
>Given four integer arrays nums1, nums2, nums3, and nums4 all of length n, return the number of tuples (i, j, k, l) such that:
>0 <= i, j, k, l < n
>nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
>>**Example 1:**
>>**Input:**
>>nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
>>**Output:**
>>2
>>**Explanation:**
>>The two tuples are:
>>1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
>>2. 2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
### Code
>这道题因为是要在4个数组里取四个数看是否能组成0，可以把四个数组分成两个两个，用dict储存。
>key是前两个数组的和因为需要查找和看是否是后两个数和的相反数。value是出现次数，因为这道题不用去重，就直接+=出现次数。
```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        dic = {}
        for a in nums1:
            for b in nums2:
                if a+b in dic:
                    dic[a+b] += 1
                else:
                    dic[a+b] = 1
        count = 0
        for c in nums3:
            for d in nums4:
                target = 0-(c+d)
                if target in dic:
                    count += dic[target]
        return count
```
> - Time: O(N**2)
> - Space: O(N**2)
## 383. Ransom Note
[Leetcode Link](https://leetcode.cn/problems/ransom-note/description/)-Easy
### Description
>Given two strings ransomNote and magazine, return true if ransomNote can be constructed by using the letters from magazine and false otherwise.
>Each letter in magazine can only be used once in ransomNote.
>>**Example 1:**
>>**Input:**
>>ransomNote = "aa", magazine = "aab"
>>**Output:**
>>true
### Code
#### Method1 数组哈希表
>这道题和昨天的字母异位词差不多，相同的方法
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        ransom_count = [0] * 26
        magazine_count = [0] * 26
        for c in ransomNote:
            ransom_count[ord(c) - ord('a')] += 1
        for c in magazine:
            magazine_count[ord(c) - ord('a')] += 1
        return all(ransom_count[i] <= magazine_count[i] for i in range(26))
```
> - Time: O(N)
> - Space: O(1)
#### Method2 Counter
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return Counter(ransomNote) <= Counter(magazine)
```
> - Time: O(N)
> - Space: O(1)
## 15. 3Sum
[Leetcode Link](https://leetcode.cn/problems/3sum/description/)- Medium
### Description
>Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.
>Notice that the solution set must not contain duplicate triplets.
>>**Example 1:**
>>**Input:**
>>nums = [-1,0,1,2,-1,-4]
>>**Output:**
>>[[-1,-1,2],[-1,0,1]]
>>**Explanation:**
>>nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
>>nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
>>nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
>>The distinct triplets are [-1,0,1] and [-1,-1,2].
>>Notice that the order of the output and the order of the triplets does not matter.n = 19
### Code
>用`双指针`的思路，遍历数组，left指针在i的后一位，right指针在末尾。然后把三个数相加，如果大了，right左移，如果小了，left右移。直到left和right靠拢。
>为什么可以用双指针，而两数之和不行，因为两数之和需要返回下标，这道题只是数值，所以可以对数组进行排序。
>难点是需要去重，需要对三个数都要去重。当`i = i-1`相等跳过。为什么必须和前一位比较，因为如果和后一位进行比较，会跳过第一个获取的结果比如结果是`[-1,-1,2]`。
>为什么left，right也要去重，因为在遍历的时候先固定了i，每一个i会移动left和right，所以只需要一组答案。
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        if nums[0] > 0:
            return []
        for i in range(len(nums)):
            if i >0 and nums[i] == nums[i-1]:
                continue
            left = i+1
            right = len(nums)-1
            while right > left:
                threesum = nums[i] + nums[left] + nums[right]
                if threesum < 0:
                    left += 1
                elif threesum >0:
                    right -= 1
                else:
                    result.append([nums[i], nums[left], nums[right]])

                    while right > left and nums[right] == nums[right-1]:
                        right -= 1
                    while right > left and nums[left] == nums[left+1]:
                        left += 1
                    right-=1
                    left+=1
        return result
```
> - Time: O(N**2)
> - Space: O(N)
## 18. 4Sum
[Leetcode Link](https://leetcode.cn/problems/4sum/description/)-Easy
### Description
>Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:
>0 <= a, b, c, d < n
>a, b, c, and d are distinct.
>nums[a] + nums[b] + nums[c] + nums[d] == target
>You may return the answer in any order.
>>**Example 1:**
>>**Input:**
>>nums = [1,0,-1,0,-2,2], target = 0
>>**Output:**
>>[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
### Code
>和3sum思路相同也是双指针，再多一层for循环k，i在k的后面。时间复杂度为3次方。
>注意`剪枝`，排序后如果目标值大于0且第一位大于0可以剪枝，遍历i时可以`二次剪枝`。小于0不能剪枝。
```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        result = []
        nums.sort()
        if target >0 and nums[0] >0 and nums[0] >target: ##一级剪枝
            return result
        for k in range(len(nums)):
            if k >0 and nums[k] == nums[k-1]: ## 一级去重
                continue
            for i in range(k+1,len(nums)):
                if nums[i]+nums[k] > target and nums[i] >0: ##二级剪枝
                    break
                if i > k+1 and nums[i] == nums[i-1]: ## 二级去重
                    continue
                left = i + 1
                right = len(nums)-1
                
                while right > left:
                    sum_ = nums[k] + nums[i] + nums[left] + nums[right]
                    if sum_ < target:
                        left+=1
                    elif sum_ > target:
                        right-=1
                    else:
                        result.append([nums[k],nums[i],nums[left],nums[right]])

                        while right > left and nums[right] == nums[right-1]:
                            right -=1
                        while right > left and nums[left] == nums[left+1]:
                            left +=1
                        
                        left += 1
                        right -= 1
        return result
```
> - Time: O(N**3)
> - Space: O(N)
## 今日心得
- 3sum和4sum非常难，需要想到双指针的思路，并且注意剪枝和去重。
- 哈希总结：一般来说哈希表都是用来快速判断一个元素是否出现集合里。
- 哈希表的作用在于降低时间复杂度，有数组，集合，字典三种形式：
- 一、数组可以用在有固定的26个字母或长度较短的场景
- 二、集合可以自动去重
- 三、字典可以用在数组处理不了的场景，数字可以较大，也可以用来记录索引等
