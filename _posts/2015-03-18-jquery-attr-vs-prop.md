---
layout: post
title: 'prop VS attr In Jquery'
date: '2015-03-18'
header-img: "img/post-bg-web.jpg"
tags:
     - web
author: 'Roy Ren'
---

## What is the difference between prop and attr?

There are many answers online, my understanding are:

* **prop**: when you are dealing with the attributes that HTML elements already have, in another words, the HTML elements's **inherent attributes**. In this case, you should use prop method.
* **attr**: when you are dealing with your own custom DOM properties, in other words, the properties **defined by yourself**. In this case, you should use attr method.

Still confusing? Let's have a look at examples below:

	<a href="http://www.google.com" target="_self" class="btn">Google</a>

In this example, the \<a> element have "href, target and class" three attributes, which are the attributes of the \<a> element itself, and the W3C standard contains these attributes, so these are called **inherent attributes**. The prop method is recommended when working with these properties.

	<a href="#" id="link1" action="delete">Delete</a>

In this example, the \<a> element have "href, id and action" three attributes. Obviously, the first two are intrinsic, but the latter "action" attribute is our own custom property. There is no such attribute originally. This is the custom DOM property. The attr method is recommended when handling these attributes.

## Let's go further

Look at the code below:

	<div class="cb-container">
	<input type="radio" class="cb-radio" id="r1" name="rd" value="left"/>
	<input type="radio" class="cb-radio cb-gap2" id="r2" name="rd" value="right"/>
	<button id="btn" type="button" class="btn btn-primary cb-gap">left</button>
	<button id="btn2" type="button" class="btn btn-primary cb-gap">right</button>
	</div>
	<script type="text/javascript">
	    $(document).ready(function () {
	        var radios = $(".cb-radio");
	        $("#btn").click(function () {
	            radios.eq(0).attr("checked", true);
	            radios.eq(1).attr("checked", false);
	        });
	        $("#btn2").click(function () {
	            radios.eq(0).attr("checked", false);
	            radios.eq(1).attr("checked", true);
	        });
	    });
	</script>
	
Result:

<iframe src="{{ site.baseurl }}/example/jquery/radio_operate_0" width="100%" height="80px" frameborder="0" scrolling="no"> </iframe>

It's so werid, because only first clicking left or right button works, and click again, thing seems goes wrong.

## Why this happen?

Let's have a look at the explanation from [Jquery official website](http://api.jquery.com/prop/)

>The difference between attributes and properties can be important in specific situations. Before jQuery 1.6, the .attr() method sometimes took property values into account when retrieving some attributes, which could cause inconsistent behavior. As of jQuery 1.6, the .prop() method provides a way to explicitly retrieve property values, while .attr() retrieves attributes.


>For example, selectedIndex, tagName, nodeName, nodeType, ownerDocument, defaultChecked, and defaultSelected should be retrieved and set with the .prop() method. Prior to jQuery 1.6, these properties were retrievable with the .attr() method, but this was not within the scope of attr. These do not have corresponding attributes and are only properties.


As a result, after Jquery 1.6, for dealing "checked and selected", we must use **prop** method instead of attr. After we changed attr to prop, we can get the result we want.

<iframe src="{{ site.baseurl }}/example/jquery/radio_operate_1" width="100%" height="80px" frameborder="0" scrolling="no"> </iframe>

## Get the selected state and value

	$("#r1").prop();
	
This will return **true or false**.
	
	$(".cb-radio:checked").val();
	
This will return **left, right or undefined**

Example:

<iframe src="{{ site.baseurl }}/example/jquery/radio_operate_2" width="100%" height="80px" frameborder="0" scrolling="no"> </iframe>

Now, I hope you are more clear about prop() and attr().
