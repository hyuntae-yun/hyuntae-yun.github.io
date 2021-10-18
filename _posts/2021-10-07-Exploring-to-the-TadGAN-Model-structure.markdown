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

모델 설명을 읽어보시면 두 개의 Generator, 두개의 Critic 함수로 이루어졌다는 것을 확인할 수 있습니다. 
Generator E는 time series sequence를 latent space로 바꿔주는 역할을 하고, Generator G는 latent space를 다시 time series sequence로 변환해주는
역할을 합니다. 여기까지는 기존의 AE와 별반 다를 것이 없어 보입니다. 하지만 Critic X함수는 원본 데이터와 생성된 데이터 중 어떤 데이터가 원본인지를 가려내고, Critic Z 함수는 Generator E가 time series squence를 latent space로 얼마나 잘 맵핑 했는지를 판별 합니다. 어느정도 이해가 됐지만 이 도식 만을 가지고는 정확히 모델이 어떻게 작동하는지 알기 힘듭니다. 그래서 저는 좀 더 알기 쉽게 Tadgan 공식 github의 이미지를 가지고 설명을 할까 합니다. 모델 이해는 pytorch에 능숙하다는 가정하에 (https://github.com/arunppsg/TadGAN)를 보시는 것을 추천드립니다.

![]({{ site.baseurl }}/images/tadgan0.png)

전체 모델 구조입니다.  E,G는 Encoder과 Decoder을 나타내고, Cx를 통해서 실제 데이터와 Reconstruction 데이터를 판별합니다. 그리고 Cz는 E를 통해 생성된 Latent space와 렌덤으로 생성된 데이터를 가지고 얼마나 잘 맵핑되어 있는 지를 판별합니다. 다음 부터는 각 Phase 별로 어떤 학습이 이루어지는지 볼텐데, 우리가 주목할 것은 학습의 주체가 무엇인지 입니다. 

![]({{ site.baseurl }}/images/tadgan1.png)

Phase 1 입니다. 실제 데이터와 random latent space가 Generator G를 통과한 재생성 데이터를 판별하는 Cx를 훈련 시키게 됩니다.

Cx Loss function = critic_score_fake_x - critic_score_valid_x + sqrt[sum(Cx(alpha * x + (1 - alpha) * x_)^2)]
  

![]({{ site.baseurl }}/images/tadgan2.png)

Phase 2 입니다. 여기서는 실제 데이터를 맵핑 시킨 latent space와 random latent space를 비교해 Cz가 어떤 데이터가 원본 데이터의 latent space인지 판별하도록 학습합니다. 

Cz Loss function =critic_score_fake_z - critic_score_valid_z + sqrt[sum(Cx(alpha * z)+ (1 - alpha) * z_)^2)]


![]({{ site.baseurl }}/images/tadgan3.png)

Phase 3 입니다. 실제 데이터에서 나온 latent space를 가지고 Generator G로 재생성한 time series squence(gen_x)와, random latent space를 가지고 Genrator G로 생성한 time series squence (fake_x)를 Cx로 판별해 실제 데이터를 구별합니다. 이 때 학습 주체는 Generator E 이고, Cx를 최대한 속이는 쪽으로 학습을 진행합니다. 그리고 원본데이터와 random latent space로 생성한 fake_x 데이터의 Cx Score들을 loss function에 추가했는데, Decoder의 학습 정도에 따라 학습 속도를 변화시킨 것을 볼 수 있습니다.

Generator E(Encoder) Loss function =mse_loss(x,gen_x)+critic_score_valid_z - critic_score_fake_z

![]({{ site.baseurl }}/images/tadgan4.png)

Phase 4 입니다.  실제 데이터를 Generator E에 통과 시킨 latent space를 Generator G에 통과시킨 gen_x와 실제 데이터 x를 비교합니다. 이 때 원본 데이터의 latent space와 random latent space를 통과시킨 Cz의 score들을 loss function에 추가했는데, Phase3 처럼 Encoder의 학습 정도에 따라 학습 속도를 변화시킨 것을 볼 수 있습니다. 

Generator G(Decoder) Loss function =mse_loss(x,gen_x)+critic_score_valid_x - critic_score_fake_x

위의 과정은 차례대로  encoder_iteration,decoder_iteration critic_x_iteration, 그리고 critic_z_iteration로 소스코드에 포함되어 있습니다. 코드를 보고 천천히 따라가시면 어떻게 모델이 작동하는 건지 잘알 수 있게 됩니다. 


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
