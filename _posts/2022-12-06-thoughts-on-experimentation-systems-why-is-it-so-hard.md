---
layout: post
title: Thoughts on experimentation systems - part I - why is it so hard?
---    
I've worked on and interacted with experimentation systems at various tech companies (Udemy, Uber, Shipt). These are some reflections I have on why it's so hard to have an effective, successful experimentation system.

### Customers of various stripes
<!-- your comment An experimentation system has multiple components: (a) an UI for experimenters to set up, configure and analyze their experiments, (b) a backend system that powers the UI and interacts with other systems via API/RPC calls, and (c) an analysis system to perform various types of analyses on the experiment. The experimentation system has -->

The customers of an experimentation system, the experimenters, are a diverse bunch. They range from engineers to product managers to data scientists to machine learning researchers, and more. These groups have different backgrounds, skillsets and concerns. An engineer would mostly care about latency of the bucketing/assignment API/RPC calls, a product manager would mostly care about being able to launch the experiment quickly, and the data scientists would want to be able to do various custom analyses, as well as be assured that the statistical procedures used are correct and can be relied on to make decisions. All these concerns are not easy to satisfy, and typically the experimentation team needs to have separate sub-teams that focus on separate concerns - (a) a UI team (b) an API team that does the backend and the other engineering requirements (c) an analysis team that focuses on statistical methodologies and integrates them into the overall system. Coordinating these sub-teams to work together is hard.

### Experimentation - correct, robust and rigorous analysis as a service

One reason that the R language is popular among statisticians and data scientists is that it was built *by statisticians for statisticians*. Experimentation systems are rarely, if ever, built or designed by statisticians. Yet they make up a major user group. This disconnect is what I think underlies many of the difficulties in building a satisfactory experimentation system. Often, the intended target users are assumed to be mainly product managers or engineers, who aren't aware of the fine but important details and assumptions in statistical inference. Such a system can have crippling issues. Below are just some examples.

* Users are bucketed into multiple treatment groups. This happens when the distribution of traffic into various treatment groups is allowed to change, or a new group is added to the experiment. Such a change necessarily leads to "reassignment" - users previously assigned to control group are now assigned to treatment group, for example. It's very hard to do analysis when users are assigned to both control and treatment. Unless, of course, when you are a trained statistician. But one wouldn't realize that this is a problem without being a statistician.
* Incomprehensible or unreliable statistical analyses. This happens when the engineers write the analysis methods themselves, or port statistical procedures from python to their production code base language (scala, go, java). Alternatively, this can also happen when one outsources the analysis to a third party provider like Optimizely, which uses proprietary analysis methods (I'll have a whole post on [Optimizely](https://lechipatrick.github.io/a-critique-of-Optimizely/)). The problem is that of trust and consistency. Experimentation users often like to deep dive into their experiments, ana a first step would typically be to reproduce the results they see on the platform. Usually, the results would not match, and it is hard to reconcile the differences. Reading scala code is hard for data scientist, and in the case of Optimizely, it's just impossible since you don't know what you're replicating. Would you use results from a system that you can't reproduce? 

### Here are some ways to avoid such issues:

#### Involve a statistician
A statistician should work closely with the engineers on the design and behavior of the system. This would ensure that crucial assumptions are satisfied for valid statistical analyses.

#### Make statistical analyses transparent and accessible
Maintain a python package that holds all statistical analysis code. This package should be made pip-installable so any data scientist can use it, as well as packaged into a container image and hooked up to the experimentation platform. This way, it's clear what's being done, and users would have no issue replicating the results. The building of such a statistical package should be accompanied by clean and detailed documentation of the methodology, how it works, why it works, and when it works. Unit tests should be provided to confirm that the procedures indeed have the expected statistical properties (e.g., false positive rate, power, etc.).


### References
Trustworthy Online Controlled Experiments: A Practical Guide to A/B Testing - Book by Diane Tang, Ron Kohavi, and Ya Xu
