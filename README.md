# Alpha-Car


### Member
- 이용한 : 팀장, 차선 인식 
- 김범영 : 딥러닝, 물체 인식
- 이지훈 : Motor, Sensor 제어
- 이신효 : 통신

### What is Alpha-Car?
- 2017 한이음 공모전에 나간 Alpha-car팀의 '딥러닝 기반 자율 주행 버스 운행 시스템' 이다.
- Deep Learning, OpenCV, Raspberry pi, RFID, 초음파 센서 등을 이용하여 구현하였다.
- AlphaGo + Car


### Implement Details
- SW
  - lane tracing using OpenCV (image processing Library)
  - Object Detection using Deep Learning ([YOLO v2](https://arxiv.org/pdf/1612.08242.pdf))
  - We can detect Car, Pedestrian, Stop sign and Traffic sign.

- HW
  - DC Motor for driving power
  - Servo Motor for direction control
  - Ultrasonic sensor for front obstacle detection
  - RFID for Bus Stop Recognition

### Result
- 2017 한이음 공모전 금상(과기정통부 장관상) 수상 프로젝트이며 자세한 사항들은 아래 링크에서 확인할 수 있다.
- [Youtube 데모 영상](https://www.youtube.com/watch?v=BcBvTIv5zpw&t=1s)
- [프로젝트 보고서 - 한이음 수상작 페이지](http://www.hanium.or.kr/portal/project/awardList.do)

### Limitations
- 학부 3학년 때 진행했던 프로젝트라 기술적인 문제들이 많다.
- 라즈베리파이를 사용했기 때문에 차내에서 자체적인 실시간 연산이 불가능해서 GPU 서버에서 연산 하도록 설계.
  - GPU 보드를 사용하면 해결 가능 (NVIDIA JETSON TX1,2 / JETSON NANO / GOOGLE EDGE TPU 등)
- 프로젝트 당시 딥러닝 물체 인식 모델인 YOLOv2에 대한 이해도 부족.
  - Pre-train된 weight를 사용한 것이 아니라 데이터 수집(from COCO, VOC, Udacity dataset) 해서 직접 학습을 시키는 부분에서 문제.
  - DarkNet의 BottleNeck 부분 잘못사용하고 있었음ㅜ
  - 논문에서 언급한 Multi-scale Training이나 Warm-up training 같은 성능에 큰 이슈가 될 수 있는 테크닉들을 사용하지 않음...
  - 모바일/임베디드 환경에서 연산 속도를 올릴 수 있는 Compression, Quantization 같은 Optimization 기법들을 사용했다면 더 좋았을듯. (TensorRT나 Tensorflow Lite 사용 권장)
- 신호등 색상 판단을 딥러닝으로 하지 못했다.
  - 별도의 Class로 학습하기에는 labeling이 안되어있거나 그 수가 매우 적었다.
  - 임시로 HSV의 threshold로 Green/Red를 판단하긴 했는데, 카메라 환경 변화에 매우 민감해서 잘못된 판단이 일어난다.
- 차선 인식 알고리즘도 많은 개선이 필요하다.
  - Opencv의 가장 기본적인 알고리즘으로 사용했기 때문에 여러 문제점이 많다.
  - 차선 영역이 뚜렷해야 동작이 되고, 환경 변화에 매우 민감하며 커브길에서 불안정하다.
  - 개선된 영상처리 알고리즘을 사용하거나 딥러닝 (Semantic Segmentation)으로 차선 인식할 필요가 있다.
- 차량 제어 알고리즘이 매우 부실함.
  - Lane Tracing을 차선 중앙 좌표만으로 판단하기 때문에 불안정하다.
  - 딥러닝으로 물체가 어디있는지 위치(좌표)는 알지만, 차량과의 거리나 각도 등을 제대로 알지 못한다. (2D perspective image 이기 때문)
  - 우선 임시로 y좌표 값으로 거리를 판단하는 식으로 사용하긴 했지만, 제대로 될리가 없다..
  - 또한 frame마다 물체를 인식하는 것이기 때문에 Object Tracking을 해야 하는데 이 부분도 임시 알고리즘으로 사용했음.
- Multi-Threading 부분도 다시 손봐야 할 것 같다.
  - 라즈베리파이에서 여러 센서들을 처리하기 위해 Multi Threading을 사용하긴 했음.
  - 하지만 당시 막판에 갑자기 계획이 변경되어 급하게 적용했던 코드이기 때문에 다듬어야할 필요가 있어보인다.
 
