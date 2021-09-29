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

![math (1)](https://user-images.githubusercontent.com/70379885/134938120-f130edd9-08aa-45e9-a266-dc2488ea42ec.png)

![]({{ site.baseurl }}/images/wgan_graph.png)

 KL,JSD를 이용해서 ![formula](https://render.githubusercontent.com/render/math?math=f(x))로 표현된 분포를 ![formula](https://render.githubusercontent.com/render/math?math=g(x))의 분포가 되도록 학습을 시켜야 하는데 그러기 위해서는 왼쪽 부분은 왼쪽 부분 끼리, 오른쪽 부분은 오른쪽 부분끼리 같아지도록 만들어줘야 합니다. 문제는 두 분포에 대해서 x,T(x) 값이 달라져 버리면 KL,JSD는 같은 기준값을 가진 확률들만 계산하도록 고안된 확률거리 함수라서 기준값이 달라져 버리면 항상 무한대의 값, 혹은 일정한 값만 나오게 되면서 학습을 전혀 할 수 없게 됩니다. 그래서 TadGAN논문은 새로운 loss function을 차용하게 되었는데, 그게 바로 wgan-gp 입니다. wgan-gp는 Wasserstein GAN + gradient panelty를 합친 용어인데 먼저 이 loss function을 알려면 Wasserstein distance가 뭔지부터 알아야 할 것입니다.

 

### Wasserstein distance에 대해 알아보자

Wasserstein distance는 무엇일까?  이걸 보기 전에, 먼저 우리가 알아야할 개념이 있습니다. 저희에게 있어서 가장 친숙한 거리재는 개념은 유클리드 거리입니다. 흔히 지도상에서 위치를 특정해서 얼마나 멀리 떨어져있는지를 직선거리로 나타낸 것이 유클리드 거리입니다. 우리는 흔히 norm2로 알고 있습니다. 하지만 특정 값이 아니라 확률에 대해서 거리를 계산하고자 한다면 우리는 다른 방법을 취할 수 밖에 없습니다. 

하지만 위에서 저희가 확인한, 그러니까 KL divergence나 JSD divergence 같은  전통적인 확률거리 계산 방법들은 같은 값에 대해서 얼마의 확률에 대한 차이가 있는 지를 확인하기 때문에 다른 값에 대해서 얼마만큼의 확률이 차이가 나는지를 계산하게 되면 아래와 같은 값만을 계속해서 나타내게 됩니다.

![]({{ site.baseurl }}/images/wgan.png)
(출처: https://www.codetd.com/en/article/12856575)

하지만 Wasserstein distance는 좀 다릅니다. Wasserstein distance는 다른 말로 EM(Eearth Mover) distance라고도 하는데, 이 확률 거리 측정 방법은 KL과 JSD와는 확실히 다릅니다. KL과 JSD가 다른 값에 대해서 서로 같은 확률을 가질 때 무한 대의 값이나 일정값 밖에 주지 않는 반면, Wasserstein distance는 다른 값에 대한 거리 정보까지 담고 있습니다. 
 아까부터 자꾸 같은 값, 다른 값이라고 하는데 이게 도대체 뭐야? 라고 생각하실 분들이 분명히 계실 것 같습니다. 여기서 값은 두 집합 X,Y에서 원소를 각각 뽑았을 때 등장하는 원소의 값과 동일합니다. 예를 들어서 X=[2,2,2,2,3,4,4,5], Y=[3,3,4,4,6,6,6,6]이 존재한다고 했을 때 X가 2를 뽑을 확률은 1/2이고, Y가 6을 뽑을 확률도 역시 1/2 입니다. 그리고 X가 2를 뽑을 확률과 Y가 6을 뽑을 확률에 대한 거리를 계산한다고 했을 때, 두 경우에 대해서 확률이 동일하기 때문에 위에서 저희가 이미 구한 것 처럼 KL은 inf, JSD는 log2 가 나오지만 Wasserstein distance는 다음과 같이 구합니다.
 
 ![formula](https://render.githubusercontent.com/render/math?math=\sqrt{(6-2)^2}%2B\sqrt{(\frac{1}{2}-\frac{1}{2})^2}=4)

즉 확률은 같아도 2를 1/2로 뽑을 확률과 6을 1/2로 뽑을 확률은 정확히 4 만큼의 거리를 가진다는 것을 알 수 있습니다. 지금까지 우리는 wasserstein distance를 어떻게 구하는 지에 대해 알아봤습니다. 하지만 이 것은 X(2) -> Y(6) 로 갈 때만의 이야기를 한 것입니다. 우리가 알고 싶은 건 X와 Y에 대한 확률 거리가 얼마나 떨어져 있는지를 어떻게 하면 정량적으로 나타낼 수 있는가? 입니다.

![XY](https://user-images.githubusercontent.com/70379885/135189137-e721db84-0e7d-4c75-b0d7-777cd4d9b062.png)


X,Y에 대해서 각 원소들이 뽑힐 확률이 uniform(모두다 균일)이라고 가정할 때, 각 원소들이 뽑힐 확률은 모두 1/n으로 동일 합니다. 그래서 각 원소들이 뽑힐 확률은 Y축에 표시되어 있고, 그 원소의 값들은 X축에 표시되어 있습니다. 그리고 우리는 이제 X를  Y처럼 만들면 되는데, 반드는 방식은 블록 쌓기와 같습니다. 이 블록은 모든 블록의 크기를 더하면 1이 되는 블록입니다. 하지만 이렇게 되면 경우의 수가 너무 많이 발생합니다. 예를 들면 X(2)를 X(6)에 전부 다 옮겨 Y(6) 처럼 만들어주는 방법이 있습니다. 그것도 아니라면 X(3),X(4),X(5)를 X(6)으로 모두 옮긴 다음, X(2)를 잘게 쪼개서 X(3),X(4),X(5)를 채워주는 방법도 있습니다. 이렇게 여러가지의 방법들이 존재하기 때문에 Wasserstein distance에는 제약 조건이 있습니다. 어떻게 옮기든 상관은 없는데, 그 중 최소비용이 드는 방법을 두 함수의 거리로 생각하겠다는 것입니다. 비용 측정은 다음과 같이 합니다.

![image](https://user-images.githubusercontent.com/70379885/135199922-11e9c2d4-1eaa-40d4-9482-1468c7aac914.png)

이 계산식은 EM distance, Wasserstein-1 이라고 불리는 거리계산 방법입니다. 

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
