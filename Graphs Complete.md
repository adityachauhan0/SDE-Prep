# Problems On BFS/ DFS
## Number Of Provinces
https://leetcode.com/problems/number-of-provinces/#:~:text=A%20province%20is%20a%20group,the%20total%20number%20of%20provinces.
There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return _the total number of **provinces**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

**Input:** isConnected = [[1,1,0],[1,1,0],[0,0,1]]
**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

**Input:** isConnected = [[1,0,0],[0,1,0],[0,0,1]]
**Output:** 3
```cpp
class Solution {
    vector<bool> vis;
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected[0].size();
        vis = vector<bool> (n);
        int c=0;

        for (int i=0; i<n; i++) {
            if (!vis[i]) {
                c++;
                dfs(isConnected, i);
            }
        }

        return c;
    }

    void dfs(vector<vector<int>>& matrix, int u) {
        int n = matrix[0].size();
        vis[u] = 1;

        for (int v=0; v<n; v++) {
            if (matrix[u][v] && !vis[v]) dfs(matrix, v);
        }
    }
};
```

## Rotten Oranges
https://leetcode.com/problems/rotting-oranges/
You are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

**Input:** grid = [[2,1,1],[1,1,0],[0,1,1]]
**Output:** 4

**Example 2:**

**Input:** grid = [[2,1,1],[0,1,1],[1,0,1]]
**Output:** -1
**Explanation:** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

**Input:** grid = [[0,2]]
**Output:** 0
**Explanation:** Since there are already no fresh oranges at minute 0, the answer is just 0.
```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        // we can only use bfs in this question
        int n = grid.size() ;
        int m = grid[0].size() ;
        vector<vector<int>> visited(n , vector<int> (m , 0)) ;
        queue<pair<pair<int , int> , int>> q ;
         for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
               if(grid[i][j] == 2){
                  q.push({{i , j} , 0 }) ;
                   visited[i][j] = 2 ;
               }
            }
         }
        int maxtime = 0 ;
        int delx[] = {-1,0,1,0} ;
        int dely[] = {0,1,0,-1} ;
          while(!q.empty()){
            int x = q.front().first.first ;
            int y = q.front().first.second ;
            int time = q.front().second ;
            maxtime = max(maxtime , time) ;
            q.pop() ;
            for(int i=0; i<4; i++){
                int row = x+delx[i] ;
                int col = y+dely[i] ;
                if(row>=0 && col>=0 && row<n && col<m && 
                visited[row][col] != 2 && grid[row][col] == 1){
                    visited[row][col] = 2 ;
                    q.push({{row , col} , time+1}) ;
                }
            }
        }  
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(grid[i][j] == 1 && visited[i][j] != 2)return -1 ;
            }
        }
         return maxtime ;
    }
};
```

## Flood Fill
https://leetcode.com/problems/flood-fill/
An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.

You are also given three integers `sr`, `sc`, and `color`. You should perform a **flood fill** on the image starting from the pixel `image[sr][sc]`.

To perform a **flood fill**, consider the starting pixel, plus any pixels connected **4-directionally** to the starting pixel of the same color as the starting pixel, plus any pixels connected **4-directionally** to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with `color`.

Return _the modified image after performing the flood fill_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg)

**Input:** image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2
**Output:** [[2,2,2],[2,2,0],[2,0,1]]
**Explanation:** From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.

**Example 2:**

**Input:** image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0
**Output:** [[0,0,0],[0,0,0]]
**Explanation:** The starting pixel is already colored 0, so no changes are made to the image.
```cpp
class Solution {
public:
    void dfs(int i,int j,int k,int color,vector<vector<int>> &image){
        int n=image.size();
        int m=image[0].size();

        //CHANGE THE ORIGINAL COLOR WITH NEW COLOR
        image[i][j]=color;

        //TRAVERSE AND CHECK FOR ALL POSSIBLE NEIGHBOURS
        if(i-1>=0 && image[i-1][j]==k ) dfs(i-1,j,k,color,image);
        if(i+1<n && image[i+1][j]==k) dfs(i+1,j,k,color,image);
        if(j-1>=0 && image[i][j-1]==k) dfs(i,j-1,k,color,image);
        if(j+1<m && image[i][j+1]==k) dfs(i,j+1,k,color,image);
            
    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        //SAVE THE STARTING OLD COLOR IN A VARIABLE
        int k=image[sr][sc];

       
        if(k==color)  return image;
        
        dfs(sr,sc,k,color,image);
        return image;
        
    }
};
```

## Cycle Detection in Undirected Graph
https://practice.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1?utm_source=youtube&utm_medium=collab_striver_ytdescription&utm_campaign=detect-cycle-in-an-undirected-graph
Given an undirected graph with V vertices labelled from 0 to V-1 and E edges, check whether it contains any cycle or not. Graph is in the form of adjacency list where adj[i] contains all the nodes ith node is having edge with.**
**Example 1:**

**Input:**  
V = 5, E = 5
adj = {{1}, {0, 2, 4}, {1, 3}, {2, 4}, {1, 3}} 
**Output:** 1
**Explanation:** 
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700219/Web/Other/891791f9-1abb-45b1-80f2-7af46d73dcd2_1685086491.png)
1->2->3->4->1 is a cycle.

**Example 2:**

**Input:** 
V = 4, E = 2
adj = {{}, {2}, {1, 3}, {2}}
**Output:** 0
**Explanation:** 
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700219/Web/Other/d8cbd97e-406e-4f50-a38c-6a58747df876_1685086491.png)
No cycle in the graph.

**Your Task:**  
You don't need to read or print anything. Your task is to complete the function **isCycle()** which takes V denoting the number of vertices and adjacency list as input parameters and returns a boolean value denoting if the undirected graph contains any cycle or not, return 1 if a cycle is present else return 0.

### DFS
```cpp
class Solution {
  public:
    // Function to detect cycle in an undirected graph.
  void  dfs(int node ,vector<int>adj[],vector<int>&vis,bool& check,int prev){
        vis[node]=1;
        for (auto it : adj[node]){
            if ( it !=prev &&vis[it]==1){
                check =true;
                return;
            }
            if (vis[it]!=1){dfs(it,adj,vis,check,node);}
        }
        
    }
    bool isCycle(int V, vector<int> adj[]) {
      bool check = false;
      vector<int>vis(V);
      for (int i=0;i<V;i++){
         if (vis[i]!=1){
             dfs(i,adj,vis,check,V+1);
             if (check==true){return 1;}
         }
      }
      return 0;
      
    }
};
```
### BFS
```cpp
class Solution {
  public:
    // Function to detect cycle in an undirected graph.
    bool valid(int src,vector<int> adj[],int vis[]){
        vis[src]=1;
        queue<pair<int,int>> q;
        q.push({src,-1});
        while(!q.empty()){
            int node=q.front().first;
            int par=q.front().second;
            q.pop();
            for(auto i:adj[node]){
                if(!vis[i]){
                    q.push({i,node});
                    vis[i]=1;
                }
                else if(i!=par) return 1;
            }
        }
        return 0;
    }
    bool isCycle(int V, vector<int> adj[]) {
        // Code here
        int vis[V]={0};
        for(int i=0;i<V;i++){
            if(vis[i]==0){
                if(valid(i,adj,vis)) return 1;
            }
        }
        return 0;
    }
};
```

## 0/1 Matrix
https://leetcode.com/problems/01-matrix/
Given an `m x n` binary matrix `mat`, return _the distance of the nearest_ `0` _for each cell_.

The distance between two adjacent cells is `1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/01-1-grid.jpg)

**Input:** mat = [[0,0,0],[0,1,0],[0,0,0]]
**Output:** [[0,0,0],[0,1,0],[0,0,0]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/01-2-grid.jpg)

**Input:** mat = [[0,0,0],[0,1,0],[1,1,1]]
**Output:** [[0,0,0],[0,1,0],[1,2,1]]

```cpp
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        if (mat.empty() || mat[0].empty())
            return {};

        int m = mat.size(), n = mat[0].size();
        queue<pair<int, int>> queue;
        int MAX_VALUE = m * n;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                    queue.push({i, j});
                } else {
                    mat[i][j] = MAX_VALUE;
                }
            }
        }
        
        vector<pair<int, int>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        
        while (!queue.empty()) {
            auto [row, col] = queue.front(); queue.pop();
            for (auto [dr, dc] : directions) {
                int r = row + dr, c = col + dc;
                if (r >= 0 && r < m && c >= 0 && c < n && mat[r][c] > mat[row][col] + 1) {
                    queue.push({r, c});
                    mat[r][c] = mat[row][col] + 1;
                }
            }
        }
        
        return mat;
    }
};
```

## Surrounded Regions
https://leetcode.com/problems/surrounded-regions/
You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: The region is surrounded with `'X'` cells if you can **connect the region** with `'X'` cells and none of the region cells are on the edge of the `board`.

A **surrounded region is captured** by replacing all `'O'`s with `'X'`s in the input matrix `board`.

**Example 1:**

**Input:** board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

**Output:** [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

**Explanation:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

**Example 2:**

**Input:** board = [["X"]]

**Output:** [["X"]]
```cpp
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        int m = board.size();
        int n = board[0].size();

        for (int i = 0; i < m; i++) {
            if (board[i][0] == 'O')
                DFS(board, i, 0, m, n);
            if (board[i][n - 1] == 'O')
                DFS(board, i, n - 1, m, n);
        }

        for (int i = 1; i < n; i++) {
            if (board[0][i] == 'O')
                DFS(board, 0, i, m, n);
            if (board[m - 1][i] == 'O')
                DFS(board, m - 1, i, m, n);
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O')
                    board[i][j] = 'X';
                else if (board[i][j] == '$')
                    board[i][j] = 'O';
            }
        }
    }

