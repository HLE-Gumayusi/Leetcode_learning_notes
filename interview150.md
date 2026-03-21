    这个文档用来记录我在Leetcode上刷Top Interview 150的学习过程，将会记录一些对我有启发的代码。

## 1. Merge Sorted Array
题目描述：给定两个有序数组nums1和nums2，将nums2合并到nums1中，使得num1成为一个有序数组。注意，nums1有足够的空间来容纳nums2中的元素。
刚开始，我的想法是创建一个新列表，然后将nums1和nums2中的元素合并到新列表中，最后将新列表复制回nums1。后来问ai才知道其实可以直接从后往前合并，这样就不需要额外的空间了。

这题的关键是使用双指针，我一开始是从数列前面开始比较的，但是这样会覆盖第一个数列原本的元素，导致无法正确合并。正确的做法是从数列的末尾开始比较，这样就不会覆盖原本的元素了。由于第一个数组在最后留下了第二个数组的空间，所以不会覆盖原本的元素。

## 2. Remove Element
题目描述：给定一个数组nums和一个值val，原地移除所有数值等于val的元素，并返回移除后数组的新长度。

这道题需要把所有不等于val的元素放到列表的前面，后面的数字可以不管。这个很简单，两个指针一个在前面一个在后面就可以。

## 3. Remove Duplicates from Sorted Array
题目描述：给定一个排序数组nums，原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
只需要记录和上一个元素不同的元素，用一个指针记录不同的元素。

## 4. Remove Duplicates from Sorted Array II
和上一题一样。

## 5. Majority Element
题目描述：给定一个大小为n的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于n/2的元素。

这题使用了Boyer-Moore Majority Vote算法，核心思想是维护一个候选元素和一个计数器。遍历数组时，如果当前元素与候选元素相同，则计数器加1；如果不同，则计数器减1。当计数器为0时，更新候选元素为当前元素。最后遍历结束后，即能得到多数元素。

这个算法非常巧妙，时间复杂度为O(n)，空间复杂度为O(1)，非常高效。

```python
def majorityElement(nums):
    count = 0
    candidate = None

    for num in nums:
        if count == 0:
            candidate = num
        count += (1 if num == candidate else -1)

    return candidate
```
## 6. Rotate Array
题目描述：给定一个数组，将数组中的元素向右移动k个位置，其中k是非负数。

这个题我写了一个将元素移动一次的函数，然后调用k次。但是这个代码的运行时间超时了。我后来使用对k取模的方式来优化，避免了不必要的移动。

后来问ai才知道其实可以使用反转数组的方法来解决这个问题。首先反转整个数组，然后反转前k个元素，最后反转剩下的元素。这个做法非常巧妙，只需要写一个反转函数就可以了。

```python
def rotate(nums, k):
    k = k % len(nums)

    def reverse(start, end):
        while start < end:
            nums[start], nums[end] = nums[end], nums[start]
            start += 1
            end -= 1

    reverse(0, len(nums) - 1)
    reverse(0, k - 1)
    reverse(k, len(nums) - 1)
```

## 7. Best Time to Buy and Sell Stock
题目描述：给定一个数组prices，其中prices[i]是第i天的股票价格。你只能选择某一天买入股票，并选择在未来的某一个不同的日子卖出该股票。设计一个算法来计算你所能获取的最大利润。

这题是要找到最大的价格爬升区间。我的想法是记录一个目前最低价格s，和一个目前最大差价p。在遍历的过程中，如果当前价格比s更低，就更新s；如果当前价格和s的差价比p更大，就更新p。最后返回p即可。

```python
def maxProfit(self, prices: List[int]) -> int:
        s = prices[0]
        l = prices[0]
        p = 0
        if not prices:
            return 0
        for i in range(len(prices)):
            if prices[i] - s > p:
                l = prices[i]
                p = l - s
            elif prices[i] < s:
                s = prices[i]
        return p
```

这个代码ai说变量l是多余的，不过不影响结果。这是我第一次写完代码直接提交就通过了，没有借助ai的帮助。

## 8. Best Time to Buy and Sell Stock II
题目描述：给定一个数组prices，其中prices[i]是第i天的股票价格。你可以尽可能地完成更多的交易（多次买卖一支股票）。设计一个算法来计算你所能获取的最大利润。

这个问题比上一题更简单，只需要找到所有的价格爬升区间就可以。我写了个程序来比较当前价格和前一个价格，如果当前价格更高，就把差价加到利润中。

太简单了，不写代码了。

## 9. Jump Game
题目描述：给定一个非负整数数组nums，你最初位于数组的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。判断你是否能够到达最后一个位置。

这道题我写return true，发现报错了，后来发现应该写True。我的想法是关注所有false的情况，因为false的唯一可能就是遇到无法跨越的0，所以我就写了一个程序来检查每个0前面是否有足够的跳跃能力来跨越它。

