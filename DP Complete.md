## Climbing Stairs
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

**Input:** n = 2
**Output:** 2
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Example 2:**

**Input:** n = 3
**Output:** 3
**Explanation:** There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
https://leetcode.com/problems/climbing-stairs/
```cpp
class Solution {
public:
    int climbStairs(int n) {
         // lets tabulate kar dete hai
	vector<int > dp(n+2, 0);
	dp[n] = 1;
	dp[n+1] = 0;
	for (int i =  n-1; i >= 0; i--){
		dp[i] = dp[i+1] + dp[i+2];
	}
    return dp[0];
    }
};
```

## Frog Jump
 Geek wants to climb from the 0th stair to the (n-1)th stair. At a time the Geek can climb either one or two steps. A height[N] array is also given. Whenever the geek jumps from stair i to stair j, the energy consumed in the jump is abs(height[i]- height[j]), where abs() means the absolute difference. return the minimum energy that can be used by the Geek to jump from stair 0 to stair N-1.

**Example:**
**Input:**
n = 4
height = {10 20 30 10}
**Output:**
20
**Explanation:**
`Geek jump from 1st to 2nd stair(|20-10| = 10 energy lost). Then a jump from the 2nd to the last stair(|10-20| = 10 energy lost). so, total energy lost is 20 which is the minimum.`

**`Your Task:`**  
You don't need to read input or print anything. Your task is to complete the function **MinimumEnergy()** which takes the array **height**, and integer **n,** and returns the minimum energy that is lost.
https://bit.ly/3Xn0Kkw
```cpp
int minimumEnergy(vector<int>& height, int n) {
        dp.resize(n,-1);
        
        dp[0]=0;
        for(int index=1;index<n;index++){
            int a=dp[index-1]+abs(height[index-1]-height[index]);
        
            int b=1e9;
            if(index-2>=0) b=dp[index-2]+abs(height[index-2]-height[index]);
            dp[index]=min(a,b);
        }
        return dp[n-1];
    }
```
## Frog Jump with k distance
There are n stones and an array of heights and Geek is standing at stone 1 and can jump to one of the following: Stone i+1, i+2, ... i+k stone, where k is the maximum number of steps that can be jumped and cost will be [hi-hj] is incurred, where j is the stone to land on. Find the minimum possible total cost incurred before the Geek reaches Stone n.

**Examples :**
**Input:** n = 5, k = 3 heights = {10, 30, 40, 50, 20}
**Output:** 30
**Explanation:** Geek will follow the path 1->2->5, the total cost would be | 10-30| + |30-20| = 30, which is minimum

**Input:** n = 3, k = 1 heights = {10,20,10}
**Output:** 20
**Explanation:** Geek will follow the path 1->2->3, the total cost would be |10 - 20| + |20 - 10| = 20.

**Your Task:**  
You don't need to read input or print anything. Your task is to complete the function **minimizeCost()** which takes the array **height**, and integer **n,** and integer k and returns the minimum energy that is lost.
https://bit.ly/3GyNRya
```cpp
class Solution {
  public:
    int minimizeCost(vector<int>& height, int n, int k) {
        // Code here
        vector<int> dp(n, -1);
        dp[0] = 0;
        for (int i = 1; i < n; i++) {
            int minSteps = INT_MAX;
            for (int j = 1; j <= k; j++) {
                if (i - j >= 0) {
                    int jump = dp[i - j] + abs(height[i] - height[i - j]);
                    minSteps = min(minSteps, jump);
                }
            }
            dp[i] = minSteps;
        }
        return dp[n - 1];
    }
};
```

## Maximum Sum of non-adjacent elements
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [1,2,3,1]
**Output:** 4
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 2:**

**Input:** nums = [2,7,9,3,1]
**Output:** 12
**Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
https://leetcode.com/problems/house-robber/
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {

        int n = nums.size();
        if (n == 1) return nums[0]; 
        vector<int> dp(n+2);
        dp[n] = 0;
        dp[n+1] = 0; //no more houses left to rob

        for (int i = n-1; i >= 0; i--){
            int rob = nums[i] + dp[i+2];
            int notrob = dp[i+1];
            dp[i] = max(rob,notrob);
        }

        return max(dp[0],dp[1]);
    }
};
```

## Maximum Sum of non-adjacent elements (Circle)
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [2,3,2]
**Output:** 3
**Explanation:** You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

**Example 2:**

**Input:** nums = [1,2,3,1]
**Output:** 4
**Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 3:**

**Input:** nums = [1,2,3]
**Output:** 3
https://leetcode.com/problems/house-robber-ii/
```cpp
class Solution {
public:
    int f(vector<int>& nums, int s, int n, vector<int>& dp){
        if(n<s){
            //return nums[s];
            return 0;
        }
        if(dp[n]!=-1){
            return dp[n];
        }
        int p=nums[n];
        if(n>=2){
            p+=f(nums,s,n-2,dp);
        }
        int np=f(nums,s,n-1,dp);
        return dp[n]=max(p,np);
    }
    int rob(vector<int>& nums) {
        int n=nums.size();
        if (n == 1) {
            return nums[0];
        }
        vector<int> dp1(n,-1);
        vector<int> dp2(n,-1);
        int a=f(nums,1,n-1,dp1);
        int b=f(nums,0,n-2,dp2);
        return max(a,b);
    }
};
```

## Ninja Training
Geek is going for **n** day training program. He can perform any one of these three activities Running, Fighting, and Learning Practice. Each activity has some point on each day. As Geek wants to improve all his skills, he can't do the same activity on two consecutive days. Help Geek to maximize his merit points as you are given a 2D array of points **points,** corresponding to each day and activity.

**Example:**
**Input:**
n = 3
points = [[1,2,5],[3,1,1],[3,3,3]]
**Output:**
11
**Explanation:**
Geek will learn a new move and earn 5 point then on second
day he will do running and earn 3 point and on third day
he will do fighting and earn 3 points so, maximum point is 11.  
  

**Example:**
**Input:**
n = 3
points = [[1,2,5],[3,1,1],[3,2,3]]
**Output:**
11
**Explanation:**
Geek will learn a new move and earn 5 point then on second
day he will do running and earn 3 point and on third day
he will do running and earn 3 points so, maximum point is 11.

**Your Task:**  
You don't have to read input or print anything. Your task is to complete the function **maximumPoints()** which takes the integer **n** and a 2D array **points** and returns the maximum points he can earn.
https://bit.ly/3glc9kp
```cpp
class Solution {
  public:
    int maximumPoints(vector<vector<int>>& nums, int n) 
    {
        // Code here
        
        vector<vector<int>> dp(n,vector<int>(3,0)) ;
        
        dp[0][0] = nums[0][0] ;
        dp[0][1] = nums[0][1] ;
        dp[0][2] = nums[0][2] ;
                
        if (n == 1)
        {
            return max(dp[0][0],max(dp[0][1] , dp[0][2])) ;
        }
        
        for (int i = 1 ; i < n ; i++)
        {
            dp[i][0] = nums[i][0] + max(dp[i-1][1],dp[i-1][2]) ;
            dp[i][1] = nums[i][1] + max(dp[i-1][0],dp[i-1][2]) ;
            dp[i][2] = nums[i][2] + max(dp[i-1][0],dp[i-1][1]) ;
        }
        
        return max(dp[n-1][0],max(dp[n-1][1],dp[n-1][2])) ;
    }
};
```


## Grid Unique Paths 
There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.
https://leetcode.com/problems/unique-paths
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        int dp[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i==0||j==0) dp[i][j]=1;
                else{
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
};
```

## Grid Unique Paths II
You are given an `m x n` integer array `grid`. There is a robot initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

An obstacle and space are marked as `1` or `0` respectively in `grid`. A path that the robot takes cannot include **any** square that is an obstacle.

Return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.
https://leetcode.com/problems/unique-paths-ii
```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int n = obstacleGrid.size();
        int m = obstacleGrid[0].size();
        vector<vector<int>> dp(n,vector<int>(m,0));
        dp[0][0] = 1;
        for (int row = 0; row < n; row++){
            for (int col = 0; col < m; col++){
                if (obstacleGrid[row][col]){
                    dp[row][col] = 0;
                    continue;
                }
                if (row > 0){
                    //check up
                    dp[row][col] += dp[row-1][col];
                }
                if (col > 0){
                    //check left
                    dp[row][col] += dp[row][col-1];
                }
            }
        }
        return (dp[n-1][m-1] > 0)? dp[n-1][m-1]: 0;
    }
};
```

