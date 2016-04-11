<properties
    pageTitle="IoT Hub のデバイスからクラウドへのメッセージの処理 | Microsoft Azure"
    description="このチュートリアルに従って、IoT Hub のデバイスからクラウドへのメッセージの処理に便利なパターンを学習します。"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/29/2015"
     ms.author="elioda"/>

# チュートリアル: IoT Hub のデバイスからクラウドへのメッセージを処理する方法

## はじめに

Azure IoT Hub は、何百万もの IoT デバイスとアプリケーション バックエンドの間に信頼性のある保護された双方向通信を確立できる、完全に管理されたサービスです。 前のチュートリアル ([Get started with IoT Hub] と [Send Cloud-to-Device messages with IoT Hub]) IoT Hub とどのようにして、デバイスからでもアクセスできるクラウド コンポーネントの基本的なデバイスからクラウドおよびクラウドとデバイス メッセージング機能を示しています。

このチュートリアルで示したコードに基づき [Get started with IoT Hub] デバイスからクラウドへのメッセージを処理するための 2 つのパターンを表示します。

最初のパターンは、デバイスからクラウドにあるメッセージの信頼性の高いストレージ [Azure Blobs]します。 実装する場合は、このシナリオは非常に一般的な *コールド パス* 分析、blob に格納されたデータが分析などのツールによって駆動型への入力として使用されている [Azure Data Factory] または [Hadoop] スタックです。

2 つ目のパターンは、処理の信頼性の高い *インタラクティブ* デバイスからクラウドへのメッセージ。 デバイスからクラウドへのメッセージと呼ばれる *対話型* がの場合で、アプリケーションのバック エンドでのアクションのセットの即時のトリガーとは対照的に、 *データ ポイント* が分析エンジンにフィードされるメッセージ。 たとえば、CRM システムでチケットの挿入をさせる必要のあるデバイスから送信されたアラームが、 *インタラクティブ* は温度のサンプルを含む製品利用統計情報メッセージではなく、デバイスからクラウド メッセージ、 *データ ポイント* メッセージです。

このチュートリアルを使用して IoT Hub は、デバイスからクラウドへのメッセージを受信するイベント ハブと互換性のあるエンドポイントを公開するため [EventProcessorHost] イベント プロセッサ クラスをホストします。

* 確実に保存 *データ ポイント* Azure Blob 内のメッセージと
* フォワード *インタラクティブ* デバイスからクラウドへのメッセージ、 [Service Bus Queue] を直ちに実行します。

[Service Bus][Service Bus Queue] 対話型のメッセージの信頼性の高い処理を保証する優れた方法は、メッセージごとのチェックポイント、およびタイム ウィンドウ ベースの重複除去を提供します。

このチュートリアルの最後には、次の 3 つの Windows コンソール アプリケーションを実行します。

* **SimulatedDevice**, で作成したアプリの変更済みバージョン [Get started with IoT Hub], 、送信し *データ ポイント* デバイスからクラウドへのメッセージ、毎秒と *対話型* デバイスからクラウドへメッセージをぞれぞれ 10 秒です。
* **ProcessDeviceToCloudMessages**, が使用される [EventProcessorHost] を確実に格納する *データ ポイント* が Azure blob にメッセージを転送 *インタラクティブ* Service Bus のキューにメッセージと
* **ProcessD2cInteractiveMessages**, 、キューからメッセージをデキューします。

> [AZURE.NOTE] IoT Hub は、SDK のさまざまなデバイス プラットフォームや言語 (C、Java、Javascript など) 時も Azure の IoT デバイスの Sdk のサポートです。 参照してください、 [Azure IoT Developer Center] このチュートリアルのコードと一般的に Azure の IoT Hub にデバイスを接続する方法については、ステップ バイ ステップです。

> [AZURE.NOTE] このチュートリアルの内容が例: イベント ハブと互換性のあるメッセージを使用するには、その他の方法に直接適用できる [Hadoop] Storm などのプロジェクトです。 参照してください [IoT Hub Guidance - Event Hubs compatibility] の詳細。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2015、

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fiot%2Ftutorials%2Fprocess-d2c%2F target ="_blank") です。

またについて一定の知識を前提として [Azure Storage] と [Azure Service Bus]します。


[AZURE.INCLUDE [iot-hub-process-d2c-device-csharp](../../includes/iot-hub-process-d2c-device-csharp.md)]


[AZURE.INCLUDE [iot-hub-process-d2c-cloud-csharp](../../includes/iot-hub-process-d2c-cloud-csharp.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio でソリューションを右クリックし、選択 **設定のスタートアップ プロジェクト]**します。 選択 **マルチ スタートアップ プロジェクト**, 選択してから、 **開始** のアクション **ProcessDeviceToCloudMessages**, 、**SimulatedDevice**, 、および **ProcessD2cInteractiveMessages** アプリです。

2.  キーを押して **f5 キーを押して**, とすべてのアプリケーションが起動し、対話型のメッセージのプロセッサによってシミュレートされたデバイスから送信されたすべての対話型のメッセージを処理する必要がありますが表示されます。

  ![][50]

> [AZURE.NOTE] 更新される、blob ファイルを表示するのには、削減する必要があります、 `MAX_BLOCK_SIZE` で定数 `StoreEventProcessor` より小さい値に (つまり `1024`)。 これは、シミュレーション対象デバイスによって送信されたデータのブロック サイズの制限に達するまでに時間がかかるためです。 このように編集することで、ストレージ コンテナーで BLOB が作成され、更新されたことを確認できます。

## 次のステップ

このチュートリアルで確実に処理する方法を学習しました *データ ポイント* と *インタラクティブ* を使用してデバイスからクラウド メッセージ [EventProcessorHost]します。 同じようなメッセージ処理ロジックを、次のようにして実装することができます。

- [Uploading files from devices], 、パターンを使用するクラウドとデバイス間のメッセージをデバイスからファイルのアップロードを容易にします。

IoT Hub に関するその他の情報:

* [IoT Hub の概要]
* [IoT Hub 開発者ガイド]
* [IoT Hub のガイダンス]
* [サポートされているデバイスのプラットフォームおよび言語][Supported devices]
* [Azure IoT デベロッパー センター]

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png


<!-- Links -->

[Azure Blobs]: https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/
[Azure Data Factory]: https://azure.microsoft.com/en-us/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/en-us/documentation/services/hdinsight/
[Service Bus Queue]: https://azure.microsoft.com/en-us/documentation/articles/service-bus-dotnet-how-to-use-queues/
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Guidance - Event Hubs compatibility]: iot-hub-guidance.md#eventhubcompatible

[Azure Storage]: https://azure.microsoft.com/en-us/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/en-us/documentation/services/service-bus/

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


