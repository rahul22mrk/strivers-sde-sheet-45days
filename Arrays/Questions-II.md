

14. Pow(x, n)

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

    Complexity Analysis
Time Complexity: O(n), where n is the exponent. The loop runs n times to compute the power.

Space Complexity: O(1), as the algorithm uses a constant amount of extra space regardless of the input size.



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
Complexity Analysis
Time Complexity : The time complexity is O(log N) due to the halving of n in the even case and linear reduction in the odd case.

Space Complexity :The space complexity is O(log n) because of the recursive call stack depth.


15. Majority Element-I

 // Function to find the majority element in an array
    public int majorityElement(int[] nums) {
        
        // Size of the given array
        int n = nums.length;
        
        // Iterate through each element of the array
        for (int i = 0; i < n; i++) {
            
            // Counter to count occurrences of nums[i]
            int cnt = 0; 
            
            // Count the frequency of nums[i] in the array
            for (int j = 0; j < n; j++) {
                if (nums[j] == nums[i]) {
                    cnt++;
                }
            }
            
            // Check if frequency of nums[i] is greater than n/2
            if (cnt > (n / 2)) {
                // Return the majority element
                return nums[i]; 
            }
        }
        
        // Return -1 if no majority element is found
        return -1; 
    }
    Complexity Analysis 
Time Complexity: O(N2), for nested for loops used, where N is the size of the array

Space Complexity: O(1) as no extra space is used.



 // Function to find the majority element in an array
    public int majorityElement(int[] nums) {
        
        // Size of the given array
        int n = nums.length;
        
        // Hash map to store element counts
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Count occurrences of each element
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        /* Iterate through the map to
        find the majority element*/
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() > n / 2) {
                return entry.getKey();
            }
        }
        
        // Return -1 if no majority element is found
        return -1;
    }

    Complexity Analysis 
Time Complexity: O(N), where N is the size of the array.
The code goes through the array once to count frequencies using a hash map (O(N)), then checks the map to find the majority element (O(N) in the worst case). Since these are separate linear operations, the overall time complexity is O(N).

Space Complexity: O(N), for using a map data structure.



// Function to find the majority element in an array
    public int majorityElement(int[] nums) {
        // Size of the given array
        int n = nums.length;
        
        // Count
        int cnt = 0;
        
        // Element
        int el = 0;
        
        // Applying the algorithm
        for (int i = 0; i < n; i++) {
            if (cnt == 0) {
                cnt = 1;
                el = nums[i];
            } else if (el == nums[i]) {
                cnt++;
            } else {
                cnt--;
            }
        }
        
        /* Checking if the stored element
         is the majority element*/
        int cnt1 = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == el) {
                cnt1++;
            }
        }
        
        // Return element if it is a majority element
        if (cnt1 > (n / 2)) {
            return el;
        }
        
        // Return -1 if no such element found
        return -1;
    }

Complexity Analysis 
Time Complexity: O(N) + O(N), where N is size of the given array. The first O(N) is to calculate the count and find the expected majority element. The second one is to check if the expected element is the majority one or not.

Space Complexity: O(1) no extra space used.


16. Majority Element-II

 // Function to find majority elements in an array
    public List<Integer> majorityElementTwo(int[] nums) {
        // Size of the array
        int n = nums.length;
        
        // List of answers
        List<Integer> result = new ArrayList<>();
        
        for (int i = 0; i < n; i++) {
            /* Checking if nums[i] is not 
            already part of the answer */
            
            if (result.size() == 0 || result.get(0) != nums[i]) {
                
                int cnt = 0;
                
                for (int j = 0; j < n; j++) {
                    // counting the frequency of nums[i]
                    if (nums[j] == nums[i]) {
                        cnt++;
                    }
                }
                
                // check if frequency is greater than n/3
                if (cnt > (n / 3)) {
                    result.add(nums[i]);
                }
            }
            
            // if result size is equal to 2 break out of loop
            if (result.size() == 2) {
                break;
            }
        }
        
        // return the majority elements
        return result;
    }

    Complexity Analysis 
Time Complexity: O(N2), where N is the size of the array. As for every element of the array the inner loop runs for N times.

Space Complexity: O(1) the space used is so small that it can be considered constant.

