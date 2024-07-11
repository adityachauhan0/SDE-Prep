## Longest Substring w/o repeating characters
https://leetcode.com/problems/longest-substring-without-repeating-characters/
Given a string `s`, find the length of the **longest**

**substring**

without repeating characters.

**Example 1:**

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

**Example 2:**

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

**Example 3:**

**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.length();
        int maxLength = 0;
        vector<int> charIndex(128, -1);
        int left = 0;
        
        for (int right = 0; right < n; right++) {
            if (charIndex[s[right]] >= left) {
                left = charIndex[s[right]] + 1;
            }
            charIndex[s[right]] = right;
            maxLength = max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
};
```

## Max Consecutive Ones III
https://leetcode.com/problems/max-consecutive-ones-iii
Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.

**Example 1:**

**Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
**Output:** 6
**Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

**Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
**Output:** 10
**Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

```cpp
int longestOnes(vector<int>& A, int K) {
        int i = 0, j;
        for (j = 0; j < A.size(); ++j) {
            if (A[j] == 0) K--;
            if (K < 0 && A[i++] == 0) K++;
        }
        return j - i;
    }
```

## Fruits into baskets
https://bit.ly/3D6d94w
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array **fruits** of size **N**, where **fruits[i]**  is the type of fruit the **ith** tree produces.  
You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow :

- You only have **two baskets**, and each basket can only hold a **single type** of fruit. There is no limit on the amount of fruit each basket can hold.
- Starting from any tree of your choice, you must pick **exactly one fruit** from **every** tree (including the start tree) while moving to the right. The picked fruits must fit in one of the baskets.
- Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array fruits, return the **maximum** number of fruits you can pick.

  
**Example 1:**

**Input:**  
N = 3  
fruits [ ] = { 2, 1, 2 }  
**Output:** 3  
**Explanation**: We can pick from all three trees. 

**Example 2:**

**Input**:  
N = 6  
fruits [ ] = { 0, 1, 2, 2, 2, 2 }  
**Output:** 5  
**Explanation**: It's optimal to pick from trees(0-indexed) [1,2,3,4,5].

```cpp
class Solution {
  public:
    int totalFruits(int N, vector<int> &fruits) {
            int i = 0, j = 1, x = 0;
            int ans = 1;
            int a = fruits[0], b = -1;
            
            for(; j < N; j++)
            {
                if(b == -1 && fruits[j] != a)
                {
                    b = fruits[j];
                    x = j;
                }
                else if(fruits[j] != a && fruits[j] != b)
                {
                    i = x;
                    x = j;
                    a = fruits[j-1];
                    b = fruits[j];
                    // continue;
                }
                else if(fruits[j] != fruits[j-1]) x = j;
                ans = max(ans, j-i+1);
            }
            return ans;
        }
};
```

## Longest repeating character replacement
https://leetcode.com/problems/longest-repeating-character-replacement/
You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return _the length of the longest substring containing the same letter you can get after performing the above operations_.

**Example 1:**

**Input:** s = "ABAB", k = 2
**Output:** 4
**Explanation:** Replace the two 'A's with two 'B's or vice versa.

**Example 2:**

**Input:** s = "AABABBA", k = 1
**Output:** 4
**Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.

```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        int n = s.size();
        int i = 0, j = 0, maxi = 0;
        unordered_map<char,int>mp;
        int ans = -1;
        while(j < n)
        {
            mp[s[j]]++;
            maxi = max(maxi, mp[s[j]]);
            if((j-i+1) - maxi > k){
                mp[s[i]]--;
                i++;
            }
            ans = max(ans, (j-i+1));
            j++;   
        }
        return ans;
    }
};
```

## Binary subarray with sum
https://leetcode.com/problems/binary-subarrays-with-sum/
Given a binary array `nums` and an integer `goal`, return _the number of non-empty **subarrays** with a sum_ `goal`.

A **subarray** is a contiguous part of the array.

**Example 1:**

**Input:** nums = [1,0,1,0,1], goal = 2
**Output:** 4
**Explanation:** The 4 subarrays are bolded and underlined below:
[**1,0,1**,0,1]
[**1,0,1,0**,1]
[1,**0,1,0,1**]
[1,0,**1,0,1**]

**Example 2:**

**Input:** nums = [0,0,0,0,0], goal = 0
**Output:** 15

```cpp
 int numSubarraysWithSum(vector<int>& A, int S) {
        return atMost(A, S) - atMost(A, S - 1);
    }

    int atMost(vector<int>& A, int S) {
        if (S < 0) return 0;
        int res = 0, i = 0, n = A.size();
        for (int j = 0; j < n; j++) {
            S -= A[j];
            while (S < 0)
                S += A[i++];
            res += j - i + 1;
        }
        return res;
    }
