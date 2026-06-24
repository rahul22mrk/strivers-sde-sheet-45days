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

1. `Reverse every word in a string`

```java
//Brute Force App..
import java.util.*;

class Solution {
    // Function to reverse every word in the given string
    public String reverseWords(String s) {
        int n = s.length(); // Length of string
        
        // List to store the words present in string
        List<String> words = new ArrayList<>();
        
        // Pointers to mark the start and end of a word
        int start, end;
        
        int i = 0;
        while (i < n) {
            
            // Finding the first character of a word (if any)
            while (i < n && s.charAt(i) == ' ') i++;
            
            // If no word is found, break 
            if (i >= n) break;
            
            start = i; // Storing the index of first character of word
            
            // Finding the last character of the word
            while (i < n && s.charAt(i) != ' ') i++;
            
            end = i - 1; // Storing the index of last character of word
            
            // Add the found word to the list of words
            String wordFound = s.substring(start, end + 1);
            words.add(wordFound);
        }
        
        StringBuilder ans = new StringBuilder();
        
        // Adding all the words to result in the reverse order 
        for (int j = words.size() - 1; j >= 0; j--) {
            ans.append(words.get(j));
            
            // Adding spaces in between words
            if (j != 0) ans.append(' ');
        }
        
        return ans.toString(); // Return the stored result
    }
}

class Main {
    public static void main(String[] args) {
        String s = " amazing coding skills ";
        
        // Creating an instance of Solution class
        Solution sol = new Solution();
        
        // Function call to reverse every word in the given string
        String ans = sol.reverseWords(s);
        
        // Output
        System.out.println("Input string: " + s);
        System.out.println("After reversing every word: " + ans);
    }
}
Complexity Analysis:
Time Complexity: O(n) (where n is the length of the input string)

The input string is scanned once to extract words, taking O(n) time, where n is the length of the input string.
Each word is stored in a list and then concatenated in reverse order, which also takes O(n).


Space Complexity: O(n)
The words list stores each extracted word, requiring O(k) space, where k is the total number of characters in all words (essentially O(n)).
The result string requires O(n) space as well.
```

```java
//optimal
import java.util.*;

class Solution {
    private void reverseString(StringBuilder s, int start, int end) {
        while (start < end) {
            char temp = s.charAt(start);
            s.setCharAt(start, s.charAt(end));
            s.setCharAt(end, temp);
            start++;
            end--;
        }
    }

    public String reverseWords(String s) {
        int n = s.length();

        // Reverse the entire string
        StringBuilder sb = new StringBuilder(s);
        reverseString(sb, 0, n - 1);

        int i = 0, j = 0, start = 0, end = 0;

        while (j < n) {
            // Skip spaces
            while (j < n && sb.charAt(j) == ' ') j++;
            if (j == n) break;

            start = i;

            // Copy the word characters forward
            while (j < n && sb.charAt(j) != ' ') {
                if (i < sb.length()) {
                    sb.setCharAt(i++, sb.charAt(j++));
                } else {
                    sb.append(sb.charAt(j++));
                    i++;
                }
            }

            end = i - 1;

            // Reverse the current word using start and end
            reverseString(sb, start, end);

            // Add a space after the word if it's not the last word
            if (j < n) {
                if (i < sb.length()) {
                    sb.setCharAt(i++, ' ');
                } else {
                    sb.append(' ');
                    i++;
                }
            }
        }

        // Remove trailing space if present
        if (i > 0 && sb.charAt(i - 1) == ' ') i--;

        return sb.substring(0, i);
    }
}

class Main {
    public static void main(String[] args) {
        String s = " amazing coding skills ";

        // Creating an instance of Solution class
        Solution sol = new Solution();

        // Function call to reverse every word in the given string
        String ans = sol.reverseWords(s);

        // Output
        System.out.println("Input string: " + s);
        System.out.println("After reversing every word: " + ans);
    }
}
Complexity Analysis:
Time Complexity: O(n) (where n is the length of the input string)

The input string is reversed firstly, taking O(n) time.
The string is then traversed taking another O(n) time.
Every word encountered in the string is again reversed taking overall O(n) time.


Space Complexity: O(1)
There is no additional space used. The reversal is done in-place taking O(1) or constant space.

Note that the space complexity for the Java solution will be O(N) because of the conversion of the given String to a character array. This is done because Strings are immutable in Java.
```


