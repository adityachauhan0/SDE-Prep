## Selection Sort
Given an unsorted array of size N, use selection sort to sort arr[] in increasing order.
**Example 1:**

**Input:**
N = 5
arr[] = {4, 1, 3, 9, 7}
**Output:**
1 3 4 7 9
**Explanation:**
Maintain sorted (in bold) and unsorted subarrays.
Select 1. Array becomes **1** 4 3 9 7.
Select 3. Array becomes **1 3** 4 9 7.
Select 4. Array becomes **1 3 4** 9 7.
Select 7. Array becomes **1 3 4 7** 9.
Select 9. Array becomes **1 3 4 7 9**.

```cpp
class Solution
{
    public:
    void selectionSort(int arr[], int n)
    {
       for(int i=0;i<n-1;i++)
           for(int j=i+1;j<n;j++)
               if (arr[i]>arr[j])
                   swap(arr[i],arr[j]);
   }

};
```

## Bubble Sort
Given an Integer **N** and a list **arr**. Sort the array using bubble sort algorithm.  
**Example 1:**

**Input**: 
N = 5
arr[] = {4, 1, 3, 9, 7}
**Output**: 
1 3 4 7 9

**Example 2:**

**Input**:
N = 10 
arr[] = {10, 9, 8, 7, 6, 5, 4, 3, 2, 1}
**Output**: 
1 2 3 4 5 6 7 8 9 10

```cpp
class Solution {
  public:
    // Function to sort the array using bubble sort algorithm.
    void bubbleSort(int arr[], int n) {
        // Your code here
        for(int i = 0; i < n-1 ; i++)
            for(int j = 1 ; j < n - i ; j++)
                if(arr[j-1]>arr[j])
                    swap(arr[j],arr[j-1]);
    }
};
```

## Insertion Sort
The task is to complete the **insert()** function which is used to implement Insertion Sort.

  
**Example 1:**

**Input**:
N = 5
arr[] = { 4, 1, 3, 9, 7}
**Output**:
1 3 4 7 9

**Example 2:**

**Input**:
N = 10
arr[] = {10, 9, 8, 7, 6, 5, 4, 3, 2, 1}
**Output**:
1 2 3 4 5 6 7 8 9 10

  
**Your Task:** 

You don't have to read input or print anything. Your task is to complete the function **insert()** and **insertionSort()** where **insert()** takes the array, it's size and an index i and **insertionSort()** uses insert function to sort the array in ascending order using insertion sort algorithm.

```cpp
class Solution
{
    public:
    void insert(int arr[], int i)
    {
        //code here
    }
     public:
    //Function to sort the array using insertion sort algorithm.
    void insertionSort(int arr[], int n)
    {
        for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        // Move elements of arr[0..i-1], that are greater than key,
        // to one position ahead of their current position
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
           
        }
    }
};
```

## Merge Sort
Given an array arr[], its starting position l and its ending position r. Sort the array using merge sort algorithm.  
**Example 1:**

**Input:**
N = 5
arr[] = {4 1 3 9 7}
**Output:**
1 3 4 7 9

**Example 2:**

**Input:**
N = 10
arr[] = {10 9 8 7 6 5 4 3 2 1}
**Output:**
1 2 3 4 5 6 7 8 9 10

  
**Your Task:**  
You don't need to take the input or print anything. Your task is to complete the function **merge()** which takes arr[], l, m, r as its input parameters and modifies arr[] in-place such that it is sorted from position l to position r, and function **mergeSort()** which uses merge() to sort the array in ascending order using merge sort algorithm.  
  
**Expected Time Complexity:** O(nlogn) 

**Expected Auxiliary Space:** O(n)

```cpp
class Solution
{
    public:
    void merge(int arr[], int l, int m, int r)
    {
        vector<int> temp(r-l+1);
        int first = l;
        int second = m +1;
        int k = 0;
        
        while((first <= m) && (second <= r)){
            if (arr[first] <= arr[second]){
                temp[k++] = arr[first];
                first++;
            }else{
                temp[k++] = arr[second];
                second++;
            }
        }
        
        // if some element left in first
        while(first <= m){
            temp[k++] = arr[first];
            first++;
        }
        
        // if some element left in second
        while(second <= r){
            temp[k++] = arr[second];
            second++;
        }
        
        // replaces arr with temp
        for(int i = l; i <= r; i++){
            arr[i] = temp[i-l];
        }
    }
    public:
    void mergeSort(int arr[], int l, int r)
    {
        if(l >= r){
            return;
        }
        
        int m = (l+r)/2;
        
        //left merge
        mergeSort(arr,l,m);
        
        //right merge
        mergeSort(arr,m+1,r);
        
        //calling merge to sort
        merge(arr,l,m,r);
    }
};
```

