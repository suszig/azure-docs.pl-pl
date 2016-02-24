<properties 
    pageTitle="Microsoft Azure Management API 証明書をポータルにアップロードする" 
    description="Management API 証明書を Microsoft Azure にアップロードする方法について説明します" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="na" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/01/2015"
    ms.author="adegeo"/>


# Azure Management API Management 証明書のアップロード

Azure のサービス管理 API に対する認証は、管理証明書で行うことができます。 これらの証明書は、各種 Azure サービスの構成とデプロイメントを自動化するために、今後多くのプログラムおよびツール (Visual Studio、Azure SDK など) で使用されることになります。 これらはクラウド サービスには実際には関連がありません。 

>[AZURE.WARNING] ご注意ください。 これらの種類の証明書を使用して認証する場合、関連付けられているサブスクリプションを管理できます。 

Azure の証明書 (自己署名証明書の作成を含む) の詳細については、 [利用可能な](cloud-services/cloud-services-certs-create.md#what-are-management-certificates) にする必要がある場合。

使用することも [Azure Active Directory](http://azure.microsoft.com/documentation/services/active-directory/) 自動化を図るのためのクライアント コードを認証します。

## 管理証明書のアップロード

管理証明書 (公開キーのみを含んだ .cer ファイル) を作成したら、それをポータルにアップロードすることができます。 ポータルで証明書が使用可能な状態になると、対になる証明書 (秘密キー) を持つすべての人が、Management API を経由して接続し、そのサブスクリプションに関連付けられているリソースにアクセスすることができます。

1. ログイン、 [Azure 管理ポータル](http://manage.windowsazure.com)します。
2. クリックして **設定** (下にスクロールする必要があります)、ポータルの左側にあります。 
    
    ![設定](./media/azure-api-management-certs/settings.png)

3. クリックして、 **管理証明書** ] タブをクリックします。

    ![設定](./media/azure-api-management-certs/certificates-tab.png)
    
4. クリックして、 **アップロード** ] ボタンをクリックします。

    ![設定](./media/azure-api-management-certs/upload.png)
    
5. ダイアログの情報を入力し、確認のため **チェック マーク**します。

    ![設定](./media/azure-api-management-certs/upload-dialog.png)

## 次のステップ

これで、サブスクリプションに関連付けられている管理証明書がある場合は、接続できること (後のローカルでの一致する証明書をインストールしておく) プログラムに、 [サービス管理 REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) し、またそのサブスクリプションに関連付けられているさまざまな Azure リソースを自動化します。 