2. `Longest Palindrome in a string `

```java
// Definition and Solution class
class Solution {

    // Helper function to expand around a center
    private int[] expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return new int[]{left + 1, right - 1};
    }

    // Function to return longest palindromic substring
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n == 0) return "";

        int start = 0, end = 0;

        // Iterate through each index as potential center
        for (int i = 0; i < n; i++) {
            // Odd-length palindrome
            int[] odd = expandAroundCenter(s, i, i);
            // Even-length palindrome
            int[] even = expandAroundCenter(s, i, i + 1);

            // Update longest palindrome
            if (odd[1] - odd[0] > end - start) {
                start = odd[0];
                end = odd[1];
            }
            if (even[1] - even[0] > end - start) {
                start = even[0];
                end = even[1];
            }
        }

        return s.substring(start, end + 1);
    }
}

// Driver class
public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        String s = "babad";
        System.out.println(sol.longestPalindrome(s)); // Output: "bab" or "aba"
    }
}
Time and Space Complexity
Time Complexity: O(n²), where n is the length of the string. For each character, we expand around it for both odd and even length palindromes, which takes O(n) for each center.
Space Complexity: O(1), as we only use a few variables to track indices and do not allocate extra space proportional to the input size.
```

3. `Roman to Integer `
```java
import java.util.*;

class Solution {
    // Function to convert roman numeral into an integer
    public int romanToInt(String s) {
        // Mapping Roman symbols to their integer values
        Map<Character, Integer> mp = new HashMap<>();
        mp.put('I', 1); mp.put('V', 5); mp.put('X', 10); mp.put('L', 50);
        mp.put('C', 100); mp.put('D', 500); mp.put('M', 1000);

        // Result variable
        int total = 0;

        // Traverse the string
        for(int i = 0; i < s.length(); i++) {
            // If next value is larger, subtract current value
            if(i + 1 < s.length() && mp.get(s.charAt(i)) < mp.get(s.charAt(i+1))) {
                total -= mp.get(s.charAt(i));
            }
            // Otherwise add current value
            else {
                total += mp.get(s.charAt(i));
            }
        }
        return total;
    }
}

class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        System.out.println(sol.romanToInt("MCMXCIV"));
    }
}
Time and Space Complexity
Time Complexity: O(n), where n is the length of the Roman numeral string. Each character is processed once, with constant-time lookups in the mapping.
Space Complexity: O(1), since the mapping of Roman symbols is constant in size and no additional data structures grow with input length.
```


4. `Implement ATOI/STRSTR `
```java
//brute
class Solution {
    // Method to convert string to integer
    public int myAtoi(String input) {
        int i = 0, n = input.length();
        
        // Step 1: Skip leading spaces
        while (i < n && input.charAt(i) == ' ') {
            i++;
        }
        
        // Step 2: Handle the sign
        int sign = 1;
        if (i < n && input.charAt(i) == '-') {
            sign = -1;
            i++;
        } else if (i < n && input.charAt(i) == '+') {
            i++;
        }
        
        // Step 3: Parse digits and build the number
        long result = 0;
        while (i < n && Character.isDigit(input.charAt(i))) {
            result = result * 10 + (input.charAt(i) - '0');
            i++;
            
            // Step 4: Handle overflow and underflow
            if (result * sign >= Integer.MAX_VALUE) {
                return Integer.MAX_VALUE;
            }
            if (result * sign <= Integer.MIN_VALUE) {
                return Integer.MIN_VALUE;
            }
        }
        
        // Step 5: Return the final result with the sign
        return (int)(result * sign);
    }
}

public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        
        // Example input
        String input = "42";
        
        // Output the result of converting the string to an integer
        int result = sol.myAtoi(input);
        System.out.println(result);  // Expected output: 42
    }
}
Complexity Analysis
Time Complexity: O(n), where n is the length of the string. We scan the string once.
Space Complexity: O(1), as we use a constant amount of extra space.
```

