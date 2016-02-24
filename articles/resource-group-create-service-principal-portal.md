<properties
   pageTitle="ポータルで AD のアプリケーションとサービス プリンシパルを作成する | Microsoft Azure"
   description="Azure リソース マネージャーでロール ベースのアクセス制御と共に使用してリソースへのアクセスを管理できる、新しい Active Directory のアプリケーションとサービス プリンシパルを作成する方法について説明します。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/01/2015"
   ms.author="tomfitz"/>

# ポータルを利用し、Active Directory のアプリケーションとサービス プリンシパルを作成する

## 概要
自動化プロセスまたはアプリケーションでサブスクリプションのリソースにアクセスするか、リソースを変更する必要があるとき、ポータルを利用し、Active Directory アプリケーションを作成し、適切なアクセス許可を持つロールにそれを割り当てることができます。 ポータルを使用して Active Directory アプリケーションを作成すると、実際、アプリケーションとサービス プリンシパルの両方が作成されます。 アクセス許可を設定するとき、サービス プリンシパルを使用します。

このトピックでは、Azure ポータルを使用して、新しいアプリケーションとサービス プリンシパルを作成する方法について説明します。 現時点では、新しい Active Directory アプリケーションを作成するには、Microsoft Azure ポータルを使用する必要があります。 この機能は、今後のリリースで、Azure プレビュー ポータルに追加されます。 プレビュー ポータルを使用し、アプリケーションをロールに割り当てることができます。 これらの手順は、Azure Powershell または Azure CLI を介して実行することもできます。 詳細については、次を参照してください。 [Azure リソース マネージャーでサービス プリンシパルの認証](resource-group-authenticate-service-principal.md)します。

## 概念
1. Azure Active Directory (AAD) - クラウド用に構築された ID およびアクセス管理サービス。 詳細について参照してください: [Azure active Directory とは](active-directory/active-directory-whatis.md)
2. サービス プリンシパル - ディレクトリ内のアプリケーションのインスタンス。
3. AD アプリケーション - AAD に対してアプリケーションを特定する ADD 内のディレクトリ レコード。 

アプリケーションとサービス プリンシパルの詳細については、次を参照してください。 [アプリケーション オブジェクトおよびサービス プリンシパル オブジェクト](active-directory/active-directory-application-objects.md)します。 
Active Directory 認証の詳細については、次を参照してください。 [Azure AD の認証シナリオ](active-directory/active-directory-authentication-scenarios.md)します。


## アプリケーション オブジェクトとサービス プリンシパル オブジェクトを作成する

1. によって、Azure アカウントにログインし、 [ポータル](https://manage.windowsazure.com/)します。

2. 選択 **Active Directory** 左側のペインからです。

     ![Active Directory を選択][1]

3. 新しいアプリケーションを作成するために使用するディレクトリを選択します。

     ![ディレクトリの選択][2]

3. アプリケーションをディレクトリを表示する] をクリックして **アプリケーション**します。

     ![アプリケーションの表示][11]

4. そのディレクトリにアプリケーションを作成したことがない場合、次の図のようなものが表示されます。 をクリックして **アプリケーションの追加**

     ![アプリケーションの追加][6]

     または、[ **追加** 、下部ウィンドウにします。

     ![追加][12]

5. 作成するアプリケーションの種類を選択します。 このチュートリアルでは、ギャラリーのアプリケーションは使用しません。

     ![新しいアプリケーション][10]

6. アプリケーションの名前を入力し、使用するアプリケーションの種類を選択します。 作成を選択して、Azure リソース マネージャーでの認証にこのアプリケーションのサービス プリンシパルを使用する予定ですので、 **WEB アプリケーションや WEB API** 次へ] ボタンをクリックします。

     ![名前のアプリケーション][9]

7. アプリのプロパティを入力します。  **サインオン URL**, 、アプリケーションについて説明する web サイトへの URI を提供します。 Web サイトの存在は検証されません。 
 **APP ID URI**, 、アプリケーションが識別する URI を指定します。 エンドポイントの一意性や存在は検証されません。 クリックして、 **完了** AAD アプリケーションを作成します。

     ![アプリケーションのプロパティ][4]

## アプリケーションの認証キーを作成する
ポータルでは、ユーザーのアプリケーションが選択されているはずです。

1. をクリックして、 **構成** タブをアプリケーションのパスワードを構成します。

     ![アプリケーションを構成する][3]

2. 下へスクロールして、 **キー** セクションし、を有効にするパスワードとなる期間を選択します。

     ![キー][7]

