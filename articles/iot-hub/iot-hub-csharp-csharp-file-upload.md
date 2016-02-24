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

Azure IoT Hub は、何百万もの IoT デバイスとアプリケーション バックエンドの間に信頼性のある保護された双方向通信を確立できる、完全に管理されたサービスです。 前のチュートリアル ([Get started with IoT Hub] と [Send Cloud-to-Device messages with IoT Hub]) IoT Hub とどのようにして、デバイスからでもアクセスできるクラウド コンポーネントの基本的なデバイスからクラウドおよびクラウドとデバイス メッセージング機能を示しています。 [Process Device-to-Cloud messages] Azure blob ストレージにデバイスからクラウドへのメッセージを確実に保存する方法を説明します。 ただし、デバイスから送信されるデータが比較的小さなデバイスからクラウドへのメッセージに容易にマップされない場合があります。 たとえば、大きなファイルには、イメージ、ビデオ、高周波振動データのサンプルが含まれるものや、何らかの形式の前処理済みデータが含まれるものがあります。 これらのファイルは通常などのツールを使用して、バッチ形式で処理 [Azure Data Factory] または [Hadoop] スタックです。 イベントの送信よりデバイスからのファイルのアップロードが好ましい場合は、IoT Hub セキュリティと信頼性機能を引き続き使用できます。

このチュートリアルで示したコードに基づき [Send Cloud-to-Device messages with IoT Hub] 、Azure をクラウドとデバイス間のメッセージを使用して、デバイスに安全に提供する方法を表示するには、blob のアップロード、ファイルおよび IoT Hub 配信の受信確認を使用して、アプリのバックエンドからのファイルの処理をトリガーする方法を使用する URI。 この方法の利点は、IoT Hub のデバイス ID と、クラウドからデバイスへのメッセージの配信確認を再利用して、ファイルが正常にアップロードされたことをアプリ バックエンドに通知できることです。

> [AZURE.NOTE] ここで使用する同じアプローチは、クラウドからファイルをダウンロード対象のデバイスを安全にあるを使用することができます。

IoT Hub セキュリティとクラウドとデバイス間のメッセージに関する詳細情報を記載することができます、 [IoT Hub Developer Guide]します。

このチュートリアルの最後には、次の 2 つの Windows コンソール アプリケーションを実行します。

* **SimulatedDevice**, で作成したアプリの変更済みバージョン [Send Cloud-to-Device messages with IoT Hub], 、クラウドからデバイスを受け取る、IoT hub に接続する Azure を含むメッセージを blob の Uri。 受信したクラウドからデバイスへのメッセージごとに、指定された BLOB URI へのファイルのアップロードがトリガーされます。
* **SendCloudToDevice**, 、どのビルドが Azure blob の URI (で説明したよう [を作成し、Blob サービスによる SAS を使用して](../storage/storage-dotnet-shared-access-signature-part-2.md), 、IoT Hub を使用してシミュレートされたデバイスにクラウドとデバイス間のメッセージで送信し、その配信 aknowledgment を受信します。

> [AZURE.NOTE] IoT Hub は、SDK のさまざまなデバイス プラットフォームや言語 (C、Java、Javascript など) 時も Azure の IoT デバイスの Sdk のサポートです。 参照してください、 [Azure IoT Developer Center] このチュートリアルのコードと一般的に Azure の IoT Hub にデバイスを接続する方法については、ステップ バイ ステップです。 Java および Node 用の Azure IoT サービス SDK は、近日リリース予定です。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2015、

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fiot%2Ftutorials%2Ffile-upload%2F target ="_blank") です。


[AZURE.INCLUDE [iot-hub-file-upload-cloud-csharp](../../includes/iot-hub-file-upload-cloud-csharp.md)]


[AZURE.INCLUDE [iot-hub-file-upload-device-csharp](../../includes/iot-hub-file-upload-device-csharp.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio でソリューションを右クリックし、選択 **設定のスタートアップ プロジェクト]**します。 選択 **マルチ スタートアップ プロジェクト**, 選択してから、 **開始** 両方のアクション **SimulatedDevice**, 、および **SendCloudToDevice** アプリです。

2.  キーを押して **f5 キーを押して**, 、およびすべてのアプリケーションが起動を表示する必要があります。 選択、 **SendCloudToDevice** ウィンドウと、キーを押します。 シミュレートされたデバイスは、ファイルがアップロードすると、メッセージを出力が表示されます、 **SendCloudToDevice** アプリケーションが成功したフィードバックの確認メッセージを表示します。 使用することができます、 [Azure portal] またはストレージ アカウント内のファイルの存在をチェックする Visual Studio サーバー エクスプ ローラー。

  ![][50]


## 次のステップ

このチュートリアルでは、クラウドからデバイスへのメッセージを活用して、デバイスからのファイルのアップロードを簡素化する方法を学習しました。 次のチュートリアルで IoT Hub の機能やシナリオをさらに詳しく調べることができます。

- [Process Device-to-Cloud messages], 、利用統計情報とデバイスから対話型のメッセージを確実に処理する方法を示します。

IoT Hub に関するその他の情報:

* [IoT Hub の概要]
* [IoT Hub 開発者ガイド]
* [IoT Hub のガイダンス]
* [サポートされているデバイスのプラットフォームおよび言語][Supported devices]
* [Azure IoT デベロッパー センター]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png

<!-- Links -->

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md

[Azure portal]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/en-us/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/en-us/documentation/services/hdinsight/

[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md
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

