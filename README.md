# Face_Mask_Detection

CV Project : Mask Detection 

# 주제 설명

**실시간으로 마스크 착용 유무를 판별하는 인공지능 **

# 프로젝트 진행 과정

1. **마스크 착용 여부를 판별 위해 얼굴의 위치를 인식하는 모델 준비**
2. **마스크를 씌운 사진, 씌우지 않은 사진을 판단하는 학습 모델 준비**
3. **OpenCV를 통해 Web-cam과 연결하여 얼굴 인식 모델, 마스크 착용 여부 판단 모델 사용 후     실시간으로 확인**

# Data Set 소개 
아래의 구성으로 훈련셋, 테스트셋을 만들어서 훈련을 진행

![image](https://user-images.githubusercontent.com/69300448/216246742-a66ff63c-b778-4619-804a-fe846f9fe16b.png)

- Train : with_mask(), without_mask() 총 1315개

- Test : with_mask(), without_mask() 총 194개

학습 데이터를 추가하기 위해서 kaggle에서 데이터셋을 가져와서 사용

https://www.kaggle.com/datasets/omkargurav/face-mask-dataset

![image](https://user-images.githubusercontent.com/69300448/216246920-8fd37912-1ce6-4680-a5cb-7daca4260fb9.png)

- Train : with_mask(), without_mask() 총 7357개

- Test : with_mask(), without_mask() 총 1705개

# **사용 모델**

- **모델 설명**
    - **Face Detection Model based DNN**
        - caffe framework 로 학습된 Face Detection Model 사용
        - 미리 정의한 caffemodel과 prototxt를 인자로 받아와서 하나의 Net 객체를 반환
    - **MoblieNet**
        - 모바일 환경 등에 사용하기 위해 경량화에 집중한 모델
        - MobileNetV2: Inverted Residuals and Linear Bottlenecks(2018) 발표 논문 : [https://arxiv.org/abs/1801.04381](https://arxiv.org/abs/1801.04381)
    - **ResNet vs MobileNetV2**
        - MobileNet을 사용하면서 처리 시간이 많이 단축
        - 정확도 부분에서는 ResNet이 더 높을 것으로 예상되었으나 그렇지 않음
        - 이진 분류이고 전체적인 성능이 너무 높게 나와서 크게 차이가 나지 않는 것으로 예상
    - **Criterion → Cross entropy Loss**
        - 분류 모델이 얼마나 잘 수행됐는지 측정하기 위한 지표
    - **Optimizer →  Adam vs AdamW**
        - CV Task에서 Adam이 일반화가 많이 뒤처진다는 결과가 있어서 AdamW 사용
        - AdamW가 Loss에서 더 좋은 성능을 보여준다는 논문 결과를 바탕으로 Optimizer         변경하여 다시 시도 > 성능 향상
        
  ## 모델 파라미터 수 비교
  - ResNet101
  
  ![image](https://user-images.githubusercontent.com/69300448/216247178-8f80977c-5ebb-4df0-be4f-65883bdb2b9f.png)
  
  - MobileNetV2
  
  ![image](https://user-images.githubusercontent.com/69300448/216247215-b51e38f9-c63f-4a83-8cc1-14264ad88842.png)

  - 파라미터 수 약 1/20 수준
  - 학습된 모델의 용량 또한 줄어들었음 160Mbyte -> 8Mbyte
  
  ## 모델 성능 비교
  - ResNet101
  
  ![image](https://user-images.githubusercontent.com/69300448/216247499-67740e1d-9e68-4a12-9d8c-d384a1e65fe5.png)
  
  - MobileNetV2 - Adam 사용
  
  ![image](https://user-images.githubusercontent.com/69300448/216247537-a941a1c2-4534-4265-945e-29dc370ee66a.png)

  - MobileNetV2 - AdamW 사용
  
  ![image](https://user-images.githubusercontent.com/69300448/216247607-9156f7e8-fe66-4599-a7a5-fab1259379a2.png)

  - 예측 정확도 평가에서는 큰 차이를 보이지 않았음, 학습 시간이 절반 가량 향상된 모습
  - Optimizer Adam 보자 AdamW에서 조금 더 나은 예측 성능을 보여줌
  
# REFERENCE
- https://github.com/prajnasb/observations
- https://github.com/kairess/mask-detection
- https://github.com/chandrikadeb7/Face-Mask-Detection