private:
    void DFS(vector<vector<char>>& board, int i, int j, int m, int n) {
        if (i >= m || j >= n || i < 0 || j < 0 || board[i][j] != 'O')
            return;
        board[i][j] = '$';
        DFS(board, i + 1, j, m, n);
        DFS(board, i - 1, j, m, n);
        DFS(board, i, j + 1, m, n);
        DFS(board, i, j - 1, m, n);
    }
};
```

## Number Of Enclaves
https://leetcode.com/problems/number-of-enclaves/
You are given an `m x n` binary matrix `grid`, where `0` represents a sea cell and `1` represents a land cell.

A **move** consists of walking from one land cell to another adjacent (**4-directionally**) land cell or walking off the boundary of the `grid`.

Return _the number of land cells in_ `grid` _for which we cannot walk off the boundary of the grid in any number of **moves**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/18/enclaves1.jpg)

**Input:** grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
**Output:** 3
**Explanation:** There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/18/enclaves2.jpg)

**Input:** grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
**Output:** 0
**Explanation:** All 1s are either on the boundary or can reach the boundary.
```cpp
class Solution {
public:
    int numEnclaves(vector<vector<int>>& grid) {
        
        int n=grid.size();
        int m=grid[0].size();

        vector<vector<int>> vis(n, vector<int>(m, 0));

        // row,col
        queue<pair<int, int>> q;

        // traverse in all 4 boundaries
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(i==0 || j==0 || i==n-1 || j==m-1)
                {
                    if(grid[i][j]==1)
                    {
                        vis[i][j]=1;
                        q.push({i, j});
                    }
                }
            }
        }

        int delrow[]={-1, 0, +1, 0};
        int delcol[]={0, +1, 0, -1};

        while(!q.empty())
        {
            int row=q.front().first;
            int col=q.front().second;
            q.pop();

            // 4 directions
            for(int i=0;i<4;i++)
            {
                int nrow=row+delrow[i];
                int ncol=col+delcol[i];

                if(nrow>=0 && nrow<n && ncol>=0 && ncol<m 
                        && !vis[nrow][ncol] && grid[nrow][ncol]==1)
                {
                    vis[nrow][ncol]=1;
                    q.push({nrow, ncol});
                }        
            }
        }

        int cnt=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(!vis[i][j] && grid[i][j]==1)
                {
                    cnt++;
                }
            }
        }

        return cnt;
    }
};
```

## Word Ladder I
https://leetcode.com/problems/word-ladder/
A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _the **number of words** in the **shortest transformation sequence** from_ `beginWord` _to_ `endWord`_, or_ `0` _if no such sequence exists._

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
**Output:** 5
**Explanation:** One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
**Output:** 0
**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end());
        queue<string> todo;
        todo.push(beginWord);
        int ladder = 1;
        while (!todo.empty()) {
            int n = todo.size();
            for (int i = 0; i < n; i++) {
                string word = todo.front();
                todo.pop();
                if (word == endWord) {
                    return ladder;
                }
                dict.erase(word);
                for (int j = 0; j < word.size(); j++) {
                    char c = word[j];
                    for (int k = 0; k < 26; k++) {
                        word[j] = 'a' + k;
                        if (dict.find(word) != dict.end()) {
                            todo.push(word);
                        }
                     }
                    word[j] = c;
                }
            }
            ladder++;
        }
        return 0;
    }
};
```

## Word Ladder II
https://leetcode.com/problems/word-ladder-ii/
A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _all the **shortest transformation sequences** from_ `beginWord` _to_ `endWord`_, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words_ `[beginWord, s1, s2, ..., sk]`.

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
**Output:** [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
**Explanation:** There are 2 shortest transformation sequences:
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
**Output:** []
**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```cpp
class Solution {
public:
    unordered_map<string,int> mp;
    vector<vector<string>> ans;
    string b;

    void dfs(string word,vector<string> &seq){
        if(word == b){
            reverse(seq.begin(),seq.end());
            ans.push_back(seq);
            reverse(seq.begin(),seq.end());
            return;
        }

        int n = word.size();
        int steps = mp[word];

        for(int i = 0; i < n; i++){
            char org = word[i];
            for(char c = 'a'; c <= 'z';c++){
                word[i] = c;
                if(mp.find(word) != mp.end() && mp[word] + 1 == steps){
                    seq.push_back(word);
                    dfs(word,seq);
                    seq.pop_back();
                }
            }
            word[i] = org;
        }
    }

    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> st(wordList.begin(),wordList.end());
        queue<string> q;
        q.push(beginWord);

        st.erase(beginWord);
        mp[beginWord] = 1;
        int n = beginWord.size();
        b = beginWord; 

        while(!q.empty()){
            string word = q.front();
            int steps = mp[word];
            q.pop();

            if(word == endWord) break;

            for(int i = 0; i < n; i++){
                char org = word[i];
                for(char c = 'a'; c <= 'z';c++){
                    word[i] = c;
                    if(st.count(word) > 0){
                        st.erase(word);
                        q.push(word);
                        mp[word] = steps + 1;
                    }
                }
                word[i] = org;
            }
        }

        if(mp.find(endWord) != mp.end()){
            vector<string> seq;
            seq.push_back(endWord);
            dfs(endWord,seq);
        }
        return ans;
    }
};
```

## Number Of Distinct Islands
https://www.geeksforgeeks.org/problems/number-of-distinct-islands/0
Given a boolean 2D matrix **grid** of size **n** * **m**. You have to find the number of distinct islands where a group of connected 1s (horizontally or vertically) forms an island. Two islands are considered to be distinct if and only if one island is not equal to another (not rotated or reflected).

**Example 1:**

**Input:**
grid[][] = {{1, 1, 0, 0, 0},
            {1, 1, 0, 0, 0},
            {0, 0, 0, 1, 1},
            {0, 0, 0, 1, 1}}
**Output:**
1
**Explanation:**
grid[][] = {{1, 1, 0, 0, 0}, 
            {1, 1, 0, 0, 0}, 
            {0, 0, 0, 1, 1}, 
            {0, 0, 0, 1, 1}}
Same colored islands are equal.
We have 2 equal islands, so we 
have only 1 distinct island.

**Example 2:**

**Input:**
grid[][] = {{1, 1, 0, 1, 1},
            {1, 0, 0, 0, 0},
            {0, 0, 0, 0, 1},
            {1, 1, 0, 1, 1}}
**Output:**
3
**Explanation:**
grid[][] = {{1, 1, 0, 1, 1}, 
            {1, 0, 0, 0, 0}, 
            {0, 0, 0, 0, 1}, 
            {1, 1, 0, 1, 1}}
Same colored islands are equal.
We have 4 islands, but 2 of them
are equal, So we have 3 distinct islands.

**Your Task:**

You don't need to read or print anything. Your task is to complete the function **countDistinctIslands()** which takes the **grid** as an input parameter and returns the total number of **distinct** islands.
```cpp
// User function Template for C++

class Solution {
  public:
    void dfs(int row, int col, vector<vector<int>> &visited, 
	vector<vector<int>>& grid, vector<pair<int,int>> &shape, 
	int baserow, int basecol){


	visited[row][col] = 1;
	shape.push_back({row-baserow, col-basecol});
	vector<int> delrow = {-1,0,1,0};
	vector<int> delcol = {0,1,0,-1};
	for (int i = 0; i < 4; i++){
		int x = row + delrow[i];
		int y = col + delcol[i];
		if (x >= 0 && y >= 0 && x < grid.size() && y < grid[0].size()){
			if (!visited[x][y] && grid[x][y]){
				dfs(x,y,visited,grid,shape,baserow,basecol);
			}
		}
	}

}

int countDistinctIslands(vector<vector<int>>& grid) {
        // code here
        int n = grid.size();
        int m = grid[0].size();
        vector<vector<int>> visited(n,vector<int> (m,0));
        set<vector<pair<int,int>>> shapes;
        for (int i = 0; i < n; i++){
        	for (int j = 0; j < m; j++){
        		if (!visited[i][j] && grid[i][j]){
        			vector<pair<int,int>> shape;
        			dfs(i,j,visited,grid,shape, i,j);
        			shapes.insert(shape);
        		}
        	}
        }
        return shapes.size();
    }
};

```

## Bipartite Graph
https://leetcode.com/problems/is-graph-bipartite/
There is an **undirected** graph with `n` nodes, where each node is numbered between `0` and `n - 1`. You are given a 2D array `graph`, where `graph[u]` is an array of nodes that node `u` is adjacent to. More formally, for each `v` in `graph[u]`, there is an undirected edge between node `u` and node `v`. The graph has the following properties:

- There are no self-edges (`graph[u]` does not contain `u`).
- There are no parallel edges (`graph[u]` does not contain duplicate values).
- If `v` is in `graph[u]`, then `u` is in `graph[v]` (the graph is undirected).
- The graph may not be connected, meaning there may be two nodes `u` and `v` such that there is no path between them.

A graph is **bipartite** if the nodes can be partitioned into two independent sets `A` and `B` such that **every** edge in the graph connects a node in set `A` and a node in set `B`.

Return `true` _if and only if it is **bipartite**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg)

**Input:** graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
**Output:** false
**Explanation:** There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg)

**Input:** graph = [[1,3],[0,2],[1,3],[0,2]]
**Output:** true
**Explanation:** We can partition the nodes into two sets: {0, 2} and {1, 3}.
```cpp
bool isBipartite(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> color(n); // 0: uncolored; 1: color A; -1: color B
        
    queue<int> q; // queue, resusable for BFS    
	
    for (int i = 0; i < n; i++) {
      if (color[i]) continue; // skip already colored nodes
      
      // BFS with seed node i to color neighbors with opposite color
      color[i] = 1; // color seed i to be A (doesn't matter A or B) 
      for (q.push(i); !q.empty(); q.pop()) {
        int cur = q.front();
        for (int neighbor : graph[cur]) 
		{
          if (!color[neighbor]) // if uncolored, color with opposite color
          { color[neighbor] = -color[cur]; q.push(neighbor); } 
		  
          else if (color[neighbor] == color[cur]) 
            return false; // if already colored with same color, can't be bipartite!
        }        
      }
    }
    
    return true;
  }
```

## Cycle Detection in Directed Graph
https://leetcode.com/problems/course-schedule-ii
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return _the ordering of courses you should take to finish all courses_. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

**Example 1:**

**Input:** numCourses = 2, prerequisites = [[1,0]]
**Output:** [0,1]
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

**Example 2:**

**Input:** numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
**Output:** [0,2,1,3]
**Explanation:** There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

**Example 3:**

**Input:** numCourses = 1, prerequisites = []
**Output:** [0]
```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        graph g = buildGraph(numCourses, prerequisites);
        vector<int> degrees = computeIndegrees(g);
        vector<int> order;
        for (int i = 0; i < numCourses; i++) {
            int j = 0;
            for (; j < numCourses; j++) {
                if (!degrees[j]) {
                    order.push_back(j);
                    break;
                }
            }
            if (j == numCourses) {
                return {};
            }
            degrees[j]--;
            for (int v : g[j]) {
                degrees[v]--;
            }
        }        
        return order;
    }
private:
    typedef vector<vector<int>> graph;
    
    graph buildGraph(int numCourses, vector<pair<int, int>>& prerequisites) {
        graph g(numCourses);
        for (auto p : prerequisites) {
            g[p.second].push_back(p.first);
        }
        return g;
    }
    
    vector<int> computeIndegrees(graph& g) {
        vector<int> degrees(g.size(), 0);
        for (auto adj : g) {
            for (int v : adj) {
                degrees[v]++;
            }
        }
        return degrees;
    }
};
```

# Topo Sort and Problems

