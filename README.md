# personal work desktop 🚀

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.109+-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16+-336791?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org/)

<p align="center">
<img width="30%" height="30%" alt="스크린샷 2026-02-15 오후 5 04 34" src="https://github.com/user-attachments/assets/a9edb2c9-39d0-4bdf-abd5-2640c397d252" />
<img width="30%" height="30%" alt="스크린샷 2026-02-15 오후 5 04 11" src="https://github.com/user-attachments/assets/6a4a117e-59a0-42e4-9545-d7bbf566e9e7" />
<img width="30%" height="30%" alt="스크린샷 2026-02-15 오후 5 04 02" src="https://github.com/user-attachments/assets/ce1ab55f-dad5-4bbe-ba9d-f8cf9569514c" />
</p>

**personal work desktop**는 파워 유저를 위한 고성능 멀티 보드 작업 관리 및 지식 허브입니다. FastAPI 백엔드와 고급 검색 기능(FTS + Trigram + Semantic), 그리고 경량의 Vanilla JS 프론트엔드를 기반으로 구성됩니다.

---

## ✨ 주요 기능

- **🚀 성능 중심 아키텍처**: **FastAPI** + **PostgreSQL** 기반으로 속도와 안정성을 확보했습니다.
- **🔍 하이브리드 검색 엔진**: **전문 검색(FTS)**, **트라이그램 유사도 검색**, **벡터 의미 검색(`pgvector`)**을 통합 제공합니다.
- **🧱 WS15 PostgreSQL 강화**: 검색 도메인 스키마를 `TIMESTAMPTZ` + `JSONB`로 표준화하고 롤백 SQL 검증 경로를 갖췄습니다.
- **📊 3D 지식 그래프**: 작업/태그/문서 관계를 `force-graph-3d` 기반으로 시각화합니다.
- **📋 멀티 보드 칸반**: 보드별 데이터(Task/Tag/Settings/Stats) 격리 운영을 지원합니다.
- **🎨 프리미엄 UX**: 다크 테마 중심 디자인, 반응형 레이아웃, 키보드 친화적 상호작용을 제공합니다.
- **🧠 시맨틱 인텔리전스**: 작업/문서 임베딩 생성으로 의미 기반 검색을 지원합니다.
- **🐳 Docker 지원**: 컨테이너 기반 로컬/운영 환경 구성이 가능합니다.

## 🛠️ 기술 스택

