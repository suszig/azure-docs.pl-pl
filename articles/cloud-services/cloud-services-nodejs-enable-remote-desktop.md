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

1. 実行 **Azure PowerShell** 管理者として。 (から、 **[スタート] メニュー** または **のスタート画面で**, 、検索 **Azure PowerShell**.)

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

1.   [Azure classic portal], [ **クラウド サービス** し、サービスを選択します。

    ![Azure クラシック ポータル][cloud-services]

2.  をクリックして **インスタンス**, 、順にクリック **運用** または **ステージング** 、サービスのインスタンスを表示します。 インスタンスを選択し、クリックして **接続** ページの下部にあります。

    ![[インスタンス] ページ][3]

2.  クリックすると **接続**, 、web ブラウザーでは、保存するように要求します。
    。 このファイルを開きます。 (たとえば、Internet Explorer を使用している場合は、クリックして **開いている**.)

    ![.rdp ファイルを開くまたは保存することを促すメッセージ][4]

3.  ファイルが開くと、セキュリティに関する次のメッセージが表示されます。

    ![Windows のセキュリティに関するメッセージ][5]

4.  クリックして **接続**, と入力するため、セキュリティのプロンプトが表示されます
    。 これに対して、
    [手順 1.] で [手順 1: Azure PowerShell を使用してリモート デスクトップ アクセス用にサービスを構成する] を順にクリック **[ok]**します。

    ![ユーザー名/パスワードの入力を求めるメッセージ][6]

接続すると、リモート デスクトップ接続に、
Azure 内のインスタンスのデスクトップが表示されます。 

![リモート デスクトップ セッション][7]

## 手順 3. リモート デスクトップ アクセスが無効になるようにサービスを構成する 

クラウドのロール インスタンスに対するリモート デスクトップ接続が不要になった場合は、
リモート デスクトップ アクセスを使用して、クラウド内のインスタンスが無効にする [Azure PowerShell]します。

1.  次の PowerShell コマンドレットに入ります。

        Disable-AzureServiceProjectRemoteDesktop

2.  次の PowerShell コマンドレットに入り、変更を発行します。

        Publish-AzureServiceProject

## その他のリソース

- [Microsoft Azure ロールのリモート デスクトップ接続をセットアップする] 
- [Azure ロールでのリモート デスクトップの使用]
- [Node.js デベロッパー センター](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure classic portal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
  [3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
  [4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
  [5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
  [6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
  [7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
  [Remotely Accessing Role Instances in Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
  [Using Remote Desktop with Azure Roles]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 
