## AI Service > OCR > Document AI > API v1.1 Guide

## Document AI API Common Information

### API Endpoints

| Region                | Endpoint                            |
| --------------------- | ----------------------------------- |
| Korea (Pangyo) Region | https://ocr.api.nhncloudservice.com |

### Authentication and Authorization

Document AI uses User Access Key tokens for authentication and authorization when making API calls. The User Access Key token is a temporary, Bearer-type access token issued from a User Access Key.
For more information on issuing and using User Access Key tokens, please refer to the [User Access Key Token](https://docs.alpha-nhncloud.com/en/nhncloud/en/public-api/user-access-key-token).

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
| 4010006    | Invalid token.                                                                             | Invalid token                         |
| 4010007    | Permission denied.                                                                         | Permission denied                     |
| 4131000    | Request size is larger than permissible limit. the permissible limit is 5mb.               | Request size exceeds limit (5MB)      |

### Document AI Analysis API

[URI]

| Method | URI                                |
| ------ | ---------------------------------- |
| POST   | /v1.1/appkeys/{appKey}/document-ai |

[Request Header]

| Name                | Value               | Description              |
| ------------------- | ------------------- | ------------------------ |
| X-NHN-Authorization | {Access Token}      | User Access Key token    |
| Content-Type        | multipart/form-data | Content type             |

[Request Body]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/document-ai' \
-H 'X-NHN-Authorization: ${Access Token}' \
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

[Field]

| Name        | Type   | Description        |
| ----------- | ------ | ------------------ |
| llmResponse | String | LLM analysis answer |
