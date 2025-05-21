---
share_link: https://share.note.sx/y4jpmea6#a+cBb2rzblxsToSlpmij5frt9IO3gcY1JR1IkCU8Fkc
share_updated: 2025-04-07T18:27:25+02:00
---
### statement

```python
def solve(n: int , arr:list[int]) -> int:  
    """  
    Mayada faces a challenge where an array of numbers is provided, you are invited to discover the number of special subsets from this array that exhibit an intriguing imbalance:
        after the selection, the elements divisible by 2 must outnumber those that are not.    
        The answer should be computed modulo 10^9 + 7.  
    Args:      
	    - single integer n (1 <= n <= 10^5) representing the size of the array.      
	    - n integers a1, a2, …, an (1 <= a[i] <= 10^9).  
    Rasies:
	    ValueError with message "Invalid input" If any input does not meet the sepecified constraints

	returns:
        - return a single integer: the count of such subsets modulo 10^9 + 7.  

	Examples:
		>>> solve(2, [2, 4])
		3
		>>> solve(3, [2, 3, 4])
		4
		>>> solve(0, [2])
		"Invalid input"
    """
    
```
---------

### solution
```python 
	# validations  
	if not (1 <= n <= 10**5):  
	    raise ValueError("Invalid input")  
	  
	for x in arr:  
	    if not (1 <= x <= 10**9):  
	        raise ValueError("Invalid input")
	
	if n != len(arr):  
	    raise ValueError("Invalid input")  
	  
	mod = 10**9 + 7  
	  
	even = 0  
	odd = 0  
	for x in arr:  
	    if x % 2 == 0:  
	        even += 1  
	    else:  
	        odd += 1  
	  
	# Precomputation limits.  
	M = n + 5  # n is at most 10^5, so this is safe.  
	  
	# Precompute factorials.  
	fact = [1] * M  
	for i in range(1, M):  
	    fact[i] = fact[i - 1] * i % mod  
	  
	# Precompute modular inverses using Fermat's little theorem.  
	inv = [1] * M  
	inv[0] = inv[1] = 1  
	for i in range(2, M):  
	    inv[i] = (mod - mod // i) * inv[mod % i] % mod  
	  
	# Precompute factorial inverses.  
	FInv = [1] * M  
	for i in range(1, M):  
	    FInv[i] = FInv[i - 1] * inv[i] % mod  
	  
	def ncr(n_val: int, r_val: int) -> int:  
	    """Compute the binomial coefficient nCr modulo mod."""  
	    if r_val > n_val:  
	        return 0  
	    return fact[n_val] * FInv[r_val] % mod * FInv[n_val - r_val] % mod  
	  
	# Precompute prefix sums for binomial coefficients for odd count.  
	# pref[i] will hold the sum of nCr(odd, j) for j = 0 to i.  
	pref = [0] * (odd + 1)  
	for i in range(odd + 1):  
	    pref[i] = ncr(odd, i)  
	    if i:  
	        pref[i] = (pref[i - 1] + pref[i]) % mod  
	  
	def query(l: int, r: int) -> int:  
	    """Return the sum of nCr(odd, j) for j in [l, r]."""  
	    return pref[r] if l == 0 else (pref[r] - pref[l - 1] + mod) % mod  
	  
	# Calculate the result.  
	# For every possible number of even elements chosen (from 1 to even),  
	# add the number of ways to choose less than that many odd elements.  
	res = 0  
	for i in range(1, even + 1):  
	    even_ways = ncr(even, i)  
	    odd_ways = query(0, min(odd, i - 1))  
	    res = (res + even_ways * odd_ways) % mod  
	  
	return res
```
-------
Test Driver
```python
import unittest  
  
class TestSpecialSubsets(unittest.TestCase):  
  
    def test_single_even(self):  
        self.assertEqual(solve(1, [2]), 1)  
  
    def test_single_odd(self):  
        self.assertEqual(solve(1, [3]), 0)  
  
    def test_two_elements_mixed(self):  
        self.assertEqual(solve(2, [2, 3]), 1)  
  
    def test_two_elements_all_even(self):  
        self.assertEqual(solve(2, [2, 4]), 3)  
  
    def test_three_elements_mixed(self):  
        self.assertEqual(solve(3, [2, 3, 4]), 4)  
  
    def test_invalid_n_low(self):  
        with self.assertRaises(ValueError) as context:  
            solve(0, [2])  
        self.assertEqual(str(context.exception), "Invalid input")  
  
    def test_invalid_n_high(self):  
        with self.assertRaises(ValueError) as context:  
            solve(10**5 + 1, [2] * (10**5 + 1))  
        self.assertEqual(str(context.exception), "Invalid input")  
  
    def test_invalid_element_low(self):  
        with self.assertRaises(ValueError) as context:  
            solve(1, [0])  
        self.assertEqual(str(context.exception), "Invalid input")  
  
    def test_invalid_element_high(self):  
        with self.assertRaises(ValueError) as context:  
            solve(2, [2, 10**9 + 1])  
        self.assertEqual(str(context.exception), "Invalid input")  
  
    def test_mismatched_array_length(self):  
        with self.assertRaises(ValueError) as context:  
            solve(3, [2, 4])  
        self.assertEqual(str(context.exception), "Invalid input")  
  
    def test_large(self):  
        self.assertEqual(solve(31, [2] * (31)), 147483633)  
    
    def test_n_40(self):  
	    expected = 832546118  
	    self.assertEqual(solve(40, list(range(1, 41))), expected)
  
  
if __name__ == "__main__":  
    unittest.main()
```
----------
Test Case:
```python
assert solve(2, [2, 4]) == 3
```