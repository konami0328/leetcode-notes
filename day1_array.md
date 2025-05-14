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



