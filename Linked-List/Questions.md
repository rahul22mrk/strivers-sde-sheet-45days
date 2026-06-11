1.    Reverse a LL
2.    Find Middle of Linked List
3.    Merge two Sorted Lists
4.    Remove Nth node from the back of the LL
5.    Add two numbers as LinkedList
6.    Delete Node in a Linked List O(1)
7.    Find the intersection point of Y LL
8.    Detect a loop in LL
9.    Reverse LL in group of given size K
10.   Check if LL is palindrome or not
11.   Find the starting point in LL
12.   Flattening of LL
13.   Rotate a LL
14.   Clone a LL with random and next pointer

**********************************************************************************************

1.    Reverse a LL
Iterative Approach : 
 /*Function to reverse a linked list
    Using the 3-pointer approach*/
    public ListNode reverseList(ListNode head) {
        /*Initialize 'temp' at
        head of linked list*/
        ListNode temp = head;
        
        /*Initialize pointer 'prev' to NULL,
        representing the previous node*/
        ListNode prev = null;
        
        /*Traverse the list, continue till
        'temp' reaches the end (NULL)*/
        while (temp != null) {
            /* Store the next node in
            'front' to preserve the reference*/
            ListNode front = temp.next;
            
            /*Reverse the direction of the
            current node's 'next' pointer
            to point to 'prev'*/
            temp.next = prev;
            
            /*Move 'prev' to the current
            node for the next iteration*/
            prev = temp;
            
            /*Move 'temp' to the 'front' node
            advancing the traversal*/
            temp = front;
        }
        
        /*Return the new head of
        the reversed linked list*/
        return prev;
    }
Complexity Analysis
Time Complexity: O(N) because the algorithm traverses the entire linked list once, where 'N' is the number of nodes in the list. Since each node is visited exactly once during the traversal, the time complexity is linear, O(N).

Space Complexity: O(1) because the algorithm uses only a constant amount of additional space. This is achieved by utilizing three pointers (prev, temp, and front) to reverse the list without any significant extra memory usage, resulting in constant space complexity, O(1).

Recusrive:
 /* Function to reverse a singly linked list using recursion */
    public ListNode reverseList(ListNode head) {
        /* Base case:
        If the linked list is empty or has only one node,
        return the head as it is already reversed. */
        if (head == null || head.next == null) {
            return head;
        }
        
        /* Recursive step:
        Reverse the linked list starting 
        from the second node (head.next). */
        ListNode newHead = reverseList(head.next);
        
        /* Save a reference to the node following
        the current 'head' node. */
        ListNode front = head.next;
        
        /* Make the 'front' node point 
        to the current
        'head' node in the 
        reversed order. */
        front.next = head;
        
        /* Break the link from 
        the current 'head' node
        to the 'front' node 
        to avoid cycles. */
        head.next = null;
        
        /* Return the 'newHead,' 
        which is the new
        head of the reversed 
        linked list. */
        return newHead;
    }
}

Complexity Analysis
Time Complexity: O(N), where N is the number of nodes in the linked list. The algorithm traverses the list exactly once through the recursive calls.

Space Complexity: O(N) (Auxiliary Space). The recursive call stack will reach a maximum depth of N before it hits the base case, taking up O(N) space.



2.    Find Middle of Linked List

Brute:
 // Function to get the middle node of linked list
    public ListNode middleOfLinkedList(ListNode head) {
        ListNode temp = head;
        int count = 0;
        
        // Traverse the linked list
        while (temp != null) {
            count += 1; // Increment the count by one 
            temp = temp.next;
        }
        
        int midPosition = (count) / 2 + 1;
        
        ListNode middleNode = head;
        for (int i = 1; i < midPosition; i++) {
            middleNode = middleNode.next;
        }
        
        return middleNode;
    }
}
Complexity Analysis:
Time Complexity: O(N), where N is the number of nodes in the linked list.
Firstly the size of the linked list is determined which takes O(N) time. Then traversing to the middle nodes takes another O(N/2) time. Thus the overall time complexity is O(N) + O(N/2) or O(3N/2) or O(N).

Space Complexity: O(1), as only a couple of variables are used.

Optimal:

 // Function to get the middle node of linked list
    public ListNode middleOfLinkedList(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        // Until the fast pointer reaches NULL or the last node
        while (fast != null && fast.next != null) {
            // Move slow pointer by one step
            slow = slow.next;
            
            // Move fast pointer by two steps
            fast = fast.next.next;
        }
        
        return slow;
    }
Complexity Analysis:
Time Complexity: O(N/2), where N is the number of nodes in the linked list.
The total iterations taken by the fast pointer to reach the end of the linked list are of the order O(N/2).

