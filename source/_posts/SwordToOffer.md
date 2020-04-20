---
title: 剑指offer刷题记录
date: 2020-04-20 18:59:56
tags: 刷题
---

[点击这里查看LeetCodeCN剑指offer官网题目列表](https://leetcode-cn.com/problemset/lcof/)

**点击上面的链接跳转查看题解等时,建议使用鼠标中键以免难以返回**

- [0.0.1. 面试题03数组中重复的数字](#001-面试题03数组中重复的数字)
- [0.0.2. 面试题04二维数组中的查找](#002-面试题04二维数组中的查找) 
- [0.0.3. 面试题04二维数组中的查找](#003-面试题04二维数组中的查找) 
- [0.0.4. 面试题05替换空格](#004-面试题05替换空格) 
- [0.0.5. 面试题06. 从尾到头打印链表](#005-面试题06-从尾到头打印链表) 
- [0.0.6. 面试题09. 用两个栈实现队列](#006-面试题09-用两个栈实现队列) 
- [0.0.7. 面试题10- I. 斐波那契数列](#007-面试题10--i-斐波那契数列) 
- [0.0.8. 面试题10- II. 青蛙跳台阶问题](#008-面试题10--ii-青蛙跳台阶问题) 
- [0.0.9. 面试题15. 二进制中1的个数](#009-面试题15-二进制中1的个数) 
- [0.0.10. 面试题24. 反转链表](#0010-面试题24-反转链表) 
- [0.0.11. 面试题41. 数据流中的中位数](#0011-面试题41-数据流中的中位数) 
- [0.0.12. 面试题36. 二叉搜索树与双向链表](#0012-面试题36-二叉搜索树与双向链表)

---

### 0.0.1. 面试题03数组中重复的数字

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
        while (nums[i] != i) {
            if (nums[i] == nums[nums[i]])
                return nums[i];
            int temp = nums[i];
            nums[i] = nums[temp];
            nums[temp] = temp;
        }
    }
    return -1;
    }
}
```

### 0.0.2. 面试题04二维数组中的查找  

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return false;
        }
        int n=matrix.length-1;
        int m=matrix[0].length-1;
        if((m+n)==-2){
            return false;
        }
        int x=0;
        int y=n;
        while(matrix[y][x]!=target){
            
            if(matrix[y][x]>target){
                y--;
            }
            else{
                x++;
            }
            if(!(x<=m&&y>=0)){
                return false;
            }
        }
        return true;
    }
}
```

### 0.0.3. 面试题04二维数组中的查找

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix==null||matrix.length==0||matrix[0].length==0){
            return false;
        }
        int n=matrix.length-1;
        int m=matrix[0].length-1;
        if((m+n)==-2){
            return false;
        }
        int x=0;
        int y=n;
        while(matrix[y][x]!=target){
            
            if(matrix[y][x]>target){
                y--;
            }
            else{
                x++;
            }
            if(!(x<=m&&y>=0)){
                return false;
            }
        }
        return true;
    }
}
```

### 0.0.4. 面试题05替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

```java
class Solution {
    public String replaceSpace(String s) {
        if(s==null||s.isEmpty()){
            return s;
        }
        StringBuilder res=new StringBuilder();
        for(int i=0;i<s.length();i++){
            if(s.charAt(i)==' '){
                res.append("%20");
            }
            else{
                res.append(s.charAt(i));
            }
        }
        return res.toString();
    }
}
```


### 0.0.5. 面试题06. 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] reversePrint(ListNode head) {
        
        Stack<Integer> stack=new Stack<>();
        int num=0;
        for(; head !=null;head=head.next) {
            stack.push(head.val);
            num++;
        }
        int res[] =new int[num];
        for(int i=0;i<num;i++){
            res[i]=stack.pop();
        }
        return res;
    }
}
```

### 0.0.6. 面试题09. 用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

```java
class CQueue {
    private Stack<Integer> s1;
    private Stack<Integer> s2;
    public CQueue() {
        s1=new Stack<>();
        s2=new Stack<>();
    }

    public void appendTail(int value) {
        s1.push(value);
    }

    public int deleteHead() {
        if(s2.isEmpty()){
            while(!s1.isEmpty()){
                s2.push(s1.pop());
            }
        }
        if(!s2.isEmpty())
            return s2.pop();
        else{
            return -1;
        }
    }
}
```

### 0.0.7. 面试题10- I. 斐波那契数列

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public int fib(int n) {
        if(n==0||n==1){
            return n;
        }
        int a=0,b=1;
        int temp=0;
        while(n>=2){
            temp=(a+b)%1000000007;
            a=b;
            b=temp;
            n--;
        }
        return temp;

    }
}
```


### 0.0.8. 面试题10- II. 青蛙跳台阶问题

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class Solution {
    public int numWays(int n) {
        if(n==0||n==1){
            return 1;
        }
        int a=1,b=1;
        int temp=0;
        while(n>=2){
            temp=(a+b)%1000000007;
            a=b;
            b=temp;
            n--;
        }
        return temp;

    }
}
```



### 0.0.9. 面试题15. 二进制中1的个数

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res += n & 1;
            n >>>= 1;
        }
        return res;
    }
}
```

### 0.0.10. 面试题24. 反转链表

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        ListNode pre=null,next=null;
        while (head!=null){
            next=head.next;
            head.next=pre;
            pre=head;
            head=next;
        }
        return pre;
    }
}
```

### 0.0.11. 面试题41. 数据流中的中位数

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```java
class MedianFinder {

    /** initialize your data structure here. */
    private PriorityQueue<Integer> max;
    private PriorityQueue<Integer> min;
    private boolean turn;
    public MedianFinder() {
        turn=true;
        min =new PriorityQueue<Integer>();
        max =new PriorityQueue<Integer>(11, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2-o1;
            }
        });
    }

    public void addNum(int num) {
        if(max.isEmpty()){
            max.add(num);
            return ;
        }
        if(min.size()==max.size()){//两个一样多，添加到第一个；或者添加到第二个，并移动一个元素到第一个
            if(num<min.peek()){//比第二个的小
                max.add(num);
            }
            else{
                max.add(min.poll());
                min.add(num);
            }
        }
        else{//第一个比第二个多
            if(num>max.peek()){//比第一个的大，添加到第二个
                min.add(num);
            }
            else{//比第一个的小，添加到第一个，移动一个元素到第二个
                max.add(num);
                min.add(max.poll());
            }
        }
    }

    public double findMedian() {
        if(max.isEmpty()){
            return 0.0;
        }
        if(((max.size()+min.size())%2)==1){
            return max.peek();
        }
        else{
            return (max.peek()+min.peek())/2.0;
        }
    }
}

```

### 0.0.12. 面试题36. 二叉搜索树与双向链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/

```java
一个月前的提交记录不见了
```

## 0.1. #

```java

```