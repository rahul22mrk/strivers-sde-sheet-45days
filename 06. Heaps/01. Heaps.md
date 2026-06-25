1. `Implement Max Heap `
2. `K-th Largest element in an array `
3. `Maximum Sum Combination `
4. `Find Median from Data Stream `
5. `Merge K Sorted Arrays `
6. `Top K Frequent Elements `

*************************************************************

1. `Implement Max Heap `
```java
class Solution {
    private List<Integer> arr; // list to store the max-heap
    private int count; // to store the count of elements in max-heap
    
    // Constructor
    public Solution() {
        arr = new ArrayList<>();
        count = 0;
    }
    
    // Function to recursively heapify the array upwards
    private void heapifyUp(int ind) {
        int parentInd = (ind - 1)/2; 

        // If current index holds larger value than the parent
        if(ind > 0 && arr.get(ind) > arr.get(parentInd)) {
            // Swap the values at the two indices
            int temp = arr.get(ind);
            arr.set(ind, arr.get(parentInd));
            arr.set(parentInd, temp);
            
            // Recursively heapify the upper nodes
            heapifyUp(parentInd);
        } 

        return;
    }
    
    // Function to recursively heapify the array downwards
    private void heapifyDown(int ind) {
        int n = arr.size(); // Size of the array

        // To store the index of largest element
        int largestInd = ind; 

        // Indices of the left and right children
        int leftChildInd = 2*ind + 1;
        int rightChildInd = 2*ind + 2;
        
        // If the left child holds larger value, update the largest index
        if(leftChildInd < n && arr.get(leftChildInd) > arr.get(largestInd)) 
            largestInd = leftChildInd;

        // If the right child holds larger value, update the largest index
        if(rightChildInd < n && arr.get(rightChildInd) > arr.get(largestInd)) 
            largestInd = rightChildInd;

        // If the largest element index is updated
        if(largestInd != ind) {
            // Swap the largest element with the current index
            int temp = arr.get(largestInd);
            arr.set(largestInd, arr.get(ind));
            arr.set(ind, temp);

            // Recursively heapify the lower subtree
            heapifyDown(largestInd);
        }

        return; 
    }
    
    // Method to intialize the max-heap data structure
    public void initializeHeap(){
        arr.clear();
        count = 0;
    }
    
    // Method to insert a given value in the max-heap
    public void insert(int key){
        // Insert the value at the back of the list 
        arr.add(key);
        
        // Heapify upwards
        heapifyUp(count);
        count = count + 1; // Increment the counter;
        
        return;
    }
        
    // Method to change the value at a given index in max-heap
    public void changeKey(int index, int new_val){
        // If the current value is replaced with a larger value
        if(arr.get(index) < new_val) {
            arr.set(index, new_val);
            heapifyUp(index);
        }
        // Otherwise (if the current value is replaced with smaller value)
        else {
            arr.set(index, new_val);
            heapifyDown(index);
        }

        return;
    }
    
    // Method to extract the maximum value from the max-heap
    public void extractMax(){
        int ele = arr.get(0); // maximum value in the heap
        
        // Swap the top value with the value at last index
        int temp = arr.get(count - 1);
        arr.set(count - 1, arr.get(0));
        arr.set(0, temp);
        
        arr.remove(count - 1); // Pop the maximum value from the list
        count = count - 1; // Decrement the counter
        
        // Heapify the root value downwards
        if(count > 0) {
            heapifyDown(0);
        }
    }
    
    // Method to return if the max-heap is empty
    public boolean isEmpty(){
        return (count == 0);
    }
    
    // Method to return the maximum value in the max-heap
    public int getMax(){
        // Return the value stored at the root
        return arr.get(0);
    }
    
    // Method to return the size of max-heap
    public int heapSize(){
        return count;
    }
}

Complexity Analysis:
Considering there are maximum N elements inserted in the heap data structure,

Time Complexity:

Insert(val): Inserting and Heapifying upwards contribute to O(logN) time.
Get Maximum(): Constant time operation, i.e., O(1).
Extract Maximum(): Involves Heapifying downwards contributing to O(logN) time.
Heap Size(): Constant time operation, i.e., O(1).
Is Empty(): Constant time operation, i.e., O(1).
Change Key(ind, val): Involves heapifying which takes O(logN) time.

Space Complexity: O(N), because of the array used to store the elements.
```

