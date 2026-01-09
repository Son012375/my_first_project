# 개발 환경 구축 학습 노트

## 1. Git & GitHub

### Git이란?
- 코드의 **버전 관리** 도구
- 변경 이력 저장, 실수 시 되돌리기 가능
- 협업 및 백업에 필수

### 기본 명령어
```bash
git clone <URL>          # GitHub 저장소를 로컬로 복사
git status               # 현재 상태 확인
git add <파일>           # 변경 파일을 커밋 준비
git commit -m "메시지"   # 변경 사항 저장 (스냅샷)
git push origin main     # GitHub에 업로드
git pull                 # GitHub에서 다운로드
```

### 설정 정보
- GitHub 계정: Son012375
- 저장소: https://github.com/Son012375/my_first_project
- 인증: Personal Access Token 사용 (Settings > Developer settings > Tokens)

---

## 2. PostgreSQL 데이터베이스

### PostgreSQL이란?
- **관계형 데이터베이스** (RDBMS)
- 데이터를 표(테이블) 형태로 저장
- SQL 언어로 데이터 조작

### 설치 및 시작
```bash
# 설치 (Ubuntu/WSL)
sudo apt update && sudo apt install -y postgresql postgresql-contrib

# 서비스 시작
sudo service postgresql start

# 사용자 및 DB 생성 (최초 1회)
sudo -u postgres createuser --superuser $USER
sudo -u postgres createdb $USER
```

### 기본 명령어
```bash
psql -c "\l"                    # 데이터베이스 목록 보기
psql -d research_db             # 특정 DB 접속
psql -d research_db -c "SQL문"  # SQL 실행
```

### SQL 기본 문법
```sql
-- 데이터베이스 생성
CREATE DATABASE research_db;

-- 테이블 생성
CREATE TABLE projects (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    status VARCHAR(50) DEFAULT 'ongoing',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 데이터 추가
INSERT INTO projects (name, description, status)
VALUES ('프로젝트명', '설명', 'ongoing');

-- 데이터 조회
SELECT * FROM projects;

-- 조건 조회
SELECT * FROM projects WHERE status = 'ongoing';

-- 데이터 수정
UPDATE projects SET status = 'completed' WHERE id = 1;

-- 데이터 삭제
DELETE FROM projects WHERE id = 1;
```

### 현재 구축된 DB 구조
```
research_db (데이터베이스)
└── projects (테이블)
    ├── id          : 고유 번호 (자동 증가)
    ├── name        : 프로젝트명
    ├── description : 설명
    ├── status      : 상태 (ongoing/planned/completed)
    └── created_at  : 생성일시
```

---

## 3. DB는 어떻게 쓰이는가?

### 웹/앱 서비스에서의 역할
```
[사용자] → [프론트엔드] → [백엔드 API] → [데이터베이스]
  화면      React/Vue      FastAPI/Node    PostgreSQL
```

### 실제 활용 예시

1. **연구 관리 시스템**
   - 논문, 실험 데이터, 프로젝트 진행 상황 저장
   - 검색, 필터링, 통계 분석

2. **건축 프로젝트 관리**
   - 도면 정보, 자재 목록, 일정 관리
   - BIM 데이터 메타정보 저장

3. **사용자 인증**
   - 회원 정보, 로그인 기록 저장

### Python에서 DB 사용 (다음 단계)
```python
import psycopg2

# DB 연결
conn = psycopg2.connect(dbname="research_db")
cur = conn.cursor()

# 데이터 조회
cur.execute("SELECT * FROM projects")
rows = cur.fetchall()

for row in rows:
    print(row)

conn.close()
```

---

## 4. 다음 학습 단계

1. **Python + DB 연동** - psycopg2 라이브러리
2. **백엔드 API** - FastAPI로 REST API 구축
3. **프론트엔드** - React/Vue로 화면 개발
4. **배포** - Docker, 클라우드 서버

---

## 5. 유용한 참고 자료

- Git 공식 문서: https://git-scm.com/doc
- PostgreSQL 공식 문서: https://www.postgresql.org/docs/
- SQL 연습: https://www.w3schools.com/sql/

---

## 6. 자주 쓰는 명령어 요약

| 작업 | 명령어 |
|------|--------|
| PostgreSQL 시작 | `sudo service postgresql start` |
| DB 목록 보기 | `psql -c "\l"` |
| 테이블 목록 보기 | `psql -d research_db -c "\dt"` |
| Git 상태 확인 | `git status` |
| GitHub에 올리기 | `git add . && git commit -m "메시지" && git push` |
