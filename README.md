# Practice Problems 2

Q1) Suppose an ArrayList is implemented with an array, and the array doubles in size whenever the array fills up. Explain why adding an element to the end of the arraylist is considered a constant time operation in general.
```
	Adding an element to the end of an ArrayList is considered a constant time operation because the array doubles in size whenever it fills up. The resizing step happens infrequently, so most of the time, you can directly add an element without resizing the array. The amortized time complexity of adding an element to the end of the ArrayList becomes O(1) in the average case. However, inserting or removing an element from the middle of the ArrayList takes linear time (O(n)) due to the need for shifting elements to maintain order.
```
Q2) Suppose we have an ArrayList of N integer objects, sorted in increasing order. Consider the following algorithms for removing a particular Integer value from that ArrayList.
    * It is already known that this value is in the ArrayList.
    * Identify the best and worst case running times (using Big O) for each algorithm. Explain each of your answers.

a) Algorithm I: Perform a linear search to find the value, starting from the BEGINNING of the list. When the value is found, remove it by calling the ArrayList's remove(index) method.
```
Best case running time: O(1) - If the target value is the first element in the ArrayList, the linear search will find it immediately and the removal operation will take constant time.
Worst case running time: O(N) - If the target value is the last element in the ArrayList or not present at all, the linear search will traverse the entire list, resulting in a linear time complexity. The removal operation also takes O(N) time because it requires shifting all the elements after the removed element to fill the gap.
```
b) Algorithm II: Perform a linear search to find the value, starting from the END of the list. When the value is found, remove it by calling the ArrayList's remove(index) method.
```
Best case running time: O(1) - If the target value is the last element in the ArrayList, the linear search will find it immediately and the removal operation will take constant time.
Worst case running time: O(N) - If the target value is the first element in the ArrayList or not present at all, the linear search will traverse the entire list, resulting in a linear time complexity. The removal operation also takes O(N) time because it requires shifting all the elements after the removed element to fill the gap.
```
c) Algorithm III: Perform a binary search to find the value. When the value is found, remove it by calling the ArrayList's remove(index) method.
```
Best case running time: O(1) - If the target value is in the middle of the ArrayList, the binary search will find it in one step, and the removal operation will take constant time.
Worst case running time: O(log N) - In the worst case, the binary search will need to divide the search space in half repeatedly until it finds the target value or determines that it is not present. This requires a logarithmic number of steps. The removal operation still takes O(N) time because it requires shifting all the elements after the removed element to fill the gap.
```
Q3) Consider the following method for the SinglyLinkedList class that works on a linked list.
```java
public E mystery() {
	Node<E> p = head;
	Node<E> q = head.next;
	while (q != null) {
		p = p.next;
		q = q.next.next;
	}
	return p.data;
}
```

a) What does this method do if there are an odd number of nodes? Explain by tracing this code with a list with 9 nodes.

```
If there are an odd number of nodes, the provided method will traverse the linked list using two pointers, p and q. Pointer p moves one step at a time, while pointer q moves two steps at a time. This process continues until q reaches the end of the list (i.e., becomes null). At this point, pointer p will be pointing to the middle node. The method then returns the value stored in that middle node.

For example, let's consider a linked list with 9 nodes: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9.
Initially, p points to the head node (1), and q points to the second node (2). In each iteration, p moves one step forward, and q moves two steps forward. After three iterations, p will be pointing to the middle node (5). Thus, the method will return the value of the middle node, which is 5.
```

b) What happens with this code if there are an even number of nodes? Explain with an example. Modify the method to deal with this situation in a reasonable manner.

```
If there are an even number of nodes, the original method provided will not correctly handle finding the middle node. Let's consider an example with a linked list of 8 nodes: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8.

With the original method, the two pointers, p and q, will traverse the list as follows:
Initially, p points to the head node (1) and q points to the second node (2).
In each iteration, p moves one step forward, while q moves two steps forward.
After four iterations, p will be pointing to the node with the value 5, while q will have reached the end of the list (null).
Since the method returns the value stored in the node pointed by p, it will incorrectly return the value 5 as the middle node, which is not accurate for a list with an even number of nodes.
To handle the even number of nodes case, we can modify the method to return the value of the first middle node instead. One approach is to check if q becomes null or reaches the last node during traversal. If either condition is met, we can return the value of p, as it will be pointing to the first middle node.

Modified code:

public E mystery() {
    Node<E> p = head;
    Node<E> q = head.next;
    while (q != null && q.next != null) {
        p = p.next;
        q = q.next.next;
    }
    return p.data;
}

```

c) What is the runtime complexity of this method in big-O notation if the list has n elements?

