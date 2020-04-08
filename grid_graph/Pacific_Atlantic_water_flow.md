leetcode 417
-------------------------
**dfs**</br>
think in a inverse way, starting from P and A, which point could be visited, so don't need to do backtrack</br>
return in 3 conditions: out of boundary, visited status same, child smaller than current
```c++
class Solution {
public:
    int row, col;
    vector<vector<int>> visited;
    vector<vector<int>> res;
    vector<vector<int>> matrix_;
    
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) {
        if(matrix.size() == 0) return res;
        matrix_ = matrix;
        row = matrix.size(), col = matrix[0].size();
        visited.resize(row, vector<int>(col, 0));
        for(int i = 0; i < row; i++) {
            dfs(i, 0, INT_MIN, 1);
            dfs(i, col - 1, INT_MIN, 2);
        }
        
        for(int j = 0; j < col; j++) {
            dfs(0, j, INT_MIN, 1);
            dfs(row - 1, j, INT_MIN, 2);
        }
        return res;
    }
    
    void dfs(int r, int c, int preh, int prev) {
        if(r < 0 || c < 0 || r > row - 1 || c > col - 1 || visited[r][c] == prev || matrix_[r][c] < preh || visited[r][c] == 3) return;
        
        visited[r][c] |= prev;
        if(visited[r][c] == 3) res.push_back({r, c});
        dfs(r + 1, c, matrix_[r][c], visited[r][c]);
        dfs(r - 1, c, matrix_[r][c], visited[r][c]);
        dfs(r, c - 1, matrix_[r][c], visited[r][c]);
        dfs(r, c + 1, matrix_[r][c], visited[r][c]);
        return;
    }
};
```

*time complexity = O(row + col) </br>
space complexity = O(row * col)*

**bfs**
```cpp
class Solution {
public:
    int row, col;
    vector<vector<int>> res;
    vector<vector<int>> matrix_;
    const vector<int> dirs{0, 1, 0, -1, 0};
    
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) {
        if(matrix.size() == 0) return res;
        matrix_ = matrix;
        row = matrix.size(), col = matrix[0].size();
        
        queue<pair<int, int>> qp;
        queue<pair<int, int>> qa;
        vector<vector<int>> vp(row, vector<int>(col, 0));
        vector<vector<int>> va(row, vector<int>(col, 0));
        
        for(int i = 0; i < row; i++) {
            qp.push({i, 0});
            qa.push({i, col - 1});
        }
        for(int j = 0; j < col; j++) {
            qp.push({0, j});
            qa.push({row - 1, j});
        }
        
        bfs(qp, vp);
        bfs(qa, va);
        
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                if(vp[i][j] && va[i][j]) res.push_back({i, j});
            }
        }
        return res;
    }
    
    void bfs(queue<pair<int, int>>& todo, vector<vector<int>>& v) {
        while (!todo.empty()) {
            int r = todo.front().first;
            int c = todo.front().second;
            todo.pop();
            if(v[r][c]) continue;
            v[r][c] = 1;
            int currh = matrix_[r][c];
            
            for(int i = 0; i < 4; i++) {
                int nr = r + dirs[i];
                int nc = c + dirs[i + 1];
                if(nr < 0 || nc < 0 || nr > row - 1 || nc > col - 1 || v[nr][nc] == 1) continue;
                if(matrix_[nr][nc] < currh) continue;
                todo.push({nr, nc});
            }    
        }
        
    }
};
```
*time complexity = O(row * col) </br>
space complexity = O(row * col)*