// Function to find majority elements in an array
    public List<Integer> majorityElementTwo(int[] nums) {
        
        // Size of the array
        int n = nums.length;

        // List of answers
        List<Integer> result = new ArrayList<>();

        // Declaring a map
        Map<Integer, Integer> mpp = new HashMap<>();

        // Least occurrence of the majority element
        int mini = n / 3 + 1;

        // Storing the elements with its occurrence
        for (int i = 0; i < n; i++) {
            mpp.put(nums[i], mpp.getOrDefault(nums[i], 0) + 1);

            // Checking if nums[i] is the majority element
            if (mpp.get(nums[i]) == mini) {
                result.add(nums[i]);
            }

            // If result size is equal to 2 break out of loop
            if (result.size() == 2) {
                break;
            }
        }

        // Return the majority elements
        return result;
    }

    Complexity Analysis 
Time Complexity: O(N), where N is size of the given array. For using an unordered map data structure, where insertion in the map takes O(1) time and we are doing it for N elements. On using map instead, the first term will be O(N*logN) for the best and average case and for the worst case, it will be O(N2).

Space Complexity: O(N) for uing a map data structure. A list that stores a maximum of 2 elements is also used, but that space used is so small that it can be considered constant.

 // Function to find majority elements in an array
    public List<Integer> majorityElementTwo(int[] nums) {
        
        // Size of the array
        int n = nums.length;

        // Counts for elements el1 and el2
        int cnt1 = 0, cnt2 = 0;
        
        /*Initialize Element 1 and 
        Element 2 with INT_MIN value*/
        int el1 = Integer.MIN_VALUE, el2 = Integer.MIN_VALUE;

        /*Find the potential candidates using
        Boyer Moore's Voting Algorithm*/
        for (int i = 0; i < n; i++) {
            if (cnt1 == 0 && el2 != nums[i]) {
                cnt1 = 1;
                // Initialize el1 as nums[i]
                el1 = nums[i]; 
            } else if (cnt2 == 0 && el1 != nums[i]) {
                cnt2 = 1;
                // Initialize el2 as nums[i]
                el2 = nums[i]; 
            } else if (nums[i] == el1) {
                // Increment count for el1
                cnt1++;
            } else if (nums[i] == el2) {
                // Increment count for el2
                cnt2++; 
            } else {
                // Decrement count for el1
                cnt1--; 
                 // Decrement count for el2
                cnt2--;
            }
        }

        //Validate the candidates by counting occurrences in nums:
        //Reset counts for el1 and el2
        cnt1 = 0; cnt2 = 0; 
        
        for (int i = 0; i < n; i++) {
            if (nums[i] == el1) {
                // Count occurrences of el1
                cnt1++; 
            }
            if (nums[i] == el2) {
                 // Count occurrences of el2
                cnt2++;
            }
        }

        /* Determine the minimum count
        required for a majority element*/
        int mini = n / 3 + 1;
        
        // List of answers
        List<Integer> result = new ArrayList<>(); 

        /*Add elements to the result vector
        if they appear more than n/3 times*/
        if (cnt1 >= mini) {
            result.add(el1);
        }
        if (cnt2 >= mini && el1 != el2) {
            // Avoid adding duplicate if el1 == el2
            result.add(el2); 
        }

        // Uncomment the following line if you want to sort the answer array 
        // Collections.sort(result); // TC --> O(2*log2) ~ O(1);

       //return the majority elements
        return result;
    }

    Complexity Analysis 
Time Complexity: O(N) + O(N), where N is size of the given array. The first O(N) is to calculate the counts and find the expected majority elements. The second one is to check if the calculated elements are the majority ones or not.

Space Complexity: O(1) for only using a list that stores a maximum of 2 elements. The space used is so small that it can be considered constant.



17. Grid unique paths
Recursion:
 //Function to solve the problem using recursion
    private int func(int i, int j) {
        // Base case
        if (i == 0 && j == 0) return 1;

        // If we go out of bounds, there are no ways
        if (i < 0 || j < 0) return 0;

        /* Calculate the number of ways by
        moving up and left recursively*/
        int up = func(i - 1, j);
        int left = func(i, j - 1);

        // Return the total ways
        return up + left;
    }
    /*Function to count the total ways
    to reach (0,0) from (m-1,n-1)*/
    public int uniquePaths(int m, int n) {
        // Return the total count (0-based indexing)
        return func(m - 1, n - 1);
    }Time Complexity: O(2(M+N)*(M+N)), where M is the number of row and N is the number of column in 2D array. As, each cell has 2 choices and path length is near about (M+N) and each path would take (M+N) to travel as well.