Space Complexity: O(1), as only a couple of variables are used.



3.    Merge two Sorted Lists

Brute :
 // Function to merge two sorted linked lists
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ArrayList<Integer> arr = new ArrayList<>();
        ListNode temp1 = list1;
        ListNode temp2 = list2;

        
        // Add elements from list1 to the vector
        while (temp1 != null) {
            arr.add(temp1.val);
            // Move to the next node in list1
            temp1 = temp1.next;
        }

        // Add elements from list2 to the vector
        while (temp2 != null) {
            arr.add(temp2.val);
            // Move to the next node in list2
            temp2 = temp2.next;
        }

        // Sorting the vector in ascending order
        Collections.sort(arr);

        // Sorted vector to linked list
        ListNode dummyNode = new ListNode(-1);
        ListNode temp = dummyNode;
        for (int i = 0; i < arr.size(); i++) {
            temp.next = new ListNode(arr.get(i));
            temp = temp.next;
        }

        // Return the head of 
        // merged sorted linked list
        return dummyNode.next;
    }
}
Complexity Analysis
Time Complexity: O(N1 + N2) + O(N log N) + O(N) where N1 is the number of linked list nodes in the first list, N2 is the number of linked list nodes in the second list, and N is the total number of nodes (N1 + N2). Traversing both lists into the array takes O(N1 + N2), sorting the array takes O((N1 + N2) X log(N1 + N2)), and then traversing the sorted array and creating a list gives us another O(N1 + N2).

Space Complexity: O(N) + O(N) where N is the total number of nodes from both lists (N1 + N2). O(N) to store all the nodes of both the lists in an external array and another O(N) to create a new combined list.

Optimal:
-------------------
 // Function to merge two sorted linked lists
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // Create a dummy node to serve as 
        // the head of the merged list
        ListNode dummyNode = new ListNode(-1);
        ListNode temp = dummyNode;

        // Traverse both lists simultaneously
        while (list1 != null && list2 != null) {
            /*Compare elements of both lists 
            and link the smaller node 
            to the merged list*/
            if (list1.val <= list2.val) {
                temp.next = list1;
                list1 = list1.next;
            } else {
                temp.next = list2;
                list2 = list2.next;
            }
            // Move the temporary pointer 
            // to the next node
            temp = temp.next;
        }

        /*If any list still 
        has remaining elements, 
        append them to the merged list*/
        if (list1 != null) {
            temp.next = list1;
        } else {
            temp.next = list2;
        }

        // Return merged list 
        return dummyNode.next;
    }

Complexity Analysis
Time Complexity: O(N1 + N2) because both lists are traversed in a single pass for merging without any additional loops or nested iterations. Here N1 is the number of nodes in the first linked list and N2 is the number of nodes in the second linked list.

Space Complexity: O(1) because no additional data structures or space is allocated for storing data, only a constant space for pointers to maintain for traversing the linked list.

existing code:
ListNode dummyNode = new ListNode(-1);
ListNode temp = dummyNode;

for (int i = 0; i < arr.size(); i++) {
    temp.next = new ListNode(arr.get(i));
    temp = temp.next;
}

return dummyNode.next;

2nd or alternative approach:
ListNode head = null;
ListNode temp = null;

for (int val : arr) {
    ListNode newNode = new ListNode(val);

    if (head == null) {
        head = newNode;
        temp = newNode;
    } else {
        temp.next = newNode;
        temp = temp.next;
    }
}

return head;

4.    Remove Nth node from the back of the LL


Brute:
// Function to remove the nth node from end
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null) {
            return null;
        }
        int cnt = 0;
        ListNode temp = head;

        // Count the number of nodes
        while (temp != null) {
            cnt++;
            temp = temp.next;
        }

        /* If N equals 
        the total number of nodes
        delete the head */
        if (cnt == n) {
            ListNode newHead = head.next;
            return newHead;
        }

        /* Calculate the position 
        of the node to delete (res) */
        int res = cnt - n;
        temp = head;

        /* Traverse to the node 
        just before the one to delete */
        while (temp != null) {
            res--;
            if (res == 0) {
                break;
            }
            temp = temp.next;
        }

        // Delete the Nth node from the end
        ListNode delNode = temp.next;
        temp.next = temp.next.next;
        return head;
    }
Complexity Analysis
Time Complexity: O(L) + O(L-N) We are calculating the length of the linked list and then iterating up to the (L-N)th node of the linked list, where L is the total length of the list and N is the position of the node to delete.

Space Complexity: O(1) as we have not used any extra space.

  optimal:
    
