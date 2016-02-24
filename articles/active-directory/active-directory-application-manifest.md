<properties
   pageTitle="Azure Active Directory のアプリケーション マニフェストについて | Microsoft Azure"
   description="Azure AD テナントに各アプリケーション構成を含む Azure AD アプリケーション マニフェストについて詳しく説明します。"
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/01/2015"
   ms.author="dkershaw;bryanla"/>

# Azure Active Directory のアプリケーション マニフェストについて

Azure Active Directory (AD) と統合するアプリケーションは、Azure AD テナントに登録され、アプリケーションの永続的な ID 構成を提供する必要があります。 この構成は実行時に参照されるため、アプリケーションが Azure AD を介して認証と承認を外部委託および仲介するシナリオが可能になります。 Azure AD のアプリケーション モデルの詳細については、[追加、更新、およびアプリケーションの削除] を参照してください。 [追加の UPD-RMV-アプリケーション] の記事です。

## アプリケーションの ID 構成の更新

次に示すように、アプリケーションの ID 構成のプロパティを更新する方法には、現実に複数の方法があります。ただし、実行できる操作と難易度がそれぞれ異なります。

-  **の Azure クラシック ポータルの [AZURE クラシック ポータル] Web ユーザー インターフェイス** アプリケーションの最も一般的なプロパティを更新することができます。 これは、最も手軽でミスを招きにくいアプリケーションのプロパティの更新方法ですが、次の 2 つの方法とは異なり、すべてのプロパティに完全にアクセスすることはできません。
- Azure クラシック ポータルに公開されていないプロパティを更新する必要がある高度なシナリオを変更することができます、 **アプリケーション マニフェスト**します。 この記事では、この方法に話題を絞り、次のセクションで詳しく説明していきます。
- ことも **API を使用して、[グラフ] [GRAPH] アプリケーションを記述** 労力を必要とするアプリケーションを更新します。 これは、管理ソフトウェアを作成している場合や定期的かつ自動的にアプリケーションのプロパティを更新する必要がある場合に魅力的な選択肢になる可能性があります。

## アプリケーション マニフェストを使用してアプリケーションの ID 構成を更新する
[Azure クラシック ポータル] で [AZURE のクラシック-ポータル]、ダウンロードし、アプリケーション マニフェストと呼ばれる JSON ファイルの形式をアップロードして、アプリケーションの id の構成を管理することができます。 ディレクトリには実際のファイルは格納されません。アプリケーション マニフェストは Azure AD Graph API アプリケーション エンティティに対する単なる HTTP GET 操作であり、アップロードはアプリケーション エンティティに対する HTTP PATCH 操作です。

その結果、形式およびアプリケーション マニフェストのプロパティを理解するために必要参照 Graph API の [アプリケーション エンティティ] に [アプリケーション エンティティ] ドキュメントです。 ただしアプリケーション マニフェストのアップロードに実行されることができる更新プログラムの例
示します。

- Web API によって公開されているアクセス許可のスコープ (oauth2Permissions) を宣言する。 [Azure Active Directory でアプリケーションを統合する] で"を公開する Web Api を他のアプリケーション] を参照してください [統合-アプリケーション-AAD] に対して、oauth2Permissions を使用してユーザーの権限借用を実装する方法については、アクセス許可スコープを委任します。 前述のように、すべてのアプリケーションのエンティティ プロパティは、「、Graph API の [エンティティおよび複合型の参照] を [OAuth2Permission] 種類のコレクションでは oauth2Permissions メンバーを含むアプリケーション、エンティティ参照のアーティクル [アプリケーション エンティティ OAUTH2 のアクセス許可].
- アプリによって公開されるアプリケーション ロール (appRoles) を宣言する。 アプリケーション エンティティのアプリケーション ロールのメンバーは、アプリケーション ロール [アプリケーションのエンティティのアプリ-ROLE] の種類のコレクションです。 [役割ベースのアクセス制御 Azure AD を使用するクラウド アプリケーションで] を参照してください。 実装例については、[RBAC-クラウドのアプリ-AZUREAD] 資料です。
- 既知のクライアント アプリケーションを宣言する。
- サインインしているユーザーのグループ メンバーシップ要求を発行するように Azure AD にリクエストする。  注: さらに、ユーザーのディレクトリ ロール メンバーシップに関する要求を発行するように構成することもできます。 [AD グループを使用してクラウド アプリケーションの承認] を参照してください。 実装例については、[AAD-グループ-FOR の承認] 資料です。
- アプリケーションが (埋め込み JavaScript Web ページまたは Single Page Application (SPA) 用に) OAuth 2.0 の暗黙的な許可フローをサポートするように設定する。
- X509 証明書の秘密キーとしての使用を可能にする。 [ビルド サービスし、デーモン アプリケーションを Office 365 で] を参照してください [O365 のサービス、デーモンのアプリ] と [Azure リソース マネージャー API を使用するために開発者ガイド] [DEV-GUIDE-TO-AUTH-WITH-ARM] 記事の例については実装します。

