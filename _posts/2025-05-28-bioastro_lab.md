---
title: Deep Learning for Biosignal Processing
subtitle: Examining failure modes in my past work at a bioastronautics lab
layout: default
date: 2025-05-28
keywords: deep learning, signal processing, identifiability analysis
published: true
---




Over the 2025 spring semester, I had the opportunity to intern at a CU Boulder Bioastronautics lab, where I assisted a master's thesis candidate.  
Their thesis was on cognitive workload classification using fNIRS-measured data, for which I explored deep learning approaches.

## The Deep Learning Pitfall

The student I was assisting had calculated statistical moments of the data, and trained and evaluated a support vector machine (SVM) on subsets of extracted features.  
In a 4-class classification task for multiple subjects, features achieving above 30% accuracy were sparse and inconsistent across subjects.

<div class="figure">
  <img src="../../../../../image/bioastro/cand-features.png" alt="Candidate features achieving greater than 30% accuracy" style="width: 100%; display: block; margin: 0 auto;" />
  <div class="caption">
    <span class="caption-label">Figure 1.</span>
    Candidate features achieving &gt;30% accuracy in cognitive workload classification
  </div>
</div>

A literature review showed that convolutional neural networks (CNNs) were commonly used to process fNIRS data. CNNs can encode features by computing weighted sums over a sliding window—weights that are iteratively updated with gradient descent with respect to a loss function.  
The convolutions act as a filter, only passing on parts of the signal to successive convolutional layers. In this way, each layer produces a denser representation of the signal. Depending on which dimension the convolution is applied to, signal data is aggregated across time or channels.

<div class="figure">
  <img src="../../../../../image/bioastro/1d-conv.png" alt="Example of 1-D convolution" style="width: 100%; display: block; margin: 0 auto;" />
  <div class="caption">
    <span class="caption-label">Figure 2.</span>
    An example of 1-D convolutions applied to sequences of characters  
    (<a href="https://arxiv.org/pdf/2402.15490v2">source</a>)
  </div>
</div>

For certain problems, this can be a very useful abstraction: instead of making a decision over which statistical moments to calculate or which sensor channels to use, the problem is reduced to optimizing a model’s performance as a function of its parameters.  
Many small models can be trained and evaluated quickly, and the parameter space rapidly explored. The efficacy of the model’s learned representations for the task should then be reflected in its performance.

Training a CNN on the fNIRS data, however, yielded performance that was little better than chance. Notably, the best-performing models achieved zero training loss with the cross-entropy loss function, which is not a good sign of generalization.

<div class="figure">
  <img src="../../../../../image/bioastro/cross-entropy.png" alt="Cross-entropy loss function" style="width: 100%; display: block; margin: 0 auto;" />
  <div class="caption">
    <span class="caption-label">Figure 3.</span>
    The cross-entropy loss function, where <em>w</em> : class weights, <em>x</em> : logits, <em>y</em> : class labels
  </div>
</div>

For a classification model to achieve zero loss via the unweighted cross-entropy loss, its output probability distribution must collapse to a single value.  
This is a catastrophic failure: it indicates that the model is not processing the signal in any meaningful way. Such a low training loss also implies that the latent state, observed through the hemodynamic response, is occluded by structured noise that the model is fitting to.

This meager insight is about the limit of what a complete black-box model will provide. Such a model offers no tooling to reason about the problem and no insight into how to improve performance.

## Identifiability Analysis

The CNN failing to filter meaningful information from the data is not surprising. The fNIRS data has a high signal-to-noise ratio, and the latent state is only indirectly observed through the hemodynamic response. Other signals, biological in origin or not, contaminate the observed data as well.

<div class="figure">
  <img src="../../../../../image/bioastro/noise.png" alt="Conceptual model of noise in fNIRS" style="width: 100%; display: block; margin: 0 auto;" />
  <div class="caption">
    <span class="caption-label">Figure 4.</span>
    A conceptual model for recovering the hemodynamic response amid other physiological signals
    (<a href="https://pubs.aip.org/aip/rsi/article-abstract/84/7/073106/360588/Noise-reduction-in-functional-near-infrared?redirectedFrom=fulltext">Article</a>)
  </div>
</div>

In the end, deep learning proved unsuitable for the task—a result I could have reached without extensive empirical testing.  
In this article, I revisit biosignal data to explore when and why models fail to recover latent signals from noisy observations. I study when and why latent signals can or cannot be recovered by systematically varying identifiability, noise structure, and model misspecification, and by quantifying bias–variance–stability tradeoffs.