## TopoSort
https://practice.geeksforgeeks.org/problems/topological-sort/1
Given an adjacency list for a Directed Acyclic Graph (DAG) where **adj_list[i]** contains a list of all vertices j such that there is a directed edge from vertex i to vertex j, with  **V**  vertices and **E**  edges, your task is to find any valid topological sorting of the graph.

In a topological sort, for every directed edge u -> v,  u must come before v in the ordering.

**Example 1:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700255/Web/Other/24aa5d54-bc1f-489c-bd2d-ad02ddccdf31_1684492511.png)
**Output:**
1
**Explanation**:
The output 1 denotes that the order is
valid. So, if you have, implemented
your function correctly, then output
would be 1 for all test cases.
One possible Topological order for the
graph is 3, 2, 1, 0.

**Example 2:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700255/Web/Other/c35bb1d1-130c-49aa-a17e-118181d7bdcd_1684492512.png)
**Output:**
1
**Explanation:**
The output 1 denotes that the order is
valid. So, if you have, implemented
your function correctly, then output
would be 1 for all test cases.
One possible Topological order for the
graph is 5, 4, 2, 1, 3, 0.

**Your Task:**  
You don't need to read input or print anything. Your task is to complete the function **topoSort()**  which takes the integer V denoting the number of vertices and adjacency list as input parameters and returns an array consisting of the vertices in Topological order. As there are multiple Topological orders possible, you may return any of them. If your returned topo sort is correct then the console output will be 1 else 0.

```cpp
class Solution
{
	public:
	
	void dfs(int u,vector<int> adj[],vector<int> &vis,stack<int> &topo)
	{
	    vis[u]=1;
	    for( auto v: adj[u])
	    {
	        if(!vis[v])
	        {
	            dfs(v,adj,vis,topo);
	        }
	    }
	    topo.push(u);
	}
	vector<int> topoSort(int V, vector<int> adj[]) 
	{
	    stack<int> topo;
	    vector<int> ans;
	    vector<int> vis(V,0);
	    
	    for(int i=0;i<V;i++)
	    {
	        if(!vis[i])
	        dfs(i,adj,vis,topo);
	    }
	    while(!topo.empty())
	    {
	        ans.push_back(topo.top());
	        topo.pop();
	    }
	    return ans;
	}
};
```

## Kahn's Algorithm
https://bit.ly/3c690mm
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
	//Function to return list containing vertices in Topological order.
	vector<int> topoSort(int V, vector<int> adj[])
	{
		int indegree[V] = {0};
		for (int i = 0; i < V; i++) {
			for (auto it : adj[i]) {
				indegree[it]++;
			}
		}

		queue<int> q;
		for (int i = 0; i < V; i++) {
			if (indegree[i] == 0) {
				q.push(i);
			}
		}
		vector<int> topo;
		while (!q.empty()) {
			int node = q.front();
			q.pop();
			topo.push_back(node);
			// node is in your topo sort
			// so please remove it from the indegree

			for (auto it : adj[node]) {
				indegree[it]--;
				if (indegree[it] == 0) q.push(it);
			}
		}

		return topo;
	}
};
```

## Cycle Detection in Directed Graph
https://practice.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1
Given a Directed Graph with **V** vertices (Numbered from **0** to **V-1**) and **E** edges, check whether it contains any cycle or not.
**Example 1:**
**Input:**

![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700218/Web/Other/9a013355-2510-4ab0-b554-1a2b9f6cb44f_1685086462.png)

**Output:** 1
**Explanation**: 3 -> 3 is a cycle

  
**Example 2:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700218/Web/Other/b1096e14-7c18-47d8-a4e9-8dd42b2e466f_1685086462.png)

**Output:** 0
**Explanation**: no cycle in the graph

  
**Your task:**  
You dont need to read input or print anything. Your task is to complete the function **isCyclic()** which takes the integer V denoting the number of vertices and adjacency list **adj** as input parameters and returns a boolean value denoting if the given directed graph contains a cycle or not.  
In the adjacency list **adj,** element **adj[i][j]** represents an edge from **i** to **j.**

```cpp
class Solution {
  public:
    
    bool isCyclic(int V, vector<int> adj[]) {
        int indegree[V] = {0};
		for (int i = 0; i < V; i++) {
			for (auto it : adj[i]) {
				indegree[it]++;
			}
		}

		queue<int> q;
		for (int i = 0; i < V; i++) {
			if (indegree[i] == 0) {
				q.push(i);
			}
		}
		vector<int> topo;
		while (!q.empty()) {
			int node = q.front();
			q.pop();
			topo.push_back(node);
			// node is in your topo sort
			// so please remove it from the indegree

			for (auto it : adj[node]) {
				indegree[it]--;
				if (indegree[it] == 0) q.push(it);
			}
		}

        if(topo.size()==V) return false;
        return true;
    }
};
```

## Course Schedule I
https://leetcode.com/problems/course-schedule/
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

**Input:** numCourses = 2, prerequisites = [[1,0]]
**Output:** true
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.

**Example 2:**

**Input:** numCourses = 2, prerequisites = [[1,0],[0,1]]
**Output:** false
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        graph g = buildGraph(numCourses, prerequisites);
        vector<int> degrees = computeIndegrees(g);
        for (int i = 0; i < numCourses; i++) {
            int j = 0;
            for (; j < numCourses; j++) {
                if (!degrees[j]) {
                    break;
                }
            }
            if (j == numCourses) {
                return false;
            }
            degrees[j]--;
            for (int v : g[j]) {
                degrees[v]--;
            }
        }
        return true;
    }
private:
    typedef vector<vector<int>> graph;
    
    graph buildGraph(int numCourses, vector<pair<int, int>>& prerequisites) {
        graph g(numCourses);
        for (auto p : prerequisites) {
            g[p.second].push_back(p.first);
        }
        return g;
    }
    
    vector<int> computeIndegrees(graph& g) {
        vector<int> degrees(g.size(), 0);
        for (auto adj : g) {
            for (int v : adj) {
                degrees[v]++;
            }
        }
        return degrees;
    }
};
```

## Find Eventual Safe States
https://leetcode.com/problems/find-eventual-safe-states/
There is a directed graph of `n` nodes with each node labeled from `0` to `n - 1`. The graph is represented by a **0-indexed** 2D integer array `graph` where `graph[i]` is an integer array of nodes adjacent to node `i`, meaning there is an edge from node `i` to each node in `graph[i]`.

A node is a **terminal node** if there are no outgoing edges. A node is a **safe node** if every possible path starting from that node leads to a **terminal node** (or another safe node).

Return _an array containing all the **safe nodes** of the graph_. The answer should be sorted in **ascending** order.

**Example 1:**

