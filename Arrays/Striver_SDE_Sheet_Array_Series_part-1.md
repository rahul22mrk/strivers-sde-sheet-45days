# 📒 Striver SDE Sheet - Part 1 (Q1-Q14)

> **Interactive Revision Guide** — Click ▶ to expand solutions. Click problem names in index to jump directly.
> 📄 **[→ Part 2 (Q15-Q28 + Pattern Guide)](Striver_SDE_Sheet_Array_Series_part-2.md)**

---

## 📑 Quick Navigation

- [🎯 Pattern-Based Index](#-pattern-based-index)
- [🔢 Sequential Index](#-sequential-index)
- [📝 Detailed Solutions Q1-Q14](#-detailed-solutions-q1-q14)

---

## 🎯 Pattern-Based Index

| # | Pattern | Problems |
|---|---------|----------|
| A | **Two Pointer** | [Q3](#q3-next-permutation), [Q5](#q5-sort-colors-dutch-national-flag), [Q9](#q9-merge-two-sorted-arrays-without-extra-space), [Q19](Striver_SDE_Sheet_Array_Series_part-2.md#q19-two-sum), [Q25](Striver_SDE_Sheet_Array_Series_part-2.md#q25-3-sum), [Q20](Striver_SDE_Sheet_Array_Series_part-2.md#q20-4-sum), [Q26](Striver_SDE_Sheet_Array_Series_part-2.md#q26-trapping-rainwater), [Q27](Striver_SDE_Sheet_Array_Series_part-2.md#q27-remove-duplicates-from-sorted-array) |
| B | **Sliding Window** | [Q24](Striver_SDE_Sheet_Array_Series_part-2.md#q24-longest-substring-without-repeating-characters), [Q22](Striver_SDE_Sheet_Array_Series_part-2.md#q22-largest-subarray-with-k-sum) (positive) |
| C | **Prefix Sum / Prefix XOR** | [Q22](Striver_SDE_Sheet_Array_Series_part-2.md#q22-largest-subarray-with-k-sum) (negatives), [Q23](Striver_SDE_Sheet_Array_Series_part-2.md#q23-count-subarrays-with-given-xor-k) |
| D | **Kadane's Algorithm** | [Q4](#q4-maximum-subarray-kadanes-algorithm) |
| E | **HashMap / HashSet** | [Q19](Striver_SDE_Sheet_Array_Series_part-2.md#q19-two-sum) (HashMap), [Q21](Striver_SDE_Sheet_Array_Series_part-2.md#q21-longest-consecutive-sequence-in-an-array) |
| F | **Boyer-Moore Voting** | [Q15](Striver_SDE_Sheet_Array_Series_part-2.md#q15-majority-element-i), [Q16](Striver_SDE_Sheet_Array_Series_part-2.md#q16-majority-element-ii) |
| G | **Binary Search** | [Q13](#q13-search-in-a-2d-matrix), [Q14](#q14-powx-n) (Binary Exponentiation) |
| H | **Merge Sort (Divide & Conquer)** | [Q12](#q12-count-inversions), [Q18](Striver_SDE_Sheet_Array_Series_part-2.md#q18-reverse-pairs) |
| I | **Floyd's Cycle Detection** | [Q10](#q10-find-the-duplicate-number) |
| J | **Matrix Manipulation** | [Q1](#q1-set-matrix-zeroes), [Q7](#q7-rotate-matrix-by-90) |
| K | **Math / Combinatorics / DP** | [Q2](#q2-pascals-triangle), [Q17](Striver_SDE_Sheet_Array_Series_part-2.md#q17-grid-unique-paths) |
| L | **Sorting + Merging** | [Q8](#q8-merge-overlapping-subintervals) |
| M | **Sign Marking / In-place** | [Q11](#q11-find-the-repeating-and-missing-number) |
| N | **Greedy** | [Q6](#q6-best-time-to-buy-and-sell-stock) |
| O | **Linear Scan** | [Q28](Striver_SDE_Sheet_Array_Series_part-2.md#q28-maximum-consecutive-ones) |

---

## 🔢 Sequential Index

| # | Problem | Pattern | Best TC | Best SC |
|---|---------|---------|---------|---------|
| 1 | [Set Matrix Zeroes](#q1-set-matrix-zeroes) | J. Matrix Manipulation | O(m×n) | O(1) |
| 2 | [Pascal's Triangle](#q2-pascals-triangle) | K. Math/Combinatorics/DP | O(n²) | O(n²) |
| 3 | [Next Permutation](#q3-next-permutation) | A. Two Pointer | O(n) | O(1) |
| 4 | [Maximum Subarray (Kadane's)](#q4-maximum-subarray-kadanes-algorithm) | D. Kadane's Algorithm | O(n) | O(1) |
| 5 | [Sort Colors](#q5-sort-colors-dutch-national-flag) | A. Two Pointer (Dutch National Flag) | O(n) | O(1) |
| 6 | [Best Time to Buy and Sell Stock](#q6-best-time-to-buy-and-sell-stock) | N. Greedy | O(n) | O(1) |
| 7 | [Rotate Matrix by 90°](#q7-rotate-matrix-by-90) | J. Matrix Manipulation | O(n²) | O(1) |
| 8 | [Merge Overlapping Intervals](#q8-merge-overlapping-subintervals) | L. Sorting + Merging | O(n log n) | O(n) |
| 9 | [Merge Sorted Arrays](#q9-merge-two-sorted-arrays-without-extra-space) | A. Two Pointer | O(n+m) | O(1) |
| 10 | [Find the Duplicate Number](#q10-find-the-duplicate-number) | I. Floyd's Cycle Detection | O(n) | O(1) |
| 11 | [Find Repeating & Missing Number](#q11-find-the-repeating-and-missing-number) | M. Sign Marking / In-place | O(n) | O(1) |
| 12 | [Count Inversions](#q12-count-inversions) | H. Merge Sort (Divide & Conquer) | O(n log n) | O(n) |
| 13 | [Search in 2D Matrix](#q13-search-in-a-2d-matrix) | G. Binary Search | O(log(n×m)) | O(1) |
| 14 | [Pow(x, n)](#q14-powx-n) | G. Binary Search (Binary Exponentiation) | O(log n) | O(log n) |
| 15-28 | [→ See Part 2](Striver_SDE_Sheet_Array_Series_part-2.md#-detailed-solutions-q15-q28) | | | |

---

## 📝 Detailed Solutions Q1-Q14

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

**Time Complexity:** O(C), where C is the column number.

**Space Complexity:** O(1), as no extra space is used.
</details>

---

### Q3. Next Permutation
**Pattern:** Two Pointer | **LeetCode:** 31

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

**Time Complexity:** O(N) | **Space Complexity:** O(1)
</details>

---

### Q4. Maximum Subarray (Kadane's Algorithm)
**Pattern:** Kadane's Algorithm | **LeetCode:** 53

<details>
<summary>⚡ Brute Force — O(n³)</summary>

```java
// Function to find maximum sum of subarrays
public int maxSubArray(int[] nums) {
    int maxi = Integer.MIN_VALUE; 

    for (int i = 0; i < nums.length; i++) {
        for (int j = i; j < nums.length; j++) {
            int sum = 0; 
            for (int k = i; k <= j; k++) {
                sum += nums[k];
            }
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
    int maxi = Integer.MIN_VALUE;

    for (int i = 0; i < nums.length; i++) {
        int sum = 0; 
        for (int j = i; j < nums.length; j++) {
            sum += nums[j];
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
    long maxi = Long.MIN_VALUE; 
    long sum = 0; 
    
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i]; 
        
        if (sum > maxi) {
            maxi = sum; 
        }
        
        if (sum < 0) {
            sum = 0; 
        }
    }
    
    return (int) maxi;
}
```
</details>

<details>
<summary>🎯 Follow-up: Print the Subarray with Max Sum</summary>

```java
// Function to find maximum sum of subarrays and print the subarray having maximum sum
public int maxSubArray(int[] nums) {
    long maxi = Long.MIN_VALUE; 
    long sum = 0; 
    int start = 0; 
    int ansStart = -1, ansEnd = -1; 
    
    for (int i = 0; i < nums.length; i++) {
        if (sum == 0) {
            start = i;
        }
        
        sum += nums[i]; 
        
        if (sum > maxi) {
            maxi = sum;
            ansStart = start;
            ansEnd = i;
        }
        
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

    return (int) maxi;
}
```
</details>

---

### Q5. Sort Colors (Dutch National Flag)
**Pattern:** Two Pointer (Dutch National Flag) | **LeetCode:** 75

<details>
<summary>⚡ Brute Force — Arrays.sort</summary>

```java
class Solution {
    // Function to sort the array
    public void sortZeroOneTwo(int[] nums) {
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

**Time Complexity:** O(N) | **Space Complexity:** O(1)
</details>

---

### Q6. Best Time to Buy and Sell Stock
**Pattern:** Greedy | **LeetCode:** 121

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
**Pattern:** Sorting + Merging | **LeetCode:** 56

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
public List<List<Integer>> mergeOverlap(List<List<Integer>> arr) {
    int n = (int) arr.size();
    if (n <= 1) return arr;

    boolean mergedSomething = true;
    while (mergedSomething) {
        mergedSomething = false;
        for (int i = 0; i < arr.size() && !mergedSomething; ++i) {
            for (int j = i + 1; j < arr.size(); ++j) {
                int a1 = arr.get(i).get(0);
                int b1 = arr.get(i).get(1);
                int a2 = arr.get(j).get(0);
                int b2 = arr.get(j).get(1);

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

    intervals.sort((a, b) -> Integer.compare(a.get(0), b.get(0)));

    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>(intervals.get(0)));

    for (int i = 1; i < intervals.size(); i++) {
        List<Integer> last = result.get(result.size() - 1);
        List<Integer> curr = intervals.get(i);

        if (curr.get(0) <= last.get(1)) {
            last.set(1, Math.max(last.get(1), curr.get(1)));
        } else {
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
**Pattern:** Two Pointer | **LeetCode:** 88

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
    int[] merged = new int[m + n];
    int left = 0;
    int right = 0;
    int index = 0;

    while (left < m && right < n) {
        if (nums1[left] <= nums2[right]) {
            merged[index++] = nums1[left++];
        } else {
            merged[index++] = nums2[right++];
        }
    }

    while (left < m) {
        merged[index++] = nums1[left++];
    }

    while (right < n) {
        merged[index++] = nums2[right++];
    }

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
    int left = m - 1;
    int right = 0;
    
    while (left >= 0 && right < n) {
        if (nums1[left] > nums2[right]) {
            int temp = nums1[left];
            nums1[left] = nums2[right];
            nums2[right] = temp;
            left--;
            right++;
        } else break;
    }
    
    Arrays.sort(nums1, 0, m);
    Arrays.sort(nums2);
    
    for (int i = m; i < m + n; i++) {
        nums1[i] = nums2[i - m];
    }
}
```

**Time Complexity:** O(min(N,M)) + O(N×logN) + O(M×logM) + O(N) | **Space Complexity:** O(1)
</details>

<details>
<summary>🔧 Optimal-2 — Gap Method (Shell Sort)</summary>

```java
// Function to merge two sorted arrays nums1 and nums2
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int len = n + m;
    int gap = (len / 2) + (len % 2);

    while (gap > 0) {
        int left = 0;
        int right = left + gap;
        while (right < len) {
            if (left < m && right >= m) {
                swapIfGreater(nums1, nums2, left, right - m);
            }
            else if (left >= m) {
                swapIfGreater(nums2, nums2, left - m, right - m);
            }
            else {
                swapIfGreater(nums1, nums1, left, right);
            }
            left++;
            right++;
        }
        if (gap == 1) break;
        gap = (gap / 2) + (gap % 2);
    }

    for (int i = m; i < m + n; i++) {
        nums1[i] = nums2[i - m];
    }
}

private void swapIfGreater(int[] arr1, int[] arr2, int idx1, int idx2) {
    if (arr1[idx1] > arr2[idx2]) {
        int temp = arr1[idx1];
        arr1[idx1] = arr2[idx2];
        arr2[idx2] = temp;
    }
}
```

**Time Complexity:** O((N+M)×log(N+M)) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal-3 — Two Pointers from End (Best)</summary>

```java
// Function to merge two sorted arrays nums1 and nums2
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1, j = n - 1;
    int ind = m + n - 1;

    while (j >= 0) {
        if (i >= 0 && nums1[i] >= nums2[j]) {
            nums1[ind] = nums1[i];
            ind--;
            i--;
        } else {
            nums1[ind] = nums2[j];
            ind--;
            j--;
        }
    }
}
```

**Time Complexity:** O(N + M) | **Space Complexity:** O(1)
</details>

---

### Q10. Find the Duplicate Number
**Pattern:** Floyd's Cycle Detection | **LeetCode:** 287

<details>
<summary>🔧 Better — Frequency Array</summary>

```java
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
```
</details>

<details>
<summary>🚀 Optimal — Floyd's Cycle Detection O(n) O(1)</summary>

```java
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
```

**Time Complexity:** O(n) | **Space Complexity:** O(1)
</details>

---

### Q11. Find the Repeating and Missing Number
**Pattern:** Sign Marking / In-place | **GFG**

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
// Function to find repeating and missing numbers
public int[] findMissingRepeatingNumbers(int[] nums) {
    int n = nums.length;
    int repeating = -1, missing = -1;

    for (int i = 1; i <= n; i++) {
        int cnt = 0;
        for (int j = 0; j < n; j++) {
            if (nums[j] == i) cnt++;
        }

        if (cnt == 2) repeating = i;
        else if (cnt == 0) missing = i;

        if (repeating != -1 && missing != -1) break;
    }

    return new int[] {repeating, missing};
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(1)
</details>

<details>
<summary>🔧 Better — Frequency Array</summary>

```java
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
```
</details>

<details>
<summary>🚀 Optimal — Sign Marking O(n) O(1)</summary>

```java
class Solution {
    public int[] findMissingRepeatingNumbers(int[] nums) {
        int missing = -1;
        int repeating = -1;

        for (int i = 0; i < nums.length; i++) {
            int idx = Math.abs(nums[i]) - 1;

            if (nums[idx] > 0) {
                nums[idx] = -nums[idx]; // mark as visited
            } else {
                repeating = idx + 1; // already visited => duplicate
            }
        }

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                missing = i + 1;
                break;
            }
        }

        return new int[]{repeating, missing};
    }
}
```

**Time Complexity:** O(n) | **Space Complexity:** O(1)
</details>

---

### Q12. Count Inversions
**Pattern:** Merge Sort (Divide & Conquer) | **GFG**

<details>
<summary>⚡ Brute Force — O(n²)</summary>

```java
class Solution {
    public long numberOfInversions(int[] nums) {
        int ans = 0;

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
```
</details>

<details>
<summary>🚀 Optimal — Merge Sort O(n log n)</summary>

```java
class Solution {
    public long numberOfInversions(int[] nums) {
        return mergeSort(nums,0,nums.length-1);
    }

    private long mergeSort(int nums[], int low, int high){
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
```

**Time Complexity:** O(n log n) | **Space Complexity:** O(n)

**⚠️ Key:** `left` must start from `low`, NOT from `0`. Use `long` for count to avoid overflow.
</details>

---

### Q13. Search in a 2D Matrix
**Pattern:** Binary Search | **LeetCode:** 74

<details>
<summary>⚡ Brute Force — O(N×M)</summary>

```java
//Function to search for a given target in matrix
public boolean searchMatrix(int[][] mat, int target) {
    if (mat.length == 0 || mat[0].length == 0) {
        return false;
    }
    
    int n = mat.length;  
    int m = mat[0].length; 
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (mat[i][j] == target) {
                return true; 
            }
        }
    }
    return false; 
}
```

**Time Complexity:** O(N×M) | **Space Complexity:** O(1)
</details>

<details>
<summary>🔧 Better — Row Binary Search O(N + logM)</summary>

```java
class Solution {
    public boolean searchMatrix(int[][] mat, int target) {
        for(int i=0; i<mat.length; i++){
            if(mat[i][0]<=target && mat[i][mat[i].length-1]>=target){
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
```

**Time Complexity:** O(N + logM) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal — Binary Search on Flattened Matrix O(log(N×M))</summary>

```java
// Function to search for a given target in matrix
public boolean searchMatrix(int[][] mat, int target) {
    int n = mat.length;
    int m = mat[0].length;

    int low = 0, high = n * m - 1;
    
    while (low <= high) {
        int mid = (low + high) / 2;
        int row = mid / m;
        int col = mid % m;
        
        if (mat[row][col] == target) return true;
        else if (mat[row][col] < target) low = mid + 1;
        else high = mid - 1;
    }
    
    return false; 
}
```

**Time Complexity:** O(log(N×M)) | **Space Complexity:** O(1)
</details>

---

### Q14. Pow(x, n)
**Pattern:** Binary Search (Binary Exponentiation) | **LeetCode:** 50

<details>
<summary>⚡ Brute Force — O(n)</summary>

```java
public double myPow(double x, int n) {
    // Base case: any number to the power of 0 is 1
    if (n == 0 || x == 1.0) return 1; 
    
    long temp = n; // to avoid integer overflow
    
    // Handle negative exponents
    if (n < 0) {
        x = 1 / x;
        temp = -1L * n;
    }

    double ans = 1;

    for (long i = 0; i < temp; i++) {
        // Multiply ans by x for n times
        ans *= x; 
    }
    return ans;
}
```

Complexity Analysis
Time Complexity: O(n), where n is the exponent. The loop runs n times to compute the power.

Space Complexity: O(1), as the algorithm uses a constant amount of extra space regardless of the input size.
</details>

<details>
<summary>🚀 Optimal — Binary Exponentiation O(log n)</summary>

```java
class Solution {
    public double myPow(double x, int n) {
        long num = n;

        if(num < 0){
            return (1.0 / pow(x, -num));
        }

        return pow(x, num);
    }

    private double pow(double x, long num){
        if(num == 0){
            return 1.0;
        }

        if(num % 2 == 0){
            return pow(x*x, num/2);
        }

        return x*pow(x, num-1);
    }
}
```
Complexity Analysis
Time Complexity : The time complexity is O(log N) due to the halving of n in the even case and linear reduction in the odd case.

Space Complexity :The space complexity is O(log n) because of the recursive call stack depth.

**⚠️ Key:** Use `long` for `n` to handle `Integer.MIN_VALUE` (whose negation overflows `int`).
</details>

---

📄 **Continue to [Part 2 (Q15-Q28 + Pattern Guide)](Striver_SDE_Sheet_Array_Series_part-2.md) →**
</task_progress>
</write_to_file>
