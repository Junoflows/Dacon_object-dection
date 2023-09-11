# Dacon_object-dection

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
+ epochs이 커질수록 train 셋은 정확도가 높아지지만 valid 셋에서는 그렇지 않음
+ 학습률과 배치도 비교해봐야겠지만 epochs이 300보다 커지면 valid 셋에서는 정확도가 떨어지는 현상 발생(overfitting)
+ 이는 실험을 통해 확인해야함

## color 이미지와 grayscale 적용한 이미지의 정확도 비교
목적은 차 종을 분류하는 것이고 색상은 큰 의미가 없음
color로 학습하는 것보다 grayscale 로 학습하는 경우 색보다는 로고나 차 종의 특징을 잘 학습함
정확도도 80점 대에서 90점 대로 10점이상 높아짐

## learning rate와 정확도의 관계
learning rate 가 높으면 학습 속도가 빨라지지만 오류 값을 제대로 산출하지 못하거나 오버플로우가 발생할 수 있
learning rate 가 너무 낮으면 학습 속도가 오래 걸리고 오버피팅의 위험이 있음
모델을 설정할 때의 각 모델의 기본 learning rate 를 확인하고 모델에 따라 다르게 설정해야 함 (모델 논문을 꼭 살펴볼 것)
처음에는 learning rate 를 높게 설정해서 학습하면서 빠르게 파라미터 튜닝을 해보고 나중에 학습률을 높여 학습하는 방법을 추천

