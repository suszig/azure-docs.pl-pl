<properties
 pageTitle="Azure IoT Hub の概要 | Microsoft Azure"
 description="デバイスの接続、通信パターン、およびサービス支援通信パターンを含む、Azure IoT Hub サービスの概要"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="11/09/2015"
 ms.author="dobett"/>

# Azure IoT Hub とは

Azure IoT Hub へようこそ。 この記事では、Azure IoT Hub の概要と、IoT ソリューションを実装したときに、このサービスを使用する理由について説明します。 

Azure IoT Hub は、何百万もの IoT デバイスとソリューション バック エンド間で、セキュリティで保護された信頼性のある双方向通信を実現する、完全に管理されたサービスです。 Azure IoT Hub の特長は次のとおりです。 

- デバイスとクラウド間の信頼性の高い双方向メッセージングを大規模に提供します。
- デバイスごとのセキュリティ資格情報とアクセス制御を使用して、セキュリティで保護された通信を可能します。
- 最も一般的な言語とプラットフォームのデバイスのライブラリが含まれます。

![クラウド ゲートウェイとしての IoT Hub][img-architecture]

## IoT デバイスの接続の課題

IoT Hub とデバイスのライブラリでは、デバイスをソリューション バック エンドに安定して安全に接続するための課題に対処できます。 IoT デバイスの特性は次のとおりです。

- 多くの場合、人間が操作することのない組み込みシステムです。
- 物理アクセスにコストがかかる遠隔地に配置されている場合があります。
- ソリューション バック エンド経由でしか到達できない場合があります。
- 電源や処理リソースが限られている場合があります。
- ネットワーク接続が断続的に切れたり、遅かったり、高コストである場合があります。
- 専用、カスタム、または業界固有のアプリケーション プロトコルを使用することが必要な場合があります。
- 一般的なハードウェアおよびソフトウェア プラットフォームを多数使用して作成できます。

上記の要件に加え、IoT ソリューションはスケール、セキュリティ、および信頼性を提供する必要もあります。 そのため、一連の接続要件は、Web コンテナーやメッセージング ブローカーなどの従来のテクノロジを使用して実装するのが難しく、時間がかかります。

## Azure IoT Hub を使用する理由

Azure IoT Hub は、次の方法でデバイスの接続の課題に対処します。

-   **デバイスごとの認証およびセキュリティで保護された接続**します。 各デバイスを独自のセキュリティ キーを使用してプロビジョニングして、IoT Hub に接続することができます。  [IoT Hub identity レジストリ][lnk-devguide-identityregistry] ソリューション内のデバイスの id とキーを格納します。  ソリューション バック エンドで個々のデバイスのホワイト リストおよびブラックリストを保持し、デバイスへのアクセスを完全に制御できます。

-   **広範囲にわたるデバイス ライブラリ**します。 Azure IoT デバイス SDK はさまざまな言語とプラットフォーム (多くの Linux ディストリビューション、Windows、および RTOC では C) で使用でき、サポートされています。 Azure IoT デバイス SDK はまた、C#、Java、JavaScript など管理対象言語に対応しています。

-   **IoT プロトコルと拡張**します。 ソリューションでデバイス ライブラリを使用できない場合、IoT Hub は、デバイスで HTTP 1.1 と AMQP 1.0 プロトコルをネイティブに使用できるようにするパブリック プロトコルを公開します。 MQTT v3.1.1 のサポートを提供する IoT Hub を拡張することも、 [Azure IoT プロトコル ゲートウェイ][protocol-gateway] オープン ソース コンポーネントです。 クラウドまたはオンプレミスで Azure IoT プロトコル ゲートウェイを実行し、カスタム プロトコルをサポートするように拡張することができます。

-   **スケール**します。 Azure IoT Hub は、デバイスの数百万単位での同時接続、および毎秒数百万単位のイベントに対応できます。

多くの通信パターンにこれらの利点があります。 現在 IoT Hub では、具体的に次の通信パターンを実装できます。

-   **イベント ベースのデバイスからクラウドへの取り込み。** IoT Hub 確実に、デバイスから数百万の 1 秒あたりのイベントを受信して、イベント プロセッサ エンジンを使用して、ホット パス上での処理したり、分析、コールド パス上に格納できます。 IoT Hub は、信頼性の高い処理を保証するためと負荷のピークを吸収するために、最大 7 日間イベント データを保持します。

