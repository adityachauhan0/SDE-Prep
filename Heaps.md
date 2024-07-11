## Kth Largest Element in an Array
https://leetcode.com/problems/kth-largest-element-in-an-array
Using min-heap
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> min_heap;
        for (int num : nums) {
            min_heap.push(num);
            if (min_heap.size() > k) {
                min_heap.pop();
            }
        }
        return min_heap.top();
    }
};
```
max-heap se its obv

## Sort k sorted array\
https://bit.ly/3QLpaAs
Given **k** sorted arrays arranged in the form of a matrix of size **k** * **k**. The task is to merge them into one sorted array. Return the merged sorted array ( as a pointer to the merged sorted arrays in **cpp,** as an ArrayList in **java,** and list in **python**).

  
**Examples :**

**Input:** k = 3, arr[][] = {{1,2,3},{4,5,6},{7,8,9}}
**Output:** 1 2 3 4 5 6 7 8 9
**Explanation:** Above test case has 3 sorted arrays of size 3, 3, 3 arr[][] = [[1, 2, 3],[4, 5, 6],[7, 8, 9]]. The merged list will be [1, 2, 3, 4, 5, 6, 7, 8, 9].

**Input:** k = 4, arr[][]={{1,2,3,4},{2,2,3,4},{5,5,6,6},{7,8,9,9}}
**Output:** 1 2 2 2 3 3 4 4 5 5 6 6 7 8 9 9 
**Explanation:** Above test case has 4 sorted arrays of size 4, 4, 4, 4 arr[][] = [[1, 2, 2, 2], [3, 3, 4, 4], [5, 5, 6, 6], [7, 8, 9, 9 ]]. The merged list will be [1, 2, 2, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7, 8, 9, 9].

```cpp
class Solution
{
    public:
    //Function to merge k sorted arrays.
    vector<int> mergeKArrays(vector<vector<int>> arr, int K)
    {
        //code here
        priority_queue<int, vector<int>, greater<int>> pq;
        for(int i=0;i<arr.size();i++){
            for(int j=0;j<arr[i].size();j++){
                pq.push(arr[i][j]);
            }
        }
        vector<int> ans;
        while(!pq.empty()){
            ans.push_back(pq.top());
            pq.pop();
        }
        return ans;
    }
};
```

## Merge M sorted Lists
https://leetcode.com/problems/merge-k-sorted-lists/
You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

**Input:** lists = [[1,4,5],[1,3,4],[2,6]]
**Output:** [1,1,2,3,4,4,5,6]
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6

**Example 2:**

**Input:** lists = []
**Output:** []

**Example 3:**

**Input:** lists = [[]]
**Output:** []

```cpp
struct compare {
    bool operator()(const ListNode* l, const ListNode* r) {
        return l->val > r->val;
    }
};
ListNode *mergeKLists(vector<ListNode *> &lists) { //priority_queue
    priority_queue<ListNode *, vector<ListNode *>, compare> q;
    for(auto l : lists) {
        if(l)  q.push(l);
    }
    if(q.empty())  return NULL;

    ListNode* result = q.top();
    q.pop();
    if(result->next) q.push(result->next);
    ListNode* tail = result;            
    while(!q.empty()) {
        tail->next = q.top();
        q.pop();
        tail = tail->next;
        if(tail->next) q.push(tail->next);
    }
    return result;
}
```

## Replace each array element by its rank
https://bit.ly/3TS3jcg
Given an array **arr** of **N** integers, the task is to replace each element of the array by its rank in the array. The **rank** of an element is defined as the distance between the element with the first element of the array when the array is arranged in ascending order. If two or more are same in the array then their rank is also the same as the rank of the first occurrence of the element. 

**Example 1**:

**Input**:
N = 6
arr = [20, 15, 26, 2, 98, 6]
**Output**:
4, 3, 5, 1, 6, 2
**Explanation**:
After sorting, array becomes {2,6,15,20,26,98}
Rank(2) = 1 (at index 0) 
Rank(6) = 2 (at index 1) 
Rank(15) = 3 (at index 2) 
Rank(20) = 4 (at index 3) and so on..

**Example 2**:

**Input**:
N = 4
arr = [2, 2, 1, 6]
**Output**:
2, 2, 1, 3
**Explanation**:
After sorting, array becomes {1, 2, 2, 6}
Rank(1) = 1 (at index 0) 
Rank(2) = 2 (at index 1) 
Rank(2) = 2 (at index 2) 
Rank(6) = 3 (at index 3)
Rank(6) = 3 because rank after 2 is 3 as rank 
of same element remains same and for next element 
increases by 1.

```cpp
//User function Template for C++

class Solution{
public:

    vector<int> replaceWithRank(vector<int> &arr, int N){
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> pq;
        for(int i=0;i<N;i++){
            pq.push({arr[i],i});
        }
        int ct = 0;
        int prev = -1;
        while(!pq.empty()){
            auto it = pq.top();
            pq.pop();
            if(it.first != prev){
                ct++;
            }
            arr[it.second] = ct;
            prev = it.first;
        }
        return arr;
    }

};
```

## Task Scheduler
https://leetcode.com/problems/task-scheduler/
You are given an array of CPU `tasks`, each represented by letters A to Z, and a cooling time, `n`. Each cycle or interval allows the completion of one task. Tasks can be completed in any order, but there's a constraint: **identical** tasks must be separated by at least `n` intervals due to cooling time.

​Return the _minimum number of intervals_ required to complete all tasks.

**Example 1:**

**Input:** tasks = ["A","A","A","B","B","B"], n = 2

**Output:** 8

**Explanation:** A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.

After completing task A, you must wait two cycles before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th cycle, you can do A again as 2 intervals have passed.

**Example 2:**

**Input:** tasks = ["A","C","A","B","D","B"], n = 1

**Output:** 6

**Explanation:** A possible sequence is: A -> B -> C -> D -> A -> B.

With a cooling interval of 1, you can repeat a task after just one other task.

**Example 3:**

**Input:** tasks = ["A","A","A", "B","B","B"], n = 3

**Output:** 10

**Explanation:** A possible sequence is: A -> B -> idle -> idle -> A -> B -> idle -> idle -> A -> B.

There are only two types of tasks, A and B, which need to be separated by 3 intervals. This leads to idling twice between repetitions of these tasks.

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        priority_queue<int> pq;
        vector<int>mp(26,0);

        for(char i:tasks){
            mp[i-'A']++;  // count the number of times a task needs to be done
        }   
        for(int i=0;i<26;++i){
            if(mp[i]) 
            pq.push(mp[i]);
        }

        int time=0; // stores the total time taken 
        while(!pq.empty()){
            vector<int>remain;
            int cycle=n+1;  // n+1 is the CPU cycle length, if n is the cooldown period then after a task A there will be n more tasks. Hence n+1.

            while(cycle and !pq.empty()){
                int max_freq=pq.top(); // the task at the top should be first assigned the CPU as it has highest frequency
                pq.pop();
                if(max_freq>1){ // task with more than one occurrence, the next occurrence will be done in the next cycle 
                    remain.push_back(max_freq-1); // add it to remaining task list
                }
                ++time; 
                --cycle; 
            }

            for(int count:remain){
                pq.push(count); 
            }
            if(pq.empty())break; // if the priority queue is empty than all tasks are completed and we don't need to include the idle time
            time+=cycle; // this counts the idle time 
        }
        return time;
    }
};
```

## Hands Of Straights
https://leetcode.com/problems/hand-of-straights/
Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the `ith` card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.

**Example 1:**

**Input:** hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
**Output:** true
**Explanation:** Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]

**Example 2:**

**Input:** hand = [1,2,3,4,5], groupSize = 4
**Output:** false
**Explanation:** Alice's hand can not be rearranged into groups of 4.

```cpp
class Solution {
public:
    using int2=pair<int, int>;
    static bool isNStraightHand(vector<int>& hand, int groupSize) {
        const int n=hand.size();
        if (n%groupSize!=0) return 0;
        unordered_map<int, int> mp;
        for(int x: hand) mp[x]++;
        priority_queue<int2> pq(mp.begin(), mp.end());// Max heap
        while(!pq.empty()){
            auto [x, f]=pq.top(); pq.pop();
            vector<int2> group;
            group.reserve(groupSize-1);
            for(int k=1; k<groupSize; k++){
                auto [y, f2]=pq.top(); pq.pop();
                f2-=f;
                if(y!=x-k || f2<0) return 0;
                if( f2>0) group.emplace_back(y, f2);
            }
            for(auto& pair: group) pq.push(pair);
        }
        return 1;
    }
};
```

## Design twitter
https://leetcode.com/problems/design-twitter/
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the `10` most recent tweets in the user's news feed.

Implement the `Twitter` class:

- `Twitter()` Initializes your twitter object.
- `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
- `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be **ordered from most recent to least recent**.
- `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
- `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

**Example 1:**

