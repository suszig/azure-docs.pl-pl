<properties
   pageTitle="Azure AD Graph API のクイック スタート | Microsoft Aure"
   description="Azure Active Directory Graph API を使用すると、OData REST API エンドポイントを介して Azure AD にプログラムによってアクセスできます。アプリケーションでは、Graph API を使用して、ディレクトリのデータとオブジェクトに対して、作成、読み取り、更新、および削除 (CRUD) の各操作を実行できます。"
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
> [AZURE.IMPORTANT] Azure AD Graph API の機能がを介して利用も [Graph](https://graph.microsoft.io/), 、Outlook、OneDrive、OneNote、プランナー、および 1 つのエンドポイントと 1 つのアクセス トークンを使用してアクセス可能な Office グラフなどの他の Microsoft サービスからの Api を含んだ API の統合します。

## Graph API URL の作成方法

Graph API では、CRUD 操作を実行するディレクトリのデータとオブジェクト (つまり、リソースまたはエンティティ) にアクセスするために、Open Data (OData) プロトコルに基づく URL を使用できます。 Graph API で使用される Url は、次の 4 つの主要な部分で構成されていますサービス ルート、テナント id、リソース パス、クエリ文字列オプション: `https://graph.windows.net/{テナント id}/{リソース パス}? [。クエリ パラメーター]`します。 次の URL の例を見て: `https://graph.windows.net/contoso.com/groups?api-version=1.5`します。

- **サービス ルート**: Azure AD Graph API では常に https://graph.windows.net にサービスのルートです。
- **テナント ID**: 確認済み (登録済み) のドメイン名を指定できます。上記の例では contoso.com です。 テナント オブジェクト ID またはエイリアス ("myorganiztion" または "me") を指定することもできます。 詳細については、次を参照してください。 [Graph API のエンティティのアドレス指定と操作](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview))します。
- **リソース パス**: URL のこのセクションは、対話するリソース (ユーザー、グループ、特定のユーザー、特定のグループなど) を識別します。 上記の例では、リソース セットのアドレスを指定する最上位の "groups" です。 特定のエンティティ ("users/{objectId}" や "users/userPrincipalName" など) のアドレスを指定することもできます。
- **クエリ パラメーター**: でしょうか。 クエリ パラメーター] セクションのリソース パス セクションを区切ります。 Graph API ではすべての要求に "api-version" クエリ パラメーターが必要です。 Graph API では、次の OData クエリ オプションもサポートします。**$filter**、**$orderby**、**$expand**、**$top**、および **$format**。 次のクエリ オプションは現在サポートされていません。**$count**、**$inlinecount**、および **$skip**。 詳細については、次を参照してください。 [サポートされているクエリ、フィルター、および Azure AD Graph API でのページング オプション](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options)します。

## Graph API のバージョン

次のバージョンの Graph API がリリースされています。

* ベータ版
* バージョン 1.5
* バージョン 2013-11-08
* バージョン 2013-04-05

“api-version” クエリ パラメーターに、Graph API 要求用のバージョンを指定します。 バージョン 1.5 の場合は、バージョン値を使用します (api-version=1.5) 。 以前のバージョンでは、YYYY-MM-DD の形式に準拠した日付文字列を使用します (例: api-version=2013-11-08)。 機能をプレビューする場合は文字列 “beta” を使用します (例: api-version=beta)。 Graph API のバージョンの違いの詳細については、次を参照してください。 [Azure AD Graph API のバージョン管理](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning)します。

## Graph API のメタデータ

Graph API メタデータ ファイルを返す、URL の例ではテナント id の後に"$metadata"セグメントを追加、次の URL はグラフ エクスプ ローラーで使用されるデモ会社のメタデータを返します: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$ メタデータ? api バージョン 1.5 を =`します。 この URL を Web ブラウザーのアドレス バーに入力して、メタデータを表示できます。 返される CSDL メタデータ ドキュメントには、要求した Graph API バージョンによって公開されているエンティティと複合型、それらのプロパティ、および関数とアクションが記述されています。 api-version パラメーターを省略すると、最新バージョンのメタデータが返されます。

## 一般的なクエリ

[Azure AD Graph API の一般的なクエリ](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) 、ディレクトリ内の最上位のリソースにアクセスするために使用するクエリや、ディレクトリで操作を実行するクエリなど、Azure AD Graph で使用できる一般的なクエリを一覧表示します。

たとえば、 `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.5` 会社ディレクトリ contoso.com の情報を返します。

または `https://graph.windows.net/contoso.com/users?api-version=1.5` 、ディレクトリ contoso.com 内のすべてのユーザー オブジェクトを一覧表示します。

## Graph Explorer の使用

