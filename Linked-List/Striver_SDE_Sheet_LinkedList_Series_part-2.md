# 📖 Striver SDE Sheet - Linked List Series - Part 2: Sequence Wise Detailed Notes

> **Question-by-question solutions with all approaches, code, and complexity analysis.**
> **Click ▶ to expand Dry Runs & Code sections.**

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
        temp = front;                // Move temp forward
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

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
reverseList(1→2→3→NULL)
  └─ reverseList(2→3→NULL)
       └─ reverseList(3→NULL)
            └─ Base case: return 3 (newHead)
       front = 3, front.next = 2, 2.next = null → return 3
  front = 2, front.next = 1, 1.next = null → return 3

Result: 3 → 2 → 1 → NULL
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode newHead = reverseList(head.next);
    ListNode front = head.next;
    front.next = head;
    head.next = null;
    return newHead;
}
```

</details>

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

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode middleOfLinkedList(ListNode head) {
    ListNode temp = head;
    int count = 0;
    while (temp != null) {
        count++;
        temp = temp.next;
    }
    int midPosition = count / 2 + 1;
    ListNode middleNode = head;
    for (int i = 1; i < midPosition; i++) {
        middleNode = middleNode.next;
    }
    return middleNode;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) + O(N/2) = O(N) — two traversals |
| **Space** | O(1) |

---

### Approach 2: Optimal (Tortoise & Hare)

**Intuition**: Slow pointer moves 1 step, fast pointer moves 2 steps. When fast reaches end, slow is at middle.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
Odd:  1 → 2 → 3 → 4 → 5
      s,f=1 → s=2,f=3 → s=3,f=5 → stop
      Middle = 3 ✅

Even: 1 → 2 → 3 → 4 → 5 → 6
      s,f=1 → s=2,f=3 → s=3,f=5 → s=4,f=null → stop
      Middle = 4 (second middle) ✅
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode middleOfLinkedList(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N/2) — fast pointer traverses half the list |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- For **first middle** (even length): use `while (fast.next != null && fast.next.next != null)`
- For **second middle** (even length): use `while (fast != null && fast.next != null)` ← standard
- This is a **sub-operation** used in Q10 (Palindrome)

---

## Q3. Merge Two Sorted Lists

**🔗 Pattern**: [Merge / Two-List](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-3-merge--two-list-traversal) + [Dummy Node](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-4-dummy-node-technique)

**Problem**: You are given two sorted linked lists. Merge them into one sorted list and return the head.

### Approach 1: Brute (Extra Array)

**Intuition**: Extract all values into an array, sort, then create a new linked list.

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ArrayList<Integer> arr = new ArrayList<>();
    ListNode temp1 = list1, temp2 = list2;
    while (temp1 != null) { arr.add(temp1.val); temp1 = temp1.next; }
    while (temp2 != null) { arr.add(temp2.val); temp2 = temp2.next; }
    Collections.sort(arr);
    ListNode dummyNode = new ListNode(-1);
    ListNode temp = dummyNode;
    for (int i = 0; i < arr.size(); i++) {
        temp.next = new ListNode(arr.get(i));
        temp = temp.next;
    }
    return dummyNode.next;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N1+N2) + O(N log N) + O(N) |
| **Space** | O(N) + O(N) — array + new list |

---

### Approach 2: Optimal (Two-Pointer Merge with Dummy Node)

**Intuition**: Since both lists are already sorted, compare heads and pick the smaller one. **Relink existing nodes** instead of creating new ones.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
list1: 1 → 3 → 5
list2: 2 → 4 → 6

1 < 2: dummy → 1, list1→3
3 > 2: 1 → 2, list2→4
3 < 4: 2 → 3, list1→5
5 > 4: 3 → 4, list2→6
5 < 6: 4 → 5, list1→null
list1 null: 5 → 6 (attach remaining)
Result: 1 → 2 → 3 → 4 → 5 → 6
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummyNode = new ListNode(-1);
    ListNode temp = dummyNode;
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) { temp.next = list1; list1 = list1.next; }
        else { temp.next = list2; list2 = list2.next; }
        temp = temp.next;
    }
    if (list1 != null) temp.next = list1;
    else temp.next = list2;
    return dummyNode.next;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N1 + N2) — single pass |
| **Space** | O(1) — only pointers, no new nodes |

---

### ⚡ Key Takeaways
- **Dummy node** eliminates head edge cases
- **Attach remaining list in one step** — no loop needed
- This merge pattern is reused in **Q12 (Flattening)**

---

## Q4. Remove Nth Node from the Back of the LL

**🔗 Pattern**: [Two-Pointer Alignment](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-7-two-pointer-intersection--alignment)

**Problem**: Given the head of a linked list, remove the Nth node from the end and return the head.

### Approach 1: Brute (Count & Traverse)

**Intuition**: Count total nodes, calculate position from front, traverse to the node before it, and delete.

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    if (head == null) return null;
    int cnt = 0;
    ListNode temp = head;
    while (temp != null) { cnt++; temp = temp.next; }
    if (cnt == n) return head.next;
    int res = cnt - n;
    temp = head;
    while (temp != null) { res--; if (res == 0) break; temp = temp.next; }
    temp.next = temp.next.next;
    return head;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(L) + O(L-N) = O(L) |
| **Space** | O(1) |

---

### Approach 2: Optimal (Fast-Slow Gap)

**Intuition**: Move `fast` N steps ahead first. Then move both until `fast.next` is null. Now `slow` is just before the node to delete.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
List: 1 → 2 → 3 → 4 → 5, N = 2

Move fast N=2 steps: fast at 3, slow at 1
Move both: slow=2,fast=4 → slow=3,fast=5 (fast.next=null, stop)
Delete slow.next (node 4): slow.next = slow.next.next
Result: 1 → 2 → 3 → 5
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode fastp = head, slowp = head;
    for (int i = 0; i < n; i++) fastp = fastp.next;
    if (fastp == null) return head.next;
    while (fastp.next != null) { fastp = fastp.next; slowp = slowp.next; }
    slowp.next = slowp.next.next;
    return head;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) — single traversal |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- The **gap of N** between fast and slow is the key insight
- When `fast == null` after N steps → **delete the head**

---

## Q5. Add Two Numbers as LinkedList

**🔗 Pattern**: [Dummy Node](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-4-dummy-node-technique)

**Problem**: You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order. Add the two numbers and return the sum as a linked list.

### Approach: Digit-wise Addition with Carry

**Intuition**: Add digits position by position (already in reverse order). Maintain a carry. Use a dummy node to build the result.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
l1: 2 → 4 → 3  (342)
l2: 5 → 6 → 4  (465)

2+5 = 7, carry=0 → 7
4+6 = 10, carry=1 → 0
3+4+1 = 8, carry=0 → 8

Result: 7 → 0 → 8  (807 = 342+465)
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode();
    ListNode temp = dummy;
    int carry = 0;
    while ((l1 != null || l2 != null) || carry != 0) {
        int sum = 0;
        if (l1 != null) { sum += l1.val; l1 = l1.next; }
        if (l2 != null) { sum += l2.val; l2 = l2.next; }
        sum += carry;
        carry = sum / 10;
        ListNode node = new ListNode(sum % 10);
        temp.next = node;
        temp = temp.next;
    }
    return dummy.next;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(max(M, N)) |
| **Space** | O(max(M, N)) — result list |

---

### ⚡ Key Takeaways
- **Carry must be in while condition** — if both lists end but carry remains (e.g., 9+9=18)
- `sum % 10` = digit, `sum / 10` = carry

---

## Q6. Delete Node in a Linked List O(1)

**🔗 Pattern**: [In-Place Modification](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-6-in-place-modification--o1-deletion)

**Problem**: Given a node in a singly linked list, delete it without access to the head. It is guaranteed the node is not the tail.

### Approach: Value Copy + Skip

**Intuition**: Copy the next node's value into the current node, then skip the next node.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
List: 4 → 5 → 1 → 9, Delete node 5

Step 1: node.val = 1 → 4 → 1 → 1 → 9
Step 2: node.next = node.next.next → 4 → 1 → 9
Result: 4 → 1 → 9 (5 effectively deleted)
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public void deleteNode(ListNode node) {
    node.val = node.next.val;
    node.next = node.next.next;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(1) |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- **Cannot work if node is the tail** — no next node to copy from
- You don't actually delete the given node — you make it a copy of the next node and delete that

---

## Q7. Find the Intersection Point of Y LL

**🔗 Pattern**: [Two-Pointer Intersection](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-7-two-pointer-intersection--alignment) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given the heads of two singly linked lists, return the node at which the two lists intersect. If they don't intersect, return null.

### Approach 1: Brute (HashSet)

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    HashSet<ListNode> nodes_set = new HashSet<>();
    while (headA != null) { nodes_set.add(headA); headA = headA.next; }
    while (headB != null) {
        if (nodes_set.contains(headB)) return headB;
        headB = headB.next;
    }
    return null;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N + M) |
| **Space** | O(N) |

---

### Approach 2: Better (Length Alignment)

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode temp1 = headA, temp2 = headB;
    int n1 = 0, n2 = 0;
    while (temp1 != null) { n1++; temp1 = temp1.next; }
    while (temp2 != null) { n2++; temp2 = temp2.next; }
    if (n1 < n2) return collisionPoint(headA, headB, n2 - n1);
    return collisionPoint(headB, headA, n1 - n2);
}
private ListNode collisionPoint(ListNode smaller, ListNode longer, int len) {
    ListNode t1 = smaller, t2 = longer;
    for (int i = 0; i < len; i++) t2 = t2.next;
    while (t1 != t2) { t1 = t1.next; t2 = t2.next; }
    return t1;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N + M) |
| **Space** | O(1) |

---

### Approach 3: Optimal (Switch Heads)

**Intuition**: When either pointer reaches the end, switch it to the head of the other list. Both travel same total distance.

<details>
<summary>🔍 Why It Works (Click to expand)</summary>

```
List A: a1 → a2 → a3 ↘
                       c1 → c2 → null
