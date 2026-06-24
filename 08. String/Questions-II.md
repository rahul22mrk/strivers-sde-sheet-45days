1. `Reverse every word in a string`
2. `Longest Palindrome in a string `
3. `Roman to Integer `
4. `Implement ATOI/STRSTR `
5. `Longest Common Prefix `
6. `Rabin Karp Algorithm `
7. `Z function `
8. `KMP Algorithm or LPS array `
9. `Minimum insertions to make string palindrome `
10. `Valid Anagram `
11. `Count and say `
12. `Compare version numbers `
    
************************************************************************************


7. `Z function 

```java

```


8. `KMP Algorithm or LPS array 


```java
//brute
import java.util.*;

class Solution {
    // Compute the Longest prefix suffix array for the combined string
    private int[] computeLPS(String s) {
        int n = s.length(); // size of string

        // To store the longest prefix suffix
        int[] LPS = new int[n];

        // Iterate on the string
        for (int i = 1; i < n; i++) {

            // For all possible lengths
            for (int len = 1; len < i; len++) {
                if (s.substring(0, len).equals(s.substring(i - len + 1, i + 1))) {
                    LPS[i] = len;
                }
            }
        }

        return LPS; // Return the computed LPS array
    }

    // Function to find all indices of pattern in text
    public List<Integer> search(String pattern, String text) {
        String s = pattern + '$' + text; // Combined string

        // Function call to find the LPS array for the combined string
        int[] LPS = computeLPS(s);

        // Length of pattern and text
        int n = text.length(), m = pattern.length();

        // To store the result
        List<Integer> ans = new ArrayList<>();

        // Iterate on the combined string after the delimiter
        for (int i = m + 1; i < s.length(); i++) {
            if (LPS[i] == m) ans.add(i - 2 * m);
        }

        return ans;
    }

}
```

```
Complexity Analysis:
Time Complexity: O(N3), where N is the length of the combined string
Computing the LPS array takes O(N3) time and finding the matches requires iterating on the LPS taking O(N) time.

Space Complexity: O(N), to store the LPS array.
```

```java
//optimal
import java.util.*;

class Solution {
    // Compute the Longest prefix suffix array for the combined string
    private int[] computeLPS(String s) {
        int n = s.length(); // size of string

        // To store the longest prefix suffix
        int[] LPS = new int[n];

        int i = 1, j = 0;

        // Iterate on the string
        while (i < n) {
            // If the character matches
            if (s.charAt(i) == s.charAt(j)) {
                LPS[i] = j + 1;
                i++;
                j++;
            }

            // Otherwise
            else {
                // Trace back j pointer till it does not match
                while (j > 0 && s.charAt(i) != s.charAt(j)) {
                    j = LPS[j - 1];
                }

                // If a match is found
                if (s.charAt(i) == s.charAt(j)) {
                    LPS[i] = j + 1;
                    j++;
                }
                i += 1;
            }
        }

        return LPS; // Return the computed LPS array
    }

    // Function to find all indices of pattern in text
    public List<Integer> search(String pattern, String text) {
        String s = pattern + '$' + text; // Combined string

        // Function call to find the LPS array for the combined string
        int[] LPS = computeLPS(s);

        // Length of pattern and text
        int n = text.length(), m = pattern.length();

        // To store the result
        List<Integer> ans = new ArrayList<>();

        // Iterate on the combined string after the delimiter
        for (int i = m + 1; i < s.length(); i++) {
            if (LPS[i] == m) ans.add(i - 2 * m);
        }

        return ans;
    }
}
```

```
Complexity Analysis:
Time Complexity: O(N), where N is the length of the combined string
Computing the LPS array takes O(N) time:

The function iterates over the string (s) using i, which goes from 1 to N.
Inside the loop:
If s[i] == s[j], both i and j are incremented → Constant time operation.
If s[i] != s[j], j is backtracked using LPS[j-1] inside a while loop.

Note that the total number of backtracking steps (decrementing j via LPS[j-1]) across all iterations is at most n because:
```


9. `Minimum insertions to make string palindrome` 


```java
//tabulation
import java.util.*;

class Solution {
    /* Function to calculate the length 
    of the Longest Common Subsequence*/
    private int lcs(String s1, String s2) {
        int n = s1.length();
        int m = s2.length();

        int[][] dp = new int[n + 1][m + 1];

        // Initialize the first row and first column to 0
        for (int i = 0; i <= n; i++) {
            dp[i][0] = 0;
        }
        for (int i = 0; i <= m; i++) {
            dp[0][i] = 0;
        }

        for (int ind1 = 1; ind1 <= n; ind1++) {
            for (int ind2 = 1; ind2 <= m; ind2++) {
                if (s1.charAt(ind1 - 1) == s2.charAt(ind2 - 1))
                    dp[ind1][ind2] = 1 + dp[ind1 - 1][ind2 - 1];
                else
                    dp[ind1][ind2] = Math.max(dp[ind1 - 1][ind2], dp[ind1][ind2 - 1]);
            }
        }
        // Return the result
        return dp[n][m];
    }
    /* Function to calculate the length of 
    the Longest Palindromic Subsequence*/
    private int longestPalindromeSubsequence(String s) {
        String t = new StringBuilder(s).reverse().toString();
        return lcs(s, t);
    }
    /* Function to calculate the minimum insertions
    required to make a string palindrome*/
    public int minInsertion(String s) {
        int n = s.length();
        int k = longestPalindromeSubsequence(s);

        /* The minimum insertions required is the
        difference between the string length and
        its longest palindromic subsequence length*/
        return n - k;
    }
}
```

```
Complexity Analysis:
Time Complexity: O(N*N), Where N is the length of the given string. As two nested loops are used to solve the problem.

Space Complexity: O(N*N), We are using an external array of size (N*N).
```