![Illustration of graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

**Input:** graph = [[1,2],[2,3],[5],[0],[5],[],[]]
**Output:** [2,4,5,6]
**Explanation:** The given graph is shown above.
Nodes 5 and 6 are terminal nodes as there are no outgoing edges from either of them.
Every path starting at nodes 2, 4, 5, and 6 all lead to either node 5 or 6.

**Example 2:**

**Input:** graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
**Output:** [4]
**Explanation:**
Only node 4 is a terminal node, and every path starting at node 4 leads to node 4.

**Constraints:**

- `n == graph.length`
- `1 <= n <= 104`
- `0 <= graph[i].length <= n`
- `0 <= graph[i][j] <= n - 1`
- `graph[i]` is sorted in a strictly increasing order.
- The graph may contain self-loops.
- The number of edges in the graph will be in the range `[1, 4 * 104]`.
```cpp
vector<int> eventualSafeNodes(vector<vector<int>>& G) {
    int N = G.size();
    vector<vector<int>> R(N);
    vector<int> outdegree(N), safe(N), ans;
    queue<int> q;
    for (int i = 0; i < N; ++i) {
        for (int v : G[i]) {
            R[v].push_back(i);
        }
        outdegree[i] = G[i].size();
        if (outdegree[i] == 0) q.push(i);
    }
    while (q.size()) {
        int u = q.front();
        q.pop();
        safe[u] = 1;
        for (int v : R[u]) {
            if (--outdegree[v] == 0) q.push(v);
        }
    }
    for (int i = 0; i < N; ++i) {
        if (safe[i]) ans.push_back(i);
    }
    return ans;
}
```

## Alien Dictionary
https://www.geeksforgeeks.org/problems/alien-dictionary/1
Given a sorted dictionary of an alien language having N words and k starting alphabets of standard dictionary. Find the order of characters in the alien language.  
**Note:** Many orders may be possible for a particular test case, thus you may return any valid order and output will be 1 if the order of string returned by the function is correct else 0 denoting incorrect string returned.  
 

**Example 1:**

**Input:** 
N = 5, K = 4
dict = {"baa","abcd","abca","cab","cad"}
**Output:**
1
**Explanation:**
Here order of characters is 
'b', 'd', 'a', 'c' Note that words are sorted 
and in the given language "baa" comes before 
"abcd", therefore 'b' is before 'a' in output.
Similarly we can find other orders.

**Example 2:**

**Input:** 
N = 3, K = 3
dict = {"caa","aaa","aab"}
**Output:**
1
**Explanation:**
Here order of characters is
'c', 'a', 'b' Note that words are sorted
and in the given language "caa" comes before
"aaa", therefore 'c' is before 'a' in output.
Similarly we can find other orders.
```cpp
// User function Template for C++

class Solution{
    public:
    string findOrder(string dict[], int N, int K) {
        unordered_map<char, unordered_set<char>> g;
        unordered_map<char, int> indegree;
        for (int i = 0; i < N; ++i) {
            string word = dict[i];
            for (char c : word) {
                indegree[c] = 0;
            }
        }
        
        for (int i=0; i<N-1; i++) {
            auto w1 = dict[i];
            auto w2 = dict[i+1];
            for (int j=0; j<min(w1.size(), w2.size()); j++) {
                if (w1[j] != w2[j]) {
                    if (g[w1[j]].find(w2[j]) == g[w1[j]].end()) {
                        g[w1[j]].insert(w2[j]);
                        indegree[w2[j]]++;
                    }
                    break;
                }
            }
        }
        
        queue<char> q;
        string s = "";
        
        // q.push(dict[0][0]);
        for (auto &c: indegree) {
            if (! c.second) {
                q.push(c.first);
            }
        }
        while (! q.empty()) {
            auto c = q.front();
            s += c;
            q.pop();
            for (auto v: g[c]) {
                if (!--indegree[v]) {
                    q.push(v);
                }
            }
        }
        if (s.size() != K)  return "";
        return s;
    }
};
```

# Shortest Path Algorithms

## Shortest Path in UG with unit weights
https://bit.ly/3UVQD4C
You are given an **Undirected Graph** having **unit weight** of the edges, find the shortest path from **src** to all the vertex and if it is **unreachable** to reach any vertex, then return **-1** for that vertex.

**Example1:**

**Input:**
n = 9, m= 10
edges=[[0,1],[0,3],[3,4],[4,5],[5,6],[1,2],[2,6],[6,7],[7,8],[6,8]] 
src=0
**Output:**
0 1 2 1 2 3 3 4 4  
**Explanation:**![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/711976/Web/Other/blobid1_1712813311.png)

**Example2:**

**Input:**
n = 4, m= 4
edges=[[0,0],[1,1],[1,3],[3,0]] 
src=3
**Output:**
1 1 -1 0  
**Explanation:**![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/711976/Web/Other/blobid3_1712814761.png)

**Your Task:  
**You don't need to print or input anything. Complete the function **shortest path()** which takes a 2d vector or array of **edges** representing the edges of an undirected graph with unit weight, an integer **n** as the number of nodes, an integer **m** as a number of edges and an integer **src** as the input parameters and returns an integer array or vector, denoting **the vector of distance from src to all nodes.**

```cpp
// User function Template for C++
class Solution {
  public:
    vector<int> shortestPath(vector<vector<int>>& edges, int N,int M, int src){
        vector<int>adj[N];
        for(int i = 0; i < M;i++)
        {
            adj[edges[i][0]].push_back(edges[i][1]);
            adj[edges[i][1]].push_back(edges[i][0]);
        }
        queue<int>q;
        vector<int>dist(N);
        for(int i = 0; i < N ;i++)
        {
            dist[i] = 1e9;
        }
        
        dist[src] = 0;
        
        q.push(src);
        
        while(!q.empty())
        {
            int node = q.front();
            q.pop();
            
            for(auto it : adj[node])
            {
                if(dist[node]+1 < dist[it])
                {
                    dist[it] = dist[node]+1;
                    q.push(it);
                }
            }
            
        }
        
        
        for(int i = 0;i < N;i++)
        {
            if(dist[i]==1e9)dist[i]=-1;
        }
        
        return dist;
        
    }
};
```

## Shortest Path in DAG
https://bit.ly/3Eo1mhq
Given a Directed Acyclic Graph of N vertices from 0 to N-1 and a 2D Integer array(or vector) edges[ ][ ] of length M, where there is a directed edge from edge[i][0] to edge[i][1] with a distance of edge[i][2] for all i.

Find the **shortest** path from **src(0)** vertex to all the vertices and if it is impossible to reach any vertex, then return **-1** for that vertex.

**Example1:**

**Input:**
N = 4, M = 2
edge = [[0,1,2],[0,2,1]]
**Output:**
0 2 1 -1  
**Explanation:  
**Shortest path from 0 to 1 is 0->1 with edge weight 2. Shortest path from 0 to 2 is 0->2 with edge weight 1.  
There is no way we can reach 3, so it's -1 for 3.

**Example2:**

**Input:**
N = 6, M = 7
edge = [[0,1,2],[0,4,1],[4,5,4],[4,2,2],[1,2,3],[2,3,6],[5,3,1]]
**Output:**
0 2 3 6 1 5  
**Explanation:  
**Shortest path from 0 to 1 is 0->1 with edge weight 2. Shortest path from 0 to 2 is 0->4->2 with edge weight 1+2=3.  
Shortest path from 0 to 3 is 0->4->5->3 with edge weight 1+4+1=6.  
Shortest path from 0 to 4 is 0->4 with edge weight 1.  
Shortest path from 0 to 5 is 0->4->5 with edge weight 1+4=5.

**Your Task:**

You don't need to print or input anything. Complete the function **shortest path()** which takes an integer N as number of vertices, an integer M as number of edges and a 2D Integer array(or vector) edges as the input parameters and returns an **integer array(or vector)**, denoting the list of distance from src to all nodes.

```cpp
// User function Template for C++
class Solution {
  public:
     vector<int> shortestPath(int n,int m, vector<vector<int>>& edges){
        // code here
        int src=0;
        vector<int>ans(n,1e9);
        ans[src]=0;
        queue<int>q;
        vector<pair<int,int> >adj[n];
        int vis[n]={0};
        for(int i=0;i<m;i++){
            adj[edges[i][0]].push_back({edges[i][1],edges[i][2]});
            //  adj[edges[i][1]].push_back(edges[i][0]);
            
        }
        // int step=0;
        q.push(src);
        // vis[src]=1;
        while(!q.empty()){
            int f=q.front();
            // int step=q.front().second;
            // ans[f]=step;
            q.pop();
            for(auto it:adj[f]){
                if(ans[f]+it.second<ans[it.first]){
                    ans[it.first]=ans[f]+it.second;
                    q.push(it.first);
                    
                }
            }
            
        }
        for(int i=0;i<n;i++){
            if(ans[i]==1e9){
                ans[i]=-1;
            }
        }
        return ans;
    }
};
```

## Dijkstra Algorithm
https://practice.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1
Given a weighted, undirected and connected graph of **V** vertices and an adjacency list adj where adj[i] is a list of lists containing two integers where the **first** integer of each list **j** denotes there is **edge** between i and j , second integers corresponds to the **weight** of that  edge . You are given the source vertex **S** and You to Find the shortest distance of all the vertex's from the source vertex **S**. You have to return a list of integers denoting shortest distance between **each node** and Source vertex **S**.  
 

**Note:** The Graph doesn't contain any negative weight cycle.

**Example 1:**

**Input:**
**V** = 2
**adj []** = {{{1, 9}}, {{0, 9}}}
**S** = 0
**Output:**
0 9
**Explanation**:
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700334/Web/Other/6a77963c-f9a6-4cf4-953c-19a2759a52a3_1685086564.png)
The source vertex is 0. Hence, the shortest 
distance of node 0 is 0 and the shortest 
distance from node 1 is 9.

**Example 2:**

**Input:
V** = 3, **E** = 3
**adj** = {{{1, 1}, {2, 6}}, {{2, 3}, {0, 1}}, {{1, 3}, {0, 6}}}
**S** = 2
**Output:**
4 3 0
**Explanation**:
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700334/Web/Other/8c9ee3a2-a7d3-4028-ae22-a22ddb6ab7a3_1685086565.png)
For nodes 2 to 0, we can follow the path-
2-1-0. This has a distance of 1+3 = 4,
whereas the path 2-0 has a distance of 6. So,
the Shortest path from 2 to 0 is 4.
The shortest distance from 0 to 1 is 1 .

**Your Task:**  
You don't need to read input or print anything. Your task is to complete the function **dijkstra()**  which takes the number of vertices V and an adjacency list adj as input parameters and Source vertex S returns a list of integers, where ith integer denotes the shortest distance of the ith node from the Source node. Here adj[i] contains a list of lists containing two integers where the first integer j denotes that there is an edge between i and j and the second integer w denotes that the weight between edge i and j is w.

```cpp
class Solution
{
	public:
	//Function to find the shortest distance of all the vertices
    //from the source vertex S.
    vector <int> dijkstra(int V, vector<vector<int>> adj[], int S)
    {
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>pq;
        vector<int>dis(V,INT_MAX);
        dis[S]=0;
        pq.push({0,S});
        // for (int )
        vector<int>vis(V,0);
        while(!pq.empty()){
            int top_distance=pq.top().first;
            int node=pq.top().second;
             pq.pop();
            if (top_distance>dis[node]){
                continue;
            }
            dis[node]=top_distance;
           
            vis[node]=1;
            for (auto ele:adj[node]){
                if (!vis[ele[0]]){
                    pq.push({top_distance+ele[1],ele[0]});
                }
            }
        }
        return dis;
    }
};
```

## Shortest Path in a Binary Maze
https://leetcode.com/problems/shortest-path-in-binary-matrix/
Given an `n x n` binary matrix `grid`, return _the length of the shortest **clear path** in the matrix_. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:

- All the visited cells of the path are `0`.
- All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

**Input:** grid = [[0,1],[1,0]]
**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

**Input:** grid = [[0,0,0],[1,1,0],[1,1,0]]
**Output:** 4

**Example 3:**

**Input:** grid = [[1,0,0],[1,1,0],[1,1,0]]
**Output:** -1
```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        // edge case: start or end not accessible
        if (grid[0][0] || grid.back().back()) return -1;
        // support variables
        int res = 2, len = 1, maxX = grid[0].size() - 1, maxY = grid.size() - 1;
        queue<pair<int, int>> q;
        // edge case: single cell matrix
        if (!maxX && !maxY) return 1 - (grid[0][0] << 1);
        // adding the starting point
        q.push({0, 0});
        // marking start as visited
        grid[0][0] = -1;
        while (len) {
            while (len--) {
                // reading and popping the coordinates on the front of the queue
                auto [cx, cy] = q.front();
                q.pop();
                for (int x = max(0, cx - 1), lmtX = min(cx + 1, maxX); x <= lmtX; x++) {
                    for (int y = max(0, cy - 1), lmtY = min(cy + 1, maxY); y <= lmtY; y++) {
                        // check if we reached the target
                        if (x == maxX && y == maxY) return res;
                        // marking it as visited and adding it to the q if it was still a valid cell
                        if (!grid[y][x]) {
                            grid[y][x] = -1;
                            q.push({x, y});
                        }
                    }
                }
            }
            // preparing for the next loop
            res++;
            len = q.size();
        }
        return -1;
    }
};
```

## Path with Minimum Effort
https://leetcode.com/problems/path-with-minimum-effort/
You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., **0-indexed**). You can move **up**, **down**, **left**, or **right**, and you wish to find a route that requires the minimum **effort**.

A route's **effort** is the **maximum absolute difference** in heights between two consecutive cells of the route.

Return _the minimum **effort** required to travel from the top-left cell to the bottom-right cell._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

**Input:** heights = [[1,2,2],[3,8,2],[5,3,5]]
**Output:** 2
**Explanation:** The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex2.png)

**Input:** heights = [[1,2,3],[3,8,4],[5,3,5]]
**Output:** 1
**Explanation:** The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex3.png)

**Input:** heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
**Output:** 0
**Explanation:** This route does not require any effort.

