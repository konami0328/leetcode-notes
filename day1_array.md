# 704. Binary Search
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

# 27. Remove Element