アプリケーション マニフェストは、アプリケーションの登録の状態を追跡するための優れた方法でもあります。 アプリケーション マニフェストは JSON 形式で使用できるため、アプリケーションのソース コードと共にファイル表現をソース管理にチェックインできます。

### 詳しい手順
ここでは、アプリケーション マニフェストを使ってアプリケーションの ID 構成を更新するための手順を紹介します。

1. [Azure クラシック ポータル] に移動 [AZURE クラシック ポータル] サービスの管理者または共同管理者の権限を持つアカウントを使用してサインインします。


2. 認証されたら、下へスクロールして、左側のナビゲーション パネルで Azure の "Active Directory" 拡張機能 (1) を選択します。次に、アプリケーションが登録されている Azure AD テナント (2) をクリックします。

    ![Azure AD テナントを選択][選択-AZURE の AD のテナント]


3. ディレクトリ ページが表示されたら、ページの上部にある [アプリケーション] (1) をクリックして、テナントに登録されているアプリケーションの一覧を表示します。 次に、更新するアプリケーションを一覧内で見つけてクリックします (2)。

    ![Azure AD テナントを選択][選択-AZURE の AD のアプリ]


4. これで、アプリケーションのメイン ページが選択されました。ここで、ページの下部にある [マニフェストの管理] 機能 (1) に注目してください。 このリンクをクリックすると、JSON マニフェスト ファイルをアップロードまたはダウンロードするように求められます。 [マニフェストのダウンロード] (2) をクリックするとダウンロードの確認ページが表示され、[マニフェストのダウンロード] (3) をクリックして確認するよう求められます。その後、ファイルを開くかローカルに保存するよう求められます (4)。

    ![マニフェストの管理、オプションのダウンロード][管理-マニフェストのダウンロード]

    ![マニフェストをダウンロードする][マニフェストのダウンロード]


5. この例では、ファイルをローカルに保存しています。これにより、エディターでファイルを開いて JSON に変更を加え、もう一度保存することができます。 次に示すのは、Visual Studio の JSON エディターで開いた JSON 構造の例です。 [アプリケーション エンティティ] のスキーマに従っていることに注意してください [アプリケーションの ENTITY] 前に述べたようにします。

    ![マニフェストの JSON の更新][更新プログラムのマニフェスト]


6. マニフェストの更新が終わったら、Azure クラシック ポータルの Azure AD アプリケーション ページに戻ります。もう一度 [マニフェストの管理] (1) をクリックし、次に [マニフェストのアップロード] (2) を選択します。 ダウンロードの場合と同様に、2 つ目のダイアログ ボックスが表示され、JSON ファイルの場所を指定するよう求められます。 "ファイルの参照] をクリックします。 (3) し、選択ファイルへのアップロード] ダイアログ ボックスを使用して、JSON ファイル (4) を選択し、[開く] を押します。 ダイアログ ボックスが閉じたら、"OK" のチェック マーク (5) を選択します。これで、マニフェストがアップロードされます。  

    ![マニフェストの管理、アップロード オプション][管理-マニフェストのアップロード]

    ![マニフェストの JSON のアップロード][マニフェストのアップロード]

    ![マニフェストの JSON の確認をアップロードする][アップロードのマニフェストの確認]

これで終了です。 アプリケーションは、マニフェストに加えた変更に基づく新しいアプリケーション構成を使用して実行されます。

## 次のステップ
下部の DISQUS コメント セクションを使って、ご意見をお寄せください。Microsoft のコンテンツの改善にご協力ください。

<!--Image references-->
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#ApplicationEntity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#AppRoleType
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionType
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

