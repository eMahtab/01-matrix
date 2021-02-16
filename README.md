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
```
e.g. 
{0, 1, 1, 1},
{1, 1, 1, 1},
{1, 1, 1, 1},
{1, 1, 1, 0}
```
If we just see the four adjacent cells of a cell to determine the result in one pass, it will lead to wrong result for the above matrix. Correct answer will be :
```
{0, 1, 2, 3},
{1, 2, 3, 2},
{2, 3, 2, 1},
{3, 2, 1, 0}
```

## Implementation 1 : BFS Approach
```java
class Solution {
    private class Position {
        int row, column;
        Position(int row, int column) {
            this.row = row;
            this.column = column;
        }
    }
    public int[][] updateMatrix(int[][] matrix) {
        if(matrix == null || matrix.length == 0)
            return matrix;
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        
        Queue<Position> q = new LinkedList<>();
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(matrix[i][j] == 0)
                    q.add(new Position(i, j));
                else
                    matrix[i][j] = -1;
            }
        }
        int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        int distance = 0;
        while(!q.isEmpty()) {
            distance++;
            int size = q.size();
            for(int i = 0; i < size; i++) {
                Position pos = q.remove();
                for(int[] direction : directions) {
                    int r = pos.row + direction[0];
                    int c = pos.column + direction[1];
                    if(r >= 0 && r < matrix.length && c >= 0 && c < matrix[0].length && 
                        matrix[r][c] == -1) {
                        matrix[r][c] = distance;
                        q.add(new Position(r, c));
                    }
                }
            }
        }
       return matrix; 
    }
}
```

## Implementation 1 : Alternate approach
```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        if(matrix == null || matrix.length == 0)
            return matrix;
        
        Queue<int[]> q = new LinkedList<>();
        int rows = matrix.length, cols = matrix[0].length;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(matrix[i][j] == 0)
                    q.add(new int[]{i,j});
                else
                    matrix[i][j] = -1;
            }
        }
        int distance = 0;
        int[][] moves = {{0,1}, {0,-1}, {-1,0}, {1, 0}};
        while(!q.isEmpty()) {
            distance++;
            int size = q.size();
            for(int i = 0; i < size; i++) {
                int[] pos = q.remove();
                for(int[] move : moves) {
                    int x = pos[0] + move[0];
                    int y = pos[1] + move[1];
                    if(x >= 0 && x < rows &&  y >= 0 && y < cols && matrix[x][y] == -1){
                        q.add(new int[]{x,y});
                        matrix[x][y] = distance;
                    }
                }
            }
        }
       return matrix; 
    }
}
```

## Implementation 2 : BFS Approach

```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        if(matrix == null || matrix.length == 0)
            return matrix;
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        Queue<int[]> q = new LinkedList<>();
        Set<String> set = new HashSet<>();
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                if(matrix[i][j] == 0) {
                    q.add(new int[]{i,j});
                    set.add(i+","+j);
                }
            }
        }
        int[][] directions = { {0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        int step = 1;
        while(!q.isEmpty()) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                int[] pos = q.remove();
                for(int[] direction : directions) {
                    int x = pos[0] + direction[0];
                    int y = pos[1] + direction[1];
                    if(x >= 0 && x < rows && y >= 0 && y < cols && matrix[x][y] == 1 && !set.contains(x+","+y)) {
                        matrix[x][y] = step;
                        q.add(new int[]{x,y});
                        set.add(x+","+y);
                    }
                }
            }
            step++;
        }
       return matrix; 
    }
}
```

## Implementation 2 : Dynamic Programming

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
https://leetcode.com/problems/01-matrix/discuss/248525/Java-BFS-solution-with-comments

https://leetcode.com/problems/01-matrix/discuss/101051/Simple-Java-solution-beat-99-(use-DP)
