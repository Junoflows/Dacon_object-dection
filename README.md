# Dacon_object-dection

## 사용하던 GPU 서버 점검으로 인해 업로드 연기

## 결과 및 리뷰
데이콘 리더보드 기준 4% 안에 든 33등으로 학업과 병행하여 마지막에는 시험기간과 겹쳐 온전히 집중하지 못한 아쉬움이 남는 공모전  
객체 탐지는 처음 해보는 분야였지만 Faster-RCNN 과 YOLO 의 차이를 공부하고 논문으로 모델을 이해하는 연습을 할 수 있었음

## 파라미터 조정값에 따른 결과 비교
### grayscale
+ (imgsz=640, epochs=100, batch=32, optimizer="Adam", patience = 5, workers = 16, lr0 = 1e-3)
iou = 0.2 conf = 0.7 75점

+ (imgsz=1024, epochs=100, batch=16, optimizer="Adam", patience = 5, workers = 16, lr0 = 1e-4)
iou = 0.2 conf = 0.5 87.789점
iou = 0.7 conf = 0.5 87.789점

+ (imgsz=1024, epochs=200, batch=16, optimizer="Adam", patience = 5, workers = 16, lr0 = 1e-4)
iou = 0.7 conf = 0.5 91.971점
iou = 0.7 con f= 0.7 93.323점

+ (imgsz=1024, epochs=300, batch=16, optimizer="Adam", patience = 5, workers = 16, lr0 = 1e-4)
iou = 0.7 conf = 0.5 93.693점
iou = 0.7 conf = 0.7 93.823점

### color
+ (imgsz=1024, epochs=200, batch=8, optimizer="Adam", patience = 5, workers = 16, lr0 = 1e-3)
iou = 0.2 conf = 0.5 84.69점
iou = 0.7 conf = 0.5 84.78점

+ (imgsz=1024, epochs=200, batch=16 optimizer="Adam", patience = 5, workers = 16, lr0 = 1e-4)
iou = 0.2 conf = 0.5 81.52점
iou = 0.7 conf = 0.5 81.52점
iou = 0.7 conf = 0.7 83.071점

+ (imgsz=1024, epochs=200, batch=16 optimizer="Adam", patience = 5, workers = 16, lr0 = 1e-3)
iou = 0.7 conf = 0.7 77.731점
iou = 0.2  conf = 0.7 76.029점


## epochs 에 따른 정확도 비교 (파일이 삭제돼서 이미지 파일 추후 업로드 예정)
<그래프>
+ epochs이 커질수록 train 셋은 정확도가 높아지지만 valid 셋에서는 그렇지 않음 (overfitting)
+ 학습률과 배치도 비교해봐야겠지만 epochs이 300보다 커지면 valid 셋에서는 정확도가 떨어지는 현상 발생(overfitting)
+ 이는 실험을 통해 확인해야함

## color 이미지와 grayscale 적용한 이미지의 정확도 비교
+ 목적은 차 종을 분류하는 것이고 색상은 큰 의미가 없음
+ color로 학습하는 것보다 grayscale 로 학습하는 경우 색보다는 로고나 차 종의 특징을 잘 학습함
+ 정확도도 80점 대에서 90점 대로 10점이상 높아짐

## learning rate와 정확도의 관계
+ learning rate 가 높으면 학습 속도가 빨라지지만 오류 값을 제대로 산출하지 못하거나 오버플로우가 발생할 수 있음
+ learning rate 가 너무 낮으면 학습 속도가 오래 걸리고 오버피팅의 위험이 있음
+ 모델을 설정할 때의 각 모델의 기본 learning rate 를 확인하고 모델에 따라 다르게 설정해야 함 (모델 논문을 꼭 살펴볼 것)
+ 처음에는 learning rate 를 높게 설정해서 학습하면서 빠르게 파라미터 튜닝을 해보고 나중에 학습률을 높여 학습하는 방법을 추천

## 회고(1년 후)
### 👍
+ Yolo와 Faster-RCNN 에 대한 논문을 찾아보면서 나름의 근거에 따라(논리적으로) Yolo를 사용하였다.
  + 짧은 기간에 성과를 내야하는 공모전에서 학습 속도가 빠른 Yolo 를 사용하였다.
+ Grayscale 변환하여 학습하여 정확도를 10% 이상 끌어올렸다. 아이디어의 중요성을 느꼈다.

### 👎
+ 학습 데이터에 가로등 헤드가 항상 들어가 있었는데 이 부분을 제거하고 학습했다면 더 좋은 결과가 나왔을 것 같다.
+ 항상 모든 이미지 파일을 넣고 학습하여서 한번 학습 시 24시간 이상 걸렸었는데 배치로 나누어 학습했다면 시간을 더 절약할 수 있었을 것 같다.
+ 처음 가상 서버를 이용했었는데 파일 백업을 좀 더 신경썼으면 좋았을 것 같다.
