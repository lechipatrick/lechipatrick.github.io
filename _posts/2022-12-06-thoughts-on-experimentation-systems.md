---
layout: post
title: Thoughts on experimentation systems - part I - why is it so hard?
---    
I've worked on and interacted with experimentation systems at various tech companies (Udemy, Uber, Shipt). These are some reflections I have why it's so hard to have an effective experimentation system.

### Built by engineers, for statisticians

One reason that the R language is popular among statisticians and data scientists is that it was built *by statisticians for statisticians*. Experimentation systems are rarely, if ever, built or designed by statisticians. Yet their main target users are (or should be) statisticians. This disconnect is what I think underlies many of the difficulties in building a satisfactory experimentation system. Often, the intended target users are mistaken to be product managers or engineers, who aren't aware of the fine but important details and assumptions in statistical inference. Such a system can have crippling issues. Below are just some examples.

* Users are bucketed into multiple treatment groups. This happens when the distribution of traffic into various treatment groups is allowed to change, or a new group is added to the experiment. Such a change necessarily leads to "reassignment" - users previously assigned to control group are now assigned to treatment group, for example. It's very hard to do analysis when users are assigned to both control and treatment. Unless, of course, when you are a trained statistician. But one wouldn't realize that this is a problem without being a statistician.
* Incomprehensible or unreliable statistical analyses. This happens when the engineers write the analysis methods themselves, or port statistical procedures from python to their production code base language (scala, go, java). Alternatively, this can also happen when one outsources the analysis to a third party provider like Optimizely, which uses proprietary analysis methods (I'll have a whole post on Optimizely). The problem is that of trust and consistency. Experimentation users often like to deep dive into their experiments, ana a first step would typically be to reproduce the results they see on the platform. Usually, the results would not match, and it is hard to reconcile the differences. Reading scala code is hard for data scientist, and in the case of Optimizely, it's just impossible since you don't know what you're replicating. Would you use results from a system that you can't reproduce? 
* 


### Ways to make it easier

#### Designed by statisticians, for statisticians, implemented by engineers
The statistician should work closely with the engineer on the design 

#### Analyzed by statisticians

### References
Trustworthy Online Controlled Experiments: A Practical Guide to A/B Testing - Book by Diane Tang, Ron Kohavi, and Ya Xu