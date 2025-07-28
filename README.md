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



-----

## 121\. Best Time to Buy and Sell Stock

[Problem Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Intuition & Approach

The goal is to find the maximum profit from a single buy and sell operation. This can be solved efficiently in a single pass using a **two-pointer (sliding window)** approach.

We use a **left pointer (`l`)** to track the **best day to buy** (the lowest price seen so far) and a **right pointer (`r`)** to iterate through the days, representing the **potential day to sell**.

1.  **Initialization**: Start with the `left` pointer at day 0 and the `right` pointer at day 1. Initialize `max_profit` to 0.

2.  **Iteration**: Move the `right` pointer through the array from left to right.

      * **Profitable Case (`prices[r] > prices[l]`)**: If the current price is higher than the buy price, we calculate the potential `profit`. We update `max_profit` if this new profit is higher than any we've seen before.
      * **New Low Price Case (`prices[r] <= prices[l]`)**: If the current price is lower than our recorded buy price, it's impossible to make a profit with the old buy day. We've found a better (cheaper) day to buy. So, we **update the `left` pointer** to the current position (`l = r`). This effectively slides our buying window forward to a more advantageous starting point.

3.  **Conclusion**: By the end of the loop, `max_profit` will hold the highest possible profit from one transaction.

### Walkthrough

Let's trace `prices = [7, 1, 5, 3, 6, 4]`:

| `l` | `r` | `prices[l]` | `prices[r]` | Condition | `profit` | `maxP` | Action |
| :-: | :-: | :---------: | :---------: | :-----------------------------: | :------: | :----: | :---------------------------------- |
| 0 | 1 | 7 | 1 | `7 >= 1` (New Low) | - | 0 | Found a new low price. `l` becomes `1`. |
| 1 | 2 | 1 | 5 | `1 < 5` (Profit\!) | 4 | 4 | Update `maxP`. |
| 1 | 3 | 1 | 3 | `1 < 3` (Profit\!) | 2 | 4 | Not \> `maxP`. |
| 1 | 4 | 1 | 6 | `1 < 6` (Profit\!) | 5 | 5 | Update `maxP`. |
| 1 | 5 | 1 | 4 | `1 < 4` (Profit\!) | 3 | 5 | Not \> `maxP`. |

Final returned profit is **5**.

### Code

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        l, r = 0, 1  # l=buy, r=sell
        maxP = 0

        while r < len(prices):
            # Check for a profitable transaction
            if prices[l] < prices[r]:
                profit = prices[r] - prices[l]
                maxP = max(maxP, profit)
            else:
                # Found a new low price, update our buy day
                l = r
            
            # Move sell pointer to the next day
            r += 1
            
        return maxP
```

### Complexity Analysis

  * **Time Complexity**: $O(n)$, since we iterate through the `prices` array only once.
  * **Space Complexity**: $O(1)$, as we only use a few variables (`l`, `r`, `maxP`) to store state.

-----


## 217\. Contains Duplicate

[Problem Link](https://leetcode.com/problems/contains-duplicate/)

### Intuition & Approach

The problem asks us to determine if any element in an array appears more than once. The most efficient way to solve this is by using a data structure that allows for fast lookups. A **hash set** is perfect for this task.

The logic is straightforward:

1.  Initialize an empty **hash set** to keep track of the numbers we have seen so far.
2.  Iterate through each number in the input array `nums`.
3.  For each number, check if it's already present in the hash set.
      * If it is, we've found a duplicate\! We can immediately stop and return `True`.
      * If it's not in the set, we add it so we can check against it later.
4.  If the loop completes without finding any duplicates, it means every element was unique. We can then return `False`.

This approach is efficient because adding an element to a hash set and checking for its existence are, on average, $O(1)$ operations.

### Code

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashset = set()
        
        for n in nums:
            if n in hashset:
                return True
            hashset.add(n)
            
        return False
```

### Complexity Analysis

  * **Time Complexity**: $O(n)$, where `n` is the number of elements in `nums`. We iterate through the array once, and each hash set operation takes constant time on average.
  * **Space Complexity**: $O(n)$, because in the worst case (where there are no duplicates), the hash set will store all `n` elements from the input array.


-----

## 238\. Product of Array Except Self

[Problem Link](https://leetcode.com/problems/product-of-array-except-self/)

### Intuition & Approach

The challenge is to compute the product of all elements in an array *except* for the element at the current index, and to do so **without using division**.

The key insight is that the result for any given index `i` is the product of two values:

1.  The product of all numbers **to the left** of `i` (prefix product).
2.  The product of all numbers **to the right** of `i` (postfix product).

We can solve this efficiently in two passes:

  * **Pass 1 (Left to Right):** We iterate through the array from the beginning. For each position `i`, we first place the product of all elements to its left into our `result` array. We maintain a `prefix` variable that accumulates this product as we go.

  * **Pass 2 (Right to Left):** We iterate through the array from the end. This time, we maintain a `postfix` variable for the product of elements to the right. For each position `i`, we multiply the existing value in `result[i]` (which is already the prefix product) by the current `postfix` product. This gives us the final desired result.

This two-pass approach calculates the products in $O(n)$ time and satisfies the $O(1)$ extra space constraint, as the output array is not counted towards space complexity.

### Walkthrough

Let's trace `nums = [1, 2, 3, 4]`:

1.  **Initialize `result`**: `result = [1, 1, 1, 1]`

2.  **First Pass (Prefix Calculation)**:

      * `i = 0`: `result[0] = 1`. `prefix` becomes `1 * 1 = 1`. `result` is `[1, 1, 1, 1]`.
      * `i = 1`: `result[1] = 1`. `prefix` becomes `1 * 2 = 2`. `result` is `[1, 1, 1, 1]`.
      * `i = 2`: `result[2] = 2`. `prefix` becomes `2 * 3 = 6`. `result` is `[1, 1, 2, 1]`.
      * `i = 3`: `result[3] = 6`. `prefix` becomes `6 * 4 = 24`. `result` is `[1, 1, 2, 6]`.
      * After the first pass, `result[i]` holds the product of elements to its left.

3.  **Second Pass (Postfix Calculation)**:

      * `i = 3`: `result[3] *= 1` (initial `postfix`). `result[3]` is `6`. `postfix` becomes `1 * 4 = 4`. `result` is `[1, 1, 2, 6]`.
      * `i = 2`: `result[2] *= 4`. `result[2]` is `2 * 4 = 8`. `postfix` becomes `4 * 3 = 12`. `result` is `[1, 1, 8, 6]`.
      * `i = 1`: `result[1] *= 12`. `result[1]` is `1 * 12 = 12`. `postfix` becomes `12 * 2 = 24`. `result` is `[1, 12, 8, 6]`.
      * `i = 0`: `result[0] *= 24`. `result[0]` is `1 * 24 = 24`. `postfix` becomes `24 * 1 = 24`. `result` is `[24, 12, 8, 6]`.

The final `result` is `[24, 12, 8, 6]`.

### Code

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        result = [1] * len(nums)
        
        # First pass: Calculate prefix products
        prefix = 1
        for i in range(len(nums)):
            result[i] = prefix
            prefix *= nums[i]
            
        # Second pass: Calculate postfix products and multiply
        postfix = 1
        for i in range(len(nums) - 1, -1, -1):
            result[i] *= postfix
            postfix *= nums[i]
            
        return result
```

### Complexity Analysis

  * **Time Complexity**: $O(n)$, since we iterate through the array twice, which simplifies from $O(2n)$ to $O(n)$.
  * **Space Complexity**: $O(1)$, because we don't use any extra space other than the output array, as per the problem's constraints.

-----

## 53\. Maximum Subarray

[Problem Link](https://leetcode.com/problems/maximum-subarray/)

### Intuition & Approach

This problem asks for the largest sum of any contiguous subarray within a given array. The classic and most efficient solution for this is **Kadane's Algorithm**.

The core idea is to iterate through the array, keeping track of the sum of the current subarray. The crucial insight is that if the sum of the current subarray (`curSum`) ever becomes negative, it's better to discard it and start a new subarray from the next element. A negative-sum subarray will only decrease the sum of any future elements added to it.

1.  **Initialization**: We need two variables:

      * `maxSub`: To store the maximum subarray sum found so far. We initialize it with the first element of the array to handle cases where all numbers are negative.
      * `curSum`: The sum of the current subarray we are considering. We initialize it to `0`.

2.  **Iteration**: We loop through each number `n` in the array.

      * **Check for Negative Sum**: Before adding the new number, we check if `curSum` is negative. If it is, we reset `curSum` to `0`. This is the "discard" step.
      * **Add Current Element**: We add the current number `n` to `curSum`.
      * **Update Maximum**: We compare the `curSum` with `maxSub` and update `maxSub` if the current sum is larger.

By the end of the loop, `maxSub` will hold the largest sum found among all possible contiguous subarrays.

### Walkthrough

Let's trace `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`:

| `n` | `curSum` (start) | Action (`if curSum < 0`) | `curSum` (after `+= n`) | `maxSub` |
| :---: | :---: | :--- | :---: | :---: |
| | | | | `-2` |
| `-2`| `0` | - | `-2` | `-2` (max(-2,-2)) |
| `1` | `-2`| `curSum = 0` | `1` | `1` (max(-2,1)) |
| `-3`| `1` | - | `-2` | `1` (max(1,-2)) |
| `4` | `-2`| `curSum = 0` | `4` | `4` (max(1,4)) |
| `-1`| `4` | - | `3` | `4` (max(4,3)) |
| `2` | `3` | - | `5` | `5` (max(4,5)) |
| `1` | `5` | - | `6` | `6` (max(5,6)) |
| `-5`| `6` | - | `1` | `6` (max(6,1)) |
| `4` | `1` | - | `5` | `6` (max(6,5)) |

The final returned profit is **6** (from the subarray `[4, -1, 2, 1]`).

### Code

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        maxSub = nums[0]
        curSum = 0

        for n in nums:
            # If the current sum is negative, it's better to start a new subarray
            if curSum < 0:
                curSum = 0
            curSum += n
            maxSub = max(maxSub, curSum)
            
        return maxSub
```

### Complexity Analysis

  * **Time Complexity**: $O(n)$, since we perform a single pass through the array.
  * **Space Complexity**: $O(1)$, as we only use a constant amount of extra space for the variables `maxSub` and `curSum`.

-----
