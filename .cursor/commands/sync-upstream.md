# Sync Upstream

fork된 저장소를 원본 저장소(TOAST-DOCS/OCR)의 최신 내용과 동기화합니다.

## Steps

1. **원본 저장소 remote 확인**
   - `git remote -v`로 등록된 remote 목록을 확인
   - origin이 아닌 remote 중 `TOAST-DOCS/OCR`을 가리키는 것을 찾아 사용 (upstream, base 등 이름은 다를 수 있음)
   - 없으면 사용자에게 원본 저장소 remote가 등록되어 있지 않다고 알리고, 추가할지 물어보기
   - 추가 시 사용자의 기존 remote 프로토콜(SSH `git@github.com:` 또는 HTTPS `https://github.com/`)을 확인하여 동일한 프로토콜로 추가

2. **원본 저장소 최신 내용 가져오기**
   - `git fetch {remote이름}` 실행

3. **현재 브랜치 기준으로 rebase**
   - 현재 브랜치 이름을 확인하고 `git rebase {remote이름}/{현재브랜치}` 실행
   - 충돌 발생 시 사용자에게 알리고 해결 방법 안내

4. **결과 확인**
   - `git log --oneline -5`로 rebase 결과 보고

## 원본 저장소 정보

- **원본 저장소**: `TOAST-DOCS/OCR`
- SSH: `git@github.com:TOAST-DOCS/OCR.git`
- HTTPS: `https://github.com/TOAST-DOCS/OCR.git`
