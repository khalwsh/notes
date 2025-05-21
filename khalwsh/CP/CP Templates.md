>[!SUMMARY] Table of Contents
>table-of-contents
>
# Math

### CRT
```cpp
class ChineseRemainderTheorem {
    typedef long long vlong;
    typedef pair<vlong,vlong> pll;

    /** CRT Equations stored as pairs of vector. See addEqation()*/
    vector<pll> equations;

public:
    void clear() {
        equations.clear();
    }
    vlong ext_gcd(vlong a, vlong b, vlong& x, vlong& y) {
        if (b == 0) {
            x = 1;
            y = 0;
            return a;
        }
        vlong x1, y1;
        vlong d = ext_gcd(b, a % b, x1, y1);
        x = y1;
        y = x1 - y1 * (a / b);
        return d;
    }
    /** Add equation of the form x = r (mod m)*/
    void addEquation( vlong r, vlong m ) {
        equations.push_back({r, m});
    }
    pll solve() {
        if (equations.size() == 0) return {-1,-1}; /// No equations to solve

        vlong a1 = equations[0].first;
        vlong m1 = equations[0].second;
        a1 %= m1;
        /** Initially x = a_0 (mod m_0)*/

        /** Merge the solution with remaining equations */
        for ( int i = 1; i < equations.size(); i++ ) {
            vlong a2 = equations[i].first;
            vlong m2 = equations[i].second;

            vlong g = __gcd(m1, m2);
            if ( a1 % g != a2 % g ) return {-1,-1}; /// Conflict in equations

            /** Merge the two equations*/
            vlong p, q;
            ext_gcd(m1/g, m2/g, p, q);

            vlong mod = m1 / g * m2;
            vlong x = ( (__int128)a1 * (m2/g) % mod *q % mod + (__int128)a2 * (m1/g) % mod * p % mod ) % mod;

            /** Merged equation*/
            a1 = x;
            if ( a1 < 0 ) a1 += mod;
            m1 = mod;
        }
        return {a1, m1};
    }
};
```
### Extended Eculdien algorithm
```cpp
int extended_euclidean(int a,int b,int &x,int &y)
{
    if(a<0)
    {
        int r= extended_euclidean(-a,b,x,y);
        x*=-1;
        return r;
    }
    if(b<0)
    {
        int r= extended_euclidean(a,-b,x,y);
        y*=-1;
        return r;
    }
    if(b==0)
    {
        x=1,y=0;
        return a;
    }
    int sol= extended_euclidean(b,a%b,y,x);
    y-=(a/b)*x;
    return sol;
}
```

###  Euler Totient function 
```cpp
int phi(int n) {
    int result = n;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            while (n % i == 0)
                n /= i;
            result -= result / i;
        }
    }
    if (n > 1)
        result -= result / n;
    return result;
}  //single number
void phi_1_to_n(int n) {
    vector<int> phi(n + 1);
    for (int i = 0; i <= n; i++)
        phi[i] = i;

    for (int i = 2; i <= n; i++) {
        if (phi[i] == i) {
            for (int j = i; j <= n; j += i)
                phi[j] -= phi[j] / i;
        }
    }
}    //range [1,n]

//a^n congerns a^(a%phi(m))  under %m
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Fast power
```cpp
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

```

### Fermat prime
```cpp
bool probablyPrimeFermat(int n, int iter=100) {
    if (n < 4)
        return n == 2 || n == 3;

    for (int i = 0; i < iter; i++) {
        int a = 2 + rand() % (n - 3);
        if (binpower(a, n - 1, n) != 1)
            return false;
    }
    return true;
}
```

### Gcd Negative
```cpp
int gcd(int a, int b, int& x, int& y) {
    x = 1, y = 0;
    int x1 = 0, y1 = 1, a1 = a, b1 = b;
    while (b1) {
        int q = a1 / b1;
        tie(x, x1) = make_tuple(x1, x - q * x1);
        tie(y, y1) = make_tuple(y1, y - q * y1);
        tie(a1, b1) = make_tuple(b1, a1 - q * b1);
    }
    return a1;
}
```
<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Linear diophantian equation
```cpp
int gcd_euldien_algo(int a,int b,int &x,int &y)
{
    if(a<0)
    {
        int r= gcd_euldien_algo(-a,b,x,y);
        x*=-1;
        return r;
    }
    if(b<0)
    {
        int r= gcd_euldien_algo(a,-b,x,y);
        y*=-1;
        return r;
    }
    if(b==0)
    {
        x=1,y=0;
        return a;
    }
    int sol= gcd_euldien_algo(b,a%b,y,x);
    y-=(a/b)*x;
    return sol;
}
bool find_any_solution(int a, int b, int c, int &x0, int &y0, int &g) {
    g = gcd_euldien_algo(abs(a), abs(b), x0, y0);
    if (c % g) {
        return false;
    }

    x0 *= c / g;
    y0 *= c / g;
    if (a < 0) x0 = -x0;
    if (b < 0) y0 = -y0;
    g = abs(g);
    return true;                             // a*x + b*y = c     //__gcd(a,b)|c
}
// x0 contain the x cofficent
// y0 contain the y cofficent
// g  contain the gcd


// to get more solutions we can x0 = x0 +k*b/g
//                              y0 = y0 -k*a/g
```
<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>
### Linear sieve
```cpp
int Lpf[N];
vector<int>LinearSieve(){
    for(int i=1;i<N;i++)Lpf[i]=i;
    vector<int>primes;
    for(int i=2 ;i<N;i++){
        if(Lpf[i]==i){
            primes.emplace_back(i);
        }
        for(auto &val:primes){
            if(i*val>=N)break;
            Lpf[i*val] = val;
        }
    }
    return primes;
}
```

### Mod inverse gcd
```cpp
int gcd_euldien_algo(int a,int b,int &x,int &y)
{
    if(a<0)
    {
        int r= gcd_euldien_algo(-a,b,x,y);
        x*=-1;
        return r;
    }
    if(b<0)
    {
        int r= gcd_euldien_algo(a,-b,x,y);
        y*=-1;
        return r;
    }
    if(b==0)
    {
        x=1,y=0;
        return a;
    }
    int sol= gcd_euldien_algo(b,a%b,y,x);
    y-=(a/b)*x;
    return sol;
}
int ModInverse(int a,int m){
    int x,y;
    int g = gcd_euldien_algo(a,m,x,y);
    return (g==1?(x%m+m)%m:-inf);
//return -inf if there is no mod inverse
}
```

### Mod inverse for Array in o(n)
```cpp
int extended_euclidean(int a,int b,int &x,int &y)
{
    if(a<0)
    {
        int r= extended_euclidean(-a,b,x,y);
        x*=-1;
        return r;
    }
    if(b<0)
    {
        int r= extended_euclidean(a,-b,x,y);
        y*=-1;
        return r;
    }
    if(b==0)
    {
        x=1,y=0;
        return a;
    }
    int sol= extended_euclidean(b,a%b,y,x);
    y-=(a/b)*x;
    return sol;
}
vector<int> invs(const std::vector<int> &a, int m) {
    int n = (int)a.size();
    if (n == 0) return {};
    std::vector<int> b(n);
    int v = 1;
    for (int i = 0; i != n; ++i) {
        b[i] = v;
        v = static_cast<long long>(v) * a[i] % m;
    }
    int x, y;
    extended_euclidean(v, m, x, y);
    x = (x % m + m) % m;
    for (int i = n - 1; i >= 0; --i) {
        b[i] = static_cast<long long>(x) * b[i] % m;
        x = static_cast<long long>(x) * a[i] % m;
    }
    return b;
}
```

### Range mod inverse
```cpp
int inv[N];
void ModInverse(int m){
//calculating the mod inverse for range [1,m-1] under module mod
    inv[1] = 1;
    for(int a = 2; a < m; ++a)
        inv[a] = mod - (long long)(mod/a) * inv[mod%a] % mod;
}
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Shift Linear Diophantine solution
```cpp
int gcd(int a, int b, int& x, int& y) {
    if (b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int x1, y1;
    int d = gcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - y1 * (a / b);
    return d;
}
bool find_any_solution(int a, int b, int c, int &x0, int &y0, int &g) {
    g = gcd(abs(a), abs(b), x0, y0);
    if (c % g) {
        return false;
    }

    x0 *= c / g;
    y0 *= c / g;
    if (a < 0) x0 = -x0;
    if (b < 0) y0 = -y0;
    return true;                             // a*x + b*y = c     //__gcd(a,b)|c
}
void shift_solution(int & x, int & y, int a, int b, int cnt) {
    x += cnt * b;
    y -= cnt * a;
}
```

### polynomial diffrentation 
```cpp
void handle(string &temp,string &ans) {
    string cof;
    int ind = -1;
    for(int i=0;i<temp.size();i++){
        if(temp[i]=='x'){
            ind = i;break;
        }
        cof += temp[i];
    }
    if(ind==-1)return;
    string deg;
    for(int i = ind+1;i<temp.size();i++){
        deg += temp[i];
    }
    if(deg.empty() && cof.empty()){
        ans += '1';
        return;
    }
    if(deg.empty()){
        ans+=cof;
        return;
    }
    if(cof.empty()){
        ans += deg;
        if(deg=="1")return;
        ans+='x';
        if(deg!="2")ans+= to_string(stoll(deg) - 1);
        return;
    }
    ans += to_string(stoll(cof) * stoll(deg));
    if(deg=="1")return;
    ans+='x';
    if(deg!="2")ans+= to_string(stoll(deg) - 1);
}
string PolynomialDifferentiation(string &s){
    //the format of the Polynomial is axb-cxd+kxl  : where the powers is sorted decreasing and if the cof or power is 1 doesn't be written and if power is 0 the x doesn't exist
    string temp;
    if(count(s.begin(),s.end(),'x')==0){
        return "0";
    }
    string ans;
    for(auto &val:s){
        if(val=='-'||val=='+'){
            handle(temp,ans);
            ans+=val;
            temp.clear();
            continue;
        }
        temp+=val;
    }
    handle(temp,ans);
    while(ans.back() == '+' ||ans.back() == '-')ans.pop_back();
    return ans;
}
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### pollard rho
```cpp
#include <bits/stdc++.h>

using namespace std;

struct Mint {
  uint64_t n;
  static uint64_t mod, inv, r2;
  Mint() : n(0) { }
  Mint(const uint64_t &x) : n(init(x)) { }
  static uint64_t init(const uint64_t &w) { return reduce(__uint128_t(w) * r2); }
  static void set_mod(const uint64_t &m) {
    mod = inv = m;
    for(int i = 0; i < 5; i++)  inv *= 2 - inv * m;
    r2 = -__uint128_t(m) % m;
  }
  static uint64_t reduce(const __uint128_t &x) {
    uint64_t y = uint64_t(x >> 64) - uint64_t((__uint128_t(uint64_t(x) * inv) * mod) >> 64);
    return int64_t(y) < 0 ? y + mod : y;
  }
  Mint& operator+= (const Mint &x) { n += x.n - mod; if(int64_t(n) < 0) n += mod; return *this; }
  Mint& operator+ (const Mint &x) const { return Mint(*this) += x; }
  Mint& operator*= (const Mint &x) { n = reduce(__uint128_t(n) * x.n); return *this; }
  Mint& operator* (const Mint &x) const { return Mint(*this) *= x; }
  uint64_t val() const { return reduce(n); }
};

uint64_t Mint::mod, Mint::inv, Mint::r2;

bool suspect(const uint64_t &a, const uint64_t &s, uint64_t d, const uint64_t &n) {
  if(Mint::mod != n)  Mint::set_mod(n);
  Mint x(1), xx(a), one(x), minusone(n - 1);
  while(d > 0) {
    if(d & 1) x *= xx;
    xx *= xx;
    d >>= 1;
  }
  if(x.n == one.n)  return true;
  for(unsigned int r = 0; r < s; r++) {
    if(x.n == minusone.n) return true;
    x *= x;
  }
  return false;
}

bool is_prime(const uint64_t &n) {
  if(n <= 1 || (n > 2 && (~n & 1))) return false;
  uint64_t d = n - 1, s = 0;
  while(~d & 1) s++, d >>= 1;
  static const uint64_t a_big[] = {2, 325, 9375, 28178, 450775, 9780504, 1795265022};
  static const uint64_t a_smol[] = {2, 7, 61};
  if(n <= 4759123141LL) {
    for(auto &&p : a_smol) {
      if(p >= n)  break;
      if(!suspect(p, s, d, n))  return false;
    }
  } else {
    for(auto &&p : a_big) {
      if(p >= n)  break;
      if(!suspect(p, s, d, n))  return false;
    }
  }
  return true;
}

uint64_t rho_pollard(const uint64_t &n) {
  if(~n & 1)  return 2u;
  static random_device rng;
  uniform_int_distribution<uint64_t> rr(1, n - 1);
  if(Mint::mod != n)  Mint::set_mod(n);
  for(;;) {
    uint64_t c_ = rr(rng), g = 1, r = 1, m = 500;
    Mint y(rr(rng)), xx(0), c(c_), ys(0), q(1);
    while(g == 1) {
      xx.n = y.n;
      for(unsigned int i = 1; i <= r; i++)  y *= y, y += c;
      uint64_t k = 0; g = 1;
      while(k < r && g == 1) {
        for(unsigned int i = 1; i <= (m > (r - k) ? (r - k) : m); i++) {
          ys.n = y.n;
          y *= y; y += c;
          uint64_t xxx = xx.val(), yyy = y.val();
          q *= Mint(xxx > yyy ? xxx - yyy : yyy - xxx);
        }
        g = __gcd<uint64_t>(q.val(), n);
        k += m;
      }
      r *= 2;
    }
    if(g == n)  g = 1;
    while(g == 1) {
      ys *= ys; ys += c;
      uint64_t xxx = xx.val(), yyy = ys.val();
      g = __gcd<uint64_t>(xxx > yyy ? xxx - yyy : yyy - xxx, n);
    }
    if(g != n && is_prime(g)) return g;
  }
  assert(69 == 420);
}

template <typename T>
vector<T> factor(T n) {
  if(n < 2) return { };
  if(is_prime(n)) return {n};
  T d = rho_pollard(static_cast<uint64_t>(n));
  vector<T> l = factor(d), r = factor(n / d);
  l.insert(l.end(), r.begin(), r.end());
  return l;
}

//credit: olaf
int main() {
  int t; cin >> t;
  while(t--) {
    uint64_t n; cin >> n;
    vector<uint64_t> fac = factor(n);
    sort(fac.begin(), fac.end());
    cout << fac.size() << ' ';
    for(auto &&f : fac) {
      cout << f << ' ';
    }
    cout << '\n';
  }
}
// https://judge.yosupo.jp/problem/factorize
```


### Segmented sieve
```cpp
vector<int>Primes;//have the primes themself
vector<bool> segmentedSieve(int L, int R) {
    // generate all primes up to sqrt(R)
    int lim = (int)sqrt(R);
    vector<bool> mark(lim + 1, true);
    vector<int> primes;//have primes [2 , sqrt(R)]
    for (int i = 2; i <= lim; ++i) {
        if (mark[i]) {
            primes.emplace_back(i);
            for (int j = i * i; j <= lim; j += i)
                mark[j] = false;
        }
    }

    vector<bool> isPrime(R - L + 1, true);
    for (long long i : primes) {
        for (long long j = max(i * i, (L + i - 1) / i * i); j <= R; j += i)
            isPrime[j - L] = false;
    }
    if (L == 1)
        isPrime[0] = false;
    for(int i = 0;i < R-L+1;i++){
        if(isPrime[i])Primes.emplace_back(i+L);
    }
    return isPrime;// the ith on the range prime or not   isPrime[i] represent L+i is prime or not
}
```

### sum of digits from 1 to n

```cpp
int sumOfDigitsFrom1ToN(int n){  
    if (n<10)  
        return n*(n+1)/2;  
    int d = log10(n);  
    int a[d+1];  
    a[0] = 0, a[1] = 45;  
    for (int i=2; i<=d; i++) {  
        a[i] = a[i-1]*10 + 45*ceil(pow(10,i-1));  
    }  
    int p = ceil(pow(10, d));  
    int msd = n/p;  
    return msd*a[d] + (msd*(msd-1)/2)*p + msd*(1+n%p) + sumOfDigitsFrom1ToN(n%p);  
}
```

### sum of divisors from 1 to n
```cpp
__int128 sum(__int128 n){  return n * ( n + 1 ) / 2;}
__int128 S(__int128 n){
    __int128 res = 0;
    for(__int128 i=1;i*i<=n;i++){
        __int128 l = i, r = n/i;
        res = res + i*(r-l+1) + sum(r) - sum(l);res %= mod;
    }
    return res;
}
__int128 RangeSum(__int128 l,__int128 r){
    return add(S(r)  , - S(l-1) , mod);
}
```
<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>


# combinatorics
### Ncr
```cpp
ll inv[N] , FInv[N] , fact[N];
void Inverse() {
    inv[0] = inv[1] = 1;
    for (int i = 2; i < N; i++)
        inv[i] = inv[mod % i] * (mod - mod / i) % mod;
}
void FactorialInverse() {
    FInv[0] = FInv[1] = 1;
    for (int i = 2; i < N; i++)
        FInv[i] = (inv[i] * FInv[i - 1]) % mod;
}
void factorial() {
    fact[0] = 1;
    for (int i = 1; i < N; i++) {
        fact[i] = (fact[i - 1] * i) % mod;
    }
}
int Ncr(int n, int r) {
    if(r > n)return 0;
    ll ans = ((fact[n] * FInv[r])
               % mod * FInv[n - r])
              % mod;
    return ans;
}
void init() {
    Inverse();
    FactorialInverse();
    factorial();
}
```

### Ncr pascal
```cpp
const int M = 1001;
int C[M + 1][M + 1];
void PreCalculation() {
    C[0][0] = 1;
    for (int n = 1; n <= M; ++n) {
        C[n][0] = C[n][n] = 1;
        for (int k = 1; k < n; ++k)
            C[n][k] = C[n - 1][k - 1] + C[n - 1][k];
    }
}
```

### Catalan number
```cpp
int Catalan(int _n){
      // Catalan[n] is the solution for:
      // number of regular sequences of brackets that consisting of n open and n closed brackets
      // The number of rooted full binary trees with n + 1 leaves (vertices are not numbered). A rooted binary tree is full if every vertex has either two children or no children.
      // The number of ways to completely parenthesize n + 1 factor
      // The number of triangulations of a convex polygon with n + 2 sides (i.e. the number of partitions of polygon into disjoint triangles by using the diagonals).
      // The number of ways to connect the 2n points on a circle to form n disjoint chords.
    // The number of non-isomorphic full binary trees with n internal nodes (i.e. nodes having at least one son).
   //   The number of monotonic lattice paths from point (0, 0) to point (n, n) in a square lattice of size n * n which do not pass above the main diagonal
      //  (i.e. connecting (0, 0) to (n, n)).
   //   Number of permutations of length n that can be stack sorted
    //    (i.e. it can be shown that the rearrangement is stack sorted if and only if there is no such index i < j < k, such that a_k < a_i < a_j).
    //  The number of non-crossing partitions of a set of n elements.
    //  The number of ways to cover the ladder 1....n using n rectangles (The ladder consists of n columns, where ith column has a height i).
    return Ncr(2 * _n , _n) * inverse(_n + 1);
}
```

```cpp
catalan[0] = catalan[1] = 1;
    for (int i=2; i<MAX; i++) {
        catalan[i] = 0;
        for (int j=0; j < i; j++) {
            catalan[i] += (catalan[j] * catalan[i-j-1]) % mod;
            if (catalan[i] >= mod) {
                catalan[i] -= mod;
            }
        }
    }
```
### derangement
```cpp
int derangement(int n){return n <= 2 ? n != n : (n - 1) * (derangement(n - 1) + derangement(n - 2));}
```

### Largest x such that n! % k ^ x == 0
```cpp
int cal (int n, int k) {
    int res = 0;
    //return largest x such that pow(k , x) | n!
    while (n) {
        n /= k;
        res += n;
    }
    return res;
}
int factorial_power(int n,int k) {
    int res = inf;
    for (int j = 2; j * j <= k; j++) {
        if (k % j == 0) {
            res = min(res, cal(n, j));
        }
        while(k % j == 0){
            k /= j;
        }
    }
    if(k != 1)res = min(res , cal(n , k));
    return res;
}
```

### Locus theorm
```cpp
int fact[N];
int inv[N]; 
int InvFact[N];
void init() {
    fact[0] = inv[1] = fact[1] = InvFact[0] = InvFact[1] = 1;
    for (long long i = 2; i < N; i++) {
        fact[i] = (fact[i - 1] * i) % mod;
        inv[i] = mod - (inv[mod % i] * (mod / i) % mod);
        InvFact[i] = (inv[i] * InvFact[i - 1]) % mod;
    }
}
int nCr(int n, int r) {
    if(r > n || n < 0 || r < 0) return 0;
    return (((fact[n] * InvFact[r]) % mod) * InvFact[n - r]) % mod;
}
int lucas(int n , int r){
    if(r == 0) return 1;
    int res = 1;
    while(r){
        res = 1LL * res * nCr(n % mod , r % mod) % mod;
        n /= mod; r/= mod;
    }
    return res;
}
```

### number of sorted arrays with n values distinct
```cpp
int Count(int n , int l ,int r) {
    return add(-1, Ncr(n + r - l + 1, n), mod);
}
```

### Stirling number 
```cpp
int stirling_number(int n,int k){
    if(k==0)return n==0;
    if(n==0)return 0;
    return stirling_number(n-1,k-1)+(n-1)* stirling_number(n-1,k);
    //S[i][j]=S[i-1][j-1]+s[i-1][j]*(i-1)
    //S(n,k) as the different ways to cut n different elements into k undifferentiated non-empty subsets. For example, S(5,3) denotes to:25
}
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

# Graph

### 2-Sat
```cpp
struct TwoSat {  
    int N;  
    vector<vector<int>> gr;  
    vector<int> values; // 0 = false, 1 = true  
  
    TwoSat(int n = 0) : N(n), gr(2*n) {}  
  
    int addVar() { // (optional)  
       gr.emplace_back();  
       gr.emplace_back();  
       return N++;  
    }  
  
    void either(int f, int j) {  
       f = max(2*f, -1-2*f);  
       j = max(2*j, -1-2*j);  
       gr[f].push_back(j^1);  
       gr[j].push_back(f^1);  
    }  
    void setValue(int x) { either(x, x); }  
  
    void atMostOne(const vector<int>& li) { // (optional)  
       if (li.size() <= 1) return;  
       int cur = ~li[0];  
       for(int i = 2;i < li.size();i++) {  
          int next = addVar();  
          either(cur, ~li[i]);  
          either(cur, next);  
          either(~li[i], next);  
          cur = ~next;  
       }  
       either(cur, ~li[1]);  
    }  
  
    vector<int> val, comp, z; int time = 0;  
    int dfs(int i) {  
       int low = val[i] = ++time, x; z.push_back(i);  
       for(int e : gr[i]) if (!comp[e])  
          low = min(low, val[e] ? low: dfs(e));  
       if (low == val[i]) do {  
          x = z.back(); z.pop_back();  
          comp[x] = low;  
          if (values[x>>1] == -1)  
             values[x>>1] = x&1;  
       } while (x != i);  
       return val[i] = low;  
    }  
  
    bool solve() {  
       values.assign(N, -1);  
       val.assign(2*N, 0); comp = val;  
       for(int i = 0;i < 2 * N;i++) if (!comp[i]) dfs(i);  
       for(int i = 0;i < N;i++) if (comp[2*i] == comp[2*i+1]) return false;  
       return true;  
    }  
};
```

