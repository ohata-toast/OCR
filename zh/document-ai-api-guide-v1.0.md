## AI Service > OCR > Document AI > API v1.0 Guide

## Document AI API Common Information

### API Endpoints

| Region                | Endpoint                            |
| --------------------- | ----------------------------------- |
| Korea (Pangyo) Region | https://ocr.api.nhncloudservice.com |

### Authentication and Authorization

AppKey and SecretKey are required to use the Document AI API.
An Appkey is a unique authentication key issued for each NHN Cloud service, used to identify the service and validate API requests. A SecretKey is a private key used to control access to the API.
For more information on checking and using Appkeys and SecretKeys, please refer to the [Appkey](https://docs.alpha-nhncloud.com/en/nhncloud/en/public-api/appkey).

Project Integrated Appkey can be used in place of the Appkey. Project Integrated Appkey is a common authentication key that can be shared across multiple services within a single NHN Cloud project.
For more information on creating and using Project Integrated Appkeys, please refer to the [Project Integrated Appkey](https://docs.alpha-nhncloud.com/en/nhncloud/en/public-api/project-appkey).

### Common Response Information

All API requests return HTTP 200 OK. The success or failure of an API request can be determined by referring to the header field in the Response Body.

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
        "llmResponse": ...
    },
    ...
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

| Name          | Type    | Description                                                   |
| ------------- | ------- | ------------------------------------------------------------- |
| resultCode    | int     | Response code<br>Returns 0 on success, error code on failure |
| resultMessage | String  | Response message                                              |
| isSuccessful  | boolean | Success or not                                                |

### Error Codes

#### Common

| Error Code | Error Message                                                                              | Description                           |
| ---------- | ------------------------------------------------------------------------------------------ | ------------------------------------- |
| -1         | Unknown error.                                                                             | Unknown error                         |
| 4000001    | Invalid parameter.                                                                         | Invalid parameter                     |
| 4000002    | Invalid file.                                                                              | Invalid file                          |
| 4000003    | Invalid file type.                                                                         | Invalid file type                     |
| 4000004    | Uploaded file is empty.                                                                    | Uploaded file is empty                |
| 4000005    | Required headers is missing.                                                               | Required headers missing              |
| 4000006    | Api call limit exceeded, If you need to adjust the limit, please contact customer service. | API call limit exceeded               |
| 4131000    | Request size is larger than permissible limit. the permissible limit is 5mb.               | Request size exceeds limit (5MB)      |

### Document AI Analysis API

[URI]

| Method | URI                                |
| ------ | ---------------------------------- |
| POST   | /v1.0/appkeys/{appKey}/document-ai |

[Request Header]

| Name          | Value               | Description                       |
| ------------- | ------------------- | --------------------------------- |
| Authorization | {secretKey}         | Security key issued from console  |
| Content-Type  | multipart/form-data | Content type                      |

[Request Body]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/document-ai' \
-H 'Authorization: ${secretKey}' \
-F 'image=@sample.png' \
-F 'prompt="Give me a quick summary"' \
-F 'documentTypeCode="GENERAL"'
```

[Field]

| Name             | Type | Required | Default | Valid Range                                    | Description                                                                                                            |
| ---------------- | ---- | -------- | ------- | ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| image            | file | O        |         |                                                | Image file                                                                                                             |
| documentTypeCode | text | X        | GENERAL | GENERAL, BUSINESS_REGISTRATION, BUSINESS_CARD  | Document type<br> General: GENERAL <br> Business registration: BUSINESS_REGISTRATION <br> Business card: BUSINESS_CARD |
| prompt           | text | O        |         |                                                | Question content<br>Up to 1000 characters                                                                              |

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
    "llmResponse": "This document appears to have been written using 'lorem ipsum', a meaningless text, to create a sentence that has letters but is difficult and illegible to read."
  }
}
```

[Header]

| Name          | Type    | Description                                                     |
| ------------- | ------- | --------------------------------------------------------------- |
| isSuccessful  | Boolean | Analysis API success or not                                     |
| resultCode    | Integer | Result code                                                     |
| resultMessage | String  | Result message (success on success, error content on failure)   |

[Field]

| Name        | Type   | Description        |
| ----------- | ------ | ------------------ |
| llmResponse | String | LLM analysis answer |
