---
layout: post
title:  "Exploring to the TadGAN-Model structure"
date:   2021-10-07 09:24:19 +0300
image:  tadgan.png
tags:   Tadgan,review
---

지금까지 우리는 Tadgan이 어떤 모델이고, 어떤 loss function을 쓰는 지 알아봤습니다. 하지만 우리는 가장 중요한 걸 모르고 있습니다. 바로 모델 그 자체가 어떻게 작동하는지 입니다. 
논문에는 tadgan모델 구조를 다음과 같이 나타냈습니다.

![]({{ site.baseurl }}/images/tadganex.png)

모델 설명을 읽어보시면, 두 개의 Generator, 두개의 Critic 함수로 이루어졌다는 것을 확인할 수 있습니다. 
Generator E는 time series sequence를 latent space로 바꿔주는 역할을 하고, Generator G는 latent space를 다시 time series sequence로 변환해주는
역할을 합니다. 여기까지는 기존의 AE와 별반 다를 것이 없어 보입니다. 


![]({{ site.baseurl }}/images/tadgan0.png)
![]({{ site.baseurl }}/images/tadgan1.png)
![]({{ site.baseurl }}/images/tadgan2.png)
![]({{ site.baseurl }}/images/tadgan3.png)
![]({{ site.baseurl }}/images/tadgan4.png)


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
