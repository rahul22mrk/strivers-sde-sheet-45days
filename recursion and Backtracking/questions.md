1. `Subset Sums`
2. `Subsets II`
3. `Combination Sum`
4. `Combination Sum II`
5. `Palindrome partitioning`
6. `Permutation Sequence`
7. `Permutations of a String`
8. `N Queen`
9. `Sudoku Solver`
10. `M Coloring Problem`
11. `Rat in a Maze`
12. `Word Break (print all ways)`

*************************************************************************************************
1. `Subset Sums`
```java
//Recursion
 /* Function to check if there is a subset of
    arr with sum equal to 'target' using recursion*/
    private boolean func(int ind, int target, int[] arr) {
        // Base cases
        if (target == 0)
            return true;
        
        if (ind == 0)
            return arr[0]== target;
        
        // Try not to take the current element into subset
        boolean notTaken = func(ind - 1, target, arr);
        
        /* Try taking the current element into the
        subset if it doesn't exceed the target*/
        boolean taken = false;
        if (arr[ind] <= target)
            taken = func(ind - 1, target - arr[ind], arr);
        
        // Return the result
        return notTaken || taken;
    }
    
    /* Function to check if there is a subset
    of 'arr' with sum equal to 'target'*/
    public boolean isSubsetSum(int[] arr, int target) {
        // Return the result
        return func(arr.length - 1, target, arr);
    }



Time Complexity: O(2(N)), where N is the length of the array. As for each index, there are two possible options.

Space Complexity:O(N), at maximum, the depth of the recursive stack can go up to N.
```

