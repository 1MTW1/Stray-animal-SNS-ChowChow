## 유기동물-인간 유대형성 및 입양률증가를 위한 SNS
<img width="9" alt="스크린샷 2024-12-19 오후 8 54 23" src="https://github.com/user-attachments/assets/21e2143b-8e13-4335-80ee-67ca0a9c9e91" />

## 결과
<img 
width="300" 
src="https://github.com/user-attachments/assets/6c4d0218-548e-45e1-9881-a7e4c52d9fdd"
style="display: block; margin: 0 auto; width: 300px;"
/>

## Requirements
- python 3.11

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
* https://huggingface.co/yanolja/EEVE-Korean-Instruct-10.8B-v1.0

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