// Function to remove the nth node from end
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Creating pointers
        ListNode fastp = head;
        ListNode slowp = head;

        /* Move the fastp pointer 
        N nodes ahead */
        for (int i = 0; i < n; i++) {
            fastp = fastp.next;
        }

        /* If fastp becomes NULL
        the Nth node from the 
        end is the head */
        if (fastp == null) {
            return head.next;
        }

        /* Move both pointers 
        Until fastp reaches the end */
        while (fastp.next != null) {
            fastp = fastp.next;
            slowp = slowp.next;
        }

        // Delete the Nth node from the end
        slowp.next = slowp.next.next;
        return head;
    }
}

Complexity Analysis
Time Complexity: O(N) since the fast pointer will traverse the entire linked list, where N is the length of the linked list.

Space Complexity: O(1), as we have not used any extra space



5.    Add two numbers as LinkedList


 // Function to add two numbers as linked list
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        /* Dummy node to act as the 
        starting point of the result list */
        ListNode dummy = new ListNode();
        /* Temp pointer to build 
        the result list */
        ListNode temp = dummy;
        // Initialize carry
        int carry = 0;

        /* Iterate while there are nodes in l1 or l2, 
        or there's a carry to process */
        while ((l1 != null || l2 != null) || carry != 0) {
            int sum = 0;

            /* Add the value from l1 
            if available */
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            /* Add the value from l2 
            if available */
            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            // Add the carry
            sum += carry;
            // Update the carry
            carry = sum / 10;

            /* Create a new node with the digit value 
            and attach it to the result list */
            ListNode node = new ListNode(sum % 10);
            temp.next = node;
            /* Move to the 
            next position in the result list */
            temp = temp.next;
        }
        /* Return the result list
        skipping the dummy node */
        return dummy.next;
    }

Complexity Analysis
Time Complexity: O(max(M, N)) Here, M and N represent the sizes of the linked lists l1 and l2, respectively. The algorithm traverses both lists at most once, hence, the time complexity depends on the length of the longer list.

Space Complexity: O(max(M,N)) The length of the new list is at most max(M, N)+1.




6.    Delete Node in a Linked List O(1)

 public void deleteNode(ListNode node) {
        // Copy value from next node
        node.val = node.next.val;
        // Skip the next node
        node.next = node.next.next;
    }

Time and Space Complexity
Time Complexity: O(1), we only copy the next node's value and modify the links between nodes..
Space Complexity: O(1), only a constant amount of extra space is used.





7.    Find the intersection point of Y LL

Brute:
// Function to find the intersection node of two linked lists
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // Create a hash set to store the nodes
        // Of the first list
        HashSet<ListNode> nodes_set = new HashSet<>();

        // Traverse the first linked list
        // And add all its nodes to the set
        while (headA != null) {
            nodes_set.add(headA);
            headA = headA.next;
        }

        // Traverse the second linked list
        // And check for intersection
        while (headB != null) {
            // If a node from the second list is found in the set,
            // It means there is an intersection
            if (nodes_set.contains(headB)) {
                return headB;
            }
            headB = headB.next;
        }

        // No intersection found, return null
        return null;
    }
Complexity Analysis:
Time Complexity: O(N + M), where N and M are the lengths of the first and second linked list respectively.

Traversing the first list and adding the nodes to the hashset takes O(N) time assuming the hashset takes O(1) time for operations. Iterating through all nodes in the second list takes O(M) time. Therefore, the total time complexity is O(N + M).
Note: If the hashset takes logarithmic time for operations, the time complexity get a multiple of logN.

Space Complexity: O(N)
Using an hashset to store the addresses of all nodes in the first list takes O(N) space.

Better:
 public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode temp1 = headA;
        ListNode temp2 = headB;
        int n1 = 0, n2 = 0;

        // Get the length of first linked list
        while (temp1 != null) {
            n1++;
            temp1 = temp1.next;
        }

        // Get the length of second linked list
        while (temp2 != null) {
            n2++;
            temp2 = temp2.next;
        }

        // Traverse the longer list and bring the pointers to same level
        if (n1 < n2) return collisionPoint(headA, headB, n2 - n1);

        return collisionPoint(headB, headA, n1 - n2);
    }

    private ListNode collisionPoint(ListNode smallerListHead, ListNode longerListHead, int len) {
        ListNode temp1 = smallerListHead;
        ListNode temp2 = longerListHead;

        // Adjust the pointers to same level
        for (int i = 0; i < len; i++) temp2 = temp2.next;

        while (temp1 != temp2) {
            temp1 = temp1.next;
            temp2 = temp2.next;
        }

        return temp1;
    }