Space Complexity:O((M-1)+(N-1)), In the worst case, the depth of the recursion can reach (M-1)+(N-1), corresponding to the maximum number of steps required to reduce both i and j to 0.


Memoization :
//Function to solve the problem using recursion
    private int func(int i, int j, int[][] dp) {
        // Base case
        if (i == 0 && j == 0) return 1;

        // If we go out of bounds, there are no ways
        if (i < 0 || j < 0) return 0;
        
        /* If the value for this cell 
        is already computed, return it.*/
        if (dp[i][j] != -1)
            return dp[i][j];

        /* Calculate the number of ways by
        moving up and left recursively*/
        int up = func(i - 1, j, dp);
        int left = func(i, j - 1, dp);

        /* Store the result in dp array
        and return the total ways*/
        return dp[i][j] = up + left;
    }
    /*Function to count the total ways
    to reach (0,0) from (m-1,n-1)*/
    public int uniquePaths(int m, int n) {
        // Declare a 2D DP array to store results
        int dp[][] = new int[m][n];
        
        /* Initialize the DP array with 
        -1 to indicate uncomputed values*/
        for (int[] row : dp)
            Arrays.fill(row, -1);
            
        // Return the total count (0-based indexing)
        return func(m - 1, n - 1, dp);
    }
    Complexity Analysis: 
Time Complexity: O(M*N), where M is the number of row and N is the number of column in 2D array. At max, there will be M*N calls of recursion as the subproblems can go upto M*N.

Space Complexity:O((N-1)+(M-1)) + O(M*N), We are using a recursion stack space: O((N-1)+(M-1)), here (N-1)+(M-1) is the path length and an external DP Array of size ‘M*N’.


Tabulation :

memoization -> Tabulation
1. declare base case
2. express all states in for loop
3. copy the recurrence & write

// Function to solve the problem using tabulation
    int func(int m, int n, int[][] dp) {
        // Loop through the grid using two nested loops
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // Base condition
                if (i == 0 && j == 0) {
                    dp[i][j] = 1;
                    /* Skip the rest of the loop and 
                    continue with the next iteration.*/
                    continue; 
                }

                /* Initialize variables to store the number
                of ways from cell above (up) and left (left)*/
                int up = 0;
                int left = 0;

                /* If we are not at first row (i > 0), update
                'up' with the value from the cell above*/
                if (i > 0)
                    up = dp[i - 1][j];

                /* If we are not at the first column (j > 0),
                update 'left' with value from the cell to left*/
                if (j > 0)
                    left = dp[i][j - 1];

                /* Calculate the number of ways to reach the 
                current cell by adding 'up' and 'left'*/
                dp[i][j] = up + left;
            }
        }

        // The result is stored in bottom-right cell (m-1, n-1)
        return dp[m - 1][n - 1];
    }

    /* Function to count the total ways 
    to reach (0,0) from (m-1,n-1)*/
    public int uniquePaths(int m, int n) {
        /* Initialize a memoization table (dp)
        to store the results of subproblems*/
        int[][] dp = new int[m][n];

        // Return the total count (0-based indexing)
        return func(m, n, dp);
    }

Complexity Analysis: 
Time Complexity: O(M*N), where M is the number of row and N is the number of column in 2D array. As the whole matrix is traversed once using two nested loops.

Space Complexity:O(M*N), As an external DP Array of size ‘M*N’ is used to store the intermediate calculations.