Azure AD Graph API 用の Graph Explorer を使用して、アプリケーションの作成時にディレクトリ データをクエリできます。
> [AZURE.IMPORTANT] Graph Explorer では、ディレクトリへのデータの書き込みやディレクトリからのデータの削除は実行できません。 Graph Explorer で実行できるのは、Azure AD ディレクトリに対する読み取り操作のみです。

グラフ エクスプ ローラーに移動し、デモ会社を使用してを選択し、入力したかどうかの表示と同じ出力は、次の `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.5` デモ ディレクトリ内のすべてのユーザーを表示します。

![Azure AD Graph API Explorer](./media/active-directory-graph-api-quickstart/screen_shot.jpg)

**グラフ エクスプ ローラーを読み込む**: 読み込むには、ツールに移動 [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/)します。 **[デモ会社を使用する]** をクリックして、サンプル テナントのデータに対して Graph Explorer を実行します。 デモの会社を使用するために資格情報は必要ありません。 または、**[サインイン]** をクリックし、Azure AD アカウント資格情報でサインインし、テナントに対して Graph Explorer を実行できます。 独自のテナントに対して Graph Explorer を実行する場合は、ユーザー自身またはユーザーの管理者がサインイン中に同意する必要があります。 Office 365 サブスクリプションがある場合は、Azure AD テナントが自動的に設定されます。 Office 365 にサインインするための資格情報は、実際は Azure AD アカウントであり、これらの資格情報を Graph Explorer で使用できます。

**クエリを実行する**:クエリを実行するには、要求テキスト ボックスにクエリを入力し、**[取得]** をクリックするか、**Enter** キーを押します。 結果が応答ボックスに表示されます。 たとえば、 `https://graph.windows.net/graphdir1.onmicrosoft.com/groups でしょうか。 api バージョン 1.5 を =` デモ ディレクトリ内のすべてのグループ オブジェクトが一覧表示します。

次の機能と、グラフ エクスプ ローラーの制限事項に注意してください。
- リソースのオートコンプリート機能を設定します。 これを確認するには、**[デモ会社を使用する]** をクリックし、(会社の URL が表示されている) 要求テキスト ボックスをクリックします。 ドロップダウン リストからリソース セットを選択できます。

- アドレス指定のエイリアスとして “me” と “myorganization” の使用をサポートします。 たとえば、使用することができます `https://graph.windows.net/me?api-version=1.5` サインインしたユーザーのユーザー オブジェクトを取得するか、 `https://graph.windows.net/myorganization/users?api-version=1.5` を現在のディレクトリ内のすべてのユーザーを返します。 デモ会社に対して “me” エイリアスを使用するとエラーが返ります。これは、要求を行うサインイン ユーザーが存在しないためです。

- 応答のヘッダー セクション。 これはクエリの実行時に発生した問題の解決に役立ちます。

- 展開機能と折りたたみ機能がある応答用の JSON ビューアー。

- サムネイル写真の表示はサポートされません。

## Fiddler を使用したディレクトリへの書き込み

このクイックスタート ガイドでは、Azure AD ディレクトリに対する「書き込み」操作を練習するために、Fiddler Web Debugger を使用できます。 詳細については、および Fiddler をインストールするには、「 [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler)します。

次の例では、Fiddler Web Debugger を使用して、Azure AD ディレクトリに新しいセキュリティ グループ 'MyTestGroup' を作成します。

**アクセス トークンを取得する**: Azure AD Graph にアクセスするには、クライアントの Azure AD に対する認証が成功している必要があります。 詳細については、次を参照してください。 [Azure AD の認証シナリオ](active-directory-authentication-scenarios.md)します。

**クエリを構成して実行する**: 次の手順を完了します。

1. Fiddler Web Debugger を開き、**[Composer]** タブに切り替えます。
2. 新しいセキュリティ グループを作成するため、プルダウン メニューから HTTP メソッドとして **[Post]** を選択します。 グループ オブジェクトに対する操作とアクセス許可の詳細については、次を参照してください。 [グループ](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) 内で、 [Azure AD Graph REST API リファレンス](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)します。
3. 横にあるフィールドで **Post**, 、要求 URL として次の種類: `https://graph.windows.net/mytenantdomain/groups?api-version=1.5`します。
    > [AZURE.NOTE] mytenantdomain は、各自の Azure AD ディレクトリのドメイン名に置き換える必要があります。

4. [Post] プルダウンのすぐ下にあるフィールドに、次のように入力します。

    ```
ホスト: graph.windows.net
認証: ユーザーのアクセス トークン
コンテンツの種類: アプリケーション/json
```

    > [AZURE.NOTE] Substitute your &lt;your access token&gt; with the access token for your Azure AD directory.

5. In the **Request body** field, type the following:
```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }

```

    For more information about creating groups, see [Create Group](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

詳細については、Azure AD エンティティと Graph によって公開されている型および Graph でそれらに対して実行できる操作については、「 [Azure AD Graph REST API リファレンス](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)します。

## 次のステップ

詳細について、 [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)