```java
//space optimization
import java.util.*;

class Solution {
    /* Function to calculate the length 
    of the Longest Common Subsequence*/
    private int lcs(String s1, String s2) {
        int n = s1.length();
        int m = s2.length();

        /* Create two arrays to store the 
        previous and current rows of DP values */
        int[] prev = new int[m + 1];
        int[] cur = new int[m + 1];

        /* Base Case is covered as we have
        initialized the prev and cur to 0. */

        for (int ind1 = 1; ind1 <= n; ind1++) {
            for (int ind2 = 1; ind2 <= m; ind2++) {
                if (s1.charAt(ind1 - 1) == s2.charAt(ind2 - 1))
                    cur[ind2] = 1 + prev[ind2 - 1];
                else
                    cur[ind2] = Math.max(prev[ind2], cur[ind2 - 1]);
            }
            // Update the prev array with current values
            System.arraycopy(cur, 0, prev, 0, m + 1);
        }
        // The value at prev[m] contains length of LCS
        return prev[m];
    }
    /* Function to calculate the length of 
    the Longest Palindromic Subsequence*/
    private int longestPalindromeSubsequence(String s) {
        String t = new StringBuilder(s).reverse().toString();
        return lcs(s, t);
    }
    /* Function to calculate the minimum insertions
    required to make a string palindrome*/
    public int minInsertion(String s) {
        int n = s.length();
        int k = longestPalindromeSubsequence(s);

        /* The minimum insertions required is the
        difference between the string length and
        its longest palindromic subsequence length*/
        return n - k;
    }
}
```

```
Complexity Analysis:
Time Complexity: O(N*N), Where N is the length of the given string. As two nested loops are used to solve the problem.

Space Complexity: O(N), We are using an external array of size ‘N+1’ to store only two rows.
```


10. `Valid Anagram `


```java
//brute
import java.util.Arrays;

class Solution {
    public boolean anagramStrings(String s, String t) {
        // If lengths are not equal, they cannot be anagrams
        if (s.length() != t.length()) return false;

        // Convert strings to char arrays and sort them
        char[] sArray = s.toCharArray();
        char[] tArray = t.toCharArray();
        Arrays.sort(sArray);
        Arrays.sort(tArray);

        // Compare sorted arrays
        return Arrays.equals(sArray, tArray);
    }
}
```

```
Complexity Analysis
Time Complexity: O(N log N) due to sorting each string.

Space Complexity: O(1) as no additional data structures are used. Note that for Java, the space complexity will be O(N) due to the creation of additional character arrays. And for Python, the space complexity will be O(N) due to the use of sorted() function, which creates a new string to hold the sorted string.
```

```java
//optimal
import java.util.*;

class Solution {
    public boolean anagramStrings(String s, String t) {
        // Edge Cases
        if (s.length() != t.length()) return false;

        // To store the count of each character
        int[] count = new int[26];

        // Count occurrence of each character in first string 
        for (char c : s.toCharArray()) count[c - 'a']++;

        // Decrement the count for each character in the second string
        for (char c : t.toCharArray()) count[c - 'a']--;

        // Check for count of every character
        for (int i : count) {
            // If the count is not zero
            if (i != 0) return false; // Return false
        }

        // Otherwise strings are anagram
        return true;
    }
}

```

```
Complexity Analysis:
Time Complexity: O(N), where N is the length of the string

Space Complexity: O(1), as there is always a constant-size array (of length 26) used to store the frequencies that does not depend on the length of the strings.
```


11. `Count and say `



```java
import java.util.*;

class Solution {
    /* Recursive function to return the nth 
    term of the count-and-say sequence */
    public String countAndSay(int n) {
        if (n == 1) return "1";
        
        // Recursive call
        String prev = countAndSay(n - 1);
        int len = prev.length();
        
        // To store the answer
        String ans = ""; 
        
        // To count the frequency of identicals
        int count = 1;  
        
        // Traverse the string 
        for (int i = 1; i < len; i++) {
            // If identicals are found, increment the counter
            if (prev.charAt(i) == prev.charAt(i - 1)) count++;
            
            // Otherwise
            else {
                ans += (char) ('0' + count); // Add frequency
                ans += prev.charAt(i - 1); // Add the digit 
                count = 1; // Reset counter to 1
            }
        }
        
        // Adding the frequency for the last digit and the last digit
        ans += (char) ('0' + count);
        ans += prev.charAt(len - 1); 
        
        return ans; // Return the result
    }
}

```

```
Complexity Analysis:
Time Complexity: Can't be determined exactly
Because it will depend on the length of the string in each step which is uncertain.

Space Complexity: Can't be determined exactly
The recursive stack space will take O(n) space and during each step, the string generated will take a variable space.
```


12. `Compare version numbers `

```java
import java.util.*;

class Solution {
    // Function to compare two version strings
    public int compareVersion(String version1, String version2) {
        // Splitting both versions by '.'
        String[] v1 = version1.split("\\.");
        String[] v2 = version2.split("\\.");

        // Finding max length
        int n = Math.max(v1.length, v2.length);

        // Padding zeros and comparing
        for (int i = 0; i < n; i++) {
            int num1 = i < v1.length ? Integer.parseInt(v1[i]) : 0;
            int num2 = i < v2.length ? Integer.parseInt(v2[i]) : 0;

            if (num1 > num2) return 1;
            if (num1 < num2) return -1;
        }
        return 0;
    }
}
```
```
Time and Space Complexity
Time Complexity: O(n), where n is the number of segments in the longer version string, since we split and compare each segment once.
Space Complexity: O(n), as we store the split segments of both version strings for comparison.
```



