<properties
    pageTitle="IoT Hub を使用したデバイスからのファイルのアップロード |Microsoft Azure"
    description="このチュートリアルに従って、C# を使用して Azure IoT Hub でデバイスからファイルをアップロードする方法を学習します。"
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


# チュートリアル: IoT Hub でデバイスからクラウドにファイルをアップロードする方法

## はじめに

Azure IoT Hub は、何百万もの IoT デバイスとアプリケーション バックエンドの間に信頼性のある保護された双方向通信を確立できる、完全に管理されたサービスです。 前のチュートリアル ([を使ってみる IoT Hub] [IoT Hub でのクラウドとデバイス間のメッセージの送信]) へのデバイスからアクセスして、クラウド コンポーネント IoT Hub の基本的なデバイスからクラウドおよびクラウドとデバイス メッセージング機能を示しています。 [デバイスからクラウドへのプロセスのメッセージ] では、Azure blob ストレージにデバイスからクラウドへのメッセージを確実に保存する方法について説明します。 ただし、デバイスから送信されるデータが比較的小さなデバイスからクラウドへのメッセージに容易にマップされない場合があります。 たとえば、大きなファイルには、イメージ、ビデオ、高周波振動データのサンプルが含まれるものや、何らかの形式の前処理済みデータが含まれるものがあります。 これらのファイルは通常、[Azure Data Factory] などのツールや [Hadoop] スタックを使用して、バッチ形式で処理されます。 イベントの送信よりデバイスからのファイルのアップロードが好ましい場合は、IoT Hub セキュリティと信頼性機能を引き続き使用できます。

このチュートリアルのビルドをデバイスに安全に提供するクラウドとデバイス間のメッセージを使用する方法を示す [IoT Hub でメッセージを送信クラウド デバイス] で示されるコードを Azure blob のアップロード、ファイルおよび IoT Hub 配信の受信確認を使用して、アプリのバックエンドからのファイルの処理をトリガーする方法を使用する URI。 この方法の利点は、IoT Hub のデバイス ID と、クラウドからデバイスへのメッセージの配信確認を再利用して、ファイルが正常にアップロードされたことをアプリ バックエンドに通知できることです。
> [AZURE.NOTE] ここで使用するのと同じ方法を使用することで、クラウドからデバイスにファイルを安全にダウンロードできます。

詳細については、クラウドとデバイス間のメッセージと IoT Hub セキュリティ [IoT Hub 開発者ガイド] にあります。

このチュートリアルの最後には、次の 2 つの Windows コンソール アプリケーションを実行します。

* **SimulatedDevice**, 、[IoT Hub でのクラウドとデバイスへの送信メッセージ] で作成したアプリの変更済みバージョン クラウドとデバイスを受け取る、IoT hub に接続する Azure を含むメッセージを blob の Uri。 受信したクラウドからデバイスへのメッセージごとに、指定された BLOB URI へのファイルのアップロードがトリガーされます。
* **SendCloudToDevice**, 、どのビルドが Azure blob の URI (で説明したよう [を作成し、Blob サービスによる SAS を使用して](../storage/storage-dotnet-shared-access-signature-part-2.md), 、IoT Hub を使用してシミュレートされたデバイスにクラウドとデバイス間のメッセージで送信し、その配信 aknowledgment を受信します。

> [AZURE.NOTE] IoT Hub には、Azure IoT デバイス SDK を介した多数のデバイス プラットフォームや言語 (C、Java、Javascript など) に対する SDK サポートがあります。 このチュートリアルのコードと一般的に Azure の IoT Hub にデバイスを接続する方法については、ステップ バイ ステップは、[Azure IoT デベロッパー センター] を参照してください。 Java および Node 用の Azure IoT サービス SDK は、近日リリース予定です。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2015、

+ アクティブな Azure アカウント <br/>アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fiot%2Ftutorials%2Ffile-upload%2F target ="_blank") です。


[AZURE.INCLUDE [iot-hub-file-upload-cloud-csharp](../../includes/iot-hub-file-upload-cloud-csharp.md)]


[AZURE.INCLUDE [iot-hub-file-upload-device-csharp](../../includes/iot-hub-file-upload-device-csharp.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio 内でソリューションを右クリックし、**[スタートアップ プロジェクトの設定...]** を選択します。 **[マルチ スタートアップ プロジェクト]** を選択してから、**SimulatedDevice** と **SendCloudToDevice** の両方のアプリの **[開始]** アクションを選択します。

2.  **F5** キーを押すと、すべてのアプリケーションが開始されていることを確認できます。 **[SendCloudToDevice]** ウィンドウを選択してキーを押します。 シミュレーション対象デバイスによってファイルがアップロードされるとメッセージが出力され、**SendCloudToDevice** アプリは正常にフィードバックが受信されたことを示します。 [Azure ポータル] を使用するか、ストレージ アカウント内のファイルの存在をチェックする Visual Studio サーバー エクスプ ローラー。

  ![][50]


## 次のステップ

このチュートリアルでは、クラウドからデバイスへのメッセージを活用して、デバイスからのファイルのアップロードを簡素化する方法を学習しました。 次のチュートリアルで IoT Hub の機能やシナリオをさらに詳しく調べることができます。

- [デバイスからクラウドへのメッセージの処理]、利用統計情報とデバイスから対話型のメッセージを確実に処理する方法を示します。

IoT Hub に関するその他の情報:

* [IoT Hub の概要]
* [IoT Hub 開発者ガイド]
* [IoT Hub ガイダンス]
* [サポートされているデバイスのプラットフォームおよび言語 ][supported devices]
* [Azure の IoT デベロッパー センター]






[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png 
[send cloud-to-device messages with iot hub]: iot-hub-csharp-csharp-c2d.md 
[azure portal]: https://portal.azure.com/ 
[azure data factory]: https://azure.microsoft.com/en-us/documentation/services/data-factory/ 
[hadoop]: https://azure.microsoft.com/en-us/documentation/services/hdinsight/ 
[get started with iot hub]: iot-hub-csharp-csharp-getstarted.md 
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

