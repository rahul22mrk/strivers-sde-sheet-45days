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

### 📐 Mathematical Intuition
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

### 🧩 Global Template

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

### 📋 Questions Under This Pattern

#### [Q2. Find Middle of Linked List ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q2-find-middle-of-linked-list)
- **Approach**: Slow moves 1, fast moves 2. When fast reaches end, slow is at middle.
- **Key Insight**: For even length, slow lands on **second middle** (standard). Use `fast.next != null && fast.next.next != null` for **first middle**.

#### [Q8. Detect a Loop in LL ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q8-detect-a-loop-in-ll)
- **Approach**: If slow and fast ever meet → cycle exists. If fast reaches null → no cycle.
- **Key Insight**: In a cycle, fast "laps" slow. The gap decreases by 1 each step, so they MUST meet.

#### [Q10. Check if LL is Palindrome ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q10-check-if-ll-is-palindrome-or-not)
- **Approach**: Use slow-fast to find middle → reverse second half → compare both halves → restore.
- **Key Insight**: Combines **Slow-Fast + Reversal** patterns. Always restore the list after!

#### [Q11. Find the Starting Point of Loop ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q11-find-the-starting-point-in-ll)
- **Approach**: Detect cycle (slow-fast meet) → reset slow to head → move both by 1 → meet at start.
- **Key Insight**: Mathematical proof: `L = m*C - K`. From meeting point and head, same distance to loop start.

---

## Pattern 2: Linked List Reversal

### 🎯 Core Concept
Reverse the direction of `next` pointers so the list flows in the opposite direction. This is the **most fundamental building block** in linked list problems.

### 🔍 When to Identify
- **Reverse entire list** → Direct application
- **Reverse in groups** → Reverse segments + reconnect
- **Process from end** → Reverse first, then process, then reverse back
- **Compare halves** → Reverse one half, compare, then restore

### 📐 How It Works (3-Pointer Approach)
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

### 🧩 Global Template

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
    // Base case
    if (head == null || head.next == null) return head;
    
    // Recursive call reverses the rest
    ListNode newHead = reverseLinkedList(head.next);
    
    // Reverse the link
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
        
        if (kThNode == null) {          // Incomplete group
            if (prevLast != null) prevLast.next = temp;
            break;
        }
        
        ListNode nextNode = kThNode.next;
        kThNode.next = null;             // Disconnect for reversal
        reverseLinkedList(temp);          // Reverse the group
        
        if (temp == head) {
            head = kThNode;              // First group: update head
        } else {
            prevLast.next = kThNode;     // Connect previous group
        }
        
        prevLast = temp;                  // temp is now the LAST node of reversed group
        temp = nextNode;                  // Move to next group
    }
    
    return head;
}

// Helper: Get Kth node from current position
private ListNode getKthNode(ListNode temp, int k) {
    k -= 1;
    while (temp != null && k > 0) {
        k--;
        temp = temp.next;
    }
    return temp;
}
```

### 📋 Questions Under This Pattern

#### [Q1. Reverse a Linked List ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q1-reverse-a-linked-list)
- **Iterative**: 3-pointer (prev, curr, next). O(N) time, O(1) space.
- **Recursive**: Reverse rest, then fix current node. O(N) time, O(N) space (call stack).

#### [Q9. Reverse LL in Group of Given Size K ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q9-reverse-ll-in-group-of-given-size-k)
- **Approach**: Get Kth node → disconnect → reverse group → reconnect → move to next group.
- **Key Insight**: After reversal, `temp` (original group start) becomes the LAST node. `kThNode` becomes the FIRST node (new head of group).

#### [Q10. Check if LL is Palindrome ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q10-check-if-ll-is-palindrome-or-not)
- **Approach**: Find middle (slow-fast) → reverse second half → compare → restore.
- **Key Insight**: Reversal is used as a **sub-operation** here. Always restore the list after comparison!

---

## Pattern 3: Merge / Two-List Traversal

### 🎯 Core Concept
Traverse two sorted linked lists simultaneously, picking the smaller element at each step to build a merged sorted list.

### 🔍 When to Identify
- **Merge two sorted lists** → Direct application
- **Flatten a list with child pointers** → Merge pairs of vertical lists recursively
- **Any "combine two sorted sequences"** problem

### 📐 How It Works
```
List 1:  1 → 3 → 5
List 2:  2 → 4 → 6

Step: Compare heads, pick smaller, advance that list
Result: 1 → 2 → 3 → 4 → 5 → 6

Key: Don't create new nodes! Just relink existing nodes.
```

### 🧩 Global Template

```java
// ===== TEMPLATE: Merge Two Sorted Lists =====
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(-1);
    ListNode temp = dummy;
    
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
    if (list1 != null) temp.next = list1;
    else temp.next = list2;
    
    return dummy.next;
}