```cpp
template<class E>
struct csr {
	std::vector<int> start;
	std::vector<E> elist;

	explicit csr(int n, const std::vector<std::pair<int, E>> &edges)
	: start(n + 1), elist(edges.size()) {
		for (auto e: edges) {
			start[e.first + 1]++;
		}
		for (int i = 1; i <= n; i++) {
			start[i] += start[i - 1];
		}
		auto counter = start;
		for (auto e: edges) {
			elist[counter[e.first]++] = e.second;
		}
	}
};
struct scc_graph {
public:
	explicit scc_graph(int n) : _n(n) {}

	int num_vertices() { return _n; }

	void add_edge(int from, int to) { edges.push_back({from, {to}}); }

    // @return pair of (# of scc, scc id)
	std::pair<int, std::vector<int>> scc_ids() {
		auto g = csr<edge>(_n, edges);
		int now_ord = 0, group_num = 0;
		std::vector<int> visited, low(_n), ord(_n, -1), ids(_n);
		visited.reserve(_n);
		auto dfs = [&](auto self, int v) -> void {
			low[v] = ord[v] = now_ord++;
			visited.push_back(v);
			for (int i = g.start[v]; i < g.start[v + 1]; i++) {
				auto to = g.elist[i].to;
				if (ord[to] == -1) {
					self(self, to);
					low[v] = std::min(low[v], low[to]);
				} else {
					low[v] = std::min(low[v], ord[to]);
				}
			}
			if (low[v] == ord[v]) {
				while (true) {
					int u = visited.back();
					visited.pop_back();
					ord[u] = _n;
					ids[u] = group_num;
					if (u == v) break;
				}
				group_num++;
			}
		};
		for (int i = 0; i < _n; i++) {
			if (ord[i] == -1) dfs(dfs, i);
		}
		for (auto &x: ids) {
			x = group_num - 1 - x;
		}
		return {group_num, ids};
	}

	std::vector<std::vector<int>> scc() {
		auto ids = scc_ids();
		int group_num = ids.first;
		std::vector<int> counts(group_num);
		for (auto x: ids.second) counts[x]++;
			std::vector<std::vector<int>> groups(ids.first);
		for (int i = 0; i < group_num; i++) {
			groups[i].reserve(counts[i]);
		}
		for (int i = 0; i < _n; i++) {
			groups[ids.second[i]].push_back(i);
		}
		return groups;
	}

private:
	int _n;
	struct edge {
		int to;
	};
	std::vector<std::pair<int, edge>> edges;
};
struct two_sat {
public:
	two_sat() : _n(0), scc(0) {}

	explicit two_sat(int n) : _n(n), _answer(n), scc(2 * n) {}

	void OR(int i, bool f, int j, bool g) {
		assert(0 <= i && i < _n);
		assert(0 <= j && j < _n);
		scc.add_edge(2 * i + (f ? 0 : 1), 2 * j + (g ? 1 : 0));
		scc.add_edge(2 * j + (g ? 0 : 1), 2 * i + (f ? 1 : 0));
	}
	void Xor(int a, bool f , int b , bool g){
		OR(a , f , b , g);
		OR(a , !f , b , !g);
	}
	void Imply(int a, bool f , int b , bool g){
		OR(a , !f , b , g);
	}
	void BiImply(int a,bool f , int b , bool g){
		// equivalent to Xnor
		Imply(a ,f , b ,g);
		Imply(b ,f  ,a ,g);
	}
	void ForceTrue(int a){
		OR(a , true , a , true);
	}
	void ForceFalse(int a){
		OR(a , false ,a , false);
	}

	bool satisfiable() {
		auto id = scc.scc_ids().second;
		for (int i = 0; i < _n; i++) {
			if (id[2 * i] == id[2 * i + 1]) return false;
			_answer[i] = id[2 * i] < id[2 * i + 1];
		}
		return true;
	}

	std::vector<bool> answer() { return _answer; }

private:
	int _n;
	std::vector<bool> _answer;
	scc_graph scc;
};
```

### bridges
```cpp
vector<int>adj[N];
vector<pair<int,int>>bridges;
int dfn[N], LowLink[N],ndfn = 0;
void Tarjan(int node,int parent){
    dfn[node] = LowLink[node] = ndfn++;
    for(auto &val:adj[node]){
        if(dfn[val]==-1){
            Tarjan(val,node);
            LowLink[node] = min(LowLink[node],LowLink[val]);
        }else if(parent!=val){
            LowLink[node] = min(LowLink[node],dfn[val]);
        }
    }
    if(LowLink[node]==dfn[node] && ~parent){
        bridges.emplace_back(parent,node);
    }
}
void Bridges(int n){
    memset(dfn,-1,sizeof dfn);
    for(int i=0;i<n;i++){
        if(dfn[i]==-1){
            Tarjan(i,-1);
        }
    }
}
```

### Art points
```cpp
int dfn[N],LowLink[N],ndfn = 0;
vector<int>adj[N];
bool IsArtPoints[N];
vector<int>ArtPoints;
void Tarjan(int node,int parent){
    dfn[node] = LowLink[node] = ndfn++;
    int child = 0;
    for(auto &val:adj[node]){
        if(dfn[val]==-1){
            child++;
            Tarjan(val,node);
            LowLink[node] = min(LowLink[node],LowLink[val]);
            if(LowLink[val]>=dfn[node]){
                if(parent==-1&&child<=1)continue;
                IsArtPoints[node] = true;
            }
        }else if(parent!=val){
            LowLink[node] = min(LowLink[node],dfn[val]);
        }
    }
}
void Art(int n){
    ndfn = 0;
    for(int i=0;i<n;i++){
        dfn[i] = -1;
        IsArtPoints[i] = false;
        LowLink[i] = 0;
    }
    for(int i=0;i<n;i++){
        if(dfn[i]==-1)Tarjan(i,-1);
    }
    for(int i=0;i<n;i++){
        if(IsArtPoints[i])ArtPoints.emplace_back(i);
    }
}
```

### SCC
```cpp
vector<vector<int>> adj , dag , comps;
int comp[N] , inStack[N] , lowLink[N] , dfn[N] , deg[N];
stack<int> st;
int ndfn;
void tarjan(int u){
    dfn[u] = lowLink[u] = ndfn++;
    inStack[u] = true;
    st.push(u);
    for(auto &v : adj[u]){
        if(dfn[v] == -1){
            tarjan(v);
            lowLink[u] = min(lowLink[u] , lowLink[v]);
        }else if(inStack[v]){
            lowLink[u] = min(lowLink[u] , dfn[v]);
        }
    }
    if(dfn[u] == lowLink[u]){
        // head of component
        int x = -1;
        comps.emplace_back(vector<int>());
        while(x != u){
            x = st.top(); st.pop(); inStack[x] = 0;
            comps.back().emplace_back(x);
            comp[x] = (int)comps.size() - 1;
        }
    }
}
void genDag(){
    dag.resize(comps.size());
    for(int u = 0 ; u < adj.size() ; u++){
        for(auto &v :adj[u]){
            if(comp[u] != comp[v]){
                dag[comp[u]].emplace_back(comp[v]);
                deg[comp[v]]++;
            }
        }
    }
}
void SCC(int n){
    ndfn = 0;
    comps.clear();
    for(int i=0;i<n;i++){
        dfn[i] = -1;
        lowLink[i] = inStack[i] = deg[i] = 0;
    }
    for(int i = 0 ; i < n ; i++)
        if(dfn[i] == -1) tarjan(i);
    genDag();
}
```

### Bridges tree
```cpp
//call init then fill the adj , edges then get the bridges tree
int n,m;
vector<pair<int,int>> adj[N] , edges;
vector<int> BridgeTree[N];
int lowLink[N] , dfn[N] , comp[N] , ndfn , comp_num;
bool isBridge[N];//max M
bool  vis[N];
int All[N];
int siz[N];
void tarjan(int u , int par) {
    dfn[u] = lowLink[u] = ndfn++;
    for (auto &[v, id]: adj[u]) {
        if (dfn[v] == -1) {
            tarjan(v, u);
            lowLink[u] = min(lowLink[u], lowLink[v]);
            if (lowLink[v] == dfn[v]) {
                int uu = u, vv = v;
                if (uu > vv) swap(uu, vv);
                isBridge[id] = true;
            }
        } else if (v != par) {
            lowLink[u] = min(lowLink[u], dfn[v]);
        }
    }
}
void Find_component(int u , int par) {
    vis[u] = true;
    comp[u] = comp_num;
    siz[comp_num]++;
    for (auto &[v, id]: adj[u])
        if (vis[v] == 0 && isBridge[id] == 0)
            Find_component(v, u);
}
void GetBridgesTree() {
    for (int i = 0; i < n; i++) {
        dfn[i] = -1;
        lowLink[i] = 0;
        
    }
    ndfn = 0;
    tarjan(0, 0);
    for (int i = 0; i < n; i++)
        if (vis[i] == 0) {
            Find_component(i, i);
            comp_num++;
        }
    for (int i = 0; i < m; i++) {
        if (isBridge[i]) {
            BridgeTree[comp[edges[i].first]].emplace_back(comp[edges[i].second]);
            BridgeTree[comp[edges[i].second]].emplace_back(comp[edges[i].first]);
        }
    }
 
}
void init(){
	for (int i = 0; i < n; i++) {
        dfn[i] = -1;
        lowLink[i] = 0;
        adj[i].clear();
        BridgeTree[i].clear();
        vis[i] = false;
        comp[i] = -1;
        siz[i] = 0;
    }
    edges.clear();
    for(int i = 0;i<m;i++){
    	isBridge[i] = false;
    }
    comp_num = 0;
}
```

### bellman-ford
```cpp

// basic
struct Edge {
    int a, b, cost;
};

int n, m;
vector<Edge> edges;

void solve(int source)
{
    vector<int> d(n, inf);
    d[source] = 0;
    for (int i = 0; i < n - 1; ++i)
        for (Edge e : edges)
            if (d[e.a] < inf)
                d[e.b] = min(d[e.b], d[e.a] + e.cost);
    // display d, for example, on the screen
}
--------------------
// any negative cycle
struct Edge {
    int a, b, cost;
};

int n, m;
vector<Edge> edges;

vector<int> solve()
{
    vector<int> d(n);
    vector<int> p(n, -1);
    int x;
    for (int i = 0; i < n; ++i) {
        x = -1;
        for (Edge e : edges) {
            if(d[e.a] < inf){
                if (d[e.a] + e.cost < d[e.b]) {
                    d[e.b] = max(-INF, d[e.a] + e.cost);
                    p[e.b] = e.a;
                    x = e.b;
                }
            }
        }
    }

    if (x == -1) {
        return vector<int>{-1};
    } else {
        for (int i = 0; i < n; ++i)
            x = p[x];

        vector<int> cycle;
        for (int v = x;; v = p[v]) {
            cycle.push_back(v);
            if (v == x && cycle.size() > 1)
                break;
        }
        reverse(cycle.begin(), cycle.end());

        return cycle;
    }
}

---------------------
// all nodes affected by negative cycle
struct Edge {
    int a, b, cost;
};

int n, m;
vector<Edge> edges;

vector<int> solve(int source)
{
    vector<int> d(n, inf);
    d[source] = 0;
    for (int i = 0; i < n - 1; ++i)
        for (Edge e : edges)
            if (d[e.a] < inf)
                d[e.b] = min(d[e.b], d[e.a] + e.cost);

    vector<int>ref = d;
    for (int i = 0; i < n; ++i)
        for (Edge e : edges)
            if (d[e.a] < inf)
                d[e.b] = min(d[e.b], d[e.a] + e.cost);

    vector<int>nodes;
    
    for(int i=0;i<n;i++){
        if(ref[i] != d[i]){
	   nodes.emplace_back(i);
        }
    }
    return nodes;
}
-------------
// get path
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
------------
// negative cycle given source
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

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Floyed
```cpp
// basic floyed
void shortest_distance(vector<vector<int>>&matrix) {
    int n = (int) matrix.size();
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < n; k++)
                matrix[j][k] = min(matrix[j][k], matrix[j][i] + matrix[i][k]);
        }
    }
}
----------
// count paths
void CountPaths(vector<vector<int>>&adj) {
    int n = (int)adj.size();
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++)
                adj[i][j] += adj[i][k] * adj[k][j];
        }
    }
}
-------------
// graph diameter
int diameter(vector<vector<int>>&adj) {
    int n = (int) adj.size();
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < n; k++) {
                adj[j][k] = min(adj[j][k], adj[j][i] + adj[i][k]);
            }
        }
    }
    int graph_diameter = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (adj[i][j] < inf)
                graph_diameter = max(graph_diameter, adj[i][j]);
        }
    }
    return graph_diameter;
}
-------------
// get path
vector<vector<int>>adj,parent;
vector<int>path;
void GetPath(int i,int j){
    if(i != j){
        GetPath(i , parent[i][j]);
    }
    path.emplace_back(j);
}
--------------
// longest path
void LongestPath(vector<vector<int>>&adj,int n) {
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                adj[i][j] = max(adj[i][j], max(adj[i][k], adj[k][j]));
            }
        }
    }
}
--------------
// max-min edge
void MaxMinEdge(vector<vector<int>>&adj,int n) {
    //find road that min value is maximum
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++)
                adj[i][j] = max(adj[i][j], min(adj[i][k], adj[k][j]));
        }
    }
}
-----------------
// min-max edge
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
----------------
// negative cycle effect ?
bool CheckCycleEffect(vector<vector<int>>&adj,int source,int dist) {
    //apply floyed first
    int n = (int)adj.size();
    for (int i = 0; i < n; i++) {
        if (adj[i][i] != 0 && adj[source][i] < inf && adj[i][dist] < inf)
            return true;
    }
    return false;
}
----------------
// negative cycle
bool NegativeCycle(vector<vector<int>>&adj,int n) {
    //apply floyed first
    for (int i = 0; i < n; i++)
        if (adj[i][i] != 0)
            return true;
    return false;
}
---------------
// path exist?
void PathExist(vector<vector<bool>>&adj) {
    int n = (int) adj.size();
    for (int k = 0; k < n; k++){
        for (int i = 0; i < n; i++){
            for (int j = 0; j < n; j++)
                adj[i][j] = (adj[i][j] | (adj[i][k] & adj[k][j]));
        }
    }
}
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>


### Dsu

#### Augmented dsu
```cpp

struct AugmentedDsu {
    int flaw;     //counting numbers of inconsistent assertions
    vector<int>diff,parent;
    AugmentedDsu(int n) {
        flaw = 0;
        diff.resize(n);parent.resize(n);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
            diff[i] = 0;
        }
    }
    int find(int x) {
        if (parent[x] == x) return x;
        int rx = find(parent[x]);  // rx is the root of x
        diff[x] = diff[parent[x]] + diff[x]; //add all potentials along the path,i.e.,potential calculated wrt root
        parent[x] = rx;
        return rx;
    }
    bool merge(int a, int b, int d) {
        int ra = find(a);
        int rb = find(b);
        if (ra == rb && diff[a] - diff[b] != d) {
            flaw++;
            return false;
        } else if (ra != rb) {
            diff[ra] = d + diff[b] - diff[a];
            parent[ra] = rb;
        }
        return true;
    }
    int query(int a,int b){
        int ra = find(a);
        int rb = find(b);
        if(ra != rb){
            //can't get answer
            return -inf;
        }
        return diff[a] - diff[b];
    }
};

```

#### dsu distant to parent
```cpp
struct dsu {
    vector<pair<int,int>>parent;//parent and distant to the leader
    vector<int>rank;
    dsu(int n){
        parent.resize(n);
        rank.resize(n);
        for(int i=0;i<n;i++)make_set(i);
    }
    void make_set(int v) {
        parent[v] = make_pair(v, 0);
        rank[v] = 0;
    }

    pair<int, int> find(int v) {
        if (v != parent[v].first) {
            int len = parent[v].second;
            parent[v] = find(parent[v].first);
            parent[v].second += len;
        }
        return parent[v];
    }

    bool merge(int a, int b) {
        a = find(a).first;
        b = find(b).first;
        if (a != b) {
            if (rank[a] < rank[b])
                swap(a, b);
            parent[b] = make_pair(a, 1);
            if (rank[a] == rank[b])
                rank[a]++;
            return true;
        }
        return false;
    }
};
```

#### dsu get set
```cpp
struct dsu{
    int n,cnt;
    vector<int>size,parent,nxt,tail,sets,pos;
    void init(int nn){
        this->n=nn;
        size.resize(n,1);
        parent.resize(n);
        nxt.resize(n,-1);
        sets.resize(n);
        pos.resize(n);
        tail.resize(n);
        iota(parent.begin(),parent.end(),0);
        iota(tail.begin(),tail.end(),0);
        iota(sets.begin(),sets.end(),0);
        iota(pos.begin(),pos.end(),0);
        cnt=nn;
    }
    dsu (int n=0){
        init(n);
    }
    int find(int child){
        return (child==parent[child]?child:parent[child]=find(parent[child]));
    }
    bool merge(int u,int v){
        u=find(u);
        v=find(v);
        if(v==u)return false;
        if(size[u]<size[v])swap(u,v);
        parent[v]=u;
        size[u]+=size[v];
        int p=pos[v];
        pos[sets[p]=sets[--cnt]]=p;
        int &t=tail[u];
        nxt[t]=v;
        t=tail[v];
        return true;

    }
    vector<int>get_set(int node){
        node=find(node);
        vector<int>res;
        for(int i=sets[node];~i;i=nxt[i]){
            res.emplace_back(i);
        }
        return  res;
    }
};

```

#### dsu on trees
```cpp
int n;
int a[N] , sz[N] , bg[N] , freq[N] , ans[N] , x;
vector<int>adj[N];
int cnt = 0;
void dfs1(int node , int par) {
    sz[node] = 1;
    for(auto &val:adj[node]) {
        if(val == par)continue;
        dfs1(val , node);
        sz[node] += sz[val];
        if(bg[node] == -1 || sz[bg[node]] < sz[val])bg[node] = val;
    }
}
void collect(int node , int par , int value) {
    if(value == 1 && freq[a[node]] == 0)cnt++;
    if(value == -1 && freq[a[node]] == 1)cnt--;
    freq[a[node]] += value;
    for(auto &val:adj[node]) {
        if(val == par)continue;
        collect(val , node , value);
    }
}
void dfs2(int node , int par , bool keep = false) {
    for(auto &val:adj[node]) {
        if(val == par || val == bg[node])continue;
        dfs2(val , node , false);
    }
    if(bg[node] != -1)dfs2(bg[node] , node , true);
    // insert yourself
    if(!freq[a[node]])cnt++;
    freq[a[node]]++;
    // insert small sub trees
    for(auto &val:adj[node]) {
        if(val == par || val == bg[node])continue;
        collect(val , node , 1);
    }
    // calculate answer
    ans[node] = cnt;
    // delete you and the small children
    if(!keep) {
        collect(node , par , -1);
    }
}
```

#### dsu online birpitate
```cpp
struct dsu {
    vector<int>rank,bipartite;
    vector<pair<int,int>>parent;
    dsu(int _n){
        rank.resize(_n),bipartite.resize(_n);
        parent.resize(_n);
        for(int i=0;i<_n;i++)make_set(i);
    }
    void make_set(int v) {
        parent[v] = make_pair(v, 0);
        rank[v] = 0;
        bipartite[v] = true;
    }

    pair<int, int> find(int v) {
        if (v != parent[v].first) {
            int parity = parent[v].second;
            parent[v] = find(parent[v].first);
            parent[v].second ^= parity;
        }
        return parent[v];
    }
    void add_edge(int a, int b) {
        pair<int, int> pa = find(a);
        a = pa.first;
        int x = pa.second;

        pair<int, int> pb = find(b);
        b = pb.first;
        int y = pb.second;

        if (a == b) {
            if (x == y)
                bipartite[a] = false;
        } else {
            if (rank[a] < rank[b])
                swap(a, b);
            parent[b] = make_pair(a, x ^ y ^ 1);
            bipartite[a] &= bipartite[b];
            if (rank[a] == rank[b])
                ++rank[a];
        }
    }

    bool is_bipartite(int v) {
        return bipartite[find(v).first];
    }
};
```

#### dynamic small to large
```cpp
int n;
int id[N],values[N];
vector<int>adj[N];
vector<int>sets[N];
int res = 0;
int dfs(int node,int par){
    int a = id[node];
    for(auto &val:adj[node]){
        if(val==par)continue;
        int b = dfs(val,node);
        if(sets[a].size() > sets[b].size())
            swap(a,b);
        for(auto &x:sets[a]){
            //small set append it to set[b]
        }
        sets[a].clear();
    }
    return a;
}
void init(){
    for(int i=0;i<n;i++){
        sets[i][values[i]]++;
        id[i] = i;
    }
}
```

#### eval dsu
```cpp
struct EvalDsu{
    vector<int>parent,lazy;
    EvalDsu(int _n){
        parent.resize(_n);
        lazy.resize(_n);
        iota(parent.begin(),parent.end(),0ll);
    }
    int find(int u){
        if(u != parent[u]){
            int leader = find(parent[u]);
            lazy[u] += lazy[parent[u]];
            parent[u] = leader;
        }
        return parent[u];
    }
    bool same(int u,int v){
        return find(u) == find(v);
    }
    bool merge(int u,int v){
        //u is the parent , v = leader of the set must be like that
        if(same(u,v))return false;
        lazy[u] += f(u,v);
        parent[v] = u;
        return true;
    }
    int eval(int u){
        find(u);
        //evaluate function from u to leader of u
        //work in this path
        return lazy[u];
    }
    int f(int x,int y){
        //write your work here and f must be asscioative   f(a,f(b,c)) == f(f(a,b),c)
        return 0;
    }
};
```

#### roll back dsu
```cpp
struct RollBackDsu{
    vector<int>parent,size;
    stack<pair<pair<int,int>,pair<int,int>>>sk;
    RollBackDsu(int _n){
        parent.resize(_n);
        iota(parent.begin(),parent.end(),0ll);
        size.resize(_n,1);
    }
    int find(int child){
        return (child == parent[child]?child:find(parent[child]));
    }
    bool Merge(int a,int b){
        a = find(a);
        b = find(b);
        if(a==b)return false;
        if(size[a]>size[b])swap(a,b);
        sk.push({{a,parent[a]},{b,size[b]}});
        parent[a] = b;
        size[b] += size[a];
        return true;
    }
    void RollBack(int cnt){
        assert(cnt <= sk.size());
        while(cnt--){
            auto it = sk.top();
            sk.pop();
            parent[it.first.first] = it.first.second;
            size[it.second.first] = it.second.second;
        }
    }
};
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Euler tour
#### undirected Euler tour
```cpp
int n,m;
set<int>adj[N];
int degree[N];
vector<int>tour;
void dfs(int node){
    while((int)adj[node].size()){
        int x = *adj[node].rbegin();
        adj[node].erase(x);
        if(adj[x].count(node)){
            adj[x].erase(node);
            dfs(x);
        }
    }
    tour.emplace_back(node);
}
bool check(){
    for(int i=0;i<n;i++){
        if(degree[i]&1){
            return false;
        }
    }
    return true;
}
bool get(){
    dfs(0);
    reverse(tour.begin(),tour.end());
    if(tour.size()!=m+1){
        return false;
    }
    return true;
}
```

#### undirected euler path
```cpp
class UnDirectedEulerPath{
public:
    int n;
    vector<vector<int>>adj;
    vector<int>deg;
    vector<int>tour;
    map<pair<int,int>,int>edges;
    int StartingNode=-1,EndingNode=-1;
    UnDirectedEulerPath(int n,vector<pair<int,int>>&edges){
        this->n=n;
        adj.resize(n);
        deg.resize(n);
        for(auto &val:edges){
            this->edges[val]++;
            pair<int,int>p=val;
            swap(p.first,p.second);
            this->edges[p]++;
        }
        for(auto &val:edges){
            adj[val.first].emplace_back(val.second);
            adj[val.second].emplace_back(val.first);
            deg[val.first]++,deg[val.second]++;
        }
    }
    void component(int node,vector<bool>&vis){
        vis[node]=true;
        for(auto &val:adj[node]){
            if(!vis[val])
                component(val,vis);
        }
    }
    bool check(){
        vector<bool>vis(n,false);
        for(int i=0;i<n;i++){
            if(deg[i]){
                component(i,vis);
                break;
            }
        }
        for(int i=0;i<n;i++){
            if(!vis[i]&&deg[i]){
                return false;
            }
        }
        for(int i=0;i<n;i++){
            if(deg[i]&1^1)continue;
            else if(StartingNode==-1)StartingNode=i;
            else if(EndingNode==-1)EndingNode=i;
            else return false;
        }
        if(StartingNode==-1){
            for(int i=0;i<n;i++){
                if(deg[i]){
                    StartingNode=i;
                    break;
                }
            }
        }
        return true;
    }
    void DFS(int node){
        for(auto &val:adj[node]){
            if(this->edges[make_pair(node,val)])
              this->edges[make_pair(node,val)]--,this->edges[make_pair(val,node)]--,DFS(val);
        }
        tour.emplace_back(node);
    }
    void Get(){
        map<pair<int,int>,int>TempEdges=edges;
        DFS(StartingNode);
        edges=TempEdges;
        reverse(tour.begin(),tour.end());
    }
};
```

#### directed euler cycle
```cpp
class DirectedEulerCycle{
public:
    vector<vector<int>>adj;
    vector<int>in,out;
    vector<int>tour;
    int n;
    int s=-1;
    DirectedEulerCycle(int n,vector<pair<int,int>>&edges){
        this->n=n;
        adj.resize(n);
        in.resize(n),out.resize(n);
        for(auto &val:edges){
            adj[val.first].emplace_back(val.second);
            out[val.first]++,in[val.second]++;
        }
    }
    void dfs(int node){
        while(!adj[node].empty()){
            int x=adj[node].back();
            adj[node].pop_back();
            dfs(x);
        }
        tour.emplace_back(node);
    }
    bool Get(){
        int counter1=0;
        for(int i=0;i<n;i++){
            if(in[i]==out[i])counter1++;
            else {
                return false;
            }
        }
        for(int i=0;i<n;++i){
            if(in[i]||out[i]){
                s=i;break;
            }
        }
        dfs(s);
        bool all=false;
        for(int i=0;i<n;i++){
            all=all||(int)adj[i].size();
        }
        if(!all){
            reverse(tour.begin(),tour.end());
            return true;
        }else
            return false;
    }
};
```

#### directed euler path
```cpp
int n,m;
vector<int>adj[N];
vector<int>tour;
int in[N],out[N];
int Start=-1,End=-1;
bool euler(){
    for(int i=0;i<n;i++){
        if(out[i]-in[i]>1||in[i]-out[i]>1)return false;
        if(in[i]==out[i]+1){
            if(End!=-1)return false;
            End = i;
        }
        if(in[i]+1==out[i]){
            if(Start!=-1)return false;
            Start = i;
        }
    }
    return true;
}
void dfs(int node){
    while(out[node]){
        dfs(adj[node][--out[node]]);
    }
    tour.emplace_back(node);
}
```

### Flows

#### standard Dinic 
```cpp
// O(V^2 E) and in unit graph works in O(E sqrt(v))
struct FlowEdge {
    int v, u;
    long long cap, flow = 0;
    FlowEdge(int v, int u, long long cap) : v(v), u(u), cap(cap) {}
};
struct Dinic {
    const long long flow_inf = 1e18;
    vector<FlowEdge> edges;
    vector<vector<int>> adj;
    int n, m = 0;
    int s, t;
    vector<int> level, ptr;
    queue<int> q;

