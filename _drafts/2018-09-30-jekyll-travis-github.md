---
layout: single
title:  "Automatic build and deployment of Jekyll to github pages through Travis CI"
date:   2018-09-15 16:16:01 +0100
categories: jekyll coding
tags: travis CI blog ruby jekyll github
---
With this blog I came to jekyll again after about a year long pause. I was thinking about using GO and Hugo instead, which was praised for its speed and fairly compact install, but in the end I went with jekyll again, mainly because I would like to learn go, but not hugo templating. Also I hate deployment and would like to use github built pages. But as it is usual, whenever I make some decision, it kicks me in the back.

Github works beautifully with jekyll and can [build it automatically](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/). But the issue is that for security reasons it doesn't build unauthorised plugins. As I wanted to use the [minimal-mistakes](https://mmistakes.github.io/minimal-mistakes/about/) template, which to some degree relies on non core jekyll plugins, such as jekyll-archive, it means that github won't include them in the build and the page won't build properly. There are two options:

1. After writing the site and testing locally, build it locally and push.
	- *For obvious reasons I didn't want to do that. For first, I cannot or don't want to install ruby on all pcs I work at. And secondly, building and pushing locally is just a pain that would have to be solved by some shell or python script, which would convolute everything and I just hated that idea.*

2. Use continuous integration such as [travis](https://travis-ci.org/) to build the site for me and when ready, push to gh-pages.
	- *The only downside is that I need to keep the project open to use the free tiers of almost all CI provider, but the blog will be open anyway, so I didn't mind.*

I went with travis and that is when the things got really bad. I have little experience with writing and running CI and wanted to simply copy some existing solution. But none really worked and I wasn't able to figure out really why.

I started with obvious most starred solution [here](https://github.com/mfenner/jekyll-travis). Helpful, but strangely not working for my particular case and I wasn't entirely sure, what to do. That is where [this new tutorial](
https://stanko.github.io/travis-jekyll-and-github-pages/) came in.

It was generally helpful, but what got me was the ambiguity of the branch parameter, which I correctly deduced was the source branch. But then I misunderstood destination as the destination branch, but the destination branch is selected by the rake task itself based on whether you are doing a project page or your personal page

Last issue was with the base url, as all links in the project were broken. Gh-pages will create all links relative to the user base page, not relative to the project page. S I needed to create a baseurl and prepend it to the project. Successfully all these tasks were taken care of  by the minimal-mistakes jekyll template that I am using, but regardless, this wasn't the simplest thing to solve (for sure the fact I totally forgot that something like url and baseurl exists in jekyll was not helpful).

And the last trip was that I needed to to create the gh-pages, if they do not exist. Because otherwise the push wouldn't happen. All in all, here is what I did:

## How to do
1. Log in to your travis account.
2. Turn on automatic build for your repo (make sure it is a public repo or you have paid travis)
3.

https://medium.com/@mcred/supercharge-github-pages-with-jekyll-and-travis-ci-699bc0bde075