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

Azure Active Directory (AD) と統合するアプリケーションは、Azure AD テナントに登録され、アプリケーションの永続的な ID 構成を提供する必要があります。 この構成は実行時に参照されるため、アプリケーションが Azure AD を介して認証と承認を外部委託および仲介するシナリオが可能になります。 Azure AD のアプリケーション モデルの詳細については、次を参照してください。、 [の追加、更新、およびアプリケーションの削除][追加 UPD RMV アプリ] 記事です。

## アプリケーションの ID 構成の更新

次に示すように、アプリケーションの ID 構成のプロパティを更新する方法には、現実に複数の方法があります。ただし、実行できる操作と難易度がそれぞれ異なります。

-  **[Azure の旧ポータル][AZURE クラシック ポータル] Web ユーザー インターフェイス** アプリケーションの最も一般的なプロパティを更新することができます。 これは、最も手軽でミスを招きにくいアプリケーションのプロパティの更新方法ですが、次の 2 つの方法とは異なり、すべてのプロパティに完全にアクセスすることはできません。
- Azure クラシック ポータルに公開されていないプロパティを更新する必要がある高度なシナリオを変更することができます、 **アプリケーション マニフェスト**します。 この記事では、この方法に話題を絞り、次のセクションで詳しく説明していきます。
- ことも **を使用するアプリケーションを作成、 [Graph API][GRAPH API]** 労力を必要とするアプリケーションを更新します。 これは、管理ソフトウェアを作成している場合や定期的かつ自動的にアプリケーションのプロパティを更新する必要がある場合に魅力的な選択肢になる可能性があります。

## アプリケーション マニフェストを使用してアプリケーションの ID 構成を更新する
を介して、 [Azure クラシック ポータル][AZURE クラシック ポータル], をダウンロードして、アプリケーションの id の構成を管理する、およびファイル、アプリケーション マニフェストと呼ばれる形式の JSON をアップロードします。 ディレクトリには実際のファイルは格納されません。アプリケーション マニフェストは Azure AD Graph API アプリケーション エンティティに対する単なる HTTP GET 操作であり、アップロードはアプリケーション エンティティに対する HTTP PATCH 操作です。

その結果、書式設定とアプリケーション マニフェストのプロパティを理解するために必要があります、Graph API を参照する [アプリケーション エンティティ][アプリケーション エンティティ] ドキュメントです。 ただしアプリケーション マニフェストのアップロードに実行されることができる更新プログラムの例
示します。

- Web API によって公開されているアクセス許可のスコープ (oauth2Permissions) を宣言する。 "を公開する Web Api を他のアプリケーション] を参照してください [と Azure Active Directory アプリケーションを統合する][統合アプリケーション-AAD] ユーザーの権限借用の実装の詳細について、oauth2Permissions を使用してアクセス許可スコープを委任します。 前述のように、すべてのアプリケーションのエンティティ プロパティは、「、Graph API [エンティティおよび複合型リファレンス][アプリケーション エンティティ] oauth2Permissions メンバー型のコレクションであるなどの関連記事 [OAuth2Permission][アプリケーション エンティティ OAUTH2 権限]します。
- アプリによって公開されるアプリケーション ロール (appRoles) を宣言する。 アプリケーション エンティティのアプリケーション ロールのメンバーは、型のコレクションを [AppRole][アプリケーション エンティティ アプリ ロール]します。 参照してください、 [ロール ベースの Azure AD を使用してクラウド アプリケーションのアクセス制御][RBAC クラウド アプリ AZUREAD] 実装例については、資料です。
- 既知のクライアント アプリケーションを宣言する。
- サインインしているユーザーのグループ メンバーシップ要求を発行するように Azure AD にリクエストする。  注: さらに、ユーザーのディレクトリ ロール メンバーシップに関する要求を発行するように構成することもできます。 参照してください、 [AD グループを使用するクラウド アプリケーションでの承認][AAD グループの承認] 実装例については、資料です。
- アプリケーションが (埋め込み JavaScript Web ページまたは Single Page Application (SPA) 用に) OAuth 2.0 の暗黙的な許可フローをサポートするように設定する。
- X509 証明書の秘密キーとしての使用を可能にする。 参照してください、 [Office 365 でのサービスとデーモンのアプリのビルド][O365 サービス デーモン アプリケーション] と [Azure リソース マネージャー API を使用するために開発者ガイド ][DEV-GUIDE-TO-AUTH-WITH-ARM] の実装例に関する記事です。