···python
def canJump(self, nums: List[int]) -> bool:
        if not len(nums):
            return False
        if nums[0] == 0:
            if len(nums) == 1:
                return True
            return False
        for i in range(len(nums)-1):
            if nums[i] == 0 :
                s = 0
                for j in (range(i)):
                    if i - j < nums[j]:
                        s += 1
                if s == 0:
                    return False
        return True
···

这个代码虽然能通过测试用例，但是效率很低。后来问ai才知道其实可以使用一个变量来记录当前能够跳跃的最远位置，如果在遍历过程中发现当前索引超过了这个最远位置，就说明无法到达最后一个位置了。

```python
def canJump(self, nums: List[int]) -> bool:
        max_reach = 0
        for i in range(len(nums)):
            if i > max_reach:
                return False
            max_reach = max(max_reach, i + nums[i])
        return True
```

## 10. Jump Game II
题目描述：给定一个非负整数数组nums，你最初位于数组的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。你的目标是使用最少的跳跃次数到达数组的最后一个位置。

这道题和上一题类似，我的方法是记录经过n次跳跃的最远位置，当这个最远位置超过了数组长度时，返回n就可以了。

```python
def jump(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return 0
        n = 1
        m = nums[0]
        while m < len(nums) - 1:
            for i in range(m+1):
                if i + nums[i] > m:
                    m = i + nums[i]
            n += 1
        return n
```

这个代码虽然能通过测试用例，但是效率很低。后来问ai才知道其实可以使用两个变量，一个记录当前能够跳跃的最远位置，一个记录下一个能够跳跃的最远位置。

```python
def jump(self, nums: List[int]) -> int:
        jumps = 0
        current_end = 0
        farthest = 0

        for i in range(len(nums) - 1):
            farthest = max(farthest, i + nums[i])
            if i == current_end:
                jumps += 1
                current_end = farthest

        return jumps
```

## 11. H-Index
题目描述：给定一个整数数组citations，表示研究人员的被引次数。计算研究人员的h指数。h指数的定义是：一个研究人员有h篇论文分别被引用了至少h次。

这个题目很简单，只需要给数组从大到小排序，然后找到第一个数值小于自己的位次（索引加1）的数字，返回它的索引即可。

```python
def hIndex(self, citations: List[int]) -> int:
        if not len(citations):
            return 0
        citations.sort(reverse = True)
        for i in range(len(citations)):
            if citations[i] < i + 1:
                return i
        return len(citations)
```

这个代码使用了内置sort函数，时间复杂度为O(n log n)。通过询问ai找到了更高效的算法：

```python
def hIndex(self, citations: List[int]) -> int:
        n = len(citations)
        buckets = [0] * (n + 1)

        for c in citations:
            if c >= n:
                buckets[n] += 1
            else:
                buckets[c] += 1

        count = 0
        for i in range(n, -1, -1):
            count += buckets[i]
            if count >= i:
                return i

        return 0
```

这个算法直接记录了每个引用次数的论文数量，然后从高到低累加，找到第一个满足条件的h指数。时间复杂度为O(n)。

## 12. Insert Delete GetRandom O(1)
题目描述：实现一个RandomizedSet类，支持以下操作：insert(val)：当元素val不存在时，向集合中插入该项，并返回true；当元素val存在时，返回false。remove(val)：当元素val存在时，从集合中移除该项，并返回true；当元素val不存在时，返回false。getRandom()：随机返回现有集合中的一项。每个元素应该有相同的概率被返回。

以上每个操作都应该在平均时间复杂度O(1)内完成。

我不知道有什么O(1)的算法来实现这个功能，后来问ai才知道哈希表可以满足插入和删除的O(1)时间复杂度，动态数组可以满足随机访问的O(1)时间复杂度。

```python
import random

class RandomizedSet:
    def __init__(self):
        # 存储具体的值
        self.nums = []
        # 存储 值 -> 索引 的映射
        self.val_to_index = {}

    def insert(self, val: int) -> bool:
        if val in self.val_to_index:
            return False
        self.val_to_index[val] = len(self.nums)
        self.nums.append(val)
        return True

    def remove(self, val: int) -> bool:
        if val not in self.val_to_index:
            return False
        
        # 1. 找到待删元素的索引和数组最后一个元素
        idx = self.val_to_index[val]
        last_val = self.nums[-1]
        
        # 2. 将最后一个元素换到待删位置
        self.nums[idx] = last_val
        self.val_to_index[last_val] = idx
        
        # 3. 删除最后一个元素并清理哈希表
        self.nums.pop()
        del self.val_to_index[val]
        return True

    def getRandom(self) -> int:
        return random.choice(self.nums)
```

