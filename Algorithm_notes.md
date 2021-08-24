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

     

   - [9.回文数](https://leetcode-cn.com/problems/palindrome-number/)

     给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。

     回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，121 是回文，而 123 不是。

     ```python
     class Solution:
         def isPalindrome(self, x: int) -> bool:
             if x < 0 or (x % 10 == 0 and x != 0): return False
             reverted = 0
             while x > reverted:
                 reverted = reverted * 10 + x % 10
                 x //= 10
             return x == reverted or x == reverted // 10
     ```

     

   - [58.最后一个单词的长度](https://leetcode-cn.com/problems/length-of-last-word/)

     给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中最后一个单词的长度。

     **单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。

     ```python
     class Solution:
         def lengthOfLastWord(self, s: str) -> int:
             s = s.strip()
             i = j = len(s) - 1
             while i >= 0 and s[i] != ' ': i -= 1
             return j - i
           
     class Solution:
         def lengthOfLastWord(self, s: str) -> int:
             i = len(s) - 1
             while i >= 0 and s[i] == ' ': i -= 1
             j = i
             while i >= 0 and s[i] != ' ': i -= 1
             return j - i
     ```

     

   - [剑指Offer05.替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

     请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

     ```python
     class Solution:
         def replaceSpace(self, s: str) -> str:
             res = []
             for c in s:
                 if c == ' ': res.append('%20')
                 else: res.append(c)
             return ''.join(res)
     ```

     

   - [剑指Offer58-2.左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

     字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

     ```python
     class Solution:
         def reverseLeftWords(self, s: str, n: int) -> str:
             return s[n:] + s[:n]
     ```

     

   - [26.删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

     给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

     不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

     说明：不需要考虑数组中超出新长度后面的元素。

     ```python
     class Solution:
         def removeDuplicates(self, nums: List[int]) -> int:
             if not nums: return 0
             i = j = 0
             while j < len(nums):
                 if nums[i] != nums[j]:
                     i += 1
                     nums[i] = nums[j]
                 j += 1
             return i + 1
     ```

     

   - [剑指Offer67.把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

     写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。

      

     首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

     当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

     该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

     注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

     在任何情况下，若函数不能进行有效的转换时，请返回 0。

     说明：

     假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2^31^,  2^31^ − 1]。如果数值超过这个范围，请返回  INT_MAX (2^31^ − 1) 或 INT_MIN (−2^31^) 。

     p.s. 补充一个语言`int`的边界：int的边界在 [-2147483648, 2147483647]

     p.s. 此题不要用 `int()` 函数
     
     ```python
     class Solution:
         def strToInt(self, str: str) -> int:
             s = str.strip()
             if not s: return 0
     
             res, i, sign = 0, 1, 1
             int_min, int_max, tmp = -2 ** 31, 2 ** 31 - 1, 2 ** 31 // 10
     
             if s[0] == '-': sign = -1
             elif s[0] != '+': i = 0
             for c in s[i:]:
                 if not '0' <= c <= '9': break
                 if res > tmp or res == tmp and c > '7': return int_max if sign == 1 else int_min
                 res = res * 10 + ord(c) - ord('0')
             return sign * res
     ```
     
     
     
     
