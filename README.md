# multi-label-classification-Mnist
[대회 링크](https://dacon.io/competitions/official/235697/overview/description)

Mnist 이미지에 들어가 있는 숫자를 맞추는 대회다.

## 간략한 데이터 설명

![image](https://user-images.githubusercontent.com/80466735/175808536-66a235ae-89ab-4852-98cb-04e0f6f0cac3.png)

데이터를 본다면 이미지에 노이즈가 굉장히 심하다. 그래서 처음에는 이진화를 적용한다면 노이즈가 threshold를 못넘었다. 그래서 숫자는 더 선명해지고 노이즈는 지워지는 효과를 볼 수 있다.
이덕에 학습속도와 정확도..를 아마도 높였다. 하지만 후반부로 가서 오히려 불필요한 작업인 거 같고 큰 효과가 없는 거 같아서 빼고 적용했다. 

## 모델과 Inference, 적용한 기법들

 1. 모델
      모델은 efficientnetb(0~4) pretrain 모델을 사용했다. 왜냐하면 군대에 있어서 속도도 문제고 이번 목표는 데이터에 대한 이해도와 다양한 기법, tuning을 위했기 때문이다.
      그렇다면 더 가볍고 빠른 mobileNet을 사용하면 되지 않냐고 말할 수 있다. 하지만 너무 가벼운 모델을 사용한다면 test 할때 내가 어느정도 좋은 결과를 냈는지 애매하기 때문에 efficientNet b7은 안쓰고 좀 낮은 버전을 썼다.
      
      ![image](https://user-images.githubusercontent.com/80466735/175808858-82bb9a86-e438-4f27-a49c-acfcb3634c56.png)

      또 다른 이유는 위에 그래프를 본다면 b4 이상부터 늘어나는 무게에 비해 큰 이득이 없다고 생각해서도 있다. 
      
 2. Inference에 시도한 점 설명
        Infenece 코드에 TTA를 적용해서 ensemble을 했다. 결과적으로 좋은 결과였던 거 같다. 그냥 간단히 학습한 모델은 80% 초반을 내놨다면 TTA+ensemble을 통해 84%까지 올렸다.
      이외에도 아쉬운게 있다면 저번 딥러닝 대회때도 그렇지만 Infernece 코드가 더럽다는 거다. 그래도 저번보다는 나아졌다. 아마 더 다양한 모델을 가져온다면 90퍼까지 갈 수 있을 것이다.
 3. 기법(MIXUP,Ray Tune)
        Mix UP은 유명한 논문인 [bag of tricks for image classification with convolutional neural networks](https://arxiv.org/abs/1812.01187)에 나온 방법중 하나다. 노이즈가 있는 데이터에 강하며 사용해보면 train할때 정확도가 막 들쑥날쑥하지만 막상 valid할때는 잘 학습된걸 확인이 가능하다. 그 이유는 mixup이 이미지를 융합..? 시켜서 그렇다. 
    
![image](https://user-images.githubusercontent.com/80466735/175809011-2a4bdee7-5bc2-4bef-b429-7798758d5850.png)
        위에 사진 뜻대로 특정 비율만큼 사진을 섞기 때문이다. 
        Ray Tune은 hyperparameter를 튜닝할때 정말 좋다. 계속해서 테스트를 하면서 조금씩 바꿔간다. 결국 가장 적합한 하이퍼파라미터를 찾는다. 결론적으로 코드는 구현을 완료했지만 쓰지는 않았다. 왜냐하면 속도도 문제고 큰 필요성이 없었다. 그래서 코드를 보면 나중에 가면 안쓰는 코드가 많다.

## 결론

![image](https://user-images.githubusercontent.com/80466735/175809151-6f3c60f4-d456-411d-bae1-fc781e7510bc.png)

큰 노력을 들이지도 않고도 83.2%정확도를 도달 할 수 있었다. 더 높이라면 높일 수 있다. 왜냐하면 일단 ensemble에 넣은 모델 개수 3개로 적을 뿐더러 그 모델들도 잘 학습된 모델이 아니었기 때문이다. 변명 같아보이지만 군대 특성상.. 어쩔수 없었다. 해야할 일들이 너무 많다. 그래서 최선적으로 선택할 결과였다. 

아무튼 결론적으로 이번 공부는 좋은 결과를 낸 거 같아서 좋다. 다음에는 Instagram Caption과 알고리즘 그리고 리액트를 계속 할 거다.
