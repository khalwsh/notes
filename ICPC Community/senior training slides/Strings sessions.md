

### ORDER

- Hashing
	- rolling hash [done]
		- number basis
		- how to represent a number in base **k**
		- sliding window of robin-karp
		- hash and reverse hash
	- poly hash    [done]     
		- what is polynomial hash
	- double poly hash           [done]
		- 2 different bases , mods
	- kth hash          [done]
		- k different bases , mods
	- range hash (L , R)  [done]
		- compare substrings
	- dp with hashing [done]
		- using hashing in dp transition
	- dynamic range hashing (L , R) values changes [done]
		- updates in the string while maintaining hash and reverse hash
	- grid hash [done]
		- template (I am sorry)
	- Permutation hashing , XOR hash , set hash [done]
		- Permutation hashing is just you want to hash something but order matters(poly hash)
		- set hash ---> just assign random and query sum or add const and raise to power taking mod after ? (interesting)
		- XOR hash ---> just assign random values and xor them to get hash where it is helpful? when the queries ask about parity of existing objects

- KMP
	- failure function [done]
	- KMP and pattern search [done]
	- problems variations
		- longest Prefix/suffix Palindrome  [done]
			- what about using S@S and build failure function what do you observe?
		- add minimal number of characters to make the string palindrome [done]
			- answer = length(S) - answer of previous problem
			- or make P = SS then match (P , S) , your answer is second match
		- Minimum Repetition Cycle  [done]
			- build the failure and observe what failure[n - 1] represent
		- Prefixes Frequency in a string [done]
			- brute force --> for each prefix try to match it with the whole string o(n ^ 2)
			- For each prefix in S , it will be suffix for another prefix , so if for each prefix we counted these suffixes we got our answer
		- Unique Prefixes in a string that exist one time in the string [done]
			- get frequency of all prefixes and count how many cell with value 1
		- Count # of distinct sub-strings [done]
			- Let's solve it using incremental thinking suppose we know the answer to the string consists of \(n\) letters and we want to add extra one how this effect the answer , simply it should add to the answer how many unique suffixes in the new string after the character added.  this can be done by reverse it and get the unique prefixes using the previous method
	- dp with KMP  [done]
		- the general idea you have a string and want to count some string has another string as substring , so it is just dp_i_j ---> represent which idx and how many matched but the idea when you fail to match next j how to know it ? use failure function 




