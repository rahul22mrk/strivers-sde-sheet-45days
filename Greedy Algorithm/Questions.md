1. `N meetings in one room`
2. `Minimum number of platforms required for a railway`
3. `Job sequencing Problem`
4. `Fractional Knapsack`
5. `Minimum coins`
6. `Assign Cookies`

1. `N meetings in one room`
```java
// Comparator function to sort meetings based on end times
    static class MeetingComparator implements Comparator<int[]> {
        public int compare(int[] a, int[] b) {
            // Sort by end time in ascending order
            return Integer.compare(a[1], b[1]);
        }
    }

    // Function to find the maximum number of meetings that can be held
    public int maxMeetings(int[] start, int[] end) {
        int n = start.length;
        // List to store meetings
        List<int[]> meetings = new ArrayList<>();
        
        // Fill the meetings list with start and end times
        for (int i = 0; i < n; i++) {
            meetings.add(new int[]{start[i], end[i]});
        }

        // Sort the meetings based on the custom comparator
        Collections.sort(meetings, new MeetingComparator());

        // The end time of last selected meeting
        int limit = meetings.get(0)[1];
        // Initialize count
        int count = 1;

        /*Iterate through the meetings 
        to select the maximum number 
        of non-overlapping meetings*/
        for (int i = 1; i < n; i++) {
            /*If the current meeting starts 
            after the last selected meeting ends*/
            if (meetings.get(i)[0] > limit) {
                /*Update the limit to the end 
                time of the current meeting*/
                limit = meetings.get(i)[1];
                // Increment count
                count++;
            }
        }

        // Return count
        return count;
    }
Complexity Analysis
Time Complexity: O(N+N logN) where 𝑁 is the size of the start and end arrays. The O(N) term accounts for filling the meetings array with start and end times. The O(NlogN) term arises from sorting the meetings based on their end times. After sorting, the function iterates through the sorted meetings in O(N) time to count the maximum number of non-overlapping meetings.
Space Complexity: O(N) since we used an additional data structure for storing the start time and end time.
```

2. `Minimum number of platforms required for a railway`
```java
 // Function to find minimum number of platforms required
    public int findPlatform(int[] Arrival, int[] Departure) {
        int n = Arrival.length;

        // To store the result
        int ans = 1;

        // Iterate on the trains platforms
        for (int i = 0; i < n; i++) {
            
            int count = 1;

            // Iterate on all the trains 
            for (int j = 0; j < n; j++) {
                // Check with other trains
                if (i != j) {

                    // Check for the overlapping trains 
                    if ((Arrival[i] >= Arrival[j] && Departure[j] >= Arrival[i])) {
                        // Increment count
                        count++;
                    }

                    // Update the minimum platforms needed
                    ans = Math.max(ans, count);
                }
            }
        }

        // Return number of platforms
        return ans;
    }
Complexity Analysis
Time Complexity: O(N2) where N is the number of trains.
The two nested loops result in quadratic time complexity.
Space Complexity : O(1) because no extra space is used.
```

```java

//  To find number of platforms
    public int findPlatform(int[] Arrival, int[] Departure) {
        int n = Arrival.length;

        // Sort both arrival and departure arrays
        Arrays.sort(Arrival);
        Arrays.sort(Departure);

        int ans = 1;
        int count = 1;
        int i = 1, j = 0;

        // Iterate through the arrays
        while (i < n && j < n) {
            if (Arrival[i] <= Departure[j]) {
                // Increment count
                count++;
                i++;
            } else {
                // Decrement count
                count--;
                j++;
            }
            // Find maximum
            ans = Math.max(ans, count);
        }
        return ans;
    }

Complexity Analysis
Time Complexity: O(N log N) where N is the size of each array.This is primarily due to the sorting operations on the arrival and departure arrays, each taking O(N log N). The subsequent traversal of the arrays using the two-pointer technique takes O(N), but this does not affect the overall complexity, which is dominated by the sorting step. Therefore, the combined time complexity remains O(N log N).
Space Complexity: O(1) as no extra space is used.
```

3. `Job sequencing Problem`
```java
 // Function to calculate maximum profit
    public int[] JobScheduling(int[][] Jobs) {
        // Sort jobs based on profit in descending order
        Arrays.sort(Jobs, (a, b) -> b[2] - a[2]);

        // Total number of jobs
        int n = Jobs.length;

        // Get the maximum deadline to complete the jobs
        int maxDeadline = -1;
        for (int[] it : Jobs) {
            maxDeadline = Math.max(maxDeadline, it[1]);
        }

        // Initialize a hash table to store selected jobs
        int[] hash = new int[maxDeadline];
        Arrays.fill(hash, -1);

        // Initialize count
        int cnt = 0;

        // Initialize the total profit earned
        int totalProfit = 0;

        // Iterate over each job
        for (int i = 0; i < n; i++) {

            /* Iterate over each deadline slot 
            starting from the job's deadline */
            for (int j = Jobs[i][1] - 1; j >= 0; j--) {

                // If the current deadline slot is available 
                if (hash[j] == -1) {
                    cnt++; // Count of selected jobs
                    hash[j] = Jobs[i][0]; // Mark the job as selected
                    totalProfit += Jobs[i][2]; // Update the total profit

                    // Move to the next job
                    break;
                }
            }
        }

        // Return the array
        return new int[]{cnt, totalProfit};
    }
}
Complexity Analysis
Time Complexity: O(N logN + N2) where N is the number of jobs. First, the jobs are sorted based on profit in descending order, resulting in O(N logN) complexity. Then, the algorithm iterates over the jobs to select them. The outer loop runs once for each job (N iterations), and the inner loop iterates up to the job’s deadline, which can be at most N in the worst case, giving a complexity of O(N2).
Space Complexity: O(N) where N is the number of jobs. An array of size N is used to keep track of occupied slots taking O(N) space.
```

