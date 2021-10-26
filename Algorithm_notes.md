# Algorithm notes

###### Content

0. 自定义数据结构

1. 纯编程题
2. 找规律题
3. 数组和链表
4. 栈和队列
5. 递归
6. 排序
7. 二分查找
8. 哈希表
9. 二叉树
10. 二叉树 + 前缀树 + 堆
11. 回溯
12. DFS + BFS
13. 动态规划



------



###### 0. 自定义数据结构

- 二叉树

  ```python
  # Definition for a binary tree node.
  class TreeNode:
  		def __init__(self, val=0, left=None, right=None):
      		self.val = val
      		self.left = left
      		self.right = right
  ```

  

- 链表

  ```python
  # Definition for singly-linked list.
  class ListNode:
    	def __init__(self, val=0, next=None):
  				self.val = val
          self.next = next
  ```



------



###### 1. 纯编程题

例题：

   - [1.两树之和](https://leetcode-cn.com/problems/two-sum/)

     给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

     你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

     你可以按任意顺序返回答案。

     ```python
     class Solution:
         def twoSum(self, nums: List[int], target: int) -> List[int]:
             dic = {}
             # 按顺序遍历nums
             for i in range(len(nums)):
                 # 如果target - nums[i]已经在dic中，
                 # 则代表该数 + nums[i] = target满足，可以直接返回两个数的index
                 if target - nums[i] in dic: return [dic[target - nums[i]], i]
                 # 否则继续将 nums[i] 加入到dic中
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
             # 循环遍历字符串 address 即可
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
             # i / j 是已对称的形式进行双指针原地调换位置
             # 当 i < j 不满足时，必有 i == j，则跳出循环，可以不必判断list的奇偶
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
             # 先删除字符串前后空格
             s = s.strip()
             res = []
             # 从后往前进行双指针遍历字符串
             i = j = len(s) - 1
             while i >= 0:
                 # 当前字符不为空格时，左指针 -1
                 while i >= 0 and s[i] != ' ': i -= 1
                 # 此时当前字符为空格，因此当前单词从 i + 1 开始
                 # 注意 j + 1 为计数右边界时，j + 1 不会计算在内的基础陷阱
                 res.append(s[i + 1: j + 1])
                 # 继续遍历，当前字符为空格时，左指针 -1
                 while s[i] == ' ': i -= 1
                 # 此时当前字符不为空格，右指针 = 当前字符
                 j = i
             return ' '.join(res)
     ```

     

   - [125.验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

     给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

     **说明：**本题中，我们将空字符串定义为有效的回文串。

     ```python
     class Solution:
         def isPalindrome(self, s: str) -> bool:
             # 双指针对称遍历字符串
             i, j = 0, len(s) - 1
             while i < j:
                 # 若 s[左指针] 不为字母或数字， 左指针 +1
                 # 若 s[右指针] 不为字母或数字， 右指针 -1
                 while i < j and not s[i].isalnum(): i += 1
                 while i < j and not s[j].isalnum(): j -= 1
                 # 此时两个指针都为字母或数字，判断是否相等即可
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
             # x为负数，或x为正数时个位数为0，均不为回文数
             if x < 0 or (x % 10 == 0 and x != 0): return False
             # 从右往左，将一半的数反转
             reverted = 0
             while x > reverted:
                 # 遍历时，反转数每增加一位，则加入原数x的个位数
                 reverted = reverted * 10 + x % 10
                 # 原数x通过去掉个位数减少一位
                 x //= 10
             # 偶位数时相等，奇位数时反转数去掉一位后相等
             return x == reverted or x == reverted // 10
     ```

     

   - [58.最后一个单词的长度](https://leetcode-cn.com/problems/length-of-last-word/)

     给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中最后一个单词的长度。

     **单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。

     ```python
     class Solution:
         def lengthOfLastWord(self, s: str) -> int:
             s = s.strip()
             # 双指针从右往左遍历字符串
             i = j = len(s) - 1
             while i >= 0 and s[i] != ' ': i -= 1
             return j - i
           
     class Solution:
         def lengthOfLastWord(self, s: str) -> int:
             # 手动去掉字符串首位空格
             i = len(s) - 1
             while i >= 0 and s[i] == ' ': i -= 1
             # 双指针从右往左遍历字符串
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
             # 循环遍历字符串s即可
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
             # 双指针从左往右遍历nums
             i = j = 0
             # 当右指针在范围内时
             while j < len(nums):
                 # 判断 nums[i] 与 nums[j] 是否相等
                 # 相等则右指针 +1 继续遍历
                 # 不等则左指针 +1 并赋值为右指针所在的新数字，接着右指针也 +1
                 if nums[i] != nums[j]:
                     i += 1
                     nums[i] = nums[j]
                 j += 1
             # 因为返回长度，注意 +1 陷阱
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
             
             # i先从1开始，sign默认正数
             res, i, sign = 0, 1, 1
             int_min, int_max, tmp = -2 ** 31, 2 ** 31 - 1, 2 ** 31 // 10
     
             # 判断是否为负数
             # 判断是否有符号，没有则i重新赋值为0
             if s[0] == '-': sign = -1
             elif s[0] != '+': i = 0
             # 顺序遍历处理后的字符串
             for c in s[i:]:
                 # 当前字符已经不为数字了，跳出for循环
                 if not '0' <= c <= '9': break
                 # 判断res是否已经大于tmp边界，若大于直接跳出边界值，不必进行多一位计算
                 # 若等于，判断一下最后一位数是否超过边界临界值，即 (> 7) = 8 or 9
                 # 虽然负数边界临界值可取8，但在计算时不能直接赋值，
                 # 因为符号与数值系统会分别判断边界
                 if res > tmp or res == tmp and c > '7': return int_max if sign == 1 else int_min
                 # 不能使用 int()
                 res = res * 10 + ord(c) - ord('0')
             return sign * res
     ```




------



###### 2. 找规律题

例题：

   - [面试题01.08.零矩阵](https://leetcode-cn.com/problems/zero-matrix-lcci/)

     编写一种算法，若M × N矩阵中某个元素为0，则将其所在的行与列清零。

     ```python
     class Solution:
         def setZeroes(self, matrix: List[List[int]]) -> None:
             """
             Do not return anything, modify matrix in-place instead.
             """
             # 分别创建行和列set，储存对应的为0的index
             row0, col0 = set(), set()
             # 循环遍历matrix
             for i in range(len(matrix)):
                 for j in range(len(matrix[0])):
                     if matrix[i][j] == 0:
                         row0.add(i)
                         col0.add(j)
             # 循环遍历行set，将行set里的所有行赋值为0
             for row in row0:
                 for j in range(len(matrix[0])):
                     matrix[row][j] = 0
             # 循环遍历列set，将列set里的所有列赋值为0
             for col in col0:
                 for i in range(len(matrix)):
                     matrix[i][col] = 0
     ```
     
     
     
   - [剑指Offer61.扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

     从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
   
     ```python
     class Solution:
         def isStraight(self, nums: List[int]) -> bool:
             joker = 0
             # 将nums从小到大排序后遍历
             nums.sort()
             for i in range(4):
                 if nums[i] == 0: joker += 1
                   # 有重复牌时直接返回False
                 elif nums[i] == nums[i + 1]: return False
             # 此时由于nums经过从小到大排序，因此 nums[joker] 即是不为0的最小数
             # 最大数 - 最小数 < 5，既能保证是顺子
             return nums[4] - nums[joker] < 5
     ```

     

   - [面试题16.11.跳水板](https://leetcode-cn.com/problems/diving-board-lcci/)
   
     你正在使用一堆木板建造跳水板。有两种类型的木板，其中长度较短的木板长度为shorter，长度较长的木板长度为longer。你必须正好使用k块木板。编写一个方法，生成跳水板所有可能的长度。
   
     返回的长度需要从小到大排列。
   
     ```python
     class Solution:
         def divingBoard(self, shorter: int, longer: int, k: int) -> List[int]:
             if k == 0: return []
             if shorter == longer: return [shorter * k]
             # 由于可能的跳水板总长度为 [shorter * (k-i) + longer * i]
             # 化简后可得 [short * k + (longer - shorter) * i]
             # 因此先计算 longer - shorter 可以加快方法效率
             tmp = longer - shorter
             return [shorter * k + tmp * i for i in range(k + 1)]
     ```
   
     
   
   - [面试题01.05.一次编辑](https://leetcode-cn.com/problems/one-away-lcci/)
   
     字符串有三种编辑操作:插入一个字符、删除一个字符或者替换一个字符。 给定两个字符串，编写一个函数判定它们是否只需要一次(或者零次)编辑。
   
     ```python
     class Solution:
         def oneEditAway(self, first: str, second: str) -> bool:
             if abs(len(first) - len(second)) > 1: return False
             # 保证first比second长，以此优化后续for循环
             if len(first) < len(second): first, second = second, first
             for i in range(len(second)):
                 if first[i] == second[i]: continue
                 # 当前位置的first和second字符不相等时，判断两种情况：
                 # 1. 两个字符串长度一样，两个字符串越过当前字符后的片段相等
                 # 2. 两个字符串差1长度，长字符+1位后的片段 == 短字符当前位后的片段
                 return first[i + 1:] == second[i + 1:] or first[i + 1:] == second[i:]
             return True
     ```
   
     
   
   - [面试题16.15.珠玑妙算](https://leetcode-cn.com/problems/master-mind-lcci/)
   
     珠玑妙算游戏（the game of master mind）的玩法如下。
   
     计算机有4个槽，每个槽放一个球，颜色可能是红色（R）、黄色（Y）、绿色（G）或蓝色（B）。例如，计算机可能有RGGB 4种（槽1为红色，槽2、3为绿色，槽4为蓝色）。作为用户，你试图猜出颜色组合。打个比方，你可能会猜YRGB。要是猜对某个槽的颜色，则算一次“猜中”；要是只猜对颜色但槽位猜错了，则算一次“伪猜中”。注意，“猜中”不能算入“伪猜中”。
   
     给定一种颜色组合solution和一个猜测guess，编写一个方法，返回猜中和伪猜中的次数answer，其中answer[0]为猜中的次数，answer[1]为伪猜中的次数。
   
     ```python
     class Solution:
         def masterMind(self, solution: str, guess: str) -> List[int]:
             dic1, dic2 = {}, {}
             answer = [0, 0]
             for i in range(len(solution)):
                 # 同位同色，‘猜中’直接+1
                 if solution[i] == guess[i]: answer[0] += 1
                 # 没有猜中，不断向dic1和dic2中加入pair 颜色：出现次数
                 else:
                     if solution[i] not in dic1: dic1[solution[i]] = 1
                     else: dic1[solution[i]] += 1
                     if guess[i] not in dic2: dic2[guess[i]] = 1
                     else: dic2[guess[i]] += 1
             # 如果dic1即solution中出现的颜色，也在dic2即guess中出现
             # ‘伪猜中’+两个dic共同出现的次数
             for key in dic1.keys():
                 if key in dic2: answer[1] += min(dic1[key], dic2[key])
             return answer
     ```
   
     
   
   - [55.跳跃游戏](https://leetcode-cn.com/problems/jump-game/)
   
     给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。
   
     数组中的每个元素代表你在该位置可以跳跃的最大长度。
   
     判断你是否能够到达最后一个下标。
   
     ```python
     class Solution:
         def canJump(self, nums: List[int]) -> bool:
             max_i = 0
             for i in range(len(nums)):
                 # 如果当前index超过了最大能到达的index max_i 直接返回False
                 if i > max_i: return False
                 # 如果当前index+当前index可跳数 > max_i
                 # 则更新max_i
                 if i + nums[i] > max_i: 
                     max_i = i + nums[i]
                     # 并判断更新后的 max_i + 1 是否 >= nums长度
                     # 若已经能到达，则直接返回True
                     if max_i + 1 >= len(nums): return True
             # 循环完成，所有index都没有超过max_i，则最终返回True
             return True
     ```
   
     
   
   - [48.旋转图像](https://leetcode-cn.com/problems/rotate-image/)
   
     给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。
   
     你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。
   
     ```python
     class Solution:
         def rotate(self, matrix: List[List[int]]) -> None:
             """
             Do not return anything, modify matrix in-place instead.
             """
             n = len(matrix)
             # e.g.
             # [102]    [401]
             # [000] -> [000]
             # [403]    [302]
             # 首先上下翻转
             # [403]
             # [000]
             # [102]
             for i in range(n // 2):
                 for j in range(n):
                     matrix[i][j], matrix[n-1-i][j] = matrix[n-1-i][j], matrix[i][j]
             # 其次箭头朝左下角45度的对角线翻转
             # [401]
             # [000]
             # [302]
             # 即可简化顺时针90度旋转
             for i in range(n):
                 for j in range(i):
                     matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
     ```
   
     
   
   - [54.螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)
   
     给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。
   
     ```python
     class Solution:
         def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
             # 首先创建上下左右边界
             # 并创建计数count，初始赋值该matrix所有数的数量
             l, r, t, b = 0, len(matrix[0]) - 1, 0, len(matrix) - 1
             count = len(matrix) * len(matrix[0])
             res = []
             # 当该matrix没有遍历完时
             while count >= 1:
                 # 贴着上边界，从左到右打印
                 # 上边界向下移动1
                 for i in range(l, r + 1):
                     if count < 1: break
                     res.append(matrix[t][i])
                     count -= 1
                 t += 1
                 # 贴着右边界，从上到下打印
                 # 右边界向左移动1
                 for i in range(t, b + 1):
                     if count < 1: break
                     res.append(matrix[i][r])
                     count -= 1
                 r -= 1
                 # 贴着下边界，从右到左打印
                 # 下边界向上移动1
                 for i in range(r, l - 1, -1):
                     if count < 1: break
                     res.append(matrix[b][i])
                     count -= 1
                 b -= 1
                 # 贴着左边界，从下到上打印
                 # 左边界向右移动1
                 for i in range(b, t - 1, -1):
                     if count < 1: break
                     res.append(matrix[i][l])
                     count -= 1
                 l += 1
             return res
     ```
   
     
   
   - [240.搜索二维矩阵2](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)
   
     编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：
   
     每行的元素从左到右升序排列。
     每列的元素从上到下升序排列。
   
     ```python
     class Solution:
         def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
             # 双指针，i代表行，j代表列
             # 从矩阵左下角开始遍历
             i, j = len(matrix) - 1, 0
             # 当双指针ij都在范围内时
             # 如果该点 小于 target: 向右移动一列
             # 如果该点 大于 target: 向上移动一行
             while i >= 0 and j < len(matrix[0]):
                 if matrix[i][j] < target: j += 1
                 elif matrix[i][j] > target: i -= 1
                 else: return True
             return False
     ```
     



------



###### 3. 数组和链表

知识点：

> 数组：
>
> (1) 数组用一组连续的内存空间，来存储一组具有相同数据类型的数据
>
> (2) 按照下标 `index` 快速访问数组元素，这个特性适合二分查找
>
> (3) 正常数组增删改查时间复杂度：O(1), O(n), O(1), O(1)
>
> 链表：
>
> (1) 链表使用非连续的内存空间来存储数据，通过next指针将内存快串联在一起
>
> (2) `head` 为头指针，`head.val` 为头指针指向的头节点所存储的数据，`head.next` 为头指针指向的头节点所存储的指向下一节点的指针
>
> (3) 正常链表增删改查时间复杂度：均是 O(n)

例题：

   - [203.移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

     给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

     ```python
     class Solution:
         def removeElements(self, head: ListNode, val: int) -> ListNode:
             if not head: return
             # 创建虚拟头节点 dum (cur) -> head
             dum = cur = ListNode(0)
             cur.next = head
             while cur.next:
                 # 若 cur.next 的值与目标值相等，则cur遍历指向下一个节点直到不相等
                 if cur.next.val == val: cur.next = cur.next.next
                 # 不相等时，向后传递cur指针继续进行循环
                 else: cur = cur.next
             return dum.next
     ```

     

   - [876.链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

     给定一个头结点为 `head` 的非空单链表，返回链表的中间结点。

     如果有两个中间结点，则返回第二个中间结点。

     ```python
     class Solution:
         def middleNode(self, head: ListNode) -> ListNode:
             # 创建快慢指针 head (slow / fast)
             slow = fast = head
             # 判断循环条件小技巧：赋值的前一位，即赋值时需要 fast.next.next，
             # 则循环时前一位 fast.next 需要存在才能赋值
             while fast and fast.next:
                 slow, fast = slow.next, fast.next.next
             return slow
     ```

     

   - [83.删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

     存在一个按升序排列的链表，给你这个链表的头节点 `head` ，请你删除所有重复的元素，使每个元素 **只出现一次** 。

     返回同样按升序排列的结果链表。

     ```python
     class Solution:
         def deleteDuplicates(self, head: ListNode) -> ListNode:
             if not head: return
             # 单指针遍历即可 head (cur)
             cur = head
             while cur.next:
                 # 若cur与cur.next的值相等，cur遍历指向下一节点
                 if cur.val == cur.next.val: cur.next = cur.next.next
                 else: cur = cur.next
             return head
     ```

     

     p.s. 该题与[203.移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)的差别是，203有处理头节点head的可能性，而该题head一定不会被删除，所以当需要处理头节点时，203便要创建虚拟头节点dum，该题则不需要。

     

   - [剑指Offer25.合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

     输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

     ```python
     class Solution:
         def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
             # 创建虚拟头节点 dum (cur)
             dum = cur = ListNode(0)
             # 遍历拼接即可
             while l1 and l2:
                 if l1.val < l2.val:
                     cur.next, l1 = l1, l1.next
                 else:
                     cur.next, l2 = l2, l2.next
                 cur = cur.next
             # 最后必有一个链表剩下，拼接上即可
             cur.next = l1 if l1 else l2
             return dum.next
     ```

     

   - [2.两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

     给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

     请你将两个数相加，并以相同形式返回一个表示和的链表。

     你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

     ```python
     class Solution:
         def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
             # 创建虚拟节点 dum (cur)
             dum = cur = ListNode(0)
             # 数字加法，carry 存储进位的数字
             carry = 0
             while l1 or l2:
                 tmp = 0
                 if l1: 
                     tmp += l1.val
                     l1 = l1.next
                 if l2: 
                     tmp += l2.val
                     l2 = l2.next
                 if carry != 0: tmp += carry
                 # carry 存储新的进位数字，即tmp除了个位数字的部分
                 carry = tmp // 10
                 # 将tmp个位数字遍历使用单指针cur拼接在dum后
                 cur.next = ListNode(tmp % 10)
                 cur = cur.next
             # 最后记得处理carry的剩余值
             if carry != 0: cur.next = ListNode(carry)
             return dum.next
     ```

     

   - [206.反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

     给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

     ```python
     class Solution:
         def reverseList(self, head: ListNode) -> ListNode:
             # 创建空节点 None (pre)
             # 使用双指针遍历将原链表第一位指向空节点形成的新链表 head (cur)
             pre, cur = None, head
             while cur:
                 tmp = cur.next
                 cur.next = pre
                 cur, pre = tmp, cur
             return pre
     ```

     

   - [234.回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

     给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

     ```python
     class Solution:
         def isPalindrome(self, head: ListNode) -> bool:
             if not head: return
             # 返回中点节点，偶数时为中点左节点
             def middleNode(head):
                 slow = fast = head
                 while fast.next and fast.next.next:
                     slow, fast = slow.next, fast.next.next
                 return slow
             # 反转节点
             def reverseList(head):
                 pre, cur = None, head
                 while cur:
                     tmp = cur.next
                     cur.next = pre
                     cur, pre = tmp, cur
                 return pre
             
             mid = middleNode(head)
             # 反转右半部分链表
             right_head = reverseList(mid.next)
             # 创建两个指针分别遍历左半部分和右半部分
             # head （cur）
             # right_head (right_cur)
             cur, right_cur = head, right_head
             # 奇数时，右半部分比左半部分少一个节点，所以以右半部分为循环条件
             while right_cur:
                 if cur.val != right_cur.val: return False
                 cur, right_cur = cur.next, right_cur.next
             # 将右半部分链表还原
             reverseList(right_head)
             return True
     ```

     

     p.s. 本题寻找中点与[876.链表的中间节点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)略有不同，因为当链表为奇数时，需要翻转该点下一点，为了保持偶数时算法的一致性，则将876返回中点右节点的循环条件改成该题返回中点左节点。翻转部分与[206.反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)一样。

     

   - [328.奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

     给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

     请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

     ```python
     class Solution:
         def oddEvenList(self, head: ListNode) -> ListNode:
             if not head: return
             # 创建奇数位头节点，并使用奇偶双指针 head (odd) -> even_head (even) 
             even_head = head.next
             odd, even = head, even_head
             # 奇数为还有 next 则代表仍需循环让偶数位连接该 next
             while even and even.next:
                 odd.next = even.next
                 odd = odd.next
                 even.next = odd.next
                 even = even.next
             # 偶数位最后拼接上奇数位头节点
             odd.next = even_head
             return head
     ```

     

   - [25.K个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

     给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

     k 是一个正整数，它的值小于或等于链表的长度。

     如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

     进阶：

     你可以设计一个只使用常数额外空间的算法来解决此问题吗？
     你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

     ```python
     class Solution:
         def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
             def reverse(head):
                 pre, cur = None, head
                 while cur:
                     tmp = cur.next
                     cur.next = pre
                     cur, pre = tmp, cur
                 return pre
             # 创建虚拟头节点 dum (pre / cur) -> head
             dum = ListNode(0, head)
             pre = cur = dum
             while cur.next:
                 # tmp即需要翻转的第一位
                 tmp = cur.next
                 # 循环让cur指针走完k步长
                 for i in range(k):
                     if cur: cur = cur.next
                 # 即最后链表节点数量不满k时直接停止while循环
                 if not cur: break
                 # 将该片段的链表与后面断开
                 # pre.next -> reverse(tmp) -> tmp_next
                 tmp_next, cur.next = cur.next, None
                 # 翻转断开的部分并与前后重新连接
                 pre.next = reverse(tmp)
                 tmp.next = tmp_next
                 # 双指针移动到翻转部分的最后一位
                 pre = cur = tmp
             return dum.next
     ```

     

   - [剑指Offer22.链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

     输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

     例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

     ```python
     class Solution:
         def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
             # 创建快慢指针 head (slow / fast)
             slow = fast = head
             # node (倒数k) -> ...... -> null
             # slow                     fast
             # for与while循环后两个指针位置如上，正好相差k
             for _ in range(k):
                 fast = fast.next
             while fast:
                 slow, fast = slow.next, fast.next
             return slow
     ```

     

   - [19.删除链表的倒数第N个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

     给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

     **进阶：**你能尝试使用一趟扫描实现吗？

     ```python
     class Solution:
         def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
             # 创建虚拟头节点以及快慢指针
             # dum (slow) -> head (fast)
             dum = ListNode(0, head)
             slow, fast = dum, head
             for _ in range(n):
                 fast = fast.next
             while fast:
                 slow, fast = slow.next, fast.next
             # 此时倒数第n个一定在slow的下一位
             slow.next = slow.next.next
             return dum.next
     ```

     

     p.s. 此题与[剑指Offer22.链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)的差别是，为了删除倒数第n个节点，需要用到倒数第n个节点的前一个节点，则在遍历时，slow需要慢一步即最终在倒数第n个节点的前一个节点上，增加虚拟头节点即可

     

   - [160.相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

     给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

     图示两个链表在节点 c1 开始相交：

     <img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png" style="zoom: 50%;" />

     题目数据 **保证** 整个链式结构中不存在环。

     **注意**，函数返回结果后，链表必须 **保持其原始结构** 。

     ```python
     class Solution:
         def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
             l1, l2 = headA, headB
             # l1与l2将会遍历相同的路程，最终在相同点上相遇
             while l1 != l2:
                 l1 = l1.next if l1 else headB
                 l2 = l2.next if l2 else headA
             return l1
     ```

     

   - [141.环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

     给定一个链表，判断链表中是否有环。

     如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

     如果链表中存在环，则返回 true 。 否则，返回 false 。

      

     进阶：

     你能用 O(1)（即，常量）内存解决此问题吗？

     ```python
     class Solution:
         def hasCycle(self, head: ListNode) -> bool:
             if not head or not head.next: return False
             # 为了循环条件快慢指针不相等，fast在初始赋值时就比slow快一步即可
             # head (slow) -> head.next (fast)
             slow, fast = head, head.next
             while slow != fast:
                 if not fast or not fast.next: return False
                 slow, fast = slow.next, fast.next.next
             return True
     ```



------



###### 4. 栈和队列

知识点：

> 栈：
>
> （1）后进先出
>
> （2）只允许在一端（栈顶）插入和删除数据
>
> （3）实现：数组栈 & 链表栈（头节点为后进点）
>
> p.s. 增加：.push(item) / .append(item)，删除：.pop()，栈顶：.peek()
>
> 队列：
>
> （1）先进先出
>
> （2）只能从队列头部取数据，尾部插入数据
>
> （3）实现：数组队列 & 链表队列（头节点为先进点）
>
> p.s. 增加：.append(item)，删除：.serve()，队首：.peek()

例题：

- [剑指Offer09.用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

  用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

  ```python
  class CQueue:
      def __init__(self):
          # 插入尾端栈
          self.stack1 = []
          # 删除头部栈
          self.stack2 = []
  
          
      def appendTail(self, value: int) -> None:
          self.stack1.append(value)
  
          
      def deleteHead(self) -> int:
          # 存在stack2直接返回
          if self.stack2: return self.stack2.pop()
          # 此时stack1和2均为空，直接返回 -1
          if not self.stack1: return -1
          # 存在stack1，stack2为空
          while self.stack1:
              # 即stack2是stack1的倒转
              self.stack2.append(self.stack1.pop())
          return self.stack2.pop()
  ```

  

- [225.用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

  请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（push、top、pop 和 empty）。

  实现 MyStack 类：

  void push(int x) 将元素 x 压入栈顶。
  int pop() 移除并返回栈顶元素。
  int top() 返回栈顶元素。
  boolean empty() 如果栈是空的，返回 true ；否则，返回 false 。

  注意：

  你只能使用队列的基本操作 —— 也就是 push to back、peek/pop from front、size 和 is empty 这些操作。你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

  ```python
  class MyStack:
  
      def __init__(self):
          """
          Initialize your data structure here.
          """
          self.queue1 = collections.deque()
          self.queue2 = collections.deque()
  
  
      def push(self, x: int) -> None:
          """
          Push element x onto stack.
          """
          # 由于队列先进先出，栈后进先出，为了使用队列的pop from front，push时便将所有元素倒置
          self.queue2.append(x)
          while self.queue1:
              self.queue2.append(self.queue1.popleft())
          self.queue1, self.queue2 = self.queue2, self.queue1
  
  
      def pop(self) -> int:
          """
          Removes the element on top of the stack and returns that element.
          """
          return self.queue1.popleft()
  
  
      def top(self) -> int:
          """
          Get the top element.
          """
          return self.queue1[0]
  
  
      def empty(self) -> bool:
          """
          Returns whether the stack is empty.
          """
          return not self.queue1
  ```

  

- [面试题03.05.栈排序](https://leetcode-cn.com/problems/sort-of-stacks-lcci/)

  栈排序。 编写程序，对栈进行排序使最小元素位于栈顶。最多只能使用一个其他的临时栈存放数据，但不得将元素复制到别的数据结构（如数组）中。该栈支持如下操作：push、pop、peek 和 isEmpty。当栈为空时，peek 返回 -1。

  ```python
  class SortedStack:
  
      def __init__(self):
          # 单调递减栈
          self.stack = []
  
  
      def push(self, val: int) -> None:
          tmp_stack = []
          # 如果栈尾元素大于要插入的元素
          # 将大于的元素压入辅助栈再放回即可
          while self.stack and val > self.stack[-1]:
              tmp_stack.append(self.stack.pop())
          self.stack.append(val)
          while tmp_stack:
              self.stack.append(tmp_stack.pop())
  
  
      def pop(self) -> None:
          if self.stack: self.stack.pop()
  
  
      def peek(self) -> int:
          return self.stack[-1] if self.stack else -1
  
  
      def isEmpty(self) -> bool:
          return not self.stack
  ```

  

- [155.最小栈](https://leetcode-cn.com/problems/min-stack/)

  设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

  push(x) —— 将元素 x 推入栈中。
  pop() —— 删除栈顶的元素。
  top() —— 获取栈顶元素。
  getMin() —— 检索栈中的最小元素。

  ```python
  class MinStack:
  
      def __init__(self):
          """
          initialize your data structure here.
          """
          self.stack = []
          # 单调递增栈
          self.min_stack = [math.inf]
  
  
      def push(self, val: int) -> None:
          self.stack.append(val)
          # 每次压入元素时判断最小值即可
          self.min_stack.append(min(val, self.min_stack[-1]))
  
  
      def pop(self) -> None:
          self.stack.pop()
          self.min_stack.pop()
  
  
      def top(self) -> int:
          return self.stack[-1]
  
  
      def getMin(self) -> int:
          return self.min_stack[-1]
  ```

  

- [面试题03.01.三合一](https://leetcode-cn.com/problems/three-in-one-lcci/)

  三合一。描述如何只用一个数组来实现三个栈。

  你应该实现push(stackNum, value)、pop(stackNum)、isEmpty(stackNum)、peek(stackNum)方法。stackNum表示栈下标，value表示压入的值。

  构造函数会传入一个stackSize参数，代表每个栈的大小。

  ```python
  class TripleInOne:
  
      def __init__(self, stackSize: int):
          # 即stack = [[], [], []]
          self.stack = [[] for _ in range(3)]
          self.size = stackSize
  
  
      def push(self, stackNum: int, value: int) -> None:
          if len(self.stack[stackNum]) < self.size:
              self.stack[stackNum].append(value)
  
  
      def pop(self, stackNum: int) -> int:
          return self.stack[stackNum].pop() if self.stack[stackNum] else -1
  
  
      def peek(self, stackNum: int) -> int:
          return self.stack[stackNum][-1] if self.stack[stackNum] else -1
  
  
      def isEmpty(self, stackNum: int) -> bool:
          return not self.stack[stackNum]
  ```

  

- [20.有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

  给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

  有效字符串需满足：

  左括号必须用相同类型的右括号闭合。
  左括号必须以正确的顺序闭合。

  ```python
  class Solution:
      def isValid(self, s: str) -> bool:
          # 若字符串为奇数，直接返回False
          if len(s) % 2 == 1: return False
          dic = {'(': ')', '{': '}', '[': ']'}
          stack = []
          for c in s:
              # 若当前字符为一种左括号，进栈
              if c in dic: stack.append(c)
              # 当栈为空却直接出现右括号时
              # or 当右括号不等于栈顶左括号时
              # 直接返回False
              elif not stack or dic[stack.pop()] != c: return False
          # 栈为空即括号均合法
          return not stack
  ```

  

- [面试题16.26.计算器](https://leetcode-cn.com/problems/calculator-lcci/)

  给定一个包含正整数、加(+)、减(-)、乘(*)、除(/)的算数表达式(括号除外)，计算其结果。

  表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

  ```python
  class Solution:
      def calculate(self, s: str) -> int:
          stack = []
          num, pre_sign = 0, '+'
          for i in range(len(s)):
              # 当出现连续数字时直接处理即可
              if s[i].isdigit(): num = num * 10 + ord(s[i]) - ord('0')
              # 当出现符号或遍历到最后一位时
              # 判断前一个符号
              # e.g. 若 2*3+4，遍历到+，则前一个符号是*，计算栈顶数2*当前数3即可
              if s[i] in '+-*/' or i == len(s) - 1:
                  if pre_sign == '+': 
                      stack.append(num)
                  elif pre_sign == '-': 
                      stack.append(-num)
                  elif pre_sign == '*': 
                      stack.append(stack.pop() * num)
                  else: 
                      stack.append(int(stack.pop() / num))
                  num, pre_sign = 0, s[i]
          return sum(stack)
  ```

  

- [772.基本计算器3](https://leetcode-cn.com/problems/basic-calculator-iii/)

  实现一个基本的计算器来计算简单的表达式字符串。

  表达式字符串只包含非负整数，算符 +、-、*、/ ，左括号 ( 和右括号 ) 。整数除法需要 向下截断 。

  你可以假定给定的表达式总是有效的。所有的中间结果的范围为 [-231, 231 - 1] 。

  ```python
  # 该题为逆波兰表达式
  class Solution:
      def calculate(self, s: str) -> int:
          def cal(num1, num2, opt):
              if opt == '+': return num2 + num1
              elif opt == '-': return num2 - num1
              elif opt == '*': return num2 * num1
              else: return int(num2 / num1)
  
          stack_num, stack_opt =[], []
          i = 0
          priority = {'(': 0, ')': 0, '+': 1, '-': 1, '*': 2, '/': 2}
          while i < len(s):
              if s[i] == ' ': 
                  i += 1
                  continue
              # 如果出现数字，则while循环遍历其之后的整串数字
              elif s[i].isdigit():
                  pre = i
                  while i + 1 < len(s) and s[i + 1].isdigit():
                      i += 1
                  stack_num.append(int(s[pre: i + 1]))
              elif s[i] == '(':
                  stack_opt.append(s[i])
              # 当出现右括号时，直到计算符栈出现左括号，while循环计算括号内剩余符号
              elif s[i] == ')':
                  while stack_opt[-1] != '(':
                      tmp = cal(stack_num.pop(), stack_num.pop(), stack_opt.pop())
                      stack_num.append(tmp)
                  stack_opt.pop()
              # 当计算符栈顶符号优先度大于当前符号时，计算栈顶符号
              else:
                  while stack_opt and priority[stack_opt[-1]] >= priority[s[i]]:
                      tmp = cal(stack_num.pop(), stack_num.pop(), stack_opt.pop())
                      stack_num.append(tmp)
                  stack_opt.append(s[i])
              i += 1
          # 遍历完成后，若最后还留下操作符，计算即可
          while stack_opt:
              tmp = cal(stack_num.pop(), stack_num.pop(), stack_opt.pop())
              stack_num.append(tmp) 
          return stack_num[-1]
  ```

  

- [1047.删除字符串中的所有相邻重复项](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/)

  给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

  在 S 上反复执行重复项删除操作，直到无法继续删除。

  在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

  ```python
  class Solution:
      def removeDuplicates(self, s: str) -> str:
          stack = []
          for c in s:
              # 若当前字符等于栈顶字符，即前一个字符，将栈顶字符删去
              # 该字符也不入栈即可
              if stack and stack[-1] == c: stack.pop()
              # 否则该字符入栈
              else: stack.append(c)
          return ''.join(stack)
  ```

  

- [剑指Offer31.栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

  输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

  ```python
  class Solution:
      def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
          # 初始i指针指向popped数组的首位
          stack, i = [], 0
          for num in pushed:
              stack.append(num)
              # 判断栈顶元素是否等于出栈数组的i指针位元素
              while stack and stack[-1] == popped[i]:
                  # 是则出栈，i指针继续往下一位遍历
                  stack.pop()
                  i += 1
          return not stack
  ```

  

- [739.每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

  请根据每日 `气温` 列表 `temperatures` ，请计算在每一天需要等几天才会有更高的温度。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

  ```python
  class Solution:
      def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
          res = [0] * len(temperatures)
          # 创建单调递减栈
          stack = []
          for i, t in enumerate(temperatures):
              # 若当前温度比栈顶温度高，则栈顶出栈，给答案数组赋值
              while stack and t > temperatures[stack[-1]]:
                  tmp_i = stack.pop()
                  # 当前位置 i 到栈顶位置 tmp_i 相差天数 i - tmp_i
                  res[tmp_i] = i - tmp_i
              stack.append(i)
          return res
  ```

  

- [42.接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

  给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

  ```python
  # 该题单调栈解法空间复杂度为O(n)，单调栈stack的空间复杂度即为数组height的长度n
  # 由于不是最优解，则最优解附下方
  # 由于该题属于栈和队列分类下，则保留单调栈解法 (单调栈：要求栈中的元素始终保持单调性)
  class Solution:
      def trap(self, height: List[int]) -> int:
          # 创建单调递减栈
          res, stack = 0, []
          for right, h in enumerate(height):
              # 若当前高度比栈顶高度高，则栈顶出栈，计算短板可以接的雨水量
              while stack and h > height[stack[-1]]:
                  # left(栈顶前一位高度) > mid(栈顶高度) < right(当前高度)
                  mid = stack.pop()
                  if not stack: break
                  left = stack[-1]
                  tmp_w = right - left - 1
                  tmp_h = min(height[left], height[right]) - height[mid]
                  res += tmp_w * tmp_h
              # 此时，right当前高度比栈顶高度低，保持单调递减栈的特性，
              # 结束遍历，当前高度index入栈
              stack.append(right)
          return res
  ```

  ```python
  # 双指针，空间复杂度为O(1)
  class Solution:
      def trap(self, height: List[int]) -> int:
          res = 0
          # 左右指针
          i, j = 0, len(height) - 1
          left_max = right_max = 0
          while i < j:
              # 判断当前左右指针是否能更新左右最大高度
              left_max = max(left_max, height[i])
              right_max = max(right_max, height[j])
              # 判断较低高度的指针，计算该指针位置能储水的量
              if height[i] < height[j]:
                  res += left_max - height[i]
                  i += 1
              else:
                  res += right_max - height[j]
                  j -= 1
          return res
  ```

  

- [84.柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

  给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

  求在该柱状图中，能够勾勒出来的矩形的最大面积。

  ```python
  class Solution:
      def largestRectangleArea(self, heights: List[int]) -> int:
          # 创建单调递增栈
          # 本题使用哨兵节点，优点是循环时不作非空判断：
          # 即从位置1开始遍历时不用考虑前一位，因为增加了哨兵头节点高度位0
          # 遍历到最后一位时不用考虑最后一位的计算缺失，因为增加了哨兵尾节点高度0
          res, stack = 0, [0]
          heights = [0] + heights + [0]
          for i in range(1, len(heights)):
              # 当前高度小于栈顶高度时
              # 计算栈顶高度所能构成的最大面积
              while heights[i] < heights[stack[-1]]:
                  tmp_i = stack.pop()
                  tmp_h = heights[tmp_i]
                  tmp_w = i - stack[-1] - 1
                  res = max(res, tmp_h * tmp_w)
              # 当前高度大于栈顶高度，保证单调递增栈的特性，当前高度index入栈
              stack.append(i)
          return res
  ```

  

- [面试题03.06.动物收容所](https://leetcode-cn.com/problems/animal-shelter-lcci/)

  动物收容所。有家动物收容所只收容狗与猫，且严格遵守“先进先出”的原则。在收养该收容所的动物时，收养人只能收养所有动物中“最老”（由其进入收容所的时间长短而定）的动物，或者可以挑选猫或狗（同时必须收养此类动物中“最老”的）。换言之，收养人不能自由挑选想收养的对象。请创建适用于这个系统的数据结构，实现各种操作方法，比如enqueue、dequeueAny、dequeueDog和dequeueCat。允许使用Java内置的LinkedList数据结构。

  enqueue方法有一个animal参数，animal[0]代表动物编号，animal[1]代表动物种类，其中 0 代表猫，1 代表狗。

  dequeue*方法返回一个列表[动物编号, 动物种类]，若没有可以收养的动物，则返回[-1,-1]。

  ```python
  class AnimalShelf:
  
      def __init__(self):
          # animals储存两个双端队列，分别为猫deque和狗deque
          self.animals = [deque(), deque()]
  
  
      def enqueue(self, animal: List[int]) -> None:
          self.animals[animal[1]].append(animal)
  
  
      def dequeueAny(self) -> List[int]:
          # 当猫狗deque皆存在，判断哪个deque的头元素动物编号小，则该元素出队列，并return
          if self.animals[0] and self.animals[1]:
              return self.animals[0].popleft() if self.animals[0][0][0] < self.animals[1][0][0] else self.animals[1].popleft()
          # 只存在猫deque
          elif self.animals[0]: return self.animals[0].popleft()
          # 只存在狗deque
          elif self.animals[1]: return self.animals[1].popleft()
          else: return [-1, -1]
  
  
      def dequeueDog(self) -> List[int]:
          return self.animals[1].popleft() if self.animals[1] else [-1, -1]
  
  
      def dequeueCat(self) -> List[int]:
          return self.animals[0].popleft() if self.animals[0] else [-1, -1]
  ```

  

- [剑指Offer59-2.队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

  请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

  若队列为空，pop_front 和 max_value 需要返回 -1

  ```python
  import queue
  
  class MaxQueue:
  
      def __init__(self):
          self.queue = queue.Queue()
          # 创建单调递减双端队列
          # 队首即是最大值
          self.deque = queue.deque()
  
  
      def max_value(self) -> int:
          return self.deque[0] if self.deque else -1
  
  
      def push_back(self, value: int) -> None:
          self.queue.put(value)
          # 当单调递减队列队尾 < 当前入队值时
          # 循环将小于的元素删除
          # 当前入队值入队
          while self.deque and self.deque[-1] < value:
              self.deque.pop()
          self.deque.append(value)
  
  
      def pop_front(self) -> int:
          if not self.deque: return -1
          val = self.queue.get()
          # 若单调递减队列队首 = 当前出队值时
          # 两队均出
          if self.deque[0] == val: self.deque.popleft()
          return val
  ```

  

- [剑指Offer59-1.滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

  给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。

  ```python
  class Solution:
      def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
          if not nums: return []
          # 创建单调递减双端队列
          # 队列首元素即是最大值
          deque = collections.deque()
          # 为形成首个窗口时
          for i in range(k):
              # 当当前值大于队列尾元素时
              # 为了保持递减性质，尾元素出队
              while deque and deque[-1] < nums[i]:
                  deque.pop()
              deque.append(nums[i])
          # 答案数组直接加入首个窗口最大值
          res = [deque[0]]
          # 形成窗口后
          for i in range(k, len(nums)):
              # 当最大值是已经不在窗口内的元素时，删去该值
              # i - k即窗口左侧第一个不在窗口内的元素
              if deque[0] == nums[i - k]: deque.popleft()
              # 当当前值大于队列尾元素时
              # 为了保持递减性质，尾元素出队
              while deque and deque[-1] < nums[i]:
                  deque.pop()
              deque.append(nums[i])
              res.append(deque[0])
          return res
  ```



------



###### 5. 递归

知识点：

> 经典递归函数：
>
> 斐波那契数列计算
>
> ```python
> def fib(self, n: int) -> int:
>     if n == 0: return 0
>     if n == 1: return 1
>     return fib(n - 1) + fib(n - 2)
> ```
>
> 该方法也是经典的时间复杂度为 `O(n^2)` 的例子
>
> p.s. 注意：在使用python时，递归方法基本超时也不效率，除非特殊情况实际不用递归
>
> p.s. 因此，递归问题通常用动态规划解决

例题：

- [剑指Offer10-1.斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

  写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：

  F(0) = 0,   F(1) = 1
  F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
  斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

  答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

  ```python
  # 动态规划
  class Solution:
      def fib(self, n: int) -> int:
          # 斐波那契数列最开始情况：F(2) = F(1) + F(0)
          # a 为 F(0)，b 为 F(1)
          a, b = 0, 1
          # 循环计算n次:
          # a    b
          # F(0) F(1) n = 0
          # F(1) F(2) n = 1 <- 开始进入循环
          # F(2) F(3) n = 2
          for _ in range(n):
              # 每次计算后的b实际上是下一项的值
              a, b = b, a + b
          return a % 1000000007
  ```

  

- [剑指Offer10-2.青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

  一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

  答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

  ```python
  # 动态规划
  class Solution:
      def numWays(self, n: int) -> int:
          # 跳台阶最开始情况：F(2) = F(1) + F(0)
          # a 为 F(0)，b 为 F(1)
          a, b = 1, 1
          # 循环计算n次
          # a    b
          # F(0) F(1) n = 0
          # F(1) F(2) n = 1 <- 开始进入循环
          # F(2) F(3) n = 2
          for _ in range(n):
              # 每次计算后的b实际上是下一项的值
              a, b = b, a + b
          return a % 1000000007
  ```

  

- [剑指Offer63.股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

  假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

  ```python
  # 动态规划
  # 此题为剑指Offer10-1和剑指Offer10-2的进阶题型
  class Solution:
      def maxProfit(self, prices: List[int]) -> int:
          # 需要找到max_profit = price - min_cost即可
          cost, profit = float('+inf'), 0
          for price in prices:
              cost = min(cost, price)
              profit = max(profit, price - cost)
          return profit
  ```

  

- [面试题08.01.三步问题](https://leetcode-cn.com/problems/three-steps-problem-lcci/)

  三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007。

  ```python
  # 动态规划
  class Solution:
      def waysToStep(self, n: int) -> int:
          # 三步问题最开始情况：F(4) = F(3) + F(2) + F(1)
          # a 为 F(1)，b 为 F(2), c 为 F(3)
          if n < 3: return n
          if n == 3: return 4
          a, b, c = 1, 2, 4
          # 循环计算n-3次
          # a    b    c
          # F(1) F(2) F(3) n = 1,2,3
          # F(2) F(3) F(4) n = 4 <- 开始进入循环
          # F(3) F(4) F(5) n = 5
          for _ in range(n - 3):
              # c即是我们需要的结果
              # 注意此题需要在计算时取模，不会超时
              a, b, c = b, c, (a + b + c) % 1000000007
          return c
  ```

  

- [剑指Offer06.从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

  输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

  ```python
  class Solution:
      def reversePrint(self, head: ListNode) -> List[int]:
          # 顺序将值加入数组，最后打印数据倒序即可
          res = []
          while head:
              res.append(head.val)
              head = head.next
          return res[::-1]
  ```

  

- [剑指Offer24.反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/submissions/)

  定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

  ```python
  class Solution:
      def reverseList(self, head: ListNode) -> ListNode:
          # None (pre)
          # head (cur)
          pre, cur = None, head
          # 循环依次将cur指针指向pre指针处即可
          # 当cur为空时，此时pre即指向反转后的头节点
          while cur:
              tmp = cur.next
              cur.next = pre
              pre = cur
              cur = tmp
          return pre
  ```

  

- [剑指Offer25.合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

  输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

  示例1：

  输入：1->2->4, 1->3->4
  输出：1->1->2->3->4->4

  ```python
  class Solution:
      def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
          # dum (cur)
          # 循环依次将cur指针指向l1 or l2中较小的节点即可
          dum = cur = ListNode(0)
          while l1 and l2:
              if l1.val < l2.val:
                  cur.next, l1 = l1, l1.next
              else:
                  cur.next, l2 = l2, l2.next
              cur = cur.next
          # 需要注意，此时的情况必为：l1和l2一个为空一个剩余一个节点
          cur.next = l1 if l1 else l2
          return dum.next
  ```

  

- [剑指Offer16.数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

  实现 pow(*x*, *n*) ，即计算 x 的 n 次幂函数（即，x^n）。不得使用库函数，同时不需要考虑大数问题

  ```python
  # 即快速幂
  class Solution:
      def myPow(self, x: float, n: int) -> float:
          # 处理0的任何次方
          if x == 0: return 0
          # 处理负数次方
          if n < 0: x, n = 1/x, -n
          res = 1
          while n:
              # 当次方数n为奇数时，res乘上一个x
              # 例：计算3^9
              # n//2 			x=x^2 								res=1
              # 9//2=4    3x3     							1x3
              # 4//2=2 		(3x3)x(3x3)						1x3
              # 2//2=1 		(3x3x3x3)x(3x3x3x3) 	1x3
              # 1//2=0 		... 									1x3x((3x3x3x3)x(3x3x3x3))
              # 即 3 x [(3x3) x (3x3)] x [(3x3) x (3x3)] 的循环过程
              # 当有奇数出现时res单独乘上一个x
              # 当最后一个奇数即n=1时，当前的 res*x 即是所求
              if n % 2 == 1: res *= x
              x *= x
              n //= 2
          return res
  ```

  

- [面试题08.05.递归乘法](https://leetcode-cn.com/problems/recursive-mulitply-lcci/)

  递归乘法。 写一个递归函数，不使用 * 运算符， 实现两个正整数的相乘。可以使用加号、减号、位移，但要吝啬一些。

  ```python
  class Solution:
      def multiply(self, A: int, B: int) -> int:
          if A == 0 or B == 0: return 0
          # A >> 1 即 A // 2
          res = self.multiply(A >> 1, B)
          # 当A为奇数时，A * B = A//2 * B + A//2 * B + B
          # 在递归的最深层，必有A//2 = 1，此时即满足不需要使用乘号也可以计算乘法
          if A % 2 == 1: return res + res + B
          # 当A为偶数时，A * B = A//2 * B + A//2 * B
          else: return res + res
  ```



------



###### 6. 排序

知识点：

> 性质：
>
> > 原地性：能不能在存储原始数据的数组上，通过倒腾实现排序；即非原地为需要申请新的存储空间来存储排序的数据
> >
> > p.s. 原地排序的空间复杂度不一定是O(1)，但是空间复杂度为O(1)的排序一定是原地排序
> >
> > 稳定性：如果待排序的数据中存在值相等的元素，相等元素经过排序后顺序一样，即为稳定排序
> >
> > e.g. 数组 = [4, 2_1, 9, 2_2] -> 稳定排序：排序后数组 = [2_1, 2_2, 4, 9]
>
> [常见排序](https://www.runoob.com/w3cnote/ten-sorting-algorithm.html)：
>
> (1) [冒泡排序](https://www.runoob.com/python3/python-bubble-sort.html)
>
> 整个冒泡排序过程包含多趟冒泡操作。每一趟冒泡操作都会遍历整个数组，依次对数组中相邻的元素进行比较，看是否满足大小关系要求，如果不满足，就将他们互换位置。一趟冒泡操作会让至少一个元素移动到它应该在的位置，重复n次，就完成了n个数据的排序工作。
>
> p.s. 原地 & 稳定
>
> <img src="https://www.runoob.com/wp-content/uploads/2019/03/bubbleSort.gif" style="zoom:50%;" />
>
> (2) [插入排序](https://www.runoob.com/python3/python-insertion-sort.html)
>
> 将数组中的数据分为两个区间：已排序区间和未排序区间。初始已排序区间只有一个元素，就是数组中的第一个元素。插入算法的核心思想是取未排序区间中的元素，在已排序区间中找合适的插入位置将其插入，保证已排序区间数据一直有序。重复这个过程，直到未排序区间中元素为空，算法结束。
>
> p.s. 原地 & 稳定
>
> <img src="https://www.runoob.com/wp-content/uploads/2019/03/insertionSort.gif" style="zoom:50%;" />
>
> (3) [选择排序](https://www.runoob.com/python3/python-selection-sort.html)
>
> 选择排序算法的实现思路有点类似插入排序，也将整个数组划分为已排序区间和未排序区间。但不同点在于：选择排序算法每次从未排序区间中，找到最小的元素，将其放到已排序区间的末尾。
>
> p.s. 原地 & 不稳定
>
> <img src="https://www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif" style="zoom:50%;" />
>
> (4) [归并排序](https://www.runoob.com/python3/python-merge-sort.html)
>
> 归并排序利用了分治的思想，采用递归来实现。如果要排序一个数组，先把数组从中间分成前后两部分，然后对前后两部分分别排序，再将排好序的两部分合并在一起，这样整个数组就都有序了。
>
> p.s. 非原地 & 稳定
>
> <img src="https://www.runoob.com/wp-content/uploads/2019/03/mergeSort.gif" style="zoom:50%;" />
>
> (5) [快速排序](https://www.runoob.com/python3/python-quicksort.html)
>
> 快速排序也利用了分治的思想，采用递归来实现。首先挑选基准值，重新排序数列，比基准值小的元素放在基准前面，比基准值大的元素放在基准后面。在这步分割结束之后，对该基准值的排序就已经完成。递归将小于基准值元素的子序列和大于基准值元素的子序列排序。
>
> p.s. 原地 & 不稳定
>
> <img src="https://www.runoob.com/wp-content/uploads/2019/03/quickSort.gif" style="zoom:50%;" />

例题：

- [面试题10.01.合并排序的数组](https://leetcode-cn.com/problems/sorted-merge-lcci/)

  给定两个排序后的数组 A 和 B，其中 A 的末端有足够的缓冲空间容纳 B。 编写一个方法，将 B 合并入 A 并排序。

  初始化 A 和 B 的元素数量分别为 m 和 n。

  ```python
  class Solution:
      def merge(self, A: List[int], m: int, B: List[int], n: int) -> None:
          """
          Do not return anything, modify A in-place instead.
          """
          # 双指针从两个数组有数字的最后一位从后往前遍历
          # tail即尾指针，指向A数组末端缓冲空间最后一位，也是从后往前赋值遍历
          # 由于缓冲空间的存在，从后往前遍历不会覆盖掉未遍历的数字
          i, j = m - 1, n - 1
          tail = m + n - 1
          while i >= 0 or j >= 0:
              # 数组A数字部分已经遍历完，直接依次赋值数组B剩余数字
              if i == -1:
                  A[tail] = B[j]
                  j -= 1
              # 数组B数字部分已经遍历完，直接依次赋值数组A剩余数字
              elif j == -1:
                  A[tail] = A[i]
                  i -= 1
              # A指针处数字大
              elif A[i] > B[j]:
                  A[tail] = A[i]
                  i -= 1
              # B指针处数字大
              else:
                  A[tail] = B[j]
                  j -= 1
              tail -= 1
  ```

  

- [242.有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

  给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

  注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

  ```python
  # Note: 使用哈希表方法而不去排序
  # 时间复杂度优化为O(n)
  # 空间复杂度仅为O(26)即常数级
  class Solution:
      def isAnagram(self, s: str, t: str) -> bool:
          if len(s) != len(t): return False
          # 使用数组型哈希表
          tmp = [0] * 26
          for char in s:
              tmp[ord(char) - ord('a')] += 1
          for char in t:
              tmp[ord(char) - ord('a')] -= 1
          for i in tmp:
              if i != 0: return False
          return True
  ```

  

- [252.会议室](https://leetcode-cn.com/problems/meeting-rooms/)

  给定一个会议时间安排的数组 intervals ，每个会议时间都会包括开始和结束的时间 intervals[i] = [starti, endi] ，请你判断一个人是否能够参加这里面的全部会议。

  ```python
  class Solution:
      def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
          # 以二维数组的第一维维key，排序二维数组
          intervals.sort()
          # 先开始的会议如果结束时间 > 下个会议的开始时间
          # 则不能参加，直接返回False
          for i in range(len(intervals) - 1):
              if intervals[i][1] > intervals[i + 1][0]: return False
          return True
  ```

  

- [56.合并区间](https://leetcode-cn.com/problems/merge-intervals/)

  以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。

  ```python
  class Solution:
      def merge(self, intervals: List[List[int]]) -> List[List[int]]:
          # 以二维数组的第一维维key，排序二维数组
          intervals.sort()
          res = []
          for interval in intervals:
              # 如果res[-1]区间的end < 下一区间的start
              # 则当前区间与下一区间无重叠，直接加入res
              if not res or res[-1][1] < interval[0]:
                  res.append(interval)
              # 否则，将res[-1]区间的end更新，更新值为其与下一区间end的较大值
              else:
                  res[-1][1] = max(res[-1][1], interval[1])
          return res
  ```

  

- [剑指Offer21.调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

  输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

  ```python
  class Solution:
      def exchange(self, nums: List[int]) -> List[int]:
          # 双指针，从该数组前后依次将奇数换到前指针处，将偶数换到后指针处即可
          i, j = 0, len(nums) - 1
          while i < j:
              while i < j and nums[i] % 2 == 1: i += 1
              while i < j and nums[j] % 2 == 0: j -= 1
              nums[i], nums[j] = nums[j], nums[i]
          return nums
  ```

  

- [75.颜色分类](https://leetcode-cn.com/problems/sort-colors/)

  给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

  此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

  **进阶：**

  你可以不使用代码库中的排序函数来解决这道题吗？

  你能想出一个仅使用常数空间的一趟扫描算法吗？

  ```python
  class Solution:
      def sortColors(self, nums: List[int]) -> None:
          """
          Do not return anything, modify nums in-place instead.
          """
          # 双指针从前后依次扫描
          # 红0: 位于[0, i)
          # 白1: 位于[i, cur)
          # 而区间[cur, j)则是未扫描的位置
          # 蓝2: 位于[j, len(nums)]
          i, j = 0, len(nums) - 1
          cur = 0
          while cur <= j:
              while cur <= j and nums[cur] == 2:
                  nums[cur], nums[j] = nums[j], nums[cur]
                  j -= 1
              if nums[cur] == 0:
                  nums[cur], nums[i] = nums[i], nums[cur]
                  i += 1
              cur += 1
          return nums
  ```

  

- [147.对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

  对链表进行插入排序。

  <img src="https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif" style="zoom: 67%;" />

  插入排序算法：

  插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
  每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
  重复直到所有输入数据插入完为止。

  ```python
  class Solution:
      def insertionSortList(self, head: ListNode) -> ListNode:
          if not head: return None
          dum = ListNode(0, head)
          # last为已排序部分的最后一个节点
          last, cur = head, head.next
          while cur:
              # 如果下一个节点 >= 已排序部分的最后一个节点，直接移动last指针即可
              if last.val <= cur.val: last = last.next
              # 否则，进入排序步骤
              else:
                  # 从dum指针处循环遍历已排序部分，当搜索到该节点大于当前需要排序的值时
                  # 将需要排序的cur节点插入到该节点之前，即pre.next
                  pre = dum
                  while pre.next.val <= cur.val:
                      pre = pre.next
                  last.next = cur.next
                  cur.next = pre.next
                  pre.next = cur
              cur = last.next
          return dum.next
  ```
  
  
  
- [148.排序链表](https://leetcode-cn.com/problems/sort-list/)

  给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

  进阶：

  你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

  ```python
  # 方法1:
  # 常规归并排序，使用了递归
  # 时间复杂度为O(nlogn)，空间复杂度为O(n)
  class Solution:
      def sortList(self, head: ListNode) -> ListNode:
          if not head or not head.next: return head
  
          # 不对对中点处进行分割
          slow, fast = head, head.next
          while fast and fast.next:
              slow = slow.next
              fast = fast.next.next
          mid, slow.next = slow.next, None
          left, right = self.sortList(head), self.sortList(mid)
  				
          # 无法再分割之后，开始排序整合
          dum = cur = ListNode(0)
          while left and right:
              if left.val < right.val:
                  cur.next = left
                  left = left.next
              else:
                  cur.next = right
                  right = right.next
              cur = cur.next
          cur.next = left if left else right
          return dum.next
  ```
  
  ```python
  # 方法2:（进阶）
  # 由于要求时间复杂度要求O(nlogn)，即使用归并排序
  # 而空间复杂度要求O(1)，则不能使用常规归并O(n)
  # 因此本题应使用迭代归并而非递归归并
  class Solution:
      def sortList(self, head: ListNode) -> ListNode:
          # 查找链表长度
          len_total = 0
          cur = head
          while cur:
              cur = cur.next
              len_total += 1
          
          # 不断循环double每组个数intv
          dum = ListNode(0, head)
          intv = 1
          while intv < len_total:
              pre, cur = dum, dum.next
              while cur:
                  # 处理第一组头节点h1
                  h1, i = cur, intv
                  while i and cur:
                      cur = cur.next
                      i -= 1
                  if i: break
  								
                  # 处理第二组头节点h2
                  h2, i = cur, intv
                  while i and cur:
                      cur = cur.next
                      i -= 1
                  len1, len2 = intv, intv - i
  								
                  # 当h1和h2两个链表长度均大于0时
                  # 循环判断数值并添加到pre指针后
                  while len1 and len2:
                      if h1.val < h2.val:
                          pre.next = h1
                          h1 = h1.next
                          len1 -= 1
                      else:
                          pre.next = h2
                          h2 = h2.next
                          len2 -=1
                      pre = pre.next
                      
                  # 当其中一个链表长度为0时，将pre指针接到另一个链表剩余节点处
                  # 并且循环遍历仍不等于0的链表长度，直接将该已排序的部分接到pre指针处
                  pre.next = h1 if len1 else h2
                  while len1 > 0 or len2 > 0:
                      pre = pre.next
                      len1 -= 1
                      len2 -= 1
                      
                  # 循环完成，将pre指针接到cur指针处
                  pre.next = cur
              intv *= 2
          return dum.next
  ```
  
  
  
- [215.数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

  给定整数数组 `nums` 和整数 `k`，请返回数组中第 `k` 个最大的元素。

  请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

  ```python
  # 方法1：
  # 正常快速排序方法，时间复杂度O(n)，空间复杂度O(logn)
  # (i/j双指针均从左指针处出发)
  # 此题引入随机数是为了增加代码通过效率，与时间复杂度并无关系
  import random
  
  class Solution:
      def findKthLargest(self, nums: List[int], k: int) -> int:
          def partition(l, r):
              # notes: 该步骤if只是为了增加代码通过效率，与时间复杂度并无关系
              if l < r:
                  tmp = random.randint(l, r)
                  nums[l], nums[tmp] = nums[tmp], nums[l]
                  
              pivot = nums[l]
              i = l
              for j in range(l + 1, r + 1):
                  if nums[j] < pivot:
                      i += 1
                      nums[i], nums[j] = nums[j], nums[i]
              nums[l], nums[i] = nums[i], nums[l]
              return i
          
          l, r = 0, len(nums) - 1
          while True:
              # 不断进行快速排序
              # index前的元素均小于index位，index后的元素均大于index位
              # 当index为len(nums) - k即第k大的位置时，返回即可
              # 并没有将前后子区间排序完
              index = partition(l, r)
              if index < len(nums) - k: l = index + 1
              elif index > len(nums) - k: r = index - 1
              else: return nums[index]
  ```

  ```python
  # 方法2：
  # 简化代码快速排序方法，时间复杂度O(n)，空间复杂度O(logn)
  # (i指针从左指针处出发，j指针从右指针处出发，且该方法直接将左指针当作pivot)
  # 此题引入随机数是为了增加代码通过效率，与时间复杂度并无关系
  import random
  
  class Solution:
      def findKthLargest(self, nums: List[int], k: int) -> int:
          def quick_sort(l, r):
              # notes: 该步骤if只是为了增加代码通过效率，与时间复杂度并无关系
              if l < r:
                  tmp = random.randint(l, r)
                  nums[l], nums[tmp] = nums[tmp], nums[l]
                  
              i, j = l, r
              while i < j:
                  while i < j and nums[j] >= nums[l]: j -= 1
                  while i < j and nums[i] <= nums[l]: i += 1
                  nums[i], nums[j] = nums[j], nums[i]
              nums[l], nums[i] = nums[i], nums[l]
              if i < len(nums) - k: return quick_sort(i + 1, r)
              if i > len(nums) - k: return quick_sort(l, i - 1)
              return nums[i]
          
          return quick_sort(0, len(nums) - 1)
  ```

  

- [面试题17.14.最小K个数](https://leetcode-cn.com/problems/smallest-k-lcci/)

  设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

  ```python
  # 方法1：
  # 正常快速排序方法，时间复杂度O(n)，空间复杂度O(logn)
  # (i/j双指针均从左指针处出发)
  class Solution:
      def smallestK(self, arr: List[int], k: int) -> List[int]:
          if not arr or k >= len(arr): return arr
          def partition(l, r):
              pivot = arr[l]
              i = l
              for j in range(l + 1, r + 1):
                  if arr[j] < pivot:
                      i += 1
                      arr[i], arr[j] = arr[j], arr[i]
              arr[l], arr[i] = arr[i], arr[l]
              return i
          
          l, r = 0, len(arr) - 1
          while True:
              # 不断进行快速排序
              # index前的元素均小于index位，index后的元素均大于index位
              # 当index为k即第k小的位置时，[0:k)即最小k个数
              # 并没有将前后子区间排序完
              index = partition(l, r)
              if index < k: l = index + 1
              elif index > k: r = index - 1
              else: return arr[:k]
  ```

  ```python
  # 方法2：
  # 简化代码快速排序方法，时间复杂度O(n)，空间复杂度O(logn)
  # (i指针从左指针处出发，j指针从右指针处出发，且该方法直接将左指针当作pivot)
  class Solution:
      def smallestK(self, arr: List[int], k: int) -> List[int]:
          if not arr or k >= len(arr): return arr
          def quick_sort(l, r):
              i, j = l, r
              while i < j:
                  while i < j and arr[j] >= arr[l]: j -= 1
                  while i < j and arr[i] <= arr[l]: i += 1
                  arr[i], arr[j] = arr[j], arr[i]
              arr[l], arr[i] = arr[i], arr[l]
              if i < k: return quick_sort(i + 1, r)
              if i > k: return quick_sort(l, i - 1)
              return arr[:k]
          
          return quick_sort(0, len(arr) - 1)
  ```

  

- [剑指Offer51.数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

  在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

  ```python
  # 即归并排序，在合并判断大小的时候判断逆序对个数
  class Solution:
      def reversePairs(self, nums: List[int]) -> int:
          def merge_sort(l, r):
              if l >= r: return 0
  
              # 不断从中心点分割
              mid = (l + r) // 2
              res = merge_sort(l, mid) + merge_sort(mid + 1, r)
  
              # 无法分割后，开始排序合并
              i, j = l, mid + 1
              tmp[l : r + 1] = nums[l : r + 1]
              for k in range(l, r + 1):
                	# 左指针数组排序完成，剩余右指针数组
                  if i == mid + 1:
                      nums[k] = tmp[j]
                      j += 1
                  # 右指针数组排序完成，剩余左指针数组
                  # or 左指针数 < 右指针数
                  elif j == r + 1 or tmp[i] <= tmp[j]:
                      nums[k] = tmp[i]
                      i += 1
                  # 左指针数 > 右指针数
                  else:
                      nums[k] = tmp[j]
                      j += 1
                      res += mid - i + 1
              return res
          
          tmp = [0] * len(nums)
          return merge_sort(0, len(nums) - 1)
  ```
  
  

- [768.最多能完成排序的块2](https://leetcode-cn.com/problems/max-chunks-to-make-sorted-ii/)

  这个问题和“最多能完成排序的块”相似，但给定数组中的元素可以重复，输入数组最大长度为2000，其中的元素最大为10**8。

  arr是一个可能包含重复元素的整数数组，我们将这个数组分割成几个“块”，并将这些块分别进行排序。之后再连接起来，使得连接的结果和按升序排序后的原数组相同。

  我们最多能将数组分成多少块？

  ```python
  class Solution:
      def maxChunksToSorted(self, arr: List[int]) -> int:
          # 创建一个单调递增栈
          res = []
          for num in arr:
              # 如果当前数比单调递增栈最后一位小
              # 则为了维持单调递增的特性
              # 将最大的数提取出，即tmp
              if res and num < res[-1]:
                  tmp = res.pop()
                  # 不断删去栈内元素，直到栈内最后一位 < 当前数
                  while res and num < res[-1]:
                      res.pop()
                  res.append(tmp)
              else:
                  res.append(num)
          # 此时res数组的长度即是分区的块数
          # res数组内的元素即是每个分区的最大数
          return len(res)
  ```



------



###### 7. 二分查找

知识点：

> 二分查找代码实现：
>
> ```python
> # 经典实现1：非递归
> # 空间复杂度O(1)，时间复杂度O(logn)
> # i / j双指针，注意循环条件是i <= j，而不是i < j
> def binary_search(nums: List[int], value: int):
>  		i, j = 0, len(nums) - 1
>  		while i <= j:
>     				mid = (i + j) // 2
>      			if nums[mid] == value:
>          			return mid
>      			elif nums[mid] < value:
>          			i = mid + 1
>      			else:
>          			j = mid - 1
>  		return -1
> ```
>
> ```python
> # 经典实现2: 递归
> # 空间复杂度O(logn)，时间复杂度O(logn)
> def binary_search(nums: List[int], value: int, i: int, j: int):
>  		if not i <= j: return -1
>  		mid = (i + j) // 2
>  		if nums[mid] == value:
>      			return mid
>  		elif nums[mid] < value:
>      			return binary_search(nums, value, mid + 1, j)
>  		else:
>      			return binary_search(nums, value, i, mid - 1)
> 
> binary_search(nums, value, 0, len(nums) - 1)
> ```
>
> 双指针小技巧：
>
> 基础取mid位置，mid = (low + high) // 2
>
> 防止加法int越界，mid = low + (high - low) // 2
>
> ```python
> # 变题1.1：查找第一个值等于给定值的元素
> # e.g. 123334 x=3, 12(3)334
> def binary_search(nums: List[int], value: int):
>    		i, j = 0, len(nums) - 1
>    		while i <= j:
>        		mid = (i + j) // 2
>        		if nums[mid] == value:
>            		if mid == 0 or nums[mid - 1] != value: return mid
>            		else: j = mid - 1
>        		elif nums[mid] < value:
>            		i = mid + 1
>        		else:
>            		j = mid - 1
>    		return -1
> ```
>
> ```python
> # 变题1.2：查找第一个大于等于x
> # e.g. 12567 x=4, 12(5)67
> def binary_search(nums: List[int], value: int):
>        i, j = 0, len(nums) - 1
>     		while i <= j:
>        		mid = (i + j) // 2
>         		if nums[mid] >= value:
>            		if mid == 0 or nums[mid - 1] < value: return mid
>            		else: j = mid - 1
>         		else:
>            		i = mid + 1
>     		return -1
> ```
>
> ```python
> # 变题1.3：查找最后一个小于等于x的数
> # e.g. 12567 x=3, 1(2)567
> def binary_search(nums: List[int], value: int):
>    		i, j = 0, len(nums) - 1
>     		while i <= j:
>        		mid = (i + j) // 2
>         		if nums[mid] <= value:
>            		if mid == len(nums) - 1 or nums[mid + 1] > value: return mid
>             		else: i = mid + 1
>         		else:
>             		j = mid - 1
>     		return -1
> ```
>
> ```python
> # 变题2.1：循环有序数组中查找元素x（无重复数据）
> # e.g. 789123
> # 不命中时，
> # 若mid在9，nums[i] < 9，即左侧有序
> # 若mid在2，nums[i] > 2，即右侧有序
> def binary_search(nums: List[int], value: int):
>    		i, j = 0, len(nums) - 1
>     		while i <= j:
>        		mid = (i + j) // 2
>         		if nums[mid] == value:
>            		return mid
>         		elif nums[i] <= nums[mid]:
>            		if nums[i] <= value and value < nums[mid]: j = mid - 1
>             		else: i = mid + 1
>         		else:
>             		if nums[mid] < value and value <= nums[j]: i = mid + 1
>             		else: j = mid - 1
>     		return -1
> ```
>
> ```python
> # 变题2.2：循环有序数组中查找最小元素（无重复数据）
> # e.g. 678123
> # 不命中时，
> # 若mid在7，7 > nums[j]，即右侧循环
> # 若mid在2, 2 < nums[j]，即左侧循环
> def binary_search(nums: List[int], value: int):
>    		i, j = 0, len(nums) - 1
>     		while i <= j:
>        		mid = (i + j) // 2
>         		if (mid != 0 and nums[mid] < nums[mid - 1]) or (mid == 0 and nums[mid] < nums[j]):
>            		return mid
>         		elif nums[mid] > nums[j]:
>            		i = mid + 1
>         		else:
>             		j = mid - 1
>     		return -1
> ```
>
> 变题总结，即二分查找标准模版：
>
> - 双指针总是从头尾开始
> - while总是i <= j
>- 总是返回mid
> - 左指针总是mid + 1，右指针总是mid - 1
> - 先处理命中区间，再处理不命中区间
>- 对于非确定性查找，使用前后探测法
> 
> Note: 以下例题不一定使用标准模版

例题：

- [704.二分查找](https://leetcode-cn.com/problems/binary-search/)

  给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

  ```python
  class Solution:
      def search(self, nums: List[int], target: int) -> int:
          i, j = 0, len(nums) - 1
          while i <= j:
              mid = i + (j - i) // 2
              if nums[mid] == target: 
                  return mid
              elif nums[mid] < target: 
                  i = mid + 1
              else: 
                  j = mid - 1
          return -1 
  ```

  

- [374.猜数字大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

  猜数字游戏的规则如下：

  每轮游戏，我都会从 1 到 n 随机选择一个数字。 请你猜选出的是哪个数字。
  如果你猜错了，我会告诉你，你猜测的数字比我选出的数字是大了还是小了。
  你可以通过调用一个预先定义好的接口 int guess(int num) 来获取猜测结果，返回值一共有 3 种可能的情况（-1，1 或 0）：

  -1：我选出的数字比你猜的数字小 pick < num
  1：我选出的数字比你猜的数字大 pick > num
  0：我选出的数字和你猜的数字一样。恭喜！你猜对了！pick == num
  返回我选出的数字。

  ```python
  # The guess API is already defined for you.
  # @param num, your guess
  # @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
  # def guess(num: int) -> int:
  class Solution:
      def guessNumber(self, n: int) -> int:
          i, j = 1, n
          while i <= j:
              mid = i + (j - i) // 2
              if guess(mid) == 0: 
                  return mid
              # guess < num
              elif guess(mid) == 1:
                  i = mid + 1
              # guess > num
              else:
                  j = mid - 1
  ```

  

- [744.寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

  给你一个排序后的字符列表 letters ，列表中只包含小写英文字母。另给出一个目标字母 target，请你寻找在这一有序列表里比目标字母大的最小字母。

  在比较时，字母是依序循环出现的。举个例子：

  如果目标字母 target = 'z' 并且字符列表为 letters = ['a', 'b']，则答案返回 'a'

  ```python
  class Solution:
      def nextGreatestLetter(self, letters: List[str], target: str) -> str:
          i, j = 0, len(letters) - 1
          while i <= j:
              mid = i + (j - i) // 2
              if letters[mid] > target:
                  if mid == 0 or letters[mid - 1] <= target: return letters[mid]
                  # in this case, target < mid-1的值 < mid的值
                  else: j = mid - 1
              else:
                  i = mid + 1
          return letters[0]
  ```

  

- [35.搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

  给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

  请必须使用时间复杂度为 O(log n) 的算法。

  ```python
  class Solution:
      def searchInsert(self, nums: List[int], target: int) -> int:
          if nums[-1] < target: return len(nums)
          i, j = 0, len(nums) - 1
          while i <= j:
              mid = i + (j - i) // 2
              if nums[mid] >= target:
                  if mid == 0 or nums[mid] == target or nums[mid - 1] < target: return mid
                  # in this case,
                  # 1. mid已经在数组第一位
                  # 2. mid的值与target相等
                  # 3. mid-1的值 < targert < mid的值
                  else: j = mid - 1
              else:
                  i = mid + 1
  ```

  

- [34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

  给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

  如果数组中不存在目标值 target，返回 [-1, -1]。

  进阶：

  你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？

  ```python
  class Solution:
      def searchRange(self, nums: List[int], target: int) -> List[int]:
          left, right = -1, -1
          
          # 找到目标值的左端点
          i, j = 0, len(nums) - 1
          while i <= j:
              mid = i + (j - i) // 2
              if nums[mid] == target:
                  if mid == 0 or nums[mid - 1] < target: 
                      left = mid
                      break
                  else: j = mid - 1
              elif nums[mid] < target: i = mid + 1
              else: j = mid - 1
          
          if left == -1: return [-1, -1]
          # 找到目标值的右端点
          i, j = 0, len(nums) - 1
          while i <= j:
              mid = i + (j - i) // 2
              if nums[mid] == target:
                  if mid == len(nums) - 1 or nums[mid + 1] > target: 
                      right = mid
                      break
                  else: i = mid + 1
              elif nums[mid] < target: i = mid + 1
              else: j = mid - 1
          return [left, right]
  ```

  

- [面试题10.05.稀疏数组搜索](https://leetcode-cn.com/problems/sparse-array-search-lcci/)

  稀疏数组搜索。有个排好序的字符串数组，其中散布着一些空字符串，编写一种方法，找出给定字符串的位置。

  ```python
  class Solution:
      def findString(self, words: List[str], s: str) -> int:
          i, j = 0, len(words) - 1
          while i <= j:
              mid = i + (j - i) // 2
              # 特殊处理空字符串即可：
              # 当出现空字符串，若空字符串不为目标，则指针直接左移
              if words[mid] == '':
                  if words[j] == s: return j
                  j -= 1
              elif words[mid] == s:
                  return mid
              elif words[mid] < s:
                  i = mid + 1
              else:
                  j = mid - 1
          return -1
  ```

  

- [33.搜索选择排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

  整数数组 nums 按升序排列，数组中的值 互不相同 。

  在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

  给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

  ```python
  class Solution:
      def search(self, nums: List[int], target: int) -> int:
          i, j = 0, len(nums) - 1
          while i <= j:
              mid = i + (j - i) // 2
              if nums[mid] == target:
                  return mid
              # 左侧有序，即i到mid有序
              elif nums[i] <= nums[mid]:
                  if nums[i] <= target and target < nums[mid]: j = mid - 1
                  else: i = mid + 1
              # 右侧有序，即mid到j有序
              else:
                  if nums[mid] < target and target <= nums[j]: i = mid + 1
                  else: j = mid - 1
          return -1
  ```

  

- [81.搜索选择排序数组2](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

  已知存在一个按非降序排列的整数数组 nums ，数组中的值不必互不相同。

  在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转 ，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,4,4,5,6,6,7] 在下标 5 处经旋转后可能变为 [4,5,6,6,7,0,1,2,4,4] 。

  给你 旋转后 的数组 nums 和一个整数 target ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 nums 中存在这个目标值 target ，则返回 true ，否则返回 false 。

  ```python
  class Solution:
      def search(self, nums: List[int], target: int) -> bool:
          if not nums: return False
          if len(nums) == 1: return nums[0] == target
  
          i, j = 0, len(nums) - 1
          while i <= j:
              mid = i + (j - i) // 2
              if nums[mid] == target:
                  return True
              # 当三个数值相等时，无法判断左侧还是右侧有序
              # 此时将左右指针均向内移动1即可
              elif nums[i] == nums[mid] and nums[mid] == nums[j]:
                  i += 1
                  j -= 1
              elif nums[i] <= nums[mid]:
                  if nums[i] <= target and target < nums[mid]: j = mid - 1
                  else: i = mid + 1
              else:
                  if nums[mid] < target and target <= nums[j]: i = mid + 1
                  else: j = mid - 1
          return False
  ```

  

- [153.寻找选择排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

  已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,2,4,5,6,7] 在变化后可能得到：
  若旋转 4 次，则可以得到 [4,5,6,7,0,1,2]
  若旋转 7 次，则可以得到 [0,1,2,4,5,6,7]
  注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

  给你一个元素值 互不相同 的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

  ```python
  # Note：本题不用标准模版，使用二分搜索return左指针
  # 确保while范围是i < j，让[i, j]范围内一定有最小值
  # 当退出循环时，一定是i = j的情况，此时返回任一指针均是最小值
  class Solution:
      def findMin(self, nums: List[int]) -> int:
          i, j = 0, len(nums) - 1
          while i < j:
              mid = i + (j - i) // 2
              if nums[mid] < nums[j]:
                  j = mid
              else:
                  i = mid + 1
          return nums[i]
  ```

  

- [154.寻找旋转排序数组中的最小值2](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

  已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,4,4,5,6,7] 在变化后可能得到：
  若旋转 4 次，则可以得到 [4,5,6,7,0,1,4]
  若旋转 7 次，则可以得到 [0,1,4,4,5,6,7]
  注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

  给你一个可能存在 重复 元素值的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

  ```python
  # Note：本题不用标准模版，使用二分搜索return左指针
  # 确保while范围是i < j，让[i, j]范围内一定有最小值
  # 当退出循环时，一定是i = j的情况，此时返回任一指针均是最小值
  class Solution:
      def findMin(self, nums: List[int]) -> int:
          i, j = 0, len(nums) - 1
          while i < j:
              mid = i + (j - i) // 2
              if nums[mid] < nums[j]:
                  j = mid
              elif nums[mid] > nums[j]:
                  i = mid + 1
              # 当nums[mid] == nums[j]的时候，
              # 因为存在mid的值等于j的值，将j左移1位重新二分即可
              # 且由于mid的值等于j的值，区间[i,j]不会丢失最小值
              else:             
                  j -= 1
          return nums[i]
  ```

  

- [852.山脉数组的峰顶索引](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/)

  符合下列属性的数组 arr 称为 山脉数组 ：
  arr.length >= 3
  存在 i（0 < i < arr.length - 1）使得：
  arr[0] < arr[1] < ... arr[i-1] < arr[i]
  arr[i] > arr[i+1] > ... > arr[arr.length - 1]
  给你由整数组成的山脉数组 arr ，返回任何满足 arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1] 的下标 i 。

  ```python
  # Note：本题不用标准模版，使用二分搜索return左指针
  # 确保while范围是i < j，让[i, j]范围内一定有峰值
  # 当退出循环时，一定是i = j的情况，此时返回任一指针均是峰值
  class Solution:
      def peakIndexInMountainArray(self, arr: List[int]) -> int:
          i, j = 0, len(arr) - 1
          while i < j:
              mid = i + (j - i) // 2
              if arr[mid] > arr[mid + 1]:
                  j = mid
              else:
                  i = mid + 1
          return i
  ```

  

- [162.寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

  峰值元素是指其值严格大于左右相邻值的元素。

  给你一个整数数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 任何一个峰值 所在位置即可。

  你可以假设 nums[-1] = nums[n] = -∞ 。

  你必须实现时间复杂度为 O(log n) 的算法来解决此问题。

  ```python
  # Note：本题不用标准模版，使用二分搜索return左指针
  # 确保while范围是i < j，让[i, j]范围内一定有峰值
  # 当退出循环时，一定是i = j的情况，此时返回任一指针均是峰值
  class Solution:
      def findPeakElement(self, nums: List[int]) -> int:
          i, j = 0, len(nums) - 1
          while i < j:
              mid = i + (j - i) // 2
              if nums[mid] > nums[mid + 1]:
                  j = mid
              else:
                  i = mid + 1
          return i
  ```

  

- [367.有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

  给定一个 正整数 num ，编写一个函数，如果 num 是一个完全平方数，则返回 true ，否则返回 false 。

  进阶：不要 使用任何内置的库函数，如  sqrt 。

  ```python
  class Solution:
      def isPerfectSquare(self, num: int) -> bool:
          if num == 1: return True
          # 缩小右指针区间进行二分即可
          i, j = 2, num // 2
          while i <= j:
              mid = i + (j - i) // 2
              tmp = mid * mid
              if tmp == num: return True
              elif tmp < num: i = mid + 1
              else: j = mid - 1
          return False
  ```

  

- [69.x的平方根](https://leetcode-cn.com/problems/sqrtx/)

  给你一个非负整数 x ，计算并返回 x 的 平方根 。

  由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。

  注意：不允许使用任何内置指数函数和算符，例如 pow(x, 0.5) 或者 x ** 0.5 。

  ```python
  class Solution:
      def mySqrt(self, x: int) -> int:
          i, j = 0, x
          # 不断更新mid平方 <= x，最后的res即是小等于x的最大数
          res = -1
          while i <= j:
              mid = i + (j - i) // 2
              tmp = mid * mid
              if tmp <= x:
                  res = mid
                  i = mid + 1
              else:
                  j = mid - 1
          return res
  ```

  

- [74.搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

  编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

  每行中的整数从左到右按升序排列。
  每行的第一个整数大于前一行的最后一个整数。

  ```python
  # 二分查找
  # 将二维矩阵当作一维
  # 时间复杂度为O(logmn)
  class Solution:
      def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
          m, n = len(matrix), len(matrix[0])
          # 右指针即是两个矩阵长度和的位置
          i, j = 0, m * n - 1
          while i <= j:
              mid = i + (j - i) // 2
              # mid//n 为二维矩阵的行数
              # mid%n 为二维矩阵的列数
              tmp = matrix[mid // n][mid % n]
              if tmp == target: return True
              elif tmp < target: i = mid + 1
              else: j = mid - 1
          return False
  ```

  ```python
  # 特殊解法：根据该矩阵的性质
  # 时间复杂度为O(m + n)
  class Solution:
      def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
          # 从二维矩阵的左小角开始搜索
          # 若target大，则向右移动
          # 若target小，则向上移动
          i, j = len(matrix) - 1, 0
          while i >= 0 and j <= len(matrix[0]) - 1:
              if matrix[i][j] == target: return True
              elif matrix[i][j] < target: j += 1
              else: i -= 1
          return False
  ```

  

- [658.找到K个最接近的元素](https://leetcode-cn.com/problems/find-k-closest-elements/)

  给定一个排序好的数组 arr ，两个整数 k 和 x ，从数组中找到最靠近 x（两数之差最小）的 k 个数。返回的结果必须要是按升序排好的。

  整数 a 比整数 b 更接近 x 需要满足：

  |a - x| < |b - x| 或者
  |a - x| == |b - x| 且 a < b

  ```python
  # Note：本题不用标准模版，使用二分搜索return左指针（变题）
  class Solution:
      def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
          # len(arr)-k 为需要返回的左指针的最大值
          i, j = 0, len(arr) - k
          while i < j:
              mid = i + (j - i) // 2
              # x与arr[mid]的差更大时，指针需向右移动，来减小差
              if x - arr[mid] > arr[mid + k] - x:
                  i = mid + 1
              # arr[mid+k]与x的差更大时，指针需向左移动，来减小差
              else:
                  j = mid
          return arr[i:i + k]
  ```

  

- [875.爱吃香蕉的珂珂](https://leetcode-cn.com/problems/koko-eating-bananas/)

  珂珂喜欢吃香蕉。这里有 N 堆香蕉，第 i 堆中有 piles[i] 根香蕉。警卫已经离开了，将在 H 小时后回来。

  珂珂可以决定她吃香蕉的速度 K （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 K 根。如果这堆香蕉少于 K 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。  

  珂珂喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

  返回她可以在 H 小时内吃掉所有香蕉的最小速度 K（K 为整数）。

  ```python
  # Note：本题不用标准模版，使用二分搜索return左指针
  # 确保while范围是i < j：当前速度吃不完香蕉时，i向右移；当前速度吃得完香蕉时，j向左移
  # 当退出循环时，一定是i = j的情况，此时当前速度的左侧速度一定吃不完香蕉
  class Solution:
      def minEatingSpeed(self, piles: List[int], h: int) -> int:
          def possible(k):
              # 向上取整除法
              return sum((p + k - 1) // k for p in piles) <= h
  
          i, j = 1, max(piles)
          while i < j:
              mid = i + (j - i) // 2
              # 退出循环的情况，例：
              # F, F, T, T
              #       ij
              if not possible(mid):
                  i = mid + 1
              else:
                  j = mid
          return i
  ```

  

- 4.寻找两个正序数组的中位数

  给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

  ```python
  
  ```



------



###### 8. 哈希表

知识点：

> 

例题：

- [1.两数之和](https://leetcode-cn.com/problems/two-sum/)

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

  

- [15.三数之和](https://leetcode-cn.com/problems/3sum/)

  给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

  注意：答案中不可以包含重复的三元组。

  ```python
  class Solution:
      def threeSum(self, nums: List[int]) -> List[List[int]]:
          nums.sort()
          res, k = [], 0
          for k in range(len(nums) - 2):
              if nums[k] > 0: break
              if k > 0 and nums[k] == nums[k - 1]: continue
              i, j = k + 1, len(nums) - 1
              while i < j:
                  s = nums[k] + nums[i] + nums[j]
                  if s < 0:
                      i += 1
                      while i < j and nums[i] == nums[i - 1]: i += 1
                  elif s > 0:
                      j -= 1
                      while i < j and nums[j] == nums[j + 1]: j -= 1
                  else:
                      res.append([nums[k], nums[i], nums[j]])
                      i += 1
                      j -= 1
                      while i < j and nums[i] == nums[i - 1]: i += 1
                      while i < j and nums[j] == nums[j + 1]: j -= 1
          return res
  ```

  

- [141.环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

  给定一个链表，判断链表中是否有环。

  如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

  如果链表中存在环，则返回 true 。 否则，返回 false 。

  ```python
  # Note: 本题若用哈希表，
  # 时间复杂度为O(N)
  # 空间复杂度为O(N)
  # 在"4.栈和队列"部分有相同题，使用双指针，将空间复杂度优化为O(1)
  ```

  

- [面试题02.01.移除重复节点](https://leetcode-cn.com/problems/remove-duplicate-node-lcci/)

  编写代码，移除未排序链表中的重复节点。保留最开始出现的节点。

  ```python
  class Solution:
      def removeDuplicateNodes(self, head: ListNode) -> ListNode:
          if not head: return None
          vals = {head.val}
          cur = head
          while cur.next:
              tmp = cur.next
              if tmp.val not in vals:
                  vals.add(tmp.val)
                  cur = cur.next
              else:
                  cur.next = cur.next.next
          return head
  ```
  
  
  
- [面试题16.02.单词频率](https://leetcode-cn.com/problems/words-frequency-lcci/)

  设计一个方法，找出任意指定单词在一本书中的出现频率。

  你的实现应该支持如下操作：

  WordsFrequency(book)构造函数，参数为字符串数组构成的一本书
  get(word)查询指定单词在书中出现的频率

  ```python
  # Note: 本题可用哈希表及字典树
  # 不常考题，以下直接使用内建函数Counter
  class WordsFrequency:
      def __init__(self, book: List[str]):
          self.dic = collections.Counter(book)
  
      def get(self, word: str) -> int:
          return self.dic.get(word, 0)
  
  
  
  # Your WordsFrequency object will be instantiated and called as such:
  # obj = WordsFrequency(book)
  # param_1 = obj.get(word)
  ```

  

- [面试题01.02判定是否互为字符重排](https://leetcode-cn.com/problems/check-permutation-lcci/)

  给定两个字符串 `s1` 和 `s2`，请编写一个程序，确定其中一个字符串的字符重新排列后，能否变成另一个字符串。

  ```python
  class Solution:
      def CheckPermutation(self, s1: str, s2: str) -> bool:
          if len(s1) != len(s2): return False
          dic = {}
          for s in s1:
              if s not in dic: dic[s] = 1
              else: dic[s] += 1
          
          for s in s2:
              if s not in dic: return False
              dic[s] -= 1
              if dic[s] < 0: return False
          return True
  ```

  

- [剑指Offer03.数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

  找出数组中重复的数字。
  
  在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
  
  ```python
  # Note: 本题若用哈希表，
  # 时间复杂度为O(N)
  # 空间复杂度为O(N)
  # 以下解题为原地交换，优化空间复杂度为O(1)
  class Solution:
      def findRepeatNumber(self, nums: List[int]) -> int:
          i = 0
          while i < len(nums):
              if nums[i] == i:
                  i += 1
                  continue
              if nums[nums[i]] == nums[i]: return nums[i]
              nums[nums[i]], nums[i] = nums[i], nums[nums[i]]
          return -1
  ```
  
  

- [242.有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

  给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

  注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

  ```python
  class Solution:
      def isAnagram(self, s: str, t: str) -> bool:
          if len(s) != len(t): return False
          tmp = [0] * 26
          for char in s:
              tmp[ord(char) - ord('a')] += 1
          for char in t:
              tmp[ord(char) - ord('a')] -= 1
          for i in tmp:
              if i != 0: return False
          return True
  ```

  

- [49.字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

  给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。

  字母异位词 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母都恰好只用一次。

  ```python
  class Solution:
      def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
          dic = collections.defaultdict(list)
          for s in strs:
              count = [0] * 26
              for c in s:
                  count[ord(c) - ord('a')] += 1
              dic[tuple(count)].append(s)
          return list(dic.values())
  ```

  

- [136.只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

  给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

  说明：

  你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

  ```python
  # Note: 为不使用额外空间，本题使用位运算解法
  class Solution:
      def singleNumber(self, nums: List[int]) -> int:
          x = 0
          for num in nums:
              x ^= num
          return x
  ```

  

- [349.两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

  给定两个数组，编写一个函数来计算它们的交集。

  ```python
  class Solution:
      def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
          nums1.sort()
          nums2.sort()
          res = []
          i, j = 0, 0
          while i < len(nums1) and j < len(nums2):
              num1, num2 = nums1[i], nums2[j]
              if num1 == num2:
                  if not res or num1 != res[-1]:
                      res.append(num1)
                  i += 1
                  j += 1
              elif num1 > num2: j += 1
              else: i += 1
          return res
  ```

  

- 1122.数组的相对排序

- 706.设计哈希映射

- [146.LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

  运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制 。
  实现 LRUCache 类：

  LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
  int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
  void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
  
  进阶：你是否可以在 O(1) 时间复杂度内完成这两种操作？
  
  ```python
  class DLinkedNode:
      def __init__(self, key=0, value=0):
          self.key = key
          self.value = value
          self.pre = None
          self.next = None
  
  
  class LRUCache:
      def __init__(self, capacity: int):
          self.cache = {}
          self.head, self.tail = DLinkedNode(), DLinkedNode()
          self.head.next = self.tail
          self.tail.pre = self.head
          self.capacity = capacity
          self.size = 0
  
      def get(self, key: int) -> int:
          if key not in self.cache:
              return -1
          node = self.cache[key]
          self.moveToHead(node)
          return node.value
  
      def put(self, key: int, value: int) -> None:
          if key not in self.cache:
              node = DLinkedNode(key, value)
              self.cache[key] = node
              self.addToHead(node)
              self.size += 1
              if self.size > self.capacity:
                  tmp = self.removeTail()
                  self.cache.pop(tmp.key)
                  self.size -= 1
          else:
              node = self.cache[key]
              node.value = value
              self.moveToHead(node)
  
      def removeNode(self, node):
          node.pre.next = node.next
          node.next.pre = node.pre
  
      def addToHead(self, node):
          node.pre = self.head
          node.next = self.head.next
          self.head.next.pre = node
          self.head.next = node
  
      def moveToHead(self, node): 
          self.removeNode(node)
          self.addToHead(node)
  
      def removeTail(self):
          node = self.tail.pre
          self.removeNode(node)
          return node
  
  
  
  # Your LRUCache object will be instantiated and called as such:
  # obj = LRUCache(capacity)
  # param_1 = obj.get(key)
  # obj.put(key,value)
  ```
  
  
  
- 面试题16.21.交换和



------



###### 9. 二叉树

知识点：

> 

例题：

> 题型1: 二叉树前中后序遍历

- 144.二叉树的前序遍历
- 94.二叉树的中序遍历
- 145.二叉树的后序遍历
- 589.N叉树的前序遍历
- 590.N叉树的后序遍历

> 题型2: 二叉树按层遍历

- 剑指Offer32-1.从上到下打印二叉树
- 剑指Offer32-2.从上到下打印二叉树2
- 剑指Offer32-3.从上到下打印二叉树3
- 102.二叉树的层序遍历

> 题型3: 二叉树上的递归

- 104.二叉树的最大深度
- 559.N叉树的最大深度
- 剑指Offer55-2.平衡二叉树
- 617.合并二叉树
- 226.翻转二叉树
- 101.对称二叉树
- 98.验证二叉搜索树

> 题型4: 二叉查找树

- 剑指Offer54.二叉搜索树的第k大节点
- 538.把二叉搜索树转换为累加树
- 面试题04.06.后继者

> 题型5: LCA最近公共祖先

- 236.二叉树的最近公共祖先
- 剑指Offer68-1.二叉搜索树的最近公共祖先

> 题型6: 二叉树转单、双、循环链表

- 114.二叉树展开为链表
- 面试题17.12.BiNode
- 剑指Offer36.二叉搜索树与双向链表
- 面试题04.03.特定深度节点链表

> 题型7: 按照遍历结果反向构建二叉树

- 105.从前序与中序遍历序列构造二叉树
- 889.根据前序和后序遍历构造二叉树
- 106.从中序与后序遍历序列构造二叉树
- 剑指Offer33.二叉搜索树的后序遍历序列

> 题型8: 二叉树上的最长路径和

- 543.二叉树的直径
- 剑指Offer34.二叉树中和为某一值的路径
- 124.二叉树中的最大路径和
- 437.路径总和3



------



###### 10. 二叉树 + 前缀树 + 堆

知识点：

> 

例题：

- 23.合并K个升序链表
- 347.前K个高频元素
- 295.数据流的中位数
- 973.最接近原点的K个点
- 313.超级丑数
- 208.实现Trie（前缀树）
- 面试题17.17.多次搜索
- 212.单词搜索2



------



###### 11. 回溯

知识点：

>

例题：

- 面试题08.12.八皇后
- 37.解数独
- 17.电话号码的字母组合
- 77.组合
- 78.子集
- 90.子集2
- 46.全排列
- 47.全排列2
- 39.组合总和
- 40.组合总和2
- 216.组合总和3
- 131.分割回文串
- 93.复原IP地址
- 22.括号生成



------



###### 12. DFS + BFS

知识点：

>问题类型：
>
>(1) 二维矩阵搜索或遍历
>
>使用深度优先搜索 DFS: 即给定一个mxn的二维矩阵时，dfs(i + 1, j) + dfs(i, j + 1)，先遍历深度 i -> 0 to m，再遍历宽度 j -> 0 to n
>
>(2) 最短路径 (广度优先搜索 BFS)
>
>(3) 连通分量 / 连通性
>
>(4) 拓扑排序
>
>(5) 检测环

例题：

- [剑指Offer13.机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

  地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

  ```python
  class Solution:
      def movingCount(self, m: int, n: int, k: int) -> int:
          def dfs(i, j, si, sj):
              if i >= m or j >= n or si + sj > k or (i, j) in visited: return 0
              visited.add((i, j))
              return 1 + dfs(i + 1, j, si + 1 if (i + 1) % 10 else si - 8, sj) + dfs(i, j + 1, si, sj + 1 if (j + 1) % 10 else sj - 8)
  
          visited = set()
          return dfs(0, 0, 0, 0)
  ```

  

- 面试题08.10.颜色填充

- 面试题04.01.节点间通路

- 200.岛屿数量

- 面试题16.19.水域大小

- 207.课程表

- 79.单词搜索

- 1306.跳跃游戏3

- 752.打开转盘锁

- 面试题17.22.单词转换

- 面试题17.07.婴儿名字

- 529.扫雷游戏

- 127.单词接龙

- 126.单词接龙2



------



###### 13. 动态规划

知识点：

> 问题类型：
>
> (1) 最值
>
> 有n个物品，选择其中一些物品装入背包，在不超过背包最大重量限制的前提下，背包中可装物品总重量的最大值是多少？
>
> > 状态：`boolean dp[n][w+1] -> dp[i][j]` 即第i个物品决策完之后，背包重量为j的状态
> >
> > 状态转移方程：`dp[i][j] = dp[i-1][j] or dp[i-1][j-weight[i]]` 即该状态是由上一个状态决策拿或不拿转移而来
>
> 有n个物品，选择其中一些物品装入背包，正好装满背包所需物品最少个数？
>
> > 状态：`int dp[n][w+1] -> dp[i][j]` 即第i个物品决策完之后，背包重量为j对应的最少物品个数
> >
> > 状态转移方程：`dp[i][j] = min(dp[i-1][j], dp[i-1][j-weight[i]] + 1)` 若这个决策拿了，需要+1
>
> (2) 可行 
>
> 有n个物品，选择其中一些物品装入背包，能不能正好装满背包？
>
> > 状态：`boolean dp[n][w+1] -> dp[i][j]` 即第i个物品决策完之后，背包重量为j的状态可达
> >
> > 状态转移方程：`dp[i][j] = dp[i-1][j] or dp[i-1][j-weight[i]]`
>
> (3) 计数
>
> 有n个物品，选择其中一些物品装入背包，装满背包有多少种不同的装法？
>
> > 状态：`int dp[n][w+1] -> dp[i][j]` 即第i个物品决策完之后，背包重要为j的对应有几种装法
> >
> > 状态转移方程：`dp[i][j] = dp[i-1][j] + dp[i-1][j-weight[i]]`
>
> 
>
> 空间优化：
>
> 将二维数组优化为滚动数组或一维数组：
>
> 滚动数组即 `dp[n][w+1] -> dp[2][w+1]` 初始赋值第0行，循环不断遍历第1行、第0行......
>
> 一维数组即 `dp[n][w+1] -> dp[w+1]` 初始赋值整个数组，循环不断从后往前遍历

例题：

> 题型1：背包

- [416.分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

  给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

  ```python
  # 状态: dp[sum(nums) // 2]
  # 状态转移方程: dp[j] = dp[j] or dp[j - num]
  # 本题使用一维数组
  class Solution:
      def canPartition(self, nums: List[int]) -> bool:
          if len(nums) < 2: return False
          
          total = sum(nums)
          if total % 2 == 1: return False
          
          val = total // 2
          dp = [True] + [False] * val
          for num in nums:
              for j in range(val, num - 1, -1):
                  dp[j] = dp[j] or dp[j - num]
          
          return dp[-1]
  ```

  

- [494.目标和](https://leetcode-cn.com/problems/target-sum/)

  给你一个整数数组 nums 和一个整数 target 。

  向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：

  例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
  返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。

  ```python
  # 状态: dp[(sum(nums) - target) // 2]
  # 状态转移方程: dp[j] = dp[j] + dp[j - num]
  # 本题使用一维数组
  class Solution:
      def findTargetSumWays(self, nums: List[int], target: int) -> int:
          total = sum(nums)
          diff = total - target
          if diff < 0 or diff % 2 == 1: return 0
  
          neg = diff // 2
          dp = [1] + [0] * neg
          for num in nums:
              for j in range(neg, num - 1, -1):
                  dp[j] += dp[j - num]
          
          return dp[-1]
  ```

  

- [322.零钱兑换](https://leetcode-cn.com/problems/coin-change/)

  给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

  计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

  你可以认为每种硬币的数量是无限的。

  ```python
  # 状态: dp[amount + 1]
  # 状态转移方程: dp[i] = min(dp[i], dp[i - coin] + 1)
  # 本题使用一维数组
  class Solution:
      def coinChange(self, coins: List[int], amount: int) -> int:
          dp = [0] + [float('inf')] * amount
          for coin in coins:
              for i in range(coin, amount + 1):
                  dp[i] = min(dp[i], dp[i - coin] + 1)
          
          return dp[-1] if dp[-1] != float('inf') else -1
  ```

  

- [518.零钱兑换2](https://leetcode-cn.com/problems/coin-change-2/)

  给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

  请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

  假设每一种面额的硬币有无限个。 

  题目数据保证结果符合 32 位带符号整数。

  ```python
  # 状态: dp[amount + 1]
  # 状态转移方程: dp[i] = dp[i] + dp[i - coin]
  # 本题使用一维数组
  class Solution:
      def change(self, amount: int, coins: List[int]) -> int:
          dp = [1] + [0] * amount
          for coin in coins:
              for i in range(coin, amount + 1):
                  dp[i] += dp[i - coin]
          
          return dp[-1]
  ```
  
  

> 题型2：路径

- [64.最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

  给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

  **说明：**每次只能向下或者向右移动一步。

  ```python
  # 状态: dp[m][n]
  # 状态转移方程: dp[i][j] += min(dp[i - 1][j], dp[i][j - 1])
  # 本题使用原地修改，不使用额外空间
  class Solution:
      def minPathSum(self, grid: List[List[int]]) -> int:
          for i in range(len(grid)):
              for j in range(len(grid[0])):
                  if i == 0 and j == 0: continue
                  elif i == 0: grid[i][j] += grid[i][j - 1]
                  elif j == 0: grid[i][j] += grid[i - 1][j]
                  else: grid[i][j] += min(grid[i - 1][j], grid[i][j - 1])
          
          return grid[-1][-1]
  ```

  

- [剑指Offer47.礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

  在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物

  ```python
  # 状态: dp[m][n]
  # 状态转移方程: dp[i][j] += max(dp[i - 1][j], dp[i][j - 1])
  # 本题使用原地修改，不使用额外空间
  class Solution:
      def maxValue(self, grid: List[List[int]]) -> int:
          for i in range(len(grid)):
              for j in range(len(grid[0])):
                  if i == 0 and j == 0: continue
                  elif i == 0: grid[i][j] += grid[i][j - 1]
                  elif j == 0: grid[i][j] += grid[i - 1][j]
                  else: grid[i][j] += max(grid[i - 1][j], grid[i][j - 1])
          
          return grid[-1][-1]
  ```

  

- [120.三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

  给定一个三角形 triangle ，找出自顶向下的最小路径和。

  每一步只能移动到下一行中相邻的结点上。相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。也就是说，如果正位于当前行的下标 i ，那么下一步可以移动到下一行的下标 i 或 i + 1 。

  ```python
  # 状态: dp[n][n]
  # 状态转移方程: dp[i][j] += min(dp[i - 1][j], dp[i - 1][j - 1])
  # 本题使用原地修改，不使用额外空间
  class Solution:
      def minimumTotal(self, triangle: List[List[int]]) -> int:
          for i in range(1, len(triangle)):
              for j in range(i + 1):
                  if j == 0: triangle[i][j] += triangle[i - 1][j]
                  elif j == i: triangle[i][j] += triangle[i - 1][j - 1]
                  else: triangle[i][j] += min(triangle[i - 1][j], triangle[i - 1][j - 1])
          
          return min(triangle[-1])
  ```

  

- [62.不同路径](https://leetcode-cn.com/problems/unique-paths/)

  一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

  机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

  问总共有多少条不同的路径？

  ```python
  # 状态: dp[m][n]
  # 状态转移方程: dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
  class Solution:
      def uniquePaths(self, m: int, n: int) -> int:
          dp = [[0] * n for _ in range(m)]
          for i in range(m):
              for j in range(n):
                  if i == 0 and j == 0: dp[i][j] = 1
                  elif i == 0: dp[i][j] = dp[i][j - 1]
                  elif j == 0: dp[i][j] = dp[i - 1][j]
                  else: dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
  
          return dp[-1][-1]
  ```

  ```python
  # 状态: dp[m][n]
  # 状态转移方程: dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
  # 本题使用滚动数组
  class Solution:
      def uniquePaths(self, m: int, n: int) -> int:
          dp = [[0] * n for _ in range(3)]
          for i in range(m):
              for j in range(n):
                  if i == 0 or j == 0: dp[i % 3][j] = 1
                  else: dp[i % 3][j] = dp[(i - 1) % 3][j] + dp[i % 3][j - 1]
  
          return dp[(m - 1) % 3][-1]
  ```

  

- [63.不同路径2](https://leetcode-cn.com/problems/unique-paths-ii/)

  一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

  机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

  现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

  ```python
  # 状态: dp[m][n]
  # 状态转移方程: dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
  # 本题使用原地修改，不使用额外空间
  class Solution:
      def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
          for i in range(len(obstacleGrid)):
              for j in range(len(obstacleGrid[0])):
                  if obstacleGrid[i][j] == 1:
                      obstacleGrid[i][j] = 0
                  else:
                      if i == 0 and j == 0: obstacleGrid[i][j] = 1
                      elif i == 0: obstacleGrid[i][j] = obstacleGrid[i][j - 1]
                      elif j == 0: obstacleGrid[i][j] = obstacleGrid[i - 1][j]
                      else: obstacleGrid[i][j] = obstacleGrid[i-1][j]+obstacleGrid[i][j-1]
  
          return obstacleGrid[-1][-1]
  ```

  

> 题型3：打家劫舍&买卖股票

- [198.打家劫舍](https://leetcode-cn.com/problems/house-robber/)

  你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

  给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

  ```python
  # 状态: dp[n]
  # 状态转移方程: dp[i] = max(nums[i - 1], nums[i - 2] + nums[i])
  # 本题使用原地修改，不使用额外空间
  class Solution:
      def rob(self, nums: List[int]) -> int:
          if not nums: return 0
          n = len(nums)
          if n == 1: return nums[0]
          
          nums[1] = max(nums[0], nums[1])
          for i in range(2, n):
              nums[i] = max(nums[i - 1], nums[i - 2] + nums[i])
          return nums[-1]
  ```

  ```python
  # 进阶，代码简化
  class Solution:
      def rob(self, nums: List[int]) -> int:
          cur, pre = 0, 0
          for num in nums:
              cur, pre = max(cur, pre + num), cur
          return cur
  ```

  

- [213.打家劫舍2](https://leetcode-cn.com/problems/house-robber-ii/)

  你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

  给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

  ```python
  # Note: 特殊情况，环形数组
  # 根据[198.打家劫舍]进阶回答，遍历两种情况即可
  # 情况1.偷第一家，nums删去最后一家
  # 情况2.不偷第一家，nums删去第一家
  class Solution:
      def rob(self, nums: List[int]) -> int:
          def lineRob(nums):
              cur, pre = 0, 0
              for num in nums:
                  cur, pre = max(cur, pre + num), cur
              return cur
          
          if len(nums) == 1: return nums[0]
          return max(lineRob(nums[:-1]), lineRob(nums[1:]))
  ```

  

- [337.打家劫舍3](https://leetcode-cn.com/problems/house-robber-iii/)

  在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

  计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

  ```python
  # Note: 特殊情况，树形数组
  # 每个节点的值分为 (偷该节点，不偷该节点)
  # 且遍历二叉树时采用后序遍历
  class Solution:
      def rob(self, root: TreeNode) -> int:
          def treeRob(root):
              if not root: return (0, 0)
              left = treeRob(root.left)
              right = treeRob(root.right)
              val1 = root.val + left[1] + right[1]
              val2 = max(left[0], left[1]) + max(right[0], right[1])
              return (val1, val2)
          
          res = treeRob(root)
          return max(res[0], res[1])
  ```

  

- [714.买卖股票的最佳时机含手续](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

  给定一个整数数组 prices，其中第 i 个元素代表了第 i 天的股票价格 ；整数 fee 代表了交易股票的手续费用。

  你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

  返回获得利润的最大值。

  注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

  ```python
  # 状态: dp[[n, n]]
  # 状态转移方程:
  # 当前不持有股票利润: 上一天不持有股票利润 / 上一天持有股票今天卖掉利润
  # dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee)
  # 当前持有股票利润: 上一天持有股票利润 / 上一天不持有股票今天买入利润
  # dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i])
  # p.s. 股票题特殊返回情况，最后一天不持有股票的利润一定大于持有的
  class Solution:
      def maxProfit(self, prices: List[int], fee: int) -> int:
          n = len(prices)
          dp = [[0, -prices[0]]] + [[0, 0] for _ in range(n - 1)]
          for i in range(1, n):
              dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee)
              dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i])
          return dp[-1][0]
  ```

  ```python
  # 使用两个常量，优化空间复杂度为O(1)
  class Solution:
      def maxProfit(self, prices: List[int], fee: int) -> int:
          not_hold, hold = 0, -prices[0]
          for price in prices:
              not_hold = max(not_hold, hold + price - fee)
              hold = max(hold, not_hold - price)
          return not_hold
  ```

  

- [309.最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

  给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。

  设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

  你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
  卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

  ```python
  # 状态: dp[[n, n, n]]
  # 状态转移方程:
  # 当前持有股票利润: 上一天持有股票利润 / 上一天不持有股票且不是冷冻期利润 + 今天买入
  # dp[i][0] = max(dp[i - 1][0], dp[i - 1][2] - prices[i])
  # 当前不持有股票且是冷冻期利润: 上一天持有股票利润 + 今天卖出
  # dp[i][1] = dp[i - 1][0] + prices[i]
  # 当前不持有股票且不是冷冻期利润: 上一天不持有股票且不是冷冻期利润 / 上一天不持有股票且是冷冻期
  # dp[i][2] = max(dp[i - 1][2], dp[i - 1][1])
  # p.s. 股票题特殊返回情况，最后一天不持有股票的利润一定大于持有的
  class Solution:
      def maxProfit(self, prices: List[int]) -> int:
          n = len(prices)
          dp = [[-prices[0], 0, 0]] + [[0] * 3 for _ in range(n - 1)]
          for i in range(1, n):
              dp[i][0] = max(dp[i - 1][0], dp[i - 1][2] - prices[i])
              dp[i][1] = dp[i - 1][0] + prices[i]
              dp[i][2] = max(dp[i - 1][2], dp[i - 1][1])
          return max(dp[-1][1], dp[-1][2])
  ```

  ```python
  # 使用三个常量，优化空间复杂度为O(1)
  class Solution:
      def maxProfit(self, prices: List[int]) -> int:
          f0, f1, f2 = -prices[0], 0, 0
          for price in prices:
              tmp_f0 = max(f0, f2 - price)
              tmp_f1 = f0 + price
              tmp_f2 = max(f2, f1)
              f0, f1, f2 = tmp_f0, tmp_f1, tmp_f2
          return max(f1, f2)
  ```

  

> 题型4：爬楼梯问题

- [70.爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

  假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

  每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

  **注意：**给定 *n* 是一个正整数。

  ```python
  # 最开始情况：F(2) = F(1) + F(0)
  # a 为 F(0)，b 为 F(1)
  class Solution:
      def climbStairs(self, n: int) -> int:
          a, b = 1, 1
          for _ in range(n):
              a, b = b, a + b
          return a
  ```

  

- 322.零钱兑换

- 518.零钱兑换2

- [剑指Offer14-1.剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

  给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

  ```python
  # 状态: dp[n + 1]
  # 状态转移方程: dp[i] = max(dp[i], j * (i - j), j * dp[i - j])
  class Solution:
      def cuttingRope(self, n: int) -> int:
          dp = [0] * (n + 1)
          for i in range(2, n + 1):
              for j in range(i):
                  dp[i] = max(dp[i], j * (i - j), j * dp[i - j])
          return dp[-1]
  ```

  

- [剑指Offer46.把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

  给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

  ```python
  # 最开始情况：F(2)
  # 1. 若10 <= s[0:2] <= 25：F(2) = F(1) + F(0)
  # 2. 若 s[0:2] < 10：F(2) = F(1)
  # a 为 F(0)，b 为 F(1)
  class Solution:
      def translateNum(self, num: int) -> int:
          s = str(num)
          a = b = 1
          for i in range(2, len(s) + 1):
              a, b = (a + b if '10' <= s[i - 2:i] <= '25' else a), a
          return a
  ```

  

- [139.单词拆分](https://leetcode-cn.com/problems/word-break/)

  给定一个非空字符串 s 和一个包含非空单词的列表 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

  说明：

  拆分时可以重复使用字典中的单词。
  你可以假设字典中没有重复的单词。

  ```python
  # 状态: dp[n + 1]
  # 状态转移方程: dp[i] and (s[i:j] in wordDict) update dp[j]
  class Solution:
      def wordBreak(self, s: str, wordDict: List[str]) -> bool:
          n = len(s)
          dp = [True] + [False] * n
          for i in range(n):
              for j in range(i + 1, n + 1):
                  if dp[i] and (s[i:j] in wordDict):
                      dp[j] = True
          return dp[-1]
  ```

  

> 题型5：匹配问题

- 1143.最长公共子序列
- 72.编辑距离

> 题型6：其他

- 437.路径总和3
- 300.最长递增子序列





























































