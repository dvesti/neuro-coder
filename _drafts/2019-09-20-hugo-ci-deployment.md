---
layout: single
title:  "Hugo continuous deployment"
date:   2018-09-20 16:16:01 +0100
categories: hugo blog
tags: hugo travis CI blog golang go
---



## Troubleshooting
Q: This page isn't working. 127.0.0.1 sent and invalid response. ERR_INVALID_HTTP_RESPONSE
A: This is probably becasue you have another process running on Jekyll's port that is not serving HTTP files. You can easilly set the `jekyll serve --port 4001` and see if that works.

**Q: You have already activated *gem-name version-number*, but your Gemfile requires *gem-name - different version-number* **
A: This happens when you have installed some gems through `gem install` that is newer than jekyll gemfile requires with `~>`. Sometimes jekyll will already tell you what to do. You need to run jekyll with gem specified by the Gemfile, so you run it as `bundle exec jekyll serve`

**Q: cannot load such file -- bundler**
A: This might have to do with reinstalling or upgrading ruby, where you 'forgot' to install bundler after getting the ruby executable. Just run `gem install bundler`

**Q: Jekyll claims gems are missings.**
A: If you are running on windows, it is possible that the first time you ran bundle install, some gems weren't properly isntalled. Just follow the recommendation above on how to solve some windows issues. If that is still the case, you can run `bundle install --force` to force reinstall all gems. This usually solves the issue.

**Q: Could not find *gem-name* in any of the sources (Bundler::GemNotFound)**
A: This can happen if the bundler got corrupted or wasn¨t working properly when you first ran `bundle install`. You can update bundler with `gem install bundler` and then run `bundle update` or `bundle install --force`.