# Stack

> Last-In-First-Out (LIFO) structure essential for parsing, nesting, and depth-first operations.

## 1. Overview
A Stack is an abstract data type adhering strictly to the Last-In-First-Out (LIFO) principle. Elements are both inserted (pushed) and extracted (popped) exclusively from the top of the stack. In modern Java, `ArrayDeque` is the preferred stack implementation over the legacy synchronized `java.util.Stack`.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| `push(e)` | O(1) / O(1) / O(1) amortized | O(1) |
| `pop()` | O(1) / O(1) / O(1) | O(1) |
| `peek()` | O(1) / O(1) / O(1) | O(1) |
| `isEmpty()` | O(1) / O(1) / O(1) | O(1) |
| Search | O(1) / O(n) / O(n) | O(1) |

Using `ArrayDeque` provides constant time operations backed by an efficient circular array buffer without synchronization overhead.

## 3. When to Use
- Parentheses, bracket validation, or XML/HTML tag matching.
- Expression evaluation and conversion (Infix to Postfix, Reverse Polish Notation).
- Tracking historical state for undo/redo or backtracking operations.
- Simulating recursion or replacing recursive call stacks to avoid stack overflow.
- Monotonic stack patterns for finding the next greater or smaller element.

## 4. When NOT to Use
- Elements must be processed in the order they arrived (use `Queue` FIFO).
- Random access to arbitrary middle elements is required (use `ArrayList`).
- Highest/lowest priority retrieval rather than latest arrival (use `PriorityQueue`).

## 5. Why It Works
Nested substructures naturally pair the most recently opened scope with the earliest closing token. By pushing open tokens and popping upon corresponding close tokens, the top always reflects the current innermost active context.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Recursive String Replace) | Repeatedly replace '()' or '[]' with empty string | O(n^2) | O(n) |
| Optimized (Stack Verification) | Push opening brackets and pop matching on closing | O(n) | O(n) |

The stack processes each character in a single pass O(n), eliminating repeated substring copying.

## 7. Data Structures Used Here
- `ArrayDeque<E>`: Resizable array deque; the recommended standard Java implementation for LIFO stack.
- `Stack<E>`: Legacy class extending `Vector`; synchronized and generally avoided in modern code.

## 8. Core Template (Java)
```java
// Recommended stack usage in Java via ArrayDeque
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);

if (!stack.isEmpty()) {
    int top = stack.peek(); // inspect top element
    int popped = stack.pop(); // remove and retrieve top
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Linked List](../04-linked-list/README.md) | [Queue & Deque](../06-queue-deque/README.md)
