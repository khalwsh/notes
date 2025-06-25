Difference constraints is a system of linear inequalities of form xj - xi <= wij
let's format the equation
xi + wij >= xj

real life example : schedule tasks such that each task has duration and some tasks must be finished before some other tasks , xj >= xi + duration(i).

so let's create graph , where there is an edge from xi to xj with weight wij

Long story short : after creating the graph , if there is negative cycle there is no solution , else you have solution , set any variable to 0 then get shortest path from it this get a minimal solution , if you want the maximum solution multiply the minimal solution by -1

min - solution
```cpp
struct DiffConstraintsMin {
    vector<array<ll,3>> constraints;  // {u, v, C} for X_u - X_v >= C
    int num_vars = 0;

    void setNumVariables(int n) {
        num_vars = n;
    }

    void addConstraint(int u, int v, ll C) {
        constraints.push_back({u, v, C});
    }

    bool isSolvableMin() {
        int n = num_vars;
        vector<ll> dist(n, 0);
        int m = constraints.size();
        for (int i = 0; i < n; ++i) {
            bool updated = false;
            for (int j = 0; j < m; ++j) {
                int u = constraints[j][0];
                int v = constraints[j][1];
                ll C  = constraints[j][2];
                if (dist[u] < dist[v] + C) {
                    dist[u] = dist[v] + C;
                    updated = true;
                }
            }
            if (!updated) return true;
            if (i == n - 1) return false;
        }
        return true;
    }

    vector<ll> getMinValues() const {
        int n = num_vars;
        vector<ll> dist(n, 0);
        int m = constraints.size();
        for (int i = 0; i < n - 1; ++i) {
            for (int j = 0; j < m; ++j) {
                int u = constraints[j][0];
                int v = constraints[j][1];
                ll C  = constraints[j][2];
                dist[u] = max(dist[u], dist[v] + C);
            }
        }
        return dist;
    }
};
```

max - solution
```cpp
struct DiffConstraintsMax {
    std::vector<std::array<int,3>> constraints;  // {u, v, C} for X_u - X_v >= C
    int num_vars = 0;

    void setNumVariables(int n) {
        num_vars = n;
    }

    void addConstraint(int u, int v, ll C) {
        constraints.push_back({u, v, static_cast<int>(C)});
    }

    // Check feasibility (no negative cycle) by transforming variables Y = -X
    bool isSolvable() const {
        std::vector<ll> dist(num_vars, 0);
        int m = constraints.size();
        for (int i = 0; i < num_vars; ++i) {
            bool updated = false;
            for (int j = 0; j < m; ++j) {
                int u = constraints[j][0];
                int v = constraints[j][1];
                ll C  = constraints[j][2];
                if (dist[v] < dist[u] + C) {
                    dist[v] = dist[u] + C;
                    updated = true;
                }
            }
            if (!updated) return true;
            if (i == num_vars - 1) return false;
        }
        return true;
    }

    vector<ll> getMaxValues() const {
        std::vector<ll> dist(num_vars, 0);
        int m = constraints.size();
        for (int i = 0; i < num_vars - 1; ++i) {
            for (int j = 0; j < m; ++j) {
                int u = constraints[j][0];
                int v = constraints[j][1];
                ll C  = constraints[j][2];
                dist[v] = max(dist[v], dist[u] + C);
            }
        }
        vector<ll> result(num_vars);
        for (int i = 0; i < num_vars; ++i) result[i] = -dist[i];
        return result;
    }
};
```

what if we want to add equality?
we can add constraints that xi - xj >= wij and in same time xj - xi >= -wij

---
problems
1 - https://atcoder.jp/contests/abc404/tasks/abc404_g
```cpp
#include <bits/stdc++.h>
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
struct DiffConstraints {
    vector<array<ll,3>> constraints;  // {u, v, C} for X_u - X_v >= C
    int num_vars = 0;

    void setNumVariables(int n) {
        num_vars = n;
    }

    void addConstraint(int u, int v, ll C) {
        constraints.push_back({u, v, C});
    }

    bool isSolvableMin() {
        int n = num_vars;
        vector<ll> dist(n, 0);
        int m = constraints.size();
        for (int i = 0; i < n; ++i) {
            bool updated = false;
            for (int j = 0; j < m; ++j) {
                int u = constraints[j][0];
                int v = constraints[j][1];
                ll C  = constraints[j][2];
                if (dist[u] < dist[v] + C) {
                    dist[u] = dist[v] + C;
                    updated = true;
                }
            }
            if (!updated) return true;
            if (i == n - 1) return false;
        }
        return true;
    }

    vector<ll> getMinValues() const {
        int n = num_vars;
        vector<ll> dist(n, 0);
        int m = constraints.size();
        for (int i = 0; i < n - 1; ++i) {
            for (int j = 0; j < m; ++j) {
                int u = constraints[j][0];
                int v = constraints[j][1];
                ll C  = constraints[j][2];
                dist[u] = max(dist[u], dist[v] + C);
            }
        }
        return dist;
    }
};

int l[4005], r[4005];
ll s[4005];

int main() {
    PRE();
    int n, m;
    cin >> n >> m;

    for (int i = 0; i < m; i++) {
        cin >> l[i] >> r[i] >> s[i];
        --l[i];
    }

    DiffConstraints bellman;
    bellman.setNumVariables(n + 1);

    for (int i = 0; i < m; i++) {
        bellman.addConstraint(r[i], l[i], s[i]);
        bellman.addConstraint(l[i], r[i], -s[i]);
    }
    for (int j = 1; j <= n; j++) {
        bellman.addConstraint(j, j - 1, 1);
    }

    if (!bellman.isSolvableMin()) {
        cout << -1 << "\n";
        return 0;
    }
    cout << bellman.getMinValues()[n] << "\n";
}

```