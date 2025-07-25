# 25일차

## 1. runpod 내에서 주피터 랩을 사용하도록 조사하기
1. rtx 3090을 선택하고 pod를 만들때, runpod pytoch 2.8.0 버전으로 바꾸고 만들었더니, 교수님처럼 주피터 랩과 연결되는 버튼이 생김.
<img width="876" height="663" alt="image" src="https://github.com/user-attachments/assets/d2fa6742-202c-4dc3-b1a0-acd57a880dc3" />

2. connect -> HTTP Service -> jupyter lab과 연결 버튼을 누르면 코드를 입력하는 곳으로 이동된다.
<img width="531" height="451" alt="image" src="https://github.com/user-attachments/assets/24e18c90-7261-408e-ae8d-c578bb9f79f1" />

## 2. Colab과 Jupyter lab 모두에서 "교통신호 인식" 코드를 사용해보고, 속도 비교하기
YOLO는 내부적으로 RunPod PyTorch 2.8.0 환경 기반이고, 추론 또한 PyTorch 위에서 실행되고 있음.

[코랩에서 실행한 코드 링크](0725_traffic_light_in_colab.ipynb)<br>
<img width="1958" height="1080" alt="image" src="https://github.com/user-attachments/assets/d8c8b1bf-dc74-4c96-9e1c-ee2960b67b3d" /><br>
-> 화질때문인지 YOLOv11 치고 정확도가 엄청 높진 않다.

(주피터 랩의 노트북 셀에서 진행)
1. 패키지/FFmpeg 설치
<img width="2183" height="161" alt="image" src="https://github.com/user-attachments/assets/3a5ff5d3-2bf1-4eba-be14-b9393c4bb962" />

2. 유튜브 다운로드 및 추론
<img width="753" height="843" alt="image" src="https://github.com/user-attachments/assets/ae5ddcd1-7cd7-43c4-bdff-d78d7fb47047" />

3. 왼쪽 메뉴에서 runs/detect/traffic_light/ 내부에 저장되는 결과영상을 우클릭하여 다운받기.
<img width="324" height="157" alt="image" src="https://github.com/user-attachments/assets/2768990d-ba96-4456-9b04-f38f77679f10" />

4. 결과영상 : 정확도는 큰 차이가 없어보이나, 다운로드 속도와 추론 속도가 압도적으로 빠른게 특징!
<img width="1925" height="1080" alt="image" src="https://github.com/user-attachments/assets/0167a8fc-43ce-4c71-a91b-a0ddc1cc74ed" />