这里关于删除的方法很巧妙，我没有想到。

## 13. Product of Array Except Self
题目描述：给定一个长度为n的整数数组nums，其中n > 1，返回一个输出数组output，其中output[i]等于nums中除了nums[i]之外其余各元素的乘积。

这题不允许使用除法，我想了很久也只能实现O(n^2)的算法，最后问ai才得到了一个O(n)的算法。这个算法的核心思想是使用两个数组，一个记录从左到右的乘积，一个记录从右到左的乘积，然后将这两个数组对应位置的乘积相乘就得到了最终结果。

```python
def productExceptSelf(self, nums: List[int]) -> List[int]:
        answer = [1]*len(nums)
        left = 1
        for i in range(len(nums)):
            answer[i] = left
            left *= nums[i]
        right = 1
        for j in range(len(nums)-1, -1, -1):
            answer[j] *= right
            right *= nums[j]
        return answer
```

在for循环进行中，左边的乘积逐渐累积，然后再让answer乘以右边的乘积。

## 14. Gas Station
题目描述：在一条环路上有N个加油站，其中第i个加油站有gas[i]升汽油。你有一辆油箱容量无限的汽车，从第i个加油站开到第i+1个加油站需要cost[i]升汽油。你从其中的一个加油站出发，开始时油箱为空。给定两个整数数组gas和cost，返回你可以完成环路的起始加油站的编号，如果不能完成环路则返回-1。

题目保证如果存在解，则它是唯一的。我创建了一个新列表，第i个元素等于gas[i] - cost[i]，这代表车经过这个站点时油量的变化量。我开始的想法是让车从任意一个站点出发，走一圈下来，看哪个站点的油量最低（允许负数），这样从这个站点出发就能保证全程油量不小于0。当然，如果总gas小于总cost，那就直接返回-1了。

后来发现一个一个计算求和太麻烦，只需要从某一个点出发，当油量小于零时，说明这是一个油量的低谷，于是取下一个点作为新的起点。

```python
def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        l = []
        for i in range(len(gas)):
            l.append(gas[i] - cost[i])
        if sum(l) < 0:
            return -1
        min_idx = 0
        sum_gas = 0
        for j in range(len(gas)):
            sum_gas += l[j]
            if sum_gas < 0:
                min_idx = j + 1
                sum_gas = 0
        return min_idx
```

后来发现不需要这个新列表l。

## 15. Candy
题目描述：有N个孩子站成一排。每个孩子都有一个评分。你需要按照以下要求，给这些孩子分发糖果：
1. 每个孩子至少分配到一个糖果。
2. 相邻的孩子中，评分较高的孩子必须获得更多的糖果。
你需要给孩子们分发糖果，计算并返回需要准备的最少糖果数。

我的想法是对于每一个孩子，只需要考虑他左侧的最长递增序列和右侧的最长递增序列，取两者的最大值就是这个孩子需要的糖果数。最后把所有孩子需要的糖果数加起来就是总数了。

```python
def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        left_up = [1]
        for i in range(1, n):
            if ratings[i] > ratings[i - 1]:
                left_up.append(left_up[i - 1] + 1)
                continue
            left_up.append(1)
        right_down = [1]
        for i in range(n-2, -1, -1):
            if ratings[i] > ratings[i + 1]:
                right_down.insert(0, right_down[0] + 1)
                continue
            right_down.insert(0, 1)
        candy_num = []
        for i in range(n):
            candy_num.append(max(left_up[i], right_down[i]))
        return sum(candy_num)
```

运行发现耗时很长，原来是因为insert函数的时间复杂度很高。可以采用预先创建一个列表，然后从后往前填充right_down列表来优化。

除此之外，还可以先给每个孩子分配一个糖果，然后从左到右遍历一次，如果当前孩子的评分比前一个孩子高，就把当前孩子的糖果数设置为前一个孩子的糖果数加1；然后从右到左遍历一次，如果当前孩子的评分比后一个孩子高，并且当前孩子的糖果数不大于后一个孩子的糖果数，就把当前孩子的糖果数设置为后一个孩子的糖果数加1。

这是我做的第一个Hard难度的题目，感觉难度并不大。

## 16. Trapping Rain Water
题目描述：给定一个非负整数数组height，其中每个元素代表一个柱子的高度，计算在这些柱子之间能够存储多少雨水。

这个题目我见过，对于每一个柱子上能积攒的水量，只需要考虑它左边的最高柱子和右边的最高柱子中较矮的那个，减去当前柱子的高度就是这个柱子上能积攒的水量。所以我写了两个列表，分别记录在i位置时左边最高柱子和右边最高柱子的高度。

