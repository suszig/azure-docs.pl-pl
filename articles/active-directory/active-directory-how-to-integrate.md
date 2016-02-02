<properties
   pageTitle="Azure Active Directory と統合する方法 | Microsoft Azure"
   description="Azure Active Directory との統合に関する利点とリソースについてのガイドです。"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="11/17/2015"
   ms.author="mbaldwin"/>


# Azure Active Directory との統合

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure Active Directory は、クラウド アプリケーションに関するエンタープライズ レベルの ID 管理を提供します。 Azure AD の統合は、簡素化されたサインイン エクスペリエンスをユーザーに提供し、アプリケーションが IT ポリシーに準拠できるようにします。

## 統合方法

アプリケーションを Azure AD と統合するにはいくつかの方法があります。 アプリケーションに適したシナリオを必要な数だけいくつでも利用できます。

### アプリケーションにサインインする手段としての Azure AD のサポート

**サインインの手間が減り、サポート コストが削減されます。**Azure AD を使用してアプリケーションにサインインすることで、ユーザーは名前とパスワードを 1 つ余計に覚える必要がなくなります。 開発者としては、保存して保護するパスワードが 1 つ減ります。 忘れたパスワードのリセットを処理する必要がないだけで、かなり手間が省けます。 Azure AD により、Office 365 や Microsoft Azure などの世界で最も人気のある一部のクラウド アプリケーションのサインインが容易になります。 数百万の組織の何億人ものユーザーが、既に Azure AD にサインインしています。 詳細について [Azure AD サインインのサポートを追加する](active-directory-authentication-scenarios.md)します。

**アプリケーションへのサインアップが簡単になります。**アプリケーションへのサインアップの間に、Azure AD はユーザーに関する基本的な情報を送信するので、サインアップ フォームに事前に入力したり、入力を完全に排除したりできます。 ユーザーは、ソーシャル メディアやモバイル アプリケーションと同様の使い慣れた同意エクスペリエンスから、Azure AD アカウントを使用してアプリケーションにサインアップできます。 すべてのユーザーが、IT スタッフの介入を必要とせずに、Azure AD と統合されているアプリケーションにサインアップおよびサインインできます。 詳細について [Azure AD アカウント ログイン用のアプリケーションへのサインアップ](../mobile-services-how-to-register-active-directory-authentication.md) します。

### ユーザーの参照、ユーザーのプロビジョニングの管理、およびアプリケーションへのアクセスの制御

**ディレクトリのユーザーを参照します。**他のユーザーを招待したりアクセスを許可するときは、Graph API を使用して組織内のユーザーを検索して参照できます。電子メール アドレスを入力する必要はありません。 ユーザーは、組織の詳細な階層の表示も含めて、使い慣れたアドレス帳スタイルのインターフェイスを使用して参照できます。 詳細について、 [Graph API](active-directory-graph-api.md)します。

**顧客が既に管理している Active Directory のグループと配布リストを再利用します。**Azure AD には、顧客が電子メールの配布やアクセスの管理に既に使用しているグループが含まれます。 Graph API を使用して、これらのグループを再利用できます。顧客がアプリケーションで別のグループ セットを作成して管理する必要はありません。 グループの情報は、サインイン トークンでアプリケーションにも送信できます。 詳細について、 [Graph API](active-directory-graph-api.md)します。

**Azure AD を使用して、アプリケーションにアクセスできるユーザーを制御します。**Azure AD の管理者およびアプリケーション所有者は、特定のユーザーおよびグループにアプリケーションへのアクセスを割り当てることができます。 Graph API を使用すると、このリストを読み取ったり、リストを使用してアプリケーション内のリソースやアクセスのプロビジョニングとプロビジョニング解除を制御したりできます。

**ロール ベースの Access Control に Azure AD を使用します。**管理者とアプリケーション所有者は、アプリケーションを Azure AD に登録するときに定義したロールに、ユーザーやグループを割り当てることができます。 ロール情報はサインイン トークンでアプリケーションに送信され、Graph API を使用して読み取ることができます。 詳細について [Azure AD 認証を使用する](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx)します。

### ユーザーのプロファイル、予定表、電子メール、連絡先、ファイル、その他へのアクセスの取得

