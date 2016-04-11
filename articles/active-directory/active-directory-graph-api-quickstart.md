<properties
   pageTitle="Azure AD Graph API のクイック スタート | Microsoft Aure"
   description="Azure Active Directory Graph API を使用すると、OData REST API エンドポイントを介して Azure AD にプログラムによってアクセスできます。 アプリケーションでは、Graph API を使用して、ディレクトリのデータとオブジェクトに対して、作成、読み取り、更新、および削除 (CRUD) の各操作を実行できます。"
   services="active-directory"
   documentationCenter="n/a"
   authors="JimacoMS"
   manager="msmbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="11/17/2015"
      ms.author="v-jibran@microsoft.com"/>

# Azure AD Graph API のクイック スタート

Azure Active Directory (AD) Graph API を使用すると、OData REST API エンドポイントを介して Azure AD にプログラムによってアクセスできます。 アプリケーションでは、Graph API を使用して、ディレクトリのデータとオブジェクトに対して、作成、読み取り、更新、および削除 (CRUD) の各操作を実行できます。 たとえば、Graph API を使用して、新しいユーザーの作成、ユーザーのプロパティの表示または更新、ユーザーのパスワードの変更、ロールベースでアクセスするためのグループ メンバーシップの確認、ユーザーの無効化または削除を行うことができます。 Graph API の機能とアプリケーション シナリオの詳細については、次を参照してください。 [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) と [Azure AD Graph API の前提条件](https://msdn.microsoft.com/library/hh974476(Azure.100).aspx)します。

> [AZURE.IMPORTANT] Azure AD Graph API の機能がを介して利用も [Graph](https://graph.microsoft.io/), 、統合など、Outlook、OneDrive、OneNote、プランナー、および Office グラフ、単一のエンドポイントと単一のアクセス トークンを使用してアクセス可能な場合は、他の Microsoft サービスからの Api が含まれる API です。

## Graph API URL の作成方法

Graph API では、CRUD 操作を実行するディレクトリのデータとオブジェクト (つまり、リソースまたはエンティティ) にアクセスするために、Open Data (OData) プロトコルに基づく URL を使用できます。 Graph API で使用される URL は、4 つの主要部分で構成されます。主要部分は、サービス ルート、テナント ID、リソース パス、およびクエリ文字列オプション (https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]) です。`https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]` 次の URL の例を使用して説明します。`https://graph.windows.net/contoso.com/groups?api-version=1.5`

- **サービス ルート**: Azure AD Graph API では常に https://graph.windows.net にサービスのルートです。
- **テナントの識別子**: contoso.com 上記の例での確認済み (登録済み) ドメイン名であるこのことができます。 テナント オブジェクト ID またはエイリアス ("myorganiztion" または "me") を指定することもできます。 詳細については、次を参照してください。 [Graph API のエンティティのアドレス指定と操作](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview))。
- **リソース パス**: URL のこのセクションでは、(ユーザー、グループ、特定のユーザーや、特定のグループなど) と対話するリソースを示します。上記の例では、リソース セットのアドレスを指定する最上位の "groups" です。 特定のエンティティ ("users/{objectId}" や "users/userPrincipalName" など) のアドレスを指定することもできます。
- **クエリ パラメーター**: でしょうか。 クエリ パラメーター] セクションのリソース パス セクションを区切ります。 Graph API ではすべての要求に "api-version" クエリ パラメーターが必要です。 Graph API には、次の OData クエリ オプションもサポートしています: **$filter**, 、**$orderby**, 、**$展開**, 、**$top**, 、および **$format**します。 次のクエリ オプションは現在サポートされていません: **$count**, 、**$inlinecount**, 、および **$skip**します。 詳細については、次を参照してください。 [サポートされているクエリ、フィルター、および Azure AD Graph API でのページング オプション](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options)します。

## Graph API のバージョン

次のバージョンの Graph API がリリースされています。

* ベータ版
* バージョン 1.5
* バージョン 2013-11-08
* バージョン 2013-04-05

“api-version” クエリ パラメーターに、Graph API 要求用のバージョンを指定します。 バージョン 1.5 の場合は、バージョン値を使用します (api-version=1.5) 。 以前のバージョンでは、YYYY-MM-DD の形式に準拠した日付文字列を使用します (例: api-version=2013-11-08)。 機能をプレビューする場合は文字列 “beta” を使用します (例: api-version=beta)。 Graph API のバージョンの違いの詳細については、次を参照してください。 [Azure AD Graph API のバージョン管理](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning)します。

## Graph API のメタデータ

Graph API メタデータ ファイルが返るようにするには、URL の tenant-identifier の後ろに "$metadata" セグメントを追加します。たとえば、次の URL では、Graph Explorer で使用されるデモ会社のメタデータが返されます。`https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.5`。 この URL を Web ブラウザーのアドレス バーに入力して、メタデータを表示できます。 返される CSDL メタデータ ドキュメントには、要求した Graph API バージョンによって公開されているエンティティと複合型、それらのプロパティ、および関数とアクションが記述されています。 api-version パラメーターを省略すると、最新バージョンのメタデータが返されます。

## 一般的なクエリ

[Azure AD Graph API の一般的なクエリ](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) 、ディレクトリ内の最上位のリソースにアクセスするために使用するクエリや、ディレクトリで操作を実行するクエリなど、Azure AD Graph で使用できる一般的なクエリを一覧表示します。

