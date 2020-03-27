## Sample 1192: critical connections

1. create dictionary for all nodes like [node : neighbor nodes]
2. DFS traverse all nodes, for each node, give it a current level
3. return the minimum level among the current node's neighbors, include the node itself
    explain: in this method, if the level given by parent node has been changed, which means there is another node reaching the current child node. In other word, the current connection between parent node and child node is not critical.
    
