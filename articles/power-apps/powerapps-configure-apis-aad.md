<properties
    pageTitle="PowerApps で Azure Active Directory ドメイン上のバックエンド システムに接続するために API を構成する | Microsoft Azure"
    description="PowerApps で AAD により保護されたバックエンド システムに接続するために API を構成する"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="dwrede"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="guayan"/>

# Azure Active Directory ドメイン上のバックエンド リソースに接続するために API を構成する
Azure Active Directory (AAD) 上にドメインを作成するユーザーが増えているため、バックエンド リソースも、そのような AAD ドメインに追加されつつあります。 これらのバックエンド リソースに接続できるように、API を作成し構成することができます。 

#### 開始のための前提条件

- サインアップ [PowerApps エンタープライズ](powerapps-get-started-azure-portal.md)します。
- 作成、 [app service 環境](powerapps-get-started-azure-portal.md)します。
- [Azure PowerShell] のインストール [11] 1.0 プレビュー以降。
- 内の API を登録、 [app service 環境](powerapps-register-api-hosted-in-app-service.md)します。

## 手順 1: Active Directory アプリケーションを作成し、アクセス許可を付与する

AAD ドメイン上のバックエンド システムにアクセスするには、AAD アプリケーションを作成し、既存のバックエンド (AAD アプリケーションでもある) への適切なアクセス許可を付与します。 手順:

1. [Azure クラシック ポータル] で [13] 移動、Azure Active Directory を開き、テナント (またはディレクトリ) を選択、 **アプリケーション** ] タブをクリックします。  
![][14]
2. 選択、 **追加** 下部にあるボタンをクリックします。 次の操作を行います。  

    a) を選択 **[組織が開発中のアプリケーションを追加**します。  
    b)、アプリケーションの名前を入力し、選択 **Web アプリケーションまたは web API**します。  
    c) **サインオン URL** と **App ID URI**, 、AAD 内で一意の Url と、組織にとって意味のある Url を入力します。 たとえば、http://powerappssignon.contoso.com または http://powerappsappid.contoso.com を入力することができます。  組織の AAD ドメイン内の URL を使用することをお勧めします。 URL は識別子として使用され、存在する上で必要な要件はありません。 入力した URL は誰からも参照されません。 HTTP または HTTPS を入力することができます。  

3. 新しく作成した AAD アプリケーションのページに移動、 **構成** ] タブをクリックします。  
![][15]
4.  **キー** セクションで、ドロップダウン リストを使用して期間を選択します。 選択した後に、キーを表示することに注意してください **保存**:  
![][16]
5.  **シングル サインオン**, 、追加 ``https://<your App Service Environment name>.azure-apim.net:456/redirect`` として、 **応答 URL**します。
6.  **他のアプリケーションに対するアクセス許可**:  

    a) 選択 **アプリケーション追加**します。 ポップアップ ウィンドウで、既存のバックエンドをセキュリティで保護する AAD アプリケーションを選択します。  
    ![][17]  

    b) アクセス許可を追加するのにドロップダウン リストを使用します。  
    ![][18]

7. 選択 **保存** 下部にあります。 
8. コピー、 **クライアント ID** と **キー** し、保存します。 Azure ポータルを閉じると、キーは二度と表示されません。 

参照してください [と Azure Active Directory アプリケーションを統合する](../active-directory-integrating-applications.md) を AAD アプリケーションの詳細を参照してください。 

## 手順 2: Azure PowerShell を使用して API を構成する

この時点で、API に必要な構成を初期化することは Azure ポータルでサポートされていません。 Azure ポータルで API を構成するには、次の Auzre PowerShell スクリプトを使用します。 

> [AZURE.TIP] インストール、構成、および Azure PowerShell を実行する方法については、[Azure PowerShell を構成する方法] を参照してください。 [11] です。 次のスクリプトは、Azure PowerShell 1.0 プレビュー以上で機能します。

```powershell
# get the API resource
$api = Get-AzureRmResource -ResourceType Microsoft.Web/apiManagementAccounts/apis -ResourceName <App Service Environment name>/<API name> -ResourceGroupName <resource group name>

# configure the API resource for AAD authentication
$connectionParameters = @{
    token = @{
        type = "oauthSetting";
        oAuthSettings = @{
            identityProvider = "aad";
            clientId = "<your AAD app client id>";
            clientSecret = "<your AAD app key>";
            customParameters = @{
                TenantId = @{ # this property is optional
                    value = "<your AAD tenant ID>"
                };
                ResourceUri = @{ # this property is required
                    value = "<the app ID URI of the AAD app protecting your backend>"
                }
            }
        }
    }
}
Add-Member -InputObject $api.Properties -MemberType NoteProperty -Name ConnectionParameters -Value $connectionParameters -Force

# update the API resource
New-AzureRmResource -Location $api.Location -ResourceId $api.ResourceId -Properties $api.Properties
```