```python
def trap(self, height: List[int]) -> int:
        n = len(height)
        left_max = [height[0]] * n
        for i in range(1, n):
            if height[i] > left_max[i - 1]:
                left_max[i] = height[i]
                continue
            left_max[i] = left_max[i - 1]
        right_max = [height[n-1]] * n
        for i in range(n-2, -1, -1):
            if height[i] > right_max[i + 1]:
                right_max[i] = height[i]
                continue
            right_max[i] = right_max[i + 1]
        water = [0] * n
        for i in range(1, n-1):
            water[i] = max(0, min(left_max[i], right_max[i]) - height[i])
        return sum(water)
```

这个算法效率较低，可以使用双指针来优化。使用两个指针分别指向数组的两端。

```python
def trap(self, height: List[int]) -> int:
        if not height:
            return 0
        left, right = 0, len(height) - 1
        left_max, right_max = height[left], height[right]
        water = 0

        while left < right:
            if left_max < right_max:
                left += 1
                left_max = max(left_max, height[left])
                water += max(0, left_max - height[left])
            else:
                right -= 1
                right_max = max(right_max, height[right])
                water += max(0, right_max - height[right])

        return water
```

这个算法两个指针从两头移动到中间，利用指针移动过程中最大值不断变大的特性可以稳定找到每一个柱子的积水量。

## 17. Roman to Integer
题目描述：给定一个罗马数字，将其转换为整数。输入确保在1到3999的范围内。

这个题目看起来很麻烦，后来想想只需要按照对应关系把每个罗马数字转换成对应的整数，唯一的特殊情况也可以用条件语句来处理。

```python
def romanToInt(self, s: str) -> int:
        m = 0
        s = s + 'o'
        for i in range(len(s)-1):
            if s[i] == 'M':
                m += 1000
            elif s[i] == 'D':
                m += 500
            elif s[i] == 'C':
                if s[i+1] == 'D' or s[i+1] == 'M':
                    m -= 100
                else:
                    m += 100
            elif s[i] == 'L':
                m += 50
            elif s[i] == 'X':
                if s[i+1] == 'L' or s[i+1] == 'C':
                    m -= 10
                else:
                    m += 10
            elif s[i] == 'V':
                m += 5
            elif s[i] == 'I':
                if s[i+1] == 'X' or s[i+1] == 'V':
                    m -= 1
                else:
                    m += 1
        return m
```
这里为了防止越界访问s[i+1]，在s的末尾添加了一个字符'o'，这个字符不会影响结果。

这个代码效率不高，可以使用一个字典来存储罗马数字和对应的整数，然后遍历字符串，根据当前字符和下一个字符的值来决定是加还是减。

```python
def romanToInt(self, s: str) -> int:
        roman_dict = {
            'I': 1,
            'V': 5,
            'X': 10,
            'L': 50,
            'C': 100,
            'D': 500,
            'M': 1000
        }
        total = 0
        for i in range(len(s) - 1):
            if roman_dict[s[i]] < roman_dict[s[i + 1]]:
                total -= roman_dict[s[i]]
            else:
                total += roman_dict[s[i]]
        total += roman_dict[s[-1]]
        return total
```

## 18. Integer to Roman
题目描述：给定一个整数，将其转换为罗马数字。输入确保在1到3999的范围内。

只需要写一个字典来存储整数和对应的罗马数字，然后按照从大到小的顺序遍历这个字典，依次将整数转换成罗马数字。

```python
def intToRoman(self, num: int) -> str:
        val_map = {
        1000: "M", 900: "CM", 500: "D", 400: "CD",
        100: "C", 90: "XC", 50: "L", 40: "XL",
        10: "X", 9: "IX", 5: "V", 4: "IV", 1: "I"
    }
    # 接下来的代码也必须保持在同一缩进层级
        res = ""
        for val in val_map:
            while num >= val:
                res += val_map[val]
                num -= val
        return res
```

## 19. Length of Last Word
题目描述：给定一个字符串s，返回其最后一个单词的长度。单词是指仅由字母组成、不包含任何空格的最大子字符串。

我的想法是找到最后一个单词之前的空格的位置，再找到最后一个单词结束的位置，然后计算它们之间的距离就是最后一个单词的长度了。

```python
def lengthOfLastWord(self, s: str) -> int:
        last_space = -1
        for i in range(len(s)-1):
            if s[i] == ' ' and s[i+1] != ' ':
                last_space = i
        end = len(s) - 1
        for i in range(len(s)-1, -1, -1):
            if s[i] != ' ':
                end = i
                break
        return end - last_space
```

这个代码效率不高，可以直接从字符串的末尾开始遍历，跳过末尾的空格，然后计算最后一个单词的长度。

## 20. Longest Common Prefix
题目描述：编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串""。

我的做法是记录第一个字符串，然后看看它和其他字符串的公共前缀是什么，更新最长公共前缀，最后返回即可。

