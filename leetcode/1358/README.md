# [1358. Number of Substrings Containing All Three Characters (Medium)](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/)

<p>Given a string <code>s</code>&nbsp;consisting only of characters <em>a</em>, <em>b</em> and <em>c</em>.</p>

<p>Return the number of substrings containing <b>at least</b>&nbsp;one occurrence of all these characters <em>a</em>, <em>b</em> and <em>c</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "abcabc"
<strong>Output:</strong> 10
<strong>Explanation:</strong> The substrings containing&nbsp;at least&nbsp;one occurrence of the characters&nbsp;<em>a</em>,&nbsp;<em>b</em>&nbsp;and&nbsp;<em>c are "</em>abc<em>", "</em>abca<em>", "</em>abcab<em>", "</em>abcabc<em>", "</em>bca<em>", "</em>bcab<em>", "</em>bcabc<em>", "</em>cab<em>", "</em>cabc<em>" </em>and<em> "</em>abc<em>" </em>(<strong>again</strong>)<em>. </em>
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "aaacb"
<strong>Output:</strong> 3
<strong>Explanation:</strong> The substrings containing&nbsp;at least&nbsp;one occurrence of the characters&nbsp;<em>a</em>,&nbsp;<em>b</em>&nbsp;and&nbsp;<em>c are "</em>aaacb<em>", "</em>aacb<em>" </em>and<em> "</em>acb<em>".</em><em> </em>
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "abc"
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 5 x 10^4</code></li>
	<li><code>s</code>&nbsp;only consists of&nbsp;<em>a</em>, <em>b</em> or <em>c&nbsp;</em>characters.</li>
</ul>

**Related Topics**:  
[String](https://leetcode.com/tag/string/)

## Solution 1. Sliding Window

For every time, we come across a valid sub string, we add n-j (n-1 - j + 1 => n-j)
It is because for a fixed L, we have n-j substrings. It is L dependent
As we increase L by one each time

```cpp
// 1st : we add count before moving l : this is there in inside while loop during shrinking
// 2nd : we add count before moving r : this is there at the end of block

class Solution {
public:
    int numberOfSubstrings(string s) {
        int n = s.size(), l = 0, ans = 0;
        vector<int> cnt(3, 0);

        for (int r = 0; r < n; r++) {
            cnt[s[r] - 'a']++;  // expand window

            while (cnt[0] && cnt[1] && cnt[2]) {
                ans += (n - r); // all extensions [l..r] to [l..n-1] are valid
                cnt[s[l] - 'a']--;
                l++;            // shrink window
            }
        }

        return ans;
    }
};
/// ____________________________
```
```cpp
// 2nd : we add count before moving r : this is there at the end of block

class Solution {
public:
    int numberOfSubstrings(string s) {
        int n = s.size(), l = 0, ans = 0;
        vector<int> cnt(3, 0);

        for (int r = 0; r < n; r++) {
            cnt[s[r] - 'a']++;  // expand window

            // shrink while window contains a, b, c
            while (cnt[0] && cnt[1] && cnt[2]) {
                cnt[s[l] - 'a']--;
                l++;
            }

            // valid starts: 0 ... l-1 for this fixed r
            ans += l;
        }

        return ans;
    }
};
```

## Solution 2. DP

We store the last index where we have the all three characters available
We do 	1 + min(last[0], min(last[1], last[2]));   to get the strings from L to 0 (L+1 will be there) having a fixed R each time
We add these to count

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int n = s.length();
        int count = 0;
        int last[3] = {-1, -1, -1};
        for(int i=0;i<n;i++){
            last[s[i]-'a'] = i;
            if(last[0]!=-1 && last[1]!=-1 && last[2]!=-1) {
                count += 1 + min(last[0], min(last[1], last[2]));
            }
        }
        return count;
    }
};
```
