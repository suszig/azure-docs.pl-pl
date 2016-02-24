<properties
    pageTitle="IoT Hub を使用したクラウドからデバイスへのメッセージの送信 | Microsoft Azure"
    description="このチュートリアルに従って、C# を使用して Azure IoT Hub でクラウドからデバイスへのメッセージを送信する方法を学習します。"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/29/2015"
     ms.author="elioda"/>

# チュートリアル: IoT Hub でクラウドからデバイスへのメッセージを送信する方法

## はじめに

Azure IoT Hub は、何百万もの IoT デバイスとアプリケーション バックエンドの間に信頼性のある保護された双方向通信を確立できる、完全に管理されたサービスです。  [Get started with IoT Hub] IoT hub を作成、内のデバイス id をプロビジョニング、およびデバイスからクラウドへのメッセージを送信するシミュレートされたデバイスをコーディングする方法について説明します。

このチュートリアルは [Get started with IoT Hub] し、1 つのデバイスをクラウドとデバイス間のメッセージを送信する方法を示しています。 配信の受信確認を要求する方法 (*フィードバック*) IoT Hub からし、アプリケーションのクラウド バック エンドから受信します。

内のクラウドとデバイス間のメッセージに関する詳細情報を記載した、 [IoT Hub 開発者ガイド][IoT Hub Developer Guide - C2D]します。

このチュートリアルの最後には、次の 2 つの Windows コンソール アプリケーションを実行します。

* **SimulatedDevice**, で作成したアプリの変更済みバージョン [Get started with IoT Hub], 、IoT hub に接続し、クラウドとデバイス間のメッセージを受信します。
* **SendCloudToDevice**, 、IoT Hub を使用してシミュレートされたデバイスをクラウドとデバイス メッセージを送信し、その配信 aknowledgment を受信します。

> [AZURE.NOTE] IoT Hub は、SDK のさまざまなデバイス プラットフォームや言語 (C、Java、Javascript など) 時も Azure の IoT デバイスの Sdk のサポートです。 参照してください、 [Azure IoT Developer Center] このチュートリアルのコードと一般的に Azure の IoT Hub にデバイスを接続する方法については、ステップ バイ ステップです。 Java および Node 用の Azure IoT サービス SDK は、近日リリース予定です。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2015、

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fiot%2Ftutorials%2Fc2d%2F target ="_blank") です。

[AZURE.INCLUDE [iot-hub-c2d-device-csharp](../../includes/iot-hub-c2d-device-csharp.md)]


[AZURE.INCLUDE [iot-hub-c2d-cloud-csharp](../../includes/iot-hub-c2d-cloud-csharp.md)]

## 次のステップ

このチュートリアルでは、クラウドからデバイスへのメッセージを送受信する方法を学習しました。 引き続き、以下のチュートリアルでは IoT Hub の機能やシナリオをさらに詳しく調べることができます。

- [Process Device-to-Cloud messages], 、利用統計情報とデバイスから対話型のメッセージを確実に処理する方法を示します。
- [Uploading files from devices], 、パターンを使用するクラウドとデバイス間のメッセージをデバイスからファイルのアップロードを容易にします。

IoT Hub に関するその他の情報:

* [IoT Hub の概要]
* [IoT Hub 開発者ガイド]
* [IoT Hub のガイダンス]
* [サポートされているデバイスのプラットフォームおよび言語][Supported devices]
* [Azure IoT デベロッパー センター]

<!-- Images. -->

<!-- Links -->

[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md

[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d

[Azure portal]: https://portal.azure.com/

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Uploading files from devices]: iot-hub-csharp-csharp-file-upload.md

[IoT Hub Overview]: iot-hub-what-is-iot-hub.md
[IoT Hub Guidance]: iot-hub-guidance.md
[IoT Hub Developer Guide]: iot-hub-devguide.md
[IoT Hub Supported Devices]: iot-hub-supported-devices.md
[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Supported devices]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/tested_configurations.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

