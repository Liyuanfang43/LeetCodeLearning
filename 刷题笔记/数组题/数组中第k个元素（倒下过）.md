# 数组中第k个最大元素
题目描述：
给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。

我的题解：
````py
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        if len(nums) == 1:
            return nums[0]
        def find(target, nums, k):
            small, large = [], []
            for n in nums:
                if target > n:
                    small.append(n)
                else:
                    large.append(n)
            # 若large的长度为k-1，则第k个最大元素就是target
            if len(large) == k - 1:
                return target
            # 若large长度大于k-1，则第k个最大元素在large中
            elif len(large) > k - 1:
                target = large[0]
                return find(target, large[1:], k)
            else:
                k = k - len(large) - 1
                target = small[0]
                return find(target, small[1:], k)
        
        target = nums[0]
        return find(target, nums[1:], k)
````
思路：使用递归快速排序的思想，将数组不断分割为两部分，寻找最大元素。
但是运行超出内存限制，空间复杂度过高。时间复杂度O(nlogn),分析：以上写法对数组中大量相同元素的case运算会非常缓慢，因为当target=数组中元素时也将其归类为small中，进行了多次重复计算。
使用如下的方案改进：
````py
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def quick_select(nums, k):
            pivot = nums[0]
            big, equal, small = [], [], []
            for num in nums:
                if num > pivot:
                    big.append(num)
                elif num < pivot:
                    small.append(num)
                else:
                    equal.append(num)
            if k <= len(big):
                return quick_select(big, k)
            if len(nums) - len(small) < k:
                return quick_select(small, k - len(nums) + len(small))
            
            return pivot
        return quick_select(nums, k)
````
时间复杂度分析：O（n），因为对长度N每轮递归后后续的子数组平均长度是n/2,平均情况下为n+n/2+n/4+ ... + n/n = 2n-1

