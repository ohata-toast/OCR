## AI Service > OCR > Document OCR > API v2.0 ガイド

## Document OCR API 共通情報

### APIエンドポイント

| リージョン      | エンドポイント                       |
| --------------- | ----------------------------------- |
| 韓国(板橋)リージョン | https://ocr.api.nhncloudservice.com |

### 認証及び権限

Document OCR APIを使用するには、AppkeyとSecretKeyが必要です。
Appkeyは、NHN Cloudの各サービスごとに発行される固有の認証キーであり、APIリクエスト時のサービス識別と有効性検証に使用されます。SecretKeyは、APIへのアクセスを制御するシークレットキーです。
Appkey及びSecretKeyの確認及び使用に関する詳細は、[Appkey](/nhncloud/ja/public-api/appkey)を参照してください。

Appkeyの代わりに、プロジェクト統合Appkeyを使用することも可能です。プロジェクト統合Appkeyは、NHN Cloudの1つのプロジェクト内の複数のサービスに対して共通で使用できる認証キーです。
プロジェクト統合Appkeyの作成及び使用に関する詳細は、[プロジェクト統合Appkey](/nhncloud/ja/public-api/project-appkey)を参照してください。

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
| 4131000      | Request size is larger than permissible limit. the permissible limit is 5mb.               | リクエストサイズが許容限度(5MB)超過 |

### v2.0 API紹介

#### v1.0と違なる点

* セキュリティがデジタルエンベロープ方式に強化されました。

#### 注意事項

* リクエスト、レスポンス時にBase64エンコードされているかどうかを確認してください。
* 暗号化、復号の詳細モード(例：AES-256/CBC/PKCS7Padding)を確認してください。
* 暗号化に使用される対称鍵は、必ず32Byte乱数で作成します。セキュリティのため、リクエストごとに新しい対称鍵を生成して使用することを推奨します。

### 公開鍵の発行

#### リクエスト

[URI]

| メソッド | URI                                              |
|------|--------------------------------------------------|
| GET  | /v2.0/appkeys/{appKey}/public-keys/{serviceName} |

[リクエストヘッダ]

| 名前            | 値           | 説明                  |
|---------------|-------------|---------------------|
| Authorization | {secretKey} | コンソールで発行されたセキュリティキー |

[Path Variable]

| 名前          | 値             | 説明                                                                            |
|-------------|---------------|-------------------------------------------------------------------------------|
| appKey      | {appKey}      | 統合AppkeyまたはサービスAppkey                                                         |
| serviceName | {serviceName} | credit-card(クレジットカードAPI呼び出し時に使用する公開鍵発行時)、<br> id-card(身分証API呼び出し時に使用する公開鍵発行時) |

[リクエスト本文]

```shell
curl -X GET 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/public-keys/{serviceName}' \
-H 'Authorization: ${secretKey}'
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
        "key": "String",
        "version": "0"
     }
}
```

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | API成否                          |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前             | タイプ    | 説明                     |
|----------------|--------|------------------------|
| result         | Object | 暗号化に必要な公開鍵             |
| result.key     | String | 公開鍵(Base64でエンコードされている) |
| result.version | String | 公開鍵のバージョン              |

* 公開鍵は **Base64**でエンコードされている状態です。

### クレジットカードAPI

#### クレジットカード分析API

#### リクエスト

[URI]

| メソッド | URI                                |
|------|------------------------------------|
| POST | /v2.0/appkeys/{appKey}/credit-card |

[リクエストヘッダ]

| 名前            | 値               | 説明                  |
|---------------|-----------------|---------------------|
| Authorization | {secretKey}     | コンソールで発行されたセキュリティキー |
| X-Key-Version | {x-key-version} | 発行された公開鍵のバージョン      |
| Symmetric-Key | {symmetricKey}  | 発行された公開鍵で暗号化された対称鍵  |

* {symmetricKey}は必ず**32byte乱数**で作成する必要があります。
* {symmetricKey}は必ず**RSA/ECB/PKCS1Padding**方式で暗号化されている必要があります(公開鍵利用)。

