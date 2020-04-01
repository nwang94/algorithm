leetcode 329
-------------------------
initial solution (time limit exceed)
```c++
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if(matrix.size() == 0) return 0;
        int res = 0;
        vector<vector<int>> status(matrix.size(), vector<int>(matrix[0].size(), 0)); // 0-unvisited, 1-visiting
        // starting from a random node, traversal all nodes to find increasing path
        for(int i = 0; i < matrix.size(); i++) {
            for(int j = 0; j < matrix[0].size(); j++) {
                res = max(res, dfs(i, j, INT_MIN, 1, matrix, status));
            }
        }
        return res;
    }
    
    int dfs(int row, int col, int prev, int level, vector<vector<int>>& matrix, vector<vector<int>>& status) {
        if(row < 0 || col < 0 || row >= matrix.size() || col >= matrix[0].size() || matrix[row][col] <= prev) {
            return level - 1;
        } else if (status[row][col] == 1) {
            return 0;
        }
        
        status[row][col] = 1;
        int left = dfs(row, col - 1, matrix[row][col], level + 1, matrix, status);
        int right = dfs(row, col + 1, matrix[row][col], level + 1, matrix, status);
        int up = dfs(row - 1, col, matrix[row][col], level + 1, matrix, status);
        int down = dfs(row + 1, col, matrix[row][col], level + 1, matrix, status);
        
        // reset status
        status[row][col] = 0;
        return max(max(left, right), max(up, down));
    }
};
```
