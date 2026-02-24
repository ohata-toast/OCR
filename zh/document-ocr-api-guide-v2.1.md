## AI Service > OCR > Document OCR > API v2.1 Guide

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

### Overview of v2.1 API

#### Changes from v2.0

* User Access Key token-based authentication has been applied.

#### Caution

* Check whether the request or response is Base64 encoded.
* Check the detailed mode of encryption and decryption (eg AES-256/CBC/PKCS7Padding).
* The symmetric key used for encryption must be generated as a 32 byte random number. For security, it is recommended to create and use a new symmetric key for each request.

### Issue Public Key

#### Request

[URI]

| Method | URI                                              |
|--------|--------------------------------------------------|
| GET    | /v2.1/appkeys/{appKey}/public-keys/{serviceName} |

[Request Header]

| Name                | Value          | Description          |
|---------------------|----------------|----------------------|
| X-NHN-Authorization | {Access Token} | User Access Key token |

[Path Variable]

| Name        | Value         | Description                                                                                                                                                |
|-------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| appKey      | {appKey}      | Integrated Appkey or Service Appkey                                                                                                                        |
| serviceName | {serviceName} | credit-card (when issuing the public key used for calling the credit card API),<br> id-card (when issuing the public key used for calling the ID card API) |

[Request Body]

```shell
curl -X GET 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/public-keys/{serviceName}' \
-H 'X-NHN-Authorization: ${Access Token}'
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
    "key": "String",
    "version": "0"
  }
}
```

[Header]

| Name          | Type    | Description                                                   |
|---------------|---------|---------------------------------------------------------------|
| isSuccessful  | Boolean | API success or not                                            |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error content on failure) |

[Field]

| Name           | Type   | Description                        |
|----------------|--------|------------------------------------|
| result         | Object | Public key required for encryption |
| result.key     | String | Public key (Base64 encoded)        |
| result.version | String | version of public key              |

* The public key is **Base64** encoded.

### Credit Card API

#### Credit Card Analysis API

#### Request

[URI]

| Method | URI                                |
|--------|------------------------------------|
| POST   | /v2.1/appkeys/{appKey}/credit-card |

[Request Header]

| Name                | Value           | Description                                        |
|---------------------|-----------------|----------------------------------------------------|
| X-NHN-Authorization | {Access Token}  | User Access Key token                              |
| X-Key-Version       | {x-key-version} | Version of the public key issued                   |
| Symmetric-Key       | {symmetricKey}  | Symmetric key encrypted with the issued public key |

* {symmetricKey} must be created as a **32-byte random number**.
* {symmetricKey} must be encrypted with the **RSA/ECB/PKCS1Padding** method (using public key).

[Path Variable]

| Name   | Value    | Description                         |
|--------|----------|-------------------------------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name  | Type                | Description | Encryption Description               |
|-------|---------------------|-------------|--------------------------------------|
| image | multipart/form-data | Image file  | Image encrypted with a symmetric key |

* Image files must be encrypted with the **AES-256/CBC/PKCS7Padding** method (using a symmetric key).
* The initialization vector (IV) uses the first 16 bytes (i.e., bytes 0-15) of the symmetric key.

[Request Body]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${Access Token}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
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

[Header]

| Name          | Type    | Description                                                   |
|---------------|---------|---------------------------------------------------------------|
| isSuccessful  | Boolean | Analysis API success or not                                   |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error content on failure) |

[Field]

| Name              | Type   | Description                                                                                                                         | Whether encrypted or not |
|-------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| fileType          | String | File extension (.jpg, .png)                                                                                                         |                          |
| resolution        | String | normal: the resolution is the recommended resolution (760\*480px) or above, low: the resolution is below the recommended resolution |                          |
| cardNums          | List   | List of card number recognition results                                                                                             |                          |
| cardNums[0].value | String | Recognition result                                                                                                                  | O                        |
| cardNums[0].conf  | Double | Confidence of the recognition result                                                                                                |                          |
| totalCardNum      | List   | Full card number recognition result                                                                                                 | O                        |
| cardNumBoxes      | List   | List of coordinates of the card number recognition area (bounding box)                                                              |                          |
| cardNumBoxes[0]   | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 }                                                                   |                          |
| validThru.value   | String | Expiration date recognition content                                                                                                 | O                        |
| validThru.conf    | Double | Confidence of expiration date recognition result                                                                                    |                          |
| validThruBox      | Object | Coordinates of the expiration date recognition area { x1, y1, x2, y2, x3, y3, x4, y4 }                                              |                          |

* Encrypted items (cardNums[0].value, totalCardNum, etc.) are encrypted with the **AES-256/CBC/PKCS7Padding** method (using symmetric key).

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### ID Card Analysis API

