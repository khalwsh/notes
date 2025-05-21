```cpp
#include <bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
/*
you want to count the number of ditinct ordered pairs of (i , j) such that 0 <= i < j < N
where N is the total number of participants , A pair (i , j) is considered balanced if the sum of participant i's sweetness level and the reverse of the participants j's sweetness level is equal to the sum of participants j's sweetness level and the reverse of participant i's weetness level
			v[i] + rev(v[j]) = v[j] + rev(v[i])
@param v : array the array you want to count pairs at (0 <= v[i] <= 1e9)

Example 1
Input : v = {42 , 11 , 1 ,97}
output : 2

Example 2
Input : v = {13 , 10 , 35 , 24 , 76}
output : 4
*/
ll konafa (vector<int> v) {  
    map<ll , ll>mp;  
    const int mod = 1e9 + 7;  
    for(int i = 0;i < v.size();i++) {  
        ll x = v[i]  
        string temp = to_string(x);  
        reverse(temp.begin() , temp.end());  
        ll rev = stoll(temp);  
        mp[x - rev]++;  
    }  
    ll res = 0;  
    for(auto &val:mp) {  
        res += (val.second) * (val.second - 1) / 2;  
    }  
    cout<<res % mod<<'\n';  
}
```

