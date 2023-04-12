---
title: 【Java 力扣刷题】题2
date: 2022-06-24 19:56:00.0
updated: 2022-09-06 21:58:56.973
url: /archives/java力扣刷题-题2
categories: 
- Java力扣刷题笔记
tags: 
---



#### 内容简介：Java 力扣刷题第2题的解题过程

<!--more-->

这道题我上来的思路就是直接将两个数转换成整数，然后对两个整数相加，最后将结果转换成链表再输出出去

~~~java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int num1 = 0;
        int num2 = 0;
        int res = 0;
        Stack<Integer> stack = new Stack<>();
        ListNode ret = null;
        ListNode retH = null;
        while(l1 != null){
            stack.add(l1.val);
            l1 = l1.next;
        }
        while(!stack.isEmpty()){
            num1 = num1 * 10 + (int)stack.pop();
        }
        while(l2 != null){
            stack.add(l2.val);
            l2 = l2.next;
        }
        while(!stack.isEmpty()){
            num2 = num2 * 10 + (int)stack.pop();
        }
        res = num1 + num2;
        while(res != 0){
            stack.add(res % 10);
            res = res / 10;
        }
        Stack resStack = new Stack();
        while(!stack.isEmpty()){
            resStack.add(stack.pop());
        }
        while(!resStack.isEmpty()){
            if(retH == null){
                retH = new ListNode((int)resStack.pop());
                ret = retH;
            } else {
                retH.next = new ListNode((int)resStack.pop());
                retH = retH.next;
            }

        }
        return ret;
    }
}
~~~

但是，有的数字超过 int 的最大值了，然后，我选择不信邪，用 long ，(⊙o⊙)… ，他还是超过 long 的最大值了

~~~java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        long num1 = 0;
        long num2 = 0;
        long res = 0;
        Stack stack = new Stack();
        ListNode ret = null;
        ListNode retH = null;
        while(l1 != null){
            stack.add(l1.val);
            l1 = l1.next;
        }
        while(!stack.isEmpty()){
            num1 = num1 * 10 + (int)stack.pop();
        }
        while(l2 != null){
            stack.add(l2.val);
            l2 = l2.next;
        }
        while(!stack.isEmpty()){
            num2 = num2 * 10 + (int)stack.pop();
        }
        res = num1 + num2;
        if(res == 0){
            return new ListNode(0);
        }
        while(res != 0){
            stack.add((int)(res % 10));
            res = res / 10;
        }
        Stack resStack = new Stack();
        while(!stack.isEmpty()){
            resStack.add(stack.pop());
        }
        while(!resStack.isEmpty()){
            if(retH == null){
                retH = new ListNode((int)resStack.pop());
                ret = retH;
            } else {
                retH.next = new ListNode((int)resStack.pop());
                retH = retH.next;
            }
        }
        return ret;
    }
}
~~~

> 网上查资料，看到的计算方式我感觉很巧，就是将链表中的数字依次取出来，相加，因为刚好这个链表的数字是倒过来的，那么按顺序相加只需要将相加后的结果考虑进位之后存放到返回链表的对应位置就行了

这一段代码我也是试了好多次才成功的，还是基础不太扎实，做起题来有点慢，还要放到 IDEA 里面慢慢调试才知道怎么调整代码

