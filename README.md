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


## batchsize와 learning rate에 대한 관계 및 정확도

