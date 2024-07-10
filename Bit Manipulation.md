## Count Number of bits to be flipped to convert A to B
https://leetcode.com/problems/minimum-bit-flips-to-convert-number/
A **bit flip** of a number `x` is choosing a bit in the binary representation of `x` and **flipping** it from either `0` to `1` or `1` to `0`.

- For example, for `x = 7`, the binary representation is `111` and we may choose any bit (including any leading zeros not shown) and flip it. We can flip the first bit from the right to get `110`, flip the second bit from the right to get `101`, flip the fifth bit from the right (a leading zero) to get `10111`, etc.

Given two integers `start` and `goal`, return _the **minimum** number of **bit flips** to convert_ `start` _to_ `goal`.

**Example 1:**

**Input:** start = 10, goal = 7
**Output:** 3
**Explanation:** The binary representation of 10 and 7 are 1010 and 0111 respectively. We can convert 10 to 7 in 3 steps:
- Flip the first bit from the right: 1010 -> 1011.
- Flip the third bit from the right: 1011 -> 1111.
- Flip the fourth bit from the right: 1111 -> 0111.
It can be shown we cannot convert 10 to 7 in less than 3 steps. Hence, we return 3.

**Example 2:**

**Input:** start = 3, goal = 4
**Output:** 3
**Explanation:** The binary representation of 3 and 4 are 011 and 100 respectively. We can convert 3 to 4 in 3 steps:
- Flip the first bit from the right: 011 -> 010.
- Flip the second bit from the right: 010 -> 000.
- Flip the third bit from the right: 000 -> 100.
It can be shown we cannot convert 3 to 4 in less than 3 steps. Hence, we return 3.

```cpp
int minBitFlips(int start, int goal) {
        return __builtin_popcount(start^goal);
}
```

```cpp
class Solution {
public:
    int minBitFlips(int start, int goal) {
        
        int a = (start ^ goal); // this will do xor of given numbers
           //now count set bits
        int count = 0; 
        while(a){
            if(a&1){
                count++;
            }
        a = a>>1;  // this is right shift operator
        }
        return count;
    }
};
```

## Find the number that appears odd number of times
https://leetcode.com/problems/single-number/
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for(auto i : nums) ans = ans ^ i;
        return ans;
    }
};
```

## Power Set
https://leetcode.com/problems/subsets/
Given an integer array `nums` of **unique** elements, return _all possible_

_subsets_

_(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example 2:**

**Input:** nums = [0]
**Output:** [[],[0]]

To give all the possible subsets, we just need to exhaust all the possible combinations of the numbers. And each number has only two possibilities: either in or not in a subset. And this can be represented using a bit.

Using `[1, 2, 3]` as an example, `1` appears once in every two consecutive subsets, `2` appears twice in every four consecutive subsets, and `3` appears four times in every eight subsets (initially all subsets are empty):

```
[], [ ], [ ], [    ], [ ], [    ], [    ], [       ]
[], [1], [ ], [1   ], [ ], [1   ], [    ], [1      ]
[], [1], [2], [1, 2], [ ], [1   ], [2   ], [1, 2   ]
[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]
```

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size(), p = 1 << n;
        vector<vector<int>> subs(p);
        for (int i = 0; i < p; i++) {
            for (int j = 0; j < n; j++) {
                if ((i >> j) & 1) {
                    subs[i].push_back(nums[j]);
                }
            }
        }
        return subs;
    }
};
```

## Find 2 numbers appearing odd number of times
https://practice.geeksforgeeks.org/problems/two-numbers-with-odd-occurrences5846/1
Given an unsorted array, **Arr**[] of size **N** and that contains **even** number of occurrences for all numbers except two numbers. Find the two numbers in **decreasing** order which has **odd** occurrences.  
  
**Example 1:**

**Input:**
N = 8
Arr = {4, 2, 4, 5, 2, 3, 3, 1}
**Output:** {5, 1} 
**Explaination:** 5 and 1 have odd occurrences.

  
**Example 2:**

**Input:**
N = 8
Arr = {1 7 5 7 5 4 7 4}
**Output:** {7, 1}
**Explaination:** 7 and 1 have odd occurrences.

```cpp
//User function Template for C++
class Solution{
    public:
    vector<long long int> twoOddNum(long long int Arr[], long long int N)  
    {
        long long ans = 0;
        for(int i=0 ; i<N ; i++) ans ^= Arr[i];
        long long mask = ans&(-ans);
        
        long long xor1 = 0;
        for(int i=0 ; i<N ; i++)
        {
            if(Arr[i]&mask) xor1 ^= Arr[i];
        }
        
        return {max(xor1, xor1^ans), min(xor1, xor1^ans)};
        
    }
};
```

## Print Prime Factors of a number
https://bit.ly/3C165p8
Given a number **N**. Find its **unique prime factors** in **increasing order.**  
 

**Example 1:**

**Input:** N = 100
**Output:** 2 5
**Explanation:** 2 and 5 are the unique prime
factors of 100.

**Example 2:**

**Input:** N = 35
**Output:** 5 7
**Explanation:** 5 and 7 are the unique prime
factors of 35.

```cpp
class Solution{
	public:
	vector<int>AllPrimeFactors(int N) {
	    vector<int>ans;
	    for(int i=2;i<=sqrt(N);i++)
	    {
	        if(N%i==0)
	        {
	            ans.push_back(i);
	        }
	        while(N%i==0)
	        {
	            N=N/i;
	        }
	    }
	    if(N!=1)
	    {
	        ans.push_back(N);
	    }
	    return ans;
	}
};
```

## Sieve
Count primes with sieve
https://leetcode.com/problems/count-primes/
```cpp
class Solution {
public:
	int countPrimes(int n) {
		//prime no. generation using sieve of eratothons
		vector<bool> prime(n + 1, true);
		prime[0] = false;
		prime[1] = false;
		for (int i = 2; i * i <= n; i++) {
			if (prime[i]) {
				for (int j = i * i; j <= n; j += i) {
					prime[j] = false;
				}
			}
		}
		//counting prime numbers
		int primeCount = 0;
		for (int i = 2; i < n; i++) {
			if (prime[i]) {
				primeCount++;
			}
		}
		return primeCount;
	}
};
```

