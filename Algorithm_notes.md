# Algorithm notes

###### Contents

1. 纯编程题
2. 找规律题
3. 数组和链表
4. 栈和队列
5. 递归
6. 排序
7. 二分查找
8. 哈希表
9. 二叉树



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
  # 使用哈希表方法而不去排序
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
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, val=0, next=None):
  #         self.val = val
  #         self.next = next
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
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, val=0, next=None):
  #         self.val = val
  #         self.next = next
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
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, val=0, next=None):
  #         self.val = val
  #         self.next = next
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
>     i, j = 0, len(nums) - 1
>     while i <= j:
>         mid = (i + j) // 2
>         if nums[mid] == value:
>             return mid
>         elif nums[mid] < value:
>             i = mid + 1
>         else:
>             j = mid - 1
>     return -1
> ```
>
> ```python
> # 经典实现2: 递归
> # 空间复杂度O(logn)，时间复杂度O(logn)
> def binary_search(nums: List[int], value: int, i: int, j: int):
>     if not i <= j: return -1
>     mid = (i + j) // 2
>     if nums[mid] == value:
>         return mid
>     elif nums[mid] < value:
>         return binary_search(nums, value, mid + 1, j)
>     else:
>         return binary_search(nums, value, i, mid - 1)
>       
> binary_search(nums, value, 0, len(nums) - 1)
> ```
>
> 

例题：

- 704.二分查找
- 374.猜数字大小
- 744.寻找比目标字母大的最小字母
- 35.搜索插入位置
- 34.在排序数组中查找元素的第一个和最后一个位置
- 面试题10.05.稀疏数组搜索
- 33.搜索选择排序数组
- 153.寻找选择排序数组中的最小值
- 852.山脉数组的峰顶索引
- 162.寻找峰值
- 367.有效的完全平方数
- 69.x的平方根
- 74.搜索二维矩阵
- 658.找到K个最接近的元素
- 875.爱吃香蕉的珂珂
- 81.搜索选择排序数组2
- 154.寻找旋转排序数组中的最小值2
- 4.寻找两个正序数组的中位数



------



###### 8. 哈希表

知识点：

> 

例题：

- 1.两数之和
- 15.三数之和
- 160.相交链表
- 141.环形链表
- 面试题02.01.移除重复节点
- 面试题16.02.单词频率
- 面试题01.02判定是否互为字符重排
- 剑指Offer03.数组中重复的数字
- 242.有效的字母异位词
- 49.字母异位词分组
- 136.只出现一次的数字
- 349.两个数组的交集
- 1122.数组的相对排序
- 706.设计哈希映射
- 146.LRU缓存机制
- 面试题16.21.交换和



------



###### 9. 二叉树

知识点：

> 

例题：











































