2. `K-th Largest element in an array `
```java
//Brute + Better:
class Solution {
    // Function to get the Kth largest element 
    public int kthLargestElement(int[] nums, int k) {
        
        // Min-heap data structure
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        
        // Add the first K elements in the Min-heap
        for(int i = 0; i < k; i++) {
            pq.add(nums[i]);
        }
        
        // Process the rest of the elements 
        for(int i = k; i < nums.length; i++) {
            // Check if a new larger element is found
            if(nums[i] > pq.peek()) {
                
                pq.poll(); // remove the smallest from the min-heap
                
                // Add the current element to the min-heap
                pq.add(nums[i]);
            }
        }
        
        return pq.peek(); // Return the kth largest element 
    }
}

Complexity Analysis:
Time Complexity: O(N*logK), where N is the size of given input array.
Traversing the array takes O(N) time and for each element, in the worst case, we perform heap operations which take O(logK) time.
Note that K can be equal to N in the worst case, making the worst-case time complexity as O(N*logN).

Space Complexity: O(K), as a Min-heap data structure of size K is used to store the K largest elements.
```

```java
//Optimal:
class Solution {
    // Function to get the Kth largest element
    public int kthLargestElement(int[] nums, int k) {
        // Return -1, if the Kth largest element does not exist
        if (k > nums.length) return -1;

        // Pointers to mark the part of working array
        int left = 0, right = nums.length - 1;

        // Until the Kth largest element is found
        while (true) {
            // Get the pivot index
            int pivotIndex = randomIndex(left, right);

            // Update the pivotIndex
            pivotIndex = partitionAndReturnIndex(nums, pivotIndex, left, right);

            // If Kth largest element is found, return
            if (pivotIndex == k - 1) return nums[pivotIndex];

            // Else adjust the end pointers in array
            else if (pivotIndex > k - 1) right = pivotIndex - 1;
            else left = pivotIndex + 1;
        }
    }
    
    private Random rand = new Random();

    // Function to get a random index
    private int randomIndex(int left, int right) {
        // Length of the array
        int len = right - left + 1;
        
        // Return a random index from the array
        return rand.nextInt(len) + left;
    }

    // Function to perform the partition and return the updated index of pivot
    private int partitionAndReturnIndex(int[] nums, int pivotIndex, int left, int right) {
        int pivot = nums[pivotIndex]; // Get the pivot element
        
        // Swap the pivot with the left element
        int temp = nums[left];
        nums[left] = nums[pivotIndex];
        nums[pivotIndex] = temp;
        
        int ind = left + 1; // Index to mark the start of right portion
        
        // Traverse on the array
        for (int i = left + 1; i <= right; i++) {
            
            // If the current element is greater than the pivot
            if (nums[i] > pivot) {
                // Place the current element in the left portion
                temp = nums[ind];
                nums[ind] = nums[i];
                nums[i] = temp;
                
                // Move the right portion index
                ind++;
            }
        }
        
        // Place the pivot at the correct index
        temp = nums[left];
        nums[left] = nums[ind - 1];
        nums[ind - 1] = temp;
        
        return ind - 1; // Return the index of pivot now
    }
}

Complexity Analysis:
Time Complexity: O(N), where N is the size of the given array.
In the average case (when the pivot is chosen randomly):
Assuming the array gets divided into two equal parts, with every partitioning step, the search range is reduced by half. Thus, the time complexity is O(N + N/2 + N/4 + ... + 1) = O(N).

In the worst-case scenario (when the element at the left or right index are chosen as pivot):
In such cases, the array is divided into two unequal halves, and the search range is reduced by one element with every partitioning step. Thus, the time complexity is O(N + N-1 + N-2 + ... + 1) = O(N2). However, the probability of this worst-case scenario is negligible.

Space Complexity: O(1), as we are modifying the input array in place and using only a constant amount of extra space.
```

3. `Maximum Sum Combination `
```java
class Solution {
    public int[] maxSumCombinations(int[] nums1, int[] nums2, int k) {
        // Sort arrays in descending order
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int n = nums1.length;
        for (int i = 0; i < n / 2; i++) {
            int temp = nums1[i];
            nums1[i] = nums1[n - i - 1];
            nums1[n - i - 1] = temp;
            temp = nums2[i];
            nums2[i] = nums2[n - i - 1];
            nums2[n - i - 1] = temp;
        }

        // Max heap storing [sum, i, j]
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        Set<String> visited = new HashSet<>();

        // Push initial pair
        pq.add(new int[]{nums1[0] + nums2[0], 0, 0});
        visited.add("0,0");

        List<Integer> res = new ArrayList<>();

        // Extract k elements
        while (k-- > 0 && !pq.isEmpty()) {
            int[] top = pq.poll();
            int sum = top[0], i = top[1], j = top[2];
            res.add(sum);

            // Move in nums1
            if (i + 1 < nums1.length && !visited.contains((i + 1) + "," + j)) {
                pq.add(new int[]{nums1[i + 1] + nums2[j], i + 1, j});
                visited.add((i + 1) + "," + j);
            }

            // Move in nums2
            if (j + 1 < nums2.length && !visited.contains(i + "," + (j + 1))) {
                pq.add(new int[]{nums1[i] + nums2[j + 1], i, j + 1});
                visited.add(i + "," + (j + 1));
            }
        }

        return res.stream().mapToInt(Integer::intValue).toArray();
    }
}

Complexity Analysis
Time Complexity: O(k log k), since each heap operation (push/pop) takes log k time, and we perform at most k extractions

Space Complexity: O(k), for heap and visited set storing up to k pairs.
```

