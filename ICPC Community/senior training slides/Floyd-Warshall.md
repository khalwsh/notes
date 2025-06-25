Normally when getting shortest path , let we have n nodes in the graph and we need shortest path from two nodes i , j  this shortest path could be direct going from i to j or indirect by using some intermediate nodes , so this gives a recurrences.

```pesudo-code 
for k in nodes:
	if k not i and k not j
		dist[i][j] = min(solve(i , k) + solve(k , j) , dist[i][j]);
```

but we note this is cyclic recurrence , the shortest path from 1 to 5 could use 3 then 3 use 1.

To avoid we can add one more parameter which is k , this is the current limit on which intermediate vertices we are allowed to use , when k = 0 this is the base case we must use no intermediate nodes , otherwise he have 2 options either to skip this node and use only nodes from 0 to k - 1 as intermediate , or we use it.

```cpp
int floyed(int i , int j , int k){
	if(k == 0){
		return cost[i][j];
	}
	return min(floyed(i , j , k - 1) , floyed(i , k , k - 1) + floyed(k , j , k - 1));
}
```

note , we can turn this into iterative dp , and it will look like this

```cpp
memset(dp , '?' , sizeof dp);
// dp[k] : kth level
for(int i = 0;i < n;i++) for(int j = 0;j < n;j++) dp[0][i][j] = adj[i][j];
for(int k = 0;k < n;k++){
	for(int i = 0;i < n;i++){
		for(int j = 0;j < n;j++){
			dp[k + 1][i][j] = min(dp[k + 1][i][j] , dp[k][i][k] , dp[k][k][j]);
			}
	}
}
dp[n][*][*] is the shortest path 
```

note we can use rolling trick , to reduce memory from 3D to 2 arrays each is 2D

```cpp
memset(dp , '?' , sizeof dp);
for(int i = 0;i < n;i++) for(int j = 0;j < n;j++) dp[0][i][j] = adj[i][j];
for(int k = 0;k < n;k++){
	for(int i = 0;i < n;i++){
		for(int j = 0;j < n;j++){
			dp[(k + 1) & 1][i][j] = min(dp[(k + 1) & 1][i][j] , dp[k & 1][i][k] + dp[k & 1][k][j]);
			}
	}
	swap(dp[0] , dp[1]);
}
```

note this is minimization problem , and we use paths from layer k - 1 to update layer k , it will not hurt us that if the path we use from layer k - 1 is update in layer k which mean we can do it in place , as it is a minimization problem it will work

```cpp
memset(dp , '?' , sizeof dp);
for(int i = 0;i < n;i++) for(int j = 0;j < n;j++) dp[i][j] = adj[i][j];
for(int k = 0;k < n;k++){
	for(int i = 0;i < n;i++){
		for(int j = 0;j < n;j++){
				dp[i][j] = min(dp[i][j] , dp[i][k] + dp[k][j]);
			}
	}
}
```

#### common mistakes 
- order of the loops matter!!! this is the key observation let us to reduce the memory and turn it to iterative!!!
- make the diagonals zero (path to yourself is 0)
- make sure that there is path before using it in the transition
- mark no edge as INF
- be careful with multiple edges and self cycles

https://cses.fi/problemset/task/1672
[Problem - B - Codeforces](https://codeforces.com/contest/295/problem/B)
https://codeforces.com/contest/25/problem/C
---
Now how to check a negative cycle exist in Floyd , it is just easy check if any node reach itself with cost less than 0

```cpp
bool NegativeCycle(vector<vector<int>>&adj,int n) {
    //apply floyed first
    for (int i = 0; i < n; i++)
        if (adj[i][i] != 0)
            return true;
    return false;
}
```

https://codeforces.com/contest/33/problem/B

How to know a certain path pass through negative cycle , like check if path u , v pass through negative cycle , just check if there is node i that in a cycle and u reach i and i reach v

[All Pairs Shortest Path – Kattis, Kattis](https://open.kattis.com/problems/allpairspath)

```cpp
bool CheckCycleEffect(vector<vector<int>>&adj,int source,int dist) {
    //apply floyed first
    int n = (int)adj.size();
    for (int i = 0; i < n; i++) {
        if (adj[i][i] != 0 && adj[source][i] < inf && adj[i][dist] < inf)
            return true;
    }
    return false;
}
```

---
### Floyd applications

##### path exist
some times we only are interesting with path exist or not , not the shortest path , we can use it in detecting connectivity 

```cpp
void PathExist(vector<vector<bool>>&adj) {
    int n = (int) adj.size();
    for (int k = 0; k < n; k++){
        for (int i = 0; i < n; i++){
            for (int j = 0; j < n; j++)
                adj[i][j] |= (adj[i][k] & adj[k][j]);
        }
    }
}
```

##### min-max
here we are interested to get the path between all pairs such that the maximum edge in the path is minimized

```cpp
void MaxMinEdgeONPath(vector<vector<int>>&adj) {
    //adj[i][j] -- > represent the min(max edge) you can get in the shortest path between i , j
    int n = (int)adj.size();
    for (int k = 0; k < n; k++){
        for (int i = 0; i < n; i++){
            for (int j = 0; j < n; j++)
                adj[i][j] = min(adj[i][j], max(adj[i][k], adj[k][j]));
        }
    }
}
```

##### max-min
here we are interested to get the path between all pairs such that the minimum edge in the path is maximized 

```cpp
void MaxMinEdge(vector<vector<int>>&adj,int n) {
    //find road that min value is maximum
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++)
                adj[i][j] = max(adj[i][j], min(adj[i][k], adj[k][j]));
        }
    }
}
```
[Audiophobia - UVA 10048 - Virtual Judge](https://vjudge.net/problem/UVA-10048)

##### longest path in directed acyclic graph (DAG)

```cpp
void LongestPath(vector<vector<int>>&adj,int n) {
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                adj[i][j] = max(adj[i][j], max(adj[i][k], adj[k][j]));
            }
        }
    }
}
```

##### count paths
here we need to know how many paths between i , j (work on DAGs) 
we can ask why this work , we assumed we can work in-place because the transition was min so it will not affect our answer , but here it affect if we used some cell , then updated it then used it again!!

but we note this can not happen , because you will break only when k = i or k = j but when this happen assuming we are in a DAG , the adj k , k will be always 0 and our transition is multiplication so it will not affect.

if this graph is not a DAG , this mean any node reach the cycle and the cycle reaches j this mean the count is useless and we mark it as inf

```cpp
void CountPaths(vector<vector<int>>&adj) {
    int n = (int)adj.size();
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++)
                adj[i][j] += adj[i][k] * adj[k][j];
        }
    }
}
```

##### SCC using Floyd

```cpp
int n;
vector<vector<int>>adj;
vector<int> comp(n, -1);
vector<vector<int>>scc() {
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        if (comp[i] == -1) {
            comp[i] = cnt++;
            for (int j = 0; j < n; j++) {
                if (adj[i][j] < inf && adj[j][i] < inf)
                    comp[j] = comp[i];
            }
        }
    }
    vector<vector<int>>CompGraph(cnt,vector<int>(cnt));
    for(int i=0;i<n;i++) {
        for (int j = 0; j < n; j++) {
            if (adj[i][j] != inf && comp[i] != comp[j]) {
                CompGraph[comp[i]][comp[j]] += 1;
            }
        }
    }
    return CompGraph;//compressed graph
}
```