## Minimum Path Sum in Grid
Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.
https://leetcode.com/problems/minimum-path-sum
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        vector<vector<int>> dp(n, vector<int> (m,1e6));
        dp[0][0] = grid[0][0];
        for (int row = 0; row < n; row++){
            for (int col = 0; col < m; col++){
                if (row > 0){
                    //check for up
                    dp[row][col] = min(dp[row][col], dp[row-1][col] + grid[row][col]);
                }
                if (col > 0){
                    //check left
                    dp[row][col] = min(dp[row][col], dp[row][col-1] + grid[row][col]);
                }
            }
        }
        return dp[n-1][m-1];
    }
};
```

## Minimum Path Sum in Triangular Grid
Given a `triangle` array, return _the minimum path sum from top to bottom_.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example 1:**

**Input:** triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
**Output:** 11
**Explanation:** The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).

**Example 2:**

**Input:** triangle = [[-10]]
**Output:** -10
https://leetcode.com/problems/triangle
```cpp
class Solution {
typedef long long ll;
#define vi vector<ll>
#define vp vector< pair <ll,ll> >
#define f first
#define s second
#define st string

#define pb push_back
#define endl '\n'
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        ll n = triangle.size();
        vector<vi> dp(n+1,vi (n+1,1e4));
        dp[0][0] = triangle[0][0];
        for (int row = 1; row < n; row++){
            for (int i = 0; i <= row; i++){
                if (i < row) dp[row][i] = min(dp[row][i], dp[row-1][i] + triangle[row][i]);
                if (i>0) dp[row][i] = min(dp[row][i], dp[row-1][i-1] + triangle[row][i]);
            }
        }
        return *min_element(dp[n-1].begin(),dp[n-1].end());
    }

};
```

## Minimum Falling Path Sum
Given an `n x n` array of integers `matrix`, return _the **minimum sum** of any **falling path** through_ `matrix`.

A **falling path** starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg)

**Input:** matrix = [[2,1,3],[6,5,4],[7,8,9]]
**Output:** 13
**Explanation:** There are two falling paths with a minimum sum as shown.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg)

**Input:** matrix = [[-19,57],[-40,-5]]
**Output:** -59
**Explanation:** The falling path with a minimum sum is shown.
https://leetcode.com/problems/minimum-falling-path-sum
```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int n=matrix.size(),m=matrix[0].size(),ans=INT_MAX;
        
        for(int i=1;i<n;i++){
            for(int j=0;j<m;j++){
                int x=matrix[i-1][j];
                if(j+1<m) x=min(x,matrix[i-1][j+1]);
                if(j-1>=0) x=min(x,matrix[i-1][j-1]);
                matrix[i][j]+=x;
            }
        }
        for(int i=0;i<m;i++) ans=min(ans,matrix[n-1][i]);
        return ans;
    }
};
```

## 3D DP Ninja and His Friends
You are given an **`n`** `rows` `and` **`m`** `cols` matrix **`grid`** representing a field of chocolates where `grid[i][j]` represents the number of chocolates that you can collect from the `(i, j)` cell.

You have two robots that can collect chocolates for you:

- **Robot #1** is located at the **top-left corner** `(0, 0)`, and
- **Robot #2** is located at the **top-right corner** `(0, cols - 1)`.

Return the maximum number of chocolates collection using both robots by following the rules below:

- From a cell `(i, j)`, robots can move to cell `(i + 1, j - 1)`, `(i + 1, j)`, or `(i + 1, j + 1)`.
- When any robot passes through a cell, It picks up all chocolates, and the cell becomes an empty cell.
- When both robots stay in the same cell, only one takes the chocolates.
- Both robots cannot move outside of the grid at any moment.
- Both robots should reach the bottom row in `grid`.
https://bit.ly/3U9k6XT
```cpp
class Solution {
  public:
  int N,M;
    int dp[80][80][80];
    int recur(int r,int i,int j,vector<vector<int>>& grid){
        if(i<0 or i>=M or j<0 or j>=M) return 0;
        if(r==N-1){
            if(i==j) return grid[r][i];
            return grid[r][i]+grid[r][j];
        }
        if(dp[r][i][j]!=-1) return dp[r][i][j];
        int maxi = 0;
        for(int k=-1;k<=1;k++){
            for(int l = -1;l<=1;l++){
                int ni = i+k,nj = j+l;
                maxi = max(maxi,recur(r+1,ni,nj,grid));
            }
        }
        int temp = 0;
        if(i==j) temp = grid[r][i];
        else temp = grid[r][i]+grid[r][j];
        return dp[r][i][j] = maxi+temp;
    }
    int solve(int n, int m, vector<vector<int>>& grid) {
        // code here
        N = n, M = m;
        memset(dp,-1,sizeof(dp));
        return recur(0,0,m-1,grid);
    }
};
```

## Subset Sum Equal to Target
Given an array of non-negative integers, and a value _sum_, determine if there is a subset of the given set with sum equal to given _sum_. 

  
**Example 1:**

**Input**:
N = 6
arr[] = {3, 34, 4, 12, 5, 2}
sum = 9
**Output:** 1 
**Explanation**: Here there exists a subset with
sum = 9, 4+3+2 = 9.
https://practice.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1
```cpp
class Solution{   
public:
    bool isSubsetSum(vector<int>arr, int sum){
        // code here int n=arr.size();
        vector<vector<int>> dp(n,vector<int>(sum+1,0));
        if(arr[0]<=sum ) dp[0][arr[0]]=1;
        dp[0][0]=1;
        for(int i=1;i<n;i++){
            // dp[i]=dp[i-1];
            for(int j=0;j<=sum;j++){
                if(dp[i-1][j] ) {
                    dp[i][j]=1;
                    if(j+arr[i]<=sum) dp[i][j+arr[i]]=1;
                }
            }
        }
        // cout<<dp[4][7]<<endl;
        return dp[n-1][sum]==1;
    }
};
```

## Partition Equal Subset Sum
Given an integer array `nums`, return `true` _if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [1,5,11,5]
**Output:** true
**Explanation:** The array can be partitioned as [1, 5, 5] and [11].

**Example 2:**

**Input:** nums = [1,2,3,5]
**Output:** false
**Explanation:** The array cannot be partitioned into equal sum subsets.
https://leetcode.com/problems/partition-equal-subset-sum
```cpp
class Solution {
public: 
    bool canPartition(vector<int>& nums)
    {
        int sum=0;
        for(int n:nums)
            sum+=n;
        if(sum%2)
            return false;
        vector<bool> dp(sum/2+1, false);
        dp[0]=true;
        int s=0;
        for(int i=0;i<nums.size();i++)
        {
            s+=nums[i];
            for(int j=min(s,sum/2);j>=nums[i];j--)
            {
                if(dp[j-nums[i]])
                    dp[j]=true;
            }
        }
        return dp[sum/2];
    }
};
```

## Partition Array into two arrays to minimize sum difference
You are given an integer array `nums` of `2 * n` integers. You need to partition `nums` into **two** arrays of length `n` to **minimize the absolute difference** of the **sums** of the arrays. To partition `nums`, put each element of `nums` into **one** of the two arrays.

Return _the **minimum** possible absolute difference_.

**Example 1:**

![example-1](https://assets.leetcode.com/uploads/2021/10/02/ex1.png)

**Input:** nums = [3,9,7,3]
**Output:** 2
**Explanation:** One optimal partition is: [3,9] and [7,3].
The absolute difference between the sums of the arrays is abs((3 + 9) - (7 + 3)) = 2.

**Example 2:**

**Input:** nums = [-36,36]
**Output:** 72
**Explanation:** One optimal partition is: [-36] and [36].
The absolute difference between the sums of the arrays is abs((-36) - (36)) = 72.

**Example 3:**

![example-3](https://assets.leetcode.com/uploads/2021/10/02/ex3.png)

**Input:** nums = [2,-1,0,4,-2,-9]
**Output:** 0
**Explanation:** One optimal partition is: [2,4,-9] and [-1,0,-2].
The absolute difference between the sums of the arrays is abs((2 + 4 + -9) - (-1 + 0 + -2)) = 0.
https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference
```cpp
class Solution 
{
public:
    int minimumDifference(vector<int>& nums) 
    {
         int n=nums.size();
        int sum=0;
        for(int i=0; i<n; i++)
        {
            sum+=nums[i];
        }
        bool dp[n+1][sum+1];
        for(int i=0; i<=n; i++)
        {
            dp[i][0] = true;
        }
        for(int i=1; i<=sum; i++)
        {
            dp[0][i] = false;
        }
        for(int i=1; i<=n; i++)
        {
            for(int j=1; j<=sum; j++)
            {
                if(nums[i-1]<=j)
                {
                    dp[i][j]=dp[i-1][j-nums[i-1]] || dp[i-1][j];
                }
                else dp[i][j]=dp[i-1][j];
                }
            }
        int diff = INT_MAX;
       for (int j = 0; j<=sum/2; j++) 
       {
           int first=j;
           int second=sum-j;
            if (dp[n][j] == true && diff>abs(first-second)) 
            {
                diff=abs(first-second);
            }
        }

        return diff;
    }
};
```

## Count Subsets with sum K
Given an array **arr** of size **n** of non-negative integers and an integer **sum**, the task is to count all subsets of the given array with a sum equal to a given **sum**.

**Note:** Answer can be very large, so, output answer modulo **109+7**.

**Examples:**

**Input**:   
n = 6, arr = [5, 2, 3, 10, 6, 8], sum = 10
**Output:**   
3
**Explanation**:   
{5, 2, 3}, {2, 8}, {10} are possible subsets.

**Input**:   
n = 5, arr = [2, 5, 1, 4, 3], sum = 10
**Output:**   
3
**Explanation**:   
{2, 1, 4, 3}, {5, 1, 4}, {2, 5, 3} are possible subsets.
https://bit.ly/3AwVr8I
```cpp
class Solution{