**Azure AD は、Office 365 やその他の Microsoft ビジネス サービスに対する承認サーバーです。**アプリケーションへのサインイン用に Azure AD をサポートしている場合、または OAuth 2.0 による Azure AD ユーザー アカウントへの現在のユーザー アカウントのリンクをサポートしている場合は、ユーザーのプロファイル、予定表、電子メール、連絡先、ファイル、その他の情報への読み書きアクセスを要求できます。 ユーザーの予定表にシームレスにイベントを書き込んだり、OneDrive のファイルを読み書きしたりできます。 詳細について [、Office 365 Api にアクセスする](https://msdn.microsoft.com/office/office365/howto/platform-development-overview)します。

### Azure および Office 365 マーケットプレースのアプリケーションの宣伝

**既に Azure AD を使用している数百万の組織にアプリケーションを宣伝します。**マーケットプレースを検索および参照しているユーザーは既にクラウド サービスを使用しており、クラウド サービスの顧客として認定されています。 アプリケーションの宣伝に関する詳細については [Azure Marketplace](http://azure.microsoft.com/marketplace/partner-program/)します。

**ユーザーがアプリケーションにサインアップすると、アプリケーションは Azure AD アクセス パネルと Office 365 アプリケーション ランチャーに表示されるようになります。**ユーザーは後で迅速かつ簡単にアプリケーションに戻ることができ、ユーザーの結びつきが強くなります。 詳細について、 [Azure AD アクセス パネル](active-directory-saas-access-panel-introduction.md)します。

### デバイスとサービスの間およびサービスとサービスの間の安全な通信

**サービスおよびデバイスの ID 管理に Azure AD を使用すると、記述する必要があるコードが減り、IT 部門はアクセスを管理できるようになります。**サービスおよびデバイスは、OAuth を使用して Azure AD からトークンを取得し、そのトークンを使用して Web API にアクセスできます。 Azure AD を使用すると、複雑な認証コードを作成しなくて済みます。 サービスおよびデバイスの ID は Azure AD に格納されるため、IT 部門はキーと失効を 1 か所で管理できます。アプリケーションで個別に行う必要はありません。

## 統合の利点

Azure AD との統合には、追加コードを記述する必要がないという利点があります。

### エンタープライズ ID 管理との統合

**アプリケーションを IT ポリシーに準拠させるのに役立ちます。**組織はエンタープライズ ID 管理システムと Azure AD を統合しているので、ユーザーが組織からいなくなると、IT 部門が余分な手順を実行しなくても、ユーザーは自動的にアプリケーションにアクセスできなくなります。 IT 部門はアプリケーションにアクセスできるユーザーを管理し、必要なアクセス ポリシーを決定できるので (多要素認証など)、複雑な企業ポリシーに準拠するコードを記述する必要性が減ります。 Azure AD はアプリケーションにサインインしたユーザーに関する詳細な監査ログを管理者に提供するので、IT 部門は使用状況を追跡できます。

**Azure AD は Active Directory をクラウドに拡張し、アプリケーションが AD と統合できるようにします。**世界中の多くの組織は、サインインおよび ID 管理の主要なシステムとして Active Directory を使用しており、アプリケーションに AD との連携を求めます。 Azure AD と統合すると、アプリが Active Directory と統合されます。

### 高度なセキュリティ機能

**多要素認証。**Azure AD は、ネイティブな多要素認証機能を提供します。 IT 管理者はアプリケーションへのアクセスに多要素認証を要求でき、このサポートを自分でコーディングする必要はありません。 詳細について [多要素認証](http://azure.microsoft.com/documentation/services/multi-factor-authentication/)します。

**異常なサインインの検出。**Azure AD は 1 日に 10 億以上のサインインを処理しながら、機械学習アルゴリズムを使用して疑わしいアクティビティを検出し、可能性のある問題を IT 管理者に通知します。 Azure AD のサインインをサポートすることにより、アプリケーションでこの保護を利用できます。 詳細について [Azure Active Directory アクセス レポートを表示する](active-directory-view-access-usage-reports.md)します。

**条件付きアクセス。**多要素認証に加えて、管理者はアプリケーションにサインインするユーザーに特定の条件を満たすことを要求できます。 設定可能な条件としては、クライアント デバイスの IP アドレスの範囲、指定したグループにおけるメンバーシップ、アクセスに使用されるデバイスの状態などがあります。 詳細について [Azure Active Directory の条件付きアクセス](active-directory-conditional-access.md)します。

### 簡単な開発

**業界標準のプロトコル。**Microsoft は業界標準のサポートに取り組んでいます。 Azure AD は、SAML 2.0、OpenID Connect 1.0、OAuth 2.0、WS-Federation 1.2 の各認証プロトコルをサポートしています。 Graph API は OData 4.0 に準拠しています。 アプリケーションがフェデレーション サインイン用に SAML 2.0 または OpenID Connect 1.0 プロトコルを既にサポートしている場合、Azure AD のサポートを簡単に追加できます。 詳細について [Azure AD で認証プロトコルのサポート](active-directory-authentication-protocols.md)します。

**オープン ソース ライブラリ。**Microsoft は、開発時間短縮のため、一般的な言語とプラットフォームの完全にサポートされたオープン ソース ライブラリを提供します。 ソース コードは Apache 2.0 でライセンスされており、自由にフォークしてプロジェクトに役立てることができます。 詳細について [Azure AD 認証ライブラリ](active-directory-authentication-libraries.md)します。

### 世界的なプレゼンスと高可用性

**Azure AD は、世界各地のデータセンターにデプロイされ、常に管理および監視されています。**Azure AD は Microsoft Azure および Office 365 用の ID 管理システムであり、世界各地にある 28 のデータセンターにデプロイされています。 ディレクトリ データは、3 つ以上のデータセンターにレプリケートされることが保証されます。 グローバルなロード バランサーが、ユーザーが自分のデータを含む最も近い Azure AD のコピーにアクセスすることを保証し、問題が検出された場合は自動的に他のデータセンターに要求を再ルーティングします。

## 次のステップ

[コードを書き始める](active-directory-developers-guide.md#getting-started)します。

[Azure AD を使用してユーザーのサインイン](active-directory-authentication-scenarios.md)





