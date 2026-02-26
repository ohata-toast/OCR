## AI Service > OCR > Document OCR > API v2.1 가이드

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

### v2.1 API 소개

#### v2.0과 달라진 점

- User Access Key 토큰 기반 인증 방식이 적용되었습니다.

#### 주의 사항

- 요청, 응답 시 Base64 인코딩 여부를 확인하십시오.
- 암호화, 복호화의 상세 모드(예: AES-256/CBC/PKCS7Padding)를 확인하십시오.
- 암호화에 사용되는 대칭 키는 반드시 32Byte 난수로 생성합니다. 보안을 위해 각 요청마다 새로운 대칭 키를 생성하여 사용하는 것을 권장합니다.

### 공개 키 발급

#### 요청

[URI]

| 메서드 | URI                                              |
| ------ | ------------------------------------------------ |
| GET    | /v2.1/appkeys/{appKey}/public-keys/{serviceName} |

[요청 헤더]

| 이름                | 값             | 설명                 |
| ------------------- | -------------- | -------------------- |
| X-NHN-Authorization | {Access Token} | User Access Key 토큰 |

[Path Variable]

| 이름        | 값            | 설명                                                                                                             |
| ----------- | ------------- | ---------------------------------------------------------------------------------------------------------------- |
| appKey      | {appKey}      | 통합 Appkey 또는 서비스 Appkey                                                                                   |
| serviceName | {serviceName} | credit-card(신용카드 API 호출 시 사용할 공개 키 발급 시),<br> id-card(신분증 API 호출 시 사용할 공개 키 발급 시) |

[요청 본문]

```shell
curl -X GET 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/public-keys/{serviceName}' \
-H 'X-NHN-Authorization: ${Access Token}'
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
    "key": "String",
    "version": "0"
  }
}
```

[헤더]

| 이름          | 타입    | 설명                                            |
| ------------- | ------- | ----------------------------------------------- |
| isSuccessful  | Boolean | API 성공 여부                                   |
| resultCode    | Integer | 결과 코드                                       |
| resultMessage | String  | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름           | 타입   | 설명                       |
| -------------- | ------ | -------------------------- |
| result         | Object | 암호화에 필요한 공개 키    |
| result.key     | String | 공개 키(Base64로 인코딩됨) |
| result.version | String | 공개 키의 버전             |

- 공개 키는 **Base64**로 인코딩된 상태입니다.

### 신용카드 API

#### 신용카드 분석 API

#### 요청

[URI]

| 메서드 | URI                                |
| ------ | ---------------------------------- |
| POST   | /v2.1/appkeys/{appKey}/credit-card |

[요청 헤더]

| 이름                | 값              | 설명                                 |
| ------------------- | --------------- | ------------------------------------ |
| X-NHN-Authorization | {Access Token}  | User Access Key 토큰                 |
| X-Key-Version       | {x-key-version} | 발급 받은 공개 키의 버전             |
| Symmetric-Key       | {symmetricKey}  | 발급 받은 공개 키로 암호화된 대칭 키 |

- {symmetricKey}는 반드시 **32byte 난수**로 생성해야 합니다.
- {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[Path Variable]

| 이름   | 값       | 설명                           |
| ------ | -------- | ------------------------------ |
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름  | 타입                | 설명        | 암호화 설명               |
| ----- | ------------------- | ----------- | ------------------------- |
| image | multipart/form–data | 이미지 파일 | 대칭 키로 암호화된 이미지 |

- 이미지 파일은 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).
- IV(초기화 벡터)는 대칭 키의 처음 16바이트(즉, 0~15번째 바이트)를 사용합니다.

