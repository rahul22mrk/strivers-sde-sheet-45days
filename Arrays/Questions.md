1. 73. Set Matrix Zeroes

Intuition
We can use extra arrays (or markers) to remember which rows and columns need to be set to zero. This avoids overwriting values prematurely, and we only modify the matrix after scanning the entire matrix.

Approach
Create two arrays: one to mark the rows and another to mark the columns that need to be zeroed.
Traverse the matrix to identify the rows and columns that contain 0s.
After marking, traverse the matrix again, and for each marked row and column, set all elements in those rows and columns to 0.

class Solution {
     // Method to set entire rows and columns to 0 if a 0 is found in the matrix
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;  // Number of rows
        int n = matrix[0].length;  // Number of columns
        
        boolean[] rows = new boolean[m];  // To mark rows to be set to 0
        boolean[] cols = new boolean[n];  // To mark columns to be set to 0
        
        // Step 1: Identify rows and columns to be set to 0
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }
        
        // Step 2: Set the corresponding rows and columns to 0
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rows[i] || cols[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}

Intuition
To optimize space complexity, we can use the first row and the first column of the matrix itself to store the markers, instead of using extra arrays. This reduces the extra space from O(m + n) to O(1). However, we need to be cautious about using the first row and column for marking, as they may also be part of the matrix.

Approach
Use the first row and the first column to mark which rows and columns need to be set to zero.
If the first row or the first column needs to be zeroed, use additional variables to remember this.
Traverse the matrix and mark the first row and first column accordingly.
Finally, use the first row and column markers to set the respective rows and columns to zero.



class Solution {
    // Method to set entire rows and columns to 0 if a 0 is found in the matrix
    public void setZeroes(int[][] matrix) {
       int m = matrix.length;  // Number of rows
        int n = matrix[0].length;  // Number of columns
        
        boolean firstRowZero = false;  // Flag to check if the first row needs to be zeroed
        boolean firstColZero = false;  // Flag to check if the first column needs to be zeroed
        
        // Step 1: Check if the first row and first column need to be zeroed
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
                break;
            }
        }
        
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
                break;
            }
        }
        
        // Step 2: Use first row and first column to mark zero rows and columns
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;  // Mark the first column of the current row
                    matrix[0][j] = 0;  // Mark the first row of the current column
                }
            }
        }
        
        // Step 3: Set matrix elements to zero based on markers in the first row and column
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        
        // Step 4: Zero the first row if needed
        if (firstRowZero) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
        
        // Step 5: Zero the first column if needed
        if (firstColZero) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}


Complexity Analysis
Time Complexity: O(m * n), because we still need to traverse every element in the matrix.
Space Complexity: O(1), since we do not use any extra space aside from the matrix itself.


2.  118. Pascal's Triangle
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> prev = new ArrayList<>();
        prev.add(1);
        ans.add(prev);
        if(numRows == 1){
            
            return ans;
        }

        for(int k=2;k<=numRows;k++){


            List<Integer> curr = new ArrayList<>();
            curr.add(1);
            for(int i=1;i<prev.size();i++){
                curr.add(prev.get(i) + prev.get(i-1));
            }

            curr.add(1);
            ans.add(curr);
            prev = curr;

        }


        return ans;
        
    }
}

3. Pascal's Triangle I
class Solution {
    public int pascalTriangleI(int r, int c) {
        r = r-1;
        c = c-1;

        c= Math.min(r-c, c);

        int mul = 1;

        for(int i =1; i<=c;i++ ){
            mul *= (r-i+1);
            mul /= i;
        }
    

        return mul;

    }
}

class Solution {
    // Function to print the element in rth row and cth column 
    public static int pascalTriangleI(int r, int c) {
        return nCr(r-1, c-1);
    }
    
    // Function to calculate nCr
    private static int nCr(int n, int r)  {
        // Choose the smaller value for lesser iterations
        if(r > n-r) r = n-r;
        
        // base case
        if(r == 1) return n;
        
        int res = 1; // to store the result 
        
        // Calculate nCr using iterative method avoiding overflow 
        for (int i = 0; i < r; i++) {
            res = res * (n - i);
            res = res / (i + 1);
        }
        
        return res; // return the result 
    }
};

Complexity Analysis:
Time Complexity: O(C), where C is the column number. This is because the loop in the nCr function runs for a total of C times, where C can be as large as N/2.

Space Complexity: O(1), as no extra space is used.

4. 
