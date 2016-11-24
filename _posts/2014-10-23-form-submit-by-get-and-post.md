---
layout: post
title: 'Submit Form to URL With Parameter by Get And Post'
date: '2014-10-23'
header-img: "img/post-bg-web.jpg"
tags:
     - web
author: 'Roy Ren'
---

Recently I was doing some work on file uploading, and encountered the following questions:

The form will be submitted to a url with parameter by post method (such as: res.php?Param = aaa), so what would be the result?

Here are testing examples though get and post method:

## get (the same parameter)

	<html>
	<body>
	<?php
	echo $_GET ['param'];
	?>
	    <form action="get_post_test.php?param=aaa" method="get">
	        <input type="text" name="param" value="bbb" /> 
	        <input type="submit" value="submit">
	    </form>
	</body>
	</html>
	
The result shows param is bbb which from the input value, so the input value overwrites the url parameter.

## get (not the same parameter)

	<html>
	<body>
	<?php
	echo $_GET ['param1'];
	echo "<br>";
	echo $_GET ['param2'];
	?>
	    <form action="get_post_test.php?param1=aaa" method="get">
	        <input type="text" name="param2" value="bbb" /> 
	        <input type="submit" value="submit">
	    </form>
	</body>
	</html>
	
Got the value of param2, but did not get the value of param1, so by get method, the original parameter from url is wiped out by form data.

## post (the same parameter)

	<html>
	<body>
	<?php
	echo "get=" . $_GET ['param'];
	echo "<br>";
	echo "post=" . $_POST ['param'];
	?>
	    <form action="get_post_test.php?param=aaa" method="post">
	        <input type="text" name="param" value="bbb" /> <input type="submit"
	            value="submit">
	    </form>
	</body>
	</html>

The output is that it's aaa by get method and bbb by post method, which means in this case it does not affect each other.

