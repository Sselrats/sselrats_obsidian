
 Staging Deployment Checklist



  1. AWS EC2 Setup

  - Create EC2 instance (Ubuntu 22.04 LTS recommended)
  - Choose instance type (t3.small or t3.medium for staging)
  - Configure Security Group:
    - SSH (22) - your IP only
    - HTTP (80) - 0.0.0.0/0
    - HTTPS (443) - 0.0.0.0/0
    - PostgreSQL (5432) - localhost only
  - Create/assign Elastic IP
  - Create SSH key pair & save .pem file

  2. Domain & DNS

  - Add DNS records in your domain provider:
    - A record: api.yourdomain.com → EC2 Elastic IP
    - A record: admin-api.yourdomain.com → EC2 Elastic IP
    - (Optional) A record: staging.yourdomain.com → EC2 Elastic IP

  1. EC2 Server Configuration

sudo apt update
sudo apt upgrade

```null
timedatectl
sudo timedatectl set-timezone Asia/Seoul
```

```null
sudo dd if=/dev/zero of=/swapfile bs=128M count=32
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapon -s


sudo vim /etc/fstab

/swapfile swap swap defaults 0 0
```
```
```

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

```
wget -qO- https://get.pnpm.io/install.sh | sh -
```

pnpm install pm2 -g

sudo apt install postgresql
sudo service postgresql status

sudo apt update && sudo apt install nginx
sudo systemctl status nginx
sudo systemctl start nginx

```shell
sudo apt update
sudo apt-get install letsencrypt
```

sudo apt update 
sudo apt install certbot
sudo apt install python3-certbot-nginx 

staging.admin-api.loandoctor.co.kr
sudo certbot certonly --nginx --cert-name staging.loandoctor.co.kr -d staging.loandoctor.co.kr -d staging.api.loandoctor.co.kr -d staging.admin.loandoctor.co.kr -d staging.admin-api.loandoctor.co.kr

sudo certbot certonly --nginx --cert-name loandoctor.co.kr -d loandoctor.co.kr -d api.loandoctor.co.kr -d admin.loandoctor.co.kr -d admin-api.loandoctor.co.kr

sudo certbot certonly --nginx --cert-name loandoctor.co.kr -d loandoctor.co.kr -d api.loandoctor.co.kr -d admin.loandoctor.co.kr -d admin-api.loandoctor.co.kr -d www.loandoctor.co.kr

sudo certbot certonly --nginx --cert-name finbox24.com -d  finbox24.com -d  www.finbox24.com 
sudo ln -s /etc/nginx/sites-available/finbox24.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

  6. SSL 설정 (Certbot)

  # Certbot 설치
  sudo apt install -y certbot python3-certbot-nginx

  - SSH into EC2
  - Install dependencies:
    - Node.js 20.x (via nvm)
    - pnpm
    - PostgreSQL 15+
    - Nginx
    - Certbot (SSL)
    - PM2 (process manager)
    - Git

  4. PostgreSQL Setup

  - Create database & user
  - Configure pg_hba.conf for local access
  - Run Prisma migrations

  5. AWS S3 Setup

  - Create S3 bucket for file uploads
  - Configure bucket policy (private)
  - Create IAM user with S3 access
  - Save Access Key & Secret Key

  6. Application Deployment

  - Clone repository to EC2
  - Create .env files for each app
  - Install dependencies (pnpm install)
  - Build apps (pnpm build)
  - Run migrations (pnpm db:migrate)
  - Start with PM2

  7. Nginx & SSL

  - Configure Nginx reverse proxy
  - Obtain SSL certificates (Certbot)
  - Configure HTTPS redirect

  8. Environment Variables Needed

  # Database
  DATABASE_URL=postgresql://user:password@localhost:5432/loandoc_staging

  # Backend
  PORT=3001
  SESSION_SECRET=<random-string>
  KAKAO_CLIENT_ID=<kakao-app-key>
  KAKAO_CLIENT_SECRET=<kakao-secret>
  KAKAO_REDIRECT_URI=https://api.yourdomain.com/auth/kakao/callback

  # Admin Backend
  ADMIN_PORT=3002
  JWT_SECRET=<random-string>

  # AWS S3
  AWS_REGION=ap-northeast-2
  AWS_ACCESS_KEY_ID=<access-key>
  AWS_SECRET_ACCESS_KEY=<secret-key>
  S3_BUCKET_NAME=loandoc-staging-uploads

  # CORS
  CORS_ORIGIN=https://staging.yourdomain.com