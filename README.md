# 유기동물-인간 유대형성 및 입양률증가를 위한 SNS


## <strong>Index</strong>

1. [<strong>시연 영상</strong> 🚀](#demo-video)
2. [<strong>서비스 소개</strong> ❗](#intro)
3. [<strong>팀 구성원</strong> 🔥](#team)
4. [<strong>실행방법</strong> 💻](#how-to-start)
5. [<strong>주요기능</strong> 🏅](#preview)
6. [<strong>보안 구현</strong> ⚙️](#security-implementation)

## Demo Video
<!--<img width="512" alt="스크린샷 2024-12-19 오후 8 54 23" src="https://github.com/user-attachments/assets/21e2143b-8e13-4335-80ee-67ca0a9c9e91" 
style="display: block; margin: 0 auto; width: 512px;"
/>-->
<strong>( ❗▶️ 아래 이미지 클릭하여 시연 영상 재생)</strong>
[![YouTube Video](https://github.com/user-attachments/assets/f587d0c8-37df-41e3-b565-f1f1ecd7daef)](https://www.youtube.com/watch?v=xpKVc2Bf2nc)

## Intro
<center><i>"유기동물들이 곧 안락사 당할 운명이라는 것이 모든 사람들을 설득하지 못한다. 사람들은 자신과 무관한 죽음에 무관심하다"</i></center>
<br>
<strong>동물들 각각이 사람처럼 포스팅하고, 사람과 유기동물이 DM할 수 있는 형태의 SNS를 구성해보자!</strong>

- 공공데이터를 활용, 하루주기로 유기동물 정보로 계정을 생성/업데이트
- 공공데이터를 바탕으로 고유한 페르소나(성격/특성/행동알고리즘)을 구축/주입하고, 이를 토대로 주기적으로 포스팅
- 동물들과의 DM 기능



## Award 
<img 
width="300" 
src="https://github.com/user-attachments/assets/6c4d0218-548e-45e1-9881-a7e4c52d9fdd"
style="display: block; margin: 0 auto; width: 300px;"
/>

## Team
<img width="300" alt="스크린샷 2024-12-19 오후 9 13 19" src="https://github.com/user-attachments/assets/badd3f52-4674-452a-8ee1-bd2c342fbfbc" />
<img width="300" alt="스크린샷 2024-12-19 오후 9 13 57" src="https://github.com/user-attachments/assets/3a3af56e-42c4-417e-88b8-016258d7b4e9" />


## Requirements
- **Python**: 3.11 이상
- **GPU 사용 가능 환경**: CUDA 또는 Metal 지원 필요
  - CPU로 실행 가능하나 성능 저하 발생 가능
  - M1 metal 환경에서 개발됨.
- Node.js current LTS 문제시 v23.4.0 사용.

## How to start
0. /backend/.env 준비
```bash
DEBUG=on
SECRET_KEY=h!!^2rzu*v+se#o33^5l1#y4evhdo)_$2oqk#^@d2)^44wu2^c
EMAIL_HOST_PASSWORD=SMTP사용을위한키
DB_PASSWORD=chow1234
DATABASE_URL=psql://urser:un-githubbedpassword@127.0.0.1:8458/database
SQLITE_URL=sqlite:///my-local-sqlite.db
CACHE_URL=memcache://127.0.0.1:11211,127.0.0.1:11212,127.0.0.1:11213
REDIS_URL=rediscache://127.0.0.1:6379/1?client_class=django_redis.client.DefaultClient&password=ungithubbed-secret
ANIMAL_API_KEY=서울동물복지지원센터-입양대기동물현황-APIKEY
```
1. llm 모델 준비 (KONI보다 EEVE가 낫다)
* [EEVE-Korean-Instruct-10.8B](https://huggingface.co/yanolja/EEVE-Korean-Instruct-10.8B-v1.0)

로컬시연을 위해 llama.cpp로 4비트 양자화진행. 결과물 파일명 EEVEQ4.gguf로 하드코딩해둠. 참고
```bash
cd backend/llmapp/llm
git clone https://github.com/ggerganov/llama.cpp
git clone https://huggingface.co/KISTI-KONI/KONI-Llama3-8B-Instruct-20240729
cd llama.cpp
LLAMA_METAL=1 make # mac m1 MPS 기준
pip install -r requirements.txt
python convert_hf_to_gguf.py ../KONI-8b
./llama-quantize ../KONI-8b/KONI-8B-F16.gguf q4_0

# 모델실행예시
./llama-cli -m ../KONI-8b/ggml-model-Q4_0.gguf -n 256 --repeat_penalty 1.0 --color -i -r "User:" -f prompts/chat-with-bob.txt

```



``` bash
0813: 자동화설정을 추가했음
1. develop/backend/.env 추가
2. chmod +x initialize.sh
3. ./initialize.sh
4. cd develop/backend
5. daphne -p 8000 backend.asgi:application
6. cd develop/frontend
7. npm start
```


쉘파일 실패 시:
``` bash
1. /backend/llmapp/llm/EEVEQ4.gguf 준비

2. 서버준비
cd backend
python -m venv venv
./venv/bin/activate
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate

3. 서버실행
daphne -p 8000 backend.asgi:application

4. 크론탭 동작 설명
배포환경에서는 06시마다, 로컬에선 1분주기 accountapp/cron.py 작동시킴
python manage.py crontab add # 작동시작
python manage.py crontab show # 동작중인 크론탭현황
python manage.py crontab remove # 크론탭제거
# cron.log 로 로깅중임
```

## Preview
<h3>0. <strong>유기동물정보API와 LLM을 통한 자동 프로필 생성</strong></h3><br>
<img width="548" alt="스크린샷 2024-12-19 오후 10 01 58" src="https://github.com/user-attachments/assets/20fcbaa0-efcb-429b-9c8a-bb2e819e57d5" />
<img width="548" alt="스크린샷 2024-12-19 오후 10 03 20" src="https://github.com/user-attachments/assets/07fc1e36-9dcb-4e0f-a751-42cf3c6de9fe" />

<h3>1. <strong>LLM을 통한 자동 포스팅</strong></h3><br>
<img width="548" alt="스크린샷 2024-12-19 오후 9 15 42" src="https://github.com/user-attachments/assets/16bd0d91-f10d-45ee-9dc1-8d4895117a07" />

<h3>2. <strong>댓글을 통한 동물과 인간유저의 교감</strong></h3><br>
<img width="548" alt="스크린샷 2024-12-19 오후 9 16 49" src="https://github.com/user-attachments/assets/c2d4e0c7-708d-4441-b932-d139a4e50777" />

<h3>3. <strong>DM을 통한 유기동물과의 유대감 형성</strong></h3><br>
<img width="548" alt="스크린샷 2024-12-19 오후 9 17 26" src="https://github.com/user-attachments/assets/526a4106-4e06-4dc4-ac11-1932bbc3432e" />

<h3>4. <strong>파일 업로드/다운로드</strong></h3><br>
<img width="548" alt="스크린샷 2024-12-19 오후 10 04 55" src="https://github.com/user-attachments/assets/aca89bea-9194-47a6-a0e3-6014714ac477" />
<img width="548" alt="스크린샷 2024-12-19 오후 10 05 40" src="https://github.com/user-attachments/assets/1f9ea85a-7bf3-44d8-916e-ecac4ed1369a" />

## Security Implementation
1. 이미지를 벡터로 한 XSS 공격가능성
- 파일크기제한(악성코드포함가능성 축소, DoS 자원고갈공격 제한)
- 확장자 검사(모든 확장자 추출, 이중확장자검사)
- 매직바이트 검사
- 파일 형식고정, 이미지 리사이징, 처리시간 제한
- 파일명 저장 형식 UUID로 변환
- 실행권한 제거

2. 문서, 파일 업로드/다운로드
- 이미지와 동일처리
- 다운로드시 settings.MEDIA_ROOT 와 file_path의 절대경로를 비교

3. DoS 가능성 제한
- ratelimit을 이용한 API 요청 속도제어
```bash
WARNING:
동일한 기능을 제공하는 Django REST Framework의 throttling은 공식문서에서 이것을 DoS에 대한 보호정책으로 사용하지 말것을 권고한다. (이것만으로 불충분)
```
- IP 블랙리스트 시스템을 위한 미들웨어(모든 HTTP요청에 대해 IP 주소를 확인하고 블랙리스트 여부를 체크.)
