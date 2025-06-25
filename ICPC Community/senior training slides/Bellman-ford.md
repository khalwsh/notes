### Introduction
remember Dijkstra : single-source shortest path algorithm , but negative weights break it!!
note by break , I mean it will TLE not WA.

Why we may need -ve weights?
like money transaction , score in game , longest path (multiply all weights by -1 and get shortest path)

some algorithms like Max-flow it needs negative weights when creating the residual graph , so if you want MCMF you need bellman

note existence of a Negative cycle not necessary mean all nodes in the graph are affected by this cycle , example
![[Pasted image 20250604152339.png]]
so in this graph if the source was 0 , we can get shortest path from nodes 5 , 6 , 7 , 0 but not for 1 , 4 , 2 , 3 cause of negative cycle

---
### Bellman ford Algorithm
imagine we have all possible shortest paths from 0 with at most 2 edges  
![[Pasted image 20250604152737.png]]
so we have {0 , 1} = 3 , {0 , 1 , 2} = 9 , {0 , 1 , 4} = 4.
so we can use this to get all shortest paths from 0 with at most 3 edges and minimize , we can observe that a simple path is at most n - 1 edges , so we need to do the previous process n - 1 times , and after finishing we will have shortest path from source to all nodes , but how we should detect and handle negative cycles , using the fact we mentioned that at most n - 1 edges , then if we try to improve our shortest path in the nth iteration and we succeed then this mean there is a path of length n which is better than a simple path !! , which mean there is a negative cycle and the nodes improved their cost is the negative cycle nodes.

so the dynamic programming code looks like this:
![[Pasted image 20250604153414.png]]

we can optimize using Adj List instead which make the time o(NM) and write iterative with rolling trick to allow memory to be o(n) , as we already taking minimum so we actually don't need rolling trick , it is enough to minimize in-place 

```cpp
int n, m;
vector<array<ll , 3>> edges;

void solve(int source){
    vector<ll> d(n, inf);
    d[source] = 0;
    for (int i = 0; i < n - 1; ++i)
        for (auto& [a , b , cost]: edges)
            d[b] = min(d[b], d[a] + cost);
}
```

Negative cycle detection
As we know if we find any node has been relaxed in the nth iteration then we have a negative cycle so we can check by the following way
```cpp
int n, m;
vector<array<ll , 3>> edges;

bool solve(int source){
    vector<ll> d(n, inf);
    d[source] = 0;
    for (int i = 0; i < n - 1; ++i)
        for (auto& [a , b , cost]: edges)
            d[b] = min(d[b], d[a] + cost);
    for (auto& [a , b , cost]: edges)
	    if (d[b] > d[a] + cost) return true; // negative cycle exist
	return false;
}
```

Get path 
we build previous array to act like chain , if we started at the goal we climb this chain until we reach the source
```cpp
struct Edge {
    int a, b, cost;
};
int n, m;
vector<Edge> edges;
vector<int> solve(int source,int goal) {
    vector<int> d(n, inf);
    d[source] = 0;
    vector<int> p(n, -1);

    for (int i=0;i<n-1;i++) {
        bool any = false;
        for (Edge e : edges)
            if (d[e.a] < inf)
                if (d[e.b] > d[e.a] + e.cost) {
                    d[e.b] = d[e.a] + e.cost;
                    p[e.b] = e.a;
                    any = true;
                }
        if (!any)
            break;
    }

    if (d[goal] == inf)
            return vector<int>{-1};
    else {
            vector<int> path;
            for (int cur = goal; cur != -1; cur = p[cur])
            path.push_back(cur);
            reverse(path.begin(), path.end());
            return path;
         }
}
```

