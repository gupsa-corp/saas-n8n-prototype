# 한국어 n8n Docker 설정 가이드

## 📋 준비된 파일들

- `ko.json` - 한국어 번역 파일 (`packages/frontend/@n8n/i18n/src/locales/ko.json`)
- `Dockerfile.n8n-korean` - 한국어가 포함된 커스텀 n8n 이미지
- `docker-compose-n8n-korean.yml` - 한국어 n8n을 위한 Docker Compose 설정

## 🚀 사용 방법

### 방법 1: 커스텀 이미지 빌드 사용 (권장)

1. **커스텀 이미지 빌드 및 실행**
   ```bash
   # 한국어 n8n 이미지 빌드 및 실행
   docker-compose -f docker-compose-n8n-korean.yml up --build -d
   ```

2. **접속**
   - URL: http://localhost:6789
   - 사용자명: admin
   - 비밀번호: admin123

### 방법 2: 볼륨 매핑 사용

1. **기존 방법으로 실행**
   ```bash
   # 볼륨 매핑을 사용한 방법
   docker-compose -f docker-compose-n8n-build-after.yml up -d
   ```

## 🛠️ 주요 설정

### 환경 변수
- `N8N_DEFAULT_LOCALE=ko` - 기본 언어를 한국어로 설정
- `GENERIC_TIMEZONE=Asia/Seoul` - 시간대를 한국 시간으로 설정
- `N8N_BASIC_AUTH_ACTIVE=true` - 기본 인증 활성화

### 포트 설정
- PostgreSQL: 4567 (외부) → 5432 (내부)
- n8n: 6789 (외부) → 5678 (내부)

## 🔧 관리 명령어

### 컨테이너 상태 확인
```bash
docker-compose -f docker-compose-n8n-korean.yml ps
```

### 로그 확인
```bash
# 모든 서비스 로그
docker-compose -f docker-compose-n8n-korean.yml logs

# n8n만 로그 확인
docker-compose -f docker-compose-n8n-korean.yml logs n8n

# 실시간 로그 확인
docker-compose -f docker-compose-n8n-korean.yml logs -f n8n
```

### 중지 및 재시작
```bash
# 중지
docker-compose -f docker-compose-n8n-korean.yml down

# 재시작 (데이터 보존)
docker-compose -f docker-compose-n8n-korean.yml restart

# 완전 정리 (데이터도 삭제)
docker-compose -f docker-compose-n8n-korean.yml down -v
```

### 이미지 재빌드
```bash
# 캐시 없이 이미지 재빌드
docker-compose -f docker-compose-n8n-korean.yml build --no-cache

# 재빌드 후 실행
docker-compose -f docker-compose-n8n-korean.yml up --build -d
```

## 📊 데이터베이스 접속 정보

PostgreSQL 데이터베이스에 직접 접속하고 싶다면:

```bash
# Docker 컨테이너 내에서 접속
docker exec -it postgres psql -U n8n -d n8n

# 외부에서 접속 (psql 클라이언트 필요)
psql -h localhost -p 4567 -U n8n -d n8n
```

## 📝 번역 파일 수정

`ko.json` 파일을 수정한 후 변경사항을 적용하려면:

1. **번역 파일 수정**
   ```bash
   # ko.json 파일 편집
   code packages/frontend/@n8n/i18n/src/locales/ko.json
   ```

2. **이미지 재빌드**
   ```bash
   docker-compose -f docker-compose-n8n-korean.yml build --no-cache n8n
   docker-compose -f docker-compose-n8n-korean.yml up -d
   ```

## 🔍 트러블슈팅

### 한국어가 적용되지 않는 경우
1. 브라우저 캐시 삭제
2. 브라우저의 언어 설정 확인
3. 컨테이너 재시작

### 접속이 안 되는 경우
1. 포트 충돌 확인: `netstat -tulpn | grep 6789`
2. 방화벽 설정 확인
3. Docker 서비스 상태 확인: `docker ps`

### 데이터베이스 연결 오류
1. PostgreSQL 컨테이너 상태 확인
2. 헬스체크 로그 확인
3. 네트워크 연결 확인

## 📚 추가 정보

- [n8n 공식 문서](https://docs.n8n.io/)
- [Docker Compose 공식 문서](https://docs.docker.com/compose/)
- [PostgreSQL 공식 문서](https://www.postgresql.org/docs/)