```java
//Memoization
  /* Function to check if there is a subset of
    arr with sum equal to 'target' using memoization*/
    private boolean func(int ind, int target, int[] arr, int[][] dp) {
        // Base cases
        if (target == 0)
            return true;
        
        if (ind == 0)
            return arr[0]== target;
            
        /* If the result for this subproblem has 
        already been calculated, return it*/
        if (dp[ind][target] != -1)
            return dp[ind][target] == 0 ? false : true;
        
        // Try not taking the current element into subset
        boolean notTaken = func(ind - 1, target, arr, dp);
        
        /* Try taking the current element into the
        subset if it doesn't exceed the target*/
        boolean taken = false;
        if (arr[ind] <= target)
            taken = func(ind - 1, target - arr[ind], arr, dp);
        
        /* Store the result in the DP table and 
        return whether either option was successful*/
        dp[ind][target] = notTaken || taken ? 1 : 0;
        return notTaken || taken;
    }
    
    /* Function to check if there is a subset
    of 'arr' with sum equal to 'target'*/
    public boolean isSubsetSum(int[] arr, int target) {
        // Declare a DP table with dimensions [n][k+1]
        int dp[][] = new int[arr.length][target + 1];
        
        // Initialize DP table with -1 
        for (int row[] : dp)
            Arrays.fill(row, -1);
            
        // Return the result
        return func(arr.length - 1, target, arr, dp);
    }
}

Complexity Analysis: 
Time Complexity: O(N*target), There are 'N*target' states therefore at max ‘N*target’ new problems will be solved.

Space Complexity:O(N*target) + O(N), As we are using a recursion stack space(O(N)) and a 2D array ( O(N*target)).
```
```java
//Tabulation
 /* Function to check if there is a subset
    of 'arr' with sum equal to 'target'*/
    private boolean func(int n, int target, int[] arr) {
        /* Initialize a 2D DP array with dimensions 
        (n x target+1) to store subproblem results*/
        boolean[][] dp = new boolean[n][target + 1];

        // Base case (when target = 0)
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }

        /* Base case (If the first element of 
        'arr' is less than or equal to 'target')*/
        if (arr[0] <= target) {
            dp[0][arr[0]] = true;
        }

        // Fill the DP array iteratively
        for (int ind = 1; ind < n; ind++) {
            for (int i = 1; i <= target; i++) {
                /* If we don't take the current element, the 
                result is the same as the previous row*/
                boolean notTaken = dp[ind - 1][i];

                /* If we take the current element, subtract its
                value from the target and check the previous row*/
                boolean taken = false;
                if (arr[ind] <= i) {
                    taken = dp[ind - 1][i - arr[ind]];
                }

                /* Store the result in the DP 
                array for the current subproblem*/
                dp[ind][i] = notTaken || taken;
            }
        }

        // The final result is stored in dp[n-1][target]
        return dp[n - 1][target];
    }

    /* Function to check if there is a subset
    of 'arr' with sum equal to 'target'*/
    public boolean isSubsetSum(int[] arr, int target) {
        // Return the result
        return func(arr.length, target, arr);
    }

Complexity Analysis: 
Time Complexity: O(N*target), As, here are three nested loops that account for O(N*target) complexity.

Space Complexity:O(N*target), As a 2D array of size N*target is used.
```
```java
//Space optimization
 /* Function to check if there is a subset
    of 'arr' with a sum equal to 'target'*/
    private boolean func(int n, int target, int[] arr) {
        /* Initialize an array 'prev' to store
        the previous row of the DP table*/
        boolean[] prev = new boolean[target + 1];
        
        /* Base case: If the target sum is 0, we 
        can always achieve it by taking no elements*/
        prev[0] = true;
        
        /* Base case: If the first element of 
        'arr' is less than or equal to 'target'*/
        if (arr[0] <= target) {
            prev[arr[0]] = true;
        }
        
        /* Iterate through the elements
        of 'arr' and update the DP table*/
        for (int ind = 1; ind < n; ind++) {
            /* Initialize a new array 'cur' to store
            the current state of the DP table*/
            boolean[] cur = new boolean[target + 1];
            
            /* Base case: If the target sum is 0, we 
            can achieve it by taking no elements*/
            cur[0] = true;
            
            for (int i = 1; i <= target; i++) {
                /* If we don't take the current element, the 
                result is the same as the previous row*/
                boolean notTaken = prev[i];
                
                /* If we take the current element, subtract its
                value from the target and check the previous row*/
                boolean taken = false;
                if (arr[ind] <= i) {
                    taken = prev[i - arr[ind]];
                }
                
                /* Store the result in the current DP
                table row for the current subproblem*/
                cur[i] = notTaken || taken;
            }
            
            /* Update 'prev' with the current 
            row 'cur' for the next iteration*/
            prev = cur;
        }
        
        // The final result is stored in prev[target]
        return prev[target];
    }
    
    /* Function to check if there is a subset 
    of 'arr' with sum equal to 'target'*/
    public boolean isSubsetSum(int[] arr, int target) {
        // Return the result
        return func(arr.length, target, arr);
    }
    
Complexity Analysis: 
Time Complexity: O(N*target), As, here are three nested loops that account for O(N*target) complexity.

Space Complexity:O(target), As we are using two external arrays of size ‘target'.
```


2. `Subsets II`
```java
 private void func(int ind, List<Integer> arr, int[] nums, List<List<Integer>> ans) {
        // If index reaches the end of nums
        if (ind == nums.length) {
            // Add the current subset (arr) to the result
            ans.add(new ArrayList<>(arr));
            return;
        }
        
        // Include the current element in the subset
        arr.add(nums[ind]);
        // Recur for the next index
        func(ind + 1, arr, nums, ans);
        // Backtrack: remove the current element from the subset
        arr.remove(arr.size() - 1);
        
        // Skip duplicates and recur for the next unique element
        for (int j = ind + 1; j < nums.length; j++) {
            if (nums[j] != nums[ind]) {
                func(j, arr, nums, ans);
                return;
            }
        }
        
        // Ensure the function finishes when no more unique elements are left
        func(nums.length, arr, nums, ans);
    }

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();  // Resulting list of subsets
        List<Integer> arr = new ArrayList<>();        // Current subset
        Arrays.sort(nums);  // Sort the array to handle duplicates
        func(0, arr, nums, ans);  // Start recursion
        return ans;
    }


Complexity Analysis
Time Complexity: O(2^N * N) - Each element is either included or excluded, leading to an exponential number of subsets.

Space Complexity: O(N) - The space complexity is dominated by the recursion stack, which can go as deep as the number of elements in the input list.

```


