---
layout: single
title:  "How to run jekyll on windows"
date:   2018-09-23 16:16:01 +0100
categories: coding jekyll ruby
tags: jekyll ruby windows chocolately programming coding personal
---

[Jekyll](https://jekyllrb.com/) is a static page generator which this blogs run on. It basically takes markdown files, html templates and few css and javascript files and then convert them all into a static html website. So every new article you add, every change rebuilds the page completely. Unlike in Wordpress, there are no logons, there is no database - which makes the blog run fast, run anywhere and run secure. But it it also a little bit tricky for those without much programming backgroud to get into. There are many sites online to teach you how to jekyll, but this is just to give you a brief overview of how to make everything run on Windows.

More technically, Jekyll is a ruby gem. It is fairly straightforward to run it on Unix systems, but it used to be pain to run it on Windows. People used somewhat complicated [multi installer setups](https://rubyinstaller.org/) or were running it on the amazing [Windows Linux Bash Shell](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) - which had it's issues with continuous file tracking. But everything changed with the brilliant [Chocolatey](https://chocolatey.org/)!

[Chocolatey](https://chocolatey.org/) brings to CMD what programmers love about shell - it allows installing useful software and packages (like python, mysql etc.) with simple syntax. It basically does what you'd do yourself by downloading executables, dependencies and registering environment variables yourself and then streamlines updates to installed packages, pays attention to updated dependencies and all. It is simply a must have on Windows.

![]({{site.baseurl}}/assets/img/2018/jekyll-windows/hackerman.jpg)

## How to do it
To run jekyll, we first need Ruby and we need bundler. Ruby is a objective programming language and bundle is a Ruby Gem (think like a ruby package).

**IMPORTANT: Always run the command prompt or PowerShell in admin mode**

1. [Install chocolatey](https://chocolatey.org/install) first. It is unobtrusive software and I guarantee you gonna thank me later, when you are updating your python distribution ;)
2. Using [chocolatey page](https://chocolatey.org/packages/ruby), you can just paste code to your cmd to install ruby (`choco install ruby`). If everything goes well, you should have access to ruby in your cmd (you can test it by `ruby irb` which starts ruby console). If it doesn't work and you didin't receive any errors during install, try refreshing your command prompt or restarting PC.
3. Install bundler by running `gem install bundler` in your command prompt.
4. Install jekyll by running `gem install jekyll`.
5. Create new website by running `jekyll new test-site`
6. Go to the website folder (`cd test-site`) and try to run it with `jekyll serve`.
7. Enjoy your windows Jekyll ;)

*NOTE: After you have succesfully installed ruby, you can just follow instructions [here](https://jekyllrb.com/docs/)*

## Troubleshooting
**Q: This page isn't working. 127.0.0.1 sent and invalid response. ERR_INVALID_HTTP_RESPONSE**
A: This is probably because you have another process running on Jekyll's port that is not serving HTTP files. You can easily set the `jekyll serve --port 4001` and see if that works.

**Q: You have already activated *gem-name version-number*, but your Gemfile requires *gem-name* - different *version-number***

A: This happens when you have installed some gems through `gem install` that is newer than jekyll gemfile requires with `~>`. Sometimes jekyll will already tell you what to do. You need to run jekyll with gem specified by the Gemfile, so you run it as `bundle exec jekyll serve`

**Q: cannot load such file -- bundler**

A: This might have to do with reinstalling or upgrading ruby, where you 'forgot' to install bundler after getting the ruby executable. Just run `gem install bundler`

**Q: Jekyll claims gems are missing**

A: If you are running on windows, it is possible that the first time you ran bundle install, some gems weren't properly installed. Just follow the recommendation above on how to solve some windows issues. If that is still the case, you can run `bundle install --force` to force reinstall all gems. This usually solves the issue.

**Q: Could not find *gem-name* in any of the sources (Bundler::GemNotFound)**

A: This can happen if the bundler got corrupted or wasn't working properly when you first ran `bundle install`. You can update bundler with `gem install bundler` and then run `bundle update` or `bundle install --force`.

**Q: Error when runnign bundle install. Cannot build native dependencies**

A: If you are installing ruby from the choco, it might be necessary to also install tools to build dependencies which do not ship with windows executables. You can do this with `ridk install 3`, as described on the choco ruby package [page](https://chocolatey.org/packages/ruby)

**Q: Issues with dependencies when running bundle install**

A: Sometimes it happens that you will not be able to install things with bundle install due to some package depending on a package which is not freely available. If that is the case, you can install all necessary package with `gem install package --ignore-dependencies` and then hope all will work when you run `bundle exec jekyll serve`. I had this issue with **nokogiri** package which only worked with ruby 2.7 in a dev version which was not available for other packages as a dependency.