```

## Count number of nice subarrays
https://leetcode.com/problems/count-number-of-nice-subarrays/
Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.

Return _the number of **nice** sub-arrays_.

**Example 1:**

**Input:** nums = [1,1,2,1,1], k = 3
**Output:** 2
**Explanation:** The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].

**Example 2:**

**Input:** nums = [2,4,6], k = 1
**Output:** 0
**Explanation:** There are no odd numbers in the array.

**Example 3:**

**Input:** nums = [2,2,2,1,2,2,1,2,2,2], k = 2
**Output:** 16

```cpp
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> cnt(n + 1, 0);
        cnt[0] = 1;
        int ans = 0, t = 0;
        for (int v : nums) {
            t += v & 1;
            if (t - k >= 0) {
                ans += cnt[t - k];
            }
            cnt[t]++;
        }
        return ans;
    }
};
```

## Subarrays containing 3 chars
https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/
Given a string `s` consisting only of characters _a_, _b_ and _c_.

Return the number of substrings containing **at least** one occurrence of all these characters _a_, _b_ and _c_.

**Example 1:**

**Input:** s = "abcabc"
**Output:** 10
**Explanation:** The substrings containing at least one occurrence of the characters _a_, _b_ and _c are "_abc_", "_abca_", "_abcab_", "_abcabc_", "_bca_", "_bcab_", "_bcabc_", "_cab_", "_cabc_"_ and _"_abc_"_ (**again**)_._ 

**Example 2:**

**Input:** s = "aaacb"
**Output:** 3
**Explanation:** The substrings containing at least one occurrence of the characters _a_, _b_ and _c are "_aaacb_", "_aacb_"_ and _"_acb_"._ 

**Example 3:**

**Input:** s = "abc"
**Output:** 1

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int n = size(s);

        int i=0,j=0,count=0;

        unordered_map<char, int> mp;

        while(j<n)
        {
            mp[s[j]]++;

            while(mp['a']>=1 && mp['b']>=1 && mp['c']>=1)
            {
                count += (n - j);

                //shrinking the window
                mp[s[i]]--;
                i++;
            }
            j++;
        }
        return count;
    }
};
```

## Max point you can obtain from cards
https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/
There are several cards **arranged in a row**, and each card has an associated number of points. The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer `k`, return the _maximum score_ you can obtain.

**Example 1:**

**Input:** cardPoints = [1,2,3,4,5,6,1], k = 3
**Output:** 12
**Explanation:** After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.

**Example 2:**

**Input:** cardPoints = [2,2,2], k = 2
**Output:** 4
**Explanation:** Regardless of which two cards you take, your score will always be 4.

**Example 3:**

**Input:** cardPoints = [9,7,7,9,7,7,9], k = 7
**Output:** 55
**Explanation:** You have to take all the cards. Your score is the sum of points of all cards.

```cpp
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int res = 0;
		
		//First k elements in our window
        for(int i=0;i<k;i++) res+=cardPoints[i];
        
        int curr=res;
        for(int i=k-1;i>=0;i--) {
			//We remove the last visited element and add the non-visited element from the last
            curr-=cardPoints[i];
            curr+=cardPoints[cardPoints.size()-k+i];
			
            //We check the maximum value any possible combination can give
			res = max(res, curr);
        }
        
        return res;
    }
};
```

## Longest substring with atmost K distinct chars
https://www.naukri.com/code360/problem-details/distinct-characters_2221410
You are given a string 'str' and an integer ‘K’. Your task is to find the length of the largest substring with at most ‘K’ distinct characters.

**For example:**

```
You are given ‘str’ = ‘abbbbbbc’ and ‘K’ = 2, then the substrings that can be formed are [‘abbbbbb’, ‘bbbbbbc’]. Hence the answer is 7.
```

```cpp
int kDistinctChars(int k, string &str)
{
    // Write your code here
  map<char,int> mpp;
  int l=0,r=0,maxc=0;
  while(r<str.size())
  {
      mpp[str[r]]++;
      if(mpp.size()>k)
      {
          mpp[str[l]]--;
          if(mpp[str[l]]==0) mpp.erase(str[l]);
          l++;
      }
      if(mpp.size()<=k) maxc=max(maxc,r-l+1);
      r++;
  }
  return maxc;
}
```

