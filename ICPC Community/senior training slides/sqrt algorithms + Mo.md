
Resources:
- [Sqrt Decomposition - Algorithms for Competitive Programming](https://cp-algorithms.com/data_structures/sqrt_decomposition.html#mos-algorithm) 
- Errichto : [Square Root Decomposition, Mo's Algorithm](https://www.youtube.com/watch?v=BJhzd_VG61k) 
- Hilbert function optimization (serso hates it because he doesn't know it): [An alternative sorting order for Mo's algorithm - Codeforces](https://codeforces.com/blog/entry/61203) khalwsh implementation for Mo with Hilbert : [Competitive-Programming/Mo's/Fast Mo Template.cpp at main · khalwsh/Competitive-Programming](https://github.com/khalwsh/Competitive-Programming/blob/main/Mo's/Fast%20Mo%20Template.cpp)
- khalwsh implementation for normal MO: https://github.com/khalwsh/Competitive-Programming/blob/main/Mo's/MoTemplate.cpp
- for Mo on Tree I can't find material but I will explain it , I found this one only: https://codeforces.com/blog/entry/43230
- optional and I will not cover (because I didn't saw any problems require them rather the ones I practiced on them when I learnt this topic) Mo with updates , Mo in more than 2 dimensions , Mo on tree with updates

problems we will solve in sessions
- normal Mo
	- http://codeforces.com/problemset/problem/86/D
	- https://www.spoj.com/problems/DQUERY
	- http://codeforces.com/problemset/problem/375/D
	- https://codeforces.com/group/Rilx5irOux/contest/562370/problem/L
	- [Problem - I - Codeforces](https://codeforces.com/group/Rilx5irOux/contest/532378/problem/I)

- Mo on tree problems
	- https://www.spoj.com/problems/COT2/  Mo on tree
	- [SPOJ.com - Problem COT](https://www.spoj.com/problems/COT/en/)
	- https://codeforces.com/gym/100962/problem/F in this one I will introduce sqrt freq array trick
- Mo with rollback dsu
	- [Chef and Graph Queries Practice Coding Problem](https://www.codechef.com/problems/GERALD07?tab=statement)

------------
#### SQRT Decomposition
- divide the array into blocks (length of one block is sqrt(n))
	- for a query , it will have not full block from start , not full block from end , some full blocks , we will solve each full block without have to loop on it while looping on the part from start , end overall the time (sqrt(n) * time to compute answer) ![[Pasted image 20250327141439.png]]
	- we can use this technique of blinding intra links between blocks , starting at any place in the block , you precompute where you reach.![[Pasted image 20250327141410.png]]
- Moore's Algorithm (Mo or Moz)
	- you want to answer some queries which are hard to compute , but you can do it inclemently , we are just doing brute force in smart way ![[Pasted image 20250327145509.png]]
	- the idea is within the same block the r pointer move only forward while l worst case are size of the block , between 2 consecutive blocks L move forward and r in worst case can move N 
	- try hard to calculate the insert and erase in o(1) or very small log , if the Mo can be avoid , then avoid it
	- How to choose the optimal block size ?
		first we are sorting all queries O(Q log Q)
		now we want to calculate how many remove , add , query , let the block size is S , we call the right pointer O(N * N / S) because we have N / S blocks and in worst case each block will move the right from 0 to N so we call insert (right) , erase(right) N * N / S times
		
		we call the left pointer O(SQ) because it changes at most O(S) in each query.
		
		so the time complexity O(SQ + N * N / S + Q log Q) we want to choose the best S to minimize the equation as possible (note if here I considered insert and remove is o(1) and if it is not the case we need to put them in the equation)
		
		as we know to find the local minimal value (depend on S) of some equation like SQ + N * N / S + Q log Q , then we need to differentiate with respect to S and solve the equation when it equal 0
		
		SQ + N * N / S + Q log Q ---> Q - N * N / (S * S) + 0 = 0 so N * N / (S * S) = Q.
		
		S * S * Q = N * N , S = sqrt(N * N / Q) which is N / Sqrt(Q) in worse case when N equal Q then N / sqrt(Q) ---> Sqrt(N) is the best block.
		
		note again , if you insert or remove in log you must add in the equation in order to do correct calculation.
- Heavy and light nodes
	- simple explanation just vary your algorithm based on the case , like do something for nodes with High adj list size and do something else to Low adj list size nodes
	- simple problem example : find number of triangles in a graph , the idea is to decompose the graph into heavy nodes > sqrt(n) , light nodes <= sqrt(n) and we want to count the nodes HHH , HLH , HLL , LLL we will vary our algorithm when count each case.
	  for the Heavy nodes we know only at most sqrt(n) such nodes exist so to count the HHH we can just 3 loops on the heavy nodes , complexity (sqrt(n) choose 3)
	  for the HHL loop on the edges and check if it connects H and L if so loop on the L adjacent and check if the H connects to it back but note we counted twice , so divide by 2 , to count HLL , if edge connect LL then we count the intersection in H (using 2 pointers) divide by 2 also , same in LLL but divide by 3.
	- also another problem ![[Pasted image 20250327152438.png]]
	 to solve this problem , we do multi source bfs (the queue keep track current cell and the source to avoid looping ) if the letter exist more than sqrt(n) otherwise we do nested loops
- batch processing 
	- the main idea is to build some structure that can answer the problem in o(1) but it can't be updated so we keep some buffer , when we get query update we put it in the buffer until the buffer reach some threshold (sqrt for example) then rebuild our data structure. but how to answer queries online ? we can take the best answer from the already built data structure and the answer of looping on the buffer to extract the answer.  (problem example : xenia and tree)
- observe tricky constraints inside the statement
	- let a knapsack problem where you  are given weights with total sum < 10 ^ 5 and N up to 10 ^ 5 , we can observe that there are at most sqrt(10 ^ 5) distinct weight so we can convert it to N = 300 and each element you can take it some number of times (this is a standard problem) then solve this variation.
- Precompute till sqrt 
	- in a problem something will get time limit and we can't precompute so we can precompute until sqrt and the bigger than sqrt we solve it online within the query time for example what is the sum if you started at the ith element and you jump with y value , you are given queries about that
--------
Mo implementations
first one the most basic
```cpp
int BLOCK_SIZE = 317;
struct Query{
    int l , r,id;
    bool operator < (const Query &other) const{
        int n1 = l / BLOCK_SIZE, n2 = other.l / BLOCK_SIZE;
        if(n1 != n2) return n1 < n2;
        return r < other.r;
    }
};
void add(int MoIdx){

}
void remove(int MoIdx){

}
void Mo(vector<Query> &queries){
    int MoLeft = 0,MoRight = -1;
    for(auto &q: queries){
        while(MoRight < q.r) add(++MoRight);
        while(MoLeft > q.l) add(--MoLeft);
        while(MoRight > q.r) remove(MoRight--);
        while(MoLeft < q.l) remove(MoLeft++);
        //res[q.id] = cnt;
    }
}
```

second minor optimization can cause great deal
```cpp
int BLOCK_SIZE = 317;
struct Query{
    int l , r,id;
    bool operator < (const Query &other) const{
        int n1 = l / BLOCK_SIZE, n2 = other.l / BLOCK_SIZE;
        if(n1 != n2) return n1 < n2;
        return (n1 % 2) ? r > other.r : r < other.r;
    }
};
void add(int MoIdx){

}
void remove(int MoIdx){

}
void Mo(vector<Query> &queries){
    int MoLeft = 0,MoRight = -1;
    for(auto &q: queries){
        while(MoRight < q.r) add(++MoRight);
        while(MoLeft > q.l) add(--MoLeft);
        while(MoRight > q.r) remove(MoRight--);
        while(MoLeft < q.l) remove(MoLeft++);
        //res[q.id] = cnt;
    }
}
```

third using Hilbert functions (مبصمجها)
```cpp
struct Query {
    int l, r, idx;
    int ord;
    static int rotateDelta[4];
    Query(int L,int R,int Idx){
        idx = Idx;
        l = L;
        r = R;
        ord = gilbertOrder(l,r,21,0);
    }
    bool operator<( const Query &b)const {
        return ord < b.ord;
    }
    int gilbertOrder(int x, int y, int pow, int rotate) {
        if (pow == 0) {
            return 0;
        }
        int hpow = 1 << (pow-1);
        int seg = (x < hpow) ? (
                (y < hpow) ? 0 : 3
        ) : (
                          (y < hpow) ? 1 : 2
                  );
        seg = (seg + rotate) & 3;
        int nx = x & (x ^ hpow), ny = y & (y ^ hpow);
        int Nrot = (rotate + rotateDelta[seg]) & 3;
        int64_t subSquareSize = int64_t(1) << (2*pow - 2);
        int64_t ans = seg * subSquareSize;
        int64_t add = gilbertOrder(nx, ny, pow-1, Nrot);
        ans += (seg == 1 || seg == 2) ? add : (subSquareSize - add - 1);
        return ans;
    }
};
int Query::rotateDelta[4] = {3, 0, 0, 1};
void add(int MoIdx){

}
void remove(int MoIdx){

}
void Mo(vector<Query> &queries){
    int MoLeft = 0,MoRight = -1;
    for(auto &q: queries){
        while(MoRight < q.r) add(++MoRight);
        while(MoLeft > q.l) add(--MoLeft);
        while(MoRight > q.r) remove(MoRight--);
        while(MoLeft < q.l) remove(MoLeft++);
        //res[q.id] = cnt;
    }
}
```
-----
Mo on tree , we just do Mo but we walk on tree so , we can decompose the path to 2 paths , from u to lca , then to v , then walk on the tree after sorting the queries , but we first linearize the tree using Euler tour (ال بترمي قدام وورا) and a query about u , v we process it to ranges in the array , but how ? first let's fix that u has timer lower than v (if not swap) then if u is the lca of v then we move from start time of lca to start time of v , and we don't have to insert the lca manually , otherwise we move from the end time of u to the start time of v but the lca in this case need to be update manually as it is the only node in the path out of the range in the linear tree , so we update it's state , then update it's state.

note here we don't just add and delete , we update the state because in the Euler tour each node in the range if this node is exist twice then this node is not in the path otherwise it is part of the path so (we need to keep track of the parity of the node frequency if odd add otherwise erase)

------
dsu with Mo
```cpp
#include <bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
  
void PRE() {  
    ios_base::sync_with_stdio(false);cin.tie(0);cout.tie(0);  
#ifndef ONLINE_JUDGE  
    freopen("in.txt", "r", stdin);  
    freopen("out.txt", "w", stdout);  
    freopen("error.txt", "w", stderr);  
#endif  
}  
  
  
struct RollBackDsu {  
    vector<int> parent, size;  
    vector<array<int,5>> history;  
    vector<int> opStack;  
    int cnt;  
  
    RollBackDsu(int n) : cnt(n) {  
        parent.resize(n);  
        size.assign(n, 1);  
        for (int i = 0; i < n; i++)  
            parent[i] = i;  
    }  
  
    int find(int a) {  
        return (parent[a] == a ? a : find(parent[a]));  
    }  
  
    bool Merge(int a, int b) {  
        a = find(a), b = find(b);  
        if(a == b) {  
            opStack.push_back(0);  
            return false;  
        }  
        if(size[a] > size[b])  
            swap(a, b);  
        history.push_back({a, parent[a], b, size[b], cnt});  
        opStack.push_back(1);  
        parent[a] = b;  
        size[b] += size[a];  
        cnt--;  
        return true;  
    }  
  
    void rollback(int steps) {  
        while(steps--) {  
            int op = opStack.back();  
            opStack.pop_back();  
            if(op == 1) {  
                auto act = history.back();  
                history.pop_back();  
                int a = act[0], oldParent = act[1], b = act[2], oldSize = act[3], oldCnt = act[4];  
                parent[a] = oldParent;  
                size[b] = oldSize;  
                cnt = oldCnt;  
            }  
        }  
    }  
};  
  
struct Query {  
    int l, r, id;  
};  
  
const int NMAX = 200010;  
pair<int, int> edges[NMAX];  
int ans[NMAX];  
int n, m, q;  
int BLOCK_SIZE;  
  
vector<Query> buckets[450];  
int main(){  
    PRE();  
  
    int t;  
    cin >> t;  
    while(t--){  
        cin >> n >> m >> q;  
        for (int i = 0; i < m; i++){  
            cin >> edges[i].first >> edges[i].second;  
        }  
        vector<Query> queries;  
        for (int i = 0; i < q; i++){  
            int l, r;  
            cin >> l >> r;  
            l--, r--;  
            queries.push_back({l, r, i});  
        }  
        BLOCK_SIZE = sqrt(m) + 1;  
        int totalBuckets = (m + BLOCK_SIZE - 1) / BLOCK_SIZE;  
        for (int i = 0; i < totalBuckets; i++){  
            buckets[i].clear();  
        }  
        for (auto &qu: queries){  
            int b = qu.l / BLOCK_SIZE;  
            buckets[b].push_back(qu);  
        }  
        for (int b = 0; b < totalBuckets; b++){  
            sort(buckets[b].begin(), buckets[b].end(), [](const Query &a, const Query &b) {  
                return a.r < b.r;  
            });  
            RollBackDsu dsu(n);  
            int base = (b + 1) * BLOCK_SIZE;  
            if (base > m)  
                base = m;  
            int curR = base;  
            for (auto &qu : buckets[b]) {  
                while (curR <= qu.r && curR < m) {  
                    dsu.Merge(edges[curR].first - 1, edges[curR].second - 1);  
                    curR++;  
                }  
                int tempOps = 0;  
                int endTemp = min(qu.r + 1, base);  
                for (int i = qu.l; i < endTemp; i++){  
                    dsu.Merge(edges[i].first - 1, edges[i].second - 1);  
                    tempOps++;  
                }  
                ans[qu.id] = dsu.cnt;  
                dsu.rollback(tempOps);  
            }  
        }  
        for (int i = 0; i < q; i++){  
            cout << ans[i] << "\n";  
        }  
    }  
}
```