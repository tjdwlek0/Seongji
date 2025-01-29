## 0. Overview
### Environment
- _Write Development environment_

### Requirements
- albumentations==1.3.1
- ipykernel==6.27.1
- ipython==8.15.0
- ipywidgets==8.1.1
- jupyter==1.0.0
- matplotlib-inline==0.1.6
- numpy==1.26.0
- pandas==2.1.4
- Pillow==9.4.0
- timm==0.9.12


## 1. Competiton Info

### Overview
- Computer Vision 분야에서 이미지를 분류하는 경진대회
- 17개의 문서 클래스로 구성된 문서 이미지 데이터셋으로 모델 학습
- 학습된 모델을 활용하여 라벨링이 안된 test 데이터셋에 대한 예측 수행
- 예측 정확도 지표인 F1 score(macro)를 기준으로 모델 성능을 평가하고 데이터 전처리/모델/하이퍼파라미터 조정을 통해 성능 개선
<img width="657" alt="image" src="https://github.com/user-attachments/assets/5c943608-80d8-4b17-907a-1fcf62ae2d5c" />


### Timeline
- 대회 개시: 2024년 
- 최종 제출: 2024년 12월 31일 
- 발표 세미나: 2025년 1월 2일  

## 2. Components

### Directory

- _Insert your directory structure_

e.g.
```
├── code
│   ├── jupyter_notebooks
│   │   └── model_train.ipynb
│   └── train.py
├── docs
│   ├── pdf
│   │   └── (Template) [패스트캠퍼스] Upstage AI Lab 1기_그룹 스터디 .pptx
│   └── paper
└── input
    └── data
        ├── eval
        └── train
```

## 3. Data descrption

### Dataset overview

- Train Dataset: 1570개 이미지 (train.csv: 이미지 파일명을 의미하는 ID와 클래스 라벨을 의미하는 target 두개 컬럼)
- Test Dataset: 3140개 이미지 (samplesubmission.csv: ID,target 두개 컬럼이 있으며 target은 모두 0으로 설정)
- Meta.csv: target 라벨의 클래스 이름(문서 종류)을 나타내는 파일

### EDA/Data Processing
- Train 이미지 라벨링 오류가 존재하여 수정하고 분간이 어려운 이미지 삭제 (6개 라벨링 수정 및 1개 이미지 삭제)
- 학습 과정에 대한 검증을 위하여 Train 데이터로부터 validation 데이터 셋을 분리 
- Test와의 통일성 맞추고 Train/Validation 데이터에 대한 증강을 적용
-- Aulbumentation, Augraphy, OpenCV, mixup 등 다양한 조합 적용
- Confusion Matrix를 사용하여 기본적으로 분류가 어려운 클래스 확인 (3, 4, 7, 14)
- 분류 오류 양상이 높은 클래스는 Grad-Cam 시각화로 모델의 학습 영역을 기준으로 특성을 추출하고자 시도
<img width="689" alt="image" src="https://github.com/user-attachments/assets/b958e8ac-eef0-4236-ba45-dae3e7c36134" />

- Test 이미지의 복구를 시도하고자 OCR을 활용하여 텍스트가 명확하게 읽히는 각도를 분석하고 반전 및 회전 조합 적용 시도
  => 약 70%의 이미지의 회전을 바로 잡았으나, 추론 결과 F1 점수는 소폭 감소하였음.
- 전체 이미지보다는 특정 텍스트 영역을 미리 검출한 뒤 텍스트를 추출하는 것이 효과가 높은 것을 확인할 수 있었음
<img width="537" alt="image" src="https://github.com/user-attachments/assets/a57f7342-3020-4170-a1fc-a676a83392a2" />
<img width="363" alt="image" src="https://github.com/user-attachments/assets/c4d1553b-682e-462a-81af-1ed88cec9065" />


## 4. Modeling

- 여러 이미지 및 OCR 모델에 대한 테스트를 진행하였고, 학습 효율성과 F1 macro 점수 결과에 따라 모델을 최종 선별함
<img width="639" alt="image" src="https://github.com/user-attachments/assets/98524998-a403-42a7-b6a6-eb5f78b15f51" />

- OCR의 경우 텍스트 영역 검출과 한글 텍스트 추출을 위하여 각 모델의 성능이 다른 Easy OCR과 paddleOCR 두개 모두를 사용
<img width="323" alt="image" src="https://github.com/user-attachments/assets/85afa777-2db2-4009-b32c-780697182765" />

- 선별한 5개 모델을 활용하여 앙상블을 시도해 보았고, 선택 모델과 앙상블 조합에 따라 hyperparameter는 유동적으로 조정
<img width="492" alt="image" src="https://github.com/user-attachments/assets/75799353-7ebc-43c8-bd66-3859c75bb1cf" />

- 앙상블 적용 기법은 각 클래스별 validation 데이터에 점수가 더 높은 모델에 가중치를 주는 방법 시도
<img width="412" alt="image" src="https://github.com/user-attachments/assets/cf5cf61b-166c-468b-9cc3-b9b72e97f80d" />


## 5. Result

### Leader Board

<img width="575" alt="image" src="https://github.com/user-attachments/assets/1e6f5f1d-6424-40bb-9e57-ef0334793223" />


### Presentation

[- _Insert your presentaion file(pdf) link_
](https://docs.google.com/presentation/d/1o2mddgG-54esmcFXVTf9mCzkZm41G_XRMoIbjlD1Cck/edit#slide=id.g32281594278_1_135)

## etc

