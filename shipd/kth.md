---
share_link: https://share.note.sx/d9a413ju#+rrCKSXe98bhY48wjCw+CqOVeXWzZHiPVRYMvwZabmE
share_updated: 2025-04-05T19:31:45+02:00
---
### statement

```python
import sys
from typing import List
def kth(N : int , Q : int , arr : list[int] , Edges: list[list[int]], operations : list[list[int]]):
	"""
	At our regular ICPC community meetups , we challenge each other with puzzles that mix fun and problem solving , Mayada the head of the ICPC community challeged the group with interesting problem , it is about family tree , Each person in the family is represented as a node in a tree , and each node is assigned an integer.
The tree consists of N nodes and it is rooted at node 1 , for a given node V , your task is to find the k-th largest number in the subtree rooted at V
and you are given them in form of queries , where each query contains 2 integers the Node V , and K asking for the Kth largest integer in subtree of node V.

Args:
	N(int) : number of nodes in the tree , 1 <= N <= 100000
	
	Q(int) : number of queries in the tree , 1 <= Q <= 100000
	
	arr(list[int]) : an array of integers of length N , 1 <= arr[i] <= 1000000000.
	
	Edges(list[list[int]]) : an 2D list of edges where each list contains 2 integers represnet that there is edge between those 2 nodes , 1 <= u <= N , 1 <= v <= N , where u , v are the 2 nodes have edge between them.
	
	operations(list[list[int]]) : A 2D list of queries each list contains 2 integers node in the queries , 1 <= V <= N , kth largest in the queries , 1 <= k <= 100

Rasies:
	ValueError with message "Invalid input" If any input does not meet the specified constraints.

Returns:
	list[int] : The answer for the queries
	
Examples : 
	>>> kth(5 , 2 , [10 , 20 ,30 , 40 ,50] , [[1 , 4],[2 ,1],[2 ,5] , [3 , 2]] , [[1 , 2] ,[2 , 1]])
	[40 , 50]

	>>> kth(1 ,1 , [42] , [] , [[1 , 1]])
	[42]
	 
	>>> kth(3 , 1 ,[10 , 20 , 30] , [[1 , 4] , [2 , 3]] , [[1 , 1]])
	"Invalid input"
	"""
```
---------