[요청 본문]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${Access Token}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
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
        "fileType": "png",
        "resolution": "low",
        "cardNums": [
                    {
                        "value": "1111",
                        "conf": 0.87
                    },
                    {
                        "value": "2222",
                        "conf": 0.99
                    },
                    {
                        "value": "3333",
                        "conf": 0.97
                    },
                    {
                        "value": "4444",
                        "conf": 0.89
                    }
        ],
        "totalCardNum": "111222233334444",
        "cardNumBoxes": [
            {
                "x1": 62,
                "y1": 256,
                "x2": 192,
                "y2": 256,
                "x3": 192,
                "y3": 301,
                "x4": 62,
                "y4": 301
            },
            ...
        ],
        "validThru": {
            "value": "04/19",
            "conf": 0.53
        },
        "validThruBox": {
            "x1": 316,
            "y1": 315,
            "x2": 426,
            "y2": 315,
            "x3": 426,
            "y3": 347,
            "x4": 316,
            "y4": 347
        }
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

| 이름              | 타입   | 설명                                                            | 암호화 여부 |
| ----------------- | ------ | --------------------------------------------------------------- | ----------- |
| fileType          | String | 파일 확장자(.jpg, .png)                                         |             |
| resolution        | String | 권장 해상도(760\*480px) 이상이면 normal, 권장 해상도 미만은 low |             |
| cardNums          | List   | 카드 번호 인식 결과 목록                                        |             |
| cardNums[0].value | String | 인식 결과                                                       | O           |
| cardNums[0].conf  | Double | 인식 결과 신뢰도                                                |             |
| totalCardNum      | List   | 카드 번호 전체 인식 결과                                        | O           |
| cardNumBoxes      | List   | 카드 번호 인식 영역(Bounding box) 좌표 목록                     |             |
| cardNumBoxes[0]   | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }               |             |
| validThru.value   | String | 유효 기간 인식 내용                                             | O           |
| validThru.conf    | Double | 유효 기간 인식 결과 신뢰도                                      |             |
| validThruBox      | Object | 유효 기간 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }     |             |

- 암호화된 항목들(cardNums[0].value, totalCardNum등)은 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어 있습니다(대칭 키 이용).

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### 신분증 분석 API

#### 요청

[URI]

| 메서드 | URI                            |
| ------ | ------------------------------ |
| POST   | /v2.1/appkeys/{appKey}/id-card |

[요청 헤더]

| 이름                | 값              | 설명                                 |
| ------------------- | --------------- | ------------------------------------ |
| X-NHN-Authorization | {Access Token}  | User Access Key 토큰                 |
| X-Key-Version       | {x-key-version} | 발급 받은 공개 키의 버전             |
| Symmetric-Key       | {symmetricKey}  | 발급 받은 공개 키로 암호화된 대칭 키 |

- {symmetricKey}는 반드시 **32byte 난수**로 생성해야 합니다.
- {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[Path Variable]

| 이름   | 값       | 설명                           |
| ------ | -------- | ------------------------------ |
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름  | 타입                | 설명        | 암호화 설명               |
| ----- | ------------------- | ----------- | ------------------------- |
| image | multipart/form–data | 이미지 파일 | 대칭 키로 암호화된 이미지 |

- 이미지 파일은 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).
- IV(초기화 벡터)는 대칭 키의 처음 16바이트(즉, 0~15번째 바이트)를 사용합니다.

[요청 본문]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/id-card' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${Access Token}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### 응답

[응답 헤더]

| 이름        | 설명                                            |
| ----------- | ----------------------------------------------- |
| Request-Key | 신분증 진위 확인 API 호출 시 사용할 Request-Key |