```python
def longestCommonPrefix(self, strs: List[str]) -> str:
        first = strs[0]
        common = len(first)
        for word in strs:
            common = min(common, len(word))
            for i in range(len(word)):
                if i >= len(first) or word[i] != first[i]:
                    common = min(common, i)
                    break
        if common == 0:
            return ''
        return first[:common]
```

这个代码效率不高，可以使用zip函数来同时遍历所有字符串的字符，找到第一个不相同的字符位置，然后返回公共前缀。

```python
def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        prefix = ""
        for chars in zip(*strs):
            if len(set(chars)) == 1:
                prefix += chars[0]
            else:
                break
        return prefix
```

## 21. Reverse Words in a String
题目描述：给定一个字符串s，反转字符串中单词的顺序。单词是由非空格字符组成的字符串。s中至少存在一个单词。输入字符串s可能在前面、后面或者单词间包含多余的空格。返回单词顺序反转且单词之间仅用一个空格分隔的字符串。

我的想法是创建一个列表来存储单词，然后从字符串的末尾开始遍历，找到每个单词并添加到列表中，最后将列表中的单词用空格连接起来返回。

```python
def reverseWords(self, s: str) -> str:
        start = -1
        end = 0
        word_list = []
        for i in range(len(s)-1):
            if s[i] == ' ' and s[i+1] != ' ':
                start = i
            if s[i] != ' ' and s[i+1] == ' ':
                end = i
                word_list.append(s[start+1: end+1])
        if s[-1] != ' ':
            word_list.append(s[start+1: ])
        string = ''
        for i in range(len(word_list)-1, -1, -1):
            string = string + word_list[i] + ' '
        return(string[:len(string)-1])
```

这个代码效率不高，可以直接使用split函数来分割字符串，然后反转列表，最后用join函数连接起来。

```python
def reverseWords(self, s: str) -> str:
        words = s.split()
        return ' '.join(reversed(words))
```

我第一次见到函数split和join，split的作用是将字符串按照空格分割成一个列表，join的作用是将列表中的元素用空格连接成一个字符串。

## 22. ZigZag Conversion
题目描述：将一个给定字符串根据给定的行数，以从上往下、从左到右进行Z字形排列。

我创建了一个列表和两个变量，一个代表当前属于哪一行，一个代表当前的方向。

```python
def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s
        count = 0
        add = 1
        li = ['' for i in range(numRows)] 
        for char in s:
            li[count] += char
            if count == numRows - 1:
                add = -1
            elif count == 0:
                add = 1        
            count += add   
        return ''.join(li)
```

这个代码用到了上一题的join函数。

## 23. Find the Index of the First Occurrence in a String
题目描述：给定两个字符串needle和haystack，返回needle在haystack中第一次出现的索引，如果不存在，则返回-1。

这个题目我直接使用了内置的find函数来实现。

```python
def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```

如果要自己实现，我想到了一种O（nm）的算法，后来学到了一种O（n+m）的算法，叫做KMP算法。这个算法的核心思想是利用已经匹配过的部分来避免重复比较，从而提高效率。

## 24. Text Justification
题目描述：给定一个单词数组和一个长度maxWidth，重新排版单词，使得每行恰好有maxWidth个字符，并且左右两端都对齐。你应该使用尽可能多的单词填充每一行。单词之间应当使用至少一个空格分隔。额外的空格应该尽可能平均分配在单词之间。如果不能平均分配，左侧的空格数应该多于右侧的空格数。对于最后一行，单词应该左对齐，并且在末尾添加额外的空格使其达到maxWidth长度。

我的想法分三部分，第一部分是把单词按长度分成几组，保证每一组都是不超过maxWidth的最大长度；第二部分是写一个空格分配函数，根据单词数量和剩余空格数量来分配空格；第三部分是把每一组的单词和空格连接起来，最后返回一个列表。

```python
def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        li = [[]]
        row = 0
        
        for word in words:
            # 当前行已有的单词数是 len(li[row])
            # 如果加入这个单词，单词之间的空格数至少需要 len(li[row]) 个
            # 逻辑：当前单词总长度 + 新单词长度 + 单词间空格数
            current_word_len = sum(len(w) for w in li[row])
            if current_word_len + len(li[row]) + len(word) > maxWidth:
                # 换行
                li.append([word])
                row += 1
            else:
                # 同一行
                li[row].append(word)
        def space(space_num, que):
            if not space_num:
                return ["" for i in range(que)]
            if not que:
                return [' ' * space_num]
            n = space_num // que
            m = space_num % que
            n_space = ' ' * n
            N_space = ' ' * (n+1)
            lis = []
            for i in range(que):
                if i < m:
                    lis.append(N_space)
                else:
                    lis.append(n_space)
            return lis
        res = []
        for i, line in enumerate(li):
            line_len = sum(len(word) for word in line)
            
            # 情况 1：最后一行 或者 该行只有一个单词 (左对齐)
            if i == len(li) - 1 or len(line) == 1:
                s = " ".join(line)
                res.append(s + " " * (maxWidth - len(s)))
                
            # 情况 2：普通行 (左右对齐，使用你的 space 函数)
            else:
                total_space = maxWidth - line_len
                que = len(line) - 1
                spaces = space(total_space, que) # 调用你的 space 函数
                
                row_str = ""
                for j in range(len(line) - 1):
                    row_str += line[j] + spaces[j]
                row_str += line[-1]
                res.append(row_str)
                
        return res
```

