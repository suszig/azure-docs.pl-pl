<properties 
   pageTitle="パフォーマンスによるトラフィック ルーティング方法の構成 | Microsoft Azure"
   description="この記事では、Traffic Manager でパフォーマンスによるトラフィック ルーティング方法を構成する方法について説明します。"
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/09/2015"
   ms.author="joaoma" />

# パフォーマンスによるトラフィック ルーティング方法の構成

世界各地 (「リージョン」と呼びます) に配置されたさまざまなデータセンターにあるクラウド サービスと Web サイト (エンドポイント) に対するトラフィック ルーティングを行うために、要求元のクライアントから最も待機時間の短いエンドポイントに受信トラフィックを転送することができます。 通常、待機時間が最も低いデータセンターは、地理的に最も近いエンドポイントと一致します。 パフォーマンスによるトラフィック ルーティング方法を選択すると、トラフィックを最も短い待機時間に基づいて振り分けることができますが、振り分け時にネットワークの構成や負荷のリアルタイムの変化を考慮することはできません。 さまざまなトラフィック ルーティング方法の詳細について、Azure Traffic Manager は、ses [Traffic Manager に関するトラフィック ルーティング方法](traffic-manager-load-balancing-methods.md)します。

## 一連のエンドポイント間で、最も短い待機時間に基づいてトラフィック ルーティングを行う

1. 左側のウィンドウでは、Azure クラシック ポータルで、クリックして、 **Traffic Manager** Traffic Manager] ウィンドウを開くにはアイコン。 Traffic Manager プロファイルを作成がない場合は、次を参照してください。 [Traffic Manager プロファイルの管理](traffic-manager-manage-profiles.md) 手順については、基本的な Traffic Manager プロファイルを作成します。
2. Azure クラシック ポータルの Traffic Manager ウィンドウで、変更対象のエンドポイント設定が保存されている Traffic Manager プロファイルを見つけて、そのプロファイル名の右側にある矢印をクリックします。 これにより、プロファイルの設定ページが開きます。
3. [プロファイルのページで、次のようにクリックします。 **エンドポイント** 、ページの上部にあるし、構成に含めるサービス エンドポイントが存在することを確認します。 追加またはプロファイルからエンドポイントを削除する手順については、次を参照してください。 [Traffic Manager でのエンドポイントの管理](traffic-manager-endpoints.md)します。
4. [プロファイルのページで、次のようにクリックします。 **構成** を開くには、構成ページの上部にあります。
5.  **トラフィックのルーティング方法の設定**, 、トラフィック ルーティング方法があることを確認 **パフォーマンス*します。 そうでない場合はクリックして **パフォーマンス** ドロップダウン リストにします。
6. いることを確認、 **監視の設定** 適切に構成されています。 監視を構成することで、オフラインになっているエンドポイントにトラフィックが送信されなくなります。 エンドポイントを監視するには、パスとファイル名を指定する必要があります。 スラッシュ "/" は、相対パスの有効な入力値で、ファイルがルート ディレクトリ (既定のディレクトリ) にあることを意味します。 監視の詳細については、次を参照してください。 [Traffic Manager の監視について](traffic-manager-monitoring.md)します。
7. 構成の変更が完了したら、クリックして **保存** ページの下部にあります。
8. 構成の変更をテストします。 詳細については、次を参照してください。 [Traffic Manager の設定のテスト](traffic-manager-testing-settings.md)します。
9. Traffic Manager プロファイルが設定されて機能したら、権限のある DNS サーバー上の DNS レコードを編集して、会社のドメイン名が Traffic Manager ドメイン名を参照するようにします。 これを行う方法の詳細については、次を参照してください。 [Traffic Manager ドメインを会社のインターネット ドメインをポイント](traffic-manager-point-internet-domain.md)します。

## 次のステップ


[会社のインターネット ドメインで Traffic Manager ドメインが参照されるようにする](traffic-manager-point-internet-domain.md)

[Traffic Manager のルーティング方法](traffic-manager-routing-methods.md)

[フェールオーバーのルーティング方法の構成](traffic-manager-configure-failover-routing-method.md)

[ラウンド ロビンによるトラフィック ルーティング方法の構成](traffic-manager-configure-round-robin-routing-method.md)

[Traffic Manager の機能低下状態のトラブルシューティング](traffic-manager-troubleshooting-degraded.md)

[Traffic Manager  - プロファイルの無効化、有効化、または削除](disable-enable-or-delete-a-profile.md)

[Traffic Manager - エンドポイントの無効化または有効化](disable-or-enable-an-endpoint.md)
 

