---
layout: post
title:  "Exploring to the TadGAN-intro"
date:   2021-09-12 16:04:19 +0300
image:  tadgan.png
tags:   Tadgan,review
---
Tadgan은 unsupervised machine learning으로, 원본 시계열 데이터로 부터 비슷한 시계열을 생성하게끔 모델을 학습하고, Theshold에 따라 이상치를 판단하는 이상치 탐지(Anomaly Detection) 방법중 하나 입니다. 많은 사람들이 검증되고, y label이 확실한 데이터를 이용해 supervised learning을 해야 모델에 대한 신뢰성을 확보할 수 있다고 생각합니다. 하지만 아쉽게도, 대부분의 산업 현장에서 y label을 확보하는 것은 많은 시간과 노력을 들여서 해야하기 때문에 대기업에서도 상당한 자본과 노력을 필요로 하고, 중소기업 및 스타트업은 더 많은 어려움을 겪을 것 입니다. 특히 주기가 굉장히 긴(몇년 단위) 데이터에서는 y label을 구하기가 거의 불가능에 가깝기 때문에, 그동안 전통적인 통계방법을 사용한 이상치 탐지는 굉장히 기초적인 수준을 넘지 못했습니다. 하지만 최근 머신러닝의 발달로 인해 우리는 조금 더 다른 방법을 사용할  수 있게 되었습니다. 

사실 우리가 이상치 탐지에서 떠올릴 수 있는 가장 빠른 방법은 PCA의 한 종류인 Auto Encoder 입니다. 특히 다변량으로 이루어진 데이터들의 성분을 분석할 때 성분들을 압축하고 재구성하는 과정에서 학습된 특징들과 보이는 차이를 측정해서 사용자가 정하는  theshold에 의해 이상치를 판단하게 됩니다. 대표적으로는 VAE(variational auto encoder)이 존재하는데, 추후에 이에 대해서도 리뷰를 진행할 것입니다.

 하지만 Tadgan은 GAN(Generative Adversarial Networks)중에서도 Cycle Consistency Loss을 사용해서 Auto Encoder와 비슷한 효과를 내는 것이 차별점 입니다. 즉, 시계열을 생성하고 이 시계열이 참고한 실제 데이터와 얼마나 비슷한 지를 확인하는 과정을 거칩니다. 이 과정을 통해서 우리는 조금 더 높은 성능을 자랑하는 오토 인코더 형태의 모델을 만들 수 있게 되었습니다.

![]({{ site.baseurl }}/images/cycleganx.png)
![]({{ site.baseurl }}/images/tadgan0.png)

 그래프를 보면 조금 더 쉽게 비교가 가능합니다. 처음 볼 수 있는 그림은 cyclegan의 원본을 시계열 데이터에 맞춰서 변경한 것입니다. 원본 시계열과 재생성된 시계열을 검사하는 {C_x}가 추가된 것 말고는 굉장히 흡사하게 보입니다. 
Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
