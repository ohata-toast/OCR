## AI Service > OCR > General OCR > 오류 코드

| resultCode | resultKey                           | resultMessage                                                                |
| ---------- | ----------------------------------- | ---------------------------------------------------------------------------- |
| 0          | SUCCESS                             | SUCCESS                                                                      |
| -1         | FAIL                                | Unknown error.                                                               |
| 400        | BAD_REQUEST                         | Bad Request                                                                  |
| 401        | UNAUTHORIZED                        | Unauthorized                                                                 |
| 403        | FORBIDDEN                           | Forbidden                                                                    |
| 404        | NOT_FOUND                           | Not Found                                                                    |
| 405        | METHOD_NOT_ALLOWED                  | Method Not Allowed                                                           |
| 415        | UNSUPPORTED_MEDIA_TYPE              | Unsupported Media Type                                                       |
| 4000001    | INVALID_PARAMETER                   | Invalid parameter.                                                           |
| 4000002    | INVALID_FILE                        | Invalid file.                                                                |
| 4000003    | INVALID_FILE_TYPE                   | Invalid file type.                                                           |
| 4000004    | UPLOADED_FILE_IS_EMPTY              | Uploaded file is empty.                                                      |
| 4000501    | GENERAL_OCR_NOT_FOUND               | General OCR not found in the image.                                          |
| 4000502    | GENERAL_OCR_IMAGE_SIZE_EXCEED       | Image size exceeded. Please check image URL.                                 |
| 4000503    | GENERAL_OCR_IMAGE_DOWNLOAD_TIME_OUT | Image download takes more than 20 seconds. Please check image URL.           |
| 4000504    | GENERAL_OCR_IMAGE_DOWNLOAD_FAIL     | Image download fail. Please check image URL.                                 |
| 4000505    | GENERAL_OCR_IMAGE_OUT_OF_RATIO      | Image out of ratio. Please check image.                                      |
| 4010001    | INVALID_APPKEY_SECRETKEY            | Invalid appKey or secretKey.                                                 |
| 4010002    | INVALID_UUID                        | Invalid uuid.                                                                |
| 4010003    | NOT_ALLOWED_USER                    | Not allowed user.                                                            |
| 4010004    | INVALID_PROJECT                     | Invalid project.                                                             |
| 4010005    | UNAUTHORIZED_ROLE                   | Unauthorized role.                                                           |
| 4010006    | INVALID_TOKEN                       | Invalid token.                                                               |
| 4010007    | PERMISSION_DENIED                   | Permission denied.                                                           |
| 4131000    | MAX_UPLOAD_SIZE_EXCEEDED            | Request size is larger than permissible limit. the permissible limit is 5mb. |
| 5000001    | INTERNAL_API_FAIL                   | Internal Api fail.                                                           |
| 5000002    | ERROR_PARSING_FAIL                  | Error parsing fail.                                                          |
| 5000003    | DATABASE_FAIL                       | Database server error.                                                       |
| 5000004    | RESOURCE_DELETE_FAIL                | All or some resource delete fail.                                            |
| 5000005    | FILE_READ_FAIL                      | File read fail.                                                              |
| 5000503    | API_TEMPORARILY_UNAVAILABLE         | The API you requested is temporarily unavailable. Please try again later.    |
| 5005001    | OCR_GENERAL_API_FAIL                | General OCR Api fail.                                                        |
| 5005002    | OCR_GENERAL_API_RETURN_EMPTY        | General OCR Api returned empty body.                                         |
| 5005003    | OCR_GENERAL_RECOGNITION_FAIL        | General OCR failed to recognize the image.                                   |