[Path Variable]

| 名前     | 値        | 説明                    |
|--------|----------|-----------------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前    | タイプ                 | 説明       | 暗号化説明          |
|-------|---------------------|----------|----------------|
| image | multipart/form–data | イメージファイル | 対称鍵で暗号化されたイメージ |

* イメージファイルは必ず **AES-256/CBC/PKCS7Padding**方式で暗号化されている必要があります(対称鍵利用)。
* IV(初期化ベクトル)は、対称鍵の最初の16バイト(すなわち、0～15番目のバイト)を使用します。

[リクエスト本文]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/credit-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
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

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | 分析API成否                        |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前                | タイプ    | 説明                                           | 暗号化するかどうか |
|-------------------|--------|----------------------------------------------|-----------|
| fileType          | String | ファイル拡張子(.jpg, .png)                          |           |
| resolution        | String | 推奨解像度(760\*480px)以上の場合はnormal、推奨解像度未満はlow    |           |
| cardNums          | List   | カード番号認識結果リスト                                 |           |
| cardNums[0].value | String | 認識結果                                         | O         |
| cardNums[0].conf  | Double | 認識結果の信頼度                                     |           |
| totalCardNum      | List   | カード番号全体認識結果                                  | O         |
| cardNumBoxes      | List   | カード番号認識領域(Bounding box)座標リスト                 |           |
| cardNumBoxes[0]   | Object | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 }     |           |
| validThru.value   | String | 有効期限認識内容                                     | O         |
| validThru.conf    | Double | 有効期限認識結果の信頼度                                 |           |
| validThruBox      | Object | 有効期限認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 } |           |

* 暗号化された項目(cardNums[0].value, totalCardNumなど)は **AES-256/CBC/PKCS7Padding**方式で暗号化されています(対称鍵を利用)。

* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### 身分証分析API

#### リクエスト

[URI]

| メソッド | URI                            |
|------|--------------------------------|
| POST | /v2.0/appkeys/{appKey}/id-card |

[リクエストヘッダ]

| 名前            | 値               | 説明                  |
|---------------|-----------------|---------------------|
| Authorization | {secretKey}     | コンソールで発行されたセキュリティキー |
| X-Key-Version | {x-key-version} | 発行された公開鍵のバージョン      |
| Symmetric-Key | {symmetricKey}  | 発行された公開鍵で暗号化された対称鍵  |

* {symmetricKey}は必ず**32byte乱数**で作成する必要があります。
* {symmetricKey}は必ず**RSA/ECB/PKCS1Padding**方式で暗号化される必要があります(公開鍵利用)。

[Path Variable]

| 名前     | 値        | 説明                    |
|--------|----------|-----------------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前    | タイプ                 | 説明     | 暗号化説明        |
|-------|---------------------|--------|--------------|
| image | multipart/form–data | 画像ファイル | 対称鍵で暗号化された画像 |

* 画像ファイルは必ず**AES-256/CBC/PKCS7Padding**方式で暗号化される必要があります(対称鍵利用)。
* IV(初期化ベクトル)は、対称鍵の最初の16バイト(すなわち、0～15番目のバイト)を使用します。

