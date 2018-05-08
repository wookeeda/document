심층강화학습 study 참고용
=======================


기계학습은 일반적으로 지도학습, 비지도학습, 강화학습으로 분류가 됩니다.

<pre>
지도학습: y = f(x)
문제(x)와 정답(y) 세트를 사용해 학습을 진행을 하게 됩니다.

비지도학습: f(x) 
데이터의 관계를 이용해 분류를 하는 방식으로 학습을 진행하게 됩니다.

강화학습: y = f(x) with Reward 
정답이 없는 상태에서 각 action의 보상을 통해서 학습을 시키는 방식입니다.
실제 사람이 학습하는 방식과 많이 유사한 방식으로, 
자전거를 처음 배울 때 넘어지지 않기 위해(Reward) 
핸들을 조작(Action)하면서 해당 경험을 통해 학습이 이뤄집니다.
</pre>

심층강화학습(Deep Reinforcement Learning)은 이름처럼 Deep Neural Network(신경망, 딥러닝) 을 활용해서
강화학습을 진행하는 방식입니다.

참고: 알파고 다큐멘터리 
  https://www.youtube.com/watch?v=DJE5iM8XPIc3

1) 동영상 강의
* 김성훈교수 유튜브 강좌(한글) 
  https://hunkim.github.io/ml/2 시즌 RL - Deep Reinforcement Learning 

* David Siver 교수님(Deep Mind의 알파고 개발) 강의
  https://www.youtube.com/watch?v=2pWv7GOvuf0&t=2291s1

* 네이버D2 세미나: Introduction of Deep Reinforcement Learning
  https://www.youtube.com/watch?v=dw0sHzE1oAc1

* 네이버D2 세미나: A3C
  https://www.youtube.com/watch?v=gINks-YCTBs&t=2009s

2) 파이썬과 케라스로 배우는 강화학습 (책: 주교재)
  http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=979115839072333. 
  추가자료: https://www.gitbook.com/book/dnddnjs/rl/details1

3) Sutton 교수님 교재 : 강화학습의 바이블입니다.
  http://incompleteideas.net/book/the-book-2nd.html2
  online draft 클릭~

4) 강화학습 실습용 환경
  https://gym.openai.com/3