```
The runtime complexity of this method in big-O notation, if the list has n elements, is O(n/2), which simplifies to O(n). The while loop iterates through the linked list, moving the pointers p and q, until q reaches the end of the list. In the worst case, when the list has n nodes, the loop will iterate approximately n/2 times, resulting in a linear time complexity of O(n).
```

Q4) Consider a variant of the singly linked list that contains a "dummy node" at the beginning of the list with no data (i.e. the data field is always null). 

An empty list consists of a dummy node only. A list with n elements has n+1 nodes, the first node being the dummy node. The dummy node makes it easier to implement add and remove operations since the list can never be truly empty.

a) Rewrite the SinglyLinkedList constructor to create an "empty" list consisting of one dummy node.

```java
public SinglyLinkedList() {
    head = new Node<>(null); // Creating the dummy node with null data
}
```
b) Rewrite the methods add and remove assuming the singly linked list always contains a dummy node.

```java
public void add(int index, E element) {
    Node<E> newNode = new Node<>(element); // Create a new node with the given element
    Node<E> current = getNodeAtIndex(index - 1); // Get the node before the index where the new node should be inserted

    newNode.next = current.next; // Set the next reference of the new node to the next node of the current node
    current.next = newNode; // Set the next reference of the current node to the new node
    size++; // Increment the size of the list
}
```

```java
public E remove(int index) {
    if (index < 0 || index >= size) {
        throw new IndexOutOfBoundsException();
    }

    Node<E> current = getNodeAtIndex(index - 1); // Get the node before the index of the node to be removed
    Node<E> removedNode = current.next; // Get the node to be removed
    current.next = removedNode.next; // Set the next reference of the current node to the next node after the removed node
    removedNode.next = null; // Remove the reference to the next node from the removed node
    size--; // Decrement the size of the list

    return removedNode.data; // Return the data of the removed node
}
```

Q5)  Smithers decides to implement his stack, called SmithersStack< E >. The contents of the stack are stored in a field called data of type ArrayList< E>, where INDEX 0 represents the TOP of the stack. 

```java
public class SmithersStack<E>
{
        private ArrayList<E> data;  //top element at index 0

        //constructors and other methods not shown
}
```


a) Complete the pop method for the SmithersStack class and determine the worst-case runtime complexity of pop assuming that the stack contains n elements. If the stack is empty, return null in this case.

```java
public E pop() {
    if (data.isEmpty()) {
        return null; // If the stack is empty, return null
    }
    return data.remove(0); // Remove and return the top element from the stack
}
```
The worst-case runtime complexity of the pop method is O(n), assuming that the stack contains n elements. This is because removing an element from index 0 of an ArrayList requires shifting all the remaining elements by one position to fill the gap. In the worst case, this shifting operation needs to be performed on all n elements, resulting in a linear time complexity of O(n).
```
b) Complete the push method for the SmithersStack class and determine the worst-case runtime complexity of push assuming that the stack contains n elements.

```java
public void push(E element) {
    data.add(0, element); // Add the element to the beginning of the ArrayList data
}
```
The worst-case runtime complexity of the push method is O(n), assuming that the stack contains n elements. This is because adding an element at index 0 of an ArrayList requires shifting all the existing elements by one position to make room for the new element. In the worst case, this shifting operation needs to be performed on all n elements, resulting in a linear time complexity of O(n).
```

Q6) Write a program to sort a stack in ascending order. You should not make any assumptions about how the stack is implemented. The following are the only functions that should be used to write this program: push, pop, peek, and isEmpty.

peek - an operation that returns the value of the top element of the stack without removing it. 

Algorithm:
Sorting can be performed with one more stack.
Iterate until the original stack is empty and in each iteration, pop an element from the original stack, while the top element in the second stack is bigger than the removed element, pop the second stack and push it to the original stack. 

```java
public static Stack<Integer> sort(Stack<Integer> s)
{
     import java.util.Stack;

public class StackSorter {
    public static Stack<Integer> sort(Stack<Integer> originalStack) {
        Stack<Integer> sortedStack = new Stack<>();

        while (!originalStack.isEmpty()) {
            int temp = originalStack.pop();

            while (!sortedStack.isEmpty() && sortedStack.peek() > temp) {
                originalStack.push(sortedStack.pop());
            }

            sortedStack.push(temp);
        }

        return sortedStack;
    }

    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(5);
        stack.push(2);
        stack.push(8);
        stack.push(1);
        stack.push(3);

        System.out.println("Original Stack: " + stack);

        Stack<Integer> sortedStack = sort(stack);

        System.out.println("Sorted Stack: " + sortedStack);
    }
}

}
```