4. `Find Median from Data Stream `
```java
//Brute:
class MedianFinder {
    // list to store numbers
    private List<Integer> nums;

    // constructor initializes the list
    public MedianFinder() {
        // initialize array list
        nums = new ArrayList<>();
    }

    // addNum: append to list
    public void addNum(int num) {
        // add number
        nums.add(num);
    }

    // findMedian: sort and compute median
    public double findMedian() {
        // if empty return 0.0
        if (nums.isEmpty()) return 0.0;
        // create copy and sort
        List<Integer> sorted = new ArrayList<>(nums);
        // sort list
        Collections.sort(sorted);
        // size
        int n = sorted.size();
        // if odd
        if (n % 2 == 1) {
            // return middle element
            return (double) sorted.get(n / 2);
        } else {
            // get two middle elements
            double a = sorted.get(n / 2 - 1);
            double b = sorted.get(n / 2);
            // return average
            return (a + b) / 2.0;
        }
    }
}

Complexity Analysis :
Time Complexity: addNum is O(1) (append). findMedian is O(n log n) due to sorting the list of size n.

Space Complexity: O(n) to store all inserted numbers.
```

```java
//Optimal:

import java.util.*;

class MedianFinder {
    PriorityQueue<Integer> mx = new PriorityQueue<>(Collections.reverseOrder());
    PriorityQueue<Integer> mn = new PriorityQueue<>();

    public void addNum(int num) {
        if (mx.isEmpty() || num <= mx.peek()) mx.offer(num);
        else mn.offer(num);

        if (mx.size() > mn.size() + 1) {
            mn.offer(mx.poll());
        } else if (mn.size() > mx.size()) {
            mx.offer(mn.poll());
        }
    }

    public double findMedian() {
        if (mx.size() == mn.size()) return (mx.peek() + mn.peek()) / 2.0;
        return mx.peek();
    }
}

Complexity Analysis
Time Complexity :

addNum: O(log n)
findMedian: O(1)
Space Complexity: O(n)
```

5. `Merge K Sorted Arrays `
```java
// Solution class to merge k sorted arrays
class Solution {

    // Function to merge k sorted arrays of size k each
    public List<Integer> mergeKSortedArrays(int[][] arr, int k) {
        // Result list to store merged elements
        List<Integer> result = new ArrayList<>();

        // Min-heap to store {value, row, column}
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));

        // Push first element of each array into the heap
        for (int i = 0; i < k; i++) {
            if (arr[i].length > 0) {
                pq.offer(new int[]{arr[i][0], i, 0}); 
            }
        }

        // Process the heap until empty
        while (!pq.isEmpty()) {
            int[] top = pq.poll();
            int val = top[0], row = top[1], col = top[2];

            // Add the smallest element to the result
            result.add(val);

            // If the current array has more elements, push the next element into the heap
            if (col + 1 < arr[row].length) {
                pq.offer(new int[]{arr[row][col + 1], row, col + 1});
            }
        }

        return result;
    }
}

Time and Space Complexity
Time Complexity: O(k² log k), where k is the number of arrays. Each array has k elements, so there are k² total elements. Each element is pushed and popped from the min-heap once, and heap operations take O(log k) time.
Space Complexity: O(k), used by the min-heap to store at most one element from each array at a time.
```

6. `Top K Frequent Elements `
```java
//Brute:
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();
        for (int num : nums)
            freq.put(num, freq.getOrDefault(num, 0) + 1);

        List<Map.Entry<Integer, Integer>> list = new ArrayList<>(freq.entrySet());
        list.sort((a, b) -> b.getValue() - a.getValue());

        int[] result = new int[k];
        for (int i = 0; i < k; i++)
            result[i] = list.get(i).getKey();

        return result;
    }
}

Complexity Analysis
Time Complexity: O(n log n), for sorting frequency pairs.

Space Complexity: O(n), for storing frequency map.
```

```java
//Optimal:

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();
        for (int num : nums)
            freq.put(num, freq.getOrDefault(num, 0) + 1);

        PriorityQueue<Map.Entry<Integer, Integer>> pq =
            new PriorityQueue<>(Comparator.comparingInt(Map.Entry::getValue));

        for (Map.Entry<Integer, Integer> e : freq.entrySet()) {
            pq.add(e);
            if (pq.size() > k) pq.poll();
        }

        int[] result = new int[k];
        int i = 0;
        while (!pq.isEmpty()) result[i++] = pq.poll().getKey();
        return result;
    }
}

Complexity Analysis
Time Complexity: O(n log k), each heap operation takes log k.

Space Complexity: O(n), frequency map + heap.
```