## Recursive Bubble Sort
```cpp
void bubble_sort(int arr[], int n) {
    // Base Case: range == 1.
    if (n == 1) return;

    for (int j = 0; j <= n - 2; j++) {
        if (arr[j] > arr[j + 1]) {
            int temp = arr[j + 1];
            arr[j + 1] = arr[j];
            arr[j] = temp;
        }
    }

    //Range reduced after recursion:
    bubble_sort(arr, n - 1);
}
```

## Recursive Insertion Sort
```cpp
void insertion_sort(int arr[], int i, int n) {

    // Base Case: i == n.
    if (i == n) return;

    int j = i;
    while (j > 0 && arr[j - 1] > arr[j]) {
        int temp = arr[j - 1];
        arr[j - 1] = arr[j];
        arr[j] = temp;
        j--;
    }

    insertion_sort(arr, i + 1, n);
}
```

## Quick Sort
Quick Sort is a Divide and Conquer algorithm. It picks an element as a pivot and partitions the given array around the picked pivot.  
Given an array arr[], its starting position is low (the index of the array) and its ending position is high(the index of the array).

**Note**: The **low** and **high** are inclusive.

Implement the partition() and quickSort() functions to sort the array.

**Example 1:**

**Input:** 
N = 5 
arr[] = { 4, 1, 3, 9, 7}
**Output:**
1 3 4 7 9

**Example 2:**

**Input:** 
N = 9
arr[] = { 2, 1, 6, 10, 4, 1, 3, 9, 7}
**Output:**
1 1 2 3 4 6 7 9 10

```cpp
class Solution
{
    public:
    //Function to sort an array using quick sort algorithm.
    void quickSort(int arr[], int start, int end)
    {
        if(start>=end)
          return;
        int pivot=partition(arr,start,end);
        quickSort(arr,start,pivot-1);
        quickSort(arr,pivot+1,end);
        
    }
    public:
    int partition (int arr[], int start, int end)
    {
       int pos=start;
       for(int i=start;i<=end;i++)
       {
           if(arr[i]<=arr[end])
           {
               swap(arr[i],arr[pos]);
               pos++;
           }
       }
       return pos-1;
    }
};
```

# Arrays (Mediums and Hards)

## FInd the Union Of the Array
https://bit.ly/3Ap7Onp
``` cpp
class Solution{
    public:
    //arr1,arr2 : the arrays
    // n, m: size of arrays
    //Function to return a list containing the union of the two arrays. 
    vector<int> findUnion(int arr1[], int arr2[], int n, int m)
    {
        //Your code here
        //return vector with correct order of elements
        
        vector<int> ans;
        int i=0,j=0;
        
        while(i<n and j<m){
            
            while(i+1<n and arr1[i]==arr1[i+1]) i++;
            while(j+1<m and arr2[j]==arr2[j+1]) j++;
            
            if(arr1[i]==arr2[j]){
                ans.push_back(arr1[i]);
                i++;
                j++;
            }
            else if(arr1[i] < arr2[j]){
                
                ans.push_back(arr1[i]);
                i++;
            }
            else{
                ans.push_back(arr2[j]);
                j++;
            }
        }
    
        while(i<n){  
            while(i+1<n and arr1[i]==arr1[i+1]) i++;
            ans.push_back(arr1[i++]);   
        }
        while(j<m){
            while(j+1<m and arr2[j]==arr2[j+1]) j++;
            ans.push_back(arr2[j++]); 
        }
        return ans;
    }
};
```

## Longest Subarray With sum K (+ve or -ve)
https://practice.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1
Given an array **arr** containing **n** integers and an integer **k**. Your task is to find the length of the longest Sub-Array with the sum of the elements equal to the given value **k**.

**Examples:**  
 

**Input :**
arr[] = {10, 5, 2, 7, 1, 9}, k = 15
**Output :** 4
**Explanation:**
The sub-array is **{5, 2, 7, 1}**.

**Input :** 
arr[] = {-1, 2, 3}, k = 6
**Output :** 0
**Explanation:** 
There is no such sub-array with sum 6.

**Expected Time Complexity:** O(n).  
**Expected Auxiliary Space:** O(n).

```cpp
class Solution
{
    public:
    int lenOfLongSubarr(int A[],  int N, int K) 
    { 
        for(int i=1; i<N; i++)
        {
            A[i] += A[i-1];
        }
        
        unordered_map<int, int> hashMap;
        hashMap[0]=-1;
        int gmax = 0;
        
        for(int i=0; i<N; i++)
        {
            int what = A[i] - K;
            if(hashMap.find(what)!=hashMap.end())
            {
                gmax = max(gmax, i - hashMap[what] );
            }
            
            if(hashMap.find(A[i])==hashMap.end())
            {
                hashMap[A[i]] = i;
            }
        }
        return gmax;
    } 
};
```

