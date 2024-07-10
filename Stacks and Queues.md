## Implement Stack using Queue
https://leetcode.com/problems/implement-stack-using-queues/
```cpp
class Stack {
    queue<int> q;
public:
    void push(int x) {
        q.push(x);
        for (int i=1; i<q.size(); i++) {
            q.push(q.front());
            q.pop();
        }
    }

    void pop() {
        q.pop();
    }

    int top() {
        return q.front();
    }

    bool empty() {
        return q.empty();
    }
};
```

## Implement Queue using Stack
```cpp
class Queue {
    stack<int> input, output;
public:

    void push(int x) {
        input.push(x);
    }

    void pop(void) {
        peek();
        output.pop();
    }

    int peek(void) {
        if (output.empty())
            while (input.size())
                output.push(input.top()), input.pop();
        return output.top();
    }

    bool empty(void) {
        return input.empty() && output.empty();
    }
};
```


# Monotonic Stack/Queue
## Next Greater Element
https://leetcode.com/problems/next-greater-element-i/
The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return _an array_ `ans` _of length_ `nums1.length` _such that_ `ans[i]` _is the **next greater element** as described above._

**Example 1:**

**Input:** nums1 = [4,1,2], nums2 = [1,3,4,2]
**Output:** [-1,3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

**Example 2:**

**Input:** nums1 = [2,4], nums2 = [1,2,3,4]
**Output:** [3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.
```cpp
// Time: O(N)
// Space: O(N)
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        stack<int> s;
        unordered_map<int, int> m;
        for (int n : nums) {
            while (s.size() && s.top() < n) {
                m[s.top()] = n;
                s.pop();
            }
            s.push(n);
        }
        vector<int> ans;
        for (int n : findNums) ans.push_back(m.count(n) ? m[n] : -1);
        return ans;
    }
};
```

## Next Greater Element II
https://leetcode.com/problems/next-greater-element-ii/
Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return _the **next greater number** for every element in_ `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

**Example 1:**

**Input:** nums = [1,2,1]
**Output:** [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.

**Example 2:**

**Input:** nums = [1,2,3,4,3]
**Output:** [2,3,4,-1,4]

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& a) {
        int n = a.size();
        vector<int>v(n,-1);

        stack<int>st;
        for(int i = 2*n - 1; i >= 0; i--)
        {
            // we pop out all elements smaller than current element
            while(!st.empty() && (a[i%n] >= st.top()))
            {
                st.pop();
            }

            // if stack is empty means no greater element is there
            // if not empty we make answer at that index equal to top element
            if(!st.empty() && (i < n))
            {
                v[i] = st.top();
            }

            st.push(a[i%n]);
        }

        return v;
    }
};
```

## Next Smaller Element
https://www.interviewbit.com/problems/nearest-smaller-element/
Given an array, find the **nearest** smaller element G[i] for every element A[i] in the array such that the element has an **index smaller than i**.

More formally,

```
    G[i] for an element A[i] = an element A[j] such that 
    j is maximum possible AND 
    j < i AND
    A[j] < A[i]
```

Elements for which no smaller element exist, consider next smaller element as -1.

```cpp
vector<int> Solution::prevSmaller(vector<int> &A) {
    vector<int> sol(A.size(), -1);
    stack<int> st;
    
    for (int i = 0; i < A.size(); i++) {
        while (!st.empty() && st.top() >= A[i]) {
            st.pop();
        }
        sol[i] = st.empty() ? -1 : st.top();
        st.push(A[i]);
    }
    
    return sol;
}
```

## Number of NGE's to the right
https://www.geeksforgeeks.org/problems/number-of-nges-to-the-right/1
Given an array of **N** integers and **Q** queries of indices, print the number of next greater elements(NGEs) to the right of the given index element.   
**Example:**

**Input: ** arr     = [3, 4, 2, 7, 5, 8, 10, 6]
        queries = 2
        indices = [0, 5]
**Output: ** 6, 1
**Explanation: ** 
The next greater elements to the right of 3(index 0)
are 4,7,5,8,10,6.  
The next greater elements to the right of 8(index 5)
is only 10.

```cpp
class Solution{
public:
    int count(vector<int>&arr,int ele,int ind){
        int n=arr.size();
        int cnt=0;
        for(int i=ind;i<n;i++){
            if(arr[i]>ele){
                cnt++;
            }
        }
    return cnt;
    }
    vector<int> count_NGE(int n,vector<int>&arr, 
    int q,vector<int> &i){
        vector<int>ans;
        int j=0;
        while(q--){
            if(j<i.size()){
                int ele=arr[i[j]];
                int cnt=count(arr,ele,i[j]);
                ans.push_back(cnt);
                j++;
            }
        }
    return ans;   
    }
};
```

## Trapping Rainwater
https://leetcode.com/problems/trapping-rain-water/
Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

**Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]
**Output:** 6
**Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Example 2:**

**Input:** height = [4,2,0,3,2,5]
**Output:** 9

```cpp
class Solution
{
public:
    int trap(vector<int> &height)
    {
        int n = height.size(); // Number of elements in the height array
        int water = 0;         // Initialize the total trapped water volume
        vector<int> stack;     // Stack to store indices of height elements
        // Iterate through the heights
        for (int right = 0; right < n; right++)
        {
            // Process each height to trap water
            while (!stack.empty() && height[stack.back()] < height[right])
            {
                // If the current height is greater than the height at the top of the stack
                int mid = stack.back(); // Get the index of the height at the top of the stack
                stack.pop_back();       // Pop the index from the stack
                // If the stack becomes empty, no more water can be trapped
                if (stack.empty())
                    break;
                int left = stack.back();                                                      // Get the index of the next height from the top of the stack
                int minHeight = min(height[right] - height[mid], height[left] - height[mid]); // Calculate the minimum height of the two borders
                int width = right - left - 1;                                                 // Calculate the width between the left and right borders
                water += minHeight * width;                                                   // Calculate the trapped water volume and add it to the total
            }
            stack.push_back(right); // Push the current index onto the stack
        }
        return water; // Return the total trapped water volume
    }
};
```

## Sum of Subarray Mins
https://leetcode.com/problems/sum-of-subarray-minimums/
Given an array of integers arr, find the sum of `min(b)`, where `b` ranges over every (contiguous) subarray of `arr`. Since the answer may be large, return the answer **modulo** `109 + 7`.

**Example 1:**

**Input:** arr = [3,1,2,4]
**Output:** 17
**Explanation:** 
Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.

**Example 2:**

**Input:** arr = [11,81,94,43,3]
**Output:** 444

```cpp
class Solution {
public:
  int sumSubarrayMins(vector<int>& A) {
    stack<pair<int, int>> in_stk_p, in_stk_n;
    // left is for the distance to previous less element
    // right is for the distance to next less element
    vector<int> left(A.size()), right(A.size());
		
    //initialize
    for(int i = 0; i < A.size(); i++) left[i] =  i + 1;
    for(int i = 0; i < A.size(); i++) right[i] = A.size() - i;
		
    for(int i = 0; i < A.size(); i++){
      // for previous less
      while(!in_stk_p.empty() && in_stk_p.top().first > A[i]) in_stk_p.pop();
      left[i] = in_stk_p.empty()? i + 1: i - in_stk_p.top().second;
      in_stk_p.push({A[i],i});
			
      // for next less
      while(!in_stk_n.empty() && in_stk_n.top().first > A[i]){
        auto x = in_stk_n.top();in_stk_n.pop();
        right[x.second] = i - x.second;
      }
      in_stk_n.push({A[i], i});
    }

    int ans = 0, mod = 1e9 +7;
    for(int i = 0; i < A.size(); i++){
      ans = (ans + A[i]*left[i]*right[i])%mod;
    }
    return ans;
  }
};
```

## Asteroid Collision
https://leetcode.com/problems/asteroid-collision/
We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example 1:**

**Input:** asteroids = [5,10,-5]
**Output:** [5,10]
**Explanation:** The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

**Example 2:**

**Input:** asteroids = [8,-8]
**Output:** []
**Explanation:** The 8 and -8 collide exploding each other.

**Example 3:**

**Input:** asteroids = [10,2,-5]
**Output:** [10]
**Explanation:** The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.
```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& ast) {
        int n = ast.size();
        stack<int> s;
        for(int i = 0; i < n; i++) {
            if(ast[i] > 0 || s.empty()) {
                s.push(ast[i]);
            }
            else {
                while(!s.empty() and s.top() > 0 and s.top() < abs(ast[i])) {
                    s.pop();
                }
                if(!s.empty() and s.top() == abs(ast[i])) {
                    s.pop();
                }
                else {
                    if(s.empty() || s.top() < 0) {
                        s.push(ast[i]);
                    }
                }
            }
        }
        vector<int> res(s.size());
        for(int i = (int)s.size() - 1; i >= 0; i--) {
            res[i] = s.top();
            s.pop();
        }
        return res;
    }
};
```

## Sum of subarray ranges
https://leetcode.com/problems/sum-of-subarray-ranges/
You are given an integer array `nums`. The **range** of a subarray of `nums` is the difference between the largest and smallest element in the subarray.

Return _the **sum of all** subarray ranges of_ `nums`_._

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** 4
**Explanation:** The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0 
[2], range = 2 - 2 = 0
[3], range = 3 - 3 = 0
[1,2], range = 2 - 1 = 1
[2,3], range = 3 - 2 = 1
[1,2,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.

**Example 2:**

**Input:** nums = [1,3,3]
**Output:** 4
**Explanation:** The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0
[3], range = 3 - 3 = 0
[3], range = 3 - 3 = 0
[1,3], range = 3 - 1 = 2
[3,3], range = 3 - 3 = 0
[1,3,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 2 + 0 + 2 = 4.

**Example 3:**

**Input:** nums = [4,-2,-3,4,1]
**Output:** 59
**Explanation:** The sum of all subarray ranges of nums is 59.

```cpp
class Solution {
public:
    vector<int> getPrevious(vector<int>&nums, bool isPrevSmall)
    {
        //(isPrevSmall = true) ===> previous smaller element problem using stack
        //(isPrevSmall = false) ==> previous greater element problem using stack
        //There could me many ways to solve this using monotonic stack, simpler way shown below
        int n = nums.size();
        stack<int>st;
        vector<int>ans(n, -1);
        st.push(n - 1);
        for (int i = n - 2; i >= 0; i--)
        {
            int curr = nums[i];
            //============================================
            while(!st.empty())
            {
                bool condition = (isPrevSmall)? (curr < nums[st.top()]) : (curr > nums[st.top()]);
                if (condition == true)
                {
                    ans[st.top()] = i;
                    st.pop();
                }
                else break;
            }
            //=================================================
            st.push(i);
        }
        return ans;    
    }
    vector<int> getNext(vector<int>&nums, bool isNextSmall)
    {
        //(isPrevSmall = true) ===> next smaller/equal element problem using stack
        //(isPrevSmall = false) ==> next smaller/equal element problem using stack
        int n = nums.size();
        vector<int>ans(n, n);
        stack<int>st;
        st.push(0);
        for (int i = 1; i < n; i++)
        {
            int curr = nums[i];
            //======================================
            while(!st.empty())
            {
                int topIdx = st.top();
                bool condition = (isNextSmall)? (curr <= nums[topIdx]) : (curr >= nums[topIdx]);
				//DON'T MISS THE "EQUAL TO" CASE :)
                if (condition == true)
                {
                    ans[topIdx] = i;
                    st.pop();
                }
                else break;
            }
            //===================================================
            st.push(i);
        }
        return ans;
    }
    long long subArrayRanges(vector<int>& nums) 
    {
        int n = nums.size();
        vector<int>prevSmaller = getPrevious(nums, true);
        vector<int>prevGreater = getPrevious(nums, false);
        //===================================================
        vector<int>nextSmaller = getNext(nums, true);
        vector<int>nextGreater = getNext(nums, false);
        //=======================================================
        long long ans = 0;
        for (int i = 0; i < n; i++)
        {
            int prevRangeCount = i - prevSmaller[i];
            int nextRangeCount = nextSmaller[i] - i;
            long long minSubarrayCount = (long long)prevRangeCount * nextRangeCount;
            ans = ans - (long long)(nums[i] * minSubarrayCount);
            
            int prevRangeCount1 = i - prevGreater[i];
            int nextRangeCount1 = nextGreater[i] - i;
            long long maxSubarrayCount = prevRangeCount1 * nextRangeCount1;
            ans = ans + (long long)(nums[i] * maxSubarrayCount);
        }
        return ans;
    }
};
```

## Remove K digits
https://leetcode.com/problems/remove-k-digits/
Given string num representing a non-negative integer `num`, and an integer `k`, return _the smallest possible integer after removing_ `k` _digits from_ `num`.

**Example 1:**

**Input:** num = "1432219", k = 3
**Output:** "1219"
**Explanation:** Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

**Example 2:**

**Input:** num = "10200", k = 1
**Output:** "200"
**Explanation:** Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

**Example 3:**

**Input:** num = "10", k = 2
**Output:** "0"
**Explanation:** Remove all the digits from the number and it is left with nothing which is 0.

```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {
        // number of operation greater than length we return an empty string
        if(num.length() <= k)   
            return "0";
        
        // k is 0 , no need of removing /  preforming any operation
        if(k == 0)
            return num;
        
        string res = "";// result string
        stack <char> s; // char stack
        
        s.push(num[0]); // pushing first character into stack
        
        for(int i = 1; i<num.length(); ++i)
        {
            while(k > 0 && !s.empty() && num[i] < s.top())
            {
                // if k greater than 0 and our stack is not empty and the upcoming digit,
                // is less than the current top than we will pop the stack top
                --k;
                s.pop();
            }
            
            s.push(num[i]);
            
            // popping preceding zeroes
            if(s.size() == 1 && num[i] == '0')
                s.pop();
        }
        
        while(k && !s.empty())
        {
            // for cases like "456" where every num[i] > num.top()
            --k;
            s.pop();
        }
        
        while(!s.empty())
        {
            res.push_back(s.top()); // pushing stack top to string
            s.pop(); // pop the top element
        }
        
        reverse(res.begin(),res.end()); // reverse the string 
        
        if(res.length() == 0)
            return "0";
        
        return res;
        
        
    }
};
```

## Largest Rectangle in Histogram
https://leetcode.com/problems/largest-rectangle-in-histogram/
Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

**Input:** heights = [2,1,5,6,2,3]
**Output:** 10
**Explanation:** The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

**Input:** heights = [2,4]
**Output:** 4

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n=heights.size();
        int maxArea = 0;
        
        for (int i = 0; i < n; i++) {
            int minHeight = INT_MAX;
            for (int j = i; j < n; j++) {
                minHeight = min(minHeight, heights[j]);
                maxArea = max(maxArea, minHeight * (j - i + 1));
            }
        }
        
        return maxArea;
    }
};
```

## Maximal Rectangles
https://leetcode.com/problems/maximal-rectangle/
Given a `rows x cols` binary `matrix` filled with `0`'s and `1`'s, find the largest rectangle containing only `1`'s and return _its area_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)

**Input:** matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
**Output:** 6
**Explanation:** The maximal rectangle is shown in the above picture.

**Example 2:**

**Input:** matrix = [["0"]]
**Output:** 0

**Example 3:**

**Input:** matrix = [["1"]]
**Output:** 1

**Constraints:**

- `rows == matrix.length`
- `cols == matrix[i].length`
- `1 <= row, cols <= 200`
- `matrix[i][j]` is `'0'` or `'1'`.
```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        if (matrix.empty() || matrix[0].empty())
            return 0;

        int rows = matrix.size();
        int cols = matrix[0].size();
        vector<int> heights(cols + 1, 0); // Include an extra element for easier calculation
        int maxArea = 0;

        for (const auto& row : matrix) {
            for (int i = 0; i < cols; i++) {
                heights[i] = (row[i] == '1') ? heights[i] + 1 : 0;
            }

            // Calculate max area using stack-based method
            stack<int> stk;
            for (int i = 0; i < heights.size(); i++) {
                while (!stk.empty() && heights[i] < heights[stk.top()]) {
                    int h = heights[stk.top()];
                    stk.pop();
                    int w = stk.empty() ? i : i - stk.top() - 1;
                    maxArea = max(maxArea, h * w);
                }
                stk.push(i);
            }
        }

        return maxArea;
    }
};
```

# Implementation problems
## Sliding Window Maximum
https://leetcode.com/problems/sliding-window-maximum/
You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

**Example 1:**

**Input:** nums = [1,3,-1,-3,5,3,6,7], k = 3
**Output:** [3,3,5,5,6,7]
**Explanation:** 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       **3**
 1 [3  -1  -3] 5  3  6  7       **3**
 1  3 [-1  -3  5] 3  6  7      ** 5**
 1  3  -1 [-3  5  3] 6  7       **5**
 1  3  -1  -3 [5  3  6] 7       **6**
 1  3  -1  -3  5 [3  6  7]      **7**

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
		vector<int> res;
        deque<int> mq;
        
        for (int i = 0; i < n; i++) {
		
            // If the mq head in out of the window --> pop head
            if (mq.size() > 0 && mq.front() <= i - k) mq.pop_front();
            
            // If mq is non empty and current val >= last element of mq
            // --> remove all elements that are <= from back 
            while (mq.size() > 0 && nums[i] >= nums[mq.back()]) 
                mq.pop_back();
            
            // Takes care of inserting into the end
            mq.push_back(i);
            
            // Store results from the end of the first window
            if (i + 1 >= k) res.push_back(nums[mq.front()]);
        }
        
        return res;
    }
};
```

