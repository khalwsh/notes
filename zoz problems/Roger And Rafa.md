Dominoes : [dp , bit masks]
Anwar has a board of size n×m filled with integers.  there are k domino pieces available. Each domino is a 2×1 rectangle and can be placed either horizontally (1×2) or vertically (2×1).

Anwar wants to place some (or none) of the kk dominoes on the board such that:

1. Each cell is covered by at most one domino.
2. Dominoes must not overlap.
3. Each domino must be placed entirely within the board, either horizontally or vertically.
4. The score of a domino placement is defined as the bitwise XOR of the two integers covered by that domino.

Help Anwar to choose a valid placement of dominoes that maximizes the total score, which is the sum of the XOR values of all placed dominoes.

Input : 
- {list<list<int>>} grid : represent the grid you want to cover , Each cell contains an integer between 0 and 10^9. , grid consists of at most 10 lines and 100 columns
   - {int} k : represent the number of dominoes I can use  , (1 <= k <= 200)

Output : 
   {number} the maximum total score

Input Validation: 
- "-1" : if grid is not grid of integers 
- "-1" : if values in grid is not in range 0 to 10 ^ 9

# test 1  
grid = [[41, 18467, 6334, 26500, 19169, 15724, 11478, 29358, 26962, 24464, 5705, 28145, 23281, 16827, 9961, 491, 2995, 11942, 4827, 5436],  
        [2316, 3035, 22190, 1842, 288, 30106, 9040, 8942, 19264, 22648, 27446, 23805, 15890, 6729, 24370, 15350, 15006, 31101, 24393, 3548],  
        [28703, 23811, 31322, 30333, 17673, 4664, 15141, 7711, 28253, 6868, 25547, 27644, 32662, 32757, 20037, 12859, 8723, 9741, 27529, 778],  
        [132391, 14604, 3902, 153, 292, 12382, 17421, 18716, 19718, 19895, 5447, 21726, 14771, 11538, 1869, 19912, 25667, 26299, 17035, 9894]  
        ]  
k = 22  
expected = 195912  
assert max_domino_score(grid, k) == expected  
  
# test 2  grid = [[41, 18467, 6334, 26500, 19169, 15724, 11478, 29358, 26962, 24464, 5705, 28145, 23281, 16827, 9961, 491, 2995, 11942, 4827, 5436],  
        [32391, 14604, 3902, 153, 292, 12382, 17421, 18716, 19718, 19895, 5447, 21726, 14771, 11538, 1869, 19912, 25667, 26299, 17035, 9894],  
        [28703, 23811, 31322, 30333, 17673, 4664, 15141, 7711, 28253, 6868, 25547, 27644, 32662, 32757, 20037, 12859, 8723, 9741, 27529, 778],  
        [12316, 3035, 22190, 1842, 288, 30106, 9040, 8942, 19264, 22648, 27446, 23805, 15890, 6729, 24370, 15350, 15006, 31101, 24393, 3548]  
        ]  
k = 10  
expected = 63608  
assert max_domino_score(grid, k) == expected  
  
# test 3  grid = [[38, 29533, 6878, 28223, 8855, 11797, 8365, 32285, 10450, 30612, 5853, 28100, 1142, 281, 20537, 15921, 8945, 26285, 2997, 14680],  
        [29533, 6878, 28223, 17887, 31597, 20584, 12212, 2240, 9725, 32278, 2446, 590, 840, 18587, 16907, 21237, 23611, 12617, 12456, 867],  
        [29533, 6878, 28223, 17887, 31597, 20584, 12212, 31111, 7578, 17066, 7629, 29404, 12279, 13505, 24388, 11649, 12329, 7176, 2331, 19264],  
        [22114, 14136, 26928, 1102, 21652, 8404, 24337, 27856, 5598, 24772, 14097, 13213, 4683, 16703, 15260, 15942, 2747, 27375, 28871, 18004]  
        ]  
k = 15  
expected = 108507  
assert max_domino_score(grid, k) == expected  
  
# test 4  grid = [[38, 7719, 21238, 2437, 8855, 11797, 8365, 32285, 10450, 30612, 5853, 28100, 1142, 281, 20537, 15921, 8945, 26285, 2997, 14680],  
        [20976, 31891, 15046, 25531, 14458, 29272, 28881, 2240, 9725, 32278, 2446, 590, 840, 18587, 16907, 21237, 23611, 12617, 12456, 867],  
        [29533, 6878, 28223, 17887, 31597, 20584, 12212, 31111, 7578, 17066, 7629, 29404, 12279, 13505, 24388, 11649, 12329, 7176, 2331, 19264],  
        [22114, 14136, 26928, 1102, 21652, 8404, 24337, 27856, 5598, 24772, 14097, 13213, 4683, 16703, 15260, 15942, 2747, 27375, 28871, 18004],  
        [16673, 3152, 11819, 23504, 239, 4186, 2804, 28937, 3023, 10335, 20533, 21393, 16020, 11574, 25983, 13961, 624, 7065, 27569, 12830],  
        [24167, 12236, 31249, 23636, 15619, 25727, 12700, 24323, 14922, 22774, 6811, 12062, 3565, 25191, 119, 18747, 26050, 19620, 16334, 5968],  
        [17524, 3577, 15046, 25531, 14458, 29272, 26530, 32134, 32581, 435, 11667, 29160, 26814, 9642, 976, 32527, 3823, 21094, 1768, 9244]  
        ]  