	public:
	const int MOD = 1e9 +7;
	int perfectSum(int arr[], int n, int sum)
	{
        vector<int> dp(sum+1);
        dp[0] = 1;
        //dp[x] = dp[x] += dp[x-arr[i]]
        for (int i = 0; i < n; ++i)
        {
        	for(int j = sum; j >= arr[i]; j--){
        		dp[j] = (dp[j] + dp[j - arr[i]])%MOD;
        	}
        }
        return dp[sum];

	}
	  
};
```

## Count Partitions with Given Difference
https://bit.ly/3AwVr8I
Given an array **arr**, partition it into two subsets(possibly empty) such that each element must belong to only one subset. Let the sum of the elements of these two subsets be **S1** and **S2**. Given a difference **d**, count the number of partitions in which **S1** is greater than or equal to **S2** and the difference between **S1** and **S2** is equal to **d**. Since the answer may be large return it **modulo 109 + 7**.

**Example 1:**

**Input:**
n = 4  
d = 3
arr[] =  { 5, 2, 6, 4}
**Output:** 1
**Explanation:**
There is only one possible partition of this array. Partition : {6, 4}, {5, 2}. The subset difference between subset sum is: (6 + 4) - (5 + 2) = 3.

**Example 2:**

**Input:**
n = 4  
d = 0   
arr[] = {1, 1, 1, 1}   
**Output:** 6   
**Explanation:**  
we can choose two 1's from indices {0,1}, {0,2}, {0,3}, {1,2}, {1,3}, {2,3} and put them in S1 and remaning two 1's in S2.  
Thus there are total 6 ways for partition the array arr.

```cpp
class Solution {
  public:
   int mod = 1e9+7;
	int helper(vector<int>&arr, int index, long long sum, vector<vector<long long>>&dp){
	    if(index == 0){
	        int count = 0;
	        count += (sum == 0);
	        count += (sum == arr[0]);
	        return count;
	    }
	    if(dp[index][sum] != -1) return dp[index][sum];
	    
	    if(arr[index] <= sum) {
	        dp[index][sum] = (helper(arr, index-1, sum, dp) + helper(arr, index-1, sum - arr[index], dp)) % mod;
	    } else {
	        dp[index][sum] = helper(arr, index-1, sum, dp);
	    }
	    
	    return dp[index][sum];
	}
	
	int perfectSum(vector<int>&arr, int n, long long sum) {
        vector<vector<long long>> dp(n, vector<long long>(sum + 1, -1));
        return helper(arr, n-1, sum, dp);
	}
    int countPartitions(int n, int d, vector<int>& arr) {
        // Code here
        long long ts = 0;
        for(int t:arr)ts += t;
        if((ts-d)%2)return 0;
        ts = (ts - d)/2;
        return 2*perfectSum(arr,n,ts);
    }
};
```

## Assign Cookies
https://leetcode.com/problems/assign-cookies/
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child `i` has a greed factor `g[i]`, which is the minimum size of a cookie that the child will be content with; and each cookie `j` has a size `s[j]`. If `s[j] >= g[i]`, we can assign the cookie `j` to the child `i`, and the child `i` will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Example 1:**

**Input:** g = [1,2,3], s = [1,1]
**Output:** 1
**Explanation:** You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.

**Example 2:**

**Input:** g = [1,2], s = [1,2,3]
**Output:** 2
**Explanation:** You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.
```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
       sort(g.begin(),g.end());
       sort(s.begin(),s.end());
       int index =0,cnt=0;
       while(index<s.size() && cnt<g.size()){
           if(s[index]>=g[cnt])cnt++;
           index++;
       }
    return cnt;
    }
};

```

## Coin Change
https://leetcode.com/problems/coin-change/
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

**Input:** coins = [1,2,5], amount = 11
**Output:** 3
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**

**Input:** coins = [2], amount = 3
**Output:** -1

**Example 3:**

**Input:** coins = [1], amount = 0
**Output:** 0
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX);
        dp[0] = 0;

        for (int i = 1; i <= amount; ++i) {
            for (int coin : coins) {
                if (i - coin >= 0 && dp[i - coin] != INT_MAX) {
                    dp[i] = min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        return (dp[amount] == INT_MAX) ? -1 : dp[amount];
    }
};
```

## Target Sum
https://leetcode.com/problems/target-sum/
You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Example 1:**

**Input:** nums = [1,1,1,1,1], target = 3
**Output:** 5
**Explanation:** There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

**Example 2:**

**Input:** nums = [1], target = 1
**Output:** 1
```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int total_sum = accumulate(nums.begin() , nums.end() , 0);

        if((total_sum - target) % 2 == 1){ // think why?
            return 0;
        }
        int req_sum = (total_sum - target)/2 + target;
        if(req_sum < 0){ // think why
            return 0;
        }

        vector<int>dp(req_sum + 1 , 0);
        dp[0] = 1;
        for(int i : nums){
            for(int j = req_sum ; j >= i; j--){
                dp[j] += dp[j - i];
            }
        }

        return dp[req_sum];
    }
};
```

## Coin Change 2
https://leetcode.com/problems/coin-change-2/
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example 1:**

**Input:** amount = 5, coins = [1,2,5]
**Output:** 4
**Explanation:** there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

**Example 2:**

**Input:** amount = 3, coins = [2]
**Output:** 0
**Explanation:** the amount of 3 cannot be made up just with coins of 2.

**Example 3:**

**Input:** amount = 10, coins = [10]
**Output:** 1
```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<vector<int>> dp ( coins.size() + 1, vector<int>( amount+1));
        int n = coins.size() ;
        for( int i = 0; i <=n; i++) dp[i][0] = 1;
        for( int i = 1; i <= n; i++ ){
            for( int j = 1; j <= amount; j++ ){    
                if( coins[i-1] <= j) 
                  dp[i][j] = dp[i][j - coins[i-1]] + dp[i-1][j];
                else {
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        return dp[n][amount]; 
    }
};
```

## Unbounded KnapSack
https://bit.ly/3Cbc5fz
Given a set of **N** items, each with a weight and a value, represented by the array **w** and **val** respectively. Also, a knapsack with weight limit **W**.  
The task is to fill the knapsack in such a way that we can get the maximum profit. Return the maximum profit.  
**Note:** Each item can be taken any number of times.

**Example 1:**

**Input:**   
N = 2  
W = 3
val = {1, 1}
wt = {2, 1}
**Output:**   
3
**Explanation:** 
1.Pick the 2nd element thrice.
2.Total profit = 1 + 1 + 1 = 3. Also the total weight = 1 + 1 + 1  = 3 which is <= 3.

**Example 2:**

**Input:**   
N = 4  
W = 8
val[] = {6, 1, 7, 7}
wt[] = {1, 3, 4, 5}
**Output:**   
48
**Explanation:**   
The optimal choice is to pick the 1st element 8 times.

**Your Task:**  
You do not need to read input or print anything. Your task is to complete the function **knapSack()** which takes the values **N**, **W** and the arrays **val** and **wt** as input parameters and returns the maximum possible value.

```cpp
class Solution{
public:
    int knapSack(int N, int W, int val[], int wt[])
    {
        vector<vector<int>> dp(N, vector<int>(W+1, 0));
        for(int i=1;i<=W;i++)
        {
            dp[0][i] = (i/wt[0])*val[0];
        }
        for(int i=1; i<N;i++)
        {
            for(int j=0; j<=W;j++)
            {
                int take = 0;
                int nottake = dp[i-1][j]; 
                if(j-wt[i] >=0) take = dp[i][j-wt[i]] + val[i]; 
                dp[i][j] = max(take,nottake);
            }
        }
        return dp[N-1][W];
    }
};
```

