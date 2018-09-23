---
layout: single
title:  "Hugo continuous deployment"
date:   2018-09-20 16:16:01 +0100
categories: hugo blog
tags: hugo travis CI blog golang go
---

http://rcoedo.com/post/hugo-static-site-generator/

But I didn¨t want to use speciall ssh acces for travies if ithub has the amazing personal repo tokens. I found another tutorial, but this one for some rason wanted another tutorial, but this one for some rason wanted me to build my own hugo libraries. I understand the benefit of that (as sometimes we don't update hugo as often as we should¨, but eventually i dcided to make the better of the both worlds. So I used the second deploy script and used modifid travis file to build the hugo libraries on the fly.

http://speps.github.io/articles/hugo-setup/

Overall, it was similarly easy to deploy as jekyll deployment throught tavis

What tripped me a bit was that I wanted to use a custom CNAME, but each deployment was overwriting the file. To be entirely hones, I never go to the shell scripting, so I have little idea what that deployment script does, but I am somewhat skilled in linux and good at copy paste, so I simply added `echo "brain.vr" > public/CNAME` into the after script with travis, which solves the problem quite efficiently.