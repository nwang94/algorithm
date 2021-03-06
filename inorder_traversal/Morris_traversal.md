leetcode 99
---------------------
### recursion ###
code structure
```cpp
void dfs(TreeNode* root) {
  if(!root) return;
  dfs(root->left);
  // visit curr node
  pred = root; // while needs to maintain a prev node
  dfs(root->right);
}
```
time complexity = O(1) best, O(n) worst<br/>
space complexity = O(H) // H = height of tree
### morris traversal ###
- one step left to root->left, find the right most node of root->left
- set the node find in previous step as predecessor of root, construct a link between predecessor and root
- go to most left node, visit the current node
- backtrack to root, delete link created in step 2 when visit them again.
- go to root->right

code structure
```cpp
while(root) {
  if(root->left) {
    // go to the most right
    TreeNode* temp = root->left;
    while(temp->right && temp->right != root) {
      temp = temp->right;
    }
    if(!temp->right) {
      // fist time get predecessor of root, build link
      temp->right = root;
      root = root->left;
    } else {
      // second time see predecessor of root, delete link
      temp->right = nullptr;
      // visit curr root
      root = root->right;
    }
  } else {
    // visit curr root
    root = root->right;
  }
}
```
 
 time complexity = O(n)<br/>
 space complexity = O(1)
 **note**:
always maintain a prev node if needs to comparison with predecessor
 
*Reference*<br/>
https://www.youtube.com/watch?v=wGXB9OWhPTg&t=435s<br/>
https://www.youtube.com/watch?v=QZMropFflv4&t=736s