#### Request

[URI]

| Method | URI                            |
|--------|--------------------------------|
| POST   | /v2.1/appkeys/{appKey}/id-card |

[Request Header]

| Name                | Value           | Description                                        |
|---------------------|-----------------|----------------------------------------------------|
| X-NHN-Authorization | {Access Token}  | User Access Key token                              |
| X-Key-Version       | {x-key-version} | Version of the public key issued                   |
| Symmetric-Key       | {symmetricKey}  | Symmetric key encrypted with the issued public key |

* {symmetricKey} must be created as a **32-byte random number**.
* {symmetricKey} must be encrypted with the **RSA/ECB/PKCS1Padding** method (using public key).

[Path Variable]

| Name   | Value    | Description                         |
|--------|----------|-------------------------------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name  | Type                | Description | Encryption Description               |
|-------|---------------------|-------------|--------------------------------------|
| image | multipart/form-data | Image file  | Image encrypted with a symmetric key |

* Image files must be encrypted with the **AES-256/CBC/PKCS7Padding** method (using a symmetric key).
* The initialization vector (IV) uses the first 16 bytes (i.e., bytes 0-15) of the symmetric key.

[Request Body]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/id-card' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${Access Token}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### Response

[Response Header]

| Name        | Description                                                     |
|-------------|-----------------------------------------------------------------|
| Request-Key | Request-Key to be used when calling the Verify Authenticity API |

* **If you use the Request-Key to make a Authenticity API call and get a normal response, the Request-Key used cannot be reused.**
* **Request-Key is valid for 1 hour after issuance and cannot be used after that.**

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

[Header]

| Name          | Type    | Description                                                   |
|---------------|---------|---------------------------------------------------------------|
| isSuccessful  | Boolean | Analysis API success or not                                   |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error content on failure) |

[Field]

| Name               | Type   | Description                                                                                                                         | Whether encrypted or not |
|--------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| fileType           | String | File extension (.jpg, .png)                                                                                                         |                          |
| resolution         | String | normal: the resolution is the recommended resolution (760\*480px) or above, low: the resolution is below the recommended resolution |                          |
| idType             | String | resident(resident registration certificate), driver(driver license), passport (passport)                                            |                          |
| keyValues          | List   |                                                                                                                                     |                          |
| keyValues[0].key   | String |                                                                                                                                     |                          |
| keyValues[0].value | String |                                                                                                                                     | O                        |
| keyValues[0].bbox  | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 }                                                                   |                          |
| keyValues[0].conf  | Double | Confidence of the recognition result                                                                                                |                          | |  |
| boxes              | List   | List of bounding box coordinates                                                                                                    |
| boxes[0]           | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 }                                                                   |

* **List included in KeyValues when "idType" is recognized as "resident"**

| key                | value type | description                             |
|--------------------|------------|-----------------------------------------|
| **name**           | string     | Recognized name                         |
| **residentNumber** | string     | Recognized resident registration number |
| **issueDate**      | string     | Recognized issued date                  |
| **issuer**         | string     | Recognized issuer                       |

* **List to be included in KeyValues when "idType" is recognized as "driver"**

