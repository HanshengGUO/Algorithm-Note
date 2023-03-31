# 链表基础
## 链表基础知识
链表总共有三个要注意的方法：
1. 虚拟头节点+模拟
2. 递归的写法
3. 快慢指针的使用，来判断和环相关的题目
### 链表的类型
1. 单链表
2. 双链表
3. 循环链表
### 链表的实现
```java
public class ListNode {
    // 结点的值
    int val;

    // 下一个结点
    ListNode next;

    // 节点的构造函数(无参)
    public ListNode() {
    }

    // 节点的构造函数(有一个参数)
    public ListNode(int val) {
        this.val = val;
    }

    // 节点的构造函数(有两个参数)
    public ListNode(int val, ListNode next) {
        this.val = val;
        this.next = next;
    }
}
```
### 题目
#### [203.移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)
本题主要是学习虚拟头节点的使用和链表的遍历方法
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode cur = dummy;
        while (cur.next != null){
            if (cur.next.val == val){
                cur.next = cur.next.next;
            }else
                cur = cur.next;
        }
        return dummy.next;
    }
}
```
#### [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)
* 在链表初始化时就可以引入虚拟头节点，从而简化插入和删除操作
```java
class ListNode {
    int val;
    ListNode next;

    public ListNode(int val) {
        this.val = val;
    }
}

class MyLinkedList {
    int size;
    ListNode head;

    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }

    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode cur = head;
        for (int i = 0; i <= index; i++) {
            cur = cur.next;
        }
        return cur.val;
    }

    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        index = Math.max(0, index);
        size++;
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }

    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        size--;
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}
```

#### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)
1. Iteration解法
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```
2. Recursion解法
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode newNode = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newNode;
    }
}
```
#### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)
简单模拟即可解题，主要是虚拟头节点的使用
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = dummy;
        while (cur.next != null && cur.next.next != null){
            ListNode tmp = cur.next;
            ListNode next_node = cur.next.next.next;
            cur.next = tmp.next;
            cur.next.next = tmp;
            tmp.next = next_node;
            cur = cur.next.next;
        }
        return dummy.next;
    }
}
```
#### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)
经典的双指针应用
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode fast = dummy;
        ListNode slow = dummy;
        while(n>0){
            fast = fast.next;
            n--;
        }
        while (fast.next != null){
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
```
#### [面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)
把链表A的尾部连到链表B的头，链表B的尾部连到链表A的头，从而对齐两个链表的长度。
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode curA = headA;
        ListNode curB = headB;
        while (true){
            if (curA == curB){
                if (curA == null) return null;
                else return curA;
            }
            if (curA == null){
                curA = headB;
                curB = curB.next;
                continue;
            }
            if (curB == null){
                curB = headA;
                curA = curA.next;
                continue;
            }
            curA = curA.next;
            curB = curB.next;
        }
        // return null;
    }
}
```
#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {// 有环
                ListNode index1 = fast;
                ListNode index2 = head;
                // 两个指针，从头结点和相遇结点，各走一步，直到相遇，相遇点即为环入口
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;
    }
}
```