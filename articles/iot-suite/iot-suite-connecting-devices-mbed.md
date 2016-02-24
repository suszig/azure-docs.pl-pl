<properties
   pageTitle="mbed で C を使用してデバイスを接続する |Microsoft Azure"
   description="C で記述され、mbed デバイスで実行されるアプリケーションを使用して、デバイスを Azure IoT Suite 構成済みリモート監視ソリューションに接続する方法について説明します。"
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

## mbed で C のサンプル ソリューションをビルドして実行する

次の手順は、接続するための手順を説明する [Freescale の FRDM K64F を mbed が有効な][lnk-mbed-home] デバイスをリモート監視ソリューションです。

### デバイスをネットワークおよびデスクトップ コンピューターに接続する

1. イーサネット ケーブルを使用して mbed デバイスをネットワークに接続する方法を説明します。 サンプル アプリケーションでは、インターネットへのアクセスが必要なため、この手順は必須です。

2. 参照してください [mbed 入門][lnk-mbed-getstarted] mbed デバイスをデスクトップ PC に接続します。

3. デスクトップ PC が Windows を実行している場合は、次を参照してください。 [PC 構成][lnk-mbed-pcconnect] mbed デバイスへのシリアル ポートへのアクセスを構成します。

### mbed プロジェクトを作成してサンプル コードをインポートする

1. Web ブラウザーで、mbed.org に移動 [開発者向けサイト](https://developer.mbed.org/)します。 サインアップしていない場合は、新しいアカウントを作成するオプションが表示されます (アカウントの作成は無料です)。 既にサインアップしている場合は、アカウントの資格情報を使用してログインします。 クリックし、 **コンパイラ** ページの右上隅にします。 これにより、ワークスペース管理インターフェイスが表示されます。

2. 使用しているハードウェア プラットフォームがウィンドウの右上隅に表示されていることを確認するか、右上隅にあるアイコンをクリックしてハードウェア プラットフォームを選択します。

3. クリックして **インポート** メイン メニューでします。 クリックし、 **ここをクリックして** mbed 地球ロゴの横にある URL] リンクからインポートします。

    ![][6]

4. ポップアップ ウィンドウで、サンプル コード https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/のリンクを入力します

    ![][7]

5. mbed コンパイラでは、このプロジェクトをインポートしたことでさまざまなライブラリがインポートされたことを確認できます。 いくつかについては、Azure の IoT チームで管理されている ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), 、[iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), 、[iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), 、[iothub_http_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_http_transport/), 、[proton c-mbed](https://developer.mbed.org/users/AzureIoTClient/code/proton-c-mbed/)) mbed ライブラリ カタログの使用可能なサード パーティ製のライブラリには他のユーザーです。

    ![][8]

6. remote_monitoring\remote_monitoring.c を開き、ファイル内で次のコードを見つけます。

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. [Device Id] と [Device Key] を自身のデバイス データに置き換えます。

8. デバイス データである IoT Hub ホスト名を使用して、IoTHub 名と IoTHub サフィックスを設定します。 たとえば、IoT Hub ホスト名が Contoso.azure-devices.net である場合は、Contoso が IoTHub 名、残りの部分がサフィックスになります。

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "Contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### プログラムをビルドして実行する

1. クリックして **コンパイル** プログラムをビルドします。 警告は無視してかまいません。ただし、ビルドでエラーが発生する場合は、続行する前にそのエラーを修正してください。

2. ビルドが成功すると、プロジェクト名の付いた .bin ファイルが生成されます。 .bin ファイルをデバイスにコピーします。 .bin ファイルをデバイスに保存すると、デバイスに対する現在のターミナル セッションがリセットされます。 再接続したら、もう一度ターミナルを手動でリセットするか、新しいターミナルを開始します。 これにより、mbed デバイスはプログラムの実行をリセットして開始することができます。

3. PuTTY などの SSH クライアント アプリケーションを使用して、デバイスに接続します。 Windows デバイス マネージャーを確認すると、デバイスで使用されているシリアル ポートを特定できます。


4. PuTTY をクリックして、 **シリアル** 接続の種類。 最も可能性の高いデバイス 115200 に接続すると、内でその値のように入力、 **速度** ボックス。 クリックして **開く**:

    ![][11]

5. プログラムの実行が開始されます。 接続時にプログラムが自動的に開始されない場合は、ボードのリセットが必要になることがあります (Ctrl キーを押しながら Break キーを押すか、ボードのリセット ボタンを押します)。

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration


