---
title: "README-Driven Development"
layout: post
og_description: "Starting with a README leads to better software and happier users. Thanks to Github for featuring it prominently."
---

Among [Github](http://github.com)'s many contributions to software development, the prominence it affords the README is undervalued. By making it obvious to users where to start looking for info about a project, it also gives developers a good starting point.

I try to fully embrace the README and treat it as a first-class citizen alongside code and tests. This is similar to [Amazon's "working backwards" approach](http://www.allthingsdistributed.com/2006/11/working_backwards.html). Werner Vogels summarises it as 

> start by writing the documents we'll need at launch (the press release and the faq) and then work towards documents that are closer to the implementation

Starting with the README puts the developer in the mindset of the user. Questions such as "what is this for?", "what does the user need to know to get started?", "what do I want the API to look like?" come to the fore right away. Complex installations, burdensome multi-step operations, etc. are revealed and can be simplified before too many users have to struggle through them.

While I'm not yet quite as rigorous as Amazon and tend to iterate on the code and the README in tandem, the benefits are still sizeable. At the very least, I'm guaranteed to have documentation (a project without documentation might as well not exist!). At best, I'll have much better and friendlier software.

java.net is the opposite of Github, in this respect. 80% of the time, I can't figure out how the library works, because I don't know where the documentation is. java.net itself is very confusing and this rubs off on the projects it hosts. The most catastrophic example of this is Rome, supposedly Java's leading RSS library. The [java.net site](https://java.net/projects/rome/pages/Home) links to a URL that has been taken over by spammers. The [real site](https://rometools.jira.com/wiki/display/ROME/Home) contains broken links (notably the Tutorials and Articles link!) and very little information.

When the project is on Github, however, there's a 90% chance there'll be a good README, or at least a pointer to a dedicated site. It's a virtuous circle: Github makes it easy to find the documentation, users feel good about checking out projects on Github and new projects are more likely to make good documentation a priority.
