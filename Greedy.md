# Easy
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
        int cookiesNums = s.size();
        if(cookiesNums == 0)  return 0;
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());

        int maxNum = 0;
        int cookieIndex = cookiesNums - 1;
        int childIndex = g.size() - 1;
        while(cookieIndex >= 0 && childIndex >=0){
            if(s[cookieIndex] >= g[childIndex]){
                maxNum++;
                cookieIndex--;
                childIndex--;
            }
            else{
                childIndex--;
            }
        }
        return maxNum;
    }
};
```

## Fractional Knapsack Problem
https://practice.geeksforgeeks.org/problems/fractional-knapsack-1587115620/1
Given weights and values of **n** items, we need to put these items in a knapsack of capacity **w** to get the **maximum** total value in the knapsack. Return a double value representing the maximum value in knapsack.  
**Note:** Unlike 0/1 knapsack, you are **allowed** to break the item here. The details of structure/class is defined in the comments above the given function.

**Examples :**

**Input:** n = 3, w = 50, value[] = [60,100,120], weight[] = [10,20,30]
**Output:** 240.000000
**Explanation:** Take the item with value 60 and weight 10, value 100 and weight 20 and split the third item with value 120 and weight 30, to fit it into weight 20. so it becomes (120/30)*20=80, so the total value becomes 60+100+80.0=240.0 Thus, total maximum value of item we can have is 240.00 from the given capacity of sack. 

**Input:** n = 2, w = 50, value[] = [60,100], weight[] = [10,20]
**Output:** 160.000000
**Explanation:** Take both the items completely, without breaking. Total maximum value of item we can have is 160.00 from the given capacity of sack.

```cpp
// class implemented
/*
struct Item{
    int value;
    int weight;
};
*/

class Solution {
  public:
  static bool compareByWeight(const Item &a, const Item &b) {
    return (double)(a.value)/(double)(a.weight) > (double)(b.value)/(double)(b.weight);
  }
    // Function to get the maximum total value in the knapsack.
    double fractionalKnapsack(int w, Item arr[], int n) {
        // Your code here
        double sum =0;
         sort(arr, arr+n, compareByWeight);
        if(arr[0].weight>w){
            return (double)(arr[0].value)/(double)(arr[0].weight)*w;
        }
       
        for(int i=0; i<n ; i++){
            if((w-arr[i].weight)<0){
                sum+=(double)(arr[i].value)/(double)(arr[i].weight)*w;
               break;
            }
            else{
                 w-=arr[i].weight;
                 sum+=arr[i].value;
            } 
        }
        return sum;
    }
};
```

## Greedy algo to find the min number of coins
https://www.geeksforgeeks.org/find-minimum-number-of-coins-that-make-a-change/
Given an array ****coins[]**** of size ****N**** and a target value ****V****, where ****coins[i]**** represents the coins of different denominations. You have an ****infinite supply**** of each of coins. The task is to find minimum number of coins required to make the given value ****V****. If it’s not possible to make a change, print ****-1****.

****Examples:****  

> ****Input:**** coins[] = {25, 10, 5}, V = 30  
> ****Output:**** Minimum 2 coins required We can use one coin of 25 cents and one of 5 cents 
> 
> ****Input:**** coins[] = {9, 6, 5, 1}, V = 11  
> ****Output:**** Minimum 2 coins required We can use one coin of 6 cents and 1 coin of 5 cents

```cpp