## Rod Cutting Problem
https://practice.geeksforgeeks.org/problems/rod-cutting0840/1
Given a rod of length N inches and an array of prices, price[]. price[i] denotes the value of a piece of length i. Determine the maximum value obtainable by cutting up the rod and selling the pieces.
**Note:** Consider 1-based indexing.
**Example 1:**
**Input:**  
N = 8  
Price[] = {1, 5, 8, 9, 10, 17, 17, 20}  
**Output:**  
22  
**Explanation:**  
The maximum obtainable value is 22 by cutting in two pieces of lengths 2 and 6, i.e., 5+17=22.
**Example 2:**
**Input:**  
N=8  
Price[] = {3, 5, 8, 9, 10, 17, 17, 20}  
**Output:**   
24  
**Explanation:** The maximum obtainable value is 24 by cutting the rod into 8 pieces of length 1, i.e, 8*price[1]= 8*3 = 24.
```cpp
class Solution{
  public:
    int cutRod(int price[], int n) {
        vector<int> dp(n, 0);
        dp[0] = price[0];
        for(int i=1;i<n;i++)
        {
            int maxi = price[i];
            for(int j=0;j<=(i-1)/2;j++)
                maxi = max(maxi, dp[j] + dp[i-j-1]);
            dp[i] = maxi;
        }
        return dp[n-1];
    }
};
```

# DP on Strings
## Longest Common Subsequence
https://leetcode.com/problems/longest-common-subsequence/
Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.
A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**
**Input:** text1 = "abcde", text2 = "ace" 
**Output:** 3  
**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**
**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**
**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no such common subsequence, so the result is 0.
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int dp[text1.size()+1][text2.size()+1];
        memset(dp, 0, sizeof dp);
        for(int i=1;i<=text1.size();i++){
            for(int j=1; j<=text2.size(); j++){
                if(text1[i-1]==text2[j-1])
                    dp[i][j] = dp[i-1][j-1] + 1;
                else
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```

## Print Longest Common Subsequence
https://bit.ly/3T1Va4U
You are given two strings **s** and **t**. Now your task is to print all longest common sub-sequences in lexicographical order.

**Note -**  A Sub-sequence is derived from another string by deleting some or none of the elements without changing the order of the remaining elements.

**Example 1:**

**Input:** s = abaaa, t = baabaca
**Output:** aaaa abaa baaa  
**Explanation -** Length of lcs is 4, in lexicographical order they are aaaa, abaa, baaa  

**Example 2:**

**Input:** s = aaa, t = a
**Output:** a

**Your Task:**  
You do not need to read or print anything. Your task is to complete the function **all_longest_common_subsequences()** which takes strings s and t as first and second parameters respectively and returns a list of strings which contains all possible longest common subsequences in lexicographical order.

```cpp
class Solution

{
public:
	vector<string> all_longest_common_subsequences(string s, string t)
	{
		int n = s.length();
		int m = t.length();
		vector<vector<int>> dp(n+1, vector<int>(m+1, -1));
 
		vector<vector<set<string>>>res (n+1, vector<set<string>>(m+1));
    
    for(int i=0;i<n+1;i++)
    {
        for(int j=0;j<m+1;j++)
        {
            if(i==0 || j==0)
            {
                dp[i][j]=0;
            }
        }
    }
    for(int i=1; i<n+1; i++)
    {
        for(int j=1; j<m+1; j++)
        {
            if(s[i-1] == t[j-1])
            {
                dp[i][j] = 1 + dp[i-1][j-1];
            }
            else
            {
                dp[i][j] = max(dp[i][j-1], dp[i-1][j]);
            }
        }
    }
    
    for(int i=0; i<n+1; i++)
    {
        for(int j=0; j<m+1; j++)
        {
            if(i==0 || j==0)
            {
                res[i][j].insert("");
            }
        }
    }
    
    for(int i=1; i<n+1; i++)
    {
        for(int j=1; j<m+1; j++)
        {
            if(s[i-1] == t[j-1])
            {
                for(auto it : res[i-1][j-1])
                     res[i][j].insert(it+s[i-1]);
            }
            
            if(dp[i][j] == dp[i-1][j])
            {
                for(auto it : res[i-1][j])
                     res[i][j].insert(it);
            }
            
            if(dp[i][j] == dp[i][j-1])
            {
                for(auto it : res[i][j-1])
                     res[i][j].insert(it);
            }
        }
        
        
    }
    
    vector<string> v1;
    
    for(auto it : res[n][m])
    {
        if(it != "") v1.push_back(it);
        
    }
    sort(v1.begin(), v1.end());
    
    return v1;
		}
};
```

## Longest Common Substring
https://practice.geeksforgeeks.org/problems/longest-common-substring1452/1
Given two strings. The task is to find the length of the longest common substring.

  
**Example 1:**

**Input:** S1 = "ABCDGH", S2 = "ACDGHR", n = 6, m = 6
**Output:** 4
**Explanation**: The longest common substring
is "CDGH" which has length 4.

**Example 2:**

**Input**: S1 = "ABC", S2 "ACB", n = 3, m = 3
**Output:** 1
**Explanation**: The longest common substrings
are "A", "B", "C" all having length 1.

```cpp
class Solution{
    public:
    
    int longestCommonSubstr (string S1, string S2, int n, int m){
        //tabulation
        vector<vector<int>> dp(n+1,vector<int>(m+1,0));
        for(int j=0;j<=m;j++) dp[0][j] = 0;
        for(int i=0;i<=n;i++) dp[i][0] = 0;
        int ans = 0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(S1[i-1]==S2[j-1]){
                    dp[i][j] = 1 + dp[i-1][j-1];
                    ans = max(ans,dp[i][j]);
                }
                else dp[i][j] = 0;
            }
        }
        return ans;
        //space optimization
        vector<int> prev(m+1,0);
        vector<int> cur(m+1,0);
        for(int j=0;j<=m;j++) prev[j] = 0;
        int ans = 0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(S1[i-1]==S2[j-1]){
                    cur[j] = 1 + prev[j-1];
                    ans = max(ans,cur[j]);
                }
                else cur[j] = 0;
            }
            prev = cur;
        }
        return ans;
    }
};
```

## Longest Palindromic Subsequence
https://leetcode.com/problems/longest-palindromic-subsequence/
Given a string `s`, find _the longest palindromic **subsequence**'s length in_ `s`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** s = "bbbab"
**Output:** 4
**Explanation:** One possible longest palindromic subsequence is "bbbb".

**Example 2:**

**Input:** s = "cbbd"
**Output:** 2
**Explanation:** One possible longest palindromic subsequence is "bb".

```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        string t=s;
        reverse(t.begin(), t.end());
        int n=s.size();
        vector<vector<int>>dp(n+1, vector<int>(n+1, 0));
        for(int i=1; i<=n; i++){
            for(int j=1; j<=n; j++){
                if(s[i-1]==t[j-1]){
                    dp[i][j]=1+dp[i-1][j-1];
                }
                else{
                    dp[i][j]=max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        return dp[n][n];
    }
};
```

## Minimum Number Of Insertions to Make a String Palindrome
https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/
Given a string `s`. In one step you can insert any character at any index of the string.

Return _the minimum number of steps_ to make `s` palindrome.

A **Palindrome String** is one that reads the same backward as well as forward.

**Example 1:**

**Input:** s = "zzazz"
**Output:** 0
**Explanation:** The string "zzazz" is already palindrome we do not need any insertions.

**Example 2:**

**Input:** s = "mbadm"
**Output:** 2
**Explanation:** String can be "mbdadbm" or "mdbabdm".

**Example 3:**

**Input:** s = "leetcode"
**Output:** 5
**Explanation:** Inserting 5 characters the string becomes "leetcodocteel".

```cpp
int minInsertions(string s) {
        int n=s.size();
        string S1=s;
        string S2=s;
        reverse(S2.begin(),S2.end());
    vector<vector<int>> dp(n+1,vector<int>(n+1,0));
        
        for(int i=1;i<n+1;i++)
        {
            for(int j=1;j<n+1;j++)
            {
                if(S1[i-1]==S2[j-1])
                    dp[i][j]=1+dp[i-1][j-1];
                else
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            
            }
        }
        return n-dp[n][n];
}
```

### Space Optimisation

```cpp
class Solution {
public:
    int minInsertions(string s) {
        int n=s.size();
        string S1=s;
        string S2=s;
        reverse(S2.begin(),S2.end());
        vector<int> prev(n+1,0);
        vector<int> next(n+1,0);
        for(int i=1;i<n+1;i++)
        {
            for(int j=1;j<n+1;j++)
            {
                if(S1[i-1]==S2[j-1])
                    next[j]=1+prev[j-1];
                else
                    next[j]=max(prev[j],next[j-1]);
            }
            prev=next;
        }
        return n-next[n];
    }
};
```

## Minimum Number of Insertions/Deletions to convert a String
https://leetcode.com/problems/delete-operation-for-two-strings/

```cpp
class Solution {
public:
    int lcs(string &s, string &t){

        int n = s.size();
        int m = t.size();

        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        // for (int j = 0; j <= m; j++) dp[0][j] = 0;
        // for (int i = 0; i <= n; i++) dp[i][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (s[i - 1] == t[j - 1])
                    dp[i][j] = 1 + dp[i - 1][j - 1];

                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }

        return dp[n][m];

    }

    int minDistance(string word1, string word2) {
        return word1.size() + word2.size()- 2*lcs(word1, word2);
    }
};
```

## Shortest Common Supersequence
https://leetcode.com/problems/shortest-common-supersequence/
Given two strings `str1` and `str2`, return _the shortest string that has both_ `str1` _and_ `str2` _as **subsequences**_. If there are multiple valid strings, return **any** of them.

A string `s` is a **subsequence** of string `t` if deleting some number of characters from `t` (possibly `0`) results in the string `s`.

**Example 1:**

**Input:** str1 = "abac", str2 = "cab"
**Output:** "cabac"
**Explanation:** 
str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
The answer provided is the shortest such string that satisfies these properties.

**Example 2:**

**Input:** str1 = "aaaaaaaa", str2 = "aaaaaaaa"
**Output:** "aaaaaaaa"
```cpp
class Solution {
    //use it as a black-box function which returns LCS of 2 strings
    string getLCS(string str1, string str2) {
        int m = str1.size(), n = str2.size();
        int dp[m + 1][n + 1];

        for(int i = 0; i <= m; i++) {
            for(int j = 0; j <= n; j++) {
                if(i == 0 || j == 0)
                    dp[i][j] = 0;
                else if(str1[i - 1] == str2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        string ans;
        int i = m, j = n;
        while(i > 0 && j > 0) {        
            if(str1[i - 1] == str2[j - 1]) {
                ans.push_back(str1[i-1]);
                i--; j--;
            }        
            else if(dp[i - 1][j] > dp[i][j - 1]) {
                i--;
            }
            else {
                j--;
            }
        }
     reverse(ans.begin(), ans.end());
     return ans;
    }
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int i = 0, j = 0;
        string lcs = getLCS(str1, str2), ans = "";
        //iterate on lcs string
        for(int k=0; k<lcs.length(); i++,j++,k++) {
        //include characters from str1 untils it matches with lcs[k]
            while(str1[i] != lcs[k]) { 
                ans += str1[i++];
            }
        //include characters from str2 untils it matches with lcs[k]
            while(str2[j] != lcs[k]) {
                ans += str2[j++];
            }
         ans += lcs[k]; //include the common character i.e. lcs[k]
        }
     ans += str1.substr(i); //include remaining characters of str1
     ans += str2.substr(j); //include remaining characters of str2
     return ans;
    }
};
```

## Distinct Subsequences
https://leetcode.com/problems/distinct-subsequences/

Given two strings s and t, return _the number of distinct_ **_subsequences_** _of_ s _which equals_ t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

**Example 1:**

**Input:** s = "rabbbit", t = "rabbit"
**Output:** 3
**Explanation:**
As shown below, there are 3 ways you can generate "rabbit" from s.
`**rabb**b**it**`
`**ra**b**bbit**`
`**rab**b**bit**`

**Example 2:**

**Input:** s = "babgbag", t = "bag"
**Output:** 5
**Explanation:**
As shown below, there are 5 ways you can generate "bag" from s.
`**ba**b**g**bag`
`**ba**bgba**g**`
`**b**abgb**ag**`
`ba**b**gb**ag**`
`babg**bag**`
```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<unsigned long long>> dp(s.size()+1,vector<unsigned long long>(t.size()+1,0));
        
        for(int i=0;i<=s.size();i++){
            dp[i][0]=1;
        }
        for(int i=1;i<=s.size();i++){
            for(int j=1;j<=t.size();j++){
                if(s[i-1]==t[j-1]){
                    dp[i][j]=dp[i-1][j-1]+dp[i-1][j];
                }
                else{
                    dp[i][j]=dp[i-1][j];
                }
            }
        }
        return dp[s.size()][t.size()];
    }
};
```

## Edit Distance
https://leetcode.com/problems/edit-distance/
Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"
**Output:** 3
**Explanation:** 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"
**Output:** 5
**Explanation:** 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```cpp
class Solution {
 public:
  int minDistance(string word1, string word2) {
    const int m = word1.length();//first word length
    const int n = word2.length();//second word length
    // dp[i][j] := min # of operations to convert word1[0..i) to word2[0..j)
    vector<vector<int>> dp(m + 1, vector<int>(n + 1));

    for (int i = 1; i <= m; ++i)
      dp[i][0] = i;

    for (int j = 1; j <= n; ++j)
      dp[0][j] = j;

    for (int i = 1; i <= m; ++i)
      for (int j = 1; j <= n; ++j)
        if (word1[i - 1] == word2[j - 1])//same characters
          dp[i][j] = dp[i - 1][j - 1];//no operation
        else
          dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
                             //replace       //delete        //insert
    return dp[m][n];
  }
};
```

## WildCard Matching
https://leetcode.com/problems/wildcard-matching/
Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'` where:

