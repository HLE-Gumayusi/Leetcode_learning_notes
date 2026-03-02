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

这个代码虽然能通过测试用例，但是效率很低。后来问ai才知道其实可以使用一个变量来记录当前能够跳跃的最远位置和下一个能够跳跃的最远位置，如果当前索引超过了当前能够跳跃的最远位置，就说明需要增加一次跳跃了。

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