we can know all nodes affected by -ve cycle
Let's build shortest path using bellman and save the result , then we re run bellman again for n iteration , while making the base array the array generated from the first time we run bellman , any changed value is part of negative cycle , either it is a negative cycle node or the negative cycle reach it.  and we will do BFS from those nodes to get all nodes affected by the cycle
```cpp
const ll INF = 1e18;  
int n;  
vector<array<ll , 3>>edges;  
vector<int> solve(int source) {  
    vector<ll> dist(n, INF);  
    dist[source] = 0;  
    for (int pass = 0; pass < n - 1; ++pass) {  
        bool changed = false;  
        for (auto &e : edges) {  
            int u = (int)e[0], v = (int)e[1];  
            ll w = e[2];  
            if (dist[u] < INF && dist[u] + w < dist[v]) {  
                dist[v] = dist[u] + w;  
                changed = true;  
            }  
        }  
        if (!changed) break;  
    }  
  
    vector<ll> ref = dist;  
    for (int pass = 0; pass < n; ++pass) {  
        for (auto &e : edges) {  
            int u = (int)e[0], v = (int)e[1];  
            ll w = e[2];  
            if (dist[u] < INF && dist[u] + w < dist[v]) {  
                dist[v] = dist[u] + w;  
            }  
        }  
    }  
  
    vector<bool> neg_inf(n, false);  
    queue<int> bfs;  
    for (int i = 0; i < n; ++i) {  
        if (ref[i] != dist[i]) {  
            neg_inf[i] = true;  
            bfs.push(i);  
        }  
    }  
    vector<vector<int>> adj(n);  
    for (auto &e : edges) {  
        int u = (int)e[0], v = (int)e[1];  
        adj[u].push_back(v);  
    }  
    while (!bfs.empty()) {  
        int u = bfs.front();  
        bfs.pop();  
        for (int nxt : adj[u]) {  
            if (!neg_inf[nxt]) {  
                neg_inf[nxt] = true;  
                bfs.push(nxt);  
            }  
        }  
    }  
    vector<int> nodes;  
    for (int i = 0; i < n; ++i) {  
        if (neg_inf[i]) {  
            nodes.push_back(i);  
        }  
    }  
    return nodes;  
}
```

Find negative cycle itself
we can start from any affected node , we have 2 options , either this node is part from cycle or the cycle reach it , so we can use the previous array we built and the chains we build will be cycled if there is a cycle , so if we climbed n iterations backward this guarantee that we reach and stuck in the cycle. after that we get a cycled node , then we start getting the nodes in the cycle until we visit the start node again

Negative cycle given source that can reach this cycle
```cpp
struct Edge {
    int a, b, cost;
};

int n, m;
vector<Edge> edges;

vector<int> solve(int source){
    vector<int> d(n, inf);
    d[source] = 0;
    vector<int> p(n, -1);
    int x;
    for (int i = 0; i < n; ++i) {
        x = -1;
        for (Edge e : edges)
            if (d[e.a] < inf)
                if (d[e.b] > d[e.a] + e.cost) {
                    d[e.b] = max(-INF, d[e.a] + e.cost);
                    p[e.b] = e.a;
                    x = e.b;
                }
    }

    if (x == -1)
        return vector<int>{-1};
    else {
        int y = x;
        for (int i = 0; i < n; ++i)
            y = p[y];

        vector<int> path;
        for (int cur = y;; cur = p[cur]) {
            path.push_back(cur);
            if (cur == y && path.size() > 1)
                break;
        }
        reverse(path.begin(), path.end());

        return path;
    }
}
```

any negative cycle , just erase the INF condition and allow any node to contribute
```cpp
struct Edge {
    int a, b, cost;
};

int n, m;
vector<Edge> edges;

vector<int> solve(int source){
    vector<int> d(n, inf);
    d[source] = 0;
    vector<int> p(n, -1);
    int x;
    for (int i = 0; i < n; ++i) {
        x = -1;
        for (Edge e : edges)
                if (d[e.b] > d[e.a] + e.cost) {
                    d[e.b] = max(-INF, d[e.a] + e.cost);
                    p[e.b] = e.a;
                    x = e.b;
                }
    }

    if (x == -1)
        return vector<int>{-1};
    else {
        int y = x;
        for (int i = 0; i < n; ++i)
            y = p[y];

        vector<int> path;
        for (int cur = y;; cur = p[cur]) {
            path.push_back(cur);
            if (cur == y && path.size() > 1)
                break;
        }
        reverse(path.begin(), path.end());

        return path;
    }
}
```