```java
//optimal
class Solution {
    // Method to convert a string to an integer
    public int myAtoi(String input) {
        int i = 0, n = input.length();
        
        // Step 1: Skip leading spaces
        while (i < n && input.charAt(i) == ' ') {
            i++;
        }

        // Step 2: Determine the sign
        int sign = 1;
        if (i < n && input.charAt(i) == '-') {
            sign = -1;
            i++;
        } else if (i < n && input.charAt(i) == '+') {
            i++;
        }

        // Step 3: Parse digits and calculate result
        long result = 0;
        while (i < n && Character.isDigit(input.charAt(i))) {
            result = result * 10 + (input.charAt(i) - '0');
            i++;

            // Step 4: Check for overflow
            if (result * sign >= Integer.MAX_VALUE) {
                return Integer.MAX_VALUE;
            }
            if (result * sign <= Integer.MIN_VALUE) {
                return Integer.MIN_VALUE;
            }
        }

        // Step 5: Return the result with sign
        return (int)(result * sign);
    }
}

public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        
        // Example input
        String input = "42";
        
        // Output the result of converting the string to an integer
        int result = sol.myAtoi(input);
        System.out.println(result);  // Expected output: 42
    }
}
Complexity Analysis
Time Complexity: O(n), where n is the length of the string. Each character is processed once.
Space Complexity: O(1), since only a fixed number of variables are used.
```



5. `Longest Common Prefix `
```java
import java.util.Arrays;

class Solution {
    // Method to find the longest common prefix in an array of strings
    public String longestCommonPrefix(String[] v) {
        // Use StringBuilder to build the result
        StringBuilder ans = new StringBuilder();
        
        // Sort the array to get the lexicographically smallest and largest strings
        Arrays.sort(v);
        // First string (smallest in sorted order)
        String first = v[0]; 
         // Last string (largest in sorted order)
        String last = v[v.length - 1];
        
        // Compare characters of the first and last strings
        for (int i = 0; i < Math.min(first.length(), last.length()); i++) {
            // If characters don't match, return the current prefix
            if (first.charAt(i) != last.charAt(i)) {
                return ans.toString();
            }
            // Append the matching character to the result
            ans.append(first.charAt(i));
        }
        
        // Return the longest common prefix found
        return ans.toString();
    }
    
    // Main method to test the longestCommonPrefix method
    public static void main(String[] args) {
        Solution solution = new Solution();
        String[] input = {"flower", "flow", "flight"};
        String result = solution.longestCommonPrefix(input);
        System.out.println("Longest Common Prefix: " + result); // Output: "fl"
    }
}
Complexity Analysis
Time Complexity: O(N * M * log N), where N is the number of strings and M is the maximum length of a string.
The sorting operation takes O(N * M * log N) time because:

Comparing two strings during sort costs up to O(M) (character-by-character comparison).
Sorting does O(N*logN) comparisons.

and the comparison of characters in the first and last strings takes O(M) time, which is dominated by the sorting step making the overall time complexity as O(N * M * logN).
Space Complexity: O(M), as the ans variable can store the length of the prefix which in the worst case will be O(M).
```




