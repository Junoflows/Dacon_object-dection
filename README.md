# Dacon_object-dection
2023.05.08 ~ 2023.06.19 에 참여했던 Dacon이 주관한 '합성데이터 기반 객체 탐지 AI 경진대회' 입니다.
## 대회 개요 및 설명
### 주제
합성 데이터를 활용한 자동차 탐지 AI 모델 개발
### 배경
비솔(VISOL)은 영상분야 AI 학습용 합성데이터(Synthetic Data) 공급 사업을 준비하고 있습니다.
합성데이터란 실제 환경에서 수집되거나 측정되는 것이 아니라 디지털 환경에서 생성되는 데이터셋으로,
최근 방대한 양질의 데이터셋이 필요해짐에 따라 그 중요성이 대두되고 있습니다.
합성 데이터는 데이터 라벨링 작업을 위한 2배 이상의 시간 절약과 10배 가까운 비용을 절감하게 하고, 자동화를 바탕으로 정확한 라벨링의 데이터 그리고 정확한 AI 모델 개발을 위한 데이터의 다양화를 가능하게 합니다.
본 경진대회는 실사와 같은 객체와 배경을 비솔의 3D Rendering과 VFX 기술로 생성된 AI 학습용 고품질 합성데이터를 바탕으로 진행됩니다.
### 설명
학습용 합성데이터를 활용하여 자동차 탐지를 수행하는 AI 모델을 개발해야 합니다.
평가는 실제데이터를 바탕으로 진행되며, 자동차 탐지 뿐만 아니라 34가지의 자동차 세부모델까지 판별해야합니다.
## 데이터셋 형식
![image](https://github.com/Junoflows/Dacon_object-dection/assets/108385417/79d68e69-fe46-48ee-9032-61e59f8e77b3) <br/>
9.0 1037 209 1312 209 1312 448 1037 448 <br/>
25.0 804 425 1127 425 1127 783 804 783 <br/>
12.0 330 250 583 250 583 511 330 511 <br/>
+ 클래스, 각 모서리 x,y 좌표로 구성
+ Yolo 모델을 사용할 예정이므로 Yolo 형식으로 변환 필요

## 데이터 Yolo 형식으로 바꾸기
```
img = plt.imread('/home/jrkim/songjunho/object_detection/train/syn_00000.png')
image_height, image_width, _ = img.shape
with open('/home/jrkim/songjunho/object_detection/train/syn_00000.txt') as reader:
    yolo_labels = []
    for line in reader.readlines():
        line = list(map(float, line.strip().split(" ")))
        class_name = int(line[0])
        x_min, y_min = float(min(line[1], line[3],line[5],line[7])), float(min(line[2], line[4], line[6], line[8]))
        x_max, y_max = float(max(line[1], line[3],line[5],line[7])), float(max(line[2], line[4], line[6], line[8]))
        x, y = float(((x_min + x_max) / 2) / image_width), float(((y_min + y_max) / 2) / image_height)
        w, h = abs(x_max - x_min) / image_width, abs(y_max - y_min) / image_height
        yolo_labels.append(f"{class_name} {x} {y} {w} {h}")
```
['9 0.61171875 0.3158653846153846 0.14322916666666666 0.2298076923076923', <br/>
 '25 0.5028645833333333 0.5807692307692308 0.16822916666666668 0.34423076923076923', <br/>
 '12 0.23776041666666667 0.3658653846153846 0.13177083333333334 0.25096153846153846'] <br/>

+ class, center_x, center_y, width, height 으로 구성(YOLO format)

## Grayscale 이미지 변환
+ 자동차를 구분할 때 형태로만 구분하고 색은 영향을 미치지 않을 것으로 예상
+ 전체 이미지에 회색조를 적용하면 더 정확한 분류를 하지 않을까?
+ 전체 이미지에 Grayscale 적용

![image](https://github.com/Junoflows/Dacon_object-dection/assets/108385417/a75cceaa-93d9-4a2a-8bfc-c3d03e7417ca)

## 결과 및 리뷰
데이콘 리더보드 기준 4% 안에 든 33등으로 학업과 병행하여 마지막에는 시험기간과 겹쳐 온전히 집중하지 못한 아쉬움이 남는 공모전  
객체 탐지는 처음 해보는 분야였지만 Faster-RCNN 과 YOLO 의 차이를 공부하고 논문으로 모델을 이해하는 연습을 할 수 있었음
+ 데이콘 리더보드 기준 4% 안에 든 33등으로 학업과 병행하여 마지막에는 시험기간과 겹쳐 온전히 집중하지 못한 아쉬움이 남는 공모전.  
+ 객체 탐지는 처음 해보는 분야였지만 Faster-RCNN 과 YOLO 의 차이를 공부하고 모델을 적용하는 연습을 할 수 있었음.
+ 구하기 힘든 데이터로 객체 탐지를 해볼 수 있는 좋은 경험이었음.

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


## epochs 에 따른 정확도 비교
+ epochs이 커질수록 train 셋은 정확도가 높아지지만 valid 셋에서는 그렇지 않음 (overfitting)
+ 학습률과 배치도 비교해봐야겠지만 epochs이 300보다 커지면 valid 셋에서는 정확도가 떨어지는 현상 발생(overfitting)
+ 이는 실험을 통해 확인해야함
+ 학습 시간 200 epochs 기준 24시간 이상 빨라짐

## color 이미지와 grayscale 적용한 이미지의 정확도 비교
+ 목적은 차 종을 분류하는 것이고 색상은 큰 의미가 없음
+ color로 학습하는 것보다 grayscale 로 학습하는 경우 색보다는 로고나 차 종의 특징을 잘 학습함
+ 정확도도 80점 대에서 90점 대로 10점이상 높아짐
## learning rate와 정확도의 관계
+ learning rate 가 높으면 학습 속도가 빨라지지만 오류 값을 제대로 산출하지 못하거나 오버플로우가 발생할 수 있음
+ learning rate 가 너무 낮으면 학습 속도가 오래 걸리고 오버피팅의 위험이 있음
+ 모델을 설정할 때의 각 모델의 기본 learning rate 를 확인하고 모델에 따라 다르게 설정해야 함 (모델 논문을 꼭 살펴볼 것)
+ 처음에는 learning rate 를 높게 설정해서 학습하면서 빠르게 파라미터 튜닝을 해보고 나중에 학습률을 높여 학습하는 방법을 추천
## 회고(6개월 후)
### 👍
+ Yolo와 Faster-RCNN 에 대한 장단점을 찾아보면서 나름의 근거에 따라(논리적으로) Yolo를 사용하였다.
  + 짧은 기간에 성과를 내야하는 공모전에서 학습 속도가 빠른 Yolo 를 사용하였다.
+ Grayscale 변환하여 학습하여 정확도를 10% 이상 끌어올렸다. 아이디어의 중요성을 느꼈다.
### 👎
+ 학습 데이터에 가로등 헤드가 항상 들어가 있었는데 이 부분을 제거하고 학습했다면 더 좋은 결과가 나왔을 것 같다.
+ 항상 모든 이미지 파일을 넣고 학습하여서 한번 학습 시 24시간 이상 걸렸었는데 배치로 나누어 학습했다면 시간을 더 절약할 수 있었을 것 같다.
+ 처음 가상 서버를 이용했었는데 파일 백업을 좀 더 신경썼으면 좋았을 것 같다.