### solution
```python 
    # Validate inputs  
    if not (1 <= N <= 100000):  
        raise ValueError("Invalid input")  
    if not (1 <= Q <= 100000):  
        raise ValueError("Invalid input")  
    if len(arr) != N:  
        raise ValueError("Invalid input")  
    for x in arr:  
        if not (1 <= x <= 1000000000):  
            raise ValueError("Invalid input")  
    if len(Edges) != N - 1:  
        raise ValueError("Invalid input")  
    for edge in Edges:  
        if not (isinstance(edge, (list, tuple)) and len(edge) == 2):  
            raise ValueError("Invalid input")  
        u, v = edge  
        if not (1 <= u <= N) or not (1 <= v <= N):  
            raise ValueError("Invalid input")  
    if len(operations) != Q:  
        raise ValueError("Invalid input")  
    for op in operations:  
        if not (isinstance(op, (list, tuple)) and len(op) == 2):  
            raise ValueError("Invalid input")  
        u, k = op  
        if not (1 <= u <= N) or not (1 <= k <= 100):  
            raise ValueError("Invalid input")  
  
  
    # Build the tree (0-indexed)  
    adj = [[] for _ in range(N)]  
    for u, v in Edges:  
        u -= 1  
        v -= 1  
        adj[u].append(v)  
        adj[v].append(u)  
  
    # Prepare queries: each query is stored as (k-1, query_index)  
    queries = [[] for _ in range(N)]  
    for idx, (u, k) in enumerate(operations):  
        queries[u - 1].append((k - 1, idx))  
  
    # dp[node] will store at most 20 largest elements (in descending order) in the subtree of node.  
    dp = [[] for _ in range(N)]  
    ans = [0] * Q  
  
    def dfs(node: int, parent: int):  
        # start with the current node's value  
        merged = [arr[node]]  
        # process all children  
        for child in adj[node]:  
            if child == parent:  
                continue  
            dfs(child, node)  
            # merge dp[child] into merged list  
            merged.extend(dp[child])  
            # clear child's dp to free memory (optional in Python)  
            dp[child].clear()  
        # sort merged values in descending order and keep at most 20  
        merged.sort(reverse=True)  
        if len(merged) > 100:  
            merged = merged[:100]  
        dp[node] = merged  
        # answer the queries at this node  
        for k_index, query_id in queries[node]:  
            # assuming the query k is always valid (as per constraints)  
            ans[query_id] = dp[node][k_index]  
  
    dfs(0, -1)  
  
    return ans
```
-------
Test Driver
```python
import unittest  
  
class TestTreeQueries(unittest.TestCase):  
  
    def test_small_tree(self):  
        N = 5  
        Q = 2  
        arr = [10, 20, 30, 40, 50]  
        Edges = [[1, 2], [1, 3], [2, 4], [2, 5]]  
        operations = [[1, 2], [2, 3]]  
        expected = [40, 20]  
        self.assertEqual(kth(N, Q, arr, Edges, operations), expected)  
  
    def test_single_node_tree(self):  
        N = 1  
        Q = 1  
        arr = [42]  
        Edges = []  
        operations = [[1, 1]]  
        expected = [42]  
        self.assertEqual(kth(N, Q, arr, Edges, operations), expected)  
  
    def test_chain_tree(self):  
        N = 4  
        Q = 2  
        arr = [5, 15, 10, 20]  
        Edges = [[1, 2], [2, 3], [3, 4]]  
        operations = [[1, 3], [2, 2]]  
        expected = [10, 15]  
        self.assertEqual(kth(N, Q, arr, Edges, operations), expected)  
  
    def test_invalid_input_arr_length(self):  
        N = 3  
        Q = 1  
        arr = [10, 20]  
        Edges = [[1, 2], [2, 3]]  
        operations = [[1, 1]]  
        with self.assertRaises(ValueError) as context:  
            kth(N, Q, arr, Edges, operations)  
        self.assertEqual(str(context.exception), "Invalid input")  
  
    def test_invalid_input_edge(self):  
        N = 3  
        Q = 1  
        arr = [10, 20, 30]  
        Edges = [[1, 4], [2, 3]]  
        operations = [[1, 1]]  
        with self.assertRaises(ValueError) as context:  
            kth(N, Q, arr, Edges, operations)  
        self.assertEqual(str(context.exception), "Invalid input")  
  
    def test_invalid_input_operation(self):  
        N = 3  
        Q = 1  
        arr = [10, 20, 30]  
        Edges = [[1, 2], [2, 3]]  
        operations = [[1, 101]]  
        with self.assertRaises(ValueError) as context:  
            kth(N, Q, arr, Edges, operations)  
        self.assertEqual(str(context.exception), "Invalid input")  
    def test_balanced_tree_multiple_queries(self):  
        N = 7  
        Q = 3  
        arr = [10, 20, 30, 40, 50, 60, 70]  
        Edges = [[1, 2], [1, 3], [2, 4], [2, 5], [3, 6], [3, 7]]  
        operations = [  
            [1, 1],  
            [2, 3],  
            [3, 2],  
        ]  
        expected = [70, 20, 60]  
        self.assertEqual(kth(N, Q, arr, Edges, operations), expected)  
  
    def test_star_shaped_tree(self):  
        N = 5  
        Q = 2  
        arr = [5, 50, 40, 30, 10]  
        Edges = [[1, 2], [1, 3], [1, 4], [4, 5]]  
        operations = [  
            [1, 2],  
            [4, 2],  
        ]  
        expected = [40, 10]  
        self.assertEqual(kth(N, Q, arr, Edges, operations), expected)  
    def test_large_tree(self):  
        import random  
        from collections import defaultdict  
        import sys  
  
        N = 10000  
        Q = 10  
        arr = list(range(1, N + 1))  
  
        Edges = []  
        for i in range(2, N + 1):  
            parent = i // 2  
            Edges.append([parent, i])  
  
        tree = defaultdict(list)  
        for u, v in Edges:  
            tree[u].append(v)  
            tree[v].append(u)  
  
        sys.setrecursionlimit(20000)  
        children = defaultdict(list)  
        def dfs(node, parent):  
            for child in tree[node]:  
                if child != parent:  
                    children[node].append(child)  
                    dfs(child, node)  
        dfs(1, 0)  
  
        def subtree_nodes(node):  
            nodes = [node]  
            for child in children[node]:  
                nodes.extend(subtree_nodes(child))  
            return nodes  
  
        operations = []  
        expected = []  
        random.seed(42)  
        for _ in range(Q):  
            node = random.randint(1, N)  
            k = random.randint(1, 50)  
            nodes = subtree_nodes(node)  
            if k > len(nodes):  
                k = len(nodes)  
            values = [arr[n - 1] for n in nodes]  
            sorted_values = sorted(values, reverse=True)  
            kth_val = sorted_values[k - 1]  
  
            operations.append([node, k])  
            expected.append(kth_val)  
  
        self.assertEqual(kth(N, Q, arr, Edges, operations), expected)  
  
if __name__ == "__main__":  
    unittest.main()
```
----------
Test Case:
```python
assert kth(5 , 2 , [10 , 20 ,30 , 40 ,50] , [[1 , 4],[2 ,1],[2 ,5] , [3 , 2]] , [[1 , 2] ,[2 , 1]]) == [40 , 50]
```