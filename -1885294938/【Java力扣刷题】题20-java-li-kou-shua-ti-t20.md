---
title: 【Java力扣刷题】题20
date: 2022-09-06 21:56:40.846
updated: 2022-09-06 21:59:44.343
url: /archives/java-li-kou-shua-ti-t20
categories: 
- Java力扣刷题笔记
tags: 
---

这道题使用了栈的结构来求解，当遇到左边的符号时，压入栈中，当遇到右边的括号时，查看栈顶是否是与之对应的左符号，如果不是，返回false，如果是，则将栈顶的符号弹出，最终，将整个字符串遍历完之后，看栈里面是否为空，如果是空的，那就证明每一个括号左边和右边都能按规则对应起来，返回true，否则，返回false。
~~~java
Stack<Character> left = new Stack<>();
for (int i = 0; i < s.length(); i++) {
	char temp = s.charAt(i);
	if (temp == '(' || temp == '[' || temp == '{') {
		left.add(temp);
	} else if (temp == ')' && !left.isEmpty() && left.peek() == '(') {
		left.pop();
	} else if (temp == ']' && !left.isEmpty() && left.peek() == '[') {
		left.pop();
	} else if (temp == '}' && !left.isEmpty() && left.peek() == '{') {
		left.pop();
	} else {
		return false;
	}
}
if (left.isEmpty()) {
	return true;
} else {
	return false;
}
~~~