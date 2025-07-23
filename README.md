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


Here is the explanation in a GitHub README format, perfect for a collection of LeetCode solutions.

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
