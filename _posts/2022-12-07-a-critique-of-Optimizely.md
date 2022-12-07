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
Experimenters are busy people. Regardless of whether they're engineers, product managers, or data scientists, they are most likely juggling multiple code bases, products, and projects. I've never seen anyone who launches an experiment and twiddles their thumbs while it runs, unable to do anything until the experiment is conclusive. The need to continuously monitor the experiment seems to be more imagined than real. I should note that monitoring the experiment for signs that something goes terribly wrong (usually by checking that the main KPIs do not tank) is recommended. But this doesn't require fancy statistics.

#### Immediate launch of the winning variant is overrated
Most experiments are not mission-critical, i.e., they're not  going to give a 10% lift to your KPI metrics. A majority of experiments do not show any improvement, and among those that do, the improvements tend to be modest, around the range of 0.5%~2%. While these numbers are only ballpark, they should make sense. Experiments are indended to further optimize the business - they're not meant to fundamentally change the business. As such, their effects are marginal.


#### So what do experimenters really care about?
* The experiment is configured correctly: traffic is being channeled as expected, giving the right ratio of control population size to treatment population size.
* The assignment is as good as random: any checks that can be done to verify that there's no systematic bias between control and treatment groups.
* Removal of pre-existing biases: if there are pre-existing biases (these sometimes happen even when assignment is random), there should be a procedure to remove such biases from final treatment effect estimate.

As far as I can tell (though my knowledge is likely outdate), Optimizely doesn't provide any of these reassuring features.



### References

[Optimizely stats engine white paper](https://lechipatrick.github.io/optimizely_stats_engine.pdf)

[Optimizely stats engine brochure](https://lechipatrick.github.io/optimizely_stats_engine_brochure.pdf)

[Always valid p values](https://lechipatrick.github.io/always_valid_p_values.pdf)
