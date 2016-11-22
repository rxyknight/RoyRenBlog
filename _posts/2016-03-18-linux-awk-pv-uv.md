---
layout: post
title: 'Using Linux AWK To Get The PV & UV'
date: '2016-03-24'
header-img: "img/post-bg-unix.jpg"
tags:
     - linux
author: 'Roy Ren'
---

If you want to know how favourite your website is, PV & UV are the key metrics to reflect that. So

## What are the PV and UV?

**PV (Page View)** is triggered when any page is loaded by any visitor to your site. For example, if you click this link and the page loads, you have triggered a page view. If you click the link 20 more times today, it will count as 20 page views.

**UV (Unique Visitor)** is an individual user who has accessed your site. It is determined by the IP address of the computer or device that the user is browsing from, combined with a cookie on the browser they are using. No matter of how many visits a visitor makes, if he is on the same device and same browser, only one unique visitor is counted. For example, if you visit this link once today, you will be counted as a unique visitor. If you come back to this site 20 more times today, you are still counted as one unique visitor. If you visit the site from another computer or device (or another browser) it will count as a new visitor.

By analysing the server logs, we can get PV & UV easily. Many programming languages can do that, but the most straightforward way (I think) is by using Linux AWK.

## Let's start.

We suppose that the server logs are from Apache server,

Log name: **access_2013_05_30.log**


Log content (only part): 	


	114.66.5.211 - - [30/May/2013:22:30:04 +0800] "GET /static/image/common/locked.gif HTTP/1.1" 200 319
	58.63.138.37 - - [30/May/2013:22:30:04 +0800] "GET /static/js/fileprogress.js?y7a HTTP/1.1" 304 -
	115.61.42.205 - - [30/May/2013:22:30:03 +0800] "GET /api/connect/like.php HTTP/1.1" 200 722
	......
	......
	......
	119.255.57.50 - - [30/May/2013:22:31:40 +0800] "GET /static/image/common/logo.png HTTP/1.1" 304 -
	119.255.57.50 - - [30/May/2013:22:31:40 +0800] "GET /uc_server/avatar.php?uid=65407&size=small HTTP/1.1" 301 -
	119.255.57.50 - - [30/May/2013:22:31:40 +0800] "GET /static/image/common/qq_bind_small.gif HTTP/1.1" 304 -
	119.255.57.50 - - [30/May/2013:22:31:40 +0800] "GET /static/image/common/pn_post.png HTTP/1.1" 304 -
	
1. Data cleaning process, filter out some of the interference (.png .gif .jpg .jpeg  .js .css) inside the data 

		awk '($7 !~ /\.png|\.jpg|\.gif|\.jpeg|\.js|\.css/) {print $0}' access_2013_05_30.log > clean_2013_05_30.log

2. Get PV & UV

	PV:
	
		awk 'BEGIN{pv=0}{pv++}END{print "PV:"pv}' clean_2013_05_30.log
		# OR
		awk 'END{print "PV:"NR}' clean_2013_05_30.log
		
		
	UV:
	
		# sort and then remove duplication
		awk '{print $1}' clean_2013_05_30.log | sort -n | uniq -u | wc -l

OK, that's it, we done:)

**PS:** If we want to get the top 10 users with the most visits:

	awk '{print $1}' clean_2013_05_30.log | sort -n | uniq -c |sort -nr -k 1 | head -10