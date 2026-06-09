# 📒 Striver SDE Sheet - Part 1 (Q1-Q14)

> **Interactive Revision Guide** — Click ▶ to expand solutions. Click problem names in index to jump directly.
> 📄 **[→ Part 2 (Q15-Q28 + Pattern Guide)](Striver_SDE_Sheet_Notes_Part2.md)**

---

## 📑 Quick Navigation

- [🎯 Pattern-Based Index](#-pattern-based-index)
- [🔢 Sequential Index](#-sequential-index)
- [📝 Detailed Solutions Q1-Q14](#-detailed-solutions-q1-q14)

---

## 🎯 Pattern-Based Index

| # | Pattern | Problems |
|---|---------|----------|
| A | **Two Pointer** | [Q3](#q3-next-permutation), [Q5](#q5-sort-colors-dutch-national-flag), [Q9](#q9-merge-two-sorted-arrays-without-extra-space), [Q19](Striver_SDE_Sheet_Notes_Part2.md#q19-two-sum), [Q25](Striver_SDE_Sheet_Notes_Part2.md#q25-3-sum), [Q20](Striver_SDE_Sheet_Notes_Part2.md#q20-4-sum), [Q26](Striver_SDE_Sheet_Notes_Part2.md#q26-trapping-rainwater), [Q27](Striver_SDE_Sheet_Notes_Part2.md#q27-remove-duplicates-from-sorted-array) |
| B | **Sliding Window** | [Q24](Striver_SDE_Sheet_Notes_Part2.md#q24-longest-substring-without-repeating-characters), [Q22](Striver_SDE_Sheet_Notes_Part2.md#q22-largest-subarray-with-k-sum) (positive) |
| C | **Prefix Sum / Prefix XOR** | [Q22](Striver_SDE_Sheet_Notes_Part2.md#q22-largest-subarray-with-k-sum) (negatives), [Q23](Striver_SDE_Sheet_Notes_Part2.md#q23-count-subarrays-with-given-xor-k) |
| D | **Kadane's Algorithm** | [Q4](#q4-maximum-subarray-kadanes-algorithm) |
| E | **HashMap / HashSet** | [Q19](Striver_SDE_Sheet_Notes_Part2.md#q19-two-sum) (HashMap), [Q21](Striver_SDE_Sheet_Notes_Part2.md#q21-longest-consecutive-sequence-in-an-array) |
| F | **Boyer-Moore Voting** | [Q15](Striver_SDE_Sheet_Notes_Part2.md#q15-majority-element-i), [Q16](Striver_SDE_Sheet_Notes_Part2.md#q16-majority-element-ii) |
| G | **Binary Search** | [Q13](#q13-search-in-a-2d-matrix), [Q14](#q14-powx-n) (Binary Exponentiation) |
| H | **Merge Sort (Divide & Conquer)** | [Q12](#q12-count-inversions), [Q18](Striver_SDE_Sheet_Notes_Part2.md#q18-reverse-pairs) |
| I | **Floyd's Cycle Detection** | [Q10](#q10-find-the-duplicate-number) |
| J | **Matrix Manipulation** | [Q1](#q1-set-matrix-zeroes), [Q7](#q7-rotate-matrix-by-90) |
| K | **Math / Combinatorics / DP** | [Q2](#q2-pascals-triangle), [Q17](Striver_SDE_Sheet_Notes_Part2.md#q17-grid-unique-paths) |
| L | **Sorting + Merging** | [Q8](#q8-merge-overlapping-subintervals) |
| M | **Sign Marking / In-place** | [Q11](#q11-find-the-repeating-and-missing-number) |
| N | **Greedy** | [Q6](#q6-best-time-to-buy-and-sell-stock) |
| O | **Linear Scan** | [Q28](Striver_SDE_Sheet_Notes_Part2.md#q28-maximum-consecutive-ones) |

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
| 15 | [Majority Element I → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q15-majority-element-i) | F. Boyer-Moore Voting | O(n) | O(1) |
| 16 | [Majority Element II → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q16-majority-element-ii) | F. Boyer-Moore Voting | O(n) | O(1) |
| 17 | [Grid Unique Paths → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q17-grid-unique-paths) | K. Math/Combinatorics/DP | O(M×N) | O(N) |
| 18 | [Reverse Pairs → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q18-reverse-pairs) | H. Merge Sort (Divide & Conquer) | O(2N log N) | O(N) |
| 19 | [Two Sum → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q19-two-sum) | A. Two Pointer / E. HashMap | O(N) | O(N) |
| 20 | [4 Sum → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q20-4-sum) | A. Two Pointer (Sort+2P) | O(N³) | O(1)* |
| 21 | [Longest Consecutive Sequence → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q21-longest-consecutive-sequence-in-an-array) | E. HashMap/HashSet | O(N) | O(N) |
| 22 | [Largest Subarray with K Sum → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q22-largest-subarray-with-k-sum) | C. Prefix Sum / B. Sliding Window | O(N) | O(N) |
| 23 | [Count Subarrays with XOR K → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q23-count-subarrays-with-given-xor-k) | C. Prefix XOR + HashMap | O(N) | O(N) |
| 24 | [Longest Substring Without Repeating → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q24-longest-substring-without-repeating-characters) | B. Sliding Window | O(N) | O(256) |
| 25 | [3 Sum → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q25-3-sum) | A. Two Pointer (Sort+2P) | O(N²) | O(1)* |
| 26 | [Trapping Rainwater → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q26-trapping-rainwater) | A. Two Pointer (Prefix-Suffix) | O(N) | O(1) |
| 27 | [Remove Duplicates → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q27-remove-duplicates-from-sorted-array) | A. Two Pointer (In-place) | O(N) | O(1) |
| 28 | [Maximum Consecutive Ones → Part 2](Striver_SDE_Sheet_Notes_Part2.md#q28-maximum-consecutive-ones) | O. Linear Scan | O(N) | O(1) |

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
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        boolean[] rows = new boolean[m];
        boolean[] cols = new boolean[n];
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }
        
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

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        
        boolean firstRowZero = false;
        boolean firstColZero = false;
        
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) { firstColZero = true; break; }
        }
        
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) { firstRowZero = true; break; }
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        
        if (firstRowZero) { for (int j = 0; j < n; j++) matrix[0][j] = 0; }
        if (firstColZero) { for (int i = 0; i < m; i++) matrix[i][0] = 0; }
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
        if(numRows == 1) return ans;

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
<summary>✅ 2.2 Pascal's Triangle I — Specific Element (r, c)</summary>

```java
class Solution {
    public int pascalTriangleI(int r, int c) {
        r = r-1; c = c-1;
        c = Math.min(r-c, c);
        int mul = 1;
        for(int i =1; i<=c;i++ ){
            mul *= (r-i+1);
            mul /= i;
        }
        return mul;
    }
}
```

**Time Complexity:** O(C) | **Space Complexity:** O(1)
</details>

---

### Q3. Next Permutation
**Pattern:** Two Pointer | **LeetCode:** 31

<details>
<summary>⚡ Brute Force — Generate All Permutations</summary>

```java
class Solution {
    public void nextPermutation(int[] nums) {
        List<List<Integer>> ans = getAllPermutations(nums);
        int index = -1;
        for(int i = 0; i < ans.size(); i++) {
            if(match(nums, ans.get(i))) { index = i; break; }
        }
        int nextPermutationIndex = (index == ans.size() - 1) ? 0 : index+1;
        for(int i = 0; i < nums.length; i++) {
            nums[i] = ans.get(nextPermutationIndex).get(i);
        }
    }
    // ... helper methods omitted for brevity
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
        int idx=-1;

        //1. Find the first index from the end where nums[i] < nums[i+1]
        for(int i=n-2;i>=0;i--){
            if(nums[i]<nums[i+1]){ idx=i; break; }
        }

        //2. Find the element just greater than nums[ind] from the end
        if(idx!=-1){
            int swapIdx = idx;
            for(int j = n-1; j>idx; j--){
                if(nums[j] > nums[idx]){ swapIdx = j; break; }
            }
            swap(nums,idx,swapIdx);
        }

        //3. Reverse the right half
        reverse(nums,idx+1,n-1);
    }

    private void reverse(int nums[], int i, int j){
        while(i<j){ swap(nums,i,j); i++; j--; }
    }

    private void swap(int nums[], int i, int j){
        int temp = nums[i]; nums[i] = nums[j]; nums[j] = temp;
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
public int maxSubArray(int[] nums) {
    int maxi = Integer.MIN_VALUE; 
    for (int i = 0; i < nums.length; i++) {
        for (int j = i; j < nums.length; j++) {
            int sum = 0; 
            for (int k = i; k <= j; k++) { sum += nums[k]; }
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
<summary>🚀 Optimal — Kadane's Algorithm O(n)</summary>

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

---

### Q5. Sort Colors (Dutch National Flag)
**Pattern:** Two Pointer (Dutch National Flag) | **LeetCode:** 75

<details>
<summary>⚡ Brute Force — Arrays.sort</summary>

```java
class Solution {
    public void sortZeroOneTwo(int[] nums) { Arrays.sort(nums); }
}
```

**Time Complexity:** O(N×logN) | **Space Complexity:** O(1)
</details>

<details>
<summary>🔧 Better — Counting O(2N)</summary>

```java
public void sortZeroOneTwo(int[] nums) {
    int cnt0 = 0, cnt1 = 0, cnt2 = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) cnt0++;
        else if (nums[i] == 1) cnt1++;
        else cnt2++;
    }
    for (int i = 0; i < cnt0; i++) nums[i] = 0;
    for (int i = cnt0; i < cnt0 + cnt1; i++) nums[i] = 1; 
    for (int i = cnt0 + cnt1; i < nums.length; i++) nums[i] = 2;
}
```

**Time Complexity:** O(2N) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal — Dutch National Flag O(n)</summary>

```java
class Solution {
    public void sortColors(int[] nums) {
        int zeroPtr = 0, twoPtr = nums.length-1;
        int mid = 0;
        while(mid<=twoPtr){
            if(nums[mid] == 1) mid++;
            else if(nums[mid]==0){ swap(nums,mid,zeroPtr); zeroPtr++; mid++; }
            else{ swap(nums,mid,twoPtr); twoPtr--; }
        }
    }
    private void swap(int nums[], int i, int j){
        int temp = nums[i]; nums[i] = nums[j]; nums[j] = temp;
    }
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(1)
</details>

---

### Q6. Best Time to Buy and Sell Stock
**Pattern:** Greedy | **LeetCode:** 121

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
            for(int j=0;j<n;j++){ temp[j][n-i-1] = matrix[i][j]; }
        }
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){ matrix[i][j] = temp[i][j]; }
        }
    }
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(N²)
</details>

<details>
<summary>🚀 Optimal — Transpose + Reverse O(1) Space</summary>

```java
public void rotateMatrix(int[][] matrix) {
    int n = matrix.length;
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            int temp = matrix[i][j]; matrix[i][j] = matrix[j][i]; matrix[j][i] = temp;
        }
    }
    // Reverse each row
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n / 2; j++) {
            int temp = matrix[i][j]; matrix[i][j] = matrix[i][n-1-j]; matrix[i][n-1-j] = temp;
        }
    }
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(1)
</details>

---

### Q8. Merge Overlapping Subintervals
**Pattern:** Sorting + Merging | **LeetCode:** 56

<details>
<summary>🚀 Optimal — Sort + Merge O(n log n)</summary>

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length == 0) return new int[0][0];
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> ans = new ArrayList<>();
        ans.add(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            int[] last = ans.get(ans.size() - 1);
            if (last[1] >= intervals[i][0]) {
                last[1] = Math.max(last[1], intervals[i][1]);
            } else {
                ans.add(intervals[i]);
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
<summary>🚀 Optimal — Two Pointers from End O(N+M) O(1)</summary>

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1, j = n - 1;
    int ind = m + n - 1;
    while (j >= 0) {
        if (i >= 0 && nums1[i] >= nums2[j]) {
            nums1[ind] = nums1[i]; ind--; i--;
        } else {
            nums1[ind] = nums2[j]; ind--; j--;
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
<summary>🚀 Optimal — Sign Marking O(n) O(1)</summary>

```java
class Solution {
    public int[] findMissingRepeatingNumbers(int[] nums) {
        int missing = -1, repeating = -1;
        for (int i = 0; i < nums.length; i++) {
            int idx = Math.abs(nums[i]) - 1;
            if (nums[idx] > 0) nums[idx] = -nums[idx];
            else repeating = idx + 1;
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) { missing = i + 1; break; }
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
                if(nums[i]>nums[j]) ans++;
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
            if(nums[left] <= nums[right]) temp[idx++] = nums[left++];
            else{ temp[idx++] = nums[right++]; cnt += (mid - left + 1); }
        }
        while(left <= mid) temp[idx++] = nums[left++];
        while(right <= high) temp[idx++] = nums[right++];
        idx = 0;
        for(int i=low; i<=high;i++) nums[i] = temp[idx++];
        return cnt;
    }
}
```

**Time Complexity:** O(n log n) | **Space Complexity:** O(n)

**⚠️ Key:** `left` must start from `low`, NOT from `0`. Use `long` for count.
</details>

---

### Q13. Search in a 2D Matrix
**Pattern:** Binary Search | **LeetCode:** 74

<details>
<summary>🚀 Optimal — Binary Search on Flattened Matrix O(log(N×M))</summary>

```java
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
    if (n == 0 || x == 1.0) return 1; 
    long temp = n;
    if (n < 0) { x = 1 / x; temp = -1L * n; }
    double ans = 1;
    for (long i = 0; i < temp; i++) ans *= x; 
    return ans;
}
```

**Time Complexity:** O(n) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal — Binary Exponentiation O(log n)</summary>

```java
class Solution {
    public double myPow(double x, int n) {
        long num = n;
        if(num < 0) return (1.0 / pow(x, -num));
        return pow(x, num);
    }
    private double pow(double x, long num){
        if(num == 0) return 1.0;
        if(num % 2 == 0) return pow(x*x, num/2);
        return x*pow(x, num-1);
    }
}
```

**Time Complexity:** O(log N) | **Space Complexity:** O(log n)

**⚠️ Key:** Use `long` for `n` to handle `Integer.MIN_VALUE`.
</details>

---

📄 **Continue to [Part 2 (Q15-Q28 + Pattern Guide)](Striver_SDE_Sheet_Notes_Part2.md) →**
</task_progress>
</write_to_file>
