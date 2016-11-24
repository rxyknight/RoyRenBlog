---
layout: post
title: 'Java HashMap Optimization'
date: '2015-04-29'
header-img: "img/post-bg-java.jpg"
tags:
     - java
author: 'Roy Ren'
---


HashMap is a frequently-used collection framework in Java. There are a lot of implementation principles online. When the number of data is known, we can specify the capacity when constructing the hashmap, and reduce the time caused by malloc. Here's an example:

	import java.util.HashMap;
	import java.util.Map;
	
	/**
	 * HashMap Test
	 * 
	 * @author Roy Ren
	 *
	 */
	public class MapTest {
	    public static void main(String[] args) {
	        int num = 10000;  //data capacity
	        Map<Integer, String> map1 = new HashMap<Integer, String>();
	        Map<Integer, String> map2 = new HashMap<Integer, String>(num);
	        Map<Integer, String> map3 = new HashMap<Integer, String>(num * 2);
	
	        long time1 = System.currentTimeMillis();
	        for (int i = 0; i < num; i++)
	            map1.put(i, "haha");
	        long time2 = System.currentTimeMillis();
	        for (int i = 0; i < num; i++)
	            map2.put(i, "haha");
	        long time3 = System.currentTimeMillis();
	        for (int i = 0; i < num; i++)
	            map3.put(i, "haha");
	        long time4 = System.currentTimeMillis();
	        System.out.println("map1 time: " + (time2 - time1) + "ms");
	        System.out.println("map2 time: " + (time3 - time2) + "ms");
	        System.out.println("map3 time: " + (time4 - time3) + "ms");
	    }
	}
	
We increase the data capacity by modifying "num", 
	
When the data capacity is 10000, the result shows:

	map1 time: 14ms
	map2 time: 9ms
	map3 time: 4ms
	
When the data capacity is 100000, the result shows:

	map1 time: 31ms
	map2 time: 16ms
	map3 time: 9ms
	
When the data capacity is 1000000, the result shows:

	map1 time: 119ms
	map2 time: 47ms
	map3 time: 59ms
	
When the data capacity is 10000000, the result shows:

	map1 time: 7718ms
	map2 time: 1035ms
	map3 time: 2156ms
	
In Conclusion, when the number of data is known, we can specify the capacity when constructing the hashmap, and reduce the time caused by malloc.