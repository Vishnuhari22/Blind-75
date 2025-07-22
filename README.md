# Blind 75 - LeetCode Solutions

This repository contains my solutions to the [Blind 75](https://www.google.com/search?q=https://www.teamblind.com/post/new-year-gift-to-everybody-some-leetcode-problem-patterns-for-you-e5oK32mP) list of LeetCode problems. The goal is to master these fundamental questions for technical interviews.

-----

## 1\. Two Sum

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.

**Example:**

**Input:** `nums = [2,7,11,15]`, `target = 9`
**Output:** `[0,1]`
**Explanation:** Because `nums[0] + nums[1] == 9`, we return `[0, 1]`.

### Approach 🧠

This problem can be solved efficiently in a single pass using a hash map (a dictionary in Python).

The idea is to iterate through the `nums` array. For each number `num`, we calculate its `complement` (`target - num`). We then check if this `complement` already exists as a key in our hash map `seen`.

  * If the **complement exists**, it means we have found the two numbers that add up to the target. We can immediately return the index of the complement (stored as the value in our map) and the index of the current number.
  * If the **complement does not exist**, we add the current number `num` as a key and its index `i` as its value to the `seen` map. This prepares us for future iterations.

This one-pass approach avoids the nested loops of a brute-force solution, significantly improving performance.

### Code

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # To store numbers we've seen and their indices
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [seen[complement], i]
            seen[num] = i
```

### Complexity Analysis

  * **Time Complexity:** $O(n)$
    We iterate through the list containing $n$ elements only once. Each lookup in the hash map (`seen`) takes, on average, $O(1)$ time.

  * **Space Complexity:** $O(n)$
    In the worst-case scenario, we might have to store all $n$ elements in the hash map `seen` before finding the pair.
