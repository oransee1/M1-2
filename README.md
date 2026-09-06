# 🤖 AI Agent: 나만의 AI 비서 & Suno AI 음원 생성 스튜디오

> **"내 일상과 컨디션을 깊이 이해하고, 나만을 위한 맞춤형 힐링 음악(Suno AI)을 기획하여 내 PC로 다운로드해 주는 AI 비서"**

---

## 1. 📌 서비스 소개 (Service Overview)

일반적인 ChatGPT는 사용자의 개인적인 일상 기록이나 컨디션 변화를 알지 못해 일방적인 대답만 제공합니다.  
본 서비스는 **사용자의 시계열 컨디션/기분 데이터(날짜, 지수, 메모)를 저장 및 요약**하고, 이를 **AI 시스템 프롬프트에 주입(Context Injection)**하여 사용자의 현재 상태를 완벽히 파악한 답변을 제공합니다.

또한, 대화 중 파악한 기분과 컨디션 키워드를 바탕으로 **10가지 Suno AI 맞춤 음악 프롬프트를 자동 기획**하고, **Apiframe Suno API v2를 통해 음원을 즉시 생성**한 뒤 **내 PC의 지정 폴더(`C:\Users\DiCiA\PycharmProjects\M1-2_Project\Output\날짜별폴더`)로 다운로드**받을 수 있는 원스톱 AI 비서 서비스를 제공합니다.

---

## 2. 🛠️ 기술 스택 (Tech Stack)

| 구분 | 사용 기술 / 라이브러리 |
| :--- | :--- |
| **Backend** | Python 3.10+, FastAPI, Uvicorn, Pydantic v2 |
| **Database** | Firebase Firestore (`firebase-admin`), Local Fallback Store |
| **AI Models** | OpenAI API (`gpt-4o-mini`), Google Gemini API (`gemini-2.0-flash`) |
| **Music AI** | Suno AI via Apiframe API v2 (`/v2/music/generate`, `/v2/jobs/{id}`) |
| **Frontend** | Vanilla HTML5, Modern CSS3 (Glassmorphism, Dark/Light Theme), JavaScript (ES6+), Chart.js |
| **Deployment** | Render (백엔드 Web Service), Vercel (프론트엔드 Static Site) |

---

## 3. 🌐 배포 URL 안내 (Deployment URLs)

- **프론트엔드 서비스 (Vercel)**: `https://a1-3-project.vercel.app/` (또는 수동 배포 URL)
- **백엔드 API 서버 (Render)**: `https://ai-assistant-backend.onrender.com`
- **Swagger API 문서**: `https://ai-assistant-backend.onrender.com/docs`

> 💡 **Render 무료 티어 콜드 스타트 안내**: Render 무료 서비스 특성상 약 15분간 요청이 없으면 서버가 잠자기에 들어갑니다. 첫 접속 시 30초~1분 정도 지연이 발생할 수 있습니다.

---

## 4. 🚀 로컬 실행 방법 (Local Setup)

### 1) 가상환경 생성 및 패키지 설치
```bash
# 가상환경 생성
python -m venv .venv

# 가상환경 활성화 (Windows PowerShell)
.venv\Scripts\Activate.ps1

# 라이브러리 설치
pip install -r requirements.txt
```

### 2) 환경 변수 설정 (`.env`)
프로젝트 루트 경로에 `.env` 파일을 생성하고 아래 키 항목을 입력합니다:
```env
OPENAI_API_KEY=your_openai_api_key
GEMINI_API_KEY=your_gemini_api_key
APIFRAME_API_KEY=your_apiframe_api_key
FIREBASE_SERVICE_ACCOUNT_JSON=firebase-service-account.json
ALLOWED_ORIGINS=*
MUSIC_DOWNLOAD_DIR=C:\Users\DiCiA\Downloads\SunoMusic
```

### 3) 백엔드 서버 실행 (웹 버전)
```bash
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```
- 서버 접속: `http://localhost:8000` 또는 `http://127.0.0.1:8000`
- Swagger 문서: `http://localhost:8000/docs`

### 4) PyQt5 데스크톱 GUI 애플리케이션 실행 (추천 🌟)
```bash
python app_gui.py
```
- 브라우저나 별도 웹서버 없이 네이티브 윈도우 창으로 AI 대화, 컨디션 데이터 CRUD, 차트 시각화, Suno 음원 기획/생성/플레이어/PC 다운로드를 모두 실행할 수 있습니다.

---


## 5. 🔑 환경 변수 목록 (Environment Variables)

| 환경 변수명 | 설명 | 필터/기본값 |
| :--- | :--- | :--- |
| `OPENAI_API_KEY` | OpenAI GPT API 호출 키 | 선택 (Gemini로 자동 폴백) |
| `GEMINI_API_KEY` | Google Gemini API 호출 키 | 필수 (Suno 프롬프트 10종 기획) |
| `APIFRAME_API_KEY` | Apiframe Suno v2 API 키 | 필수 (Suno 음원 생성) |
| `FIREBASE_SERVICE_ACCOUNT_JSON` | Firebase Firestore 서비스 계정 JSON 경로/문자열 | 선택 (미설정 시 로컬 DB 폴백) |
| `ALLOWED_ORIGINS` | CORS 허용 도메인 목록 | `*` 또는 프론트엔드 도메인 |
| `MUSIC_DOWNLOAD_DIR` | 내 PC 음원 다운로드 저장 기본 폴더 경로 | `C:\Users\DiCiA\Downloads\SunoMusic` |

---

## 6. 📸 제출용 핵심 기능 스크린샷 안내 (Screenshots)

### 1) 데이터 요약이 보이는 AI 채팅 화면
- **기능**: Firestore에서 계산된 사용자의 컨디션 평균(예: 7.2점), 데이터 기간, 추세 정보가 상단 배지 및 시스템 프롬프트에 주입되어 AI가 맞춤 대화를 나누고 10가지 Suno 음악을 추천함.

### 2) 데이터 관리 화면 (CRUD)
- **기능**: (date, value, memo) 시계열 데이터 추가/수정/삭제 동작 및 통계 지표 카드 + Chart.js 시각화 그래프 렌더링.

### 3) 대화 기록 화면 (History & Load)
- **기능**: Firestore에 자동 저장된 이전 대화 목록 조회 및 `[불러오기]` 버튼 클릭 시 메시지 내용 재표시.

### 4) Suno AI 맞춤 음원 생성 & 내 PC 다운로드 화면
- **기능**: 기분/컨디션 기반 10가지 Suno 음악 프롬프트 실시간 생성 ➔ Suno API(Apiframe v2) 호출 ➔ MP3 음원 생성 완료 후 `[내 PC 지정 폴더로 저장]` 클릭 시 지정 경로로 다운로드.
