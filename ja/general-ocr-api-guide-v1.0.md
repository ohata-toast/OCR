## AI Service > OCR > General OCR > API v1.0 ガイド

## General OCR API 共通情報

### APIエンドポイント

| リージョン              | エンドポイント                      |
| ----------------------- | ----------------------------------- |
| 韓国(パンギョ)リージョン | https://ocr.api.nhncloudservice.com |

### 認証及び権限

General OCR APIを使用するには、AppkeyとSecretKeyが必要です。
Appkeyは、NHN Cloudの各サービスごとに発行される固有の認証キーであり、APIリクエスト時のサービス識別と有効性検証に使用されます。SecretKeyは、APIへのアクセスを制御するシークレットキーです。
Appkey及びSecretKeyの確認及び使用に関する詳細は、[Appkey](https://docs.alpha-nhncloud.com/ja/nhncloud/ja/public-api/appkey)を参照してください。

Appkeyの代わりに、プロジェクト統合Appkeyを使用することも可能です。プロジェクト統合Appkeyは、NHN Cloudの1つのプロジェクト内の複数のサービスに対して共通で使用できる認証キーです。
プロジェクト統合Appkeyの作成及び使用に関する詳細は、[プロジェクト統合Appkey](https://docs.alpha-nhncloud.com/ja/nhncloud/ja/public-api/project-appkey)を参照してください。

### レスポンス共通情報

すべてのAPIリクエストに対してHTTP 200 OKレスポンスを返します。APIリクエストの成否はResponse Bodyのheader項目を参照して判断できます。

<details>
  <summary><strong>成功レスポンス</strong></summary>

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
  <summary><strong>失敗レスポンス</strong></summary>

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

| 名前          | タイプ  | 説明                                              |
| ------------- | ------- | ------------------------------------------------- |
| resultCode    | int     | レスポンスコード<br>成功時0、失敗時エラーコード返却 |
| resultMessage | String  | レスポンスメッセージ                               |
| isSuccessful  | boolean | 成否                                              |

### エラーコード

#### 共通

| エラーコード | エラーメッセージ                                                                           | 説明                                |
| ------------ | ------------------------------------------------------------------------------------------ | ----------------------------------- |
| -1           | Unknown error.                                                                             | 不明なエラー                        |
| 4000001      | Invalid parameter.                                                                         | 無効なパラメータ                    |
| 4000002      | Invalid file.                                                                              | 無効なファイル                      |
| 4000003      | Invalid file type.                                                                         | 無効なファイルタイプ                |
| 4000004      | Uploaded file is empty.                                                                    | アップロードされたファイルが空      |
| 4000005      | Required headers is missing.                                                               | 必須ヘッダ不足                      |
| 4000006      | Api call limit exceeded, If you need to adjust the limit, please contact customer service. | API呼び出し上限超過                 |
| 4131000      | Request size is larger than permissible limit. the permissible limit is 5mb.               | リクエストサイズが許容上限(5MB)超過 |

### General OCR API

#### リクエスト

[URI]

| メソッド | URI                            |
| -------- | ------------------------------ |
| POST     | /v1.0/appkeys/{appKey}/general |

#### 画像ファイルを利用したリクエスト

[リクエストヘッダ]

| 名前          | 値                  | 説明                         |
| ------------- | ------------------- | ---------------------------- |
| Authorization | {secretKey}         | コンソールで発行された秘密鍵 |
| Content-Type  | multipart/form-data | コンテンツタイプ             |

[リクエスト本文]

- 画像ファイルのバイナリデータを入れます。

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[フィールド]

| 名前  | タイプ              | 説明       |
| ----- | ------------------- | ---------- |
| image | multipart/form-data | 画像ファイル |

#### 画像URLを利用したリクエスト

[リクエストヘッダ]

| 名前          | 値               | 説明                         |
| ------------- | ---------------- | ---------------------------- |
| Authorization | {secretKey}      | コンソールで発行された秘密鍵 |
| Content-Type  | application/json | コンテンツタイプ             |

[リクエスト本文]

- 画像のURLを入れます。

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[フィールド]

| 名前     | タイプ | 説明    |
| -------- | ------ | ------- |
| imageUrl | String | 画像URL |

- イメージURLにポートを直接指定する場合は80、443、10000～12000ポートのみ使用できます。

#### レスポンス

[レスポンス本文]

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

[ヘッダ]

| 名前          | タイプ  | 説明                                              |
| ------------- | ------- | ------------------------------------------------- |
| isSuccessful  | Boolean | 分析API成否                                       |
| resultCode    | Integer | 結果コード                                        |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前                                    | タイプ | 説明                                              |
| --------------------------------------- | ------ | ------------------------------------------------- |
| fileType                                | String | ファイル拡張子(jpg、png)                          |
| listOfInferTexts                        | List   | 認識結果リスト                                    |
| listOfInferTexts[0].inferTexts[0].value | String | 認識内容                                          |
| listOfInferTexts[0].inferTexts[0].conf  | Double | 認識結果の信頼度                                  |
| listOfBoundingBoxes                     | List   | 認識領域(Bounding box)座標リスト                  |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object | 認識領域の座標{ x1, y1, x2, y2, x3, y3, x4, y4 } |

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### General OCR分割認識API

#### リクエスト

[URI]

| メソッド | URI                                     |
| -------- | --------------------------------------- |
| POST     | /v1.0/appkeys/{appKey}/general/cropping |

#### 画像ファイルを利用したリクエスト

[リクエストヘッダ]

| 名前          | 値                  | 説明                         |
| ------------- | ------------------- | ---------------------------- |
| Authorization | {secretKey}         | コンソールで発行された秘密鍵 |
| Content-Type  | multipart/form-data | コンテンツタイプ             |

[リクエスト本文]

- 画像ファイルのバイナリデータを入れます。

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: multipart/form-data'
```

[フィールド]

| 名前  | タイプ              | 説明       |
| ----- | ------------------- | ---------- |
| image | multipart/form-data | 画像ファイル |

#### 画像URLを利用したリクエスト

[リクエストヘッダ]

| 名前          | 値               | 説明                         |
| ------------- | ---------------- | ---------------------------- |
| Authorization | {secretKey}      | コンソールで発行された秘密鍵 |
| Content-Type  | application/json | コンテンツタイプ             |

[リクエスト本文]

- 画像のURLを入れます。

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/general/cropping' \
-H 'Authorization: ${secretKey}' \
-H 'Content-Type: application/json' \
--data '{ "imageUrl": "https://example.com/example.jpg" }'
```

[フィールド]

| 名前     | タイプ | 説明    |
| -------- | ------ | ------- |
| imageUrl | String | 画像URL |

- イメージURLにポートを直接指定する場合は80、443、10000～12000ポートのみ使用できます。

#### レスポンス

[レスポンス本文]

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

[ヘッダ]

| 名前          | タイプ  | 説明                                              |
| ------------- | ------- | ------------------------------------------------- |
| isSuccessful  | Boolean | 分析API成否                                       |
| resultCode    | Integer | 結果コード                                        |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前                                    | タイプ  | 説明                                                          |
| --------------------------------------- | ------- | ------------------------------------------------------------- |
| fileType                                | String  | ファイル拡張子(jpg、png)                                      |
| listOfInferTexts                        | List    | 認識結果リスト                                                |
| listOfInferTexts[0].inferTexts[0].value | String  | 認識内容                                                      |
| listOfInferTexts[0].inferTexts[0].conf  | Double  | 認識結果の信頼度                                              |
| listOfBoundingBoxes                     | List    | 認識領域(Bounding box)座標リスト                              |
| listOfBoundingBoxes[0].boundingBoxes[0] | Object  | 認識領域の座標{ x1, y1, x2, y2, x3, y3, x4, y4 }             |
| slicesImages                            | Integer | 入力画像のアスペクト比に応じて内部的に分割された画像の数      |

- boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)