## Stock span problem
https://leetcode.com/problems/online-stock-span
Design an algorithm that collects daily price quotes for some stock and returns **the span** of that stock's price for the current day.

The **span** of the stock's price in one day is the maximum number of consecutive days (starting from that day and going backward) for which the stock price was less than or equal to the price of that day.

- For example, if the prices of the stock in the last four days is `[7,2,1,2]` and the price of the stock today is `2`, then the span of today is `4` because starting from today, the price of the stock was less than or equal `2` for `4` consecutive days.
- Also, if the prices of the stock in the last four days is `[7,34,1,2]` and the price of the stock today is `8`, then the span of today is `3` because starting from today, the price of the stock was less than or equal `8` for `3` consecutive days.

Implement the `StockSpanner` class:

- `StockSpanner()` Initializes the object of the class.
- `int next(int price)` Returns the **span** of the stock's price given that today's price is `price`.

**Example 1:**

**Input**
["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]
[[], [100], [80], [60], [70], [60], [75], [85]]
**Output**
[null, 1, 1, 1, 2, 1, 4, 6]

**Explanation**
StockSpanner stockSpanner = new StockSpanner();
stockSpanner.next(100); // return 1
stockSpanner.next(80);  // return 1
stockSpanner.next(60);  // return 1
stockSpanner.next(70);  // return 2
stockSpanner.next(60);  // return 1
stockSpanner.next(75);  // return 4, because the last 4 prices (including today's price of 75) were less than or equal to today's price.
stockSpanner.next(85);  // return 6

```cpp
class StockSpanner {
public:
    StockSpanner() {
        // maintain a monotonic stack for stock entry
        
		// definition of stock entry:
        // first parameter is price quote
        // second parameter is price span
    }
    
    int next(int price) {
        
        int curPrice = price;
        int curSpan = 1;
        
        // Compute price span in stock data with monotonic stack
        while( stack.size() && stack.back().first <= price ){
            
            auto [prevPrice, prevSpan] = stack.back();
            
            stack.pop_back();
            
            // update current price span with history data in stack
            curSpan += prevSpan;
        }
        
        // update latest price quote and price span
        stack.push_back( pair<int, int>{curPrice, curSpan} );
        
        return curSpan;
    }
private:
    vector< pair<int, int> > stack;
};
```

## The Celebrity Problem
https://leetcode.com/accounts/login/?next=/problems/find-the-celebrity/
Suppose you are at a party with `n` people (labeled from `0` to `n - 1`) and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function `bool knows(a, b)` which tells you whether A knows B. Implement a function `int findCelebrity(n)`. There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/02/277_example_1_bold.PNG)