## Subarray with k different integers
https://leetcode.com/problems/subarrays-with-k-different-integers
Given an integer array `nums` and an integer `k`, return _the number of **good subarrays** of_ `nums`.

A **good array** is an array where the number of different integers in that array is exactly `k`.

- For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [1,2,1,2,3], k = 2
**Output:** 7
**Explanation:** Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]

**Example 2:**

**Input:** nums = [1,2,1,3,4], k = 3
**Output:** 3
**Explanation:** Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].

```cpp
    int subarraysWithKDistinct(vector<int>& A, int K) {
        return atMostK(A, K) - atMostK(A, K - 1);
    }
    int atMostK(vector<int>& A, int K) {
        int i = 0, res = 0;
        unordered_map<int, int> count;
        for (int j = 0; j < A.size(); ++j) {
            if (!count[A[j]]++) K--;
            while (K < 0) {
                if (!--count[A[i]]) K++;
                i++;
            }
            res += j - i + 1;
        }
        return res;
    }
```

## Minimum windows substring
https://leetcode.com/problems/minimum-window-substring/
Given two strings `s` and `t` of lengths `m` and `n` respectively, return _the **minimum window**_

**_substring_**

_of_ `s` _such that every character in_ `t` _(**including duplicates**) is included in the window_. If there is no such substring, return _the empty string_ `""`.

The testcases will be generated such that the answer is **unique**.

**Example 1:**

**Input:** s = "ADOBECODEBANC", t = "ABC"
**Output:** "BANC"
**Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Example 2:**

**Input:** s = "a", t = "a"
**Output:** "a"
**Explanation:** The entire string s is the minimum window.

**Example 3:**

**Input:** s = "a", t = "aa"
**Output:** ""
**Explanation:** Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

```cpp
    string minWindow(string s, string t) {
        unordered_map<char, int> letters; //unordered map for storing the characters in t that we need to check for in s
        for(auto c : t) letters[c]++; 
        int count = 0; //counts number of t's letters in current window
        int low = 0, min_length = INT_MAX, min_start = 0;     
        for(int high = 0; high<s.length(); high++) {
            if(letters[s[high]] > 0) count++; //means that this letter is in t   
            letters[s[high]]--; //reduce the count for the letter on which we are currently 
            if(count == t.length()) { //if current windows contains all of the letters in t
                while(low < high && letters[s[low]] < 0) letters[s[low]]++, low++; //move low ahead if its not of any significance
                if(min_length > high-low) min_length = high-(min_start=low)+1; //update the min length
                letters[s[low++]]++; //move low ahaead and also increment the value
                count--; //count-- as we are moving low ahead & low pointed to a char in t before
            }
        }
        return min_length == INT_MAX ? "" : s.substr(min_start, min_length); //check for edge case & return the result
    }
```

## Minimum window subsequence
https://www.geeksforgeeks.org/problems/minimum-window-subsequence/1
Given strings **str1** and **str2**, find the **minimum (contiguous) substring** W of str1, so that str2 is a **subsequence** of W.

If there is **no such window in str1** that covers all characters in str2, return the **empty string** "". If there are **multiple** such minimum-length windows, return the one with the **left-most starting index**.  
 

**Example 1:**

**Input:** 
str1: geeksforgeeks
str2: eksrg
**Output:** 
eksforg
**Explanation:** 
"eksforg" satisfies all required conditions. str2 is its subsequence and it is longest and leftmost among all possible valid substrings of str1.

**Example 2:**

**Input:** 
str1: `abcdebdde`   
str2: bde   
**Output: ** bcde   
**Explanation: ** `"bcde" is the answer and "deb" is not a smaller window because the elements in the window must occur in order.`

```cpp
class Solution {
  public:
    string minWindow(string s, string t) {
        int n = s.size(), m = t.size();
        int i=0, j=0, k=0;
        string ans;
        int sz = 1e9;
        for(i=0,j=0; i<n && k<m; ){
            if(s[i]==t[k]){
                k++;
            }
            
            // fixing end point
            if(k==m){
                j=i;
                k=m-1;
                
                //minimising our starting point
                while(k>=0 && j>=0){
                    if(s[j]==t[k]) k--;
                    j--;
                }
                j++;
                
                // now this can be our substring i.e. s[j..i]
                if(sz>(i-j+1)){
                    sz = i-j+1;
                    ans = s.substr(j,sz);
                }
                i=j;
                k=0;
            }
            i++;
        }
        return ans;
    }
};
```