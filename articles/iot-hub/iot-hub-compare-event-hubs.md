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

| 領域 | IoT Hub | Event Hubs |
| ---- | ------- | ---------- |
| 通信パターン | D2C イベント イングレスと C2D メッセージング。 | イベント イングレスのみ (通常、D2C シナリオのみ)。 |
| セキュリティ | デバイスごとの ID と取り消し可能なアクセス制御。 See [IoT Hub Developer Guide - Security]. | イベント ハブ全体 [共有アクセス ポリシー][Event Hub - security], 、制限付きの失効とサポートを使用して [発行元のポリシー][Event Hub publisher policies]します。 IoT ソリューションでは、デバイスごとの資格情報となりすまし対策をサポートするために、カスタム ソリューションの実装が求められることがよくあります。 |
| スケール | IoT Hub は、同時接続された数百万のデバイスをサポートするように最適化されています。 | Event Hubs がより限定された同時接続数をサポートできます: AMQP 接続の数は 5,000 としてあたり最大 [Service Bus のクォータ][]します。 一方、Event Hubs を使用すると、送信される各メッセージのパーティションを指定できます。 |
| デバイスの SDK | IoT Hub は、 [デバイス Sdk][Azure IoT Hub SDKs] のさまざまなプラットフォームおよび言語 | Event Hubs は .NET、C でサポートされ、AMQP と HTTP の送信インターフェイスを提供します。 |

まとめると、唯一のユース ケースが D2C テレメトリ イングレスである場合でも、IoT Hub によって IoT デバイス接続用に設計されたサービスを提供し、IoT 固有の機能を使ったシナリオの価値提案を展開し続けることができます。 Event Hubs は、大規模なイベント イングレス向けに設計されており、データ センター間およびデータ センター内の両方のシナリオに対応できます。

IoT Hub と Event Hubs を組み合わせて使用し、IoT Hub が D2C 通信を処理してから、Event Hubs が後でリアルタイム処理エンジンへのイベント イングレスを処理できるようにすることはよくあります。

## 次のステップ

Azure IoT Hub についてさらに学習するには、次のリンクを使用してください。

- [IoT Hubs の使用 (チュートリアル)][lnk-get-started]
- [Azure IoT Hub とは][]

[Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[IoT Hub Developer Guide - Security]: iot-hub-devguide.md#security
[Event Hub - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hub publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Service Bus Quotas]: ../service-bus/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[What is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md