[リクエスト本文]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/id-card' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
```

#### レスポンス

[レスポンスヘッダ]

| 名前          | 説明                              |
|-------------|---------------------------------|
| Request-Key | 身分証真偽確認API呼び出し時に使用するRequest-Key |

* **Request-Keyを真偽確認API呼び出しに使用して正常レスポンスを得た場合、使用されたRequest-Keyは再使用できません。**
* **Request-Keyは発行後1時間有効で、その後は使用できません。**

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

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | 分析API成否                        |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前                 | タイプ    | 説明                                            | 暗号化するかどうか |
|--------------------|--------|-----------------------------------------------|-----------|
| fileType           | String | ファイル拡張子(.jpg, .png)                           |           |
| resolution         | String | 推奨解像度(760\*480px)以上はnormal、推奨解像度未満はlow        |           |
| idType             | String | resident(住民登録証)、driver(運転免許証)、passport(パスポート) |           |
| keyValues          | List   |                                               |           |
| keyValues[0].key   | String |                                               |           |
| keyValues[0].value | String |                                               | O         |
| keyValues[0].bbox  | Object | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 }      |           |
| keyValues[0].conf  | Double | 有効期限認識結果の信頼度                                  |           |   
| boxes              | List   | 認識領域(Bounding box)座標リスト                       |
| boxes[0]           | Object | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 }      |

* **"idType"が"resident"と認識される場合のKeyValuesに含まれるリスト**

| key                | value type | description |
|--------------------|------------|-------------|
| **name**           | string     | 認識された名前     |
| **residentNumber** | string     | 認識された住民登録番号 |
| **issueDate**      | string     | 認識された発行日時   |
| **issuer**         | string     | 認識された発行機関   |

* **"idType"が"driver"と認識される場合のKeyValuesに含まれるリスト**

| key                     | value type | description                                             |
|-------------------------|------------|---------------------------------------------------------|
| **driverLicenseNumber** | string     | 認識された運転免許番号                                             |
| **licenseType**         | string     | 認識された免許の種類(1種普通など)<br>2つ以上の場合は文字列内の"/"で区分               |
| **name**                | string     | 認識された名前                                                 |
| **residentNumber**      | string     | 認識された住民登録番号                                             |
| **condition**           | string     | 認識された免許条件<br>(運転免許証に当該フィールドが存在しない場合、当該フィールドのvalueはnone) |
| **serialNum**           | string     | 認識された暗号一連番号                                             |
| **issueDate**           | string     | 認識された発行日時                                               |
| **issuer**              | string     | 認識された発行機関                                               |



* **"idType"が"passport"で認識される場合、KeyValuesに含まれるリスト**

| key                 | value type | description                |
|---------------------|------------|----------------------------|
| **passportType**    | string     | 認識されたパスポート番号               |
| **countryCode**     | string     | 認識された国コード                  |
| **passportNo**      | string     | 認識されたパスポート番号               |
| **surName**         | string     | 認識された姓                     |
| **givenName**       | string     | 認識された名前                    |
| **nationality**     | string     | 認識された国籍                    |
| **dateOfBirth**     | string     | 認識された生年月日                  |
| **dateOfBirthYMD**  | string     | 認識された生年月日<br>(YYYYMMDD 8桁) |
| **sex**             | string     | 認識された性別                    |
| **dateOfIssue**     | string     | 認識された発行日                   |
| **dateOfIssueYMD**  | string     | 認識された発行日<br>(YYYYMMDD 8桁)  |
| **dateOfExpiry**    | string     | 認識された有効期限                  |
| **dateOfExpiryYMD** | string     | 認識された有効期限<br>(YYYYMMDD 8桁) |
| **koreanName**      | string     | 認識されたハングル姓名                |
| **personalNo**      | string     | 認識された住民登録番号                |
| **MRZ1**            | string     | 機械判読領域1                    |
| **MRZ2**            | string     | 機械判読領域2                    |

* 暗号化された項目(keyValues[0].valueなど)は**AES-256/CBC/PKCS7Padding**方式で暗号化されています(対称鍵利用)。
* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)

### 身分証真偽確認API

#### リクエスト

[URI]

| メソッド | URI                                         |
|------|---------------------------------------------|
| POST | /v2.0/appkeys/{appKey}/id-card/authenticity |

[リクエストヘッダ]

| 名前            | 値               | 説明                      |
|---------------|-----------------|-------------------------|
| Authorization | {secretKey}     | コンソールで発行されたセキュリティキー     |
| X-Key-Version | {x-key-version} | 発行された公開鍵のバージョン          |
| Symmetric-Key | {symmetricKey}  | 発行された公開鍵で暗号化された対称鍵      |
| Request-Key   | {Request-Key}   | 身分証分析後に発行されたRequest-Key |

* {symmetricKey}は必ず**32byteの乱数**で作成する必要があります。
* {symmetricKey}は必ず**RSA/ECB/PKCS1Padding**方式で暗号化される必要があります(公開鍵利用)。

[Path Variable]

| 名前     | 値        | 説明                    |
|--------|----------|-----------------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前                  | タイプ    | 説明                                                                                              | idType             | 暗号化有無 | 必須 |
|---------------------|--------|-------------------------------------------------------------------------------------------------|--------------------|-------|----|
| idType              | String | resident(住民登録証), driver(運転免許証), passport(パスポート)                                                 |                    | X     | O  |
| name                | String | 名前                                                                                              |                    | O     | O  |
| residentNumber      | String | 住民登録番号<br>- resident(住民登録証)の場合、住民登録番号数字13桁<br>- driver(運転免許証)の場合、住民登録番号の前6桁と後ろの最初の1桁を組み合わせた数字7桁 | resident, driver   | O     | O  |
| issueDate           | String | 発行日時(YYYYMMDD)                                                                                  | resident, passport | O     | O  |
| driverLicenseNumber | String | 12桁の運転免許番号                                                                                      | driver             | O     | O  |
| serialNum           | String | 5～6桁の暗号一連番号                                                                                     | driver             | O     | X  |
| passportNumber      | String | パスポート番号(9桁英字大文字、数字の組み合わせ)                                                                       | passport           | O     | O  |
| birthDate           | String | 生年月日(YYYYMMDD)                                                                                  | passport           | O     | O  |
| expirationDate      | String | 有効期限(YYYYMMDD)                                                                                  | passport           | X     | O  |

* 暗号化が必要なフィールドは必ず**AES-256/CBC/PKCS7Padding**方式で暗号化される必要があります(対称鍵利用)。
* IV(初期化ベクトル)は、対称鍵の最初の16バイト(すなわち、0～15番目のバイト)を使用します。

[リクエスト本文]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/id-card/authenticity' \
-H 'Authorization: ${secretKey}' \
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
        "isAuthenticity": false
    }
}
```

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | 真偽確認API成否                      |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前             | タイプ     | 説明 |
|----------------|---------|----|
| isAuthenticity | Boolean | 真偽 |