// ===== TEMPLATE: Flatten Linked List (Recursive Merge) =====
public ListNode flattenLinkedList(ListNode head) {
    if (head == null || head.next == null) return head;
    
    // Recursively flatten the rest
    ListNode mergedHead = flattenLinkedList(head.next);
    
    // Merge current vertical list with flattened rest
    head = merge(head, mergedHead);
    return head;
}

// Merge using child pointers instead of next
private ListNode merge(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(-1);
    ListNode res = dummy;
    
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
    
    if (dummy.child != null) dummy.child.next = null;
    return dummy.child;
}
```

### 📋 Questions Under This Pattern

#### [Q3. Merge Two Sorted Lists ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q3-merge-two-sorted-lists)
- **Optimal**: Two-pointer traversal with dummy node. O(N1+N2) time, O(1) space.
- **Key Insight**: Relink existing nodes, don't create new ones. Attach remaining list in one step.

#### [Q12. Flattening of LL ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q12-flattening-of-ll)
- **Optimal**: Recursively flatten rest → merge current with flattened result.
- **Key Insight**: Uses `child` pointers instead of `next`. Always set `next = null` after merge to prevent cycles.

---

## Pattern 4: Dummy Node Technique

### 🎯 Core Concept
Create a placeholder "dummy" node before the actual head. This eliminates special-case handling for the head node (insertion at beginning, empty list, etc.).

### 🔍 When to Identify
- **Building a new list** from scratch
- **Merging lists** where head is determined later
- **Any operation** where the head might change
- **Carry propagation** (add two numbers)

### 📐 How It Works
```
Without dummy: Need to handle head separately
  if (head == null) head = newNode;  // special case
  else { ... find last, attach }

With dummy: Uniform handling
  dummy → [build here] → ...
  return dummy.next;  // skip dummy, get real head
```

### 🧩 Global Template

```java
// ===== TEMPLATE: Dummy Node Pattern =====
ListNode dummy = new ListNode(-1);  // Placeholder
ListNode temp = dummy;               // Builder pointer

// Build the list uniformly (no head special case)
while (/* condition */) {
    temp.next = someNode;  // Always attach to temp.next
    temp = temp.next;      // Move builder forward
}

return dummy.next;  // Real head is after dummy

// ===== TEMPLATE: Array to Linked List (using dummy) =====
ListNode dummy = new ListNode(-1);
ListNode temp = dummy;
for (int val : arr) {
    temp.next = new ListNode(val);
    temp = temp.next;
}
return dummy.next;

// ===== TEMPLATE: Array to Linked List (without dummy) =====
ListNode head = null;
ListNode temp = null;
for (int val : arr) {
    ListNode newNode = new ListNode(val);
    if (head == null) {
        head = newNode;     // Special case for head
        temp = newNode;
    } else {
        temp.next = newNode;
        temp = temp.next;
    }
}
return head;
```

### 📋 Questions Under This Pattern

#### [Q3. Merge Two Sorted Lists ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q3-merge-two-sorted-lists)
- Dummy node handles the case where either list could provide the first element.

#### [Q5. Add Two Numbers as LinkedList ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q5-add-two-numbers-as-linkedlist)
- Dummy node handles the case where the result list is built digit by digit with carry.

#### [Q12. Flattening of LL ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q12-flattening-of-ll)
- Dummy node used in the merge helper function for child-pointer lists.

---

## Pattern 5: Hashing / Node Tracking

### 🎯 Core Concept
Use a HashSet or HashMap to store node references as you traverse. This allows O(1) lookup to detect if a node has been visited before.

### 🔍 When to Identify
- **Detect cycle/loop** → If you see a node again, there's a cycle
- **Find intersection** → Store one list's nodes, check against the other
- **Clone with random pointers** → Map original nodes to their copies
- **Find cycle start** → First repeated node is the cycle start

### 📐 When to Use vs. Slow-Fast
| Scenario | Hashing | Slow-Fast |
|----------|---------|-----------|
| Need O(1) space | ❌ O(N) space | ✅ O(1) space |
| Need the actual node | ✅ Direct | Needs extra step |
| Simpler to code | ✅ Intuitive | Needs math proof |
| Interview preference | Brute/Better | Optimal ✅ |

### 🧩 Global Template

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
// Pass 1: Create copies
ListNode temp = head;
while (temp != null) {
    map.put(temp, new ListNode(temp.val));
    temp = temp.next;
}
// Pass 2: Set pointers
temp = head;
while (temp != null) {
    ListNode copy = map.get(temp);
    copy.next = map.get(temp.next);
    copy.random = map.get(temp.random);
    temp = temp.next;
}
return map.get(head);
```

### 📋 Questions Under This Pattern