k = 10  
expected = 140636  
assert max_domino_score(grid, k) == expected  
  
# test 5  grid = [[38, 7719, 21238, 2437, 8855, 11797, 8365, 32285, 10450, 30612],  
        [5853, 28100, 1142, 281, 20537, 15921, 8945, 26285, 2997, 14680],  
        [20976, 31891, 21655, 25906, 18457, 1323, 28881, 2240, 9725, 32278],  
        [2446, 590, 840, 18587, 16907, 21237, 23611, 12617, 12456, 867],  
        [29533, 6878, 28223, 17887, 31597, 20584, 12212, 31111, 7578, 17066],  
        [7629, 29404, 12279, 13505, 24388, 11649, 12329, 7176, 2331, 19264],  
        [22114, 14136, 26928, 1102, 21652, 8404, 24337, 27856, 5598, 24772],  
        [14097, 13213, 4683, 16703, 15260, 15942, 2747, 27375, 28871, 18004],  
        [16673, 3152, 11819, 23504, 239, 4186, 2804, 28937, 3023, 10335],  
        [20533, 21393, 16020, 11574, 25983, 13961, 624, 7065, 27569, 12830]  
        ]  
k = 100  
expected = 173402  
assert max_domino_score(grid, k) == expected  
  
# Test 6  
grid = [[1, 2], [-3, 4]]  
k = 1  
expected = -1  
assert max_domino_score(grid, k) == expected  
  
# test 7  
bad_grid_1 = [[1, 2], [3, 'x']]  
k = 1  
expected = -1  
assert max_domino_score(grid, k) == expected
from typing import List  
  
def max_domino_score(grid: List[List[int]], k: int) -> int:  
    from collections import defaultdict  
	 if not isinstance(grid, list) or not all(isinstance(row, list) for row in grid):
        return -1
    if not all(isinstance(val, int) and 0 <= val <= 10**9 for row in grid for val in row):
        return -1
  
    n = len(grid)  
    m = len(grid[0])  
  
    def on(mask, i):  
        return (mask >> i) & 1  
  
    dp = [[[0] * (k + 1) for _ in range(1 << n)] for _ in range(2)]  
  
    for col in range(m):  
        cur = col & 1  
  
        # Try placing vertical dominoes  
        for i in range(n - 1):  
            for msk in range(1 << n):  
                for x in range(k):  
                    if not on(msk, i) and not on(msk, i + 1):  
                        nmsk = msk | (1 << i) | (1 << (i + 1))  
                        dp[cur][nmsk][x + 1] = max(  
                            dp[cur][nmsk][x + 1],  
                            dp[cur][msk][x] + (grid[i][col] ^ grid[i + 1][col])  
                        )  
  
        nxt = cur ^ 1  
        dp[nxt] = [[0] * (k + 1) for _ in range(1 << n)]  
  
        if col == m - 1:  
            break  
  
        tmp = [[0] * (k + 1) for _ in range(1 << n)]  
        for cmsk in range(1 << n):  
            nmsk = ((1 << n) - 1) ^ cmsk  
            for x in range(k + 1):  
                tmp[nmsk][x] = dp[cur][cmsk][x]  
  
        for msk in range((1 << n) - 1, 0, -1):  
            for i in range(n):  
                if on(msk, i):  
                    nmsk = msk ^ (1 << i)  
                    for x in range(k + 1):  
                        tmp[nmsk][x] = max(tmp[nmsk][x], tmp[msk][x])  
  
        # Try placing horizontal dominoes  
        for msk in range(1 << n):  
            tot = 0  
            c = 0  
            for i in range(n):  
                if on(msk, i):  
                    tot += grid[i][col] ^ grid[i][col + 1]  
                    c += 1  
            for x in range(c, k + 1):  
                dp[nxt][msk][x] = max(dp[nxt][msk][x], tmp[msk][x - c] + tot)  
  
    # Find max score in the DP array  
    ans = 0  
    for o in range(2):  
        for msk in range(1 << n):  
            for x in range(k + 1):  
                ans = max(ans, dp[o][msk][x])  
    return ans