Need positive cycle ? multiple with -1 and get negative cycle

---
### problems
1 - https://open.kattis.com/problems/shortestpath3
```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
  
void PRE() {  
    ios_base::sync_with_stdio(false);  
    cin.tie(0);  
    cout.tie(0);  
#ifndef ONLINE_JUDGE  
    freopen("in.txt", "r", stdin);  
    freopen("out.txt", "w", stdout);  
    freopen("error.txt", "w", stderr);  
#endif  
}  
  
int main() {  
    PRE();  
    int n , m , q , s;  
    while (cin>>n>>m>>q>>s && n) {  
        vector<array<ll , 3>>edges(m);  
        vector<int>adj[n];  
        for (auto &[a , b , w]: edges) {  
            cin>>a>>b>>w;  
            adj[a].emplace_back(b);  
        }  
        vector<bool>part_cycle(n , false);  
        vector<ll>dist(n , 1e18);  
        dist[s] = 0;  
        for (int i = 0;i < n - 1;i++) {  
            for (auto &[a , b , w] : edges) {  
                if (dist[a] < 1e18)  
                    dist[b] = min(dist[a] + w , dist[b]);  
            }  
        }  
        auto ref = dist;  
        for (int i = 0;i < n;i++) {  
            for (auto &[a , b , w] : edges) {  
                if (dist[a] < 1e18)  
                    dist[b] = min(dist[a] + w , dist[b]);  
            }  
        }  
        queue<int>qq;  
        for (int i = 0;i < n;i++) {  
            if (ref[i] != dist[i]) {  
                qq.push(i);  
                part_cycle[i] = true;  
            }  
        }  
        while (qq.size()) {  
            int u = qq.front();  
            qq.pop();  
            for (auto &v : adj[u]) {  
                if (!part_cycle[v]) {  
                    part_cycle[v] = true;  
                    qq.push(v);  
                }  
            }  
        }  
        while (q--) {  
            int u;cin>>u;  
            if (part_cycle[u]) cout<<"-Infinity";  
            else {  
                if (dist[u] == 1e18) {  
                    cout<<"Impossible";  
                }else  
                    cout<<dist[u];  
            }  
            cout<<'\n';  
        }  
        cout<<'\n';  
    }  
}
```

2 - https://cses.fi/problemset/task/1673/
```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
  
void PRE() {  
    ios_base::sync_with_stdio(false);  
    cin.tie(0);  
    cout.tie(0);  
#ifndef ONLINE_JUDGE  
    freopen("in.txt", "r", stdin);  
    freopen("out.txt", "w", stdout);  
    freopen("error.txt", "w", stderr);  
#endif  
}  
const ll inf = 1e18;  
const int N = 5001;  
vector<int>adj[N];  
int main() {  
    PRE();  
    int n , m;cin>>n>>m;  
    vector<array<ll , 3>>edges(m);  
    for (auto &[u ,  v , w] : edges) {  
        cin>>u>>v>>w;  
        w = -w;  
        u-- , v--;  
        adj[u].emplace_back(v);  
    }  
    vector<ll>dist(n , inf);  
    dist[0] = 0;  
    for (int i = 0;i < n - 1;i++) {  
        for (auto &[u , v , w] : edges) {  
            if (dist[u] < inf) dist[v] = min(dist[u] + w , dist[v]);  
        }  
    }  
    auto ref = dist;  
    queue<int>q;  
    for (int i = 0;i < n;i++) {  
        for (auto &[u , v , w] : edges) {  
            if (dist[u] < inf) dist[v] = min(dist[u] + w , dist[v]);  
        }  
    }  
    vector<bool>cyclic(n , false);  
    for (int i = 0;i < n;i++) {  
        if (ref[i] != dist[i])  
            q.push(i) , cyclic[i] = true;  
    }  
    while (q.size()) {  
        int u = q.front();  
        q.pop();  
        for (auto &v : adj[u]) {  
            if (!cyclic[v]) {  
                cyclic[v] = true;  
                q.push(v);  
            }  
        }  
    }  
    if (cyclic[n - 1]) {  
        cout<<-1;  
    }else  
        cout<<-dist[n - 1];  
}
```

