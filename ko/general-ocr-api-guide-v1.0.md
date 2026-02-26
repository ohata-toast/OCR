## AI Service > OCR > General OCR > API v1.0 가이드

## General OCR API 공통 정보

### API 엔드포인트

| 리전            | 엔드포인트                          |
| --------------- | ----------------------------------- |
| 한국(판교) 리전 | https://ocr.api.nhncloudservice.com |

### 인증 및 권한

General OCR API를 사용하려면 Appkey와 SecretKey가 필요합니다.
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
        ...
    }
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
| resultCode    | int     | 응답 코드<br>성공 시 0, 실패시 오류 코드 반환 |
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

### General OCR API

#### 요청

[URI]

| 메서드 | URI                            |
| ------ | ------------------------------ |
| POST   | /v1.0/appkeys/{appKey}/general |

#### 이미지 파일을 이용한 요청

[요청 헤더]

| 이름          | 값                  | 설명                      |
| ------------- | ------------------- | ------------------------- |
| Authorization | {secretKey}         | 콘솔에서 발급받은 비밀 키 |
| Content-Type  | multipart/form-data | 콘텐츠 타입               |

[요청 본문]

- 이미지 파일의 바이너리 데이터를 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[필드]

| 이름  | 타입                | 설명        |
| ----- | ------------------- | ----------- |
| image | multipart/form-data | 이미지 파일 |

#### 이미지 URL을 이용한 요청

[요청 헤더]

| 이름          | 값               | 설명                      |
| ------------- | ---------------- | ------------------------- |
| Authorization | {secretKey}      | 콘솔에서 발급받은 비밀 키 |
| Content-Type  | application/json | 콘텐츠 타입               |

[요청 본문]

- 이미지 URL을 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[필드]

| 이름     | 타입   | 설명       |
| -------- | ------ | ---------- |
| imageUrl | String | 이미지 URL |

- 이미지 URL에 포트를 직접 지정하는 경우 80, 443, 10000~12000 포트만 사용 가능합니다.

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
    "fileType": "png",
    "listOfInferTexts": [
      {
        "inferTexts": [
          {
            "value": "stella",
            "conf": 0.99
          },
          {
            "value": "artois",
            "conf": 0.98
          }
        ],
        "inferTexts": [
          {
            "value": "belgium",
            "conf": 0.99
          }
        ]
      }
    ],
    "listOfBoundingBoxes": [
      {
        "boundingBoxes": [
          {
            "x1": 32,
            "y1": 23,
            "x2": 65,
            "y2": 23,
            "x3": 65,
            "y3": 35,
            "x4": 32,
            "y4": 35
          }
        ]
      }
    ]
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

| 이름                                    | 타입   | 설명                                              |
| --------------------------------------- | ------ | ------------------------------------------------- |
| fileType                                | String | 파일 확장자(jpg, png)                             |
| listOfInferTexts                        | List   | 인식 결과 목록                                    |
| listOfInferTexts[0].inferTexts[0].value | String | 인식 내용                                         |
| listOfInferTexts[0].inferTexts[0].conf  | Double | 인식 결과 신뢰도                                  |
| listOfBoundingBoxes                     | List   | 인식 영역(Bounding box) 좌표 목록                 |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### General OCR 분할 인식 API

#### 요청

[URI]

| 메서드 | URI                                     |
| ------ | --------------------------------------- |
| POST   | /v1.0/appkeys/{appKey}/general/cropping |

#### 이미지 파일을 이용한 요청

[요청 헤더]

| 이름          | 값                  | 설명                      |
| ------------- | ------------------- | ------------------------- |
| Authorization | {secretKey}         | 콘솔에서 발급받은 비밀 키 |
| Content-Type  | multipart/form-data | 콘텐츠 타입               |

[요청 본문]

- 이미지 파일의 바이너리 데이터를 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[필드]

| 이름  | 타입                | 설명        |
| ----- | ------------------- | ----------- |
| image | multipart/form-data | 이미지 파일 |

#### 이미지 URL을 이용한 요청

[요청 헤더]

| 이름          | 값               | 설명                      |
| ------------- | ---------------- | ------------------------- |
| Authorization | {secretKey}      | 콘솔에서 발급받은 비밀 키 |
| Content-Type  | application/json | 콘텐츠 타입               |

[요청 본문]

- 이미지 URL을 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[필드]

| 이름     | 타입   | 설명       |
| -------- | ------ | ---------- |
| imageUrl | String | 이미지 URL |

- 이미지 URL에 포트를 직접 지정하는 경우 80, 443, 10000~12000 포트만 사용 가능합니다.

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
    "fileType": "png",
    "listOfInferTexts": [
      {
        "inferTexts": [
          {
            "value": "stella",
            "conf": 0.99
          },
          {
            "value": "artois",
            "conf": 0.98
          }
        ],
        "inferTexts": [
          {
            "value": "belgium",
            "conf": 0.99
          }
        ]
      }
    ],
    "listOfBoundingBoxes": [
      {
        "boundingBoxes": [
          {
            "x1": 32,
            "y1": 23,
            "x2": 65,
            "y2": 23,
            "x3": 65,
            "y3": 35,
            "x4": 32,
            "y4": 35
          }
        ]
      }
    ],
    "slicesImages": 2
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

| 이름                                    | 타입    | 설명                                                              |
| --------------------------------------- | ------- | ----------------------------------------------------------------- |
| fileType                                | String  | 파일 확장자(jpg, png)                                             |
| listOfInferTexts                        | List    | 인식 결과 목록                                                    |
| listOfInferTexts[0].inferTexts[0].value | String  | 인식 내용                                                         |
| listOfInferTexts[0].inferTexts[0].conf  | Double  | 인식 결과 신뢰도                                                  |
| listOfBoundingBoxes                     | List    | 인식 영역(Bounding box) 좌표 목록                                 |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object  | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }                 |
| slicesImages                            | Integer | 입력 이미지의 가로-세로 비율에 따라 내부적으로 분할된 이미지 개수 |

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)
