---
share_link: https://share.note.sx/dfutpm5h#zfP7aNtyzv/CTl3U+SsGHjeJaKx+++7lJ3QSrvm1e/U
share_updated: 2025-04-14T19:25:01+02:00
---
```cpp
#include<bits/stdc++.h>  
using namespace std;  

unsigned long long fawazir(unsigned long long a , unsigned long long b) {
/*
Mayada challenged khaled with hard challenge , she asked him to count how many number between a and b that have an odd number of digits
@param a the lowerbound of the interval (1 <= a <= 1e18)
@param b the upperbound of the interval (1 <= b <= 1e18)
Example 1: 
input : a = 6 , b = 12
output : 4
Example 2:
input : a = 10 , b = 101
output : 2
*/
    vector<pair<unsigned long long, unsigned long long>> intervals;  
    intervals.push_back({0ULL, 9ULL});  
  
    for (int d = 3; d <= 19; d += 2) {  
        unsigned long long low = 1;  
        for (int i = 1; i < d; i++)  
            low *= 10ULL;  
        unsigned long long high = low * 10ULL - 1;  
        intervals.push_back({low, high});  
    }  
    if(a > b) swap(a, b);  
  
    unsigned long long ans = 0;  
    for(auto &pr : intervals){  
        unsigned long long L = pr.first, R = pr.second;  
        unsigned long long start = max(a, L);  
        unsigned long long end = min(b, R);  
        if(start <= end)  
            ans += (end - start + 1);  
    }  
    return ans;  
}
```

Test Driver
```cpp
#include <cassert>
#include <iostream>
using namespace std;
Test cases:
test1:
assert(fawazir(6, 12) == 4ULL);  
test2:
assert(fawazir(10, 101) == 2ULL);  
test3:
assert(fawazir(7, 7) == 1ULL);  
test4:
assert(fawazir(10, 10) == 0ULL);  
test5:
assert(fawazir(12, 6) == 4ULL);
test6:
assert(fawazir(1012 , 23149234) == 9090000ULL);
test7:
assert(fawazir(1 , 1000000000000000000) == 90909090909090910ULL);
```