**Input**
["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
[[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
**Output**
[null, null, [5], null, null, [6, 5], null, [5]]

**Explanation**
Twitter twitter = new Twitter();
twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5]. return [5]
twitter.follow(1, 2);    // User 1 follows user 2.
twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 2 tweet ids -> [6, 5]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.unfollow(1, 2);  // User 1 unfollows user 2.
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5], since user 1 is no longer following user 2.

```cpp
class Twitter {
    long long time;
    unordered_map<int,set<int>>following;
    unordered_map<int,vector<pair<int,int>>>tweets;
  public:
  
    Twitter() {
        time=0;
    }


    void postTweet(int userId, int tweetId) {
        tweets[userId].push_back({time++,tweetId});
    }


    vector<int> getNewsFeed(int userId) {
        vector<int>res;
        priority_queue<pair<int,int>>pq;
        for(auto it:tweets[userId])pq.push(it);
        for(int follower:following[userId]){
            for(auto it:tweets[follower])pq.push(it);
        }
        while(!pq.empty() && res.size()<10){
            res.push_back(pq.top().second);
            pq.pop();
        }
        return res;
    }


    void follow(int followerId, int followeeId) {
        following[followerId].insert(followeeId);
    }


    void unfollow(int followerId, int followeeId) {
        following[followerId].erase(followeeId);
    }
};
```

## Kth largest element in a stream of running integers
https://leetcode.com/problems/kth-largest-element-in-a-stream
Design a class to find the `kth` largest element in a stream. Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Implement `KthLargest` class:

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of integers `nums`.
- `int add(int val)` Appends the integer `val` to the stream and returns the element representing the `kth` largest element in the stream.

**Example 1:**

**Input**
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
**Output**
[null, 4, 5, 5, 8, 8]

**Explanation**
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3);   // return 4
kthLargest.add(5);   // return 5
kthLargest.add(10);  // return 5
kthLargest.add(9);   // return 8
kthLargest.add(4);   // return 8

```cpp
class KthLargest {
public:
    KthLargest(int k, vector<int>& nums) {
        // Initialize min-heap with the first k elements
        this->k = k;
        for (int i = 0; i < nums.size(); i++) {
            minHeap.push(nums[i]);
        }
        // Keep only the k largest elements
        while (minHeap.size() > this->k) {
            minHeap.pop();
        }
    }
    
    int add(int val) {
        // Add new element to the min-heap
        minHeap.push(val);
        // If heap size exceeds k, remove the smallest element
        if (minHeap.size() > k) {
            minHeap.pop();
        }
        // Return the current kth largest element
        return minHeap.top();
    }
private:
    int k;
    priority_queue<int, vector<int>, greater<int>> minHeap;
};
```

## Maximum sum combination
https://www.interviewbit.com/problems/maximum-sum-combinations/
```cpp
#define PII pair<int, pair<int, int>>

vector<int> Solution::solve(vector<int> &A, vector<int> &B, int C) {
    int n = A.size();
    vector<int> ans(C);
    
	// Sorted in Descending order
    sort(A.begin(), A.end(), greater<int>());
    sort(B.begin(), B.end(), greater<int>());
    
	// Max Heap
    priority_queue<PII> q;
	
	// Pushed all elements of A wrt the largest element of B
    for(int i = 0; i < n; i++)
        q.push({A[i]+B[0], {i, 0}});
        
    PII t;
    int ind = 0;
    while(ind < C){
        t = q.top();
        ans[ind++] = t.first;
        q.pop();
        int i = t.second.first, j = t.second.second;
		// Only have to push the Sum of current largest element of A wrt the next largest element of B
        q.push({A[i] + B[j+1], {i, j+1}});
    }    
    return ans;
}
```

## Find median from data stream
https://leetcode.com/problems/find-median-from-data-stream/
The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

- For example, for `arr = [2,3,4]`, the median is `3`.
- For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

**Example 1:**

**Input**
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
**Output**
[null, null, null, 1.5, null, 2.0]

**Explanation**
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0

```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    priority_queue<int, vector<int>, greater<int> > minHeap;
	priority_queue<int> maxHeap;
    MedianFinder(){}

    void addNum(int num) {
        if (maxHeap.empty() or maxHeap.top() > num) {
			maxHeap.push(num);
		} else {
			minHeap.push(num);
		}

		if (maxHeap.size() > minHeap.size() + 1) {
			minHeap.push(maxHeap.top());
			maxHeap.pop();
		} else if (minHeap.size() > maxHeap.size() + 1) {
			maxHeap.push(minHeap.top());
			minHeap.pop();
		}
    }
    
    double findMedian() {
        if (maxHeap.size() == minHeap.size()) {
			if (maxHeap.empty()) {
				return 0;
			} else {
				double avg = (maxHeap.top() + minHeap.top()) / 2.0;
				return avg;
			}

		} else {
			return maxHeap.size() > minHeap.size() ? maxHeap.top() : minHeap.top();
		}
    }
};
```

## Find K most frequent elements
https://leetcode.com/problems/top-k-frequent-elements/
Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,1,1,2,2,3], k = 2
**Output:** [1,2]

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        priority_queue<pair<int,int>>pq;
        unordered_map<int,int>mpp;
        for(auto i:nums){
            mpp[i]++;
        }
        for(auto i:mpp){
            pq.push({i.second,i.first});
        }
        vector<int>ans;
        while(k-- && !pq.empty()){
            ans.push_back(pq.top().second);
            pq.pop();
        }
        return ans;
    }
};
```