3. `Combination Sum`
```java
// Recursive function to find all subsequences with the given target sum
    public void func(List<Integer> v, int i, int sum, List<Integer> v2, List<List<Integer>> ans) {
        // Base case: if the sum is zero, add the current subsequence to the result
        if (sum == 0) {
            ans.add(new ArrayList<>(v2));
            return;
        }
        
        // Base case: if the sum becomes negative or no elements are left
        if (sum < 0 || i < 0) {
            return;
        }

        // Exclude the current element and move to the next
        func(v, i - 1, sum, v2, ans);
        
        // Include the current element in the subsequence
        v2.add(v.get(i));
        
        // Recursively call the function with the included element
        func(v, i, sum - v.get(i), v2, ans);
        
        // Backtrack by removing the last added element
        v2.remove(v2.size() - 1);
    }

    // Main function to find all unique combinations of candidates that sum to the target
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> v = new ArrayList<>();
        
        // Convert the array to a list for easier manipulation
        for (int num : candidates) {
            v.add(num);
        }
        
        // Start the recursive process
        func(v, v.size() - 1, target, new ArrayList<>(), ans);
        
        return ans;
    }



Complexity Analysis
Time Complexity: O(K*N(Target/m)), where N is the number of elements, and m is the minimum value among the elements. This is because the algorithm explores an exponential number of possible combinations in the worst case, as elements can be chosen repeatedly to form the target. For each valid combination found, it may take up to K operations to copy or process the combination, where K is the maximum length of any combination in the result.

Space Complexity: O(Target/m), because the deepest recursion and the longest combination both occur when repeatedly choosing the smallest element.

```


4. `Combination Sum II`
```java

// Recursive helper function to find combinations
    private void func(int ind, int sum, List<Integer> nums, 
                      int[] candidates, List<List<Integer>> ans) {
        // If the sum is zero, add the current combination to the result
        if (sum == 0) {
            ans.add(new ArrayList<>(nums));
            return;
        }

        // If the sum is negative or we have exhausted the candidates, return
        if (sum < 0 || ind == candidates.length) return; 

        // Include the current candidate
        nums.add(candidates[ind]);

        // Recursively call with updated sum and next index
        func(ind + 1, sum - candidates[ind], nums, candidates, ans);

        // Backtrack by removing the last added candidate
        nums.remove(nums.size() - 1);

        // Skip duplicates: if not picking the current candidate, 
        // ensure the next candidate is different
        for(int i = ind + 1; i < candidates.length; i++) {
            if(candidates[i] != candidates[ind]) {
                func(i, sum, nums, candidates, ans);
                break;
            }
        }
    }

    // Main function to find all unique combinations
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> nums = new ArrayList<>();

        // Sort candidates to handle duplicates
        Arrays.sort(candidates);

        // Start the recursive process
        func(0, target, nums, candidates, ans);
        return ans;
    }

Complexity Analysis
Time Complexity: O(2N * N), where N is the number of coins.
At each step, we explore two choices (include or exclude), leading to 2N possible subsets. For each valid subset, copying the current combination into the result takes up to O(N), giving the total complexity as O(2N * N) in the worst case.

Space Complexity: O(N), due to the recursion stack depth and storage for the current combination.

```


5. `Palindrome partitioning`
```java
 public List<List<String>> partition(String s) {
        // Resultant list to store all partitions
        List<List<String>> res = new ArrayList<>();
        // Temporary list to store current partition
        List<String> path = new ArrayList<>();
        // Start the depth-first search from index 0
        dfs(0, s, path, res);
        return res;
    }

    private void dfs(int index, String s, List<String> path, List<List<String>> res) {
        // If the index reaches the end of the string
        if (index == s.length()) {
            // Add the current partition to the result
            res.add(new ArrayList<>(path));
            return;
        }
        // Iterate over the substring starting from 'index'
        for (int i = index; i < s.length(); i++) {
            // Check if the substring s[index..i] is a palindrome
            if (isPalindrome(s, index, i)) {
                // If true, add it to the current path
                path.add(s.substring(index, i + 1));
                // Recur for the remaining substring
                dfs(i + 1, s, path, res);
                // Backtrack: remove the last added substring
                path.remove(path.size() - 1);
            }
        }
    }

    private boolean isPalindrome(String s, int start, int end) {
        // Check if the substring s[start..end] is a palindrome
        while (start <= end) {
            // If characters do not match, it's not a palindrome
            if (s.charAt(start++) != s.charAt(end--))
                return false;
        }
        return true; // Otherwise, it's a palindrome
    }



Complexity Analysis
Time Complexity: O(N × 2N), where N is the length of string given.
This is because we generate all possible partitions (exponential) and each palindrome check can take up to O(N).

Space Complexity: O(N), because the auxiliary space used (recursion stack + path) is O(N).
```


