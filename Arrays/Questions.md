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

8. Merge Overlapping Subintervals
Brute:
public List<List<Integer>> mergeOverlap(List<List<Integer>> arr) {
        // Get the initial number of intervals
        int n = (int) arr.size();
        // If there are 0 or 1 intervals, no merging is needed
        if (n <= 1) return arr;

        // Flag to track whether any merge happened in the last pass
        boolean mergedSomething = true;
        // Keep repeating passes until a full pass finds no merge
        while (mergedSomething) {
            // Reset the flag at the start of each pass
            mergedSomething = false;
            // Iterate over all interval indices i
            for (int i = 0; i < arr.size() && !mergedSomething; ++i) {
                // For each i, check all j > i to find an overlapping pair
                for (int j = i + 1; j < arr.size(); ++j) {
                    // Extract start and end of interval i
                    int a1 = arr.get(i).get(0);
                    // Extract start and end of interval i (end)
                    int b1 = arr.get(i).get(1);
                    // Extract start of interval j
                    int a2 = arr.get(j).get(0);
                    // Extract end of interval j
                    int b2 = arr.get(j).get(1);

                    // Check if intervals i and j overlap (not (i ends before j starts or j ends before i starts))
                    if (!(b1 < a2 || b2 < a1)) {
                        // Compute merged start as the minimum of the two starts
                        int ns = Math.min(a1, a2);
                        // Compute merged end as the maximum of the two ends
                        int ne = Math.max(b1, b2);
                        // Replace interval i with the merged interval
                        arr.get(i).set(0, ns);
                        arr.get(i).set(1, ne);
                        // Remove interval j since it is now merged into i
                        arr.remove(j);
                        // Mark that a merge happened to trigger another pass
                        mergedSomething = true;
                        // Break to restart scanning from the beginning in the next pass
                        break;
                    }
                }
            }
        }

        // Return the fully merged list of intervals
        return arr;
    }
    Complexity Analysis:
Time Complexity: O(N2), where N is the size of the array.
In the worst case, each merge requires scanning the entire list again. For N intervals, this can lead to repeated scans after each merge, resulting in O(N2) time complexity.

Space Complexity: O(1), as only a few extra variables and a flag are used.

optimize:

public List<List<Integer>> mergeOverlap(List<List<Integer>> intervals) {

        // If the list is empty, return an empty result
        if (intervals.size() == 0) return new ArrayList<>();

        // Step 1: Sort intervals based on start time
        intervals.sort((a, b) -> Integer.compare(a.get(0), b.get(0)));

        // Step 2: List to store merged intervals
        List<List<Integer>> result = new ArrayList<>();

        // Add the first interval
        result.add(new ArrayList<>(intervals.get(0)));

        // Iterate through remaining intervals
        for (int i = 1; i < intervals.size(); i++) {

            List<Integer> last = result.get(result.size() - 1);  // Last merged interval
            List<Integer> curr = intervals.get(i);               // Current interval

            // Overlap condition
            if (curr.get(0) <= last.get(1)) {
                // Merge
                last.set(1, Math.max(last.get(1), curr.get(1)));
            } else {
                // No overlap → add new interval
                result.add(new ArrayList<>(curr));
            }
        }

        return result;
    }


class Solution {
    public int[][] merge(int[][] intervals) {

        if (intervals.length == 0) {
            return new int[0][0];
        }

        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> ans = new ArrayList<>();

        ans.add(intervals[0]);

        for (int i = 1; i < intervals.length; i++) {

            int[] last = ans.get(ans.size() - 1);
            int[] curr = intervals[i];

            if (last[1] >= curr[0]) {
                last[1] = Math.max(last[1], curr[1]);
            } else {
                ans.add(curr);
            }
        }

        return ans.toArray(new int[ans.size()][]);
    }
}
    Complexity Analysis
Time Complexity: O(n log n) for sorting the intervals, plus O(n) for iterating over them. Thus, the total time complexity is O(n log n).
Space Complexity: O(n) for storing the merged intervals.


9. Merge two sorted arrays without extra space

