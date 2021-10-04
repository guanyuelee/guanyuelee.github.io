---
title: "Discovering Density-Preserving Latent Space Walks in GANs for Semantic Image Transformations"
collection: publications
permalink: /publication/multimedia2021
excerpt: ''
date: 2021-6-1
venue: 'ACM Multimedia'
header:
    teaser: 'papers/acmmm2021/teaser.png'
excerpt:
    <p>A new traversing method that preserves the latent probability density; Excavate useful directions from the pretrained generator.</p>
    <p>Generative adversarial network (GAN)-based models possess superior capability of high-fidelity image synthesis. There are a wide range of semantically meaningful directions in the latent representation space of well-trained GANs, and the corresponding latent space walks are meaningful for semantic controllability in the synthesized images. To explore the underlying organization of a latent space, we propose an unsupervised Density-Preserving Latent Semantics Exploration model (DP-LaSE). The important latent directions are determined by maximizing the variations in intermediate features, while the correlation between the directions is minimized. Considering that latent codes are sampled from a prior distribution, we adopt a density-preserving regularization approach to ensure latent space walks are maintained in iso-density regions, since moving to a higher/lower density region tends to cause unexpected transformations. To further refine semantics-specific transformations, we perform subspace learning over intermediate feature channels, such that the transformations are limited to the most relevant subspaces. Extensive experiments on a variety of benchmark datasets demonstrate that DP-LaSE is able to discover interpretable latent space walks, and specific properties of synthesized images can thus be precisely controlled. </p>

---

When I was tackling traversing in the latent space of generator, I was haunted by the identity shift in which you cannot well preserve the identity of the synthesized face images. Then I try to dig into this phenomenon. 

Problem Description
======

Our Ideas
======

Experiments
======
We demonstrate some directions in BigGAN and StyleGAN. For dog in BigGAN, we mainly found some directions that cause position change, like left-right, zoom in-out and up-down. For human face in StyleGAN, various directions are found. If the intermediate layer we set to optimize the maximum variation is close to the input, the attribution we found are age, gender, etc. Otherwise, attributes like glasses and bear can be found. 
<table>
    <tr>
        <td>
            <center>
            <img src="../images/papers/acmmm2021/dog-left-right.gif" width=300>
            <br>
            <div style="color:orange;
            display: inline-block;
            color: black;
            padding: 2px;">
            (a) Left-right
            </div>
            </center>
        </td>
        <td>
            <center>
            <img src="../images/papers/acmmm2021/dog_zoom.gif" width=300>
            <br>
            <div style="color:orange;
            display: inline-block;
            color: black;
            padding: 2px;">
            (b) Zoom in - out
            </div>
            </center>
        </td>
        <td>
            <center>
            <img src="../images/papers/acmmm2021/dog_up_down.gif" width=300>
            <br>
            <div style="color:orange;
            display: inline-block;
            color: black;
            padding: 2px;">
            (c) Up - down
            </div>
            </center>
        </td>
    </tr>
</table>
<center>
<div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure x. Some of the directions in BigGAN. 
    </div>
</center>


Reference 
=====

[Download paper here](https://1drv.ms/b/s!AqN-jN9xngyFohsv5_4BvANJPMcG)