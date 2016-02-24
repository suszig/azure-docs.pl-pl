<properties
    pageTitle="Azure IoT Hub の使用 | Microsoft Azure"
    description="このチュートリアルに従って、C# で Azure IoT Hub を使用します。"
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="12/14/2015"
     ms.author="dobett"/>

# チュートリアル: Azure IoT Hub の使用

## はじめに

Azure IoT Hub は、何百万もの IoT デバイスとソリューション バック エンド間で、セキュリティで保護された信頼性のある双方向通信を実現する、完全に管理されたサービスです。 IoT プロジェクトが直面する最も大きな課題の 1 つは、ソリューション バックエンドにデバイスを確実かつ安全に接続する方法です。 この課題に対応するために、IoT Hub は次の機能を備えています。

- 非常にスケール性が高く信頼性に優れた、デバイスとクラウド間の双方向メッセージングを提供します。
- デバイスごとのセキュリティ資格情報とアクセス制御を使用して、セキュリティで保護された通信を可能します。
- 最も一般的な言語とプラットフォームのデバイスのライブラリが含まれます。

このチュートリアルでは、次の操作方法について説明します。

- Azure ポータルを使用して IoT Hub を作成する。
- IoT Hub におけるデバイス ID を作成する。
- クラウド バックエンドへのテレメトリの送信とクラウド バックエンドからのコマンドの受信をシミュレーション対象デバイスを作成する。

このチュートリアルの最後には、次の 3 つの Windows コンソール アプリケーションが作成されています。

* **CreateDeviceIdentity**, 、デバイス id と、シミュレートされたデバイスの接続に関連付けられているセキュリティ キーを作成します。
* **ReadDeviceToCloudMessages**, 、シミュレートされたデバイスによって送信されたテレメトリが表示されます。
* **SimulatedDevice**, 、以前に作成されたデバイス id を持つ IoT hub に接続し、毎秒テレメトリ メッセージを送信します。

> [AZURE.NOTE] 記事 [IoT Hub Sdk][lnk-hub-sdks] デバイスと、ソリューションのバック エンドで実行する両方のアプリケーションの構築に使用できるさまざまな Sdk について説明します。

このチュートリアルで行う作業には次のものが必要となります。

+ Microsoft Visual Studio 2015

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料の試用アカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト][lnk-free-trial]をご覧ください。

## IoT Hub の作成

シミュレーション対象デバイスの接続先となる IoT Hub を作成する必要があります。 次の手順では、この作業を Azure ポータルで行う方法を示しています。

1. [Azure ポータル][lnk-portal]にサインインします。

2. ジャンプバーで、次のようにクリックします。 **新規**, 、] をクリックし、 **モ ノのインターネット**, 、] をクリックし、 **Azure IoT Hub**します。

    ![][1]

3.  **IoT hub** ブレードで、IoT hub の構成を選択します。

    ![][2]

    *  **名前** ボックスで、IoT hub の名前を入力します。 場合、 **名前** が有効に表示される緑色のチェック マークを使用可能な **名前** ボックス。
    * 選択、 **価格有効桁数と小数点層**します。 このチュートリアルでは特定のレベルは必要ありません。
    *  **リソース グループ**, で新しいリソース グループを作成または既存のものを選択します。 詳細については、次を参照してください。 [リソース グループを使用した Azure リソースの管理に][lnk-resource-groups]します。
    *  **場所**, 、IoT hub をホストする場所を選択します。  

4. 構成オプションの IoT hub を選択した場合にクリックして **作成**します。  Azure が IoT Hub を作成するまでに数分かかる場合があります。 状態を確認するには、スタート画面または通知パネルで進行状況を監視してください。

    ![][3]

5. IoT hub が正常に作成されると、新しい IoT hub のブレードを開き、メモ、 **ホスト名**, 、クリックして、 **キー** アイコン。

    ![][4]

6. をクリックして、 **iothubowner** ポリシーをコピーし、[接続文字列をメモして、 **iothubowner** ブレードです。

    ![][5]

IoT Hub の作成は以上です。以降の作業に必要なホスト名と接続文字列が得られました。

[AZURE.INCLUDE [iot-hub-get-started-cloud-csharp](../../includes/iot-hub-get-started-cloud-csharp.md)]


[AZURE.INCLUDE [iot-hub-get-started-device-csharp](../../includes/iot-hub-get-started-device-csharp.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  ソリューション エクスプ ローラーでの Visual Studio で、ソリューションを右クリックし、をクリックして **設定のスタートアップ プロジェクト**します。 選択 **マルチ スタートアップ プロジェクト**, 選択してから、 **開始** として、 **アクション** の両方、 **ReadDeviceToCloudMessages** と **SimulatedDevice** プロジェクトです。

    ![][41]

2.  キーを押して **f5 キーを押して** を実行している 2 つのアプリケーションを開始します。 コンソール出力を **SimulatedDevice** アプリは、シミュレートされたデバイスは、IoT hub と出力をコンソールに送信メッセージを表示、 **ReadDeviceToCloudMessages** アプリ IoT hub を受信するメッセージが表示されます。

    ![][42]

## 次のステップ

このチュートリアルでは、ポータルで新しい IoT Hub を構成し、ハブの ID レジストリにデバイス ID を作成しました。 デバイスからクラウドへのメッセージをハブに送信するシミュレーション対象デバイスでこのデバイス ID を使用しました。また、それとは別に、ハブで受け取ったメッセージを表示するアプリケーションを作成しました。 IoT Hub の機能や、他の IoT のシナリオを次のチュートリアルでさらに詳しく説明しています。

- [クラウドとデバイス間でメッセージを送信 IoT Hub][lnk c2d チュートリアル] は、デバイスにメッセージを送信し、IoT Hub によって生成される配信からのフィードバックを処理する方法を示します。
- [デバイスからクラウドへのプロセスのメッセージ][lnk-プロセス-d2c-チュートリアル] は、利用統計情報とデバイスから対話型のメッセージを確実に処理する方法を示します。
- [デバイスからファイルをアップロードする][lnk アップロード チュートリアル] パターンを説明することで利用するクラウドとデバイス間のメッセージをデバイスからファイルのアップロードを容易にします。

IoT Hub の詳細については、次の記事を参照してください。

* [IoT Hub の概要][lnk-hub-overview]
* [Azure IoT Hub 開発者ガイド][lnk-hub-dev-guide]
* [IoT Hub ソリューションの設計][lnk-hub-guidance]
* [サポートされているデバイスのプラットフォームおよび言語][lnk-supported-devices]
* [Azure IoT デベロッパー センター][lnk-dev-center]

<!-- Images. -->
[1]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub1.png
[2]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub2.png
[3]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub3.png
[4]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub4.png
[5]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub5.png
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png

<!-- Links -->
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-hub-guidance]: iot-hub-guidance.md
[lnk-hub-dev-guide]: iot-hub-devguide.md
[lnk-supported-devices]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/tested_configurations.md
[lnk-dev-center]: http://www.azure.com/develop/iot
[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-resource-groups]: resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
