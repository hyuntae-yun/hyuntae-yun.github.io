---
layout: post
title:  "Exploring to the TadGAN-background"
date:   2021-09-12 16:04:19 +0300
image:  tadgan.png
tags:   Tadgan,review
--- 

바로 전 포스트에서는 TadGAN이 무엇이며, TadGAN은 어떤 모델인지에 대해서 알아봤습니다. 지금부터는 조금 더 TadGAN에 대한 이론적인 부분을 알아보겠습니다.

TadGAN은 loss function을 굉장히 중요하게 생각했습니다. 기존의 loss function(https://lifeignite.tistory.com/57 참조)에 대한 한계가 존재하기 때문입니다. 소개해드린 블로그 글은
GAN의 기본적인 loss function으로 부터 어떻게 KL divergence , JSD divergence를 유도하는지를 잘 나타냈는데, 그래서 그렇게 유도한 식이 왜 문제가 되는지는 다음 그래프를 통해 설명드리겠습니다.

![formula](https://render.githubusercontent.com/render/math?math=KL(P \parallel Q)= \sum_xP(x)log\frac{P(x)}{Q(x)})

![formula](https://render.githubusercontent.com/render/math?math=\begin{aligned}\operatorname{JSD}(P \| Q)=\frac{1}{2} KL(P \| M) &+\frac{1}{2} KL(Q \| M) \\M &=\frac{1}{2}(P+Q)\end{aligned})

![]({{ site.baseurl }}/images/wgan.png)
(출처: https://www.codetd.com/en/article/12856575)



### Wasserstein distance에 대해 알아보자






![]({{ site.baseurl }}/images/wgan_graph.png)





[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
