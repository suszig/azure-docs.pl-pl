<properties
    pageTitle="アプリ モデル v2.0 | Microsoft Azure"
    description="一般公開の Azure AD と v2.0 アプリ モデル パブリック プレユーの違いを比較しています。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/09/2015"
    ms.author="dastrock"/>

# アプリ モデル v2.0 プレビュー: 相違点

一般公開されている Azure AD サービスを使い慣れている場合、または以前にアプリを Azure AD に統合した場合は、v2.0 アプリ モデルで予想外の違いを体験する可能性があります。  このドキュメントでは、それらの違いについて説明します。

> [AZURE.NOTE]
    この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。  一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。


## Microsoft アカウントと Azure AD アカウント
v2.0 アプリ モデルを使用すると、開発者は、1 つのエンドポイントを使用して Microsoft アカウントと Azure AD アカウントの両方からのサインインを受け付けるアプリを作成できます。  これにより、まったくアカウントに依存しないアプリを作成できます。ユーザーがサインインにどのアカウントを使用するかを考慮する必要がありません。  もちろん、する *できます* アプリを特定のセッションで使用されているアカウントの種類を認識させるが、する必要はありません。

たとえば、アプリを呼び出す場合、 [Office 365 REST Api](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2), 、いくつかのデータと追加の機能は、SharePoint サイトまたはディレクトリのデータなど、エンタープライズ ユーザーに表示されます。  多数のアクションなど [ユーザーのメールを読む](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2), 、両方の Microsoft アカウントと Azure AD のアカウントの同じコードを記述することができます。  

Microsoft アカウントと Azure AD アカウントのアプリを 1 つの単純なプロセスで統合できるようになりました。  1 組のエンドポイント、1 つのライブラリ、1 回のアプリ登録によって、コンシューマー向けおよびエンタープライズ向けの両方の環境にアクセスできます。  V2.0 アプリ モデルのプレビューの詳細については、チェック アウト [概要](active-directory-appmodel-v2-overview.md)します。