- **Request-Key를 진위 확인 API 호출에 사용하여 정상 응답을 받은 경우 사용된 Request-Key는 다시 사용할 수 없습니다.**
- **Request-Key는 발급 이후 1시간 동안 유효하며 그 이후에는 사용할 수 없습니다.**

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
        "resolution": "low",
        "idType": "resident",
        "keyValues": [
            {
                "key": "name",
                "value": "String",
                "conf": 0.67,
                "bbox": {
                    "x1": 191,
                    "y1": 75,
                    "x2": 240,
                    "y2": 75,
                    "x3": 240,
                    "y3": 95,
                    "x4": 191,
                    "y4": 95
                }
            },
            {
                "key": "residentNumber",
                "value": "String",
                "conf": 0.91,
                "bbox": {
                    "x1": 190,
                    "y1": 43,
                    "x2": 382,
                    "y2": 43,
                    "x3": 382,
                    "y3": 64,
                    "x4": 190,
                    "y4": 64
                }
            },
            {
                "key": "issueDate",
                "value": "String",
                "conf": 0.86,
                "bbox": {
                    "x1": 191,
                    "y1": 75,
                    "x2": 240,
                    "y2": 75,
                    "x3": 240,
                    "y3": 95,
                    "x4": 191,
                    "y4": 95
                },
            },
            {
                "key": "issuer",
                "value": "String",
                "conf": 0.8,
                "bbox": {
                    "x1": 19,
                    "y1": 10,
                    "x2": 148,
                    "y2": 10,
                    "x3": 148,
                    "y3": 52,
                    "x4": 19,
                    "y4": 52
                }
            }
        ],
        "boxes": [
            {
                "x1": 280,
                "y1": 271,
                "x2": 354,
                "y2": 271,
                "x3": 354,
                "y3": 305,
                "x4": 280,
                "y4": 305
            },
            ...
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

| 이름               | 타입   | 설명                                                            | 암호화 여부 |
| ------------------ | ------ | --------------------------------------------------------------- | ----------- |
| fileType           | String | 파일 확장자(.jpg, .png)                                         |             |
| resolution         | String | 권장 해상도(760\*480px) 이상이면 normal, 권장 해상도 미만은 low |             |
| idType             | String | resident(주민등록증), driver(운전면허증), passport(여권)        |             |
| keyValues          | List   |                                                                 |             |
| keyValues[0].key   | String |                                                                 |             |
| keyValues[0].value | String |                                                                 | O           |
| keyValues[0].bbox  | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }               |             |
| keyValues[0].conf  | Double | 인식 결과 신뢰도                                                |             |
| boxes              | List   | 인식 영역(Bounding box) 좌표 목록                               |
| boxes[0]           | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }               |

- **"idType"이 "resident"로 인식될 경우 KeyValues에 포함되는 목록**

| key                | value type | description         |
| ------------------ | ---------- | ------------------- |
| **name**           | string     | 인식된 이름         |
| **residentNumber** | string     | 인식된 주민등록번호 |
| **issueDate**      | string     | 인식된 발급 일자    |
| **issuer**         | string     | 인식된 발급기관     |

- **"idType"이 "driver"로 인식될 경우 KeyValues에 포함되는 목록**

| key                     | value type | description                                                                                     |
| ----------------------- | ---------- | ----------------------------------------------------------------------------------------------- |
| **driverLicenseNumber** | string     | 인식된 운전면허번호                                                                             |
| **licenseType**         | string     | 인식된 면허 종류(1종 보통 등)<br>2개 이상일 경우 문자열 내 "/"로 구분                           |
| **name**                | string     | 인식된 이름                                                                                     |
| **residentNumber**      | string     | 인식된 주민등록번호                                                                             |
| **condition**           | string     | 인식된 면허 조건<br>(운전면허증에 따라 해당 필드가 존재하지 않는 경우 해당 필드의 value는 none) |
| **serialNum**           | string     | 인식된 암호 일련번호                                                                            |
| **issueDate**           | string     | 인식된 발급 일자                                                                                |
| **issuer**              | string     | 인식된 발급기관                                                                                 |

- **"idType"이 "passport"로 인식될 경우 KeyValues에 포함되는 목록**

