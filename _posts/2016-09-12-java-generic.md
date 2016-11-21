---
layout: post
title: 'Java generics methods'
date: '2016-09-12'
header-img: "img/home-bg.jpg"
tags:
     - java
author: 'Roy Ren'
---

Generics are a facility of generic programming that were added to the Java programming language in 2004 within the official version J2SE 5.0. The following is a very common use of generics:

    List<String> list = new ArrayList<String>();
    
## The Use of Generic Methods
Generics methods can be used not only in collection, but also in definition of classe, interface and method. However, the use of the generic method of the scene is not too much. The following code shows the implementation of the maximum number of two numbers.

### Generics Class (interface)

    package me.royren.test;

    /**
     * generic test
     * Created by Roy Ren on 9/12/16.
     */
    public class MathTest<T extends Comparable> {

    	public static void main(String[] args) {
        	MathTest<Integer> mathTest1 = new MathTest<Integer>();
        	MathTest<Double> mathTest2 = new MathTest<Double>();
        	System.out.println(mathTest1.max(1, 2));
        	System.out.println(mathTest2.max(2.0, 3.0));
    	}

    	private T max(T t1, T t2) {
        	return t1.compareTo(t2) > 0 ? t1 : t2;
    	}
    }
    
### Generics Method

	/**
	 * generic test
	 * Created by Roy Ren on 9/12/16.
	 */
	public class MathTest {
	
	    public static void main(String[] args) {
	        MathTest mathTest = new MathTest();
	        System.out.println(mathTest.max(1, 2));
	        System.out.println(mathTest.max(2.0, 3.0));
	    }
	
	    private <T extends Comparable> T max(T t1, T t2) {
	        return t1.compareTo(t2) > 0 ? t1 : t2;
	    }
	}
	
### Static Generics Method

	/**
	 * generic test
	 * Created by Roy Ren on 9/12/16.
	 */
	public class MathTest {
	
	    public static void main(String[] args) {
	        System.out.println(max(1, 2));
	        System.out.println(max(2.0, 3.0));
	    }
	
	    private static <T extends Comparable> T max(T t1, T t2) {
	        return t1.compareTo(t2) > 0 ? t1 : t2;
	    }
	}


## The Advantages And Disadvantages of Generics Methods

Advantages are obvious, the using generics method is concise and clear and static generics method is much more clear. However, things have two sides, the static generic method also has the corresponding shortcomings, look at some code:

	import java.util.ArrayList;
	import java.util.List;

	/**
	 * test entry
	 * Created by Roy Ren on 9/10/16.
	 */
	public class Test {
	
	    public static void main(String[] args) {
	        List<String> list = new ArrayList<>();
	        list.add("test");
	
	        //普通转换
	        ArrayList<String> result1 = (ArrayList<String>) list;
	
	        //静态泛型转换
	        String result2 = convert(list);
	    }
	
	    private static <T> T convert(Object a) {
	        return (T) a;
	    }
	}
	
We found that **compile succeeds, run fails**. Why this happend? his is because Java generic methods are pseudo-generics, and will be **type-erased** at compile time. The type in generic methods have already been confirmed when the corresponding class is constructed, whereas the type can not be directly speculated in the static generic method. As the lack of a clear type, finally it causes a type conversion exception.

## Exploring The Mechanism

Decompile the code above:

	Compiled from "Test.java"
	public class Test {
	  public Test();
	    Code:
	       0: aload_0
	       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
	       4: return
	
	  public static void main(java.lang.String[]);
	    Code:
	       0: new           #2                  // class java/util/ArrayList
	       3: dup
	       4: invokespecial #3                  // Method java/util/ArrayList."<init>":()V
	       7: astore_1
	       8: aload_1
	       9: ldc           #4                  // String test
	      11: invokeinterface #5,  2            // InterfaceMethod java/util/List.add:(Ljava/lang/Object;)Z
	      16: pop
	      17: aload_1
	      18: checkcast     #2                  // class java/util/ArrayList
	      21: astore_2
	      22: aload_1
	      23: invokestatic  #6                  // Method convert:(Ljava/lang/Object;)Ljava/lang/Object;
	      26: checkcast     #7                  // class java/lang/String
	      29: astore_3
	      30: return
	}

As we can see the "convert" function is:

	Method convert:(Ljava/lang/Object;)Ljava/lang/Object;
	
The type of paremeter is Object，return valus is also Object，and in the next:

	checkcast

With

	List
	
and

	String
	
not the same，the exception throws。

    private static <T extends List> T convert(Object a) {
        return (T) a;
    } 
    
After decompiling the code above:
 
 	Method convert:(Ljava/lang/Object;)Ljava/util/List;
 	
We found that the type of parameter is Object, but return value is List
 	

## In summary

While “generics” in Java is pseudo-generics, generics can make the code more concise, with special attention to type conversions when using generics methods and static generics methods. 	
 	
	 