Complexity Analysis:
Time Complexity: O(N + M), where N and M are the lengths of first and second linked list respectively.

Calculating the lengths of the two linked list takes O(N) and O(M) time. Another O(|N-M|) time is needed for aligning the nodes. The final traversal of the aligned lists takes O(min(N,M)) time in the worst case. Thus, the overall time complexity is O(N + 2M) or O(N + M).

Space Complexity: O(1), because only a couple of pointers are used.

   Optimal: 
 // Function to find the intersection node of two linked lists
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // Edge case
        if (headA == null || headB == null) return null;

        // Initialize two pointers to traverse the lists
        ListNode d1 = headA;
        ListNode d2 = headB;

        // Traverse both lists until the pointers meet
        while (d1 != d2) {
            // Move both the pointers by one place
            d1 = d1.next;
            d2 = d2.next;

            // If intersection is found
            if (d1 == d2) return d1;

            // If either of the two pointers reaches end, place at the front of next linked list 
            if (d1 == null) d1 = headB;
            if (d2 == null) d2 = headA;
        }

        // Return the intersection node
        return d1;
    }
Complexity Analysis:
Time Complexity: O(N + M), where N and M are the lengths of first and second linked list respectively.

In the worst case (when the last node in a linked list is the intersection node), both the pointers will traverse the total length of the two linked lists before meeting at the intersection node. Hence, the time complexity is O(N + M).


Space Complexity: O(1), as no extra space was used.



8.    Detect a loop in LL

Brute:
 // Function to detect a loop in the linked list
    public boolean hasCycle(ListNode head) {
        // Initialize a pointer 'temp'
        // At the head of the linked list
        ListNode temp = head;  

        // Create a set to keep track of
        // Encountered nodes
        HashSet<ListNode> nodeSet = new HashSet<>();  

        // Traverse the linked list
        while (temp != null) {
            // If the node is already in the
            // Set, there is a loop
            if (nodeSet.contains(temp)) {
                return true;
            }
            // Store the current node
            // In the set
            nodeSet.add(temp);
            
            // Move to the next node
            temp = temp.next;  
        }

        // If the list is successfully traversed 
        // Without a loop, return false
        return false;
    }
Complexity Analysis
Time Complexity: O(N), where N is the number of nodes in the LL.
The algorithm traverses the linked list once, and each insertion or lookup operation in a HashSet (or unordered_set) takes O(1) on average due to hashing. Hence, total time = O(N) * O(1) = O(N).
(Note: In the worst case of excessive hash collisions, it can degrade to O(N2), but this is extremely rare with a good hash function.)

Space Complexity: O(N)The HashSet stores references to all visited nodes in the worst case (when no loop exists), resulting in O(N) auxiliary space.

   Optimal:
    
// Function to detect a loop in a linked
    // list using the Tortoise and Hare Algorithm
    public boolean hasCycle(ListNode head) {
        // Initialize two pointers, slow and fast,
        // to the head of the linked list
        ListNode slow = head;
        ListNode fast = head;

        // Step 2: Traverse the linked list with
        // the slow and fast pointers
        while (fast != null && fast.next != null) {
            // Move slow one step
            slow = slow.next;
            // Move fast two steps
            fast = fast.next.next;

            // Check if slow and fast pointers meet
            if (slow == fast) {
                return true;  // Loop detected
            }
        }

        // If fast reaches the end of the list,
        // there is no loop
        return false;
    }
Time Complexity: O(N), where N represents the number of nodes in the linked list. In the worst-case scenario, the fast pointer, which advances more quickly, will either reach the end of the list (if there's no loop) or catch up to the slow pointer (if there's a loop) in a time proportional to the length of the list.

The reason this complexity is O(N) and not slower is due to the fact that each step of the algorithm decreases the gap between the fast and slow pointers (when they are within the loop) by one node. Thus, the maximum number of steps required for them to meet is directly related to the number of nodes in the list.

Space Complexity: O(1) The algorithm utilizes a constant amount of additional space, regardless of the size of the linked list. This efficiency is achieved by using only two pointers (slow and fast) to detect the loop, without needing any significant extra memory, resulting in a constant space complexity of O(1).