#### [Q7. Intersection Point of Y LL (Brute) ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q7-find-the-intersection-point-of-y-ll)
- Store all nodes of list A in HashSet → traverse list B → first match is intersection.

#### [Q8. Detect a Loop in LL (Brute) ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q8-detect-a-loop-in-ll)
- Add each node to HashSet → if already exists, cycle detected.

#### [Q11. Find Starting Point of Loop (Brute) ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q11-find-the-starting-point-in-ll)
- Add each node to HashMap → first repeated node is the cycle start.

#### [Q14. Clone LL with Random Pointer (Brute) ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q14-clone-a-ll-with-random-and-next-pointer)
- Two-pass: Pass 1 creates copies in HashMap, Pass 2 sets next/random pointers using map.

---

## Pattern 6: In-Place Modification / O(1) Deletion

### 🎯 Core Concept
Modify the linked list structure **in-place** without using extra data structures. This often involves clever pointer manipulation or value copying.

### 🔍 When to Identify
- **Delete a node without head access** → Copy next node's value, skip next
- **Clone list without HashMap** → Interleave copies between originals
- **Any "O(1) space" constraint** on modification problems

### 📐 Key Techniques

**Technique 1: Value Copy Deletion**
```
Given: ... → [A] → [B] → [C] → ...
Delete B (but only have pointer to B):

Step 1: Copy C's value to B: ... → [A] → [C'] → [C] → ...
Step 2: Skip C: ... → [A] → [C'] → ...
Result: B is effectively deleted (its value became C, and C was skipped)
```

**Technique 2: Interleaving Copies (for cloning)**
```
Original: [1] → [2] → [3] → null
After insert: [1] → [1'] → [2] → [2'] → [3] → [3'] → null
Set random: copy.random = original.random.next
Separate: [1] → [2] → [3] → null  and  [1'] → [2'] → [3'] → null
```

### 🧩 Global Template

```java
// ===== TEMPLATE: O(1) Deletion (given node pointer only) =====
public void deleteNode(ListNode node) {
    node.val = node.next.val;    // Copy next node's value
    node.next = node.next.next;  // Skip the next node
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
        if (temp.random != null) {
            copyNode.random = temp.random.next;  // original.random.next = its copy
        } else {
            copyNode.random = null;
        }
        temp = temp.next.next;
    }
}

// Step 3: Separate copies from originals
ListNode getDeepCopyList(ListNode head) {
    ListNode temp = head;
    ListNode dummy = new ListNode(-1);
    ListNode res = dummy;
    while (temp != null) {
        res.next = temp.next;       // Attach copy
        res = res.next;
        temp.next = temp.next.next;  // Restore original
        temp = temp.next;
    }
    return dummy.next;
}
```

### 📋 Questions Under This Pattern

#### [Q6. Delete Node in a Linked List O(1) ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q6-delete-node-in-a-linked-list-o1)
- **Approach**: Copy next node's value → skip next node. O(1) time and space.
- **Key Insight**: You can't delete the node itself, but you can make it identical to the next node and delete that instead.

#### [Q14. Clone a LL with Random and Next Pointer (Optimal) ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q14-clone-a-ll-with-random-and-next-pointer)
- **Approach**: 3-step in-place: Insert copies → connect randoms → separate lists.
- **Key Insight**: `copy.random = original.random.next` works because copies are right next to originals.

---

## Pattern 7: Two-Pointer Intersection / Alignment

### 🎯 Core Concept
When two linked lists of different lengths need to meet at a common point, first **align their starting positions** so both pointers travel the same remaining distance.

### 🔍 When to Identify
- **Find intersection of Y-shaped lists** → Align lengths, then move together
- **Remove Nth node from end** → Create a gap of N between two pointers
- **Any "two lists meet at same node"** problem

### 📐 Key Intuition

**For Intersection:**
```
List A: [a1] → [a2] → [a3] ↘
                            [c1] → [c2] → null
List B:       [b1] → [b2] ↗

Length A = 5, Length B = 4, Diff = 1
Move longer list's pointer 1 step ahead, then move both together.

OR (Optimal): Switch heads when reaching end!
Pointer 1: A → B (travels 5 + 4 = 9 steps)
Pointer 2: B → A (travels 4 + 5 = 9 steps)
They meet at intersection after same total distance!
```

**For Remove Nth from End:**
```
List: [1] → [2] → [3] → [4] → [5], N = 2

Move fast N steps ahead: fast at [3]
Move both until fast.next == null: slow at [3]
slow.next = slow.next.next → deletes [4]
```

### 🧩 Global Template

