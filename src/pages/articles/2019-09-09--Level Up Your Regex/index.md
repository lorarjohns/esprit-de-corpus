---
title: Level Up Your Regex
date: "2019-09-09T23:24:37.121Z"
layout: post
draft: false
path: "/posts/level_up_regex/"
category: "Python"
tags:
  - "regex"
  - "programming"
  - "python"
description: "using regular expressions for power and glory"
---

# Level up your regex skills

### Regular expressions for fun and profit

![](https://cdn-images-1.medium.com/max/2960/1*_j7XilcCld70c8FZvwbtmQ.png)

Regular expressions have a bad reputation. They’re hard, they’re opaque, they break things.

![They even [broke Stack Overflow](https://stackstatus.net/post/147710624694/outage-postmortem-july-20-2016).](https://cdn-images-1.medium.com/max/2852/1*gde2kdfg8scE1pV_Dx9lrg.png)*They even [broke Stack Overflow](https://stackstatus.net/post/147710624694/outage-postmortem-july-20-2016).*

Relying on built-in modules for string matching is fine for some tasks, but you’ll miss out on the power and flexibility that comes from being able to writing your own regex. (If you’re really dedicated, you can even make them [do arithmetic](https://regex101.com/r/xF5sEs/1).)

This post will show you some use cases for regex you might not have tried before and give you resources to make learning them — dare I say — fun.

### **Make your f-strings more versatile**

Python’s [f-strings](https://realpython.com/python-f-strings/) are fantastic — they’re readable, concise, and less error-prone than the older %-formatting method.

You can get even more out of them with regex. Prefix you expression with *r *for “raw”, which tells Python to ignore all the escape characters.

![But beware, even a raw string can’t consist **entirely** of a single backslash.](https://cdn-images-1.medium.com/max/3228/1*QvWb3aaGZ3zdc3oODW01kw.png)*But beware, even a raw string can’t consist **entirely** of a single backslash.*

Combine *r* with the *f *prefix when you need to swap parts of your regex. You can use this to write shorter loops or hold a place for a value you don’t have yet or need to compile. ([Here’s an example](https://nbviewer.jupyter.org/github/lorarjohns/Miniprojects/blob/master/notebooks/Python/Regex%20for%20Eric.ipynb) of the latter case.)

![Use re.compile to pre-compile a regular expression object.](https://cdn-images-1.medium.com/max/3028/1*2aNGnat0GOZCSHb_YbQr5A.png)*Use re.compile to pre-compile a regular expression object.*

![](https://cdn-images-1.medium.com/max/3364/1*A7CPIZT8LRJfVkuZ1hji4Q.png)



### Find and replace text with precision

Regex give you a parsimonious way to alter the content of strings. In one line, you can target the items to replace and alter them using capturing groups.

Here, I’ve used this technique to scan sentences from news articles and make hashtags from the word “alien”. 

![Use a numbered reference to the capturing group enclosed in parentheses.](https://cdn-images-1.medium.com/max/3196/1*IcPEAyRpGvucrW-Z7XQK1g.png)*Use a numbered reference to the capturing group enclosed in parentheses.*

![The second expression replaces the targeted substring in the call to re.sub.](https://cdn-images-1.medium.com/max/2892/1*egcKGfYhAbr1oKzLVWugFQ.png)*The second expression replaces the targeted substring in the call to re.sub.*

### Get valuable information from noisy data

I use regex in my everyday life to simplify other tasks (yes, really). For example, I wanted a list of packages from a requirements.txt file, but didn’t want their specific versions.

![Not pleasant.](https://cdn-images-1.medium.com/max/2000/1*kfA-ZHTjh-ZVcAhLdlXosQ.png)*Not pleasant.*

Regex prevented the tedium of having to extract the package names manually. You can see how I did it at [Regex101](https://regex101.com/r/9r8tGT/2/). I like using [BBEdit](https://www.barebones.com/products/bbedit/) (formerly TextWrangler) for this, but you can also use the “export matches” feature in Regex101. The website gives you the added benefit of debugging your expression in real time.

The time spent learning regex pays for itself several times over by saving you from tedious searching. I’ve used regex to [extract regular expressions ](https://regex101.com/r/ublbXJ/3)from other Python scripts and [grepping](https://www.regular-expressions.info/grep.html) for files on the command line.

### Train your brain and enjoy the challenge

In applying regex, you’ll improve your computational thinking skills by decomposing a search problem, abstracting patterns, and applying them algorithmically.

![I’m fun at parties.](https://cdn-images-1.medium.com/max/2154/1*yD9djCtAIvse_GrPNiqiXA.png)*I’m fun at parties.*

But the best reason to use regex might be that they’re just *fun*. If you’re the kind of person who enjoys puzzles, you’ll get hooked on finding  different ways to solve the same problem and resolving edge cases. 

While it’s true that regular expressions can be hard and sometimes dangerous, most of the best things in life are. Do a few [crosswords](https://regexcrossword.com), play a little [regex golf](https://nbviewer.jupyter.org/url/norvig.com/ipython/xkcd1313.ipynb), and see if you don’t agree.


