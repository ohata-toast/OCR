## AI Service > OCR > Document AI > API v1.0 가이드

## Document AI API 공통 정보

### API 엔드포인트

| 리전            | 엔드포인트                          |
| --------------- | ----------------------------------- |
| 한국(판교) 리전 | https://ocr.api.nhncloudservice.com |

### 인증 및 권한

Document AI API를 사용하려면 Appkey와 SecretKey가 필요합니다.
Appkey는 NHN Cloud의 각 서비스별로 발급되는 고유 인증 키로 API 요청 시 서비스 식별과 유효성 검증에 사용됩니다. SecretKey는 API에 대한 접근을 제어하는 비밀 키입니다.
Appkey 및 SecretKey 확인 및 사용에 대한 자세한 내용은 [Appkey](/nhncloud/ko/public-api/appkey)를 참고하세요.

Appkey 대신 프로젝트 통합 Appkey를 사용할 수도 있습니다. 프로젝트 통합 Appkey는 NHN Cloud에서 하나의 프로젝트 내 여러 서비스에 대해 공통으로 사용할 수 있는 인증 키입니다.
프로젝트 통합 Appkey 생성 및 사용에 대한 자세한 내용은 [프로젝트 통합 Appkey](/nhncloud/ko/public-api/project-appkey)를 참고하세요.

### 응답 공통 정보

모든 API 요청 응답으로 HTTP 200 OK를 전달합니다. API 요청 성공 여부는 Response Body의 header 항목을 참고하여 판단할 수 있습니다.

<details>
  <summary><strong>성공 응답</strong></summary>

```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "result": {
        "llmResponse": ...
    },
    ...
}
```

</details>

<details>
  <summary><strong>실패 응답</strong></summary>

```
{
    "header": {
        "isSuccessful": false,
        "resultCode": -1,
        "resultMessage": "Unknown error."
    }
}
```

</details>

| 이름          | 타입    | 설명                                          |
| ------------- | ------- | --------------------------------------------- |
| resultCode    | int     | 응답 코드<br>성공 시 0, 실패 시 오류 코드 반환 |
| resultMessage | String  | 응답 메시지                                   |
| isSuccessful  | boolean | 성공 여부                                     |

### 오류 코드

#### 공통

| 오류 코드 | 오류 메시지                                                                                | 설명                            |
| --------- | ------------------------------------------------------------------------------------------ | ------------------------------- |
| -1        | Unknown error.                                                                             | 알 수 없는 오류                 |
| 4000001   | Invalid parameter.                                                                         | 유효하지 않은 파라미터          |
| 4000002   | Invalid file.                                                                              | 유효하지 않은 파일              |
| 4000003   | Invalid file type.                                                                         | 유효하지 않은 파일 타입         |
| 4000004   | Uploaded file is empty.                                                                    | 업로드된 파일이 비어 있음       |
| 4000005   | Required headers is missing.                                                               | 필수 헤더 누락                  |
| 4000006   | Api call limit exceeded, If you need to adjust the limit, please contact customer service. | API 호출 한도 초과              |
| 4131000   | Request size is larger than permissible limit. the permissible limit is 5mb.               | 요청 크기가 허용 한도(5MB) 초과 |

### Document AI 분석 API

[URI]

| 메서드 | URI                                |
| ------ | ---------------------------------- |
| POST   | /v1.0/appkeys/{appKey}/document-ai |

[요청 헤더]

| 이름          | 값                  | 설명                      |
| ------------- | ------------------- | ------------------------- |
| Authorization | {secretKey}         | 콘솔에서 발급받은 비밀 키 |
| Content-Type  | multipart/form-data | 콘텐츠 타입               |

[요청 본문]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/document-ai' \
-H 'Authorization: ${secretKey}' \
-F 'image=@sample.png' \
-F 'prompt="간단하게 요약 해줘"' \
-F 'documentTypeCode="GENERAL"'
```

[필드]

| 이름             | 타입 | 필수 여부 | 기본값  | 유효 범위                                     | 설명                                                                                           |
| ---------------- | ---- | --------- | ------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| image            | file | O         |         |                                               | 이미지 파일                                                                                    |
| documentTypeCode | text | X         | GENERAL | GENERAL, BUSINESS_REGISTRATION, BUSINESS_CARD | 문서 유형<br> 일반: GENERAL <br> 사업자 등록증: BUSINESS_REGISTRATION <br> 명함: BUSINESS_CARD |
| prompt           | text | O         |         |                                               | 질문 내용<br>최대 1000자                                                                       |

#### 응답

[응답 본문]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "result": {
    "llmResponse": "본 문서는 의미가 없는 텍스트인 '로렌 ipsum'을 사용하여 글자가 있는 그러나 읽기 어렵고 가독성이 떨어지는 문장을 작성한 것 같습니다."
  }
}
```

[헤더]

| 이름          | 타입    | 설명                                            |
| ------------- | ------- | ----------------------------------------------- |
| isSuccessful  | Boolean | 분석 API 성공 여부                              |
| resultCode    | Integer | 결과 코드                                       |
| resultMessage | String  | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름        | 타입   | 설명          |
| ----------- | ------ | ------------- |
| llmResponse | String | LLM 분석 답변 |
