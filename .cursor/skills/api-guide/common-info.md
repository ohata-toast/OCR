# 공통 정보 섹션 작성 가이드

## API 엔드포인트

```markdown
| 리전            | 엔드포인트                          |
| --------------- | ----------------------------------- |
| 한국(판교) 리전 | https://ocr.api.nhncloudservice.com |
```

## 인증 및 권한

### v1.0 / v2.0 (Appkey + SecretKey)

`{서비스명}`을 실제 서비스명(Document AI, General OCR, Document OCR)으로 치환합니다.
`{link}`의 도메인은 브랜치에 따라 다릅니다. (alpha: `docs.alpha-nhncloud.com`, beta: `docs.beta-nhncloud.com`, release: `docs.nhncloud.com`)
모든 링크는 `https://`를 포함한 절대경로를 사용합니다.

**KO:**
```
{서비스명} API를 사용하려면 Appkey와 SecretKey가 필요합니다.
Appkey는 NHN Cloud의 각 서비스별로 발급되는 고유 인증 키로 API 요청 시 서비스 식별과 유효성 검증에 사용됩니다. SecretKey는 API에 대한 접근을 제어하는 비밀 키입니다.
Appkey 및 SecretKey 확인 및 사용에 대한 자세한 내용은 [Appkey](https://{domain}/ko/nhncloud/ko/public-api/appkey)를 참고하세요.

Appkey 대신 프로젝트 통합 Appkey를 사용할 수도 있습니다. 프로젝트 통합 Appkey는 NHN Cloud에서 하나의 프로젝트 내 여러 서비스에 대해 공통으로 사용할 수 있는 인증 키입니다.
프로젝트 통합 Appkey 생성 및 사용에 대한 자세한 내용은 [프로젝트 통합 Appkey](https://{domain}/ko/nhncloud/ko/public-api/project-appkey)를 참고하세요.
```

**EN:**
```
AppKey and SecretKey are required to use the {서비스명} API.
An Appkey is a unique authentication key issued for each NHN Cloud service, used to identify the service and validate API requests. A SecretKey is a private key used to control access to the API.
For more information on checking and using Appkeys and SecretKeys, please refer to the [Appkey](https://{domain}/en/nhncloud/en/public-api/appkey).

Project Integrated Appkey can be used in place of the Appkey. Project Integrated Appkey is a common authentication key that can be shared across multiple services within a single NHN Cloud project.
For more information on creating and using Project Integrated Appkeys, please refer to the [Project Integrated Appkey](https://{domain}/en/nhncloud/en/public-api/project-appkey).
```

**JA:**
```
{서비스명} APIを使用するには、AppkeyとSecretKeyが必要です。
Appkeyは、NHN Cloudの各サービスごとに発行される固有の認証キーであり、APIリクエスト時のサービス識別と有効性検証に使用されます。SecretKeyは、APIへのアクセスを制御するシークレットキーです。
Appkey及びSecretKeyの確認及び使用に関する詳細は、[Appkey](https://{domain}/ja/nhncloud/ja/public-api/appkey)を参照してください。

Appkeyの代わりに、プロジェクト統合Appkeyを使用することも可能です。プロジェクト統合Appkeyは、NHN Cloudの1つのプロジェクト内の複数のサービスに対して共通で使用できる認証キーです。
プロジェクト統合Appkeyの作成及び使用に関する詳細は、[プロジェクト統合Appkey](https://{domain}/ja/nhncloud/ja/public-api/project-appkey)を参照してください。
```

### v1.1 / v2.1 (User Access Key Token)

**KO:**
```
{서비스명}은(는) API 호출 시 인증/인가를 위해 User Access Key 토큰을 사용합니다. User Access Key 토큰은 User Access Key를 기반으로 발급되는 Bearer 타입의 일시적 액세스 토큰입니다.
User Access Key 토큰 발급 및 사용에 대한 자세한 내용은 [User Access Key 토큰](https://{domain}/ko/nhncloud/ko/public-api/user-access-key-token)을 참고하세요.
```

**EN:**
```
{서비스명} uses User Access Key tokens for authentication and authorization when making API calls. The User Access Key token is a temporary, Bearer-type access token issued from a User Access Key.
For more information on issuing and using User Access Key tokens, please refer to the [User Access Key Token](https://{domain}/en/nhncloud/en/public-api/user-access-key-token).
```

**JA:**
```
{서비스명}は、API呼び出し時の認証/認可のためにUser Access Keyトークンを使用します。User Access Keyトークンは、User Access Keyに基づいて発行されるBearerタイプの一時的なアクセストークンです。
User Access Keyトークンの発行及び使用に関する詳細は、[User Access Keyトークン](https://{domain}/ja/nhncloud/ja/public-api/user-access-key-token)を参照してください。
```

## 응답 공통 정보

`<details>` 태그로 성공/실패 응답 예시를 감싸서 표시합니다.

```markdown
<details>
  <summary><strong>성공 응답</strong></summary>

HTTP/1.1 200 OK ...

</details>

<details>
  <summary><strong>실패 응답</strong></summary>

{ "header": { "isSuccessful": false, ... } }

</details>
```

언어별 라벨:
- KO: 성공 응답 / 실패 응답
- EN: Success Response / Failure Response
- JA: 成功レスポンス / 失敗レスポンス

## 오류 코드

### 공통 오류 코드 (모든 버전)

| 오류 코드 | 오류 메시지 |
| --------- | ----------- |
| -1 | Unknown error. |
| 4000001 | Invalid parameter. |
| 4000002 | Invalid file. |
| 4000003 | Invalid file type. |
| 4000004 | Uploaded file is empty. |
| 4000005 | Required headers is missing. |
| 4000006 | Api call limit exceeded... |
| 4131000 | Request size is larger than permissible limit... |

### 토큰 인증 추가 오류 코드 (v1.1 / v2.1 전용)

| 오류 코드 | 오류 메시지 |
| --------- | ----------- |
| 4010006 | Invalid token. |
| 4010007 | Permission denied. |

## 요청 헤더 패턴

### v1.0 / v2.0

```markdown
| 이름          | 값          | 설명                       |
| ------------- | ----------- | -------------------------- |
| Authorization | {secretKey} | 콘솔에서 발급 받은 보안 키 |
```

### v1.1 / v2.1

```markdown
| 이름                | 값             | 설명                  |
| ------------------- | -------------- | --------------------- |
| X-NHN-Authorization | {Access Token} | User Access Key 토큰 |
```

## 브랜치별 도메인 매핑

| 브랜치 | 도메인 |
|--------|--------|
| alpha | `docs.alpha-nhncloud.com` |
| beta | `docs.beta-nhncloud.com` |
| release / master | `docs.nhncloud.com` |

beta 브랜치 작업 시 alpha의 변경사항을 merge한 뒤 도메인만 일괄 변경합니다.