```cpp
#define pii pair<int, pair<int,int>>

class Solution {
public:
    //Directions (top, right, bottom, left)
    const int d4x[4] = {-1,0,1,0}, d4y[4] = {0,1,0,-1};
    
    int minimumEffortPath(vector<vector<int>>& h) {
        int n = h.size(), m = h[0].size();
        //min-heap
        priority_queue <pii, vector<pii>, greater<pii>> pq;
        //to store distances from (0,0)
        vector<vector<int>> dis(n, vector<int>(m, INT_MAX));
        dis[0][0] = 0;
        pq.push({0, {0, 0}});
        
        //Dijstra algorithm
        while(!pq.empty()) {
            pii curr = pq.top(); pq.pop();
            int d = curr.first, r = curr.second.first, c = curr.second.second;
            // bottom right position
            if(r==n-1 && c==m-1) return d;
            for(int i=0; i<4; ++i) {
                int nx = r + d4x[i], ny = c + d4y[i];
                //check if new position is invalid
                if(nx < 0 || nx >= n || ny < 0 || ny >= m)continue;
				//nd => new distance: which is max of distance till now(d) and curr distance (difference between heights of current cells)
                int nd = max(d, abs(h[nx][ny] - h[r][c]));
                if (nd < dis[nx][ny]) {
                    dis[nx][ny] = nd;
                    pq.push({nd, {nx, ny}});
                }
            }
        }
        return 0;
    }
};
```

## Cheapest Flight within K stops
https://leetcode.com/problems/cheapest-flights-within-k-stops/
There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return _**the cheapest price** from_ `src` _to_ `dst` _with at most_ `k` _stops._ If there is no such route, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-3drawio.png)

**Input:** n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
**Output:** 700
**Explanation:**
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-1drawio.png)

**Input:** n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
**Output:** 200
**Explanation:**
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 2 is marked in red and has cost 100 + 100 = 200.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-2drawio.png)

**Input:** n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
**Output:** 500
**Explanation:**
The graph is shown above.
The optimal path with no stops from city 0 to 2 is marked in red and has cost 500.

```cpp
  int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        // create adjacency list
        unordered_map<int,vector<pair<int, int>>> adjList;
        for( auto f : flights )
            adjList[f[0]].push_back( { f[1], f[2] } );
        
        // minHeap based on cost of distance from source
        priority_queue< vector<int>, vector<vector<int>>, greater<vector<int>> > minHeap;
        minHeap.push( { 0, src, K+1 } ); // cost, vertex, hops
        
        while( !minHeap.empty() ) {
            auto t = minHeap.top(); minHeap.pop();
            int cost = t[0];
            int curr = t[1];
            int stop = t[2];
            if( curr == dst )
                return cost;

            if( stop > 0 )
                for( auto next : adjList[curr] )
                    minHeap.push( { cost+next.second, next.first, stop-1 } );
        }
        return -1;
    }
```

```cpp
 /*  In bellman-ford algo calculates the shortest distance from the source
        point to all of the vertices.
        Time complexity of Bellman-Ford is O(VE),
    */
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        /* distance from source to all other nodes */
        vector<int> dist( n, INT_MAX );
        dist[src] = 0;
        
        // Run only K+1 times since we want shortest distance in K hops
        for( int i=0; i <= K; i++ ) {
            vector<int> tmp( dist );
            for( auto flight : flights ) {
                if( dist[ flight[0] ] != INT_MAX ) {
                    tmp[ flight[1] ] = min( tmp[flight[1]], dist[ flight[0] ] + flight[2] );
                }
            }
            dist = tmp;
        }
        return dist[dst] == INT_MAX ? -1 : dist[dst];
    }
```

## Network Delay Time
https://leetcode.com/problems/network-delay-time/
You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return _the **minimum** time it takes for all the_ `n` _nodes to receive the signal_. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

**Input:** times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
**Output:** 2

**Example 2:**

**Input:** times = [[1,2,1]], n = 2, k = 1
**Output:** 1

**Example 3:**

**Input:** times = [[1,2,1]], n = 2, k = 2
**Output:** -1

```cpp
   int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<pair<int,int>> g[N+1];
        for(int i=0;i<times.size();i++)
            g[times[i][0]].push_back(make_pair(times[i][1],times[i][2]));
        vector<int> dist(N+1, 1e9);
        dist[K] = 0;
        priority_queue<pair<int,int>, vector<pair<int,int>> , greater<pair<int,int>>> q;
        q.push(make_pair(0,K));
        pair<int,int> temp;
        bool visit[N+1];
        memset(visit, false, sizeof(visit));
        while(!q.empty()){
            temp = q.top();
            q.pop();
            int u = temp.second;
            visit[u] = true;
            for(int i=0;i<g[u].size();i++){
                int v = g[u][i].first;
                int weight = g[u][i].second;
                if(visit[v]==false && dist[v] > dist[u] + weight){
                    dist[v] = dist[u] + weight;
                    q.push(make_pair(dist[v], v));
                }
            }
        }
        int ans = 0;
        for(int i=1;i<dist.size();i++){
            ans = max(ans, dist[i]);
        }
        if(ans==1e9) return -1;
        return ans;
    }
```

## Number Of Ways to Arrive at Destination
https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/
You are in a city that consists of `n` intersections numbered from `0` to `n - 1` with **bi-directional** roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

You are given an integer `n` and a 2D integer array `roads` where `roads[i] = [ui, vi, timei]` means that there is a road between intersections `ui` and `vi` that takes `timei` minutes to travel. You want to know in how many ways you can travel from intersection `0` to intersection `n - 1` in the **shortest amount of time**.

Return _the **number of ways** you can arrive at your destination in the **shortest amount of time**_. Since the answer may be large, return it **modulo** `109 + 7`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/17/graph2.png)

**Input:** n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
**Output:** 4
**Explanation:** The shortest amount of time it takes to go from intersection 0 to intersection 6 is 7 minutes.
The four ways to get there in 7 minutes are:
- 0 ➝ 6
- 0 ➝ 4 ➝ 6
- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

**Example 2:**

**Input:** n = 2, roads = [[1,0,10]]
**Output:** 1
**Explanation:** There is only one way to go from intersection 0 to intersection 1, and it takes 10 minutes.
```cpp
#define ll long long
#define pll pair<ll, ll>
class Solution {
public:
    int MOD = 1e9 + 7;
    int countPaths(int n, vector<vector<int>>& roads) {
        vector<vector<pll>> graph(n);
        for(auto& road: roads) {
            ll u = road[0], v = road[1], time = road[2];
            graph[u].push_back({v, time});
            graph[v].push_back({u, time});
        }
        return dijkstra(graph, n, 0);
    }
    int dijkstra(const vector<vector<pll>>& graph, int n, int src) {
        vector<ll> dist(n, LONG_MAX);
        vector<ll> ways(n);
        ways[src] = 1;
        dist[src] = 0;
        priority_queue<pll, vector<pll>, greater<>> minHeap;
        minHeap.push({0, 0}); // dist, src
        while (!minHeap.empty()) {
            auto[d, u] = minHeap.top(); minHeap.pop();
            if (d > dist[u]) continue; // Skip if `d` is not updated to latest version!
            for(auto [v, time] : graph[u]) {
                if (dist[v] > d + time) {
                    dist[v] = d + time;
                    ways[v] = ways[u];
                    minHeap.push({dist[v], v});
                } else if (dist[v] == d + time) {
                    ways[v] = (ways[v] + ways[u]) % MOD;
                }
            }
        }
        return ways[n-1];
    }
};
```

## Minimum steps to reach end from start by performing multiplication and mod operations with array elements
https://bit.ly/3QAEsrY
Given **start**, **end** and an array **arr** of **n** numbers. At each step, **start** is multiplied with any number in the array and then mod operation with **100000** is done to get the new start.

Your task is to find the minimum steps in which **end** can be achieved starting from **start**. If it is not possible to reach **end**, then return **-1**.

**Example 1:**

**Input:**
arr[] = {2, 5, 7}
start = 3, end = 30
**Output:**
2
**Explanation:**
Step 1: 3*2 = 6 % 100000 = 6 
Step 2: 6*5 = 30 % 100000 = 30

**Example 2:**

**Input:**
arr[] = {3, 4, 65}
start = 7, end = 66175
**Output:**
4
**Explanation:**
Step 1: 7*3 = 21 % 100000 = 21 
Step 2: 21*3 = 63 % 100000 = 63 
Step 3: 63*65 = 4095 % 100000 = 4095 
Step 4: 4095*65 = 266175 % 100000 = 66175

**Your Task:  
**You don't need to print or input anything. Complete the function **minimumMultiplications()** which takes an integer array **arr**, an integer **start** and an integer **end** as the input parameters and returns an integer, denoting the minumum steps to reach in which **end** can be achieved starting from **start**.

```cpp
class Solution {
  public:
    int minimumMultiplications(vector<int>& arr, int start, int end) {
        // code here
        if(start==end){
            return 0;
        }
        vector<int> dist(100000,1e9);
        queue<pair<int,int>> q;
        dist[start] =0;
        int mod = 100000;
        
        q.push({0,start});
        while(!q.empty()){
            int step = q.front().first;
            int node = q.front().second;
            q.pop();
            
            for(int i=0;i<arr.size();i++){
                int num = (arr[i]*node)%mod;
                if(step+1 <dist[num]){
                    dist[num] = step+1;
                    
                    if(num == end){
                        return step+1;
                    }
                    q.push({step+1, num});
                }
            }
        }
        
        return -1;
        
    }
};
```

## Bellman Ford Algorithm
https://practice.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1
Given a weighted and directed graph of V vertices and E edges, Find the shortest distance of all the vertex's from the source vertex S. If a vertices can't be reach from the S then mark the distance as 10^8. Note: If the Graph contains a negative cycle then return an array consisting of only -1.

**Example 1:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/706218/Web/Other/c8d8b64c-f87e-4b44-ad81-5069e9698985_1685087173.png)
**E** = [[0,1,9]]
**S** = 0
**Output:**
0 9
**Explanation**:
Shortest distance of all nodes from
source is printed.

**Example 2:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/706218/Web/Other/46db67e8-b4da-46d6-a9ab-604249bea60a_1685087173.png)
**E** = [[0,1,5],[1,0,3],[1,2,-1],[2,0,1]]
**S** = 2
**Output:**
1 6 0
**Explanation**:
For nodes 2 to 0, we can follow the path-
2-0. This has a distance of 1.
For nodes 2 to 1, we cam follow the path-
2-0-1, which has a distance of 1+5 = 6,

**Your Task:**  
You don't need to read input or print anything. Your task is to complete the function **bellman_ford( )** which takes a number of vertices **V** and an **E**-sized list of lists of three integers where the three integers are **u,v**, and **w**; denoting there's an edge from **u to v**, which has a weight of **w** and source node **S** as input parameters and returns a list of integers where the ith integer denotes the distance of an ith node from the source node.

