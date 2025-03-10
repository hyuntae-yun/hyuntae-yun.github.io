---
layout: post
title:  "Exploring to the TadGAN-loss function"
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

KL,JSD의 문제점을 다시 한번 말하자면, 어떤 확률 집합, 혹은 확률함수등이 주어질 때 항상 같은 값에서만 비교가 가능하다는 것입니다. 만약에 다른 값에 대해서 얼마나 거리가 떨어져 있는 지를 확인한다면, KL과 JSD는 아래의 값만 계속 전달하게 됩니다.

![]({{ site.baseurl }}/images/wgan.png)
(출처: https://www.codetd.com/en/article/12856575)

하지만 우리가 대부분의 무작위로 주어지는 확률 집합, 혹은 확률 함수들은 특수한 경우(값의 범위가 정해져 있는 경우)를 제외하고서는 분포가 같을 경우가 거의 없을 것 입니다. 그리고 값의 범위가 정해져 있다면 애초에 관심도 별로 없을 것 입니다. 값이 조금 헷갈린다면 이미지로 대체해서 생각해봐도 좋을 것 입니다.  우리는 사과를 다른 모양의 사과로 변환시키는 것(값의 분포가 같은 경우)보다는 사과를 오렌지, 혹은 배등으로 변환시키는 것(값의 분포가 다를 경우)에 더 관심이 있을 것 입니다. 

 우리는 다른 분포를 가진 확률함수에 대해 KL과 JSD가 전혀 쓸데없는 값을 나타낸 다는 것을 알아보았습니다. 그래서 다른 분포를 가지는 확률 함수들에 대해서 값을 비교하기 위해  Wasserstein distance를 채택하게 됩니다. Wasserstein distance는 다른 말로 EM(Eearth Mover) distance라고도 하는데, 쉽게 말해 유클리드 거리라고 생각하시면 됩니다. 이 확률 거리 측정 방법은 KL과 JSD와는 확실히 다릅니다. KL과 JSD가 다른 값에 대해서 서로 같은 확률을 가질 때 무한 대의 값이나 일정값 밖에 주지 않는 반면, Wasserstein distance는 다른 분포에 대한 거리 정보까지 담고 있습니다. 
 
 조금 더 쉽게 설명해 보겠습니다. 예를 들어서 X=[2,2,2,2,3,4,4,5], Y=[3,3,4,4,6,6,6,6]이 존재한다고 하겠습니다. 이 때 X와 Y가 각각 원소를 뽑게 될 때 두 집합의 확률이 얼마나 거리가 떨어져 있는 지에 대해서 알아보려고 합니다. 바로 전체를 비교하면 어려울 수 있으니 각 집합에서 X(2)와 Y(6)이 얼마나 떨어져 있는지 한번 비교해 보도록 하겠습니다.
 
  각 원소들이 뽑힐 확률이 1/n이라고 했을 때, X가 2를 뽑을 확률은 1/2이고, Y가 6을 뽑을 확률도 역시 1/2 입니다. 그리고 X가 2를 뽑을 확률과 Y가 6을 뽑을 확률에 대한 거리를 계산한다면, 두 경우에 대해서 확률이 동일하기 때문에 위에서 저희가 이미 구한 것 처럼 KL은 inf, JSD는 log2 가 나오지만 Wasserstein distance는 다음과 같이 구합니다.
 
 ![formula](https://render.githubusercontent.com/render/math?math=\sqrt{(6-2)^2}%2B\sqrt{(\frac{1}{2}-\frac{1}{2})^2}=4)

즉 확률은 같아도 2를 1/2로 뽑을 확률과 6을 1/2로 뽑을 확률은 정확히 4 만큼의 거리를 가진다는 것을 알 수 있습니다. 


지금까지 우리는 wasserstein distance를 어떻게 구하는 지에 대해 알아봤습니다. 하지만 이 것은 X(2) -> Y(6) 로 갈 때만의 이야기를 한 것입니다. 우리가 알고 싶은 건 X와 Y에 대한 확률 거리가 얼마나 떨어져 있는지를 어떻게 하면 정량적으로 나타낼 수 있는가? 입니다.

![XY](https://user-images.githubusercontent.com/70379885/135189137-e721db84-0e7d-4c75-b0d7-777cd4d9b062.png)

다시 한번, X,Y에 대해서 각 원소들이 뽑힐 확률이 uniform(모두다 균일)이라고 가정할 때, 각 원소들이 뽑힐 확률은 모두 1/n으로 동일 합니다. 그래서 각 원소들이 뽑힐 확률은 Y축에 표시되어 있고, 그 원소의 값들은 X축에 표시되어 있습니다. 그리고 우리는 이제 X를  Y처럼 만들면 되는데, 만드는 방식은 블록 쌓기와 같습니다. 이 블록은 모든 블록의 크기를 더하면 1이 되는 블록입니다. 하지만 이렇게 되면 경우의 수가 너무 많이 발생합니다. 예를 들면 X(2)를 X(6)에 전부 다 옮겨 Y(6) 처럼 만들어주는 방법이 있습니다. 그것도 아니라면 X(3),X(4),X(5)를 X(6)으로 모두 옮긴 다음, X(2)를 잘게 쪼개서 X(3),X(4),X(5)를 채워주는 방법도 있습니다. 이렇게 여러가지의 방법들이 존재하기 때문에 Wasserstein distance에는 제약 조건이 있습니다. 어떻게 옮기든 상관은 없는데, 그 중 최소비용이 드는 방법을 두 함수의 거리로 생각하겠다는 것입니다. 비용 측정은 다음과 같이 합니다.

![image](https://user-images.githubusercontent.com/70379885/135199922-11e9c2d4-1eaa-40d4-9482-1468c7aac914.png)

이 계산식은 EM distance, Wasserstein-1 이라고 불리는 거리계산 방법입니다. 언뜻 보면 어려워 보일 수 있지만 집합 r과 집합 g에서 사건이 동시에 발생할 때 가능한 모든 확률들 중에서,L2 norm 공식을 적용했을 때 임의로 선택된 x,y의 확률의 차이와 값의 차이가 가장 작은 것을 Wasserstein distance라고 정하기로 한 것입니다. 그렇다면 이제 우리는 우리가 처음에 말했던,  "왼쪽 부분은 왼쪽 부분 끼리, 오른쪽 부분은 오른쪽 부분끼리 같아지도록 만들어줘야"라는 의미가 무엇인지 생각해볼 수 있습니다. 물론 이렇게 확률을 변화시키지 않아도 되지만 최대한 근접하게 확률을 변화해야만 Wasserstein distance를 구하기 수월해 집니다.

#### 요약

 즉, wasserstein distance는 어떤 사건이 동시에 발생했을 때, 두 사건이 가지는 확률과 분포가 달라지는 경우가 발생합니다. 분포가 같고 확률만 다르다면 기존의 KL, JSD로 충분히 계산이 가능하겠지만, 분포가 다르면 KL은 무한대로 발산하고 JSD는 log2의 값만 나타내게 됩니다. 하지만 wasserstein distance는 분포가 다르더라도 분포의 차이, 그리고 확률의 차이를 동시에 고려합니다. 그래서 분포가 다른 두 확률에 대해서 얼마나 차이가 나는 지를 구별하여 나타낼 수 있는 거리 함수입니다.

### wgan-gp

자, 우리는 기본적인 GAN loss function의 한계를 알아보았고, 왜 KL, JSD divergnece를 사용해서 학습을 하면 안되는지, 그리고 그 것을 극복하기 위해서 어떤 확률거리 함수를 새롭게 사용해야 했는 지를 알아보았습니다. 실제로 tadgan 논문의 5쪽을 보면 일반적으로 GAN에서 사용하는 loss function과, 저자가 학습이 잘 되지 않는 이슈 때문에 wgan-gp를 사용하겠다는 내용이 나옵니다. 그리고 Wasserstein loss는 1-Lipschitz continuous function 조건(llf(x1) − f(x2)ll ≤ Klx1 − x2l, ∀x1, x2 ∈ dom f) 을 만족 시키기 때문에 어느정도 기울기에 대한 변화를 제어할 수 있습니다. 여기까지 보면보면 wasserstein distance를 이용한 wasserstein loss로 충분해보이지만, 사실은 그렇지 않았습니다. 우리는 이것을 다룬 논문( Improved Training of Wasserstein GANs:https://arxiv.org/abs/1704.00028)을 통해 확인해 볼 수 있습니다. 1-Lipschitz  조건을 만족하긴 하지만, 이것 역시 아직 gredient 변화에 대해서 적절히 반응하지 못합니다. 그래서 gradient penalty를 추가 해서 gradient의 변화를 조절하기로 합니다. 

### 결론

loss function을 사용할 때, 기존의 GAN에 적용하던 확률거리함수(KL, JSD) 보다는 Wasserstein distance를 사용해야 모델이 학습을 잘 하게 되는데, 그 이유는 KL, JSD 발산은 서로 다른 값을 뽑을 확률에 대한 계산을 하게 되면 특정 값이나 무한대의 값을 측정하기 때문에 학습이 제대로 이루어지지 않는다. 하지만 Wasserstein distance는 서로 다른 값을 뽑을 확률에 대해서 그 확률에 상관 없이 가변적인 값을 도출해 내기 때문에 손실 함수의 거리 계산으로 적합하다는 내용 입니다. 이 Wasserstein distance를 사용한 GAN을 wgan이라고 하는데, 이 wgan도 조금 문제가 있어서 wgan-gp라는 손실 함수가 새로 나오게 되었습니다.
 

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
