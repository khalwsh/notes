```cpp
#include <bits/stdc++.h>  
using namespace std;  
typedef long long ll; 
/*
Mayada challenged khaled to count how many distinct mariom number exist in a given array , Mayada define mariom number as a number can be formed by multiplying two or more given prime numbers , can you help khaled to count mariom number can be gernerated from the array , answer mod 1e9 + 7

@param v : vector represent the vector khaled need to count from (2 <= v[i] <= 1e9)
Example 1:
Input : v = {11}
output : 0
Example 2 : 
Input : v = {2 , 3 ,5 }
output : 4
*/
ll numbers(vector<int> v) {  
    const int mod = 1e9 + 7;  
    map<ll , int>mp;  
    for(int i = 0;i < v.size();i++) {  
        int x = v[i];  
        mp[x]++;  
    }  
    ll res = 1;  
    for(auto &val:mp) {  
        res = res * (val.second + 1) % mod;  
    }  
    res--;  
    res -= mp.size();  
    res = (res + mod) % mod;  
    return res;  
}
```

test
```cpp
if (number({11}) != 0) assert(false);  
if (number({2, 3, 5}) != 4) assert(false);  
if (number({2, 2, 2}) != 2) assert(false);  
if (number({2, 3, 2}) != 3) assert(false);  
if (number({7, 11, 13, 17}) != 11) assert(false);  
if (number({2, 3, 5, 7, 11, 13}) != 57) assert(false);  
if(number({2, 3, 5, 7, 11, 13 , 2, 3, 5, 7, 11, 13 , 7, 11, 13, 17}) != 3448)assert(false);
```