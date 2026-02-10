## AI Service > OCR > General OCR > API 가이드

### General OCR API

#### 요청

* {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드  | URI                                                               |
|------|-------------------------------------------------------------------|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general |

#### 이미지 파일을 이용한 요청

[요청 헤더]

| 이름            | 값                   | 설명              |
|---------------|---------------------|-----------------|
| Authorization | {secretKey}         | 콘솔에서 발급 받은 보안 키 |
| Content-Type  | multipart/form-data | 콘텐츠 타입          |

[요청 본문]

* 이미지 파일의 바이너리 데이터를 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[필드]

| 이름    | 타입                  | 설명     |
|-------|---------------------|--------|
| image | multipart/form–data | 이미지 파일 |

#### 이미지 URL을 이용한 요청
[요청 헤더]

| 이름            | 값                | 설명              |
|---------------|------------------|-----------------|
| Authorization | {secretKey}      | 콘솔에서 발급 받은 보안 키 |
| Content-Type  | application/json | 콘텐츠 타입          |

[요청 본문]

* 이미지 URL을 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[필드]

| 이름       | 타입     | 설명      |
|----------|--------|---------|
| imageUrl | String | 이미지 URL |

* 이미지 URL에 포트를 직접 지정하는 경우 80, 443, 10000~12000 포트만 사용 가능합니다.

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
                        "value":"stella",
                        "conf":0.99
                    },
                    {
                        "value":"artois",
                        "conf":0.98
                    }
                ],
                "inferTexts": [
                    {
                        "value":"belgium",
                        "conf":0.99
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

| 이름            | 타입      | 설명                               |
|---------------|---------|----------------------------------|
| isSuccessful  | Boolean | 분석 API 성공 여부                     |
| resultCode    | Integer | 결과 코드                            |
| resultMessage | String  | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름                                      | 타입     | 설명                                          |
|-----------------------------------------|--------|---------------------------------------------|
| fileType                                | String | 파일 확장자(jpg, png)                            |
| listOfInferTexts                        | List   | 인식 결과 목록                                    |
| listOfInferTexts[0].inferTexts[0].value | String | 인식 내용                                       |
| listOfInferTexts[0].inferTexts[0].conf  | Double | 인식 결과 신뢰도                                   |
| listOfBoundingBoxes                     | List   | 인식 영역(Bounding box) 좌표 목록                   |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### General OCR 분할 인식 API

#### 요청

* {appKey}와 {secretKey}는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

[URI]

| 메서드  | URI                                                                        |
|------|----------------------------------------------------------------------------|
| POST | https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping |

#### 이미지 파일을 이용한 요청

[요청 헤더]

| 이름            | 값                   | 설명              |
|---------------|---------------------|-----------------|
| Authorization | {secretKey}         | 콘솔에서 발급 받은 보안 키 |
| Content-Type  | multipart/form-data | 콘텐츠 타입          |

[요청 본문]

* 이미지 파일의 바이너리 데이터를 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[필드]

| 이름    | 타입                  | 설명     |
|-------|---------------------|--------|
| image | multipart/form–data | 이미지 파일 |

#### 이미지 URL을 이용한 요청

[요청 헤더]

| 이름            | 값                | 설명              |
|---------------|------------------|-----------------|
| Authorization | {secretKey}      | 콘솔에서 발급 받은 보안 키 |
| Content-Type  | application/json | 콘텐츠 타입          |

[요청 본문]

* 이미지 URL을 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[필드]

| 이름       | 타입     | 설명      |
|----------|--------|---------|
| imageUrl | String | 이미지 URL |

* 이미지 URL에 포트를 직접 지정하는 경우 80, 443, 10000~12000 포트만 사용 가능합니다.

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
                        "value":"stella",
                        "conf":0.99
                    },
                    {
                        "value":"artois",
                        "conf":0.98
                    }
                ],
                "inferTexts": [
                    {
                        "value":"belgium",
                        "conf":0.99
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

| 이름            | 타입      | 설명                               |
|---------------|---------|----------------------------------|
| isSuccessful  | Boolean | 분석 API 성공 여부                     |
| resultCode    | Integer | 결과 코드                            |
| resultMessage | String  | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름                                      | 타입      | 설명                                          |
|-----------------------------------------|---------|---------------------------------------------|
| fileType                                | String  | 파일 확장자(jpg, png)                            |
| listOfInferTexts                        | List    | 인식 결과 목록                                    |
| listOfInferTexts[0].inferTexts[0].value | String  | 인식 내용                                       |
| listOfInferTexts[0].inferTexts[0].conf  | Double  | 인식 결과 신뢰도                                   |
| listOfBoundingBoxes                     | List    | 인식 영역(Bounding box) 좌표 목록                   |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object  | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 } |
| slicesImages                            | Integer | 입력 이미지의 가로-세로 비율에 따라 내부적으로 분할된 이미지 개수       |

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)