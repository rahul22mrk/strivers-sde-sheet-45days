1. `The N-th root of an integer `
2. `Matrix Median `
3. `Single element in sorted array `
4. `Search element in a sorted and rotated array/ find pivot where it is rotated `
5. `Median of 2 sorted arrays `
6. `Kth element of 2 sorted arrays `
7. `Allocate Minimum Number of Pages `
8. `Aggressive Cows `

********************************************************************************************

1. `The N-th root of an integer `
```java
//Linear Search

 /* Function to calculate power using
    exponentiation by squaring method */
    private long Pow(int b, int exp) {
        long ans = 1;
        long base = b;

        // Exponentiation by squaring method
        while (exp > 0) {
            if (exp % 2 == 1) {
                exp--;
                ans *= base;
            } else {
                exp /= 2;
                base *= base;
            }
        }
        return ans;
    }

    /* Function to find the nth 
    root of m using linear search */
    public int NthRoot(int N, int M) {
        // Linear search on the answer space
        for (int i = 1; i <= M; i++) {
            long val = Pow(i, N);

            /* Check if the computed 
            value is equal to M*/
            if (val == M) {
                // Return the root value
                return i;
            } else if (val > M) {
                break;
            }
        }
        // Return -1 if no root found
        return -1;
    }
Complexity Analysis:
Time Complexity: O(N*logN)
The for loop runs takes O(M) time and calculating Pow(x, N) takes O(logN) time. However, since the for loop breaks as soon as the result of Pow(x, N) becomes greater than the M, thus, the for loops actually runs only for N iterations making overall complexity as O(N*logN).

Space Complexity: O(1), as there are only a couple of variables used.
```
```java
//Binary Search
 // Helper function to check mid^N compared to M
    private int helperFunc(int mid, int n, int m) {
        long ans = 1, base = mid;
        
        while (n > 0) {
            if (n % 2 == 1) {
                ans *= base;
                if (ans > m) return 2;  // Early exit
                n--;
            } else {
                n /= 2;
                base *= base;
                if (base > m) return 2;
            }
        }
        if (ans == m) return 1;
        return 0;
    }

    // Function to find the Nth root of M using Binary Search
    public int NthRoot(int N, int M) {
        int low = 1, high = M;
        
        while (low <= high) {
            int mid = (low + high) / 2;
            int midN = helperFunc(mid, N, M);
            
            if (midN == 1) return mid; // Found exact root
            else if (midN == 0) low = mid + 1; // Move right
            else high = mid - 1; // Move left
        }
        return -1; // No integer root found
    }
Complexity Analysis:
Time Complexity: O(logM * logN)
The binary search on the search space (of size M) takes O(logM) and the helper function takes O(logN) taking overall O(logM * logN).

Space Complexity: O(1), as there are only a couple of variables used.
```

2. `Matrix Median `
```java
//Brute:
// Function to find the median of a row-wise sorted matrix
    public int findMedian(int[][] matrix) {
        // Step 1: Flatten the matrix into a list
        List<Integer> flattened = new ArrayList<>();
        for (int[] row : matrix) {
            for (int val : row) {
                flattened.add(val);
            }
        }

        // Step 2: Sort the list
        Collections.sort(flattened);

        // Step 3: Return the middle element
        int n = flattened.size();
        return flattened.get(n / 2);
    }

 Complexity Analysis: 
Time Complexity: O(n * m * log(n * m)), as Flattening the matrix takes O(n * m) time and Sorting the flattened list takes O(n * m * log(n * m)) time.

Space Complexity: O(n * m), Extra space is required to store the flattened list of matrix elements.   
```
```java
//Optional

 // Function to find the upper bound of an element in a sorted row
    private int upperBound(int[] arr, int x, int m) {
        int low = 0, high = m - 1;
        int ans = m;

        // Apply binary search
        while (low <= high) {
            int mid = (low + high) / 2;

            // If arr[mid] > x, it can be a possible upper bound
            if (arr[mid] > x) {
                ans = mid;
                // Look for a smaller upper bound on the left
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }

        return ans;
    }

    // Function to count how many elements in the matrix are less than or equal to x
    private int countSmallEqual(int[][] matrix, int n, int m, int x) {
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            cnt += upperBound(matrix[i], x, m);
        }
        return cnt;
    }

    // Function to find the median element in the matrix
    public int findMedian(int[][] matrix) {
        int n = matrix.length; // Number of rows
        int m = matrix[0].length; // Number of columns

        int low = Integer.MAX_VALUE, high = Integer.MIN_VALUE;

        // Initialize low and high
        for (int i = 0; i < n; i++) {
            low = Math.min(low, matrix[i][0]);
            high = Math.max(high, matrix[i][m - 1]);
        }

        int req = (n * m) / 2;

        // Perform binary search to find the median
        while (low <= high) {
            int mid = low + (high - low) / 2;

            int smallEqual = countSmallEqual(matrix, n, m, mid);

            if (smallEqual <= req) low = mid + 1;
            else high = mid - 1;
        }

        return low;
    }
Complexity Analysis: 
Time Complexity: O(log(max - min + 1)) * O(N * logM), where N is the number of rows in the matrix and M is the number of columns in each row.

Our search space ranges from [min, max], where min is the minimum element and max is the maximum element of the matrix. Binary search is applied within this value range, which operates with a time complexity of O(log(max - min + 1)). Then, the countSmallEqual() function is called for each mid, which takes O(N * logM) time because it performs binary search on each of the N sorted rows.

Therefore, the overall time complexity is O(N * logM * log(max - min + 1)).
```