List B:      b1 → b2 ↗

Pointer 1: A→B (travels lenA + lenB)
Pointer 2: B→A (travels lenB + lenA)
Same total distance → meet at intersection!
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;
    ListNode d1 = headA, d2 = headB;
    while (d1 != d2) {
        d1 = d1.next;
        d2 = d2.next;
        if (d1 == d2) return d1;
        if (d1 == null) d1 = headB;
        if (d2 == null) d2 = headA;
    }
    return d1;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N + M) |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- **Compare node references**, not values
- If no intersection, both reach null simultaneously after N+M steps

---

## Q8. Detect a Loop in LL

**🔗 Pattern**: [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given head of a linked list, determine if the list has a cycle in it.

### Approach 1: Brute (HashSet)

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public boolean hasCycle(ListNode head) {
    ListNode temp = head;
    HashSet<ListNode> nodeSet = new HashSet<>();
    while (temp != null) {
        if (nodeSet.contains(temp)) return true;
        nodeSet.add(temp);
        temp = temp.next;
    }
    return false;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) |
| **Space** | O(N) |

---

### Approach 2: Optimal (Tortoise & Hare)

**Intuition**: Slow moves 1 step, fast moves 2 steps. If cycle exists, they'll meet. If no cycle, fast reaches null.

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- In a cycle, fast gains 1 step on slow each iteration → gap decreases → they MUST meet
- This is the **foundation** for Q11 (Find Starting Point)

---

## Q9. Reverse LL in Group of Given Size K

**🔗 Pattern**: [Reversal](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-2-linked-list-reversal)

**Problem**: Given the head of a linked list, reverse every K nodes. If the remaining nodes are less than K, leave them as is.

### Approach: Reverse Groups + Reconnect

**Intuition**: For each group: (1) Find Kth node, (2) Disconnect group, (3) Reverse, (4) Reconnect to previous group.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
List: 1 → 2 → 3 → 4 → 5, K = 3

Group 1: [1→2→3]
  getKthNode(1, 3) = node 3
  Disconnect: 3.next = null
  Reverse: [3→2→1], head = 3, prevLast = 1

Group 2: [4→5]
  getKthNode(4, 3) = null (less than K)
  Don't reverse, connect: prevLast.next = 4

Result: 3 → 2 → 1 → 4 → 5
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode reverseLinkedList(ListNode head) {
    ListNode temp = head, prev = null;
    while (temp != null) {
        ListNode front = temp.next;
        temp.next = prev;
        prev = temp;
        temp = front;
    }
    return prev;
}

public ListNode getKthNode(ListNode temp, int k) {
    k -= 1;
    while (temp != null && k > 0) { k--; temp = temp.next; }
    return temp;
}

public ListNode reverseKGroup(ListNode head, int k) {
    ListNode temp = head, prevLast = null;
    while (temp != null) {
        ListNode kThNode = getKthNode(temp, k);
        if (kThNode == null) {
            if (prevLast != null) prevLast.next = temp;
            break;
        }
        ListNode nextNode = kThNode.next;
        kThNode.next = null;
        reverseLinkedList(temp);
        if (temp == head) head = kThNode;
        else prevLast.next = kThNode;
        prevLast = temp;
        temp = nextNode;
    }
    return head;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(2N) — each node visited twice |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- After reversal: `temp` = LAST node, `kThNode` = FIRST node of reversed group
- **Incomplete groups** (< K) are left as-is

---

## Q10. Check if LL is Palindrome or Not

**🔗 Pattern**: [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare) + [Reversal](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-2-linked-list-reversal)

**Problem**: Given the head of a singly linked list, determine if it is a palindrome.

### Approach 1: Brute (Stack)

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public boolean isPalindrome(ListNode head) {
    Stack<Integer> stack = new Stack<>();
    ListNode temp = head;
    while (temp != null) { stack.push(temp.val); temp = temp.next; }
    temp = head;
    while (temp != null) {
        if (temp.val != stack.pop()) return false;
        temp = temp.next;
    }
    return true;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(2N) |
| **Space** | O(N) |

---

### Approach 2: Optimal (Find Middle + Reverse + Compare + Restore)

**Intuition**: Find middle → reverse second half → compare → restore.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
List: 1 → 2 → 3 → 2 → 1

Step 1: Find middle: slow at 2
Step 2: Reverse second half: 3→2→1 becomes 1→2→3
Step 3: Compare: 1→2→3 vs 1→2→3 → match!
Step 4: Restore: reverse back 1→2→3 to 3→2→1, reconnect
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
private ListNode reverseLinkedList(ListNode head) {
    ListNode prev = null, curr = head;
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
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next; fast = fast.next.next;
    }
    ListNode newHead = reverseLinkedList(slow.next);
    ListNode first = head, second = newHead;
    while (second != null) {
        if (first.val != second.val) {
            reverseLinkedList(newHead); return false;
        }
        first = first.next; second = second.next;
    }
    reverseLinkedList(newHead);
    return true;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(2N) |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- **Always restore the list** after modification — interviewers look for this
- Use `fast.next != null && fast.next.next != null` for **first middle**
- Combines **Slow-Fast + Reversal** patterns

---

## Q11. Find the Starting Point in LL

**🔗 Pattern**: [Slow-Fast Pointer](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-1-two-pointer-slow-fast--tortoise--hare) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

### Approach 1: Brute (HashMap)

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode findStartingPoint(ListNode head) {
    ListNode temp = head;
    HashMap<ListNode, Integer> map = new HashMap<>();
    while (temp != null) {
        if (map.containsKey(temp)) return temp;
        map.put(temp, 1);
        temp = temp.next;
    }
    return null;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) |
| **Space** | O(N) |

---

### Approach 2: Optimal (Tortoise & Hare + Reset)

**Intuition**: (1) Detect cycle, (2) Reset slow to head, (3) Move both by 1 — they meet at cycle start.

<details>
<summary>📐 Mathematical Proof (Click to expand)</summary>

```
L = head to cycle start, K = cycle start to meeting point, C = cycle length

slow = L + K, fast = L + K + m*C
Since fast = 2*slow: 2(L+K) = L+K+m*C → L+K = m*C → L = m*C - K

From meeting point, moving L steps = cycle start
From head, moving L steps = cycle start
→ Reset one to head, move both by 1 → meet at cycle start!
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode findStartingPoint(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            slow = head;
            while (slow != fast) { slow = slow.next; fast = fast.next; }
            return slow;
        }
    }
    return null;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- Extension of Q8 — first detect, then find start
- Math proof `L = m*C - K` is crucial for interviews
- After detection, **both pointers move by 1 step**

---

## Q12. Flattening of LL

**🔗 Pattern**: [Merge / Two-List](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-3-merge--two-list-traversal) + [Dummy Node](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-4-dummy-node-technique)

**Problem**: Given a linked list where each node has `next` and `child` pointers, flatten it into a single level sorted list.

### Approach 1: Brute (Array + Sort + Rebuild)

<details>
<summary>💻 Code (Click to expand)</summary>

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
        while (t2 != null) { arr.add(t2.val); t2 = t2.child; }
        head = head.next;
    }
    Collections.sort(arr);
    return convertArrToLinkedList(arr);
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N×M) + O(N×M log(N×M)) + O(N×M) |
| **Space** | O(N×M) + O(N×M) |

---

### Approach 2: Optimal (Recursive Merge)

**Intuition**: Recursively flatten the rest, then merge current vertical list with flattened result.

<details>
<summary>💻 Code (Click to expand)</summary>

```java
private ListNode merge(ListNode list1, ListNode list2) {
    ListNode dummyNode = new ListNode(-1);
    ListNode res = dummyNode;
    while (list1 != null && list2 != null) {
        if (list1.val < list2.val) { res.child = list1; res = list1; list1 = list1.child; }
        else { res.child = list2; res = list2; list2 = list2.child; }
        res.next = null;
    }
    if (list1 != null) res.child = list1;
    else res.child = list2;
    if (dummyNode.child != null) dummyNode.child.next = null;
    return dummyNode.child;
}

public ListNode flattenLinkedList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode mergedHead = flattenLinkedList(head.next);
    head = merge(head, mergedHead);
    return head;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(M × N²) |
| **Space** | O(N) — recursive call stack |

---

### ⚡ Key Takeaways
- Uses `child` pointers instead of `next`
- Always set `res.next = null` after merge to prevent cycles
- Time complexity O(M×N²) because each merge increases list size

---

## Q13. Rotate a LL

**🔗 Pattern**: [Circular / Rotation](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-8-circular--rotation-manipulation)

**Problem**: Given the head of a linked list, rotate the list to the right by K places.

### Approach 1: Brute (Rotate One Step, K Times)

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null) return head;
    for (int i = 0; i < k; i++) {
        ListNode temp = head;
        while (temp.next.next != null) temp = temp.next;
        ListNode end = temp.next;
        temp.next = null;
        end.next = head;
        head = end;
    }
    return head;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N × K) |
| **Space** | O(1) |

---

### Approach 2: Optimal (Make Circular + Break)

**Intuition**: Calculate length, make list circular, find new tail at (length - K), break there.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
List: 1 → 2 → 3 → 4 → 5, K = 2

Step 1: length = 5, tail = node 5
Step 2: Make circular: 5.next = 1
Step 3: K = 2 % 5 = 2
Step 4: Move temp 3 steps from tail: temp = node 3
Step 5: New head = temp.next = 4, Break: temp.next = null
Result: 4 → 5 → 1 → 2 → 3
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;
    ListNode temp = head;
    int length = 1;
    while (temp.next != null) { length++; temp = temp.next; }
    temp.next = head;          // Make circular
    k = k % length;            // Handle K > length
    int end = length - k;
    while (end-- > 0) temp = temp.next;
    head = temp.next;          // New head
    temp.next = null;          // Break circular link
    return head;
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(N) |
| **Space** | O(1) |

---

### ⚡ Key Takeaways
- Always `k = k % length` — rotating by length returns same list
- Temporarily make circular, then break at right point
- New tail at position `(length - k)` from head

---

## Q14. Clone a LL with Random and Next Pointer

**🔗 Pattern**: [In-Place Modification](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-6-in-place-modification--o1-deletion) | [Hashing (Brute)](Striver_SDE_Sheet_LinkedList_Series_part-1.md#pattern-5-hashing--node-tracking)

**Problem**: Given a linked list where each node has `next` and `random` pointers, create a deep copy.

### Approach 1: Brute (HashMap)

<details>
<summary>💻 Code (Click to expand)</summary>

```java
public ListNode copyRandomList(ListNode head) {
    if (head == null) return null;
    HashMap<ListNode, ListNode> map = new HashMap<>();
    ListNode temp = head;
    while (temp != null) { map.put(temp, new ListNode(temp.val)); temp = temp.next; }
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

</details>

| | Complexity |
|---|---|
| **Time** | O(2N) |
| **Space** | O(N) + O(N) — HashMap + copied list |

---

### Approach 2: Optimal (Interleave Copies In-Place)

**Intuition**: (1) Insert copies between originals, (2) Set random pointers, (3) Separate copies from originals. Key: `copy.random = original.random.next`.

<details>
<summary>🔍 Dry Run (Click to expand)</summary>

```
Original: 1 → 2 → 3 (with random pointers)

Step 1: Insert copies: 1 → 1' → 2 → 2' → 3 → 3'
Step 2: Set random: 1'.random = 1.random.next
Step 3: Separate: Original: 1→2→3, Copy: 1'→2'→3'
```

</details>

<details>
<summary>💻 Code (Click to expand)</summary>

```java
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

void connectRandomPointers(ListNode head) {
    ListNode temp = head;
    while (temp != null) {
        ListNode copyNode = temp.next;
        if (temp.random != null) copyNode.random = temp.random.next;
        else copyNode.random = null;
        temp = temp.next.next;
    }
}

ListNode getDeepCopyList(ListNode head) {
    ListNode temp = head;
    ListNode dummyNode = new ListNode(-1);
    ListNode res = dummyNode;
    while (temp != null) {
        res.next = temp.next;
        res = res.next;
        temp.next = temp.next.next;
        temp = temp.next;
    }
    return dummyNode.next;
}

ListNode copyRandomList(ListNode head) {
    if (head == null) return null;
    insertCopyInBetween(head);
    connectRandomPointers(head);
    return getDeepCopyList(head);
}
```

</details>

| | Complexity |
|---|---|
| **Time** | O(3N) — three passes |
| **Space** | O(N) — only the copied list |

---

### ⚡ Key Takeaways
- HashMap approach is simpler; In-place is optimal
- `copy.random = original.random.next` works because copies are interleaved
- Three steps in order: insert → connect random → separate
- Separation also restores the original list

---

## 🏆 Master Summary - All 14 Problems at a Glance

| # | Problem | Best Approach | Time | Space | Key Pattern |
|---|---------|---------------|------|-------|-------------|
| 1 | Reverse LL | Iterative 3-pointer | O(N) | O(1) | Reversal |
| 2 | Middle of LL | Slow-Fast | O(N/2) | O(1) | Slow-Fast |
| 3 | Merge Sorted Lists | Two-pointer + Dummy | O(N1+N2) | O(1) | Merge + Dummy |
| 4 | Remove Nth from End | Fast-Slow gap of N | O(N) | O(1) | Two-Pointer Alignment |
| 5 | Add Two Numbers | Digit-wise + Carry + Dummy | O(max(M,N)) | O(max(M,N)) | Dummy Node |
| 6 | Delete Node O(1) | Value copy + Skip | O(1) | O(1) | In-Place |
| 7 | Intersection Y LL | Switch heads | O(N+M) | O(1) | Two-Pointer Intersection |
| 8 | Detect Loop | Slow-Fast | O(N) | O(1) | Slow-Fast |
| 9 | Reverse K Group | Reverse groups + Reconnect | O(2N) | O(1) | Reversal |
| 10 | Palindrome | Mid + Reverse + Compare + Restore | O(2N) | O(1) | Slow-Fast + Reversal |
| 11 | Loop Start | Detect + Reset to head | O(N) | O(1) | Slow-Fast |
| 12 | Flattening | Recursive flatten + Merge | O(M×N²) | O(N) | Merge + Recursion |
| 13 | Rotate LL | Make circular + Break | O(N) | O(1) | Circular |
| 14 | Clone Random | Interleave copies (3-step) | O(3N) | O(N) | In-Place |

---

⬅️ **Back to [Part 1: Pattern Wise Notes](Striver_SDE_Sheet_LinkedList_Series_part-1.md)**