6. `Permutation Sequence`
```java
 // Function to get k-th permutation sequence of numbers 1..n
    public String getPermutation(int n, int k) {
        // Initialize list of numbers from 1 to n
        List<Integer> nums = new ArrayList<>();
        for(int i = 1; i <= n; i++) nums.add(i);

        // Compute factorials up to n
        int[] fact = new int[n];
        fact[0] = 1;
        for(int i = 1; i < n; i++) fact[i] = fact[i-1]*i;

        // Decrement k to convert to 0-based index
        k--;

        StringBuilder ans = new StringBuilder();

        // Build permutation one position at a time
        for(int i = n-1; i >= 0; i--) {
            int index = k / fact[i];
            ans.append(nums.get(index));
            nums.remove(index);
            k %= fact[i];
        }

        return ans.toString();
    }
}


Time and Space Complexity
Time Complexity: O(n²), where n is the number of elements. At each of n positions, removing an element from the list takes O(n) time.
Space Complexity: O(n), used to store the list of numbers and factorial array.

```


7. `Permutations of a String`
```java

 // Function to generate all unique permutations
    public List<String> permuteUnique(String s) {
        // Convert to char array and sort to handle duplicates
        char[] arr = s.toCharArray();
        Arrays.sort(arr);

        // List to store all unique permutations
        List<String> result = new ArrayList<>();
        // StringBuilder for building current permutation
        StringBuilder path = new StringBuilder();
        // Boolean array to mark used characters
        boolean[] used = new boolean[arr.length];

        // Backtracking function
        backtrack(arr, used, path, result);
        return result;
    }

    // Recursive helper for backtracking
    private void backtrack(char[] arr, boolean[] used, StringBuilder path, List<String> result) {
        // Base case: when permutation is complete
        if (path.length() == arr.length) {
            result.add(path.toString());
            return;
        }

        // Try each character
        for (int i = 0; i < arr.length; i++) {
            // Skip already used characters
            if (used[i]) continue;
            // Skip duplicates
            if (i > 0 && arr[i] == arr[i - 1] && !used[i - 1]) continue;

            // Choose character
            used[i] = true;
            path.append(arr[i]);
            // Recurse
            backtrack(arr, used, path, result);
            // Undo choice
            path.deleteCharAt(path.length() - 1);
            used[i] = false;
        }
    }

Complexity Analysis
Time Complexity: O(N × N!) In the worst case (when all characters are distinct), there are N! permutations. For each permutation, building and copying the string takes O(N) time, giving O(N × N!). When duplicates exist, the total number of permutations decreases, but the upper bound remains O(N × N!).

Space Complexity: O(N) We use O(N) space for recursion (function call stack) and the used[] array. The output list consumes O(N × N!) space if all permutations are stored.

```

```java
class Solution {
    public List<String> permuteUnique(String s) {
        Set<String> ans = new HashSet<>();

        boolean freq[] = new boolean[s.length()];

        solve(s, new StringBuilder(), ans, freq);
        List<String> list =  new ArrayList<>(ans);
        Collections.sort(list);
        return list;
    }

    private void solve(String s, StringBuilder curr, Set<String> ans, boolean freq[]){
        if(s.length() == curr.length()){
            ans.add(curr.toString());
            return ;
        }

        for(int i=0; i<s.length(); i++){
            if(!freq[i]){
                freq[i] = true;
                curr.append(s.charAt(i));
                solve(s,curr,ans,freq);

                curr.deleteCharAt(curr.length() - 1);
                freq[i] = false;
            }
        }
    }
}


```

