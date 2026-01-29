staging.loandoctor.co.kr
staging.api.loandoctor.co.kr
staging.admin.loandoctor.co.kr
staging.admin-api.loandoctor.co.kr

  sudo ln -s /etc/nginx/sites-available/staging.loandoctor.co.kr /etc/nginx/sites-enabled/

  sudo ln -s /etc/nginx/sites-available/staging.api.loandoctor.co.kr /etc/nginx/sites-enabled/

  sudo ln -s /etc/nginx/sites-available/staging.admin.loandoctor.co.kr /etc/nginx/sites-enabled/

  sudo ln -s /etc/nginx/sites-available/staging.admin-api.loandoctor.co.kr /etc/nginx/sites-enabled/

server {
      listen 80;
      server_name staging.loandoctor.co.kr;
      return 301 https://$server_name$request_uri;
  }

  server {
      listen 443 ssl http2;
      server_name staging.loandoctor.co.kr;

      # SSL
      ssl_certificate /etc/letsencrypt/live/staging.loandoctor.co.kr/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/staging.loandoctor.co.kr/privkey.pem;
      include /etc/letsencrypt/options-ssl-nginx.conf;
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

      # 보안 헤더
      include snippets/security-headers.conf;

      # 로그
      access_log /var/log/nginx/staging.loandoctor.co.kr.access.log;
      error_log /var/log/nginx/staging.loandoctor.co.kr.error.log;

      # 파일 업로드 크기
      client_max_body_size 50M;

      location / {
          proxy_pass http://127.0.0.1:3000;
          proxy_http_version 1.1;

          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 60s;

          proxy_cache_bypass $http_upgrade;
      }
  }


server {
      listen 80;
      server_name staging.api.loandoctor.co.kr;
      return 301 https://$server_name$request_uri;
  }

  server {
      listen 443 ssl http2;
      server_name staging.api.loandoctor.co.kr;

      # SSL
      ssl_certificate /etc/letsencrypt/live/staging.loandoctor.co.kr/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/staging.loandoctor.co.kr/privkey.pem;
      include /etc/letsencrypt/options-ssl-nginx.conf;
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

      # 보안 헤더
      include snippets/security-headers.conf;

      # 로그
      access_log /var/log/nginx/staging.api.loandoctor.co.kr.access.log;
      error_log /var/log/nginx/staging.api.loandoctor.co.kr.error.log;

      # 파일 업로드 크기
      client_max_body_size 50M;

      location / {
          proxy_pass http://127.0.0.1:3001;
          proxy_http_version 1.1;

          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 60s;

          proxy_cache_bypass $http_upgrade;
      }

      location /health {
          proxy_pass http://127.0.0.1:3001/health;
          access_log off;
      }
  }


server {
      listen 80;
      server_name staging.admin.loandoctor.co.kr;
      return 301 https://$server_name$request_uri;
  }

  server {
      listen 443 ssl http2;
      server_name staging.admin.loandoctor.co.kr;

      # SSL
      ssl_certificate /etc/letsencrypt/live/staging.loandoctor.co.kr/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/staging.loandoctor.co.kr/privkey.pem;
      include /etc/letsencrypt/options-ssl-nginx.conf;
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

      # 보안 헤더
      include snippets/security-headers.conf;

      # 로그
      access_log /var/log/nginx/staging.admin.loandoctor.co.kr.access.log;
      error_log /var/log/nginx/staging.admin.loandoctor.co.kr.error.log;

      # 파일 업로드 크기
      client_max_body_size 50M;

      location / {
          proxy_pass http://127.0.0.1:4000;
          proxy_http_version 1.1;

          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 60s;

          proxy_cache_bypass $http_upgrade;
      }
  }