    Dinic(int n, int s, int t) : n(n), s(s), t(t) {
        adj.resize(n);
        level.resize(n);
        ptr.resize(n);
    }

    void add_edge(int v, int u, long long cap) {
        edges.emplace_back(v, u, cap);
        edges.emplace_back(u, v, 0);
        adj[v].push_back(m);
        adj[u].push_back(m + 1);
        m += 2;
    }

    bool bfs() {
        while (!q.empty()) {
            int v = q.front();
            q.pop();
            for (int id : adj[v]) {
                if (edges[id].cap == edges[id].flow)
                    continue;
                if (level[edges[id].u] != -1)
                    continue;
                level[edges[id].u] = level[v] + 1;
                q.push(edges[id].u);
            }
        }
        return level[t] != -1;
    }

    long long dfs(int v, long long pushed) {
        if (pushed == 0)
            return 0;
        if (v == t)
            return pushed;
        for (int& cid = ptr[v]; cid < (int)adj[v].size(); cid++) {
            int id = adj[v][cid];
            int u = edges[id].u;
            if (level[v] + 1 != level[u])
                continue;
            long long tr = dfs(u, min(pushed, edges[id].cap - edges[id].flow));
            if (tr == 0)
                continue;
            edges[id].flow += tr;
            edges[id ^ 1].flow -= tr;
            return tr;
        }
        return 0;
    }

    long long flow() {
        long long f = 0;
        while (true) {
            fill(level.begin(), level.end(), -1);
            level[s] = 0;
            q.push(s);
            if (!bfs())
                break;
            fill(ptr.begin(), ptr.end(), 0);
            while (long long pushed = dfs(s, flow_inf)) {
                f += pushed;
            }
        }
        return f;
    }
};

```

#### optimized push relabel in o(n ^ 3)
```cpp
// O(V ^ 3)
const ll inf = 1e18;

int n; // Number of nodes
vector<vector<ll>> capacity, flow; // Capacity and flow matrices
vector<ll> height, excess; // Height and excess flow vectors

void push(int u, int v) {
    ll d = min(excess[u], capacity[u][v] - flow[u][v]);
    flow[u][v] += d;
    flow[v][u] -= d;
    excess[u] -= d;
    excess[v] += d;
}

void relabel(int u) {
    ll d = inf;
    for (int i = 0; i < n; i++) {
        if (capacity[u][i] - flow[u][i] > 0)
            d = min(d, height[i]);
    }
    if (d < inf)
        height[u] = d + 1;
}

vector<int> find_max_height_vertices(int s, int t) {
    vector<int> max_height;
    for (int i = 0; i < n; i++) {
        if (i != s && i != t && excess[i] > 0) {
            if (!max_height.empty() && height[i] > height[max_height[0]])
                max_height.clear();
            if (max_height.empty() || height[i] == height[max_height[0]])
                max_height.push_back(i);
        }
    }
    return max_height;
}

ll max_flow(int s, int t) {
    // Initialize
    height.assign(n, 0);
    height[s] = n;
    flow.assign(n, vector<ll>(n, 0));
    excess.assign(n, 0);
    excess[s] = inf;
    for (int i = 0; i < n; i++) {
        if (i != s)
            push(s, i);
    }

    // Process vertices with excess flow
    vector<int> current;
    while (!(current = find_max_height_vertices(s, t)).empty()) {
        for (int i : current) {
            bool pushed = false;
            for (int j = 0; j < n && excess[i]; j++) {
                if (capacity[i][j] - flow[i][j] > 0 && height[i] == height[j] + 1) {
                    push(i, j);
                    pushed = true;
                }
            }
            if (!pushed) {
                relabel(i);
                break;
            }
        }
    }

    // Return total flow to the sink
    return excess[t];
}
```

#### max matching Hopcroft-Karp
```cpp
const int INF = 1e9; // Infinity value for unmatched nodes

// BFS Phase to build layers
bool bfs(int n, int m, vector<vector<int>>& adj, vector<int>& pairU, vector<int>& pairV, vector<int>& dist) {
    queue<int> q;

    // Initialize distances
    for (int u = 1; u <= n; ++u) {
        if (pairU[u] == 0) { // If the boy is unmatched
            dist[u] = 0;
            q.push(u);
        } else {
            dist[u] = INF;
        }
    }
    dist[0] = INF; // Sentinel for unmatched nodes

    // Perform BFS
    while (!q.empty()) {
        int u = q.front(); q.pop();
        if (dist[u] < dist[0]) { // Check all neighbors
            for (int v : adj[u]) {
                if (dist[pairV[v]] == INF) {
                    dist[pairV[v]] = dist[u] + 1;
                    q.push(pairV[v]);
                }
            }
        }
    }

    return dist[0] != INF; // Returns true if there's an augmenting path
}

// DFS Phase to find augmenting paths
bool dfs(int u, vector<vector<int>>& adj, vector<int>& pairU, vector<int>& pairV, vector<int>& dist) {
    if (u != 0) { // Sentinel check
        for (int v : adj[u]) {
            if (dist[pairV[v]] == dist[u] + 1) {
                if (dfs(pairV[v], adj, pairU, pairV, dist)) {
                    pairV[v] = u;
                    pairU[u] = v;
                    return true;
                }
            }
        }
        dist[u] = INF;
        return false;
    }
    return true;
}

// Hopcroft-Karp Algorithm
vector<pair<int , int>> hopcroftKarp(int n, int m, vector<vector<int>>& adj) {
    // graph should be 1 based
    vector<int> pairU(n + 1, 0); // Pairings for boys
    vector<int> pairV(m + 1, 0); // Pairings for girls
    vector<int> dist(n + 1);     // Distance array for BFS

    int matching = 0;

    // Repeat until no augmenting path exists
    while (bfs(n, m, adj, pairU, pairV, dist)) {
        for (int u = 1; u <= n; ++u) {
            if (pairU[u] == 0 && dfs(u, adj, pairU, pairV, dist)) {
                ++matching;
            }
        }
    }

    // Output the matches
    vector<pair<int , int>>matches;
    for (int v = 1; v <= m; ++v) {
        if (pairV[v] != 0) {
            matches.emplace_back(pairV[v] , v);
        }
    }

    return matches;
}
```

#### max matching in o(VE)
```cpp
#include bitsstdc++.h
using namespace std;
using ll = long long;
bool dfs(int boy, vectorvectorint& adj, vectorint& match, vectorbool& visited) {
    if (visited[boy]) return false;
    visited[boy] = true;

    for (int girl  adj[boy]) {
         If the girl is not matched or we can find an alternative match for her current partner
        if (match[girl] == -1  dfs(match[girl], adj, match, visited)) {
            match[girl] = boy;
            return true;
        }
    }
    return false;
}

int main() {
    iossync_with_stdio(false);
    cin.tie(nullptr);
    int n, m, k;
    cin  n  m  k;

    vectorvectorint adj(n + 1);  Adjacency list for boys and girls
    for (int i = 0; i  k; ++i) {
        int a, b;
        cin  a  b;
        adj[a].push_back(b);
    }

    vectorint match(m + 1, -1);   Matches for girls (1-indexed)
    int max_pairs = 0;

     Maximum bipartite matching
    for (int boy = 1; boy = n; ++boy) {
        vectorbool visited(n + 1, false);
        if (dfs(boy, adj, match, visited)) {
            ++max_pairs;
        }
    }

    cout  max_pairs  'n';
    for (int girl = 1; girl = m; ++girl) {
        if (match[girl] != -1) {
            cout  match[girl]     girl  'n';
        }
    }

}
```

#### min-cost - max-flow
```cpp
const int INF = 1e9;

struct Edge {
    int to, capacity, cost, flow, rev;
};

class MinCostMaxFlow {
private:
    int n;
    vector<vector<Edge>> adj;
    vector<int> dist, parent, parentEdge;
    vector<bool> inQueue;

public:
    MinCostMaxFlow(int n) : n(n), adj(n), dist(n), parent(n), parentEdge(n), inQueue(n) {}
    // note you need dummy source , dummy sink when create paths and you should delete them from the path later
    void addEdge(int u, int v, int capacity, int cost) {
        Edge a = {v, capacity, cost, 0, (int)adj[v].size()};
        Edge b = {u, 0, -cost, 0, (int)adj[u].size()};
        adj[u].push_back(a);
        adj[v].push_back(b);
    }

    bool spfa(int source, int sink) {
        fill(dist.begin(), dist.end(), INF);
        fill(inQueue.begin(), inQueue.end(), false);
        queue<int> q;

        dist[source] = 0;
        q.push(source);
        inQueue[source] = true;

        while (!q.empty()) {
            int u = q.front();
            q.pop();
            inQueue[u] = false;

            for (int i = 0; i < adj[u].size(); ++i) {
                Edge &e = adj[u][i];
                if (e.flow < e.capacity && dist[e.to] > dist[u] + e.cost) {
                    dist[e.to] = dist[u] + e.cost;
                    parent[e.to] = u;
                    parentEdge[e.to] = i;
                    if (!inQueue[e.to]) {
                        q.push(e.to);
                        inQueue[e.to] = true;
                    }
                }
            }
        }
        return dist[sink] < INF;
    }

    pair<int, int> solve(int source, int sink, int maxFlow) {
        int flow = 0, cost = 0;

        while (flow < maxFlow && spfa(source, sink)) {
            int pathFlow = INF;

            for (int u = sink; u != source; u = parent[u]) {
                Edge &e = adj[parent[u]][parentEdge[u]];
                pathFlow = min(pathFlow, e.capacity - e.flow);
            }

            for (int u = sink; u != source; u = parent[u]) {
                Edge &e = adj[parent[u]][parentEdge[u]];
                e.flow += pathFlow;
                adj[u][e.rev].flow -= pathFlow;
                cost += pathFlow * e.cost;
            }

            flow += pathFlow;
        }

        if (flow < maxFlow) return {-1, -1};
        return {flow, cost};
    }

    vector<vector<int>> getRoutes(int source, int sink, int k) {
        vector<vector<int>> routes;
        for (int i = 0; i < k; ++i) {
            vector<int> path;
            int u = source;

            while (u != sink) {
                path.push_back(u);
                for (auto &e : adj[u]) {
                    if (e.flow > 0) {
                        e.flow--;
                        u = e.to;
                        break;
                    }
                }
            }

            path.push_back(sink);
            routes.push_back(path);
        }

        return routes;
    }
};

```

#### assignment problem hungarian
```cpp
const int INF = 1000 * 1000 * 1000;
pair<int, vector<pair<int, int>>> hungarianAlgorithm(const vector<vector<int>>& cost) {
    int n = cost.size();
    vector<int> u(n + 1, 0), v(n + 1, 0), p(n + 1, 0), way(n + 1, 0);

    for (int i = 1; i <= n; i++) {
        vector<int> minv(n + 1, INF);
        vector<bool> used(n + 1, false);
        int j0 = 0;
        p[0] = i;
        do {
            used[j0] = true;
            int i0 = p[j0], delta = INF, j1;
            for (int j = 1; j <= n; j++) {
                if (!used[j]) {
                    int cur = cost[i0 - 1][j - 1] - u[i0] - v[j];
                    if (cur < minv[j]) {
                        minv[j] = cur;
                        way[j] = j0;
                    }
                    if (minv[j] < delta) {
                        delta = minv[j];
                        j1 = j;
                    }
                }
            }
            for (int j = 0; j <= n; j++) {
                if (used[j]) {
                    u[p[j]] += delta;
                    v[j] -= delta;
                } else {
                    minv[j] -= delta;
                }
            }
            j0 = j1;
        } while (p[j0] != 0);
        do {
            int j1 = way[j0];
            p[j0] = p[j1];
            j0 = j1;
        } while (j0);
    }

    vector<pair<int, int>> assignment;
    for (int j = 1; j <= n; j++) {
        if (p[j]) {
            assignment.emplace_back(p[j], j);
        }
    }
    return {-v[0], assignment};
}
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>


# Game theory

### Grundy number
```cpp
int computeGrundy(int x) {
    if (x == 0) return 0;
    vector<int> possibleMoves; // fill this array with all possible moves
    for (auto &i : possibleMoves) {
        possibleMoves.emplace_back(computeGrundy(i)); // Consider replacing x with new x
    }

    sort(possibleMoves.begin(), possibleMoves.end());
    possibleMoves.erase(unique(possibleMoves.begin(), possibleMoves.end()), possibleMoves.end());

    int mex = 0;
    for (int move : possibleMoves) {
        if (move == mex) {
            mex++;
        } else if (move > mex) {
            break;
        }
    }

    return mex;
}
```

### Nim
```cpp
// nim game have k piles and you choose one pile then remove some stones
// the loser who couldn't choose
// if the xor != 0 I win so my move is to make his xor == 0
// you find number has MSB same as the total Xor then any number has this MSB you can reduce it
// to make xor == 0
int Nim(vector<ll>&piles) {
    ll res = 0;
    for(auto &val:piles)res ^= val;
    return res ? 1 : 2;
}
```


### Nim with skip
```cpp
int Nim(vector<ll>&piles) {
    ll res = 0;
    for(auto &val:piles)res ^= val;
    return res ? 1 : 2;
}
int NimSkip(vector<ll>&piles , int a , int b) {
    if(a > b)return 1;
    if(b > a)return 2;
    return Nim(piles);
}
```

### MooreNimK
```cpp
// we have N piles and we can remove stones from any k piles
// represent the piles in binary and the sum in every column should be
// divisable by (k + 1)
int MooreNimK(const vector<ll>&piles , int k) {
    vector<int>bits(64);
    for(auto &val:piles) {
        for(int j = 0;j < 63;j++) {
            if(val & (1LL<<j))bits[j]++;
        }
    }
    bool all = true;
    for(auto &val:bits) {
        all = all && ((val % (k + 1)) == 0);
    }
    return all ? 2 : 1;
}
```

### Miser Nim
```cpp
// solution for Miser is same as Nim unless all piles are ones then the reverse of Nims
// so we do same strategy as Nim before we reach leaf node then we can make us win in the leaf nodes
int MiserNim(vector<ll>&piles) {
    ll res = 0;
    int ones = 0;
    for(auto &val:piles)res ^= val , ones += val == 1;
    if(ones == piles.size())res = res ? 0 : 1;
    return res ? 1 : 2;
}
```

### Nimble
```cpp
// you have n piles of stones in one move you can pick a coin and move it to any left square
// first one can't move lose
int Nimble(const vector<ll>&coins) {
    ll res = 0;
    for(int i = 0;i < coins.size();i++) {
        res ^= (coins[i] & 1) * i;
    }
    return res ? 1 : 2;
}
```

### Silver dollar game
```cpp
int SilverDollarGame (vector<ll>& locations) {
    // given N cells that has 1 coins in these positions you can move the coin to the left
    // as long as you not jumping over other coins , and no cell has more than 1 coin
    // locations = {2 , 5 , 7 , 11}
    // 1 2 3 4 5 6 7 8 9 10 11
    // 0 1 0 0 1 0 0 0 0 0 1
    if(locations.size() & 1)locations.insert(locations.begin() , 0);
    ll xoring = 0;
    for(int i = 0;i < locations.size();i+=2) {
        xoring ^= (locations[i + 1] - locations[i] - 1);
    }
    return xoring ? 1 : 2;
}
```

### star case game
```cpp
int Nim(vector<ll>&piles) {
    ll res = 0;
    for(auto &val:piles)res ^= val;
    return res ? 1 : 2;
}
int StairCase(vector<ll>&piles) {
    vector<ll>all;
    for(int i = 1;i < piles.size();i+=2) {
        all.emplace_back(piles[i]);
    }
    return Nim(all);
}
```

### Turning Turtles Game forcing
```cpp
int TurningTurtlesGame (const string &s) {
    // Given a horizontal line of N coins: Head / Tail
    // Moves
    //  - pick any head and flip it to tail
    //  - optionally , flip any coin on left of your chosen coin
    // loser who can't make flips any more
    // observation:
    // if the kth position is a head it is equalivant to pile with size k (one base)
    int xoring = 0;
    for(int i = 1;i <= s.size();i++) {
        if(s[i - 1] == 'H')xoring ^= i - 1;
    }
    return xoring ? 1 : 2;
}
```

### Turning Turtles Game optional
```cpp
int TurningTurtlesGame (const string &s) {
    // Given a horizontal line of N coins: Head / Tail
    // Moves
    //  - pick any head and flip it to tail
    //  - optionally , flip any coin on left of your chosen coin
    // loser who can't make flips any more
    // observation:
    // if the kth position is a head it is equalivant to pile with size k (one base)
    int xoring = 0;
    for(int i = 1;i <= s.size();i++) {
        if(s[i - 1] == 'H')xoring ^= i;
    }
    return xoring ? 1 : 2;
}
```

**<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

# Strings

### Hashing

#### single polyhashing
```cpp
// many collision ? make 2 hashes with different base and to make s[i] == s[j]
// then hash1(s[i]) should equal hash1(s[j]) also hash2
const uint64_t HashMod = (1ULL << 61) - 1;
const uint64_t seed = chrono::system_clock::now().time_since_epoch().count();
const uint64_t base = mt19937_64(seed)() % (HashMod / 3) + (HashMod / 3);
uint64_t base_pow[N];

int64_t MUL(uint64_t a, uint64_t b) {
    uint64_t l1 = (uint32_t) a, h1 = a >> 32, l2 = (uint32_t) b, h2 = b >> 32;
    uint64_t l = l1 * l2, m = l1 * h2 + l2 * h1, h = h1 * h2;
    uint64_t ret = (l & HashMod) + (l >> 61) + (h << 3) + (m >> 29) + (m << 35 >> 3) + 1;
    ret = (ret & HashMod) + (ret >> 61);
    ret = (ret & HashMod) + (ret >> 61);
    return (int64_t) ret - 1;
}

void init() {
    base_pow[0] = 1;
    for (int i = 1; i < N; i++) {
        base_pow[i] = MUL(base_pow[i - 1], base);
    }
}
struct PolyHash {
    /// Remove suf vector and usage if reverse hash is not required for more speed
    vector<int64_t> pref, suf;

        template<typename T>
    PolyHash(const vector<T> &ar) {
        if (!base_pow[0]) init();

        int n = ar.size();
        assert(n < N);
        pref.resize(n + 3, 0), suf.resize(n + 3, 0);

        for (int i = 1; i <= n; i++) {
            pref[i] = MUL(pref[i - 1], base) + ar[i - 1] + 997;
            if (pref[i] >= HashMod) pref[i] -= HashMod;
        }

        for (int i = n; i >= 1; i--) {
            suf[i] = MUL(suf[i + 1], base) + ar[i - 1] + 997;
            if (suf[i] >= HashMod) suf[i] -= HashMod;
        }
    }

    explicit PolyHash(string str) : PolyHash(vector<char>(str.begin(), str.end())) {}

    uint64_t get_hash(int l, int r) {
        int64_t h = pref[r + 1] - MUL(base_pow[r - l + 1], pref[l]);
        return h < 0 ? h + HashMod : h;
    }

    uint64_t rev_hash(int l, int r) {
        int64_t h = suf[l + 1] - MUL(base_pow[r - l + 1], suf[r + 2]);
        return h < 0 ? h + HashMod : h;
    }
};
```

#### double poly hashing
```cpp
const uint64_t HashMod = (1ULL << 61) - 1;
const uint64_t seed1 = chrono::system_clock::now().time_since_epoch().count();
const uint64_t base1 = mt19937_64(seed1)() % (HashMod / 3) + (HashMod / 3);
const uint64_t seed2 = chrono::system_clock::now().time_since_epoch().count() * 2ULL;
const uint64_t base2 = mt19937_64(seed2)() % (HashMod / 3) + (HashMod / 3);
uint64_t base_pow[2][N];
int64_t MUL(uint64_t a, uint64_t b) {
    uint64_t l1 = (uint32_t) a, h1 = a >> 32, l2 = (uint32_t) b, h2 = b >> 32;
    uint64_t l = l1 * l2, m = l1 * h2 + l2 * h1, h = h1 * h2;
    uint64_t ret = (l & HashMod) + (l >> 61) + (h << 3) + (m >> 29) + (m << 35 >> 3) + 1;
    ret = (ret & HashMod) + (ret >> 61);
    ret = (ret & HashMod) + (ret >> 61);
    return (int64_t) ret - 1;
}
void init() {
    base_pow[0][0] = 1;
    for (int i = 1; i < N; i++) {
        base_pow[0][i] = MUL(base_pow[0][i - 1], base1);
    }
    base_pow[1][0] = 1;
    for (int i = 1; i < N; i++) {
        base_pow[1][i] = MUL(base_pow[1][i - 1], base2);
    }

}
struct PolyHash {
    /// Remove suf vector and usage if reverse hash is not required for more speed
    vector<int64_t> pref[2], suf[2];

    template<typename T>
    PolyHash(const vector<T> &ar) {
        if (!base_pow[1][0] || !base_pow[0][0]) init();
        int n = ar.size();
        assert(n < N);
        pref[0].resize(n + 3, 0), suf[0].resize(n + 3, 0);
        pref[1].resize(n + 3, 0), suf[1].resize(n + 3, 0);
        for (int i = 1; i <= n; i++) {
            pref[0][i] = MUL(pref[0][i - 1], base1) + ar[i - 1] + 997;
            if (pref[0][i] >= HashMod) pref[0][i] -= HashMod;
            pref[1][i] = MUL(pref[1][i - 1], base2) + ar[i - 1] + 997;
            if (pref[1][i] >= HashMod) pref[1][i] -= HashMod;
        }

        for (int i = n; i >= 1; i--) {
            suf[0][i] = MUL(suf[0][i + 1], base1) + ar[i - 1] + 997;
            if (suf[0][i] >= HashMod) suf[0][i] -= HashMod;
            suf[1][i] = MUL(suf[1][i + 1], base2) + ar[i - 1] + 997;
            if (suf[1][i] >= HashMod) suf[1][i] -= HashMod;
        }
    }

    explicit PolyHash(string str) : PolyHash(vector<char>(str.begin(), str.end())) {}

    pair<uint64_t, uint64_t> get_hash(int l, int r) {
        int64_t h = pref[0][r + 1] - MUL(base_pow[0][r - l + 1], pref[0][l]);
        uint64_t f = h < 0 ? h + HashMod : h;
        h = pref[1][r + 1] - MUL(base_pow[1][r - l + 1], pref[1][l]);
        uint64_t g = h < 0 ? h + HashMod : h;
        return make_pair(f, g);
    }

    pair<uint64_t, uint64_t> rev_hash(int l, int r) {
        int64_t h = suf[0][l + 1] - MUL(base_pow[0][r - l + 1], suf[0][r + 2]);
        uint64_t f = h < 0 ? h + HashMod : h;
        h = suf[1][l + 1] - MUL(base_pow[1][r - l + 1], suf[1][r + 2]);
        uint64_t k = h < 0 ? h + HashMod : h;
        return make_pair(f, k);
    }
};
```