**通知** を **トークン** 接続パラメーターの名前は重要です。 Camel ケースである限り、独自の名前を選択できます。 この名前は後で、バックエンド コードまたは API ポリシーの中で使用することになります。

次に、[Azure ポータル] に移動 [19] に移動し、 **全般** API の設定] ブレードです。 追加の構成オプションが表示されます。  
![][21]


## 実際に使ってみる

PowerApps でアプリを開きます。  **利用可能な接続**, 、新しい API が一覧表示します。 選択すると **接続**, 、AAD のサインイン ウィンドウが表示されます。 組織の AAD アカウントの詳細を入力して、接続を作成します。

バックエンドがでユーザーの AAD トークンを受け取ったランタイム呼び出しが行われたときに、アプリケーションから API をこの接続を使用して、これで、 **x ms apim トークン** 次 [base 64 エンコード] [20] の形式で HTTP ヘッダー。  

```json
{
  "token": {
    "AccessToken": ""
    // ...
  }
}
```

**通知** をプロパティ名 **トークン** 設定を構成するときに使用する接続パラメーター名と一致します。

バックエンド コードできますし、トークンの取得、AAD から、 **AccessToken** プロパティして必要な場合に使用することです。 トークンはアプリ サービス環境によって自動的に更新されます。

## API ポリシーの構成

標準の HTTP に、その AAD トークンを設定する API のポリシーも使用する必要に応じて、 **承認** ヘッダー。 このように、バックエンド コードで AAD トークンを使用する必要がある場合は、カスタム HTTP ヘッダーを検索するのでなく標準的な方法で AAD トークンを取得し、Base64 デコードを実行することができます。 これを行うには、Azure ポータルに移動して、 **ポリシー** ブレードの API、および次のポリシーを設定します。  

```xml
<policies>
    <inbound>
        <base/>
        <choose>
            <when condition="@(context.Variables.ContainsKey(&quot;tokens&quot;) &amp;&amp; ((JObject)context.Variables[&quot;tokens&quot;])[&quot;token&quot;] != null &amp;&amp; !String.IsNullOrEmpty((string)((JObject)context.Variables[&quot;tokens&quot;])[&quot;token&quot;][&quot;AccessToken&quot;]))">
                <set-header exists-action="override" name="Authorization">
                    <value>@("Bearer " + (string)((JObject)context.Variables["tokens"])[&quot;token&quot;]["AccessToken"])</value>
                </set-header>
            </when>
        </choose>
    </inbound>
    <backend>
        <base/>
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>
```

このポリシーを見ると、基本的にできるの値は、参照、 **x ms apim トークン** ヘッダーを使用してデコードされた JObject として、 **トークン** 変数です。 使用することができます、 **ヘッダーを設定する** 実際 AAD トークンを取得しに設定するポリシー、 **承認** ヘッダー。 これは、同じポリシーを使用して [Azure API Management](https://azure.microsoft.com/services/api-management/)します。 詳細については、次を参照してください。 [Azure API Management のポリシー](../api-management-howto-policies.md)します。

**通知** をプロパティ名 **トークン** 設定を構成するときに使用した接続のパラメーター名と一致します。

## まとめと次のステップ

このトピックでは、Azure Active Directory ドメイン上のバックエンド リソースに接続 (および認証) するために API を構成する方法について見てきました。 PowerApps の詳細については、次の関連するトピックやリソースも参照してください。

- [PowerApps 用 API の開発](powerapps-develop-api.md)


<!--References-->
[11]: ../powershell-install-configure.md
[13]: https://manage.windowsazure.com
[14]: ./media/powerapps-configure-apis-aad/aad-applications-tab.png
[15]: ./media/powerapps-configure-apis-aad/aad-application-configure-tab.png
[16]: ./media/powerapps-configure-apis-aad/aad-application-configure-keys.png
[17]: ./media/powerapps-configure-apis-aad/aad-application-add-other-application.png
[18]: ./media/powerapps-configure-apis-aad/aad-application-add-permissions.png
[19]: https://portal.azure.com
[20]: https://tools.ietf.org/html/rfc4648
[21]: ./media/powerapps-configure-apis-aad/api-settings-aad.png

