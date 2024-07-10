# Miscellaneous
## ATOI Recursive
https://leetcode.com/problems/string-to-integer-atoi/
```c++
class Solution {
    int myAtoi(string s, int sign, int i, int result) {

        if (i >= s.size() || s[i] < '0' || s[i] > '9')
            return sign * result;

        int tmp = s[i] - '0';
        if (result > INT_MAX / 10 || result == INT_MAX / 10 && tmp > 7)
            return sign > 0 ? INT_MAX : INT_MIN;

        return myAtoi(s, sign, i + 1, result * 10 + tmp);
    }

public:
    int myAtoi(string s) {
        
        int i = 0, n = s.size(), sign = 1;
        while (i < n && s[i] == ' ')
            ++i;

        if (s[i] == '-')
            sign = -1, ++i;
        else if (s[i] == '+')
            ++i;

        return myAtoi(s, sign, i, 0);
    }
};
```

## Pow(x,n)
https://leetcode.com/problems/powx-n/
Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `xn`).

**Example 1:**

**Input:** x = 2.00000, n = 10
**Output:** 1024.00000

**Example 2:**

**Input:** x = 2.10000, n = 3
**Output:** 9.26100

**Example 3:**

**Input:** x = 2.00000, n = -2
**Output:** 0.25000
**Explanation:** 2-2 = 1/22 = 1/4 = 0.25

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if(n==0) return 1;
        if(n<0) {
            n = abs(n);
            x = 1/x;
        }
        if(n%2==0){
            return myPow(x*x, n/2);
        }
        else{
            return x*myPow(x, n-1);
        }
    }
};
```

## Count Good Numbers
https://leetcode.com/problems/count-good-numbers
A digit string is **good** if the digits **(0-indexed)** at **even** indices are **even** and the digits at **odd** indices are **prime** (`2`, `3`, `5`, or `7`).

- For example, `"2582"` is good because the digits (`2` and `8`) at even positions are even and the digits (`5` and `2`) at odd positions are prime. However, `"3245"` is **not** good because `3` is at an even index but is not even.

Given an integer `n`, return _the **total** number of good digit strings of length_ `n`. Since the answer may be large, **return it modulo** `109 + 7`.

A **digit string** is a string consisting of digits `0` through `9` that may contain leading zeros.

**Example 1:**

**Input:** n = 1
**Output:** 5
**Explanation:** The good numbers of length 1 are "0", "2", "4", "6", "8".

**Example 2:**

**Input:** n = 4
**Output:** 400

**Example 3:**

**Input:** n = 50
**Output:** 564908303

```cpp
class Solution {
public:
    int M = 1e9+7;
    long long power(long long b, long long e, long long ans) {
        if(e == 0) return ans;
        if(e & 1) return power(b, e - 1, (ans * b) % M);
        else return power((b * b) % M, e / 2, ans);
    }
    int countGoodNumbers(long long n) {
        long long temp = power(20, n/2, 1);
        return (n & 1) ? (temp * 5) % M : temp;
    }
};
```
a more normal solution
```cpp
#define mod 1000000007
class Solution 
{
private:

long long int modPower(int num,long long int ind)
{
    if(ind==0)
        return 1;
    
    long long int temp=modPower(num,ind/2);
    if(ind%2==0)
        return (temp%mod * temp%mod)%mod;
    
    return (((long long int)num%mod) * (temp%mod) * (temp%mod))%mod;
}

public:
    int countGoodNumbers(long long n) 
    {
        if(n%2==0)
            (modPower(5,n/2)%mod *modPower(4,n/2)%mod)%mod;
        
        return (modPower(5,n/2+1)*modPower(4,n/2))%mod;
    }
};
```

## Sort a stack using recursion
https://www.geeksforgeeks.org/problems/sort-a-stack/1
Given a stack, the task is to sort it such that the top of the stack has the greatest element.

**Example 1:**

**Input:**
Stack: 3 2 1
**Output:** 3 2 1

**Example 2:**

**Input:**
Stack: 11 2 32 3 41
**Output:** 41 32 11 3 2

```cpp
/*The structure of the class is
class SortedStack{
public:
	stack<int> s;
	void sort();
};
*/

