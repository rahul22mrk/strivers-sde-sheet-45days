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

2.2. Pascal's Triangle I
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

3. Next Permutation
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;

        // To store the index of the first smaller element from right
        int idx=-1;

        //1. Find the first index from the end where nums[i] < nums[i+1]
        //nums[i]<nums[i+1]
        for(int i=n-2;i>=0;i--){
            if(nums[i]<nums[i+1]){
                idx=i;
                break;
            }
        }

        //2. Find the element just greater than nums[ind] from the end
        if(idx!=-1){
            int swapIdx = idx;
            for(int j = n-1; j>idx; j--){
                if(nums[j] > nums[idx]){
                    swapIdx = j;
                    break;
                }
            }
            swap(nums,idx,swapIdx);
        }

        //3. Reverse the right half to get the next smallest permutation
        reverse(nums,idx+1,n-1);
        
    }

    private void reverse(int nums[], int i, int j){
        while(i<j){
            swap(nums,i,j);
            i++;
            j--;
        }
    }

    private void swap(int nums[], int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }


}
Complexity Analysis
Time Complexity: O(N), where N is the size of the input array.
Finding the pivot takes O(N) time. Finding the next greater element also takes O(N) in the worst case. And, reversing the subarray takes O(N). All this adds up to a total of O(N) time complexity.

Space Complexity: O(1), as the modification is done in-place and no extra data structure was used apart from a few variables.

Brute Force App: solution by tuf

class Solution {

    // Function to get the next permutation of given array
    public void nextPermutation(int[] nums) {
        // Get all the Permutations
        List<List<Integer>> ans = getAllPermutations(nums);

        int index = -1; // Current permutation index

        /* Perform a linear search to get the
        permutation of current permutation */
        for(int i = 0; i < ans.size(); i++) {
            if(match(nums, ans.get(i))) {
                index = i;
                break;
            }
        }

        // Next Permutation index
        int nextPermutationIndex = -1;
        if(index == ans.size() - 1) nextPermutationIndex =  0;
        else nextPermutationIndex = index+1;

        // Store the next permutation in-place
        for(int i = 0; i < nums.length; i++) {
            nums[i] = ans.get(nextPermutationIndex).get(i);
        }

        return;
    }

    /* Function to generate all permutations of 
    the given array in sorted order */
    private List<List<Integer>> getAllPermutations(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>(); // To store the permutation

        // Recursive Helper function call 
        helperFunc(0, nums, ans);

        // Sort the result
        Collections.sort(ans, (a, b) -> {
            for(int i = 0; i < a.size(); i++) {
                if(!a.get(i).equals(b.get(i))) {
                    return a.get(i) - b.get(i);
                }
            }
            return 0;
        });

        return ans; // Return the result
    }

    // Helper function to get all the permutations of the given array
    private void helperFunc(int ind, int[] nums, List<List<Integer>> ans) {

        // Base case
        if(ind == nums.length) {
            // Add the permutation to the answer
            List<Integer> temp = new ArrayList<>();
            for(int num : nums) temp.add(num);
            ans.add(temp);
            return;
        }

        // Traverse the array
        for(int i = ind; i < nums.length; i++) {
            swap(nums, ind, i); // Swap-In

            // Recursively call the helper function
            helperFunc(ind + 1, nums, ans);

            swap(nums, ind, i); // Swap-Out
        }

        return;
    }

    // Function to swap two numbers
    private void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

    // Function to match two arrays
    private boolean match(int[] nums, List<Integer> list) {
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] != list.get(i)) return false;
        }
        return true;
    }
}
Time Complexity: O(N × N!)
Space Complexity: O(N × N!)


4. Maximum Subarray (Kadane Algo)

Brute:
 // Function to find maximum sum of subarrays
    public int maxSubArray(int[] nums) {
        
        /* Initialize maximum sum with 
        the smallest possible integer*/
        int maxi = Integer.MIN_VALUE; 

        // Iterate over each starting index of subarrays
        for (int i = 0; i < nums.length; i++) {
            
            /* Iterate over each ending index
            of subarrays starting from i*/
            for (int j = i; j < nums.length; j++) {
                
                // Variable to store the sum of the current subarray
                int sum = 0; 

                // Calculate the sum of subarray nums[i...j]
                for (int k = i; k <= j; k++) {
                    sum += nums[k];
                }

                /* Update maxi with the maximum of its current 
                value and the sum of the current subarray*/
                maxi = Math.max(maxi, sum);
                
            }
        }
        
        // Return the maximum subarray sum found
        return maxi; 
    }
}

