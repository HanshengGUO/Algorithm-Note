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
1. 数据结构基础
### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)
基础知识。
```java
class MyQueue {
    Stack<Integer> stack1;
    Stack<Integer> stack2;

    public MyQueue() {
        this.stack1 = new Stack<Integer>();
        this.stack2 = new Stack<Integer>();
    }
    
    public void push(int x) {
        while (!stack2.empty()) stack1.push(stack2.pop());
        stack1.push(x);
    }
    
    public int pop() {
        while (!stack1.empty()) stack2.push(stack1.pop());
        return stack2.pop();
    }
    
    public int peek() {
        while (!stack1.empty()) stack2.push(stack1.pop());
        return stack2.peek();
    }
    
    public boolean empty() {
        return stack1.empty() && stack2.empty();
    }
}
```

### [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)
```java
class MyStack {
    Queue<Integer> q1;
    Queue<Integer> q2;

    public MyStack() {
        q1 = new LinkedList<>();
        q2 = new LinkedList<>();
    }
    
    public void push(int x) {
        q1.add(x);
    }
    
    public int pop() {
        int tmp = q1.poll();
        while (!q1.isEmpty()){
            q2.add(tmp);
            tmp = q1.poll();
        }
        while (!q2.isEmpty()) q1.add(q2.poll());
        return tmp;
    }
    
    public int top() {
        int tmp = q1.poll();
        while (!q1.isEmpty()){
            q2.add(tmp);
            tmp = q1.poll();
        }
        q2.add(tmp);
        while (!q2.isEmpty()) q1.add(q2.poll());
        return tmp;
    }
    
    public boolean empty() {
        return q1.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```
2. 括号匹配
### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        int n = s.length();
        for (int i = 0; i<n; i++){
            if (s.charAt(i) == '(')
                stack.push(')');
            else if (s.charAt(i) == '[')
                stack.push(']');
            else if (s.charAt(i) == '{')
                stack.push('}');
            else{
                if (stack.empty() || stack.pop() != s.charAt(i))
                    return false;
            }
        }
        return stack.empty();
    }
}
```
[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)
限制了只删除相邻的2个重复项，就和括号匹配一样了。要注意Stack和String的使用。
* 使用Stack的解法
```java
class Solution {
    public String removeDuplicates(String s) {
        Stack<Character> stack = new Stack<Character>();
        int n = s.length();
        for (int i = 0; i<n; i++){
            if (stack.empty() || stack.peek() != s.charAt(i))
                stack.push(s.charAt(i));
            else{
                stack.pop();
            }
        }
        String res = "";
        while (!stack.empty()){
            res = stack.pop() + res;
        }
        return res;
    }
}
```
* 使用StringBuffer的写法，略快
```java
class Solution {
    public String removeDuplicates(String s) {
        StringBuffer sb = new StringBuffer();
        int n = s.length();
        for (int i = 0; i<n; i++){
            int sbLen = sb.length();
            if (sbLen == 0 || sb.charAt(sbLen-1) != s.charAt(i))
                sb.append(s.charAt(i));
            else{
                sb.deleteCharAt(sbLen-1);
            }
        }
        
        return sb.toString();
    }
}
```

[150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)
```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<Integer>();
        int n = tokens.length;
        for (int i = 0; i<n; i++){
            if (tokens[i].equals("+")){
                stack.push(Integer.valueOf(stack.pop()) + Integer.valueOf(stack.pop()));
            }else if (tokens[i].equals("-")){
                stack.push(Integer.valueOf(stack.pop()) * (-1) + Integer.valueOf(stack.pop()));
            }else if (tokens[i].equals("*")){
                stack.push(Integer.valueOf(stack.pop()) * Integer.valueOf(stack.pop()));
            }else if (tokens[i].equals("/")){
                int tmp1 = stack.pop();
                int tmp2 = stack.pop();
                stack.push(tmp2 / tmp1);
            }else{
                stack.push(Integer.valueOf(tokens[i]));
            }
        }
        return stack.peek();
    }
}
```
3. 单调栈/单调队列
### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/description/)
用双端队列实现单调队列
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        MonotonicQueue queue = new MonotonicQueue();
        int n = nums.length;
        for (int i = 0; i<k; i++){
            queue.push(nums[i]);
        }
        int[] res = new int[n-k+1];
        for (int i = 0; i<n-k; i++){
            res[i] = queue.peek();
            queue.pop(nums[i]);
            queue.push(nums[i+k]);
        }
        res[n-k] = queue.peek();
        return res;
    }
}

class MonotonicQueue{
    Deque<Integer> deque = new LinkedList<Integer>();
    void push(int i){
        while (!deque.isEmpty() && deque.getLast() < i){
            deque.removeLast();
        }
        deque.add(i);
    }
    void pop(int i){
        if (deque.getLast() == i) deque.removeLast();
    }
    int peek(){
        return deque.peek();
    }
}
```
[347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/description/)
使用Collection.sort()排序整个Entry，O(nlogn)
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        int n = nums.length;
        for (int i = 0; i<n; i++){
            if (map.containsKey(nums[i])){
                map.put(nums[i], map.get(nums[i]) + 1);
            }else{
                map.put(nums[i], 1);
            }
        }
        List<Map.Entry<Integer, Integer>> list = new LinkedList<Map.Entry<Integer, Integer>>(map.entrySet());
        Collections.sort(list, new Comparator<Map.Entry<Integer, Integer>>(){
            public int compare(Map.Entry<Integer, Integer> entry1, Map.Entry<Integer, Integer> entry2){
                return entry2.getValue() - entry1.getValue();
            }
        });
        int[] res = new int[k];
        int p = 0;
        for (Map.Entry<Integer, Integer> entry: list){
            res[p] = entry.getKey();
            p++;
            if (p >= k) break;
        }
        return res;
    }
}
```
按照题意使用heap解决，O(klogn)
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        int n = nums.length;
        for (int i = 0; i<n; i++){
            if (map.containsKey(nums[i])){
                map.put(nums[i], map.get(nums[i]) + 1);
            }else{
                map.put(nums[i], 1);
            }
        }
        PriorityQueue<int[]> heap = new PriorityQueue<>((pair1, pair2)->pair2[1]-pair1[1]);
        for (Map.Entry<Integer, Integer> entry : map.entrySet()){
            heap.add(new int[]{entry.getKey(),entry.getValue()});
        }
        int[] res = new int[k];
        for (int i = 0; i<k; i++){
            res[i] = heap.poll()[0];
        }
        return res;
    }
}
```
## 总结
1. Java Collection的使用
2. 数据结构基础
3. 匹配问题（常用栈）
4. 单调栈单调队列解决滑动窗口里的部分问题
5. Heap数据结构