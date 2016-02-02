<properties
 pageTitle="Azure IoT Hub のスケーリング | Microsoft Azure"
 description="Azure IoT Hub のスケーリングについて説明します。"
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


# IoT Hub のスケーリング

IoT Hub では、IoT Hub S1 または S2 ユニット数を 2.000 に増やすことで、最大 100 万の同時接続デバイスをサポートできます。 詳細については、次を参照してください。 [IoT Hub の料金が ][lnk-pricing]します。

各 IoT Hub ユニットで、同時に接続できるレジストリ内のデバイス ID 数と、毎日のメッセージ数が許可されます。

ソリューションを適切に調整するには、IoT Hub の特定の用途を考慮します。 具体的には、次の操作カテゴリに対して必要なピーク スループットを検討してください。

* デバイスからクラウドへのメッセージ
* クラウドからデバイスへのメッセージ
* ID レジストリの操作

このスループット情報に加えて、次を参照してください、 [IoT Hub クォータとスロットルの][] し、それに応じて、ソリューションを設計します。

## デバイスからクラウドおよびクラウドからデバイスへのメッセージのスループット

IoT Hub ソリューションのサイズを設定する方法として最適なのは、デバイスごとにトラフィックを評価することです。

デバイスからクラウドへのメッセージは、次の持続スループット ガイドラインに従います。

| レベル| 持続スループット| 持続送信レート|
| ---- | -------------------- | ------------------- |
| S1| デバイスあたり最大 8 KB/時間| デバイスあたり平均 4 メッセージ/時間|
| S2| デバイスあたり最大 4 KB/分| デバイスあたり平均 2 メッセージ/分|

デバイスからクラウドへのメッセージを受け取ると、アプリケーションのバックエンドは、(複数のすべてのリーダー) 以下の最大のスループットを期待できます。

| レベル| 持続スループット|
| ---- | -------------------- |
| S1| ユニットあたり最大 120 KB/分、最小 2 MB/秒。|
| S2| ユニットあたり最大 4 MB/分、最小 2 MB/秒。|

クラウドからデバイスへのメッセージのパフォーマンスはデバイスごとに調整され、デバイスごとに 1 分あたり最大 5 つのメッセージを受信します。

## ID レジストリ操作のスループット

IoT Hub の ID レジストリの操作は、ほとんどがデバイス プロビジョニングに関連しているため、ランタイム操作にはなりません。

特定のバースト パフォーマンス数値の場合、次を参照してください。 [IoT Hub クォータとスロットルの []][]します。

## シャーディング

1 つの IoT Hub を数百万のデバイスに拡張できますが、使用しているソリューションに、1 つの IoT Hub では保証できない特定のパフォーマンス特性が必要になる場合があります。 この場合は、複数の IoT Hub でデバイスを分割して、トラフィック バーストを円滑化し、必要なスループットまたは操作レートを実現することをお勧めします。

## 次のステップ

Azure IoT Hub についてさらに学習するには、次のリンクを使用してください。

- [IoT Hub (チュートリアル) ][lnk-get-started]
- [Azure の IoT Hub とは何ですか。[]][]


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub 
[iot hub quotas and throttles]: iot-hub-devguide.md#throttling 
[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md 
[what is azure iot hub?]: iot-hub-what-is-iot-hub.md 

