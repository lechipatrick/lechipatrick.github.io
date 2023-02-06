---
layout: post
title: An Ideal Experiment Analysis Framework
---

<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>

How should one analyze an experiment, given data at hand?
##### Imbalance check - verify that randomization works as intended
Typically, the experiment setup specifies a split between control and treatment. One can check that this split is indeed similar to what is actually observed in the data. Statistical tests can be performed to check whether the intended distribution is not too different from the observed distribution. If there seems to be a difference, then one should look into possible root causes for this imbalance. Surprising things can come up. For example, a reason that an experiment on mobile app has imbalance (from the desired 50/50 split) is that very often the treatment causes the app to crash, failing to even log the fact that the user has been exposed to treatment. Failing to recognize this problem would lead to the analyses of only non-crash user experience, biasing the results.

##### Pre-existing biases
Sometimes, despite our best efforts to randomize (to make all variation groups statistically similar to one another), one group is still systematically different from another. For example, average order volume from treatment group is higher than the control group, even before the experiment. This could be a bug in the randomization logic, or it could be pure bad luck. Not taking this into account would bias the results toward finding that the treatment lifts order volume, whereas in fact it could have done nothing. If pre-existing biases exist, a standard way would be to take them into account by using regression analysis approach to control for pre-experiment metric values. 

##### Detection and removal of outliers
Outliers can really mess up the experiment results. Typically, one looks at the metric averages, and averages tend to be significantly affected by the presence of outliers. One could resort to non-average measures such as the quantiles and median, but such analysis is not standard and the statistics can get complicated. Instead, it would be beneficial to run the data through a outlier/anomaly detection algorithm and remove and outliers found. One can also resort to rules of thumb, such as "drop all values more than X" (windsorize) or "truncate values 4 standard deviations away from the mean."
