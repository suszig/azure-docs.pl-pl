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

Azure IoT Hub は、何百万もの IoT デバイスとアプリケーション バックエンドの間に信頼性のある保護された双方向通信を確立できる、完全に管理されたサービスです。 [入門 IoT Hub] IoT hub を作成、内のデバイス id をプロビジョニング、およびデバイスからクラウドへのメッセージを送信するシミュレートされたデバイスをコーディングする方法について説明します。

このチュートリアルに基づいて記述 [を使ってみる IoT Hub] クラウドとデバイスのメッセージを 1 つのデバイスに送信する方法を示しています配信の受信確認を要求する方法 (*フィードバック*) IoT Hub からし、アプリケーションのクラウド バック エンドから受信します。

内のクラウドとデバイス間のメッセージに関する詳細情報を記載した、 [IoT Hub 開発者ガイド ][iot hub developer guide - c2d]します。

このチュートリアルの最後には、次の 2 つの Windows コンソール アプリケーションを実行します。

* **SimulatedDevice**, 、変更したバージョンで作成したアプリの [入門 IoT Hub]、IoT hub に接続し、クラウドとデバイス間のメッセージを受信します。
* **SendCloudToDevice**。これは、クラウドからデバイスへのメッセージを IoT Hub を介してシミュレーション済みデバイスに送信し、その配信確認を受け取ります。

> [AZURE.NOTE] IoT Hub には、Azure IoT デバイス SDK を介した多数のデバイス プラットフォームや言語 (C、Java、Javascript など) に対する SDK サポートがあります。 このチュートリアルのコードと一般的に Azure の IoT Hub にデバイスを接続する方法については、ステップ バイ ステップは、[Azure IoT デベロッパー センター] を参照してください。 Java および Node 用の Azure IoT サービス SDK は、近日リリース予定です。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2015、

+ アクティブな Azure アカウント <br/>アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fiot%2Ftutorials%2Fc2d%2F target ="_blank") です。

[AZURE.INCLUDE [iot-hub-c2d-device-csharp](../../includes/iot-hub-c2d-device-csharp.md)]


[AZURE.INCLUDE [iot-hub-c2d-cloud-csharp](../../includes/iot-hub-c2d-cloud-csharp.md)]

## 次のステップ

このチュートリアルでは、クラウドからデバイスへのメッセージを送受信する方法を学習しました。 引き続き、以下のチュートリアルでは IoT Hub の機能やシナリオをさらに詳しく調べることができます。

- [デバイスからクラウドへのメッセージの処理]、利用統計情報とデバイスから対話型のメッセージを確実に処理する方法を示します。
- パターンを説明する [デバイスからファイルをアップロードする] を使用するクラウドとデバイス間のメッセージをデバイスからファイルのアップロードを容易にします。

IoT Hub に関するその他の情報:

* [IoT Hub の概要]
* [IoT Hub 開発者ガイド]
* [IoT Hub ガイダンス]
* [サポートされているデバイスのプラットフォームおよび言語 ][supported devices]
* [Azure の IoT デベロッパー センター]






[get started with iot hub]: iot-hub-csharp-csharp-getstarted.md 
[iot hub developer guide - c2d]: iot-hub-devguide.md#c2d 
[azure portal]: https://portal.azure.com/ 
[send cloud-to-device messages with iot hub]: iot-hub-csharp-csharp-c2d.md 
[process device-to-cloud messages]: iot-hub-csharp-csharp-process-d2c.md 
[uploading files from devices]: iot-hub-csharp-csharp-file-upload.md 
[iot hub overview]: iot-hub-what-is-iot-hub.md 
[iot hub guidance]: iot-hub-guidance.md 
[iot hub developer guide]: iot-hub-devguide.md 
[iot hub supported devices]: iot-hub-supported-devices.md 
[get started with iot hub]: iot-hub-csharp-csharp-getstarted.md 
[supported devices]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/tested_configurations.md 
[azure iot developer center]: http://www.azure.com/develop/iot 