3. 選択 **保存** してキーを作成します。

     ![保存][13]

     保存されたキーが表示され、それをコピーすることができます。 キーは後からは取得できないため、今すぐコピーしてください。

     ![保存されたキー][8]

4. これで、サービス プリンシパルとして認証するためにそのキーを使用できます。 必要があります、 **クライアント ID** に加え、 **キー** サインインします。 移動して **クライアント ID** し、それをコピーします。
  
     ![クライアント id][5]

5. 場合によっては、認証要求とともにテナント ID を渡す必要があります。 テナント id を取得するには、選択を **エンドポイントを表示** と次のように、id を取得します。

     ![テナント ID](./media/resource-group-create-service-principal-portal/save-tenant.png)

これで、アプリケーションの準備が完了し、サービス プリンシパルがテナントに作成されました。 サービス プリンシパルとしてサインインするときには、以下を使用してください。

* **クライアント ID** - ユーザー名として。
* **キー** - パスワードとして。

## ロールにアプリケーションを割り当てる

アクションを実行するアクセス許可を付与するには、ロールにアプリケーションを割り当てる必要があります。 使用することができます、 [プレビュー ポータル](https://portal.azure.com) に Active Directory アプリケーションを適切なアクセス許可を持つロールに割り当てます。

プレビュー ポータルでのアクセス制御を開始するには、選択、 **アクセス** アイコン。

![ユーザーの選択](./media/resource-group-create-service-principal-portal/select-users.png)

アプリケーションを割り当てるロールを選択し、アプリケーションを選択します。

![ユーザーの選択](./media/resource-group-create-service-principal-portal/assign-to-role.png)

ユーザーまたはアプリケーションをポータルでロールの割り当ての詳細については、次を参照してください。 [Azure 管理ポータルを使用してアクセスを管理](../role-based-access-control-configure/#manage-access-using-the-azure-management-portal)します。

## アクセス トークンをコードで取得する

.NET を使用している場合は、次のコードを使用し、アプリケーションのアクセス トークンを取得できます。

まず、Visual Studio プロジェクトに Active Directory 認証ライブラリをインストールする必要があります。 これを行う最も簡単な方法は、NuGet パッケージを使用することです。 パッケージ マネージャー コンソールを開き、次のコマンドを入力します。

    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213
    PM> Update-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Safe

アプリケーションで、次のようなメソッドを追加し、トークンを取得します。

    public static string GetAccessToken()
    {
        var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenantId or tenant name}");  
        var credential = new ClientCredential(clientId: "{application id}", clientSecret: "{application password}");
        var result = authenticationContext.AcquireToken(resource: "https://management.core.windows.net/", clientCredential:credential);

        if (result == null) {
            throw new InvalidOperationException("Failed to obtain the JWT token");
        }

        string token = result.AccessToken;

        return token;
    }

## 次のステップ

- セキュリティ ポリシーを指定する方法については、次を参照してください。 [の管理とリソースへのアクセスの監査](resource-group-rbac.md)します。  
- 次の手順のビデオ デモについては、次を参照してください。 [Azure Active Directory と Azure リソースのプログラムによる管理を有効にすると](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory)です。
- Azure PowerShell または Azure CLI を使用して、Active Directory アプリケーションとサービス プリンシパル、証明書を使用して認証をする方法などを操作する方法について、次を参照してください。 [Azure リソース マネージャーでサービス プリンシパルの認証](./resource-group-authenticate-service-principal.md)します。
- Azure リソース マネージャーでのセキュリティを実装する方法の詳細については、次を参照してください。 [セキュリティの考慮事項 Azure リソース マネージャーの](best-practices-resource-manager-security.md)です。


<!-- Images. -->
[1]: ./media/resource-group-create-service-principal-portal/active-directory.png
[2]: ./media/resource-group-create-service-principal-portal/active-directory-details.png
[3]: ./media/resource-group-create-service-principal-portal/application-configure.png
[4]: ./media/resource-group-create-service-principal-portal/app-properties.png
[5]: ./media/resource-group-create-service-principal-portal/client-id.png
[6]: ./media/resource-group-create-service-principal-portal/create-application.png
[7]: ./media/resource-group-create-service-principal-portal/create-key.png
[8]: ./media/resource-group-create-service-principal-portal/save-key.png
[9]: ./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png
[10]: ./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png
[11]: ./media/resource-group-create-service-principal-portal/view-applications.png
[12]: ./media/resource-group-create-service-principal-portal/add-icon.png
[13]: ./media/resource-group-create-service-principal-portal/save-icon.png

