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

   - [344.反转字符串](https://leetcode-cn.com/problems/reverse-string/)

     编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

     不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

     你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

     ```python
     class Solution:
         def reverseString(self, s: List[str]) -> None:
             """
             Do not return anything, modify s in-place instead.
             """
             i, j = 0, len(s) - 1
             while i < j:
                 s[i], s[j] = s[j], s[i]
                 i += 1
                 j -= 1
     ```

     

   - [剑指Offer58-1.翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

     输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

     ```python
     class Solution:
         def reverseWords(self, s: str) -> str:
             s = s.strip()
             res = []
             i = j = len(s) - 1
             while i >= 0:
                 while i >= 0 and s[i] != ' ': i -= 1
                 res.append(s[i + 1: j + 1])
                 while s[i] == ' ': i -= 1
                 j = i
             return ' '.join(res)
     ```

     

   - [125.验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

     给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

     **说明：**本题中，我们将空字符串定义为有效的回文串。

     ```python
     class Solution:
         def isPalindrome(self, s: str) -> bool:
             i, j = 0, len(s) - 1
             while i < j:
                 while i < j and not s[i].isalnum(): i += 1
                 while i < j and not s[j].isalnum(): j -= 1
                 if i < j:
                     if s[i].lower() != s[j].lower(): return False
                     i, j = i + 1, j - 1
             return True
     ```

     

   - 9.回文数

     给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。

     回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，121 是回文，而 123 不是。

     ```python
     
     ```

     

   - 58.最后一个单词的长度

   - 剑指Offer05.替换空格

   - 剑指Offer58-2.左旋转字符串

   - 26.删除排序数组中的重复项

   - 剑指Offer67.把字符串转换成整数