/* The below method sorts the stack s 
you are required to complete the below method */

void insert(stack<int> &s, int top){
    if(s.empty() || (!s.empty() && s.top()<top)){
        s.push(top);
        return ;
    }
    
    int n= s.top();
    s.pop();
    insert(s,top);
    s.push(n);
}

void SortedStack :: sort()
{
    //Your code here
   if(s.empty()) return ;
   
   int top=s.top();
   s.pop();
   sort();
   insert(s,top);
}
```

## Reverse a stack using recursion
https://bit.ly/3podAiY
```cpp
//User function Template for C++

class Solution{
public:
void rev(int temp,stack<int>&st){
    if (st.empty()){
        st.push(temp);
        return ;
    }
    int tmp2 = st.top();
    st.pop();
    rev(temp,st);
    st.push(tmp2);
}
    void Reverse(stack<int> &st){
        if (!st.empty()){
            int temp =st.top();
            st.pop();
            Reverse(st);
            rev(temp,st);
        }
    }
};
```

# Subsequences Pattern
## Generate all binary strings
https://bit.ly/3QJ0vwc
Given an integer, K. Generate all binary strings of size k without consecutive 1’s.

****Examples:**** 

****Input :**** K = 3    
****Output :**** 000 , 001 , 010 , 100 , 101   
****Input :**** K  = 4   
****Output :****0000 0001 0010 0100 0101 1000 1001 1010

```cpp
// C++ program to Generate 
// all binary string without
// consecutive 1's of size K
#include<bits/stdc++.h>
using namespace std ;

// A utility function generate all string without
// consecutive 1'sof size K
void generateAllStringsUtil(int K, char str[], int n)
{
	
	// Print binary string without consecutive 1's
	if (n == K)
	{
		
		// Terminate binary string
		str[n] = '\0' ;
		cout << str << " ";
		return ;
	}

	// If previous character is '1' then we put
	// only 0 at end of string
	//example str = "01" then new string be "010"
	if (str[n-1] == '1')
	{
		str[n] = '0';
		generateAllStringsUtil (K , str , n+1);
	}

	// If previous character is '0' than we put
	// both '1' and '0' at end of string
	// example str = "00" then 
	// new string "001" and "000"
	if (str[n-1] == '0')
	{
		str[n] = '0';
		generateAllStringsUtil(K, str, n+1);
		str[n] = '1';
		generateAllStringsUtil(K, str, n+1) ;
	}
}

// Function generate all binary string without
// consecutive 1's
void generateAllStrings(int K )
{
	// Base case
	if (K <= 0)
		return ;

	// One by one stores every 
	// binary string of length K
	char str[K];

	// Generate all Binary string 
	// starts with '0'
	str[0] = '0' ;
	generateAllStringsUtil ( K , str , 1 ) ;

	// Generate all Binary string 
	// starts with '1'
	str[0] = '1' ;
	generateAllStringsUtil ( K , str , 1 );
}

// Driver program to test above function
int main()
{
	int K = 3;
	generateAllStrings (K) ;
	return 0;
}

```

## Generate Parenthesis
https://leetcode.com/problems/generate-parentheses/
Given `n` pairs of parentheses, write a function to _generate all combinations of well-formed parentheses_.

**Example 1:**

**Input:** n = 3
**Output:** ["((()))","(()())","(())()","()(())","()()()"]

**Example 2:**

**Input:** n = 1
**Output:** ["()"]

**Constraints:**

- `1 <= n <= 8`
```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        addingpar(res, "", n, 0);
        return res;
    }
    void addingpar(vector<string> &v, string str, int n, int m){
        if(n==0 && m==0) {
            v.push_back(str);
            return;
        }
        if(m > 0){ addingpar(v, str+")", n, m-1); }
        if(n > 0){ addingpar(v, str+"(", n-1, m+1); }
    }
};
```

## Print all subsequences
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

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> subs;
        vector<int> sub;
        subsets(nums, 0, sub, subs);
        return subs;
    }
private:
    void subsets(vector<int>& nums, int i, vector<int>& sub, vector<vector<int>>& subs) {
        subs.push_back(sub);
        for (int j = i; j < nums.size(); j++) {
            sub.push_back(nums[j]);
            subsets(nums, j + 1, sub, subs);
            sub.pop_back();
        }
    }
};
```