#### random long long 
```cpp
long long randomLongLong(long long l, long long r) {
    random_device rd;
    mt19937_64 rng(rd());
    uniform_int_distribution<long long> dist(l, r);
    return dist(rng);
}
```

#### custom unordered set
```cpp
struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        // http://xorshift.di.unimi.it/splitmix64.c
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};

// to initalize
//unordered_set<ll, custom_hash> s;
```

#### custom unordered map
```cpp
struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        // http://xorshift.di.unimi.it/splitmix64.c
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};

// to initalize
//unordered_map<ll, int, custom_hash> mp;
```

### Kmp
```cpp
struct KMP {
    //keywords: substring , prefix , suffix , equality
    //typically processing for the pattern similar to the input
    //try: s , s + s , rev(s) , s + 'a' + s ,....

    vector<int> failure;

    void build(const string &s) {
        // failure[i] = length of the longest proper prefix of s[0..i] that is 
        // also a suffix of this substring
        int n = (int) s.size();
        if (failure.size() <= s.size())failure.clear(), failure.resize(n);
        for (int i = 1; i < n; i++) {
            int j = failure[i - 1];
            while (j > 0 && s[i] != s[j])
                j = failure[j - 1];
            if (s[i] == s[j])
                j++;
            failure[i] = j;
        }
    }

    static vector<int> pattern(const string &s, const string &t) {
    	KMP temp;
        temp.build(t);
        int n = (int) s.size(), m = (int) t.size(), k = 0;
        vector<int> pos;
        for (int i = 0; i < n; i++) {
            while (k > 0 && s[i] != t[k]) {
                k = temp.failure[k - 1];
            }
            if (s[i] == t[k])k++;
            if (k == m) {
                pos.emplace_back(i - m + 1);
                k = temp.failure[k - 1];
            }
        }
        return pos;
    }

    static pair<int , string> LongestPalndromeSuffix(const string &s) {
        KMP temp;
        string rev = s;
        reverse(rev.begin(), rev.end());
        string x = rev + '@' + s;
        temp.build(x);
        int res = temp.failure[x.size() - 1];
   		string t = s.substr(0 , s.size() - res);
   		reverse(t.begin() , t.end());
   		string res2 = s + t;
   		return make_pair(res , res2);
    }

    static pair<int,string> LongestPalndromePrefix(const string &s) {
        KMP temp;
        string rev = s;
        reverse(rev.begin(), rev.end());
        string x = s + '@' + rev;
        temp.build(x);
        int res = temp.failure[x.size() - 1];
    	string t = s.substr(res) ;
    	reverse(t.begin() , t.end());
   		string res1 = t + s;
   		return make_pair(res , res1);
    }

    static int MinUnit(const string &s) {
        // what is the smallest prefix can build up the string
        KMP temp;
        temp.build(s);
        if (s.size() % (s.size() - temp.failure[s.size() - 1]) != 0)return (int) s.size();
        return (int) s.size() - temp.failure[s.size() - 1];
    }

    static int MinUnitVersion2(const string &s) {
        KMP temp;
        string p = s + s;
        vector<int> res = temp.pattern(p, s);
        if (res.size() >= 2)return res[1];
        else return (int) s.size();
    }

    static vector<int> FreqAllPrefixes(const string &s) {
        // count for every prefix it's frequency as sub string and count it self
        KMP temp;
        temp.build(s);
        vector<int> freq(s.size() + 1);
        for (int i = 0; i < s.size(); i++) {
            ++freq[temp.failure[i]];
        }
        for (int i = (int) s.size() - 1; i > 0; i--) {
            freq[temp.failure[i - 1]] += freq[i];
        }
        freq.erase(freq.begin());
        for (auto &val: freq)val++;
        return freq;
    }
    static int CountUniquePrefixes(const string &s){
        vector<int> temp = KMP::FreqAllPrefixes(s);
        return count(temp.begin() , temp.end() , 1);
    }
    static int UniqueSubStrings(const string &s){
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
};
```

### minimal rotation 
```cpp
int findMinRotation(string s){
    // Concatenate string to itself to avoid modular
    // arithmetic
    s += s;

    // Initialize failure function
    vector<int> failureFunc(s.size(), -1);

    // Initialize least rotation of string found so far
    int minRotationIdx = 0;

    // Iterate over the concatenated string
    for (int currentIdx = 1; currentIdx < s.size();
         ++currentIdx) {
        char currentChar = s[currentIdx];
        int failureIdx
            = failureFunc[currentIdx - minRotationIdx - 1];

        // Find the failure function value
        while (
            failureIdx != -1
            && currentChar
                   != s[minRotationIdx + failureIdx + 1]) {
            if (currentChar
                < s[minRotationIdx + failureIdx + 1]) {
                minRotationIdx
                    = currentIdx - failureIdx - 1;
                }
            failureIdx = failureFunc[failureIdx];
                   }

        // Update the failure function and the minimum
        // rotation index
        if (currentChar
            != s[minRotationIdx + failureIdx + 1]) {
            if (currentChar < s[minRotationIdx]) {
                minRotationIdx = currentIdx;
            }
            failureFunc[currentIdx - minRotationIdx] = -1;
            }
        else {
            failureFunc[currentIdx - minRotationIdx]
                = failureIdx + 1;
        }
         }

    // Return the index of the lexicographically minimal
    // rotation
    return minRotationIdx;
}
```

### Z function
```cpp
vector<int> zFunction(string s) {
    //it computes for every index i starts at i what is the largest string is matching a prefix
    int n = (int)s.size();
    vector<int> z(n);
    int left = 0, right = 0;
    for(int i = 1; i < n; i++) {
        if(i <= right) {
            z[i] = min(right - i + 1, z[i - left]);
        }
        while(i + z[i] < n && s[z[i]] == s[z[i] + i]) {
            z[i]++;
        }
        if(i + z[i] - 1 > right) {
            left = i;
            right = i + z[i] - 1;
        }
    }
    return z;
}
```

### Manacher
```cpp
struct Manacher {
	vector<int>p;
	void build(string &s) {
		string t;
		for(auto &val:s)t += '#' , t += val;
		t += '#';
		int n = (int)t.size();
		p.resize(n , 1);
		int l = 1 , r = 1;
		for(int i = 1;i < n;i++) {
			p[i] = max(0 , min(r - i , p[l + r - i]));
			while(i - p[i] >= 0 and i + p[i] < n and t[i + p[i]] == t[i - p[i]])p[i]++;
			if(p[i] + i > r) {
				r = i + p[i];
				l = i - p[i];
			}
		}
	}
	int GetLongest(int center , bool odd = true) {
		int NewCenter = 2 * center + !odd + 1;
		return p[NewCenter] - 1;
	}
	bool IsPalndrome(int l , int r) {
		return (r - l + 1) <= GetLongest((l + r) / 2 , l % 2 == r % 2);
	}
};

```

### suffix array
#### fast template
```cpp
int n;
string s;
vector<int> suf, group, NewGroup, tempSuf;
int curLen = 0;

void countingSort(int maxGroup) {
    vector<int> count(maxGroup + 1, 0);
    tempSuf.resize(n);

    for (int i = 0; i < n; i++) {
        count[group[min(n - 1, suf[i] + curLen)]]++;
    }
    for (int i = 1; i <= maxGroup; i++) {
        count[i] += count[i - 1];
    }
    for (int i = n - 1; i >= 0; i--) {
        tempSuf[--count[group[min(n - 1, suf[i] + curLen)]]] = suf[i];
    }
    for (int i = 0; i < n; i++) {
        suf[i] = tempSuf[i];
    }
}

void radixSort() {
    int maxGroup = max(n, 256);  // Initial max group value for radix sort

    // Sort by the second key (group[i + curLen])
    countingSort(maxGroup);

    // Sort by the first key (group[i])
    curLen = 0;
    countingSort(maxGroup);
}

void build() {
    suf.clear();
    group.clear();
    NewGroup.clear();
    n = (int)s.size();
    NewGroup.resize(n + 1);
    n++;
    for (int i = 0; i < n; i++) {
        suf.emplace_back(i);
        group.emplace_back(s[i]);
    }
    for (int len = 1;; len *= 2) {
        curLen = len;
        radixSort();
        for (int i = 1; i < n; i++) {
            NewGroup[i] = NewGroup[i - 1] + (group[suf[i - 1]] != group[suf[i]] || group[suf[i - 1] + len] != group[suf[i] + len]);
        }
        for (int i = 0; i < n; i++) {
            group[suf[i]] = NewGroup[i];
        }
        if (NewGroup[n - 1] == n - 1) break;
    }
}
vector<int>lcp;
void BuildLcp() {
    lcp.resize(n);
    int k = 0;
    for (int i = 0; i < n - 1; i++) {
        int pi = group[i] - 1; // Corrected index for accessing the previous suffix
        int j = suf[pi];
        while (s[j + k] == s[i + k]) k++;
        lcp[pi] = k; // Store the LCP value at the correct index
        if (k > 0) k--;
    }

}
```

#### suffix array neal
```cpp
// the suffix array doesn't has empty suffix and lcp compare suf[i] , suf[i + 1] , first entrie in it is dummy
template<typename T, bool maximum_mode = false>
struct RMQ {
    int n = 0;
    vector<vector<T>> range_min;

    RMQ(const vector<T> &values = {}) {
        if (!values.empty())
            build(values);
    }

    static int largest_bit(int x) {
        return 31 - __builtin_clz(x);
    }

    static T better(T a, T b) {
        return maximum_mode ? max(a, b) : min(a, b);
    }

    void build(const vector<T> &values) {
        n = int(values.size());
        int levels = largest_bit(n) + 1;
        range_min.resize(levels);

        for (int k = 0; k < levels; k++)
            range_min[k].resize(n - (1 << k) + 1);

        if (n > 0)
            range_min[0] = values;

        for (int k = 1; k < levels; k++)
            for (int i = 0; i <= n - (1 << k); i++)
                range_min[k][i] = better(range_min[k - 1][i], range_min[k - 1][i + (1 << (k - 1))]);
    }

    T query_value(int a, int b) const {
        assert(0 <= a && a < b && b <= n);
        int level = largest_bit(b - a);
        return better(range_min[level][a], range_min[level][b - (1 << level)]);
    }
};
template<typename T_string = string>
struct suffix_array {
    int n = 0;
    T_string str;
    vector<int> suffix;
    vector<int> rank;
    vector<int> lcp;
    RMQ<int , false> rmq;
    suffix_array() {}

    suffix_array(const T_string &_str,bool build_lcp = true, bool build_rmq = true) {
        build(_str,build_lcp ,build_rmq);
    }

    void build(const T_string &_str,bool build_lcp = true , bool build_rmq = true) {
        str = _str;
        n = int(str.size());
        suffix.resize(n);

        for (int i = 0; i < n; i++)
            suffix[i] = i;

        bool large_alphabet = false;

        for (int i = 0; i < n; i++)
            if (str[i] < 0 || str[i] >= 128)
                large_alphabet = true;

        // Sort each suffix by the first character.
        if (large_alphabet) {
            sort(suffix.begin(), suffix.end(), [&](int a, int b) {
                return str[a] < str[b];
            });
        } else {
            vector<int> freq(128, 0);

            for (int i = 0; i < n; i++)
                freq[str[i]]++;

            for (int c = 1; c < 128; c++)
                freq[c] += freq[c - 1];

            for (int i = 0; i < n; i++)
                suffix[--freq[str[i]]] = i;
        }

        // Compute the rank of each suffix. Tied suffixes share the same rank.
        rank.resize(n);
        rank[suffix[0]] = 0;

        for (int i = 1; i < n; i++)
            rank[suffix[i]] = str[suffix[i]] == str[suffix[i - 1]] ? rank[suffix[i - 1]] : i;

        vector<int> next_index(n);
        vector<int> values(n);
        bool done = false;

        for (int len = 1; len < n && !done; len *= 2) {
            // next_index[i] = the next index to use for a suffix of rank i. We insert them in order of the rank of the
            // suffix that comes len characters after the current suffix.
            for (int i = 0; i < n; i++)
                next_index[i] = i;

            // Compute the suffix array for 2 * len. Suffixes of length <= len are prioritized first.
            for (int i = n - len; i < n; i++)
                values[next_index[rank[i]]++] = i;

            for (int i = 0; i < n; i++) {
                int prev = suffix[i] - len;

                if (prev >= 0)
                    values[next_index[rank[prev]]++] = prev;
            }

            swap(suffix, values);

            // Compute the rank array for 2 * len.
            values[suffix[0]] = 0;
            done = true;

            for (int i = 1; i < n; i++) {
                int s = suffix[i], prev = suffix[i - 1];

                if (s + len < n && prev + len < n && rank[s] == rank[prev] && rank[s + len] == rank[prev + len]) {
                    values[s] = values[prev];
                    done = false;
                } else {
                    values[s] = i;
                }
            }

            swap(rank, values);
        }

        if(build_lcp)compute_lcp();

        if (build_rmq && build_lcp)
            rmq.build(lcp);
    }

    void compute_lcp() {
        lcp.assign(n, 0);
        int match = 0;

        for (int i = 0; i < n; i++) {
            if (rank[i] == 0)
                continue;

            int a = suffix[rank[i]] + match;
            int b = suffix[rank[i] - 1] + match;

            while (a < n && b < n && str[a++] == str[b++])
                match++;

            // lcp[r] = the longest common prefix length of the suffixes starting at suffix[r] and at suffix[r - 1].
            // Note that lcp[0] is always 0.
            lcp[rank[i]] = match;
            match = max(match - 1, 0);
        }
    }

    int get_lcp_from_ranks(int a, int b) const {
        if (a == b)
            return n - suffix[a];

        if (a > b)
            swap(a, b);

        return rmq.query_value(a + 1, b + 1);
    }

    int get_lcp(int a, int b) const {
        if (a >= n || b >= n)
            return 0;

        if (a == b)
            return n - a;

        return get_lcp_from_ranks(rank[a], rank[b]);
    }

    // Compares the substrings starting at `a` and `b` up to `length` in O(1).
    int compare(int a, int b, int length = -1) const {
        if (length < 0)
            length = n;

        if (a == b)
            return 0;

        int common = get_lcp(a, b);

        if (common >= length)
            return 0;

        if (a + common >= n || b + common >= n)
            return a + common >= n ? -1 : 1;

        return str[a + common] < str[b + common] ? -1 : (str[a + common] == str[b + common] ? 0 : 1);
    }
    int compare(int i1 , int j1 , int i2 , int j2) {
        array<int , 2>a{i1 , j1} , b{i2 , j2};
        int shorter = min(a[1] - a[0], b[1] - b[0]);
        int comp = compare(a[0], b[0], ++shorter);

        if (comp != 0)
            return comp;
        if(a[1] - a[0] == b[1] - b[0])return 0;
        return a[1] - a[0] < b[1] - b[0] ? -1 : 1;
    }
};
```

### Aho-corasick

#### kactal 
```cpp
/**
 * Description: Aho-Corasick automaton, used for multiple pattern matching.
 * Initialize with AhoCorasick ac(patterns); the automaton start node will be at index 0.
 * find(word) returns for each position the index of the longest word that ends there, or -1 if none.
 * findAll($-$, word) finds all words (up to $N \sqrt N$ many if no duplicate patterns)
 * that start at each position (shortest first).
 * Duplicate patterns are allowed; empty patterns are not.
 * To find the longest words that start at each position, reverse all input.
 * For large alphabets, split each symbol into chunks, with sentinel bits for symbol boundaries.
 * Time: construction takes $O(N * ALPHA)$, where $N =$ sum of length of patterns.
 * find(x) is $O(N)$, where N = length of x. findAll is $O(NM)$.
 */
struct AhoCorasick {
	enum {alpha = 26, first = 'A'}; // change this!
	struct Node {
		// (nmatches is optional)
		int back, next[alpha], start = -1, end = -1, nmatches = 0;
		Node(int v) { memset(next, v, sizeof(next)); }
	};
	vector<Node> N;
	vector<int> backp;
	void insert(string& s, int j) {
		assert(!s.empty());
		int n = 0;
		for (char c : s) {
			int& m = N[n].next[c - first];
			if (m == -1) { n = m = N.size(); N.emplace_back(-1); }
			else n = m;
		}
		if (N[n].end == -1) N[n].start = j;
		backp.push_back(N[n].end);
		N[n].end = j;
		N[n].nmatches++;
	}
	AhoCorasick(vector<string>& pat) : N(1, -1) {
		for(int i = 0;i < pat.size();i++) insert(pat[i], i);
		N[0].back = N.size();
		N.emplace_back(0);

		queue<int> q;
		for (q.push(0); !q.empty(); q.pop()) {
			int n = q.front(), prev = N[n].back;
			for(int i= 0;i < alpha;i++) {
				int &ed = N[n].next[i], y = N[prev].next[i];
				if (ed == -1) ed = y;
				else {
					N[ed].back = y;
					(N[ed].end == -1 ? N[ed].end : backp[N[ed].start])
						= N[y].end;
					N[ed].nmatches += N[y].nmatches;
					q.push(ed);
				}
			}
		}
	}
	vector<int> find(string word) {
		int n = 0;
		vector<int> res; // ll count = 0;
		for (char c : word) {
			n = N[n].next[c - first];
			res.push_back(N[n].end);
			// count += N[n].nmatches;
		}
		return res;
	}
	vector<vector<int>> findAll(vector<string>& pat, string word) {
		vector<int> r = find(word);
		vector<vector<int>> res(word.size());
		for(int i = 0;i < word.size();i++) {
			int ind = r[i];
			while (ind != -1) {
				res[i - pat[ind].size() + 1].push_back(ind);
				ind = backp[ind];
			}
		}
		return res;
	}
};

```

#### khalwsh 
```cpp
struct AhoCorasick {
    // make sure the patterns are unique
    /*  // iterating over the outlinks
        int u = 0;
        for (int i = 0; i < n; i++) {
            char c = s[i];
            u = aho.advance(u, c);
            for (int node = u; node; node = aho.out_link[node]) {
                for (auto PatIdx : aho.out[node]) {
                    res[PatIdx]++;
                }
            }
        }
     */
    int N, P;
    const int A = 128;
    vector <vector <int>> next;
    vector <int> link, out_link;
    vector <vector <int>> out;
    AhoCorasick(): N(0), P(0) {node();}
    int node() {
        next.emplace_back(A, 0);
        link.emplace_back(0);
        out_link.emplace_back(0);
        out.emplace_back(0);
        return N++;
    }
    inline int get (char c) {
        return c;
    }
    int add_pattern (const string &T) {
        int u = 0;
        for (auto c : T) {
            if (!next[u][get(c)]) next[u][get(c)] = node();
            u = next[u][get(c)];
        }
        out[u].push_back(P);
        return P++;
    }
    void compute() {
        queue <int> q;
        for (q.push(0); !q.empty();) {
            int u = q.front(); q.pop();
            for (int c = 0; c < A; ++c) {
                int v = next[u][c];
                if (!v) next[u][c] = next[link[u]][c];
                else {
                    link[v] = u ? next[link[u]][c] : 0;
                    out_link[v] = out[link[v]].empty() ? out_link[link[v]] : link[v];
                    q.push(v);
                }
            }
        }
    }
    int advance (int u, char c) {
        while (u && !next[u][get(c)]) u = link[u];
        u = next[u][get(c)];
        return u;
    }
    vector<vector<int>> AllPos(string &text , vector<string>&patterns) {
        P = 0;
        int n = (int)patterns.size();
        map<string,int>last;
        vector<int>ans(n);
        for(int i = 0;i < patterns.size();i++) {
            if(last.count(patterns[i]))ans[i] = last[patterns[i]] , patterns[i] = "";
            else last[patterns[i]] = i , ans[i] = i;
        }
        vector<int> len(n + 3, 0);
        for (auto &pat: patterns) {
            if(pat == "")P++;
            else len[add_pattern(pat)] = (int)pat.size();
        }
        compute();
        vector<vector<int>>res(n);
        n = (int)text.size();
        int u = 0;
        for (int i = 0; i < n; i++) {
            char c = text[i];
            u = advance(u, c);
            for (int node = u; node; node = out_link[node]) {
                for (auto PatIdx : out[node]) {
                    res[PatIdx].emplace_back(i - len[PatIdx] + 1);
                }
            }
        }
        for(int i = 0;i < res.size();i++) {
            res[i] = res[ans[i]];
        }
        return res;
    }
    vector<int> match(string &text , vector<string>&patterns) {
        P = 0;
        int n = (int)patterns.size();
        map<string,int>last;
        vector<int>ans(n);
        for(int i = 0;i < patterns.size();i++) {
            if(last.count(patterns[i]))ans[i] = last[patterns[i]] , patterns[i] = "$";
            else last[patterns[i]] = i , ans[i] = i;
        }
        vector<int> len(n + 3, 0);
        for (auto &pat: patterns) {
            if(pat == "$")P++;
            else len[add_pattern(pat)] = (int)pat.size();
        }
        compute();
        vector<int>res(n);
        n = (int)text.size();
        int u = 0;
        for (int i = 0; i < n; i++) {
            char c = text[i];
            u = advance(u, c);
            for (int node = u; node; node = out_link[node]) {
                for (auto PatIdx : out[node]) {
                    res[PatIdx]++;
                }
            }
        }
        for(int i = 0;i < res.size();i++) {
            res[i] = res[ans[i]];
        }
        return res;
    }
    vector<int> FirstPos(string &text , vector<string>&patterns) {
        P = 0;
        int n = (int)patterns.size();
        map<string,int>last;
        vector<int>ans(n);
        for(int i = 0;i < patterns.size();i++) {
            if(last.count(patterns[i]))ans[i] = last[patterns[i]] , patterns[i] = "";
            else last[patterns[i]] = i , ans[i] = i;
        }
        vector<int> len(n + 3, 0);
        for (auto &pat: patterns) {
            if(pat == "")P++;
            else len[add_pattern(pat)] = (int)pat.size();
        }
        compute();
        vector<int>res(n , 1e9);
        n = (int)text.size();
        int u = 0;
        for (int i = 0; i < n; i++) {
            char c = text[i];
            u = advance(u, c);
            for (int node = u; node; node = out_link[node]) {
                for (auto PatIdx : out[node]) {
                    res[PatIdx] = min(res[PatIdx] , i - len[PatIdx] + 1);
                }
            }
        }
        for(int i = 0;i < res.size();i++)res[i] = (res[i] == 1e9 ? -2 : res[i]);

        for(int i = 0;i < res.size();i++) {
            res[i] = res[ans[i]];
        }
        return res;
    }
};
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>


# Trees

### Edge classification
```cpp
int bridges,lvl [N],dp [N];
vector<int> adj [N];
void dfs (int vertex) {
    dp[vertex] = 0;
    for (int nxt : adj[vertex]) {
        if (lvl[nxt] == 0) { 
            //parent child edge
            lvl[nxt] = lvl[vertex] + 1;
            dfs(nxt);
            dp[vertex] += dp[nxt];
        } else if (lvl[nxt] < lvl[vertex]) {
            //backward edge [vertex,nxt]
            dp[vertex]++;
        } else if (lvl[nxt] > lvl[vertex]) {
            //forward edge [vertex,nxt]
            dp[vertex]--;
        }
    }
//  the parent edge isn't a back-edge, subtract 1 to compensate 
    dp[vertex]--;
    if (lvl[vertex] > 1 && dp[vertex] == 0) {
        bridges++;
    }
}
```

### Tree diameter
```cpp
pair<int, int> calc(int u, int par = -1) {
    int diam = 0;
    int mxHeights[3] = {-1, -1, -1};    // keep 2 highest trees
    for (auto &v: adj[u])
        if (v != par) {
            auto p = calc(v, u);
            diam = max(diam, p.first);
            mxHeights[0] = p.second + 1;
            sort(mxHeights, mxHeights + 3);
        }
    for (int i = 0; i < 3; i++)
        if (mxHeights[i] == -1)
            mxHeights[i] = 0;
    diam = max(diam, mxHeights[1] + mxHeights[2]);
    return {diam, mxHeights[2]};
}
int diameter(int root){
    return calc(root).first;
}
```

### Tree linearization

#### range subtree
```cpp
int LinearTree[2*N],ST[N],EN[N],Timer = 0;
vector<int>adj[N];
void Linear(int node,int par) {
    LinearTree[Timer] = node;
    ST[node] = Timer;
    Timer++;
    for (auto &val: adj[node]) {
        if (val == par)continue;
        Linear(val, node);
    }
    EN[node] = Timer - 1;
}
```

#### range path
```cpp
int LinearTree[2*N],ST[N],EN[N],Timer = 0;
vector<int>adj[N];
void Linear(int node,int par) {
    LinearTree[Timer] = node;
    ST[node] = Timer;
    Timer++;
    for (auto &val: adj[node]) {
        if (val == par)continue;
        Linear(val, node);
    }
    LinearTree[Timer] = node;
    EN[node] = Timer;
    Timer++;
}
```

#### Euler tour
```cpp
int Euler[N],Ind[N],Timer = 0;
vector<int>adj[N];
void dfs(int node,int par){
    Euler[Timer] = node;
    Ind[node] = Timer;
    Timer++;
    for(auto &val:adj[node]){
        if(val==par)continue;
        dfs(val,node);
        Euler[Timer++] = node;
    }
}
```

### LCA

#### Euler tour o(1)
```cpp
vector<int> adj[N];
vector<int> euler_tour;
int n;
int in[4 * N] , Timer = 0;
void dfs(int u = 0 , int p = 0){
    in[u] = Timer++;
    euler_tour.emplace_back(u);
    for(auto &v : adj[u]) if(v != p){
            dfs(v , u);
            euler_tour.emplace_back(u);
            Timer++;
        }
}
struct SparseTable {
    vector<int> log;
    vector<vector<pair<int, int>>> spt;
    void init(int _n) {
        int k = 20;
        spt = vector<vector<pair<int, int>>>(k, vector<pair<int, int>>(_n));
        for (int i = 0; i < _n; i++) {
            spt[0][i] = { in[euler_tour[i]] , euler_tour[i] };
        }
        for (int j = 1; 1 << j <= _n; j++) {
            for (int i = 0; i + (1 << j) - 1 < _n; i++) {
                spt[j][i] = merge(spt[j - 1][i], spt[j - 1][i + (1 << (j - 1))]);
            }
        }
    }
    pair<int, int> merge(pair<int, int> &x, pair<int, int> &y) {
        if(x.first < y.first) return x;
        return y;
    }
    pair<int, int> query(int i, int j) {
        int len = j - i + 1;
        int k = log2_floor(len);
        return merge(spt[k][i], spt[k][j - (1 << k) + 1]);
    }
    int LCA(int i , int j){
        if(in[i] > in[j]) swap(i , j);
        return query(in[i] , in[j]).second;
    }
};
SparseTable spt;
void init(){
    Timer = 0;
    euler_tour.clear()
}
void build(){
    dfs();
    spt.init(Timer);
}
```

#### Lca binary lifting
```cpp
const int Log = 21;
int up[N][Log],depth[N],parent[N],root = 0;
vector<int>adj[N];
void rooting(){
    queue<pair<int,int>>q;
    q.push({root,-1});
    parent[root] = root;
    while(!q.empty()){
        int node = q.front().first;
        int par = q.front().second;
        q.pop();
        for(auto &val:adj[node]){
            if(val==par)continue;
            parent[val] = node;
            q.push({val,node});
        }
    }
}
void MarkDepth(int node,int par,int Dist = 0) {
    depth[node] = Dist;
    for (auto &val: adj[node]) {
        if (val == par)continue;
        MarkDepth(val, node,  Dist+ 1);
    }
}
void build(int n) {
    MarkDepth(root, -1);
    for (auto &val: up)for (auto &i: val)i = -1;
    parent[root] = root;// to handle non-existing edges
    for (int i = 0; i < n; i++)up[i][0] = parent[i];
    for (int i = 1; i < Log; i++) {
        for (int j = 0; j < n; j++) {
            up[j][i] = up[up[j][i - 1]][i - 1];
        }
    }
}
int walk(int node,int k) {
    if (depth[node] < k)return -1;
    for (int i = 0; i < Log; i++) {
        if (k & (1 << i))node = up[node][i];
    }
    return node;
}
int LCA(int a,int b){
    if(depth[a]<depth[b])swap(a,b);
    a = walk(a,depth[a] - depth[b]);
    if(a==b)return a;
    for(int i = Log - 1;i>=0;i--){
        if(up[a][i] != up[b][i]){
            a = up[a][i];
            b = up[b][i];
        }
    }
    assert(parent[a]==parent[b]);
    return parent[a];
}
int dist(int a,int b){
    return depth[a] + depth[b] - 2 * depth[LCA(a,b)];
}
void init(){
    for(int i=0;i<n;i++)adj[i].clear();
}
```

#### common nodes in path
```cpp
int CalculateCommonNodesInPaths(int f, int s, int t){
    //s is source one
    //t is source two
    //f is the destination
    int ans = 0;
    bool is1 = lca(f, s) == f, is2 = lca(f, t) == f;
    if(is1 != is2)  return 1;
    if(is1)
        ans = max(ans, h[ lca(s, t) ] - h[ f ]);
    else if(lca(f, s) != lca(f, t))
        ans = max(ans, h[ f ] - max(h[ lca(f, s) ], h[ lca(f, t) ]));
    else
        ans = max(ans, h[ f ] + h[ lca(s, t) ] - 2 * h[ lca(f, t) ]);
    return ans + 1;
}
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>


