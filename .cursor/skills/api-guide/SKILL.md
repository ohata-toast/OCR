---
name: api-guide
description: OCR 서비스 API 가이드 문서를 생성하거나 수정합니다. API 가이드 추가, 버전별 문서 분리, 인증 방식 변경, 공통 정보 섹션 작성, 다국어 문서 생성 시 사용합니다.
---

# OCR API Guide 작성 가이드

## 디렉토리 구조

```
ko/   # 한국어 (원본)
en/   # 영어
ja/   # 일본어
zh/   # 중국어 (영문 동일)
```

ko를 먼저 작성하고, en → ja → zh 순으로 진행합니다. zh는 en 파일을 그대로 복사합니다.

## 파일 명명 규칙

```
{서비스}-api-guide-v{버전}.md
```

| 서비스 | 파일명 패턴 | 버전 |
|--------|-------------|------|
| Document AI | `document-ai-api-guide-v1.0.md` | v1.0, v1.1 |
| General OCR | `general-ocr-api-guide-v1.0.md` | v1.0, v1.1 |
| Document OCR | `document-ocr-api-guide-v1.0.md` | v1.0, v1.1, v2.0, v2.1 |

## 버전 구분 기준

| 버전 | 인증 방식 | 요청 헤더 |
|------|-----------|-----------|
| v1.0 / v2.0 | Appkey + SecretKey | `Authorization: {secretKey}` |
| v1.1 / v2.1 | User Access Key Token | `X-NHN-Authorization: Bearer {User Access Key Token}` |

버전이 올라가도 **URI 엔드포인트 경로 구조는 동일**하게 유지합니다. 버전 숫자만 변경됩니다.

```
v1.0: /v1.0/appkeys/{appKey}/general
v1.1: /v1.1/appkeys/{appKey}/general
```

## API 가이드 문서 구조

모든 API 가이드는 아래 구조를 따릅니다.

```markdown
## AI Service > OCR > {서비스명} > API v{버전} 가이드

## {서비스명} API 공통 정보

### API 엔드포인트
### 인증 및 권한
### 응답 공통 정보
### 오류 코드

### {API명} API
#### 요청
#### 응답
```

### 공통 정보 섹션 상세

공통 정보 섹션의 작성 방법은 [common-info.md](common-info.md)를 참조하세요.

## 관련 파일 업데이트 체크리스트

API 가이드를 추가/수정할 때 함께 업데이트해야 하는 파일:

- [ ] API 가이드 파일 (v1.0/v1.1 또는 v2.0/v2.1)
- [ ] 에러코드 파일 (`{서비스}-error-code.md`) - 토큰 에러코드 추가
- [ ] 릴리스 노트 (`{서비스}-release-notes.md`) - 출시 안내 추가
- [ ] 4개 언어 디렉토리 (ko, en, ja, zh) 모두 반영

## 커밋 컨벤션

관심사별로 커밋을 분리합니다.

```
📝 {lang}: {서비스명} API v{버전} 가이드 추가
📝 {lang}: 에러코드에 토큰 인증 코드 추가 및 릴리스 노트 업데이트
🔗 {lang}: 문서 링크 절대경로 변경
```

## 워크플로우

**새로운 인증 방식이 추가된 경우:**

1. ko 디렉토리에서 기존 v1.0을 복사하여 v1.1 생성
2. v1.1에서 인증 섹션, URI 버전, 요청 헤더, curl 예시 변경
3. v1.0의 인증 문구도 최신 표준 문구로 업데이트
4. en 디렉토리에 동일 구조로 영문 가이드 생성
5. ja 디렉토리에 동일 구조로 일본어 가이드 생성
6. zh 디렉토리에 en 파일 복사
7. 각 언어의 에러코드, 릴리스 노트 업데이트
8. 관심사별로 커밋 분리
