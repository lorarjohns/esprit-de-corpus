---
title: Hacking Your Research
date: "2019-04-30T07:56:00.000Z"
layout: post
draft: false
path: "/posts/hacking-your-research/"
category: "Gatsby"
tags:
  - "Research"
  - "FlatironSchool"
  - "Git"
  - "Github"
description: "In which the author jumps into a project before they feel qualified, with solid research skills as a safety net"
---
# Dr. Data: Or How I Learned to Stop Worrying and Love the Chaos

I am a recovering perfectionist. I jones for total knowledge of a subject before I start an endeavor. I love the high of getting it right on the first try. I get the DTs when I have an unfinished to-do list. I crave the security of feeling like I am in total control of a project.

But we all know that isn't how real life works. For one thing, we have to collaborate, so no one can (or should) have total control over any project. For another, if we tried to wait until we knew "enough" to start a project, we'd never get anything done.

## Everyone Is an Impostor, So Relax and Get Messy

<figure style="width: 700px">
	<img src="./bus.jpg" alt="The Magic School Bus, Lost in the solar system, https://www.flickr.com/photos/xmoltarx/7763397212">
	<figcaption>“Take chances, make mistakes, get messy!” - Miss Frizzle</figcaption>
</figure>


[Impostor syndrome](https://www.apa.org/gradpsych/2013/11/fraud) affects everyone, but especially people starting a new venture (like a data science bootcamp...?).

Here's the truth of the matter: You will never be 'ready' to start. No one is.

* You're never going to read all the documentation or exhaust the literature.
* Even if you did, you wouldn't know what's worth retaining and what's not, because you haven't applied it to a real problem.
* Even if you think you know everything, you can't foresee all the problems and questions that will pop up as you go.
* You will **not** get it perfect the first time you try it.

The good news is that [research says](https://psycnet.apa.org/doiLanding?doi=10.1037%2Fxlm0000073) that making mistakes strengthens your learning and recall. So jumping in and starting with the tools you've got is often the fastest way to make progress.

## ...But Make Tactical Messes

<figure style="width: 700px">
	<img src="./magic.jpg" alt="911 - Magic School Bus, https://www.flickr.com/photos/philwolff/10606951823">
	<figcaption>Information is everywhere, if you know how to look for it.</figcaption>
</figure>


If you can't have omniscience, you need something to rescue you when you get stuck. I like to call this hacking your research. Give as much care to formulating questions and searching for their answers as you do to your code. Ask yourself:

* Am I asking the **right** question?
* How did I phrase my question? Could I use other, more specific (or more general) keywords to target the information I need?
* What databases have I checked? (If you've only checked Google and StackExchange, you're not thinking creatively enough.) Where else might this information live?
* How did I query those databases? (If you're only using natural-language searching, you're missing out on a lot.)

## Case Study: This Blog

As a Flatiron student, I blog about data science. But where would I host that blog? Wordpress and Squarespace seemed excessive (I like minimalism). Medium is a great, easy platform.

But while I've learned to kick the perfectionist tendencies, I still set the bar high for myself. I expect a lot, and "easy" is seldom the path I want to take.

Part of learning data science is learning to master git and GitHub. So when I learned there was a way to blog using GitHub, how could I resist?

Using [Gatsby](https://www.gatsbyjs.org/) and [Netlify](https://www.netlify.com/), you can create a static blog, put it in a GitHub repo, and push posts to it from your local repository. I encountered two problems and solved them using the analytical framework I prescribe above.

### Problem 0: GitHub is a Git

I haven't used GitHub in any systematic way until Flatiron School.

<figure style="width: 700px">
	<img src="./git.png" alt="Git, https://xkcd.com/1597/">
	<figcaption>This was me.</figcaption>
</figure>

Choosing this blogging platform forced me to really learn how to use GitHub properly and competently manipulate files via the command line. Did I delete my entire repo and start over at least once (okay, twice)? Yes. But the long and tedious process of setting up this blog made me a _lot_ better at git and GitHub.

### Problem 1: I'm not a web dev

When I first set up my blog, I forked a template from a repo on GitHub. As I set it up to work with Netlify, I got a lot of weird errors. Many came with alarming-looking warnings, most of which were opaque to me on first glance. But my research-library background means I have the patience and skills to not panic. Instead, I systematically broke down each error message, then Googled and researched what each one meant and how to fix it. (For one thing, I had to install a bunch of dependencies.)    

The first time I tried to deploy my blog, I got some weird error messages. Methodical Googling with boolean searching taught me a lot about both how to fix them and about how these platforms work.
Gatsby is based on React, which I don't know (I'm not a JavaScript guru -- the last time I did front-end web development was in graduate school). I got some error messages related to variables in the code, which I tried to fix myself and also messaged Netlify tech support about. I had already Googled the solution they suggested, which didn't work, so I went looking in other documentation for answers. I found a thread on the GitHub repo of the blog template I forked in which someone had the same problem I did, and returned to comment with a fix that worked for them. I tried my own version of it, which succeeded, and passed the info on to Netlify's tech support. (You can take the reference librarian out of the library...)

## Google-Fu: Your Secret Weapon

Despite not feeling ready to start, I managed to get traction with my blog project through a combination of curiosity and tactical research savvy. Knowing _how_ to find information is often more important than knowing the information itself.