### HLD
```cpp
// note you need to apply changes to segment tree logic
template<typename T>
struct SegmentTree {
    vector<T> tree, lazy;
    int n;

    void init(int _n) {
        this->n = _n;
        tree.assign(4 * n, 0);
        lazy.assign(4 * n, 0);
    }

    static T Merge(T a, T b) {
        return max(a, b);
    }

    void prop(int node, int nl, int nr) {
        if (lazy[node] != 0) {
            tree[node] += lazy[node];
            if (nl != nr) {
                lazy[2 * node + 1] += lazy[node];
                lazy[2 * node + 2] += lazy[node];
            }
            lazy[node] = 0;
        }
    }

    void modify(int node, int nl, int nr, int l, int r, T value) {
        prop(node, nl, nr);
        if (nl > r || nr < l) return;
        if (nl >= l && nr <= r) {
            lazy[node] += value;
            prop(node, nl, nr);
            return;
        }
        int mid = nl + (nr - nl) / 2;
        modify(2 * node + 1, nl, mid, l, r, value);
        modify(2 * node + 2, mid + 1, nr, l, r, value);
        tree[node] = Merge(tree[2 * node + 1], tree[2 * node + 2]);
    }

    T query(int node, int nl, int nr, int l, int r) {
        prop(node, nl, nr);
        if (nl > r || nr < l) return -1e15; // Neutral value for max operation
        if (nl >= l && nr <= r) return tree[node];
        int mid = nl + (nr - nl) / 2;
        return Merge(query(2 * node + 1, nl, mid, l, r), query(2 * node + 2, mid + 1, nr, l, r));
    }

    void set(int idx, T value) {
        modify(0, 0, n - 1, idx, idx, value);
    }

    void modify(int l, int r, T value) {
        modify(0, 0, n - 1, l, r, value);
    }

    T query(int l, int r) {
        return query(0, 0, n - 1, l, r);
    }
};

template <class T>
class HeavyLight {
    SegmentTree<T> seg;
    bool Edge = false;
    vector<int> parent, heavy, depth, root, treePos;
    vector<int> siz;
    int cur_pos = 0;

    template <class G>
    int dfs(const G& graph, int v) {
        int size = 1, maxSubtree = 0;
        for (int u : graph[v]) if (u != parent[v]) {
            parent[u] = v;
            depth[u] = depth[v] + 1;
            int subtree = dfs(graph, u);
            if (subtree > maxSubtree) heavy[v] = u, maxSubtree = subtree;
            size += subtree;
        }
        siz[v] = size;
        return size;
    }

    template <class BinaryOperation>
    void processPath(int u, int v, BinaryOperation op) {
        for (; root[u] != root[v]; v = parent[root[v]]) {
            if (depth[root[u]] > depth[root[v]]) swap(u, v);
            op(treePos[root[v]], treePos[v]);
        }
        if (depth[u] > depth[v]) swap(u, v);
        if(treePos[u] == treePos[v] && !Edge || treePos[u] != treePos[v])
            op(treePos[u] + Edge, treePos[v]);
    }

    template <class G>
    void decompose(int v, int h, const G& adj) {
        root[v] = h;
        treePos[v] = cur_pos++;
        if (heavy[v] != -1) {
            decompose(heavy[v], h, adj);
        }
        for (int u : adj[v]) {
            if (u != parent[v] && u != heavy[v]) {
                decompose(u, u, adj);
            }
        }
    }
public:
    template <class G>
    void init(const G& graph, bool OnEdge = false) {
        Edge = OnEdge;
        int n = graph.size();
        parent.resize(n);
        heavy.resize(n, -1);
        depth.resize(n);
        root.resize(n);
        treePos.resize(n);
        siz.resize(n);
        parent[0] = -1;
        depth[0] = 0;
        dfs(graph, 0);
        decompose(0, 0, graph);
        seg.init(n);
    }

    void set(int v, const T& value) {
        seg.set(treePos[v], value);
    }

    void modifyPath(int u, int v, const T& value) {
        processPath(u, v, [this, &value](int l, int r) { seg.modify(l, r, value); });
    }

    void modifySubtree(int v, T value) {
        seg.modify(treePos[v], treePos[v] + siz[v] - 1, value);
    }
    T queryPath(int u, int v) {
        T res = -1e15;
        processPath(u, v, [this, &res](int l, int r) { res = SegmentTree<T>::Merge(res, seg.query(l, r)); });
        return res;
    }
};
```

# Data structures

### Fenwick Tree

#### basic fenwick
```cpp
template<class T>
struct Fenwick{
    int n;
    vector<T>tree;
    int N = 1;
    void init(int _n){
        n=_n;
        tree.resize(this->n);
        N = log2_floor(n) + 1;
    }
    void add(int pos,T value){
        for(int i=pos+1;i<=n;i+=i&-i)tree[i-1]+=value;
    }
    T get(int pos) {
        T sum = 0;
        for (int i = pos + 1; i; i -= i & -i)sum += tree[i - 1];
        return sum;
    }
    T query(int l,int r){
        return get(r)-get(l-1);//send zero base
    }
    int lower_bound(T t){
        T sum = 0;
        int pos = 0;
        for(int i = N; i >= 0; i--){
            int next_pos = pos + (1 << i);
            if(next_pos <= n && sum + tree[next_pos - 1] < t){
                sum += tree[next_pos - 1];
                pos = next_pos;
            }
        }
        return pos; // zero-based index
    }
};
```

#### range update bit
```cpp
struct BitRange{
    //expected from user to deal with zero base
    int N;
    vector<int>m,c;
    void init(int x){
        N=x;
        m.resize(N),c.resize(N);
    }
    void add(int pos,int mVal,int cVal){
        for(++pos;pos<=N;pos+=pos&-pos){
            m[pos-1]+=mVal;
            c[pos-1]+=cVal;
        }
    }
    int get(int pos){
        int ret=0;
        int x=pos;
        for(pos++;pos;pos-=pos&-pos){
            ret+=m[pos-1]*x+c[pos-1];
        }
        return ret;
    }
    void addRange(int l,int r,int value){
        add(l,value,-value*(l-1));
        add(r+1,-value,value*r);
    }
};

```

#### multiset fenwick

```cpp
// you can't insert zeros and you can't insert negative make shift or compression

struct Bit{
    int N=1<<20;
    vector<int>tree;
    void init(){
        tree.resize(this->N);
    }
    void add(int pos,int value){
        for(int i=pos+1;i<=N;i+=i&-i)tree[i-1]+=value;
    }
    int get(int pos) {
        int sum = 0;
        for (int i = pos + 1; i; i -= i & -i)sum += tree[i - 1];
        return sum;
    }
    int find(int t){
        int st=0;
        for(int sz=N>>1;sz;sz>>=1){
            if(tree[st+sz-1]<t){
                t-=tree[(st+=sz)-1];
            }
        }
        return st;
    }
};
struct MultiSet{
    Bit bit;
    MultiSet(){
        bit.init();
        bit.add(0,-1);
    }
    void insert(int value){
        bit.add(value,1);
    }
    void erase(int value){
        bit.add(value,-1);
    }
    int count(int value){
        return bit.get(value)-bit.get(value-1);
    }
    int size(){
        return bit.get(bit.N-1)+1;
    }
    int at(int index){
        return bit.find(index);//return the value which at index (index)
    }
    int order_of_key(int key){
        if(key == 0)assert(false);
        assert(key < bit.N);
        return lessThanMe(key); // bit.get(key - 1);
    }
    int largerThanMe(int val) {
        return size() - bit.get(val);
    }

    int largerThanOrEqualMe(int val) {
        return size() - bit.get(val - 1);
    }

    int lessThanMe(int val) {
        return bit.get(val - 1);
    }

    int lessThanOrEqualMe(int val) {
        return bit.get(val);
    }
};
```

#### 2d fenwick
```cpp
template<class T = int>
struct BIT2D {
    vector<vector<T>> tree;
    int n , m;
    void init(int n , int m) {
        this->n = n; this->m = m;
        tree.assign(n , vector<T>(m , 0));
    }
    void add(int x, int y, T val) {
        for (int i = x + 1; i <= n; i += (i & (-i))) {
            for (int j = y + 1; j <= m; j += (j & (-j))) {
                tree[i - 1][j - 1] += val;
            }
        }
    }

    T sum(int x, int y) {
        T ret = 0;
        for (int i = x + 1; i; i -= (i & (-i))) {
            for (int j = y + 1; j; j -= (j & (-j))) {
                ret += tree[i - 1][j - 1];
            }
        }
        return ret;
    }

    T qru(int sx, int sy, int ex, int ey) {
        return sum(ex, ey) - sum(ex, sy - 1) - sum(sx - 1, ey) + sum(sx - 1, sy - 1);
    }
    T qru(int x , int y){ return sum(x , y , x , y); }
};
```

#### 2d offline bit
```cpp
template<class T = int>
struct Bit2D {
    vector<T> ord;
    vector<vector<T>> fw, coord;

    // pts needs all points that will be used in the upd
    // if range upds remember to build with {x1, y1}, {x1, y2 + 1}, {x2 + 1, y1}, {x2 + 1, y2 + 1}
    Bit2D(vector<pair<T, T>> pts) {
        sort(pts.begin(), pts.end());
        for (auto a : pts)
            if (ord.empty() || a.first != ord.back())
                ord.push_back(a.first);
        fw.resize(ord.size() + 1);
        coord.resize(fw.size());

        for (auto& a : pts)
            swap(a.first, a.second);
        sort(pts.begin(), pts.end());
        for (auto& a : pts) {
            swap(a.first, a.second);
            for (int on = std::upper_bound(ord.begin(), ord.end(), a.first) - ord.begin(); on < fw.size(); on += on & -on)
                if (coord[on].empty() || coord[on].back() != a.second)
                    coord[on].push_back(a.second);
        }

        for (int i = 0; i < fw.size(); i++)
            fw[i].assign(coord[i].size() + 1, 0);
    }
    T merge(T a , T b){
        return a + b;
    }
    // point upd
    void upd(T x, T y, T v) {
        for (int xx = upper_bound(ord.begin(), ord.end(), x) - ord.begin(); xx < fw.size(); xx += xx & -xx)
            for (int yy = upper_bound(coord[xx].begin(), coord[xx].end(), y) - coord[xx].begin(); yy < fw[xx].size(); yy += yy & -yy)
                fw[xx][yy] = merge(fw[xx][yy], v);
    }

    // point qry
    T qry(T x, T y) {
        T ans = 0;
        for (int xx = upper_bound(ord.begin(), ord.end(), x) - ord.begin(); xx > 0; xx -= xx & -xx)
            for (int yy = upper_bound(coord[xx].begin(), coord[xx].end(), y) - coord[xx].begin(); yy > 0; yy -= yy & -yy)
                ans = merge(ans, fw[xx][yy]);
        return ans;
    }

    // range qry
    T qry(T x1, T y1, T x2, T y2) {
        return qry(x2, y2) - qry(x2, y1 - 1) - qry(x1 - 1, y2) + qry(x1 - 1, y1 - 1);
    }

    // range upd
    void upd(T x1, T y1, T x2, T y2, T v) {
        upd(x1, y1, v);
        upd(x1, y2 + 1, -v);
        upd(x2 + 1, y1, -v);
        upd(x2 + 1, y2 + 1, v);
    }
};
```

#### 2d range upd bit
```cpp
const int maxn = 1e3 + 5;
ll S[4][maxn][maxn];

void add_point(int z, int x, int Y, ll c) {
    for(; x < maxn; x += x & -x) {
        for(int y = Y; y < maxn; y += y & -y) {
            S[z][x][y] += c;
        }
    }
}

// add c on [a, inf) x [b, inf)
void add_suffix(int a, int b, ll c) {
    add_point(0, a, b, c);
    add_point(1, a, b, -c * (a - 1));
    add_point(2, a, b, -c * (b - 1));
    add_point(3, a, b, c * (a - 1) * (b - 1));
}

// add c on [x1, x2] x [y1, y2]
void add_range(int x1, int y1, int x2, int y2, ll c) {
    add_suffix(x1, y1, c);
    add_suffix(x1, y2 + 1, -c);
    add_suffix(x2 + 1, y1, -c);
    add_suffix(x2 + 1, y2 + 1, c);
}


ll get_point(int z, int x, int Y) {
    ll res = 0;
    for(; x > 0; x -= x & -x) {
        for(int y = Y; y > 0; y -= y & -y) {
            res += S[z][x][y];
        }
    }
    return res;
}

// get sum on [0, x] x [0, y]
ll get_prefix(int x, int y) {
    return get_point(0, x, y) * x * y
         + get_point(1, x, y) * y
         + get_point(2, x, y) * x
         + get_point(3, x, y);
}

// get sum on [x1, x2] x [y1, y2]
ll get_range(int x1, int y1, int x2, int y2) {
    return get_prefix(x2, y2)
        - get_prefix(x1 - 1, y2)
        - get_prefix(x2, y1 - 1)
        + get_prefix(x1 - 1, y1 - 1);
}

```

#### offline distinct in range bit
```cpp
/* the idea is to sort the queries from large l to smallest and maintain map
   of left most index a value appear in and whenever you find query has l == i where i iterating from n - 1
   you handle those queries and when you see a value exist in the map you Update it's postion with -1 and update
   the map_postion with i then update this postion in fenwick with 1
*/
int n,q;
int v[N];
vector<pair<int,int>>queries[N];
int ans[N];
struct Fenwick{
    int n;
    vector<int>tree;
    void init(int _n){
        n=_n;
        tree.resize(this->n);
    }
    void add(int pos,int value){
        for(int i=pos+1;i<=n;i+=i&-i)tree[i-1]+=value;
    }
    int get(int pos) {
        int sum = 0;
        for (int i = pos + 1; i; i -= i & -i)sum += tree[i - 1];
        return sum;
    }
    int query(int l,int r){
        return get(r)-get(l-1);//send zero base
    }
};

signed main() {
    khaled
    cin>>n>>q;
    for(int i=0;i<n;i++){
        cin>>v[i];
    }
    Fenwick fen;
    fen.init(n);
    map<int,int>last_index;
    for(int i=0;i<q;i++){
        int l,r;
        cin>>l>>r;
        l--,r--;
        queries[l].emplace_back(r,i);
    }
    for(int i=n-1;i>=0;i--){
        int val = v[i];
        if(last_index.count(val)){
            fen.add(last_index[val],-1);
        }
        last_index[val] = i;
        fen.add(i,1);
        for(auto &j:queries[i]){
            ans[j.second] = fen.get(j.first);
        }
    }
    for(int i=0;i<q;i++){
        cout<<ans[i]<<line;
    }
}
```

### K-d Tree

#### 3D
```cpp
struct Point {
    long double x, y, z;
    Point(long double x , long double y , long double z):x(x) , y(y) , z(z){}
    bool operator<(const Point& p) const {
        return x < p.x || (x == p.x && (y < p.y || (y == p.y && z < p.z)));
    }
};
 
class KDTree {
private:
    struct Node {
        Point point;
        Node* left;
        Node* right;
        Node(Point p) : point(p), left(nullptr), right(nullptr) {}
    };
    Node* root;
 
    Node* build(vector<Point>& points, int depth) {
        if (points.empty()) return nullptr;
        int axis = depth % 3;
        sort(points.begin(), points.end(), [axis](Point a, Point b) {
            if (axis == 0) return a.x < b.x;
            if (axis == 1) return a.y < b.y;
            return a.z < b.z;
        });
        int mid = points.size() / 2;
        Node* node = new Node(points[mid]);
        vector<Point> leftPoints(points.begin(), points.begin() + mid);
        vector<Point> rightPoints(points.begin() + mid + 1, points.end());
        node->left = build(leftPoints, depth + 1);
        node->right = build(rightPoints, depth + 1);
        return node;
    }
 
    long double dist(Point p1, Point p2) {
        long double res = (p1.x - p2.x) * (p1.x - p2.x);
        res += (p1.y - p2.y) * (p1.y - p2.y);
        res += (p1.z - p2.z) * (p1.z - p2.z);
        return sqrtl(res);
    }
 
    void nearest(Node* node, Point target, int depth, long double& bestDist) {
        if (!node) return;
        long double currentDist = dist(node->point, target);
        if (currentDist < bestDist && currentDist > 0) bestDist = currentDist;  // Added currentDist > 0 check
 
        int axis = depth % 3;
        Node* next = (axis == 0 ? target.x < node->point.x :
                    (axis == 1 ? target.y < node->point.y : target.z < node->point.z)) ? node->left : node->right;
        Node* other = next == node->left ? node->right : node->left;
 
        nearest(next, target, depth + 1, bestDist);
 
        long double axisDist = (axis == 0 ? abs(target.x - node->point.x) :
                              (axis == 1 ? abs(target.y - node->point.y) : abs(target.z - node->point.z)));
        if (axisDist < bestDist) {
            nearest(other, target, depth + 1, bestDist);
        }
    }
 
    void findMinDistanceHelper(Node* node, long double& minDist) {
        if (!node) return;
 
        // Find nearest point to current node's point
        long double currentMin = numeric_limits<long double>::max();
        nearest(root, node->point, 0, currentMin);
        minDist = min(minDist, currentMin);
 
        // Recurse on children
        findMinDistanceHelper(node->left, minDist);
        findMinDistanceHelper(node->right, minDist);
    }
 
public:
    KDTree(vector<Point>& points) {
        root = build(points, 0);
    }
 
    long double nearest(Point target) {
        long double bestDist = numeric_limits<long double>::max();
        nearest(root, target, 0, bestDist);
        return bestDist;
    }
 
    long double findMinDistance() {
        if (!root || (!root->left && !root->right)) return numeric_limits<long double>::max();
        long double minDist = numeric_limits<long double>::max();
        findMinDistanceHelper(root, minDist);
        return minDist;
    }
};
```

