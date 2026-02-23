## AI Service > OCR > Document AI > API v1.0 ガイド

## Document AI API 共通情報

### API エンドポイント

| リージョン          | エンドポイント                      |
| ------------------- | ----------------------------------- |
| 韓国(パンギョ)リージョン | https://ocr.api.nhncloudservice.com |

### 認証及び権限

Document AI APIを使用するには、AppkeyとSecretKeyが必要です。
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
        "llmResponse": ...
    },
    ...
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

| 名前          | タイプ  | 説明                                            |
| ------------- | ------- | ----------------------------------------------- |
| resultCode    | int     | レスポンスコード<br>成功時0、失敗時エラーコード返却 |
| resultMessage | String  | レスポンスメッセージ                             |
| isSuccessful  | boolean | 成否                                            |

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

### Document AI 分析 API

[URI]

| メソッド | URI                                |
| -------- | ---------------------------------- |
| POST     | /v1.0/appkeys/{appKey}/document-ai |

[リクエストヘッダ]

| 名前          | 値                  | 説明                         |
| ------------- | ------------------- | ---------------------------- |
| Authorization | {secretKey}         | コンソールで発行された秘密鍵 |
| Content-Type  | multipart/form-data | コンテンツタイプ             |

[リクエスト本文]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.0/appkeys/{appKey}/document-ai' \
-H 'Authorization: ${secretKey}' \
-F 'image=@sample.png' \
-F 'prompt="簡潔に要約してくれ"' \
-F 'documentTypeCode="GENERAL"'
```

[フィールド]

| 名前             | タイプ | 必須かどうか | デフォルト値 | 有効範囲                                      | 説明                                                                                               |
| ---------------- | ------ | ------------ | ------------ | --------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| image            | file   | O            |              |                                               | イメージファイル                                                                                   |
| documentTypeCode | text   | X            | GENERAL      | GENERAL, BUSINESS_REGISTRATION, BUSINESS_CARD | 文書タイプ<br> 一般: GENERAL <br> 事業者登録証: BUSINESS_REGISTRATION <br> 名刺: BUSINESS_CARD |
| prompt           | text   | O            |              |                                               | 質問内容<br>最大1000文字                                                                           |

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
    "llmResponse": "この文書は、意味のないテキストである'ローレンipsum'を使用して、文字はあるが読みにくい、可読性が落ちる文章を作成したようです。"
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

| 名前        | タイプ | 説明           |
| ----------- | ------ | -------------- |
| llmResponse | String | LLM分析回答    |