- `'?'` Matches any single character.
- `'*'` Matches any sequence of characters (including the empty sequence).

The matching should cover the **entire** input string (not partial).

**Example 1:**

**Input:** s = "aa", p = "a"
**Output:** false
**Explanation:** "a" does not match the entire string "aa".

**Example 2:**

**Input:** s = "aa", p = "*"
**Output:** true
**Explanation:** '*' matches any sequence.

**Example 3:**

**Input:** s = "cb", p = "?a"
**Output:** false
**Explanation:** '?' matches 'c', but the second letter is 'a', which does not match 'b'.

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size(), m = p.size();
        vector<vector<bool>> dp(n + 1, vector<bool>(m + 1));

        dp[0][0] = true;
        for(int k=1; k<=m; k++) {
            if(p[k - 1] == '*') dp[0][k] = true;
            else break;
        }
        for(int i=1; i<=n; i++) {
            for(int j=1; j<=m; j++) {
                if(s[i - 1] == p[j - 1] || p[j - 1] == '?') dp[i][j] = dp[i - 1][j - 1];
                if(p[j - 1] == '*') {
                    dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
                }
            }
        }
        return dp[n][m];
    }
};
```

# DP on Stocks

## Best Time To buy and sell stocks
https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]
**Output:** 5
**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transactions are done and the max profit = 0.
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() <= 1) return 0;
        int maxProfit = 0;
        int min_buy = prices[0];
        for(int i = 0; i < prices.size(); i++) {
            min_buy = min(min_buy, prices[i]);
            maxProfit = max(maxProfit, prices[i] - min_buy);
        }
        return maxProfit;
    }
};
```

## Buy and Sell Stocks II
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return _the **maximum** profit you can achieve_.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]
**Output:** 7
**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.

**Example 2:**

**Input:** prices = [1,2,3,4,5]
**Output:** 4
**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.

**Example 3:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.

```cpp
class Solution {
public:
int dp[100005][2];
    int maxProfit(vector<int>& prices) {
    int n=prices.size();
        dp[n][0]=0;
        dp[n][1]=0;
        for(int i=n-1;i>=0;i--)
        {
            for(int canbuy=0;canbuy<2;canbuy++)
            {
                if(canbuy)
                {
                    dp[i][canbuy]=max(-prices[i]+dp[i+1][0],dp[i+1][1]);
                }
                else
                {
                    dp[i][canbuy]=max(prices[i]+dp[i+1][1],dp[i+1][0]);
                }
            }
        }
        return dp[0][1];
    }
};
```

## Buy and Sell Stocks III
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/
You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = [3,3,5,0,0,3,1,4]
**Output:** 6
**Explanation:** Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

**Example 2:**

**Input:** prices = [1,2,3,4,5]
**Output:** 4
**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.

**Example 3:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transaction is done, i.e. max profit = 0.
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        vector<vector<vector<int>>>dp(n+1, vector<vector<int>>(2, vector<int>(3, 0)));
        for(int i=n-1; i>=0; i--){
            for(int j=0; j<2; j++){
                for(int k=1; k<3; k++){
                    if(j==1){
                        dp[i][j][k]=max((-1)*prices[i]+dp[i+1][0][k], dp[i+1][j][k]);
                    }
                    else{
                        dp[i][j][k]=max(prices[i]+dp[i+1][1][k-1], dp[i+1][j][k]);
                    }
                }
            }
        }
        return dp[0][1][2];
    }
};
```

## Buy and Sell Stocks IV
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/
You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions: i.e. you may buy at most `k` times and sell at most `k` times.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** k = 2, prices = [2,4,1]
**Output:** 2
**Explanation:** Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.

**Example 2:**

**Input:** k = 2, prices = [3,2,6,5,0,3]
**Output:** 7
**Explanation:** Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        ios_base::sync_with_stdio(false);
        cin.tie(NULL);
        cout.tie(NULL);

        int n = prices.size();
        vector<vector<vector<int>>> dp(n+1, vector<vector<int>>(2, vector<int>(k+1, 0)));

        for(int index= n-1; index >=0; index--){
            for(int buy = 0; buy <2; buy++){
                for(int cap =1; cap<=k; cap++){
                    int profit = 0;
                    if(buy){
                        int take = -1*prices[index] + dp[index+1][0][cap];
                        int nottake = 0 + dp[index+1][1][cap];
                        profit = max(profit, max(take,nottake));
                    }
                    else{
                        int take = prices[index] + dp[index+1][1][cap-1];
                        int nottake = 0 + dp[index+1][0][cap];
                        profit = max(profit, max(take,nottake));
                    }
                    
                    dp[index][buy][cap] = profit;                    
                }
            }
        }

        // return f(0,1,2,prices,dp);
        return dp[0][1][k];
    }
};
```

