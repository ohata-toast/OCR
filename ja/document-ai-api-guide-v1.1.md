## AI Service > OCR > Document AI > API v1.1 ガイド

## Document AI API 共通情報

### API エンドポイント

| リージョン          | エンドポイント                      |
| ------------------- | ----------------------------------- |
| 韓国(パンギョ)リージョン | https://ocr.api.nhncloudservice.com |

### 認証及び権限

Document AIは、API呼び出し時の認証/認可のためにUser Access Keyトークンを使用します。User Access Keyトークンは、User Access Keyに基づいて発行されるBearerタイプの一時的なアクセストークンです。
User Access Keyトークンの発行及び使用に関する詳細は、[User Access Keyトークン](https://docs.alpha-nhncloud.com/ja/nhncloud/ja/public-api/user-access-key-token)を参照してください。

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
| 4010006      | Invalid token.                                                                             | 無効なトークン                      |
| 4010007      | Permission denied.                                                                         | 権限なし                            |
| 4131000      | Request size is larger than permissible limit. the permissible limit is 5mb.               | リクエストサイズが許容上限(5MB)超過 |

### Document AI 分析 API

[URI]

| メソッド | URI                                |
| -------- | ---------------------------------- |
| POST     | /v1.1/appkeys/{appKey}/document-ai |

[リクエストヘッダ]

| 名前                | 値                  | 説明                    |
| ------------------- | ------------------- | ----------------------- |
| X-NHN-Authorization | {Access Token}      | User Access Keyトークン |
| Content-Type        | multipart/form-data | コンテンツタイプ        |

[リクエスト本文]

```shell
curl -X POST 'https://ocr.api.nhncloudservice.com/v1.1/appkeys/{appKey}/document-ai' \
-H 'X-NHN-Authorization: ${Access Token}' \
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

[フィールド]

| 名前        | タイプ | 説明           |
| ----------- | ------ | -------------- |
| llmResponse | String | LLM分析回答    |