4. `Fractional Knapsack`
```java
 public double fractionalKnapsack(int[] val, int[] wt, long cap) {
        int n = val.length;

        // Create array of (ratio, index)
        double[][] ratio = new double[n][2];
        for (int i = 0; i < n; i++)
            ratio[i] = new double[]{(double) val[i] / wt[i], i};

        // Sort in descending order of ratio
        Arrays.sort(ratio, (a, b) -> Double.compare(b[0], a[0]));

        double totalValue = 0.0;
        for (double[] r : ratio) {
            int i = (int) r[1];
            if (wt[i] <= cap) {
                totalValue += val[i];
                cap -= wt[i];
            } else {
                totalValue += val[i] * ((double) cap / wt[i]);
                break;
            }
        }

        return Math.round(totalValue * 1e6) / 1e6;
    }
Complexity Analysis
Time Complexity: O(n log n), for sorting items by ratio.

Space Complexity: O(n), to store the ratio and index pairs.

```

5. `Minimum coins`
```java
//Brute force
 final int mod = (int)1e9 + 7;

    /* Function to calculate the minimum number
    of elements to form the target sum */
    private int func(int[] arr, int ind, int T) {
        // Base case: If we're at the first element
        if (ind == 0) {
            /* Check if the target sum is
            divisible by the first element */
            if (T % arr[0] == 0)
                return T / arr[0];
            else
                /* Otherwise, return a very large
                value to indicate it's not possible */
                return (int)1e9;
        }

        /* Calculate the minimum elements needed
        without taking the current element */
        int notTaken = func(arr, ind - 1, T);

        /* Calculate the minimum elements
        needed by taking the current element */
        int taken = (int)1e9;
        if (arr[ind] <= T)
            taken = 1 + func(arr, ind, T - arr[ind]);

        // Return minimum of 'notTaken' and 'taken' 
        return Math.min(notTaken, taken);
    }

    /* Function to find the minimum number
    of coins needed to form the target sum */
    public int minimumCoins(int[] coins, int amount) {
        int n = coins.length;

        // Call utility function to calculate the answer
        int ans = func(coins, n - 1, amount);

        /* If 'ans' is still very large, it means
        it's not possible to form the target sum */
        if (ans >= (int)1e9)
            return -1;

        // Return the minimum number of coins
        return ans;
    }
Complexity Analysis:
Time Complexity:O(2N), as each element has 2 choices, and there are N elements in the array.

Space Complexity:O(N), the stack space will be O(N), the maximum depth of the stack.
```

```java
//Memoization
 final int mod = (int)1e9 + 7;

    /* Function to calculate the minimum number
    of elements to form the target sum */
    private int func(int[] arr, int ind, int T, int[][] dp) {
        // Base case: If we're at the first element
        if (ind == 0) {
            /* Check if the target sum is
            divisible by the first element */
            if (T % arr[0] == 0)
                return T / arr[0];
            else
                /* Otherwise, return a very large
                value to indicate it's not possible */
                return (int)1e9;
        }

        /* If the result for this index and target
        sum is already calculated, return it */
        if (dp[ind][T] != -1)
            return dp[ind][T];

        /* Calculate the minimum elements needed
        without taking the current element */
        int notTaken = func(arr, ind - 1, T, dp);

        /* Calculate the minimum elements
        needed by taking the current element */
        int taken = (int)1e9;
        if (arr[ind] <= T)
            taken = 1 + func(arr, ind, T - arr[ind], dp);

        /* Store the minimum of 'notTaken' and
        'taken' in the DP array and return it */
        return dp[ind][T] = Math.min(notTaken, taken);
    }

    /* Function to find the minimum number
    of coins needed to form the target sum */
    public int minimumCoins(int[] coins, int amount) {
        int n = coins.length;

        /* Create a DP (Dynamic Programming) table with
        n rows and amount+1 columns and initialize it with -1 */
        int[][] dp = new int[n][amount + 1];
        for (int[] row : dp)
            Arrays.fill(row, -1);

        // Call utility function to calculate the answer
        int ans = func(coins, n - 1, amount, dp);

        /* If 'ans' is still very large, it means
        it's not possible to form the target sum */
        if (ans >= (int)1e9)
            return -1;

        // Return the minimum number of coins
        return ans;
    }
Complexity Analysis:
Time Complexity:O(N*Target), as there are N*Target states therefore at max ‘N*Target’ new problems will be solved.

Space Complexity:O(N*Target) + O(N), the stack space will be O(N) and a 2D array of size N*T is used.
```