class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        
        int arr[] = new int[m+n];

        int  i=0, j=0,k=0;

        while(i<m && j<n){
            if(nums1[i] <= nums2[j]){
                arr[k] = nums1[i];
                i++;
            }else{
                arr[k] = nums2[j];
                j++;
            }
            k++;
        }

        while(i<m){
            arr[k++] = nums1[i++];
        }

        while(j<n){
            arr[k++] = nums2[j++];
        }

        for(i=0;i<arr.length;i++){
            nums1[i]= arr[i];
        }

    }
}

Brute:
 // Function to merge two sorted arrays nums1 and nums2
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        
        // Declare a 3rd array and 2 pointers:
        int[] merged = new int[m + n];
        int left = 0;
        int right = 0;
        int index = 0;

        /* Insert elements from nums1 and nums2 into
        merged array using left and right pointers */
        while (left < m && right < n) {
            if (nums1[left] <= nums2[right]) {
                merged[index++] = nums1[left++];
            } else {
                merged[index++] = nums2[right++];
            }
        }

        // If right pointer reaches the end of nums2:
        while (left < m) {
            merged[index++] = nums1[left++];
        }

        // If left pointer reaches the end of nums1:
        while (right < n) {
            merged[index++] = nums2[right++];
        }

        /* Copy elements from merged array
        array back to nums1 */
        for (int i = 0; i < m + n; i++) {
            nums1[i] = merged[i];
        }
    }

    Complexity Analysis
Time Complexity: O(N+M) + O(N+m), where N and M are the sizes of the given arrays. O(N+M) is for copying the elements from nums1[] and nums2[] to the third array. And another O(N+M) is for filling back nums1[].

Space Complexity: O(N+M) for using an extra array of size N+M.


optimal-1
 // Function to merge two sorted arrays nums1 and nums2
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Pointer for nums1 (end of valid elements)
        int left = m - 1;
        
        // Pointer for nums2 (beginning of valid elements)
        int right = 0;
        
        /* Swap the elements until nums1[left]
        is smaller than nums2[right]*/
        while (left >= 0 && right < n) {
            if (nums1[left] > nums2[right]) {
                int temp = nums1[left];
                nums1[left] = nums2[right];
                nums2[right] = temp;
                left--;
                right++;
            } 
            //break out of loop if nums1[left] > nums2[right] 
            else break;
        }
        
        // Sort nums1 from index 0 to m-1
        Arrays.sort(nums1, 0, m);
        
        // Sort nums2 from start to end
        Arrays.sort(nums2);
        
        // Put the elements of nums2 in nums1
        for (int i = m; i < m + n; i++) {
            nums1[i] = nums2[i - m];
        }
    }

   Complexity Analysis 
Time Complexity: O(min(N, M)) + O(NxlogN) + O(MxlogM) + O(N), where N and M are the sizes of the given arrays. O(min(N, M)) is for swapping the array elements. And O(NxlogN) and O(MxlogM) are for sorting the two arrays. Another O(N) adds up to copy the elements from the 2nd list to the 1st list.

Space Complexity: O(1) as no additional space is used apart from the input array.


optimnal-2
 // Function to merge two sorted arrays nums1 and nums2
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        
        int len = n + m;
        int gap = (len / 2) + (len % 2);

        while (gap > 0) {
            int left = 0;
            int right = left + gap;
            while (right < len) {
                
                // When left in nums1[] and right in nums2[]
                if (left < m && right >= m) {
                    swapIfGreater(nums1, nums2, left, right - m);
                }
                // When both pointers in nums2[]
                else if (left >= m) {
                    swapIfGreater(nums2, nums2, left - m, right - m);
                }
                // When both pointers in nums1[]
                else {
                    swapIfGreater(nums1, nums1, left, right);
                }
                // Increment the pointers by 1 each
                left++;
                right++;
            }
            // If gap is equal, break out of the loop
            if (gap == 1)
                break;
            gap = (gap / 2) + (gap % 2);
        }

        // Copy elements of nums2 into nums1
        for (int i = m; i < m + n; i++) {
            nums1[i] = nums2[i - m];
        }
    }

    // Utility function to swap elements if needed
    private void swapIfGreater(int[] arr1, int[] arr2, int idx1, int idx2) {
        if (arr1[idx1] > arr2[idx2]) {
            
            int temp = arr1[idx1];
            arr1[idx1] = arr2[idx2];
            arr2[idx2] = temp;
        }
    }
    Complexity Analysis 
