---
layout: post
title:  "Exploring to the TadGAN-background"
date:   2021-09-12 16:04:19 +0300
image:  tadgan.png
tags:   Tadgan,review
--- 

바로 전 포스트에서는 TadGAN이 무엇이며, TadGAN은 어떤 모델인지에 대해서 알아봤습니다. 지금부터는 조금 더 TadGAN에 대한 이론적인 부분을 알아보겠습니다.

TadGAN은 loss function을 굉장히 중요하게 생각했습니다. 기존의 loss function(https://lifeignite.tistory.com/57 참조)에 대한 한계가 존재하기 때문입니다. 소개해드린 블로그 글은
GAN의 기본적인 loss function으로 부터 어떻게 KL divergence , JSD divergence를 유도하는지를 잘 나타냈는데 두 공식을 표현하자면 아래와 같습니다.

![formula](https://render.githubusercontent.com/render/math?math=KL(P \parallel Q)= \sum_xP(x)log\frac{P(x)}{Q(x)})

![formula](https://render.githubusercontent.com/render/math?math=A %2B B))

![formula](https://render.githubusercontent.com/render/math?math={JSD}(P \| Q)={{\frac{1}{2} KL(P \| M)} %2B {\frac{1}{2} KL(Q \| M)}} , M=\frac{1}{2}(P %2B Q))

![]({{ site.baseurl }}/images/wgan_graph.png)

 KL,JSD를 이용해서 ![formula](https://render.githubusercontent.com/render/math?math=f(x))로 표현된 분포를 ![formula](https://render.githubusercontent.com/render/math?math=g(x))의 분포가 되도록 학습을 시켜야 하는데 그러기 위해서는 왼쪽 부분은 왼쪽 부분 끼리, 오른쪽 부분은 오른쪽 부분끼리 같아지도록 만들어줘야 합니다. 문제는 두 분포에 대해서 x,T(x) 값이 달라져 버리면 KL,JSD는 같은 기준값을 가진 확률들만 계산하도록 고안된 확률거리 함수라서 기준값이 달라져 버리면 항상 무한대의 값, 혹은 일정한 값만 나오게 되면서 학습을 전혀 할 수 없게 됩니다. 그래서 TadGAN논문은 새로운 loss function을 차용하게 되었는데, 그게 바로 wgan-gp 입니다. wgan-gp는 Wasserstein GAN + gradient panelty를 합친 용어인데 먼저 이 loss function을 알려면 Wasserstein distance가 뭔지부터 알아야 할 것입니다.

 

### Wasserstein distance에 대해 알아보자

Wasserstein distance는 무엇일까?  이걸 보기 전에, 먼저 우리가 알아야할 개념이 있습니다. 저희에게 있어서 가장 친숙한 거리재는 개념은 유클리드 거리입니다. 흔히 지도상에서 위치를 특정해서 얼마나 멀리 떨어져있는지를 직선거리로 나타낸 것이 유클리드 거리입니다. 우리는 흔히 norm2로 알고 있습니다. 하지만 특정 값이 아니라 확률에 대해서 거리를 계산하고자 한다면 우리는 다른 방법을 취할 수 밖에 없습니다. 하지만 전통적인 확률거리 계산 방법들은 같은 값에 대해서 얼마의 확률에 대한 차이가 있는 지를 확인하기 때문에 다른 값에 대해서 얼마만큼의 확률이 차이가 나는지를 계산하게 되면 아래와 같은 값만을 계속해서 나타내게 됩니다.

![]({{ site.baseurl }}/images/wgan.png)
(출처: https://www.codetd.com/en/article/12856575)

하지만 Wasserstein distance는 좀 다릅니다.










[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
