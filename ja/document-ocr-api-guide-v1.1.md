## AI Service > OCR > Document OCR > API v1.1 ガイド

## Document OCR API 共通情報

### APIエンドポイント

| リージョン      | エンドポイント                       |
| --------------- | ----------------------------------- |
| 韓国(板橋)リージョン | https://ocr.api.nhncloudservice.com |

### 認証及び権限

Document OCRは、API呼び出し時の認証/認可のためにUser Access Keyトークンを使用します。User Access Keyトークンは、User Access Keyに基づいて発行されるBearerタイプの一時的なアクセストークンです。
User Access Keyトークンの発行及び使用に関する詳細は、[User Access Keyトークン](/nhncloud/ja/public-api/user-access-key-token)を参照してください。

### レスポンス共通情報

すべてのAPIリクエストレスポンスとしてHTTP 200 OKを返します。APIリクエストの成否はResponse Bodyのheader項目を参照して判断できます。

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

| 名前          | タイプ  | 説明                                       |
| ------------- | ------- | ------------------------------------------ |
| resultCode    | int     | レスポンスコード<br>成功時0、失敗時エラーコードを返す |
| resultMessage | String  | レスポンスメッセージ                        |
| isSuccessful  | boolean | 成否                                       |

### エラーコード

#### 共通

| エラーコード | エラーメッセージ                                                                           | 説明                          |
| ------------ | ------------------------------------------------------------------------------------------ | ----------------------------- |
| -1           | Unknown error.                                                                             | 不明なエラー                  |
| 4000001      | Invalid parameter.                                                                         | 無効なパラメータ              |
| 4000002      | Invalid file.                                                                              | 無効なファイル                |
| 4000003      | Invalid file type.                                                                         | 無効なファイルタイプ          |
| 4000004      | Uploaded file is empty.                                                                    | アップロードされたファイルが空 |
| 4000005      | Required headers is missing.                                                               | 必須ヘッダの欠落              |
| 4000006      | Api call limit exceeded, If you need to adjust the limit, please contact customer service. | API呼び出し限度超過           |
| 4010006      | Invalid token.                                                                             | 無効なトークン                |
| 4010007      | Permission denied.                                                                         | 権限なし                      |
| 4131000      | Request size is larger than permissible limit. the permissible limit is 5mb.               | リクエストサイズが許容限度(5MB)超過 |

### 事業者登録証分析API

#### リクエスト

[URI]

| メソッド | URI                             |
|------|---------------------------------|
| POST | /v1.1/appkeys/{appKey}/business |

[リクエストヘッダ]

| 名前                | 値             | 説明                  |
|-------------------|--------------|---------------------|
| X-NHN-Authorization | Bearer {Access Token} | 発行されたAccess Token |

[Path Variable]

| 名前     | 値        | 説明                    |
|--------|----------|-----------------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[リクエスト本文]

* 画像ファイルのBinary Dataを入れます。

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/business' \
-F 'image=@sample.png' \
-H 'X-NHN-Authorization: Bearer ${Access Token}'
```

[フィールド]

| 名前    | タイプ                 | 説明     |
|-------|---------------------|--------|
| image | multipart/form–data | 画像ファイル |

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
        "unitType": "pixel",
        "keyValues": [
            {
                "key":"区分",
                "value":"簡易課税者",
                "conf":0.93
            },
            {
                "key":"登録番号",
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

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | 分析API成否                        |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前                 | タイプ    | 説明                                           |
|--------------------|--------|----------------------------------------------|
| fileType           | String | ファイル拡張子(.pdf、.jpg、.png)                      |
| keyValues          | List   | 認識結果リスト                                      |
| keyValues[0].key   | String | 認識項目名                                        |
| keyValues[0].value | String | 認識内容                                         |
| keyValues[0].conf  | Double | 認識結果の信頼度                                     |
| resolution         | String | 推奨解像度(HD 1280*720px)以上の場合はnormal、推奨解像度未満はlow |
| unitType           | String | boxes座標単位(基本pixel、PDFの場合point)               |
| boxes              | List   | 認識領域(Bounding box)座標リスト                      |
| boxes[0]           | Object | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 }     |

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### 事業者登録証 休/廃業照会API

#### リクエスト

[URI]

| メソッド | URI                                    |
|------|----------------------------------------|
| POST | /v1.1/appkeys/{appKey}/business/status |

[リクエストヘッダ]

| 名前                | 値             | 説明                  |
|-------------------|--------------|---------------------|
| X-NHN-Authorization | Bearer {Access Token} | 発行されたAccess Token |

[Path Variable]

| 名前     | 値        | 説明                    |
|--------|----------|-----------------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前             | タイプ    | 説明                  |
|----------------|--------|---------------------|
| businessNumber | String | 事業者登録証の登録番号(10桁の数字) |

[リクエスト本文]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/business/status' \
-H 'X-NHN-Authorization: Bearer ${Access Token}' \
--data-raw '{
  "businessNumber": "1234567890"
}'
```

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
        "statusCode": "00",
        "statusMessage": ""
    }
}
```

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | 休業/廃業照会API成否                   |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前            | タイプ    | 説明                         |
|---------------|--------|----------------------------|
| statusCode    | String | 事業者登録証のステータスコード(国税庁の結果コード) |
| statusMessage | String | 事業者登録証のステータスメッセージ          |

* **"statusCode"別の事業者登録証の状態リスト**

| コード値 | 説明                                   |
|------|--------------------------------------|
| 00   | 事業を行っていない事業者                         |
| 01   | 付加価値税一般課税者                           |
| 02   | 付加価値税簡易課税者                           |
| 03   | 付加価値税免税事業者                           |
| 04   | 収益事業を営んでいない非営利法人または固有番号が付与された団体。国家機関 |
| 05   | 休業者                                  |
| 06   | 廃業者                                  |
| 09   | その他                                  |
