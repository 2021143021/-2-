# 감정 분석 웹 서비스

**이름:** 김병수  
**학번:** 2021143021  

---

## 프로젝트 개요

이 프로젝트는 FastAPI와 HuggingFace Transformers를 활용한 감정 분석 REST API와 HTML/JS 기반의 간단한 프론트엔드를 제공합니다.

- 사용자는 웹페이지에 리뷰 텍스트를 입력하면
- FastAPI 서버가 학습된 감정 분석 모델로 예측을 수행해
- 별점(1~5점)과 신뢰도를 반환합니다.

---

## 🛠️ 사용 기술

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=HTML5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=CSS3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=JavaScript&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=Python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=FastAPI&logoColor=white)
![Transformers](https://img.shields.io/badge/Transformers-FF9900?style=for-the-badge&logo=HuggingFace&logoColor=white)

---

## 주요 기술 스택

- **Backend:** FastAPI, Huggingface Transformers, PyTorch
- **Frontend:** HTML, CSS, JavaScript (fetch API 사용)
- **모델:** 사전학습 DistilBERT 기반 감정 분석 모델

---

## 프로젝트 동작 흐름

```
[ 데이터 다운로드 ]
        ↓
[ 데이터 전처리 ]
        ↓
[ 모델 학습 (train.py) ]
        ↓
[ FastAPI 서버 구동 (main.py) ]
        ↓
[ 웹페이지(index.html)에서 리뷰 입력 → 예측 결과 출력 ]
```

---

## 폴더 구조

```
├── download.py              # Yelp 데이터 다운로드 스크립트
├── preprocess.py            # 데이터 전처리 스크립트
├── train.py                 # 모델 학습 스크립트
├── main.py                  # FastAPI 서버 및 감정 분석 API
├── index.html               # 프론트엔드 웹페이지
├── sentiment_model/         # 학습된 모델 저장 (config.json, model.safetensors, tokenizer 등)
├── sample_yelp.json         # 샘플 JSON 데이터
├── yelp_small_train.csv     # 원본 학습용 CSV 파일
├── yelp_small_train_cleaned.csv # 전처리된 학습 CSV 파일
└── README.md
```

---

## 실행 방법

### 1. 패키지 설치

```bash
pip install -r requirements.txt
```

### 2. 데이터 다운로드 및 전처리

```bash
python download.py
python preprocess.py
```

### 3. 모델 학습

```bash
python train.py
```

또는 이미 학습된 모델(`sentiment_model/`)을 준비합니다.

### 4. FastAPI 서버 실행

```bash
uvicorn main:app --reload --host 127.0.0.1 --port 8001
```

### 5. 프론트엔드 실행

- `index.html` 파일을 브라우저에서 직접 열어 사용합니다.
- (또는 간단한 로컬 서버에서 실행)

---

## 모델 학습 설정

- 모델: distilbert-base-uncased
- 최대 입력 길이: 512 토큰
- 학습 에폭(epoch): 1
- 배치 크기: 16
- 옵티마이저: AdamW
- 러닝레이트: 5e-5
- 손실 함수: CrossEntropyLoss
- 평가 지표: Accuracy

---

## API 사용 예시

### 엔드포인트

- **POST** `/predict`

### 요청 예시

```json
{
  "text": "The food was delicious and the staff was friendly"
}
```

### 응답 예시

```json
{
  "result": [
    {
      "label": "LABEL_4",
      "score": 0.87
    }
  ]
}
```

---

## API 테스트 예시 (curl)

```bash
curl -X POST "http://127.0.0.1:8001/predict" -H "Content-Type: application/json" -d '{"text": "This restaurant was amazing!"}'
```

---

## 프론트엔드 예시 화면

- 이름: 김병수
- 학번: 2021143021
- 리뷰 텍스트 입력 → "분석하기" 클릭 → 별점 및 신뢰도 결과 표시

---

## 모델 저장 경로

- 학습 완료된 모델은 `sentiment_model/` 폴더에 저장됩니다.
- 필수 파일 목록:
  - `config.json`
  - `model.safetensors`
  - `tokenizer.json`
  - `tokenizer_config.json`
  - `vocab.txt`

---

## CORS 설정

FastAPI 코드에 다음과 같이 CORS 미들웨어가 추가되어 있습니다:

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # 개발용 전체 허용, 배포 시 도메인 지정 권장
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

## 참고

- 서버 주소(`http://127.0.0.1:8001/predict`)는 프론트엔드의 fetch 요청 URL과 반드시 일치해야 합니다.
- 로컬 테스트 후 배포 시에는 `CORS` 설정과 포트 설정을 주의해야 합니다.

---

## 라이선스

MIT License
