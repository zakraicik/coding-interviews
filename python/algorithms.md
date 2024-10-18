# Common Data Structures and Algorithms in Python

Implementing common data structures and algorithms using Python classes enhances understanding and provides reusable code components for various projects.

## Table of Contents

1. [Common Data Structures](#common-data-structures)
   - [Stack](#stack)
   - [Queue](#queue)
   - [Linked List](#linked-list)
   - [Binary Tree](#binary-tree)
   - [Binary Search Tree](#binary-search-tree)
   - [Graph](#graph)
   - [Heap (Priority Queue)](#heap-priority-queue)
2. [Common Algorithms](#common-algorithms)
   - [Sorting Algorithms](#sorting-algorithms)
     - [QuickSort](#quicksort)
     - [MergeSort](#mergesort)
   - [Searching Algorithms](#searching-algorithms)
     - [Binary Search](#binary-search)
   - [Graph Algorithms](#graph-algorithms)
     - [Depth-First Search (DFS)](#depth-first-search-dfs)
     - [Breadth-First Search (BFS)](#breadth-first-search-bfs)

---

## Common Data Structures

### Stack

A **stack** is a linear data structure following the Last-In-First-Out (LIFO) principle.

#### Time Complexity

- **Push (Insertion):** O(1)
- **Pop (Deletion):** O(1)
- **Peek (Access Top Element):** O(1)

#### Space Complexity

- **O(n)**, where _n_ is the number of elements in the stack.

```python
class Stack:
    def __init__(self):
        self.items = []

    def is_empty(self):
        return len(self.items) == 0

    def push(self, item):
        self.items.append(item)
        print(f"Pushed {item}, Stack: {self.items}")

    def pop(self):
        if self.is_empty():
            print("Stack is empty, cannot pop.")
            return None
        item = self.items.pop()
        print(f"Popped {item}, Stack: {self.items}")
        return item

    def peek(self):
        if self.is_empty():
            print("Stack is empty, nothing to peek.")
            return None
        return self.items[-1]
```

Sample Usage

```python
stack = Stack()
stack.push(1)
stack.push(2)
stack.pop()
```

### Queue

A queue is a linear data structure following the First-In-First-Out (FIFO) principle.

#### Time Complexity

- **Enqueue (Insertion)**: O(1)
- **Dequeue (Deletion)**: O(1)
- **Peek (Access Front Element)**: O(1)

#### Space Complexity

- O(n), where n is the number of elements in the queue.

A **queue** is a linear data structure following the First-In-First-Out (FIFO) principle.

```python
class Queue:
    def **init**(self):
        self.items = []

        def is_empty(self):
            return len(self.items) == 0

        def enqueue(self, item):
            self.items.append(item)
            print(f"Enqueued {item}, Queue: {self.items}")

        def dequeue(self):
            if self.is_empty():
                print("Queue is empty, cannot dequeue.")
                return None
            item = self.items.pop(0)
            print(f"Dequeued {item}, Queue: {self.items}")
            return item
```

Sample Usage

```python
queue = Queue()
queue.enqueue(1)
queue.enqueue(2)
queue.dequeue()
```

### Linked-list

A linked list consists of nodes where each node contains data and a reference to the next node.

#### Time Complexity

- **Insertion at Head/Tail:** O(1)
- **Deletion at Head:** O(1)
- **Deletion at Tail:** O(n) (without a tail pointer), O(1) (with a tail pointer and doubly linked list)
- **Search/Access:** O(n)

#### Space Complexity

- O(n), where n is the number of nodes.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = Node(data)
        print(f"Inserting {data} into LinkedList.")
        if not self.head:
            self.head = new_node
            print(f"Head is now {data}.")
            return
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node
        print(f"Inserted {data} after {last.data}.")

    def print_list(self):
        current = self.head
        print("LinkedList:", end=" ")
        while current:
            print(current.data, end=" -> ")
            current = current.next
        print("None")

```

Sample Usage

```python
ll = LinkedList()
ll.insert(1)
ll.insert(2)
ll.insert(3)
ll.print_list()
```

### Binary Tree

A binary tree is a tree data structure where each node has at most two children.

#### Time Complexity

- **Insertion:** O(log n) (balanced), O(n) (unbalanced)
- **Deletion:** O(log n) (balanced), O(n) (unbalanced)
- **Search:** O(log n) (balanced), O(n) (unbalanced)
- **Traversal (In-order, Pre-order, Post-order):** O(n)

#### Space Complexity

- O(n) for storing the nodes.
- O(h) auxiliary space for recursion stack during traversal, where h is the height of the tree.

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

    def insert_left(self, value):
        self.left = TreeNode(value)
        print(f"Inserted {value} to the left of {self.value}")
        return self.left

    def insert_right(self, value):
        self.right = TreeNode(value)
        print(f"Inserted {value} to the right of {self.value}")
        return self.right

```

Sample Usage

```python
root = TreeNode(10)
left_child = root.insert_left(5)
right_child = root.insert_right(15)
```

### Binary Search Tree

A binary search tree (BST) is a binary tree where each node's left child contains a value less than the parent node, and the right child contains a value greater than the parent node.

#### Time Complexity

- **Insertion:** O(log n) average, O(n) worst-case (unbalanced)
- **Deletion:** O(log n) average, O(n) worst-case
- **Search:** O(log n) average, O(n) worst-case

#### Space Complexity

- O(n) for storing the nodes.
- O(h) auxiliary space for recursion stack during operations.

```python
class BSTNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

    def insert(self, value):
        print(f"Attempting to insert {value} into BST.")
        if value < self.value:
            if self.left:
                self.left.insert(value)
            else:
                self.left = BSTNode(value)
                print(f"Inserted {value} to the left of {self.value}")
        elif value > self.value:
            if self.right:
                self.right.insert(value)
            else:
                self.right = BSTNode(value)
                print(f"Inserted {value} to the right of {self.value}")
        else:
            print(f"Value {value} already exists in the BST.")

    def search(self, value):
        print(f"Searching for {value} in BST.")
        if value < self.value:
            if self.left:
                return self.left.search(value)
            else:
                print(f"{value} not found in BST.")
                return False
        elif value > self.value:
            if self.right:
                return self.right.search(value)
            else:
                print(f"{value} not found in BST.")
                return False
        else:
            print(f"Found {value} in BST.")
            return True

```

Sample Usage

```python
bst = BSTNode(10)
bst.insert(5)
bst.insert(15)
bst.insert(7)
bst.search(7)
bst.search(3)

```

### Graph

A graph consists of a set of nodes (vertices) and edges connecting them.

#### Time Complexity

- Adjacency List Representation:

  - **Add Vertex:** O(1)
  - **Add Edge:** O(1)
  - **Remove Vertex:** O(V + E)
  - **Remove Edge:** O(E)

- Adjacency Matrix Representation:
  - **Add Vertex:** O(V²)
  - **Add Edge:** O(1)
  - **Remove Vertex:** O(V²)
  - **Remove Edge:** O(1)

#### Space Complexity

- O(V²)

```python
class Graph:
    def __init__(self):
        self.graph = {}  # Adjacency list

    def add_vertex(self, vertex):
        if vertex not in self.graph:
            self.graph[vertex] = []
            print(f"Added vertex {vertex}.")

    def add_edge(self, vertex1, vertex2):
        self.add_vertex(vertex1)
        self.add_vertex(vertex2)
        self.graph[vertex1].append(vertex2)
        print(f"Added edge from {vertex1} to {vertex2}.")

    def get_vertices(self):
        return list(self.graph.keys())

    def get_edges(self):
        edges = []
        for vertex in self.graph:
            for neighbor in self.graph[vertex]:
                edges.append((vertex, neighbor))
        return edges

```

Sample Usage

```python
g = Graph()
g.add_edge('A', 'B')
g.add_edge('A', 'C')
g.add_edge('B', 'C')
print("Vertices:", g.get_vertices())
print("Edges:", g.get_edges())
```

### Priority Queue

A heap is a specialized tree-based data structure that satisfies the heap property. It's commonly used to implement priority queues.

#### Time Complexity

- **Insertion:** O(log n)
- **Deletion (Extract Max/Min):** O(log n)
- **Peek (Get Max/Min):** O(1)

#### Space Complexity

- O(n), where n is the number of elements.

```python
import heapq

class MinHeap:
    def __init__(self):
        self.heap = []

    def push(self, item):
        heapq.heappush(self.heap, item)
        print(f"Pushed {item} into MinHeap: {self.heap}")

    def pop(self):
        if not self.heap:
            print("MinHeap is empty, cannot pop.")
            return None
        item = heapq.heappop(self.heap)
        print(f"Popped {item} from MinHeap: {self.heap}")
        return item

    def peek(self):
        if not self.heap:
            print("MinHeap is empty, nothing to peek.")
            return None
        return self.heap[0]

```

Sample Usage

```python
min_heap = MinHeap()
min_heap.push(5)
min_heap.push(3)
min_heap.push(8)
min_heap.pop()
```

## Common Data Structures

### QuickSort

QuickSort is a divide-and-conquer algorithm that picks an element as a pivot and partitions the array around the pivot.

#### Time Complexity

- **Best Case:** O(n log n)
- **Average Case:** O(n log n)
- **Worst Case:** O(n²) (occurs when the smallest or largest element is always chosen as the pivot)

#### Space Complexity

- O(log n) average auxiliary space due to recursion.
- O(n) worst-case space complexity (due to recursion stack).

```python
class QuickSort:
    def sort(self, array):
        print(f"Starting QuickSort on: {array}")
        self._quick_sort(array, 0, len(array) - 1)
        print(f"QuickSorted array: {array}")
        return array

    def _quick_sort(self, array, low, high):
        if low < high:
            pi = self._partition(array, low, high)
            self._quick_sort(array, low, pi - 1)
            self._quick_sort(array, pi + 1, high)

    def _partition(self, array, low, high):
        pivot = array[high]
        print(f"Choosing pivot {pivot} between {low} and {high}")
        i = low - 1
        for j in range(low, high):
            if array[j] <= pivot:
                i += 1
                array[i], array[j] = array[j], array[i]
                print(f"Swapped {array[i]} and {array[j]}, array: {array}")
        array[i + 1], array[high] = array[high], array[i + 1]
        print(f"Moved pivot {pivot} to correct position, array: {array}")
        return i + 1

```

Sample Usage

```Python
qs = QuickSort()
array = [10, 7, 8, 9, 1, 5]
qs.sort(array)
```

### MergeSort

MergeSort is a divide-and-conquer algorithm that divides the array into halves, sorts them, and then merges them.

#### Time Complexity

- **All Cases:** O(n log n)

#### Space Complexity

- O(n) auxiliary space for merging.

```python
class MergeSort:
    def sort(self, array):
        print(f"Starting MergeSort on: {array}")
        sorted_array = self._merge_sort(array)
        print(f"MergeSorted array: {sorted_array}")
        return sorted_array

    def _merge_sort(self, array):
        if len(array) <= 1:
            return array
        mid = len(array) // 2
        left_half = self._merge_sort(array[:mid])
        right_half = self._merge_sort(array[mid:])
        return self._merge(left_half, right_half)

    def _merge(self, left, right):
        print(f"Merging {left} and {right}")
        merged_array = []
        left_index = right_index = 0

        while left_index < len(left) and right_index < len(right):
            if left[left_index] <= right[right_index]:
                merged_array.append(left[left_index])
                left_index += 1
            else:
                merged_array.append(right[right_index])
                right_index += 1

        merged_array.extend(left[left_index:])
        merged_array.extend(right[right_index:])
        print(f"Merged array: {merged_array}")
        return merged_array


```

Sample Usage

```python
ms = MergeSort()
array = [12, 11, 13, 5, 6, 7]
ms.sort(array)
```

### Binary-search

Binary Search is an efficient algorithm for finding an item from a sorted list of items.

#### Time Complexity

- O(log n)

#### Space Complexity

- Iterative Version: O(1)
- Recursive Version: O(log n) due to recursion stack.

```python
class BinarySearch:
    def search(self, array, target):
        print(f"Starting BinarySearch for {target} in {array}")
        result = self._binary_search(array, target, 0, len(array) - 1)
        if result != -1:
            print(f"Found {target} at index {result}")
        else:
            print(f"{target} not found in the array.")
        return result

    def _binary_search(self, array, target, low, high):
        if high >= low:
            mid = (high + low) // 2
            if array[mid] == target:
                return mid
            elif array[mid] > target:
                return self._binary_search(array, target, low, mid - 1)
            else:
                return self._binary_search(array, target, mid + 1, high)
        else:
            return -1

```

Sample Usage

```python
bs = BinarySearch()
array = [1, 3, 5, 7, 9]
bs.search(array, 7)
bs.search(array, 2)

```

### Depth-first-search-dfs

DFS explores as far as possible along each branch before backtracking.

#### Time Complexity

- O(V + E)

#### Space Complexity

- O(V) for visited array.
  O(V) auxiliary space in the worst case due to recursion stack (for recursive implementation).

Graph

```python
class GraphDFS(Graph):
    def dfs(self, start_vertex, visited=None):
        if visited is None:
            visited = set()
        print(f"Visiting {start_vertex}")
        visited.add(start_vertex)
        for neighbor in self.graph.get(start_vertex, []):
            if neighbor not in visited:
                self.dfs(neighbor, visited)
        return visited
```

Sample Usage

```python
graph_dfs = GraphDFS()
graph_dfs.add_edge('A', 'B')
graph_dfs.add_edge('A', 'C')
graph_dfs.add_edge('B', 'D')
graph_dfs.add_edge('C', 'D')
visited_nodes = graph_dfs.dfs('A')
print("DFS Visited Nodes:", visited_nodes)
```

Tree

```python
class TreeDFS:
    def dfs_recursive(self, node, traversal=None):
        if traversal is None:
        traversal = []
        print(f"Starting DFS from {node.value}")

                traversal.append(node.value)
                print(f"Visiting {node.value}")

                for child in node.children:
                    self.dfs_recursive(child, traversal)

                return traversal
```

Sample Usage

```python

root = TreeNode(1)
child_a = TreeNode(2)
child_b = TreeNode(3)
child_c = TreeNode(4)
child_d = TreeNode(5)
child_e = TreeNode(6)


root.children.extend([child_a, child_b])
child_a.children.append(child_c)
child_b.children.extend([child_d, child_e])


tree_dfs = TreeDFS()
result_recursive = tree_dfs.dfs_recursive(root)
print("DFS Recursive Traversal:", result_recursive)

result_iterative = tree_dfs.dfs_iterative(root)
print("DFS Iterative Traversal:", result_iterative)

```

### Breadth-first-search-bfs

BFS explores all the neighbor nodes at the present depth before moving on to nodes at the next depth level.

#### Time Complexity

- O(V + E)

#### Space Complexity

- O(V) for visited array and queue data structure.

Graph

```python
from collections import deque

class GraphBFS(Graph):
    def bfs(self, start_vertex):
        visited = set()
        queue = deque([start_vertex])
        visited.add(start_vertex)
        print(f"Starting BFS from {start_vertex}")
        while queue:
            vertex = queue.popleft()
            print(f"Visiting {vertex}")
            for neighbor in self.graph.get(vertex, []):
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
        return visited
```

Sample Usage

```python
graph_bfs = GraphBFS()
graph_bfs.add_edge('A', 'B')
graph_bfs.add_edge('A', 'C')
graph_bfs.add_edge('B', 'D')
graph_bfs.add_edge('C', 'D')
visited_nodes = graph_bfs.bfs('A')
print("BFS Visited Nodes:", visited_nodes)
```

Tree

```python
from collections import deque

class TreeNode:
    def **init**(self, value):
    self.value = value
    self.children = [] # List to store child nodes

class TreeBFS:
    def bfs(self, root):
    if not root:
    return []

            queue = deque([root])
            traversal = []
            print(f"Starting BFS from {root.value}")

            while queue:
                node = queue.popleft()
                traversal.append(node.value)
                print(f"Visiting {node.value}")

                for child in node.children:
                    queue.append(child)

            return traversal

```

Sample Usage

```python

root = TreeNode(1)
child_a = TreeNode(2)
child_b = TreeNode(3)
child_c = TreeNode(4)
child_d = TreeNode(5)
child_e = TreeNode(6)


root.children.extend([child_a, child_b])
child_a.children.append(child_c)
child_b.children.extend([child_d, child_e])


tree_bfs = TreeBFS()
result = tree_bfs.bfs(root)
print("BFS Traversal:", result)
```
