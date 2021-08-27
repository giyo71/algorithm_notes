# Algorithm notes

1. ==纯编程题==

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

     

2. ==找规律题==

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
     
     