たとえば、`https://graph.windows.net/contoso.com/tenantDetails?api-version=1.5` は、contoso.com ディレクトリの会社情報が返ります。

`https://graph.windows.net/contoso.com/users?api-version=1.5` は、contoso.com ディレクトリのすべてのユーザー オブジェクトを一覧表示します。

## Graph Explorer の使用

Azure AD Graph API 用の Graph Explorer を使用して、アプリケーションの作成時にディレクトリ データをクエリできます。

> [AZURE.IMPORTANT] グラフ エクスプ ローラーは、書き込みまたはディレクトリからのデータの削除をサポートしていません。 Graph Explorer で実行できるのは、Azure AD ディレクトリに対する読み取り操作のみです。

次に示す図は、Graph Explorer に移動し、[デモ会社を使用する] を選択し、デモ ディレクトリのすべてのユーザーを表示するために「`https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.5`」と入力した場合に表示される出力です。

![Azure AD Graph API Explorer](./media/active-directory-graph-api-quickstart/screen_shot.jpg)

**グラフ エクスプ ローラーを読み込む**: 読み込むには、ツールに移動 [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/)します。 クリックして **デモ会社を使用** をサンプル テナントからのデータに対してグラフ エクスプ ローラーを実行します。 デモの会社を使用するために資格情報は必要ありません。 クリックする代わりに、 **サインイン** し、テナントに対するグラフ エクスプ ローラーを実行する Azure AD アカウントの資格情報でサインインします。 独自のテナントに対して Graph Explorer を実行する場合は、ユーザー自身またはユーザーの管理者がサインイン中に同意する必要があります。 Office 365 サブスクリプションがある場合は、Azure AD テナントが自動的に設定されます。 Office 365 にサインインするための資格情報は、実際は Azure AD アカウントであり、これらの資格情報を Graph Explorer で使用できます。

**クエリを実行する**: クエリを実行する要求のテキスト ボックスにクエリを入力し、をクリックして **取得** かをクリックして、 **入力** キー。 結果が応答ボックスに表示されます。 たとえば、`https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.5` は、デモ ディレクトリ内のすべてのグループ オブジェクトを一覧表示します。

Graph Explorer の次の機能と制限事項に注意してください。
- リソース セットのオートコンプリート機能。 これを参照するにはクリックして **デモ会社を使用** し (会社の URL が表示されます)、要求のテキスト ボックスをクリックします。 ドロップダウン リストからリソース セットを選択できます。

- アドレス指定のエイリアスとして “me” と “myorganization” の使用をサポートします。 たとえば、`https://graph.windows.net/me?api-version=1.5` を使用してサインインしているユーザーのユーザー オブジェクトを返し、`https://graph.windows.net/myorganization/users?api-version=1.5` を使用して現在のディレクトリのすべてのユーザーを返すことができます。 デモ会社に対して “me” エイリアスを使用するとエラーが返ります。これは、要求を行うサインイン ユーザーが存在しないためです。

- 応答のヘッダー セクション。 これはクエリの実行時に発生した問題の解決に役立ちます。

- 展開機能と折りたたみ機能がある応答用の JSON ビューアー。

- サムネイル写真の表示はサポートされません。

## Fiddler を使用したディレクトリへの書き込み

このクイックスタート ガイドでは、Azure AD ディレクトリに対する「書き込み」操作を練習するために、Fiddler Web Debugger を使用できます。 詳細については、および Fiddler をインストールするには、「 [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler)します。

次の例では、Fiddler Web Debugger を使用して、Azure AD ディレクトリに新しいセキュリティ グループ 'MyTestGroup' を作成します。

**アクセス トークンを取得**: Azure AD Graph にアクセスするには、まず、Azure AD に対する認証に成功にクライアントが必要です。 詳細については、次を参照してください。 [Azure AD の認証シナリオ](active-directory-authentication-scenarios.md)します。

**作成してクエリを実行する**: 次の手順を完了します。

1. Fiddler Web Debugger を開きに切り替えて、 **Composer** ] タブをクリックします。
2. 新しいセキュリティ グループを作成するので、選択 **Post** プルダウン メニューから HTTP メソッドとします。 グループ オブジェクトに対する操作とアクセス許可の詳細については、次を参照してください。 [グループ](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) 内で、 [Azure AD Graph REST API リファレンス](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)します。
3. 横にあるフィールドで **Post**, 、要求 URL として次の種類: `https://graph.windows.net/mytenantdomain/groups?api-version=1.5`です。

    > [AZURE.NOTE] Mytenantdomain は Azure AD ディレクトリのドメイン名に置き換える必要があります。

4. [Post] プルダウンのすぐ下にあるフィールドに、次のように入力します。

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] 代わりに & lt; アクセス トークン & gt;で、Azure AD ディレクトリのアクセス トークンです。

5.  **要求本文** フィールドで、次を入力します。

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    グループの作成の詳細については、次を参照してください。 [グループの作成](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup)します。

詳細については、Azure AD エンティティと Graph によって公開されている型および Graph でそれらに対して実行できる操作については、「 [Azure AD Graph REST API リファレンス](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)します。

## 次のステップ

詳細について、 [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)


