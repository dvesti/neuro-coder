---
layout: single
title:  " On the subject of manuals: Board games and coding"
date:   2017-06-02 16:16:01 +0100
categories: coding boardgames
tags: Hugo programming coding personal
---

It happened again. I was trying to solve a stupid coding problem for 4 hours without any long term success. I tried the examples provided on the documentation page and they worked flawlessly, but my "slight" change never built. To be honest, I had no idea what I was doing. It looked like I forgot everything about the framework, about the language and about programing all together. Internet couldn't help me. Seemingly nobody had the same problem. Those issues slightly similar that I found discussed on forums were no good. So as a last resort, I tried to give the manual one more try. And there it was. The simple answer to my problem in black on white, on the first page in the second paragraph. I felt so stupid. Suddenly all the fails made sense. I knew what I was doing wrong and it only required to read the manual properly. All those *helpful* people said so. *Just read the manual, here". They were right. Or were they? If I learned anything from my 8 years of boardgaming experience, it is that manuals are most of the time CRAP.

# Are manuals useless?

Don't get me wrong, they are not! Manuals are awesome! But what I want to get at is, that manuals provide extensive overview of a content you often don't understand at all. If I just handed you a book on meteorology and said: "Tell me if it's going to rain on Friday. All the relevant information is here.", you'd send me places. But at the same time this is exactly what other people tell you when answering coding questions on forums or StackOverflow. 

## Two types of manuals

There are generally two ways to write the manual. One is to write in legible and understandable way, use familiar language, include examples and engaging text, slowly progress through the content. But this in turn means it includes a lots of fluff and fillers. It allows newcomers to understand concepts better, but slows anybody trying to find anything in the sheer amount of text. It also makes the entire thing longer to go through, heavier, messier and generally difficult to separate important facts from the fluff. 

The other way is to write the manual a a handbook for those already somewhat familiar with the field. The manual then includes only relevant and important information, it is structured not necessarily in a chronological/learning order, but often alphabetically or context-wise. You can rely on it providing you all the necessary information when you go to the wanted section, but it is practically unusable for any newcomer to really use. Mainly because they sometimes have no idea what section they even want.

Both of these approaches are great and important, yet both feature various caveats that cannot be simply swept under the table. Let's take a look at them, see how boardgaming industry tried to implement it in the past few years and what can we learn from its successes and failures.

# Writing manuals as textbooks
When I received my Settlers of Catan, there were already two manuals - the official one and an Example of play. The example of play was basically scripted manual that told each player what to do on their turns. It faked rolls of die, faked trades etc. You had no say in your play, you only did what the manual told you to. It was OK and implemented the core mechanic of teaching - learn by doing. But a the same time, I remember my grandfather hating it because he wanted to make his own decisions.

In the past years, many board game companies started releasing manuals in more user friendly, learn as you go way. Fantasy Flight Games started releasing every game with two or more manuals: Learn to play and Reference rules. This was implemented to a partial content and discontent of various users. The Learn to Play rules are written simply, they outline all main parts of the game, give clear overview nad examples and should, hypothetically, start you right off. The Rules reference are written as series of terms (Turn sequence, Combat, Healt ..) listed in alphabetical order with dry, law like heavy language.

Some manuals also try to ease players into the game by slow incorporation of rules. One of the first I can remember was the Space Alert by te Vladimír Chvátil, published by Czech Games. The game is incredibly complicated, features fast action and so many rules, endemic to Chvátil's work. But the game realised this problem and introduced several tutorial scenarios. Each of these basically told you to forget about some aspect of the game. Second scenario bumps up the game a bit and so on until you are finally playing te game as it was intended. 

As a last example in this category, I'd like to mention newly released This War of Mine. TWoM takes even more audatious approach, basically telling you to start playing while passing the manual around. It works as an interactive storytelling game. The manual even tells you to figure out answers to some of your questions rather than try and look into the manual to not slow the game. Sounds weird? Because it is.

## Issues
The first thing to realise is, that this type of manual is amazing for two groups of gamers. Those, who take gaming as a casual relaxing time and those, who might be more serious, but are new to the game and want to start really fast. I consider our group to be casual yet competitive, but when we want to start some game and are bored or disgusted by a badly written manual, we rather think our own rules than slow everything down turning pages.

So most geeks hate this type of manual. It makes it difficult to find those simple rules like: How many cards to deal? How does the game end? etc. Most of the time, after you've played the game once, you have a good understanding of how it plays and you only want to check those few details (e.g. I never remember how many cards to draw in ANY card game). 

All the fluff part can make it more confusing and hard to find important information. It also makes it easy to miss rules, which is fine as long people are fine with it as well. But loosing a long game because you messed up some rules might make some salty and butthurt.

So if you are a gamer who cares about the relaxed experience, this manual is amazing. But if you are more experienced or serious about the game, this manual is crap.
 
# Writing manuals as handbooks
1989, Twilight struggle and Labyrinth

## Issues
Not knowing how the system works will impede any effort to understand it.
It takes ages to start playng
It takes

# What makes a bad manual


# Possible solutions?
- Arkham horror - Grim rule = Allow users to fail when they continue to enjoy the ride. 
- Lots and lots of examples


Write software so that warnings are understandable. Don't impede using your library by throwing errors that mean nothing, rather try to inform the user what is it that they are missing. IN some languages you can enforce type, but in others (javascript, python, R) you need to inform user why you failed (This param takes only int array)

Remember you are writing manuals for two groups - newcommmers and experienced users. If you can manage it, try to write two manuals, one for each.

But despite the modern approach of using the first type of written manual in boardgames as well as a form of documenting software has still 

# Good practice
- Always include FAQ, most common problems
- Always include reproducible examples
- Errors should provide clear feedback
- 