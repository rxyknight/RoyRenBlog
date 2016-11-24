---
layout: post
title: 'a+=b is equivalent to a=a+b ?'
date: '2015-07-10'
header-img: "img/post-bg-java.jpg"
tags:
     - java
author: 'Roy Ren'
---

Are **a += b** and **a = a + b** exactly equivalent(java)？Many peple think so，however it's not. Let's have a look at the following:

	public class Test {
	    public static void main(String[] args) {
	        int a = 0;
	        float c = 2.0f;
	        a += c;
	        a = a +  c;  //①
	    }
	}

Are there any issues above？ Can be compiled？ The answer is **NOT**。

	$ javac Test.java
	Test.java:6: error: possible loss of precision
	         a = a +  c;
	               ^
	  required: int
	  found:    float
	1 error

It cannot pass compiling, **a += c** however doesn't show any compiling error.

**Why?**

After **①** is removed and pass compiling，we can use jd-gui to decompile the code for **a += c**：

	public class Test
	{
	  public static void main(String[] paramArrayOfString)
	  {
	    int i = 0;
	    float f = 2.0F;
	    i = (int)(i + f);
	  }
	}

Please pay attention to the following code:

	 i = (int)(i + f);
	 
It shows that **type cast** applies to **a += c** during compiling.

In conclusion: 
For a += c
(A + = c) is equivalent to (a = a + c) if the type of a is compatible with b,
Otherwise, type cast applies after adding.