```java
// ===== TEMPLATE: Find Intersection (Switch Heads - Optimal) =====
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;
    
    ListNode d1 = headA, d2 = headB;
    while (d1 != d2) {
        d1 = d1.next;
        d2 = d2.next;
        if (d1 == d2) return d1;       // Intersection found (or both null)
        if (d1 == null) d1 = headB;    // Switch to other list
        if (d2 == null) d2 = headA;    // Switch to other list
    }
    return d1;
}

// ===== TEMPLATE: Find Intersection (Length Alignment - Better) =====
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int lenA = getLength(headA);
    int lenB = getLength(headB);
    
    if (lenA < lenB) return collisionPoint(headA, headB, lenB - lenA);
    return collisionPoint(headB, headA, lenA - lenB);
}

private ListNode collisionPoint(ListNode smaller, ListNode longer, int diff) {
    ListNode t1 = smaller, t2 = longer;
    for (int i = 0; i < diff; i++) t2 = t2.next;  // Align
    while (t1 != t2) {
        t1 = t1.next;
        t2 = t2.next;
    }
    return t1;
}

// ===== TEMPLATE: Remove Nth Node from End =====
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode fast = head, slow = head;
    
    // Move fast N steps ahead
    for (int i = 0; i < n; i++) fast = fast.next;
    
    // If fast is null, delete head
    if (fast == null) return head.next;
    
    // Move both until fast reaches last node
    while (fast.next != null) {
        fast = fast.next;
        slow = slow.next;
    }
    
    // Delete the Nth node from end
    slow.next = slow.next.next;
    return head;
}
```

### 📋 Questions Under This Pattern

#### [Q4. Remove Nth Node from Back of LL ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q4-remove-nth-node-from-the-back-of-the-ll)
- **Approach**: Move fast N steps ahead → move both until fast.next is null → slow.next is the node to delete.
- **Key Insight**: The gap of N between fast and slow means when fast is at the end, slow is just before the target node.

#### [Q7. Intersection Point of Y LL ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q7-find-the-intersection-point-of-y-ll)
- **Optimal**: Switch heads when reaching end. Both pointers travel same total distance.
- **Better**: Calculate lengths, align by moving longer list's pointer ahead, then move together.
- **Key Insight**: After switching heads, both pointers travel `lenA + lenB` steps total, meeting at intersection.

---

## Pattern 8: Circular / Rotation Manipulation

### 🎯 Core Concept
Temporarily convert the linked list into a circular list by connecting the tail to the head. Then find the new break point and sever the connection to create the rotated result.

### 🔍 When to Identify
- **Rotate list by K positions** → Find new tail, break circular link
- **Any "shift elements circularly"** problem
- **Reordering that wraps around** the end

### 📐 How It Works
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

### 🧩 Global Template

```java
// ===== TEMPLATE: Rotate Linked List =====
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;
    
    // Step 1: Calculate length and find tail
    ListNode temp = head;
    int length = 1;
    while (temp.next != null) {
        length++;
        temp = temp.next;
    }
    // temp is now at tail
    
    // Step 2: Make circular
    temp.next = head;
    
    // Step 3: Handle k > length
    k = k % length;
    
    // Step 4: Find new tail (length - k steps from head)
    int end = length - k;
    while (end-- > 0) temp = temp.next;
    
    // Step 5: New head and break circular link
    head = temp.next;
    temp.next = null;
    
    return head;
}
```

### 📋 Questions Under This Pattern

#### [Q13. Rotate a LL ➡️ Part 2](Striver_SDE_Sheet_LinkedList_Series_part-2.md#q13-rotate-a-ll)
- **Optimal**: Make circular → find new tail → break. O(N) time, O(1) space.
- **Brute**: Rotate one step at a time, K times. O(N×K) time.
- **Key Insight**: Always do `k = k % length` to handle K > length. The new tail is at position `(length - k)` from head.

---

## 🔄 Pattern Combination Cheat Sheet

Many problems combine multiple patterns. Here's how they compose:

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

### ListNode Definition
```java
public class ListNode {
    int val;
    ListNode next;
    ListNode random;  // Only for Q14
    
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```

### Common Operations Cheat Sheet
```java
// Traverse a linked list
ListNode temp = head;
while (temp != null) {
    // process temp.val
    temp = temp.next;
}

// Count nodes
int count = 0;
ListNode temp = head;
while (temp != null) { count++; temp = temp.next; }

// Get last node
ListNode temp = head;
while (temp.next != null) temp = temp.next;

// Insert at end
ListNode newNode = new ListNode(val);
if (head == null) { head = newNode; }
else {
    ListNode temp = head;
    while (temp.next != null) temp = temp.next;
    temp.next = newNode;
}

// Delete a node (given prev)
prev.next = prev.next.next;

// Delete head
head = head.next;
```

---

➡️ **For detailed question-by-question solutions with all approaches, see [Part 2: Sequence Wise Notes](Striver_SDE_Sheet_LinkedList_Series_part-2.md)**
