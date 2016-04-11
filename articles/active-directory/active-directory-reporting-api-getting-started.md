<properties
   pageTitle="Azure AD Reporting API の概要"
   description="Azure Active Directory Reporting API の概要について説明します。"
   services="active-directory"
   documentationCenter=""
   authors="kenhoff"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/07/2015"
   ms.author="kenhoff"/>


# Azure AD Reporting API の概要

*このドキュメントの一部である、 [Azure Active Directory のレポート作成ガイド](active-directory-reporting-guide.md)します。*

Azure Active Directory には、アクティビティ、セキュリティ、監査に関するさまざまなレポートがあります。 このデータは、Azure ポータルで使用できますが、SIEM システム、監査、ビジネス インテリジェンス ツールなど、他の多くのアプリケーションでも非常に役に立ちます。

Azure AD Reporting API を使用すると、さまざまなプログラミング言語とツールから呼び出すことができる REST ベースの API セットにより、これらのデータにプログラムからアクセスできます。

この記事では、PowerShell を使用して Azure AD Reporting API を呼び出す手順について説明します。 必要に応じて、サンプルの PowerShell スクリプトを変更し、使用可能な任意のレポートから、JSON、XML、またはテキストの形式でデータにアクセスできます。

このサンプルを使用する必要があります、 [Azure Active Directory](active-directory-whatis.md)

## API にアクセスする Azure AD アプリケーションの作成

Reporting API を使用して [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) web Api へのアクセスを承認します。 ディレクトリから情報にアクセスするには、Active Directory でアプリケーションを作成し、AAD データにアクセスするための適切なアクセス許可を付与する必要があります。


### アプリケーションの作成
- 移動し、 [Azure 管理ポータル](https://manage.windowsazure.com/)します。
- ディレクトリに移動します。
- アプリケーションに移動します。
- 下のバーで [追加] をクリックします。
    - [組織で開発中のアプリケーションを追加] をクリックします。
    - **名前**: 任意の名前は問題ありません。 「Reporting API アプリケーション」のような名前をお勧めします。
    - **型**:"Web アプリケーションや Web API] を選択します。
    - 矢印をクリックして次のページに進みます。
    - **サインオン URL**: ```http://localhost```です。
    - **アプリケーション ID URI**: ```http://localhost```です。
    - チェック マークをクリックして、アプリケーションの追加を完了します。

### API を使用するためのアクセス許可をアプリケーションに付与します。
- [アプリケーション] タブに移動します。
- 新しく作成したアプリケーションに移動します。
- クリックして、 **構成** ] タブをクリックします。
- [その他のアプリケーションに対するアクセス許可] セクションで、次の手順に従って設定します。
    - Microsoft Azure Active Directory > アプリケーションのアクセス許可を選択 **ディレクトリ データを読み取る**します。
- クリックして **保存** 下部のバーにします。


### ディレクトリ ID、クライアント ID、クライアント シークレットの取得

以下の手順では、アプリケーションのクライアント ID とクライアント シークレットを取得する方法を説明します。  また、テナントの名前を知る必要があります。これは、*.onmicrosoft.com またはカスタム ドメイン名です。  これらを別の場所にコピーします。スクリプトを変更するときに使用します。

#### アプリケーション クライアント ID
- [アプリケーション] タブに移動します。
- 新しく作成したアプリケーションに移動します。
- 移動し、 **構成** ] タブをクリックします。
- ID が記載されているアプリケーションのクライアント、 **クライアント ID** フィールドです。

#### アプリケーション クライアント シークレット
- [アプリケーション] タブに移動します。
- 新しく作成したアプリケーションに移動します。
- [構成] タブに移動します。
- [キー] セクションで期間を選択して、アプリケーションの新しいシークレット キーを生成します。
- このキーは保存時に表示されます。 後で取得する方法がないため、忘れずにそれをコピーして安全な場所に貼り付けておいてください。


## スクリプトの変更
次のスクリプトのいずれかで、$ClientID、$ClientSecret、$tenantdomain を「Delegating Access in Azure AD (Azure AD でのアクセスの委任)」の説明に従って正しい値に置き換えることで、ディレクトリで動作するように編集します。

### PowerShell スクリプト

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "your-application-client-id-here"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "your-application-client-secret-here"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net"
    $tenantdomain   = "your-directory-name-here.onmicrosoft.com"            # For example, contoso.onmicrosoft.com

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.windows.net/$tenantdomain/reports/auditEvents?api-version=beta&`$filter=eventTime gt $7daysago"

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }
        $myReport.Content | Out-File -FilePath auditEvents.json -Force
    } else {
        Write-Host "ERROR: No Access Token"
    }

### Bash スクリプト

    #!/bin/bash

    # Author: Ken Hoff (kenhoff@microsoft.com)
    # Date: 2015.08.20
    # NOTE: This script requires jq (https://stedolan.github.io/jq/)

    CLIENT_ID="your-application-client-id-here"         # Should be a ~35 character string insert your info here
    CLIENT_SECRET="your-application-client-secret-here" # Should be a ~44 character string insert your info here
    LOGIN_URL="https://login.windows.net"
    TENANT_DOMAIN="your-directory-name-here.onmicrosoft.com"    # For example, contoso.onmicrosoft.com

    TOKEN_INFO=$(curl -s --data-urlencode "grant_type=client_credentials" --data-urlencode "client_id=$CLIENT_ID" --data-urlencode "client_secret=$CLIENT_SECRET" "$LOGIN_URL/$TENANT_DOMAIN/oauth2/token?api-version=1.0")

    TOKEN_TYPE=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.token_type')
    ACCESS_TOKEN=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.access_token')

    # get yesterday's date

    YESTERDAY=$(date --date='1 day ago' +'%Y-%m-%d')

    URL="https://graph.windows.net/$TENANT_DOMAIN/reports/auditEvents?api-version=beta&\$filter=eventTime%20gt%20$YESTERDAY"


    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" $URL)

    echo $REPORT | ./jq-win64.exe -r '.value' | ./jq-win64.exe -r ".[]"





## スクリプトの実行
スクリプトの編集が完了したら、実行して、AuditEvents レポートから予期したデータが返されることを確認します。

スクリプトは、使用可能なすべてのレポートの一覧、および AccountProvisioningEvents レポートからの出力を、PowerShell ウィンドウに JSON 形式で返します。 また、同じ出力を JSON、テキスト、XML でファイルに作成します。 他のレポートからデータを返すようにスクリプトを変更してテストしたり、必要のない出力形式をコメント化したりできます。

## メモ

- Azure AD Reporting API によって (OData 改ページ調整を使用) 返されるイベントの数に制限はありません。　
    - レポート データの保有期間の制限についてチェック アウト [保持ポリシーのレポート作成](active-directory-reporting-retention.md)します。


## 次のステップ
- 使用可能なセキュリティ、監査、およびアクティビティ レポートに興味がある場合は、 チェック アウト [Azure AD のセキュリティ、監査、およびアクティビティ レポート](active-directory-view-access-usage-reports.md)
- 参照してください [Azure AD の監査レポート イベント](active-directory-reporting-audit-events.md) 監査レポートの詳細については
- 参照してください [Azure AD のレポートおよびイベント (プレビュー)](https://msdn.microsoft.com/library/azure/mt126081.aspx) Graph API REST サービスの詳細については

