Q.25. 3 Sum
Brute:
 //Function to find triplets having sum equals to target
    public List<List<Integer>> threeSum(int[] nums) {
        // Set to store unique triplets
        Set<List<Integer>> tripletSet = new HashSet<>();

        int n = nums.length;

        // Check all possible triplets
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        // Found a triplet that sums up to target
                        List<Integer> temp = new ArrayList<>();
                        temp.add(nums[i]);
                        temp.add(nums[j]);
                        temp.add(nums[k]);
                        
                        /* Sort the triplet to ensure 
                        uniqueness when storing in set*/
                        Collections.sort(temp);
                        tripletSet.add(temp);
                    }
                }
            }
        }

        // Convert set to list of lists (unique triplets)
        List<List<Integer>> ans = new ArrayList<>(tripletSet);

        //Return the ans
        return ans;
    }

Complexity Analyis
Time Complexity: O(N3 x log(no. of unique triplets)), where N is size of the array. Using 3 nested loops & inserting triplets into the set takes O(log(no. of unique triplets)) time complexity. But we are not considering the time complexity of sorting as we are just sorting 3 elements every time.

Space Complexity: O(2 x no. of the unique triplets) for using a set data structure and a list to store the triplets.

Better:
// Function to find triplets having sum equals to 0
    public List<List<Integer>> threeSum(int[] nums) {
        // Set to store unique triplets
        Set<List<Integer>> tripletSet = new HashSet<>();

        int n = nums.length;

        // Check all possible triplets
        for (int i = 0; i < n; i++) {
            // Set to store elements seen so far in the loop
            Set<Integer> hashset = new HashSet<>();

            for (int j = i + 1; j < n; j++) {
                // Calculate the 3rd element needed to reach 0
                int third =  - (nums[i] + nums[j]);

                /* Find if third element exists in
                hashset (complements seen so far)*/
                if (hashset.contains(third)) {
                    // Found a triplet that sums up to target
                    List<Integer> temp = new ArrayList<>();
                    temp.add(nums[i]);
                    temp.add(nums[j]);
                    temp.add(third);

                    /* Sort the triplet to ensure
                    uniqueness when storing in set*/
                    Collections.sort(temp);
                    tripletSet.add(temp);
                }

                /* Insert the current element 
                 into hashset for future checks*/
                hashset.add(nums[j]);
            }
        }

        // Convert set to list of lists (unique triplets)
        List<List<Integer>> ans = new ArrayList<>(tripletSet);

        //Return the ans
        return ans;
    }
Complexity Analysis
Time Complexity: O(N2 x log(no. of unique triplets)), where N is size of the array.
Inserting triplets into the set takes O(log(no. of unique triplets)) time complexity. However, we are not considering the time complexity of sorting, as we are only sorting 3 elements each time.
Note: For Java (HashSet), insertion operation takes O(1) time. Thus, the overall time complexity for Java code will be O(N2)

Space Complexity: O(2 x no. of the unique triplets) + O(N) for using a set data structure and a list to store the triplets and extra O(N) for storing the array elements in another set.

Optimal:
 // Function to find triplets having sum equals to target
    public List<List<Integer>> threeSum(int[] nums) {
        
        // List to store the triplets that sum up to target
        List<List<Integer>> ans = new ArrayList<>();
        
        int n = nums.length;
        
        // Sort the input array nums
        Arrays.sort(nums);
        
        // Iterate through the array to find triplets
        for (int i = 0; i < n; i++) {
            // Skip duplicates
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            
            // Two pointers approach
            int j = i + 1;
            int k = n - 1;
            
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                
                if (sum < 0) {
                    j++;
                } else if (sum > 0) {
                    k--;
                } else {
                    // Found a triplet that sums up to target
                    List<Integer> temp = new ArrayList<>();
                    temp.add(nums[i]);
                    temp.add(nums[j]);
                    temp.add(nums[k]);
                    ans.add(temp);
                    
                    // Skip duplicates
                    j++;
                    k--;
                    while (j < k && nums[j] == nums[j - 1]) j++;
                    while (j < k && nums[k] == nums[k + 1]) k--;
                }
            }
        }
        
        return ans;
    }
Complexity Analysis
Time Complexity: O(NlogN)+O(N2), where N is size of the array. As the pointer i, is running for approximately N times. And both the pointers j and k combined can run for approximately N times including the operation of skipping duplicates. So the total time complexity will be O(N2).

Space Complexity: O(1), no extra space is used.

