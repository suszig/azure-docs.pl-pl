
<properties 
   pageTitle="Azure クラシック ポータルを使用したインターネットに接続するクラシック デプロイ モデルのロード バランサーの作成の開始 | Microsoft Azure"
   description="Azure クラシック ポータルを使用し、インターネットに接続するクラシック デプロイ モデルのロード バランサーを作成する方法について説明します"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/07/2015"
   ms.author="joaoma" />

# Azure クラシック ポータルを使用したインターネットに接続するロード バランサー (クラシック) の作成の開始

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、従来のデプロイ モデルについて説明します。 こともできます [インターネットへの Azure リソース マネージャーを使用してロード バランサーを作成する方法について](load-balancer-get-started-internet-arm-ps.md)します。

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## インターネットに接続するロード バランサーを仮想マシンに設定する

クラウド サービスの仮想マシン全体でインターネットからのネットワーク トラフィックを負荷分散するには、負荷分散セットを作成する必要があります。 この手順では、既に仮想マシンが作成されていて、これらがすべて同じクラウド サービス内にあることを前提としています。

**仮想マシンの負荷分散セットを構成するには**

1. Azure クラシック ポータルで、クリックして **仮想マシン**, 、負荷分散セット内の仮想マシンの名前をクリックします。
2.  クリックして **エンドポイント**, 、] をクリックし、 **追加**します。

4.   **仮想マシンにエンドポイントを追加** ] ページで、右矢印をクリックします。

4.   **エンドポイントの詳細を指定する** ページ。
    -  **名**, 、エンドポイントの名前を入力、または一般的なプロトコル用の定義済みのエンドポイントの一覧から名前を選択します。
    -   **プロトコル**, 、必要に応じて、エンドポイント、TCP または UDP のいずれかの種類で必要なプロトコルを選択します。
    -   **パブリック ポートとプライベート ポート**, 、必要に応じて、使用するには、仮想マシンが必要なポート番号を入力します。 仮想マシンのプライベート ポートとファイアウォール ルールを使って、アプリケーションに適した方法でトラフィックをリダイレクトすることができます。 プライベート ポートはパブリック ポートと同じにできます。 たとえば、Web (HTTP) トラフィック用のエンドポイントの場合、パブリック ポートとプライベート ポートの両方にポート 80 を割り当てることができます。

5.  選択 **負荷分散セットの作成**, 、右矢印をクリックします。

6.   **負荷分散セットを構成する** ] ページで、負荷分散セットの名前を入力して、Azure ロード バランサーのプローブ動作の値を割り当てます。
Load Balancer はプローブを使用して、負荷分散セット内の仮想マシンが着信トラフィックを受信できるかどうかを判断します。

7.  チェック マークをクリックして、負荷分散されたエンドポイントを作成します。 表示されます **はい** で、 **負荷分散セットの名前** の列、 **エンドポイント** 仮想マシンのページです。

8.  ポータルで、次のようにクリックします。 **仮想マシン**, を負荷分散セット内のその他の仮想マシンの名前をクリックし、[] をクリック **エンドポイント**, 、] をクリックし、 **追加**します。

9.   **仮想マシンにエンドポイントを追加** ] ページで [ **既存の負荷分散セットにエンドポイントを追加**, を負荷分散セットの名前を選択して、右矢印をクリックします。

10.  **エンドポイントの詳細を指定する** ] ページで、エンドポイントの名前を入力してチェック マークをクリックします。
負荷分散セットに仮想マシンを追加する場合は、手順 8. ～ 10. を繰り返します。



## 次のステップ

[内部ロード バランサーの構成の開始](load-balancer-internal-getstarted.md)

[ロード バランサー分散モードの構成](load-balancer-distribution-mode.md)

[ロード バランサーのアイドル TCP タイムアウト設定の構成](load-balancer-tcp-idle-timeout.md)