Space Optimization :
-> if there is a prevuious row & previous column, we can space optimize it
 // Function to solve the problem using space optimization
    int func(int m, int n) {
        /* Create an array to represent 
        the previous row of the grid*/
        int[] prev = new int[n];

        // Iterate through the rows of the grid
        for (int i = 0; i < m; i++) {
            /* Initialize a temporary array to
            represent the current row*/
            int[] temp = new int[n];

            for (int j = 0; j < n; j++) {
                // Base case
                if (i == 0 && j == 0) {
                    temp[j] = 1;
                    continue;
                }

                /* Initialize variables to store the number
                of ways from cell above (up) and left (left)*/
                int up = (i > 0) ? prev[j] : 0;
                int left = (j > 0) ? temp[j - 1] : 0;

                /* Calculate the number of ways to reach
                the current cell by adding 'up' and 'left'*/
                temp[j] = up + left;
            }

            /* Update the previous array with values
            calculated for the current row*/
            prev = temp;
        }

        /* The result is stored in the last
        cell of the previous row (n-1)*/
        return prev[n - 1];
    }

    /* Function to count the total ways
    to reach (0,0) from (m-1,n-1)*/
    public int uniquePaths(int m, int n) {
        // Return the total count (0-based indexing)
        return func(m, n);
    }

    Complexity Analysis: 
Time Complexity: O(M*N), where M is the number of row and N is the number of column in 2D array. As the whole matrix is traversed once using two nested loops.

Space Complexity:O(N), We are using an external array of size ‘N’ to store only one row.


18. Reverse Pairs
Brute Force:

 /* Function to count reverse
    pairs where a[i] > 2 * a[j]*/
    public int reversePairs(int[] nums) {
        
        // Call countPairs with the array and its length
        return countPairs(nums, nums.length); 
        
    }

    /* Helper function to count pairs
    satisfying the condition a[i] > 2 * a[j]*/
    private int countPairs(int[] nums, int n) {
        
        // Initialize count of reverse pairs
        int cnt = 0;
        
        /* Nested loops to check each
        pair (i, j) where i < j*/
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                
                /* Check if the condition 
                a[i] > 2 * a[j] holds*/
                if ((long)nums[i] > (long)2 * nums[j]) {
                    
                    /* Increment count if
                    condition is satisfied*/
                    cnt++; 
                    
                }
            }
        }
        // Return the total count of reverse pairs
        return cnt; 
    }
    Complexity Analysis 
Time Complexity: O(N2), where N is size of the given array. For using nested loops here and those two loops roughly run for N times.

Space Complexity: O(1), no extra space is used to solve this problem.

class Solution {
    public int reversePairs(int[] nums) {
        return mergeSort(nums, 0, nums.length - 1);
    }

    // Merge sort function
    private int mergeSort(int[] nums, int low, int high) {
        if (low >= high) return 0;

        int mid = (low + high) / 2;
        int cnt = 0;

        cnt += mergeSort(nums, low, mid);
        cnt += mergeSort(nums, mid + 1, high);
        cnt += countPairs(nums, low, mid, high);
        merge(nums, low, mid, high);

        return cnt;
    }

    // Count reverse pairs
    private int countPairs(int[] nums, int low, int mid, int high) {
        int right = mid + 1, cnt = 0;

        for (int i = low; i <= mid; i++) {
            while (right <= high && (long) nums[i] > 2L * nums[right]) {
                right++;
            }
            cnt += (right - (mid + 1));
        }

        return cnt;
    }

    // Merge two sorted halves
    private void merge(int[] nums, int low, int mid, int high) {
        List<Integer> temp = new ArrayList<>();
        int left = low, right = mid + 1;

        while (left <= mid && right <= high) {
            if (nums[left] <= nums[right]) {
                temp.add(nums[left++]);
            } else {
                temp.add(nums[right++]);
            }
        }

        while (left <= mid) temp.add(nums[left++]);
        while (right <= high) temp.add(nums[right++]);

        for (int i = low; i <= high; i++) {
            nums[i] = temp.get(i - low);
        }
    }
}
Time Complexity: O(2N * logN), where N is size of the given array.
Inside the mergeSort() we call merge() and countPairs() except mergeSort() itself. Now, inside the function countPairs(), though we are running a nested loop, we are actually iterating the left half once and the right half once in total.
That is why, the time complexity is O(N). And the merge() function also takes O(N). The mergeSort() takes O(logN) time complexity. Therefore, the overall time complexity will be O(logN x (N+N)) = O(2NxlogN)

Space Complexity: O(N), as in the merge sort, a temporary array to store elements in sorted order is used.


19. Two Sum

20. 4 Sum

21. Longest Consecutive Sequence in an Array

22. Largest Subarray with K sum

23. Count subarrays with given xor K

24. Longest Substring Without Repeating Characters