## Sort an arrays of 0's, 1's and 2's
https://leetcode.com/problems/sort-colors/
Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**

**Input:** nums = [2,0,2,1,1,0]
**Output:** [0,0,1,1,2,2]

**Example 2:**

**Input:** nums = [2,0,1]
**Output:** [0,1,2]

**Constraints:**

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either `0`, `1`, or `2`.
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0, mid = 0, high = nums.size() - 1;
        while (mid <= high) {
            switch (nums[mid]) {
                case 0:
                    swap(nums[low++], nums[mid++]);
                    break;
                case 1:
                    mid++;
                    break;
                case 2:
                    swap(nums[mid], nums[high--]);
                    break;
            }
        }
    }

};
```

## Majority Element (>n/2 times)
https://leetcode.com/problems/majority-element/
Moore Voting Algorithm
Given an array `nums` of size `n`, return _the majority element_.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

**Input:** nums = [3,2,3]
**Output:** 3

**Example 2:**

**Input:** nums = [2,2,1,1,1,2,2]
**Output:** 2

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int count = 0;
        int candidate = 0;
        
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }
        return candidate;
    }
};
```

## Kadane Max Subarray Sum
https://leetcode.com/problems/maximum-subarray/
Given an integer array `nums`, find the subarray with the largest sum, and return _its sum_.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**

**Input:** nums = [1]
**Output:** 1
**Explanation:** The subarray [1] has the largest sum 1.

**Example 3:**

**Input:** nums = [5,4,-1,7,8]
**Output:** 23
**Explanation:** The subarray [5,4,-1,7,8] has the largest sum 23.

```cpp
int maxSubArray(vector<int>& a) {

    int ans = INT_MIN;
    int sm = 0;
    for (int i: a){
        sm += i;
        ans = max(ans,sm);
        sm = max(sm,0);
    }

    return ans;
    }
```

## Print the subarray with max sum
https://bit.ly/3SLFFhs
Given an array **arr[]**, with index ranging from **0 to n-1**, select any two indexes, **i** and **j** such that **i < j** . From the subarray **arr[i...j]**, select the **two minimum numbers** and add them, you will get the **score** for that subarray. Return the **maximum possible score** across all the subarrays of array **arr[]**.  
 

**Examples :**

**Input :** arr[] = [4, 3, 1, 5, 6]
**Output :** 11
**Explanation :** Subarrays with smallest and second smallest are:- [4, 3] smallest = 3,second smallest = 4
[4, 3, 1] smallest = 1, second smallest = 3
[4, 3, 1, 5] smallest = 1, second smallest = 3
[4, 3, 1, 5, 6] smallest = 1, second smallest = 3
[3, 1] smallest = 1, second smallest = 3
[3, 1, 5] smallest = 1, second smallest = 3
[3, 1, 5, 6] smallest = 1, second smallest = 3
[1, 5] smallest = 1, second smallest = 5
[1, 5, 6] smallest = 1, second smallest = 5
[5, 6] smallest = 5, second smallest = 6
Maximum sum among all above choices is, 5 + 6 = 11.

**Input :** arr[] = [5, 4, 3, 1, 6] 
**Output :** 9

  
**Expected Time Complexity:** O(n)  
**Expected Auxiliary Space:** O(1)

```cpp
long long maxSubarraySum(int arr[], int n) {
    long long maxi = LONG_MIN; // maximum sum
    long long sum = 0;

    int start = 0;
    int ansStart = -1, ansEnd = -1;
    for (int i = 0; i < n; i++) {

        if (sum == 0) start = i; // starting index

        sum += arr[i];

        if (sum > maxi) {
            maxi = sum;

            ansStart = start;
            ansEnd = i;
        }

        // If sum < 0: discard the sum calculated
        if (sum < 0) {
            sum = 0;
        }
    }

    //printing the subarray:
    cout << "The subarray is: [";
    for (int i = ansStart; i <= ansEnd; i++) {
        cout << arr[i] << " ";
    }
    cout << "]n";

    // To consider the sum of the empty subarray
    // uncomment the following check:

    //if (maxi < 0) maxi = 0;

    return maxi;
}
```

## Rearrange Array elements by sign
https://leetcode.com/problems/rearrange-array-elements-by-sign
You are given a **0-indexed** integer array `nums` of **even** length consisting of an **equal** number of positive and negative integers.

You should return the array of nums such that the the array follows the given conditions:

1. Every **consecutive pair** of integers have **opposite signs**.
2. For all integers with the same sign, the **order** in which they were present in `nums` is **preserved**.
3. The rearranged array begins with a positive integer.