Q.26 Trapping Rainwater
Brute:
 // Function to find the prefix maximum array
    private int[] findPrefixMax(int[] arr, int n) {
        // To store the prefix maximum
        int[] prefixMax = new int[n];
        
        // Initial configuration
        prefixMax[0] = arr[0];
        
        // Traverse the array
        for(int i = 1; i < n; i++) {
            // Store the maximum till ith index
            prefixMax[i] = 
                Math.max(prefixMax[i-1], arr[i]);
        }
        
        // Return the prefix maximum array
        return prefixMax;
    }
    
    // Function to find the suffix maximum array
    private int[] findSuffixMax(int[] arr, int n) {
        // To store the suffix maximum
        int[] suffixMax = new int[n];
        
        // Initial configuration
        suffixMax[n-1] = arr[n-1];
        
        // Traverse the array
        for(int i = n-2; i >= 0; i--) {
            // Store the maximum till ith index
            suffixMax[i] = 
                Math.max(suffixMax[i+1], arr[i]);
        }
        
        // Return the suffix maximum array
        return suffixMax;
    }

    // Function to get the trapped water
    public int trap(int[] height) {
        
        int n = height.length; // Size of array
    
        // To store the total trapped rainwater
        int total = 0;
        
        // Calculate prefix maximum array
        int[] leftMax = 
            findPrefixMax(height, n);
        
        // Calculate suffix maximum array
        int[] rightMax = 
            findSuffixMax(height, n);
        
        // Traverse the array
        for(int i = 0; i < n; i++) {
            
            /* If there are higher grounds 
            on both side to hold water */
            if(height[i] < leftMax[i] && 
               height[i] < rightMax[i]) {
                   
                // Add up the water on top of current height
                total += ( Math.min(leftMax[i], rightMax[i]) 
                           - height[i] );
            }
        }
        
        // Return the result
        return total;
    }
Complexity Analysis:
Time Complexity: O(N) (where N is the size of given array)

Calculating the Prefix and Suffix Maximum Arrays take O(N) time each.
Traversing on the given array once takes O(N) time.
Space Complexity: O(N)
Storing the Prefix and Suffix Maximum Arrays takes O(N) space each.

Optimal:
// Function to get the trapped water
    public int trap(int[] height) {
        
        int n = height.length; // Size of array
    
        // To store the total trapped rainwater
        int total = 0;
        
        // To store the maximums on both sides
        int leftMax = 0, rightMax = 0;
        
        // Left and Right pointers
        int left = 0, right = n - 1;
        
        // Traverse from both ends
        while (left < right) {
            
            // If left height is smaller or equal
            if (height[left] <= height[right]) {
                
                // If water can be stored
                if (leftMax > height[left]) {
                    
                    // Update total water
                    total += leftMax - height[left];
                }
                
                // Else update maximum height on left
                else leftMax = height[left];
                
                // Shift left by 1
                left = left + 1;
            }
            
            // Else if right height is smaller
            else {
                
                // If water can be stored
                if (rightMax > height[right]) {
                    
                    // Update total water
                    total += rightMax - height[right];
                }
                
                // Else update maximum height on right
                else rightMax = height[right];
                
                // Shift right by 1
                right = right - 1;
            }
        }
        
        // Return the result
        return total;
    }
Complexity Analysis:
Time Complexity: O(N) (where N is the size of given array)
The left and right pointers will traverse the whole array in total taking O(N) time.

Space Complexity: O(1)
Using only a couple of variables.

Q.27 Remove duplicates from sorted array
Brute:
// Function to remove duplicates from the array
    public int removeDuplicates(int[] nums) {
        
        // TreeSet to store unique elements in sorted order
        Set<Integer> s = new TreeSet<>();
        
        // Add all elements from array to the set
        for (int val : nums) {
            s.add(val);
        }
        
        // Get the number of unique elements
        int k = s.size();
        
        int j = 0;
        // Copy unique elements from set to array
        for (int val : s) {
            nums[j++] = val;
        }
        
        // Return the number of unique elements
        return k;
    }
Complexity Analysis 
Time Complexity: O(N * log N) + O(N), for using hashset, it will take O(N * log N) and also to traverse the array once O(N). Here N is the size of the array.

Space Complexity: O(N) because in the worst case, all the elements of the array can be unique and it will take O(N) space. Here N represents the size of the array.

Optimal:
 // Function to remove duplicates from the array
    public int removeDuplicates(int[] nums) {
        
        
        // Initialize pointer for unique elements
        int i = 0;
        
        // Iterate through the array
        for (int j = 1; j < nums.length; j++) {
            /*If current element is different 
            from the previous unique element*/
            if (nums[i] != nums[j]) {
                /* Move to the next position in 
                the array for the unique element*/
                i++;
                /* Update the current position 
                   with the unique element*/
                nums[i] = nums[j];
            }
        }
        
        // Return the number of unique elements
        return i + 1;
    }
Complexity Analysis 
Time Complexity: O(N), for single traversal of the array, where N is the size of the array.

Space Complexity: O(1), not using any extra space.

Q.28 Maximum Consecutive Ones

public int findMaxConsecutiveOnes(int[] nums) {
        /* Initialize count and max_count 
               to track current and maximum consecutive 1s */
        int cnt = 0;
        int maxi = 0;

        // Traverse the array
        for (int i = 0; i < nums.length; i++) {

            // If the current element is 1, increment the count
            if (nums[i] == 1) {
                cnt++;

                // Update maxi if current count is greater than maxi
                maxi = Math.max(maxi, cnt);

            } else {
                // If the current element is 0, reset the count
                cnt = 0;
            }
        }
        // Return the maximum count of consecutive 1s
        return maxi;
    }

Complexity Analysis 
Time Complexity: O(N), as there is single traversal of the array .Here N is the number of elements in the array.
Space Complexity: O(1), as no additional space is used .

ye 4 aur add karo ...

fir ese likho Pattern Name:
uske niche question with their index also usme trick kare to ques me pahuch jaye 

jo pattern ayaya usko kaise pahchane 
uska universal template ye bhi mention kara