using namespace std;
int main() {
  int V = 49;
  vector < int > ans;
  int coins[] = {1, 2, 5, 10, 20, 50, 100, 500, 1000};
  int n = 9;
  for (int i = n - 1; i >= 0; i--) {
    while (V >= coins[i]) {
      V -= coins[i];
      ans.push_back(coins[i]);
    }
  }
  cout<<"The minimum number of coins is "<<ans.size()<<endl;
  cout<<"The coins are "<<endl;
  for (int i = 0; i < ans.size(); i++) {
    cout << ans[i] << " ";
  }

  return 0;
}
```

## Lemonade Change
https://leetcode.com/problems/lemonade-change/
At a lemonade stand, each lemonade costs `$5`. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a `$5`, `$10`, or `$20` bill. You must provide the correct change to each customer so that the net transaction is that the customer pays `$5`.

Note that you do not have any change in hand at first.

Given an integer array `bills` where `bills[i]` is the bill the `ith` customer pays, return `true` _if you can provide every customer with the correct change, or_ `false` _otherwise_.

**Example 1:**

**Input:** bills = [5,5,5,10,20]
**Output:** true
**Explanation:** 
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.

**Example 2:**

**Input:** bills = [5,5,10,10,20]
**Output:** false
**Explanation:** 
From the first two customers in order, we collect two $5 bills.
For the next two customers in order, we collect a $10 bill and give back a $5 bill.
For the last customer, we can not give the change of $15 back because we only have two $10 bills.
Since not every customer received the correct change, the answer is false.

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int fives = 0, tens = 0;
        for (int bill : bills) {
            if (bill == 5) {
                fives++;
            } else if (bill == 10) {
                if (fives == 0) {
                    return false;
                }
                fives--;
                tens++;
            } else {
                if (tens > 0 && fives > 0) {
                    tens--;
                    fives--;
                } else if (fives >= 3) {
                    fives -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
};
```

# Med/Hard

## N meetings in one room
https://practice.geeksforgeeks.org/problems/n-meetings-in-one-room-1587115620/1
```cpp
class Solution
{
    public:
    //Function to find the maximum number of meetings that can
    //be performed in a meeting room.
    static bool comp(pair<int,int>a,pair<int,int>b){
        return a.second<b.second;
    }
    int maxMeetings(int start[], int end[], int n)
    {
        vector<pair<int,int>>vec;
        for(int i=0;i<n;i++){
            vec.push_back({start[i],end[i]});
        }
        sort(vec.begin(),vec.end(),comp);
        int count=1;
        int meetend=vec[0].second;
        for(int i=1;i<n;i++){
            int start=vec[i].first;
            if(start>meetend){
                count++;
                meetend=vec[i].second;
            }
        }
        return count;
    }
};
```

## Jump Game
https://leetcode.com/problems/jump-game/
You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` _if you can reach the last index, or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [2,3,1,1,4]
**Output:** true
**Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [3,2,1,0,4]
**Output:** false
**Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

```cpp
class Solution {
    public:
    bool canJump(vector<int>& nums) {
     
        int n=nums.size();
        int reachable=0;
        
        for(int i=0;i<n;i++){
            if(i>reachable) return false;
            reachable=max(reachable,i+nums[i]);
        }
        return true;
        
    }
};
```

## Jump Game II
https://leetcode.com/problems/jump-game-ii/
You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]` and
- `i + j < n`

Return _the minimum number of jumps to reach_ `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

**Example 1:**

**Input:** nums = [2,3,1,1,4]
**Output:** 2
**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [2,3,0,1,4]
**Output:** 2

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        
        if(nums.size()<2) return 0;   //base case
        
        //initialize jump=1 , we are taking jump from 0th index to the range mxjump
        //currjump, we can take jump from particular  index
		//mxjump , we cango up to maximum
		// jump to count no. of jump
        int jump=1,n=nums.size(),currjmp=nums[0],mxjmp=nums[0];
        
        int i=0;
		
		//till we reach last index, NOTE: Not necessary to cross last index
        while(i<n-1)
        {
            mxjmp=max(mxjmp,i+nums[i]);
             
            if(currjmp==i) //we have to take jump now because our currjump now ends.
            {
                jump++;//increment in jump
                currjmp=mxjmp; //assign new maxjmp to currjmp
            }
            i++;
        }
        return jump;
    }
};
```

## Min platforms required for a railway
https://practice.geeksforgeeks.org/problems/minimum-platforms-1587115620/1#
Given arrival and departure times of all trains that reach a railway station. Find the minimum number of platforms required for the railway station so that no train is kept waiting.  
Consider that all the trains arrive on the same day and leave on the same day. Arrival and departure time can never be the same for a train but we can have arrival time of one train equal to departure time of the other. At any given instance of time, same platform can not be used for both departure of a train and arrival of another train. In such cases, we need different platforms**.**

