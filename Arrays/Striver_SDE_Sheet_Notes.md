# 📒 Striver SDE Sheet - Complete Notes

> **Interactive Revision Guide** — Click ▶ to expand solutions. All solutions included (Brute → Better → Optimal). Organized by Pattern & Sequence.

---

## 📑 Quick Navigation

- [🎯 Pattern-Based Index](#-pattern-based-index)
- [🔢 Sequential Index](#-sequential-index)
- [📝 Detailed Solutions](#-detailed-solutions)

---

## 🎯 Pattern-Based Index

| # | Pattern | Problems |
|---|---------|----------|
| A | **Matrix Manipulation** | Q1 Set Matrix Zeroes, Q7 Rotate Matrix, Q13 Search in 2D Matrix |
| B | **Sorting (In-place)** | Q5 Sort Colors (Dutch National Flag), Q9 Merge Sorted Arrays |
| C | **Subarray / Substring** | Q4 Maximum Subarray (Kadane's), Q22 Largest Subarray with K Sum, Q23 Count Subarrays with XOR K, Q24 Longest Substring Without Repeating |
| D | **Merging / Overlapping** | Q8 Merge Overlapping Intervals |
| E | **Inversions / Pairs** | Q12 Count Inversions, Q18 Reverse Pairs |
| F | **Majority / Voting** | Q15 Majority Element I, Q16 Majority Element II |
| G | **Math / Combinatorics** | Q2 Pascal's Triangle, Q17 Grid Unique Paths, Q14 Pow(x,n) |
| H | **Permutation / Next** | Q3 Next Permutation |
| I | **Hashing / Two Pointer** | Q19 Two Sum, Q20 4 Sum, Q21 Longest Consecutive Sequence |
| J | **Cycle / Duplicate Detection** | Q10 Find Duplicate Number, Q11 Find Repeating & Missing |
| K | **Stock / Greedy** | Q6 Best Time to Buy and Sell Stock |

---

## 🔢 Sequential Index

| # | Problem | Pattern | Best TC | Best SC |
|---|---------|---------|---------|---------|
| 1 | Set Matrix Zeroes | Matrix Manipulation | O(m×n) | O(1) |
| 2 | Pascal's Triangle | Math/Combinatorics | O(n²) | O(n²) |
| 3 | Next Permutation | Permutation | O(n) | O(1) |
| 4 | Maximum Subarray (Kadane's) | Subarray | O(n) | O(1) |
| 5 | Sort Colors | Sorting (In-place) | O(n) | O(1) |
| 6 | Best Time to Buy and Sell Stock | Stock/Greedy | O(n) | O(1) |
| 7 | Rotate Matrix by 90° | Matrix Manipulation | O(n²) | O(1) |
| 8 | Merge Overlapping Intervals | Merging/Overlapping | O(n log n) | O(n) |
| 9 | Merge Sorted Arrays (No Extra Space) | Sorting (In-place) | O(n+m) | O(1) |
| 10 | Find the Duplicate Number | Cycle/Duplicate | O(n) | O(1) |
| 11 | Find Repeating & Missing Number | Cycle/Duplicate | O(n) | O(1) |
| 12 | Count Inversions | Inversions/Pairs | O(n log n) | O(n) |
| 13 | Search in 2D Matrix | Matrix Manipulation | O(log(n×m)) | O(1) |
| 14 | Pow(x, n) | Math/Combinatorics | O(log n) | O(log n) |
| 15 | Majority Element I | Majority/Voting | O(n) | O(1) |
| 16 | Majority Element II | Majority/Voting | O(n) | O(1) |
| 17 | Grid Unique Paths | Math/Combinatorics | — | — |
| 18 | Reverse Pairs | Inversions/Pairs | — | — |
| 19 | Two Sum | Hashing/Two Pointer | — | — |
| 20 | 4 Sum | Hashing/Two Pointer | — | — |
| 21 | Longest Consecutive Sequence | Hashing/Two Pointer | — | — |
| 22 | Largest Subarray with K Sum | Subarray | — | — |
| 23 | Count Subarrays with XOR K | Subarray | — | — |
| 24 | Longest Substring Without Repeating | Subarray/Sliding Window | — | — |

---

## 📝 Detailed Solutions

---

### Q1. Set Matrix Zeroes
**Pattern:** Matrix Manipulation | **LeetCode:** 73

<details>
<summary>📖 Intuition</summary>

We can use extra arrays (or markers) to remember which rows and columns need to be set to zero. This avoids overwriting values prematurely, and we only modify the matrix after scanning the entire matrix.

**Approach:**
- Create two arrays: one to mark the rows and another to mark the columns that need to be zeroed.
- Traverse the matrix to identify the rows and columns that contain 0s.
- After marking, traverse the matrix again, and for each marked row and column, set all elements in those rows and columns to 0.
</details>

<details>
<summary>✅ Better Solution — O(m+n) Space</summary>

```java
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
```

**Time Complexity:** O(m * n) | **Space Complexity:** O(m + n)
</details>

<details>
<summary>🚀 Optimal Solution — O(1) Space</summary>

**Intuition:** To optimize space complexity, we can use the first row and the first column of the matrix itself to store the markers, instead of using extra arrays. This reduces the extra space from O(m + n) to O(1). However, we need to be cautious about using the first row and column for marking, as they may also be part of the matrix.

**Approach:**
- Use the first row and the first column to mark which rows and columns need to be set to zero.
- If the first row or the first column needs to be zeroed, use additional variables to remember this.
- Traverse the matrix and mark the first row and first column accordingly.
- Finally, use the first row and column markers to set the respective rows and columns to zero.

```java
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
```

**Time Complexity:** O(m * n) | **Space Complexity:** O(1)
</details>

---

### Q2. Pascal's Triangle
**Pattern:** Math/Combinatorics | **LeetCode:** 118, 119

<details>
<summary>✅ 2.1 Pascal's Triangle — Generate All Rows</summary>

```java
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
```
</details>

<details>
<summary>✅ 2.2 Pascal's Triangle I — Specific Element (r, c) — My Solution</summary>

```java
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
```
</details>

<details>
<summary>✅ 2.2 Pascal's Triangle I — Reference Solution (nCr)</summary>

```java
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
```

**Time Complexity:** O(C), where C is the column number. The loop runs for a total of C times, where C can be as large as N/2.

**Space Complexity:** O(1), as no extra space is used.
</details>

---

### Q3. Next Permutation
**Pattern:** Permutation | **LeetCode:** 31

<details>
<summary>⚡ Brute Force — Generate All Permutations</summary>

```java
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
        List<List<Integer>> ans = new ArrayList<>();

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

        return ans;
    }

    // Helper function to get all the permutations of the given array
    private void helperFunc(int ind, int[] nums, List<List<Integer>> ans) {
        if(ind == nums.length) {
            List<Integer> temp = new ArrayList<>();
            for(int num : nums) temp.add(num);
            ans.add(temp);
            return;
        }

        for(int i = ind; i < nums.length; i++) {
            swap(nums, ind, i); // Swap-In
            helperFunc(ind + 1, nums, ans);
            swap(nums, ind, i); // Swap-Out
        }
    }

    private void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

    private boolean match(int[] nums, List<Integer> list) {
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] != list.get(i)) return false;
        }
        return true;
    }
}
```

**Time Complexity:** O(N × N!) | **Space Complexity:** O(N × N!)
</details>

<details>
<summary>🚀 Optimal Solution — O(n) O(1)</summary>

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;

        // To store the index of the first smaller element from right
        int idx=-1;

        //1. Find the first index from the end where nums[i] < nums[i+1]
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
```

**Time Complexity:** O(N) — Finding pivot O(N) + Finding next greater O(N) + Reversing O(N)

**Space Complexity:** O(1), as the modification is done in-place and no extra data structure was used apart from a few variables.
</details>

---

### Q4. Maximum Subarray (Kadane's Algorithm)
**Pattern:** Subarray | **LeetCode:** 53

<details>
<summary>⚡ Brute Force — O(n³)</summary>

```java
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
    
    return maxi; 
}
```
</details>

<details>
<summary>🔧 Better — O(n²)</summary>

```java
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

    return maxi;
}
```
</details>

<details>
<summary>🚀 Optimal-1 — Kadane's (My Solution) O(n)</summary>

```java
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
```
</details>

<details>
<summary>🚀 Optimal-2 — Kadane's (Reference) O(n)</summary>

```java
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
```
</details>

<details>
<summary>🎯 Follow-up: Print the Subarray with Max Sum</summary>

```java
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
```
</details>

---

### Q5. Sort Colors (Dutch National Flag)
**Pattern:** Sorting (In-place) | **LeetCode:** 75

<details>
<summary>⚡ Brute Force — Arrays.sort</summary>

```java
class Solution {
    // Function to sort the array
    public void sortZeroOneTwo(int[] nums) {
        // Sort the array using Arrays.sort
        Arrays.sort(nums);
    }
}
```

**Time Complexity:** O(N×logN) | **Space Complexity:** O(1)
</details>

<details>
<summary>🔧 Better — Counting O(2N)</summary>

```java
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
```

**Time Complexity:** O(N)+O(N) = O(2N) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal — Dutch National Flag (My Solution) O(n)</summary>

```java
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
```
</details>

<details>
<summary>🚀 Optimal — Dutch National Flag (Reference) O(n)</summary>

```java
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
```

**Time Complexity:** O(N), single traversal | **Space Complexity:** O(1)
</details>

---

### Q6. Best Time to Buy and Sell Stock
**Pattern:** Stock/Greedy | **LeetCode:** 121

<details>
<summary>✅ My Solution — Min Prefix Array</summary>

```java
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
```
</details>

<details>
<summary>🚀 Optimal — Single Pass O(n) O(1)</summary>

```java
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
```

**Time Complexity:** O(N) | **Space Complexity:** O(1)
</details>

---

### Q7. Rotate Matrix by 90°
**Pattern:** Matrix Manipulation | **LeetCode:** 48

<details>
<summary>🔧 Better — Using Extra Space</summary>

```java
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
```

**Time Complexity:** O(N²) + O(N²) | **Space Complexity:** O(N²)
</details>

<details>
<summary>🚀 Optimal — Transpose + Reverse O(1) Space</summary>

```java
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
```

**Time Complexity:** O(N²) + O(N²) | **Space Complexity:** O(1)
</details>

---

### Q8. Merge Overlapping Subintervals
**Pattern:** Merging/Overlapping | **LeetCode:** 56

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
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
                int a1 = arr.get(i).get(0);
                int b1 = arr.get(i).get(1);
                int a2 = arr.get(j).get(0);
                int b2 = arr.get(j).get(1);

                // Check if intervals i and j overlap
                if (!(b1 < a2 || b2 < a1)) {
                    int ns = Math.min(a1, a2);
                    int ne = Math.max(b1, b2);
                    arr.get(i).set(0, ns);
                    arr.get(i).set(1, ne);
                    arr.remove(j);
                    mergedSomething = true;
                    break;
                }
            }
        }
    }

    return arr;
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal-1 — Sort + Merge (List<List<Integer>>)</summary>

```java
public List<List<Integer>> mergeOverlap(List<List<Integer>> intervals) {
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
```
</details>

<details>
<summary>🚀 Optimal-2 — Sort + Merge (int[][] — LeetCode format)</summary>

```java
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
```

**Time Complexity:** O(n log n) | **Space Complexity:** O(n)
</details>

---

### Q9. Merge Two Sorted Arrays Without Extra Space
**Pattern:** Sorting (In-place) | **LeetCode:** 88

<details>
<summary>✅ My Solution — Extra Array</summary>

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int arr[] = new int[m+n];
        int i=0, j=0, k=0;

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
```
</details>

<details>
<summary>⚡ Brute Force (Reference)</summary>

```java
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
```

**Time Complexity:** O(N+M) + O(N+M) | **Space Complexity:** O(N+M)
</details>

<details>
<summary>🔧 Optimal-1 — Swap + Sort</summary>

```java
// Function to merge two sorted arrays nums1 and nums2
public void merge(int[] nums1, int m, int[] nums2, int n) {
    // Pointer for nums1 (end of valid elements)
    int left = m - 1;
    
    // Pointer for nums2 (beginning