#### 2D
```cpp
template<class T>  
struct Point {  
    typedef Point P;  
    T x, y;  
    explicit Point(T x=0, T y=0) : x(x), y(y) {}  
    bool operator<(P p) const { return tie(x,y) < tie(p.x,p.y); }  
      
    P operator+(P p) const { return P(x+p.x, y+p.y); }  
    P operator-(P p) const { return P(x-p.x, y-p.y); }  
     
      
    T dist2() const { return abs(x) + abs(y); }  
     
};  
  
typedef long long ll;  // Added missing typedef  
typedef Point<ll> P;  
const ll INF = numeric_limits<ll>::max();  
  
bool on_x(const P& a, const P& b) { return a.x < b.x; }  
bool on_y(const P& a, const P& b) { return a.y < b.y; }  
  
struct Node {  
    P pt;  
    ll x0 = INF, x1 = -INF, y0 = INF, y1 = -INF;  
    Node *first = 0, *second = 0;  
      
    ll distance(const P& p) {   
        ll x = (p.x < x0 ? x0 : p.x > x1 ? x1 : p.x);  
        ll y = (p.y < y0 ? y0 : p.y > y1 ? y1 : p.y);  
        return (P(x,y) - p).dist2();  
    }  
      
    Node(vector<P>&& vp) : pt(vp[0]) {  
        for (P p : vp) {  
            x0 = min(x0, p.x); x1 = max(x1, p.x);  
            y0 = min(y0, p.y); y1 = max(y1, p.y);  
        }  
        if (vp.size() > 1) {  
            sort(vp.begin(), vp.end(), x1 - x0 >= y1 - y0 ? on_x : on_y);  
            int half = vp.size()/2;  
            first = new Node({vp.begin(), vp.begin() + half});  
            second = new Node({vp.begin() + half, vp.end()});  
        }  
    }  
};  
  
struct KDTree {  
    Node* root;  
      
    KDTree(const vector<P>& vp) : root(new Node({vp.begin(), vp.end()})) {}  
      
    pair<ll, P> search(Node *node, const P& p) {  
        if (!node->first) {  
            return make_pair((p - node->pt).dist2(), node->pt);  
        }  
        Node *f = node->first, *s = node->second;  
        ll bfirst = f->distance(p), bsec = s->distance(p);  
        if (bfirst > bsec) swap(bsec, bfirst), swap(f, s);  
        auto best = search(f, p);  
        if (bsec < best.first)  
            best = min(best, search(s, p));  
        return best;  
    }  
      
    pair<ll, P> nearest(const P& p) {  
        return search(root, p);  
    }  
};
```

### Merge sort Tree
#### smaller than x 
```cpp
struct MergeSortTree{
    vector<vector<int>>tree;
    MergeSortTree(int n){
        tree.resize(4 * n);
    }
    void build(int node , int nl,int nr , vector<int>&a){
        if(nl == nr){
            tree[node].emplace_back(a[nl]);
            return;
        }
        int mid = nl + (nr - nl) / 2;
        build(2 * node + 1 , nl , mid , a);
        build(2 * node + 2 , mid + 1 , nr , a);
        merge(tree[2 * node + 1].begin(), tree[2 * node + 1].end(),tree[2 * node + 2].begin(), tree[2 * node + 2].end(),back_inserter(tree[node]));
    }
    int query(int node,int nl,int nr,int l ,int r,int value){
        // less than
        if(nl >=l && nr <= r){
            return (int)(lower_bound(tree[node].begin() , tree[node].end() , value) - tree[node].begin());
        }
        if(nl > r || nr < l)return 0;
        int mid = nl + (nr - nl) / 2;
        return query(2 * node + 1 , nl , mid , l , r ,value) + query(2 * node + 2 , mid + 1 , nr , l , r , value);
    }
};
```

#### greater than x
```cpp
struct MergeSortTree{
    vector<vector<int>>tree;
    int n;
    MergeSortTree(int _n){
        n = _n;
        tree.resize(4*n);
    }
    void build(int node,int nl,int nr){
        if(nl==nr){
            tree[node].emplace_back(arr[nl]);
            return;
        }
        int mid = nl + (nr - nl)/2;
        build(2*node+1,nl,mid);
        build(2*node+2,mid+1,nr);
        merge(tree[2*node+1].begin(), tree[2*node+1].end(),tree[2*node+2].begin(), tree[2*node+2].end(),inserter(tree[node], tree[node].end()));
    }
    int query(int node,int nl,int nr,int l,int r,int k){
        if(nl>=l && nr<= r){
            auto it = nr - nl + 1 - (upper_bound(tree[node].begin(),tree[node].end(),k) - tree[node].begin());
            return it;
        }
        if(nl>r||nr<l)return 0;
        int mid = nl + (nr - nl)/2;
        return query(2*node+1,nl,mid,l,r,k) + query(2*node+2,mid+1,nr,l,r,k);
    }
};
```

### 2d prefix sum

```cpp
const int N = 1001, M = 1001;
int pref[N][M]; // 1-base
int n , m;
int qry(int from_x , int from_y , int to_x , int to_y){
    return  pref[to_x][to_y] - pref[from_x - 1][to_y] - pref[to_x][from_y - 1] + pref[from_x - 1][from_y - 1];
}
void upd(int from_x , int from_y, int to_x , int to_y, int val){
    pref[from_x][from_y] += val;
    pref[to_x + 1][from_y] -= val;
    pref[from_x][to_y + 1] -= val;
    pref[to_x + 1][to_y + 1] += val;
}
void build(){
    for(int i = 1; i <= n; ++i) for(int j = 1; j <= m; ++j){
        pref[i][j] += pref[i - 1][j] + pref[i][j - 1] - pref[i - 1][j - 1];
    }
}
void init(){
    for(int i = 1; i <= n; ++i) for(int j = 1; j <= m; ++j){
        pref[i][j] = 0;
    }
}
```

### 3d prefix sum
```cpp
const int N = 500, M = 500 , K = 500;
int P[N][M][K]; // 1-base
int n , m , k;
int qry(int from_x , int from_y , int from_z , int to_x , int to_y , int to_z){
    int result = P[to_x][to_y][to_z]
                   - P[from_x-1][to_y][to_z] 
                   - P[to_x][from_y-1][to_z]
                   - P[to_x][to_y][from_z-1]
                   + P[from_x-1][from_y-1][to_z]
                   + P[from_x-1][to_y][from_z-1] 
                   + P[to_x][from_y-1][from_z-1]
                   - P[from_x-1][from_y-1][from_z-1];
}
void upd(int from_x , int from_y, int from_z, int to_x , int to_y, int to_z , int val){
    P[from_x][from_y][from_z] += val;
    P[to_x + 1][from_y][from_z] -= val;
    P[from_x][to_y + 1][from_z] -= val;
    P[to_x][from_y][to_z + 1] -= val;
    P[to_x + 1][to_y + 1][from_z] += val;
    P[to_x + 1][from_y][to_z + 1] += val;
    P[from_x][to_y + 1][to_z + 1] += val;
    P[to_x + 1][to_y + 1][to_z + 1] -= val;
}
void build(vector<vector<vector<int>>> &A){
    for (int x = 1; x <= N; ++x) {
        for (int y = 1; y <= N; ++y) {
            for (int z = 1; z <= N; ++z) {
                P[x][y][z] = A[x][y][z]
                           + P[x-1][y][z]
                           + P[x][y-1][z]
                           + P[x][y][z-1]
                           - P[x-1][y-1][z]
                           - P[x-1][y][z-1]
                           - P[x][y-1][z-1]
                           + P[x-1][y-1][z-1];
            }
        }
    }
}
void init(){
    for(int i = 1; i <= n; ++i) for(int j = 1; j <= m; ++j) for(int k = 1; k <= K; ++k){
        P[i][j][k] = 0;
    }
}
```

### sparse table

#### struct
```cpp
struct SparseTable {
    vector<vector<int> > sp;
    vector<int> LOG;
    vector<int> arr;
    int n, LG;
    SparseTable(vector<int> &_arr) : arr(_arr) {
        n = (int)_arr.size();
 
        LOG = vector<int>(n + 1);
        LOG[0] = LOG[1] = 0;
 
        for (int i = 2; i <= n; ++i) {
            LOG[i] += LOG[i - 1] + !(i & (i - 1));
        }
 
        LG = LOG[n];
        sp = vector<vector<int> >(LG + 1, vector<int>(n));
 
        build();
    }
 
    int f(int lfV, int rtV) {
        return min(lfV, rtV);
    }
 
    void build() { // O( n log(n) )
        sp[0] = arr;
 
        for (int lvl = 1; lvl <= LG; ++lvl) {
            for (int j = 0; j + (1 << lvl) <= n; ++j) {
                sp[lvl][j] = f(sp[lvl - 1][j], sp[lvl - 1][j + (1 << (lvl - 1))]);
            }
        }
    }
 
    int Query1(int l, int r) { // O( 1 )
        int lg = LOG[r - l + 1];
 
        return f(sp[lg][l], sp[lg][r - (1 << lg) + 1]);
    }
 
    int Query2(int l, int r) { // O( LogN )
        int lg = LOG[n];
        int ans = 0;
 
        for (int j = lg; ~j; --j) {
            if ((1 << j) <= (r - l + 1)) {
                ans = f(ans, sp[j][l]);
                l += (1 << j);
            }
        }
 
        return ans;
    }
};
 
```

#### index
```cpp
const int k = 20;
int sp1[k][N];//max
int sp2[k][N];//min
void build(int n,vector<int>&v){
    for(int i = 1;i<k;i++){
        for(int j = 0;j+(1<<i)<=n;j++){
            if(v[sp1[i-1][j]]>v[sp1[i-1][j+(1<<(i-1))]]){
                sp1[i][j]=sp1[i-1][j];
            }else{
                sp1[i][j]=sp1[i-1][j+(1<<(i-1))];
            }
            if(v[sp2[i-1][j]]>v[sp2[i-1][j+(1<<(i-1))]]){
                sp2[i][j]=sp2[i-1][j+(1<<(i-1))];
            }else{
                sp2[i][j]=sp2[i-1][j];
            }
        }
    }
}
int query1(int l,int r,vector<int>&v){
    int lg = log2_floor(r-l+1);
    if(v[sp1[lg][l]]>v[sp1[lg][r-(1<<lg)+1]])return sp1[lg][l];
    return sp1[lg][r-(1<<lg)+1];
}
int query2(int l,int r,vector<int>&v){
    int lg = log2_floor(r-l+1);
    if(v[sp2[lg][l]]>v[sp2[lg][r-(1<<lg)+1]])return sp2[lg][r-(1<<lg)+1];
    return sp2[lg][l];
}
```

#### 2d const
```cpp
const int M = 1e3 + 1;
const int LOG = 10;
int f[LOG][LOG][M][M];
int grid[M][M];
int n, m;
/*
 2d sparse table f[log(n)][log(m)][N][M] --> take m * n * log(n) * log(m) memory and time and o(1) for quering
 it can be used for min , max , gcd , lcm , and , or ,... any non mutable function
 it support no updates.
 note it written like that to be cache friendly don't change the dimesions order.
 if your problem have N = 5000 change the LOG make it 13 
 if your problem have N * M <= 1e6 then you have to use vectors instead of arrays but avoid that as possible
*/
int combine(int a, int b){
    return max(a , b);
}
void build() {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            f[0][0][i][j] = grid[i][j];
        }
    }
    for (int k1 = 0; k1 < LOG; k1++) {
        for (int k2 = 0; k2 < LOG; k2++) {
            if (k1 == 0 && k2 == 0) continue;
            for (int i = 0; i + (1 << k1) <= n; i++) {
                for (int j = 0; j + (1 << k2) <= m; j++) {
                    if (k1 > 0) {
                        f[k1][k2][i][j] = combine(f[k1 - 1][k2][i][j], f[k1 - 1][k2][i + (1 << (k1 - 1))][j]);
                    }
                    if (k2 > 0) {
                        f[k1][k2][i][j] = combine(f[k1][k2 - 1][i][j], f[k1][k2 - 1][i][j + (1 << (k2 - 1))]);
                    }
                    if (k1 > 0 && k2 > 0) {
                        f[k1][k2][i][j] = combine(combine(
                                f[k1 - 1][k2 - 1][i][j],
                                f[k1 - 1][k2 - 1][i + (1 << (k1 - 1))][j]), combine(
                                f[k1 - 1][k2 - 1][i][j + (1 << (k2 - 1))],
                                f[k1 - 1][k2 - 1][i + (1 << (k1 - 1))][j + (1 << (k2 - 1))])
                        );
                    }
                }
            }
        }
    }
}
int query(int l, int d, int r, int u) {
    int k1 = std::__lg(r - l + 1);
    int k2 = std::__lg(u - d + 1);
    return combine(combine(
                           f[k1][k2][l][d],
                           f[k1][k2][r - (1 << k1) + 1][d]),
                   combine(
                           f[k1][k2][l][u - (1 << k2) + 1],
                           f[k1][k2][r - (1 << k1) + 1][u - (1 << k2) + 1])
    );
}
```

#### 2d variable
```cpp
template<class T = int>
struct sparse_2d {
    int n, m, LOG , N;
    T DEFAULT = 0;
    vector<vector<vector<T>>> tree;
 
    void init(int _n, int _m , vector<vector<T>> &a) {
        n = _n;
        m = _m;
        LOG = 1;
        while ((1 << LOG) < m) ++LOG;
        N = 1;
        while(N < n) N *= 2;
        tree = vector<vector<vector<T>>>(2 * N + 1, vector<vector<T>>(LOG, vector<T>(m + 1)));
 
        for (int x = 0; x < n; x++)
        {
            for (int y = 0; y < m; y++)
                tree[N + x][0][y] = a[x][y];
            for (int k = 1; k < LOG; k++)
                for (int y = 0; y + (1 << k) <= m; y++)
                    tree[N + x][k][y] = comb(tree[N + x][k - 1][y], tree[N + x][k - 1][y + (1 << (k - 1))]);
        }
        for (int v = N - 1; v > 0; v--)
            for (int k = 0; k < LOG; k++)
                for (int y = 0; y + (1 << k) <= m; y++)
                    tree[v][k][y] = comb(tree[2 * v][k][y], tree[2 * v + 1][k][y]);
     
    }
 
    T comb(T a, T b) {
        return max(a, b); // Change the custom operator
    }
 
    T qry(int v, int a, int b, int x1, int x2, int y1, int y2, int k) {
        if (x1 <= a && b <= x2){
            return comb(tree[v][k][y1], tree[v][k][y2]); 
        }
        if (x1 >= b || a >= x2) return 0;
        int mid = (a + b) / 2;
        return comb(qry(2 * v, a, mid, x1, x2, y1, y2, k), qry(2 * v + 1, mid, b, x1, x2, y1, y2, k));
    }
 
    T qry(int lx, int ly, int rx, int ry) {
        int k = 31 - __builtin_clz(ry - ly + 1);
        return qry(1, 0, N, lx, rx + 1, ly, ry - (1 << k) + 1, k);
    }
};
```


### Trie
#### string
```cpp
struct Trie {
	Trie* arr[26] = {nullptr};
	int lst = 0;
	int st = 0;

	Trie() {
		for (int i = 0; i < 26; ++i) {
			arr[i] = nullptr;
		}
		lst = 0;
		st = 0;
	}

	~Trie() {
		for (int i = 0; i < 26; ++i) {
			if (arr[i] != nullptr) {
				delete arr[i];
				arr[i] = nullptr;
			}
		}
	}

	void insert(const string &word, int idx = 0) {
		if (idx == word.size()) {
			lst++;
			st++;
			return;
		}
		int ch = word[idx] - 'a';
		if (arr[ch] == nullptr) {
			arr[ch] = new Trie();
		}
		if(idx!=0)st++;
		arr[ch]->insert(word, idx + 1);
	}

	int count_equal(const string &word, int idx = 0) const {
		if (idx == word.size()) {
			return lst;
		}
		int ch = word[idx] - 'a';
		if (arr[ch] == nullptr) {
			return 0;
		}
		return arr[ch]->count_equal(word, idx + 1);
	}

	int count_prefix(const string &word, int idx = 0) const {
		if (idx == word.size()) {
			return st;
		}
		int ch = word[idx] - 'a';
		if (arr[ch] == nullptr) {
			return 0;
		}
		return arr[ch]->count_prefix(word, idx + 1);
	}

	bool erase(const string &word, int idx = 0) {
		if (idx == word.size()) {
			lst--;
			st--;
			return true;
		}
		int ch = word[idx] - 'a';
		if (arr[ch] == nullptr) {
			return false;
		}
		if (!arr[ch]->erase(word, idx + 1)) {
			return false;
		}
		if(idx!=0) st--;
		if (arr[ch]->st == 0) {
			delete arr[ch];
			arr[ch] = nullptr;
		}
		return true;
	}
	bool GetProperPrefixTo(const string &s , string &temp , vector<string>&ans , int idx = 0){
		if(s.size() > idx){
			int ch = s[idx] - 'a';
			if(arr[ch] == nullptr)return false;
			temp += s[idx];
			return arr[ch]->GetProperPrefixTo(s , temp,ans, idx + 1);
		}else{
			for(int i = 0;idx != s.size() && i<lst;i++){
				ans.emplace_back(temp);
				break;
			}
			for(int i = 0;i<26;i++){
				if(arr[i]){
					temp += (char)(i + 'a');
					bool x = arr[i]->GetProperPrefixTo(s , temp ,ans, idx + 1);
					temp.pop_back();
				}
			}
			return true;
		}
	}
};
```

#### bit
```cpp
struct Trie{
	Trie* arr[2];
	int freq;
	Trie(){
		freq = 0;
		for(auto &val:arr){
			val = nullptr;
		}
	}
	~Trie(){
		if(arr[0])delete arr[0];
		if(arr[1])delete arr[1];
	}
	void insert(ll x, int idx = 40) {
		if (idx == -1) {
			freq++;
			return;
		}
		bool temp = ((x & (1LL << idx)));
		if (arr[temp] == nullptr) {
			arr[temp] = new Trie();
		}
    	freq++; // Increment freq only if a node is traversed
    	arr[temp]->insert(x, idx - 1);
	}

	bool erase(ll x, int idx = 40) {
		if (idx == -1) {
			freq--;
			return true;
		}
		bool temp = ((x & (1LL << idx)));
		if (arr[temp] == nullptr) return false;
		if (arr[temp]->erase(x, idx - 1)) {
		    freq--; // Decrement freq only if a node is traversed
		    if (arr[temp]->freq == 0) {
	        	delete arr[temp];
	        	arr[temp] = nullptr;
	        }
		    return true;
		}
		return false;
	}

	ll Get(ll x , int idx = 40){
		if(idx == -1)return 0;
		bool temp = ((x & (1LL<<idx)));
		if(arr[!temp] != nullptr)return arr[!temp]->Get(x , idx - 1) | (1LL<<idx);
		else{
			assert(arr[temp]);
			return arr[temp]->Get(x , idx - 1);
		}
	}
};
```

### ordered set
```cpp
struct ordered__set {

    ordered_multiset<ll> se;

    void erase(ll val) {
        if (se.size() == 0 || *se.find_by_order(se.size() - 1) < val || *se.lower_bound(val - 1) != val) return;
        se.erase(se.lower_bound(--val));
    }

    int lw_bound(ll val) { // log --> return index ;
        if (se.size() == 0 || *se.find_by_order(se.size() - 1) < val) return -1;
        return se.order_of_key(*se.lower_bound(--val));
    }

    int up_bound(ll val) { return lw_bound(val + 1ll); }

    void insert(ll val) { se.insert(val); }

    ll operator[](int idx) { return *se.find_by_order(idx); }

    int size() { return se.size(); }

    void clr() { se.clear(); }
};

```

### Mo
#### gilbert
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

#### sort
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

#### Mo on trees
```cpp
int n,m;
const int k = 20,SQ = 400;
vector<int>adj[N];
int Cost[N],UP[k][N],Parent[N],LinearTree[N],Timer = 0,Depth[N],root = 0,ST[N],EN[N],CompressionValue = 1,ans[N],res = 0,Freq[N];
bool IsOdd[N];
void MarkDepth(int node,int par,int c = 0) {
    Depth[node] = c;
    for (auto &val: adj[node]) {
        if (val == par)continue;
        MarkDepth(val, node, c + 1);
    }
}
void Rooting() {
    queue<pair<int, int>> q;
    q.emplace(root, -1);
    while (!q.empty()) {
        int node = q.front().first;
        int par = q.front().second;
        q.pop();
        for (auto &val: adj[node]) {
            if (val != par) {
                Parent[val] = node;
                q.emplace(val, node);
            }
        }
    }
    Parent[root] = root;
}
void BuildLiftingTable() {
    MarkDepth(root, -1);
    for (int i = 0; i < n; i++) {
        UP[0][i] = Parent[i];
    }
    for (int i = 1; i < k; i++) {
        for (int j = 0; j < n; j++) {
            UP[i][j] = UP[i - 1][UP[i - 1][j]];
        }
    }
}
int Walk(int node,int kth) {
    if (Depth[node] < kth)return -1;
    for (int i = 0; i < k; i++) {
        if (kth & (1 << i)) {
            node = UP[i][node];
        }
    }
    return node;
}
int LCA(int a,int b) {
    if (Depth[b] > Depth[a])swap(a, b);
    a = Walk(a, Depth[a] - Depth[b]);
    if (a == b)return a;
    for (int i = k - 1; i >= 0; i--) {
        if (UP[i][a] != UP[i][b]) {
            a = UP[i][a];
            b = UP[i][b];
        }
    }
    return Parent[a];
}
void Linear(int node,int par) {
    LinearTree[Timer] = node;
    ST[node] = Timer;
    Timer++;
    for (auto &val: adj[node]) {
        if (val == par)continue;
        Linear(val, node);
    }
    LinearTree[Timer] = node;
    EN[node] = Timer;
    Timer++;
}
map<int,int>compression;
struct Query {
    int l, r, lca , idx ;
    bool work;

    Query(int L = 0, int R = 0, int Lca = 0,int i = 0, bool W = false) {
        l = L;
        r = R;
        lca = Lca;
        idx = i;
        work = W;
    }

    bool operator<(const Query &other) {
        return make_pair(l / SQ, r) < make_pair(other.l / SQ, other.r);
    }
};
void AddElement(int node){
    Freq[Cost[node]]++;
    if(Freq[Cost[node]]==1)res++;
}

void EraseElement(int node){
    Freq[Cost[node]]--;
    if(Freq[Cost[node]]==0)res--;
}

void UpdateMoState(int node){
    IsOdd[node]^=true;
    return void(IsOdd[node]? AddElement(node): EraseElement(node));
}

signed main() {
    khaled
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> Cost[i];
        if (compression.count(Cost[i])) {
            Cost[i] = compression[Cost[i]];
            continue;
        }
        compression[Cost[i]] = CompressionValue++;
        Cost[i] = compression[Cost[i]];
    }
    for (int j = 0; j < n - 1; j++) {
        int a, b;
        cin >> a >> b;
        a--, b--;
        adj[a].emplace_back(b);
        adj[b].emplace_back(a);
    }
    Rooting();
    BuildLiftingTable();
    Linear(root, -1);
    vector<Query> queries;
    for (int i = 0; i < m; i++) {
        int l, r;
        cin >> l >> r;
        l--, r--;
        if (ST[l] > ST[r])swap(l, r);
        int lca = LCA(l, r);
        if (lca == l) {
            queries.emplace_back(ST[lca], ST[r], lca,i, false);
        } else {
            queries.emplace_back(EN[l], ST[r], lca, i,true);
        }
    }
    sort(queries.begin(), queries.end());
    int MoRight = -1, MoLeft = 0;
    for (auto &val: queries) {
        int idx = val.idx;
        int l = val.l;
        int r = val.r;
        while (MoRight < r) {
            MoRight++;
            UpdateMoState(LinearTree[MoRight]);
        }
        while (MoLeft > l) {
            MoLeft--;
            UpdateMoState(LinearTree[MoLeft]);
        }
        while (MoRight > r) {
            UpdateMoState(LinearTree[MoRight]);
            MoRight--;
        }
        while (MoLeft < l) {
            UpdateMoState(LinearTree[MoLeft]);
            MoLeft++;
        }
        if (val.work)UpdateMoState(val.lca);
        ans[idx] = res;
        if (val.work) UpdateMoState(val.lca);
    }
    for (int i = 0; i < m; i++) {
        cout << ans[i] << line;
    }
}
```

