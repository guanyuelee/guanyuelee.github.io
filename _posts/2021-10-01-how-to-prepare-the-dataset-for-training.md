---
title: 'Prepare the training dataset for PT-CS-Translator'
date: 2021-10-21
header: 
    teaser: "../images/posts/how-to-prepare-the-dataset-for-training/teaser.png"
excerpt:
    <p>In this blog, I will introduce how I prepare a website for collecting audio datasets from Chaoshanese. </p>
    <p>如果你会说潮汕话，请帮助我翻译训练数据集。 By the way, if you can speak ChaoshanHua, help us by providing your translation. </p>
---

How to prepare dataset for training the model? 
=====

This blog will introduce how I prepare a website for collecting audio datasets from Chaoshanese. 

About the dataset
===========
I chose a Chinese Audio Speech Recognition (ASR) dataset [ST-CMDS](https://link.ailemon.net/?target=http://www.openslr.org/resources/38/ST-CMDS-20170001_1-OS.tar.gz) for my convenience. The dataset contains approximately 100 thousand sentences and the associate Chinese translations. Therefore, I can use the sentences to prompt users to upload their Chaoshanese translations. 

About the website
========
The website is based on [gunicorn](https://gunicorn.org/) for it's developed in Python environment. Gunicorn enables simple implementation, light server dependencies and fairly speediness. 

About the User Interface Design
=======
I use the [Bulma](https://bulma.io/) as my CSS framework that renders beautiful and user-friendly UI design. Bulma is a free, open-source framework that provides ready-to-use frontend components that you can easily combine to build responsive web interfaces. 






