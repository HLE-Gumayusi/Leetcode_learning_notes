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


              