server {
      listen 80;
      server_name staging.admin-api.loandoctor.co.kr;
      return 301 https://$server_name$request_uri;
  }

  server {
      listen 443 ssl http2;
      server_name staging.admin-api.loandoctor.co.kr;

      # SSL
      ssl_certificate /etc/letsencrypt/live/staging.loandoctor.co.kr/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/staging.loandoctor.co.kr/privkey.pem;
      include /etc/letsencrypt/options-ssl-nginx.conf;
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

      # 보안 헤더
      include snippets/security-headers.conf;

      # 로그
      access_log /var/log/nginx/staging.admin-api.loandoctor.co.kr.access.log;
      error_log /var/log/nginx/staging.admin-api.loandoctor.co.kr.error.log;

      # 파일 업로드 크기
      client_max_body_size 50M;

      location / {
          proxy_pass http://127.0.0.1:4001;
          proxy_http_version 1.1;

          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 60s;

          proxy_cache_bypass $http_upgrade;
      }

      location /health {
          proxy_pass http://127.0.0.1:4001/health;
          access_log off;
      }
  }



 Nginx 설정 가이드 (EC2 Ubuntu)

  1. Nginx 설치

  # 설치
  sudo apt update
  sudo apt install -y nginx

  # 상태 확인
  sudo systemctl status nginx

  # 자동 시작 설정
  sudo systemctl enable nginx

  2. 기본 구조

  /etc/nginx/
  ├── nginx.conf              # 메인 설정
  ├── sites-available/        # 사이트 설정 파일들
  │   ├── default
  │   ├── api.yourdomain.com
  │   └── admin-api.yourdomain.com
  ├── sites-enabled/          # 활성화된 설정 (심볼릭 링크)
  └── snippets/               # 재사용 설정 조각

  3. Backend API 설정 (api.yourdomain.com)

  sudo nano /etc/nginx/sites-available/api.yourdomain.com

  server {
      listen 80;
      server_name api.yourdomain.com;

      # Certbot이 SSL 설정 후 자동으로 HTTPS 리다이렉트 추가함
      # 일단 HTTP로 설정, SSL은 나중에 추가

      location / {
          proxy_pass http://127.0.0.1:3001;
          proxy_http_version 1.1;

          # WebSocket 지원
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';

          # 원본 요청 정보 전달
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          # 타임아웃 설정
          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 60s;
      }

      # 헬스체크 엔드포인트
      location /health {
          proxy_pass http://127.0.0.1:3001/health;
          access_log off;
      }
  }

  4. Admin API 설정 (admin-api.yourdomain.com)

  sudo nano /etc/nginx/sites-available/admin-api.yourdomain.com

  server {
      listen 80;
      server_name admin-api.yourdomain.com;

      location / {
          proxy_pass http://127.0.0.1:3002;
          proxy_http_version 1.1;

          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 60s;
      }

      location /health {
          proxy_pass http://127.0.0.1:3002/health;
          access_log off;
      }
  }

  5. 설정 활성화

  # 심볼릭 링크 생성
  sudo ln -s /etc/nginx/sites-available/api.yourdomain.com /etc/nginx/sites-enabled/
  sudo ln -s /etc/nginx/sites-available/admin-api.yourdomain.com /etc/nginx/sites-enabled/

  # 기본 설정 비활성화 (선택)
  sudo rm /etc/nginx/sites-enabled/default

  # 설정 문법 검사
  sudo nginx -t

  # Nginx 재시작
  sudo systemctl reload nginx

  6. SSL 설정 (Certbot)

  # Certbot 설치
  sudo apt install -y certbot python3-certbot-nginx

  # SSL 인증서 발급 (각 도메인별)
  sudo certbot --nginx -d api.yourdomain.com
  sudo certbot --nginx -d admin-api.yourdomain.com

  # 이메일 입력, 약관 동의 후 자동 설정됨
  # 옵션: HTTPS 리다이렉트 선택 (권장)

  SSL 적용 후 자동 생성되는 설정

  server {
      listen 80;
      server_name api.yourdomain.com;
      return 301 https://$server_name$request_uri;  # HTTP → HTTPS 리다이렉트
  }

  server {
      listen 443 ssl;
      server_name api.yourdomain.com;

      ssl_certificate /etc/letsencrypt/live/api.yourdomain.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/api.yourdomain.com/privkey.pem;
      include /etc/letsencrypt/options-ssl-nginx.conf;
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

      location / {
          proxy_pass http://127.0.0.1:3001;
          # ... 나머지 동일
      }
  }

  7. SSL 자동 갱신 확인

  # 갱신 테스트
  sudo certbot renew --dry-run

  # Cron 자동 등록 확인 (certbot이 자동으로 추가함)
  sudo systemctl list-timers | grep certbot

  8. 보안 헤더 추가 (권장)

  sudo nano /etc/nginx/snippets/security-headers.conf

  # 보안 헤더
  add_header X-Frame-Options "SAMEORIGIN" always;
  add_header X-Content-Type-Options "nosniff" always;
  add_header X-XSS-Protection "1; mode=block" always;
  add_header Referrer-Policy "strict-origin-when-cross-origin" always;

  사이트 설정에 추가:
  server {
      # ...
      include snippets/security-headers.conf;
      # ...
  }

  9. 파일 업로드 크기 제한

  sudo nano /etc/nginx/nginx.conf

  http {
      # ... 기존 설정 ...

      # 파일 업로드 크기 (기본 1MB → 50MB)
      client_max_body_size 50M;
  }

  10. 최종 설정 예시 (SSL 포함)

  # /etc/nginx/sites-available/api.yourdomain.com

  server {
      listen 80;
      server_name api.yourdomain.com;
      return 301 https://$server_name$request_uri;
  }

  server {
      listen 443 ssl http2;
      server_name api.yourdomain.com;

      # SSL
      ssl_certificate /etc/letsencrypt/live/api.yourdomain.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/api.yourdomain.com/privkey.pem;
      include /etc/letsencrypt/options-ssl-nginx.conf;
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

      # 보안 헤더
      include snippets/security-headers.conf;

      # 로그
      access_log /var/log/nginx/api.yourdomain.com.access.log;
      error_log /var/log/nginx/api.yourdomain.com.error.log;

      # 파일 업로드 크기
      client_max_body_size 50M;

      location / {
          proxy_pass http://127.0.0.1:3001;
          proxy_http_version 1.1;

          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          proxy_connect_timeout 60s;
          proxy_send_timeout 60s;
          proxy_read_timeout 60s;

          proxy_cache_bypass $http_upgrade;
      }

      location /health {
          proxy_pass http://127.0.0.1:3001/health;
          access_log off;
      }
  }

  11. 유용한 명령어

  # 설정 테스트
  sudo nginx -t

  # 재시작 (설정 변경 후)
  sudo systemctl reload nginx

  # 상태 확인
  sudo systemctl status nginx

  # 로그 확인
  sudo tail -f /var/log/nginx/access.log
  sudo tail -f /var/log/nginx/error.log

  # 특정 사이트 로그
  sudo tail -f /var/log/nginx/api.yourdomain.com.error.log

  12. 체크리스트

  | 항목             | 명령어                             |
  |------------------|------------------------------------|
  | Nginx 실행 중    | sudo systemctl status nginx        |
  | 설정 문법 OK     | sudo nginx -t                      |
  | 포트 리스닝      | sudo netstat -tlnp | grep nginx    |
  | SSL 인증서 유효  | sudo certbot certificates          |
  | 외부 접속 테스트 | curl -I https://api.yourdomain.com |