| 구성 요소 | 기술 | 설명 |
|-----------|------------|-------------|
| **백엔드** | ![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white) | Pydantic 검증 기반 고성능 비동기 API |
| **데이터베이스** | ![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=flat-square&logo=postgresql&logoColor=white) | `pgvector` 및 `pg_trgm` 확장을 사용하는 기본 저장소 |
| **프론트엔드** | ![JavaScript](https://img.shields.io/badge/-Vanilla%20JS-F7DF1E?style=flat-square&logo=javascript&logoColor=black) | 프레임워크 종속성이 없는 경량 UI |
| **테스트** | ![Pytest](https://img.shields.io/badge/-Pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white) | API 통합 테스트 + Playwright E2E |

## 📐 아키텍처

```mermaid
graph TD
    Client["프론트엔드 (Vanilla JS)"] -->|"REST API"| API["FastAPI 백엔드"]
    subgraph DataLayer["데이터 계층"]
        API -->|"SQL + Vector"| DB[("PostgreSQL 16+")]
        DB -->|"벡터"| PGVector["pgvector 확장"]
        DB -->|"FTS/Trigram"| SearchIdx["검색 인덱스"]
    end
    API -->|"S3 프로토콜"| Storage["오브젝트 스토리지 / 로컬"]
```

## 🚀 시작하기

### 사전 준비

- **Docker & Docker Compose** (가장 빠른 실행 권장)
- 또는 **Python 3.11+ + PostgreSQL 16+** (로컬 개발 환경)

### 빠른 시작 (Docker)

1.  **저장소 클론**
    ```bash
    git clone https://github.com/coreline-ai/personal_work_desktop.git
    cd personal_work_desktop
    ```

2.  **Docker Compose 실행**
    ```bash
    cp .env.example .env
    docker-compose up -d --build
    ```

3.  **접속 주소**
    -   **API Base**: `http://localhost:18000/api`
    -   **API 문서**: `http://localhost:18000/docs`
    -   **MinIO 콘솔**: `http://localhost:9001`

4.  **프론트 정적 서버 실행 (Docker 스택과 별도)**
    ```bash
    python -m http.server 18080 --directory web
    ```
    -   **프론트엔드**: `http://localhost:18080`

### 수동 설치 (로컬 개발)

<details>
<summary>단계별 실행 가이드 펼치기</summary>

#### 1. 개발 환경 구성
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

#### 2. DB 설정
PostgreSQL 실행 후 환경변수를 지정하세요. (런타임은 PostgreSQL 전용)
```bash
export APP_DB_BACKEND=postgres
export DATABASE_URL='postgresql://user:password@localhost:5432/dbname'
```

#### 3. 마이그레이션 및 초기화
```bash
# 스키마 마이그레이션 적용
python scripts/apply_migrations.py

# (선택) 기존 데이터가 있다면 검색 인덱스 재구성
python scripts/rebuild_search_index.py --with-embeddings

# (권장) 역순 롤백 SQL 트랜잭션 검증
python scripts/verify_postgres_search_rollback.py
```

#### 4. 백엔드 실행
```bash
uvicorn app.main:app --host 127.0.0.1 --port 18000 --reload
```

#### 5. 프론트 실행
```bash
python -m http.server 18080 --directory web
```
</details>

## 🧭 런타임 정책 (WS15)

- 런타임 DB 백엔드는 **PostgreSQL만 지원**합니다.
- 런타임/진단 스크립트 실행 시 `DATABASE_URL`이 필요합니다.
- SQLite 런타임 fallback은 제거되었습니다.
- `scripts/migrate_sqlite_to_postgres.py`, `scripts/validate_postgres_integrity.py`는 데이터 이관/무결성 검증 용도로만 유지됩니다.

## 🔎 통합 검색 / 임베딩

```bash
# 통합 검색 인덱스 재생성
python scripts/rebuild_search_index.py

# 인덱스 + 임베딩 재생성
python scripts/rebuild_search_index.py --with-embeddings --embedding-force

# 임베딩만 재생성
python scripts/rebuild_search_embeddings.py --force
```

의미검색 관련 선택 플래그:

```bash
export APP_SEARCH_ENABLE_PGVECTOR=true
export APP_SEARCH_EMBED_PROVIDER=local_hash
export APP_SEARCH_EMBED_DIM=1536
export APP_SEARCH_EMBED_LAZY_INDEX=true
export APP_SEARCH_LEXICAL_TOPK=1200
```

## 📚 문서

-   **[개발 계획](docs/DEVELOPMENT_PLAN.md)**: 로드맵 및 기능 진행 현황
-   **[API 변경 이력](docs/API_CHANGELOG.md)**: API 변경 내역 정리
-   **[멀티 보드 가이드](docs/BOARD_MULTI_WORKSPACE_TASKS.md)**: 멀티 워크스페이스 운영 가이드
-   **[배포 가이드](docs/DEPLOYMENT.md)**: 로컬/Docker 배포 절차
-   **[WS15 컷오버 런북](docs/WS15_POSTGRES_CUTOVER_RUNBOOK.md)**: PostgreSQL 전환/롤백 절차

## 🧪 테스트

시스템 무결성 확인을 위해 아래 테스트를 실행하세요.

```bash
# 전체 테스트
pytest -q

# API 통합 테스트만 실행
pytest -q tests/test_api.py

# 프론트 E2E 테스트만 실행
pytest -q tests/test_frontend_e2e.py
```

PostgreSQL 기준 전체 테스트 실행:

```bash
export TEST_DATABASE_URL='postgresql://super_desktop:super_desktop@127.0.0.1:5432/super_desktop'
pytest -q
```

## 📈 운영 / 성능

```bash
# 런타임 스모크 테스트
python scripts/smoke_check.py --api-base http://127.0.0.1:18000/api

# 환경 진단
python scripts/diagnose_env.py

# 역순 롤백 SQL 트랜잭션 검증 (영구 변경 없음)
python scripts/verify_postgres_search_rollback.py

# 통합 검색 벤치마크 + 리포트
python scripts/benchmark_search_p95.py \
  --api-base http://127.0.0.1:18000/api \
  --seed-min-tasks 2000 \
  --requests 120 \
  --warmup 12 \
  --report-json reports/search_benchmark.json \
  --report-md reports/search_benchmark.md

# 벤치마크 요약 (병목 쿼리 Top + 개선 제안)
python scripts/summarize_search_benchmark.py \
  --report-json reports/search_benchmark.json \
  --top-n 10 \
  --min-requests 5 \
  --output-json reports/search_benchmark.summary.json \
  --output-md reports/search_benchmark.summary.md
```

## 📌 현재 WS15 상태 (2026-02-15)

- WS15 컷오버 체크리스트 중 **스키마/마이그레이션 일원화, 롤백 검증, PostgreSQL 전용 런타임**은 완료되었습니다.
- 최근 벤치마크(2k seed / 120 req): **p95 약 425ms**.
- 남은 목표: **10k 문서 기준 p95 < 300ms** (`docs/DEVELOPMENT_PLAN.md`의 지속 튜닝 항목).

## 🤝 기여

이슈 제보 및 PR 기여를 환영합니다.

1.  저장소 Fork
2.  브랜치 생성 (`git checkout -b feature/AmazingFeature`)
3.  변경 커밋 (`git commit -m 'Add some AmazingFeature'`)
4.  원격 브랜치 푸시 (`git push origin feature/AmazingFeature`)
5.  Pull Request 생성

## 📄 라이선스

현재 저장소에는 라이선스 파일이 아직 포함되어 있지 않습니다.