## 新しいアプリ登録ポータル
V2.0 アプリ モデルでは、Microsoft のアプリケーションの新しい場所に登録する必要があります: [apps.dev.microsoft.com](https://apps.dev.microsoft.com)します。  このポータルでは、アプリケーション ID の取得、アプリのサインイン ページの外観のカスタマイズなどを行うことができます。  主にプレビュー期間中にお寄せいただくフィードバックに基づいて、このアプリ登録ポータルに多くの機能を継続的に追加していく予定です。  このポータルは、Microsoft アプリに関連したあらゆる情報を一元管理できる新たな場所になります。

最大のメリットは、使用に Azure または Office のサブスクリプションが不要なことです。  必要なものは、個人の Microsoft アカウントまたは職場や学校のアカウントだけです。

今日では、新しいアプリの登録ポータルに登録されているアプリはのみで動作するアプリケーション モデルのバージョン 2.0 に注意してくださいと *のみ* 新しいアプリの登録ポータルに登録されているアプリはアプリケーション モデルのバージョン 2.0 では機能します。  一般公開されている Microsoft アカウントまたは Azure AD サービスで、これらのアプリのいずれかを使用しようとしたとき、または v2.0 アプリ モデルで既存のアプリを使用しようとしたときに、非互換性の問題が発生する可能性があります。


## 1 つのアプリ ID ですべての製品に対応
GA の Azure AD サービスでは、1 つのプロジェクトに対して異なる複数のアプリを登録している場合があります。  その場合、ネイティブ クライアントと Web アプリで別々にアプリ登録する必要がありました。

![古いアプリケーションの登録 UI](../media/active-directory-v2-flows/old_app_registration.PNG)

たとえば、Web サイトと iOS アプリの両方を構築した場合は、2 つの異なるアプリケーション ID を使用して、それぞれを別々に登録する必要がありました。  Web サイトとバックエンド Web API を使用していた場合、Azure AD に別々のアプリとして登録していました。  また、iOS アプリと Android アプリを使用していた場合は、2 つの異なるアプリを登録していました。  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

v2.0 では、各プロジェクトに対して 1 つのアプリケーション ID を使用して 1 回のアプリ登録を行うだけでかまいません。  それぞれのプロジェクトに複数の "プラットフォーム" を追加し、追加した各プラットフォームに対して適切なデータを提供できます。  もちろんアプリは要件に応じて必要な数だけ作成できますが、大半の場合、必要なアプリケーション ID は 1 つだけです。

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

マイクロソフトが目指しているのは、アプリの管理と開発のエクスペリエンスをいっそう簡素化し、より高度に統合されたビューで作業中の各プロジェクトの状況を把握できるようにすることです。


## リソースではなくスコープ
として動作できるアプリケーション、Azure AD の一般公開サービスで、 **リソース**, 、またはトークンの受信者。  リソースの数を定義できます **スコープ** または **oAuth2Permissions** に理解している一連のスコープのリソースへのトークンを要求するアプリケーションをクライアントに許可します。  リソースの例として、Azure AD Graph API があります。

- リソース識別子、または `AppID URI`:`https://graph.windows.net/`
- スコープ、または `OAuth2Permissions`:`Directory.Read`、`Directory.Write` などです。  

このすべてが v2.0 アプリ モデルに当てはまります。  アプリはリソースとして動作し、スコープを定義することができ、URI によって識別できます。  また、クライアント アプリは、スコープへのアクセスを要求できます。  ただし、クライアントがアクセス許可を要求する方法が変更されました。  以前は、Azure AD への OAuth 2.0 承認要求は、次のようになっていました。

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

ここで、 **リソース** のどのリソースに、クライアント アプリケーションが要求の承認にパラメーターが示されます。  Azure AD は、Azure ポータルでの静的な構成に基づいて、アプリによって要求されたアクセス許可を計算し、適切なトークンを発行していました。  現在は、同じ OAuth 2.0 承認要求が次のようになっています。

```
GET https://login.microsoftonline.com/common/v2.0/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

ここで、 **スコープ** パラメーターには、どのリソースを示すし、アクセス許可、アプリケーションが要求を承認します。 要求されているリソースはまだ要求内に多く存在し、scope パラメーターの各値に含まれています。  このように scope パラメーターを使用すると、v2.0 アプリ モデルの OAuth 2.0 仕様への準拠を高め、一般的な業界の慣行に近づけることができます。  アプリを実行することもできます [増分同意](#incremental-and-dynamic-consent), 、次のセクションで説明されています。

## 増分および動的な同意
一般公開の Azure AD サービスに登録されたアプリは、作成時に Azure ポータルで、必要な OAuth 2.0 のアクセス許可を指定する必要がありました。

![アクセス許可の登録 UI](../media/active-directory-v2-flows/app_reg_permissions.PNG)

必要なアプリケーションが構成されたアクセス許可 **静的に**します。  これにより Azure ポータルにアプリの構成を存在させることができ、コードを適切に維持できる一方、開発者にとっては問題もいくつかあります。

- アプリの作成時に、そのアプリで必要になる可能性のあるすべてのアクセス許可がわかっている必要があります。  後からアクセス許可を追加することは困難なプロセスでした。
- また、アプリでアクセスする可能性のあるすべてのリソースが事前にわかっている必要があります。  任意の数のリソースにアクセスするアプリを作成することは、困難でした。
- アプリは、ユーザーの初回サインインの時点で、そのアプリで必要になる可能性があるすべてのアクセス許可を要求する必要があります。  アクセス許可のリストは非常に長くなる場合があり、エンドユーザーが初回サインイン時にアプリのアクセスを承認する阻害要因となっていました。

バージョン 2.0 のアプリケーション モデルでは、アクセス許可を指定できます、アプリのニーズ **動的に**, 、アプリの通常の使用時に、実行時にします。  これを行うには、任意の時点でアプリに必要なスコープを承認要求の `scope` パラメーターで指定できます。

```
GET https://login.microsoftonline.com/common/v2.0/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

この要求は、アプリで Azure AD ユーザーのディレクトリ データの読み取りおよび書き込みのためのアクセス許可を求めています。  ユーザーが過去にこの特定のアプリでこれらのアクセス許可に同意した場合、資格情報を入力してアプリにサインインできます。  ユーザーがこれらのアクセス許可のいずれにも同意していない場合、v2.0 エンドポイントはこれらのアクセス許可に対する同意をユーザーに求めます。  詳細についてを参照してください。 [アクセス許可、承認、およびスコープ](active-directory-v2-scopes.md)します。

アプリで `scope` パラメーターを使用してアクセス許可を動的に要求できるため、ユーザーの操作を完全に制御できます。  必要に応じて、同意の操作を初期段階に組み込み、1 回の初期認証要求ですべてのアクセス許可を求めます。  または、アプリで多数のアクセス許可が必要な場合は、時間の経過と共に、ユーザーがアプリの特定の機能を使用するときにアクセス許可を集めることができます。

## オフライン アクセス
v2.0 アプリ モデルでは、アプリのために、`offline_access` スコープというよく知られた新しいアクセス許可を採用しています。  すべてのアプリは、ユーザーがアプリを積極的に利用しない可能性がある場合でも、長期間ユーザーに代わってリソースにアクセスするために必要な、このアクセス許可を要求する必要があります。  `offline_access` スコープは、コンテンツ ダイアログで "データにオフラインでアクセスします" と表示されます。  `offline_access` アクセス許可を要求すると、Web アプリで v2.0 エンドポイントから OAuth 2.0 の refresh_tokens を受け取ることができます。  refresh_tokens は、長期間維持され、アクセスの期間を延長するために新しい OAuth 2.0 の access_tokens と交換することができます。  

アプリが `offline_access` スコープを要求しない場合、refresh_tokens を受け取ることはありません。  つまり、authorization_code でを使用するときに、 [OAuth 2.0 承認コード フロー](active-directory-v2-protocols.md#oauth2-authorization-code-flow), 、だけを受信するバックアップから、access_token、 `/oauth2/token` エンドポイント。  その access_token は、短時間 (通常は 1 時間) 有効ですが、最終的には期限が切れます。  その時点で、アプリはユーザーを `/oauth2/authorize` エンドポイントにリダイレクトして、新しい authorization_code を取得する必要があります。  このリダイレクト中に、アプリの種類によっては、ユーザーが資格情報を再入力したり、アクセス許可に再同意したりする必要がある場合もあります。

OAuth 2.0、refresh_tokens、および access_tokens の詳細については、チェック アウト、 [v2.0 アプリ モデル プロトコル リファレンス](active-directory-v2-protocols.md)します。

## トークン要求
v2.0 エンドポイントによって発行されたトークンでの要求は、一般公開の Azure AD エンドポイントによって発行されるトークンと同一にはなりません。新しいサービスに移行するアプリでは、id_tokens または access_tokens 内に特定の要求の存在を想定しないでください。   v2.0 エンドポイントによって発行されたトークンは OAuth 2.0 および OpenID Connect 仕様に準拠していますが、一般公開の Azure AD サービスとは異なるセマンティクスに従っている可能性があります。

V2.0 アプリ モデル トークンで出力された特定のクレームの詳細についてを参照してください、 [v2.0 トークン参照](active-directory-v2-tokens.md)します。


## プレビューの制限事項
パブリック プレビュー期間中に v2.0 アプリ モデルを使用してアプリをビルドする際、注意が必要な制限事項がいくつかあります。  参照してください、 [v2.0 アプリ モデルの制限事項 doc](active-directory-v2-limitations.md) にこれらの制限のいずれかが特定のシナリオに当てはまるかどうかを参照してください。