**Input:** graph = [
  [1,1,0],
  [0,1,0],
  [1,1,1]
]
**Output:** 1
**Explanation:** There are three persons labeled with 0, 1 and 2. graph[i][j] = 1 means person i knows person j, otherwise graph[i][j] = 0 means person i does not know person j. The celebrity is the person labeled as 1 because both 0 and 2 know him but 1 does not know anybody.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/02/02/277_example_2.PNG)

**Input:** graph = [
  [1,0,1],
  [1,1,0],
  [0,1,1]
]
**Output:** -1
**Explanation:** There is no celebrity.

**Note:**

1. The directed graph is represented as an adjacency matrix, which is an `n x n` matrix where `a[i][j] = 1` means person `i` knows person `j` while `a[i][j] = 0` means the contrary.
2. Remember that you won't have direct access to the adjacency matrix.

```cpp
class Solution {
public:
    int findCelebrity(int n) {
        int ans = 0;
        for (int i = 1; i < n; ++i) {
            if (knows(ans, i)) {
                ans = i;
            }
        }
        for (int i = 0; i < n; ++i) {
            if (ans != i) {
                if (knows(ans, i) || !knows(i, ans)) {
                    return -1;
                }
            }
        }
        return ans;
    }
};
```

## LRU cache
https://leetcode.com/problems/lru-cache/
Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input**
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
**Output**
[null, null, null, 1, null, -1, null, -1, 3, 4]