### 身分証分析(単独) API

#### 既存身分証分析APIとの違い

* 真偽確認に必要なRequest-Keyを含みません。
* 真偽確認ができない代わりに、低料金が課金されます。

#### リクエスト

[URI]

| メソッド | URI                                        |
|------|--------------------------------------------|
| POST | /v2.0/appkeys/{appKey}/id-card/stand-alone |

[リクエストヘッダ]

| 名前            | 値               | 説明                  |
|---------------|-----------------|---------------------|
| Authorization | {secretKey}     | コンソールで発行されたセキュリティキー |
| X-Key-Version | {x-key-version} | 発行された公開鍵のバージョン      |
| Symmetric-Key | {symmetricKey}  | 発行された公開鍵で暗号化された対称鍵  |

* {symmetricKey}は必ず**32byte乱数**で作成する必要があります。
* {symmetricKey}は必ず**RSA/ECB/PKCS1Padding**方式で暗号化する必要があります(公開鍵利用)。

[Path Variable]

| 名前     | 値        | 説明                    |
|--------|----------|-----------------------|
| appKey | {appKey} | 統合AppkeyまたはサービスAppkey |

[フィールド]

| 名前    | タイプ                 | 説明       | 暗号化説明          |
|-------|---------------------|----------|----------------|
| image | multipart/form–data | イメージファイル | 対称鍵で暗号化されたイメージ |

* イメージファイルは必ず**AES-256/CBC/PKCS7Padding**方式で暗号化する必要があります(対称鍵利用)。
* IV(初期化ベクトル)は、対称鍵の最初の16バイト(すなわち、0～15番目のバイト)を使用します。

[リクエスト本文]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v2.0/appkeys/{appKey}/id-card/stand-alone' \
-F 'image=@sample.png' \
-H 'Authorization: ${secretKey}' \
-H 'X-Key-Version: ${x-key-version}' \
-H 'Symmetric-Key: ${symmetricKey}'
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

[ヘッダ]