Return _the modified array after rearranging the elements to satisfy the aforementioned conditions_.

**Example 1:**

**Input:** nums = [3,1,-2,-5,2,-4]
**Output:** [3,-2,1,-5,2,-4]
**Explanation:**
The positive integers in nums are [3,1,2]. The negative integers are [-2,-5,-4].
The only possible way to rearrange them such that they satisfy all conditions is [3,-2,1,-5,2,-4].
Other ways such as [1,-2,2,-5,3,-4], [3,1,2,-2,-5,-4], [-2,3,-5,1,-4,2] are incorrect because they do not satisfy one or more conditions.  

**Example 2:**

**Input:** nums = [-1,1]
**Output:** [1,-1]
**Explanation:**
1 is the only positive integer and -1 the only negative integer in nums.
So nums is rearranged to [1,-1].

```cpp
class Solution {
public:
    vector<int> rearrangeArray(vector<int>& nums) {
        vector<int>ans(nums.size(),0);
        int pos=0,neg=1;
        
        for(int i=0;i<nums.size();i++){
            if(nums[i]>0){
                ans[pos] = nums[i];
                pos+=2;
            }else{
                ans[neg] = nums[i];
                neg+=2;
            }
        }
        return ans;
    }
};
```

## Next Permutation
https://leetcode.com/problems/next-permutation/
A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [1,3,2]

**Example 2:**

**Input:** nums = [3,2,1]
**Output:** [1,2,3]

**Example 3:**

**Input:** nums = [1,1,5]
**Output:** [1,5,1]

```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size(), index = -1;
        for(int i=n-2; i>=0; i--){
            if(nums[i] < nums[i+1]){
                index = i;
                break;
            }
        }
        for(int i=n-1; i>=index && index != -1; i--){
            if(nums[i] > nums[index]){
                swap(nums[i], nums[index]);
                break;
            }
        }
        reverse(nums.begin() + index + 1, nums.end());
    }
};
```

## Leaders in an array problem
https://bit.ly/3bZqbGc
Given an array **arr** of **n** positive integers, your task is to find all the leaders in the array. An element of the array is considered a leader if it is greater than all the elements on its right side or if it is equal to the maximum element on its right side. The rightmost element is always a leader.

**Examples  
**

**Input:** n = 6, arr[] = {16,17,4,3,5,2}
**Output:** 17 5 2
**Explanation:** Note that there is nothing greater on the right side of 17, 5 and, 2.

**Input:** n = 5, arr[] = {10,4,2,4,1}
**Output:** 10 4 4 1  
**Explanation:** Note that both of the 4s are in output, as to be a leader an equal element is also allowed on the right. side

**Input:** n = 4, arr[] = {5, 10, 20, 40}   
**Output:** 40  
**Explanation:** When an array is sorted in increasing order, only the rightmost element is leader.

**Input:** n = 4, arr[] = {30, 10, 10, 5} **Output:** 30 10 10 5  
**Explanation:** When an array is sorted in non-increasing order, all elements are leaders.

**Expected Time Complexity:** O(n)  
**Expected Auxiliary Space:** O(n)

```cpp
class Solution {
    // Function to find the leaders in the array.
  public:
    vector<int> leaders(int n, int arr[]) {
       vector<int> ans;
       int maxi=arr[n-1];
       for(int i=n-1;i>=0;i--){
           if(arr[i]>=maxi){
               ans.push_back(arr[i]);
               maxi=max(maxi,arr[i]);
           }
       }
       reverse(ans.begin(),ans.end());
       return ans;
       
    }
};
```

## Longest Consecutive Sequence in an Array
https://leetcode.com/problems/longest-consecutive-sequence
Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

**Input:** nums = [100,4,200,1,3,2]
**Output:** 4
**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

**Example 2:**

**Input:** nums = [0,3,7,2,5,8,4,6,0,1]
**Output:** 9
```cpp

class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> set;
        for(int num : nums){
            set.insert(num);
        }
        int longestConsecutiveSequence = 0;
        for(int num : nums){
            if(set.find(num-1) == set.end()){
                int currentNumber = num;
                int currentConsecutiveSequence = 1;
                while(set.find(currentNumber+1) != set.end()){
                    currentNumber++;
                    currentConsecutiveSequence++;
                }
                longestConsecutiveSequence = max(longestConsecutiveSequence, currentConsecutiveSequence);
            }
        }
        return longestConsecutiveSequence;
    }
};
```

## Rotate Matrix By 90 degrees
https://leetcode.com/problems/rotate-image/
You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