第一部分和第三部分利用了ai的帮助。

后来ai告诉我可以用divmod函数来同时得到商和余数，这样就不需要分别计算n和m了。

## 25. Valid Palindrome
题目描述：给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

我的做法是先遍历一遍，删除所有非字母数字的字符，并把所有字母转换成小写，然后再判断这个字符串是否是回文串。

```python
def isPalindrome(self, s: str) -> bool:
        word = ''
        for char in s:
            if ord(char) >= 48 and ord(char) <= 57:
                word += char
            elif ord(char) >= 65 and ord(char) <= 90:
                word += chr(ord(char)+32)
            elif ord(char) >= 97 and ord(char) <= 122:
                word += char
        i = 0
        j = len(word) -1
        while i < j:
            if word[i] != word[j]:
                return False
            i += 1
            j -= 1
        return True
```

可以使用内置的isalnum函数来判断一个字符是否是字母或数字，使用lower函数来将字母转换成小写，这样代码会更简洁。第一部分也可以省略，直接使用双指针从两端开始遍历字符串，跳过非字母数字的字符，并比较字母的大小写。

```python
def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1
        while left < right:
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1
            if s[left].lower() != s[right].lower():
                return False
            left += 1
            right -= 1
        return True
```

## 26. Is Subsequence
题目描述：给定字符串s和t，判断s是否是t的子序列。一个字符串的子序列是指在不改变字符顺序的情况下，删除某些字符（也可以不删除）后形成的新字符串。

很简单，不写代码了。

## 27. Two Sum II - Input array is sorted
题目描述：给定一个已按照升序排列的整数数组numbers，找到两个数使它们相加之和等于目标数target。函数应该返回这两个下标值index1和index2，其中index1必须小于index2。注意：返回的下标值（index1和index2）不是从零开始的。

很简单，还是双指针。

## 28. Container With Most Water
题目描述：给定n个非负整数a1，a2，...，an，每个数代表坐标中的一个点(i, ai)。在坐标内画n条垂直线，垂直线i的两个端点分别为(i, ai)和(i, 0)。找出其中的两条线，使得它们与x轴共同构成的容器可以容纳最多的水。

一开始，我想到，一个数可以增加至它左边和右边最高值的较小值，此时不会影响答案。于是，经过这样操作后，数组变成一个先递增后递减的数组。此时按照不同高度分组，考察每一个高度下的最大宽度，全部记录下来，最后取最大值就是答案了。后来我简化了这个过程，从两端开始，先记录两端形成的容器积水量，然后移动较短的一端，更新最大值，直到两端相遇。这样可以记录到所有的容积极大值，最大值也在其中。

我又简化了整个流程，不需要把数组变成一个先递增后递减的数组了，直接使用双指针从两端开始，移动较短的一端，更新最大值，直到两端相遇。

```python
def maxArea(self, height: List[int]) -> int:
        front = 0
        rear = len(height)-1
        max_area = 0
        while front < rear:
            max_area = max(max_area, min(height[front], height[rear])* (rear - front))
            if height[front] > height[rear]:
                rear -= 1
            else:
                front += 1
        return max_area
```

## 29. 3Sum
题目描述：给定一个包含n个整数的数组nums，判断nums中是否存在三个元素a，b，c，使得a + b + c = 0？找出所有满足条件且不重复的三元组。

我一开始的想法是先把数组变成一个哈希表，然后遍历数组两遍，看看是否存在一个数等于-(a+b)，如果存在就把这个三元组加入结果中。后来发现这样会有重复的三元组，所以需要对结果进行去重。问ai得到一个算法，先对数组排序，然后开始遍历，寻找以这个元素为最小值的三元组，使用双指针来寻找剩下的两个数，在这个过程中如果当前元素已经大于0，直接结束，当前元素和前一个相同时，直接跳过。后面利用while函数来跳过重复的元素。

不写代码了。

## 30. Minimum Size Subarray Sum
题目描述：给定一个含有n个正整数的数组和一个正整数s，找出该数组中满足其和≥s的长度最小的连续子数组，并返回其长度。如果不存在符合条件的连续子数组，返回0。

