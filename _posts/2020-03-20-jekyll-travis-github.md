---
layout: single
title:  "Automatic deployment of Jekyll site to Github pages through Travis CI"
date:   2020-03-20 16:16:16 +0100
categories: programming
tags: travis CI jekyll
---
When I started writing posts for this blog I returned to Jekyll again after about a year long break. I was thinking about using GO and Hugo instead (more on Hugo in the future), which was praised for its speed and fairly compact install, but in the end I went with Jekyll again. The main reasons being I would like to learn Go, but not Hugo templating. Also I hate deployment and would like to use Github built pages. They are ideal for small websites with low enough traffic. But as per usual, whenever I make any decision, it kicks me in the back.

Github works beautifully with Jekyll and can [build it automatically](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/). But the issue is that for security reasons it doesn't build unauthorised gems. Neither it works well with submodules, if you use them. Unfortunately, after much searching I decided to use the [minimal-mistakes](https://mmistakes.github.io/minimal-mistakes/about/) template, which to some degree relies on non core jekyll plugins, such as jekyll-archive, it means that Github won't include them in the build and the page won't build properly. There are two main options:

1. After coding and testing the site locally, build it and push the result to a gh-pages branch.
	- *For obvious reasons I didn't want to do that. For first, I can't and I don't want to install ruby on all pcs I work at. Secondly, building and pushing locally is just a pain that would have to be solved by some shell or python script, which would convolute everything and I just hated that idea.*

2. Use continuous integration such as [travis](https://travis-ci.org/) to build the site and if it succeeds, push the result to gh-pages.
	- *The only downside is that I need to keep the project open to use the free tiers of almost all CI providers, but the blog will be open anyway, so I didn't mind.*

## Process
The process is fairly straightforward these days. It was a very convoluted process in 2017, but Travis now allows automated deploy to Github Pages, so you only need to tweak few parameters. Depending on if you are building your personal or project page, you should have the code and final build in different branches. See the difference [here](https://help.github.com/en/github/working-with-github-pages/about-github-pages#types-of-github-pages-sites)

1. Make the project public, if you are using free tier of Travis.
2. Activate builds on the repository in Travis.
3. In Github -> Settings -> Developper Settings add new "Personal access token" for repo (just check `repo` (and all subcategories) and leave the rest empty). Name it appropriately and copy (it will only show once)
4. In travis settings for your repository, add this TOKEN to environment variables (remeber to keep it hidden)
5. Add the following .travis.yml to the project (this is for a project page, you will need to do some changes to build and deployment branches for personal site)

```
language: ruby
rvm:
- 2.5
branches:
  only:
  - master
install:
- bundle install #make sure your bundle works locally :)
script:
  - JEKYLL_ENV=production bundle exec jekyll build --destination out #builds to out directory
deploy:
  provider: pages # github pages
  local-dir: ./out 
  target-branch: gh-pages
  email: deploy@travis-ci.org
  name: Deployment Bot
  skip-cleanup: true
  github-token: $TOKEN # The token you saved
  keep-history: true # does only incremental builds
  on:
    branch: master
```

That is it :) You should have an automatically built and deployed jekyll blog. If something doesn't work, just check this blog `.travis.yml` settings [here](https://github.com/hejtmy/neuro-coder/blob/master/.travis.yml), some settings might have changed between years and versions. 

### Custom domain

If you are using a custom domain you can follow [this tutorial](https://github.com/hejtmy/neuro-coder/blob/master/.travis.yml).

## Troubleshooting
Potential issue can with the base url, as all links in the project were broken on my first try. Gh-pages will create all links relative to the user base page, not relative to the project page. This got fixed in future versions of Jekyll, but it was a pain in 2017-19, where I needed different setup for local and for production deployments. Luckily, URL handling was taken care of by the `minimal-mistakes` gem, but regardless, this wasn't the simplest thing to solve (for sure the fact I totally forgot that something like url and baseurl exists in jekyll was not helpful).
