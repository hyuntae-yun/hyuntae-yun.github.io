---
layout: post
title:  "Liquid Time-constant Networks-intro.markdown
"
date:   2021-11-02 22:01:00 +0300
image:  tliquidnetwork.png
tags:   Liquid Time-constant Networks,review
---


데이터의 중요성 때문에 최근에 이상치 탐지에 대해 많은 관심을 가지고 있었지만, 최근 유튜브를 보던 중 제법 흥미로운 주제가 있어서 리뷰를 시작하게 되었습니다.

 MITCBMM에서 Liquid Neural Networks(https://www.youtube.com/watch?v=IlliqYiRhMU&t=2024s)를 보게 되었는데, 아마도 많은 사람들이 가장 관심있어 하는 '인간의 뇌를 모방하여 만든 모델로 
  
문제를 해결하기'를 실제로 한 게 아닐까 싶습니다. 관심도 높아서 오늘자 기준으로 (2021년 11월 2일) 벌써 10만 뷰를 달성했습니다.


1. 어떤 식으로 뇌를 모방했을까?


논문을 발표하는 Ramin Hasani씨는 유튜브에서 다음과 같이 말합니다.

"실제 두뇌가 어떻게 조직에 접근하는 지에 대한 관심은 지금까지 많이 있었습니다. AI의 궁극적인 목표는 인간을 닮아가는 것이기 때문이죠. 

그래서 실제로 어떻게 세상과 상호작용을 하는지를 잘 이해해야 좋은 딥러닝 모델을 설계할 수 있습니다. 

그리고 우리 뇌는 robust(이상치에 영향을 적게 받고) 하면서도 굉장히 유연한 사고방식을 가지고 있습니다."

이렇게 설명하면서 영상을 보여주는데, 컴퓨터 비젼을 예로 들고 있습니다. 컴퓨터 비젼은 CNN(Convolutional Neural Network)을 사용하는데, 이 때 fully connected layer중에서 어떤 부분이

활성화 되는지에 따라서 모델의 예측이 달라집니다. 




Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

