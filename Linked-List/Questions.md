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









8.    Detect a loop in LL









9.    Reverse LL in group of given size K









10.   Check if LL is palindrome or not









11.   Find the starting point in LL









12.   Flattening of LL
13.   Rotate a LL
14.   Clone a LL with random and next pointer
