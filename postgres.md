⏺ PostgreSQL 설정 상세 가이드 (EC2 Ubuntu)

  1. PostgreSQL 설치

  # 시스템 업데이트
  sudo apt update && sudo apt upgrade -y

  # PostgreSQL 15 설치
  sudo apt install -y postgresql-15 postgresql-contrib-15

  # 서비스 상태 확인
  sudo systemctl status postgresql

  # 자동 시작 설정
  sudo systemctl enable postgresql

  2. PostgreSQL 사용자 및 데이터베이스 생성

  # postgres 사용자로 전환
  sudo -i -u postgres

  # psql 접속
  psql

  -- 데이터베이스 사용자 생성 (비밀번호 설정)
  CREATE USER loandoc_user WITH PASSWORD 'your_secure_password_here';

  -- 데이터베이스 생성
  CREATE DATABASE loandoc_staging OWNER loandoc_user;

  -- 사용자에게 권한 부여
  GRANT ALL PRIVILEGES ON DATABASE loandoc_staging TO loandoc_user;

  -- 스키마 권한 부여 (only PostgreSQL 15+)
  \c loandoc_staging
  GRANT ALL ON SCHEMA public TO loandoc_user;

  -- 확인
  \l   -- 데이터베이스 목록
  \du  -- 사용자 목록

  -- 종료
  \q

  # postgres 사용자에서 나가기
  exit

  3. PostgreSQL 설정 파일 수정

  3.1 postgresql.conf (연결 설정)

  # 설정 파일 위치 확인
  sudo -u postgres psql -c "SHOW config_file;"
  # 보통: /etc/postgresql/14/main/postgresql.conf

  # 설정 파일 편집
  sudo vim /etc/postgresql/14/main/postgresql.conf

  # 변경할 설정들:

  # 로컬만 허용 (보안상 권장)
  listen_addresses = 'localhost'

  # 포트 (기본값 유지)
  port = 5432

  # 최대 연결 수 (staging용 적당히)
  max_connections = 100

  # 메모리 설정 (EC2 t3.small 기준: 2GB RAM)
  shared_buffers = 512MB
  effective_cache_size = 1GB
  work_mem = 16MB
  maintenance_work_mem = 128MB

  # 로깅 (디버깅용)
  logging_collector = on
  log_directory = 'pg_log'
  log_filename = 'postgresql-%Y-%m-%d.log'
  log_statement = 'ddl'  # DDL 문만 로깅

  3.2 pg_hba.conf (인증 설정)

  # 인증 설정 파일 편집
  sudo vim /etc/postgresql/14/main/pg_hba.conf

  # 파일 끝에 추가 (기존 설정 아래에):

  # TYPE  DATABASE        USER            ADDRESS                 METHOD

  # 로컬 소켓 연결 (서버 내부)
  local   loandoc_staging loandoc_user                            scram-sha-256

  # localhost TCP 연결 (애플리케이션)
  host    loandoc_staging loandoc_user    127.0.0.1/32            scram-sha-256
  host    loandoc_staging loandoc_user    ::1/128                 scram-sha-256

  # ⚠️ 외부 접속이 필요한 경우만 (보안 주의!)
  # host    loandoc_staging loandoc_user    0.0.0.0/0               scram-sha-256

  3.3 설정 적용

  # PostgreSQL 재시작
  sudo systemctl restart postgresql

  # 상태 확인
  sudo systemctl status postgresql

  4. 연결 테스트

  # 로컬 연결 테스트
  psql -h localhost -U loandoc_user -d loandoc_staging

  # 비밀번호 입력 후 접속되면 성공
  # \q 로 종료

  5. DATABASE_URL 형식

  # .env 파일에 추가
  DATABASE_URL="postgresql://loandoc_user:your_secure_password_here@localhost:5432/loandoc_staging?schema=public"

  6. Prisma 마이그레이션 실행

  # 프로젝트 디렉토리에서
  cd /home/ubuntu/LoanDoc

  # 마이그레이션 실행
  pnpm db:migrate

  # 또는 Prisma CLI 직접 사용
  npx prisma migrate deploy

  # Prisma Client 생성
  pnpm db:generate

  7. 백업 설정 (선택사항)

  7.1 수동 백업

  # 백업 디렉토리 생성
  sudo mkdir -p /var/backups/postgresql
  sudo chown postgres:postgres /var/backups/postgresql

  # 백업 실행
  sudo -u postgres pg_dump loandoc_staging > /var/backups/postgresql/loandoc_staging_$(date +%Y%m%d_%H%M%S).sql

  # 복원
  sudo -u postgres psql loandoc_staging < /var/backups/postgresql/backup_file.sql

  7.2 자동 백업 (Cron)

  # 백업 스크립트 생성
  sudo nano /usr/local/bin/backup_postgres.sh

  #!/bin/bash
  BACKUP_DIR="/var/backups/postgresql"
  TIMESTAMP=$(date +%Y%m%d_%H%M%S)
  DBNAME="loandoc_staging"

  # 백업 실행
  pg_dump $DBNAME > $BACKUP_DIR/${DBNAME}_${TIMESTAMP}.sql

  # 7일 이상된 백업 삭제
  find $BACKUP_DIR -name "*.sql" -mtime +7 -delete

  # 실행 권한 부여
  sudo chmod +x /usr/local/bin/backup_postgres.sh

  # Cron 등록 (매일 새벽 3시)
  sudo crontab -u postgres -e
  # 추가: 0 3 * * * /usr/local/bin/backup_postgres.sh

  8. 보안 체크리스트

  | 항목               | 상태          | 설명                           |
  |--------------------|---------------|--------------------------------|
  | listen_addresses   | localhost     | 외부 직접 접속 차단            |
  | 강력한 비밀번호    | ✓             | 특수문자 포함 16자 이상        |
  | pg_hba.conf        | scram-sha-256 | 최신 인증 방식 사용            |
  | EC2 Security Group | 5432 차단     | PostgreSQL 포트 외부 노출 안함 |
  | 정기 백업          | Cron 설정     | 데이터 손실 방지               |

  9. 유용한 명령어

  # PostgreSQL 버전 확인
  psql --version

  # 서비스 상태
  sudo systemctl status postgresql

  # 로그 확인
  sudo tail -f /var/log/postgresql/postgresql-15-main.log

  # 연결된 클라이언트 확인
  sudo -u postgres psql -c "SELECT * FROM pg_stat_activity WHERE datname='loandoc_staging';"

  # 데이터베이스 크기 확인
  sudo -u postgres psql -c "SELECT pg_size_pretty(pg_database_size('loandoc_staging'));"

  # 테이블 목록 확인
  psql -h localhost -U loandoc_user -d loandoc_staging -c "\dt"

  10. 문제 해결

  연결 실패 시

  # 1. PostgreSQL 실행 중인지 확인
  sudo systemctl status postgresql

  # 2. 포트 확인
  sudo netstat -tlnp | grep 5432

  # 3. 로그 확인
  sudo tail -50 /var/log/postgresql/postgresql-15-main.log

  # 4. pg_hba.conf 설정 확인
  sudo cat /etc/postgresql/15/main/pg_hba.conf | grep loandoc

  권한 오류 시

  # postgres 사용자로 접속 후
  sudo -u postgres psql

  # 권한 재부여
  GRANT ALL PRIVILEGES ON DATABASE loandoc_staging TO loandoc_user;
  \c loandoc_staging
  GRANT ALL ON SCHEMA public TO loandoc_user;
  GRANT ALL ON ALL TABLES IN SCHEMA public TO loandoc_user;
  GRANT ALL ON ALL SEQUENCES IN SCHEMA public TO loandoc_user;