```java
class Solution {
    public List<String> permuteUnique(String s) {
        char arr[] = s.toCharArray();

        Set<String> ans = new HashSet<>();
        solve(0, arr, ans);
        List<String> list = new ArrayList<>(ans);
        Collections.sort(list);
        return list;
    }

    private void solve(int idx, char arr[], Set<String> ans){
        if(idx == arr.length){
            StringBuilder sb = new StringBuilder();

            for(char ch: arr){
                sb.append(ch);
            }
            ans.add(sb.toString());
        }

        for(int i=idx; i<arr.length; i++){
            swap(arr, idx, i);
            solve(idx+1, arr, ans);
            swap(arr, idx, i);
        }
    }

    private void swap(char []arr, int i, int j){
        char ch = arr[i];
        arr[i] = arr[j];
        arr[j] = ch;
    }
}


```

7.1 `46. Permutations`
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        boolean[] freq  = new boolean[nums.length];
        recurPermute(nums,new ArrayList<>(),res,freq);
        return res;
        
    }

    private void recurPermute(int []nums,List<Integer>current,List<List<Integer>>res,boolean[] freq){
        if(current.size()==nums.length){
            res.add(new ArrayList<>(current));
            return;
        }

        for(int i=0;i<nums.length;i++){
            if(!freq[i]){
                freq[i]=true;
                current.add(nums[i]);
                recurPermute(nums,current,res,freq);
                freq[i]=false;
                current.remove(current.size()-1);
            }
        }
    }
}
```

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        solve(0,nums,ans);
        return ans;
    }

    private void solve(int idx, int[]nums, List<List<Integer>>ans){
        if(idx==nums.length){
            List<Integer> temp = new ArrayList();
            for(int num : nums){
                temp.add(num);
            }
            ans.add(temp);
            return ;
        }

        for(int i=idx;i<nums.length;i++){
            swap(nums,i,idx);
            solve(idx+1,nums,ans);
            swap(nums,i,idx);
        }
    }

    private void swap(int nums[], int i, int idx){
        int temp = nums[i];
        nums[i] = nums[idx];
        nums[idx] = temp;
    }
}
```

8. `N Queen`
```java
 // Check if it's safe to place a queen at board[row][col]
    private boolean isSafe(List<String> board, int row, int col) {
        int r = row, c = col;

        // Check for upper left diagonal
        while (r >= 0 && c >= 0) {
            if (board.get(r).charAt(c) == 'Q') return false;
            r--;
            c--;
        }

        // Reset to the original position
        r = row;
        c = col;

        // Check for top
        while (r >= 0) {
            if (board.get(r).charAt(c) == 'Q') return false;
            r--;
        }

        // Reset to the original position
        r = row;
        c = col;

        // Check for top right diagonal
        while (r >= 0 && c < board.get(0).length()) {
            if (board.get(r).charAt(c) == 'Q') return false;
            r--;
            c++;
        }

        // If no queens are found, it's safe
        return true;
    }

    // Function to place queens on the board
    private void func(int row, List<List<String>> ans, List<String> board) {
        // If all columns are filled, add the solution to the answer
        if (row == board.size()) {
            ans.add(new ArrayList<>(board));
            return;
        }

        // Try placing a queen in each column for the current row
        for (int col = 0; col < board.get(0).length(); col++) {
            // Check if it's safe to place a queen
            if (isSafe(board, row, col)) {
                // Place the queen
                char[] rowArr = board.get(row).toCharArray();
                rowArr[col] = 'Q';
                board.set(row, new String(rowArr));

                // Recursively place queens in the next rows
                func(row + 1, ans, board);

                // Remove the queen and backtrack
                rowArr[col] = '.';
                board.set(row, new String(rowArr));
            }
        }
    }

    // Solve the N-Queens problem
    public List<List<String>> solveNQueens(int n) {
        // List to store the solutions
        List<List<String>> ans = new ArrayList<>();
        // Initialize the board with empty cells
        List<String> board = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            board.add(".".repeat(n));
        }

        // Start placing queens from the first column
        func(0, ans, board);
        return ans;
    }


Complexity Analysis
Time Complexity: O(N!), where N is the number of queens
Due to the recursive search through potential placements and backtracking.

Space Complexity: O(N), for the recursion stack and the storage of the solutions.

```


