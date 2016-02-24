> [AZURE.SELECTOR]
- [Windows 上の C](../articles/iot-suite/iot-suite-connecting-devices.md)
- [Linux での C](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C mbed に](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## シナリオの概要

このシナリオでは、リモートの監視に次のテレメトリを送信するデバイスを作成します [ソリューションを構成済み][lnk-what-are-preconfig-solutions]:

- 外部温度
- 内部温度
- 湿度

わかりやすくするために、デバイス上のコードではサンプル値を生成しますが、デバイスに実際のセンサーを接続し、実際のテレメトリを送信して、サンプルを拡張することをお勧めします。

## 開始する前に

デバイス用のコードを作成する前に、リモート監視構成済みソリューションをプロビジョニングし、そのソリューションにデバイスをプロビジョニングする必要があります。

### リモート監視構成済みソリューションをプロビジョニングする

作成するデバイスのインスタンスにデータが送信されます、 [リモート監視][lnk-remote-monitoring] ソリューションをあらかじめ構成されています。 参照してください [Azure IoT スイートを使ってみる][lnk-getstarted] を Azure のアカウントを作成して IoT スイートをプロビジョニングします。 選択 **リモート監視** 、新しいソリューションを作成する場合。

リモートの監視ソリューションがプロビジョニングされると、クリックして **起動** ソリューション ダッシュ ボードを開きます。

![][img-dashboard]

### リモート監視ソリューションでデバイスをプロビジョニングする

> [AZURE.NOTE] ソリューションで、デバイスが既にプロビジョニングされている場合は、この手順を省略できます。 クライアント アプリケーションを作成するときに、デバイスの資格情報が必要になります。

デバイスが構成済みソリューションに接続するには、デバイスが有効な資格情報を使用してそれ自体を識別できる必要があります。 ソリューション ダッシュボードからデバイスの資格情報を取得し、クライアント アプリケーションに含めることができます。 

新しいデバイスをリモート監視ソリューションに追加するには、ソリューション ダッシュボードで次の手順を実行します。

1.  ダッシュ ボードの左下隅で、クリックして **デバイスを追加する**です。

    ![][1]

2.   **カスタム デバイス** パネルで、をクリックして **新規追加**します。

    ![][2]

3.  選択 **自分で自分のデバイス ID を定義します**, 、デバイス ID を入力します。 **mydevice**, 、] をクリック **ID の確認** その名が使用中でないことを確認し **作成** 、デバイスをプロビジョニングします。

    ![][3]

5. デバイスの資格情報 (デバイス ID、IoT Hub ホスト名、デバイス キー) をメモしておきます。クライアント アプリケーションがデバイスをリモート監視ソリューションに接続する際に、この資格情報が必要になります。 クリックして **実行**します。

    ![][4]

6. デバイス セクションにデバイスが正しく表示されていることを確認します。 状態が **保留** デバイスがリモート監視ソリューションへの接続を確立するまでです。

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-getstarted]: http://www.microsoft.com/server-cloud/internet-of-things/getting-started.aspx
[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md