#### problems 
- https://cses.fi/problemset/task/1753/
- https://vjudge.net/problem/UVA-11475
- https://cses.fi/problemset/task/1732/
- https://codeforces.com/gym/101466/problem/E
- https://codeforces.com/contest/633/problem/C
- https://cses.fi/problemset/task/2420/
- https://vjudge.net/problem/UVA-11019
- [CCC '20 S3 - Searching for Strings - DMOJ: Modern Online Judge](https://dmoj.ca/problem/ccc20s3)
- [Problem - B - Codeforces](https://codeforces.com/gym/101808/problem/B)
- [Problem - H - Codeforces](https://codeforces.com/contest/2014/problem/H)
- -----------------------
- [SPOJ.com - Problem EPALIN](https://www.spoj.com/problems/EPALIN/)
- [Online Judge](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=396)
- [Problem - D - Codeforces](https://codeforces.com/contest/432/problem/D)
- [CSES - Required Substring](https://cses.fi/problemset/task/1112)
- [Problem - G - Codeforces](https://codeforces.com/contest/808/problem/G)


### rolling hash
- recall sliding window sum problem
- revision on number bases
	- base k has k digits
		- we can represent any base
		- base b
			- X = Tn * b ^ n  + .... + T2 * b ^ 2 + T1 * b ^ 1 + T0 * b ^ 0

p1: [CSES - String Matching](https://cses.fi/problemset/task/1753/)
```cpp
const int mod = 1e9 + 9;  
const int base = 97;  
const int N = 1e6 + 10;  
ll powers[N];  
ll cur_hash = 0 , pattern_hash = 0;  
int main() {  
    PRE();  
    powers[0] = 1;  
    for(int i = 1;i < N;i++)powers[i] = base * powers[i - 1] % mod;  
    string s , t;cin>>s>>t;  
    if(t.size() > s.size()) {  
        cout<<0<<'\n';  
        exit(0);  
    }  
    for(int i = 0;i < t.size();i++) {  
        pattern_hash = (pattern_hash * base + t[i]) % mod;  
        cur_hash = (cur_hash * base + s[i]) % mod;  
    }  
    int res = 0;  
    for(int i = t.size();i < s.size();i++) {  
        if(cur_hash == pattern_hash)res++;  
        cur_hash -= powers[t.size() - 1] * s[i - t.size()];  
        cur_hash = (cur_hash % mod + mod) % mod;  
        cur_hash = (cur_hash * base + s[i]) % mod;  
    }  
    if(cur_hash == pattern_hash)res++;  
    cout<<res<<'\n';  
}
```

p2: [Extend to Palindrome - UVA 11475 - Virtual Judge](https://vjudge.net/problem/UVA-11475)
```cpp
const int mod = 1e9 + 9;
const int base = 97;
const int N = 1e6 + 10;
ll powers[N];
ll cur_hash = 0 , pattern_hash = 0;
int main() {
    PRE();
    powers[0] = 1;
    for(int i = 1;i < N;i++)powers[i] = base * powers[i - 1] % mod;
    string s;
    while(cin>>s) {
        ll cur_hash = 0;
        ll reverse_hash = 0;
        int idx = -1;
        for(int i = s.size() - 1;i >= 0;i--) {
            cur_hash = (cur_hash * base + s[i]) % mod;
            reverse_hash = (reverse_hash + s[i] * powers[s.size() - i - 1]) % mod;
            if(cur_hash == reverse_hash) {
                idx = i;
            }
        }
        string extra = s.substr(0 , idx);
        reverse(extra.begin() , extra.end());
        cout<<s + extra<<'\n';
    }
}
```


intro to 2d hash
3- [CSES - Finding Borders](https://cses.fi/problemset/task/1732/)

```cpp
const int mod1 = 1e9 + 7 , mod2 = 1e9 + 91;  
const int base1 = 253 , base2 = 99;  
const int N = 1e6 + 10;  
ll powers1[N] , powers2[N];  
  
int main() {  
    PRE();  
    powers1[0] = 1;  
    powers2[0] = 1;  
    for(int i = 1;i < N;i++)powers1[i] = base1 * powers1[i - 1] % mod1;  
    for(int i = 1;i < N;i++)powers2[i] = base2 * powers2[i - 1] % mod2;  
  
    string s;  
    cin>>s;  
    ll cur_hash1 = 0 , cur_hash2 = 0;  
    ll reverse_hash1 = 0 , reverse_hash2 = 0;  
    unordered_map<ll , ll>hashes;  
    for(int i = s.size() - 1;i >= 0;i--) {  
        reverse_hash1 = (reverse_hash1 + s[i] * powers1[s.size() - i - 1]) % mod1;  
        reverse_hash2 = (reverse_hash2 + s[i] * powers2[s.size() - i - 1]) % mod2;  
        hashes[reverse_hash1] = reverse_hash2;  
    }  
    for(int i = 0;i < s.size();i++) {  
        cur_hash1 = (cur_hash1 * base1 + s[i]) % mod1;  
        cur_hash2 = (cur_hash2 * base2 + s[i]) % mod2;  
        if(hashes.count(cur_hash1) && hashes[cur_hash1] == cur_hash2 && i + 1 != s.size())cout<<i + 1<<" ";  
    }  
}
```

intro to k-th hash
[Problem - E - Codeforces](https://codeforces.com/gym/101466/problem/E)

```cpp
string s, t;  
int n;  
const int k = 8;  
const int bases[k] = {97, 89, 83, 79, 71, 67, 61, 53};  
const int mods[k] = {1000000007, 1000000009, 1000000021, 1000000033, 1000000087, 1000000093, 1000000097, 1000000103};  
const int N = 1e5 + 10;  
ll p[k][N];  
  
bool can(int x) {  
    vector<ll> cur_hash(k, 0), target_hash(k, 0);  
  
    for (int i = 0; i < x; i++) {  
        for (int j = 0; j < k; j++) {  
            target_hash[j] = (target_hash[j] * bases[j] + t[i]) % mods[j];  
            cur_hash[j] = (cur_hash[j] * bases[j] + s[i]) % mods[j];  
        }  
    }  
  
    int res = 0;  
    for (int i = x; i < s.size(); i++) {  
        if (cur_hash == target_hash) res++;  
  
        for (int j = 0; j < k; j++) {  
            cur_hash[j] = (cur_hash[j] - s[i - x] * p[j][x - 1] % mods[j] + mods[j]) % mods[j];  
            cur_hash[j] = (cur_hash[j] * bases[j] + s[i]) % mods[j];  
        }  
    }  
  
    if (cur_hash == target_hash) res++;  
    return res >= n;  
}  
  
int main() {  
    PRE();  
    getline(cin, s);  
    getline(cin, t);  
    cin >> n;  
  
    for (int j = 0; j < k; j++) {  
        p[j][0] = 1;  
        for (int i = 1; i < N; i++) {  
            p[j][i] = p[j][i - 1] * bases[j] % mods[j];  
        }  
    }  
  
    int left = 1, right = min(s.size(), t.size());  
    int res = 0;  
  
    while (left <= right) {  
        int mid = left + (right - left) / 2;  
        if (can(mid)) {  
            left = mid + 1;  
            res = mid;  
        } else {  
            right = mid - 1;  
        }  
    }  
  
    if (res == 0) cout << "IMPOSSIBLE";  
    else cout << t.substr(0, res);
```

intro to dp with hashing and khalwsh mod

[Problem - C - Codeforces](https://codeforces.com/contest/633/problem/C)
correct code but hashing fail
```cpp
int n , m;  
string s;  
bool dp[N];  
int parent[N];  
   
ll pref[N] , p[N] , inv[N];  
ll get_hash(int l, int r) {  
    if (l == 0) return pref[r];  
    return ((pref[r] - pref[l - 1] * p[r - l + 1]) % mod + mod) % mod;  
}  
   
int main() {  
    PRE();  
    cin>>n>>s>>m;  
    p[0] = 1;  
    inv[0] = 1;  
    for(int i = 1;i < N;i++) {  
        p[i] = p[i - 1] * base % mod;  
        inv[i] = binpow(p[i] , mod - 2 , mod);  
    }  
    pref[0] = s[0];  
    for (int i = 1; i < n; i++) {  
        pref[i] = (pref[i - 1] * base + s[i]) % mod;  
        // s[0] * base ^ (s_len) + .... + s.back();  
    }  
   
    unordered_set<ll>hashes;  
    map<ll , string>mapping_hashes;  
    for(int i = 0;i < m;i++) {  
        string temp;cin>>temp;  
        string temp_before = temp;  
        for(auto &val:temp)val = tolower(val);  
        ll cur_hash = 0;  
        int j = 0;  
        for(auto &val:temp) {  
            cur_hash = (cur_hash + p[j++] * val) % mod;  
        }  
        // each string represeted as  
        // temp[0] * base ^ (0) + temp[1] * base ^ (1) + .... + temp.back() * base ^ (temp_len - 1)        hashes.insert(cur_hash);  
        mapping_hashes[cur_hash] = temp_before;  
    }  
    dp[0] = true;  
    for(int i = 1;i <= n;i++) {  
        for(int j = 1;j <= i;j++) {  
            if(dp[j - 1] && hashes.count(get_hash(j - 1, i - 1))) {  
                dp[i] = true;  
                parent[i] = j;  
                break;  
            }  
        }  
    }  
    assert(dp[n]);  
    int cur = n;  
    vector<string>ans;  
    while(parent[cur] != 0) {  
        int prev = parent[cur];  
        ans.emplace_back(mapping_hashes[get_hash(prev - 1 , cur - 1)]);  
        cur = prev - 1;  
    }  
    reverse(ans.begin() , ans.end());  
    for(auto &val:ans)  
        cout<<val<<" ";  
}
```

correct code with big mod
```cpp
const int N = 1e4 + 1;  
const uint64_t mod = (1ULL << 61) - 1;  
const uint64_t seed = chrono::system_clock::now().time_since_epoch().count();  
const uint64_t base = mt19937_64(seed)() % (mod / 3) + (mod / 3);  
  
uint64_t MUL(uint64_t a, uint64_t b) {  
    uint64_t l1 = (uint32_t) a, h1 = a >> 32, l2 = (uint32_t) b, h2 = b >> 32;  
    uint64_t l = l1 * l2, m = l1 * h2 + l2 * h1, h = h1 * h2;  
    uint64_t ret = (l & mod) + (l >> 61) + (h << 3) + (m >> 29) + (m << 35 >> 3) + 1;  
    ret = (ret & mod) + (ret >> 61);  
    ret = (ret & mod) + (ret >> 61);  
    return ret - 1;  
}  
  
int n, m;  
string s;  
bool dp[N];  
int parent[N];  
  
uint64_t pref[N], p[N];  
  
uint64_t get_hash(int l, int r) {  
    if (l == 0) return pref[r];  
    return ((pref[r] - MUL(pref[l - 1], p[r - l + 1])) + mod) % mod;  
}  
  
int main() {  
    PRE();  
    cin >> n >> s >> m;  
  
    p[0] = 1;  
    for (int i = 1; i < N; i++) {  
        p[i] = MUL(p[i - 1], base);  
    }  
  
    pref[0] = s[0];  
    for (int i = 1; i < n; i++) {  
        pref[i] = (MUL(pref[i - 1], base) + s[i]) % mod;  
    }  
  
    unordered_set<uint64_t> hashes;  
    map<uint64_t, string> mapping_hashes;  
  
    for (int i = 0; i < m; i++) {  
        string temp;  
        cin >> temp;  
        string temp_before = temp;  
  
        for (auto &val : temp) val = tolower(val);  
  
        uint64_t cur_hash = 0;  
        for (int j = 0; j < temp.size(); j++) {  
            cur_hash = (cur_hash + MUL(p[j], temp[j])) % mod;  
        }  
  
        hashes.insert(cur_hash);  
        mapping_hashes[cur_hash] = temp_before;  
    }  
  
    dp[0] = true;  
    for (int i = 1; i <= n; i++) {  
        for (int j = 1; j <= i; j++) {  
            if (dp[j - 1] && hashes.count(get_hash(j - 1, i - 1))) {  
                dp[i] = true;  
                parent[i] = j - 1;  
                break;  
            }  
        }  
    }  
  
    assert(dp[n]);  
  
    int cur = n;  
    vector<string> ans;  
    while (cur > 0) {  
        int prev = parent[cur];  
  
        uint64_t word_hash = get_hash(prev, cur - 1);  
        if (mapping_hashes.count(word_hash)) {  
            ans.emplace_back(mapping_hashes[word_hash]);  
        }  
  
        cur = prev;  
    }  
  
    reverse(ans.begin(), ans.end());  
  
    for (auto &val : ans) {  
        cout << val << " ";  
    }  
}
```

dynamic hashing
[CSES - Palindrome Queries](https://cses.fi/problemset/task/2420/)
```cpp
  
const ll HASH = 257, MOD = 1e9 + 7;  
  
const int md = 1e9+7;  
int B = 73; //Sheldon prime (lol)  
  
const int mxN = 1e6+6;  
ll pB[mxN];  
struct {  
    ll bit[mxN] = {0};  
  
    void update(int k, ll x) {  
        x %= md;  
        if (x < md) x += md;  
        for (; k < mxN; k += k&-k) {  
            (bit[k] += x) %= md;  
        }  
    }  
  
    ll query(int k) {  
        ll s = 0;  
        for (; k > 0; k -= k&-k)  
            (s += bit[k]) %= md;  
        return s;  
    }  
} pre, suf;  
  
int main() {  
    PRE();  
    mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());  
    B = uniform_int_distribution<int>(73, 7337)(rng);  
    int n, t; cin>>n>>t;  
  
    string s; cin>>s;  
    pB[0] = 1;  
    for (int i = 1; i < mxN; i++)  
        pB[i] = pB[i-1] * B % md;  
  
    for (int i = 0; i < n; i++) {  
        pre.update(i+1, (s[i]) * pB[i]);  
        suf.update(i+1, (s[i]) * pB[n-i-1]);  
    }  
    while (t--) {  
        int ch; cin>>ch;  
        if (ch == 1) {  
            int k; char x;  
            cin>>k>>x; k--;  
            pre.update(k+1, -(s[k]-'a'+1) * pB[k]);  
            pre.update(k+1, (x-'a'+1) * pB[k]);  
            suf.update(k+1, -(s[k]-'a'+1) * pB[n-k-1]);  
            suf.update(k+1, (x-'a'+1) * pB[n-k-1]);  
            s[k] = x;  
        }  
        else {  
            int x, y; cin>>x>>y;  
            ll h1 = (pre.query(y) - pre.query(x - 1) + md) % md;  
            ll m1 = pB[x-1];  
            ll h2 = (suf.query(y) - suf.query(x - 1) + md) % md;  
            ll m2 = pB[n-y];  
            cout << (h1 * m2 % md == h2 * m1 % md ? "YES" : "NO" ) << '\n';  
        }  
    }  
}
```

[CCC '20 S3 - Searching for Strings - DMOJ: Modern Online Judge](https://dmoj.ca/problem/ccc20s3)

do set hash and when match store premutation hash


https://codeforces.com/gym/101808/problem/B

```cpp
const ll k = 3453463456723;  
const ll H = 231;  
const int mod = 1e9 + 253;  
  
long long binpow(long long a, long long b, long long m) {  
    a %= m;  
    long long res = 1;  
    while (b > 0) {  
        if (b & 1)  
            res = res * a % m;  
        a = a * a % m;  
        b >>= 1;  
    }  
    return res;  
}  
long long randomLongLong(long long l, long long r) {  
    random_device rd;  
    mt19937_64 rng(rd());  
    uniform_int_distribution<long long> dist(l, r);  
    return dist(rng);  
}  
  
int main() {  
    PRE();  
    int t;cin>>t;  
    while(t--) {  
        int n;cin>>n;  
        vector<ll>Rand(2 * n + 1);  
        for(int i = 1;i <= 2 * n;i++) {  
            Rand[i] = randomLongLong(0 , mod - 1);  
        }  
        vector<ll>hashes1 , hashes2;  
        for(int i = 0;i < n;i++) {  
            int u , v;cin>>u>>v;  
            if(u > v)swap(u , v);  
            hashes1.emplace_back(binpow(Rand[u] + k , H , mod));  
            hashes2.emplace_back(binpow(Rand[v] + k , H , mod));  
        }  
        map<pair<ll , ll> , int>freq;  
        for(int i = 0;i < n;i++) {  
            ll cur_hash1 = 0 , cur_hash2 = 0;  
            for(int j = i;j < n;j++) {  
                cur_hash1 += hashes1[j];  
                cur_hash1 %= mod;  
                cur_hash2 += hashes2[j];  
                cur_hash2 %= mod;  
                freq[{cur_hash1 , cur_hash2}]++;  
            }  
        }  
        ll res = 0;  
        for(auto &val:freq)res += 1LL * val.second * (val.second - 1) / 2;  
        cout<<res<<'\n';  
    }  
}
```


### KMP

What is prefix function ? it is the longest proper prefix that is also a suffix (eg: abcdabcd) it's prefix function has value 4.

in order to use KMP in pattern matching we need to build this prefix function for each prefix of the pattern

we need to build pi[i] --> as the value of the prefix function for the substring (0 , .1 , ... , i)
first observation pi[i + 1] <= pi[i] + 1

so another naive approach will be for building pi[i + 1] just checking from pi[i] + 1 to 0 to see which length matches. so this is o(n ^ 2) , stop right there can we make this fast with hashing? oh yes.

but why all what we want is calculate pi[i + 1] so we already assumed that pi[i] is calculated so we if we just failed to extend it then we fail to it's value because we know pi[i] --> longest suffix is a prefix of the substring (0 , 1 , .... , i)
and keep doing so until we find a match.

```cpp
int n ;string s;cin>>n>>s;  
vector<int>failure(n);  
for(int i = 1;i < n;i++) {  
    int j = failure[i - 1];  
    while(j > 0 && s[i] != s[j])  
        j = failure[j - 1];  
    if(s[i] == s[j])j++;  
    failure[i] = j;  
}
```


[CSES - String Matching](https://cses.fi/problemset/task/1753)
```cpp
string text , pat;  
cin>>text>>pat;  
int n = text.size();  
int m = pat.size();  
  
vector<int>failure(m);  
for(int i = 1;i < m;i++) {  
    int len = failure[i - 1];  
    while(len > 0 && pat[len] != pat[i]) {  
        len = failure[len - 1];  
    }  
    if(pat[len] == pat[i])len++;  
    failure[i] = len;  
}  
vector<int>matches;  
int len = 0;  
for(int i = 0;i < n;i++) {  
    while(len > 0 && text[i] != pat[len]) {  
        len = failure[len - 1];  
    }  
    if(text[i] == pat[len])len++;  
    if(len == m) {  
        matches.emplace_back(i - m + 1);  
        len = failure[len - 1];  
    }  
}  
cout<<matches.size()<<'\n';
```

[Online Judge](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=396)
```cpp
int t;cin>>t;  
while(t--) {  
    string s;cin>>s;  
    int n = s.size();  
    vector<int>failure(n);  
    for(int i = 1;i < s.size();i++) {  
        int j = failure[i - 1];  
        while(j > 0 && s[i] != s[j]) j = failure[j - 1];  
        if(s[i] == s[j])j++;  
        failure[i] = j;  
    }  
    if(n % (n - failure[n - 1]) == 0)  
        cout<<n - failure[n - 1]<<'\n';  
    else  
        cout<<n<<'\n';  
  
    if(t)cout<<'\n';  
}
```

```cpp
vector<int> FreqAllPrefixes(const string &s) {  
    vector<int>failure;  // build the failure
    vector<int> freq(s.size() + 1);  
    for (int i = 0; i < s.size(); i++) {  
        ++freq[failure[i]];  
    }  
    for (int i = (int) s.size() - 1; i > 0; i--) {  
        freq[failure[i - 1]] += freq[i];  
    }  
    freq.erase(freq.begin());  
    for (auto &val: freq)val++;  
    return freq;  
}
```

```cpp
CountUniquePrefixes(const string &s){  
    vector<int> temp = FreqAllPrefixes(s);  
    return count(temp.begin() , temp.end() , 1);  
}  
  
int UniqueSubStrings(const string &s){  
    string cur;  
    int n = (int)s.size();  
    int res = 0;  
    for(int i = 0;i<n;i++){  
        cur += s[i];  
        string tt = cur;  
        reverse(tt.begin() , tt.end());  
        res += CountUniquePrefixes(tt);  
    }  
    return res;  
}
```

https://cses.fi/problemset/task/1112
```cpp
int n;  
string s;  
int dp[1002][102];  
const int mod = 1e9 + 7;  
  
long long binpow(long long a, long long b, long long m) {  
    a %= m;  
    long long res = 1;  
    while (b > 0) {  
        if (b & 1)  
            res = res * a % m;  
        a = a * a % m;  
        b >>= 1;  
    }  
    return res;  
}  
  
int main() {  
    PRE();  
    cin >> n >> s;  
    vector<int> failure(s.size(), 0);  
  
    for (int j = 1; j < s.size(); j++) {  
        int i = failure[j - 1];  
        while (i > 0 && s[i] != s[j])  
            i = failure[i - 1];  
        if (s[i] == s[j])  
            i++;  
        failure[j] = i;  
    }  
  
    int m = s.size();  
    memset(dp, 0, sizeof(dp));  
    dp[0][0] = 1;  
    // dp[i][j] : reprsent how many way you can create string of length i matching j-characters from the suffix but don't contain string pattern  
  
    for (int i = 0; i < n; i++) {  
        for (int j = 0; j < m; j++) {  
            for (char c = 'A'; c <= 'Z'; c++) {  
                int nxt = j;  
                while (nxt > 0 && s[nxt] != c)  
                    nxt = failure[nxt - 1];  
                if (s[nxt] == c)  
                    nxt++;  
                dp[i + 1][nxt] = (dp[i + 1][nxt] + dp[i][j]) % mod;  
            }  
        }  
    }  
  
    ll res = 0;  
    for (int i = 0; i < m; i++) {  
        res = (res + dp[n][i]) % mod;  
    }  
  
    ll total = binpow(26, n, mod);  
    ll answer = (total - res + mod) % mod;  
  
    cout << answer << '\n';
```