9.    Reverse LL in group of given size K

 // Function to reverse a linked list 
    // using the 3-pointer approach
    public ListNode reverseLinkedList(ListNode head) {
        /* Initialize 'temp' at 
         * head of linked list */
        ListNode temp = head;
        
        /* Initialize pointer 'prev' 
         * to NULL, representing 
         * the previous node */
        ListNode prev = null;
        
        // Continue till 'temp' 
        // reaches the end (NULL)
        while (temp != null) {
            /* Store the next node in 'front' 
             * to preserve the reference */
            ListNode front = temp.next;
            
            /* Reverse the direction of the 
             * current node's 'next' pointer 
             * to point to 'prev' */
            temp.next = prev;
            
            /* Move 'prev' to the current 
             * node for the next iteration */
            prev = temp;
            
            /* Move 'temp' to the 'front' node 
             * advancing the traversal */
            temp = front;
        }
        
        // Return the new head 
        // of the reversed linked list
        return prev;
    }

    // Function to get the Kth node from a 
    // given position in the linked list
    public ListNode getKthNode(ListNode temp, int k) {
        // Decrement K 
        // as we already start 
        // from the 1st node
        k -= 1;

        // Decrement K until it reaches the desired position
        while (temp != null && k > 0) {
            // Decrement k as temp progresses
            k--;
            
            // Move to the next node
            temp = temp.next;
        }
        
        // Return the Kth node
        return temp;
    }

    // Function to reverse nodes in groups of K
    public ListNode reverseKGroup(ListNode head, int k) {
        /* Initialize a temporary 
         * node to traverse the list */
        ListNode temp = head;

        /* Initialize a pointer to track 
         * the last node of the previous group */
        ListNode prevLast = null;
        
        // Traverse through the linked list
        while (temp != null) {
            // Get the Kth node of the current group
            ListNode kThNode = getKthNode(temp, k);

            /* If the Kth node is NULL 
             * (not a complete group) */
            if (kThNode == null) {
                /* If there was a previous group, 
                 * link the last node to the current node */
                if (prevLast != null) {
                    prevLast.next = temp;
                }
                
                // Exit the loop
                break;
            }
            
            /* Store the next node 
             * after the Kth node */
            ListNode nextNode = kThNode.next;

            /* Disconnect the Kth node 
             * to prepare for reversal */
            kThNode.next = null;

            // Reverse the nodes from temp to the Kth node
            reverseLinkedList(temp);
            
            /* Adjust the head if the reversal 
             * starts from the head */
            if (temp == head) {
                head = kThNode;
            } else {
                /* Link the last node of the previous 
                 * group to the reversed group */
                prevLast.next = kThNode;
            }

            /* Update the pointer to the 
             * last node of the previous group */
            prevLast = temp;

            // Move to the next group
            temp = nextNode;
        }
        
        // Return the head of the modified linked list
        return head;
    }
Complexity Analysis
Time Complexity: O(2N) because it consists of actions of reversing segments of K and finding the Kth node, both of which operate in linear time. Thus, O(N) + O(N) = O(2N), which simplifies to O(N).

Space Complexity: O(1) because the code operates in place without any additional space requirements.






10.   Check if LL is palindrome or not


Brute:
public boolean isPalindrome(ListNode head) {
        /*Create an empty stack 
        to store values*/
        Stack<Integer> stack = new Stack<>();
        
        /*Initialize temporary pointer 
        to the head of the linked list*/
        ListNode temp = head;
        
        /*Traverse the linked list 
        and push values onto the stack*/
        while (temp != null) {
            /*Push the data from 
            the current node onto the stack*/
            stack.push(temp.val);
            
            // Move to the next node
            temp = temp.next;
        }
        
        /*Reset temporary pointer 
        back to the head of 
        the linked list*/
        temp = head;
        
        /*Compare values by popping 
        from the stack and checking 
        against linked list nodes*/
        while (temp != null) {
            if (temp.val != stack.pop()) {
                /*If values don't match, 
                it's not a palindrome*/
                return false;
            }
            
            /*Move to the next node 
            in the linked list*/
            temp = temp.next;
        }
        
        /*If all values match, 
        it's a palindrome*/
        return true;
    }
   Complexity Analysis
Time Complexity: O(2xN) because we need to traverse the linked list twice: once to push the values onto the stack and once more to pop the values and compare them with the nodes in the linked list. Here, N represents the number of nodes in the linked list. Even though it's O(2xN), it effectively simplifies to O(N).

Space Complexity: O(N) We use a stack to store the values of the linked list. In the worst-case scenario, the stack will hold all N values from the linked list, essentially storing the entire list.

Optimal:

/* Function to reverse a linked list
       using the iterative approach */
    private ListNode reverseLinkedList(ListNode head) {
        // Initialize previous pointer as null
        ListNode prev = null;

        // Initialize current pointer as head
        ListNode curr = head;

        // Traverse the list until all nodes are processed
        while (curr != null) {

            // Temporarily store the next node
            ListNode nextNode = curr.next;

            // Reverse the link direction
            curr.next = prev;

            // Move 'prev' one step forward
            prev = curr;

            // Move 'curr' one step forward
            curr = nextNode;
        }

        // 'prev' now points to the new head after reversal
        return prev;
    }

    public boolean isPalindrome(ListNode head) {
        /* Check if the linked list is empty
           or has only one node */
        if (head == null || head.next == null) {
            // It's a palindrome by definition
            return true;
        }

        /* Initialize two pointers, slow and fast,
           to find the middle of the linked list */
        ListNode slow = head;
        ListNode fast = head;

        /* Traverse the linked list to find the
           middle using the slow-fast pointer approach */
        while (fast.next != null && fast.next.next != null) {

            // Move slow pointer one step
            slow = slow.next;

            // Move fast pointer two steps
            fast = fast.next.next;
        }

        /* Reverse the second half of the linked list
           starting from the node after the middle */
        ListNode newHead = reverseLinkedList(slow.next);

        // Pointer to the first half
        ListNode first = head;

        // Pointer to the reversed second half
        ListNode second = newHead;

        /* Compare nodes from both halves
           one by one to check for palindrome */
        while (second != null) {

            // If mismatch found, it's not a palindrome
            if (first.val != second.val) {

                // Restore the original list before returning
                reverseLinkedList(newHead);

                return false;
            }

            // Move both pointers one step ahead
            first = first.next;
            second = second.next;
        }

        /* Restore the second half of the linked list
           to its original order */
        reverseLinkedList(newHead);

        // All values matched, the list is a palindrome
        return true;
    }

Complexity Analysis
Time Complexity: O(2xN) The algorithm involves traversing the linked list twice. The first traversal finds the middle and reverses the second half, while the second traversal compares elements from both halves. Since each traversal covers N/2 elements, the total time complexity is O(N/2 + N/2 + N/2 + N/2), which simplifies to O(2N), ultimately reducing to O(N).

Space Complexity: O(1) This approach uses a constant amount of additional space, regardless of the linked list's size. It does not require any extra data structures that depend on the input size, resulting in a space complexity of O(1).


11.   Find the starting point in LL
Brute:
public ListNode findStartingPoint(ListNode head) {
        // Use temp to traverse the linked list
        ListNode temp = head;
        
        // HashMap to store all visited nodes
        HashMap<ListNode, Integer> map = new HashMap<>();
        
        // Traverse the list using temp
        while (temp != null) {
            // Check if temp has been encountered again
            if (map.containsKey(temp)) {
                // A loop is detected hence return temp
                return temp;
            }
            // Store temp as visited
            map.put(temp, 1);
            // Move to the next node
            temp = temp.next;
        }

        // If no loop is detected, return null
        return null;
    }
Time Complexity: O(N) The algorithm goes through the entire linked list once, with 'N' representing the total number of nodes. As a result, the time complexity is linear, or O(N).

Space Complexity: O(N) The algorithm utilizes a hash map to store the nodes it encounters. This hash map can store up to 'N' nodes, where 'N' is the total number of nodes in the list. Therefore, the space complexity is O(N) because of the additional space used by the hash map.


Optimal:
    

 public ListNode findStartingPoint(ListNode head) {
        // Initialize a slow and fast 
        // pointers to the head of the list
        ListNode slow = head;
        ListNode fast = head;

        // Phase 1: Detect the loop
        while (fast != null && fast.next != null) {
            
            // Move slow one step
            slow = slow.next;
            
            // Move fast two steps
            fast = fast.next.next;

            // If slow and fast meet,
            // a loop is detected
            if (slow == fast) {
                
                // Reset the slow pointer
                // to the head of the list
                slow = head;

                // Phase 2: Find the first node of the loop
                while (slow != fast) {
                    
                    // Move slow and fast one step
                    // at a time
                    slow = slow.next;
                    fast = fast.next;

                    // When slow and fast meet again,
                    // it's the first node of the loop
                }
                
                // Return the first node of the loop
                return slow;
            }
        }
        
        // If no loop is found, return null
        return null;
    }

Time Complexity: O(N) The code examines each node in the linked list exactly once, where 'N' is the total number of nodes. This results in a linear time complexity, O(N), as the traversal through the list is direct and sequential.

Space Complexity: O(1) The code uses a fixed amount of extra space, regardless of the size of the linked list. This is accomplished by employing two pointers, slow and fast, to detect the loop. Since no additional data structures are used that depend on the size of the list, the space complexity remains constant, O(1).






12.   Flattening of LL