アプリケーション マニフェストは、アプリケーションの登録の状態を追跡するための優れた方法でもあります。 アプリケーション マニフェストは JSON 形式で使用できるため、アプリケーションのソース コードと共にファイル表現をソース管理にチェックインできます。

### 詳しい手順
ここでは、アプリケーション マニフェストを使ってアプリケーションの ID 構成を更新するための手順を紹介します。

1. 移動し、 [Azure クラシック ポータル][AZURE クラシック ポータル] サービスの管理者または共同管理者の権限を持つアカウントを使用してサインインします。


2. 認証されたら、下へスクロールして、左側のナビゲーション パネルで Azure の "Active Directory" 拡張機能 (1) を選択します。次に、アプリケーションが登録されている Azure AD テナント (2) をクリックします。

    ![Select the Azure AD tenant][SELECT-AZURE-AD-TENANT]


3. ディレクトリ ページが表示されたら、ページの上部にある [アプリケーション] (1) をクリックして、テナントに登録されているアプリケーションの一覧を表示します。 次に、更新するアプリケーションを一覧内で見つけてクリックします (2)。

    ![Select the Azure AD tenant][SELECT-AZURE-AD-APP]


4. これで、アプリケーションのメイン ページが選択されました。ここで、ページの下部にある [マニフェストの管理] 機能 (1) に注目してください。 このリンクをクリックすると、JSON マニフェスト ファイルをアップロードまたはダウンロードするように求められます。 [マニフェストのダウンロード] (2) をクリックするとダウンロードの確認ページが表示され、[マニフェストのダウンロード] (3) をクリックして確認するよう求められます。その後、ファイルを開くかローカルに保存するよう求められます (4)。

    ![Manage the manifest, download option][MANAGE-MANIFEST-DOWNLOAD]

    ![Download the manifest][DOWNLOAD-MANIFEST]


5. この例では、ファイルをローカルに保存しています。これにより、エディターでファイルを開いて JSON に変更を加え、もう一度保存することができます。 次に示すのは、Visual Studio の JSON エディターで開いた JSON 構造の例です。 スキーマに従っていることに注意してください、 [アプリケーション エンティティ][アプリケーション エンティティ] 前に述べたようにします。

    ![Update the manifest JSON][UPDATE-MANIFEST]


6. マニフェストの更新が終わったら、Azure クラシック ポータルの Azure AD アプリケーション ページに戻ります。もう一度 [マニフェストの管理] (1) をクリックし、次に [マニフェストのアップロード] (2) を選択します。 ダウンロードの場合と同様に、2 つ目のダイアログ ボックスが表示され、JSON ファイルの場所を指定するよう求められます。 "ファイルの参照] をクリックします。 (3) し、選択ファイルへのアップロード] ダイアログ ボックスを使用して、JSON ファイル (4) を選択し、[開く] を押します。 ダイアログ ボックスが閉じたら、"OK" のチェック マーク (5) を選択します。これで、マニフェストがアップロードされます。  

    ![Manage the manifest, upload option][MANAGE-MANIFEST-UPLOAD]

    ![Upload the manifest JSON][UPLOAD-MANIFEST]

    ![Upload the manifest JSON - confirmation][UPLOAD-MANIFEST-CONFIRM]

これで終了です。 アプリケーションは、マニフェストに加えた変更に基づく新しいアプリケーション構成を使用して実行されます。

## 次のステップ
下部の DISQUS コメント セクションを使って、ご意見をお寄せください。Microsoft のコンテンツの改善にご協力ください。

<!--Image references-->
[管理のマニフェストのダウンロード]: ./media/active-directory-application-manifest/manage-manifest-download.png
[管理-マニフェストのアップロード]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[マニフェストのダウンロード]: ./media/active-directory-application-manifest/download-manifest.png
[[AZURE AD アプリ]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[[AZURE AD テナント]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[マニフェストの更新]: ./media/active-directory-application-manifest/update-manifest.png
[マニフェストのアップロード]: ./media/active-directory-application-manifest/upload-manifest.png
[アップロード マニフェスト確認]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD グループの承認]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[追加 UPD RMV アプリ]: active-directory-integrating-applications.md
[アプリケーション エンティティ]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#ApplicationEntity
[アプリケーション エンティティ アプリ ロール]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#AppRoleType
[アプリケーション エンティティ OAUTH2 権限]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionType
[AZURE クラシック ポータル]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH API]: active-directory-graph-api.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365 の詳細] アクセス許可]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365 サービス デーモン アプリケーション]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC クラウド アプリ AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

<!--HONumber=Apr16_HO2-->
