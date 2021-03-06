<properties
	pageTitle="OAuth 2.0 でのエラー処理 | Microsoft Azure"
	description="この記事では、Azure Active Directory に実装される OAuth 2.0 でのエラーの一般的なクラス、およびそれらを処理するためのベスト プラクティスについて説明します。"
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="priyamo"/>

# OAuth 2.0 でのエラー処理

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

この記事では、Azure Active Directory (Azure AD) を使用してアプリケーションを承認するときに発生する可能性のあるエラーの一般的なクラスを処理するためのベスト プラクティスを学習します。承認エンドポイントと、トークン発行エンドポイントの詳細については、[Azure AD でのアプリケーションの認証フローのページ](active-directory-protocols-oauth-code.md)を参照してください。

## 承認エンドポイント エラー

承認プロセスの最初の手順で、クライアント アプリケーションは Azure AD の `/authorize` エンドポイントにユーザーからの必要なアクセス許可の一覧を含む要求を送信します。

承認エンドポイント エラーは `/authorize` エンドポイントで生成されたものです。これらは、Web ページの HTTP 200 エラーとして、またはクライアント アプリケーションでエラー処理が使用可能なときには HTTP 302 リダイレクト コードを使用して返されます。

要求に必要な `response_type` パラメーターが欠けている場合の、Azure AD 承認エンドポイントからの HTTP 302 エラー応答の例を次に示します。

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?error=invalid_request&error_description=AADSTS90014%3a+The+request+body+must+contain+the+following+parameter%3a+%27response_type%27.%0d%0aTrace+ID%3a+57f5cb47-2278-4802-a018-d05d9145daad%0d%0aCorrelation+ID%3a+570a9ed3-bf1d-40d1-81ae-63465cc25488%0d%0aTimestamp%3a+2013-12-31+05%3a51%3a35Z&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| パラメーター | 説明 |
|-----------|-------------|
| error | 「[OAuth 2.0 Authorization Framework (OAuth 2.0 承認フレームワーク)](http://tools.ietf.org/html/rfc6749)」のセクション 5.2 で定義されているエラー コード値。次の表で、Azure AD が返すエラー コードについて説明します。 |
| error\_description | エラーの詳しい説明。このメッセージはエンドユーザー向けではありません。 |
| state | state 値は、ランダムに生成された再利用されない値で、クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐために、要求で送信され、応答で返されます。 |

### 承認エンドポイント エラーのエラー コード

次の表で、エラー応答の `error` パラメーターで返される可能性のあるさまざまなエラー コードを説明します。

| エラー コード | 説明 | クライアント側の処理 |
|------------|-------------|---------------|
| invalid\_request | 必要なパラメーターが不足しているなどのプロトコル エラーです。 | 要求を修正し再送信します。これは、開発エラーであり、通常は初期テスト中に発生します。|
| unauthorized\_client | クライアント アプリケーションは、認証コードの要求を許可されていません。 | これは通常、クライアント アプリケーションが Azure AD に登録されていない、またはユーザーの Azure AD テナントに追加されていないときに発生します。アプリケーションでは、アプリケーションのインストールと Azure AD への追加を求める指示をユーザーに表示できます。 |
| access\_denied | リソースの所有者が同意を拒否しました。 | クライアント アプリケーションは、ユーザーが同意しないと続行できないことを、ユーザーに通知できます。 |
| unsupported\_response\_type | 承認サーバーは要求に含まれる応答の種類をサポートしていません。 | 要求を修正し再送信します。これは、開発エラーであり、通常は初期テスト中に発生します。|
|server\_error | サーバーで予期しないエラーが発生しました。 | 要求をやり直してください。これらのエラーは一時的な状況によって発生します。クライアント アプリケーションは、一時的なエラーのため応答が遅れることをユーザーに説明できます。 |
| temporarily\_unavailable | サーバーが一時的にビジー状態であるため、要求を処理できません。 | 要求をやり直してください。クライアント アプリケーションは、一時的な状況が原因で応答が遅れることをユーザーに説明できます。 |
| invalid\_resource |対象のリソースは、存在しない、Azure AD が見つけられない、または正しく構成されていないために無効です。| これは、リソース (存在する場合) がテナントで構成されていないことを示します。アプリケーションでは、アプリケーションのインストールと Azure AD への追加を求める指示をユーザーに表示できます。 |

## トークン発行エンドポイント エラー

アプリケーションは、`/authorize` エンドポイントから認証コードを受け取ったら、`/token` エンドポイントに渡して、指定したリソースのアクセス トークンを受け取ります。

トークン発行エンドポイント エラーは `/token` エンドポイントで生成されたものです。クライアントがトークン発行エンドポイントを直接呼び出すため、これらは HTTP エラー コードです。HTTP 状態コードだけでなく、Azure AD トークン発行エンドポイントも、エラーを説明するオブジェクトを含む JSON ドキュメントを返します。

たとえば、要求内の `client_id` パラメーターが無効な場合のエラーは、次のように見えます。

```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8

{"error":"invalid_request","error_description":"AADSTS90011: Request is ambiguous, multiple application identifiers found. Application identifiers: '197451ec-ade4-40e4-b403-02105abd9049, 597451ec-ade4-40e4-b403-02105abd9049'.\r\nTrace ID: 4457d068-2a03-42b2-97f2-d55325289d86\r\nCorrelation ID: 6b3474d8-233e-463f-b0a3-86433d8ba889\r\nTimestamp: 2013-12-31 06:31:41Z","error_codes":[90011],"timestamp":"2013-12-31 06:31:41Z","trace_id":"4457d068-2a03-42b2-97f2-d55325289d86","correlation_id":"6b3474d8-233e-463f-b0a3-86433d8ba889"}
```
### HTTP 状態コード

次の表に、トークン発行エンドポイントが返す HTTP 状態コードを示します。場合によっては、エラー コードで十分に応答が説明されることがありますが、エラーの場合は、付属の JSON ドキュメントを解析し、そのエラー コードを確認する必要があります。

| HTTP コード | 説明 |
|-----------|-------------|
| 400 | 既定の HTTP コード。ほとんどの場合に使用され、通常は、形式が正しくない要求が原因です。要求を修正し再送信します。 |
| 401 | 認証に失敗しました。たとえば、要求に client\_secret パラメーターがありません。|
| 403 | 承認に失敗しました。たとえば、ユーザーにリソースにアクセスするためのアクセス許可がありません。 |
| 500 | サービスで内部エラーが発生しました。要求をやり直してください。 |

### エラー応答に含まれる JSON ドキュメント オブジェクト

| JSON オブジェクト | 説明 |
|-------------|-------------|
| error | 「[OAuth 2.0 Authorization Framework (OAuth 2.0 承認フレームワーク)](http://tools.ietf.org/html/rfc6749)」のセクション 5.2 で定義されているエラー コード値。次の表で、Azure AD が返すエラー コードについて説明します。 |
| error\_description | エラーの詳しい説明。このメッセージはエンドユーザー向けではありません。 |
| timestamp | 世界協定時刻 (UTC) 形式のエラーが発生した日付と時刻。 |
| trace\_id | エラーのトレースを識別する ID。 |
| correlation\_id | 	クライアントによって生成された値。トークン発行エンドポイントに対する要求の `client-request-id` ヘッダーにこの値が含まれている場合は、この ID が JSON エラー ドキュメントに含まれます。この ID は、同じセッションの他の呼び出しで使用できます。 |
| error\_codes | サービスのさまざまな状態にマップされているエラー コードの一覧が含まれています。アプリケーション ロジックの中では、これらのコードを使用しません。ただし、問題を診断し、エラー コードによって情報をオンライン検索するために使用できます。 |

### JSON ドキュメント エラー コード値

| エラー コード | 説明 | クライアント側の処理 |
|------------|-------------|---------------|
| invalid\_request | 必要なパラメーターが不足しているなどのプロトコル エラーです。 | 要求を修正し再送信します。 |
| invalid\_grant | 認証付与 (またはそのパラメーター) または更新トークンが、無効である、期限が切れている、または取り消されました。 | 要求を修正し再送信します。 |
| unauthorized\_client | 認証されたクライアントは、この承認付与の種類を使用する権限がありません。 | これは通常、クライアント アプリケーションが Azure AD に登録されていない、またはユーザーの Azure AD テナントに追加されていないときに発生します。アプリケーションでは、アプリケーションのインストールと Azure AD への追加を求める指示をユーザーに表示できます。 |
| invalid\_client | クライアント認証に失敗しました。 | クライアント資格情報が有効ではありません。修正するには、アプリケーション管理者が資格情報を更新します。 |
| unsupported\_grant\_type | 承認サーバーが承認付与の種類をサポートしていません。 | 要求の付与の種類を変更します。この種のエラーは、開発時にのみ発生し、初期テスト中に検出する必要があります。 |
| invalid\_resource | 対象のリソースは、存在しない、Azure AD が見つけられない、または正しく構成されていないために無効です。 | これは、リソース (存在する場合) がテナントで構成されていないことを示します。アプリケーションでは、アプリケーションのインストールと Azure AD への追加を求める指示をユーザーに表示できます。 |
| interaction\_required | 要求にユーザーの介入が必要です。たとえば、追加の認証手順が必要です。 | 同じリソースで要求を再試行します。 |
| temporarily\_unavailable | サーバーが一時的にビジー状態であるため、要求を処理できません。 | 要求をやり直してください。クライアント アプリケーションは、一時的な状況が原因で応答が遅れることをユーザーに説明できます。|

## セキュリティ保護されたリソースのエラー

これらは、OAuth 2.0 の承認フレームワークの [RFC 6750](http://tools.ietf.org/html/rfc6750) 仕様を実装している場合に、Web API などのセキュリティで保護されたリソースが返す可能性のあるエラーです。

RFC 6750 を実装するセキュリティで保護されたリソースは、HTTP 状態コードを発行します。要求に認証資格情報が含まれていない、または要求にトークンがない場合は、応答に `WWW-Authenticate` ヘッダーが含まれます。要求が失敗したときに、リソース サーバーは HTTP 状態コードとエラー コードを含む応答を返します。

次に、クライアント要求にベアラー トークンが含まれていないときの失敗応答の一例を示します。

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

## エラーのパラメーター

| パラメーター | 説明 |
|-----------|-------------|
| authorization\_uri | 承認サーバーの URI (物理エンドポイント)。この値は、探索エンドポイントからサーバーの詳細を取得するための、ルックアップ キーとしても使用します。<p><p> クライアントは、承認サーバーが信頼されていることを検証する必要があります。リソースが Azure AD によって保護されている場合は、URL が https://login.windows.net または Azure AD がサポートする別のホスト名で始まることを確認するだけで十分です。テナント固有のリソースは、テナント固有の承認 URI を常に返すはずです。 |
| error | 「[OAuth 2.0 Authorization Framework (OAuth 2.0 承認フレームワーク)](http://tools.ietf.org/html/rfc6749)」のセクション 5.2 で定義されているエラー コード値。|
| error\_description | エラーの詳しい説明。このメッセージはエンドユーザー向けではありません。|
| resource\_id | リソースの一意の識別子を返します。クライアント アプリケーションは、リソースのトークンを要求するときに、この識別子を `resource` パラメーターの値として使用できます。<p><p> クライアント アプリケーションにとってこの値を確認することは非常に重要です。確認しないと、悪意のあるサービスが**権限昇格**攻撃を仕掛ける可能性があります。<p><p> 攻撃を防止するための推奨方法として、`resource_id` と、アクセスしている Web API URL のベースが一致していることを確認します。たとえば、https://service.contoso.com/data にアクセスしている場合は、`resource_id` は htttps://service.contoso.com/ になります。クライアント アプリケーションは、ID を検証する信頼性の高い代替方法がない限り、ベース URL ではじまらない `resource_id` を拒否する必要があります。 |

## ベアラー スキームのエラー コード

RFC 6750 仕様では、応答で WWW-Authenticate ヘッダーとベアラー スキームを使用するリソースのために、次のエラーが定義されています。

| HTTP 状態コード | エラー コード | 説明 | クライアント側の処理 |
|------------------|------------|-------------|---------------|
| 400 | invalid\_request | 要求の形式が正しくありません。たとえば、パラメーターがない、または同じパラメーターを 2 回使用している可能性があります。 | エラーを修正して、要求を再試行してください。この種のエラーは、開発時にのみ発生し、初期テスト中に検出する必要があります。 |
| 401 | invalid\_token | アクセス トークンがない、無効である、または取り消されました。error\_description パラメーターの値で追加の詳細情報が提供されます。 | 承認サーバーから新しいトークンを要求します。新しいトークンが失敗すると、予期しないエラーが発生しました。ユーザーにエラー メッセージを送信し、ランダムな遅延後に再試行します。 |
| 403 | insufficient\_scope | アクセス トークンに、リソースにアクセスするために必要な偽装アクセス許可が含まれていません。 | 新しい承認要求を承認エンドポイントに送信します。応答にスコープのパラメーターが含まれている場合は、リソースへの要求でそのスコープ値を使用します。 |
| 403 | insufficient\_access | トークンのサブジェクトに、リソースにアクセスするために必要なアクセス許可がありません。 | ユーザーに別のアカウントの使用か、指定のリソースへのアクセス許可の要求を求めるメッセージを表示します。 |

<!---HONumber=AcomDC_0608_2016-->