Better:
 // Function to find maximum sum of subarrays
    public int maxSubArray(int[] nums) {
        
        /* Initialize maximum sum with
        the smallest possible integer*/
        int maxi = Integer.MIN_VALUE;

        // Iterate over each starting index of subarrays
        for (int i = 0; i < nums.length; i++) {
            
            /* Variable to store the sum
            of the current subarray*/
            int sum = 0; 
            
            /* Iterate over each ending index
            of subarrays starting from i*/
            for (int j = i; j < nums.length; j++) {
                
                /* Add the current element nums[j] to
                the sum i.e. sum of nums[i...j-1]*/
                sum += nums[j];

                /* Update maxi with the maximum of its current
                value and the sum of the current subarray*/
                maxi = Math.max(maxi, sum);
            }
        }

        // Return the maximum subarray sum found
        return maxi;
    }

Optimal Solutions:
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        int currSum = 0;

        for(int i=0;i<nums.length;i++){
            currSum = Math.max(currSum+nums[i], nums[i]);
            maxSum = Math.max(maxSum, currSum);
        }

        return maxSum;
    }
}

// Function to find maximum sum of subarrays
    public int maxSubArray(int[] nums) {
        
        // maximum sum
        long maxi = Long.MIN_VALUE; 
        
        //current sum of subarray 
        long sum = 0; 
        
        // Iterate through the array
        for (int i = 0; i < nums.length; i++) {
            
            // Add current element to the sum
            sum += nums[i]; 
            
            // Update maxi if current sum is greater
            if (sum > maxi) {
                maxi = sum; 
            }
            
            // Reset sum to 0 if it becomes negative
            if (sum < 0) {
                sum = 0; 
            }
        }
        
        // Return the maximum subarray sum found
        return (int) maxi;
    }


Follow up question
Can you print the subarray that has the max sum ?
// Function to find maximum sum of subarrays and print the subarray having maximum sum
    public int maxSubArray(int[] nums) {
        
        // maximum sum
        long maxi = Long.MIN_VALUE; 
        
        // current sum of subarray
        long sum = 0; 
        
        // starting index of current subarray
        int start = 0; 
        
        // indices of the maximum sum subarray
        int ansStart = -1, ansEnd = -1; 
        
        // Iterate through the array
        for (int i = 0; i < nums.length; i++) {
            
            // update starting index if sum is reset
            if (sum == 0) {
                start = i;
            }
            
            // add current element to the sum
            sum += nums[i]; 
            
            /* Update maxi and subarray indices
            if current sum is greater */
            if (sum > maxi) {
                maxi = sum;
                ansStart = start;
                ansEnd = i;
            }
            
            // Reset sum to 0 if it becomes negative
            if (sum < 0) {
                sum = 0;
            }
        }
        
        // Printing the subarray
        System.out.print("The subarray is: [");
        for (int i = ansStart; i <= ansEnd; i++) {
            System.out.print(nums[i] + " ");
        }
        System.out.println("]");

        // Return the maximum subarray sum found
        return (int) maxi;
    }


5. Sort Colors
My Solution:
class Solution {
    public void sortColors(int[] nums) {
        int zeroPtr = 0, twoPtr = nums.length-1;
        int mid = 0;

        while(mid<=twoPtr){
            if(nums[mid] == 1){
                mid++;
            }else if(nums[mid]==0){
                swap(nums,mid,zeroPtr);
                zeroPtr++;
                mid++;
            }else{
                swap(nums,mid,twoPtr);
                twoPtr--;
            }
        }
        
    }