If some node isn't possible to visit, then its distance should be 100000000(1e8). Also, If the Graph contains a negative cycle then return an array consisting of a single -1.
```cpp
// User function Template for C++

class Solution {
  public:
    /*  Function to implement Bellman Ford
    *   edges: vector of vectors which represents the graph
    *   S: source vertex to start traversing graph with
    *   V: number of vertices
    */
    vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
        // Code here
        vector<int>dis(V,1e8);
        dis[S]=0;
        for (int i=0;i<V-1;i++){
            for (int j=0;j<edges.size();j++){
                if (dis[edges[j][0]]==1e8){
                    continue;
                }
            else if ((dis[edges[j][0]]+edges[j][2]<dis[edges[j][1]])){
                    dis[edges[j][1]]=edges[j][2]+dis[edges[j][0]];
                }
            }
        }
        vector<int>flag;
        for (int j=0;j<edges.size();j++){
            if (dis[edges[j][0]]==1e8){
                continue;
            }
            if (dis[edges[j][0]]+edges[j][2]<dis[edges[j][1]]){
                flag.push_back(-1);
                return flag;
            }
        }
        return dis;
    }
};
```

## Floyd Warshall 
https://practice.geeksforgeeks.org/problems/implementing-floyd-warshall2042/1
The problem is to find the shortest distances between every pair of vertices in a given **edge-weighted directed** graph. The graph is represented as an adjacency matrix of size **n*n**. **Matrix[i][j]** denotes the weight of the edge from **i to j.** If **Matrix[i][j]=-1,** it means there is no edge from **i to j.**  
**Note : Modify** the distances **for every pair in-place.**

**Examples :**

