---
layout: post
title: Simple group sequential testing
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


#### Peeking, inflated false positive rate, and $\alpha$-spending functions
In a typical, finite horizon experiment, the experimenter is supposed to look at the data once at the very end of data collection, and performs statistical tests using the traditional thresholds corresponding to a size of 5% (e.g., a threshold of 1.96 for a t-test). In practice, A/B experimenters often look at the data and perform tests before the experiment has run its intended duration. This so-called "peeking" leads to inflated false positive rate (FPR) - the chance of falsely "detecting" a treatment effect while there is none is no longer 5%, but much larger (e.g. 30%). Imagine running a business when 30% of your decisions can be wrong.

The field of biostatistics realized this problem long ago. It's only natural that during the course of clinical trials, one might want to peek to check that patients aren't adversely affected, or that the treatment is obviously effective so it's no longer sensical to continue expensive testing. A recognized way to control for FPR inflation is to employ appropriate thresholds at each peek, so that the overall FPR is controled. The formal procedure uses $\alpha$-spending functions. One can think of each peek as a shopping trip - spend a little of your error I, i.e., $\alpha$, each time, but ensure that in total you never spend more than 5%. $\alpha$-spending functions control how much you spend over your peeks.

One can choose any $\alpha$-spending function to suit one's taste. It could be front-loaded to accommodate your instinct that the experiment should show significant treatment effect very soon, or back-loaded, for the opposite case. The tradition one-shot analysis is the extreme of being back-loaded: no spending at all until the very end.

Given a choice of $\alpha$-spending function, one can back out the appropriate thresholds that should be used for each peek. This computation can be rather involved, but one can try a simpler way: given a number of peeks, is there a constant threshold that can be used for every peek? Such a constant corresponds to the $\alpha$-spending function called *Pocock*. For 5 peeks, for example, the constant would be 2.41.

### References

[Alpha-spending functions - book chapter](https://lechipatrick.github.io/lemets-interim-analyses-alpha-spending.pdf)
[Alpha-spending functions - paper](https://lechipatrick.github.io/LanDeMetsPaper.pdf)
