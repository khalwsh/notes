---
share_link: https://share.note.sx/q0rs48z3#2rFzvVymcAvxTe2JdhPAhuEILEQA6oFDo7I58S41jzQ
share_updated: 2025-04-11T17:34:44+02:00
---
```cpp
#include <cassert>  
#include <iostream>  
#include <vector>  
using namespace std;  
/*
Kareem is a devoted caretaker tending the grand mosque courtyard during the sacred nights of Ramadan. The courtyard is arranged in a grid, where each cell holds a lantern that is either lit (represented by 1) or unlit (represented by 0). His challenge is steeped in tradition and mystique:

If a lantern was originally lit, then every lantern that shares a diagonal with it must remain lit—spreading only the light of the originally illuminated lanterns, not those turned on by the process.

Your task is to help Kareem accomplish his mission before the Taraweeh prayers begin.

- @param n:The number of rows in the grid (1 ≤ n ≤ 1000)
    
- @param m:The number of columns in the grid (1 ≤ m ≤ 1000)
    
- @param grid : n * m grid of characters either 0 or 1 representing the grid

Example 1:
Input : n = 3 , m = 3 , grid = {{'0' , '1' , '0'} ,{'0' , '1' , '0'} ,{'0' , '0' , '0'}  }
output : {{'1' , '1' , '1'} ,{'1' , '1' , '1'} ,{'1' , '0' , '1'}  }

Example 2 : 
Input : n = 1 , m = 2 , grid = {{'0' , '1'}}
output : {{'0' , '1'}}
*/
bool valid(int i , int j , int n , int m) {  
    return i >= 0 && j >= 0 && i < n && j < m;  
}  
vector<vector<char>>diagonals(int n , int m , vector<vector<char>>&grid) {  
    auto res = grid;  
    for (int i = 0; i < n; i++) {  
        for (int j = 0; j < m; j++) {  
            if (grid[i][j] == '1') {  
                for (auto [dx, dy] : {pair<int,int>{1, -1}, {1, 1}, {-1, -1}, {-1, 1}}) {  
                    int ni = i + dx, nj = j + dy;  
                    while (valid(ni, nj, n, m) && grid[ni][nj] == '0') {  
                        res[ni][nj] = '1';  
                        ni += dx;  
                        nj += dy;  
                    }  
                }  
            }  
        }  
    }  
  
    return res;  
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
if (diagonals(3, 3, {  
        {'0', '1', '0'},  
        {'0', '1', '0'},  
        {'0', '0', '0'}  
    }) != vector<vector<char>>{  
        {'1', '1', '1'},  
        {'1', '1', '1'},  
        {'1', '0', '1'}  
    })  
    assert(false);  
  
test2:
if (diagonals(1, 2, {  
        {'0', '1'}  
    }) != vector<vector<char>>{  
        {'0', '1'}  
    })  
    assert(false);  
  
test3: 
if (diagonals(2, 2, {  
        {'0', '0'},  
        {'0', '0'}  
    }) != vector<vector<char>>{  
        {'0', '0'},  
        {'0', '0'}  
    })  
    assert(false);  
  
test4: 
if (diagonals(2, 2, {  
        {'1', '1'},  
        {'1', '1'}  
    }) != vector<vector<char>>{  
        {'1', '1'},  
        {'1', '1'}  
    })  
    assert(false);  
  
test5: 
if (diagonals(3, 3, {  
        {'0', '0', '0'},  
        {'0', '1', '0'},  
        {'0', '0', '0'}  
    }) != vector<vector<char>>{  
        {'1', '0', '1'},  
        {'0', '1', '0'},  
        {'1', '0', '1'}  
    })  
    assert(false);  
  
test6: 
if (diagonals(1, 1, {  
        {'0'}  
    }) != vector<vector<char>>{  
        {'0'}  
    })  
    assert(false);  
  
test7:
if (diagonals(1, 1, {  
        {'1'}  
    }) != vector<vector<char>>{  
        {'1'}  
    })  
    assert(false);  
  
test8: 
if (diagonals(10, 10, {  
        {'1','0','1','0','1','0','0','0','0','0'},  
        {'0','1','0','1','0','1','1','1','1','0'},  
        {'1','0','1','0','1','0','0','1','0','0'},  
        {'0','1','0','1','0','0','1','0','1','0'},  
        {'1','0','0','0','1','0','0','1','0','1'},  
        {'0','1','1','0','0','1','1','0','1','0'},  
        {'1','0','0','1','1','0','1','0','1','0'},  
        {'1','0','1','0','0','1','1','1','0','1'},  
        {'0','0','0','1','1','0','1','0','1','0'},  
        {'0','1','0','0','0','1','0','0','0','1'}  
    }) != vector<vector<char>>(10, vector<char>(10, '1')))  
    assert(false);
```