**Examples:**

**Input**: n = 6, arr[] = {0900, 0940, 0950, 1100, 1500, 1800},   
            dep[] = {0910, 1200, 1120, 1130, 1900, 2000}
**Output**: 3
**Explanation**: There are three trains during the time 0940 to 1200. So we need minimum 3 platforms.

**Input**: n = 3, arr[] = {0900, 1235, 1100},   
            dep[] = {1000, 1240, 1200}
**Output**: 1
**Explanation**: All train times are mutually exlusive. So we need only one platform

**Input**: n = 3, arr[] = {1000, 0935, 1100},   
            dep[] = {1200, 1240, 1130}
**Output**: 3
**Explanation**: All 3 trains have to be their from 11:00 to 11:30
```cpp
class Solution{
    public:
    //Function to find the minimum number of platforms required at the
    //railway station such that no train waits.
    int findPlatform(int arr[], int dep[], int n)
    {
    	// Your code here
    	sort (arr , arr + n);
    	sort (dep , dep + n);
    	int count = 1 ;
    	int ans = 1;
    	int i = 1;
    	int j = 0;
    	while ( i< n && j < n ){
    	if (arr[i] <= dep[j]){
    	    count ++;
    	    i++;
    	}
    	else{
    	    count--;
    	j++;
    	}
    	
    	ans = max(ans,count);
    	}
    	return ans;
    }
};
```

## Job Sequencing
https://practice.geeksforgeeks.org/problems/job-sequencing-problem-1587115620/1#
Given a set of **N** jobs where each **jobi** has a deadline and profit associated with it.

Each job takes **_1_** unit of time to complete and only one job can be scheduled at a time. We earn the profit associated with job if and only if the job is completed by its deadline.

Find the number of jobs done and the **maximum profit**.

**Note:** Jobs will be given in the form (Jobid, Deadline, Profit) associated with that Job. Deadline of the job is the time before which job needs to be completed to earn the profit.
```cpp
/*
struct Job 
{ 
    int id;	 // Job Id 
    int dead; // Deadline of job 
    int profit; // Profit if job is over before or on deadline 
};
*/

class Solution 
{
    public:
    //Function to find the maximum profit and the number of jobs done.
    //Sorting on basis of Profit
    static bool profitComparator(Job a1, Job a2) {
        return a1.profit > a2.profit;
    }
    vector<int> JobScheduling(Job arr[], int n) 
    { 
        // your code here
        vector<int> ans;
        
        sort(arr,arr+n,profitComparator);
        int max_dead=0;
        for(int i=0;i<n;i++){
            max_dead=max(max_dead,arr[i].dead);
        }
        
        vector<int> schedule(max_dead+1,-1);
        
        int count=0;
        int maxProfit=0;
        for (int i = 0; i < n; i++) {
        // Find a free slot for this job (starting from its deadline)
            for (int j = arr[i].dead; j > 0; j--) {
                if (schedule[j] == -1) {
                    schedule[j] = arr[i].id; // Assign this job to the free slot
                    count++;
                    maxProfit += arr[i].profit;
                    break;
                }
            }
        }
    
        ans = {count, maxProfit};
        return ans;
    } 
};
```

## Candy
https://leetcode.com/problems/candy/
There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return _the minimum number of candies you need to have to distribute the candies to the children_.

**Example 1:**

**Input:** ratings = [1,0,2]
**Output:** 5
**Explanation:** You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

**Example 2:**

**Input:** ratings = [1,2,2]
**Output:** 4
**Explanation:** You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.

```cpp
class Solution {
public:
    int candy(std::vector<int>& ratings) {
        int n = ratings.size();
        std::vector<int> candies(n, 1);

        for (int i = 1; i < n; ++i) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }

        for (int i = n - 2; i >= 0; --i) {
            if (ratings[i] > ratings[i + 1]) {
                candies[i] = std::max(candies[i], candies[i + 1] + 1);
            }
        }

        int totalCandies = 0;
        for (int candy : candies) {
            totalCandies += candy;
        }

        return totalCandies;
    }
};
```

