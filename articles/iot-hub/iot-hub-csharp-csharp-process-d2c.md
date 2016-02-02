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

Azure IoT Hub は、何百万もの IoT デバイスとアプリケーション バックエンドの間に信頼性のある保護された双方向通信を確立できる、完全に管理されたサービスです。 前のチュートリアル ([を使ってみる IoT Hub] [IoT Hub でのクラウドとデバイス間のメッセージの送信]) へのデバイスからアクセスして、クラウド コンポーネント IoT Hub の基本的なデバイスからクラウドおよびクラウドとデバイス メッセージング機能を示しています。

このチュートリアルで示したコードは [を使ってみる IoT Hub] デバイスからクラウドへのメッセージを処理するための 2 つのパターンを表示します。

最初のパターンは、[Azure Blob] でデバイスからクラウドへのメッセージの信頼性の高いストレージです。 実装する場合は、このシナリオは非常に一般的な *コールド パス* 分析、blob に格納されたデータが [Azure Data Factory] などのツールや [Hadoop] スタックによって駆動 analytics への入力として使用されています。

2 つ目のパターンは、*対話型*のデバイスからクラウドへのメッセージの信頼性の高い処理です。 デバイスからクラウドへのメッセージは、アプリケーション バックエンドでの一連のアクションの即時トリガーである場合、*対話型*と呼ばれ、分析エンジンにフィードされる*データ ポイント* メッセージとは異なります。 たとえば、CRM システムでチケットの挿入をトリガーする必要のあるデバイスから送信されるアラームは*対話型*のデバイスからクラウドへのメッセージで、*データ ポイント* メッセージである温度サンプルを含むテレメトリ メッセージとは異なります。

IoT Hub はメッセージを受信するデバイスからクラウドに、このチュートリアルを使用して [EventProcessorHost] イベント プロセッサ ホストへのイベント ハブと互換性のあるエンドポイントからクラスします。

* Azure BLOB に*データ ポイント* メッセージを確実に格納する。
* フォワード *インタラクティブ* [Service Bus キュー] を直ちに実行するデバイスからクラウドへのメッセージ。

[Service Bus ][service bus queue] 対話型のメッセージの信頼性の高い処理を保証する優れた方法は、メッセージごとのチェックポイント、およびタイム ウィンドウ ベースの重複除去を提供します。

このチュートリアルの最後には、次の 3 つの Windows コンソール アプリケーションを実行します。

* **SimulatedDevice**, で作成したアプリの変更済みバージョン [を使ってみる IoT Hub]、送信し *データ ポイント* デバイスからクラウドへのメッセージ、毎秒と *対話型* デバイスからクラウドへメッセージをぞれぞれ 10 秒です。
* **ProcessDeviceToCloudMessages**, 、[EventProcessorHost] を使用して、確実に保存する *データ ポイント* が Azure blob にメッセージを転送 *インタラクティブ* Service Bus のキューにメッセージと
* **ProcessD2cInteractiveMessages**。キューからメッセージをデキューします。

> [AZURE.NOTE] IoT Hub には、Azure IoT デバイス SDK を介した多数のデバイス プラットフォームや言語 (C、Java、Javascript など) に対する SDK サポートがあります。 このチュートリアルのコードと一般的に Azure の IoT Hub にデバイスを接続する方法については、ステップ バイ ステップは、[Azure IoT デベロッパー センター] を参照してください。

> [AZURE.NOTE] このチュートリアルの内容は、Storm などの [Hadoop] プロジェクトなど、イベント ハブと互換性のあるメッセージを使用するには、その他の方法に直接適用されます。 詳細については、[IoT Hub ガイド - Event Hubs の互換性] を参照してください。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2015、

+ アクティブな Azure アカウント <br/>アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fiot%2Ftutorials%2Fprocess-d2c%2F target ="_blank") です。

いくつかの [Azure ストレージ] の知識と [Azure Service Bus] もと仮定します。


[AZURE.INCLUDE [iot-hub-process-d2c-device-csharp](../../includes/iot-hub-process-d2c-device-csharp.md)]


[AZURE.INCLUDE [iot-hub-process-d2c-cloud-csharp](../../includes/iot-hub-process-d2c-cloud-csharp.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio 内でソリューションを右クリックし、**[スタートアップ プロジェクトの設定...]** を選択します。 **[マルチ スタートアップ プロジェクト]** を選択してから、**ProcessDeviceToCloudMessages**、**SimulatedDevice**、**ProcessD2cInteractiveMessages** アプリの **[開始]** アクションを選択します。

2.  **F5** キーを押すと、すべてのアプリケーションが開始され、シミュレーション対象デバイスによって送信されたすべての対話型メッセージが対話型メッセージ プロセッサで処理されていることを確認できます。

  ![][50]

> [AZURE.NOTE] 更新される、blob ファイルを表示するのには、削減する必要があります、 `MAX_BLOCK_SIZE` で定数 `StoreEventProcessor` より小さい値に (つまり `1024年`)。 これは、シミュレーション対象デバイスによって送信されたデータのブロック サイズの制限に達するまでに時間がかかるためです。 このように編集することで、ストレージ コンテナーで BLOB が作成され、更新されたことを確認できます。

## 次のステップ

このチュートリアルで確実に処理する方法を学習しました *データ ポイント* と *インタラクティブ* [EventProcessorHost] を使用してデバイスからクラウドへのメッセージ。 同じようなメッセージ処理ロジックを、次のようにして実装することができます。

- パターンを説明する [デバイスからファイルをアップロードする] を使用するクラウドとデバイス間のメッセージをデバイスからファイルのアップロードを容易にします。

IoT Hub に関するその他の情報:

* [IoT Hub の概要]
* [IoT Hub 開発者ガイド]
* [IoT Hub ガイダンス]
* [サポートされているデバイスのプラットフォームおよび言語 ][supported devices]
* [Azure の IoT デベロッパー センター]





[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png 
[azure blobs]: https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-blobs/ 
[azure data factory]: https://azure.microsoft.com/en-us/documentation/services/data-factory/ 
[hadoop]: https://azure.microsoft.com/en-us/documentation/services/hdinsight/ 
[service bus queue]: https://azure.microsoft.com/en-us/documentation/articles/service-bus-dotnet-how-to-use-queues/ 
[eventprocessorhost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx 
[transient fault handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx 
[iot hub guidance - event hubs compatibility]: iot-hub-guidance.md#eventhubcompatible 
[azure storage]: https://azure.microsoft.com/en-us/documentation/services/storage/ 
[azure service bus]: https://azure.microsoft.com/en-us/documentation/services/service-bus/ 
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

