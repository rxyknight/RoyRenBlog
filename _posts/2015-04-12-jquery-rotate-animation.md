---
layout: post
title: 'Rotating Animation by Jquery'
date: '2015-03-18'
header-img: "img/post-bg-web.jpg"
tags:
     - web
author: 'Roy Ren'
---

The principle is very simple: from time to time, rotate the picture, so the picture is rotating.

Result:

<iframe src="{{ site.baseurl }}/example/jquery/rotate" width="100%" height="80px" frameborder="0" scrolling="no"> </iframe>


Code:

	/**
	 * Created by Roy Ren on 2015-04-12.
	 * Base on Jquery
	 */
	var ele ;
	
	//UDF
	$.fn.extend({
	    rotate: function () {
	        ele = this ;
	        setInterval('singleRotate()',20);
	    }
	});
	//Initial degree
	var degree = 0;
	
	//One time rotation
	function singleRotate() {
		 //Increased by 50 degrees for one time
	    degree = degree + 50 * Math.PI / 180;
	    ele.css("transform","rotate("+degree+"deg)");
	}
	
Finally, in your own code, just call the rotate method like:

	$(element).rotate();