<properties
   pageTitle="Linux で C を使用してデバイスを接続する |Microsoft Azure"
   description="C で記述され、Linux で実行されるアプリケーションを使用して、デバイスを Azure IoT Suite 構成済みリモート監視ソリューションに接続する方法について説明します。"
   services=""
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="na"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/10/2015"
   ms.author="dobett"/>


# デバイスを IoT Suite リモート監視構成済みソリューションに接続する

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## Linux で C のサンプル ソリューションをビルドして実行する

1. クローンを作成する、 *Microsoft Azure の IoT Sdk* GitHub リポジトリし、インストール、 *C の Microsoft Azure の IoT デバイス SDK* 、Linux デスクトップ環境で次の [Linux 開発環境を設定する][lnk-setup-linux] 指示します。

2. ファイルを開く **c/serializer/samples/remote_monitoring/remote_monitoring.c** テキスト エディターでします。

3. ファイル内で次のコードを見つけます。

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

4. 置換 **[デバイス Id]** と **[デバイスのキー]** リモートの監視ソリューションのダッシュ ボードから、デバイスの値を持つ。

5. 使用して、 **IoT Hub のホスト名** を交換できるダッシュ ボードから **[IoTHub Name]** と **[IoTHub サフィックス、つまり azure devices.net]**します。 などの場合、 **IoT Hub のホスト名** は **contoso.azure devices.net**, 、置換 **[IoTHub Name]** で **contoso** と置換 **[IoTHub サフィックス、つまり azure devices.net]** と **azure devices.net** 次のように。

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "Contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

6. 変更を保存し、サンプルをビルドします。 Build.sh スクリプトを実行する、サンプルをビルドする、 **c、build_all または linux** ディレクトリ。 ビルド スクリプトを作成、 **cmake** コンパイルしたサンプル プログラムを格納するフォルダーです。

7. 実行、 **cmake/シリアライザー/サンプル/remote_monitoring/remote_monitoring** サンプル アプリケーションです。

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-setup-linux]: https://github.com/azure/azure-iot-sdks/blob/develop/c/doc/devbox_setup.md#linux


