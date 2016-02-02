<properties
 pageTitle="Azure IoT Hub と Azure Event Hubs の比較 | Microsoft Azure"
 description="機能の相違点とユース ケースを中心とした、Azure IoT Hub サービスと Azure Event Hubs サービスの比較です。"
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


# IoT Hub と Event Hubs の比較

デバイスからのテレメトリ データの収集は、Azure IoT Hub の主なユース ケースの 1 つです。 このため、IoT Hub によくたとえられます [Event Hubs][], 、低待機時間と高い信頼性の大規模クラウドへのイベントとテレメトリ受信を提供するイベント処理サービスであります。

ただし、サービスには異なる点も多数あります。次のセクションでは、この違いについて詳しく説明します。

| 領域| IoT Hub| Event Hubs|
| ---- | ------- | ---------- |
| 通信パターン| D2C イベント イングレスと C2D メッセージング。| イベント イングレスのみ (通常、D2C シナリオのみ)。|
| セキュリティ| デバイスごとの ID と取り消し可能なアクセス制御。[IoT Hub 開発者ガイド - セキュリティ] を参照してください。| イベント ハブ全体 [共有アクセス ポリシーの ][event hub - security], 、制限付きの失効とサポートを使用して [発行元のポリシー ][event hub publisher policies]します。IoT ソリューションでは、デバイスごとの資格情報となりすまし対策をサポートするために、カスタム ソリューションの実装が求められることがよくあります。|
| スケール| IoT Hub は、同時接続された数百万のデバイスをサポートするように最適化されています。| Event Hubs がより限定された同時接続数をサポートできます: AMQP 接続の数は 5,000 としてあたり最大 [Service Bus のクォータ][]します。一方、Event Hubs を使用すると、送信される各メッセージのパーティションを指定できます。|
| デバイスの SDK| IoT Hub は、 [デバイス Sdk ][azure iot hub sdks] のさまざまなプラットフォームおよび言語| Event Hubs は .NET、C でサポートされ、AMQP と HTTP の送信インターフェイスを提供します。|

まとめると、唯一のユース ケースが D2C テレメトリ イングレスである場合でも、IoT Hub によって IoT デバイス接続用に設計されたサービスを提供し、IoT 固有の機能を使ったシナリオの価値提案を展開し続けることができます。 Event Hubs は、大規模なイベント イングレス向けに設計されており、データ センター間およびデータ センター内の両方のシナリオに対応できます。

IoT Hub と Event Hubs を組み合わせて使用し、IoT Hub が D2C 通信を処理してから、Event Hubs が後でリアルタイム処理エンジンへのイベント イングレスを処理できるようにすることはよくあります。

## 次のステップ

Azure IoT Hub についてさらに学習するには、次のリンクを使用してください。

- [IoT Hub (チュートリアル) ][lnk-get-started]
- [Azure の IoT Hub とは何ですか。[]][]


[event hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md 
[iot hub developer guide - security]: iot-hub-devguide.md#security 
[event hub - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md 
[event hub publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks 
[service bus quotas]: ../service-bus/service-bus-quotas.md 
[azure iot hub sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md 
[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md 
[what is azure iot hub?]: iot-hub-what-is-iot-hub.md 

