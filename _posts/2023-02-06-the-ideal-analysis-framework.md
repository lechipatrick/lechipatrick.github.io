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

##### Detection and removal of pre-existing biases
Sometimes, despite our best efforts to randomize (to make all variation groups statistically similar to one another), one group is still systematically different from another. For example, average order volume from treatment group is higher than the control group, even before the experiment. This could be a bug in the randomization logic, or it could be pure bad luck. Not taking this into account would bias the results toward finding that the treatment lifts order volume, whereas in fact it could have done nothing. If pre-existing biases exist, a standard way would be to take them into account by using regression analysis approach to control for pre-experiment metric values. 

##### Detection and removal of outliers
Outliers can really mess up the experiment results. Typically, one looks at the metric averages, and averages tend to be significantly affected by the presence of outliers. One could resort to non-average measures such as the quantiles and median, but such analysis is not standard and the statistics can get complicated. Instead, it would be beneficial to run the data through a outlier/anomaly detection algorithm and remove and outliers found. One can also resort to rules of thumb, such as "drop all values more than X" (windsorize) or "truncate values 4 standard deviations away from the mean."

##### Validity of individual statistical tests
Statisticians have developed various statistical tests to work effectively for various settings, but no one single test works for all settings. It's easy to slap a t-test on everything, but there are cases where it's not appropriate, or not efficient. For example, to perform tests on ratio metric, we need not the t-test, but the delta method. But the delta method doesn't always work - sometimes we would need to resort to Fieller's theorem instead. Having the awareness of the limitations and being able to automatically use the appropriate statistical test given the data type/property is important.

##### Validity of multiple statistical tests
One flavor of multiple statistical tests is repeatedly testing the same metric. Another flavor pertains to testing multiple metrics at analysis time. I've heard that Twitter's analysis platform would automatically run tests on thousands of metrics and then "filter" to show just the ones with statistical significance. Now that's flawed statistical testing at scale. But what would one pragmatically do when it comes to multiple tests?

The simplest way to address this is to impose a stricter threshold across all metrics, a method called "Bonferroni correction." This method is simple and effective, and easy to convey to data scientists and others. More sophisticated methods involve "ranking" the metrics to be tested in some sequential order, and "carry over" the alpha from one metric to the next, depending on the test result. This method is fascinating, but tends to be too complicated to be used by non-statisticians in a large scale A/B analysis system.

##### Metric query capabilities
In early stages, a company might have an analyst hand-code a ETL that would pre-compute each and every experiment metric beforehand. At analysis time, the metrics would already be sitting pretty in a table, ready for all kinds of manipulations. Obviously, this would not scale to a large company with thousands of metrics.

What I've seen to be effective is to have a system of metric definition and a query engine that can take those definitions and returns the metric values for the metrics for the experiment at hand. This does take a decent amount of engineering and alignment on metric definitions, but can bring a host of benefits in standardization, organization, etc. (ever been in an org where every team has their own definition of "revenue"?)

##### Variance reduction
This tends to be more of a custom analysis. Sometimes there would be a lot of variation in the user population, leading to a low signal to noise ratio. In order to decrease the noise, one can use variance reduction technique. A popular one is called CUPED (Controlled-experiment Using Pre-Existing Data) - basically perform regression analysis using pre-experiment metrics as additional controls.
