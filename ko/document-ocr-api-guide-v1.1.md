## AI Service > OCR > Document OCR > API v1.1 가이드

## Document OCR API 공통 정보

### API 엔드포인트

| 리전            | 엔드포인트                          |
| --------------- | ----------------------------------- |
| 한국(판교) 리전 | https://ocr.api.nhncloudservice.com |

### 인증 및 권한

Document OCR은 API 호출 시 인증/인가를 위해 User Access Key 토큰을 사용합니다. User Access Key 토큰은 User Access Key를 기반으로 발급되는 Bearer 타입의 일시적 액세스 토큰입니다. User Access Key 토큰 발급 및 사용에 대한 자세한 내용은 [User Access Key 토큰](/nhncloud/ko/public-api/user-access-key-token)을 참고하세요.

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
| 4010006   | Invalid token.                                                                             | 유효하지 않은 토큰              |
| 4010007   | Permission denied.                                                                         | 권한 없음                       |
| 4131000   | Request size is larger than permissible limit. the permissible limit is 5mb.               | 요청 크기가 허용 한도(5MB) 초과 |

### 사업자등록증 분석 API

#### 요청

[URI]

| 메서드 | URI                             |
| ------ | ------------------------------- |
| POST   | /v1.1/appkeys/{appKey}/business |

[요청 헤더]

| 이름                | 값             | 설명                  |
| ------------------- | -------------- | --------------------- |
| X-NHN-Authorization | Bearer {Access Token} | User Access Key 토큰 |

[Path Variable]

| 이름   | 값       | 설명                           |
| ------ | -------- | ------------------------------ |
| appKey | {appKey} | 프로젝트 통합 Appkey 또는 서비스 Appkey |

[요청 본문]

- 이미지 파일의 바이너리 데이터를 넣습니다.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/business' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: Bearer ${Access Token}'
```

[필드]

| 이름  | 타입                | 설명        |
| ----- | ------------------- | ----------- |
| image | multipart/form–data | 이미지 파일 |

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
        "unitType": "pixel",
        "keyValues": [
            {
                "key":"구분",
                "value":" 간이과세자",
                "conf":0.93
            },
            {
                "key":"등록번호",
                "value":"123-45-67890",
                "conf":1
            },
            ...
        ],
        "boxes": [
            {
                "x1": 340,
                "y1": 3231,
                "x2": 523,
                "y2": 3231,
                "x3": 523,
                "y3": 3297,
                "x4": 340,
                "y4": 3297
            },
            ...
        ],
        "resolution": "normal"
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

| 이름               | 타입   | 설명                                                                |
| ------------------ | ------ | ------------------------------------------------------------------- |
| fileType           | String | 파일 확장자(.pdf, .jpg, .png)                                       |
| keyValues          | List   | 인식 결과 목록                                                      |
| keyValues[0].key   | String | 인식 항목명                                                         |
| keyValues[0].value | String | 인식 내용                                                           |
| keyValues[0].conf  | Double | 인식 결과 신뢰도                                                    |
| resolution         | String | 권장 해상도(HD 1280\*720px) 이상이면 normal, 권장 해상도 미만은 low |
| unitType           | String | boxes 좌표 단위(기본 pixel, PDF의 경우 point)                       |
| boxes              | List   | 인식 영역(Bounding box) 좌표 목록                                   |
| boxes[0]           | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }                   |

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### 사업자등록증 휴/폐업 조회 API

#### 요청

[URI]

| 메서드 | URI                                    |
| ------ | -------------------------------------- |
| POST   | /v1.1/appkeys/{appKey}/business/status |

[요청 헤더]

| 이름                | 값             | 설명                  |
| ------------------- | -------------- | --------------------- |
| X-NHN-Authorization | Bearer {Access Token} | User Access Key 토큰 |

[Path Variable]

| 이름   | 값       | 설명                           |
| ------ | -------- | ------------------------------ |
| appKey | {appKey} | 프로젝트 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름           | 타입   | 설명                                |
| -------------- | ------ | ----------------------------------- |
| businessNumber | String | 사업자등록증 등록 번호(10자리 숫자) |

[요청 본문]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/business/status' \
-H 'X-NHN-Authorization: Bearer ${Access Token}' \
--data-raw '{
  "businessNumber": "1234567890"
}'
```

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
    "statusCode": "00",
    "statusMessage": ""
  }
}
```

[헤더]

| 이름          | 타입    | 설명                                            |
| ------------- | ------- | ----------------------------------------------- |
| isSuccessful  | Boolean | 휴/폐업 조회 API 성공 여부                      |
| resultCode    | Integer | 결과 코드                                       |
| resultMessage | String  | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름          | 타입   | 설명                                     |
| ------------- | ------ | ---------------------------------------- |
| statusCode    | String | 사업자등록증 상태 코드(국세청 결과 코드) |
| statusMessage | String | 사업자등록증 상태 메시지                 |

- **"statusCode"에 따른 사업자등록증 상태 목록**

| 코드값 | 설명                                                                        |
| ------ | --------------------------------------------------------------------------- |
| 00     | 사업을 하고 있지 않는 사업자                                                |
| 01     | 부가가치세 일반과세자                                                       |
| 02     | 부가가치세 간이과세자                                                       |
| 03     | 부가가치세 면세사업자                                                       |
| 04     | 수익 사업을 영위하지 않는 비영리법인이거나 고유번호가 부여된 단체. 국가기관 |
| 05     | 휴업자                                                                      |
| 06     | 폐업자                                                                      |
| 09     | 기타                                                                        |
