
# 새 브랜치와 함께 worktree 생성 (권장)
git worktree add -b feature/frontend ../myproject-frontend

# 백엔드용 새 브랜치와 worktree 생성
git worktree add -b feature/backend ../myproject-backend

# 기존 브랜치를 사용하는 경우 (브랜치가 이미 존재할 때)
git worktree add ../myproject-hotfix hotfix/critical-bug

# 현재 worktree 목록 확인
git worktree list