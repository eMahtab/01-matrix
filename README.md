# 01-matrix



## Implementation :

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