-   **クラウドとデバイスの信頼性の高いメッセージング (または *コマンド*)。** ソリューション バック エンドでは、IoT Hub を使用して、最低 1 回の配信保証で個々のデバイスにメッセージを送信できます。 各メッセージには個々の有効期限設定があり、バック エンドでは配信と有効期限の両方の受信通知を要求して、クラウドからデバイスへのメッセージのライフサイクルの完全な可視性を保証します。 これにより、デバイス上で実行される操作を含むビジネス ロジックの実装が可能になります。

また、一貫性のあるデバイス ID 管理、接続の監視、スケールなど、IoT 固有の機能を IoT Hub で利用して、ファイルのアップロードやダウンロードなどの他の一般的なパターンを実装することもできます。

記事 [IoT Hub の比較と Event Hubs][lnk-compare] IoT ソリューションの IoT Hub を使用する利点を示し、これら 2 つのサービスの主な違いについて説明します。

## ゲートウェイ

IoT ソリューションでは、ゲートウェイは、いずれかでは通常、 [プロトコル ゲートウェイ][lnk-gateway] 、クラウドに展開または [フィールド ゲートウェイ][lnk-field-gateway] 、デバイスでローカルに配置します。 プロトコル ゲートウェイは、MQTT から AMQP への変換など、プロトコル変換を実行します。 フィールド ゲートウェイは、デバイスのローカル管理サービスを提供します。フィールド ゲートウェイは、専用のデバイスまたは既存のハードウェアで実行されているソフトウェアです。 どちらの種類のゲートウェイも、デバイスと IoT Hub 間の仲介役として機能します。

通常、フィールド ゲートウェイは、ソリューションでのアクセスと情報フローの管理で大きな役割を果たすため、単純なトラフィック ルーティング デバイス (NAT デバイスやファイアウォールなど) とは異なります。

プロトコル ゲートウェイとフィールド ゲートウェイの両方をソリューションに含めることもできます。

## IoT Hub のしくみ

Azure の IoT Hub を実装して、 [サービス経由の通信][lnk-service-assisted-pattern] 、デバイスと、ソリューションのバック エンド間の相互作用を仲介するパターンを指定します。 サービス支援通信の目的は、管理システム (IoT Hub など) と、信頼されていない物理領域にデプロイされている特殊用途のデバイス間の信頼できる双方向通信パスを確立することです。 このパターンでは次の原則が確立されています。

- セキュリティがその他のすべての機能に優先されます。
- デバイスは、要求されていないネットワーク情報を受け入れません。 デバイスは送信のみの方法で、すべての接続とルートを確立します。 バック エンドからコマンドを受信する場合、デバイスは定期的に接続を開始して、処理待ちのコマンドがないか確認する必要があります。
- デバイスは、IoT Hub など、ピアリングされている既知のサービスへの接続のみ、またはルートの確立のみを行います。
- デバイスとサービスまたはデバイスとゲートウェイ間の通信パスは、アプリケーション プロトコル レイヤーで保護されます。
- システム レベルの承認と認証は、デバイスごとの ID に基づいており、アクセス資格情報とアクセス許可をほぼ瞬時に取り消すことができます。
- コマンドとデバイスの通知は、デバイスが接続され、それらを受信するまで保持されるので、電源や接続の問題によって散発的に接続されるデバイスの双方向通信が円滑になります。 IoT Hub は、送信されるコマンドのデバイス固有のキューを維持しています。
- アプリケーション ペイロード データは個別に保護され、ゲートウェイを介して特定のサービスに転送されます。

モバイルの業界はようにプッシュ通知サービスの実装に非常に大きな規模でサービス経由の通信パターンを問題なく使用が [Windows Notification Service][lnk-wns], 、[Google Cloud Messaging][lnk-google-messaging], 、および [Apple Push Notification Service][lnk-apple-push]します。

## 次のステップ

Azure IoT Hub の詳細については、次のリンクを参照してください。

* [IoT Hub の概要][lnk-get-started]
* [デバイスの接続][lnk-connect-device]
* [デバイスからクラウドへのメッセージの処理][lnk-d2c]

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-d2c]: iot-hub-csharp-csharp-process-d2c.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted Communication, blog post by Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-guidance.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide.md#identityregistry
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9