3. `Single element in sorted array `
```java
//Brute-1

 /* Function to find the single non
    duplicate element in a sorted array */
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length; // Size of the array.
        
        /* If array has only one element
           return it immediately.*/
        if (n == 1) return nums[0];

        /* Traverse through the array to find 
           the single non-duplicate element.*/
        for (int i = 0; i < n; i++) {
            // Check for the first index.
            if (i == 0) {
                if (nums[i] != nums[i + 1])
                    return nums[i];
            }
            // Check for the last index.
            else if (i == n - 1) {
                if (nums[i] != nums[i - 1])
                    return nums[i];
            }
            // Check for any other index.
            else {
                if (nums[i] != nums[i - 1] && nums[i] != nums[i + 1])
                    return nums[i];
            }
        }

        /* Dummy return statement,
           should never reach here.*/
        return -1;
    }

Complexity Analysis: 
Time Complexity:O(N), where N is size of the array. As the array is being traversed only once using a single loop.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```
```java
//Brute-II
/* Function to find the single non
       duplicate element in a sorted array */
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length; // Size of the array.
        
        /* XOR all the elements to find 
           the single non-duplicate element. */
        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans ^= nums[i];
        }
        
        /* Return the single non 
           duplicate element found. */
        return ans;
    }

Complexity Analysis: 
Time Complexity:O(N), where N is size of the array. As the array is being traversed only once using a loop.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```

```java
//OPtimal

/* Function to find the single non 
       duplicate element in a sorted array */
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length; // Size of the array.

        // Edge cases:
        if (n == 1) return nums[0];
        if (nums[0] != nums[1]) return nums[0];
        if (nums[n - 1] != nums[n - 2]) return nums[n - 1];

        int low = 1, high = n - 2;
        while (low <= high) {
            int mid = (low + high) / 2;

            // If nums[mid] is the single element:
            if (nums[mid] != nums[mid + 1] && nums[mid] != nums[mid - 1]) {
                return nums[mid];
            }

            // We are in the left part:
            if ((mid % 2 == 1 && nums[mid] == nums[mid - 1])
                || (mid % 2 == 0 && nums[mid] == nums[mid + 1])) {
                // Eliminate the left half:
                low = mid + 1;
            }
            // We are in the right part:
            else {
                // Eliminate the right half:
                high = mid - 1;
            }
        }

        // Dummy return statement:
        return -1;
    }
Complexity Analysis: 
Time Complexity:O(logN), N is size of the given array. We are basically using the Binary Search algorithm.

Space Complexity: As no additional space is used, so the Space Complexity is O(1)
```

4. `Search element in a sorted and rotated array/ find pivot where it is rotated `
```java
//Linerar Search

// Function to search for the target element in the array
    public int search(int[] nums, int target) {
        int n = nums.length;

        // Loop through the array to find the target element
        for (int i = 0; i < n; i++) {
            // Check if the current element is the target
            if (nums[i] == target)
                // Return the index if the target is found
                return i;
        }

        // Return -1 if the target is not found
        return -1;
    }
Complexity Analysis: 
Time Complexity: O(N), N = size of the given array. Since we have to iterate through the entire array to check if the target is present in the array.

Space Complexity: O(1), as we have not used any extra data structures. This makes space complexity, even in the worst case, O(1).    
```

