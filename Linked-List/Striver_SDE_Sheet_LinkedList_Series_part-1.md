# 🧠 Striver SDE Sheet - Linked List Series - Part 1: Pattern Wise Notes

> **Master Linked List problems by recognizing patterns, not memorizing solutions.**

---

## 📑 Pattern Index (Click to Navigate)

| # | Pattern | Core Idea | Questions |
|---|---------|-----------|-----------|
| 1 | [Two Pointer (Slow-Fast / Tortoise & Hare)](#pattern-1-two-pointer-slow-fast--tortoise--hare) | Move two pointers at different speeds | Q2, Q8, Q10, Q11 |
| 2 | [Linked List Reversal](#pattern-2-linked-list-reversal) | Reverse direction of pointers | Q1, Q9, Q10 |
| 3 | [Merge / Two-List Traversal](#pattern-3-merge--two-list-traversal) | Combine two sorted lists into one | Q3, Q12 |
| 4 | [Dummy Node Technique](#pattern-4-dummy-node-technique) | Simplify edge cases at head | Q3, Q5, Q12 |
| 5 | [Hashing / Node Tracking](#pattern-5-hashing--node-tracking) | Track visited nodes using HashSet/HashMap | Q7, Q8, Q11, Q14 |
| 6 | [In-Place Modification / O(1) Deletion](#pattern-6-in-place-modification--o1-deletion) | Modify structure without extra space | Q6, Q14 |
| 7 | [Two-Pointer Intersection / Alignment](#pattern-7-two-pointer-intersection--alignment) | Align two lists to find meeting point | Q4, Q7 |
| 8 | [Circular / Rotation Manipulation](#pattern-8-circular--rotation-manipulation) | Temporarily make list circular, break at point | Q13 |

---

## 🔗 Question-to-Pattern Mapping

| Question | Primary Pattern | Secondary Pattern |
|----------|----------------|-------------------|
| Q1. Reverse a LL | Reversal | — |
| Q2. Find Middle of LL | Slow-Fast Pointer | — |
| Q3. Merge two Sorted Lists | Merge / Two-List | Dummy Node |
| Q4. Remove Nth Node from Back | Two-Pointer Alignment | — |
| Q5. Add Two Numbers as LL | Dummy Node | — |
| Q6. Delete Node in O(1) | In-Place Modification | — |
| Q7. Intersection Point of Y LL | Two-Pointer Intersection | Hashing (Brute) |
| Q8. Detect a Loop in LL | Slow-Fast Pointer | Hashing (Brute) |
| Q9. Reverse LL in Group of K | Reversal | — |
| Q10. Check if LL is Palindrome | Slow-Fast Pointer + Reversal | — |
| Q11. Find Starting Point of Loop | Slow-Fast Pointer | Hashing (Brute) |
| Q12. Flattening of LL | Merge / Two-List | Dummy Node |
| Q13. Rotate a LL | Circular / Rotation | — |
| Q14. Clone LL with Random Pointer | In-Place Modification | Hashing (Brute) |

---

## Pattern 1: Two Pointer (Slow-Fast / Tortoise & Hare)

### 🎯 Core Concept
Use two pointers moving at different speeds through the list. The **slow pointer moves 1 step** and the **fast pointer moves 2 steps**. Their relative position reveals information about the list structure.

### 🔍 When to Identify
- **Find middle** → When fast reaches end, slow is at middle
- **Detect cycle** → If they meet inside a loop, cycle exists
- **Find cycle start** → After meeting, reset one pointer to head, move both 1 step
- **Find palindrome** → Use slow-fast to find middle, then reverse second half

<details>
<summary>📐 Mathematical Intuition (Click to expand)</summary>

```
Before loop:    L nodes (from head to loop start)
Inside loop:    C nodes (loop length)
Meeting point:  K nodes into the loop from loop start

When slow and fast meet:
  Distance(slow) = L + K
  Distance(fast) = L + K + m*C  (m = number of extra loops fast made)
  
  Since fast = 2 * slow:
  2(L + K) = L + K + m*C
  L + K = m*C
  L = m*C - K
  
  This means: from meeting point, moving L steps = moving (m*C - K) steps
  Which is exactly back to the loop start!
  
  That's why resetting one pointer to head and moving both by 1 works.
```

</details>

<details>
<summary>🧩 Global Template (Click to expand)</summary>

```java
// ===== TEMPLATE: Slow-Fast Pointer =====

// Variant 1: Find Middle
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;       // moves 1 step
    fast = fast.next.next;  // moves 2 steps
}
// slow is now at middle
return slow;

// Variant 2: Detect Cycle
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) return true;  // cycle detected
}
return false;  // no cycle

// Variant 3: Find Cycle Start (after detection)
// After slow == fast (meeting point):
slow = head;  // reset slow to head
while (slow != fast) {
    slow = slow.next;  // both move 1 step
    fast = fast.next;
}
return slow;  // cycle start node

// Variant 4: Find Middle (for palindrome - stop at first middle)
ListNode slow = head, fast = head;
while (fast.next != null && fast.next.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
// slow is at FIRST middle (for even length) or exact middle (for odd)
```

</details>

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q2. Find Middle ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q2-find-middle-of-linked-list) | Slow moves 1, fast moves 2. For **first middle** use `fast.next != null && fast.next.next != null` |
| [Q8. Detect Loop ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q8-detect-a-loop-in-ll) | If slow and fast meet → cycle. Fast "laps" slow, gap decreases by 1 each step |
| [Q10. Palindrome ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q10-check-if-ll-is-palindrome-or-not) | Combines **Slow-Fast + Reversal**. Always restore the list after! |
| [Q11. Loop Start ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q11-find-the-starting-point-in-ll) | After detection, reset slow to head, move both by 1. Math: `L = m*C - K` |

---

## Pattern 2: Linked List Reversal

### 🎯 Core Concept
Reverse the direction of `next` pointers so the list flows in the opposite direction. This is the **most fundamental building block** in linked list problems.

### 🔍 When to Identify
- **Reverse entire list** → Direct application
- **Reverse in groups** → Reverse segments + reconnect
- **Process from end** → Reverse first, then process, then reverse back
- **Compare halves** → Reverse one half, compare, then restore

<details>
<summary>📐 How It Works - 3-Pointer Approach (Click to expand)</summary>

```
Initial:  NULL ← [prev]    [head/curr] → [2] → [3] → NULL
                              ↑temp

Step 1:   Save next node
          front = temp.next

Step 2:   Reverse the link
          temp.next = prev

Step 3:   Move pointers forward
          prev = temp
          temp = front

After:    NULL ← [1] ← [prev]    [2/temp] → [3] → NULL
                              ↑front

Continue until temp == null.
prev is the new head.
```

</details>

<details>
<summary>🧩 Global Template (Click to expand)</summary>

```java
// ===== TEMPLATE: Iterative Reversal =====
public ListNode reverseLinkedList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextNode = curr.next;  // Step 1: Save next
        curr.next = prev;               // Step 2: Reverse link
        prev = curr;                     // Step 3: Move prev
        curr = nextNode;                 // Step 4: Move curr
    }
    return prev;  // New head
}

// ===== TEMPLATE: Recursive Reversal =====
public ListNode reverseLinkedList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode newHead = reverseLinkedList(head.next);
    ListNode front = head.next;
    front.next = head;
    head.next = null;
    return newHead;
}

// ===== TEMPLATE: Reverse K Group =====
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode temp = head;
    ListNode prevLast = null;
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

private ListNode getKthNode(ListNode temp, int k) {
    k -= 1;
    while (temp != null && k > 0) { k--; temp = temp.next; }
    return temp;
}
```

</details>

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q1. Reverse LL ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q1-reverse-a-linked-list) | Iterative O(1) space preferred. After reversal, `prev` = new head |
| [Q9. Reverse K Group ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q9-reverse-ll-in-group-of-given-size-k) | After reversal, `temp` becomes LAST node, `kThNode` becomes FIRST |
| [Q10. Palindrome ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q10-check-if-ll-is-palindrome-or-not) | Reversal as sub-operation. Always restore the list after! |

---

## Pattern 3: Merge / Two-List Traversal

### 🎯 Core Concept
Traverse two sorted linked lists simultaneously, picking the smaller element at each step to build a merged sorted list.

### 🔍 When to Identify
- **Merge two sorted lists** → Direct application
- **Flatten a list with child pointers** → Merge pairs of vertical lists recursively
- **Any "combine two sorted sequences"** problem

<details>
<summary>🧩 Global Template (Click to expand)</summary>

```java
// ===== TEMPLATE: Merge Two Sorted Lists =====
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(-1);
    ListNode temp = dummy;
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) { temp.next = list1; list1 = list1.next; }
        else { temp.next = list2; list2 = list2.next; }
        temp = temp.next;
    }
    if (list1 != null) temp.next = list1;
    else temp.next = list2;
    return dummy.next;
}

// ===== TEMPLATE: Flatten Linked List (Recursive Merge) =====
public ListNode flattenLinkedList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode mergedHead = flattenLinkedList(head.next);
    head = merge(head, mergedHead);
    return head;
}

private ListNode merge(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(-1);
    ListNode res = dummy;
    while (list1 != null && list2 != null) {
        if (list1.val < list2.val) { res.child = list1; res = list1; list1 = list1.child; }
        else { res.child = list2; res = list2; list2 = list2.child; }
        res.next = null;
    }
    if (list1 != null) res.child = list1;
    else res.child = list2;
    if (dummy.child != null) dummy.child.next = null;
    return dummy.child;
}
```

</details>

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q3. Merge Sorted Lists ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q3-merge-two-sorted-lists) | Relink existing nodes, attach remaining in one step. O(1) space |
| [Q12. Flattening ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q12-flattening-of-ll) | Uses `child` pointers. Always set `next = null` after merge |

---

## Pattern 4: Dummy Node Technique

### 🎯 Core Concept
Create a placeholder "dummy" node before the actual head. This eliminates special-case handling for the head node.

### 🔍 When to Identify
- **Building a new list** from scratch
- **Merging lists** where head is determined later
- **Carry propagation** (add two numbers)

<details>
<summary>🧩 Global Template (Click to expand)</summary>

```java
// ===== TEMPLATE: Dummy Node Pattern =====
ListNode dummy = new ListNode(-1);
ListNode temp = dummy;
while (/* condition */) {
    temp.next = someNode;
    temp = temp.next;
}
return dummy.next;

// ===== TEMPLATE: Array to Linked List (using dummy) =====
ListNode dummy = new ListNode(-1);
ListNode temp = dummy;
for (int val : arr) {
    temp.next = new ListNode(val);
    temp = temp.next;
}
return dummy.next;

// ===== TEMPLATE: Array to Linked List (without dummy) =====
ListNode head = null, temp = null;
for (int val : arr) {
    ListNode newNode = new ListNode(val);
    if (head == null) { head = newNode; temp = newNode; }
    else { temp.next = newNode; temp = temp.next; }
}
return head;
```

</details>

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q3. Merge Sorted Lists ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q3-merge-two-sorted-lists) | Dummy handles case where either list could provide first element |
| [Q5. Add Two Numbers ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q5-add-two-numbers-as-linkedlist) | Result built digit by digit with carry |
| [Q12. Flattening ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q12-flattening-of-ll) | Dummy in merge helper for child-pointer lists |

---

## Pattern 5: Hashing / Node Tracking

### 🎯 Core Concept
Use a HashSet or HashMap to store node references as you traverse. This allows O(1) lookup to detect if a node has been visited before.

### 🔍 When to Identify
- **Detect cycle/loop** → If you see a node again, there's a cycle
- **Find intersection** → Store one list's nodes, check against the other
- **Clone with random pointers** → Map original nodes to their copies

| Scenario | Hashing | Slow-Fast |
|----------|---------|-----------|
| Need O(1) space | ❌ O(N) space | ✅ O(1) space |
| Need the actual node | ✅ Direct | Needs extra step |
| Simpler to code | ✅ Intuitive | Needs math proof |
| Interview preference | Brute/Better | Optimal ✅ |

<details>
<summary>🧩 Global Template (Click to expand)</summary>

```java
// ===== TEMPLATE: HashSet for Cycle Detection =====
HashSet<ListNode> set = new HashSet<>();
ListNode temp = head;
while (temp != null) {
    if (set.contains(temp)) return temp;  // or true
    set.add(temp);
    temp = temp.next;
}
return null;  // or false

// ===== TEMPLATE: HashMap for Cloning =====
HashMap<ListNode, ListNode> map = new HashMap<>();
ListNode temp = head;
while (temp != null) { map.put(temp, new ListNode(temp.val)); temp = temp.next; }
temp = head;
while (temp != null) {
    ListNode copy = map.get(temp);
    copy.next = map.get(temp.next);
    copy.random = map.get(temp.random);
    temp = temp.next;
}
return map.get(head);
```

</details>

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q7. Intersection (Brute) ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q7-find-the-intersection-point-of-y-ll) | Store list A nodes in HashSet → check list B |
| [Q8. Detect Loop (Brute) ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q8-detect-a-loop-in-ll) | Add each node to HashSet → if exists, cycle |
| [Q11. Loop Start (Brute) ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q11-find-the-starting-point-in-ll) | First repeated node in HashMap = cycle start |
| [Q14. Clone Random (Brute) ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q14-clone-a-ll-with-random-and-next-pointer) | Two-pass: create copies → set pointers using map |

---

## Pattern 6: In-Place Modification / O(1) Deletion

### 🎯 Core Concept
Modify the linked list structure **in-place** without using extra data structures. Often involves clever pointer manipulation or value copying.

### 🔍 When to Identify
- **Delete a node without head access** → Copy next node's value, skip next
- **Clone list without HashMap** → Interleave copies between originals

<details>
<summary>📐 Key Techniques (Click to expand)</summary>

**Technique 1: Value Copy Deletion**
```
Given: ... → [A] → [B] → [C] → ...
Delete B (but only have pointer to B):

Step 1: Copy C's value to B: ... → [A] → [C'] → [C] → ...
Step 2: Skip C: ... → [A] → [C'] → ...
Result: B is effectively deleted
```

**Technique 2: Interleaving Copies (for cloning)**
```
Original: [1] → [2] → [3] → null
After insert: [1] → [1'] → [2] → [2'] → [3] → [3'] → null
Set random: copy.random = original.random.next
Separate: [1] → [2] → [3] → null  and  [1'] → [2'] → [3'] → null
```

</details>

<details>
<summary>🧩 Global Template (Click to expand)</summary>

```java
// ===== TEMPLATE: O(1) Deletion =====
public void deleteNode(ListNode node) {
    node.val = node.next.val;
    node.next = node.next.next;
}

// ===== TEMPLATE: Clone with Random Pointer (In-Place) =====
// Step 1: Insert copies between originals
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
        if (temp.random != null) copyNode.random = temp.random.next;
        else copyNode.random = null;
        temp = temp.next.next;
    }
}

// Step 3: Separate copies from originals
ListNode getDeepCopyList(ListNode head) {
    ListNode temp = head;
    ListNode dummy = new ListNode(-1);
    ListNode res = dummy;
    while (temp != null) {
        res.next = temp.next;
        res = res.next;
        temp.next = temp.next.next;
        temp = temp.next;
    }
    return dummy.next;
}
```

</details>

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q6. Delete Node O(1) ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q6-delete-node-in-a-linked-list-o1) | Copy next value + skip next. Can't work on tail node |
| [Q14. Clone Random (Optimal) ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q14-clone-a-ll-with-random-and-next-pointer) | `copy.random = original.random.next` works because copies are interleaved |

---

## Pattern 7: Two-Pointer Intersection / Alignment

### 🎯 Core Concept
When two linked lists of different lengths need to meet at a common point, first **align their starting positions** so both pointers travel the same remaining distance.

### 🔍 When to Identify
- **Find intersection of Y-shaped lists** → Align lengths, then move together
- **Remove Nth node from end** → Create a gap of N between two pointers

<details>
<summary>📐 Key Intuition (Click to expand)</summary>

**For Intersection:**
```
List A: [a1] → [a2] → [a3] ↘
                            [c1] → [c2] → null
List B:       [b1] → [b2] ↗

Optimal: Switch heads when reaching end!
Pointer 1: A → B (travels lenA + lenB steps)
Pointer 2: B → A (travels lenB + lenA steps)
They meet at intersection after same total distance!
```

**For Remove Nth from End:**
```
List: [1] → [2] → [3] → [4] → [5], N = 2

Move fast N steps ahead: fast at [3]
Move both until fast.next == null: slow at [3]
slow.next = slow.next.next → deletes [4]
```

</details>

<details>
<summary>🧩 Global Template (Click to expand)</summary>

```java
// ===== TEMPLATE: Find Intersection (Switch Heads) =====
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

// ===== TEMPLATE: Find Intersection (Length Alignment) =====
private ListNode collisionPoint(ListNode smaller, ListNode longer, int diff) {
    ListNode t1 = smaller, t2 = longer;
    for (int i = 0; i < diff; i++) t2 = t2.next;
    while (t1 != t2) { t1 = t1.next; t2 = t2.next; }
    return t1;
}

// ===== TEMPLATE: Remove Nth Node from End =====
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode fast = head, slow = head;
    for (int i = 0; i < n; i++) fast = fast.next;
    if (fast == null) return head.next;
    while (fast.next != null) { fast = fast.next; slow = slow.next; }
    slow.next = slow.next.next;
    return head;
}
```

</details>

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q4. Remove Nth from Back ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q4-remove-nth-node-from-the-back-of-the-ll) | Gap of N between fast and slow. If fast==null after N steps, delete head |
| [Q7. Intersection Y LL ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q7-find-the-intersection-point-of-y-ll) | Switch heads: both travel `lenA + lenB` total, meet at intersection |

---

## Pattern 8: Circular / Rotation Manipulation

### 🎯 Core Concept
Temporarily convert the linked list into a circular list by connecting the tail to the head. Then find the new break point and sever the connection.

### 🔍 When to Identify
- **Rotate list by K positions** → Find new tail, break circular link
- **Any "shift elements circularly"** problem

<details>
<summary>📐 How It Works (Click to expand)</summary>

```
Original: 1 → 2 → 3 → 4 → 5, K = 2

Step 1: Calculate length = 5
Step 2: K = K % length = 2 (handle K > length)
Step 3: Make circular: 1 → 2 → 3 → 4 → 5 → (back to 1)
Step 4: New tail is at position (length - K) = 3 (node with value 3)
Step 5: Move temp to new tail (3 steps from head)
Step 6: New head = temp.next (node 4)
Step 7: Break: temp.next = null

Result: 4 → 5 → 1 → 2 → 3
```

</details>

<details>
<summary>🧩 Global Template (Click to expand)</summary>

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

### 📋 Questions Under This Pattern

| Question | Key Insight |
|----------|-------------|
| [Q13. Rotate LL ➡️](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q13-rotate-a-ll) | Always `k = k % length`. New tail at `(length - k)` from head |

---

## 🔄 Pattern Combination Cheat Sheet

| Problem | Pattern 1 | Pattern 2 | Pattern 3 |
|---------|-----------|-----------|-----------|
| Q10. Palindrome | Slow-Fast (find middle) | Reversal (reverse 2nd half) | Compare + Restore |
| Q9. Reverse K Group | Reversal (reverse each group) | Traversal (get Kth node) | Reconnect groups |
| Q12. Flattening | Merge (combine sorted lists) | Recursion (flatten rest first) | Dummy Node |
| Q14. Clone Random | In-Place (interleave copies) | Traversal (3 passes) | — |
| Q13. Rotate | Circular (make & break) | Length calculation | — |

---

## 📝 Quick Pattern Recognition Flowchart

```
Given a Linked List problem, ask yourself:

1. Need to find MIDDLE or detect CYCLE?
   → Use Slow-Fast Pointer (Pattern 1)

2. Need to REVERSE the list or part of it?
   → Use Reversal (Pattern 2)

3. Need to COMBINE two sorted lists?
   → Use Merge / Two-List (Pattern 3)

4. Building a NEW list or head might change?
   → Use Dummy Node (Pattern 4)

5. Need to TRACK visited nodes or map originals to copies?
   → Use Hashing (Pattern 5)

6. Need to DELETE/MODIFY without extra space?
   → Use In-Place Modification (Pattern 6)

7. Need to ALIGN two lists or find meeting point?
   → Use Two-Pointer Intersection (Pattern 7)

8. Need to ROTATE or SHIFT elements circularly?
   → Use Circular / Rotation (Pattern 8)
```

---

## 🎯 Before You Start: Must-Know Basics

<details>
<summary>📋 ListNode Definition & Common Operations (Click to expand)</summary>

```java
// ListNode Definition
public class ListNode {
    int val;
    ListNode next;
    ListNode random;  // Only for Q14
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

// Traverse a linked list
ListNode temp = head;
while (temp != null) { temp = temp.next; }

// Count nodes
int count = 0; ListNode temp = head;
while (temp != null) { count++; temp = temp.next; }

// Get last node
ListNode temp = head;
while (temp.next != null) temp = temp.next;

// Delete a node (given prev)
prev.next = prev.next.next;

// Delete head
head = head.next;
```

</details>

---

➡️ **For detailed question-by-question solutions with all approaches, see [Part 2: Sequence Wise Notes](Striver_SDE_Sheet_LinkedList_Series_part-2.md)**
