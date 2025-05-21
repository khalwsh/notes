```cpp
#include<bits/stdc++.h>  
using namespace std;  
typedef long long ll;  
/*
Mayada gave khalwsh 2 binary strings A , B and she asked him to transform A into B using special operation any number of times , he can pick any position k and flip the prefix ending at k lanterns , flip all lanterns from the start up to poition k , can you help him to know what is the minimum indices he needs to flip in order to convert A into B and in what order he flips them
@param a : string represnet the string you want to convert it , a[i] is 0 or 1
@param b : string represnet the string you want to reach to , b[i] is 0 or 1

Example 1:
input : a = "1101011" , b = "0110100"
output : {7 , 2 , 1}

Example 2:
input : a = "101" , b = "101"
output : {}
*/
vector<int> lanterns (string a , string b) {  
    bool state = false;  
    vector<int>v;  
    for(int i = a.size() - 1;i >= 0;i--) {  
        if(state ^ (a[i] == b[i]))continue;  
        state ^= true;v.emplace_back(i);  
    }  
    return v;  
}
```

Test driver
```cpp

    {

        string a = "101";
        string b = "101";
        vector<int> result = lanterns(a, b);
        vector<int> expected = {};
        assert(result == expected);
    }
    {
        string a = "0";
        string b = "1";
        vector<int> result = lanterns(a, b);
        vector<int> expected = {1};
        assert(result == expected);
    }
    {
        string a = "00";
        string b = "11";
        vector<int> result = lanterns(a, b);
        vector<int> expected = {2};
        assert(result == expected);
    }
    {
        string a = "1010";
        string b = "0101";
        vector<int> result = lanterns(a, b);
        vector<int> expected = {4};
        assert(result == expected);
    }
    {
        string a = "111000";
        string b = "000111";
        vector<int> result = lanterns(a, b);
        vector<int> expected = {6};
        assert(result == expected);
    }
```