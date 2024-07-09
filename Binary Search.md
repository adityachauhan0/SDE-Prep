# 1D binary search
## Problem statement

You're given a sorted array **_'a'_** of **_'n'_** integers and an integer **_'x'_**.
Find the floor and ceiling of 'x' in 'a[0..n-1]'.
**Note:**

```
Floor of 'x' is the largest element in the array which is smaller than or equal to 'x'.
Ceiling of 'x' is the smallest element in the array greater than or equal to 'x'.
```
 
**Example:**

```
Input: 
n=6, x=5, a=[3, 4, 7, 8, 8, 10]   

Output:
4

Explanation:
The floor and ceiling of 'x' = 5 are 4 and 7, respectively.
``` 

Detailed explanation ( Input/output format, Notes, Images )

##### Sample Input 1 :

```
6 8
3 4 4 7 8 10
```

##### Sample Output 1 :

```
8 8
```
 
##### Explanation of sample input 1 :

```
Since x = 8 is present in the array, it will be both floor and ceiling.
```

##### Sample Input 2 :

```
6 2
3 4 4 7 8 10
```

##### Sample Output 2 :

```
-1 3
```

##### Explanation of sample input 2 :

```
Since no number is less than or equal to x = 2 in the array, its floor does not exist. The ceiling will be 3.
```

```cpp
pair<int, int> getFloorAndCeil(vector<int> &a, int n, int x) {
    //target-->floor and ceil of x 

    int floor=INT_MIN;int ceil=INT_MAX;
	int low=0;int high=n-1;
	while(low<=high){
		int mid=(high+low)/2;
		if(a[mid]<=x){
		    floor=max(floor,a[mid]);
		    low=mid+1;
		}
		if(a[mid]>=x){
			ceil=min(ceil,a[mid]);
			high=mid-1;
		}
	}
	if(floor ==INT_MIN){
	    //floor doesnot exists
	    floor=-1;
	}
	if(ceil==INT_MAX)ceil=-1;
	return {floor,ceil};
}
```

## Search in Rotated Sorted Array
https://leetcode.com/problems/search-in-rotated-sorted-array/
There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1

**Example 3:**

**Input:** nums = [1], target = 0
**Output:** -1
```cpp
class Solution {
public:
    int search(vector<int>& a, int target) {
        int n = a.size();
        int l = 0, r = n-1;
        while (l <= r){
            int mid = l + (r-l)/2 ;
            if (a[mid] == target) return mid;
            //if left side is sorted
            if (a[l] <= a[mid]){
                //if it lies in that range
                if (a[l] <= target && target <= a[mid]){
                    //go left
                    r = mid -1;
                }
                //else go right
                else l = mid + 1;
            }
            //if right side is sorted
            else {
                // target lies in this range
                if (a[mid] <= target && target <= a[r]){
                    //go right
                    l = mid + 1;
                }
                //else go left
                else r = mid - 1;
            }
        }
        return -1;
        
    }
};
```

## Search in Rotated Sorted Array II
https://leetcode.com/problems/search-in-rotated-sorted-array-ii/
There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` _if_ `target` _is in_ `nums`_, or_ `false` _if it is not in_ `nums`_._

You must decrease the overall operation steps as much as possible.

**Example 1:**

**Input:** nums = [2,5,6,0,0,1,2], target = 0
**Output:** true

**Example 2:**

**Input:** nums = [2,5,6,0,0,1,2], target = 3
**Output:** false

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int l = 0;
        int r = nums.size() - 1;
        
        while(l <= r)
        {
            int mid = l + (r-l) / 2;
            if (nums[mid] == target)
                return true;
			// with duplicates we can have this contdition, just update left & right
            if((nums[l] == nums[mid]) && (nums[r] == nums[mid]))
            {
                l++;
                r--;
            }
			// first half
			// first half is in order
            else if(nums[l] <= nums[mid])
            {
				// target is in first  half
                if((nums[l] <= target) && (nums[mid] > target))
                    r = mid - 1;
                else
                    l = mid + 1;
            }
			// second half
			// second half is order
			// target is in second half
            else
            {
                if((nums[mid] < target) && (nums[r]>= target))
                    l = mid + 1;
                else
                    r = mid - 1;
            }
        }
        return false;
    }
};
```

## Find Minimum in Rotated Sorted Array
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return _the minimum element of this array_.

You must write an algorithm that runs in `O(log n) time.`

**Example 1:**

