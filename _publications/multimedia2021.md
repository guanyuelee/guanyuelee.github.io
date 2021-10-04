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
Traversing in pre-trained generator has great potential in real life applications, such as image editing, attribute manipulation, and so on. Given a datapoint $z$ in the latent space and a traversing direction $d$, we can change the datapoint $z$ along the direction $d$ with a stride $\epsilon$ as follows: 

$$z_m = z + \epsilon d. \tag{1}$$

if the resulting datapoint reflects some attributes, for example, adding glasses to a face, change the length of the hair, etc, then we can say the direction is meaningful. How to find these directions remains open problems. 

In our model DP-LaSE, we hypothesize that meaningful directions results in maximum variations in the intermediate layer. Let me put it in this way: Imagine a extreme situation where a direction that results in no changes in the intermediate layer, and then this direction cannot be meaningful direction because it's naive solution. Some works such as [$MCR^2$](https://book-wright-ma.github.io/) proposed that dataset can be spaned by several independent subspace that maximizes the coding rate. We believe that meaningful direction should results in largest variation so that the directions can expressed the dataset efficiently. 

Secondly, once the directions are found, it remains unsolved in how to traverse in the latent sapce. Many research such [SeFA]() and [UDUI]() traverses latent space by linear interpolation. But there is a issue of this --- large stride causes interpolation path to diverge from the data manifold. As is shown in Figure 1, increasing stride generally leads the interpolation path to diverge from the data manifold. 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/acmmm2021/teaser.png" width=500>
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 1. Large stride cause the traverse path diverge from the data manifold.  
    </div>
</center>

A datapoint that is out of data manifold usually abnormal, because it's hardly trained in the training phrase since it's in low probability density area. On the contrary, a datapoint in the high probability density area are frequently trained, so the generation by datapoints there interweaves many attributes even by a small disturbance. We validate the hypothesis in the BigGAN and WGAN, and the results are shown in Figure 2. The pattern can be summarized as follows: 

+ Moving to higher probability density can cause dramatic change of the image, such as label and background.
+ Moving to lower probability density would fade out. 
+ Moving in the region of same probability density can results in reasonable changes. 

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="../images/papers/acmmm2021/figure2.png" width=500>
    <br>
    <div style="color:orange;
    display: inline-block;
    color: black;
    padding: 2px;">
    Figure 2. Comparison of path that moves to higher (top), lower (bottom) and same density regions (middle).  
    </div>
</center>

Our Implementation
======
Let's dive deeper on the implementation. 

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

<table>
    <tr>
        <td>
            <center>
            <img src="../images/papers/acmmm2021/bear.gif" width=300>
            <br>
            <div style="color:orange;
            display: inline-block;
            color: black;
            padding: 2px;">
            (a) Bear
            </div>
            </center>
        </td>
        <td>
            <center>
            <img src="../images/papers/acmmm2021/hair.gif" width=300>
            <br>
            <div style="color:orange;
            display: inline-block;
            color: black;
            padding: 2px;">
            (b) Hair
            </div>
            </center>
        </td>
        <td>
            <center>
            <img src="../images/papers/acmmm2021/gender.gif" width=300>
            <br>
            <div style="color:orange;
            display: inline-block;
            color: black;
            padding: 2px;">
            (d) Gender
            </div>
            </center>
        </td>
        <td>
            <center>
            <img src="../images/papers/acmmm2021/glass.gif" width=300>
            <br>
            <div style="color:orange;
            display: inline-block;
            color: black;
            padding: 2px;">
            (c) Glasses
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
    Figure x. Some of the directions in PGGAN. 
    </div>
</center>

Reference 
=====

[Download paper here](https://1drv.ms/b/s!AqN-jN9xngyFohsv5_4BvANJPMcG)