#### Mo with updates
```cpp
int A[N],n,comp[2 * N],ans[N];
int Freq[2 * N],old[2 * N],New[2 * N];
int upIdx[N];
int st,en,tim,BlockSize,CompSize,q,nq,cnt = 0;
int qs[N],qe[N],qi[N],qt[N];
void compress() {
    sort(comp, comp + CompSize);
    CompSize = (int)(unique(comp, comp + CompSize) - comp);
    for (int i = 0; i < n; i++) {
        A[i] = (int)(lower_bound(comp, comp + CompSize, A[i]) - comp);
    }
    for (int i = 1; i <= tim; i++) {
        old[i] = (int)(lower_bound(comp, comp + CompSize, old[i]) - comp);
        New[i] = (int)(lower_bound(comp, comp + CompSize, New[i]) - comp);
    }
}
void Add(int CompressedValue) {
    
}
void Erase(int CompressedValue) {
    
}
void update(int qId) {
    while (tim < qt[qId]) {
        tim++;
        if (upIdx[tim] >= st && upIdx[tim] < en) {
            Erase(old[tim]);
            Add(New[tim]);
        }
        A[upIdx[tim]] = New[tim];
    }
    while (tim > qt[qId]) {
        if (upIdx[tim] >= st && upIdx[tim] < en) {
            Erase(New[tim]);
            Add(old[tim]);
        }
        A[upIdx[tim]] = old[tim];
        tim--;
    }
    while (en <= qe[qId]) {
        Add(A[en++]);
    }
    while (en - 1 > qe[qId]) {
        Erase(A[--en]);
    }
    while (st - 1 >= qs[qId]) {
        Add(A[--st]);
    }
    while (st < qs[qId]) {
        Erase(A[st++]);
    }
}
signed main() {
    khaled
    cin >> n;
    cin >> q;
    for (int i = 0; i < n; i++) {
        cin >> A[i];
        comp[i] = A[i];
    }
    CompSize = n;
    for (int i = 0; i < q; i++) {
        char ch;
        cin >> ch;
        int x, y;
        cin >> x >> y;
        if (ch == '2') {
            qs[nq] = x;
            qe[nq] = y;
            qi[nq] = nq;
            qt[nq] = tim;
            nq++;
        } else {
            tim++;
            old[tim] = A[x];
            New[tim] = y;
            upIdx[tim] = x;
            A[x] = y;
            comp[CompSize++] = y;
        }
    }
    compress();
    BlockSize = ceil(pow(n, 2.0 / 3.0));
    sort(qi,qi + nq , [](int a,int b)->int{
        int BsA, BeA,BsB,BeB;
        BsA = qs[a]/BlockSize;
        BeA = qe[a]/BlockSize;
        BsB = qs[b]/BlockSize;
        BeB = qe[b]/BlockSize;
        return tie(BsA,BeA,qt[a]) < tie(BsB,BeB,qt[b]);
    });
    for(int i=0;i<nq;i++){
        update(qi[i]);
        ans[qi[i]] = cnt;
    }
    for(int i=0;i<nq;i++){
        cout<<ans[i]<<line;
    }
}
```

#### Mo with updates on tree
```cpp
int n,q;
const int K = 18;
int values[N],St[N],En[N],LinearTree[N],depth[N],parent[N],UP[K][N],qs[N],qe[N],qi[N],qlca[N],freq[N],comp[N],qt[N],Timer,en,st,nw[N],old[N],upIdx[N],nq,tim,CompSize;
vector<int>adj[N];
void dfs(int node,int par){
    parent[node] = par;
    depth[node] = (node == 0 ? 0 : depth[par] + 1);
    LinearTree[St[node] = Timer] = node;
    Timer++;
    for(auto &val:adj[node]){
        if(val == par)continue;
        dfs(val,node);
    }
    LinearTree[En[node] = Timer] = node;
    Timer++;
}
void build(){
    for(int i=0;i<n;i++){
        UP[0][i] = parent[i];
    }
    for(int i = 1 ;i < K;i++){
        for(int j=0;j<n;j++){
            UP[i][j] = UP[i-1][UP[i-1][j]];
        }
    }
}
int walk(int node,int kth){
    for(int i = 0;i<K;i++){
        if(kth & (1<<i)){
            node = UP[i][node];
        }
    }
    return node;
}
int Lca(int a,int b){
    if(depth[a] < depth[b])swap(a,b);
    a = walk(a,depth[a] - depth[b]);
    if(a == b)return a;
    for(int i = K - 1 ;i >= 0 ;i--){
        if(UP[i][a] != UP[i][b]){
            a = UP[i][a];
            b = UP[i][b];
        }
    }
    return parent[a];
}
int BlockSize,cnt,res[N];
void add(int value){
    cnt += !freq[value]++;
}
void remove(int value){
    cnt -= !--freq[value];
}
bool vis[N];
void upd(int MoIdx){
    vis[LinearTree[MoIdx]] ^= true;
    if(vis[LinearTree[MoIdx]])add(values[LinearTree[MoIdx]]);
    else remove(values[LinearTree[MoIdx]]);
}
int MoLeft = 0 , MoRight = -1;
bool in(int a,int l,int r){
    return a >= l && a <= r;
}
void update(int qId) {
    while (tim < qt[qId]) {
        tim++;
        bool f1 = in(St[upIdx[tim]], MoLeft, MoRight);
        bool f2 = in(En[upIdx[tim]], MoLeft, MoRight);
        if (f1 && !f2 || f2 && !f1) {
            remove(old[tim]);
            add(nw[tim]);
        }
        values[upIdx[tim]] = nw[tim];
    }
    while (tim > qt[qId]) {
        bool f1 = in(St[upIdx[tim]], MoLeft, MoRight);
        bool f2 = in(En[upIdx[tim]], MoLeft, MoRight);
        if (f1 && !f2 || f2 && !f1) {
            remove(nw[tim]);
            add(old[tim]);
        }
        values[upIdx[tim]] = old[tim];
        tim--;
    }
    while (MoRight < qe[qId]) upd(++MoRight);
    while (MoLeft > qs[qId]) upd(--MoLeft);
    while (MoRight > qe[qId]) upd(MoRight--);
    while (MoLeft < qs[qId]) upd(MoLeft++);
}

signed main() {
    khaled
    cin>>n>>q;
    for(int i=0;i<n;i++)cin>>values[i],comp[i] = values[i];
    for(int i=0;i<n-1;i++){
        int l,r;cin>>l>>r;
        l--,r--;
        adj[l].emplace_back(r);
        adj[r].emplace_back(l);
    }
    CompSize = n;
    dfs(0,0);build();
    for(int i=0;i<q;i++){
        int type;
        cin>>type;
        if(type == 1) {
            int u, v;
            cin >> u >> v;
            u--, v--;
            if (St[u] > St[v])swap(u, v);
            int s, e, L = Lca(u, v);
            if (L == u) {
                s = St[u] + 1;
            } else
                s = En[u];
            e = St[v];
            qs[nq] = s;
            qe[nq] = e;
            qi[nq] = nq;
            qlca[nq] = L;
            qt[nq] = tim;
            nq++;
        }else{
            int u,v;cin>>u>>v;
            tim++;
            u--;
            old[tim] = values[u];
            nw[tim] = v;
            values[u] = v;
            upIdx[tim] = u;
            comp[CompSize++] = v;
        }
    }
    BlockSize = ceil(pow(n,2.0/3.0) + 1);
    sort(comp , comp + CompSize);
    CompSize = unique(comp , comp + CompSize) - comp;
    for(int i=0;i<n;i++){
        values[i] = lower_bound(comp , comp + CompSize,values[i]) - comp;
    }
    for(int i = 1;i<=tim;i++){
        old[i] = lower_bound(comp , comp + CompSize, old[i]) - comp ;
        nw[i] = lower_bound(comp , comp + CompSize , nw[i]) - comp;
    }
    sort(qi,qi + nq , [](int a,int b) -> int{
        int bsa = qs[a] / BlockSize;
        int bea = qe[a] / BlockSize;
        int bsb = qs[b] / BlockSize;
        int beb = qe[b] / BlockSize;
        return tie(bsa,bea,qt[a]) < tie(bsb,beb,qt[b]);
    });
    for(int i=0;i<nq;i++){
        update(qi[i]);
        add(values[qlca[qi[i]]]);
        res[qi[i]] = cnt;
        remove(values[qlca[qi[i]]]);
    }
    for(int i=0;i<nq;i++){
        cout<<res[i]<<line;
    }
}
```

### mono stack
#### prev , next
```cpp
vector<int> getNxtMin(vector<int> &arr) {
    stack<int> st;
    vector<int> res(arr.size(), -1);
    for (int i = 0; i < arr.size(); i++) {
        while (!st.empty() && arr[st.top()] > arr[i]) {
            res[st.top()] = i;
            st.pop();
        }
        st.push(i);
    }
    return res;
}
 
vector<int> getPrevMin(vector<int> &arr) {
    stack<int> st;
    vector<int> res(arr.size(), -1);
    for (int i = arr.size() - 1; i >= 0; i--) {
        while (!st.empty() && arr[st.top()] > arr[i]) {
            res[st.top()] = i;
            st.pop();
        }
        st.push(i);
    }
    return res;
}
 
vector<int> getNxtMax(vector<int> &arr) {
    stack<int> st;
    vector<int> res(arr.size(), -1);
    for (int i = 0; i < arr.size(); i++) {
        while (!st.empty() && arr[st.top()] < arr[i]) {
            res[st.top()] = i;
            st.pop();
        }
        st.push(i);
    }
    return res;
}

vector<int> getPrevMax(vector<int> &arr) {
    stack<int> st;
    vector<int> res(arr.size(), -1);
    for (int i = arr.size() - 1; i >= 0; i--) {
        while (!st.empty() && arr[st.top()] < arr[i]) {
            res[st.top()] = i;
            st.pop();
        }
        st.push(i);
    }
    return res;
}
```

#### maximum sum for montain
```cpp

vector<long long> prevMaxSum(vector<int>& nums) {
        int n = nums.size();
        vector<long long> prevSmall(n, -1), leftSum(n, 0);
        leftSum[0] = nums[0];
        stack<int> st;
        st.push(0);

        for(int i=1;i<n;i++) {
            while(!st.empty() and nums[st.top()] >= nums[i]) st.pop();
            if(!st.empty()) prevSmall[i] = st.top();

            int prevSmallIdx = prevSmall[i];
            int temp = i - prevSmallIdx;
            leftSum[i] += (temp*1LL*nums[i]);
            if(prevSmall[i] != -1) leftSum[i] += leftSum[prevSmallIdx];

            st.push(i);
        }
        return leftSum;
    }

    vector<long long> rightMaxSum(vector<int>& nums) {
        int n = nums.size();
        vector<long long> nextSmall(n, n), rightSum(n, 0);
        rightSum[n-1] = nums[n-1];
        stack<int> st;
        st.push(n-1);

        for(int i=n-2;i>=0;i--) {
            while(!st.empty() and nums[st.top()] >= nums[i]) st.pop();
            if(!st.empty()) nextSmall[i] = st.top();

            int nextSmallIdx = nextSmall[i];
            int temp = nextSmallIdx - i;
            rightSum[i] += (temp*1LL*nums[i]);
            if(nextSmall[i] != n) rightSum[i] += rightSum[nextSmallIdx];

            st.push(i);
        }
        return rightSum;
    }

    long long maximumSumOfHeights(vector<int>& nums) {
        int n = nums.size();
        vector<long long> leftSum = prevMaxSum(nums);
        vector<long long> rightSum = rightMaxSum(nums);

        long long ans = 0;
        for(int i=0;i<n;i++) {
            long long temp = leftSum[i] + rightSum[i] - nums[i];
            ans = max(ans, temp);
        }
        return ans;
    }
```

### matrix expo
```cpp
struct matrix {
    int siz;
    vector<vector<int>>mat;
    matrix(int _siz) {
        mat.resize(_siz,vector<int>(_siz));
        siz = _siz;
        for (int i = 0; i < siz; i++) {
            for (int j = 0; j < siz; j++) {
                mat[i][j] = 0;
            }
        }
    }

    matrix operator*(const matrix &b) {
        matrix c(siz);
        for (int i = 0; i < siz; i++) {
            for (int j = 0; j < siz; j++) {
                c.mat[i][j] = 0;
                for (int k = 0; k < siz; k++) {
                    c.mat[i][j] += mul(mat[i][k] , b.mat[k][j],mod);
                }
                c.mat[i][j] %= mod;
            }
        }
        return c;
    }

    matrix operator+(const matrix &b){
        matrix res(siz);
        for(int i = 0; i < siz; i++){
            for(int j = 0; j < siz; j++){
                res.mat[i][j] = mat[i][j] + b.mat[i][j];
                res.mat[i][j] %= mod;
            }
        }
        return res;
    }

    int Trace() {
        int sum = 0;
        for (int i = 0; i < siz; i++)sum += mat[i][i] % mod;
        return sum % mod;
    }
};
matrix Identity(int siz) {
    matrix res(siz);
    for (int i = 0; i < siz; i++) {
        for (int j = 0; j < siz; j++) {
            res.mat[i][j] = 0;
        }
    }
    for (int i = 0; i < siz; i++) {
        res.mat[i][i] = 1;
    }
    return res;
}
matrix ZeroMatrix(int siz) {
    matrix res(siz);
    return res;
}
matrix FastPower(matrix a, int power) {
    matrix res = Identity(a.siz);
    while (power > 0) {
        if (power & 1) {
            res = res * a;
        }
        power >>= 1ll;
        a = a * a;
    }
    return res;
}
matrix Rotation(const matrix &a){
    matrix res(a.siz);
    int siz = a.siz;
    for(int i = 0; i < siz; i++){
        for(int j = 0; j < siz; j++){
            res.mat[j][siz - 1 - i] = a.mat[i][j];
        }
    }
    return res;
}
matrix Reflection(const matrix &a){
    matrix res(a.siz);
    int siz = a.siz;
    for(int i = 0; i < siz; i++){
        for(int j = 0; j < siz; j++){
            res.mat[i][siz - 1 - j] = a.mat[i][j];
        }
    }
    return res;
}
matrix SumPowers(matrix &a , int _n) {
    if (_n == 0) return ZeroMatrix(a.siz);
    if (_n & 1) {
        return a * (Identity(a.siz) + SumPowers(a, _n - 1));
    } else {
        return SumPowers(a, _n / 2) * (Identity(a.siz) + FastPower(a, _n / 2));
    }
}
```

### Segment Tree
#### sweep line
```cpp
template<class T>
struct Fenwick{
    int n;
    vector<T>tree;
    int N = 1;
    void init(int _n){
        n=_n;
        tree.resize(this->n);
        N = log2_floor(n) + 1;
    }
    void add(int pos,T value){
        for(int i=pos+1;i<=n;i+=i&-i)tree[i-1]+=value;
    }
    T get(int pos) {
        T sum = 0;
        for (int i = pos + 1; i; i -= i & -i)sum += tree[i - 1];
        return sum;
    }
    T query(int l,int r){
        return get(r)-get(l-1);//send zero base
    }
    int lower_bound(T t){
        T sum = 0;
        int pos = 0;
        for(int i = N; i >= 0; i--){
            int next_pos = pos + (1 << i);
            if(next_pos <= n && sum + tree[next_pos - 1] < t){
                sum += tree[next_pos - 1];
                pos = next_pos;
            }
        }
        return pos; // zero-based index
    }
};
vector<pair<int , int>>Horizontal[2 * N] , Vertical[2 * N];
signed main() {
    khaled
    int t;
    cin>>t;
    Fenwick<ll> seg;
    seg.init(2 * N);
    while(t--) {
        int n;cin>>n;
        vector<array<int , 4>>inital(n);
        vector<int>all;
        for(int i = 0;i < n;i++) {
            cin>>inital[i][0]>>inital[i][1]>>inital[i][2]>>inital[i][3];
            all.emplace_back(inital[i][0]);
            all.emplace_back(inital[i][1]);
            all.emplace_back(inital[i][2]);
            all.emplace_back(inital[i][3]);
        }
        sort(all.begin() , all.end());
        all.erase(unique(all.begin() , all.end()) , all.end());
        for(int i = 0;i < n;i++) {
            inital[i][0] = lower_bound(all.begin() , all.end() , inital[i][0]) - all.begin();
            inital[i][1] = lower_bound(all.begin() , all.end() , inital[i][1]) - all.begin();
            inital[i][2] = lower_bound(all.begin() , all.end() , inital[i][2]) - all.begin();
            inital[i][3] = lower_bound(all.begin() , all.end() , inital[i][3]) - all.begin();
        }
        int mx = -1;
        for(int i = 0;i < n;i++) {
            int x1 , x2 , y1 , y2;
            x1 = inital[i][0];y1 = inital[i][1];
            x2 = inital[i][2];y2 = inital[i][3];
            if(x1 > x2)swap(x1 , x2);
            if(y1 > y2)swap(y1 , y2);
            mx = max({mx ,x1 , x2 , y1 , y2});
            if(x1 == x2) {
                // vertical
                Vertical[x1].emplace_back(y1 , y2);
            }else {
                // horziontal
                Horizontal[x1].emplace_back(y1 , 1);
                Horizontal[x2].emplace_back(y2 , -1);
            }
        }
        ll res = 0;
        for(int i = 0;i <= all.size();i++) {
            for(auto &val:Horizontal[i]) {
                if(val.second == 1)
                    seg.add(val.first , val.second);
            }
            for(auto &val:Vertical[i]) {
                res += seg.query(val.first , val.second);
            }
            for(auto &val:Horizontal[i]) {
                if(val.second == -1)
                    seg.add(val.first , val.second);
            }
            Horizontal[i].clear();
            Vertical[i].clear();
        }
        for(int i = 0;i <= all.size();i++) {
            seg.add(i , -seg.query(i , i));
        }
        cout<<res<<line;
    }
}
```

#### 2d seg
```cpp
/*
    2D segment Tree
    - Memory allocation: O(4n x 4m)
    - range [l..r] , r: inclusive 
    - Base: 0-index
    
    Function description:
        1. upd(r , c , val): update cell (r,c) with value val
           Time complexity: O(log(n) x log(m))
        2. qry(lx , rx , ly , ry): find (min,max,sum,produc) of rectangle [lx...rx] x [ly...ry]
           Time complexity: O(log(n) x log(m))
        3. init(n , m): initial the segment tree 
        4. build(g): build the segment tree with grid[n][m]
           Time complexity: O(n log(n) x m log(m))
*/
struct Node{
    int v;
    Node(){ v = 0;} 
    Node(int x){ this->v = x; } 
    Node operator +(const Node &other) const{
        Node res;
        res.v = __gcd(v , other.v); // custom operator
        return res;
    }
};
struct segTree_2d{
private: 
    vector<vector<Node>> t;
    int n , m;
    void build_y(int vx, int lx, vector<vector<int>> &g ,int rx, int vy, int ly, int ry) {
        if (ly == ry) {
            if (lx == rx){
                t[vx][vy] = Node(g[lx][ly]);
            }
            else{
                t[vx][vy] = t[vx*2][vy] + t[vx*2+1][vy];
            }
        } else {
            int my = (ly + ry) / 2;
            build_y(vx, lx, g, rx, vy*2, ly, my);
            build_y(vx, lx, g, rx, vy*2+1, my+1, ry);
            t[vx][vy] = t[vx][vy*2] + t[vx][vy*2+1];
        }
    }
    void build_x(vector<vector<int>> &g , int vx, int lx, int rx) {
        if (lx != rx) {
            int mx = (lx + rx) / 2;
            build_x(g , vx*2, lx, mx);
            build_x(g , vx*2+1, mx+1, rx);
        }
        build_y(vx, lx, g, rx, 1, 0, m-1);
    }
    Node qry_y(int vx, int vy, int tly, int try_, int ly, int ry) {
        if (ly > ry) 
            return Node();
        if (ly == tly && try_ == ry)
            return Node(t[vx][vy]);
        int tmy = (tly + try_) / 2;
        return qry_y(vx, vy*2, tly, tmy, ly, min(ry, tmy)) + 
               qry_y(vx, vy*2+1, tmy+1, try_, max(ly, tmy+1), ry);
    }
 
    Node qry_x(int vx, int tlx, int trx, int lx, int rx, int ly, int ry) {
        if (lx > rx)
            return Node();
        if (lx == tlx && trx == rx)
            return qry_y(vx, 1, 0, m-1, ly, ry);
        int tmx = (tlx + trx) / 2;
        return qry_x(vx*2, tlx, tmx, lx, min(rx, tmx), ly, ry) + 
               qry_x(vx*2+1, tmx+1, trx, max(lx, tmx+1), rx, ly, ry);
    }
    void upd_y(int vx, int lx, int rx, int vy, int ly, int ry, int x, int y, int nval) {
        if (ly == ry) {
            if (lx == rx)
                t[vx][vy] = nval;
            else
                t[vx][vy] = t[vx*2][vy] + t[vx*2+1][vy];
        } else {
            int my = (ly + ry) / 2;
            if (y <= my)
                upd_y(vx, lx, rx, vy*2, ly, my, x, y, nval);
            else
                upd_y(vx, lx, rx, vy*2+1, my+1, ry, x, y, nval);
            t[vx][vy] = t[vx][vy*2] + t[vx][vy*2+1];
        }
    }
    void upd_x(int vx, int lx, int rx, int x, int y, int nval) {
        if (lx != rx) {
            int mx = (lx + rx) / 2;
            if (x <= mx)
                upd_x(vx*2, lx, mx, x, y, nval);
            else
                upd_x(vx*2+1, mx+1, rx, x, y, nval);
        }
        upd_y(vx, lx, rx, 1, 0, m-1, x, y, nval);
    }
public: 
    void init(int n, int m){
        this->n = n;
        this->m = m;
        int r = 1 , c = 1;
        while(r < n) r *= 2;
        while(c < m) c *= 2;
        t = vector<vector<Node>>(2 * r , vector<Node>(2 * c));    
    }
    void build(vector<vector<int>> &g){
        build_x(g , 1 , 0 , n - 1);
    }
    int qry(int x , int y , int xx , int yy){
        return qry_x(1 , 0 , n - 1 , x , xx , y , yy).v; 
    }
    void upd(int r , int c , int val){
        upd_x(1 , 0 , n - 1 , r , c , val);
    }
};
```

#### add arithmetic sequence
```cpp
struct Node {
    ll firstValue, Increment;
    Node(ll f = 0, ll i = 0) {
        firstValue = f;
        Increment = i;
    }
    Node operator+(const Node &a) {
        Node res;
        res.firstValue = firstValue + a.firstValue;
        res.Increment = Increment + a.Increment;
        return res;
    }
};

struct SegmentTree {
    vector<ll> tree;
    vector<Node> lazy;

    SegmentTree(int n) {
        tree.resize(4 * n);
        lazy.resize(4 * n);
    }

    void build(int node, int nl, int nr) {
        if (nl == nr) {
            tree[node] = 0;
            return;
        }
        int mid = nl + (nr - nl) / 2;
        build(2 * node + 1, nl, mid);
        build(2 * node + 2, mid + 1, nr);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    ll sum(ll s, ll inc, ll len) {
        return 1LL * s * len + 1LL * inc * (len - 1) * len / 2;
    }

    void prop(int node, int nl, int nr) {
        if (lazy[node].Increment) {
            tree[node] += sum(lazy[node].firstValue, lazy[node].Increment, nr - nl + 1);
            if (nl != nr) {
                lazy[2 * node + 1] = lazy[2 * node + 1] + lazy[node];
                ll mid = nl + (nr - nl) / 2;
                lazy[2 * node + 2] = lazy[2 * node + 2] + Node(
                    lazy[node].firstValue + lazy[node].Increment * (mid - nl + 1),
                    lazy[node].Increment
                );
            }
            lazy[node].Increment = 0;
            lazy[node].firstValue = 0;
        }
    }

    void upd(int node, int nl, int nr, int l, int r, ll f, ll inc) {
        prop(node, nl, nr);
        if (nl > r || nr < l) return;
        if (nl >= l && nr <= r) {
            lazy[node].firstValue = (nl - l) * inc + f;
            lazy[node].Increment += inc;
            prop(node, nl, nr);
            return;
        }
        ll mid = nl + (nr - nl) / 2;
        upd(2 * node + 1, nl, mid, l, r, f, inc);
        upd(2 * node + 2, mid + 1, nr, l, r, f, inc);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    ll query(int node, int nl, int nr, int l, int r) {
        prop(node, nl, nr);
        if (nl >= l && nr <= r) return tree[node];
        if (nl > r || nr < l) return 0;
        int mid = nl + (nr - nl) / 2;
        return query(2 * node + 1, nl, mid, l, r) + query(2 * node + 2, mid + 1, nr, l, r);
    }
};
```

