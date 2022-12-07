---
layout: post
title: A critique of Optimizely
---   

[Optimizely](https://www.optimizely.com/) is an experimetation platform that offers 2 main services: configuring experiments (traffic splitting, treatment group assignment) and statistical analyses of the configured experiments. Here I outline several limitations, mainly with the analysis part, with using Optimizely.

### Motivations behind Optimizely's statistical methods
Optimizely uses a rather complicated statistical inference process that supposedly allows for continuous monitoring, efficient detection of any treatment effect, while guaranteeing that all inferences made in the process are statistically valid. Several main rationales are given for using such a procedure:
* Users want to monitor their experiments continuously and stop the experiment as soon as some effect is detected. This "peeking" inflates the false positive error rate when traditional statistical methods are used.
* Designing an experiment in the traditional sense (power calculation, sample size calculation, when to look at the results, etc.) is hard and involves too much guessing.

While these motivations sound great on paper, my experience with experimentation systems and experimenters/users reveal that these issues are rather minor.

#### Continuous monitoring is overrated
Experimenters are busy people. Regardless of whether they're engineers, product managers, or data scientists, they are most likely juggling multiple codebases, products, and/or projects. I've never seen anyone who launches an experiment and twiddles their thumbs while it runs, unable to do anything until the experiment is conclusive. 


### References

[Optimizely stats engine white paper](https://lechipatrick.github.io/optimizely_stats_engine.pdf)

[Optimizely stats engine brochure](https://lechipatrick.github.io/optimizely_stats_engine_brochure.pdf)

[Always valid p values](https://lechipatrick.github.io/always_valid_p_values.pdf)