我的方法是先从头开始加，加到target时开始移动这一块，每次从后面加一个，然后前面减若干个，直到不满足条件了，再从后面加一个，如此循环，直到后面加到数组末尾了。最后返回最短的长度。

中途遇到很多次越界访问的问题，通过ai解决。

## 31. Longest Substring Without Repeating Characters
题目描述：给定一个字符串，请你找出其中不含有重复字符的最长子串的长度。

本题的题目用词不准确，repeating改成duplicate更合适。我的做法是创建一个列表来存储当前的子串，然后遍历字符串，如果当前字符不在列表中，就把它加入列表；如果当前字符在列表中，就把它之前的部分删除掉，直到这个字符不在列表中了，然后再把这个字符加入列表。每次更新最长长度。这个算法需要多次删除，时间复杂度很低，后来知道可以用一个哈希表来记录每个字符最后一次出现的位置，同样用左指针和右指针，每次只需要检测当前字符是否在哈希表中，并且它的最后一次出现位置是否在当前窗口内，如果是，就把左指针移动到这个位置的下一个位置，然后更新哈希表中这个字符的位置，最后更新最长长度。

```python
def lengthOfLongestSubstring(self, s: str) -> int:
        left = 0
        n = len(s)
        length = 0
        current_str = {}
        for right in range(n):
            if s[right] in current_str and current_str[s[right]] >= left:
                left = current_str[s[right]] + 1
            current_str[s[right]] = right
            length = max(length, right - left +1)
        return length
```

## 32. Substring with Concatenation of All Words
题目描述：给定一个字符串s和一些长度相同的单词words，找出s中恰好可以由words中所有单词串联形成的子串的起始位置。注意子串要与words中的单词完全匹配，中间不能有其他字符，但不需要考虑words中单词串联的顺序。

这个题目很难，我自己写了一个窗口每次只滑动一个字符的算法，效率很低，后来发现可以每次滑动n个字符，n是word的长度，同时从0，1，...，n-1这n个位置开始滑动，这样就能保证每次滑动的都是一个完整的单词了。这个算法的优势是可以根据滑动中新的词在不在words中而直接调整窗口的大小，而不需要每次都检查整个窗口中的单词是否满足条件了。我直接附上ai的代码。

```python
from collections import Counter

class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        if not s or not words: return []
        
        n, m = len(words[0]), len(words)
        total_len = n * m
        word_count = Counter(words)
        res = []
        
        # 遍历所有可能的起始偏移量 (0 到 n-1)
        for i in range(n):
            left = i
            right = i
            current_count = Counter()
            
            while right + n <= len(s):
                # 1. 提取当前单词
                word = s[right : right + n]
                right += n
                
                if word in word_count:
                    current_count[word] += 1
                    
                    # 2. 如果当前单词多了，从左侧不断移除，直到频率恢复
                    while current_count[word] > word_count[word]:
                        left_word = s[left : left + n]
                        current_count[left_word] -= 1
                        left += n
                    
                    # 3. 检查窗口是否正好包含所有单词
                    if right - left == total_len:
                        res.append(left)
                else:
                    # 4. 如果单词根本不在 words 里，整个窗口作废
                    current_count.clear()
                    left = right
                    
        return res
```

## 33. Minimum Window Substring
题目描述：给定一个字符串S和一个字符串T，找到S中包含T所有字符的最小窗口。如果S中不存在这样的窗口，返回空字符串""。如果存在这样的窗口，我们保证它是唯一的。

我参照上一题的做法，写出来一个很臃肿的算法，答案也不对，ai告诉我可以创建一个need字典来记录t中需要的字符和它们的数量，根据窗口里的字符来更新这个need字典，当need字典中的所有值都小于等于0时，说明当前窗口包含了t中的所有字符了，这时就可以尝试缩小窗口了。每次缩小窗口时，如果当前窗口仍然满足条件，就继续缩小；如果不满足条件了，就停止缩小，记录当前窗口的长度和起始位置。最后返回最短的窗口。

这里有一个很有趣的技巧，就是无论当前字符在不在t中，都在need中减去1，这样无论如何这个计数都是负的，后面只需要考虑当前字符的need=0时停止缩小窗口就好了，因为不在t中的字符不影响窗口是否满足条件了。

## 34. Valid Sudoku
题目描述：判断一个9x9的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。数字1-9在每一行只能出现一次。数字1-9在每一列只能出现一次。数字1-9在每一个以粗实线分隔的3x3宫内只能出现一次。