## Buy and Sell Stocks with Cooldown
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = [1,2,3,0,2]
**Output:** 3
**Explanation:** transactions = [buy, sell, cooldown, buy, sell]

**Example 2:**

**Input:** prices = [1]
**Output:** 0

```cpp
class Solution {
public:
    int maxProfit(vector<int>& p) {
        int n = p.size();
        vector<vector<int>> dp(n + 2, vector<int>(2));
        
        for(int i=n-1; i>=0; i--) {
            for(int buy=0; buy<=1; buy++) {
                if(buy) {
                    dp[i][buy] = max(-p[i] + dp[i + 1][0], 0 + dp[i + 1][1]);
                }
                else dp[i][buy] = max(p[i] + dp[i + 2][1], 0 + dp[i + 1][0]);
            }
        }
        return dp[0][1];
    }
};
```
Space Optimization
```cpp
class Solution {
public:
    int maxProfit(vector<int>& p) {
        int n = p.size();
        vector<int> dp(2), temp1(2), temp2(2);
        
        for(int i=n-1; i>=0; i--) {
            for(int buy=0; buy<=1; buy++) {
                if(buy) {
                    temp1[buy] = max(-p[i] + dp[0], 0 + dp[1]);
                }
                else temp1[buy] = max(p[i] + temp2[1], 0 + dp[0]);
            }
            temp2 = dp;
            dp = temp1;
        }
        return dp[1];
    }
};
```

## Buy and Sell Stocks With Transaction Fee
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/
You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:**

- You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
- The transaction fee is only charged once for each stock purchase and sale.

**Example 1:**

**Input:** prices = [1,3,2,8,4,9], fee = 2
**Output:** 8
**Explanation:** The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

**Example 2:**

**Input:** prices = [1,3,7,5,10,3], fee = 3
**Output:** 6
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
       int n=prices.size();
        vector<vector<int>> dp(n+1,vector<int>(2,0));
        for(int index=n-1; index>=0; index--){
            for(int buy=0; buy<=1 ;buy++){
                  int profit=0;
        if(buy)
            profit= max((-prices[index]+dp[index+1][0]),(dp[index+1][1])); 
        else
             profit= max((prices[index]+dp[index+1][1]-fee),(dp[index+1][0]));
         dp[index][buy]= profit;
        }   
            }
        return dp[0][1];
    }
};
```

# DP on LIS
## Longest Increasing Subsequence
https://leetcode.com/problems/longest-increasing-subsequence/
Given an integer array `nums`, return _the length of the longest **strictly increasing**_
_**subsequence.**_
**Example 1:**
**Input:** nums = [10,9,2,5,3,7,101,18]
**Output:** 4
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
**Example 2:**
**Input:** nums = [0,1,0,3,2,3]
**Output:** 4
**Example 3:**
**Input:** nums = [7,7,7,7,7,7,7]
**Output:** 1
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) {
            return 0;
        }
        int n = nums.size();
        vector<int> dp(n, 1);
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```

## Printing Longest Increasing Subsequence
https://bit.ly/3XiRbmG
Given an integer **n** and an array of integers **arr**, return the **Longest Increasing Subsequence** which is _Index-wise_ lexicographically smallest.  
**Note -** A subsequence S1 is **Index-wise** **lexicographically smaller** than a subsequence S2 if in the first position where S1 and S2 differ, subsequence `S1` has an element that appears **earlier** in the array  arr than the corresponding element in `S2`.  
LIS  of a given sequence is defined as that longest possible subsequence all of whose elements are in increasing order. For example, the length of LIS for {10, 22, 9, 33, 21, 50, 41, 60, 80} is 6 and the LIS is {10, 22, 33, 50, 60, 80}. 

**Example 1:**

**Input:**
n = 16
arr = [0,8,4,12,2,10,6,14,1,9,5,13,3,11,7,15]
**Output:**
0 4 6 9 13 15 
**Explanation:**
longest Increasing subsequence is 0 4 6 9 13 15  and the length of the longest increasing subsequence is 6.

**Example 2:**

**Input:**
n = 1
arr = [1]
**Output:**
1

**Your Task:**  
You don't need to read input or print anything. Your task is to complete the function **longestIncreasingSubsequence()** which takes integer n and array arr and returns the longest increasing subsequence.

```cpp
class Solution {
  public:
    vector<int> longestIncreasingSubsequence(int n, vector<int>& arr) {
        vector<int>dp(n,1), hash(n);
        int ans=1, last_index=0;
        for(int i=0;i<n;i++){
            hash[i]=i;
            for(int prev=0; prev<i;prev++){
                if(arr[i]>arr[prev] && dp[i]<1+dp[prev]){
                    dp[i]=1+dp[prev];
                    hash[i]=prev;
                }
            }
        }
        for(int i=0; i<=n-1; i++){
        if(dp[i]>ans){
            ans=dp[i];
            last_index=i;
        }
        }
        vector<int>temp;
        temp.push_back(arr[last_index]);
        while(hash[last_index]!=last_index){
            last_index=hash[last_index];
            temp.push_back(arr[last_index]);
        }
        reverse(temp.begin(),temp.end());
        return temp;
    }
};
```

## Longest Strictly Increasing Subsequence
https://bit.ly/3Pxf84L
Given an **array a[ ] of n integers**, find the **Length** of the **Longest Strictly Increasing Subsequence**.

> A sequence of numbers is called "strictly increasing" when each term in the sequence is smaller than the term that comes after it.

**Example 1:**

**Input:** n = 6, a[ ] = {5,8,3,7,9,1}
**Output:** 3
**Explanation:** There are more than one **LIS** in this array.  One such Longest increasing subsequence is {5,7,9}.

**Example 2:**

**Input:** n = 16, a[ ] = {0,8,4,12,2,10,6,14,1,9,5,13,3,11,7,15}
**Output:** 6
**Explanation:** There are more than one LIS in this array.   
One such Longest increasing subsequence is {0,2,6,9,13,15}.

**Your Task:**  
You do not need to read input or print anything. Complete the function **longestSubsequence()** which takes the given array and its size as input parameters and returns the **length** of the **longest increasing subsequence.**
```cpp
class Solution
{
    public:
    //Function to find length of longest increasing subsequence.
    int longestSubsequence(int n, int a[])
    {
       // your code here
       vector<int>dp;
       dp.push_back(a[0]);
       int len=1;
       for (int i=1 ; i<n ; i++){
           if (dp.back()<a[i]){
               dp.push_back(a[i]);
               len++;
           }
           else{
               int ind = lower_bound(dp.begin() , dp.end() , a[i])-dp.begin();
               dp[ind]=a[i];
           }
       }
       return len;
    }
};
```

## Largest Divisible Subset
https://leetcode.com/problems/largest-divisible-subset/
Given a set of **distinct** positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

- `answer[i] % answer[j] == 0`, or
- `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.
**Example 1:**
**Input:** nums = [1,2,3]
**Output:** [1,2]
**Explanation:** [1,3] is also accepted.

**Example 2:**
**Input:** nums = [1,2,4,8]
**Output:** [1,2,4,8]
```cpp
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        if (nums.size() == 1)
            return {nums[0]};
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<int> dp(n, 1), temp(n, -1);
        int len = 0, idx = -1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0 && dp[i] < dp[j] + 1) {
                    dp[i] = dp[j] + 1;
                    temp[i] = j;
                }
                if (dp[i] > len) {
                    len = dp[i];
                    idx = i;
                }
            }
        }

        vector<int> ans(len, 0);
        int index = len - 1;
        while (index >= 0) {
            ans[index] = nums[idx];
            idx = temp[idx];
            index--;
        }
        return ans;
    }
};
```

## Longest String Chain
https://leetcode.com/problems/longest-string-chain/
You are given an array of `words` where each word consists of lowercase English letters.

`wordA` is a **predecessor** of `wordB` if and only if we can insert **exactly one** letter anywhere in `wordA` **without changing the order of the other characters** to make it equal to `wordB`.

- For example, `"abc"` is a **predecessor** of `"abac"`, while `"cba"` is not a **predecessor** of `"bcad"`.

A **word chain** is a sequence of words `[word1, word2, ..., wordk]` with `k >= 1`, where `word1` is a **predecessor** of `word2`, `word2` is a **predecessor** of `word3`, and so on. A single word is trivially a **word chain** with `k == 1`.

Return _the **length** of the **longest possible word chain** with words chosen from the given list of_ `words`.

**Example 1:**

**Input:** words = ["a","b","ba","bca","bda","bdca"]
**Output:** 4
**Explanation**: One of the longest word chains is ["a","ba","bda","bdca"].

**Example 2:**

**Input:** words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
**Output:** 5
**Explanation:** All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].

**Example 3:**

**Input:** words = ["abcd","dbqca"]
**Output:** 1
**Explanation:** The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.

```cpp
class Solution {
public:

    int longestStrChain(vector<string>& words) {
        // Sort the words by their lengths
        sort(words.begin(), words.end(), [](const string& a, const string& b) {
            return a.length() < b.length();
        });
        // Map to store the longest chain length for each word
        unordered_map<string, int> longestChainLength;
        // Initialize the answer
        int maxChainLength = -1;
        for(auto &word: words){
            // Initialize the chain length for the current word
            longestChainLength[word] = 1;
            // Try removing one character at a time from the word and check if the resulting word exists
            for(int i = 0 ; i < word.size() ; i++){
                string reducedWord = word.substr(0, i) + word.substr(i + 1) ;
                // If the reduced word exists in the map
                if(longestChainLength.find(reducedWord) != longestChainLength.end())
                    // Update the chain length for the current word
                    longestChainLength[word] = max(longestChainLength[word], longestChainLength[reducedWord] + 1) ;
            }
            // Update the maximum chain length seen so far
            maxChainLength = max(maxChainLength, longestChainLength[word]) ;
        }
        return maxChainLength;
    }
};
```

## Longest Bitonic Subsequence
https://practice.geeksforgeeks.org/problems/longest-bitonic-subsequence0824/1
Given an array of positive integers. Find the maximum length of Bitonic subsequence.   
A subsequence of array is called Bitonic if it is first strictly increasing, then strictly decreasing. Return the maximum length of bitonic subsequence.  
   
**Note** : A strictly increasing or a strictly decreasing sequence should not be considered as a bitonic sequence

**Examples :**

**Input:** n = 5, nums[] = [1, 2, 5, 3, 2]
**Output:** 5
**Explanation:** The sequence {1, 2, 5} is increasing and the sequence {3, 2} is decreasing so merging both we will get length 5.

**Input:** n = 8, nums[] = [1, 11, 2, 10, 4, 5, 2, 1]
**Output:** 6
**Explanation:** The bitonic sequence {1, 2, 10, 4, 2, 1} has length 6.

```cpp
   class Solution {
public:
    int LongestBitonicSequence(int n, vector<int>& nums) {
        if (n == 0) return 0;
        // Arrays to store the length of increasing and decreasing subsequences
        vector<int> inc(n, 1);
        vector<int> dec(n, 1);
        // Compute the length of the increasing subsequence ending at each index
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j] && inc[i] < inc[j] + 1) {
                    inc[i] = inc[j] + 1;
                }
            }
        }
        // Compute the length of the decreasing subsequence starting at each index
        for (int i = n - 2; i >= 0; i--) {
            for (int j = n - 1; j > i; j--) {
                if (nums[i] > nums[j] && dec[i] < dec[j] + 1) {
                    dec[i] = dec[j] + 1;
                }
            }
        }
        // Combine the results to find the maximum length of the bitonic sequence
        int maxLength = 0;
        for (int i = 0; i < n; i++) {
            if (inc[i] > 1 && dec[i] > 1) { // Ensure there's both increasing and decreasing parts
                maxLength = max(maxLength, inc[i] + dec[i] - 1);
            }
        }
        return maxLength;
    }
};
```

## Number of Longest Increasing Subsequences
https://leetcode.com/problems/number-of-longest-increasing-subsequence/
Given an integer array `nums`, return _the number of longest increasing subsequences._

**Notice** that the sequence has to be **strictly** increasing.

**Example 1:**

**Input:** nums = [1,3,5,4,7]
**Output:** 2
**Explanation:** The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].

**Example 2:**

**Input:** nums = [2,2,2,2,2]
**Output:** 5
**Explanation:** The length of the longest increasing subsequence is 1, and there are 5 increasing subsequences of length 1, so output 5.

```cpp
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int max_seq = 1;
        unordered_map<int,int> mp;
        vector<pair<int,int>> dp(nums.size());
        for(int i=0;i<nums.size();i++){
            int sub_ans = 0; //longest subsequence ending at i index
            int f = 1; // freq of the longest subsequences ending at i index
            for(int j=i-1;j>=0;j--){
                if(nums[j]<nums[i]){
                    if(sub_ans<dp[j].first){
                        sub_ans = dp[j].first;
                        f = dp[j].second; 
                    }
                    else if(sub_ans==dp[j].first) f+=dp[j].second;
                }
            }
            dp[i] = {1+sub_ans,f};
            mp[dp[i].first] += f; //adding the relevant freq in map
            max_seq = max(max_seq,dp[i].first);
        }
        return mp[max_seq];
    }
};
```

# Partition DP
## Matrix Chain Multiplication
https://bit.ly/3Cgg36D
Given a sequence of matrices, find the most efficient way to multiply these matrices together. The efficient way is the one that involves the least number of multiplications.

The dimensions of the matrices are given in an array **arr[]** of size **N** (such that N = number of matrices + 1) where the **ith** matrix has the dimensions **(arr[i-1] x arr[i])**.

**Example 1:**

**Input:** N = 5
arr = {40, 20, 30, 10, 30}
**Output:** 26000
**Explanation:** There are 4 matrices of dimension 
40x20, 20x30, 30x10, 10x30. Say the matrices are 
named as A, B, C, D. Out of all possible combinations,
the most efficient way is (A*(B*C))*D. 
The number of operations are -
20*30*10 + 40*20*10 + 40*10*30 = 26000.

  
**Example 2:**

**Input:** N = 4
arr = {10, 30, 5, 60}
**Output:** 4500
**Explanation:** The matrices have dimensions 
10*30, 30*5, 5*60. Say the matrices are A, B 
and C. Out of all possible combinations,the
most efficient way is (A*B)*C. The 
number of multiplications are -
10*30*5 + 10*5*60 = 4500.

  
**Your Task:**  
You do not need to take input or print anything. Your task is to complete the function **matrixMultiplication()** which takes the value **N** and the array **arr[]** as input parameters and returns the minimum number of multiplication operations needed to be performed.

```cpp
class Solution{
public:
    int matrixMultiplication(int N, int arr[])
    {
        // code here
        vector<vector<int>> dp(N-1, vector<int> (N-1, 1e9));
        for(int len=1;len<=N-1;len++){
            for(int i=0;i+len<N;i++){
                if(len==1){
                    dp[i][i]=0;
                }
                else{
                    int r = i+len-1;
                    for(int j=i;j<r;j++){
                        dp[i][r]=min(dp[i][r], dp[i][j]+dp[j+1][r]+arr[i]*arr[j+1]*arr[r+1]);
                    }
                }
            }
        }
        return dp[0][N-2];
    }
};
```

## Minimum Cost to cut the stick
https://leetcode.com/problems/minimum-cost-to-cut-a-stick/
Given a wooden stick of length `n` units. The stick is labelled from `0` to `n`. For example, a stick of length **6** is labelled as follows:

![](https://assets.leetcode.com/uploads/2020/07/21/statement.jpg)

Given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return _the minimum total cost_ of the cuts.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/23/e1.jpg)

**Input:** n = 7, cuts = [1,3,4,5]
**Output:** 16
**Explanation:** Using cuts order = [1, 3, 4, 5] as in the input leads to the following scenario:
![](https://assets.leetcode.com/uploads/2020/07/21/e11.jpg)
The first cut is done to a rod of length 7 so the cost is 7. The second cut is done to a rod of length 6 (i.e. the second part of the first cut), the third is done to a rod of length 4 and the last cut is to a rod of length 3. The total cost is 7 + 6 + 4 + 3 = 20.
Rearranging the cuts to be [3, 5, 1, 4] for example will lead to a scenario with total cost = 16 (as shown in the example photo 7 + 4 + 3 + 2 = 16).

**Example 2:**

**Input:** n = 9, cuts = [5,6,1,4,2]
**Output:** 22
**Explanation:** If you try the given cuts ordering the cost will be 25.
There are much ordering with total cost <= 25, for example, the order [4, 6, 5, 2, 1] has total cost = 22 which is the minimum possible.

```cpp
class Solution {
public:
    int minCost(int n, std::vector<int>& cuts) {
        std::sort(cuts.begin(), cuts.end());
        int m = cuts.size();
        std::vector<std::vector<int>> dp(m + 2, std::vector<int>(m + 2, 0));

        for (int l = 2; l <= m + 1; l++) {
            for (int i = 0; i + l <= m + 1; i++) {
                int j = i + l;
                dp[i][j] = INT_MAX;
                for (int k = i + 1; k < j; k++) {
                    dp[i][j] = std::min(dp[i][j], dp[i][k] + dp[k][j]);
                }
                int left = (i == 0) ? 0 : cuts[i - 1];
                int right = (j == m + 1) ? n : cuts[j - 1];
                dp[i][j] += right - left;
            }
        }

        return dp[0][m + 1];
    }
};
```

## Burst Balloons
https://leetcode.com/problems/burst-balloons/
You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return _the maximum coins you can collect by bursting the balloons wisely_.

**Example 1:**

**Input:** nums = [3,1,5,8]
**Output:** 167
**Explanation:**
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167

**Example 2:**

**Input:** nums = [1,5]
**Output:** 10
```cpp
class Solution {
public:
    int maxCoins(vector<int>& v) {
        int n=v.size();
        v.insert(v.begin(),1);
        v.push_back(1);
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        for(int i=n;i>=1;i--)
        {
            for(int j=i;j<=n;j++)
            {
                int ans=0;
                for(int k=i;k<=j;k++)
                {
                    int a=v[k]*v[i-1]*v[j+1];
                    a+=dp[i][k-1];
                    a+=dp[k+1][j];
                    ans=max(ans,a);
                }
                dp[i][j]=ans;
            }
        }
        return dp[1][n];
    }
};
```

## Evaluate Boolean Expression to True
https://leetcode.com/problems/parsing-a-boolean-expression/
A **boolean expression** is an expression that evaluates to either `true` or `false`. It can be in one of the following shapes:

- `'t'` that evaluates to `true`.
- `'f'` that evaluates to `false`.
- `'!(subExpr)'` that evaluates to **the logical NOT** of the inner expression `subExpr`.
- `'&(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical AND** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.
- `'|(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical OR** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.

Given a string `expression` that represents a **boolean expression**, return _the evaluation of that expression_.

It is **guaranteed** that the given expression is valid and follows the given rules.

**Example 1:**

**Input:** expression = "&(|(f))"
**Output:** false
**Explanation:** 
First, evaluate |(f) --> f. The expression is now "&(f)".
Then, evaluate &(f) --> f. The expression is now "f".
Finally, return false.

**Example 2:**

**Input:** expression = "|(f,f,f,t)"
**Output:** true
**Explanation:** The evaluation of (false OR false OR false OR true) is true.

**Example 3:**

**Input:** expression = "!(&(f,t))"
**Output:** true
**Explanation:** 
First, evaluate &(f,t) --> (false AND true) --> false --> f. The expression is now "!(f)".
Then, evaluate !(f) --> NOT false --> true. We return true.

**Constraints:**

- `1 <= expression.length <= 2 * 104`
- expression[i] is one following characters: `'('`, `')'`, `'&'`, `'|'`, `'!'`, `'t'`, `'f'`, and `','`.
```cpp
class Solution {
    bool sol(int &i,string &s){
        char exp=s[i];// store which expression is present in firstly
        bool tPresent=0,fPresent=0;// declare two variables for check if true is present and false is present
        i+=2;//it skips '(' and move to expression inside it
        while(s[i-1]!=')'){//if we skip closing bracket then it break 
            if(s[i]=='t')tPresent=true;
            else if(s[i]=='f')fPresent=true;
            else{//if any other operator with another brackets is present
                bool rec=sol(i,s);// do recursive call abd calulate the value
                if(rec)tPresent=true;
                else fPresent=true;
            }
            i+=2;// again skip ','
        }
        i--;// go to index after this ')'
        if(exp=='|')return tPresent;// if true present return true
        if(exp=='&')return !fPresent;// if false present return false
        //if '!' is there
        if(tPresent)return false;
        return true;
    }
public:
    bool parseBoolExpr(string expression) {
        int i=0;
        return sol(i,expression);
    }
};
```

## Palindrome Partitioning
https://leetcode.com/problems/palindrome-partitioning-ii/
Given a string `s`, partition `s` such that every substring of the partition is a palindrome.
Return _the **minimum** cuts needed for a palindrome partitioning of_ `s`.

**Example 1:**

**Input:** s = "aab"
**Output:** 1
**Explanation:** The palindrome partitioning ["aa","b"] could be produced using 1 cut.

**Example 2:**

**Input:** s = "a"
**Output:** 0

**Example 3:**

**Input:** s = "ab"
**Output:** 1

```cpp
bool palindrome(string &s,int i,int j){
    while(i<j){
        if (s[i]!=s[j]){
            return false;
        }
        i++;
        j--;
    }
    return true;
}