| key                 | value type | description                         |
| ------------------- | ---------- | ----------------------------------- |
| **passportType**    | string     | 인식된 여권 타입                    |
| **countryCode**     | string     | 인식된 국가 코드                    |
| **passportNo**      | string     | 인식된 여권 번호                    |
| **surName**         | string     | 인식된 성                           |
| **givenName**       | string     | 인식된 이름                         |
| **nationality**     | string     | 인식된 국적                         |
| **dateOfBirth**     | string     | 인식된 생년월일                     |
| **dateOfBirthYMD**  | string     | 인식된 생년월일<br>(YYYYMMDD 8자리) |
| **sex**             | string     | 인식된 성별                         |
| **dateOfIssue**     | string     | 인식된 발급일                       |
| **dateOfIssueYMD**  | string     | 인식된 발급일<br>(YYYYMMDD 8자리)   |
| **dateOfExpiry**    | string     | 인식된 만료일                       |
| **dateOfExpiryYMD** | string     | 인식된 만료일<br>(YYYYMMDD 8자리)   |
| **koreanName**      | string     | 인식된 한글 성명                    |
| **personalNo**      | string     | 인식된 주민등록번호                 |
| **MRZ1**            | string     | 기계 판독 영역1                     |
| **MRZ2**            | string     | 기계 판독 영역2                     |

- 암호화된 항목들(keyValues[0].value 등)은 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어 있습니다(대칭 키 이용).
- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### 신분증 진위 확인 API

#### 요청

[URI]

| 메서드 | URI                                         |
| ------ | ------------------------------------------- |
| POST   | /v2.1/appkeys/{appKey}/id-card/authenticity |

[요청 헤더]

| 이름                | 값              | 설명                                 |
| ------------------- | --------------- | ------------------------------------ |
| X-NHN-Authorization | {Access Token}  | User Access Key 토큰                 |
| X-Key-Version       | {x-key-version} | 발급 받은 공개 키의 버전             |
| Symmetric-Key       | {symmetricKey}  | 발급 받은 공개 키로 암호화된 대칭 키 |
| Request-Key         | {Request-Key}   | 신분증 분석 후 발급 받은 Request-Key |

- {symmetricKey}는 반드시 **32byte 난수**로 생성해야 합니다.
- {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[Path Variable]

| 이름   | 값       | 설명                           |
| ------ | -------- | ------------------------------ |
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름                | 타입   | 설명                                                                                                                                                               | idType             | 암호화 여부 | 필수 여부 |
| ------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------ | ----------- | --------- |
| idType              | String | resident(주민등록증), driver(운전면허증), passport(여권)                                                                                                           |                    | X           | O         |
| name                | String | 이름                                                                                                                                                               |                    | O           | O         |
| residentNumber      | String | 주민등록번호<br>- resident(주민등록증)의 경우 주민등록번호 숫자 13자리<br>- driver(운전면허증)의 경우 주민등록번호 앞 6자리와 뒤 첫 번째 1자리를 조합한 숫자 7자리 | resident, driver   | O           | O         |
| issueDate           | String | 발급 일자(YYYYMMDD)                                                                                                                                                | resident, passport | O           | O         |
| driverLicenseNumber | String | 12자리 운전면허번호                                                                                                                                                | driver             | O           | O         |
| serialNum           | String | 5~6자리 암호 일련번호                                                                                                                                              | driver             | O           | X         |
| passportNumber      | String | 여권 번호(9자리 영문 대문자, 숫자 조합)                                                                                                                            | passport           | O           | O         |
| birthDate           | String | 생년월일(YYYYMMDD)                                                                                                                                                 | passport           | O           | O         |
| expirationDate      | String | 만료 일자(YYYYMMDD)                                                                                                                                                | passport           | X           | O         |

- 암호화가 필요한 필드는 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).
- IV(초기화 벡터)는 대칭 키의 처음 16바이트(즉, 0~15번째 바이트)를 사용합니다.

