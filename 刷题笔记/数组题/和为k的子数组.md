# 和为k的子数组

给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的子数组的个数 。

子数组是数组中元素的连续非空序列。

 

示例 1：

输入：nums = [1,1,1], k = 2
输出：2
示例 2：

输入：nums = [1,2,3], k = 3
输出：2
 

提示：

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107


** 思路：前缀和解法，用一个数组s记录nums的前缀和，该数组长度为0：len(nums) + 1，则i到j-1的和为s[j]-s[i].对每个j值找出值为s[j]-k的i的个数，最后再求和（为降低时间复杂度，使用哈希表保存值为s[j]-k的i的个数的统计） **

````py
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        # 暴力解法：遍历， 复杂度O(n2)
        # res = [0] * len(nums)
        # if nums[0] == k:
        #         res[0] = 1
        # for i in range(1, len(nums)):
        #     tmp = 0
        #     count = 0
        #     for j in range(i, -1, -1):
        #         tmp += nums[j]
        #         if tmp == k:
        #             count += 1
        #     res[i] = res[i - 1] + count
        # return res[-1]

        # 前缀和解法
        prefix_s = [0] * (len(nums) + 1)
        for i in range(len(nums)):
            prefix_s[i + 1] = prefix_s[i] + nums[i]
        
        c_dict = {}
        ans = 0
        for i in range(len(prefix_s)):
            if prefix_s[i] - k in c_dict:
                ans += c_dict[prefix_s[i] - k]
            if prefix_s[i] in c_dict.keys():
                c_dict[prefix_s[i]] += 1
            else:
                c_dict[prefix_s[i]] = 1
        return ans
````