int helper(string &s,int i,vector <int> &dp){
    if (i==s.length()){
        return 0;
    }
    if (dp[i]!=-1){
        return dp[i];
    }
    int mini=INT_MAX;
    for(int k=i;k<s.length();k++){
        if (palindrome(s,i,k)){
            int cost=helper(s,k+1,dp);
            if (k!=s.length()-1){
                cost++;
            }
            mini=min(mini,cost);
        }
    }
    dp[i]=mini;
    return dp[i];
}
class Solution {
public:
    int minCut(string s) {
        vector <int> dp(s.length(),-1);
        return helper(s,0,dp);
        //-1 can also be done to the result generated if we do not
        //want to take conditional statement of last index partition.
    }
};
```

## Partition Array for Maximum Sum
https://leetcode.com/problems/partition-array-for-maximum-sum/
Given an integer array `arr`, partition the array into (contiguous) subarrays of length **at most** `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return _the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a **32-bit** integer._

**Example 1:**

**Input:** arr = [1,15,7,9,2,5,10], k = 3
**Output:** 84
**Explanation:** arr becomes [15,15,15,9,10,10,10]

**Example 2:**

**Input:** arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
**Output:** 83

**Example 3:**

**Input:** arr = [1], k = 1
**Output:** 1

```cpp
class Solution {
public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) {
        vector<int> dp(arr.size()+1);
        int n = arr.size();

        for(int i = 1; i<= arr.size(); i++){
            int maxi = 0 , ans=0;
            for(int j = 1;j<= k and i-j>=0; j++){
                cout<<i-j<<max(0,i-j)<<endl;

                maxi = max(maxi, arr[i-j]);
                ans = max(ans, dp[i-j]+maxi*(j));
            }
            dp[i] = ans;
        }
        return dp[n];
    }
};
```

## Maximum Rectangle Area with all 1's
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
```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        
        vector<vector<int>>table(matrix.size(), vector<int>(matrix[0].size()));
        int i, j, k, l, maxi = 0, counter1, counter2;
        for(i = 0; i<matrix.size(); i++){
            table[i][0] = (matrix[i][0] == '1');
        }
        for(i = 0; i<matrix[0].size(); i++){
            table[0][i] = (matrix[0][i] == '1');
        }
        for(i = 1; i<matrix.size(); i++){
            for(j = 1; j<matrix[0].size(); j++){
                if(matrix[i][j] == '1'){
                    table[i][j] = min({table[i][j-1], table[i-1][j], table[i-1][j-1]})+1;
                }
            }
        }
        for(i = 0; i<matrix.size(); i++){
            for(j = 0; j<matrix[0].size(); j++){
                if(matrix[i][j] == '1'){
                    counter1 = table[i][j] * table[i][j];
                    counter2 = table[i][j] * table[i][j];
                    for(k = i+1; k<matrix.size() && table[i][j] <= table[k][j] && table[k][j]; k++){
                        counter1+=table[i][j];
                    }
                    for(l = j+1; l<matrix[0].size() && table[i][j] <= table[i][l] && table[i][l]; l++){
                        counter2+=table[i][j];
                    }
                    maxi = max({counter1, counter2, maxi});
                }
            }
        }
        return maxi;
    }
};
```

## Count Square Submatrices with All Ones
https://leetcode.com/problems/count-square-submatrices-with-all-ones/
Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.

**Example 1:**

**Input:** matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
**Output:** 15
**Explanation:** 
There are **10** squares of side 1.
There are **4** squares of side 2.
There is  **1** square of side 3.
Total number of squares = 10 + 4 + 1 = **15**.

**Example 2:**

**Input:** matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
**Output:** 7
**Explanation:** 
There are **6** squares of side 1.  
There is **1** square of side 2. 
Total number of squares = 6 + 1 = **7**.

```cpp
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        int n=matrix.size(), m=matrix[0].size(), cnt=0;
        vector<vector<int>>dp(n, vector<int>(m, 0));
        for(int i=0; i<n; i++){
            dp[i][0]=matrix[i][0];
        }
        for(int i=0; i<m; i++){
            dp[0][i]=matrix[0][i];
        }
        for(int i=1; i<n; i++){
            for(int j=1; j<m; j++){
                if(matrix[i][j]==1){
                    dp[i][j]=min({dp[i-1][j-1], dp[i-1][j], dp[i][j-1]})+1;
                }
            }
        }
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(matrix[i][j]==1){
                    cnt+=dp[i][j];
                }
            }
        }
        return cnt;
    }
};
```