3 - https://cses.fi/problemset/task/1197/
```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
  
void PRE() {  
    ios_base::sync_with_stdio(false);  
    cin.tie(0);  
    cout.tie(0);  
#ifndef ONLINE_JUDGE  
    freopen("in.txt", "r", stdin);  
    freopen("out.txt", "w", stdout);  
    freopen("error.txt", "w", stderr);  
#endif  
}  
const ll inf = 1e18;  
int main() {  
    PRE();  
    int n , m;cin>>n>>m;  
    vector<array<ll , 3>>edges(m);  
    for (auto &[u ,  v , w] : edges) {  
        cin>>u>>v>>w;  
        u-- , v--;  
    }  
    vector<ll>dist(n , 0);  
    vector<int>prev(n , -1);  
    int cyclic_node = -1;  
    for (int i = 0;i < n;i++) {  
        for (auto &[u , v , w] : edges) {  
            if (dist[v] > dist[u] + w) {  
                dist[v] = dist[u] + w;  
                prev[v] = u;  
                if (i == n - 1) cyclic_node = v;  
            }  
        }  
    }  
    if (~cyclic_node) {  
        cout<<"YES\n";  
        for (int i = 0;i < n;i++) {  
            cyclic_node = prev[cyclic_node];  
        }  
        vector<int> path;  
        for (int v = cyclic_node;; v = prev[v]) {  
            path.push_back(v);  
            if (v == cyclic_node && path.size() > 1)  
                break;  
        }  
        reverse(path.begin(), path.end());  
        for(auto &val:path)  
            cout<<val+1<<" ";  
  
    }else {  
        cout<<"NO";  
    }  
}
```

4 - [Problem - L - Codeforces](https://codeforces.com/gym/101498/problem/L)
```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
  
void PRE() {  
    ios_base::sync_with_stdio(false);  
    cin.tie(0);  
    cout.tie(0);  
#ifndef ONLINE_JUDGE  
    freopen("in.txt", "r", stdin);  
    freopen("out.txt", "w", stdout);  
    freopen("error.txt", "w", stderr);  
#endif  
}  
  
int main() {  
    PRE();  
    int t;  
    cin>>t;  
    while(t--) {  
        int n,m;  
        cin>>n>>m;  
        vector<array<ll , 3>>edges(m);  
        ll mn = 1e18;  
        for (auto &[u , v , w] : edges) {  
            cin>>u>>v>>w;  
            u-- , v--;  
            mn = min(mn , w);  
        }  
        if (mn >= 0) {  
            cout<<mn<<'\n';  
            continue;  
        }  
        int dummy = n;  
        for (int i = 0;i < n;i++) {  
            edges.emplace_back(array<ll , 3>{dummy , i , 0});  
        }  
        vector<ll>dist(n + 1, 1e18);  
        dist[dummy] = 0;  
        bool cycle = false;  
        for (int i = 0;i <= n;i++) {  
            for (auto &[u , v , w] : edges) {  
                if (dist[v] > dist[u] + w) {  
                    dist[v] = dist[u] + w;  
                    cycle = cycle | (i == n);  
                }  
            }  
        }  
        ll res = 1e18;  
        for (int i = 0;i < n;i++) res = min(res , dist[i]);  
        if (cycle || res == 1e18) {  
            cout<<"-inf";  
        }else  
            cout<<res;  
        cout<<'\n';  
    }  
}
```