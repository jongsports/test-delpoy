# 1. 기본 패키지 + SSH

sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git wget openssh-server ufw
sudo systemctl enable ssh
sudo ufw allow OpenSSH && sudo ufw enable

# 2. 노트북 덮어도 안 꺼지게

sudo nano /etc/systemd/logind.conf

# HandleLidSwitch=ignore

# HandleLidSwitchExternalPower=ignore

# HandleLidSwitchDocked=ignore

sudo systemctl restart systemd-logind

# 3. 고정 IP

sudo nmcli con mod "와이파이이름" ipv4.method manual \
 ipv4.addresses 192.168.45.53/24 \
 ipv4.gateway 192.168.45.1 \
 ipv4.dns "8.8.8.8,8.8.4.4"
sudo nmcli con down "와이파이이름" && sudo nmcli con up "와이파이이름"

# 4. Docker 설치

curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
sudo systemctl enable docker

# 5. GitHub Actions Runner 설치 (Organization용)

mkdir ~/actions-runner && cd ~/actions-runner

# GitHub org → Settings → Actions → Runners에서 제공하는 명령어 실행

# ./config.sh --url https://github.com/org이름 --token 토큰

sudo ./svc.sh install
sudo ./svc.sh start

```

### 프로젝트 파일 구조
```

my-project/
├── .github/workflows/deploy.yml ← 자동 배포 규칙
├── Dockerfile ← 컨테이너 빌드 방법
├── docker-compose.yml ← 컨테이너 실행 설정
├── requirements.txt ← Python 패키지
└── main.py ← 실제 코드

# 사용중지

1. 컨테이너만 내리기 (서버에서)
   docker stop test-deploy
   docker rm test-deploy
2. 워크플로우 비활성화 (GitHub에서)
   GitHub → test-deploy 레포 → Actions 탭 → 왼쪽에서 Deploy to Home Server 클릭 → 오른쪽 ... → Disable workflow
