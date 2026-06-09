# 📒 Striver SDE Sheet - Part 2 (Q15-Q28 + Pattern Guide)

> 📄 **[← Part 1 (Q1-Q14)](Striver_SDE_Sheet_Notes_Part1.md)**

---

## 📑 Quick Navigation

- [📝 Detailed Solutions Q15-Q28](#-detailed-solutions-q15-q28)
- [🧠 Pattern Identification Guide](#-pattern-identification-guide--universal-templates)

---

## 📝 Detailed Solutions Q15-Q28

---

### Q15. Majority Element I
**Pattern:** Boyer-Moore Voting | **LeetCode:** 169

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
public int majorityElement(int[] nums) {
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        int cnt = 0; 
        for (int j = 0; j < n; j++) {
            if (nums[j] == nums[i]) cnt++;
        }
        if (cnt > (n / 2)) return nums[i]; 
    }
    return -1; 
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(1)
</details>

<details>
<summary>🔧 Better — HashMap O(N)</summary>

```java
public int majorityElement(int[] nums) {
    int n = nums.length;
    HashMap<Integer, Integer> map = new HashMap<>();
    for (int num : nums) map.put(num, map.getOrDefault(num, 0) + 1);
    for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
        if (entry.getValue() > n / 2) return entry.getKey();
    }
    return -1;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(N)
</details>

<details>
<summary>🚀 Optimal — Boyer-Moore Voting O(N) O(1)</summary>

```java
public int majorityElement(int[] nums) {
    int n = nums.length, cnt = 0, el = 0;
    for (int i = 0; i < n; i++) {
        if (cnt == 0) { cnt = 1; el = nums[i]; }
        else if (el == nums[i]) cnt++;
        else cnt--;
    }
    int cnt1 = 0;
    for (int i = 0; i < n; i++) if (nums[i] == el) cnt1++;
    if (cnt1 > (n / 2)) return el;
    return -1;
}
```

**Time Complexity:** O(N) + O(N) | **Space Complexity:** O(1)
</details>

---

### Q16. Majority Element II
**Pattern:** Boyer-Moore Voting | **LeetCode:** 229

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
public List<Integer> majorityElementTwo(int[] nums) {
    int n = nums.length;
    List<Integer> result = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        if (result.size() == 0 || result.get(0) != nums[i]) {
            int cnt = 0;
            for (int j = 0; j < n; j++) if (nums[j] == nums[i]) cnt++;
            if (cnt > (n / 3)) result.add(nums[i]);
        }
        if (result.size() == 2) break;
    }
    return result;
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(1)
</details>

<details>
<summary>🔧 Better — HashMap O(N)</summary>

```java
public List<Integer> majorityElementTwo(int[] nums) {
    int n = nums.length;
    List<Integer> result = new ArrayList<>();
    Map<Integer, Integer> mpp = new HashMap<>();
    int mini = n / 3 + 1;
    for (int i = 0; i < n; i++) {
        mpp.put(nums[i], mpp.getOrDefault(nums[i], 0) + 1);
        if (mpp.get(nums[i]) == mini) result.add(nums[i]);
        if (result.size() == 2) break;
    }
    return result;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(N)
</details>

<details>
<summary>🚀 Optimal — Extended Boyer-Moore O(N) O(1)</summary>

```java
public List<Integer> majorityElementTwo(int[] nums) {
    int n = nums.length;
    int cnt1 = 0, cnt2 = 0;
    int el1 = Integer.MIN_VALUE, el2 = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        if (cnt1 == 0 && el2 != nums[i]) { cnt1 = 1; el1 = nums[i]; }
        else if (cnt2 == 0 && el1 != nums[i]) { cnt2 = 1; el2 = nums[i]; }
        else if (nums[i] == el1) cnt1++;
        else if (nums[i] == el2) cnt2++;
        else { cnt1--; cnt2--; }
    }
    cnt1 = 0; cnt2 = 0;
    for (int i = 0; i < n; i++) {
        if (nums[i] == el1) cnt1++;
        if (nums[i] == el2) cnt2++;
    }
    int mini = n / 3 + 1;
    List<Integer> result = new ArrayList<>();
    if (cnt1 >= mini) result.add(el1);
    if (cnt2 >= mini && el1 != el2) result.add(el2);
    return result;
}
```

**Time Complexity:** O(N) + O(N) | **Space Complexity:** O(1)

**⚠️ Key:** Guard conditions `el2 != nums[i]` and `el1 != nums[i]` prevent both candidates from becoming the same element.
</details>

---

### Q17. Grid Unique Paths
**Pattern:** Math/Combinatorics / DP | **LeetCode:** 62

<details>
<summary>⚡ Recursion — O(2^(M+N))</summary>

```java
private int func(int i, int j) {
    if (i == 0 && j == 0) return 1;
    if (i < 0 || j < 0) return 0;
    return func(i - 1, j) + func(i, j - 1);
}
public int uniquePaths(int m, int n) { return func(m - 1, n - 1); }
```

**Time Complexity:** O(2^(M+N)) | **Space Complexity:** O((M-1)+(N-1))
</details>

<details>
<summary>🔧 Memoization — O(M×N)</summary>

```java
private int func(int i, int j, int[][] dp) {
    if (i == 0 && j == 0) return 1;
    if (i < 0 || j < 0) return 0;
    if (dp[i][j] != -1) return dp[i][j];
    return dp[i][j] = func(i - 1, j, dp) + func(i, j - 1, dp);
}
public int uniquePaths(int m, int n) {
    int dp[][] = new int[m][n];
    for (int[] row : dp) Arrays.fill(row, -1);
    return func(m - 1, n - 1, dp);
}
```

**Time Complexity:** O(M×N) | **Space Complexity:** O((N-1)+(M-1)) + O(M×N)
</details>

<details>
<summary>🔧 Tabulation — O(M×N) O(M×N)</summary>

```java
int func(int m, int n, int[][] dp) {
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) { dp[i][j] = 1; continue; }
            int up = (i > 0) ? dp[i-1][j] : 0;
            int left = (j > 0) ? dp[i][j-1] : 0;
            dp[i][j] = up + left;
        }
    }
    return dp[m-1][n-1];
}
```

**Time Complexity:** O(M×N) | **Space Complexity:** O(M×N)
</details>

<details>
<summary>🚀 Space Optimization — O(M×N) O(N)</summary>

```java
int func(int m, int n) {
    int[] prev = new int[n];
    for (int i = 0; i < m; i++) {
        int[] temp = new int[n];
        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) { temp[j] = 1; continue; }
            int up = (i > 0) ? prev[j] : 0;
            int left = (j > 0) ? temp[j-1] : 0;
            temp[j] = up + left;
        }
        prev = temp;
    }
    return prev[n-1];
}
```

**Time Complexity:** O(M×N) | **Space Complexity:** O(N)
</details>

---

### Q18. Reverse Pairs
**Pattern:** Merge Sort (Divide & Conquer) | **LeetCode:** 493

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
public int reversePairs(int[] nums) {
    int cnt = 0, n = nums.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if ((long)nums[i] > (long)2 * nums[j]) cnt++;
        }
    }
    return cnt;
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal — Merge Sort O(2N log N)</summary>

```java
class Solution {
    public int reversePairs(int[] nums) {
        return mergeSort(nums, 0, nums.length - 1);
    }
    private int mergeSort(int[] nums, int low, int high) {
        if (low >= high) return 0;
        int mid = (low + high) / 2, cnt = 0;
        cnt += mergeSort(nums, low, mid);
        cnt += mergeSort(nums, mid + 1, high);
        cnt += countPairs(nums, low, mid, high);
        merge(nums, low, mid, high);
        return cnt;
    }
    private int countPairs(int[] nums, int low, int mid, int high) {
        int right = mid + 1, cnt = 0;
        for (int i = low; i <= mid; i++) {
            while (right <= high && (long) nums[i] > 2L * nums[right]) right++;
            cnt += (right - (mid + 1));
        }
        return cnt;
    }
    private void merge(int[] nums, int low, int mid, int high) {
        List<Integer> temp = new ArrayList<>();
        int left = low, right = mid + 1;
        while (left <= mid && right <= high) {
            if (nums[left] <= nums[right]) temp.add(nums[left++]);
            else temp.add(nums[right++]);
        }
        while (left <= mid) temp.add(nums[left++]);
        while (right <= high) temp.add(nums[right++]);
        for (int i = low; i <= high; i++) nums[i] = temp.get(i - low);
    }
}
```

**Time Complexity:** O(2N × logN) | **Space Complexity:** O(N)

**⚠️ Key:** Count pairs **before** merge. Use `long` cast: `(long) nums[i] > 2L * nums[right]`.
</details>

---

### Q19. Two Sum
**Pattern:** Two Pointer / HashMap | **LeetCode:** 1

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) return new int[]{i, j};
            }
        }
        return new int[]{-1, -1};
    }
}
```
</details>

<details>
<summary>🔧 Better — HashMap O(N)</summary>

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> hm = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int required = target - nums[i];
            if (hm.containsKey(required)) return new int[]{hm.get(required), i};
            hm.put(nums[i], i);
        }
        return new int[]{-1, -1};
    }
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(N)
</details>

<details>
<summary>🚀 Optimal — Sort + Two Pointer O(N log N)</summary>

**Note:** Returns **values**, not indices. For indices, use HashMap.

```java
public int[] twoSum(int[] nums, int target) {
    int n = nums.length;
    int[][] eleIndex = new int[n][2];
    for (int i = 0; i < n; i++) { eleIndex[i][0] = nums[i]; eleIndex[i][1] = i; }
    Arrays.sort(eleIndex, (a, b) -> Integer.compare(a[0], b[0]));
    int left = 0, right = n - 1;
    while (left < right) {
        int sum = eleIndex[left][0] + eleIndex[right][0];
        if (sum == target) return new int[]{eleIndex[left][1], eleIndex[right][1]};
        else if (sum < target) left++;
        else right--;
    }
    return new int[]{-1, -1};
}
```

**Time Complexity:** O(N log N) | **Space Complexity:** O(N)
</details>

---

### Q20. 4 Sum
**Pattern:** Two Pointer (Sort+2P) | **LeetCode:** 18

<details>
<summary>⚡ Brute Force — O(N⁴)</summary>

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            for (int k = j+1; k < n; k++) {
                for (int l = k+1; l < n; l++) {
                    long sum = nums[i] + nums[j] + nums[k] + nums[l];
                    if (sum == target) {
                        List<Integer> temp = Arrays.asList(nums[i], nums[j], nums[k], nums[l]);
                        Collections.sort(temp); set.add(temp);
                    }
                }
            }
        }
    }
    return new ArrayList<>(set);
}
```
</details>