**Input:** nums = [3,4,5,1,2]
**Output:** 1
**Explanation:** The original array was [1,2,3,4,5] rotated 3 times.

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2]
**Output:** 0
**Explanation:** The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.

**Example 3:**

**Input:** nums = [11,13,15,17]
**Output:** 11
**Explanation:** The original array was [11,13,15,17] and it was rotated 4 times.

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;

        while(left < right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] < nums[right]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return nums[left];
    }
};
```

## Find peak element
https://leetcode.com/problems/find-peak-element
A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**

**Input:** nums = [1,2,3,1]
**Output:** 2
**Explanation:** 3 is a peak element and your function should return the index number 2.

**Example 2:**

**Input:** nums = [1,2,1,3,5,6,4]
**Output:** 5
**Explanation:** Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.

```cpp

class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        int low = 0;
        int high = nums.size()-1;
        while(low < high){
            int mid = (low + high) >> 1;
            if(nums[mid] > nums[mid+1]){
                high = mid;
            }
            else{
                low = mid + 1;
            }
        }
        return low;
    }
};
```

# BS On Answers
## Find Square Root of a num in log n
https://bit.ly/3JXtGcE
Given an integer **x,** find the square root of x. If **x** is not a perfect square, then return floor(√x).

**Example 1:**

**Input:**
x = 5
**Output:** 2
**Explanation:** Since, 5 is not a perfect 
square, floor of square_root of 5 is 2.

**Example 2:**

**Input:**
x = 4
**Output:** 2
**Explanation:** Since, 4 is a perfect 
square, so its square root is 2.

```cpp
class Solution{
  public:
    long long int floorSqrt(long long int x) 
    {
        // Your code goes here   long long int low = 1;
        long long int high = x;
        while(low<=high)
        {
            long long int mid = (low+high)/2;
            long long val =(mid*mid);
            if(val<=x)
            {
                low=mid+1;
            }
            else
            {
                high=mid-1;
            }
        }
        return high;
    }
};
```

## Find Nth Root of a number
https://bit.ly/3zWNyrL
You are given 2 numbers **(n , m)**; the task is to find **n√m** (nth root of m).  
**Example 1:**

**Input:** n = 2, m = 9
**Output:** 3
**Explanation:** 32 = 9

**Example 2:**

**Input:** n = 3, m = 9
**Output:** -1
**Explanation:** 3rd root of 9 is not
integer.
```cpp
class Solution{
	public:

