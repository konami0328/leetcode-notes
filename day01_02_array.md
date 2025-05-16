# 704. 二分查找
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        while left <= right:  # mention
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                return mid
        return -1
```
## why left <= right?
left <= right: [left, right] searching space  
left < right: [left, right) searching space  
left == right means: we are examining a single element at that index, which cannot be missed. 

# 27. 移除元素 (REVIEW, two-pointer)
## exhaustive solution
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        k = len(nums)
        while i < k:
            if nums[i] == val:
                k -= 1
                for j in range(i, k):
                    nums[j] = nums[j + 1]
            else:
                i += 1
        return k
```
## two-pointer technique (https://www.youtube.com/watch?v=pGKDzt0gk-A&ab_channel=AlgoEngine)
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        for x in nums:
            if x != val:
                nums[i] = x  # if x is a valid value, overwrite place i with x, then move i to next position; 
                             # if not, let i stay there (means this place stores a bad value, later needs to be overwritten with a valid value)
                i += 1
        return i
```
## what does x and i mean?
x: the current element we are checking.  
i: overwrite pointer, it keeps track of where to place the next valid element. can think of i as the index of the updated list.

# 977. 有序数组的平方 (two-pointer)
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        i = 0
        j = len(nums) - 1
        result = list(range(len(nums)))
        k = len(nums)
        while i <= j:
            k -= 1
            if (nums[i]) ** 2 >= (nums[j]) ** 2:
                result[k] = (nums[i]) ** 2
                i += 1
            else:
                result[k] = (nums[j]) ** 2
                j -= 1
        return result
```

# 209. 长度最小的子数组 (REVIEW)
## exhaustive solution
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        n = len(nums)
        min_len = n + 1

        for i in range(n):
            current_sum = 0
            for j in range(i, n):
                current_sum += nums[j]
                if current_sum >= target:
                    min_len = min(min_len, j - i + 1)
                    break

        return 0 if min_len == n + 1 else min_len
```
## two-pointer, sliding window
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        l = len(nums)
        result = l + 1
        cur_sum = 0
        i = 0
        for j in range(l):
            cur_sum += nums[j]
            while cur_sum >= target:  # key part 
                result = min(result, j-i+1)
                cur_sum -= nums[i]
                i += 1
        return 0 if result == l+1 else result
```

```python
class Solution:  # 结果正确，但时间复杂度 O(N^2)?
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        l = len(nums)
        result = l + 1
        cur_sum = 0
        i = 0
        for j in range(l):
            cur_sum += nums[j]
            for i in range(i, j+1):  # for+if+break, 会重复遍历部分区间
                if cur_sum >= target:
                    cur_sum -= nums[i]
                    result = min(result, j-i+1)
                else:
                    break
        return 0 if result == l+1 else result
```
## 滑动窗口思路：
nums = [2, 3, 1, 2, 4, 3]  
        ↑              ↑  
      left           right  

窗口不断扩展：  
1. right 向右移，把 nums[right] 加进窗口；  
2. 一旦当前窗口和 ≥ target，就试着右移 left；  
3. 每次尝试都用当前区间更新最短长度。  

窗口移动过程示意（right从0到5）：  

初始状态：  
[2]                         → sum = 2 < 7  

[2, 3]                      → sum = 5 < 7  

[2, 3, 1]                   → sum = 6 < 7  

[2, 3, 1, 2]                → sum = 8 ≥ 7 → 收缩：  
   [3, 1, 2]                → sum = 6     → 不满足，停止收缩  

[3, 1, 2, 4]                → sum = 10 ≥ 7 → 收缩：  
   [1, 2, 4] → sum = 8  
      [2, 4] → sum = 6 → 不满足，停止  

[2, 4, 3]                   → sum = 13 ≥ 7 → 收缩：  
   [4, 3] → sum = 7 → 收缩  
      [3] → sum = 3 → 停止  

| 方法   | 指针方向             | 每对(i, j) | 冗余计算 | 复杂度     |
| ---- | ---------------- | -------- | ---- | ------- |
| 暴力   | i 从左到右，j 从 i 到末尾 | 每个子数组都尝试 | 是    | `O(n²)` |
| 滑动窗口 | right 扩张，left 收缩 | 尽量少的尝试   | 否    | `O(n)`  |  

# 暴力解法和滑动窗口双指针的总结
暴力是“固定左边，向右扩展”  
双指针是“右边扩展，左边收缩”（动态窗口）  

什么时候可以把双重for循环优化为双指针？  
1. 涉及连续区间（子数组、子串）的问题  
    适用：最小长度的子数组使得和 ≥ target; 最长的不含重复字符的子串; 所有和等于 k 的子数组  
    不适用：任意两个数是否和为 target（不连续）  
2. 数组中元素为非负数（滑动窗口成立的前提之一）  
    很多滑动窗口问题依赖“区间总和随右边增加只会增大，不会减少”。这只有在数组中的元素都是非负数时才成立。如果有负数，那么右边扩大，区间总和可能变小，你就无法保证左边收缩不会丢失可能的最优解。  

理解“右边扩展，左边收缩” ：  
    右指针不断向右扩展 → 把新元素纳入窗口  
    左指针按需收缩窗口 → 去掉多余元素，保证当前窗口仍满足题意  

| 问题类型              | 模式         | 示例           |
| ----------------- | ---------- | ------------ |
| 最短子数组和 ≥ `target` | 满足后收缩左边    | Leetcode 209 |
| 最长不含重复字符的子串       | 重复时收缩左边    | Leetcode 3   |
| 最多 K 个不同字符的最长子串   | 超过 K 时收缩左边 | Leetcode 340 |
| 所有字母异位词的起始索引      | 窗口固定，滑动    | Leetcode 438 |



