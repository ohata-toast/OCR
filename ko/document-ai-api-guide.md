## AI Service > OCR > Document AI > API 가이드

### Document AI 분석 API

#### 요청

* {appKey}와 {secretKey}는 오른쪽 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드  | URI                                                               |
|------|-------------------------------------------------------------------|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/document-ai |

[요청 헤더]

| 이름            | 값                   | 설명              |
|---------------|---------------------|-----------------|
| Authorization | {secretKey}         | 콘솔에서 발급받은 비밀 키 |
| Content-Type  | multipart/form-data | 콘텐츠 타입          |

[요청 본문]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/document-ai' \
-H 'Authorization: ${secretKey}' \
-F 'image=@sample.png' \
-F 'prompt="간단하게 요약 해줘"' \
-F 'documentTypeCode="GENERAL"'
```

[필드]

| 이름    | 타입   | 필수 여부 | 기본값 | 유효 범위 | 설명    |
|-------|--------|---------|--------|--------|--------|
| image | file | O |     |   | 이미지 파일|
| documentTypeCode | text | X |  GENERAL | GENERAL, BUSINESS_REGISTRATION, BUSINESS_CARD  | 문서 유형<br> 일반: GENERAL <br> 사업자 등록증: BUSINESS_REGISTRATION <br> 명함: BUSINESS_CARD |
| prompt | text | O |    |   | 질문 내용<br>최대 1000자  |

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

| 이름            | 타입      | 설명                               |
|---------------|---------|----------------------------------|
| isSuccessful  | Boolean | 분석 API 성공 여부                     |
| resultCode    | Integer | 결과 코드                            |
| resultMessage | String  | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름                                      | 타입     | 설명                                          |
|-----------------------------------------|--------|---------------------------------------------|
| llmResponse                                | String | LLM 분석 답변                           |