| key                     | value type | description                                                                                                                            |
|-------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------|
| **driverLicenseNumber** | string     | Recognized driver license number                                                                                                       |
| **licenseType**         | string     | Recognized driver license type (Class 1 Normal, etc.)<br>When the values are 2 or more, separate them with "/"                         |
| **name**                | string     | Recognized name                                                                                                                        |
| **residentNumber**      | string     | Recognized resident registration number                                                                                                |
| **condition**           | string     | Recognized driver license condition<br>(If the field does not exist according to the driver's license, the value of the field is none) |
| **serialNum**           | string     | Recognized serial number                                                                                                               |
| **issueDate**           | string     | Recognized issued date                                                                                                                 |
| **issuer**              | string     | Recognized issuer                                                                                                                      |



* **List included in KeyValues when "idType" is recognized as "passport"**

| key                 | value type | description                                       |
|---------------------|------------|---------------------------------------------------|
| **passportType**    | string     | Recognized passport type                          |
| **countryCode**     | string     | Recognized country code                           |
| **passportNo**      | string     | Recognized passport number                        |
| **surName**         | string     | Recognized surname                                |
| **givenName**       | string     | Recognized name                                   |
| **nationality**     | string     | Recognized nationality                            |
| **dateOfBirth**     | string     | Recognized birthdate                              |
| **dateOfBirthYMD**  | string     | Recognized birthdate<br>(YYYYMMDD 8 digits)       |
| **sex**             | string     | Recognized gender                                 |
| **dateOfIssue**     | string     | Recognized issue date                             |
| **dateOfIssueYMD**  | string     | Recognized issue date<br>(YYYYMMDD 8 digits)      |
| **dateOfExpiry**    | string     | Recognized expiration date                        |
| **dateOfExpiryYMD** | string     | Recognized expiration date<br>(YYYYMMDD 8 digits) |
| **koreanName**      | string     | Recognized Korean name                            |
| **personalNo**      | string     | Recognized resident registration number           |
| **MRZ1**            | string     | Machine readable zone 1                           |
| **MRZ2**            | string     | Machine readable zone 2                           |

* Encrypted items (keyValues[0].value, etc.) are encrypted with the **AES-256/CBC/PKCS7Padding** method (using symmetric key).
* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### Verify Authenticity API

#### Request

[URI]

| Method | URI                                         |
|--------|---------------------------------------------|
| POST   | /v2.1/appkeys/{appKey}/id-card/authenticity |

[Request Header]

| Name                | Value           | Description                                        |
|---------------------|-----------------|----------------------------------------------------|
| X-NHN-Authorization | {Access Token}  | User Access Key token                              |
| X-Key-Version       | {x-key-version} | Version of the public key issued                   |
| Symmetric-Key       | {symmetricKey}  | Symmetric key encrypted with the issued public key |
| Request-Key         | {Request-Key}   | Request-Key issued after ID card analysis          |

* {symmetricKey} must be created as a **32-byte random number**.
* {symmetricKey} must be encrypted with the **RSA/ECB/PKCS1Padding** method (using public key).

[Path Variable]

| Name   | Value    | Description                         |
|--------|----------|-------------------------------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name                | Type   | Description                                                                                                                                                                                                                                                             | idType             | Whether encrypted or not | Required |
|---------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|--------------------------|----------|
| idType              | String | resident(resident registration certificate), driver(driver license), passport (passport)                                                                                                                                                                                |                    | X                        | O        |
| name                | String | Name                                                                                                                                                                                                                                                                    |                    | O                        | O        |
| residentNumber      | String | Resident registration number<br>- For resident (resident registration certificate), 13 digits of resident registration number<br>- For a driver (driver's license), 7 digits that comprise of the first 6 digits and the first 1 digit of  resident registration number | resident, driver   | O                        | O        |
| issueDate           | String | Issued date (YYYYMMDD)                                                                                                                                                                                                                                                  | resident, passport | O                        | O        |
| driverLicenseNumber | String | 12 digits of driver license number                                                                                                                                                                                                                                      | driver             | O                        | O        |
| serialNum           | String | 5 and 6 digits of serial number                                                                                                                                                                                                                                         | driver             | O                        | X        |
| passportNumber      | String | Passport number (9 digits in uppercase letters and numbers)                                                                                                                                                                                                             | passport           | O                        | O        |
| birthDate           | String | Birthdate (YYYYMMDD)                                                                                                                                                                                                                                                    | passport           | O                        | O        |
| expirationDate      | String | Expiration date (YYYYMMDD)                                                                                                                                                                                                                                              | passport           | X                        | O        |

* A field that requires encryption must be encrypted with the **AES-256/CBC/PKCS7Padding** method (using a symmetric key).
* The initialization vector (IV) uses the first 16 bytes (i.e., bytes 0-15) of the symmetric key.

[Request Body]

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
    "isAuthenticity": false
  }
}
```

[Header]

| Name          | Type    | Description                                                   |
|---------------|---------|---------------------------------------------------------------|
| isSuccessful  | Boolean | Whether the Verify Authenticity API succeeds or not           |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error content on failure) |

[Field]

| Name           | Type    | Description                    |
|----------------|---------|--------------------------------|
| isAuthenticity | Boolean | Whether it is authentic or not |

### ID Card Analysis API (Stand alone)

#### Differences from the existing ID analysis API

* It does not contain the Request-Key required for authenticity verification.
* Authenticity cannot be verified, but a low fee is charged.

#### Request

[URI]

| Method | URI                                        |
|--------|--------------------------------------------|
| POST   | /v2.1/appkeys/{appKey}/id-card/stand-alone |

[Request Header]

| Name                | Value           | Description                                        |
|---------------------|-----------------|----------------------------------------------------|
| X-NHN-Authorization | {Access Token}  | User Access Key token                              |
| X-Key-Version       | {x-key-version} | Version of the public key issued                   |
| Symmetric-Key       | {symmetricKey}  | Symmetric key encrypted with the issued public key |

* {symmetricKey} must be created as a **32-byte random number**.
* {symmetricKey} must be encrypted with the **RSA/ECB/PKCS1Padding** method (using public key).

[Path Variable]

| Name   | Value    | Description                         |
|--------|----------|-------------------------------------|
| appKey | {appKey} | Integrated Appkey or Service Appkey |

[Field]

| Name  | Type                | Description | Encryption Description               |
|-------|---------------------|-------------|--------------------------------------|
| image | multipart/form-data | Image file  | Image encrypted with a symmetric key |

* Image files must be encrypted with the **AES-256/CBC/PKCS7Padding** method (using a symmetric key).
* The initialization vector (IV) uses the first 16 bytes (i.e., bytes 0-15) of the symmetric key.

[Request Body]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.1/appkeys/{appKey}/id-card/stand-alone' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: ${Access Token}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
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

[Header]

| Name          | Type    | Description                                                   |
|---------------|---------|---------------------------------------------------------------|
| isSuccessful  | Boolean | Analysis API success or not                                   |
| resultCode    | Integer | Result code                                                   |
| resultMessage | String  | Result message (success on success, error content on failure) |

[Field]

| Name               | Type   | Description                                                                                                                         | Whether encrypted or not |
|--------------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| fileType           | String | File extension (.jpg, .png)                                                                                                         |                          |
| resolution         | String | normal: the resolution is the recommended resolution (760\*480px) or above, low: the resolution is below the recommended resolution |                          |
| idType             | String | resident(resident registration certificate), driver(driver license), passport (passport)                                            |                          |
| keyValues          | List   |                                                                                                                                     |                          |
| keyValues[0].key   | String |                                                                                                                                     |                          |
| keyValues[0].value | String |                                                                                                                                     | O                        |
| keyValues[0].bbox  | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 }                                                                   |                          |
| keyValues[0].conf  | Double | Confidence of the recognition result                                                                                                |                          |          
| boxes              | List   | List of bounding box coordinates                                                                                                    |
| boxes[0]           | Object | Coordinates of recognized area { x1, y1, x2, y2, x3, y3, x4, y4 }                                                                   |

* **List included in KeyValues when "idType" is recognized as "resident"**

| key                | value type | description                             |
|--------------------|------------|-----------------------------------------|
| **name**           | string     | Recognized name                         |
| **residentNumber** | string     | Recognized resident registration number |
| **issueDate**      | string     | Recognized issued date                  |
| **issuer**         | string     | Recognized issuer                       |

* **List to be included in KeyValues when "idType" is recognized as "driver"**

| key                     | value type | description                                                                                                                            |
|-------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------|
| **driverLicenseNumber** | string     | Recognized driver license number                                                                                                       |
| **licenseType**         | string     | Recognized driver license type (Class 1 Normal, etc.)<br>When the values are 2 or more, separate them with "/"                         |
| **name**                | string     | Recognized name                                                                                                                        |
| **residentNumber**      | string     | Recognized resident registration number                                                                                                |
| **condition**           | string     | Recognized driver license condition<br>(If the field does not exist according to the driver's license, the value of the field is none) |
| **serialNum**           | string     | Recognized serial number                                                                                                               |
| **issueDate**           | string     | Recognized issued date                                                                                                                 |
| **issuer**              | string     | Recognized issuer                                                                                                                      |



* **List included in KeyValues when "idType" is recognized as "passport"**

| key                 | value type | description                                       |
|---------------------|------------|---------------------------------------------------|
| **passportType**    | string     | Recognized passport type                          |
| **countryCode**     | string     | Recognized country code                           |
| **passportNo**      | string     | Recognized passport number                        |
| **surName**         | string     | Recognized surname                                |
| **givenName**       | string     | Recognized name                                   |
| **nationality**     | string     | Recognized nationality                            |
| **dateOfBirth**     | string     | Recognized birthdate                              |
| **dateOfBirthYMD**  | string     | Recognized birthdate<br>(YYYYMMDD 8 digits)       |
| **sex**             | string     | Recognized gender                                 |
| **dateOfIssue**     | string     | Recognized issue date                             |
| **dateOfIssueYMD**  | string     | Recognized issue date<br>(YYYYMMDD 8 digits)      |
| **dateOfExpiry**    | string     | Recognized expiration date                        |
| **dateOfExpiryYMD** | string     | Recognized expiration date<br>(YYYYMMDD 8 digits) |
| **koreanName**      | string     | Recognized Korean name                            |
| **personalNo**      | string     | Recognized resident registration number           |
| **MRZ1**            | string     | Machine readable zone 1                           |
| **MRZ2**            | string     | Machine readable zone 2                           |

* Encrypted items (keyValues[0].value, etc.) are encrypted with the **AES-256/CBC/PKCS7Padding** method (using symmetric key).
* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)