我写的代码检查了三遍矩阵，第一遍检查行，第二遍检查列，第三遍检查3x3的宫。后来ai告诉我可以只检查一遍矩阵，在检查每个元素时，同时检查它所在的行、列和3x3的宫是否已经出现过这个数字了，如果出现过了就返回False，否则就把这个数字记录下来。这样就能在一次遍历中完成所有的检查了。

## 35. Spiral Matrix
题目描述：给定一个包含m x n个元素的矩阵（m行，n列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

这个题目很经典，按照四个方向来遍历就可以。

## 36. Rotate Matrix
题目描述：给定一个n x n的二维矩阵matrix表示一个图像。请你将图像顺时针旋转90度。你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

我使用两次翻转来实现旋转，第一次翻转是沿着主对角线翻转，第二次翻转是沿着水平中线翻转。这样就能实现顺时针旋转90度了。

## 37. Set Matrix Zeroes
题目描述：给定一个m x n的矩阵，如果一个元素为0，则将其所在行和列的所有元素都设为0。请使用原地算法。

我参照数独题的算法，使用一个标志矩阵来记录哪些行和列需要被置零，然后再根据这个标志矩阵来修改原矩阵。后来ai告诉我可以使用第一行和第一列来存储这个标志，这样就不需要额外的空间了。首先检查第一行和第一列是否有0，如果有的话就记录下来；然后遍历剩下的矩阵，如果遇到0，就把对应的第一行和第一列的位置也置为0；最后根据第一行和第一列的标志来置零剩下的矩阵，最后再根据之前记录的标志来置零第一行和第一列。

## 38. Life Game
题目描述：生命游戏

这个问题我没有头绪，后来ai告诉我可以使用一个临时变量来记录每个细胞的状态变化，0表示原来是死的，现在还是死的；1表示原来是活的，现在还是活的；2表示原来是活的，现在变成死的；3表示原来是死的，现在变成活的。这样在遍历矩阵时就可以根据当前细胞周围八个细胞的状态来更新这个临时变量了。最后再遍历一次矩阵，根据这个临时变量来更新细胞的最终状态。

## 39. Ransom Note
题目描述：给定一个赎金信（ransom）字符串和一个杂志（magazine）字符串，判断赎金信能不能由杂志里面的字符构成。如果可以构成，返回true；否则返回false。杂志字符串中的每个字符只能在赎金信字符串中使用一次。

好简单，创建一个字典来记录杂志中字符的数量就行。

## 40. Isomorphic Strings
题目描述：给定两个字符串s和t，判断它们是否是同构的。如果s中的字符可以被替换得到t，那么这两个字符串是同构的。所有出现的字符都必须用另一个字符替换，同时保留字符的顺序。两个字符不能映射到同一个字符上，但字符可以映射到自己上。

我创建了两个字典来记录s和t中字符的映射关系，然后遍历字符串，检查每个字符的映射关系是否一致，如果不一致就返回False，最后返回True。

```python
def isIsomorphic(self, s: str, t: str) -> bool:
        char_map = {}
        for i in range(len(s)):
            if s[i] not in char_map:
                if t[i] in char_map.values():
                    return False
                char_map[s[i]] = t[i]
            else:
                if char_map[s[i]] != t[i]:
                    return False
        return True
```

## 41. Word Pattern
题目描述：给定一种规律pattern和一个字符串str，判断str是否遵循相同的规律。这里的规律指的是每个字符在pattern中对应一个单词在str中，并且这个对应关系是双向的，也就是说pattern中的不同字符应该对应str中的不同单词。

利用split函数就行。

## 42. Valid Anagram
题目描述：给定两个字符串s和t，编写一个函数来判断t是否是s的字母异位词。字母异位词指的是两个字符串中每个字符出现的次数都相同，但字符的顺序可以不同。

利用Counter，一行就行。

## 43. Group Anagrams
题目描述：给定一个字符串数组，将字母异位词组合在一起。字母异位词指的是字母相同，但排列不同的字符串。可以按任意顺序返回结果列表。

我原本打算用Counter来统计每个字符串中字符的数量，然后把这些统计结果作为键来分组，但是Counter对象不能作为字典的键，所以我用sorted函数来把字符串中的字符排序，然后把排序后的字符串作为键来分组。

```python
def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        char_map = {}
        for word in strs:
            key = "".join(sorted(word))
            if key not in char_map:
                char_map[key] = []
            char_map[key].append(word)
        return list(char_map.values())
```

# 我的copilot过期了，接下来不写问题描述了。

## 44. Two Sum
我只能想到先sort再左右指针的做法，时间复杂度是O(nlogn)，ai告诉我可以先存下所有数在哈希表里，然后再遍历一遍看每个数和目标的差在不在表里。

## 45. Happy Number
很简单，只需要写一个函数来把一个数变成它下一个数，使用先变成字符串，然后变成list，再求和就行。用一个字典记录出现过的数，如果到1或者重复就退出。


