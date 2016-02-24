<properties 
   pageTitle="Application Gateway の概要 | Microsoft Azure"
   description="このページは、ゲートウェイのサイズ、HTTP の負荷分散、cookie ベースのセッション アフィニティ、SSL オフロードを含む、Application Gateway サービスのレイヤー 7 負荷分散の概要について説明します。"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="carmonm"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="11/09/2015"
   ms.author="joaoma"/>

# Application Gateway とは


Microsoft Azure Application Gateway は、レイヤー 7 の負荷分散に基づく、Azure で管理される HTTP の負荷分散ソリューションを提供します。 

アプリケーションの負荷分散により、IT 管理者および開発者を HTTP に基づくネットワーク トラフィックのルーティング規則を作成できます。  この Application Gateway サービスは可用性が高く、従量課金制で使用できます。 価格と SLA を参照してください、 [SLA](http://azure.microsoft.com/support/legal/sla/) と [価格](https://azure.microsoft.com/pricing/details/application-gateway/) ページです。

Application Gateway は現在、次のレイヤー 7 アプリケーションの配信をサポートします。

- HTTP の負荷分散
- cookie ベースのセッション アフィニティ
- SSL オフロード

![Application Gateway](./media/application-gateway-introduction/appgateway1.png)

## HTTP レイヤー 7 の負荷分散

Azure では、トランスポート レベル (TCP/UDP) で機能する Azure Load Balancer を介してレイヤー 4 の負荷分散を提供し、すべての受信ネットワーク トラフィックを Application Gateway サービスに負荷分散させます。 Application Gateway は、HTTP トラフィックにルーティング ルールを適用して レベル 7 (HTTP) の負荷分散を提供します。 アプリケーション ゲートウェイを作成すると、エンドポイント (VIP) が関連付けられ、受信ネットワーク トラフィック用のパブリック IP として使用されます。

Application Gateway は、仮想マシン、クラウド サービス、Web アプリ、外部 IP アドレスのいずれであるかに関わらず、その構成に基づいて HTTP トラフィックをルーティングします。

次の図に、Application Gateway のトラフィックの流れを示します。

 
![Application Gateway2](./media/application-gateway-introduction/appgateway2.png)

HTTP レイヤー 7 の負荷分散は、次の場合に役立ちます。


- 同じバックエンド VM に到達するために同じユーザー/クライアントのセッションからの要求を必要とするアプリケーション。 この例としては、ショッピング カート アプリや Web メール サーバーなどが挙げられます。
- Web サーバーのファームを SSL 終了オーバーヘッドから解放する必要があるアプリケーション。
- 実行時間の長い同じ TCP 接続で複数の HTTP 要求を異なるバックエンド サーバーにルーティング/負荷分散する必要があるアプリケーション (CDN など)。

## ゲートウェイのサイズとインスタンス

Application Gateway は現在、Small、Medium、および Large の 3 つのサイズで提供されています。 Small サイズのインスタンスは、開発用およびシナリオのテスト用です。 

サブスクリプションごとに最大 50 個のアプリケーション ゲートウェイを作成できるほか、各アプリケーション ゲートウェイには最大 10 個のインスタンスが存在します。 Azure で管理されるサービスとしての Application Gateway の負荷分散により、Azure のソフトウェア ロード バランサーの背後でレイヤー 7 ロード バランサーをプロビジョニングできます。

次の表では、Application Gateway インスタンスごとにパフォーマンス スループットの平均値を示します。


| バック エンド ページの応答 | Small | 中 | Large|
|---|---|---|---|
| 6K | 7.5 mbps | 13 mbps | 50 mbps |
|100k | 35 mbps | 100 mbps| 200 mbps |


>[AZURE.NOTE] これは、アプリケーション ゲートウェイのスループットのおおよそのガイダンスです。 実際のスループットは、ページの平均サイズ、バックエンドのインスタンスの場所、サーバーのページへの処理時間などさまざまな環境の詳細に依存します。

## 正常性の監視
 

Azure Application Gateway はバックエンド インスタンスの状態を 30 秒おきに監視します。 構成されているポートの各インスタンスに HTTP 正常性プローブ要求を送信して *BackendHttpSettings* の構成要素。 正常性プローブは、200 ～ 399 の範囲の応答状態コードで正常な HTTP 応答を予測します。

正常な HTTP 応答が届くと、バックエンド サーバーには「正常」の印が付けられ、引き続き Azure Application Gateway からトラフィックを受信します。 プローブが失敗すると、バック エンド インスタンスは、正常なプールから削除し、このサーバーに送られるトラフィックは停止します。 正常性プローブは引き続き 30 秒おきに失敗したバックエンド インスタンスに送信され、現在の正常性を確認します。 背面の終了時にインスタンスに正常に応答正常性プローブが正常であるバック エンド プールに追加され、トラフィックの送信が再びインスタンスに送られています。

## 構成と管理

REST API や PowerShell コマンドレットを使用して、アプリケーション ゲートウェイを作成および管理できます。



## 次のステップ

Application Gateway を作成します。 参照してください [アプリケーション ゲートウェイの作成](application-gateway-create-gateway.md)します。

SSL オフロードを構成します。 参照してください [SSL オフロードのアプリケーション ゲートウェイの構成](application-gateway-ssl.md)します。



