<properties 
    pageTitle="クラウド サービス デプロイのステージング (Node.js) | Microsoft Azure" 
    description="Azure アプリケーションをステージング環境にデプロイしてから、仮想 IP (VIP) スワップを使用して運用環境にデプロイする方法について説明します。" 
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



# Azure アプリケーションのステージング

パッケージ アプリケーションは、
インターネット上でアクセスできる運用環境に移行する前に、
Azure でステージング環境にデプロイしてテストすることができます。 この
運用環境とほぼ同じです。唯一の違いは、
ステージングされたアプリケーションには、Azure で生成されたわかりにくい URL を使用しなければアクセスできない点です
。 アプリケーションが正常に動作することを確認したら、
仮想 IP (VIP) スワップを実行して、
運用環境にデプロイできます。

> [AZURE.NOTE] この記事の手順では、Azure クラウド サービスとしてホストされるノード アプリケーションにのみ適用されます。

## 手順 1. アプリケーションをステージングする

このタスクを使用してアプリケーションをステージングする方法では、 **Microsoft
Azure PowerShell**します。

1.  サービスを発行するときに単純に渡す、 **-スロット** パラメーター
     **Publish-AzureServiceProject** コマンドレットです。

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  ログオン、 [Azure classic portal] 選択 **クラウド サービス**します。 クラウド サービスを作成し、 **ステージング** に列の状態が更新されて **を実行している**, 、サービス名をクリックします。

    ![実行中のサービスを表示するポータル][cloud-service]

3.  選択、 **ダッシュ ボード**, 、し、[ **ステージング**します。

    ![クラウド サービス ダッシュボード][cloud-service-dashboard]

4. 値に注意してください、 **サイトの URL** 右に入力します。 DNS 名は、Azure で生成されたわかりにくい内部 ID です。

    ![サイトの URL][cloud-service-staging-url]

これで、ステージング サイトの URL を使用して、アプリケーションがステージング環境で正常に動作していることを確認できます。

## 手順 2. VIP スワップにより運用環境でアプリケーションをアップグレードする

ステージング環境でアップグレードされたバージョンのアプリケーションを検証したら、
ステージング環境と運用環境の仮想 IP (VIP) をスワップするだけで、
そのアプリケーションを運用環境で使用可能にすることができます
。

> [AZURE.NOTE] この手順は、既に展開するいると想定しています、
そのアプリケーションのアップグレードされたバージョンをステージング済みであると想定しています
想定しています。

1.  ログイン、 [Azure classic portal], 、クリックして **クラウド サービス** サービス名を選択します。

2.   **ダッシュ ボード**, [ **ステージング**, 、] をクリックし、 **スワップ** ページの下部にあります。 これにより、[VIP のスワップ] ダイアログが開きます。

    ![VIP のスワップ ダイアログ][vip-swap-dialog]

3.  情報を確認し、 **OK**します。 ステージング環境のデプロイメントが運用環境に切り替わるとき、および運用環境のデプロイメントがステージングに 切り替わるときに、これら 2 つのデプロイメントで更新が開始されます。

正常にデプロイメントをステージングし、
ステージング環境のデプロイで VIP をスワップすることによって運用環境のデプロイをアップグレードしました。

## その他のリソース

- [Azure の VIP スワップによってサービス アップグレードを運用環境にデプロイする方法]

[Azure classic portal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[How to Deploy a Service Upgrade to Production by Swapping VIPs in Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production

