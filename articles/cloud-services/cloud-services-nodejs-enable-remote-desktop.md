<properties 
    pageTitle="クラウド サービスのリモート デスクトップの有効化 (Node.js)" 
    description="Azure Node.js アプリケーションをホストする仮想マシンについてリモート デスクトップ アクセスを有効にする方法を説明します。" 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/20/2015" 
    ms.author="robmcm"/>



# Azure でのリモート デスクトップの有効化

リモート デスクトップを使用して、Azure で実行されているロール インスタンスのデスクトップにアクセスできます
Azure で実行するアプリケーションをエミュレートします。 リモート デスクトップ接続を使用して
仮想マシンの構成やアプリケーションの問題のトラブルシューティングを行うことができます
想定しています。
> [AZURE.NOTE] この記事は、Azure クラウド サービスとしてホストされる Node.js アプリケーションに適用されます。


## 前提条件

- インストールし、構成 [Azure Powershell](../install-configure-powershell.md)します。
- Azure クラウド サービスへの Node.js アプリのデプロイ。 詳細については、次を参照してください。 [の構築と Azure クラウド サービスへの Node.js アプリケーションをデプロイ](cloud-services-nodejs-develop-deploy-app.md)します。


## 手順 1. Azure PowerShell を使用して、リモート デスクトップ アクセス用にサービスを構成する

リモート デスクトップを使用するのには、Azure サービス定義を更新する必要があり、
ユーザー名、パスワード、および証明書を構成します。

アプリのソース ファイルが含まれているコンピューターから次の手順を実行します。

1. **Azure PowerShell** を管理者として実行します。 (**[スタート] メニュー**または**スタート画面**で、**Azure PowerShell** を検索します。)

2.  サービス定義 (.csdef) を含むディレクトリに移動し、
サービス構成 (.cscfg) ファイルです。

3. 次の PowerShell コマンドレットに入ります。

        Enable-AzureServiceProjectRemoteDesktop

4. プロンプトに、ユーザー名とパスワードを入力します。

    ![Enable-AzureServiceProjectRemoteDesktop][enable-rdp]

3.  次の PowerShell コマンドレットに入り、変更を発行します。

        Publish-AzureServiceProject

    ![Publish-AzureServiceProject][publish-project]

## 手順 2. ロール インスタンスに接続する

サービス定義の更新を発行した後に接続できます。
。

1.  [Azure クラシック ポータル]、[選択 **クラウド サービス** し、サービスを選択します。

    ![Azure クラシック ポータル][cloud-services]

2.  **[インスタンス]** をクリックし、**[運用]** または **[ステージング]** をクリックしてサービスのインスタンスを表示します。 インスタンスを選択し、ページの下部にある **[接続]** をクリックします。

    ![[インスタンス] ページ][3]

2.  クリックすると **接続**, 、web ブラウザーでは、保存するように要求します。
    。 このファイルを開きます。 (たとえば、Internet Explorer を使用している場合は、**[開く]** をクリックします。)

    ![.rdp ファイルを開くまたは保存することを促すメッセージ][4]

3.  ファイルが開くと、セキュリティに関する次のメッセージが表示されます。

    ![Windows のセキュリティに関するメッセージ][5]

4.  クリックして **接続**, と入力するため、セキュリティのプロンプトが表示されます
    。 これに対して、
    [手順 1. ][step 1: configure the service for remote desktop access using azure powershell], 、] をクリックし、 **[ok]**します。

    ![ユーザー名/パスワードの入力を求めるメッセージ][6]

接続すると、リモート デスクトップ接続に、
Azure 内のインスタンスのデスクトップが表示されます。

![リモート デスクトップ セッション][7]

## 手順 3. リモート デスクトップ アクセスが無効になるようにサービスを構成する

クラウドのロール インスタンスに対するリモート デスクトップ接続が不要になった場合は、
クラウド内のインスタンスは、[Azure PowerShell] を使用して、リモート デスクトップ アクセスを無効にします。

1.  次の PowerShell コマンドレットに入ります。

        Disable-AzureServiceProjectRemoteDesktop

2.  次の PowerShell コマンドレットに入り、変更を発行します。

        Publish-AzureServiceProject


## その他のリソース

- [Azure のロール インスタンスにリモートでアクセスする]
- [Azure ロールでリモート デスクトップを使用する]
- [Node.js デベロッパー センター](/develop/nodejs/)


[azure powershell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409 
[azure classic portal]: http://manage.windowsazure.com 
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png 
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png 
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png 
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png 
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png 
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png 
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png 
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png 
[remotely accessing role instances in azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx 
[using remote desktop with azure roles]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx 

