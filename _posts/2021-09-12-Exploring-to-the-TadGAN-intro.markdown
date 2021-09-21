---
layout: post
title:  "Exploring to the TadGAN-intro"
date:   2021-09-12 16:04:19 +0300
image:  tadgan.png
tags:   Tadgan,review
---
Tadgan은 unsupervised machine learning으로, 원본 시계열 데이터로 부터 비슷한 시계열을 생성하게끔 모델을 학습하고, Theshold에 따라 이상치를 판단하는 이상치 탐지(Anomaly Detection) 방법중 하나 입니다. 많은 사람들이 검증되고, y label이 확실한 데이터를 이용해 supervised learning을 해야 모델에 대한 신뢰성을 확보할 수 있다고 생각합니다. 하지만 아쉽게도, 대부분의 산업 현장에서 y label을 확보하는 것은 많은 시간과 노력을 들여서 해야하기 때문에 대기업에서도 상당한 자본과 노력을 필요로 하고, 중소기업 및 스타트업은 더 많은 어려움을 겪을 것 입니다. 특히 주기가 굉장히 긴(몇년 단위) 데이터에서는 y label을 구하기가 거의 불가능에 가깝기 때문에, 그동안 전통적인 통계방법을 사용한 이상치 탐지는 굉장히 기초적인 수준을 넘지 못했습니다. 하지만 최근 머신러닝의 발달로 인해 우리는 조금 더 다른 방법을 사용할  수 있게 되었습니다. 

사실 우리가 이상치 탐지에서 떠올릴 수 있는 가장 빠른 방법은 PCA의 한 종류인 Auto Encoder 입니다. 특히 다변량으로 이루어진 데이터들의 성분을 분석할 때 성분들을 압축하고 재구성하는 과정에서 학습된 특징들과 보이는 차이를 측정해서 사용자가 정하는  theshold에 의해 이상치를 판단하게 됩니다. 대표적으로는 VAE(variational auto encoder)이 존재하는데, 추후에 이에 대해서도 리뷰를 진행할 것입니다.
 
 사실 이상치 탐지를 하는 방법은 굉장히 여러가지가 존재 하지만, 저는 그 중에서도 논문에서 소개한 세가지 방법을 먼저 소개 하려고 합니다.
 
 ###1. Proximity-based methods
 ![]({{ site.baseurl }}/images/Proximity.jpg)
 가장 먼저, 근접도 기반 방법입니다. 우리가 주로 알고 있는 clutering이 대표적인데, 각 데이터 요소들간의 거리를 계산하고, 이를 바탕으로 근접도를 판단을 합니다. 근접도가 커지면 커질 수록 멀리 있다는 뜻이기 때문에 여러가지 제약조건을 통해서 outlier을 결정할 수 있습니다. 


###2. Predict-based methods
 ![]({{ site.baseurl }}/images/predict.jpg)
 예측 기반 방법입니다. ARIMA가 가장 대표적인데요, 시점  t-1까지의 데이터를 활용해서 시점 t를 예측합니다. 그리고 실제 값이 예측값과 얼마나 다른지를 보고 이상치인지 아닌지를 판별합니다. 세가지 방법중에서는 난이도가 가장 높은 편이라고 할 수 있습니다.
 
 
###3. Reconstuction-based methods
 ![]({{ site.baseurl }}/images/reconstruction.png)
 재생성 기반 방법입니다. Auto Encoder이 가장 대표적입니다. 여러개의 특징(다변량 혹은 시계열데이터)을 Encoder을 통해 latent space로 압축시키고, 이후 Decoder을 통과해 원래 데이터로 복원하게 되는데, 이때 Auto Encoder은 그 데이터의 특징을 학습한 상태임으로 이상치가 포함된 데이터라면 복원이 잘 되어있지 않을 것입니다. 그래서 복원된 데이터와 원본 데이터의 차이를 비교해서 이상치를 추정하게 됩니다.

 위의 방법중에서 TadGAN은 마지막 방법인 Reconsturction-based methods를 통해 이상치를 판별합니다. 조금 더 나아가서 TadGAN은 이런 reconsturction method를 CycleGAN에 사용된 기법을 이용해 진행하는 모델입니다. 먼저 CycleGAN에 대해서 이야기해 보자면, CycleGAN은 짝지어진 데이터가 굉장히 다르더라도 유사점을 찾아내 비슷한 형태로 나타낼 수 있습니다. 이미지로 예를 들면, 말이 초원을 달리는 모습이 담긴 사진과 얼룩말이 서있는 모습이 담긴 사진은 학습하기가 불가능 합니다. 얼룩말과 말의 주변환경도 굉장히 다르고, 동일한 특성은 오로지 '말의 특징을 가진 오브젝트' 밖에 없습니다. cycleGAN은 우선 각 데이터가 가진 공통된 특징을 잘 찾아내고, 거기에 더 나아가 원본데이터의 특성을 유지시키는 방향으로 학습을 진행시킵니다. 

![]({{ site.baseurl }}/images/cycleganx.png)
![]({{ site.baseurl }}/images/tadgan0.png)

 그래프를 보면 조금 더 쉽게 비교가 가능합니다. 처음 볼 수 있는 그림은 cyclegan의 원본을 시계열 데이터에 맞춰서 변경한 것입니다. 원본 시계열과 재생성된 시계열을 검사하는 ![formula](https://render.githubusercontent.com/render/math?math=C_x)가 추가된 것 말고는 굉장히 흡사하게 보입니다. 
 
 
 
Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
