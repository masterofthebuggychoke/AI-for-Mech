# CWRU Spectrogram Notebook Files

이 압축 파일에는 두 개의 Jupyter Notebook이 들어 있다.

## 01_make_report_figures.ipynb

보고서/발표용 spectrogram figure를 만드는 노트북이다.

- 축, 제목, colorbar 포함
- 파일별 figure 저장
- label별 대표 figure 저장
- 컬러 spectrogram 생성

## 02_make_training_dataset_sliding.ipynb

AI 학습용 spectrogram 이미지 데이터셋을 만드는 노트북이다.

- sliding window 적용
- hop_length 조절로 이미지 수 조절
- train/val/test split 생성
- ImageFolder 구조 생성
- metadata.csv, split.csv, label_map.json 저장

## 권장 폴더 구조

노트북과 같은 위치에 `mat_files` 폴더를 만들고, CWRU `.mat` 파일들을 넣으면 된다.

```text
project_folder/
├─ 01_make_report_figures.ipynb
├─ 02_make_training_dataset_sliding.ipynb
└─ mat_files/
   ├─ Fault_BALL_007_1.mat
   ├─ Fault_BALL_007_2.mat
   ├─ ...
   └─ Normal_2.mat
```

## Label rule

파일명 끝의 `_1`, `_2`는 부하 조건으로만 쓰고, label에서는 제거한다.

```text
Fault_BALL_007_1.mat -> label = Fault_BALL_007, load_condition = 1
Fault_BALL_007_2.mat -> label = Fault_BALL_007, load_condition = 2
Normal_1.mat -> label = Normal, load_condition = 1
Normal_2.mat -> label = Normal, load_condition = 2
```

## Sliding window 조절

`02_make_training_dataset_sliding.ipynb`에서 아래 값을 바꾸면 된다.

```python
SEGMENT_LENGTH = 4096
HOP_LENGTH = 1024
MAX_SEGMENTS_PER_FILE = -1
```

- `HOP_LENGTH=4096`: overlap 없음
- `HOP_LENGTH=2048`: 50% overlap
- `HOP_LENGTH=1024`: 75% overlap
- `HOP_LENGTH=512`: 87.5% overlap
- `MAX_SEGMENTS_PER_FILE=-1`: 가능한 모든 sliding window 저장