| 名前            | タイプ     | 説明                             |
|---------------|---------|--------------------------------|
| isSuccessful  | Boolean | 分析API成否                        |
| resultCode    | Integer | 結果コード                          |
| resultMessage | String  | 結果メッセージ(成功時はsuccess、失敗時はエラー内容) |

[フィールド]

| 名前                 | タイプ    | 説明                                            | 暗号化するかどうか |
|--------------------|--------|-----------------------------------------------|-----------|
| fileType           | String | ファイル拡張子(.jpg, .png)                           |           |
| resolution         | String | 推奨解像度(760\*480px)以上の場合はnormal、推奨解像度未満はlow     |           |
| idType             | String | resident(住民登録証)、driver(運転免許証)、passport(パスポート) |           |
| keyValues          | List   |                                               |           |
| keyValues[0].key   | String |                                               |           |
| keyValues[0].value | String |                                               | O         |
| keyValues[0].bbox  | Object | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 }      |           |
| keyValues[0].conf  | Double | 有効期限認識結果の信頼度                                  |           |   
| boxes              | List   | 認識領域(Bounding box)座標リスト                       |
| boxes[0]           | Object | 認識領域座標{ x1, y1, x2, y2, x3, y3, x4, y4 }      |

* **"idType"が"resident"と認識される場合、KeyValuesに含まれるリスト**

| key                | value type | description |
|--------------------|------------|-------------|
| **name**           | string     | 認識された名前     |
| **residentNumber** | string     | 認識された住民登録番号 |
| **issueDate**      | string     | 認識された発行日時   |
| **issuer**         | string     | 認識された発行機関   |

* **"idType"が"driver"と認識される場合、KeyValuesに含まれるリスト**

| key                     | value type | description                                                |
|-------------------------|------------|------------------------------------------------------------|
| **driverLicenseNumber** | string     | 認識された運転免許番号                                                |
| **licenseType**         | string     | 認識された免許種類(1種普通など)<br>2つ以上の場合、文字列内"/"で区切る                   |
| **name**                | string     | 認識された名前                                                    |
| **residentNumber**      | string     | 認識された住民登録番号                                                |
| **condition**           | string     | 認識された免許条件<br>(運転免許証によって該当フィールドが存在しない場合、該当フィールドのvalueはnone) |
| **serialNum**           | string     | 認識された暗号一連番号                                                |
| **issueDate**           | string     | 認識された発行日時                                                  |
| **issuer**              | string     | 認識された発行機関                                                  |

* **"idType"が"passport"と認識される場合、KeyValuesに含まれるリスト**

| key                 | value type | description                |
|---------------------|------------|----------------------------|
| **passportType**    | string     | 認識されたパスポート番号               |
| **countryCode**     | string     | 認識された国コード                  |
| **passportNo**      | string     | 認識されたパスポート番号               |
| **surName**         | string     | 認識された姓                     |
| **givenName**       | string     | 認識された名前                    |
| **nationality**     | string     | 認識された国籍                    |
| **dateOfBirth**     | string     | 認識された生年月日                  |
| **dateOfBirthYMD**  | string     | 認識された生年月日<br>(YYYYMMDD 8桁) |
| **sex**             | string     | 認識された性別                    |
| **dateOfIssue**     | string     | 認識された発行日                   |
| **dateOfIssueYMD**  | string     | 認識された発行日<br>(YYYYMMDD 8桁)  |
| **dateOfExpiry**    | string     | 認識された有効期限                  |
| **dateOfExpiryYMD** | string     | 認識された有効期限<br>(YYYYMMDD 8桁) |
| **koreanName**      | string     | 認識されたハングル姓名                |
| **personalNo**      | string     | 認識された住民登録番号                |
| **MRZ1**            | string     | 機械判読領域1                    |
| **MRZ2**            | string     | 機械判読領域2                    |

* 暗号化された項目(keyValues[0].valueなど)は**AES-256/CBC/PKCS7Padding**方式で暗号化されています(対称鍵利用)。
* boxes[0]
  ![Bounding box](http://static.toastoven.net/prod_ocr/bbox.png)
