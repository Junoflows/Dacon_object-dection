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
 
for label in yolo_labels:
    x, y, w, h = label.split(' ')[1:]
    x, y, w, h = float(x), float(y), float(w), float(h)
    start_x, start_y = x - w/2, y - h/2
    start_x, start_y, w, h = start_x*image_width, start_y*image_height, w*image_width, h*image_height
    
    rect = patches.Rectangle((start_x, start_y), w, h, linewidth = 1, edgecolor = 'lime', facecolor = 'none')
 을 찾아보면서 나름의 근거에 따라(논리적으로) Yolo를 사용하였다.
  + 짧은 기간에 성과를 내야하는 공모전에서 학습 속도가 빠른 Yolo 를 사용하였다.
+ Grayscale 변환하여 학습하여 정확도를 10% 이상 끌어올렸다. 아이디어의 중요성을 느꼈다.

### 👎
+ 학습 데이터에 가로등 헤드가 항상 들어가 있었는데 이 부분을 제거하고 학습했다면 더 좋은 결과가 나왔을 것 같다.
+ 항상 모든 이미지 파일을 넣고 학습하여서 한번 학습 시 24시간 이상 걸렸었는데 배치로 나누어 학습했다면 시간을 더 절약할 수 있었을 것 같다.
+ 처음 가상 서버를 이용했었는데 파일 백업을 좀 더 신경썼으면 좋았을 것 같다.