```java
//Binary Search

 // Function to search for the target element in a rotated sorted array
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;

        // Applying binary search algorithm 
        while (low <= high) {
            int mid = (low + high) / 2;

            // Check if mid points to the target
            if (nums[mid] == target) return mid;

            // Check if the left part is sorted
            if (nums[low] <= nums[mid]) {
                if (nums[low] <= target && target <= nums[mid]) {
                    // Target exists in the left sorted part
                    high = mid - 1;
                } else {
                    // Target does not exist in the left sorted part
                    low = mid + 1;
                }
            } else {
                // Check if the right part is sorted
                if (nums[mid] <= target && target <= nums[high]) {
                    // Target exists in the right sorted part
                    low = mid + 1;
                } else {
                    // Target does not exist in the right sorted part
                    high = mid - 1;
                }
            }
        }
        // If target is not found
        return -1;
    }

Complexity Analysis: 
Time Complexity: O(logN), as the search space is reduced logarithmically, where N is the size of the given array.

Space Complexity: O(1), not using any extra data structure.
```


5. `Median of 2 sorted arrays `
```java
//Brute:
 //Function to find the median of two sorted arrays.
    public double median(int[] arr1, int[] arr2) {
        // Size of two given arrays
        int n1 = arr1.length, n2 = arr2.length;

        int[] merged = new int[n1 + n2];
        // Apply the merge step
        int i = 0, j = 0, k = 0;
        while (i < n1 && j < n2) {
            if (arr1[i] < arr2[j]) merged[k++] = arr1[i++];
            else merged[k++] = arr2[j++];
        }

        // Copy the remaining elements
        while (i < n1) merged[k++] = arr1[i++];
        while (j < n2) merged[k++] = arr2[j++];

        // Find the median
        int n = n1 + n2;
        if (n % 2 == 1) {
            return (double) merged[n / 2];
        }

        double median = ((double) merged[n / 2] + (double) merged[(n / 2) - 1]) / 2.0;
        return median;
    }
Complexity Analysis: 
Time Complexity:O(N1+N2), where N1 and N2 are the sizes of the given arrays. As, both are arrays are being traversed linearly.

Space Complexity:O(N1+N2), As, an extra array of size (N1+N2) is being used to solve this problem.
```

```java
//Better:
//Function to find the median of two sorted arrays.
    public double median(int[] arr1, int[] arr2) {
        // Size of two given arrays
        int n1 = arr1.length, n2 = arr2.length;
        int n = n1 + n2; // Total size

        // Required indices for median calculation
        int ind2 = n / 2;
        int ind1 = ind2 - 1;
        int cnt = 0;
        int ind1el = -1, ind2el = -1;

        // Apply the merge step
        int i = 0, j = 0;
        while (i < n1 && j < n2) {
            if (arr1[i] < arr2[j]) {
                if (cnt == ind1) ind1el = arr1[i];
                if (cnt == ind2) ind2el = arr1[i];
                cnt++;
                i++;
            } else {
                if (cnt == ind1) ind1el = arr2[j];
                if (cnt == ind2) ind2el = arr2[j];
                cnt++;
                j++;
            }
        }

        // Copy the remaining elements
        while (i < n1) {
            if (cnt == ind1) ind1el = arr1[i];
            if (cnt == ind2) ind2el = arr1[i];
            cnt++;
            i++;
        }
        while (j < n2) {
            if (cnt == ind1) ind1el = arr2[j];
            if (cnt == ind2) ind2el = arr2[j];
            cnt++;
            j++;
        }

        // Find the median
        if (n % 2 == 1) {
            return (double) ind2el;
        }

        return (double) ((double) (ind1el + ind2el)) / 2.0;
    }
Complexity Analysis: 
Time Complexity:O(N1+N2), where N1 and N2 are the sizes of the given arrays. As, both are arrays are being traversed linearly.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```

