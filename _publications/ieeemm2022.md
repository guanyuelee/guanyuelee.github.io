---
title: "Adversarial Adaptive Interpolation in Autoencoders for Dually Regularizing Representation Learning"
collection: publications
permalink: /publication/ieeemm2022
excerpt: ''
date: 2022-1-1
venue: 'IEEE MultiMedia'
header:
    teaser: 'papers/ieeemm2022/teaser.png'
excerpt:
    <p>A new interpolation method that follows the data manifold. </p>
    <p>Data interpolation is typically used to explore and understand the latent representation learnt by a deep network. Naive linear interpolation may induce mismatch between the interpolated data and the underlying manifold of the original data. In this paper, we propose an Adversarial Adaptive Interpolation (AdvAI) approach for facilitating representation learning and image synthesis in autoencoders. To determine an interpolation path that stays on the manifold, we incorprate an interpolation correction module, which learns to offset the deviation from the manifold. Further, we perform matching with a prior distribution to control the characteristics of the representation. The data synthesized from random codes along with interpolation-based regularization are in turn used to constrain the representation learning process. In the experiments, the superior performance of the proposed approach demonstrates the effectiveness of AdvAI and associated regularizers in a variety of downstream tasks.</p>
    
---
[[Download]](https://ieeexplore.ieee.org/abstract/document/9706300), [[Github]](https://github.com/guanyuelee/AdvAI)

----

If you are interested in the followings: 
- How to interpolate the latent space of GAN by interpolation?
- Most of researchers neglect distribution mismatch in the latent space.  

This article is suitable to you. Enjoy it!

Interpolation in GAN
======
There are plenty of ways to interprete the latent space of GAN, like [GAN inversion](https://guanyueli.com/publication/aaai2021), interpolation. In this paper, we are going to talk about interpolation and examine its properties. 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/ieeemm2022/interpolation figure.png" width=400>
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 1. An interpolation diagram. Figure adapted from [1]. 
    </div>
</center>

GAN is capable of learning a complex distribution of real dataset. It's believed that if GAN learns the data manifold of real dataset, it learns semantics from the dataset instead of memorizing the dataset. Formally speaking, given a pre-trained generator $G$ and two randomly sampled noise $z_1$ and $z_2$, we can use interpolation to get the middle datapoint $z_\alpha=f(z_1, z_2, \alpha)$ by interpolation method $f$. There exist plenty of interpolation methods, and I will list them as follows: 
- Most commom interpolation --- linear interpolation: 
$$f(z_1, z_2, \alpha) = \alpha z_1 + (1-\alpha) z_2.$$
- Spherical interpolation: 
$$f(z_1, z_2, \alpha) = \frac{\sin((1-\alpha)\Theta)}{sin(\Theta)} z_1 + \frac{\sin(\alpha\Theta)}{sin(\Theta)} z_2.$$

You can use interpolation to generate a series of images by varying interpolation coefficient $\alpha$. For example, Figure 1 shows the interpolation path using [PGGAN](). 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/ieeemm2022/interpolation.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 2. An interpolation path in PGGAN. 
    </div>
</center>

Distribution Mismatch 
======
A phenomenon that is frequently neglected by researchers is distribution mismatch. Let's look at an example in Figure 3. 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/ieeemm2022/mismatch.png" width=400>
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 3. Distribution mismatch example. 
    </div>
</center>

In this diagram, we use uniform and Gaussian distribution as examples. We plot the resulting distribution after applying interpolation on the original distribution. As is shown in Figure 3, the resulting interpolated distributions are obviously different to original ones for both uniform and Gaussian distribution, which we call it **distribution mismatch**.

AdvAI
======
Our method adopts **adversarial training** into interpolation to cope with distribution mismatch, since adversarial training is a good way to estimate the original distribution. 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/ieeemm2022/advai.png" width=600>
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 4. A diagram of AdvAI. 
    </div>
</center>

Since the interpolation path is incorrect, or say, out of distribution, we propose to correct the interpolation path by adding a correction bias. We correct the simplest interpolation --- linear interpolation by a learnable bias $R(z_1, z_2, \alpha)$, and we formulate the our interpolation AdvAI as follows: 

$$f(z_1, z_2, \alpha) = \alpha z_1 + (1-\alpha) z_2 + \alpha (1 - \alpha) R(z_1, z_2, \alpha). $$

We use $\alpha(1-\alpha)$ to ensure that both two endpoints are $z_1$ and $z_2$. The next problem is how to determine the correction bias. We adopt a distribution disriminator $D_R$ into our model. The procedure is similar to generative adversarial networks: the discriminator is used to distinguish the original datapoints from the interpolated datapoints, while the correction module learns to fool the discriminator that the interpolated ones are from original distribution. Formally, $D_R$ is trained to distinguish $z_\alpha$ from by estimating the coefficients $\alpha$: 

$$\min_{\theta_{D_R}} \frac{1}{|X_\alpha|}\sum_{x_\alpha \in X_\alpha}\|D_R(x_\alpha)-\alpha\|^2 + \frac{1}{|X|}\sum_{x \in X}\|D_R(x)\|^2. $$

And correction module $R$ aims to deceive $D_R$, and $D_R(z_\alpha)$ is thus expected to be 0: 

$$\min_{\theta_{R}} \frac{1}{|X_\alpha|}\sum_{x_\alpha \in X_\alpha}\|D_R(x_\alpha)\|^2. $$

After appropriate training, we show the experiment results of AdvAI in Figure 5. you can see that AdvAI can greatly reduce distribution mismatch. 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/ieeemm2022/advai_toyexample.png" width=600>
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 5. Results of AdvAI on uniform(top left) and Gaussian(bottom left) distributin. Comparison among linear, spherical and AdvAI on more complicated distribution. 
    </div>
</center>

AdvAI can be used to interprete latent space of GAN with priority over other pre-defined interpolation methods. Let me explain it in this way: We train a generator by sampling noise from a prior distribution, so it's natural that we should inspect interpolation path also follows the prior distribution. In the past, most interpolation methods are out of data manifold due to distribution mismatch. Figure 6 shows that AdvAI results in shape interpolation path compared with linear interpolation. 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/ieeemm2022/exp_interpolation.png">
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 6. Interpolation results of linear interpolation (top) and AdvAI (bottom) on WGAN. 
    </div>
</center>

For more analysis on:
- How to use AdvAI on representation learning? 
- How they can improve generation quality? 

Please refer to [our paper](https://ieeexplore.ieee.org/abstract/document/9706300) for more information. 
 

Reference
======
[1] White, T. (2016). Sampling generative networks. arXiv preprint arXiv:1609.04468.

Recommended citation: G. Li, X. Wei, S. Wu, Z. Yu, S. Qian and H. S. Wong, "Adversarial Adaptive Interpolation in Autoencoders for Dually Regularizing Representation Learning," in IEEE MultiMedia, doi: 10.1109/MMUL.2022.3148991.