## Combination Sum II
https://leetcode.com/problems/combination-sum-ii
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = [10,1,2,7,6,1,5], target = 8
**Output:** 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

**Example 2:**

**Input:** candidates = [2,5,2,1,2], target = 5
**Output:** 
[
[1,2,2],
[5]
]

```cpp
class Solution {
public:

vector<int> vec;
vector <vector<int>> ans;
void func(int n, vector<int>& v,int i)
{
    if(n<0) return ;
    if(n==0)
    {
      ans.push_back(vec);
        return ; 
    } 
   for (int j = i; j < v.size(); j++) {
    if (n - v[j] < 0) break;  // check if index j is within bounds of v
    vec.push_back(v[j]);
    func(n - v[j], v, j + 1);
    vec.pop_back();
    while (j + 1 < v.size() && v[j] == v[j + 1]) j++; // skip duplicates
}

}
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
     
     sort(candidates.begin(),candidates.end());
func(target,candidates,0);

       
       
        return ans;
    }
};
```

##  Subset sum II
https://leetcode.com/problems/subsets-ii/
Given an integer array `nums` that may contain duplicates, return _all possible_
_subsets_
_(the power set)_.
The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,2]
**Output:** [[],[1],[1,2],[1,2,2],[2],[2,2]]

**Example 2:**

**Input:** nums = [0]
**Output:** [[],[0]]

```cpp
class Solution {
public:
    std::vector<std::vector<int> > subsetsWithDup(std::vector<int> &nums) {
		std::sort(nums.begin(), nums.end());
        std::vector<std::vector<int> > res;
		std::vector<int> vec;
		subsetsWithDup(res, nums, vec, 0);
		return res;
    }
private:
	void subsetsWithDup(std::vector<std::vector<int> > &res, std::vector<int> &nums, std::vector<int> &vec, int begin) {
		res.push_back(vec);
		for (int i = begin; i != nums.size(); ++i)
			if (i == begin || nums[i] != nums[i - 1]) { 
				vec.push_back(nums[i]);
				subsetsWithDup(res, nums, vec, i + 1);
				vec.pop_back();
			}
	}
};
```

## Combination Sum III
https://leetcode.com/problems/combination-sum-iii/
Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return _a list of all possible valid combinations_. The list must not contain the same combination twice, and the combinations may be returned in any order.

**Example 1:**

**Input:** k = 3, n = 7
**Output:** [[1,2,4]]
**Explanation:**
1 + 2 + 4 = 7
There are no other valid combinations.

**Example 2:**

**Input:** k = 3, n = 9
**Output:** [[1,2,6],[1,3,5],[2,3,4]]
**Explanation:**
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.

**Example 3:**

**Input:** k = 4, n = 1
**Output:** []
**Explanation:** There are no valid combinations.
Using 4 different numbers in the range [1,9], the smallest sum we can get is 1+2+3+4 = 10 and since 10 > 1, there are no valid combination.

```cpp
class Solution {
public:
    vector<vector<int>> ans;
    
    void f(vector<int>& cur, int cnum, int k, int n) {
        if(n < 0 || cur.size() > k) return;
        if(n == 0 && cur.size() == k) {
            ans.push_back(cur);
            return;
        }
        
        for(int i=cnum; i<=9; ++i) {
            cur.push_back(i);
            f(cur, i+1, k, n-i);
            cur.pop_back();
        }
    }
    
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<int> cur;
        f(cur, 1, k, n);
        return ans;
    }
};
```