#### segment tree of spare table
```cpp
/*
    2D segment sparse table
    - Memory allocation: O(4n x mlog(m))
    - range [l..r] , r: inclusive 
    - Base: 0-index
    
    Function description:
        1. build(n , m , n): build the segment tree with grid[n][m]
           Time complexity: O(4n x mlog(m))
        2. qry(lx , rx , ly , ry): find (min,max,sum,produc) of rectangle [lx...rx] x [ly...ry]
           Time complexity: O(log(n))
*/
template<class T = int>
struct sparse_2d {
    int n, m, LOG , N;
    T DEFAULT = 0;
    vector<vector<vector<T>>> tree;
 
    void build(int _n, int _m , vector<vector<T>> &a) {
        n = _n;
        m = _m;
        LOG = 31 - __builtin_clz(m) + 1;
 
        N = 1;
        while(N < n) N *= 2; // N must be power of 2
        tree = vector<vector<vector<T>>>(2 * N + 1, vector<vector<T>>(LOG, vector<T>(m + 1)));
 
        for (int x = 0; x < n; x++)
        {
            for (int y = 0; y < m; y++)
                tree[N + x][0][y] = a[x][y];
            for (int k = 1; k < LOG; k++)
                for (int y = 0; y + (1 << k) <= m; y++)
                    tree[N + x][k][y] = comb(tree[N + x][k - 1][y], tree[N + x][k - 1][y + (1 << (k - 1))]);
        }
        for (int v = N - 1; v > 0; v--)
            for (int k = 0; k < LOG; k++)
                for (int y = 0; y + (1 << k) <= m; y++)
                    tree[v][k][y] = comb(tree[2 * v][k][y], tree[2 * v + 1][k][y]);
        return;
    }
 
    T comb(T a, T b) {
        return __gcd(a, b); // Change the custom operator
    }
 
    T qry(int v, int a, int b, int x1, int x2, int y1, int y2, int k) {
        if (x1 <= a && b <= x2){
            return comb(tree[v][k][y1], tree[v][k][y2]); 
        }
        if (x1 >= b || a >= x2) return 0;
        int mid = (a + b) / 2;
        return comb(qry(2 * v, a, mid, x1, x2, y1, y2, k), qry(2 * v + 1, mid, b, x1, x2, y1, y2, k));
    }
 
    T qry(int lx, int ly, int rx, int ry) {
        int k = 31 - __builtin_clz(ry - ly + 1);
        return qry(1, 0, N, lx, rx + 1, ly, ry - (1 << k) + 1, k);
    }
};
```

#### segment tree geometric progression
```cpp
const int MOD = 1e9 + 7;
ll power(ll base, ll exp) {
    ll result = 1;
    base %= MOD;
    while (exp > 0) {
        if (exp & 1) result = (result * base) % MOD;
        base = (base * base) % MOD;
        exp >>= 1;
    }
    return result;
}
class SegmentTree {
private:
    int n;
    vector<ll> tree, lazy_a, lazy_r;  // lazy_a: coefficient, lazy_r: first power of K
    ll K;

    // Get geometric sum: a*r^0 + a*r^1 + ... + a*r^(n-1)
    ll geometric_sum(ll a, ll r, ll n) {
        if (n == 0) return 0;
        if (n == 1) return a % MOD;
        // Using formula: a*(1-r^n)/(1-r) for r != 1
        ll rn = power(r, n);
        ll denominator = power(MOD + 1 - r, MOD - 2); // Modular multiplicative inverse of (1-r)
        return (a * ((1 - rn + MOD) % MOD) % MOD * denominator) % MOD;
    }

    void push_down(int node, int left, int right) {
        if (lazy_a[node] == 0) return;

        if (left != right) {
            int mid = (left + right) >> 1;
            int left_child = node << 1;
            int right_child = left_child | 1;

            // Left child
            lazy_a[left_child] = (lazy_a[left_child] + lazy_a[node]) % MOD;
            lazy_r[left_child] = lazy_r[node];

            // Right child - adjust power of K by length of left subtree
            lazy_a[right_child] = (lazy_a[right_child] +
                                 (lazy_a[node] * power(K, mid - left + 1)) % MOD) % MOD;
            lazy_r[right_child] = (lazy_r[node] + (mid - left + 1)) % (MOD - 1);
        }

        // Update current node
        tree[node] = (tree[node] + geometric_sum(lazy_a[node], K, right - left + 1)) % MOD;
        lazy_a[node] = 0;
        lazy_r[node] = 0;
    }

    void update_range(int node, int tree_left, int tree_right, int query_left, int query_right, ll val) {
        push_down(node, tree_left, tree_right);

        if (tree_left > query_right || tree_right < query_left) return;

        if (tree_left >= query_left && tree_right <= query_right) {
            ll offset = tree_left - query_left;
            lazy_a[node] = (val * power(K, offset)) % MOD;
            lazy_r[node] = offset;
            push_down(node, tree_left, tree_right);
            return;
        }

        int mid = (tree_left + tree_right) >> 1;
        update_range(node << 1, tree_left, mid, query_left, query_right, val);
        update_range((node << 1) | 1, mid + 1, tree_right, query_left, query_right, val);

        tree[node] = (tree[node << 1] + tree[(node << 1) | 1]) % MOD;
    }

    ll query_range(int node, int tree_left, int tree_right, int query_left, int query_right) {
        if (tree_left > query_right || tree_right < query_left) return 0;

        push_down(node, tree_left, tree_right);

        if (tree_left >= query_left && tree_right <= query_right) {
            return tree[node];
        }

        int mid = (tree_left + tree_right) >> 1;
        return (query_range(node << 1, tree_left, mid, query_left, query_right) +
                query_range((node << 1) | 1, mid + 1, tree_right, query_left, query_right)) % MOD;
    }

public:
    SegmentTree(int size, ll k) : n(size), K(k) {
        tree.resize(4 * n);
        lazy_a.resize(4 * n);
        lazy_r.resize(4 * n);
    }

    void update(int left, int right, ll val) {
        update_range(1, 0, n - 1, left, right, val);
    }

    ll query(int left, int right) {
        return query_range(1, 0, n - 1, left, right);
    }

    vector<ll> get_final_array() {
        vector<ll> result(n);
        for (int i = 0; i < n; i++) {
            result[i] = query(i, i);
        }
        return result;
    }
};
```


#### sub array max sum

```cpp
struct Node {
    // note: you don't consider empty subarray so if you want max(answer , 0)
    ll left, right, max, sum;

    Node(ll a = -inf, ll b = -inf, ll c = -inf, ll d = -inf) {
        sum = a, left = b, right = c, max = d;
    }

    Node operator+(const Node &a) {
        Node res;
        res.sum = a.sum + sum;
        res.left = std::max(left, sum + a.left);
        res.right = std::max(a.right, a.sum + right);
        res.max = std::max({max, a.max, right + a.left});
        return res;
    }

};
struct SegmentTree {
    vector<Node> tree;
    vector<ll> lazy;
    int n;

    SegmentTree(int _n) {
        n = _n;
        tree.resize(4 * n);
        lazy.resize(4 * n, -inf);
    }

    void build(int node, int nl, int nr, vector<ll> &v) {

        if (nl == nr) {
            tree[node] = Node(v[nl], v[nl], v[nl], v[nl]);
            return;
        }
        int mid = nl + (nr - nl) / 2;
        build(2 * node + 1, nl, mid, v);
        build(2 * node + 2, mid + 1, nr, v);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    void prop(int node, int nl, int nr) {
        if (lazy[node] != -inf) {
            ll val = 1LL * (nr - nl + 1) * lazy[node];
            tree[node] = Node(val, val, val, val);
            if (nl != nr) {
                lazy[node * 2 + 1] = lazy[node];
                lazy[node * 2 + 2] = lazy[node];
            }
            lazy[node] = -inf;
        }
    }

    void update(int node, int nl, int nr, int l, int r, ll newValue) {
        prop(node, nl, nr);
        if (nl >= l && nr <= r) {
            ll val = 1LL * (nr - nl + 1) * newValue;
            tree[node] = Node(val, val, val, val);
            if (nl != nr) {
                lazy[node * 2 + 1] = newValue;
                lazy[node * 2 + 2] = newValue;
            }
            return;
        }
        if (nl > r || nr < l)return;
        int mid = nl + (nr - nl) / 2;
        update(2 * node + 1, nl, mid, l, r, newValue);
        update(2 * node + 2, mid + 1, nr, l, r, newValue);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    Node query(int node, int nl, int nr, int l, int r) {
        prop(node, nl, nr);
        if (nl >= l && nr <= r)return tree[node];
        if (nl > r || nr < l)return Node();
        int mid = nl + (nr - nl) / 2;
        return query(2 * node + 1, nl, mid, l, r) + query(2 * node + 2, mid + 1, nr, l, r);
    }
};
```

#### segment tree min-max-sum
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int MAXN = 200001;  // 1-based

int N;
ll A[MAXN];

struct Node {
	ll sum;   // Sum tag
	ll max1;  // Max value
	ll max2;  // Second Max value
	ll maxc;  // Max value count
	ll min1;  // Min value
	ll min2;  // Second Min value
	ll minc;  // Min value count
	ll lazy;  // Lazy tag
} T[MAXN * 4];

void merge(int t) {
	// sum
	T[t].sum = T[t << 1].sum + T[t << 1 | 1].sum;

	// max
	if (T[t << 1].max1 == T[t << 1 | 1].max1) {
		T[t].max1 = T[t << 1].max1;
		T[t].max2 = max(T[t << 1].max2, T[t << 1 | 1].max2);
		T[t].maxc = T[t << 1].maxc + T[t << 1 | 1].maxc;
	} else {
		if (T[t << 1].max1 > T[t << 1 | 1].max1) {
			T[t].max1 = T[t << 1].max1;
			T[t].max2 = max(T[t << 1].max2, T[t << 1 | 1].max1);
			T[t].maxc = T[t << 1].maxc;
		} else {
			T[t].max1 = T[t << 1 | 1].max1;
			T[t].max2 = max(T[t << 1].max1, T[t << 1 | 1].max2);
			T[t].maxc = T[t << 1 | 1].maxc;
		}
	}

	// min
	if (T[t << 1].min1 == T[t << 1 | 1].min1) {
		T[t].min1 = T[t << 1].min1;
		T[t].min2 = min(T[t << 1].min2, T[t << 1 | 1].min2);
		T[t].minc = T[t << 1].minc + T[t << 1 | 1].minc;
	} else {
		if (T[t << 1].min1 < T[t << 1 | 1].min1) {
			T[t].min1 = T[t << 1].min1;
			T[t].min2 = min(T[t << 1].min2, T[t << 1 | 1].min1);
			T[t].minc = T[t << 1].minc;
		} else {
			T[t].min1 = T[t << 1 | 1].min1;
			T[t].min2 = min(T[t << 1].min1, T[t << 1 | 1].min2);
			T[t].minc = T[t << 1 | 1].minc;
		}
	}
}

void push_add(int t, int tl, int tr, ll v) {
	if (v == 0) { return; }
	T[t].sum += (tr - tl + 1) * v;
	T[t].max1 += v;
	if (T[t].max2 != -llINF) { T[t].max2 += v; }
	T[t].min1 += v;
	if (T[t].min2 != llINF) { T[t].min2 += v; }
	T[t].lazy += v;
}

// corresponds to a chmin update
void push_max(int t, ll v, bool l) {
	if (v >= T[t].max1) { return; }
	T[t].sum -= T[t].max1 * T[t].maxc;
	T[t].max1 = v;
	T[t].sum += T[t].max1 * T[t].maxc;
	if (l) {
		T[t].min1 = T[t].max1;
	} else {
		if (v <= T[t].min1) {
			T[t].min1 = v;
		} else if (v < T[t].min2) {
			T[t].min2 = v;
		}
	}
}

// corresponds to a chmax update
void push_min(int t, ll v, bool l) {
	if (v <= T[t].min1) { return; }
	T[t].sum -= T[t].min1 * T[t].minc;
	T[t].min1 = v;
	T[t].sum += T[t].min1 * T[t].minc;
	if (l) {
		T[t].max1 = T[t].min1;
	} else {
		if (v >= T[t].max1) {
			T[t].max1 = v;
		} else if (v > T[t].max2) {
			T[t].max2 = v;
		}
	}
}

void pushdown(int t, int tl, int tr) {
	if (tl == tr) return;
	// sum
	int tm = (tl + tr) >> 1;
	push_add(t << 1, tl, tm, T[t].lazy);
	push_add(t << 1 | 1, tm + 1, tr, T[t].lazy);
	T[t].lazy = 0;

	// max
	push_max(t << 1, T[t].max1, tl == tm);
	push_max(t << 1 | 1, T[t].max1, tm + 1 == tr);

	// min
	push_min(t << 1, T[t].min1, tl == tm);
	push_min(t << 1 | 1, T[t].min1, tm + 1 == tr);
}

void build(int t = 1, int tl = 0, int tr = N - 1) {
	T[t].lazy = 0;
	if (tl == tr) {
		T[t].sum = T[t].max1 = T[t].min1 = A[tl];
		T[t].maxc = T[t].minc = 1;
		T[t].max2 = -llINF;
		T[t].min2 = llINF;
		return;
	}

	int tm = (tl + tr) >> 1;
	build(t << 1, tl, tm);
	build(t << 1 | 1, tm + 1, tr);
	merge(t);
}

void update_add(int l, int r, ll v, int t = 1, int tl = 0, int tr = N - 1) {
	if (r < tl || tr < l) { return; }
	if (l <= tl && tr <= r) {
		push_add(t, tl, tr, v);
		return;
	}
	pushdown(t, tl, tr);

	int tm = (tl + tr) >> 1;
	update_add(l, r, v, t << 1, tl, tm);
	update_add(l, r, v, t << 1 | 1, tm + 1, tr);
	merge(t);
}

void update_chmin(int l, int r, ll v, int t = 1, int tl = 0, int tr = N - 1) {
	if (r < tl || tr < l || v >= T[t].max1) { return; }
	if (l <= tl && tr <= r && v > T[t].max2) {
		push_max(t, v, tl == tr);
		return;
	}
	pushdown(t, tl, tr);

	int tm = (tl + tr) >> 1;
	update_chmin(l, r, v, t << 1, tl, tm);
	update_chmin(l, r, v, t << 1 | 1, tm + 1, tr);
	merge(t);
}

void update_chmax(int l, int r, ll v, int t = 1, int tl = 0, int tr = N - 1) {
	if (r < tl || tr < l || v <= T[t].min1) { return; }
	if (l <= tl && tr <= r && v < T[t].min2) {
		push_min(t, v, tl == tr);
		return;
	}
	pushdown(t, tl, tr);

	int tm = (tl + tr) >> 1;
	update_chmax(l, r, v, t << 1, tl, tm);
	update_chmax(l, r, v, t << 1 | 1, tm + 1, tr);
	merge(t);
}

ll query_sum(int l, int r, int t = 1, int tl = 0, int tr = N - 1) {
	if (r < tl || tr < l) { return 0; }
	if (l <= tl && tr <= r) { return T[t].sum; }
	pushdown(t, tl, tr);

	int tm = (tl + tr) >> 1;
	return query_sum(l, r, t << 1, tl, tm) +
	       query_sum(l, r, t << 1 | 1, tm + 1, tr);
}

int main() {
	int Q;

	cin >> N >> Q;
	for (int i = 0; i < N; i++) { cin >> A[i]; }
	build();
	for (int q = 0; q < Q; q++) {
		int t;
		cin >> t;
		if (t == 0) {
			int l, r;
			ll x;
			cin >> l >> r >> x;
			update_chmin(l, r - 1, x);
		} else if (t == 1) {
			int l, r;
			ll x;
			cin >> l >> r >> x;
			update_chmax(l, r - 1, x);
		} else if (t == 2) {
			int l, r;
			ll x;
			cin >> l >> r >> x;
			update_add(l, r - 1, x);
		} else if (t == 3) {
			int l, r;
			cin >> l >> r;
			cout << query_sum(l, r - 1) << '\n';
		}
	}
}
```


<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>


### Techniques

#### arithmetic progression sum

```cpp
ll sum(ll s, ll inc, ll len) {
        return (len * (2 * s + (len - 1) * inc)) / 2;
    }
```

#### biggest rectangle all ones
```cpp
int calc(const vector<int>& heights) {
    int result = 0;
    stack<int> stack;

    for (int i = 0; i <= heights.size(); ++i) {
        while (!stack.empty() && (i == heights.size() || heights[stack.top()] > heights[i])) {
            const int m = heights[stack.top()];
            stack.pop();
            const int v = stack.empty() ? i : i - stack.top() - 1;
            result = max(result, m * v);
        }
        stack.push(i);
    }

    return result;
}
int BiggestRec(vector<vector<int>>& matrix) {
    if (matrix.empty())
        return 0;

    int result = 0;
    vector<int> hist(matrix[0].size());

    for (auto &row: matrix) {
        for (int i = 0; i < row.size(); ++i)
            hist[i] = row[i] == 0 ? 0 : hist[i] + 1;
        result = max(result, calc(hist));
    }
    return result;
}

```

#### target number of connected components of grid
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

signed main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    // problem Given n , m , k generate matrix with size n * m and has k connected coponenets either 0 or 1.
    int n,m,k;cin>>n>>m>>k;
    int a[n][m];
    if(n==1||m==1){
        cout<<"YES"<<'\n';
        int num=0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                cout<<num;
                if(k>1) num=1-num,k--;
            }
            cout<<'\n';
        }
    }else if(k==n*m-1){
        cout<<"NO\n";
    }else{
        cout<<"YES\n";
        int num=0;
        int flag=0;
        if((k+1)%m==0) k--,flag=1;
        int i,j;
        for(i=0;i<n;i++){
            for(j=0;j<m;j++){
                a[i][j]=num;
                num=1-num;
                k--;
                if(k<=0) break;
            }
            if(k<=0) break;
            if(m%2==0) num=1-num;
        }
        if(i==0){
            for(int x=j+1;x<m;x++) a[i][x]=1-num;
        }else if(j+1<m){
            a[i][j+1]=num;
        }
        for(int x=0;x<m;x++){
            int st;
            if(x<=j+1||i==0) st=i;
            else st=i-1;
            for(int y=st+1;y<n;y++) a[y][x]=a[st][x];
        }
        if(flag) a[n-1][m-1]=1-a[n-1][m-2];
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                cout<<a[i][j];
            }
            cout<<"\n";
        }
    }

}

```

#### sub arrays divisible by k
```cpp
int n;
int arr[N];
int CountSubArrayDivision(int k){
    map<int,int>mp;
    int sum = 0;
    mp[sum] = 1;
    for(int i=0;i<n;i++){
        sum+=arr[i];
        mp[add(sum,0,k)]++;
    }
    int res = 0;
    for(auto &val:mp){
        res += val.second * (val.second - 1) / 2;
    }
    return res;
}
```

#### count number of subarrays given max , min
```cpp
int CountSubArraysWithKnownMxAndMn(vector<int>&v , int mn,int mx){
    int ans=0;
    int maxi=-1, mini=-1;
    int s=(int)v.size();
    for(int r=0, l=0; r<s; r++){
        int x=v[r];
        if (x<mn ||x>mx){
            l=r+1;
            continue;
        }
        if (x==mx) maxi=r;
        if (x==mn) mini=r;
        ans+=max((min(maxi, mini)-l+1),0ll);
    }
    return ans;
}
```

#### first k shortest paths
```cpp
vector<pair<int,int>> adj[N];
int n , m, k;
signed main() {
    khaled
    cin >> n >> m >> k;
    for(int i = 0; i < m; ++i){
        int u , v, w;
        cin >> u >> v >> w;
        --u, --v;
        adj[u].emplace_back(v , w);
    }
    priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>> q;
    vector<int> cnt(n) , ans;
    int start = 0 , end = n - 1;
    q.push({0 , start});
    while(q.size() && cnt[end] < k){
        auto [d , u] = q.top(); q.pop();
        cnt[u]++;
        if(u == end) ans.emplace_back(d);
        if(cnt[u] <= k){
            for(auto &[v,  w] : adj[u]) {
                if (cnt[v] < k)
                    q.push({(w + d), v});
            }
        }
    }
    sort(ans.begin() , ans.end());
    for(auto &w : ans) cout << w << " ";
}

```


#### maximize distance between 2 sequences
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;


int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);cout.tie(nullptr);
    int t;cin>>t;
    while(t--) {
        int n;cin>>n;
        deque<int>a(n) , b(n);
        for(auto &val:a)cin>>val;
        for(auto &val:b)cin>>val;
        sort(a.begin() , a.end());
        sort(b.begin() , b.end());
        int res = 0;
        while(n--) {
            int temp = 2e9;
            for(int i = 0;i < a.size();++i) {
                temp = min(temp , abs(a[i] - b[i]));
            }
            res = max(res , temp);
            b.push_front(b.back());
            b.pop_back();
        }
        cout<<res<<'\n';
    }
}
```

#### mex in o(n)
```cpp
ll mex(const vector<ll>&a) {
    vector<ll>b = a;
    int n = a.size();
    for(int i = 0;i < n;i++) {
        while(b[i] >= 0 && b[i] < n && b[b[i]] != b[i]) {
            swap(b[i] , b[b[i]]);
        }
    }
    for(int i = 0;i < n;i++) {
        if(b[i] != i)return i;
    }
    return n;
}
```

#### min number of swaps to make binary string equal
```cpp
#include <ext/pb_ds/assoc_container.hpp> // Common file
#include <ext/pb_ds/tree_policy.hpp> // Including tree_order_statistics_node_update
using namespace __gnu_pbds;
template<class T> using ordered_set = tree<T, null_type , less_equal<T> , rb_tree_tag , tree_order_statistics_node_update> ;
int solve(string &s1 , string &s2){
	if(s1.size() != s2.size() || count(s1.begin() , s1.end() , '1') != count(s2.begin() , s2.end() , '1')){
		return INT32_MAX;
	}
	// cerr << s1 << ' ' << s2 << '\n';
	ordered_set<int> pos[2];
	for(int i = 0; i < s1.size(); ++i){
		pos[s1[i] == '1'].insert(i);
	}
	ll ans = 0;
	for(auto &ch : s2){
		int f = (ch == '1');
		assert(pos[f].size());
		int i = *pos[f].find_by_order(0);
		int ope = pos[f ^ 1].order_of_key(i);
		ans += ope;
		pos[f].erase( pos[f].lower_bound(i - 1) );
	}
	return ans;
}
```


#### mth digit after decimal point
```cpp
int solve(int n,int m){
    //return the mth digit after the decimal point 
    if(n == 1){
        return 0;
    }
    return 10 * fast_power(10, m - 1,n) / n;
}

```

#### subset xor sum
```cpp
int xorSum(vector<int>&arr, int n)
{
    int bits = 0;
    for (int i=0; i < n; ++i)
        bits |= arr[i];
    int ans = mul(bits , fast_power(2ll, n-1,mod),mod);
    return ans;
}
```

#### xor sum for range
```cpp
ll range_xor(ll l, ll r) {
    ll res = 0;
    if (l&1) res ^= l++;
    if (!(r&1)) res ^= r--;
    if ((r-l+1)/2&1) res ^= 1;
    return res;
}
```

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>



# templates
```cpp
#include<bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp> // Common file
#include <ext/pb_ds/tree_policy.hpp> // Including tree_order_statistics_node_update
#include <tr2/dynamic_bitset>
using namespace __gnu_pbds; // for ordered set
using namespace std; // global name space
using namespace tr2; // for dynamic_bitset<>
//#define int long long
//#define double long double
#define line '\n'
typedef long long ll;
#define khaled ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
template<typename T>
using ordered_set=tree<T,null_type,less_equal<T>,rb_tree_tag,tree_order_statistics_node_update>;
bool valid(long long i,long long j,long long n,long long m){return i>=0&&i<n&&j>=0&&j<m;}
long long mul(long long x,long long y,const long long&mod){return ((x%mod)*(y%mod))%mod;}
long long add(long long x,long long y,const long long&mod){return (((x%mod)+(y%mod))%mod+mod)%mod;}
int SafeMul(int a,int b,int m){if(b==0)return 0;int res = SafeMul(a,b/2,m);res = add(res,res,m);if(b&1)return add(res,a,m);else return res;}
long long fast_power(long long base,long long power,const long long &m=INT64_MAX){if (power == 1 || power == 0)return base * power + (!power);long long res = (fast_power(base, power / 2, m) % m) % m;if (power & 1)return mul(base,mul(res,res,m),m);else return mul(res,res,m);}
int log2_floor(long long i) {return i ? __builtin_clzll(1) - __builtin_clzll(i) : 0;}
int power_of_2(int n){ return __builtin_popcountll(n)==1;}
bool line_checking(int a1,int b1,int a2,int b2,int a3,int b3) { return (b2-b1)*(a2-a3)==(b2-b3)*(a2-a1); }
pair<int,int> rotate(int i,int j,int n){ return make_pair(j,n-1-i); }
const int N = 2e5+5;
const int mod=1e9+7;
//const int mod = 998244353;
const int inf=1e9;
const double pi=3.14159265350979323846,eps=1e-10;
/*==============================================  KHALWSH  ==============================================*/

signed main() {
    khaled

}

```

