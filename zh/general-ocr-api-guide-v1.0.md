## AI Service > OCR > General OCR > API v1.0 Guide

## General OCR API Common Information

### API Endpoints

| Region               | Endpoint                            |
| -------------------- | ----------------------------------- |
| Korea (Pangyo) Region | https://ocr.api.nhncloudservice.com |

### Authentication and Authorization

AppKey and SecretKey are required to use the General OCR API.
An Appkey is a unique authentication key issued for each NHN Cloud service, used to identify the service and validate API requests. A SecretKey is a private key used to control access to the API.
For more information on checking and using Appkeys and SecretKeys, please refer to the [Appkey](/nhncloud/en/public-api/appkey).

Project Integrated Appkey can be used in place of the Appkey. Project Integrated Appkey is a common authentication key that can be shared across multiple services within a single NHN Cloud project.
For more information on creating and using Project Integrated Appkeys, please refer to the [Project Integrated Appkey](/nhncloud/en/public-api/project-appkey).

### Common Response Information

All API requests return HTTP 200 OK. The success or failure of an API request can be determined by referring to the header field in the response body.

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

| Name          | Type    | Description                                                         |
| ------------- | ------- | ------------------------------------------------------------------- |
| resultCode    | int     | Response code<br>Returns 0 on success, error code on failure        |
| resultMessage | String  | Response message                                                    |
| isSuccessful  | boolean | Success or not                                                      |

### Error Codes

#### Common

| Error Code | Error Message                                                                              | Description                                      |
| ---------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| -1         | Unknown error.                                                                             | Unknown error                                    |
| 4000001    | Invalid parameter.                                                                         | Invalid parameter                                |
| 4000002    | Invalid file.                                                                              | Invalid file                                     |
| 4000003    | Invalid file type.                                                                         | Invalid file type                                |
| 4000004    | Uploaded file is empty.                                                                    | Uploaded file is empty                           |
| 4000005    | Required headers is missing.                                                               | Required headers are missing                     |
| 4000006    | Api call limit exceeded, If you need to adjust the limit, please contact customer service. | API call limit exceeded                          |
| 4131000    | Request size is larger than permissible limit. the permissible limit is 5mb.               | Request size exceeds the permissible limit (5MB) |

### General OCR API

#### Request

[URI]

| Method | URI                            |
| ------ | ------------------------------ |
| POST   | /v1.0/appkeys/{appKey}/general |

#### Request with Image File

[Request Header]

| Name          | Value               | Description                          |
| ------------- | ------------------- | ------------------------------------ |
| Authorization | {secretKey}         | Security key issued from the console |
| Content-Type  | multipart/form-data | Content type                         |

[Request Body]

- Put binary data of the image file.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[Field]

| Name  | Type                | Description |
| ----- | ------------------- | ----------- |
| image | multipart/form-data | Image file  |

#### Request with Image URL

[Request Header]

| Name          | Value            | Description                          |
| ------------- | ---------------- | ------------------------------------ |
| Authorization | {secretKey}      | Security key issued from the console |
| Content-Type  | application/json | Content type                         |

[Request Body]

- Put the image URL.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[Field]

| Name     | Type   | Description |
| -------- | ------ | ----------- |
| imageUrl | String | Image URL   |

- When directly specifying the port in the image URL, only ports 80, 443, and 10000-12000 are available.

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

[Header]

| Name          | Type    | Description                                                   |
| ------------- | ------- | ------------------------------------------------------------- |
| isSuccessful  | Boolean | Analysis API success or not                                   |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error details on failure) |

[Field]

| Name                                    | Type   | Description                                                       |
| --------------------------------------- | ------ | ----------------------------------------------------------------- |
| fileType                                | String | File extension (jpg, png)                                         |
| listOfInferTexts                        | List   | List of recognition results                                       |
| listOfInferTexts[0].inferTexts[0].value | String | Recognized content                                                |
| listOfInferTexts[0].inferTexts[0].conf  | Double | Confidence of the recognition result                              |
| listOfBoundingBoxes                     | List   | List of recognized area (bounding box) coordinates                |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 } |

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### General OCR Segmentation Recognition API

#### Request

[URI]

| Method | URI                                     |
| ------ | --------------------------------------- |
| POST   | /v1.0/appkeys/{appKey}/general/cropping |

#### Request with Image File

[Request Header]

| Name          | Value               | Description                          |
| ------------- | ------------------- | ------------------------------------ |
| Authorization | {secretKey}         | Security key issued from the console |
| Content-Type  | multipart/form-data | Content type                         |

[Request Body]

- Put binary data of the image file.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[Field]

| Name  | Type                | Description |
| ----- | ------------------- | ----------- |
| image | multipart/form-data | Image file  |

#### Request with Image URL

[Request Header]

| Name          | Value            | Description                          |
| ------------- | ---------------- | ------------------------------------ |
| Authorization | {secretKey}      | Security key issued from the console |
| Content-Type  | application/json | Content type                         |

[Request Body]

- Put the image URL.

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[Field]

| Name     | Type   | Description |
| -------- | ------ | ----------- |
| imageUrl | String | Image URL   |

- When directly specifying the port in the image URL, only ports 80, 443, and 10000-12000 are available.

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

[Header]

| Name          | Type    | Description                                                   |
| ------------- | ------- | ------------------------------------------------------------- |
| isSuccessful  | Boolean | Analysis API success or not                                   |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error details on failure) |

[Field]

| Name                                    | Type    | Description                                                                                     |
| --------------------------------------- | ------- | ----------------------------------------------------------------------------------------------- |
| fileType                                | String  | File extension (jpg, png)                                                                       |
| listOfInferTexts                        | List    | List of recognition results                                                                     |
| listOfInferTexts[0].inferTexts[0].value | String  | Recognized content                                                                              |
| listOfInferTexts[0].inferTexts[0].conf  | Double  | Confidence of the recognition result                                                            |
| listOfBoundingBoxes                     | List    | List of recognized area (bounding box) coordinates                                              |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object  | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 }                               |
| slicesImages                            | Integer | Number of images internally segmented based on the aspect ratio of the input image              |

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)
