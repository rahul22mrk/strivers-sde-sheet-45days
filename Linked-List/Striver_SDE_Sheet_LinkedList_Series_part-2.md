# 📖 Striver SDE Sheet - Linked List Series - Part 2: Sequence Wise Detailed Notes

> **Question-by-question solutions with all approaches, code, and complexity analysis.**

---

## 📑 Sequence Index

| # | Question | Pattern | Best Approach | Best TC | Best SC |
|---|----------|---------|---------------|---------|---------|
| 1 | [Reverse a Linked List](#q1-reverse-a-linked-list) | Reversal | Iterative 3-pointer | O(N) | O(1) |
| 2 | [Find Middle of Linked List](#q2-find-middle-of-linked-list) | Slow-Fast | Tortoise & Hare | O(N/2) | O(1) |
| 3 | [Merge Two Sorted Lists](#q3-merge-two-sorted-lists) | Merge + Dummy | Two-pointer merge | O(N1+N2) | O(1) |
| 4 | [Remove Nth Node from Back](#q4-remove-nth-node-from-the-back-of-the-ll) | Two-Pointer Alignment | Fast-Slow gap | O(N) | O(1) |
| 5 | [Add Two Numbers as LinkedList](#q5-add-two-numbers-as-linkedlist) | Dummy Node | Digit-wise add with carry | O(max(M,N)) | O(max(M,N)) |
| 6 | [Delete Node in O(1)](#q6-delete-node-in-a-linked-list-o1) | In-Place | Value copy + skip | O(1) | O(1) |
| 7 | [Intersection Point of Y LL](#q7-find-the-intersection-point-of-y-ll) | Two-Pointer Intersection | Switch heads | O(N+M) | O(1) |
| 8 | [Detect a Loop in LL](#q8-detect-a-loop-in-ll) | Slow-Fast | Tortoise & Hare | O(N) | O(1) |
| 9 | [Reverse LL in Group of K](#q9-reverse-ll-in-group-of-given-size-k) | Reversal | Reverse groups + reconnect | O(2N) | O(1) |
| 10 | [Check if LL is Palindrome](#q10-check-if-ll-is-palindrome-or-not) | Slow-Fast + Reversal | Find mid + reverse + compare | O(2N) | O(1) |
| 11 | [Find Starting Point of Loop](#q11-find-the-starting-point-in-ll) | Slow-Fast | Detect + reset to head | O(N) | O(1) |
| 12 | [Flattening of LL](#q12-flattening-of-ll) | Merge + Recursion | Recursive flatten + merge | O(M×N²) | O(N) |
| 13 | [Rotate a LL](#q13-rotate-a-ll) | Circular | Make circular + break | O(N) | O(1) |
| 14 | [Clone LL with Random Pointer](#q14-clone-a-ll-with-random-and-next-pointer) | In-Place | Interleave copies | O(3N) | O(N) |

---

## 📂 Pattern-wise Quick Navigation

| Pattern | Questions |
|---------|-----------|
| [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare) | Q2, Q8, Q10, Q11 |
| [Reversal](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-2-linked-list-reversal) | Q1, Q9, Q10 |
| [Merge / Two-List](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-3-merge--two-list-traversal) | Q3, Q12 |
| [Dummy Node](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-4-dummy-node-technique) | Q3, Q5, Q12 |
| [Hashing](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking) | Q7(Brute), Q8(Brute), Q11(Brute), Q14(Brute) |
| [In-Place Modification](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-6-in-place-modification--o1-deletion) | Q6, Q14 |
| [Two-Pointer Alignment](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-7-two-pointer-intersection--alignment) | Q4, Q7 |
| [Circular / Rotation](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-8-circular--rotation-manipulation) | Q13 |

---

## Q1. Reverse a Linked List

**🔗 Pattern**: [Reversal](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-2-linked-list-reversal)

**Problem**: Given the head of a singly linked list, reverse the list and return the reversed list.

### Approach 1: Iterative (3-Pointer)

**Intuition**: Use three pointers — `prev`, `temp` (current), and `front` (next). At each step, reverse the current node's pointer to point to `prev`, then move all three forward.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
Original: 1 → 2 → 3 → NULL

Step 1: prev=NULL, temp=1, front=2
        1.next = NULL (point to prev)
        prev=1, temp=2

Step 2: prev=1, temp=2, front=3
        2.next = 1 (point to prev)
        prev=2, temp=3

Step 3: prev=2, temp=3, front=NULL
        3.next = 2 (point to prev)
        prev=3, temp=NULL

Result: 3 → 2 → 1 → NULL (prev is new head)
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode reverseList(ListNode head) {
    ListNode temp = head;
    ListNode prev = null;
    
    while (temp != null) {
        ListNode front = temp.next;  // Save next node
        temp.next = prev;            // Reverse the link
        prev = temp;                 // Move prev forward
        temp = front;               // Move temp forward
    }
    
    return prev;  // New head
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) — single traversal |
| **Space** | O(1) — only 3 pointers |

---

### Approach 2: Recursive

**Intuition**: Recursively reverse the rest of the list first. Then make the next node point back to current node, and set current's next to null.

**Dry Run**:
```
reverseList(1→2→3→NULL)
  └─ reverseList(2→3→NULL)
       └─ reverseList(3→NULL)
            └─ Base case: return 3 (newHead)
       front = 3, front.next = 2, 2.next = null → return 3
  front = 2, front.next = 1, 1.next = null → return 3

Result: 3 → 2 → 1 → NULL
```

**Code**:
```java
public ListNode reverseList(ListNode head) {
    // Base case: empty or single node
    if (head == null || head.next == null) {
        return head;
    }
    
    // Reverse the rest of the list
    ListNode newHead = reverseList(head.next);
    
    // Make the next node point back to current
    ListNode front = head.next;
    front.next = head;
    
    // Remove the old forward link
    head.next = null;
    
    return newHead;
}
```

| | Complexity |
|---|---|
| **Time** | O(N) — each node visited once |
| **Space** | O(N) — recursive call stack depth = N |

---

### ⚡ Key Takeaways
- **Iterative is preferred** — O(1) space vs O(N) for recursive
- The 3-pointer pattern (prev, curr, next) is the **building block** for many LL problems
- After reversal, `prev` is the new head, original head becomes the tail

---

## Q2. Find Middle of Linked List

**🔗 Pattern**: [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare)

**Problem**: Given the head of a singly linked list, return the middle node. If there are two middle nodes, return the **second** middle node.

### Approach 1: Brute (Count & Traverse)

**Intuition**: First count total nodes, then traverse to the middle position.

**Code**:
```java
public ListNode middleOfLinkedList(ListNode head) {
    ListNode temp = head;
    int count = 0;
    
    // Count total nodes
    while (temp != null) {
        count++;
        temp = temp.next;
    }
    
    // Calculate middle position
    int midPosition = count / 2 + 1;
    
    // Traverse to middle
    ListNode middleNode = head;
    for (int i = 1; i < midPosition; i++) {
        middleNode = middleNode.next;
    }
    
    return middleNode;
}
```

| | Complexity |
|---|---|
| **Time** | O(N) + O(N/2) = O(N) — two traversals |
| **Space** | O(1) |

---

### Approach 2: Optimal (Tortoise & Hare)

**Intuition**: Slow pointer moves 1 step, fast pointer moves 2 steps. When fast reaches end, slow is at middle.

**Dry Run**:
```
Odd:  1 → 2 → 3 → 4 → 5
      s,f=1
      s=2, f=3
      s=3, f=5 → fast.next=null → stop
      Middle = 3 ✅

Even: 1 → 2 → 3 → 4 → 5 → 6
      s,f=1
      s=2, f=3
      s=3, f=5
      s=4, f=null → stop
      Middle = 4 (second middle) ✅
```

**Code**:
```java
public ListNode middleOfLinkedList(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;       // Move 1 step
        fast = fast.next.next;  // Move 2 steps
    }
    
    return slow;
}
```

| | Complexity |
|---|---|
| **Time** | O(N/2) — fast pointer traverses half the list |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- For **first middle** (even length): use `while (fast.next != null && fast.next.next != null)`
- For **second middle** (even length): use `while (fast != null && fast.next != null)` ← standard
- This is a **sub-operation** used in Q10 (Palindrome) and many other problems

---

## Q3. Merge Two Sorted Lists

**🔗 Pattern**: [Merge / Two-List](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-3-merge--two-list-traversal) + [Dummy Node](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-4-dummy-node-technique)

**Problem**: You are given two sorted linked lists. Merge them into one sorted list and return the head.

### Approach 1: Brute (Extra Array)

**Intuition**: Extract all values into an array, sort, then create a new linked list.

**Code**:
```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ArrayList<Integer> arr = new ArrayList<>();
    ListNode temp1 = list1;
    ListNode temp2 = list2;

    // Add all values from list1
    while (temp1 != null) {
        arr.add(temp1.val);
        temp1 = temp1.next;
    }

    // Add all values from list2
    while (temp2 != null) {
        arr.add(temp2.val);
        temp2 = temp2.next;
    }

    // Sort the array
    Collections.sort(arr);

    // Convert sorted array to linked list
    ListNode dummyNode = new ListNode(-1);
    ListNode temp = dummyNode;
    for (int i = 0; i < arr.size(); i++) {
        temp.next = new ListNode(arr.get(i));
        temp = temp.next;
    }

    return dummyNode.next;
}
```

| | Complexity |
|---|---|
| **Time** | O(N1+N2) + O(N log N) + O(N) — traverse + sort + rebuild |
| **Space** | O(N) + O(N) — array + new list |

---

### Approach 2: Optimal (Two-Pointer Merge with Dummy Node)

**Intuition**: Since both lists are already sorted, compare heads and pick the smaller one. Use a dummy node to avoid head edge cases. **Relink existing nodes** instead of creating new ones.

**Dry Run**:
```
list1: 1 → 3 → 5
list2: 2 → 4 → 6

dummy(-1) → ?
1 < 2: dummy → 1, list1 moves to 3
3 > 2: 1 → 2, list2 moves to 4
3 < 4: 2 → 3, list1 moves to 5
5 > 4: 3 → 4, list2 moves to 6
5 < 6: 4 → 5, list1 moves to null
list1 null: 5 → 6 (attach remaining)

Result: 1 → 2 → 3 → 4 → 5 → 6
```

**Code**:
```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummyNode = new ListNode(-1);
    ListNode temp = dummyNode;

    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            temp.next = list1;
            list1 = list1.next;
        } else {
            temp.next = list2;
            list2 = list2.next;
        }
        temp = temp.next;
    }

    // Attach remaining nodes (no loop needed!)
    if (list1 != null) {
        temp.next = list1;
    } else {
        temp.next = list2;
    }

    return dummyNode.next;
}
```

| | Complexity |
|---|---|
| **Time** | O(N1 + N2) — single pass through both lists |
| **Space** | O(1) — only pointers, no new nodes created |

---

### ⚡ Key Takeaways
- **Dummy node** eliminates the need to handle the first node specially
- **Attach remaining list in one step** — no need to iterate through remaining nodes
- This merge pattern is reused in **Q12 (Flattening)** and **Merge Sort for LL**
- **Alternative**: Array-to-LL without dummy node:
```java
ListNode head = null, temp = null;
for (int val : arr) {
    ListNode newNode = new ListNode(val);
    if (head == null) { head = newNode; temp = newNode; }
    else { temp.next = newNode; temp = temp.next; }
}
return head;
```

---

## Q4. Remove Nth Node from the Back of the LL

**🔗 Pattern**: [Two-Pointer Alignment](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-7-two-pointer-intersection--alignment)

**Problem**: Given the head of a linked list, remove the Nth node from the end and return the head.

### Approach 1: Brute (Count & Traverse)

**Intuition**: Count total nodes, calculate position from front, traverse to the node before it, and delete.

**Code**:
```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    if (head == null) return null;
    
    int cnt = 0;
    ListNode temp = head;

    // Count total nodes
    while (temp != null) {
        cnt++;
        temp = temp.next;
    }

    // If N equals total count, delete head
    if (cnt == n) {
        ListNode newHead = head.next;
        return newHead;
    }

    // Calculate position from front
    int res = cnt - n;
    temp = head;

    // Traverse to node just before the one to delete
    while (temp != null) {
        res--;
        if (res == 0) break;
        temp = temp.next;
    }

    // Delete the Nth node from end
    ListNode delNode = temp.next;
    temp.next = temp.next.next;
    return head;
}
```

| | Complexity |
|---|---|
| **Time** | O(L) + O(L-N) = O(L) — two traversals |
| **Space** | O(1) |

---

### Approach 2: Optimal (Fast-Slow Gap)

**Intuition**: Move `fast` N steps ahead first. Then move both `fast` and `slow` until `fast.next` is null. Now `slow` is just before the node to delete.

**Dry Run**:
```
List: 1 → 2 → 3 → 4 → 5, N = 2

Step 1: Move fast N=2 steps ahead
        fast at node 3, slow at node 1

Step 2: Move both until fast.next is null
        slow=2, fast=4
        slow=3, fast=5 → fast.next=null, stop

Step 3: Delete slow.next (node 4)
        slow.next = slow.next.next

Result: 1 → 2 → 3 → 5
```

**Code**:
```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode fastp = head;
    ListNode slowp = head;

    // Move fast N steps ahead
    for (int i = 0; i < n; i++) {
        fastp = fastp.next;
    }

    // If fast becomes null, Nth from end is the head
    if (fastp == null) {
        return head.next;
    }

    // Move both until fast reaches last node
    while (fastp.next != null) {
        fastp = fastp.next;
        slowp = slowp.next;
    }

    // Delete the Nth node from end
    slowp.next = slowp.next.next;
    return head;
}
```

| | Complexity |
|---|---|
| **Time** | O(N) — single traversal |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- The **gap of N** between fast and slow is the key insight
- When `fast == null` after N steps, it means we need to **delete the head**
- This is the same pattern as finding the Nth node from end, but we need the **node before it** to delete

---

## Q5. Add Two Numbers as LinkedList

**🔗 Pattern**: [Dummy Node](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-4-dummy-node-technique)

**Problem**: You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each node contains a single digit. Add the two numbers and return the sum as a linked list.

### Approach: Digit-wise Addition with Carry

**Intuition**: Add digits from both lists position by position (they're already in reverse order, so units digit comes first). Maintain a carry. Use a dummy node to build the result.

**Dry Run**:
```
l1: 2 → 4 → 3  (represents 342)
l2: 5 → 6 → 4  (represents 465)

Step 1: 2+5 = 7, carry=0 → 7
Step 2: 4+6 = 10, carry=1 → 0
Step 3: 3+4+1 = 8, carry=0 → 8

Result: 7 → 0 → 8  (represents 807 = 342+465)
```

**Code**:
```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode();
    ListNode temp = dummy;
    int carry = 0;

    while ((l1 != null || l2 != null) || carry != 0) {
        int sum = 0;

        if (l1 != null) {
            sum += l1.val;
            l1 = l1.next;
        }

        if (l2 != null) {
            sum += l2.val;
            l2 = l2.next;
        }

        sum += carry;
        carry = sum / 10;

        ListNode node = new ListNode(sum % 10);
        temp.next = node;
        temp = temp.next;
    }

    return dummy.next;
}
```

| | Complexity |
|---|---|
| **Time** | O(max(M, N)) — traverse the longer list |
| **Space** | O(max(M, N)) — result list length |

---

### ⚡ Key Takeaways
- **Carry must be checked in the while condition** — if both lists end but carry remains (e.g., 9+9=18), we need one more node
- `sum % 10` gives the digit, `sum / 10` gives the carry
- **Dummy node** is essential here since we don't know the head in advance
- If digits were in **forward order**, you'd need to reverse both lists first, add, then reverse result

---

## Q6. Delete Node in a Linked List O(1)

**🔗 Pattern**: [In-Place Modification](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-6-in-place-modification--o1-deletion)

**Problem**: Given a node in a singly linked list, delete it without access to the head. It is guaranteed the node is not the tail.

### Approach: Value Copy + Skip

**Intuition**: You can't delete the node itself (no access to previous node's next pointer). Instead, copy the next node's value into the current node, then skip the next node.

**Dry Run**:
```
List: 4 → 5 → 1 → 9
Delete node with value 5 (given pointer to it)

Step 1: Copy next node's value: node.val = 1
        List becomes: 4 → 1 → 1 → 9

Step 2: Skip next node: node.next = node.next.next
        List becomes: 4 → 1 → 9

Result: 4 → 1 → 9 (5 is effectively deleted)
```

**Code**:
```java
public void deleteNode(ListNode node) {
    node.val = node.next.val;     // Copy next node's value
    node.next = node.next.next;   // Skip the next node
}
```

| | Complexity |
|---|---|
| **Time** | O(1) |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- **Cannot work if node is the tail** — there's no next node to copy from
- This is a **trick question** — the key insight is that you don't actually delete the given node, you make it a copy of the next node and delete the next node
- **Constraint**: The node to delete is never the tail

---

## Q7. Find the Intersection Point of Y LL

**🔗 Pattern**: [Two-Pointer Intersection](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-7-two-pointer-intersection--alignment) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given the heads of two singly linked lists, return the node at which the two lists intersect. If they don't intersect, return null.

### Approach 1: Brute (HashSet)

**Intuition**: Store all nodes of list A in a HashSet. Then traverse list B and check if any node exists in the set.

**Code**:
```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    HashSet<ListNode> nodes_set = new HashSet<>();

    while (headA != null) {
        nodes_set.add(headA);
        headA = headA.next;
    }

    while (headB != null) {
        if (nodes_set.contains(headB)) {
            return headB;
        }
        headB = headB.next;
    }

    return null;
}
```

| | Complexity |
|---|---|
| **Time** | O(N + M) — traverse both lists |
| **Space** | O(N) — store all nodes of list A |

---

### Approach 2: Better (Length Alignment)

**Intuition**: Calculate lengths of both lists. Move the longer list's pointer ahead by the difference. Then move both together — they'll meet at the intersection.

**Code**:
```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode temp1 = headA;
    ListNode temp2 = headB;
    int n1 = 0, n2 = 0;

    // Get length of first list
    while (temp1 != null) { n1++; temp1 = temp1.next; }

    // Get length of second list
    while (temp2 != null) { n2++; temp2 = temp2.next; }

    // Align and find collision point
    if (n1 < n2) return collisionPoint(headA, headB, n2 - n1);
    return collisionPoint(headB, headA, n1 - n2);
}

private ListNode collisionPoint(ListNode smallerListHead, ListNode longerListHead, int len) {
    ListNode temp1 = smallerListHead;
    ListNode temp2 = longerListHead;

    // Move longer list's pointer ahead by difference
    for (int i = 0; i < len; i++) temp2 = temp2.next;

    // Move both together
    while (temp1 != temp2) {
        temp1 = temp1.next;
        temp2 = temp2.next;
    }

    return temp1;
}
```

| | Complexity |
|---|---|
| **Time** | O(N + M) — calculate lengths + traverse aligned |
| **Space** | O(1) |

---

### Approach 3: Optimal (Switch Heads)

**Intuition**: When either pointer reaches the end of its list, switch it to the head of the other list. Both pointers will travel the same total distance (lenA + lenB) and meet at the intersection.

**Why it works**:
```
List A: a1 → a2 → a3 ↘
                       c1 → c2 → null
List B:      b1 → b2 ↗

Pointer 1 path: a1 → a2 → a3 → c1 → c2 → null → b1 → b2 → c1 (found!)
Pointer 2 path: b1 → b2 → c1 → c2 → null → a1 → a2 → a3 → c1 (found!)

Both travel 5 + 4 = 9 steps before meeting at c1.
```

**Code**:
```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;

    ListNode d1 = headA;
    ListNode d2 = headB;

    while (d1 != d2) {
        d1 = d1.next;
        d2 = d2.next;

        if (d1 == d2) return d1;  // Intersection found (or both null = no intersection)

        if (d1 == null) d1 = headB;  // Switch to other list
        if (d2 == null) d2 = headA;  // Switch to other list
    }

    return d1;
}
```

| | Complexity |
|---|---|
| **Time** | O(N + M) — both pointers travel at most N+M steps |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- **Compare node references**, not values — different nodes can have the same value
- The **switch-heads trick** is elegant but the math proof is important for interviews
- If no intersection exists, both pointers reach null simultaneously after N+M steps
- The `if (d1 == d2) return d1` check AFTER moving both pointers handles the case where they meet at null (no intersection)

---

## Q8. Detect a Loop in LL

**🔗 Pattern**: [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given head of a linked list, determine if the list has a cycle in it.

### Approach 1: Brute (HashSet)

**Intuition**: Store each node in a HashSet as you traverse. If you encounter a node already in the set, there's a cycle.

**Code**:
```java
public boolean hasCycle(ListNode head) {
    ListNode temp = head;
    HashSet<ListNode> nodeSet = new HashSet<>();

    while (temp != null) {
        if (nodeSet.contains(temp)) {
            return true;  // Cycle detected
        }
        nodeSet.add(temp);
        temp = temp.next;
    }

    return false;  // No cycle
}
```

| | Complexity |
|---|---|
| **Time** | O(N) — each node visited once, HashSet ops are O(1) avg |
| **Space** | O(N) — store all nodes in worst case |

---

### Approach 2: Optimal (Tortoise & Hare)

**Intuition**: Slow moves 1 step, fast moves 2 steps. If there's a cycle, fast will eventually "lap" slow and they'll meet. If no cycle, fast reaches null.

**Why they must meet**: In a cycle, fast gains 1 step on slow each iteration. The gap decreases by 1 each time, so they MUST meet within C steps (C = cycle length).

**Code**:
```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;       // Move 1 step
        fast = fast.next.next;  // Move 2 steps

        if (slow == fast) {
            return true;  // Cycle detected
        }
    }

    return false;  // No cycle
}
```

| | Complexity |
|---|---|
| **Time** | O(N) — fast pointer traverses the list |
| **Space** | O(1) — only two pointers |

---

### ⚡ Key Takeaways
- **HashSet approach** is intuitive but uses O(N) space
- **Tortoise & Hare** is the optimal O(1) space solution
- The condition `fast != null && fast.next != null` prevents null pointer exceptions
- This is the **foundation** for Q11 (Find Starting Point of Loop)

---

## Q9. Reverse LL in Group of Given Size K

**🔗 Pattern**: [Reversal](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-2-linked-list-reversal)

**Problem**: Given the head of a linked list, reverse every K nodes. If the remaining nodes are less than K, leave them as is.

### Approach: Reverse Groups + Reconnect

**Intuition**: For each group of K nodes: (1) Find the Kth node, (2) Disconnect the group, (3) Reverse the group, (4) Reconnect to previous group. If less than K nodes remain, don't reverse.

**Dry Run**:
```
List: 1 → 2 → 3 → 4 → 5, K = 3

Group 1: [1 → 2 → 3]
  getKthNode(1, 3) = node 3
  Disconnect: 3.next = null → [1 → 2 → 3] [4 → 5]
  Reverse: [3 → 2 → 1]
  head = 3 (first group)
  prevLast = 1 (now the last node of reversed group)

Group 2: [4 → 5]
  getKthNode(4, 3) = null (less than K)
  Don't reverse, just connect: prevLast.next = 4

Result: 3 → 2 → 1 → 4 → 5
```

**Code**:
```java
// Helper: Reverse a linked list
public ListNode reverseLinkedList(ListNode head) {
    ListNode temp = head;
    ListNode prev = null;
    while (temp != null) {
        ListNode front = temp.next;
        temp.next = prev;
        prev = temp;
        temp = front;
    }
    return prev;
}

// Helper: Get Kth node from current position
public ListNode getKthNode(ListNode temp, int k) {
    k -= 1;  // Already at 1st node
    while (temp != null && k > 0) {
        k--;
        temp = temp.next;
    }
    return temp;
}

// Main function
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode temp = head;
    ListNode prevLast = null;

    while (temp != null) {
        // Get the Kth node of current group
        ListNode kThNode = getKthNode(temp, k);

        // If less than K nodes, don't reverse
        if (kThNode == null) {
            if (prevLast != null) {
                prevLast.next = temp;
            }
            break;
        }

        // Save next group start and disconnect
        ListNode nextNode = kThNode.next;
        kThNode.next = null;

        // Reverse the current group
        reverseLinkedList(temp);

        // Update head for first group
        if (temp == head) {
            head = kThNode;
        } else {
            prevLast.next = kThNode;
        }

        // Update prevLast (temp is now the LAST node after reversal)
        prevLast = temp;

        // Move to next group
        temp = nextNode;
    }

    return head;
}
```

| | Complexity |
|---|---|
| **Time** | O(2N) — each node visited twice (once for getKthNode, once for reversal) |
| **Space** | O(1) — in-place reversal |

---

### ⚡ Key Takeaways
- After reversal, `temp` (original group start) becomes the **LAST** node of the reversed group
- `kThNode` (original group end) becomes the **FIRST** node (new head) of the reversed group
- **Incomplete groups** (less than K) are left as-is and connected to the previous group
- This is a **combination** of reversal + traversal + reconnection

---

## Q10. Check if LL is Palindrome or Not

**🔗 Pattern**: [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare) + [Reversal](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-2-linked-list-reversal)

**Problem**: Given the head of a singly linked list, determine if it is a palindrome.

### Approach 1: Brute (Stack)

**Intuition**: Push all values onto a stack. Then traverse again and compare each node with the stack top. Stack gives reverse order.

**Code**:
```java
public boolean isPalindrome(ListNode head) {
    Stack<Integer> stack = new Stack<>();
    ListNode temp = head;

    // Push all values onto stack
    while (temp != null) {
        stack.push(temp.val);
        temp = temp.next;
    }

    // Compare with stack (reverse order)
    temp = head;
    while (temp != null) {
        if (temp.val != stack.pop()) {
            return false;
        }
        temp = temp.next;
    }

    return true;
}
```

| | Complexity |
|---|---|
| **Time** | O(2N) — two traversals |
| **Space** | O(N) — stack stores all values |

---

### Approach 2: Optimal (Find Middle + Reverse + Compare + Restore)

**Intuition**: Find the middle using slow-fast, reverse the second half, compare both halves, then restore the list.

**Dry Run**:
```
List: 1 → 2 → 3 → 2 → 1

Step 1: Find middle (first middle for even length)
        slow at node 2, fast at node 3

Step 2: Reverse second half starting from slow.next
        Second half: 3 → 2 → 1 becomes 1 → 2 → 3
        newHead = 1

Step 3: Compare first half and reversed second half
        first: 1 → 2 → 3
        second: 1 → 2 → 3
        All match → palindrome!

Step 4: Restore second half (reverse back)
        1 → 2 → 3 becomes 3 → 2 → 1
        Reconnect: 2.next = 3
```

**Code**:
```java
private ListNode reverseLinkedList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextNode = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextNode;
    }
    return prev;
}

public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) return true;

    // Step 1: Find first middle using slow-fast
    ListNode slow = head;
    ListNode fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // Step 2: Reverse second half
    ListNode newHead = reverseLinkedList(slow.next);

    // Step 3: Compare both halves
    ListNode first = head;
    ListNode second = newHead;
    while (second != null) {
        if (first.val != second.val) {
            reverseLinkedList(newHead);  // Restore before returning
            return false;
        }
        first = first.next;
        second = second.next;
    }

    // Step 4: Restore second half
    reverseLinkedList(newHead);

    return true;
}
```

| | Complexity |
|---|---|
| **Time** | O(2N) — find mid + reverse + compare + restore = O(N/2 + N/2 + N/2 + N/2) |
| **Space** | O(1) — only pointers used |

---

### ⚡ Key Takeaways
- **Always restore the list** after modification — interviewers look for this
- Use `fast.next != null && fast.next.next != null` to find **first middle** (needed for even-length lists)
- This problem **combines two patterns**: Slow-Fast Pointer + Reversal
- The restore step is important — without it, the original list is permanently modified

---

## Q11. Find the Starting Point in LL

**🔗 Pattern**: [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

### Approach 1: Brute (HashMap)

**Intuition**: Traverse the list, storing each node in a HashMap. The first node that's already in the map is the cycle start.

**Code**:
```java
public ListNode findStartingPoint(ListNode head) {
    ListNode temp = head;
    HashMap<ListNode, Integer> map = new HashMap<>();

    while (temp != null) {
        if (map.containsKey(temp)) {
            return temp;  // Cycle start found
        }
        map.put(temp, 1);
        temp = temp.next;
    }

    return null;  // No cycle
}
```

| | Complexity |
|---|---|
| **Time** | O(N) — single traversal |
| **Space** | O(N) — HashMap stores all nodes |

---

### Approach 2: Optimal (Tortoise & Hare + Reset)

**Intuition**: 
1. Detect cycle using slow-fast pointers
2. When they meet, reset slow to head
3. Move both by 1 step — they meet at the cycle start

**Mathematical Proof**:
```
L = distance from head to cycle start
K = distance from cycle start to meeting point
C = cycle length

When they meet:
  slow traveled: L + K
  fast traveled: L + K + m*C (m loops)

  Since fast = 2 * slow:
  2(L + K) = L + K + m*C
  L + K = m*C
  L = m*C - K

  This means: from meeting point, moving L steps reaches cycle start
  And from head, moving L steps also reaches cycle start
  So reset one pointer to head, move both by 1 → they meet at cycle start!
```

**Code**:
```java
public ListNode findStartingPoint(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;

    // Phase 1: Detect the loop
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            // Phase 2: Find the starting point
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;  // Cycle start
        }
    }

    return null;  // No cycle
}
```

| | Complexity |
|---|---|
| **Time** | O(N) — detect cycle + find start |
| **Space** | O(1) — only two pointers |

---

### ⚡ Key Takeaways
- This is an **extension of Q8** (Detect Loop) — first detect, then find start
- The math proof `L = m*C - K` is crucial for interviews
- After detection, **both pointers move by 1 step** (not 1 and 2)
- If asked "detect cycle" → just Phase 1. If "find start" → Phase 1 + Phase 2

---

## Q12. Flattening of LL

**🔗 Pattern**: [Merge / Two-List](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-3-merge--two-list-traversal) + [Dummy Node](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-4-dummy-node-technique)

**Problem**: Given a linked list where each node has a `next` pointer (to the next node on the same level) and a `child` pointer (to a sublist), flatten the list such that all nodes appear in a single level sorted list.

### Approach 1: Brute (Array + Sort + Rebuild)

**Intuition**: Extract all values into an array, sort, then rebuild a flat linked list using child pointers.

**Code**:
```java
private ListNode convertArrToLinkedList(List<Integer> arr) {
    ListNode dummyNode = new ListNode(-1);
    ListNode temp = dummyNode;
    for (int i = 0; i < arr.size(); i++) {
        temp.child = new ListNode(arr.get(i));
        temp = temp.child;
    }
    return dummyNode.child;
}

public ListNode flattenLinkedList(ListNode head) {
    List<Integer> arr = new ArrayList<>();

    while (head != null) {
        ListNode t2 = head;
        while (t2 != null) {
            arr.add(t2.val);
            t2 = t2.child;
        }
        head = head.next;
    }

    Collections.sort(arr);
    return convertArrToLinkedList(arr);
}
```

| | Complexity |
|---|---|
| **Time** | O(N×M) + O(N×M log(N×M)) + O(N×M) — traverse + sort + rebuild |
| **Space** | O(N×M) + O(N×M) — array + new list |

---

### Approach 2: Optimal (Recursive Merge)

**Intuition**: Recursively flatten the rest of the list first, then merge the current vertical list with the already-flattened result. This uses the same merge pattern as Q3.

**Dry Run**:
```
5 → 10 → 19 → 28
|    |    |    |
7    19   22   35
|    |    |    |
8    20   25   40
|          |    |
30         50   45

Step 1: Recursively flatten 10 → 19 → 28
  Step 2: Recursively flatten 19 → 28
    Step 3: Recursively flatten 28 → returns 28 → 35 → 40 → 45
  Merge 19 → 22 → 25 → 50 with 28 → 35 → 40 → 45
  = 19 → 22 → 25 → 28 → 35 → 40 → 45 → 50
Merge 5 → 7 → 8 → 30 with 19 → 22 → 25 → 28 → 35 → 40 → 45 → 50
= 5 → 7 → 8 → 19 → 22 → 25 → 28 → 30 → 35 → 40 → 45 → 50
```

**Code**:
```java
private ListNode merge(ListNode list1, ListNode list2) {
    ListNode dummyNode = new ListNode(-1);
    ListNode res = dummyNode;

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
        res.next = null;  // Disconnect next pointer
    }

    if (list1 != null) res.child = list1;
    else res.child = list2;

    if (dummyNode.child != null) dummyNode.child.next = null;
    return dummyNode.child;
}

public ListNode flattenLinkedList(ListNode head) {
    if (head == null || head.next == null) return head;

    // Recursively flatten the rest
    ListNode mergedHead = flattenLinkedList(head.next);

    // Merge current vertical list with flattened rest
    head = merge(head, mergedHead);
    return head;
}
```

| | Complexity |
|---|---|
| **Time** | O(M × N²) — merge sizes: M, 2M, 3M, ..., NM → sum = M×N² |
| **Space** | O(N) — recursive call stack |

---

### ⚡ Key Takeaways
- Uses `child` pointers instead of `next` for vertical traversal
- **Always set `res.next = null`** after merge to prevent cycles from the original structure
- The merge is identical to Q3 but uses `child` instead of `next`
- **Time complexity is O(M×N²)** because each merge increases the list size

---

## Q13. Rotate a LL

**🔗 Pattern**: [Circular / Rotation](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-8-circular--rotation-manipulation)

**Problem**: Given the head of a linked list, rotate the list to the right by K places.

### Approach 1: Brute (Rotate One Step, K Times)

**Intuition**: For each rotation, find the last node, move it to the front. Repeat K times.

**Code**:
```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null) return head;

    for (int i = 0; i < k; i++) {
        ListNode temp = head;
        // Find second last node
        while (temp.next.next != null) temp = temp.next;
        // Get last node
        ListNode end = temp.next;
        // Break link
        temp.next = null;
        // Move last to front
        end.next = head;
        head = end;
    }
    return head;
}
```

| | Complexity |
|---|---|
| **Time** | O(N × K) — K rotations, each O(N) |
| **Space** | O(1) |

---

### Approach 2: Optimal (Make Circular + Break)

**Intuition**: Calculate length, make the list circular, find the new tail position (length - K), break the link there.

**Dry Run**:
```
List: 1 → 2 → 3 → 4 → 5, K = 2

Step 1: length = 5, tail = node 5
Step 2: Make circular: 5.next = 1
Step 3: K = 2 % 5 = 2
Step 4: New tail at position (5 - 2) = 3 from head
        Move temp (at tail=5) 3 steps: temp = 3
Step 5: New head = temp.next = 4
        Break: temp.next = null

Result: 4 → 5 → 1 → 2 → 3
```

**Code**:
```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;

    // Calculate length and find tail
    ListNode temp = head;
    int length = 1;
    while (temp.next != null) {
        length++;
        temp = temp.next;
    }
    // temp is now at tail

    // Make circular
    temp.next = head;

    // Handle K > length
    k = k % length;

    // Find new tail: move (length - k) steps from head
    // Since temp is at tail (connected to head), move (length - k) more steps
    int end = length - k;
    while (end-- > 0) temp = temp.next;

    // New head and break circular link
    head = temp.next;
    temp.next = null;

    return head;
}
```

| | Complexity |
|---|---|
| **Time** | O(N) — calculate length + find break point |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- **Always do `k = k % length`** — rotating by length returns the same list
- The trick is making the list **temporarily circular**, then breaking at the right point
- New tail is at position `(length - k)` from head
- The temp pointer starts at tail (after length calculation), so it needs `(length - k)` more steps

---

## Q14. Clone a LL with Random and Next Pointer

**🔗 Pattern**: [In-Place Modification](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-6-in-place-modification--o1-deletion) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given a linked list where each node has a `next` pointer and a `random` pointer, create a deep copy of the list.

### Approach 1: Brute (HashMap)

**Intuition**: Two passes — first pass creates copies and maps original→copy, second pass sets next and random pointers using the map.

**Code**:
```java
public ListNode copyRandomList(ListNode head) {
    if (head == null) return null;

    HashMap<ListNode, ListNode> map = new HashMap<>();
    ListNode temp = head;

    // Pass 1: Create copies
    while (temp != null) {
        map.put(temp, new ListNode(temp.val));
        temp = temp.next;
    }

    // Pass 2: Set next and random pointers
    temp = head;
    while (temp != null) {
        ListNode copyNode = map.get(temp);
        copyNode.next = map.get(temp.next);
        copyNode.random = map.get(temp.random);
        temp = temp.next;
    }

    return map.get(head);
}
```

| | Complexity |
|---|---|
| **Time** | O(2N) — two passes |
| **Space** | O(N) + O(N) — HashMap + copied list |

---

### Approach 2: Optimal (Interleave Copies In-Place)

**Intuition**: Three steps — (1) Insert copy nodes between originals, (2) Set random pointers using the interleaved structure, (3) Separate copies from originals.

**Why `copy.random = original.random.next` works**: Since copies are right next to originals, the copy of `original.random` is `original.random.next`.

**Dry Run**:
```
Original: 1 → 2 → 3 (with random pointers)

Step 1: Insert copies between originals
1 → 1' → 2 → 2' → 3 → 3'

Step 2: Set random pointers
If 1.random = 3, then 1'.random = 1.random.next = 3.next = 3'
If 2.random = 1, then 2'.random = 2.random.next = 1.next = 1'

Step 3: Separate
Original: 1 → 2 → 3
Copy:     1' → 2' → 3'
```

**Code**:
```java
// Step 1: Insert copy nodes between originals
void insertCopyInBetween(ListNode head) {
    ListNode temp = head;
    while (temp != null) {
        ListNode nextElement = temp.next;
        ListNode copy = new ListNode(temp.val);
        copy.next = nextElement;
        temp.next = copy;
        temp = nextElement;
    }
}

// Step 2: Connect random pointers
void connectRandomPointers(ListNode head) {
    ListNode temp = head;
    while (temp != null) {
        ListNode copyNode = temp.next;
        if (temp.random != null) {
            copyNode.random = temp.random.next;
        } else {
            copyNode.random = null;
        }
        temp = temp.next.next;
    }
}

// Step 3: Separate copies from originals
ListNode getDeepCopyList(ListNode head) {
    ListNode temp = head;
    ListNode dummyNode = new ListNode(-1);
    ListNode res = dummyNode;

    while (temp != null) {
        res.next = temp.next;       // Attach copy
        res = res.next;
        temp.next = temp.next.next;  // Restore original
        temp = temp.next;
    }

    return dummyNode.next;
}

// Main function
ListNode copyRandomList(ListNode head) {
    if (head == null) return null;

    insertCopyInBetween(head);
    connectRandomPointers(head);
    return getDeepCopyList(head);
}
```

| | Complexity |
|---|---|
| **Time** | O(3N) — three passes (insert + connect + separate) |
| **Space** | O(N) — only the copied list (no extra data structures) |

---

### ⚡ Key Takeaways
- **HashMap approach** is simpler to understand and code — good for quick implementation
- **In-place approach** is the optimal solution — no extra data structure needed
- The key insight: `copy.random = original.random.next` works because copies are interleaved
- **Three distinct steps** must be done in order: insert → connect random → separate
- The separation step also **restores the original list**

---

## 🏆 Master Summary - All 14 Problems at a Glance

| # | Problem | Best Approach | Time | Space | Key Pattern |
|---|---------|---------------|------|-------|-------------|
| 1 | Reverse LL | Iterative 3-pointer | O(N) | O(1) | Reversal |
| 2 | Middle of LL | Slow-Fast (Tortoise & Hare) | O(N/2) | O(1) | Slow-Fast |
| 3 | Merge Sorted Lists | Two-pointer + Dummy | O(N1+N2) | O(1) | Merge + Dummy |
| 4 | Remove Nth from End | Fast-Slow gap of N | O(N) | O(1) | Two-Pointer Alignment |
| 5 | Add Two Numbers | Digit-wise + Carry + Dummy | O(max(M,N)) | O(max(M,N)) | Dummy Node |
| 6 | Delete Node O(1) | Value copy + Skip | O(1) | O(1) | In-Place |
| 7 | Intersection Y LL | Switch heads | O(N+M) | O(1) | Two-Pointer Intersection |
| 8 | Detect Loop | Slow-Fast (Tortoise & Hare) | O(N) | O(1) | Slow-Fast |
| 9 | Reverse K Group | Reverse groups + Reconnect | O(2N) | O(1) | Reversal |
| 10 | Palindrome | Mid + Reverse + Compare + Restore | O(2N) | O(1) | Slow-Fast + Reversal |
| 11 | Loop Start | Detect + Reset to head | O(N) | O(1) | Slow-Fast |
| 12 | Flattening | Recursive flatten + Merge | O(M×N²) | O(N) | Merge + Recursion |
| 13 | Rotate LL | Make circular + Break | O(N) | O(1) | Circular |
| 14 | Clone Random | Interleave copies (3-step) | O(3N) | O(N) | In-Place |

---

⬅️ **Back to [Part 1: Pattern Wise Notes](Striver_SDE_Sheet_LinkedList_Series_part-1.md)**