9. `Sudoku Solver`
```java

  public void solveSudoku(char[][] board) {
        solve(board);
    }

    // Recursive method to solve the Sudoku
    private boolean solve(char[][] board) {
        int n = 9;  // Size of the board
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                // Empty cell found
                if (board[i][j] == '.') {  
                    for (char digit = '1'; digit <= '9'; digit++) {
                        // Check if digit can be placed
                        if (areRulesMet(board, i, j, digit)) {  
                            // Place digit
                            board[i][j] = digit;  
                            // Recur to place next digits
                            if (solve(board)) {  
                                return true;
                            } else {
                                // Reset if placing digit doesn't solve Sudoku
                                board[i][j] = '.';  
                            }
                        }
                    }
                    // If no digit can be placed, return false
                    return false;  
                }
            }
        }
        // Sudoku solved
        return true;  
    }

    // Method to check if placing a digit follows Sudoku rules
    private boolean areRulesMet(char[][] board, int row, int col, char digit) {
        for (int i = 0; i < 9; i++) {
            if (board[row][i] == digit || board[i][col] == digit) {
                // Digit already in row or column
                return false;  
            }
        }
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; i++) {
            for (int j = startCol; j < startCol + 3; j++) {
                if (board[i][j] == digit) {
                    // Digit already in 3x3 sub-box
                    return false;  
                }
            }
        }
        // Digit can be placed
        return true;  
    }


Complexity Analysis
Time Complexity: O(9E), where E (<= 81) is the number of empty cells.
As each empty cell can be filled with 1 to 9 digits.

Space Complexity: O(E), because of the recursive stack space.

```


10. `M Coloring Problem`
```java
 // Function to check if it's safe to color the node with a given color
    private boolean isSafe(int col, int node, int[] colors, List<List<Integer>> adj) {
        // Check adjacent nodes
        for (int neighbor : adj.get(node)) {
            // If an adjacent node has the same color
            if (colors[neighbor] == col) return false;
        }
        return true; // Safe to color
    }

    // Recursive function to solve graph coloring problem
    private boolean solve(int node, int m, int n, int[] colors, List<List<Integer>> adj) {
        // If all nodes are colored
        if (n == node) return true;
        // Try all colors from 1 to m
        for (int i = 1; i <= m; i++) {
            // Check if it is safe to color the node with color i
            if (isSafe(i, node, colors, adj)) {
                colors[node] = i; // Assign color i to node
                // Recursively try to color the next node
                if (solve(node + 1, m, n, colors, adj)) return true;
                colors[node] = 0; // Reset color if it doesn't lead to a solution
            }
        }
        return false; // No color can be assigned
    }

    // Function to check if the graph can be colored with m colors
    public boolean graphColoring(int[][] edges, int m, int n) {
        // Create adjacency list representation of the graph
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        // Build the graph from edges
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        int[] colors = new int[n]; // Initialize all colors to 0 (uncolored)
        // Start solving from the first node
        return solve(0, m, n, colors, adj);
    }



Complexity Analysis
Time Complexity : O(M^N) where m is the number of colors and n is the number of nodes, since each node can be colored in m ways and there are n nodes to color.

Space Complexity : O(N) for the colors array and O(n) for the adjacency list, resulting in O(N) total space complexity.
```