## Letter Combinations of a Phone Number
https://leetcode.com/problems/letter-combinations-of-a-phone-number/
Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

**Example 1:**

**Input:** digits = "23"
**Output:** ["ad","ae","af","bd","be","bf","cd","ce","cf"]

**Example 2:**

**Input:** digits = ""
**Output:** []

**Example 3:**

**Input:** digits = "2"
**Output:** ["a","b","c"]

```cpp
class Solution {
private:
    void letterCombinations(string digits, vector<string>& output, string &temp, vector<string>& pad, int index){
        if(index == digits.size()){
            output.push_back(temp);
            return;
        }
        string value = pad[digits[index]-'0'];
        for(int i=0; i<value.size(); i++){
            temp.push_back(value[i]);
            letterCombinations(digits, output, temp, pad, index+1);
            temp.pop_back();
        }
    }
public:
    vector<string> letterCombinations(string digits) {
        if(digits.empty()){
            return {};
        }
        vector<string> pad = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> output;
        string temp;
        letterCombinations(digits, output, temp, pad, 0);
        return output;
    }
};
```

# Hard

## N queens
https://leetcode.com/problems/n-queens/
The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4
**Output:** [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above

**Example 2:**

**Input:** n = 1
**Output:** [["Q"]]

```cpp
class Solution {
public:
    vector<vector<string>> ret;
    bool is_valid(vector<string> &board, int row, int col){
        // check col
        for(int i=row;i>=0;--i)
            if(board[i][col] == 'Q') return false;
        // check left diagonal
        for(int i=row,j=col;i>=0&&j>=0;--i,--j)
            if(board[i][j] == 'Q') return false;
        //check right diagonal
        for(int i=row,j=col;i>=0&&j<board.size();--i,++j)
            if(board[i][j] == 'Q') return false;
        return true;
    }
    void dfs(vector<string> &board, int row){
        // exit condition
        if(row == board.size()){
            ret.push_back(board);
            return;
        }
        // iterate every possible position
        for(int i=0;i<board.size();++i){
            if(is_valid(board,row,i)){
                // make decision
                board[row][i] = 'Q';
                // next iteration
                dfs(board,row+1);
                // back-tracking
                board[row][i] = '.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
		// return empty if n <= 0
        if(n <= 0) return {{}};
        vector<string> board(n,string(n,'.'));
        dfs(board,0);
        return ret;
    }
};
```

## Rat in a Maze
https://practice.geeksforgeeks.org/problems/rat-in-a-maze-problem/1
Consider a rat placed at **(0, 0)** in a square matrix of order **N * N**. It has to reach the destination at **(N - 1, N - 1)**. Find all possible paths that the rat can take to reach from source to destination. The directions in which the rat can move are **'U'(up)**, **'D'(down)**, **'L' (left)**, **'R' (right)**. Value 0 at a cell in the matrix represents that it is blocked and rat cannot move to it while value 1 at a cell in the matrix represents that rat can be travel through it.  
**Note**: In a path, no cell can be visited more than one time. If the source cell is 0, the rat cannot move to any other cell.

**Example 1:**

**Input**:
N = 4
m[][] = {{1, 0, 0, 0},
         {1, 1, 0, 1}, 
         {1, 1, 0, 0},
         {0, 1, 1, 1}}
**Output:**
DDRDRR DRDDRR
**Explanation**:
The rat can reach the destination at 
(3, 3) from (0, 0) by two paths - DRDDRR 
and DDRDRR, when printed in sorted order 
we get DDRDRR DRDDRR.

**Example 2:**

**Input**:
N = 2
m[][] = {{1, 0},
         {1, 0}}
**Output:**
-1
**Explanation**:
No path exists and destination cell is 
blocked.
```cpp
// User function template for C++

class Solution{
    typedef int ll;
    #define vi vector<ll>
    #define vp vector< pair <ll,ll> >
    #define f first
    #define s second
    #define st string
    #define pb push_back
    #define endl '\n'
    public:
    bool notValid = false;
    void f(int row, int col, st s,vector<vector<int>> &m, vector<string> &paths, vector<vi> &visited){
        int n = m.size();
        //base case
        if (row == n-1 && col == n-1){
            //reached end
            paths.pb(s);
            return;
        }
        //down
        if (row != n-1){
            if (m[row+1][col] == 1 && !visited[row+1][col]){
                visited[row][col] = 1;
                f(row+1,col,s+'D',m,paths,visited);
                 visited[row][col] = 0;
                 
            }
        }
        //left
        if (col != 0){
            if (m[row][col-1] == 1 && !visited[row][col-1]){
                visited[row][col] = 1;
                f(row,col-1,s+'L',m,paths,visited);
                visited[row][col] = 0;
            }
        }
        //right
        if (col != n-1){
            if (m[row][col+1] == 1 && !visited[row][col+1]){
                visited[row][col] = 1;
                f(row,col+1,s+'R',m,paths,visited);
                visited[row][col] = 0;
            }
        }
        //up
        if (row != 0){
            if (m[row-1][col] == 1 && !visited[row-1][col]){
                visited[row][col] = 1;
                f(row-1,col,s+'U',m,paths,visited);
                visited[row][col] = 0;
            }
        }
    }
    vector<string> findPath(vector<vector<int>> &m, int n) {
       vector<string> paths;
       int row = 0, col = 0;
       vector<vi> visited(n,vi (n,0));
       if (m[0][0] != 1) return paths;
       f(row,col,"",m,paths,visited);
       return paths;
    }
};
```

## Word Break
https://leetcode.com/problems/word-break/
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "leetcode", wordDict = ["leet","code"]
**Output:** true
**Explanation:** Return true because "leetcode" can be segmented as "leet code".

**Example 2:**

**Input:** s = "applepenapple", wordDict = ["apple","pen"]
**Output:** true
**Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
**Output:** false

```cpp
bool tabulation(string s, vector<string> &wordDict){
        vector<int> dp(s.length()+1,0);
        dp[s.length()] = true;

        for(int start = s.length()-1;start>=0;start--){
            bool ans = false;
            string word = "";
            for(int i = start; i<s.length();i++){
                word+=s[i];
                if(check(wordDict,word)){
                    ans = ans || dp[i+1];
                }
            }
            dp[start] =  ans;
        } 
        return dp[0];
    }

    bool wordBreak(string s, vector<string>& wordDict) {
      
    //    return solve(s,wordDict,0);
    //    vector<int> dp(s.length()+1,-1); 
    //    return memoization(s,wordDict,0,dp);
       return tabulation(s,wordDict);
        
    }
```

## M coloring Problem
https://practice.geeksforgeeks.org/problems/m-coloring-problem-1587115620/1#
Given an undirected graph and an integer **M**. The task is to determine if the graph can be colored with at most M colors such that no two adjacent vertices of the graph are colored with the same color. Here coloring of a graph means the assignment of colors to all vertices. Print 1 if it is possible to colour vertices and 0 otherwise.

**Example 1:**

**Input:**
N = 4
M = 3
E = 5
Edges[] = {(0,1),(1,2),(2,3),(3,0),(0,2)}
**Output:** 1
**Explanation:** It is possible to colour the
given graph using 3 colours.

**Example 2:**

**Input:**
N = 3
M = 2
E = 3
Edges[] = {(0,1),(1,2),(0,2)}
**Output:** 0

```cpp
class Solution{
public:
bool isSafe(int node, int colour[], bool graph[101][101], int i, int n)
{
    for(int k=0;k<n;k++)
    {
        if(k!=node && graph[k][node] == 1 && colour[k]==i)
        return false;
    }
    return true;
}
public:
bool solve(int node, bool graph[101][101], int m, int n, int colour[])
{
    if(node == n)
    {
        return true;
    }
    for(int i=1;i<=m;i++)
    {
        if(isSafe(node,colour,graph,i,n))
        {
            colour[node] = i;
            if(solve(node+1,graph,m,n,colour)) return true;
            colour[node] = 0;
        }
    }
    return false;
}
public:
    // Function to determine if graph can be coloured with at most M colours such
    // that no two adjacent vertices of graph are coloured with same colour.
    bool graphColoring(bool graph[101][101], int m, int n) {
        // your code here
        int colour[n] = {0};
        if(solve(0,graph,m,n,colour)) return true;
        return false;
    }
};
```

## Sudoku Solver
https://leetcode.com/problems/sudoku-solver/
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

**Example 1:**

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

**Input:** board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
**Output:** [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
**Explanation:** The input board is shown above and the only valid solution is shown below:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)
```cpp
class Solution {
private:
    bool isValid(vector<vector<char>>& board, int row, int col, char ch){
        for(int i=0; i<9; i++){
            if(board[i][col] == ch){
                return false;
            }
            if(board[row][i] == ch){
                return false;
            }
            if(board[3*(row/3)+i/3][3*(col/3)+i%3] == ch){
                return false;
            }
        }
        return true;
    }
    bool solve(vector<vector<char>>& board) {
        for(int i=0; i<board.size(); i++){
            for(int j=0; j<board[0].size(); j++){
                if(board[i][j] == '.'){
                    for(char ch='1'; ch<='9'; ch++){
                        if(isValid(board, i, j, ch)){
                            board[i][j] = ch;
                            if(solve(board) == true){
                                return true;
                            }
                            board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        solve(board);
    }
};
```

## Expression Add Operators
https://leetcode.com/problems/expression-add-operators/
Given a string `num` that contains only digits and an integer `target`, return _**all possibilities** to insert the binary operators_ `'+'`_,_ `'-'`_, and/or_ `'*'` _between the digits of_ `num` _so that the resultant expression evaluates to the_ `target` _value_.

Note that operands in the returned expressions **should not** contain leading zeros.

**Example 1:**

**Input:** num = "123", target = 6
**Output:** ["1*2*3","1+2+3"]
**Explanation:** Both "1*2*3" and "1+2+3" evaluate to 6.

**Example 2:**

**Input:** num = "232", target = 8
**Output:** ["2*3+2","2+3*2"]
**Explanation:** Both "2*3+2" and "2+3*2" evaluate to 8.

**Example 3:**

**Input:** num = "3456237490", target = 9191
**Output:** []
**Explanation:** There are no expressions that can be created from "3456237490" to evaluate to 9191.

```csharp
class Solution {
private:
    // cur: {string} expression generated so far.
    // pos: {int}    current visiting position of num.
    // cv:  {long}   cumulative value so far.
    // pv:  {long}   previous operand value.
    // op:  {char}   previous operator used.
    void dfs(std::vector<string>& res, const string& num, const int target, string cur, int pos, const long cv, const long pv, const char op) {
        if (pos == num.size() && cv == target) {
            res.push_back(cur);
        } else {
            for (int i=pos+1; i<=num.size(); i++) {
                string t = num.substr(pos, i-pos);
                long now = stol(t);
                if (to_string(now).size() != t.size()) continue;
                dfs(res, num, target, cur+'+'+t, i, cv+now, now, '+');
                dfs(res, num, target, cur+'-'+t, i, cv-now, now, '-');
                dfs(res, num, target, cur+'*'+t, i, (op == '-') ? cv+pv - pv*now : ((op == '+') ? cv-pv + pv*now : pv*now), pv*now, op);
            }
        }
    }

public:
    vector<string> addOperators(string num, int target) {
        vector<string> res;
        if (num.empty()) return res;
        for (int i=1; i<=num.size(); i++) {
            string s = num.substr(0, i);
            long cur = stol(s);
            if (to_string(cur).size() != s.size()) continue;
            dfs(res, num, target, s, i, cur, cur, '#');         // no operator defined.
        }
        return res;
    }
};
```