Time Complexity: O((N+M)xlog(N+M)), where N and M are the sizes of the given arrays. The gap is ranging from N+M to 1 and every time the gap gets divided by 2. So, the time complexity of the outer loop will be O(log(N+M)). Now, for each value of the gap, the inner loop can at most run for (N+M) times. So, the time complexity of the inner loop will be O(N+M). So, the overall time complexity will be O((N+M)xlog(N+M)).

Space Complexity: O(1) as no additional space is used apart from the input array.

Optimal-3

 // Function to merge two sorted arrays nums1 and nums2
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1, j = n - 1;
        int ind = m + n - 1;

        // Until all the elements from nums2 are placed
        while (j >= 0) {
            // If nums1[i] >= nums2[j]
            if (i >= 0 && nums1[i] >= nums2[j]) {
                // Place the element
                nums1[ind] = nums1[i];

                // Move both indices back by one place
                ind--;
                i--;
            } 
            // Otherwise
            else {
                // Place the element
                nums1[ind] = nums2[j];

                // Move both indices back by one place
                ind--;
                j--;
            }
        }
    }
Complexity Analysis
Time Complexity: O(N + M), where N and M are the sizes of the given arrays.
Because a single linear traversal is performed from the end of both arrays, with each element being processed at most once.

Space Complexity: O(1), as only couple of variables are used.
    

10. Find the Duplicate Number
class Solution {
    public int findDuplicate(int[] nums) {
        int freq[] = new int[nums.length+1];

        for(int i=0;i<nums.length;i++){
            freq[nums[i]]++;

            if(freq[nums[i]]>1) return nums[i];
        }
        
        return -1;
    }
}

class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[0];

        while(true){
            slow = nums[slow];
            fast = nums[nums[fast]];

            if(slow == fast) break;
        }

        slow = nums[0];

        while(slow!=fast){
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
}

Time and Space Complexity
Time Complexity: O(n), the cycle is detected in linear time and entry point is also found in linear time.
Space Complexity: O(1), we use constant additional space to find duplicate element.

11. Find the repeating and missing number

class Solution {
    public int[] findMissingRepeatingNumbers(int[] nums) {
        int freq[] = new int[nums.length+1];

        for(int i=0;i<nums.length;i++){
            freq[nums[i]]++;
        }

        int missedNum = -1, repeatedNum = -1;
        for(int i=1;i<=nums.length;i++){

            if(freq[i]==0) missedNum = i;

            if(freq[i]>1) repeatedNum = i;

             if (repeatedNum != -1 && missedNum != -1) {
                break;
            }
        }

        return new int[]{repeatedNum,missedNum};

    }
}


class Solution {
    public int[] findMissingRepeatingNumbers(int[] nums) {

        int missing = -1;
        int repeating = -1;

        for (int i = 0; i < nums.length; i++) {

            // Map value x to index (x - 1)
            int idx = Math.abs(nums[i]) - 1;

            if (nums[idx] > 0) {
                nums[idx] = -nums[idx]; // mark as visited
            } else {
                repeating = idx + 1; // already visited => duplicate
            }
        }

        for (int i = 0; i < nums.length; i++) {

            // positive index was never visited
            if (nums[i] > 0) {
                missing = i + 1;
                break;
            }
        }

        return new int[]{repeating, missing};
    }
}


 // Function to find repeating and missing numbers
    public int[] findMissingRepeatingNumbers(int[] nums) {
        int n = nums.length; // Size of the array
        int repeating = -1, missing = -1;

        // Find the repeating and missing number:
        for (int i = 1; i <= n; i++) {
            // Count the occurrences:
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                if (nums[j] == i) cnt++;
            }

            // Check if i is repeating or missing
            if (cnt == 2) repeating = i;
            else if (cnt == 0) missing = i;

            /* If both repeating and missing
            are found, break out of loop*/
            if (repeating != -1 && missing != -1)
                break;
        }

        // Return {repeating, missing}
        return new int[] {repeating, missing};
    }

    Complexity Analysis 
Time Complexity: O(N2), where N is the size of the array. Since we are using nested loops to count occurrences of every element between 1 to N.

