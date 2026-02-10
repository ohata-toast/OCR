## AI Service > OCR > Document OCR > 오류 코드

| resultCode | resultKey                                 | resultMessage                                                                              |
| ---------- | ----------------------------------------- | ------------------------------------------------------------------------------------------ |
| 0          | SUCCESS                                   | SUCCESS                                                                                    |
| -1         | FAIL                                      | Unknown error.                                                                             |
| 400        | BAD_REQUEST                               | Bad Request                                                                                |
| 401        | UNAUTHORIZED                              | Unauthorized                                                                               |
| 403        | FORBIDDEN                                 | Forbidden                                                                                  |
| 404        | NOT_FOUND                                 | Not Found                                                                                  |
| 405        | METHOD_NOT_ALLOWED                        | Method Not Allowed                                                                         |
| 415        | UNSUPPORTED_MEDIA_TYPE                    | Unsupported Media Type                                                                     |
| 4000001    | INVALID_PARAMETER                         | Invalid parameter.                                                                         |
| 4000002    | INVALID_FILE                              | Invalid file.                                                                              |
| 4000003    | INVALID_FILE_TYPE                         | Invalid file type.                                                                         |
| 4000004    | UPLOADED_FILE_IS_EMPTY                    | Uploaded file is empty.                                                                    |
| 4000005    | MISSING_REQUIRED_HEADER                   | Required headers is missing.                                                               |
| 4000006    | API_CALL_LIMIT_EXCEEDED                   | Api call limit exceeded, If you need to adjust the limit, please contact customer service. |
| 4000101    | INVALID_BUSINESS_REGISTRATION_FORMAT      | Invalid business registration format.                                                      |
| 4000102    | NOT_EXIST_BUSINESS_NUMBER                 | Business number that does not exist.                                                       |
| 4000103    | PDF_PASSWORD_PROTECTED                    | PDF file is password protected.                                                            |
| 4000111    | CREDIT_CARD_INFO_NOT_FOUND                | Credit card info not found.                                                                |
| 4000121    | ID_CARD_INFO_NOT_FOUND                    | Id card info not found.                                                                    |
| 4000122    | AUTHENTICITY_REQUEST_KEY_INVALID          | Request Key is invalid or expired.                                                         |
| 4000123    | REGISTRATION_CARD_AUTHENTICATION_LOCKED   | Registration card authentication is locked.                                                |
| 4000301    | INVALID_KEY_VERSION                       | Invalid key version.                                                                       |
| 4000302    | INVALID_SYMMETRIC_KEY                     | Invalid symmetric key.                                                                     |
| 4000303    | INVALID_ENCRYPTED_DATA                    | Invalid encrypted data.                                                                    |
| 4000401    | SERVICE_NOT_ENABLED                       | Service not enabled. Please submit service use request.                                    |
| 4000402    | SERVICE_USE_REQUEST_ALREADY_IN_PROGRESS   | Service use request already in progress.                                                   |
| 4000403    | SERVICE_USE_REQUEST_NOT_IN_PROGRESS       | Service use request not in progress.                                                       |
| 4000404    | SERVICE_ALREADY_ENABLED                   | Service already enabled.                                                                   |
| 4010001    | INVALID_APPKEY_SECRETKEY                  | Invalid appKey or secretKey.                                                               |
| 4010002    | INVALID_UUID                              | Invalid uuid.                                                                              |
| 4010003    | NOT_ALLOWED_USER                          | Not allowed user.                                                                          |
| 4010004    | INVALID_PROJECT                           | Invalid project.                                                                           |
| 4010005    | UNAUTHORIZED_ROLE                         | Unauthorized role.                                                                         |
| 4010006    | INVALID_TOKEN                             | Invalid token.                                                                             |
| 4010007    | PERMISSION_DENIED                         | Permission denied.                                                                         |
| 4131000    | MAX_UPLOAD_SIZE_EXCEEDED                  | Request size is larger than permissible limit. the permissible limit is 5mb.               |
| 5000001    | INTERNAL_API_FAIL                         | Internal Api fail.                                                                         |
| 5000002    | ERROR_PARSING_FAIL                        | Error parsing fail.                                                                        |
| 5000003    | DATABASE_FAIL                             | Database server error.                                                                     |
| 5000004    | RESOURCE_DELETE_FAIL                      | All or some resource delete fail.                                                          |
| 5000005    | FILE_READ_FAIL                            | File read fail.                                                                            |
| 5000006    | DECRYPT_FAIL                              | Decrypt fail.                                                                              |
| 5000007    | ENCRYPT_FAIL                              | Encrypt fail.                                                                              |
| 5000503    | API_TEMPORARILY_UNAVAILABLE               | The API you requested is temporarily unavailable. Please try again later.                  |
| 5003001    | OCR_DOCUMENT_BUSINESS_API_FAIL            | Document(business) OCR Api fail.                                                           |
| 5003002    | OCR_DOCUMENT_BUSINESS_API_RETURN_EMPTY    | Document(business) OCR Api returned empty body.                                            |
| 5003003    | OCR_DOCUMENT_BUSINESS_RECOGNITION_FAIL    | Document(business) OCR failed to recognize the document.                                   |
| 5003004    | OCR_DOCUMENT_BUSINESS_STATUS_CHECK_FAIL   | Document(business) Status Api fail.                                                        |
| 5003011    | OCR_DOCUMENT_CREDIT_CARD_API_FAIL         | Document(credit card) OCR Api fail.                                                        |
| 5003012    | OCR_DOCUMENT_CREDIT_CARD_API_RETURN_EMPTY | Document(credit card) OCR Api returned empty body.                                         |
| 5003013    | OCR_DOCUMENT_CREDIT_CARD_RECOGNITION_FAIL | Document(credit card) OCR failed to recognize the document.                                |
| 5003021    | OCR_DOCUMENT_ID_CARD_API_FAIL             | Document(id card) OCR Api fail.                                                            |
| 5003022    | OCR_DOCUMENT_ID_CARD_API_RETURN_EMPTY     | Document(id card) OCR Api returned empty body.                                             |
| 5003023    | OCR_DOCUMENT_ID_CARD_RECOGNITION_FAIL     | Document(id card) OCR failed to recognize the document.                                    |
