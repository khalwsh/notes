---
share_link: https://share.note.sx/sv3917tx#1XvJyCvT2ZHCd+k9ZRftH+qp6FXzZ2Qsg6gGZMV4LmY
share_updated: 2025-04-11T17:01:29+02:00
---
```cpp
#include<bits/stdc++.h>  
using namespace std;  
/*
Mayada is an adventurer exploring an ancient land filled with digital gems. Each gem has a number, and she collects points by adding the gem's value to her magical bag. However, there is a special rule:

If she encounters a gem with value 0, the bag instantly empties (resets) if it contains a non-zero amount of points , and she must start collecting from scratch.

Mayada wants to determine the following after collecting all gems:

- The maximum sum of points obtained before a reset or at the end.
    
- The minimum sum of points obtained before a reset or at the end.
    
- The number of times the sum was reset to zero.
can you help Mayada in her journey?

@param n the length of the array (1 <= n <= 100000)
@param arr the array itself (0 <= a[i] <= 1000)

Example 1:
Input : n = 6 , arr = {7 , 5 , 0 , 3 , 0 , 1}
output : {12 1 2}

Example 2: 
Input : n = 5 , arr = {2 , 8 , 4 , 0 , 3}
output : {14 , 3 , 1}
*/
vector<int> points(int n , vector<int> arr) {  
solution code:
    int sum = 0 , mx = 0 , c = 0 , mn = 1e9;  
    bool all = true;  
    for(int i = 0;i < n;i++) {  
        int x = arr[i];  
        all &= (x == 0);  
        if(x == 0 && sum != 0) {  
            mx = max(mx , sum);  
            mn = min(mn , sum);  
            c++;  
            sum = 0;  
        }  
  
        sum += x;  
    }  
    if(all) {  
        return vector<int>{0 , 0 , 0};  
    }  
    if(sum != 0) {  
        mx = max(mx , sum);  
        mn = min(mn , sum);  
    }  
    return vector<int>{mx , mn , c};  
}
```

Test Driver
```cpp
#include <cassert>
#include <iostream>
#include <vector>
using namespace std;
Test Cases:
test1 : 
assert(points(6, {7, 5, 0, 3, 0, 1) == vector<int>({12, 1, 2}));
test2 : 
assert(points(5, {2, 8, 4, 0, 3}) == vector<int>({14, 3, 1}));
test3 :
assert(points(3, {0, 0, 0}) == vector<int>({0, 0, 0}));
test4:
assert(points(5, {1, 2, 3, 4, 5}) == vector<int>({15, 15, 0}));
test5:
assert(points(7, {5, 0, 6, 0, 7, 0, 8}) == vector<int>({8, 5, 3}));
test6

assert(points(7, {10, 20, 0, 5, 5, 0, 15}) == vector<int>({30, 10, 2}));
```