**Input:** matrix = [[0, 25],[-1, 0]]
![](https://media.geeksforgeeks.org/wp-content/uploads/20221106202714/WhatsAppImage20221106at82359PM.jpeg)
**Output:** [[0, 25],[-1, 0]]
![](https://media.geeksforgeeks.org/wp-content/uploads/20221106202714/WhatsAppImage20221106at82359PM.jpeg)
**Explanation:** The shortest distance between every pair is already given(if it exists).

**Input:** matrix = [[0, 1, 43],[1, 0, 6],[-1, -1, 0]]
![](https://media.geeksforgeeks.org/wp-content/uploads/20221106203741/WhatsAppImage20221106at83711PM.jpeg)
**Output: [[**0, 1, 7],[1, 0, 6],[-1, -1, 0]]
![](https://media.geeksforgeeks.org/wp-content/uploads/20221106204057/WhatsAppImage20221106at84031PM.jpeg)
**Explanation:** We can reach 2 from 0 as 0->1->2 and the cost will be 1+6=7 which is less than 43.

```cpp
class Solution {
  public:
  
    void shortest_distance(vector<vector<int>>&matrix)
    {
	    int rows = matrix.size();
	    if(rows <= 1) return;
	    int cols = matrix[0].size();

        for(int v=0; v<cols; v++)
	        for(int j=0; j<cols; j++)
	            for(int i=0; i<rows; i++)
			    	if(matrix[i][v] != -1 && matrix[v][j] != -1 && (matrix[i][j] == -1 || matrix[i][j] > matrix[i][v] + matrix[v][j]))
				    	matrix[i][j] = matrix[i][v] + matrix[v][j];

        // cycle detection	    	
		for(int i=0; i<rows; i++)
		    if(matrix[i][i] < 0)
		        std::cout << "Cycle exists";
    }
};
```

## Find the city with the smallest number of neighbors in a threshold distance
https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/
There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is **at most** `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities _**i**_ and _**j**_ is equal to the sum of the edges' weights along that path.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_01.png)

**Input:** n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
**Output:** 3
**Explanation:** The figure above describes the graph. 
The neighboring cities at a distanceThreshold = 4 for each city are:
City 0 -> [City 1, City 2] 
City 1 -> [City 0, City 2, City 3] 
City 2 -> [City 0, City 1, City 3] 
City 3 -> [City 1, City 2] 
Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4, but we have to return city 3 since it has the greatest number.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_02.png)

**Input:** n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
**Output:** 0
**Explanation:** The figure above describes the graph. 
The neighboring cities at a distanceThreshold = 2 for each city are:
City 0 -> [City 1] 
City 1 -> [City 0, City 4] 
City 2 -> [City 3, City 4] 
City 3 -> [City 2, City 4]
City 4 -> [City 1, City 2, City 3] 
The city 0 has 1 neighboring city at a distanceThreshold = 2.

```cpp
int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<vector<int>> dis(n, vector(n, 10001));
        int res = 0, smallest = n;
        for (auto& e : edges)
            dis[e[0]][e[1]] = dis[e[1]][e[0]] = e[2];
        for (int i = 0; i < n; ++i)
            dis[i][i] = 0;
        for (int k = 0; k < n; ++k)
            for (int i = 0; i < n; ++i)
                for (int j = 0; j < n; ++j)
                    dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
        for (int i = 0; i < n; i++) {
            int count = 0;
            for (int j = 0; j < n; ++j)
                if (dis[i][j] <= distanceThreshold)
                    ++count;
            if (count <= smallest) {
                res = i;
                smallest = count;
            }
        }
        return res;
    }
```

# MST / Disjoint Set

## Minimum Spanning Tree
https://practice.geeksforgeeks.org/problems/minimum-spanning-tree/1
Given a weighted, undirected, and connected graph with V vertices and E edges, your task is to find the sum of the weights of the edges in the Minimum Spanning Tree (MST) of the graph. The graph is represented by an adjacency list, where each element adj[i] is a vector containing pairs of integers. Each pair represents an edge, with the first integer denoting the endpoint of the edge and the second integer denoting the weight of the edge.

**Example 1:**

**Input:**
3 3
0 1 5
1 2 3
0 2 1
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700343/Web/Other/064ccfb5-e351-4908-a660-b228a091eb47_1685086606.png)
**Output:**
4
**Explanation**:
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700343/Web/Other/64f692e2-1acf-4515-8f46-516521cf0bab_1685086607.png)
The Spanning Tree resulting in a weight
of 4 is shown above.

**Example 2:**

**Input:**
2 1
0 1 5
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700343/Web/Other/944e4620-f860-4e62-aa2a-086f31e142cb_1685086607.png)
**Output:**
5
**Explanation**:
Only one Spanning Tree is possible
which has a weight of 5.

**Your task:**  
Since this is a functional problem you don't have to worry about input, you just have to complete the function spanningTree() which takes a number of vertices V and an adjacency list adj as input parameters and returns an integer denoting the sum of weights of the edges of the Minimum Spanning Tree. Here adj[i] contains vectors of size 2, where the first integer in that vector denotes the end of the edge and the second integer denotes the edge weight.
```cpp
class Solution
{
public:
	//Function to find sum of weights of edges of the Minimum Spanning Tree.
	int spanningTree(int V, vector<vector<int>> adj[])
	{
		priority_queue<pair<int, int>,
		               vector<pair<int, int> >, greater<pair<int, int>>> pq;

		vector<int> vis(V, 0);
		// {wt, node}
		pq.push({0, 0});
		int sum = 0;
		while (!pq.empty()) {
			auto it = pq.top();
			pq.pop();
			int node = it.second;
			int wt = it.first;

			if (vis[node] == 1) continue;
			// add it to the mst
			vis[node] = 1;
			sum += wt;
			for (auto it : adj[node]) {
				int adjNode = it[0];
				int edW = it[1];
				if (!vis[adjNode]) {
					pq.push({edW, adjNode});
				}
			}
		}
		return sum;
	}
};
```

## Disjoint Set 
```cpp
#include <bits/stdc++.h>
using namespace std;
class DisjointSet {
    vector<int> rank, parent, size;
public:
    DisjointSet(int n) {
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        size.resize(n + 1);
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    int findUPar(int node) {
        if (node == parent[node])
            return node;
        return parent[node] = findUPar(parent[node]);
    }

    void unionByRank(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (rank[ulp_u] < rank[ulp_v]) {
            parent[ulp_u] = ulp_v;
        }
        else if (rank[ulp_v] < rank[ulp_u]) {
            parent[ulp_v] = ulp_u;
        }
        else {
            parent[ulp_v] = ulp_u;
            rank[ulp_u]++;
        }
    }

    void unionBySize(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        }
        else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
    }
};
int main() {
    DisjointSet ds(7);
    ds.unionBySize(1, 2);
    ds.unionBySize(2, 3);
    ds.unionBySize(4, 5);
    ds.unionBySize(6, 7);
    ds.unionBySize(5, 6);
    // if 3 and 7 same or not
    if (ds.findUPar(3) == ds.findUPar(7)) {
        cout << "Same\n";
    }
    else cout << "Not same\n";

    ds.unionBySize(3, 7);

    if (ds.findUPar(3) == ds.findUPar(7)) {
        cout << "Same\n";
    }
    else cout << "Not same\n";
    return 0;
}
```

## Kruskal's Algorithm
```cpp
class Solution
{
public:
    //Function to find sum of weights of edges of the Minimum Spanning Tree.
    int spanningTree(int V, vector<vector<int>> adj[])
    {
        // 1 - 2 wt = 5
        /// 1 - > (2, 5)
        // 2 -> (1, 5)

        // 5, 1, 2
        // 5, 2, 1
        vector<pair<int, pair<int, int>>> edges;
        for (int i = 0; i < V; i++) {
            for (auto it : adj[i]) {
                int adjNode = it[0];
                int wt = it[1];
                int node = i;

                edges.push_back({wt, {node, adjNode}});
            }
        }
        DisjointSet ds(V);
        sort(edges.begin(), edges.end());
        int mstWt = 0;
        for (auto it : edges) {
            int wt = it.first;
            int u = it.second.first;
            int v = it.second.second;

            if (ds.findUPar(u) != ds.findUPar(v)) {
                mstWt += wt;
                ds.unionBySize(u, v);
            }
        }

        return mstWt;
    }
};
```

## Number Of Operations Needed To Make a Network Connected
https://leetcode.com/problems/number-of-operations-to-make-network-connected/
There are `n` computers numbered from `0` to `n - 1` connected by ethernet cables `connections` forming a network where `connections[i] = [ai, bi]` represents a connection between computers `ai` and `bi`. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network `connections`. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return _the minimum number of times you need to do this in order to make all the computers connected_. If it is not possible, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png)

**Input:** n = 4, connections = [[0,1],[0,2],[1,2]]
**Output:** 1
**Explanation:** Remove cable between computer 1 and 2 and place between computers 1 and 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png)

**Input:** n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
**Output:** 2

**Example 3:**

**Input:** n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
**Output:** -1
**Explanation:** There are not enough cables.

```cpp
class Solution {
    void dfs(vector<int> adj[], vector<bool> &visited, int src)
    {
        visited[src] = true;
        for(int i : adj[src]){
            if(!visited[i]){
                dfs(adj, visited, i);
            }
        }
    }
public:
    int makeConnected(int n, vector<vector<int>>& arr) {
        int len = arr.size();
        if(len<n-1) return -1;
         vector<int> adj[n];
        for(auto v : arr)
        {
            adj[v[0]].push_back(v[1]);
            adj[v[1]].push_back(v[0]);
        }
        vector<bool> visited(n, false);
        int ans = 0;
        for(int i=0; i<n; i++)
        if(!visited[i])
        {
            dfs(adj, visited, i);
            ans++;
        }
        return ans - 1;
    }
};
```
Union FInd
```cpp
class Solution {
public:
    int find(int x, int parent[]){
        while(parent[x]!=x){
            x = parent[parent[x]];
        }
        return x;
    }

    void makeUnion(int x, int y, int parent[], int rank[]){
        int parX = find(x, parent);
        int parY = find(y, parent);
        if(parX == parY){
            return;
        }
        else if(rank[parX]<rank[parY]){
            parent[parX] = parY;
        }
        else if(rank[parX]>rank[parY]){
            parent[parY] = parX;
        }
        else{
            parent[parY] = parX;
            rank[parX]++;
        }
    }

    int makeConnected(int n, vector<vector<int>>& connections) {
        int edges = connections.size();
        if(edges<n-1){
            return -1;
        }
        int parent[n];
        int rank[n];
        for(int i=0; i<n; i++){
            parent[i] = i;
        }
        for(auto con : connections){
            makeUnion(con[0], con[1], parent, rank);
        }
        set<int> groups;
        for(int i=0; i<n; i++){
            int par = find(i, parent);
            groups.insert(par);
        }
        int unions = groups.size();
        return unions-1;
    }
```

## Most stones removed with same Rows or Columns
https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/
On a 2D plane, we place `n` stones at some integer coordinate points. Each coordinate point may have at most one stone.

A stone can be removed if it shares either **the same row or the same column** as another stone that has not been removed.

Given an array `stones` of length `n` where `stones[i] = [xi, yi]` represents the location of the `ith` stone, return _the largest possible number of stones that can be removed_.

**Example 1:**

**Input:** stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
**Output:** 5
**Explanation:** One way to remove 5 stones is as follows:
1. Remove stone [2,2] because it shares the same row as [2,1].
2. Remove stone [2,1] because it shares the same column as [0,1].
3. Remove stone [1,2] because it shares the same row as [1,0].
4. Remove stone [1,0] because it shares the same column as [0,0].
5. Remove stone [0,1] because it shares the same row as [0,0].
Stone [0,0] cannot be removed since it does not share a row/column with another stone still on the plane.

**Example 2:**

**Input:** stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
**Output:** 3
**Explanation:** One way to make 3 moves is as follows:
1. Remove stone [2,2] because it shares the same row as [2,0].
2. Remove stone [2,0] because it shares the same column as [0,0].
3. Remove stone [0,2] because it shares the same row as [0,0].
Stones [0,0] and [1,1] cannot be removed since they do not share a row/column with another stone still on the plane.

**Example 3:**

**Input:** stones = [[0,0]]
**Output:** 0
**Explanation:** [0,0] is the only stone on the plane, so you cannot remove it.
```cpp
class Solution {
public:
    int dfs(vector<vector<int>>&stones,int index,vector<bool>&visited,int&n){
        visited[index]=true;
        int result=0;
        for(int i=0;i<n;i++)
            if(!visited[i]&&(stones[i][0]==stones[index][0]||stones[i][1]==stones[index][1]))
                result +=(dfs(stones,i,visited,n) + 1);
        return result;
    }
    int removeStones(vector<vector<int>>&stones) {
        int n = stones.size();
        vector<bool>visited(n,0);
        int result=0;
        for(int i=0;i<n;i++){
            if(visited[i]){continue;}
            result+=dfs(stones,i,visited,n);
        }
        return result;
    }
};
```

## Accounts Merge
https://leetcode.com/problems/accounts-merge/
Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

**Example 1:**

**Input:** accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
**Output:** [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
**Explanation:**
The first and second John's are the same person as they have the common email "johnsmith@mail.com".
The third John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.

**Example 2:**

**Input:** accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
**Output:** [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]

```cpp
class Solution {
public:
    // standard union find methods
    
    int find(int node,vector<int>&parent) {
        while(parent[node] != node) {
            node = parent[node];
        }
        
        return node;
    }
    
    void unify(int i, int j,vector<int>&parent) {
        int iRoot = find(i,parent);
        int jRoot = find(j,parent);
        
        if(iRoot != jRoot) {
            parent[jRoot] = iRoot;
        }
        
    }
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        int n=accounts.size();
        vector<int>parent(n);
        for(int i=0;i<n;i++)
            parent[i]=i;
        map<string,int>mp; // map which will store the string and their IDs
        
        //iterate through all the strings and put it int the map if it is not seen till now else merge that ID with the already seen ID.
        for(int i=0;i<n;i++)
        {
           for(int j=1;j<accounts[i].size();j++)
           {
               if(mp.find(accounts[i][j])==mp.end())
               {
                   mp[accounts[i][j]]=i;
               }
               else
               {
                   unify(mp[accounts[i][j]],i,parent);
               }
           }
        }
        
       // finding the total number of unique users or total classes that we have in total, after merging
      int cnt=0;
        for(int i=0;i<n;i++)
        {
               
            if(parent[i]==i)
                cnt++;
        }
        
          vector<vector<string>>ans(cnt,vector<string>());
        
        // now putting them into our final answer by using a map which will maintain the IDs
        int newidx=0;
        map<int,int>mp1;
        for(auto &e:mp)  // iterate through this map as it contains all unique strings only
        {
            string mail=e.first;
            int index=e.second;
            int grouphead=find(index,parent); // find class or grouphead of that index
            if(mp1.find(grouphead)==mp1.end())
            {
              
                mp1[grouphead]=newidx;
                ans[newidx].push_back(accounts[grouphead][0]);  // putting the name at first index
                ans[newidx].push_back(mail);
                newidx++;
            }
            else
            {
                int putindex=mp1[grouphead];
                ans[putindex].push_back(mail);
            }
            
        }
        // sorting the emails
        for(int i=0;i<ans.size();i++)
        {
            sort(ans[i].begin()+1,ans[i].end());
        }
      return ans;
    }
};
```

## Number of Islands II
https://leetcode.com/problems/number-of-islands-ii/
## Problem statement

You have a 2D grid of ‘N’ rows and ‘M’ columns which are initially filled with water. You are given ‘Q’ queries each consisting of two integers ‘X’ and ‘Y’ and in each query operation, you have to turn the water at position (‘X’, ‘Y’) into a land. You are supposed to find the number of islands in the grid after each query.

An island is a group of lands surrounded by water horizontally, vertically, or diagonally.

```cpp
class DSU{
	private:
		vector<int>parent, size;
		int numComponents ;
	public:
		DSU(int n){
			parent.resize(n+1);
			size.resize(n+1,1);
			for(int i = 0;i<=n;i++)parent[i] = i;
		}

		int findParent(int node){
			if(parent[node] == node)return parent[node];
			return parent[node] = findParent(parent[node]);
		}

		void unionSize(int u, int v){
			int u_p = findParent(u);
			int v_p = findParent(v);
			if(u_p == v_p)return ;
			if(size[u_p] < size[v_p]){
				parent[u_p] = v_p;
				size[v_p] += size[u_p];
			}else{
				parent[v_p] = u_p;
				size[u_p] += size[v_p];
			}
		}

};
vector<int> numOfIslandsII(int n, int m, vector<vector<int>> &q){
	// Write your code here.
	int total = n*m;
	int sz = q.size();
	DSU dsu(total);
	vector<vector<int>>grid(n,vector<int>(m,0));
	vector<int>ans(sz,sz);
	int count = 0;
	for(int i = 0;i<q.size();i++){
		int x = q[i][0], y = q[i][1];
		if(grid[x][y]==1){
			ans[i] = count;
			continue;
		}
		grid[x][y] = 1;
		count++;
		int ind = x*m + y;
		int dx[5] = {0,1,0,-1,0};
		for(int j = 0; j<4;j++){
			int nx = x + dx[j], ny = y + dx[j+1];
			if(nx<0 or nx>=n or ny<0 or ny>=m)continue;
			int newNode = nx*m + ny;
			if(grid[nx][ny] and dsu.findParent(ind)!=dsu.findParent(newNode)){
				dsu.unionSize(ind, newNode);
				count--;
			}
		}
		
		ans[i] = count;
	}
	return ans;
}
```

## Making a Large Island
https://leetcode.com/problems/making-a-large-island/
You are given an `n x n` binary matrix `grid`. You are allowed to change **at most one** `0` to be `1`.

Return _the size of the largest **island** in_ `grid` _after applying this operation_.

An **island** is a 4-directionally connected group of `1`s.

**Example 1:**

**Input:** grid = [[1,0],[0,1]]
**Output:** 3
**Explanation:** Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

**Example 2:**

**Input:** grid = [[1,1],[1,0]]
**Output:** 4
**Explanation:** Change the 0 to 1 and make the island bigger, only one island with area = 4.

**Example 3:**

**Input:** grid = [[1,1],[1,1]]
**Output:** 4
**Explanation:** Can't change any 0 to 1, only one island with area = 4.

```cpp
class DSU
{
    vector<int>rank , parent , size;
    public:
    DSU(int n)
    {
        size.resize(n+1 , 1);
        rank.resize(n+1 , 0);
        parent.resize(n+1);
        for(int i = 0;i<=n;i++)
        parent[i] = i;
    }
    int findpar(int node)
    {
        if(node == parent[node])return node;

        return parent[node] = findpar(parent[node]);
    }
    void unionbyrank(int u , int v)
    {
        int p_u = findpar(u);
        int p_v = findpar(v);
        if(p_u == p_v) return ;
        if(rank[p_u] > rank[p_v])
        {
            parent[p_v] = p_u;
        }
        else if(rank[p_v] > rank[p_u])
        {
            parent[p_u] = p_v;
        }
        else 
        {
            parent[p_v] = p_u;
            rank[p_u]++;
        }
    }
    void unionbysize(int u , int v)
    {
        int p_u = findpar(u);
        int p_v = findpar(v);
        if(p_u == p_v) return ;
        if(size[p_u] > size[p_v])
        {
            size[p_u] += size[p_v];
            parent[p_v] = p_u;
        }
        else 
        {
            size[p_v] += size[p_u];
            parent[p_u] = p_v;
        }
    }
};
class Solution {
public:
    int largestIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        DSU dsu(n*n);
        int dc[] = {1 , 0 , 0 , -1};
        int dr[] = {0 , -1 , 1 , 0};
        for(int i = 0;i<n;i++)
        {
            for(int j = 0;j<n;j++)
            {
                int node = i*n + j;
                if(grid[i][j] == 1)
                {
                    for(int k = 0;k<4;k++)
                    {
                        int next_i = i + dr[k];
                        int next_j = j + dc[k];
                        if(next_i < 0 || next_j < 0 || next_i >= n || next_j >= n) continue;
                        if(grid[next_i][next_j] == 0) continue;
                        int next_node = next_i*n + next_j;
                        if(dsu.findpar(node) == dsu.findpar(next_node))
                            continue;
                        dsu.unionbyrank(node , next_node);
                    }
                }
            }
        }
        int ans = 0 , maxi = 0;
        unordered_map<int , int>mp;
        for(int i = 0;i<n*n;i++)
        {
            mp[dsu.findpar(i)]++;
        }
        for(int i = 0;i<n;i++)
        {
            for(int j = 0;j<n;j++)
            {
                int count = 1;
                unordered_set<int>s;
                if(grid[i][j] == 0)
                {
                    for(int k = 0;k<4;k++)
                    {
                        int next_i = i + dr[k];
                        int next_j = j + dc[k];
                        if(next_i < 0|| next_j < 0|| next_i >= n|| next_j >= n) continue;
                        if(grid[next_i][next_j] == 0) continue;
                        int next_node = next_i*n + next_j;
                        if(s.find(dsu.findpar(next_node)) != s.end()) continue;
                        s.insert(dsu.findpar(next_node));
                        count += mp[dsu.findpar(next_node)];
                        
                    }
                }
                ans = max(ans , count);
            }
        }
        for(auto it:mp)
            ans = max(ans , it.second);
        return ans;
    }
};
```

## Swim in rising water
https://leetcode.com/problems/swim-in-rising-water/
You are given an `n x n` integer matrix `grid` where each value `grid[i][j]` represents the elevation at that point `(i, j)`.

The rain starts to fall. At time `t`, the depth of the water everywhere is `t`. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most `t`. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return _the least time until you can reach the bottom right square_ `(n - 1, n - 1)` _if you start at the top left square_ `(0, 0)`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg)

**Input:** grid = [[0,2],[1,3]]
**Output:** 3
Explanation:
At time 0, you are in grid location (0, 0).
You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
You cannot reach point (1, 1) until time 3.
When the depth of water is 3, we can swim anywhere inside the grid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/29/swim2-grid-1.jpg)

**Input:** grid = [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
**Output:** 16
**Explanation:** The final route is shown.
We need to wait until time 16 so that (0, 0) and (4, 4) are connected.

```cpp
class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>>time(n, vector<int>(m, 1e9));
        priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int,int>>>,
        greater<pair<int,pair<int,int>>>> pq;
        time[0][0] = grid[0][0];
        int ans = 1e9;
     
        pq.push({time[0][0], {0,0}});
        
        int dr[] = {-1, 0, 1, 0};
        int dc[] = {0, 1, 0, -1};
        while(!pq.empty())
        {
            auto it = pq.top();
            pq.pop();
            int diff = it.first;
            int r = it.second.first;
            int c = it.second.second;
           
            
            for(int i = 0; i<4; i++)
            {
                int nrow = dr[i] + r;
                int ncol = dc[i] + c;
                
                if(nrow>=0 && nrow<n && ncol >=0 && ncol <m && grid[nrow][ncol] < time[nrow][ncol])
                {
                    if(diff <  grid[nrow][ncol])
                    {
                        time[nrow][ncol] = grid[nrow][ncol];
                        pq.push({time[nrow][ncol], {nrow, ncol}});
                         
                    }
                    else if(diff < time[nrow][ncol])
                    {
                        time[nrow][ncol] = diff;
                        pq.push({time[nrow][ncol], {nrow, ncol}});
                    }  
                }
            }
            
        }
        return time[n-1][m-1];
    }
};
```

# Other Algorithms

## Bridges in Graph
https://leetcode.com/problems/critical-connections-in-a-network/
There are `n` servers numbered from `0` to `n - 1` connected by undirected server-to-server `connections` forming a network where `connections[i] = [ai, bi]` represents a connection between servers `ai` and `bi`. Any server can reach other servers directly or indirectly through the network.

A _critical connection_ is a connection that, if removed, will make some servers unable to reach some other server.

Return all critical connections in the network in any order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png)

**Input:** n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
**Output:** [[1,3]]
**Explanation:** [[3,1]] is also accepted.

**Example 2:**

**Input:** n = 2, connections = [[0,1]]
**Output:** [[0,1]]
```cpp
class Solution {
public:
    //use find the bridges in a graph approach to get all critical connections
	//Tarjans Algorithm / DFS
    void dfs(int node, int parent, vector<int> &tin, vector<int> &low, vector<int> &vis, vector<int> graph[], vector<vector<int>> &ans, int &time)
    {
        //when we visite node first time, tin and low of the node are equal to the time
        tin[node]=low[node]=time++;
        vis[node] = 1; //mark node as visited
        
        for(auto it : graph[node])
        {
            if(it == parent) continue; //to avoid backtracking
            
            //if node is not visited, call the dfs function
            if(!vis[it])
            {
                dfs(it, node, tin, low, vis, graph, ans, time);
                
                //when we return, low of the node is equal to the minimum of low of its child and low of itself
                low[node] = min(low[node], low[it]);
                
                //when we get low of child is greater than tin of the node
                //it means there is only path to cover child that's why {node, it} will be our critical connections or bridge 
                if(low[it] > tin[node])
                    ans.push_back({node, it});
            }
            
            //if node is already visited and tin of the child is less than the low of itself
            //we assign tin of the child to the low of the node
            else
                low[node] = min(low[node], tin[it]);
            
        }
    }
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) 
    {
        //tin - time of insertion at the node
        //low - lowest time of insertion at the node
        //vis - mark visited node 
        vector<int> tin(n, -1), low(n, -1), vis(n, 0);
        vector<vector<int>> ans;
        vector<int> graph[n]; //stores connections in the form of graph
        int time=0;
        
        for(auto it: connections)
        {
            graph[it[0]].push_back(it[1]);
            graph[it[1]].push_back(it[0]);
        }
        
        //call dfs function
        dfs(0, -1, tin, low, vis, graph, ans, time);
        return ans;
    }
};
```

## Articulation Point
Given an undirected connected graph with **V** vertices and adjacency list **adj**. You are required to find all the vertices removing which (and edges through it) disconnects the graph into 2 or more components and return it in sorted manner.  
**Note:** Indexing is zero-based i.e nodes numbering from (0 to V-1). There might be loops present in the graph.

**Example 1:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/708502/Web/Other/a27f9040-9783-4386-92f9-b8684c75db07_1685087852.png)
**Output:**{1,4}
**Explanation:** Removing the vertex 1 will
discconect the graph as-
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/708502/Web/Other/7e12629a-ba31-411e-b6ac-ccf5a8baa6a3_1685087852.png)
Removing the vertex 4 will disconnect the
graph as-
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/708502/Web/Other/fb781bda-91d6-4920-96a8-c976412c3ada_1685087852.png)

**Your Task:**  
You don't need to read or print anything. Your task is to complete the function **articulationPoints****()** which takes V and adj as input parameters and returns a list containing all the vertices removing which turn the graph into two or more disconnected components in sorted order. If there are no such vertices then returns a list containing -1.

```cpp
int timer=1;