Brute:

 // Function to convert a vector to a linked list
    private ListNode convertArrToLinkedList(List<Integer> arr) {
        /* Create a dummy node to serve as
         the head of the linked list */
        ListNode dummyNode = new ListNode(-1);
        ListNode temp = dummyNode;

        /* Iterate through the vector and
         create nodes with vector elements */
        for (int i = 0; i < arr.size(); i++) {
            // Create a new node with the vector element
            temp.child = new ListNode(arr.get(i));
            
            // Update the temporary pointer
            temp = temp.child;
        }
        
        /* Return the linked list starting
         from the next of the dummy node */
        return dummyNode.child;
    }

    // Function to flatten a linked list with child pointers 
    public ListNode flattenLinkedList(ListNode head) {
        List<Integer> arr = new ArrayList<>();

        // Traverse through the linked list
        while (head != null) {
            /* Traverse through the child
             nodes of each head node */
            ListNode t2 = head;
            
            while (t2 != null) {
                // Store each node's data in the array
                arr.add(t2.val);
                
                // Move to the next child node
                t2 = t2.child;
            }
            // Move to the next head node
            head = head.next;
        }

        // Sort the array containing node values
        Collections.sort(arr);

        // Convert the sorted array back to a linked list
        return convertArrToLinkedList(arr);
    }
Complexity Analysis
Time Complexity: O(NxM) + O(NxM log(NxM)) + O(NxM) where N is the number of nodes along the next pointers and M is the number of nodes along the child pointers.

O(NxM) because we traverse through all the nodes, iterating through N nodes along the next pointers and M nodes along the child pointers.
O(NxM log(NxM)) because we sort the array containing NxM total elements.
O(NxM) because we reconstruct the linked list from the sorted array by iterating over the NxM elements.
Space Complexity: O(NxM) + O(NxM) where N is the number of nodes along the next pointers and M is the number of nodes along the child pointers.

O(NxM) for storing all the elements in an additional array for sorting.
O(NxM) to reconstruct the linked list from the array after sorting.






  Optimal:  
/* Merge the two linked lists in a particular
     order based on the data value */
    private ListNode merge(ListNode list1, ListNode list2) {
        /* Create a dummy node as a 
        placeholder for the result */
        ListNode dummyNode = new ListNode(-1);
        ListNode res = dummyNode;

        // Merge the lists based on data values
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                res.child = list1;
                res = list1;
                list1 = list1.child;
            } else {
                res.child = list2;
                res = list2;
                list2 = list2.child;
            }
            res.next = null;
        }

        // Connect the remaining elements if any
        if (list1 != null) {
            res.child = list1;
        } else {
            res.child = list2;
        }

        // Break the last node's link to prevent cycles
        if (dummyNode.child != null) {
            dummyNode.child.next = null;
        }

        return dummyNode.child;
    }

    // Function to flatten a linked list with child pointers 
    public ListNode flattenLinkedList(ListNode head) {
        // If head is null or there is no next node
        if (head == null || head.next == null) {
            return head; // Return head
        }

        // Recursively flatten the rest of the linked list
        ListNode mergedHead = flattenLinkedList(head.next);

        // Merge the lists
        head = merge(head, mergedHead);
        return head;
    }
Complexity Analysis
Time Complexity: O(M × N²), where N is the number of linked lists (nodes connected through next) and M is the average number of nodes in each vertical list (connected through child).

Each time a merge is performed, the size of the merged list increases — the first merge takes O(M), the second O(2M), the third O(3M), and so on.
Hence, TC = O(M) + O(2M) + … + O(NM) = O(M × N²).

Space Complexity: O(N), due to recursive stack calls (no extra data structures used).

  
13.   Rotate a LL
Brute:
// Function to rotate the list by k steps
    public ListNode rotateRight(ListNode head, int k) {
        // Base case: if list is empty or has only one node
        if (head == null || head.next == null)
            return head;

        // Perform rotation k times
        for (int i = 0; i < k; i++) {
            ListNode temp = head;
            // Find the second last node
            while (temp.next.next != null) temp = temp.next;
            // Get the last node
            ListNode end = temp.next;
            // Break the link between 
            // second last and last node
            temp.next = null;
            // Make the last node
            // as new head
            end.next = head;
            head = end;
        }
        return head;
    }
Complexity Analysis
Time Complexity: O(NxK) because for K times, we are iterating through the entire list to get the last element and move it to the first position. Here, N represents the number of nodes in the linked list, and K is the number of steps by which the list has to be rotated.

Space Complexity: O(1) because no extra data structure is required for computation.