```java
//Optimal:
 //Function to find the median of two sorted arrays.
    public double median(int[] arr1, int[] arr2) {
        // Size of two given arrays
        int n1 = arr1.length, n2 = arr2.length;

        /* Ensure arr1 is not larger than 
        arr2 to simplify implementation*/
        if (n1 > n2) return median(arr2, arr1);

        int n = n1 + n2;
        
        // Length of left half
        int left = (n1 + n2 + 1) / 2; 

        // Apply binary search
        int low = 0, high = n1;
        while (low <= high) {
            
            // Calculate mid index for arr1
            int mid1 = (low + high) >>> 1; 
            
            // Calculate mid index for arr2
            int mid2 = left - mid1; 

            // Calculate l1, l2, r1, and r2
            int l1 = (mid1 > 0) ? arr1[mid1 - 1] : Integer.MIN_VALUE;
            int r1 = (mid1 < n1) ? arr1[mid1] : Integer.MAX_VALUE;
            int l2 = (mid2 > 0) ? arr2[mid2 - 1] : Integer.MIN_VALUE;
            int r2 = (mid2 < n2) ? arr2[mid2] : Integer.MAX_VALUE;

            if (l1 <= r2 && l2 <= r1) {
                // If condition for finding median
                if (n % 2 == 1) return Math.max(l1, l2);
                else return (Math.max(l1, l2) + Math.min(r1, r2)) / 2.0;
            } 
            else if (l1 > r2) {
                // Eliminate the right half of arr1
                high = mid1 - 1;
            } else {
                // Eliminate the left half of arr1
                low = mid1 + 1;
            }
        }
        // Dummy statement
        return 0; 
    }
Complexity Analysis: 
Time Complexity: O(log(min(N1,N2))), where N1 and N2 are the sizes of two given arrays. As, binary search is being applied on the range [0, min(N1, N2)]

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```

6. `Kth element of 2 sorted arrays `
```java
public int kthElement(int[] a, int[] b, int k) {
        int m = a.length;
        int n = b.length;

        // Ensure a is smaller array for optimization
        if (m > n) {
            // Swap a and b
            return kthElement(b, a, k); 
        }
        
        // Length of the left half
        int left = k; 

        // Apply binary search
        int low = Math.max(0, k - n), high = Math.min(k, m);
        while (low <= high) {
            int mid1 = (low + high) >> 1;
            int mid2 = left - mid1;

            // Initialize l1, l2, r1, r2
            int l1 = (mid1 > 0) ? a[mid1 - 1] : Integer.MIN_VALUE;
            int l2 = (mid2 > 0) ? b[mid2 - 1] : Integer.MIN_VALUE;
            int r1 = (mid1 < m) ? a[mid1] : Integer.MAX_VALUE;
            int r2 = (mid2 < n) ? b[mid2] : Integer.MAX_VALUE;

            // Check if we have found the answer
            if (l1 <= r2 && l2 <= r1) {
                return Math.max(l1, l2);
            } 
            else if (l1 > r2) {
                // Eliminate the right half
                high = mid1 - 1;
            } 
            else {
                // Eliminate the left half
                low = mid1 + 1;
            }
        }
        
         // Dummy return statement 
        return -1;
    }
Complexity Analysis: 
Time Complexity:O(log(min(M, N))), where M and N are the sizes of two given arrays. As, binary search is being applied on the range [max(0, k-N2), min(k, N1)]. The range length <= min(M, N).

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```


7. `Allocate Minimum Number of Pages `
```java
//Linear Search:
 /*Function to count the number of 
    students required given the maximum 
    pages each student can read*/
    private int countStudents(int[] nums, int pages) {
        // Size of array
        int n = nums.length;
        
        int students = 1;
        int pagesStudent = 0;
        
        for (int i = 0; i < n; i++) {
            if (pagesStudent + nums[i] <= pages) {
                // Add pages to current student
                pagesStudent += nums[i];
            } else {
                // Add pages to next student
                students++;
                pagesStudent = nums[i];
            }
        }
        return students;
    }
    
    /* Function to allocate the book to ‘m’ 
    students such that the maximum number 
    of pages assigned to a student is minimum*/
    public int findPages(int[] nums, int m) {
        int n = nums.length;
        
        // Book allocation impossible
        if (m > n) return -1;

        // Calculate the range for search
        int low = Integer.MIN_VALUE;
        int high = 0;
        for(int i = 0; i < n; i++){
            low = Math.max(low, nums[i]);
            high = high + nums[i];
        }

        // Linear search for minimum maximum pages
        for (int pages = low; pages <= high; pages++) {
            if (countStudents(nums, pages) <= m) {
                return pages;
            }
        }
        return low;
    }

Complexity Analysis: 
Time Complexity:O(N * (sum-max)), where N is size of the array, sum is the sum of all array elements, max is the maximum of all array elements.
As the loop runs from max to sum to check all possible numbers of pages. Inside the loop, the countStudents() function is being called for each number, and the loop inside this runs for N times.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```