class Solution {
  public:
  
  void dfs(int node,int parent,vector<int>&vis,vector<int>adj[],vector<int>&ans,vector<int>&tim,vector<int>&low){ 
      vis[node]=1;
      
      low[node]=timer;
      tim[node]=timer;
      timer++;
      
      int child=0;
      
      for(auto nbr:adj[node]){
          
          if(nbr==parent)continue;
          
          
          
          if(!vis[nbr]){
              child++;
              dfs(nbr,node,vis,adj,ans,tim,low);
              
          low[node]=min(low[node],low[nbr]);
          
          if(low[nbr]>=tim[node] && parent!=-1){
              ans[node]=1;
              
          }
              
              
          }
          
          
          else{
              low[node]=min(low[node],tim[nbr]);
          }
      }
      
      
      if(child>1 && parent==-1)
          ans[0]=1;
      
      
      
  }
  

    vector<int> articulationPoints(int V, vector<int>adj[]) {
        vector<int>vis(V,false);
        
        vector<int>ans(V,0);
        
        
        vector<int>tim(V);
        vector<int>low(V);
        
    
        dfs(0,-1,vis,adj,ans,low,tim);
        
        vector<int>result;
        
        for(int i=0;i<V;i++){
            
            if(ans[i])
            result.push_back(i);
            
        }
        
        if(result.size()==0){
            result.push_back(-1);
        }
        
    
    
    return result;
    
    
    }
};
```

## Kosaraju's Algorithm
https://bit.ly/3TbvByL
Given a Directed Graph with **V** vertices **(**Numbered from **0 to V-1)** and **E** edges, Find the number of strongly connected components in the graph.  
 

**Example 1:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700394/Web/Other/89b7c4e7-e03c-402f-b445-3e8815299af6_1685086635.png)
**Output:**
3
**Explanation**:
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700394/Web/Other/9f4ccc7f-8ad8-4f81-908a-01f27090ba5e_1685086635.png)
We can clearly see that there are 3 Strongly
Connected Components in the Graph

**Example 2:**

**Input:**
![](https://media.geeksforgeeks.org/img-practice/PROD/addEditProblem/700394/Web/Other/8b9b3908-a800-4ffa-acaf-26cb760eac8e_1685086635.png)
**Output:**
1
**Explanation**:
All of the nodes are connected to each other.
So, there's only one SCC.
```cpp
class Solution
{
	public:
	 void dfs(int i,vector<int>&vis,stack<int>&st,vector<vector<int>>&adj){
	     vis[i]=true;
	     for (auto ele:adj[i]){
	         if (!vis[ele]){
	             dfs(ele,vis,st,adj);
	         }
	     }
	     st.push(i);
	 }
	 void reverse(vector<vector<int>>&adj,int V,vector<vector<int>>&adj1){
	     for (int i=0;i<V;i++){
	         for (auto ele:adj[i]){
	             adj1[ele].push_back(i);
	         }
	     }
	 }
	//Function to find number of strongly connected components in the graph.
    int kosaraju(int V, vector<vector<int>>& adj)
    {
        //code here
        stack<int>st;
        vector<int>after_vis(V,0);
        vector<int>vis(V,0);
        for (int i=0;i<V;i++){
            if (vis[i]==0){
            dfs(i,vis,st,adj);
            }
        }
        
        int no_comp=0;
        vector<vector<int>>adj1(V);
        stack<int>st1;
        reverse(adj,V,adj1);
        while(!st.empty()){
           int top=st.top(); 
           st.pop();
           if (after_vis[top]==0){
           dfs(top,after_vis,st1,adj1);
           no_comp++;
           }
        }
        return no_comp;
    }
};
```