**Input:** matrix = [[1,2,3],[4,5,6],[7,8,9]]
**Output:** [[7,4,1],[8,5,2],[9,6,3]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

**Input:** matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
**Output:** [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

```cpp
class Solution {

public:
    void rotate(vector<vector<int>>& matrix) {
        for (int i = 0; i < matrix.size(); i++)
            for (int j =i; j < matrix[0].size(); j++) swap(matrix[i][j],matrix[j][i]);

        for (int i = 0; i < matrix.size();i++)
            reverse(matrix[i].begin(),matrix[i].end());      
    }
};
```

## Print Matrix in a spiral manner
https://leetcode.com/problems/spiral-matrix/
Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

**Input:** matrix = [[1,2,3],[4,5,6],[7,8,9]]
**Output:** [1,2,3,6,9,8,7,4,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

**Input:** matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
**Output:** [1,2,3,4,8,12,11,10,9,5,6,7]

```cpp
class Solution {
    #define ll int
    #define ld long double
    #define st string
    #define vi vector<ll>
    #define li list<ll>
    #define pb push_back
    #define endl "\n"
    #define vp vector<pair<ll,ll>>
public:
    vector<int> spiralOrder(vector<vector<int>>& a) {
        ll n = a.size();
        ll m = a[0].size();
        ll top =0, left = 0;
        ll bottom = n-1, right = m-1;
        vi ans = {};

        while (top <= bottom && left <= right){
            //printing the top side
            for (ll i = left; i <= right; i++){
                ans.pb(a[top][i]);
            }
            top++;

            //printing right side
            for (ll i = top; i <= bottom; i++){
                ans.pb(a[i][right]);
            }
            right--;

            //printing bottom side
            if (top <= bottom){
                for (ll i = right; i >= left; i--){
                    ans.pb(a[bottom][i]);
                    
                }
                bottom--;
            }
            //printing left side
            if (left <= right){
                for (ll i = bottom; i >= top; i--){
                    ans.pb(a[i][left]);
                }
                left++;
            }
        }
        return ans;
    }
};
```

## Pascal's Triangle
https://leetcode.com/problems/pascals-triangle/
Given an integer `numRows`, return the first numRows of **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** numRows = 5
**Output:** [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]

**Example 2:**

**Input:** numRows = 1
**Output:** [[1]]

**Constraints:**

- `1 <= numRows <= 30`

```cpp
class Solution {
public:
    vector<vector<int> > generate(int numRows) {
        vector<vector<int>> r(numRows);

        for (int i = 0; i < numRows; i++) {
            r[i].assign(i + 1, 1);    

            for (int j = 1; j < i; j++)
                r[i][j] = r[i - 1][j - 1] + r[i - 1][j];
        }

        return r;
    }
};
```

## Majority Element [n/3 times]
https://leetcode.com/problems/majority-element-ii/
Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

**Example 1:**

**Input:** nums = [3,2,3]
**Output:** [3]

**Example 2:**

**Input:** nums = [1]
**Output:** [1]

**Example 3:**

**Input:** nums = [1,2]
**Output:** [1,2]

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int num1 = INT_MIN, num2 = INT_MIN;
        int count1 = 0, count2 = 0;
        for(auto element : nums){
            if(num1 == element){
                count1++;
            }
            else if(num2 == element){
                count2++;
            }
            else if(count1 == 0){
                num1 = element;
                count1 = 1;
            }
            else if(count2 == 0){
                num2 = element;
                count2 = 1;
            }
            else{
                count1--;
                count2--;
            }
        }
        vector<int> output;
        int countMajority = nums.size()/3;
        count1 = 0, count2 = 0;
        for(auto element : nums){
            if(num1 == element){
                count1++;
            }
            if(num2 == element){
                count2++;
            }
        }
        if(count1 > countMajority){
            output.push_back(num1);
        }
        if(count2 > countMajority){
            output.push_back(num2);
        }
        return output;
    }
};
```

## 3 Sum
https://leetcode.com/problems/3sum/
Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = [-1,0,1,2,-1,-4]
**Output:** [[-1,-1,2],[-1,0,1]]
**Explanation:** 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.

**Example 2:**

**Input:** nums = [0,1,1]
**Output:** []
**Explanation:** The only possible triplet does not sum up to 0.

**Example 3:**

**Input:** nums = [0,0,0]
**Output:** [[0,0,0]]
**Explanation:** The only possible triplet sums up to 0.

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin() , nums.end());     //Sorted Array
        if(nums.size() < 3){                // Base Case 1
            return {};
        }
        if(nums[0] > 0){                    // Base Case 2
            return {};
        }
        unordered_map<int , int> hashMap;
        for(int i = 0 ; i < nums.size() ; ++i){     //Hashing of Indices
            hashMap[nums[i]] = i;
        }
        vector<vector<int>> answer;
        for(int i = 0 ; i < nums.size() - 2 ; ++i){     //Traversing the array to fix the number.
            if(nums[i] > 0){     //If number fixed is +ve, stop there because we can't make it zero by searching after it.
                break;
            }
            for(int j = i + 1 ; j < nums.size() - 1 ; ++j){     //Fixing another number after first number
                int required = -1*(nums[i] + nums[j]);    //To make sum 0, we would require the -ve sum of both fixed numbers.
                if(hashMap.count(required) && hashMap.find(required)->second > j){ //If it exists in hashmap and its last occurrence index > 2nd fixed index, we found our triplet.
                    answer.push_back({nums[i] , nums[j] , required});
                }
                j = hashMap.find(nums[j])->second; //Update j to last occurence of 2nd fixed number to avoid duplicate triplets.
            }
            i = hashMap.find(nums[i])->second;     //Update i to last occurence of 1st fixed number to avoid duplicate triplets.
        }
        return answer;  //Return answer vector.
    }
};
```

## 4Sum
https://leetcode.com/problems/4sum/
Given an array `nums` of `n` integers, return _an array of all the **unique** quadruplets_ `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,0,-1,0,-2,2], target = 0
**Output:** [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]

**Example 2:**

**Input:** nums = [2,2,2,2,2], target = 8
**Output:** [[2,2,2,2]]

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> output;
        for(int i=0; i<n-3; i++){
            for(int j=i+1; j<n-2; j++){
                long long newTarget = (long long)target - (long long)nums[i] - (long long)nums[j];
                int low = j+1, high = n-1;
                while(low < high){
                    if(nums[low] + nums[high] < newTarget){
                        low++;
                    }
                    else if(nums[low] + nums[high] > newTarget){
                        high--;
                    }
                    else{
                        output.push_back({nums[i], nums[j], nums[low], nums[high]});
                        int tempIndex1 = low, tempIndex2 = high;
                        while(low < high && nums[low] == nums[tempIndex1]) low++;
                        while(low < high && nums[high] == nums[tempIndex2]) high--;
                    }
                }
                while(j+1 < n && nums[j] == nums[j+1]) j++;
            }
            while(i+1 < n && nums[i] == nums[i+1]) i++;
        }
        return output;
    }
};
```

## Largest Subarray with sum 0
https://bit.ly/3w5QSzC
Given an array having both positive and negative integers. The task is to compute the length of the largest subarray with sum 0.

**Examples:**

**Input:** arr[] = {15,-2,2,-8,1,7,10,23}, n = 8
**Output:** 5
**Explanation:** The largest subarray with sum 0 is -2 2 -8 1 7.

**Input:** arr[] = {2,10,4}, n = 3
**Output:** 0
**Explanation:** There is no subarray with 0 sum.

**Input:** arr[] = {1, 0, -4, 3, 1, 0}, n = 6
**Output:** 5
**Explanation:** The subarray is 0 -4 3 1 0.

**Expected Time Complexity:** O(n).  
**Expected Auxiliary Space:** O(n).

```cpp
/*You are required to complete this function*/

class Solution {
  public:
    int maxLen(vector<int>& arr, int n) {
        // Your code here
        unordered_map<int,int>mp;
        int sum = 0,ans=0;
        mp[0]=-1;
        for(int i=0;i<arr.size();i++){
            
            sum+=arr[i];
            
            if(mp.find(sum)!=mp.end()){
                ans = max(ans,i-mp[sum]);
            }
            else{
                mp[sum]=i;
            }
        }
        
        return ans;
    }
};
```

## Count subarrays with XOR K
https://www.interviewbit.com/problems/subarray-with-given-xor/
```cpp
#include <bits/stdc++.h>
using namespace std;

int subarraysWithXorK(vector<int> a, int k) {
    int n = a.size(); //size of the given array.
    int xr = 0;
    map<int, int> mpp; //declaring the map.
    mpp[xr]++; //setting the value of 0.
    int cnt = 0;

    for (int i = 0; i < n; i++) {
        // prefix XOR till index i:
        xr = xr ^ a[i];

        //By formula: x = xr^k:
        int x = xr ^ k;

        // add the occurrence of xr^k
        // to the count:
        cnt += mpp[x];

        // Insert the prefix xor till index i
        // into the map:
        mpp[xr]++;
    }
    return cnt;
}

int main()
{
    vector<int> a = {4, 2, 2, 6, 4};
    int k = 6;
    int ans = subarraysWithXorK(a, k);
    cout << "The number of subarrays with XOR k is: "
         << ans << "\n";
    return 0;
}
```

## Merge Overlapping Subintervals
https://leetcode.com/problems/merge-intervals/
Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = [[1,3],[2,6],[8,10],[15,18]]
**Output:** [[1,6],[8,10],[15,18]]
**Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

**Example 2:**

**Input:** intervals = [[1,4],[4,5]]
**Output:** [[1,5]]
**Explanation:** Intervals [1,4] and [4,5] are considered overlapping.

```cpp
class Solution {
public:

vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if (intervals.size() == 1) return intervals;
        sort(intervals.begin(), intervals.end());

    vector<vector<int>> ans;
    int l = intervals[0][0], r = intervals[0][1];
    int n = intervals.size();
    for (int i = 1; i < n; ++i)
    {
        if (intervals[i][0] <= r){
            r = max(r,intervals[i][1]);
            if (i == n-1){
                ans.push_back({l,r});
            }
        }
        else {
            ans.push_back({l,r});
            l = intervals[i][0];
            r = intervals[i][1];
            if (i == n-1){
                ans.push_back({l,r});
            }
        }
    }
    return ans;  
    }
};
```

## Merge two sorted arrays without extra space
https://leetcode.com/problems/merge-sorted-array/
You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

**Input:** nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
**Output:** [1,2,2,3,5,6]
**Explanation:** The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

**Example 2:**

**Input:** nums1 = [1], m = 1, nums2 = [], n = 0
**Output:** [1]
**Explanation:** The arrays we are merging are [1] and [].
The result of the merge is [1].
```cpp
class Solution {

public:

void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {

    int p1 = m - 1;  // Pointer for nums1
    int p2 = n - 1;  // Pointer for nums2
    int p = m + n - 1;  // Pointer for the merged array  
    // Start from the end and work backwards
    while (p2 >= 0) {
        if (p1 >= 0 && nums1[p1] > nums2[p2]) {
            nums1[p] = nums1[p1];
            p1--;
        } else {
            nums1[p] = nums2[p2];
            p2--;
        }
        p--;
    }
}
};
```

## Find Repeated and Missing Number
https://bit.ly/3zWZoCs
Given an unsorted array _arr_ of size n of positive integers. One number '**A**' from set {1, 2,....,N} is missing and one number '**B**' occurs twice in array. Find these two numbers.  
Your task is to complete the function _findTwoElement()_ which takes the array of integers arr and n as parameters and returns an array of integers of size 2 denoting the answer (_The first index contains B and second index contains A_)  

**Examples  
**

**Input:** n = 2 arr[] = {2, 2}
**Output:** 2 1
**Explanation:** Repeating number is 2 and smallest positive missing number is 1.

**Input:** n = 3 arr[] = {1, 3, 3} 
**Output:** 3 2
**Explanation:** Repeating number is 3 and smallest positive missing number is 2.

**Expected Time Complexity:** O(n)  
**Expected Auxiliary Space:** O(1)

**Constraints:**  
2 ≤ n ≤ 105  
1 ≤ arr[i] ≤ n

```cpp
class Solution{
public:
    vector<int> findTwoElement(vector<int> arr, int n) {
        // code here
         vector<int> ans(2);
        unordered_map<int, int> hash;

        // Counting occurrences of each element
        for (int i = 0; i < n; i++) {
            hash[arr[i]]++;
        }

        // Find the repeating element
        for (int i = 0; i < n; i++) {
            if (hash[arr[i]] > 1) {
                ans[0] = arr[i];
                break;
            }
        }

        // Find the missing element
        for (int i = 1; i <= n; i++) {
            if (hash.find(i) == hash.end()) {
                ans[1] = i;
                break;
            }
        }

        return ans;
    
    }
};
```

## Count Inversions
https://bit.ly/3PtLWLM
Given an array of integers. Find the Inversion Count in the array.  Two array elements arr[i] and arr[j] form an inversion if arr[i] > arr[j] and i < j.

_**Inversion Count**:_ For an array, inversion count indicates how far (or close) the array is from being sorted. If the array is already sorted then the inversion count is 0.  
If an array is sorted in the reverse order then the inversion count is the maximum. 

**Examples:**

**Input**: n = 5, arr[] = {2, 4, 1, 3, 5}  
**Output**: 3
**Explanation**: The sequence 2, 4, 1, 3, 5 has three inversions (2, 1), (4, 1), (4, 3).

**Input**: n = 5, arr[] = {2, 3, 4, 5, 6}  
**Output**: 0
**Explanation**: As the sequence is already sorted so there is no inversion count.

**Input**: n = 3, arr[] = {10, 10, 10}  
**Output**: 0
**Explanation**: As all the elements of array are same, so there is no inversion count.

**Expected Time Complexity:** O(nLogn).  
**Expected Auxiliary Space:** O(n).

```cpp
class Solution {
  public:
    // arr[]: Input Array
    // N : Size of the Array arr[]
    // Function to count inversions in the array.
    long long merge(long long arr[],vector<long long int>&temp,int start,int mid,int end){
        int i=start;
        int j=mid+1;
        int k=start;
        long long c=0;
        while(i<=mid && j<=end){
            if(arr[i]<=arr[j])
            temp[k++]=arr[i++];
            else{
                temp[k++]=arr[j++];
                c+=mid-i+1;
            }
        }
        while(i<=mid){
           temp[k++]=arr[i++];   
        }
        while(j<=end){
           temp[k++]=arr[j++];  
        }
        while(start<=end){
            arr[start]=temp[start];
            start++;
        }
        return c;
    }
    long long mergesort(long long arr[],vector<long long int>&temp,int start,int end){
        if(start>=end) return 0;
        long long count=0;
        int mid=start+(end-start)/2;
      count+=  mergesort(arr,temp,start,mid);
      count+=  mergesort(arr,temp,mid+1,end);
       count+= merge(arr,temp,start,mid,end);
       return count;
    }
    long long int inversionCount(long long arr[], int n) {
       vector<long long int>temp(n);
       long long int k=0;
     k=  mergesort(arr,temp,0,n-1);
     return k;
       
    }
};
```

## Reverse Pairs
https://leetcode.com/problems/reverse-pairs
Given an integer array `nums`, return _the number of **reverse pairs** in the array_.

A **reverse pair** is a pair `(i, j)` where:

- `0 <= i < j < nums.length` and
- `nums[i] > 2 * nums[j]`.

**Example 1:**

**Input:** nums = [1,3,2,3,1]
**Output:** 2
**Explanation:** The reverse pairs are:
(1, 4) --> nums[1] = 3, nums[4] = 1, 3 > 2 * 1
(3, 4) --> nums[3] = 3, nums[4] = 1, 3 > 2 * 1

**Example 2:**

**Input:** nums = [2,4,3,5,1]
**Output:** 3
**Explanation:** The reverse pairs are:
(1, 4) --> nums[1] = 4, nums[4] = 1, 4 > 2 * 1
(2, 4) --> nums[2] = 3, nums[4] = 1, 3 > 2 * 1
(3, 4) --> nums[3] = 5, nums[4] = 1, 5 > 2 * 1

```cpp
class Solution {
private: 
    void merge(vector<int>& nums, int low, int mid, int high, int& reversePairsCount){
        int j = mid+1;
        for(int i=low; i<=mid; i++){
            while(j<=high && nums[i] > 2*(long long)nums[j]){
                j++;
            }
            reversePairsCount += j-(mid+1);
        }
        int size = high-low+1;
        vector<int> temp(size, 0);
        int left = low, right = mid+1, k=0;
        while(left<=mid && right<=high){
            if(nums[left] < nums[right]){
                temp[k++] = nums[left++];
            }
            else{
                temp[k++] = nums[right++];
            }
        }
        while(left<=mid){
            temp[k++] = nums[left++]; 
        }
        while(right<=high){
            temp[k++] = nums[right++]; 
        }
        int m=0;
        for(int i=low; i<=high; i++){
            nums[i] = temp[m++];
        }
    }

    void mergeSort(vector<int>& nums, int low, int high, int& reversePairsCount){
        if(low >= high){
            return;
        }
        int mid = (low + high) >> 1;
        mergeSort(nums, low, mid, reversePairsCount);
        mergeSort(nums, mid+1, high, reversePairsCount);
        merge(nums, low, mid, high, reversePairsCount);
    }
public:
    int reversePairs(vector<int>& nums) {
        int reversePairsCount = 0;
        mergeSort(nums, 0, nums.size()-1, reversePairsCount);
        return reversePairsCount;
    }
};
```

## MaxProduct Subarray
https://leetcode.com/problems/maximum-product-subarray/
Given an integer array `nums`, find a subarray that has the largest product, and return _the product_.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [2,3,-2,4]
**Output:** 6
**Explanation:** [2,3] has the largest product 6.

**Example 2:**

**Input:** nums = [-2,0,-1]
**Output:** 0
**Explanation:** The result cannot be 2, because [-2,-1] is not a subarray.

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxi = INT_MIN;
        int prod=1;

        for(int i=0;i<nums.size();i++)
        {
          prod*=nums[i];
          maxi=max(prod,maxi);
          if(prod==0)
           prod=1;
        }
        prod=1;
        for(int i=nums.size()-1;i>=0;i--)
        {
          prod*=nums[i];

          maxi=max(prod,maxi);
          if(prod==0)
           prod=1;
        }
        return maxi;
    }
};
```