**Explanation**
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4

```cpp
unordered_map <int, int> m, cnt;
queue <int> q;
int n;
LRUCache(int capacity) : n(capacity) {}
int get(int key) {
	if (cnt.find(key) == cnt.end()) 
		return -1;
	q.push(key);
	cnt[key]++;
	return m[key];
}
void put(int key, int value) {
	q.push(key);
	cnt[key]++;
	m[key] = value;
	while (cnt.size() > n) {
		int cur = q.front(); q.pop();
		if (cnt[cur]-- == 1) 
			cnt.erase(cur);
	}
}
```

## LFU Cache
https://leetcode.com/problems/lfu-cache/
Design and implement a data structure for a [Least Frequently Used (LFU)](https://en.wikipedia.org/wiki/Least_frequently_used) cache.

Implement the `LFUCache` class:

- `LFUCache(int capacity)` Initializes the object with the `capacity` of the data structure.
- `int get(int key)` Gets the value of the `key` if the `key` exists in the cache. Otherwise, returns `-1`.
- `void put(int key, int value)` Update the value of the `key` if present, or inserts the `key` if not already present. When the cache reaches its `capacity`, it should invalidate and remove the **least frequently used** key before inserting a new item. For this problem, when there is a **tie** (i.e., two or more keys with the same frequency), the **least recently used** `key` would be invalidated.

To determine the least frequently used key, a **use counter** is maintained for each key in the cache. The key with the smallest **use counter** is the least frequently used key.

When a key is first inserted into the cache, its **use counter** is set to `1` (due to the `put` operation). The **use counter** for a key in the cache is incremented either a `get` or `put` operation is called on it.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input**
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
**Output**
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

**Explanation**
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3

```cpp
struct Node {
  int key;
  int value;
  int freq;
  list<int>::const_iterator it;
};

class LFUCache {
 public:
  LFUCache(int capacity) : capacity(capacity), minFreq(0) {}

  int get(int key) {
    if (!keyToNode.count(key))
      return -1;

    Node& node = keyToNode[key];
    touch(node);
    return node.value;
  }

  void put(int key, int value) {
    if (capacity == 0)
      return;
    if (keyToNode.count(key)) {
      Node& node = keyToNode[key];
      node.value = value;
      touch(node);
      return;
    }

    if (keyToNode.size() == capacity) {
      // Evict LRU key from the minFreq list
      const int keyToEvict = freqToList[minFreq].back();
      freqToList[minFreq].pop_back();
      keyToNode.erase(keyToEvict);
    }

    minFreq = 1;
    freqToList[1].push_front(key);
    keyToNode[key] = {key, value, 1, cbegin(freqToList[1])};
  }

 private:
  int capacity;
  int minFreq;
  unordered_map<int, Node> keyToNode;
  unordered_map<int, list<int>> freqToList;

  void touch(Node& node) {
    // Update the node's frequency
    const int prevFreq = node.freq;
    const int newFreq = ++node.freq;

    // Remove the iterator from prevFreq's list
    freqToList[prevFreq].erase(node.it);
    if (freqToList[prevFreq].empty()) {
      freqToList.erase(prevFreq);
      // Update minFreq if needed
      if (prevFreq == minFreq)
        ++minFreq;
    }

    // Insert the key to the front of newFreq's list
    freqToList[newFreq].push_front(node.key);
    node.it = cbegin(freqToList[newFreq]);
  }
};
```