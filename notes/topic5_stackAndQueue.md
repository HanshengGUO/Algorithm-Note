# Stack and Queue
## Stack and Queue in Java
### Stack Class in Java
![stack.png](..%2Fimgs%2Fstack.png)
```java
Stack<E> stack = new Stack<E>();
// push()
// pop()
// peek()
// empty()
// whether an object exists in the stack
// search()
// It returns the position of the element from the top of the stack. Else, it returns -1.

```
### Queue Interface in Java
The Queue interface is present in java.util package 
and extends the Collection interface is used to hold the elements about to be processed in FIFO(First In First Out) order.
#### LinkedList Class
We can use LinkedList class to implement a Queue.
```java
// Java program to demonstrate a Queue 

import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {

    public static void main(String[] args)
    {
        Queue<Integer> q
                = new LinkedList<>();

        // Adds elements {0, 1, 2, 3, 4} to 
        // the queue 
        for (int i = 0; i < 5; i++)
            q.add(i);

        // Display contents of the queue. 
        System.out.println("Elements of queue "
                + q);

        // To remove the head of queue. 
        int removedele = q.remove();
        System.out.println("removed element-"
                + removedele);

        System.out.println(q);

        // To view the head of queue 
        int head = q.peek();
        System.out.println("head of queue-"
                + head);

        // Rest all methods of collection 
        // interface like size and contains 
        // can be used with this 
        // implementation. 
        int size = q.size();
        System.out.println("Size of queue-"
                + size);
    }
}

```
#### Deque Class
The Deque is related to the double-ended queue that supports the addition or removal of elements from either end of the data structure.
```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Example {
public static void main(String[] args) {
	Deque<Integer> deque = new ArrayDeque<>();
	deque.addFirst(1);
	deque.addLast(2);
	int first = deque.removeFirst();
	int last = deque.removeLast();
	System.out.println("First: " + first + ", Last: " + last);
}
}
```

## 题目练习
### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)