```java
//Binary Search:

 /*Function to count the number of 
    students required given the maximum 
    pages each student can read*/
    private int countStudents(int[] nums, int pages) {
        // Size of array
        int n = nums.length;
        
        int students = 1;
        int pagesStudent = 0;
        
        for (int i = 0; i < n; i++) {
            if (pagesStudent + nums[i] <= pages) {
                // Add pages to current student
                pagesStudent += nums[i];
            } else {
                // Add pages to next student
                students++;
                pagesStudent = nums[i];
            }
        }
        return students;
    }
    
    /* Function to allocate the book to ‘m’ 
    students such that the maximum number 
    of pages assigned to a student is minimum*/
    public int findPages(int[] nums, int m) {
        int n = nums.length;
        
        // Book allocation impossible
        if (m > n) return -1;

        // Calculate the range for search
        int low = Integer.MIN_VALUE;
        int high = 0;
        for(int i = 0; i < n; i++){
            low = Math.max(low, nums[i]);
            high = high + nums[i];
        }

        // Binary search for minimum maximum pages
        while (low <= high) {
            int mid = (low + high) / 2;
            int students = countStudents(nums, mid);
            if (students > m) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }
        return low;
    }

Complexity Analysis: 
Time Complexity:O(N * log(sum-max)), where N is size of the array, sum is the sum of all array elements, max is the maximum of all array elements.
As, binary search is being applied on [max, sum]. Inside the loop, we are calling the countStudents() function for the value of ‘mid’. Now, inside the countStudents() function, the loop runs for N times.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```


8. `Aggressive Cows `
```java
//Linear Search:
 /* Function to check if we can place 'cows' cows
    with at least 'dist' distance apart */
    private boolean canWePlace(int[] nums, int dist, int cows) {
        // Size of array
        int n = nums.length;

        // Number of cows placed
        int cntCows = 1;

        // Position of last placed cow
        int last = nums[0];
        
        for (int i = 1; i < n; i++) {
            if (nums[i] - last >= dist) {
                // Place next cow
                cntCows++;

                // Update the last location
                last = nums[i];
            }
            if (cntCows >= cows) return true;
        }
        return false;
    }

    /* Function to find the maximum possible minimum
    distance 'k' cows can have between them in nums */
    public int aggressiveCows(int[] nums, int k) {
        // Size of array
        int n = nums.length;
        // Sort the nums
        Arrays.sort(nums);

        int limit = nums[n - 1] - nums[0];
        for (int i = 1; i <= limit; i++) {
            if (!canWePlace(nums, i, k)) {
                return (i - 1);
            }
        }
        // Return the answer
        return limit;
    }

Complexity Analysis: 
Time Complexity:O(NlogN) + O(N *(max-min)), where N is size of the array, max is the maximum element in array, min is the minimum element in array.
O(NlogN) for sorting the array. The loop runs for 1 to (max-min) to check all possible distances. Inside the loop, canWePlace() function is being called for each distance. Now, inside the canWePlace() function, the loop runs for N times.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```

```java
//Binary Search:
 /* Function to check if we can place 'cows' cows
    with at least 'dist' distance apart */
    private boolean canWePlace(int[] nums, int dist, int cows) {
        // Size of array
        int n = nums.length;

        // Number of cows placed
        int cntCows = 1;

        // Position of last placed cow
        int last = nums[0];
        
        for (int i = 1; i < n; i++) {
            if (nums[i] - last >= dist) {
                // Place next cow
                cntCows++;

                // Update the last location
                last = nums[i];
            }
            if (cntCows >= cows) return true;
        }
        return false;
    }

    /* Function to find the maximum possible minimum
    distance 'k' cows can have between them in nums */
    public int aggressiveCows(int[] nums, int k) {
        // Size of array
        int n = nums.length;
        // Sort the nums
        Arrays.sort(nums);

        int low = 1, high = nums[n - 1] - nums[0];

        //Apply binary search:
        while (low <= high) {
            int mid = (low + high) / 2;
            if (canWePlace(nums, mid, k) == true) {
                low = mid + 1;
            }
            else high = mid - 1;
        }
        return high;
    }
Complexity Analysis: 
Time Complexity:O(NlogN) + O(N *log(max-min)), where N is size of the array, max is the maximum element in array, min is the minimum element in array.
O(NlogN) for sorting the array. As binary search is applied, which runs for 1 to (max-min) to check all possible distances, so O(log(max-min)). Inside the loop, canWePlace() function is being called for each distance. Now, inside the canWePlace() function, the loop runs for N times.

Space Complexity: As no additional space is used, so the Space Complexity is O(1).
```

