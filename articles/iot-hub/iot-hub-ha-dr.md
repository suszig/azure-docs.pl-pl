<properties
 pageTitle="IoT Hub の HA および DR | Microsoft Azure"
 description="障害復旧機能を持つ高可用性ソリューションの構築を支援する機能について説明します。"
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/02/2015"
 ms.author="elioda"/>


# IoT Hub の高可用性と障害復旧

IoT Hub は、Azure サービスとして、Azure リージョン レベルの冗長性を使用して高可用性 (HA) を提供します。その際、ソリューションによる追加操作は必要ありません。 さらに、Azure には、障害復旧 (DR) 機能または複数のリージョンにわたる可用性を備えたソリューションを、必要に応じて構築するときに役立つ機能が複数用意されています。 複数のリージョンにわたるグローバルな可用性をデバイスまたはユーザーに提供するには、こうした機能を使用できるようにソリューションを設計し、準備する必要があります。 記事 [Azure ビジネス継続性テクニカル ガイダンス:operator[]][] ビジネス継続性と災害復旧に Azure の組み込み機能について説明します。  [災害復旧と高可用性を Azure アプリケーションの][] ホワイト ペーパーでは、HADR を実現するために Azure アプリケーションの方法に関するアーキテクチャのガイダンスを示します。

## IoT Hub での地域フェールオーバー

このセクションでは、IoT ソリューションでのデプロイ トポロジの詳細については取り上げませんが、高可用性と障害復旧を実現するために、*地域フェールオーバー* デプロイ モデルについて検討します。

地域的なフェールオーバー モデルでは、ソリューションのバック エンドが 1 つのデータ センターの場所で、主に実行されているが、IoT hub プライマリ データ センターで障害が低下したり、デバイスから、プライマリ データ センターへのネットワーク接続が何らかの理由で中断された場合にフェールオーバー用に、追加のデータ センターのリージョンに配置する追加の IoT hub とバック エンド。 プライマリ ゲートウェイにアクセスできない場合は必ず、セカンダリ サービス エンドポイントがデバイスによって使用されます。 複数のリージョンにまたがるフェールオーバー機能を使用すると、ソリューションの可用性は、1 つのリージョンの高可用性を超えて向上させることができます。

大まかに言えば、IoT Hub で地域フェールオーバー モデルを実装するには、以下が必要です。

* **セカンダリ IoT Hub とデバイス ルーティングのロジック**: プライマリ リージョンでサービスの中断が発生した場合、セカンダリ リージョンへのデバイスの接続を開始する必要があります。 関連するサービスのほとんどが状態認識型の場合、ソリューションの管理者がリージョン間のフェールオーバー プロセスをトリガーするのはよくあることです。 プロセスの制御を維持しながら、新しいエンドポイントをデバイスに伝達する最善の方法は、*コンシェルジェ* サービスで、現在のアクティブなエンドポイント サービスがないかどうかを定期的にチェックすることです。 コンシェルジェ サービスは到達可能な保持され、レプリケートされる単純な web アプリケーションであることができます DNS リダイレクト手法を使用して (を使用するなど、 [Azure Traffic Manager の][])。
* **ID レジストリ レプリケーション**: 使用するには、ソリューションに接続できるすべてのデバイス ID をセカンダリ IoT Hub に格納する必要があります。 ソリューションでは、デバイス ID の geo レプリケートされたバックアップを保持し、デバイスのアクティブなエンドポイントを切り替える前に、そのバックアップをセカンダリ IoT Hub にアップロードしなければなりません。 IoT Hub のデバイス ID エクスポート機能は、この点で役に立ちます。 詳細については、次を参照してください。 [IoT Hub 開発者ガイド - identity レジストリ []][]します。
* **マージ ロジック**: 再度プライマリ リージョンが使用可能になった時点で、セカンダリ サイトで作成されたすべての状態とデータをプライマリ リージョンに移行する必要があります。 これは主にデバイス ID およびアプリケーション メタデータと関連しており、プライマリ IoT Hub とマージする必要があります。また、プライマリ リージョンにある他のアプリケーション固有のストアとのマージが必要になることもあります。 この手順を簡単にするために、通常は、べき等操作を使用することをお勧めします。 これにより、最終的に一貫性のあるイベント分布だけでなく、イベント重複や順不同配信の副次的な影響を最小限に抑えられます。 また、アプリケーション ロジックは、潜在的な不整合を許容できるように、あるいは「少しばかり」時代遅れに設計してください。 回復ポイントの目標 (RPO) に基づいてシステムが「回復する」ために余計な時間がかかるためです。 次の記事では、このトピックに関するガイダンスを提供します。 [フェール セーフ: 回復力のあるクラウド アーキテクチャ [] に関するガイダンス][]です。

## 次のステップ

Azure IoT Hub についてさらに学習するには、次のリンクを使用してください。

- [IoT Hub (チュートリアル) ][lnk-get-started]
- [Azure の IoT Hub とは何ですか。[]][]


[azure business continuity technical guidance]: https://msdn.microsoft.com/library/azure/hh873027.aspx 
[disaster recovery and high availability for azure applications]: https://msdn.microsoft.com/library/azure/dn251004.aspx 
[failsafe: guidance for resilient cloud architectures]: https://msdn.microsoft.com/library/azure/jj853352.aspx 
[azure traffic manager]: https://azure.microsoft.com/documentation/services/traffic-manager/ 
[iot hub developer guide - identity registry]: iot-hub-devguide.md#identityregistry 
[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md 
[what is azure iot hub?]: iot-hub-what-is-iot-hub.md 