<details>
<summary>🔧 Better — Hashing O(N³ × logM)</summary>

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> ans = new ArrayList<>();
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            Set<Long> hashset = new HashSet<>();
            for (int k = j+1; k < n; k++) {
                long sum = (long) nums[i] + nums[j] + nums[k];
                long fourth = target - sum;
                if (hashset.contains(fourth)) {
                    List<Integer> temp = Arrays.asList(nums[i], nums[j], nums[k], (int) fourth);
                    Collections.sort(temp); set.add(temp);
                }
                hashset.add((long) nums[k]);
            }
        }
    }
    ans.addAll(set); return ans;
}
```
</details>

<details>
<summary>🚀 Optimal — Sort + Two Pointer O(N³)</summary>

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> ans = new ArrayList<>();
    int n = nums.length;
    Arrays.sort(nums);
    for (int i = 0; i < n; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        for (int j = i+1; j < n; j++) {
            if (j > i+1 && nums[j] == nums[j-1]) continue;
            int k = j+1, l = n-1;
            while (k < l) {
                long sum = (long) nums[i] + nums[j] + nums[k] + nums[l];
                if (sum == target) {
                    ans.add(Arrays.asList(nums[i], nums[j], nums[k], nums[l]));
                    k++; l--;
                    while (k < l && nums[k] == nums[k-1]) k++;
                    while (k < l && nums[l] == nums[l+1]) l--;
                } else if (sum < target) k++;
                else l--;
            }
        }
    }
    return ans;
}
```

**Time Complexity:** O(N³) | **Space Complexity:** O(1)*

**⚠️ Key:** Use `long` for sum. Skip duplicates at each level.
</details>

---

### Q21. Longest Consecutive Sequence in an Array
**Pattern:** HashMap/HashSet | **LeetCode:** 128

<details>
<summary>⚡ Brute Force — O(N³)</summary>

```java
private boolean linearSearch(int[] a, int num) {
    for (int i = 0; i < a.length; i++) if (a[i] == num) return true;
    return false;
}
public int longestConsecutive(int[] nums) {
    if (nums.length == 0) return 0;
    int longest = 1;
    for (int i = 0; i < nums.length; i++) {
        int x = nums[i], cnt = 1;
        while (linearSearch(nums, x + 1)) { x++; cnt++; }
        longest = Math.max(longest, cnt);
    }
    return longest;
}
```
</details>

<details>
<summary>🔧 Better — Sorting O(N log N)</summary>

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;
        Arrays.sort(nums);
        int maxCnt = 1, cnt = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i-1]) continue;
            if (nums[i-1] + 1 == nums[i]) cnt++;
            else { maxCnt = Math.max(maxCnt, cnt); cnt = 1; }
        }
        return Math.max(maxCnt, cnt);
    }
}
```

**Time Complexity:** O(NlogN) | **Space Complexity:** O(1)
</details>

<details>
<summary>🚀 Optimal — HashSet O(N)</summary>

```java
public int longestConsecutive(int[] nums) {
    int n = nums.length;
    if (n == 0) return 0;
    int longest = 1;
    Set<Integer> st = new HashSet<>();
    for (int i = 0; i < n; i++) st.add(nums[i]);
    for (int it : st) {
        if (!st.contains(it - 1)) {
            int cnt = 1, x = it;
            while (st.contains(x + 1)) { x++; cnt++; }
            longest = Math.max(longest, cnt);
        }
    }
    return longest;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(N)
</details>

---

### Q22. Largest Subarray with K Sum
**Pattern:** Prefix Sum / Sliding Window | **LeetCode:** 560

<details>
<summary>⚡ Brute Force — O(N³)</summary>

```java
public int longestSubarray(int[] nums, int k) {
    int n = nums.length, maxLength = 0;
    for (int si = 0; si < n; si++) {
        for (int ei = si; ei < n; ei++) {
            int sum = 0;
            for (int i = si; i <= ei; i++) sum += nums[i];
            if (sum == k) maxLength = Math.max(maxLength, ei - si + 1);
        }
    }
    return maxLength;
}
```
</details>

<details>
<summary>🚀 Optimal — Prefix Sum + HashMap (Positives + Negatives) O(N)</summary>

```java
public int longestSubarray(int[] nums, int k) {
    int n = nums.length;
    Map<Integer, Integer> preSumMap = new HashMap<>();
    int sum = 0, maxLen = 0;
    for (int i = 0; i < n; i++) {
        sum += nums[i];
        if (sum == k) maxLen = Math.max(maxLen, i + 1);
        int rem = sum - k;
        if (preSumMap.containsKey(rem)) maxLen = Math.max(maxLen, i - preSumMap.get(rem));
        if (!preSumMap.containsKey(sum)) preSumMap.put(sum, i);
    }
    return maxLen;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(N)
</details>

<details>
<summary>🚀 Optimal — Two Pointer (Positive Only) O(N) O(1)</summary>

```java
public int longestSubarray(int[] nums, int k) {
    int n = nums.length, maxLen = 0, left = 0, right = 0, sum = nums[0];
    while (right < n) {
        while (left <= right && sum > k) { sum -= nums[left]; left++; }
        if (sum == k) maxLen = Math.max(maxLen, right - left + 1);
        right++;
        if (right < n) sum += nums[right];
    }
    return maxLen;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(1)

**⚠️ Key:** Two Pointer only works for **positive numbers**. For negatives, use Prefix Sum + HashMap.
</details>

---

### Q23. Count Subarrays with Given XOR K
**Pattern:** Prefix XOR + HashMap | **LeetCode:** —

<details>
<summary>⚡ Brute Force — O(N³)</summary>

```java
public int subarraysWithXorK(int[] nums, int k) {
    int n = nums.length, cnt = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int xorr = 0;
            for (int K = i; K <= j; K++) xorr = xorr ^ nums[K];
            if (xorr == k) cnt++;
        }
    }
    return cnt;
}
```
</details>

<details>
<summary>🔧 Better — O(N²)</summary>

```java
public int subarraysWithXorK(int[] nums, int k) {
    int n = nums.length, cnt = 0;
    for (int i = 0; i < n; i++) {
        int xorr = 0;
        for (int j = i; j < n; j++) {
            xorr = xorr ^ nums[j];
            if (xorr == k) cnt++;
        }
    }
    return cnt;
}
```
</details>

<details>
<summary>🚀 Optimal — Prefix XOR + HashMap O(N)</summary>

```java
public int subarraysWithXorK(int[] nums, int k) {
    int n = nums.length, xr = 0, cnt = 0;
    Map<Integer, Integer> mpp = new HashMap<>();
    mpp.put(0, 1);
    for (int i = 0; i < n; i++) {
        xr = xr ^ nums[i];
        int x = xr ^ k;
        cnt += mpp.getOrDefault(x, 0);
        mpp.put(xr, mpp.getOrDefault(xr, 0) + 1);
    }
    return cnt;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(N)

**⚠️ Key:** Formula: `xr ^ k = x`. Initialize map with `{0: 1}`.
</details>

---

### Q24. Longest Substring Without Repeating Characters
**Pattern:** Sliding Window | **LeetCode:** 3

<details>
<summary>⚡ Brute Force — O(N²)</summary>

```java
public int longestNonRepeatingSubstring(String s) {
    int n = s.length(), maxLen = 0;
    for (int i = 0; i < n; i++) {
        int[] hash = new int[256];
        for (int j = i; j < n; j++) {
            if (hash[s.charAt(j)] == 1) break;
            hash[s.charAt(j)] = 1;
            maxLen = Math.max(maxLen, j - i + 1);
        }
    }
    return maxLen;
}
```
</details>

<details>
<summary>🚀 Optimal — Sliding Window + Hash Array O(N)</summary>

```java
public int longestNonRepeatingSubstring(String s) {
    int n = s.length();
    int[] hash = new int[256];
    Arrays.fill(hash, -1);
    int l = 0, r = 0, maxLen = 0;
    while (r < n) {
        if (hash[s.charAt(r)] >= l) l = Math.max(hash[s.charAt(r)] + 1, l);
        maxLen = Math.max(r - l + 1, maxLen);
        hash[s.charAt(r)] = r;
        r++;
    }
    return maxLen;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(256)

**⚠️ Key:** `hash[s.charAt(r)] >= l` checks if character's last occurrence is within current window.
</details>

---

### Q25. 3 Sum
**Pattern:** Two Pointer (Sort+2P) | **LeetCode:** 15

<details>
<summary>⚡ Brute Force — O(N³)</summary>

```java
public List<List<Integer>> threeSum(int[] nums) {
    Set<List<Integer>> tripletSet = new HashSet<>();
    int n = nums.length;
    for (int i = 0; i < n-2; i++) {
        for (int j = i+1; j < n-1; j++) {
            for (int k = j+1; k < n; k++) {
                if (nums[i]+nums[j]+nums[k] == 0) {
                    List<Integer> temp = Arrays.asList(nums[i], nums[j], nums[k]);
                    Collections.sort(temp); tripletSet.add(temp);
                }
            }
        }
    }
    return new ArrayList<>(tripletSet);
}
```
</details>

<details>
<summary>🔧 Better — Hashing O(N²)</summary>

```java
public List<List<Integer>> threeSum(int[] nums) {
    Set<List<Integer>> tripletSet = new HashSet<>();
    int n = nums.length;
    for (int i = 0; i < n; i++) {
        Set<Integer> hashset = new HashSet<>();
        for (int j = i+1; j < n; j++) {
            int third = -(nums[i] + nums[j]);
            if (hashset.contains(third)) {
                List<Integer> temp = Arrays.asList(nums[i], nums[j], third);
                Collections.sort(temp); tripletSet.add(temp);
            }
            hashset.add(nums[j]);
        }
    }
    return new ArrayList<>(tripletSet);
}
```
</details>

<details>
<summary>🚀 Optimal — Sort + Two Pointer O(N²)</summary>

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    int n = nums.length;
    Arrays.sort(nums);
    for (int i = 0; i < n; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        int j = i+1, k = n-1;
        while (j < k) {
            int sum = nums[i] + nums[j] + nums[k];
            if (sum < 0) j++;
            else if (sum > 0) k--;
            else {
                ans.add(Arrays.asList(nums[i], nums[j], nums[k]));
                j++; k--;
                while (j < k && nums[j] == nums[j-1]) j++;
                while (j < k && nums[k] == nums[k+1]) k--;
            }
        }
    }
    return ans;
}
```

**Time Complexity:** O(N²) | **Space Complexity:** O(1)
</details>

---

### Q26. Trapping Rainwater
**Pattern:** Two Pointer (Prefix-Suffix) | **LeetCode:** 42

<details>
<summary>🔧 Better — Prefix/Suffix Max Arrays O(N) O(N)</summary>

```java
public int trap(int[] height) {
    int n = height.length, total = 0;
    int[] leftMax = new int[n], rightMax = new int[n];
    leftMax[0] = height[0];
    for (int i = 1; i < n; i++) leftMax[i] = Math.max(leftMax[i-1], height[i]);
    rightMax[n-1] = height[n-1];
    for (int i = n-2; i >= 0; i--) rightMax[i] = Math.max(rightMax[i+1], height[i]);
    for (int i = 0; i < n; i++) {
        if (height[i] < leftMax[i] && height[i] < rightMax[i])
            total += Math.min(leftMax[i], rightMax[i]) - height[i];
    }
    return total;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(N)
</details>

<details>
<summary>🚀 Optimal — Two Pointer O(N) O(1)</summary>

```java
public int trap(int[] height) {
    int n = height.length, total = 0;
    int leftMax = 0, rightMax = 0, left = 0, right = n-1;
    while (left < right) {
        if (height[left] <= height[right]) {
            if (leftMax > height[left]) total += leftMax - height[left];
            else leftMax = height[left];
            left++;
        } else {
            if (rightMax > height[right]) total += rightMax - height[right];
            else rightMax = height[right];
            right--;
        }
    }
    return total;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(1)

**⚠️ Key:** Always move pointer with **smaller** height — guaranteeing the other side has a wall ≥ current max.
</details>

---

### Q27. Remove Duplicates from Sorted Array
**Pattern:** Two Pointer (In-place) | **LeetCode:** 26

<details>
<summary>🔧 Better — TreeSet O(N log N)</summary>

```java
public int removeDuplicates(int[] nums) {
    Set<Integer> s = new TreeSet<>();
    for (int val : nums) s.add(val);
    int j = 0;
    for (int val : s) nums[j++] = val;
    return s.size();
}
```

**Time Complexity:** O(N log N) | **Space Complexity:** O(N)
</details>

<details>
<summary>🚀 Optimal — Two Pointer O(N) O(1)</summary>

```java
public int removeDuplicates(int[] nums) {
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[i] != nums[j]) { i++; nums[i] = nums[j]; }
    }
    return i + 1;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(1)
</details>

---

### Q28. Maximum Consecutive Ones
**Pattern:** Linear Scan | **LeetCode:** 485

<details>
<summary>✅ Optimal — Single Pass O(N) O(1)</summary>

```java
public int findMaxConsecutiveOnes(int[] nums) {
    int cnt = 0, maxi = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 1) { cnt++; maxi = Math.max(maxi, cnt); }
        else cnt = 0;
    }
    return maxi;
}
```

**Time Complexity:** O(N) | **Space Complexity:** O(1)
</details>

---

## 🧠 Pattern Identification Guide & Universal Templates

> **How to identify which pattern applies to a problem, and the universal template to code it.**
> 📄 **[← Back to Part 1](Striver_SDE_Sheet_Notes_Part1.md)**

---

### A. 👆 Two Pointer
**🔍 How to Identify:**
- Two indices moving toward each other or in same direction
- Sorted array + find pairs/triplets/quadruplets with sum condition
- In-place partitioning (Dutch National Flag)
- Merge two sorted structures, remove duplicates in-place
- Keywords: "sum equals target", "sort colors", "merge sorted", "remove duplicates", "trapping water"

**📋 Universal Templates:**

**Opposite Direction (K-Sum):**
```java
Arrays.sort(nums);
// 2 Sum: left=0, right=n-1, move based on sum vs target
// 3 Sum: fix i, then two pointers j & k in remaining
// 4 Sum: fix i & j, then two pointers k & l in remaining
// Always skip duplicates after each level
```

**Dutch National Flag (3-way partition):**
```java
int low = 0, mid = 0, high = n - 1;
while (mid <= high) {
    if (nums[mid] == 0) swap(nums[low++], nums[mid++]);
    else if (nums[mid] == 1) mid++;
    else swap(nums[mid], nums[high--]);
}
```

**Merge from End (avoid overwriting):**
```java
int i = m - 1, j = n - 1, k = m + n - 1;
while (j >= 0) {
    if (i >= 0 && nums1[i] > nums2[j]) nums1[k--] = nums1[i--];
    else nums1[k--] = nums2[j--];
}
```

**Remove Duplicates (slow/fast pointer):**
```java
int i = 0;
for (int j = 1; j < n; j++) {
    if (nums[i] != nums[j]) { i++; nums[i] = nums[j]; }
}
return i + 1;
```

**Trapping Rainwater (left/right max):**
```java
int left = 0, right = n - 1, leftMax = 0, rightMax = 0, total = 0;
while (left < right) {
    if (height[left] <= height[right]) {
        if (leftMax > height[left]) total += leftMax - height[left];
        else leftMax = height[left];
        left++;
    } else {
        if (rightMax > height[right]) total += rightMax - height[right];
        else rightMax = height[right];
        right--;
    }
}
```

**Problems:** Q3, Q5, Q9, Q19, Q25, Q20, Q26, Q27

---

### B. 🪟 Sliding Window
**🔍 How to Identify:**
- Longest/shortest subarray/substring with a condition
- Window expands and shrinks dynamically
- Keywords: "longest substring without repeating", "longest subarray with sum K (positive only)"

**📋 Universal Template:**
```java
int l = 0, r = 0, maxLen = 0;
while (r < n) {
    // Expand window: add nums[r]
    // Shrink window while condition violated
    while (condition_violated) { remove nums[l]; l++; }
    maxLen = Math.max(maxLen, r - l + 1);
    r++;
}
```

**Problems:** Q24, Q22 (positive only)

---

### C. 📊 Prefix Sum / Prefix XOR
**🔍 How to Identify:**
- Subarray sum/XOR equals K (with negatives possible)
- Keywords: "subarray sum equals K", "subarray XOR equals K", "count subarrays"

**📋 Universal Templates:**

**Prefix Sum + HashMap:**
```java
Map<Integer, Integer> map = new HashMap<>();
int sum = 0, maxLen = 0;
for (int i = 0; i < n; i++) {
    sum += nums[i];
    if (sum == k) maxLen = i + 1;
    if (map.containsKey(sum - k)) maxLen = Math.max(maxLen, i - map.get(sum - k));
    if (!map.containsKey(sum)) map.put(sum, i);
}
```

**Prefix XOR + HashMap:**
```java
Map<Integer, Integer> map = new HashMap<>();
map.put(0, 1);
int xr = 0, cnt = 0;
for (int i = 0; i < n; i++) {
    xr ^= nums[i];
    cnt += map.getOrDefault(xr ^ k, 0);
    map.put(xr, map.getOrDefault(xr, 0) + 1);
}
```

**Problems:** Q22 (negatives), Q23

---

### D. 📈 Kadane's Algorithm
**🔍 How to Identify:** Maximum/minimum sum of contiguous subarray

**📋 Universal Template:**
```java
int maxSum = Integer.MIN_VALUE, currSum = 0;
for (int num : nums) {
    currSum = Math.max(currSum + num, num);
    maxSum = Math.max(maxSum, currSum);
}
```

**Problems:** Q4

---

### E. 🗂️ HashMap / HashSet
**🔍 How to Identify:** Need O(1) lookup for complement/previous element

**📋 Universal Templates:**

**HashMap (Two Sum with indices):**
```java
Map<Integer, Integer> map = new HashMap<>();
for (int i = 0; i < n; i++) {
    if (map.containsKey(target - nums[i])) return {map.get(target - nums[i]), i};
    map.put(nums[i], i);
}
```

**HashSet (Longest Consecutive Sequence):**
```java
Set<Integer> st = new HashSet<>();
for (int it : st) {
    if (!st.contains(it - 1)) {
        int cnt = 1, x = it;
        while (st.contains(x + 1)) { x++; cnt++; }
        longest = Math.max(longest, cnt);
    }
}
```

**Problems:** Q19 (HashMap), Q21

---

### F. 🗳️ Boyer-Moore Voting
**🔍 How to Identify:** Find element appearing > n/2 or > n/3 times, O(1) space

**📋 Universal Template:**
```java
// For n/2: 1 candidate
int cnt = 0, el = 0;
for (int num : nums) {
    if (cnt == 0) { cnt = 1; el = num; }
    else if (el == num) cnt++;
    else cnt--;
}
// Verify: count occurrences of el

// For n/3: 2 candidates
int cnt1 = 0, cnt2 = 0, el1 = MIN, el2 = MIN;
for (int num : nums) {
    if (cnt1 == 0 && el2 != num) { cnt1 = 1; el1 = num; }
    else if (cnt2 == 0 && el1 != num) { cnt2 = 1; el2 = num; }
    else if (num == el1) cnt1++;
    else if (num == el2) cnt2++;
    else { cnt1--; cnt2--; }
}
// Verify both candidates
```

**Problems:** Q15, Q16

---

### G. 🔍 Binary Search
**🔍 How to Identify:** Search in sorted space, compute x^n efficiently

**📋 Universal Templates:**

**Binary Search on Flattened Matrix:**
```java
int low = 0, high = n * m - 1;
while (low <= high) {
    int mid = (low + high) / 2;
    int row = mid / m, col = mid % m;
    if (mat[row][col] == target) return true;
    else if (mat[row][col] < target) low = mid + 1;
    else high = mid - 1;
}
```

**Binary Exponentiation (x^n):**
```java
double pow(double x, long n) {
    if (n == 0) return 1.0;
    if (n % 2 == 0) return pow(x * x, n / 2);
    return x * pow(x, n - 1);
}
```

**Problems:** Q13, Q14

---

### H. 🔄 Merge Sort (Divide & Conquer)
**🔍 How to Identify:** Count inversions / reverse pairs during sort

**📋 Universal Template:**
```java
int mergeSort(int[] nums, int low, int high) {
    if (low >= high) return 0;
    int mid = (low + high) / 2;
    int cnt = mergeSort(nums, low, mid) + mergeSort(nums, mid+1, high);
    cnt += countPairs(nums, low, mid, high); // Count BEFORE merge
    merge(nums, low, mid, high);
    return cnt;
}
```

**Problems:** Q12, Q18

---

### I. 🔁 Floyd's Cycle Detection
**🔍 How to Identify:** Array values in [1, n] range → forms linked list with cycle

**📋 Universal Template:**
```java
int slow = nums[0], fast = nums[0];
do { slow = nums[slow]; fast = nums[nums[fast]]; } while (slow != fast);
slow = nums[0];
while (slow != fast) { slow = nums[slow]; fast = nums[fast]; }
return slow;
```

**Problems:** Q10

---

### J. 🧮 Matrix Manipulation
**🔍 How to Identify:** Input is 2D matrix, modify rows/columns

**📋 Universal Template:**
```
1. In-place markers: use first row/column as markers (Q1)
2. Transpose + Reverse: for 90° rotation (Q7)
3. Flatten index: row = mid / cols, col = mid % cols (Q13)
```

**Problems:** Q1, Q7

---

### K. 📐 Math / Combinatorics / DP
**🔍 How to Identify:** Compute nCr, Pascal's triangle, grid paths

**📋 Universal Template (Grid Unique Paths - Space Optimized):**
```java
int[] prev = new int[n];
for (int i = 0; i < m; i++) {
    int[] temp = new int[n];
    for (int j = 0; j < n; j++) {
        if (i == 0 && j == 0) { temp[j] = 1; continue; }
        int up = (i > 0) ? prev[j] : 0;
        int left = (j > 0) ? temp[j-1] : 0;
        temp[j] = up + left;
    }
    prev = temp;
}
```

**Problems:** Q2, Q17

---

### L. 🔗 Sorting + Merging
**🔍 How to Identify:** Array of intervals, merge overlapping

**📋 Universal Template:**
```java
Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
List<int[]> ans = new ArrayList<>();
ans.add(intervals[0]);
for (int i = 1; i < n; i++) {
    int[] last = ans.get(ans.size() - 1);
    if (intervals[i][0] <= last[1]) last[1] = Math.max(last[1], intervals[i][1]);
    else ans.add(intervals[i]);
}
```

**Problems:** Q8

---

### M. ✏️ Sign Marking / In-place
**🔍 How to Identify:** Array values in [1, n] range, find missing + repeating without extra space

**📋 Universal Template:**
```java
for (int i = 0; i < n; i++) {
    int idx = Math.abs(nums[i]) - 1;
    if (nums[idx] > 0) nums[idx] = -nums[idx];
    else repeating = idx + 1;
}
for (int i = 0; i < n; i++) if (nums[i] > 0) missing = i + 1;
```

**Problems:** Q11

---

### N. 💰 Greedy
**🔍 How to Identify:** Buy low, sell high, local optimal → global optimal

**📋 Universal Template:**
```java
int minPrice = Integer.MAX_VALUE, maxProfit = 0;
for (int price : prices) {
    minPrice = Math.min(minPrice, price);
    maxProfit = Math.max(maxProfit, price - minPrice);
}
```

**Problems:** Q6

---

### O. 🔢 Linear Scan
**🔍 How to Identify:** Simple traversal with running count/tracker

**📋 Universal Template:**
```java
int cnt = 0, maxi = 0;
for (int num : nums) {
    if (num == target) { cnt++; maxi = Math.max(maxi, cnt); }
    else cnt = 0;
}
```

**Problems:** Q28

---

📄 **[← Back to Part 1 (Q1-Q14)](Striver_SDE_Sheet_Notes_Part1.md)**
</task_progress>
</write_to_file>