	int NthRoot(int n, int m)
	{
	    if(m == 1) return 1;
	    if(m == 0) return 0;
	    int root = -1;
	    
	    int start = 0;
	    int end  = m;
	    
	    while(start <= end){
	        int mid = start + (end-start)/2;
	        long long midn = 1;
	        
	        for(int i = 0; i<n; ++i){
	            midn *= mid;
	            if(midn>m) break;
	        }
	        
	        if(midn == m){
	            root = mid;
	            break;
	        }
	        
	        else if(midn<m){
	            start = mid + 1;
	        }
	        else{
	            end = mid - 1;
	        }
	    }
	    return root;
	}  
};
```

## Koko Eating bananas
https://leetcode.com/problems/koko-eating-bananas/
Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.

**Example 1:**

**Input:** piles = [3,6,7,11], h = 8
**Output:** 4

**Example 2:**

**Input:** piles = [30,11,23,4,20], h = 5
**Output:** 30

**Example 3:**

**Input:** piles = [30,11,23,4,20], h = 6
**Output:** 23

```cpp
class Solution {
public:
    long long getHoursToEatAll(vector<int>&piles, int bananasPerHour)
    {
        long long totalHours = 0;
        for (int i = 0; i < piles.size(); i++)
        {
            int hoursToEatPile = ceil(piles[i] / (double)bananasPerHour);
            totalHours += hoursToEatPile;
        }
        return totalHours;
    }
    int minEatingSpeed(vector<int>& piles, int targetHours)
    {
        int low = 1, high = *(max_element(piles.begin(), piles.end()));
        int ans = -1;
        //================================================================
        while(low <= high)
        {
            int mid = low + (high - low) / 2;
            long long hoursToEatAll = getHoursToEatAll(piles, mid);
            
            if (hoursToEatAll <= targetHours)
            {
                ans = mid; 
                high = mid - 1;
            }
            else low = mid + 1;
        }
        //=================================================================
        return ans;
    }
};
```

## Minimum Days Needed to make K bouqets
https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/
You are given an integer array `bloomDay`, an integer `m` and an integer `k`.

You want to make `m` bouquets. To make a bouquet, you need to use `k` **adjacent flowers** from the garden.

The garden consists of `n` flowers, the `ith` flower will bloom in the `bloomDay[i]` and then can be used in **exactly one** bouquet.

Return _the minimum number of days you need to wait to be able to make_ `m` _bouquets from the garden_. If it is impossible to make m bouquets return `-1`.

**Example 1:**

**Input:** bloomDay = [1,10,3,10,2], m = 3, k = 1
**Output:** 3
**Explanation:** Let us see what happened in the first three days. x means flower bloomed and _ means flower did not bloom in the garden.
We need 3 bouquets each should contain 1 flower.
After day 1: [x, _, _, _, _]   // we can only make one bouquet.
After day 2: [x, _, _, _, x]   // we can only make two bouquets.
After day 3: [x, _, x, _, x]   // we can make 3 bouquets. The answer is 3.

**Example 2:**

**Input:** bloomDay = [1,10,3,10,2], m = 3, k = 2
**Output:** -1
**Explanation:** We need 3 bouquets each has 2 flowers, that means we need 6 flowers. We only have 5 flowers so it is impossible to get the needed bouquets and we return -1.

**Example 3:**

**Input:** bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
**Output:** 12
**Explanation:** We need 2 bouquets each should have 3 flowers.
Here is the garden after the 7 and 12 days:
After day 7: [x, x, x, x, _, x, x]
We can make one bouquet of the first three flowers that bloomed. We cannot make another bouquet from the last three flowers that bloomed because they are not adjacent.
After day 12: [x, x, x, x, x, x, x]
It is obvious that we can make two bouquets in different ways.

```cpp
class Solution {
public:
    int minDays(vector<int>& bloomDay, int m, int k) {
        if ((long long)m * k > bloomDay.size()) {
            return -1;
        }

        int low = 1, high = 1e9;
        while (low < high) {
            int mid = low + (high - low) / 2;

            if (canMakeBouquets(bloomDay, m, k, mid)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        return low;
    }

private:
    bool canMakeBouquets(vector<int>& bloomDay, int m, int k, int day) {
        int total = 0;
        for (int i = 0; i < bloomDay.size(); i++) {
            int count = 0;
            while (i < bloomDay.size() && count < k && bloomDay[i] <= day) {
                count++;
                i++;
            }
            if (count == k) {
                total++;
                i--;
            }
            if (total >= m) {
                return true;
            }
        }
        return false;
    }
};
```

## Find the smallest divisor
https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/
Given an array of integers `nums` and an integer `threshold`, we will choose a positive integer `divisor`, divide all the array by it, and sum the division's result. Find the **smallest** `divisor` such that the result mentioned above is less than or equal to `threshold`.

Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: `7/3 = 3` and `10/2 = 5`).

The test cases are generated so that there will be an answer.

**Example 1:**

**Input:** nums = [1,2,5,9], threshold = 6
**Output:** 5
**Explanation:** We can get a sum to 17 (1+2+5+9) if the divisor is 1. 
If the divisor is 4 we can get a sum of 7 (1+1+2+3) and if the divisor is 5 the sum will be 5 (1+1+1+2). 

**Example 2:**

**Input:** nums = [44,22,33,11,1], threshold = 5
**Output:** 44

```cpp
class Solution {
public:
    int smallestDivisor(vector<int>& nums, int threshold) {
        int l=1,r=1000001;
        int ans=0;
        while(l<=r){
            int mid=l+(r-l)/2;
            long long int sum=0;
            for(int i=0;i<nums.size();i++){
                if(nums[i]%mid==0){
                    sum+=(nums[i]/mid);
                } else{
                    sum+=(nums[i]/mid)+1;
                }
            }
            if(sum>threshold){
                l=mid+1;
            } else{
                ans=mid;
                r=mid-1;
            }
        }
        return ans;
    }
};
```

## Capacity to ship packages within D days
https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/
A conveyor belt has packages that must be shipped from one port to another within `days` days.

The `ith` package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

**Example 1:**

**Input:** weights = [1,2,3,4,5,6,7,8,9,10], days = 5
**Output:** 15
**Explanation:** A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.

**Example 2:**

**Input:** weights = [3,2,2,4,1,4], days = 3
**Output:** 6
**Explanation:** A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4

**Example 3:**

**Input:** weights = [1,2,3,1,1], days = 4
**Output:** 3
**Explanation:**
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1

```cpp
int shipWithinDays(vector<int>& weights, int D) {
        int left = 0, right = 25000000;
        for (int w: weights)
            left = max(left, w);
        while (left < right) {
            int mid = (left + right) / 2, need = 1, cur = 0;
            for (int i = 0; i < weights.size() && need <= D; cur += weights[i++])
                if (cur + weights[i] > mid)
                    cur = 0, need++;
            if (need > D) left = mid + 1;
            else right = mid;
        }
        return left;
    }
```

## Kth Missing Positive Number
https://leetcode.com/problems/kth-missing-positive-number
Given an array `arr` of positive integers sorted in a **strictly increasing order**, and an integer `k`.

Return _the_ `kth` _**positive** integer that is **missing** from this array._

**Example 1:**

**Input:** arr = [2,3,4,7,11], k = 5
**Output:** 9
**Explanation:** The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.

**Example 2:**

**Input:** arr = [1,2,3,4], k = 2
**Output:** 6
**Explanation:** The missing positive integers are [5,6,7,...]. The 2nd missing positive integer is 6.
```cpp
 int findKthPositive(vector<int>& A, int k) {
        int l = 0, r = A.size(), m;
        while (l < r) {
            m = (l + r) / 2;
            if (A[m] - 1 - m < k)
                l = m + 1;
            else
                r = m;
        }
        return l + k;
    }
```

## Aggressive Cows
https://www.spoj.com/problems/AGGRCOW/
Farmer John has built a new long barn, with N (2 <= N <= 100,000) stalls. The stalls are located along a straight line at positions x1 ... xN (0 <= xi <= 1,000,000,000).  
  
His C (2 <= C <= N) cows don't like this barn layout and become aggressive towards each other once put into a stall. To prevent the cows from hurting each other, FJ wants to assign the cows to the stalls, such that the minimum distance between any two of them is as large as possible. What is the largest minimum distance?  

### Input

_t_ – the number of test cases, then _t_ test cases follows.  
* Line 1: Two space-separated integers: N and C  
* Lines 2..N+1: Line i+1 contains an integer stall location, xi  

### Output

For each test case output one integer: the largest minimum distance.  

### Example

**Input:**

1
5 3
1
2
8
4
9

**Output:**

3

**Output details:**

FJ can put his 3 cows in the stalls at positions 1, 4 and 8,  
resulting in a minimum distance of 3.

```cpp
bool canWePlace(vector<int> &stalls, int dist, int cows) {
    int n = stalls.size(); //size of array
    int cntCows = 1; //no. of cows placed
    int last = stalls[0]; //position of last placed cow.
    for (int i = 1; i < n; i++) {
        if (stalls[i] - last >= dist) {
            cntCows++; //place next cow.
            last = stalls[i]; //update the last location.
        }
        if (cntCows >= cows) return true;
    }
    return false;
}
int aggressiveCows(vector<int> &stalls, int k) {
    int n = stalls.size(); //size of array
    //sort the stalls[]:
    sort(stalls.begin(), stalls.end());

    int low = 1, high = stalls[n - 1] - stalls[0];
    //apply binary search:
    while (low <= high) {
        int mid = (low + high) / 2;
        if (canWePlace(stalls, mid, k) == true) {
            low = mid + 1;
        }
        else high = mid - 1;
    }
    return high;
}
```

## Book Allocation problem
https://bit.ly/3MZQOct
## Problem statement

Given an array _**‘arr’**_ of integer numbers, ‘arr[i]’ represents the number of pages in the ‘i-th’ book.

  

There are _**‘m’**_ number of students, and the task is to allocate all the books to the students.

  

Allocate books in such a way that:

```
1. Each student gets at least one book.
2. Each book should be allocated to only one student.
3. Book allocation should be in a contiguous manner.
```

  

You have to allocate the book to ‘m’ students such that the maximum number of pages assigned to a student is minimum.

  

If the allocation of books is not possible, return -1.

  

**Example:**

```
Input: ‘n’ = 4 ‘m’ = 2 
‘arr’ = [12, 34, 67, 90]

Output: 113

Explanation: All possible ways to allocate the ‘4’ books to '2' students are:

12 | 34, 67, 90 - the sum of all the pages of books allocated to student 1 is ‘12’, and student two is ‘34+ 67+ 90 = 191’, so the maximum is ‘max(12, 191)= 191’.

12, 34 | 67, 90 - the sum of all the pages of books allocated to student 1 is ‘12+ 34 = 46’, and student two is ‘67+ 90 = 157’, so the maximum is ‘max(46, 157)= 157’.

12, 34, 67 | 90 - the sum of all the pages of books allocated to student 1 is ‘12+ 34 +67 = 113’, and student two is ‘90’, so the maximum is ‘max(113, 90)= 113’.

We are getting the minimum in the last case.

Hence answer is ‘113’.
```

```cpp
  
int f(vi &a, ll pages){
    ll stu = 1, pagesHolding = 0;
    for (ll i: a){
        if (pagesHolding + i <= pages){
            pagesHolding += i;
        }
        else{
            stu++;
            pagesHolding = i;
        }
    }
    return stu;
}  
int findPages(vector<int>& a, int n, int m) {
    if (m > n) return -1;  
    ll low = *max_element(a.begin(),a.end());
    ll high = accumulate(a.begin(),a.end(),0);
    while (low <= high){
        ll mid = (low + high)/2;
        ll stud = f(a,mid);
        if (stud > m){
            low = mid + 1;
        }
        else {
            high = mid -1;
        }
    }
    return low;
}
```

## Split Array- Largest Sum
https://leetcode.com/problems/split-array-largest-sum/
Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return _the minimized largest sum of the split_.

A **subarray** is a contiguous part of the array.

**Example 1:**

**Input:** nums = [7,2,5,10,8], k = 2
**Output:** 18
**Explanation:** There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

**Example 2:**

**Input:** nums = [1,2,3,4,5], k = 2
**Output:** 9
**Explanation:** There are four ways to split nums into two subarrays.
The best way is to split it into [1,2,3] and [4,5], where the largest sum among the two subarrays is only 9.

```cpp
class Solution {
public:
    bool check(vector<int>& nums, int m, int curr){
        int i=-1, n = nums.size(), count=1;
        
        int sum=0;
        
        while(++i<n && count<=m){
		//if adding the nums[i] does not make the sum greater than curr (mid), we add it to group
            if(sum+nums[i] <= curr) sum += nums[i];  
		// else we can add a new group to store the remaining element.
            else sum=nums[i], count++;
        }
		
		//If the total number of groups is less than the m, we can split the any of the current group of size >=2.
		//this will not increase the maximum sum and hence our answer will still be valid
		
        return count<=m;
    }
    
    int splitArray(vector<int>& nums, int m) {
        int e=0,s=0,mid,ans;
        for(auto x : nums){
			e += x;               //e stores the sum of the nums, upper range of Binary Search
			s=max(s,x);          //s stores the largest element in array nums, lower range of Binary Search
		}	
        ans = e; 
        
        while(s<=e){
            mid = s + (e-s)/2;
            
            if(check(nums,m,mid)){
                ans = min(ans,mid);
                e = mid - 1;
            }
            else s = mid + 1;
        }
        return ans;
    }
};
```

## Painter's Partition
https://www.codingninjas.com/studio/problems/painter-s-partition-problem_1089557
## Problem statement

Given an array/list of length _**‘n’**_, where the array/list represents the boards and each element of the given array/list represents the length of each board. Some _**‘k’**_ numbers of painters are available to paint these boards. Consider that each unit of a board takes 1 unit of time to paint.

  

You are supposed to return the area of the minimum time to get this job done of painting all the ‘n’ boards under a constraint that any painter will only paint the continuous sections of boards.

  

**Example :**

```
Input: arr = [2, 1, 5, 6, 2, 3], k = 2

Output: 11

Explanation:
First painter can paint boards 1 to 3 in 8 units of time and the second painter can paint boards 4-6 in 11 units of time. Thus both painters will paint all the boards in max(8,11) = 11 units of time. It can be shown that all the boards can't be painted in less than 11 units of time.
```

```cpp
bool ifpossible(vector<int>& arr, int n, int m,int mid){
    int studentcount=1;
    int pageSum=0;
    for(int i=0;i<n;i++){
        if(pageSum + arr[i]<=mid){
            pageSum+=arr[i];
    }
    else{
        studentcount++;
        if(studentcount>m|| arr[i]>mid){
            return false;
        }
        pageSum=arr[i];
    }
    }
    return true;
}

int findLargestMinDistance(vector<int> &boards, int k)

{
  int s=0;
    int sum=0;
    int ans =-1;
    int n= boards.size();
    for(int i=0;i<n;i++){
        sum+=boards[i];
    }

    int e=sum;
    int mid = s+(e-s)/2;
    while(s<=e && k<=n){
        if(ifpossible (boards,n,k,mid)){
            ans=mid;
            e=mid-1;
        } else {
            s = mid + 1;
        }
        mid = s + (e - s) / 2;
    }
    return ans;
}
```

## Minimising Max Distance to Gas Station
https://www.geeksforgeeks.org/problems/minimize-max-distance-to-gas-station/1
We have a horizontal number line. On that number line, we have gas **stations** at positions stations[0], stations[1], ..., stations[N-1], where **n** = size of the stations array. Now, we add **k** more gas stations so that **d**, the maximum distance between adjacent gas stations, is minimized. We have to find the smallest possible value of d. Find the answer **exactly** to 2 decimal places.

**Example 1:**

**Input:**
n = 10
stations = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
k = 9
**Output:** 0.50
**Explanation:** Each of the 9 stations can be added mid way between all the existing adjacent stations.

**Example 2:**

**Input:**
n = 10
stations = `[3,6,12,19,33,44,67,72,89,95]`   
k = 2   
**Output:** 14.00   
**Explanation:** Construction of gas stations at 8th(between 72 and 89) and 6th(between 44 and 67) locations.

```cpp
class Solution {
    long double maxi(vector<int> arr){
        long double maxi = 0;
        int n = arr.size();
        
        for(int i = 1;i < n; ++i){
            maxi = max(maxi,(long double)(arr[i] - arr[i - 1]));
        }
        
        return maxi;
    }
    
    int isPossible(vector<int>arr,long double distn){
        int cnt = 0;
        int n = arr.size();
        for(int i = 1;i < n; ++i){
            int num = (arr[i] - arr[i - 1]) / distn;
            if((arr[i] - arr[i - 1]) / distn == num * distn){
                num--;
            }
            cnt += num;
        }
        
        return cnt;
    }
    
  public:
    double findSmallestMaxDist(vector<int> &stations, int k) {
        // Code here
        int n = stations.size();
        long double low = 0;
        long double high = maxi(stations);
        long double diff = 1e-6;
        
        while(high - low > diff){
            long double mid = (low + high) / (2.0);
            int cnt = isPossible(stations,mid);
            if(cnt > k){
                low = mid;
            }
            else{
                high = mid;
            }
        }
        
        return high;
        
    }
};
```

## Median of 2 sorted arrays
https://leetcode.com/problems/median-of-two-sorted-arrays/
Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

**Input:** nums1 = [1,3], nums2 = [2]
**Output:** 2.00000
**Explanation:** merged array = [1,2,3] and median is 2.

**Example 2:**

**Input:** nums1 = [1,2], nums2 = [3,4]
**Output:** 2.50000
**Explanation:** merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int> &nums1, vector<int> &nums2) {
        int n1 = nums1.size(), n2 = nums2.size();
        
        // Ensure nums1 is the smaller array for simplicity
        if (n1 > n2)
            return findMedianSortedArrays(nums2, nums1);
        
        int n = n1 + n2;
        int left = (n1 + n2 + 1) / 2; // Calculate the left partition size
        int low = 0, high = n1;
        
        while (low <= high) {
            int mid1 = (low + high) >> 1; // Calculate mid index for nums1
            int mid2 = left - mid1; // Calculate mid index for nums2
            
            int l1 = INT_MIN, l2 = INT_MIN, r1 = INT_MAX, r2 = INT_MAX;
            
            // Determine values of l1, l2, r1, and r2
            if (mid1 < n1)
                r1 = nums1[mid1];
            if (mid2 < n2)
                r2 = nums2[mid2];
            if (mid1 - 1 >= 0)
                l1 = nums1[mid1 - 1];
            if (mid2 - 1 >= 0)
                l2 = nums2[mid2 - 1];
            
            if (l1 <= r2 && l2 <= r1) {
                // The partition is correct, we found the median
                if (n % 2 == 1)
                    return max(l1, l2);
                else
                    return ((double)(max(l1, l2) + min(r1, r2))) / 2.0;
            }
            else if (l1 > r2) {
                // Move towards the left side of nums1
                high = mid1 - 1;
            }
            else {
                // Move towards the right side of nums1
                low = mid1 + 1;
            }
        }
        
        return 0; // If the code reaches here, the input arrays were not sorted.
    }
};
```

## Kth Element of 2 sorted arrays
https://bit.ly/3Amcomr
Given two sorted arrays **arr1** and **arr2** of size **N** and **M** respectively and an element **K**. The task is to find the element that would be at the kth position of the final sorted array.  
 

**Example 1:**

**Input:**
arr1[] = {2, 3, 6, 7, 9}
arr2[] = {1, 4, 8, 10}
k = 5
**Output:**
6
**Explanation:**
The final sorted array would be -
1, 2, 3, 4, 6, 7, 8, 9, 10
The 5th element of this array is 6.

**Example 2:**

**Input:**
arr1[] = {100, 112, 256, 349, 770}
arr2[] = {72, 86, 113, 119, 265, 445, 892}
k = 7
**Output:**
256
**Explanation:**
Final sorted array is - 72, 86, 100, 112,
113, 119, 256, 265, 349, 445, 770, 892
7th element of this array is 256.

```cpp
class Solution{
    public:
    int kthElement(int arr1[], int arr2[], int n, int m, int k)
    {
        if(n > m) {
            return kthElement(arr2, arr1, m, n, k);
        }
        int low = max(0, k-m), high = min(k, n);
        int left = k;
        while(low <= high) {
            int mid1 = (low + high) >> 1;
            int mid2 = left - mid1;
            int l1 = mid1 - 1 >= 0 ? arr1[mid1 - 1] : INT_MIN;
            int l2 = mid2 - 1 >= 0 ? arr2[mid2 - 1] : INT_MIN;
            int r1 = mid1 < n ? arr1[mid1] : INT_MAX;
            int r2 = mid2 < m ? arr2[mid2] : INT_MAX;
            if(l1 <= r2 && l2 <= r1) {
                return max(l1, l2);
            }
            else if(l1 > r2) {
                high = mid1 - 1;
            }
            else {
                low = mid1 + 1;
            }
        }
        return 0;
    }
};
```

# BS on 2D Arrays

## Find the row with maximum number of 1's
https://www.geeksforgeeks.org/problems/row-with-max-1s0023/1
Given a boolean 2D array of n x m dimensions, **consisting of only 1's and 0's**, where each row is sorted. Find the 0-based index of the first row that has the maximum number of **1's**. Return the 0-based index of the first row that has the most number of 1s. If no such row exists, return **-1**.

**Examples :**

**Input:** n = 4, m = 4, arr[][] = [[0, 1, 1, 1],[0, 0, 1, 1],[1, 1, 1, 1],[0, 0, 0, 0]]
**Output:** 2
**Explanation:** Row 2 contains **4** 1's (0-based indexing).

**Input:** n = 2, m = 2, arr[][] = [[0, 0], [1, 1]]
**Output:** 1
**Explanation:** Row 1 contains **2** 1's (0-based indexing).
 

**Expected Time Complexity:** O(n+m)  
**Expected Auxiliary Space:** O(1)

```cpp
class Solution{
public:
    int lowerbound(vector<vector<int> > arr, int n,int m){
        int low = 0;
        int high = m-1;
        int ans=m;
        
        while(low<=high){
            int mid=(low+high)/2;
            if(arr[n][mid]>=1){
                ans=mid;
                high=mid-1;
            }
            else{
                low=mid+1;
            }
        }
        return ans;
    }
	int rowWithMax1s(vector<vector<int> > arr, int n, int m) {
	   int maxi = 0,ind=-1;
	   
	   for(int i=0;i<n;i++){
	       
	       int cnt = m-lowerbound(arr,i,m);
	       if(cnt>maxi) {maxi = cnt; ind=i;}
	       
	   }
	   return ind;
	}

};
```

## Search in a 2D Matrix
https://leetcode.com/problems/search-a-2d-matrix/
You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` _if_ `target` _is in_ `matrix` _or_ `false` _otherwise_.

You must write a solution in `O(log(m * n))` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

**Input:** matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

**Input:** matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
**Output:** false

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    // treat the matrix as an array, just taking care of indices
    // [0..n*m]
    // (row, col) -> row*n + col
    // i -> [i/n][i%n]
    if(matrix.empty() || matrix[0].empty())
    {
        return false;
    }
    int m = matrix.size(), n = matrix[0].size();
    int start = 0, end = m*n - 1;
    while(start <= end)
    {
        int mid = start + (end - start)/2;
        int e = matrix[mid/n][mid%n];
        if(target < e)
        {
            end = mid - 1;
        }
        else if(target > e)
        {
            start = mid + 1;
        }
        else
        {
            return true;
        }
    }
    return false;
}
```

## Search in a row and column wise sorted matrix
https://leetcode.com/problems/search-a-2d-matrix-ii/
Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

**Input:** matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid.jpg)

**Input:** matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
**Output:** false

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    if (m == 0) return false;
    int n = matrix[0].size();