Optimal:
 // Function to rotate the list by k steps
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) 
            return head;

        // Calculating length
        ListNode temp = head;
        int length = 1;
        while (temp.next != null) {
            ++length;
            temp = temp.next;
        }

        // Link last node to first node
        temp.next = head;
        // When k is more than length of list
        k = k % length; 
        // To get end of the list
        int end = length - k; 
        while (end-- > 0) 
            temp = temp.next;

        // Breaking last node link and pointing to NULL
        head = temp.next;
        temp.next = null;

        return head;
    }
   Complexity Analysis
Time Complexity: O(N) + O(N - (K % N)) because O(N) is for calculating the length of the list, and O(N - (N % k)) is for breaking the link and pointing to NULL. Here N is the length of the linked list and K is the number of iterations required. Space Complexity: O(1) because no extra data structure is required for computation.


14.   Clone a LL with random and next pointer

Brute:
 // Function to clone linked list with random pointers
    public ListNode copyRandomList(ListNode head) {
        // If the head is null, return null
        if (head == null) return null;

        /*Create a HashMap to map 
        original nodes to their corresponding copied nodes*/
        HashMap<ListNode, ListNode> map = new HashMap<>();
        ListNode temp = head;

        // Create copies of each node
        while (temp != null) {
            // Create new node with same value as original
            ListNode newNode = new ListNode(temp.val);
            // Map to original node 
            map.put(temp, newNode);
            // Move to next node
            temp = temp.next;
        }

        // Reset temp 
        temp = head;

        /*Connect the next and 
        random pointers of the 
        copied nodes using the map*/
        while (temp != null) {
            // Get copied node from the map
            ListNode copyNode = map.get(temp);
            /*Set next pointer of copied node 
            to the copied node of the next 
            original node*/
            copyNode.next = map.get(temp.next);
            /*Set the random pointer of the 
            copied node to the copied node of 
            the random original node*/
            copyNode.random = map.get(temp.random);
            temp = temp.next;
        }

        // Return the head
        return map.get(head);
    }
}
Complexity Analysis
Time Complexity: O(2N) because the linked list is traversed twice, once for creating copies of each node and for the second time to set the next and random pointers for each copied node. The time to access the nodes in the map is O(1) due to hashing. Here N is the length of the Linked List.

Space Complexity: O(N)+O(N) where N is the number of nodes in the linked list as all nodes are stored in the map to maintain mappings and the copied linked list takes O(N) space as well.

Optimal:
// Insert a copy of each node in between the original nodes
    void insertCopyInBetween(ListNode head) {
        ListNode temp = head;
        while (temp != null) {
            ListNode nextElement = temp.next;
            // Create a new node with the same data
            ListNode copy = new ListNode(temp.val);
            
            copy.next = nextElement;
            
            temp.next = copy;
            
            temp = nextElement;
        }
    }

    // Function to connect random pointers of the copied nodes
    void connectRandomPointers(ListNode head) {
        ListNode temp = head;
        while (temp != null) {
            // Access the copied node
            ListNode copyNode = temp.next;
            
            /*If the original node has a random pointer
            point the copied node's random to the 
            corresponding copied random node
            set the copied node's random to null 
            if the original random is null*/
            if (temp.random != null) {
                
                copyNode.random = temp.random.next;
            } else {
                
                copyNode.random = null;
            }
            
            // Move to next original node
            temp = temp.next.next;
        }
    }

    // Function to retrieve the deep copy of the linked list
    ListNode getDeepCopyList(ListNode head) {
        ListNode temp = head;
        // Create a dummy node
        ListNode dummyNode = new ListNode(-1);
        // Initialize a result pointer
        ListNode res = dummyNode;

        while (temp != null) {
            /*Creating a new List by 
            pointing to copied nodes*/
            res.next = temp.next;
            res = res.next;

            /*Disconnect and revert back 
            to the initial state of the 
            original linked list*/
            temp.next = temp.next.next;
            temp = temp.next;
        }
        
        /*Return the deep copy 
        of the list starting 
        from the dummy node*/
        return dummyNode.next;
    }

    // Function to clone the linked list
    ListNode copyRandomList(ListNode head) {
        // If the original list is empty, return null
        if (head == null) return null;

        // Insert nodes in between
        insertCopyInBetween(head);
        // Connect random pointers
        connectRandomPointers(head);
        // Retrieve deep copy of linked list
        return getDeepCopyList(head);
    }
Complexity Analysis
Time Complexity: O(3N) where N is the number of nodes in the linked list:
First traversal to create copies of the nodes and insert them between the original nodes.
Second traversal to set the random pointers of the copied nodes to their corresponding copied nodes.
Third traversal to separate the copied nodes from the original nodes.
Space Complexity: O(N) where N is the number of nodes in the linked list as the only extra space allocated is to create the copied list without creating any other additional data structures.
