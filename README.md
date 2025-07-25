# 25일차

## 1. runpod 내에서 주피터 랩을 사용하도록 조사하기
1. rtx 3090을 선택하고 pod를 만들때, runpod pytoch 2.8.0 버전으로 바꾸고 만들었더니, 교수님처럼 주피터 랩과 연결되는 버튼이 생김.
<img width="876" height="663" alt="image" src="https://github.com/user-attachments/assets/d2fa6742-202c-4dc3-b1a0-acd57a880dc3" />

2. connect -> HTTP Service -> jupyter lab과 연결 버튼을 누르면 코드를 입력하는 곳으로 이동된다.
<img width="531" height="451" alt="image" src="https://github.com/user-attachments/assets/24e18c90-7261-408e-ae8d-c578bb9f79f1" />

## 2. Colab과 Jupyter lab 모두에서 "교통신호 인식" 코드를 사용해보고, 속도 비교하기
YOLO는 내부적으로 RunPod PyTorch 2.8.0 환경 기반이고, 추론 또한 PyTorch 위에서 실행되고 있음.<br>
챗GPT에게 Colab용 코드와 Jupyter lab용 코드를 모두 짜달라고 요청함.

### 코랩에서 진행
[코랩에서 실행한 코드 링크](0725_traffic_light_in_colab.ipynb)<br>
<img width="1958" height="1080" alt="image" src="https://github.com/user-attachments/assets/d8c8b1bf-dc74-4c96-9e1c-ee2960b67b3d" /><br>
-> 화질때문인지 YOLOv11 치고 정확도가 엄청 높진 않고, 속도 역시 2분 이상으로 오래걸렸다.

```python
⚙️ YOLO 추론 처리 정보
 - 전체 처리 시간: 128.99초
 - 처리된 프레임 수: 6091
 - 평균 추론 FPS: 47.22 frames/sec
```

### 주피터 랩의 노트북 셀에서 진행
1. 패키지/FFmpeg 설치
<img width="2183" height="161" alt="image" src="https://github.com/user-attachments/assets/3a5ff5d3-2bf1-4eba-be14-b9393c4bb962" /><br>
```python
!pip install -U ultralytics>=8.3.0 yt-dlp opencv-python tqdm
!apt-get update && apt-get -y install ffmpeg
```

2. 유튜브 다운로드 및 추론. **속도가 30초 정도로 매우 빠르다.**
```python
import yt_dlp, os, glob
from ultralytics import YOLO

# -------------------
# 1) 유튜브 다운로드
# -------------------
YOUTUBE_URL = "https://www.youtube.com/watch?v=BhTLa3fdZhE&pp=ygUT64-E66Gc7KO87ZaJIOyYgeyDgQ%3D%3D"

os.makedirs("videos", exist_ok=True)
video_path = "videos/input.mp4"

ydl_opts = {
    "outtmpl": video_path,
    "format": "mp4/bestvideo+bestaudio/best",
    "merge_output_format": "mp4",
}
with yt_dlp.YoutubeDL(ydl_opts) as ydl:
    ydl.download([YOUTUBE_URL])

print("다운로드 완료:", video_path)

# -------------------
# 2) YOLO 추론
# -------------------
MODEL_WEIGHTS = "yolo11n.pt"   # 안 되면 "yolov8n.pt"
model = YOLO(MODEL_WEIGHTS)

names = model.names
if isinstance(names, dict):
    tl_idx = [k for k, v in names.items() if v == 'traffic light'][0]
else:
    tl_idx = names.index('traffic light')

save_dir = "runs/detect/traffic_light"
results = model.predict(
    source=video_path,
    conf=0.25,
    classes=[tl_idx],
    save=True,
    project="runs/detect",
    name="traffic_light",
    vid_stride=1
)

out_videos = glob.glob(os.path.join(save_dir, "*.mp4"))
print("결과 비디오:", out_videos[0] if out_videos else "없음")
```

3. 왼쪽 메뉴에서 runs/detect/traffic_light/ 내부에 저장되는 결과영상을 우클릭하여 다운받기.
<img width="324" height="157" alt="image" src="https://github.com/user-attachments/assets/2768990d-ba96-4456-9b04-f38f77679f10" />

4. 결과영상 : 정확도는 큰 차이가 없어보이나, 다운로드 속도와 추론 속도가 압도적으로 빠른게 특징!
<img width="1925" height="1080" alt="image" src="https://github.com/user-attachments/assets/0167a8fc-43ce-4c71-a91b-a0ddc1cc74ed" />

[makesense ai를 사용한 양근영님 깃허브](https://github.com/audalsgh/20250725/blob/main/README.md)
