# [1248. Count Number of Nice Subarrays (Medium)](https://leetcode.com/problems/count-number-of-nice-subarrays/)

<p>Given an array of integers <code>nums</code> and an integer <code>k</code>. A continuous subarray is called <strong>nice</strong> if there are <code>k</code> odd numbers on it.</p>

<p>Return <em>the number of <strong>nice</strong> sub-arrays</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,1,2,1,1], k = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,4,6], k = 1
<strong>Output:</strong> 0
<strong>Explanation:</strong> There is no odd numbers in the array.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [2,2,2,1,2,2,1,2,2,2], k = 2
<strong>Output:</strong> 16
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10^5</code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

**Companies**:  
[Booking.com](https://leetcode.com/company/bookingcom), [Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Math](https://leetcode.com/tag/math/), [Sliding Window](https://leetcode.com/tag/sliding-window/)


## Solution 1. AtMost to Equal + Find Maximum Sliding Window

Check out "[C++ Maximum Sliding Window Cheatsheet Template!](https://leetcode.com/problems/frequency-of-the-most-frequent-element/discuss/1175088/C%2B%2B-Maximum-Sliding-Window-Cheatsheet-Template!)"

Exactly `k` times = At Most `k` times - At Most `k - 1` times.

```cpp
class Solution {
public:

    int atMostK(vector<int>& nums, int k) {
        int left = 0, right = 0;
        int ans = 0;

        while (right < nums.size()) {
            if (nums[right] % 2) k--;
            right++;

            while (k < 0) {
                if (nums[left] % 2) k++;
                left++;
            }

            ans += right - left;
        }

        return ans;
    }

    int numberOfSubarrays(vector<int>& nums, int k) {
        return atMostK(nums, k) - atMostK(nums, k - 1);
    }
};
```
## Solution 2. Prefix State Map

```cpp

// mp[sum-k] means there are those many prefix sum possible from o to l, then l+1 to current
// have the sum value as k.

class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        mp[0] = 1;

        int sum = 0, ans = 0;

        for (int x : nums) {
            sum += (x % 2);

            if (mp.count(sum - k)) ans += mp[sum - k];
            mp[sum]++;
        }
        return ans;
    }
};
```

## Discuss

https://leetcode.com/problems/count-number-of-nice-subarrays/discuss/1515501/C%2B%2B-Prefix-State-Map-Two-Pointers-Sliding-Window
