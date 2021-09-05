# Algorithm notes

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
          self.stack1 = []
          self.stack2 = []
  
          
      def appendTail(self, value: int) -> None:
          self.stack1.append(value)
  
          
      def deleteHead(self) -> int:
          if self.stack2: return self.stack2.pop()
          if not self.stack1: return -1
          while self.stack1:
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

  你只能使用队列的基本操作 —— 也就是 push to back、peek/pop from front、size 和 is empty 这些操作。
  你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

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
          self.stack = []
  
  
      def push(self, val: int) -> None:
          tmp_stack = []
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
          self.min_stack = [math.inf]
  
  
      def push(self, val: int) -> None:
          self.stack.append(val)
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
          if len(s) % 2 == 1: return False
          dic = {'(': ')', '{': '}', '[': ']'}
          stack = []
          for c in s:
              if c in dic: stack.append(c)
              elif not stack or dic[stack.pop()] != c: return False
          return not stack
  ```

  

- 面试题16.26.计算器

- 772.基本计算器3

- 1047.删除字符串中的所有相邻重复项

- 剑指Offer31.栈的压入、弹出序列

- 739.每日温度

- 42.接雨水

- 84.柱状图中最大的矩形

- 面试题03.06.动物收容所

- 剑指Offer59-2.队列的最大值

- 剑指Offer59-1.滑动窗口的最大值

