---
share_link: https://share.note.sx/tlm0tiya#F6EwLQQS5wqiGOCsTghCPNhawzPaGRBN28AKMVM6Jjw
share_updated: 2025-04-11T16:38:53+02:00
---
cpp problem : 
problem description:
```cpp
#include<bits/stdc++.h>  
using namespace std;  
/*
mayada and her friend khaled decided to decorate their neighborhood with beautiful string of lanterns , the lanterns come in two colors : white and black , the white color is represented with 0 and black represented with 1
Each of them had a different idea for how the decorations should look. After a lot of arguments, they agreed on the following rules:

1. The decoration must consist of exactly n lanterns.
2. The string must contain exactly k black lanterns.
3. No two adjacent lanterns should be the same color (no two consecutive white or black lanterns).
can you help mayada and khaled to decide if it is possible to create such a decoration if yes return true otherwise return false

@param n the length of the string (1 <= n <= 100000)
@param k the number of black lanterns (1 <= k <= n)

output : boolean value represent does mayada and khaled can decorate the neighborhood or not

Example 1:
Input:  n = 7 , k = 5
output: false

Example 2
input : n = 3 , k = 1
output : true
*/
bool decoration(int n , int k) {  
Solution Code:
    string s1 , s2;  
    char values[]{'1' , '0'};  
    bool turn = false;  
    for(int i = 0;i < n;i++) {  
        s1 += values[turn];  
        s2 += values[!turn];  
        turn ^= true;  
    }  
    if(s1.find("00") == string::npos && s1.find("11") == string::npos && count(s1.begin() , s1.end() , '1') == k) {  
        return true;  
    }else{  
        s1 = s2;  
        if(s1.find("00") == string::npos && s1.find("11") == string::npos && count(s1.begin() , s1.end() , '1') == k) {  
            return true;  
        }else  
            return false;  
    }  
}
```

Test Driver
```cpp
#include <cassert>
#include <iostream>
#include <vector>
using namespace std;
Test Cases:
test1:
assert(decoration(7, 5) == false);
test2:
assert(decoration(3, 1) == true);
test3:
assert(decoration(4, 2) == true);
test4:
assert(decoration(4, 3) == false); 
test5:
assert(decoration(6, 3) == true);
test6:
assert(decoration(13, 7) == true);
test7:
assert(decoration(100, 7) == false);
test8:
assert(decoration(1000, 500) == true);
```