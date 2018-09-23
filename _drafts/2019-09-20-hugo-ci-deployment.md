---
layout: single
title:  "Hugo continuous deployment"
date:   2018-09-20 16:16:01 +0100
categories: hugo blog
tags: hugo travis CI blog golang go
---
This is an assortment of things I collected from several other manuals online. Each with their own approach and own shortcomings :) which might or might not happen based on your specific user case.

The basic manual about github pages hosting can be found [on hugo website](https://gohugo.io/hosting-and-deployment/hosting-on-github/), but it requires you building it and deploying it personally every time. Which is fine, just not always managable. Also, it is not great if you publish to the blog through pull requests, so some level of automation is needed.

A summary of travis use case with hugo is also [here](http://rcoedo.com/post/hugo-static-site-generator/), but I didn't want to use special ssh access for travis when github has the amazing personal repo tokens. I found another [tutorial](http://speps.github.io/articles/hugo-setup/), but this one for some reason wanted me to build my own hugo libraries. I understand the benefit of that (as sometimes we don't update hugo as often as we should), but eventually I decided to make the better of the both worlds. So I used the second deploy script and used modifid travis file to build the hugo libraries on the fly.

Overall, it was similarly easy to deploy as jekyll deployment.

What tripped me a bit was that I wanted to use a custom CNAME, but each deployment was overwriting the file. To be entirely honest, I never go to the shell scripting, so I have little idea what that deployment script does, but I am somewhat skilled in linux and good at copy paste, so I simply added `echo "brain.vr" > public/CNAME` into the after script with travis, which solves the problem quite efficiently.

## Procedure



But you need to run  hugo server --appendPort=false, otehrwise it will append port unumber as a first path so localhost:1313/topic/thing will be split into 1313 topic and thing

Hosting hugo on github has another plethora of it's own problems
First is defining the baseurl properly. Despite what is said on the hugo website, baseurl needs to have the full https absolute path - https://<username>.github.io/<project-name> 
If you skip the https:// part, some relative urls can become broken

IF you are publishing to github pages, then you need to start the develop server with --baseURL option hugo server baseURL="http://localhost/project-name", otherwise it might work 



But you need to run  hugo server --baseURL="/" --appendPort=false, otehrwise it will append port unumber as a first path so localhost:1313/topic/thing will be split into 1313 topic and thing