# 01-matrix
Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.
```
Example 1:

Input:
[[0,0,0],
 [0,1,0],
 [0,0,0]]

Output:
[[0,0,0],
 [0,1,0],
 [0,0,0]]
 
Example 2:

Input:
[[0,0,0],
 [0,1,0],
 [1,1,1]]

Output:
[[0,0,0],
 [0,1,0],
 [1,2,1]]
 
```
**Note:**

1. The number of elements of the given matrix will not exceed 10,000.
2. There are at least one 0 in the given matrix.
3. The cells are adjacent in only four directions: up, down, left and right.

## Approach :
We can't solve this problem in one pass through the matrix, as it will lead to wrong result.
e.g. 
{0, 1, 1, 1},
{1, 1, 1, 1},
{1, 1, 1, 1},
{1, 1, 1, 0}

If we just see the four adjacent cells of a cell to determine the result in one pass, it will lead to wrong result for the above matrix. Correct answer will be :

{0, 1, 2, 3},
{1, 2, 3, 2},
{2, 3, 2, 1},
{3, 2, 1, 0}

## Implementation : Dynamic Programming

```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
       if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
			     return matrix;

       int distanceUpperBound = matrix.length + matrix[0].length;

       for (int i = 0; i < matrix.length; i++) {
           for (int j = 0; j < matrix[0].length; j++) {
	         if (matrix[i][j] != 0) {
			int topCell = (i > 0) ? matrix[i - 1][j] : distanceUpperBound;
			int leftCell = (j > 0) ? matrix[i][j - 1] : distanceUpperBound;
			matrix[i][j] = Math.min(topCell, leftCell) + 1;
		  }
	    }
       }

       for (int i = matrix.length - 1; i >= 0; i--) {
	   for (int j = matrix[0].length - 1; j >= 0; j--) {
	          if (matrix[i][j] != 0) {
			 int bottomCell = (i < matrix.length - 1) ? matrix[i + 1][j] : distanceUpperBound;
			 int rightCell = (j < matrix[0].length - 1) ? matrix[i][j + 1] : distanceUpperBound;
			 matrix[i][j] = Math.min(Math.min(bottomCell, rightCell) + 1, matrix[i][j]);
		   }
	    }
        }
      
      return matrix;
   }
}
```


# References :
https://leetcode.com/problems/01-matrix/discuss/101051/Simple-Java-solution-beat-99-(use-DP)
