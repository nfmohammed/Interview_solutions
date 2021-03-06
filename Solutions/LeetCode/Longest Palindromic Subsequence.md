# Solution 1

### Algorithm

We can represent the Longest Palindromic Subsequence (LPS) as
```
LPS(i,j) =

0                                  if i < j
1                                  if i == j
2 + LPS(i + 1, j - 1)              if A[i] == A[j]
max(LPS(i + 1, j), LPS(i, j - 1))  otherwise
```

We can use Dynamic Programming to fill a 2-D array for every `i`, `j`

### Code

```java
class Solution {
    public int longestPalindromeSubseq(String str) {
        int[][] dp = new int[str.length()][str.length()];
        for (int i = str.length() - 1; i >= 0; i--) {
            dp[i][i] = 1;
            for (int j = i + 1; j < str.length(); j++) {
                if (str.charAt(i) == str.charAt(j)) {
                    dp[i][j] = 2 + dp[i + 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][str.length() - 1];
    }
}
```

### Example

- We will start with a 2-D array, with rows as `i`, and columns as `j`.
- Java automatically initializes the array with all 0s. We will initialize the diagonal with 1s.

|       | 0 | 1 | 2 | 3 |
|-------|---|---|---|---|
| __0__ | 1 | 0 | 0 | 0 |
| __1__ | 0 | 1 | 0 | 0 |
| __2__ | 0 | 0 | 1 | 0 |
| __3__ | 0 | 0 | 0 | 1 |

- Let's take example string `baaa`
- The code above will use a "double for loop" to calculate the top-right values (the 2s and 3s you see below)

|       | 0 | 1 | 2 | 3 |
|-------|---|---|---|---|
| __0__ | 1 | 1 | 2 | 3 |
| __1__ | 0 | 1 | 2 | 3 |
| __2__ | 0 | 0 | 1 | 2 |
| __3__ | 0 | 0 | 0 | 1 |

The top-right element in the table will be our final answer.


### Time/Space Complexity

-  Time Complexity: O(n<sup>2</sup>)
- Space Complexity: O(n<sup>2</sup>)


# Solution 2

The above solution is definitely satisfactory in an interview. This next solution is very tricky, and can either
### Algorithm

- We can reduce the space complexity to `O(n)`, by noticing that `dp[i][j]` only depends on the current row `(dp[i][j-1]`), and the row below it (`dp[i+1][j-1]` and `dp[i+1][j]`). So instead of storing an entire 2-D array of data, a 1-D array of data will be enough. Mentioning this will score bonus points in an interview.
- Coding it is tricky. Now, from `dp[j]`, to get the value:
  - "down", it's `dp[j]`
  - "left", it's `dp[j-1]`
  - "down-left", it's `dp[j-1]` __before__ we overwrite it, so we preserve that value across iterations of the inner for loop. This is done using 2 variables: `pre` and `tmp` (see code below)

### Code

```java
public int longestPalindromeSubseq(String str) {
	int[] dp = new int[str.length()];
	for (int i = str.length() - 1; i >= 0; i--) {
		dp[i] = 1;
		int pre = 0;
		for (int j = i + 1; j < str.length(); j++) {
			int tmp = dp[j];
			if (str.charAt(i) == str.charAt(j)) {
				dp[j] = 2 + pre;
			}
			else {
				dp[j] = Math.max(dp[j], dp[j - 1]);
			}
			pre = tmp;
		}
	}
	return dp[str.length() - 1];
}
```


### Time/Space Complexity

-  Time Complexity: O(n<sup>2</sup>)
- Space Complexity: O(n)
