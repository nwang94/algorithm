leetcode 269
---------------------
- build a char par -> child graph, row: parent, col: child
- starting from a random existing node, call dfs to traverse all nodes
  - dfs return a bool showing whether there's a cycle， while no cycle put the node to result
  - **Tarjan's** algorithm to detect cycle
 - maintain an visited array
