leetcode 778
-------------------------
typical question for finding minimum path in a grid 

**Dijkstra (bfs + pq)**</br>
```cpp
class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        int row = grid.size(), col = grid[0].size();
        vector<int> dirs{-1, 0, 1, 0, -1};
        vector<vector<int>> visited(row, vector<int>(col, 0)); // 0-unvisit, 1-visited;
        priority_queue<pair<int, int>> pq; // {time, c + col * r} 
        pq.push({-grid[0][0], 0});
        visited[0][0] = 1;
        while (!pq.empty()) {
            pair<int, int> curr = pq.top(); pq.pop();
            int t = -curr.first;
            int r = curr.second / col; //y
            int c = curr.second % col; //x
            if(r == row - 1 && c == col - 1) return t;
            for(int i = 0; i < 4; i++) {
                int nr = r + dirs[i + 1];
                int nc = c + dirs[i];
                if(nr < 0 || nc < 0 || nr > row - 1 || nc > col - 1) continue;
                if(visited[nr][nc] == 1) continue;
                visited[nr][nc] = 1;
                pq.push({-max(t, grid[nr][nc]), nc + col * nr});
            }
        }
        return 0;
    }
    
    // struct comp {
    //     bool operator() (const pair<int, int>& l, pair<int, int>& r) {
    //         return l.first < r.first;
    //     }
    // };
};
```

*time complexity = <a href="https://www.codecogs.com/eqnedit.php?latex=O(n^{2}log(n^{2}))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?O(n^{2}log(n^{2}))" title="O(n^{2}log(n^{2}))" /></a> </br>
space complexity = <a href="https://www.codecogs.com/eqnedit.php?latex=O(n^{2})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?O(n^{2})" title="O(n^{2})" /></a>*

**binary search + bfs / dfs**
```cpp
class Solution {
public:
    int n;
    vector<vector<int>> grid_;
        
    int swimInWater(vector<vector<int>>& grid) {
        grid_ = grid;
        n = grid.size();
        int L = 0, R = n * n;
        while(L < R) {
            int m = L + (R - L) / 2;
            if(bfs(m)) {
                R = m;
            } else {
                L = m + 1;
            }
        }
        return L;
    }
    
    bool bfs(int t) {
        if(t < grid_[0][0]) return false;
        priority_queue<pair<int, int>> pq; // {-time, c + col * r}
        vector<vector<int>> visited(n, vector<int>(n, 0));
        vector<int> dirs{-1, 0, 1, 0, -1};
        pq.push({-grid_[0][0], 0});
        visited[0][0] = 1;
        while (!pq.empty()) {
            pair<int, int> curr = pq.top(); pq.pop();
            int time = -curr.first;
            int r = curr.second / n;
            int c = curr.second % n;
            if (r == n - 1 && c == n - 1) return time <= t ? true : false;
            for(int i = 0; i < 4; i++) {
                int nr = r + dirs[i];
                int nc = c + dirs[i + 1];
                if(nr < 0 || nc < 0 || nr > n - 1 || nc > n - 1) continue;
                if(visited[nr][nc] == 1) continue;
                visited[nr][nc] = 1;
                pq.push({-max(time, grid_[nr][nc]), nc + n * nr});
            }
        }
        return false;
    }
};
```
*time complexity = <a href="https://www.codecogs.com/eqnedit.php?latex=O(n^{2}log(n^{2}))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?O(n^{2}log(n^{2}))" title="O(n^{2}log(n^{2}))" /></a> </br>
space complexity = <a href="https://www.codecogs.com/eqnedit.php?latex=O(n^{2})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?O(n^{2})" title="O(n^{2})" /></a>*