~~~java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack temp = new Stack();	// 将相加的结果存进栈中
        Stack resStack = new Stack();	// 将上面的栈中存储的内容翻过来存的栈
        int t = 0;  // 进位
        int c = 0;  // 计算结果
        ListNode tNode = new ListNode();	// 作为临时指针的一个节点，方便添加节点
        ListNode ret = tNode;	// 要返回的节点，第一个节点是空，它的next才是要返回数据

        while (l1 != null || l2 != null) {
            if (l1 != null && l2 != null) { // 两个链表中对应的节点都有值，需要进行相加操作
                c = l1.val + l2.val + t;    // 计算两个节点的和同时加上进位
                t = 0;  // 进位在计算完成之后置零
                if (c >= 10) {    // 有进位
                    t = 1;      // 进位置1
                    c = c % 10; // 获取个位的数字，作为对应位置的结果存入栈中
                    temp.add(c);
                } else {
                    temp.add(c);
                }
                l1 = l1.next;
                l2 = l2.next;
            } else if (l1 != null) {    // 只有一个节点的情况
                c = l1.val + t;
                t = 0;
                if (c >= 10) {    // 有进位
                    t = 1;      // 进位置1
                    c = c % 10; // 获取个位的数字，作为对应位置的结果存入栈中
                    temp.add(c);
                } else {
                    temp.add(c);
                }
                l1 = l1.next;
            } else {
                c = l2.val + t;
                t = 0;
                if (c >= 10) {    // 有进位
                    t = 1;      // 进位置1
                    c = c % 10; // 获取个位的数字，作为对应位置的结果存入栈中
                    temp.add(c);
                } else {
                    temp.add(c);
                }
                l2 = l2.next;
            }
        }
        // 相加到最后的时候要看一下有没有进位，有进位的话，要将进位存进链表中
        if (t == 1) {
            temp.add(1);
        }
        // 栈里面按顺序存储的是结果的个位，十位，百位。。。，这里要将他翻过来，一个一个弹出来存进链表中
        while (!temp.isEmpty()) {
            resStack.add(temp.pop());
        }
        // 翻过来了，可以按顺序边弹节点边创建链表了
        while (!resStack.isEmpty()) {
            tNode.next = new ListNode((int)(resStack.pop()));
            tNode = tNode.next;
        }
        // 返回结果
        return ret.next;
    }
}
~~~

![002](http://img.shuyepl.com/202207042019591.png)

终于通过一次了，O(∩_∩)O哈哈~ ，虽然写得很烂，但还是很开心！！！

![001](http://img.shuyepl.com/202207042019270.png)

好多的解答错误，o(ﾟДﾟ)っ！

---

刚刚在洗澡的时候突然想到，要是直接将每个节点相加得到的结果直接就存在返回的链表上了，那不就可以节省两个栈了嘛，而且，还少了一次头尾翻转的操作，应该能节省一点时间，所以将代码改成下面这样

~~~java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int t = 0;  // 进位
        int c = 0;  // 计算结果
        ListNode tNode = new ListNode();
        ListNode ret = tNode;

        while (l1 != null || l2 != null) {
            if (l1 != null && l2 != null) { // 两个链表中对应的节点都有值，需要进行相加操作
                c = l1.val + l2.val + t;    // 计算两个节点的和同时加上进位
                t = 0;  // 进位在计算完成之后置零
                if (c >= 10) {    // 有进位
                    t = 1;      // 进位置1
                    c = c % 10; // 获取个位的数字，作为对应位置的结果存入栈中
                    tNode.next = new ListNode(c);
                    tNode = tNode.next;
                } else {
                    tNode.next = new ListNode(c);
                    tNode = tNode.next;
                }
                l1 = l1.next;
                l2 = l2.next;
            } else if (l1 != null) {    // 只有一个节点的情况
                c = l1.val + t;
                t = 0;
                if (c >= 10) {    // 有进位
                    t = 1;      // 进位置1
                    c = c % 10; // 获取个位的数字，作为对应位置的结果存入栈中
                    tNode.next = new ListNode(c);
                    tNode = tNode.next;
                } else {
                    tNode.next = new ListNode(c);
                    tNode = tNode.next;
                }
                l1 = l1.next;
            } else {
                c = l2.val + t;
                t = 0;
                if (c >= 10) {    // 有进位
                    t = 1;      // 进位置1
                    c = c % 10; // 获取个位的数字，作为对应位置的结果存入栈中
                    tNode.next = new ListNode(c);
                    tNode = tNode.next;
                } else {
                    tNode.next = new ListNode(c);
                    tNode = tNode.next;
                }
                l2 = l2.next;
            }
        }
        if (t == 1) {
            tNode.next = new ListNode(1);
        }
        return ret.next;
    }
}
~~~

最终的结果真的要好一些

![003](http://img.shuyepl.com/202207042019876.png)