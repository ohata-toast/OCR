## AI Service > OCR > Document OCR > API v1.1 Guide

## Document OCR API Common Information

### API Endpoints

| Region                | Endpoint                            |
| --------------------- | ----------------------------------- |
| Korea (Pangyo) Region | https://ocr.api.nhncloudservice.com |

### Authentication and Authorization

Document OCR uses User Access Key tokens for authentication and authorization when making API calls. The User Access Key token is a temporary, Bearer-type access token issued from a User Access Key.
For more information on issuing and using User Access Key tokens, please refer to the [User Access Key Token](https://docs.nhncloud.com/en/nhncloud/en/public-api/user-access-key-token).

### Common Response Information

All API requests return HTTP 200 OK. The success or failure of an API request can be determined by referring to the header in the Response Body.

<details>
  <summary><strong>Success Response</strong></summary>

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
  <summary><strong>Failure Response</strong></summary>

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

| Name          | Type    | Description                                                        |
| ------------- | ------- | ------------------------------------------------------------------ |
| resultCode    | int     | Response code<br>0 on success, error code on failure               |
| resultMessage | String  | Response message                                                   |
| isSuccessful  | boolean | Success or not                                                     |

### Error Codes

#### Common

| Error Code | Error Message                                                                              | Description                                      |
| ---------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| -1         | Unknown error.                                                                             | Unknown error                                    |
| 4000001    | Invalid parameter.                                                                         | Invalid parameter                                |
| 4000002    | Invalid file.                                                                              | Invalid file                                     |
| 4000003    | Invalid file type.                                                                         | Invalid file type                                |
| 4000004    | Uploaded file is empty.                                                                    | Uploaded file is empty                           |
| 4000005    | Required headers is missing.                                                               | Required headers missing                         |
| 4000006    | Api call limit exceeded, If you need to adjust the limit, please contact customer service. | API call limit exceeded                          |
| 4010006    | Invalid token.                                                                             | Invalid token                                    |
| 4010007    | Permission denied.                                                                         | Permission denied                                |
| 4131000    | Request size is larger than permissible limit. the permissible limit is 5mb.               | Request size exceeds the permissible limit (5MB) |

### Business Registration Certificate Analysis API

#### Request

[URI]

| Method | URI                             |
|--------|---------------------------------|
| POST   | /v1.1/appkeys/{appKey}/business |

[Request Header]

| Name                | Value          | Description              |
|---------------------|----------------|--------------------------|
| X-NHN-Authorization | {Access Token} | Issued Access Token      |

[Path Variable]

| Name   | Value    | Description                         |
|--------|----------|-------------------------------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Request Body]

* Put binary data of the image file.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/business' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${Access Token}'
```

[Field]

| Name  | Type                | Description |
|-------|---------------------|-------------|
| image | multipart/form–data | Image file  |

#### Response

[Response Body]

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

[Header]

| Name          | Type    | Description                                                   |
|---------------|---------|---------------------------------------------------------------|
| isSuccessful  | Boolean | Analysis API success or not                                   |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error details on failure) |

[Field]

| Name               | Type   | Description                                                                                                                            |
|--------------------|--------|----------------------------------------------------------------------------------------------------------------------------------------|
| fileType           | String | File extension (.pdf, .jpg, .png)                                                                                                      |
| keyValues          | List   | List of recognition results                                                                                                            |
| keyValues[0].key   | String | Recognized item name                                                                                                                   |
| keyValues[0].value | String | Recognized content                                                                                                                     |
| keyValues[0].conf  | Double | Confidence of the recognition result                                                                                                   |
| resolution         | String | normal: the resolution is the recommended resolution (HD 1280*720px) or above, low: the resolution is below the recommended resolution |
| unitType           | String | Coordinate unit for boxes (pixel by default, point for PDF)                                                                            |
| boxes              | List   | List of recognized area (bounding box) coordinates                                                                                     |
| boxes[0]           | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 }                                                                      |

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### Retrieve Business Registration Stoppage/Closure API

#### Request

[URI]

| Method | URI                                    |
|--------|----------------------------------------|
| POST   | /v1.1/appkeys/{appKey}/business/status |

[Request Header]

| Name                | Value          | Description              |
|---------------------|----------------|--------------------------|
| X-NHN-Authorization | {Access Token} | Issued Access Token      |

[Path Variable]

| Name   | Value    | Description                         |
|--------|----------|-------------------------------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name           | Type   | Description                                          |
|----------------|--------|------------------------------------------------------|
| businessNumber | String | Business registration certificate number (10 digits) |

[Request Body]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/business/status' \
-H 'X-NHN-Authorization: ${Access Token}' \
--data-raw '{
  "businessNumber": "1234567890"
}'
```

#### Response

[Response Body]

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

[Header]

| Name          | Type    | Description                                                 |
|---------------|---------|-------------------------------------------------------------|
| isSuccessful  | Boolean | Whether Retrieve stoppage/closure API successful or not     |
| resultCode    | Integer | Result code                                                 |
| resultMessage | String  | Result message (Success when successful, error when failed) |

[Field]

| Name          | Type   | Description                                                          |
|---------------|--------|----------------------------------------------------------------------|
| statusCode    | String | Business registration certificate status code (Hometax result code) |
| statusMessage | String | Business registration certificate status message                    |

* **List of Business Registration Certificate Statuses by "statusCode"**

| Code value | Description                                                                                                                      |
|------------|----------------------------------------------------------------------------------------------------------------------------------|
| 00         | Businesses not in business                                                                                                       |
| 01         | VAT general taxpayers                                                                                                            |
| 02         | VAT simplified taxpayer                                                                                                          |
| 03         | Exempt from VAT                                                                                                                  |
| 04         | Non-profit corporation or organization with a unique number that is not engaged in a profitable business. National organizations |
| 05         | Inactive                                                                                                                         |
| 06         | Closed                                                                                                                           |
| 09         | Others                                                                                                                           |