[요청 본문]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/id-card/authenticity' \
-H 'X-NHN-Authorization: ${Access Token}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}' \
-H 'Request-Key: ${Request-Key}' \
-H 'Content-Type: application/json' \
--data-raw '{
  "idType": "driver",
  "name": "J/MTycDJ...",
  "residentNumber": "P12ztmj...",
  "driverLicenseNumber": "OHjVJrUMh...",
  "serialNum": "7tnTOKuKGJ..."
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
    "isAuthenticity": false
  }
}
```

[헤더]

| 이름          | 타입    | 설명                                            |
| ------------- | ------- | ----------------------------------------------- |
| isSuccessful  | Boolean | 진위 확인 API 성공 여부                         |
| resultCode    | Integer | 결과 코드                                       |
| resultMessage | String  | 결과 메시지(성공 시 success, 실패 시 오류 내용) |

[필드]

| 이름           | 타입    | 설명      |
| -------------- | ------- | --------- |
| isAuthenticity | Boolean | 진위 여부 |

### 신분증 분석(단독) API

#### 기존 신분증 분석 API와 차이점

- 진위 확인에 필요한 Request-Key를 포함하지 않습니다.
- 진위 확인이 불가능한 대신 낮은 요금이 부과됩니다.

#### 요청

[URI]

| 메서드 | URI                                        |
| ------ | ------------------------------------------ |
| POST   | /v2.1/appkeys/{appKey}/id-card/stand-alone |

[요청 헤더]

| 이름                | 값              | 설명                                 |
| ------------------- | --------------- | ------------------------------------ |
| X-NHN-Authorization | {Access Token}  | User Access Key 토큰                 |
| X-Key-Version       | {x-key-version} | 발급 받은 공개 키의 버전             |
| Symmetric-Key       | {symmetricKey}  | 발급 받은 공개 키로 암호화된 대칭 키 |

- {symmetricKey}는 반드시 **32byte 난수**로 생성해야 합니다.
- {symmetricKey}는 반드시 **RSA/ECB/PKCS1Padding** 방식으로 암호화되어야 합니다(공개 키 이용).

[Path Variable]

| 이름   | 값       | 설명                           |
| ------ | -------- | ------------------------------ |
| appKey | {appKey} | 통합 Appkey 또는 서비스 Appkey |

[필드]

| 이름  | 타입                | 설명        | 암호화 설명               |
| ----- | ------------------- | ----------- | ------------------------- |
| image | multipart/form–data | 이미지 파일 | 대칭 키로 암호화된 이미지 |

- 이미지 파일은 반드시 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어야 합니다(대칭 키 이용).
- IV(초기화 벡터)는 대칭 키의 처음 16바이트(즉, 0~15번째 바이트)를 사용합니다.

[요청 본문]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/id-card/stand-alone' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${Access Token}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
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
        "fileType": "png",
        "resolution": "low",
        "idType": "resident",
        "keyValues": [
            {
                "key": "name",
                "value": "String",
                "conf": 0.67,
                "bbox": {
                    "x1": 191,
                    "y1": 75,
                    "x2": 240,
                    "y2": 75,
                    "x3": 240,
                    "y3": 95,
                    "x4": 191,
                    "y4": 95
                }
            },
            {
                "key": "residentNumber",
                "value": "String",
                "conf": 0.91,
                "bbox": {
                    "x1": 190,
                    "y1": 43,
                    "x2": 382,
                    "y2": 43,
                    "x3": 382,
                    "y3": 64,
                    "x4": 190,
                    "y4": 64
                }
            },
            {
                "key": "issueDate",
                "value": "String",
                "conf": 0.86,
                "bbox": {
                    "x1": 191,
                    "y1": 75,
                    "x2": 240,
                    "y2": 75,
                    "x3": 240,
                    "y3": 95,
                    "x4": 191,
                    "y4": 95
                },
            },
            {
                "key": "issuer",
                "value": "String",
                "conf": 0.8,
                "bbox": {
                    "x1": 19,
                    "y1": 10,
                    "x2": 148,
                    "y2": 10,
                    "x3": 148,
                    "y3": 52,
                    "x4": 19,
                    "y4": 52
                }
            }
        ],
        "boxes": [
            {
                "x1": 280,
                "y1": 271,
                "x2": 354,
                "y2": 271,
                "x3": 354,
                "y3": 305,
                "x4": 280,
                "y4": 305
            },
            ...
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

| 이름               | 타입   | 설명                                                            | 암호화 여부 |
| ------------------ | ------ | --------------------------------------------------------------- | ----------- |
| fileType           | String | 파일 확장자(.jpg, .png)                                         |             |
| resolution         | String | 권장 해상도(760\*480px) 이상이면 normal, 권장 해상도 미만은 low |             |
| idType             | String | resident(주민등록증), driver(운전면허증), passport(여권)        |             |
| keyValues          | List   |                                                                 |             |
| keyValues[0].key   | String |                                                                 |             |
| keyValues[0].value | String |                                                                 | O           |
| keyValues[0].bbox  | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }               |             |
| keyValues[0].conf  | Double | 인식 결과 신뢰도                                                |             |
| boxes              | List   | 인식 영역(Bounding box) 좌표 목록                               |
| boxes[0]           | Object | 인식 영역 좌표 { x1, y1, x2, y2, x3, y3, x4, y4 }               |

- **"idType"이 "resident"로 인식될 경우 KeyValues에 포함되는 목록**

| key                | value type | description         |
| ------------------ | ---------- | ------------------- |
| **name**           | string     | 인식된 이름         |
| **residentNumber** | string     | 인식된 주민등록번호 |
| **issueDate**      | string     | 인식된 발급 일자    |
| **issuer**         | string     | 인식된 발급 기관    |

- **"idType"이 "driver"로 인식될 경우 KeyValues에 포함되는 목록**

| key                     | value type | description                                                                                     |
| ----------------------- | ---------- | ----------------------------------------------------------------------------------------------- |
| **driverLicenseNumber** | string     | 인식된 운전면허번호                                                                             |
| **licenseType**         | string     | 인식된 면허 종류(1종 보통 등)<br>2개 이상일 경우 문자열 내 "/"로 구분                           |
| **name**                | string     | 인식된 이름                                                                                     |
| **residentNumber**      | string     | 인식된 주민등록번호                                                                             |
| **condition**           | string     | 인식된 면허 조건<br>(운전면허증에 따라 해당 필드가 존재하지 않는 경우 해당 필드의 value는 none) |
| **serialNum**           | string     | 인식된 암호 일련번호                                                                            |
| **issueDate**           | string     | 인식된 발급 일자                                                                                |
| **issuer**              | string     | 인식된 발급 기관                                                                                |

- **"idType"이 "passport"로 인식될 경우 KeyValues에 포함되는 목록**

| key                 | value type | description                         |
| ------------------- | ---------- | ----------------------------------- |
| **passportType**    | string     | 인식된 여권 타입                    |
| **countryCode**     | string     | 인식된 국가 코드                    |
| **passportNo**      | string     | 인식된 여권 번호                    |
| **surName**         | string     | 인식된 성                           |
| **givenName**       | string     | 인식된 이름                         |
| **nationality**     | string     | 인식된 국적                         |
| **dateOfBirth**     | string     | 인식된 생년월일                     |
| **dateOfBirthYMD**  | string     | 인식된 생년월일<br>(YYYYMMDD 8자리) |
| **sex**             | string     | 인식된 성별                         |
| **dateOfIssue**     | string     | 인식된 발급일                       |
| **dateOfIssueYMD**  | string     | 인식된 발급일<br>(YYYYMMDD 8자리)   |
| **dateOfExpiry**    | string     | 인식된 만료일                       |
| **dateOfExpiryYMD** | string     | 인식된 만료일<br>(YYYYMMDD 8자리)   |
| **koreanName**      | string     | 인식된 한글 성명                    |
| **personalNo**      | string     | 인식된 주민등록번호                 |
| **MRZ1**            | string     | 기계 판독 영역1                     |
| **MRZ2**            | string     | 기계 판독 영역2                     |

- 암호화된 항목들(keyValues[0].value 등)은 **AES-256/CBC/PKCS7Padding** 방식으로 암호화되어 있습니다(대칭 키 이용).
- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)