11. `Rat in a Maze`
```java
class Solution {
    public List<String> findPath(int[][] grid) {
        int n = grid.length;

        List<String> ans = new ArrayList<>();

        if(grid[0][0] == 0 || grid[n-1][n-1] == 0) return ans;

        solve(grid, 0, 0, new StringBuilder(), n, ans);
        Collections.sort(ans);

        return ans;
    }

    private void solve(int [][]grid, int x, int y, StringBuilder path, int n, List<String> ans){
        if(x == n-1 && y == n-1){
            ans.add(path.toString());
            return ;
        }

        //if cell is blocked, return
        if(grid[x][y] == 0){
            return;
        }

        grid[x][y] = 0; // mark cell as visited by setting it to 0

        //move up if possible
        path.append('U');
        if(x > 0) solve(grid, x-1, y, path, n, ans);
        path.deleteCharAt(path.length()-1);

        //move left if possible
        path.append('L');
        if(y > 0) solve(grid, x, y-1, path, n, ans);
        path.deleteCharAt(path.length()-1);

        //move Down if possible
        path.append('D');
        if(x < n-1) solve(grid, x + 1, y, path, n, ans);
        path.deleteCharAt(path.length()-1);

        //move Right if possible
        path.append('R');
        if(y < n-1) solve(grid, x, y + 1, path, n, ans);
        path.deleteCharAt(path.length()-1);

        grid[x][y] = 1;
    }
}
```
```java

 // List to store the resulting paths
    List<String> result = new ArrayList<>();

    // Recursive function to find paths
    private void path(int[][] m, int x, int y, String dir, int n) {
        // If destination is reached, add path to result
        if (x == n - 1 && y == n - 1) {
            result.add(dir);
            return;
        }

        // If cell is blocked, return
        if (m[x][y] == 0) return;

        // Mark cell as visited by setting it to 0
        m[x][y] = 0;

        // Move up if possible
        if (x > 0) path(m, x - 1, y, dir + 'U', n);
        // Move left if possible
        if (y > 0) path(m, x, y - 1, dir + 'L', n);
        // Move down if possible
        if (x < n - 1) path(m, x + 1, y, dir + 'D', n);
        // Move right if possible
        if (y < n - 1) path(m, x, y + 1, dir + 'R', n);

        // Unmark cell as visited by setting it to 1
        m[x][y] = 1;
    }

    public List<String> findPath(int[][] grid) {
        int n = grid.length;

        // Clear previous results
        result.clear();

        // If starting or ending cell is blocked, return empty result
        if (grid[0][0] == 0 || grid[n - 1][n - 1] == 0) return result;

        // Start finding paths from (0, 0)
        path(grid, 0, 0, "", n);

        // Sort the result paths
        Collections.sort(result);

        return result;
    }

Complexity Analysis
Time Complexity : The time complexity is O(4^(N^2)) due to recursion exploring all paths in the grid.

Space Complexity :The space complexity is O(N^2) for the recursion stack and result storage.
```


12. `Word Break (print all ways)`   

```java
//Approach-1 (Recur + Memoiz)
class Solution {
    private Boolean[] t;
    int n;
    public boolean wordBreak(String s, List<String> wordDict) {
        n = s.length();
        t = new Boolean[s.length()];
        return solve(s, 0, wordDict);
    }
    
    private boolean solve(String s, int idx, List<String> wordDict) {
        if (idx == n) {
            return true;
        }
        
        if (t[idx] != null) {
            return t[idx];
        }
        
        for (int endIdx = idx + 1; endIdx <= n; endIdx++) {
            
            String split = s.substring(idx, endIdx);
            
            if (wordDict.contains(split) && solve(s, endIdx, wordDict)) {
                return t[idx] = true;
            }
        }
        
        return t[idx] = false;
    }
}


```

```java
//Approach-2 (Bottom Up DP)
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet<>();
        wordSet.addAll(wordDict);
        
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i < dp.length ; i++) {
            for (int k = 1; k <= i; k++) {
                dp[i] = dp[i] || (dp[i - k] && wordSet.contains(s.substring(i - k, i)));
            }
        }
        return dp[s.length()];
    }
}

```


```java

  public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        int n = s.length();

        boolean[] dp = new boolean[n + 1];
        dp[0] = true;

        int maxLen = 0;
        for (String w : wordDict) maxLen = Math.max(maxLen, w.length());

        for (int i = 0; i < n; i++) {
            if (!dp[i]) continue;

            for (int len = 1; len <= maxLen && i + len <= n; len++) {
                if (dict.contains(s.substring(i, i + len))) {
                    dp[i + len] = true;
                }
            }
        }
        return dp[n];
    }

Complexity Analysis
Time Complexity: O(n × L). For each index, we try at most L substrings, where L is the longest dictionary word.
Space Complexity: O(n). Required for the DP array.

```

