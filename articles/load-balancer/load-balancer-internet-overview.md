<properties 
   pageTitle ="インターネットへのロード バランサーの概要 |Microsoft Azure]
   description ="インターネットへのロード バランサーとその機能の概要です。 ロード バランサーのしくみのバーチャル マシンおよびクラウド サービスを使用して Azure"
   サービス =「ロード バランサー」
   documentationCenter ="na"
   authors ="joaoma"
   manager="adinah"
   エディター"tysonn"=/>
<tags 
   ms.service=「ロード バランサー」
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload=「インフラストラクチャ サービス」
   ms.date="10/16/2015"
   ms.author="joaoma"/>


# 複数の Virtual Machines またはサービス間でインターネットに接続するロード バランサー

エンドポイントの用途の 1 つは、特定の種類の複数の仮想マシンまたはサービス間のトラフィックを配布する、Azure ロード バランサーの構成です。 たとえば、複数の Web サーバーまたは Web ロール間で Web 要求のトラフィックの負荷を分散できます。

Azure ロード バランサーは、プライベート IP アドレスとポート番号の応答トラフィックはその逆の仮想マシンのバーチャル マシンからの着信トラフィックのパブリック IP アドレスとポート番号をマップします。
>[AZURE.NOTE] Azure ロード バランサーは、既定の設定を使用して複数の仮想マシン インスタンス間でハッシュ配布ネットワーク トラフィック (でハッシュ分散方法の詳細について [ロード バランサーの機能](load-balancer-overview.md#load-balancer-features) します。 セッション アフィニティを探している場合はチェック アウト [ロード バランサー分散モード](load-balancer-distribution-mode.md)します。

クラウド サービスに Web ロールまたは Worker ロールのインスタンスが含まれる場合、サービス定義でパブリック エンドポイントを定義できます。

Servicedefinition.csdef ファイルにはエンドポイント構成が含まれ、Web ロールまたは Worker ロールのデプロイメント用の複数のロール インスタンスがある場合は、ロード バランサーをセットアップできます。 インスタンスをクラウドのデプロイメントに追加するには、サービス構成ファイル (.csfg) のインスタンス数を変更します。

次の図は、暗号化された Web トラフィック用の負荷分散されたエンドポイントを示しています。このエンドポイントは、パブリックとプライベートの TCP ポートが 443 である 3 台の仮想マシン間で共有されています。 この 3 台の仮想マシンは、1 つの負荷分散セット内にあります。


![パブリックのロード バランサーの例](./media/load-balancer-internet-overview/IC727496.png))



複数のインターネット クライアントがクラウド サービスのパブリック IP アドレスと TCP ポート 443 に Web ページ要求を送信すると、Azure Load Balancer は、負荷分散セット内の 3 台の仮想マシン間でこれらの要求のハッシュ ベースの負荷分散を行います。 ロード バランサー アルゴリズムについての詳細を取得できます [ロード バランサーの概要] ページ](load-balancer-overview.md#load-balancer-features)します。


## 次のステップ

[内部ロード バランサーの概要](load-balancer-internal-overview.md)

[インターネットへのロード バランサーの構成の開始します。](load-balancer-internet-getstarted.md)

[ロード バランサー分散モードを構成します。](load-balancer-distribution-mode.md)

[ロード バランサーのアイドル TCP タイムアウト設定を構成します。](load-balancer-tcp-idle-timeout.md)