    private void swap(int nums[], int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}

Brute:
class Solution {
    // Function to sort the array
    public void sortZeroOneTwo(int[] nums) {
        // Sort the array using Arrays.sort
        Arrays.sort(nums);
    }
}
Complexity Analysis 
Time Complexity: O(NxlogN), where N is the size of the array. As the optimal sorting take O(N * logN) time.

Space Complexity: O(1) no extra space is used to solve the problem.

Better:
 // Function to sort the array containing only 0s, 1s, and 2s
    public void sortZeroOneTwo(int[] nums) {
        int cnt0 = 0, cnt1 = 0, cnt2 = 0;

        // Counting the number of 0s, 1s, and 2s in the array
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) cnt0++;
            else if (nums[i] == 1) cnt1++;
            else cnt2++;
        }

        /* Placing the elements in the
           original array based on counts */
        // placing 0's
        for (int i = 0; i < cnt0; i++) nums[i] = 0;

        // placing 1's
        for (int i = cnt0; i < cnt0 + cnt1; i++) nums[i] = 1; 
        
        // placing 2's
        for (int i = cnt0 + cnt1; i < nums.length; i++) nums[i] = 2;
    }

    Complexity Analysis 
Time Complexity: O(N)+O(N) = O(2N), where N is the size of the array. There are 2 traversals in the array to count the frequencies then in second iteration we are overwriting.

Space Complexity: O(1) no extra space used.


Optimal:

 // Function to sort the array containing only 0s, 1s, and 2s
    public void sortZeroOneTwo(int[] nums) {
        // 3 pointers: low, mid, high
        int low = 0, mid = 0, high = nums.length - 1;
        
        while (mid <= high) {
            if (nums[mid] == 0) {
                
                /* Swap nums[low] and nums[mid], then 
                move both low and mid pointers forward*/
                int temp = nums[low];
                nums[low] = nums[mid];
                nums[mid] = temp;
                low++;
                mid++;
                
            } else if (nums[mid] == 1) {
                // Move mid pointer forward
                mid++;
            } else {
                /* Swap nums[mid] and nums[high], 
                then move high pointer backward*/
                int temp = nums[mid];
                nums[mid] = nums[high];
                nums[high] = temp;
                high--;
            }
        }
    }
Complexity Analysis 
Time Complexity: O(N), where N is the size of the array, as there is single traversal of the array.

Space Complexity: O(1), no extra space is used.

6. Best Time to Buy and Sell Stock
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int minPrefix[] = new int[n];
        minPrefix[0] = prices[0];

        for(int i=1;i<n;i++){
            minPrefix[i] = Math.min(minPrefix[i-1],prices[i]);
        }

        int maxProfit = 0;
        for(int i=n-1;i>=0;i--){
            maxProfit = Math.max(maxProfit, prices[i]-minPrefix[i]);
        }

        return maxProfit;
        
    }
}

class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for(int i=0;i<prices.length;i++){
            minPrice = Math.min(prices[i], minPrice);
            maxProfit = Math.max(maxProfit, prices[i]-minPrice);
        }
        
        return maxProfit;
        
    }
}

Complexity Analysis:
Time Complexity:O(N), As the whole array is being traversed only once.

Space Complexity:O(1), As no extra space is being used.

7. Rotate matrix by 90 degrees

class Solution {
    public void rotateMatrix(int[][] matrix) {
        int n = matrix.length;
        int temp[][] = new int[n][n];

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                temp[j][n-i-1] = matrix[i][j];
            }
        }

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                matrix[i][j]  = temp[i][j];
            }
        }
    }
}
Complexity Analysis 
Time Complexity: O(N2) +O(N2), to linearly iterate and put elements into dummy matrix and another O(N2) to copy elements of dummy matrix back to original matrix.

Space Complexity: O(N2), to store the elements in the dummy matrix.

Optimize:

//Rotate the given matrix by 90 degrees clockwise.
    
    public void rotateMatrix(int[][] matrix) {
        int n = matrix.length;
        
        // Transpose the matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                
                // Swap elements across the diagonal
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
                
            }
        }
        
        // Reverse each row of the matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n / 2; j++) {
                
                // Swap elements symmetrically
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - 1 - j];
                matrix[i][n - 1 - j] = temp;
                
            }
        }
    }

    Complexity Analysis 
Time Complexity: O(N2) +O(N2), to linearly iterate and find transpose of the matrix and another O(N2) to find the reverse of each row.

Space Complexity: O(1), as no extra space is being used.

8. 