6. `Rabin Karp Algorithm `
```java
//brute force

import java.util.*;

class Solution {
    // Function to find the starting index of all occurrences of pattern in text
    public List<Integer> search(String pat, String txt) {
        int n = pat.length();
        int m = txt.length();

        // List to store the result
        List<Integer> ans = new ArrayList<>();

        // Traverse the text string
        for (int i = 0; i <= m - n; i++) {
            boolean flag = true;

            // Check for every character in pattern
            for (int j = 0; j < n; j++) {

                // If characters does not match
                if (txt.charAt(i + j) != pat.charAt(j)) {
                    flag = false; // Set the flag as false
                    break;
                }
            }

            // if the pattern is found, store the index
            if (flag) ans.add(i);
        }

        return ans; // Return the stored result
    }
}

class Main {
    public static void main(String[] args) {
        String txt = "ababcabcababc";
        String pat = "abc";

        // Creating an instance of Solution class
        Solution sol = new Solution();

        /* Function call to find the starting index
           of all occurrences of pattern in text */
        List<Integer> ans = sol.search(pat, txt);

        // Output
        System.out.print("The starting indices of all occurrences of " + pat + " in " + txt + " are: ");
        for (int it : ans) System.out.print(it + " ");
    }
}
Complexity Analysis:
Time Complexity: O(M*N) (where M and N are the lengths of text and pattern respectively)
The outer loop iterates (M-N+1) times. For each position, the inner loop (the character-by-character comparison between the pattern and text) takes O(N) time in the worst case. taking overall O(M*N) time.

Space Complexity: O(K), because the code uses a constant space and the output list requires O(K) space where K is the number of times the pattern appears in the text.
```

```java
//optimal
import java.util.*;

class Solution {
    // Function to find the starting index of all occurrences of pattern in text
    public List<Integer> search(String pat, String txt) {
        int n = pat.length();
        int m = txt.length();

        // Primes for Rabin-Karp algorithm
        int p = 7, mod = 101;

        // To store the hash values of pattern and substring of text
        int hashPat = 0, hashText = 0;

        int pRight = 1, pLeft = 1;

        // Computing the initial hash values
        for (int i = 0; i < n; i++) {
            hashPat = (hashPat + ((pat.charAt(i) - 'a' + 1) * pRight) % mod) % mod;
            hashText = (hashText + ((txt.charAt(i) - 'a' + 1) * pRight) % mod) % mod;
            pRight = (pRight * p) % mod;
        }

        // List to store the result
        List<Integer> ans = new ArrayList<>();

        // Traverse the text string
        for (int i = 0; i <= m - n; i++) {

            // If the hash value matches
            if (hashPat == hashText) {
                // Add the index of the result if the substring matches
                if (txt.substring(i, i + n).equals(pat)) ans.add(i);
            }

            // Updating the hash values
            if (i < m - n) {
                hashText = (hashText - ((txt.charAt(i) - 'a' + 1) * pLeft) % mod + mod) % mod;
                hashText = (hashText + ((txt.charAt(i + n) - 'a' + 1) * pRight) % mod) % mod;
                hashPat = (hashPat * p) % mod;

                // Updating the prime multiples
                pLeft = (pLeft * p) % mod;
                pRight = (pRight * p) % mod;
            }
        }

        return ans; // Return the stored result
    }
}

class Main {
    public static void main(String[] args) {
        String txt = "ababcabcababc";
        String pat = "abc";

        // Creating an instance of Solution class
        Solution sol = new Solution();

        /* Function call to find the starting index
           of all occurrences of pattern in text */
        List<Integer> ans = sol.search(pat, txt);

        // Output
        System.out.print("The starting indices of all occurrences of " + pat + " in " + txt + " are: ");
        for (int it : ans) System.out.print(it + " ");
    }
}

Complexity Analysis:
Time Complexity: O(M), where M is the length of the text string.

The pattern and first substring's hash values are computed in O(N). The algorithm slides the window across the taking text O(M-N+1) time. In the best/average case, hash collisions are rare, meaning most iterations involve only constant-time hash updates. Thus, the overall time complexity comes out to be O(M).
```
