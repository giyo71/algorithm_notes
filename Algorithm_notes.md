# Algorithm notes

1. **纯编程题**

   例题：

   - [1.两树之和](https://leetcode-cn.com/problems/two-sum/)

     给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

     你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

     你可以按任意顺序返回答案。

     ```python
     class Solution:
         def twoSum(self, nums: List[int], target: int) -> List[int]:
             dic = {}
             for i in range(len(nums)):
                 if target - nums[i] in dic: return [dic[target - nums[i]], i]
                 dic[nums[i]] = i
             return []
     ```

   - [1108.IP地址无效化](https://leetcode-cn.com/problems/defanging-an-ip-address/)

     给你一个有效的 IPv4 地址 `address`，返回这个 IP 地址的无效化版本。

     所谓无效化 IP 地址，其实就是用 `"[.]"` 代替了每个 `"."`。

     ```python
     class Solution:
         def defangIPaddr(self, address: str) -> str:
             res = []
             for c in address:
                 if c != '.': res.append(c)
                 else: res.append('[.]')
             return ''.join(res)
     ```

   - 344.反转字符串
   - 剑指Offer58-1.翻转单词顺序
   - 125.验证回文串
   - 9.回文数
   - 58.最后一个单词的长度
   - 剑指Offer05.替换空格
   - 剑指Offer58-2.左旋转字符串
   - 26.删除排序数组中的重复项
   - 剑指Offer67.把字符串转换成整数
