<properties
 pageTitle="モノのインターネット用の Azure ソリューション | Microsoft Azure"
 description="サンプル ソリューション アーキテクチャや、それが、Azure IoT Hub、デバイス SDK、および構成済みソリューションとどのように関連するかなど、Azure での IoT の概要。"
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
 ms.date="11/05/2015"
 ms.author="dobett"/>


[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## 次のステップ

Azure IoT Hub を使用して独自のソリューション バックエンドの実装が行えます。Azure IoT Hub は、テレメトリをデバイスから大量に受信し、そのデータをストリーム イベント プロセッサにルーティングする Azure のサービスです。 IoT Hub は、特定のデバイスにクラウドとデバイスのコマンドを送信することもできます。 さらに、IoT Hub には、使用できるは、デバイスをプロビジョニングし、デバイスがハブに接続を管理するデバイスの id レジストリが含まれています。 詳細については、次を参照してください。

- [IoT Hub とは何ですか。][lnk-iot-hub]
- [IoT Hub ][lnk-getstarted]

さまざまなデバイスのハードウェア プラットフォームやオペレーティング システムのでは、クライアント アプリケーションを実装するには、IoT デバイスの Sdk を使用できます。 IoT デバイス Sdk には、IoT hub とクラウドとデバイスのコマンドを受信する送信側のテレメトリを容易にするためのライブラリが含まれます。 Sdk を使用する場合は、IoT Hub と通信するさまざまなネットワーク プロトコルから選択できます。 詳細については、を参照してください [デバイス Sdk には何ですか? ][lnk-device-sdks]します。

興味場合もあります。 [Azure IoT スイート ][lnk-iot-suite], 、を使用すると、すぐに始めると、リモートの監視、資産管理、および予測のメンテナンスなどの一般的な IoT シナリオに対処する IoT プロジェクトの規模の設定を構成済みのソリューションのコレクション。


[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md 
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md 
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md 
[lnk-iot-suite]: http://azure.microsoft.com/solutions/iot/ 
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/ 