Space Complexity: O(1) as no extra space is used.



12. Count Inversions

class Solution {
    public long numberOfInversions(int[] nums) {
        int ans = 0 ;

        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                if(nums[i]>nums[j]){
                    ans++;
                }
            }
        }
        
        return ans;

    }
}
class Solution {
    public long numberOfInversions(int[] nums) {
        return mergeSort(nums,0,nums.length-1);
    }

    private long mergeSort(int nums[], int low, int high ){
        long cnt = 0;

        if(low < high){
            int mid = low + (high-low)/2;

            cnt += mergeSort(nums, low, mid);

            cnt += mergeSort(nums, mid+1, high);
            cnt += merge(nums, low, mid, high);
        }
        
        return cnt;
    }

    private long merge(int nums[], int low, int mid, int high){
        int[] temp = new int[high-low+1];

        int left = low, right = mid+1, idx = 0;

        long cnt = 0;
        while(left <= mid && right <= high){
            if(nums[left] <= nums[right]){
                temp[idx++] = nums[left++];
            }else{
                temp[idx++] = nums[right++];
                cnt += (mid - left + 1);
            }
        } 

        while(left <= mid){
            temp[idx++] = nums[left++];
        }

        while(right <= high){
            temp[idx++] = nums[right++];
        }

        idx = 0;
        for(int i=low; i<=high;i++){
            nums[i] = temp[idx++];
        }
        return cnt;
    }
}

13. Search in a 2D matrix

 //Function to search for a given target in matrix
    public boolean searchMatrix(int[][] mat, int target) {
        // Check if the matrix is empty
        if (mat.length == 0 || mat[0].length == 0) {
            return false;
        }
        
        int n = mat.length;  
        int m = mat[0].length; 
        
        // Traverse the matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (mat[i][j] == target) {
                    // Return true if target is found
                    return true; 
                }
            }
        }
        // Return false if target is not found
        return false; 
    }
Complexity Analysis: 
Time Complexity: O(N X M), where N is the number of rows in the matrix, M is the number of columns in each row. As, nested loops are being used to traverse the matrix.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).

    class Solution {
    public boolean searchMatrix(int[][] mat, int target) {
        
        for(int i=0; i<mat.length; i++){
            while(mat[i][0]<=target && mat[i][mat[i].length-1]>=target){
               return binarySearch(mat[i], target);
            }
        }

        return false;

    }

    private boolean binarySearch(int arr[], int target){
        int low = 0, high = arr.length-1;

        while(low<=high){
            int mid = low + (high - low)/2;

            if(arr[mid] == target){
                return true;
            }else if(arr[mid]<target){
                low = mid+1;
            }else{
                high = mid -1;
            }
        }
        return false;
    }
}
Complexity Analysis: 
Time Complexity: O(N + logM), where N is given row number, M is given column number. The rows are traversed in O(N) time complexity. Binary search is applied only once for a particular row, resulting in a time complexity of O(N + logM) instead of O(N*logM).

Space Complexity: As no additional space is used, so the Space Complexity is O(1).

 // Function to search for a given target in matrix
    public boolean searchMatrix(int[][] mat, int target) {
        int n = mat.length;
        int m = mat[0].length;

        int low = 0, high = n * m - 1;
        
        // Perform binary search
        while (low <= high) {
            int mid = (low + high) / 2;
            
            // Calculate the row and column
            int row = mid / m;
            int col = mid % m;
            
            // If target is found return true
            if (mat[row][col] == target) return true;
            else if (mat[row][col] < target) low = mid + 1;
            else high = mid - 1;
        }
        
        // Return false if target is not found
        return false; 
    }
    
Time Complexity: O(log(N*M)), where N is the number of rows in the matrix, M is the number of columns in each row. As, binary search is being applied on the 1-D array of size N*M.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).


14. Pow(x, n)

15. Majority Element-I

16. Majority Element-II

17. Grid unique paths

18. Reverse Pairs

19. Two Sum

20. 4 Sum

21. Longest Consecutive Sequence in an Array

22. Largest Subarray with K sum

23. Count subarrays with given xor K

24. Longest Substring Without Repeating Characters