## Shortest Job First
Geek is a software engineer. He is assigned with the task of calculating **average waiting time** of all the processes by following **shortest job first** policy.

The shortest job first (SJF) or shortest job next, is a scheduling policy that selects the waiting process with the smallest execution time to execute next.

Given an array of integers **bt** of size **n**. Array **bt** denotes the **burst time** of each process. Calculate the **average waiting time** of all the processes and return the nearest integer which is smaller or equal to the output.

**Note:** Consider all process are available at time 0.

**Example 1:**

**Input:**
n = 5
bt = [4,3,7,1,2]
**Output:** 4
**Explanation:** After sorting burst times by shortest job policy, calculated average waiting time is 4.

**Example 2:**

**Input:**
n = 4
arr = [1,2,3,4]
**Output:** 2
Explanation: After sorting burst times by shortest job policy, calculated average waiting time is 2.

```cpp
// User function Template for C++

// Back-end complete function Template for C++

class Solution {
  public:
    long long solve(vector<int>& bt) {
         sort(bt.begin(),bt.end());
         int start_time=0;
         int waiting_time=0;
         for(auto x:bt)
         {
             waiting_time=waiting_time+start_time;
             start_time=start_time+x;
         }
         return waiting_time/bt.size();
    }
};
```

## Insert Interval
https://leetcode.com/problems/insert-interval/
You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` _after the insertion_.

**Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

**Example 1:**

**Input:** intervals = [[1,3],[6,9]], newInterval = [2,5]
**Output:** [[1,5],[6,9]]

**Example 2:**

**Input:** intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
**Output:** [[1,2],[3,10],[12,16]]
**Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

```cpp
class Solution {
public:
   vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int n = intervals.size(), i = 0;
        vector<vector<int>> res;
        //case 1: no overlapping case before the merge intervals
		//compare ending point of intervals to starting point of newInterval
        while(i < n && intervals[i][1] < newInterval[0]){
            res.push_back(intervals[i]);
            i++;
        }                           
		//case 2: overlapping case and merging of intervals
        while(i < n && newInterval[1] >= intervals[i][0]){
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);
            i++;
        }
        res.push_back(newInterval);
        // case 3: no overlapping of intervals after newinterval being merged
        while(i < n){
            res.push_back(intervals[i]);
            i++;
        }
        return res;
    }
};
```

## Merge Intervals
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
        int n=intervals.size();
        vector<vector<int>> sol;

        sort(intervals.begin(), intervals.end());

        for(int i=0;i<n;i++){
            int maxEnd=intervals[i][1];
            int j=i+1;

            while((j<n)&&(intervals[j][0]<=maxEnd)){
                maxEnd=max(maxEnd, intervals[j][1]);
                j++;
            }
            sol.push_back({intervals[i][0], maxEnd});
            i=j-1;
        }   
        return sol;
    }
};
```

## Non overlapping intervals
https://leetcode.com/problems/non-overlapping-intervals/
Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping_.

**Example 1:**

**Input:** intervals = [[1,2],[2,3],[3,4],[1,3]]
**Output:** 1
**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.

**Example 2:**

**Input:** intervals = [[1,2],[1,2],[1,2]]
**Output:** 2
**Explanation:** You need to remove two [1,2] to make the rest of the intervals non-overlapping.

**Example 3:**

**Input:** intervals = [[1,2],[2,3]]
**Output:** 0
**Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.
```cpp
bool comp(vector<int> &a,vector<int> &b) {
	return a[1]<b[1];
}
class Solution {
public:
	int eraseOverlapIntervals(vector<vector<int>>& intervals) {
		int ans=-1;      
		if(intervals.size()==0) return 0;       
		sort(intervals.begin(),intervals.end(),comp);      //custom comperator is used.
		vector<int> prev= intervals[0];

		for(vector<int> i: intervals) {
			if(prev[1]>i[0]) {
				ans++;                //we dont update previous, because i[1] will be grater then prev[1]
			}else prev=i;           // we want the end point to be minimum
		}
		return ans;                 //ans was initially made -1 because our prev and intervals[0] will always match
	}
};
```