    int i = 0, j = n - 1;
    while (i < m && j >= 0) {
        if (matrix[i][j] == target)
            return true;
        else if (matrix[i][j] > target) {
            j--;
        } else 
            i++;
    }
    return false;
}
```

## Find Peak Element (2D Matrix)
https://leetcode.com/problems/find-a-peak-element-ii/
A **peak** element in a 2D grid is an element that is **strictly greater** than all of its **adjacent** neighbors to the left, right, top, and bottom.

Given a **0-indexed** `m x n` matrix `mat` where **no two adjacent cells are equal**, find **any** peak element `mat[i][j]` and return _the length 2 array_ `[i,j]`.

You may assume that the entire matrix is surrounded by an **outer perimeter** with the value `-1` in each cell.

You must write an algorithm that runs in `O(m log(n))` or `O(n log(m))` time.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/08/1.png)

**Input:** mat = [[1,4],[3,2]]
**Output:** [0,1]
**Explanation:** Both 3 and 4 are peak elements so [1,0] and [0,1] are both acceptable answers.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2021/06/07/3.png)**

**Input:** mat = [[10,20,15],[21,30,14],[7,16,32]]
**Output:** [1,1]
**Explanation:** Both 30 and 32 are peak elements so [1,1] and [2,2] are both acceptable answers.

```java
class Solution {
public:
    vector<int> findPeakGrid(vector<vector<int>>& mat) {
        int n = mat.size();
        int m = mat[0].size();
        int low = 0, high = m-1;
        while(low <= high){
            int maxRow = 0;
            int midCol = (low+high) >> 1;
            for(int row=0; row<n; row++){
                if(mat[row][midCol] > mat[maxRow][midCol]){
                    maxRow = row;
                }
            }
            int currElement =  mat[maxRow][midCol];
            int leftElement = midCol == 0 ? -1 : mat[maxRow][midCol-1];
            int rightElement = midCol == m-1 ? -1 : mat[maxRow][midCol+1];
            if(currElement > leftElement && currElement > rightElement){
                return {maxRow, midCol};
            }
            else if(currElement < leftElement){
                high = midCol - 1;
            }
            else{
                low = midCol + 1;
            }
        }
        return {-1, -1};
    }
};
```

## Matrix Median
https://www.geeksforgeeks.org/problems/median-in-a-row-wise-sorted-matrix1527/1
Given a row wise sorted matrix of size **R*C** where R and C are always **odd**, find the median of the matrix.

**Example 1:**

**Input**:
R = 3, C = 3
M = [[1, 3, 5], 
     [2, 6, 9], 
     [3, 6, 9]]
**Output:** 5
**Explanation**: Sorting matrix elements gives 
us {1,2,3,3,5,6,6,9,9}. Hence, 5 is median. 

**Example 2:**

**Input:**
R = 3, C = 1
M = [[1], [2], [3]]
**Output:** 2
**Explanation**: Sorting matrix elements gives 
us {1,2,3}. Hence, 2 is median.

```cpp
class Solution{   
public:
    int median(vector<vector<int>> &mat, int r, int c){
        // Initialize the minimum and maximum elements in the matrix
        int min = mat[0][0];
        int max = mat[0][c-1];
        
        // Calculate the position of the median
        int medpos = ((r * c) + 1) / 2;
        
        // Find the overall minimum and maximum elements in the matrix
        for(int i = 1; i < r; i++){
            if(mat[i][0] < min){ min = mat[i][0]; }
            if(mat[i][c-1] > max){ max = mat[i][c-1]; }
        }
        
        // This Problem is based on “Binary search on Answer” concept.
        // Similar Problems on this concept are:
        // 1. Aggressive cows
        // 2. Painters partition
        // 3. Allocate minimum number of pages
        // 4. Peak element in unsorted array
        // 5. Matrix median/Kth element in a matrix.
        // In all of the above questions the array does not need to be sorted!
        
        // Perform binary search between min and max
        while(min <= max){
            // Initialize the middle position and the count of elements less than or equal to mid
            int midpos = 0;
            int mid = min + (max - min) / 2;
            
            // Count elements less than or equal to mid
            for(int i = 0; i < r; i++){
                midpos += upper_bound(mat[i].begin(), mat[i].end(), mid) - mat[i].begin();
            }
            
            // Adjust the search range based on the count
            if(medpos > midpos){
                min = mid + 1;
            }
            else{
                max = mid - 1;
            }
        }
        
        // The maximum element in the adjusted range is the median
        return min;
    }
};
```