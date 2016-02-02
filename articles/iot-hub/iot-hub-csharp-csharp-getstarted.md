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

* **CreateDeviceIdentity**。デバイス ID と関連付けられているセキュリティ キーを作成し、シミュレーション対象デバイスを接続します。
* **ReadDeviceToCloudMessages**。シミュレーション対象デバイスから送信されたテレメトリを表示します。
* **SimulatedDevice**。以前に作成したデバイス ID で IoT ハブに接続して、1 秒ごとにテレメトリ メッセージを送信します。

> [AZURE.NOTE] 記事 [IoT Hub Sdk ][lnk-hub-sdks] デバイスと、ソリューションのバック エンドで実行する両方のアプリケーションの構築に使用できるさまざまな Sdk について説明します。

このチュートリアルで行う作業には次のものが必要となります。

+ Microsoft Visual Studio 2015

+ アクティブな Azure アカウント <br/>アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 [Azure 無料評価版 ][lnk-free-trial]します。

## IoT Hub の作成

シミュレーション対象デバイスの接続先となる IoT Hub を作成する必要があります。 次の手順では、この作業を Azure ポータルで行う方法を示しています。

1. サインイン、 [Azure ポータルの ][lnk-portal]します。

2. ジャンプバーで、**[新規]**、**[モノのインターネット]**、**[Azure IoT Hub]** の順にクリックします。

    ![][1]

3. **[IoT Hub]** ブレードで、IoT Hub の構成を選択します。

    ![][2]

    * **[名前]** ボックスに IoT Hub の名前を入力します。 その**名前**が有効で利用できる場合、**[名前]** ボックスに緑色のチェック マークが表示されます。
    * **[価格とスケール レベル]** を選択します。 このチュートリアルでは特定のレベルは必要ありません。
    * **[リソース グループ]** で、新しいリソース グループを作成するか、既存のリソース グループを選択します。 詳細については、次を参照してください。 [[lnk リソース グループ] Azure リソースを管理するリソース グループを使用した][lnk-resource-groups]します。
    * **[場所]** で、IoT Hub をホストする場所を選択します。

4. 必要な IoT Hub 構成オプションを選択したら、**[作成]** をクリックします。 Azure が IoT Hub を作成するまでに数分かかる場合があります。 状態を確認するには、スタート画面または通知パネルで進行状況を監視してください。

    ![][3]

5. IoT Hub が正常に作成されたら、新しい IoT Hub のブレードを開き、**[ホスト名]** を書き留めて**鍵**のアイコンをクリックします。

    ![][4]

6. **[iothubowner]** ポリシーをクリックし、**[iothubowner]** ブレードで接続文字列をコピーして書き留めます。

    ![][5]

IoT Hub の作成は以上です。以降の作業に必要なホスト名と接続文字列が得られました。

[AZURE.INCLUDE [iot-hub-get-started-cloud-csharp](../../includes/iot-hub-get-started-cloud-csharp.md)]


[AZURE.INCLUDE [iot-hub-get-started-device-csharp](../../includes/iot-hub-get-started-device-csharp.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio のソリューション エクスプローラーでソリューションを右クリックし、**[スタートアップ プロジェクトの設定]** をクリックします。 **[マルチ スタートアップ プロジェクト]** を選択し、**ReadDeviceToCloudMessages** プロジェクトと **SimulatedDevice** プロジェクトの **[アクション]** としてどちらも **[開始]** を選択します。

    ![][41]

2.  **F5** キーを押して両方のアプリケーションを実行します。 **SimulatedDevice** アプリケーションからのコンソール出力には、シミュレーション対象デバイスから IoT Hub に送られたメッセージが表示されます。また、**ReadDeviceToCloudMessages** アプリケーションからのコンソール出力には、IoT Hub で受信されたメッセージが表示されます。

    ![][42]

## 次のステップ

このチュートリアルでは、ポータルで新しい IoT Hub を構成し、ハブの ID レジストリにデバイス ID を作成しました。 デバイスからクラウドへのメッセージをハブに送信するシミュレーション対象デバイスでこのデバイス ID を使用しました。また、それとは別に、ハブで受け取ったメッセージを表示するアプリケーションを作成しました。 IoT Hub の機能や、他の IoT のシナリオを次のチュートリアルでさらに詳しく説明しています。

- [クラウドとデバイス間でメッセージを送信 IoT Hub ][lnk-c2d-tutorial] 、デバイスにメッセージを送信し、IoT Hub によって生成される配信からのフィードバックを処理する方法を示します。
- [プロセス デバイスからクラウド メッセージ ][lnk-process-d2c-tutorial] 利用統計情報とデバイスから対話型のメッセージを確実に処理する方法を示します。
- [[Lnk アップロード チュートリアル] のデバイスからファイルをアップロードする][lnk-upload-tutorial] パターンを使用するクラウドとデバイス間のメッセージをデバイスからファイルのアップロードを容易にします。

IoT Hub の詳細については、次の記事を参照してください。

* [IoT Hub の概要 ][lnk-hub-overview]
* [IoT Hub 開発者ガイド ][lnk-hub-dev-guide]
* [IoT Hub ソリューション ][lnk-hub-guidance]
* [サポートされているデバイスのプラットフォームおよび言語 ][lnk-supported-devices]
* [Azure の IoT デベロッパー センター ][lnk-dev-center]




[1]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub1.png 
[2]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub2.png 
[3]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub3.png 
[4]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub4.png 
[5]: ./media/iot-hub-csharp-csharp-getstarted/create-iot-hub5.png 
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png 
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png 
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

