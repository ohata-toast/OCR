# API 가이드 템플릿

## v1.0 기본 템플릿 (Appkey + SecretKey)

실제 서비스에 맞게 `{서비스명}`, `{서비스경로}`, `{API목록}`을 치환합니다.

```markdown
## AI Service > OCR > {서비스명} > API v1.0 가이드

### {서비스명} API 공통 정보

#### API 엔드포인트

| 리전            | 엔드포인트                          |
| --------------- | ----------------------------------- |
| 한국(판교) 리전 | https://ocr.api.nhncloudservice.com |

#### 인증 및 권한

<!-- common-info.md의 v1.0 인증 텍스트 참조 -->

#### 응답 공통 정보

<!-- common-info.md의 응답 공통 정보 참조 -->

#### 오류 코드

<!-- common-info.md의 오류 코드 테이블 참조 -->

---

### {API이름} API

#### 요청

[URI]

| 메서드 | URI                                                |
| ------ | -------------------------------------------------- |
| POST   | /v1.0/appkeys/{appKey}/{서비스경로} |

[요청 헤더]

| 이름          | 값          | 설명                       |
| ------------- | ----------- | -------------------------- |
| Authorization | {secretKey} | 콘솔에서 발급 받은 보안 키 |
| Content-Type  | multipart/form-data |                  |

[Path Variable]

| 이름   | 값     | 설명                         |
| ------ | ------ | ---------------------------- |
| appKey | String | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름  | 타입     | 설명          |
| ----- | -------- | ------------- |
| image | multipart/form-data | 이미지 파일 |

#### 응답

[응답 본문]

| 이름   | 타입    | 설명         |
| ------ | ------- | ------------ |
| header | Object  | 헤더 정보    |
| result | Object  | 분석 결과    |

[응답 본문 헤더]

| 이름         | 타입    | 설명             |
| ------------ | ------- | ---------------- |
| isSuccessful | Boolean | 성공 여부        |
| resultCode   | Integer | 결과 코드        |
| resultMessage | String | 결과 메시지      |
```

## v1.1 변환 체크리스트

기존 v1.0 파일에서 v1.1을 만들 때 변경할 부분:

1. **제목**: `API v1.0 가이드` → `API v1.1 가이드`
2. **인증 텍스트**: Appkey/SecretKey 설명 → User Access Key Token 설명
3. **URI 버전**: `/v1.0/appkeys/{appKey}/` → `/v1.1/appkeys/{appKey}/`
4. **요청 헤더**: `Authorization: {secretKey}` → `X-NHN-Authorization: Bearer {User Access Key Token}`
5. **curl 예시**: `-H "Authorization: ${SECRET_KEY}"` → `-H "X-NHN-Authorization: ${ACCESS_TOKEN}"`
6. **오류 코드**: `INVALID_TOKEN`, `PERMISSION_DENIED` 추가

> 엔드포인트의 경로 구조(`/appkeys/{appKey}/{서비스경로}`)는 변경하지 않습니다.

## 서비스별 경로 매핑

| 서비스 | API | 경로 |
|--------|-----|------|
| Document AI | Document AI 분석 | `document-ai` |
| General OCR | General OCR 분석 | `general` |
| Document OCR | 사업자등록증 분석 | `business` |
| Document OCR | 사업자등록증 진위확인 | `business/verify` |
| Document OCR | 사업자등록증 폐업조회 | `business/status` |
| Document OCR | 신용카드 분석 | `credit-card` |
| Document OCR | 신분증 분석 | `id-card` |
| Document OCR | 신분증 진위확인 | `id-card/verify` |
| Document OCR | 차량번호판 분석 | `vehicle-plate` |

## curl 예시 패턴

### v1.0 / v2.0

```bash
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/{경로}' \
-F 'image=@sample.png' \
-H 'Authorization: ${SECRET_KEY}'
```

### v1.1 / v2.1

```bash
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/{경로}' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${ACCESS_TOKEN}'
```

## 릴리스 노트 항목 패턴

각 버전 출시 시 릴리스 노트에 추가하는 형식:

**KO:**
```markdown
### {날짜}

* API {버전} : User Access Token 기반 인증 추가
```

**EN:**
```markdown
### {날짜}

* API {버전}: Added User Access Token-based authentication
```

**JA:**
```markdown
### {날짜}

* API {버전}: User Access Tokenベースの認証を追加
```