```java
//Tabulation
 /* Function to find the minimum number 
    of elements needed to form the target sum */
    public int minimumCoins(int[] coins, int amount) {
        int n = coins.length;

        /* Create a 2D DP table with
        n rows and amount+1 columns */
        int[][] dp = new int[n][amount + 1];

        // Initialize the first row of the DP table
        for (int i = 0; i <= amount; i++) {
            if (i % coins[0] == 0)
                dp[0][i] = i / coins[0];
            else
                dp[0][i] = (int)1e9;
        }

        // Fill the DP table using a bottom-up approach
        for (int ind = 1; ind < n; ind++) {
            for (int target = 0; target <= amount; target++) {
                /* Calculate the minimum elements needed
                without taking the current element */
                int notTake = dp[ind - 1][target];

                /* Calculate the minimum elements 
                needed by taking the current element */
                int take = (int)1e9;
                if (coins[ind] <= target)
                    take = 1 + dp[ind][target - coins[ind]];

                /* Store the minimum of 'notTake'
                and 'take' in the DP table */
                dp[ind][target] = Math.min(notTake, take);
            }
        }

        // The answer is in the bottom-right cell 
        int ans = dp[n - 1][amount];

        /* If 'ans' is still very large, it means 
        it's not possible to form the target sum */
        if (ans >= (int)1e9)
            return -1;

        // Return the minimum number of coins needed
        return ans;
    }
Complexity Analysis:
Time Complexity:O(N*Target), as there are N*Target states therefore at max ‘N*Target’ new problems will be solved.

Space Complexity:O(N*Target), as a 2D array of size N*Target is used.
```

```java
//Space Optimnization
 /* Function to find the minimum number 
    of elements needed to form the target sum */
    public int minimumCoins(int[] coins, int amount) {
        int n = coins.length;

        /* Initialize two arrays to store 
        the previous and current DP states */
        int[] prev = new int[amount + 1];
        int[] cur = new int[amount + 1];

        // Initialize the first row of the DP table
        for (int i = 0; i <= amount; i++) {
            if (i % coins[0] == 0)
                prev[i] = i / coins[0];
            // Set it to a very large value if not possible
            else
                prev[i] = (int)1e9;
        }

        // Fill the DP table using a bottom-up approach
        for (int ind = 1; ind < n; ind++) {
            for (int target = 0; target <= amount; target++) {
                /* Calculate the minimum elements needed
                without taking the current element */
                int notTake = prev[target];

                /* Calculate the minimum elements 
                needed by taking the current element */
                int take = (int)1e9;
                if (coins[ind] <= target)
                    take = 1 + cur[target - coins[ind]];

                /* Store the minimum of 'notTake' 
                and 'take' in the current DP state */
                cur[target] = Math.min(notTake, take);
            }
            /* Update the previous DP state with
            the current state for the next iteration */
            System.arraycopy(cur, 0, prev, 0, amount + 1);
        }

        // The answer is in the last row of the DP table
        int ans = prev[amount];

        /* If 'ans' is still very large, it means 
        it's not possible to form the target sum */
        if (ans >= (int)1e9)
            return -1;

        // Return the minimum number of coins
        return ans;
    }
Complexity Analysis:
Time Complexity:O(N*Target), as there are N*Target states therefore at max ‘N*Target’ new problems will be solved.

Space Complexity:O(Target), as a 2D array of size (Target+1) is used.
```

6. `Assign Cookies`
```java
public int findMaximumCookieStudents(int[] Student, int[] Cookie) {
        int n = Student.length;
        int m = Cookie.length;
        // Pointers
        int l = 0, r = 0;
        // Sorting of arrays
        Arrays.sort(Student);
        Arrays.sort(Cookie);

        // Traverse through both arrays
        while (l < n && r < m) {
            /*If the current cookie can satisfy 
            the current student, move to the 
            next student*/
            if (Cookie[r] >= Student[l]) {
                l++;
            }
            // Move to next cookie
            r++;
        }
        // Return number of students
        return l; 
    }

Complexity Analysis
Time Complexity: O(N logN + M logM + min(N, M)) where N is the length of the student array, and M is the length of the cookies array.

Sorting the student array takes O(N logN) time, and sorting the cookies array takes O(M logM) time. After sorting, both arrays are traversed simultaneously using two pointers, each moving at most once, leading to O(min(N, M)) iterations.
Therefore, the total time complexity is O(N logN + M logM + min(N, M)).

Space Complexity: O(1) no extra space used.
```
