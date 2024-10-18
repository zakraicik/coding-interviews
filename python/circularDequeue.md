# Circular Deque Implementation in Python

A Circular Deque (Double-ended queue) is a data structure that allows insertion and deletion at both ends. It's implemented using a fixed-size array and uses modular arithmetic to simulate circularity.

## Implementation

Here's a Python implementation of a Circular Deque:

```python
class CircularDeque:
    def __init__(self, k: int):
        self.size = k
        self.deque = [None] * k
        self.front = -1
        self.rear = -1

    def insertFront(self, value: int) -> bool:
        if self.isFull():
            return False
        if self.isEmpty():
            self.front = self.rear = 0
        else:
            self.front = (self.front - 1) % self.size
        self.deque[self.front] = value
        return True

    def insertLast(self, value: int) -> bool:
        if self.isFull():
            return False
        if self.isEmpty():
            self.front = self.rear = 0
        else:
            self.rear = (self.rear + 1) % self.size
        self.deque[self.rear] = value
        return True

    def deleteFront(self) -> bool:
        if self.isEmpty():
            return False
        if self.front == self.rear:
            self.front = self.rear = -1
        else:
            self.front = (self.front + 1) % self.size
        return True

    def deleteLast(self) -> bool:
        if self.isEmpty():
            return False
        if self.front == self.rear:
            self.front = self.rear = -1
        else:
            self.rear = (self.rear - 1) % self.size
        return True

    def getFront(self) -> int:
        return -1 if self.isEmpty() else self.deque[self.front]

    def getRear(self) -> int:
        return -1 if self.isEmpty() else self.deque[self.rear]

    def isEmpty(self) -> bool:
        return self.front == -1

    def isFull(self) -> bool:
        return (self.rear + 1) % self.size == self.front
```

## Explanation

1. The `CircularDeque` class is initialized with a size `k`. It creates an array of that size and initializes `front` and `rear` pointers to -1 (indicating an empty deque).

2. `insertFront` and `insertLast` methods add elements to the front and rear of the deque respectively. They use modular arithmetic to wrap around the array.

3. `deleteFront` and `deleteLast` methods remove elements from the front and rear of the deque respectively.

4. `getFront` and `getRear` methods return the front and rear elements without removing them.

5. `isEmpty` and `isFull` methods check if the deque is empty or full.

## Usage Example

Here's an example of how to use the CircularDeque:

```python
# Initialize a deque with size 3
deque = CircularDeque(3)

# Insert elements
print(deque.insertLast(1))   # Returns True
print(deque.insertLast(2))   # Returns True
print(deque.insertFront(3))  # Returns True
print(deque.insertFront(4))  # Returns False (deque is full)

# Get front and rear elements
print(deque.getFront())      # Returns 3
print(deque.getRear())       # Returns 2

# Delete elements
print(deque.deleteLast())    # Returns True
print(deque.insertFront(4))  # Returns True

# Check if empty or full
print(deque.isEmpty())       # Returns False
print(deque.isFull())        # Returns True
```

This implementation provides an efficient way to add and remove elements from both ends of the queue with O(1) time complexity for all operations.
