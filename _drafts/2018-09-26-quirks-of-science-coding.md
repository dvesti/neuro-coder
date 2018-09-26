---
layout: single
title:  "Quirks of scientific coding that need to go away. Now!"
date:   2017-3-10 16:16:01 +0100
categories: Coding
tags: science coding habits
comments: true
---
I get it. Scientists are pressured to produce results fast, no need to clean up code, everything is unique and everything is hasty. Well, lemme tell you something - everybody's coding is pressured to be fast and efficient. We scientists don't have it easier. WE are not special. Yet the code I have seen shows that . I've heard excuses such as - "we should do science, not coding" or "why should I clean it up, I can understand it well".  None of these are truly relevant. 

At the same time in which journals are pushing for the analysis code to be released along with the data and the paper, at the time of increased scientific networking, I think it is time to drop the sense of privilege, loose the feeling of being special nad different from other coders, and let us build more reproducible and clear, coding science. 

## Quirks

1. Passing code on usb-sticks or in emails

It still happens that my colleagues are passing along "libraries" or "toolboxes" on flash disks, or those more progressive are keeping their code in cloud in dropbox or google drive. Please stop.

Not only it always happens that you don't have the same copy and those "toolboxes" just don't toolbox anything, but in case I find the mistake, I fix it and pass along, with you passing it along and we all start forming this weird pass along thing resembling  the thing from the Expanse. 

Better yet, we send the code in emails. In multiple emails, from differnet addresses, and the names get more and more convoluted to toolox_1.4c_fixed_mikeDec72016.m . Multiple people are working on it and it just get messy. Please all, learn git or have somebody of your lab learn it and share the knwoledge. Please

2. Thinking that we are all super smart and decrypting code in real time

It happens all the time that scientific code looks like it's been encrypted so that the reviewer won't really find the line of code that deletes all outliers. Code like this makes me reconsider my intelligence, sanity and life choices every day.

```matlab
clrbar_inds = cVals2cInds(repmat(clrbar_vals, [1, 10]), [clims(1), clims(2)], size(clrmap.brain, 1) + [1,size(clrmap.chnls, 1)]);
```

And that is matlab, user friendly language. But I will always prefer clearly written C++ to badly writen python. 

I have no damm clue what it does but evidently it returns a slice of mri? Please. Name variables and functions like they matter. Because they do!

3. Believing that one file is better than two
If goes a bit with the previous comment, but functions/scripts shouldn't make my scrolling finger hurt. I guess there can be a bit of a debate how much code can reside in a single file, so make your own judgment about this. My rule is, that if you are not compiling a library to drive mars rover, your single file doesn't have to have 400 lines. And if you are, your files don’t have 400 lines because NASA says they cant :)

4. Organize files like we organize our desks
I know I can `find` stuff with ctrl-f, and modern IDEs allow nice search and hyperlinks to definitions and usages, but it is much easier to find function `clearbadelectrodes` in a file `clearbadelectrodes.m` than in file `eegHelpersFunctionsModified.m`

Files should be names as clearly as the functions and code it resides in and be in places where its logical to search for them. Please use folders! Folders have been invented much before computers for a reason. Look:

[Picture of mess and picture of organized stuff]

There are specific naming conventions for diffrent languages and frameworks, please don't reinvent the wheel. And if there isn't one, just use your scientific mind :) 

It happened many times that I was offered to use a toolbox that looked like somebody vomited a bunch of files in my lap and ask me to clean it. At least include some file where I can start from - viz next section.

5. Document your code a bit
You don't need to overdo it with comments. As a very good rule says, every comment is a poorly written code. Although I know its not necessarily true, the idea is clear. Comments should comment what isn't clear. But code SHOULD be clear. If you use a weird hack, if you initialize variable at 6, instead of common 0/1/NULL, that is a place for a comment. Comment such sections, but don't bother with clear, readable code.

One of the best ways to document how to use your code is always examples. If you are writing an extensive piece of code, try add example folder and put few biuts and pieces in there. Don't forget to provide sample data as well!

And that might be it. These are the most common and most annoying things many scientists (me included) are still guilty of doing. Lots have been written on this subject, this is just another bit to the mix. Please, let us build good science together :)