<properties
    pageTitle="C 用 Azure IoT デバイス SDK の使用 | Microsoft Azure"
    description="C 用 Azure IoT デバイス SDK のサンプル コードについて説明し、操作を開始します。"
    services="iot-hub"
    documentationCenter=""
    authors="MichelBarnett"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="11/10/2015"
     ms.author="michelb"/>

# C 用 Azure IoT デバイス SDK の概要

 **Azure IoT デバイス SDK** 一連のライブラリからのメッセージを送受信するイベントを送信するためのプロセスを簡略化するためには、 **Azure IoT Hub** サービスです。 特定のプラットフォームを対象とする各 SDK のさまざまなバリエーションがありますが、この記事で説明、 **c Azure IoT デバイス SDK**します。

C 用 Azure IoT デバイス SDK は、移植性を最大限まで高めるために ANSI C (C99) で記述されています。 これにより、さまざまなプラットフォームとデバイス、特にディスクとメモリの量を最小限に抑えることが優先される環境で操作する場合に適しています。  

プラットフォーム SDK はテスト済みの範囲を広げてがある (を参照してください、 [SDK ドキュメント](https://github.com/Azure/azure-iot-sdks/tree/master/c) 詳細)。 この記事には、Windows プラットフォームで実行されるサンプル コードのチュートリアルが含まれますが、この記事で説明するコードはサポートされているプラットフォーム全体でまったく同じです。

この記事では、C 用 Azure IoT デバイス SDK のアーキテクチャについて紹介します。また、デバイスのライブラリを初期化する方法、IoT Hub にイベントを送信する方法や IoT Hub からメッセージを受信する方法の例を示します。 この記事の情報は、SDK を使用し始めるにあたり十分な内容ですが、ライブラリに関する追加情報を入手できる場所も紹介します。

## SDK のアーキテクチャ

検索することができます、 **c Azure IoT デバイス SDK** 次の GitHub リポジトリ内。

[azure-iot-sdks](https://github.com/Azure/azure-iot-sdks)

ライブラリの最新バージョンは記載されて、 **マスター** このリポジトリの分岐します。

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

このリポジトリには、Azure IoT デバイス SDK のファミリ全体が含まれています。 ただし、この記事では、Azure の IoT デバイス SDK について *c* 内に含まれている、 **c** フォルダーです。

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

SDK の中核となる実装を参照して、 **共通**, 、**iothub\_client**, 、および **シリアライザー** リポジトリ内のフォルダーです。  **共通** フォルダーには、ライブラリ全体で使用される共有コードが含まれています (通常のコードを使用しない、 **共通** フォルダーに直接)。 ただし、 **iothub\_client** と **シリアライザー** フォルダーには、コードで使用する SDK の 2 つの独立した層の実装が含まれています。

-   **IoTHubClient** - **iothub\_client** フォルダーには、SDK では、最下位の API 層の実装が含まれています。 **IoTHubClient** ライブラリです。  **IoTHubClient** ライブラリには、IoT Hub にイベントを送信するとメッセージを受信の Api が含まれています。 このライブラリを使用する場合、メッセージのシリアル化はユーザー自身が実装する必要がありますが、IoT Hub と通信するためのその他の詳細は自動で処理されます。

-   **シリアライザー** - **シリアライザー** フォルダーの実装を含む、 **シリアライザー** ライブラリです。  **シリアライザー** ライブラリによって提供される機能の上にモデリング機能を追加する **IoTHubClient**します。 使用する場合、 **シリアライザー** IoT Hub だけでなく、そこから受信するメッセージを送信するイベントを指定するモデルを定義することで開始するライブラリです。 いったんモデルを定義すれば、SDK が提供する API にアクセスして、シリアル化の詳細を気にすることなく、イベントやメッセージを簡単に処理できます。

これらはすべて、サンプル コードを確認した方が簡単に理解できます。 次のセクションでは、SDK に含まれているいくつかのサンプル アプリケーションについて説明します。 API の動作のしくみの概要だけでなく、SDK のアーキテクチャの層のさまざまな機能を理解するうえで役立つ内容です。

## サンプルを実行する前に

C 用 Azure IoT デバイス SDK でサンプルを実行する前に、開発環境の準備とデバイスの資格情報の取得の 2 つのタスクを完了する必要があります。  [Readme ファイル](https://github.com/Azure/azure-iot-sdks/tree/master/c) に含まれている、SDK には、両方のタスクに関する指示が用意されています。 次のセクションで、これらの手順について追加でコメントしています。

### 開発環境の準備

最初に、GitHub から SDK のコピーを入手して、ソースを作成する必要があります。 元のコピーを取得する必要があります、 **マスター** の分岐、 [GitHub リポジトリ](https://github.com/Azure/azure-iot-sdks)します。

ソースのコピーをダウンロードしたときに、SDK の記事で説明する手順を行う必要があります [開発環境を準備](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)します。 Windows が含まれる場合に実行する必要があります作業のほとんど [Qpid Proton ライブラリを準備する](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md#preparing-qpid-proton-libraries-in-windows), 、および [、環境の検証](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md#verify-your-environment)します。 ここでは、準備ガイドで説明されている手順を完了するのに役立ついくつかのヒントを紹介します。

-   作成するとき、 **PROTON\_PATH** 環境変数やすく、 **システム** 環境変数は、ここに示すようにします。

  ![](media/iot-hub-device-sdk-c-intro/07-EnvironmentVariables.PNG)

-   インストールすると、 **CMake** ユーティリティを追加するオプションを選択して **CMake** システム パスの **すべてのユーザー** (への追加 **、現在のユーザー** とように動作)。

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)

-   必ず、適切なバージョンの Python をインストールしてください。 Windows では通常、x86 バージョンをインストールします。

  ![](media/iot-hub-device-sdk-c-intro/09-Python.PNG)

インストーラーの既定のオプションを使用できます。

- Python のディレクトリをシステムに追加 **パス** 環境変数です。 Windows では、次のようになります。
- 
  ![](media/iot-hub-device-sdk-c-intro/16-PythonPath.PNG)

-   開く前に、 **VS2015 の開発者コマンド プロンプト**, 、Git コマンド ライン ツールをインストールします。 これらのツールをインストールするには、次の手順を実行します。

    1. 起動、 **Visual Studio 2015** セットアップ プログラム (または選択 **Microsoft Visual Studio 2015** から、 **プログラムと機能** コントロール パネル] を選択 **変更**)。
    
    2. 確認、 **Git for Windows** 機能は、インストーラーの選択されていますが、確認することも、 **GitHub Extension for Visual Studio** IDE の統合を提供するオプション。

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. セットアップ ウィザードを実行してツールをインストールします。

    4. Git ツールを追加する **bin** ディレクトリをシステムに **パス** 環境変数です。 Windows では、次のようになります。

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)

実行すると、 **build\_proton.cmd** コマンド プロンプトで、スクリプトからのスクリプトは、GitHub のリポジトリから Proton ソースをダウンロードし、Proton ライブラリをビルドします。

MQTT サンプルを実行する場合、指示に従って [Apache MQTT ライブラリ構築](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md#building-the-apache-mqtt-library-in-windows) SDK ドキュメントです。 実行する最後の処理を行う必要がありますに関係なく、 **build.cmd**します。

以上の手順が済んだら、サンプル アプリケーションをコンパイルする準備は完了です。

### デバイスの資格情報の取得

これで開発環境をセットアップできたので、最後にデバイスの資格情報のセットを取得します。  デバイスを IoT Hub にアクセスできるようにするには、まず、そのデバイスを IoT Hub デバイス レジストリに追加する必要があります。 デバイスを追加すると、そのデバイスを IoT Hub に接続するために必要な、デバイスの資格情報のセットが得られます。 次のセクションで紹介するサンプル アプリケーションでは、これらの資格情報を期待の形式で、 **デバイスの接続文字列**します。

 [デバイス エクスプ ローラー](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) ツール (Azure の IoT デバイス SDK に付属) ライブラリを使用して、Azure の IoT サービス デバイスを追加するなど、IoT Hub でさまざまな機能を実行します。 デバイス エクスプローラーを使用してデバイスを追加すると、対応する接続文字列が表示されます。 この接続文字列は、サンプル アプリケーションを実行するために必要です。

このプロセスにまだ詳しくない方のために、次の手順で、デバイス エクスプローラーを使用してデバイスを追加し、デバイスの接続文字列を取得する方法について説明します。

開いている **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** で **Visual Studio 2015** ソリューションをビルドします。 プログラムを実行すると、次のインターフェイスが表示されます。

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

入力、 **IoT Hub の接続文字列** 最初のフィールドをクリック **更新**します。 これにより、IoT Hub と通信できるようにツールが構成されます。

IoT Hub の接続文字列を構成したら、 **管理** ] タブをクリックします。

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

ここで、IoT Hub に登録されたデバイスを管理します。

デバイスを作成するをクリックすると、 **作成** ] ボタンをクリックします。 ダイアログ ボックスが、一連のキー (プライマリおよびセカンダリ) が入力された状態で表示されます。 入力行う必要があるは、 **デバイス ID** ] をクリックし、 **作成**します。

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

デバイスを作成したら、今作成したデバイスを含めたすべての登録済みデバイスで、デバイスの一覧が更新されます。 新しいデバイスを右クリックすると、次のメニューが表示されます。

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

選択した場合、 **選択したデバイスの接続文字列をコピー** オプションの場合は、デバイスが、クリップボードにコピーされたため、接続文字列。 接続文字列のコピーを保存しておきます。 次のセクションで説明するサンプル アプリケーションを実行するときにこれが必要になります。

前の手順が完了すると、コードを実行する準備が整います。 どちらのサンプルにも、メインのソース ファイルの上部に、接続文字列を入力するための定数が使用されています。 たとえば、対応するコード行から、 **iothub\_client\_sample\_amqp** アプリケーションが次のように表示されます。

```
static const char* connectionString = "[device connection string]";
```

ここに使用しているデバイスの接続文字列を入力し、ソリューションを再コンパイルすると、サンプルを実行することができます。

## IoTHubClient

内で、 **iothub\_client** フォルダー azure iot-sdk リポジトリでは、 **サンプル** という名前のアプリケーションを含むフォルダー **iothub\_client\_sample\_amqp**します。

Windows バージョン、 **iothub\_client\_sample\_ampq** アプリケーションには、次の Visual Studio ソリューションが含まれています。

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

このソリューションには単一のプロジェクトが含まれています。 このソリューションには 4 つの NuGet パッケージがインストールされていることに注意してください。

  ![](media/iot-hub-device-sdk-c-intro/17-iothub-client-sample-amqp-githubpackages.PNG)

常に必要な **Microsoft.Azure.IoTHub.Common** SDK を使用する場合にパッケージ化します。 このサンプルは、AMQP に依存するためも含める必要があります、 **Apache.QPID.Proton.AzureIoT** と **Microsoft.Azure.IoTHub.AmqpTransport** パッケージ (HTTP の場合と同じパッケージがあります)。 このサンプルを使用するため、 **IoTHubClient** ライブラリを含める必要があります、 **Microsoft.Azure.IoTHub.IoTHubClient** 、ソリューション内のパッケージです。

サンプル アプリケーションでの実装を見つけることができます、 **iothub\_client\_sample\_amqp.c** ソース ファイル。

  ![](media/iot-hub-device-sdk-c-intro/13-iothub_client_sample_amqp_c.PNG)

このサンプル アプリケーションを使用して何が必要な手順を使用して、 **IoTHubClient** ライブラリです。

### ライブラリの初期化

ライブラリで作業を開始するには、最初に、IoT Hub クライアントのハンドルを割り当てる必要があります。

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

この関数 (デバイス エクスプローラーから取得したもの) にデバイスの接続文字列のコピーを渡していることに注意してください。 また、使用するプロトコルを指定します。 この例では AMQP を使用しますが、HTTP も使用できます。

ある場合、有効な **IOTHUB\_CLIENT\_HANDLE**, 、イベントを送信して、IoT Hub からメッセージを受信する Api の呼び出しを開始することができます。 次で確認します。

### イベントの送信

IoT Hub にイベントを送信するには、次の手順を実行する必要があります。

まず、次のようにメッセージを作成します。

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

次に、次のようにメッセージを送信します。

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

メッセージを送信するたびに、データが送信されるときに呼び出されるコールバック関数への参照を指定します。

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

呼び出しに注意してください、 **IoTHubMessage\_Destroy** メッセージが終わったときに機能します。 この呼び出しを行って、メッセージを作成したときに割り当てられたリソースを解放する必要があります。

### メッセージの受信

メッセージの受信は非同期操作です。 最初に、デバイスがメッセージを受信したときに呼び出されるコールバックを登録します。

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

最後のパラメーターは任意のものを指す void ポインターです。 このサンプルのポインターは整数を指していますが、より複雑なデータ構造を指すこともできます。 これにより、このコールバック関数を、この関数の呼び出し元と共有した状態で操作できます。

デバイスがメッセージを受信すると、登録済みのコールバック関数が呼び出されます。

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

使用すること、 **IoTHubMessage\_GetByteArray** 文字列であるこの例では、メッセージを取得します。

### ライブラリの初期化解除

イベントの送信とメッセージの受信を完了すると、次の方法で IoT ライブラリの初期化を解除することができます。 これを行うには、次の関数呼び出しを発行します。

```
IoTHubClient_Destroy(iotHubClientHandle);
```

これに割り当てられているリソースが解放、 **IoTHubClient\_CreateFromConnectionString** 関数です。

簡単にイベントを送信して、ご覧のとおり、 **IoTHubClient** ライブラリです。 使用するプロトコルなど (開発者の観点からは単純な構成オプションです)、IoT Hub との通信に関する詳細情報は、ライブラリで処理されます。

 **IoTHubClient** ライブラリにも、デバイスは、IoT Hub に送信するイベントをシリアル化する方法を正確に制御が用意されています。 これが利点になる場合もありますが、この実装の詳細が関心の対象にならない場合もあります。 使用を検討している場合は、 **シリアライザー** ライブラリで、次のセクションで説明します。

## シリアライザー

概念的には、 **シリアライザー** ライブラリがの上位に位置、 **IoTHubClient** ライブラリ、SDK に含まれています。 使用して、 **IoTHubClient** IoT Hub ですが、基になる通信用のライブラリがある開発者からのメッセージのシリアル化処理の負担を削除するモデリング機能を追加します。 このライブラリの動作のわかりやすい例を示します。

内で、 **シリアライザー** azure iot-sdk リポジトリ内のフォルダーは、 **サンプル** という名前のアプリケーションを含むフォルダー **simplesample\_amqp**します。 このサンプルの Windows バージョンには、次の Visual Studio のソリューションが含まれます。

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

前のサンプルと同様、このソリューションにも、いくつかの NuGet パッケージが含まれています。

  ![](media/iot-hub-device-sdk-c-intro/18-simplesample_amqp-githubpackages.PNG)

前の例では、これらのほとんどは説明しましたが、 **Microsoft.Azure.IoTHub.Serializer** 機能します。 これは、使用するときに必要な **シリアライザー** ライブラリです。

サンプル アプリケーションでの実装を見つけることができます、 **simplesample\_amqp.c** ファイル。

  ![](media/iot-hub-device-sdk-c-intro/15-simplesample_amqp_c.PNG)

次のセクションで、このサンプルの主要な部分について説明します。

### ライブラリの初期化

操作を開始する、 **シリアライザー** ライブラリ、Api の初期化を呼び出す必要があります。

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

呼び出し、 **serializer\_init** 関数は 1 回限りの呼び出しであり、基になるライブラリを初期化するために使用します。 次を呼び出す、 **IoTHubClient\_CreateFromConnectionString** は in と同じ API 関数、 **IoTHubClient** サンプルです。 この呼び出しにより、デバイスに接続文字列が設定されます (使用するプロトコルを選択する場所でもあります)。 このサンプルでは、トランスポートとして AMQP を使用していますが、HTTP を使用することもできます。

最後を呼び出して、 **CREATE\_MODEL\_INSTANCE** 関数です。 注意してください **WeatherStation** は、モデルの名前空間と **ContosoAnemometer** 、モデルの名前を指定します。 モデルのインスタンスを作成したら、そのモデルを使用して、イベントの送信とメッセージの受信を開始することができます。 ただし、モデルがどのようなものかを理解する必要があります。

### モデルの定義

モデル、 **シリアライザー** ライブラリは、デバイスは、IoT Hub とという名前のメッセージを送信できるイベントを定義 *アクション* を受信できるモデリング言語でします。 C マクロのセットを使用してモデルを定義する、 **simplesample\_amqp** サンプル アプリケーション。

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

 **BEGIN\_NAMESPACE** と **END\_NAMESPACE** 両方のマクロが引数としてのモデルの名前空間をとります。 この 2 つのマクロの間には、モデルの定義とモデルが使用するデータ構造があることが想定されます。

この例では、1 つのモデルと呼ばれる **ContosoAnemometer**します。 このモデルは、デバイスは、IoT Hub に送信できる 2 つのイベントを定義します。 **DeviceId** と **WindSpeed**します。 さらに、デバイスが受信可能な 3 つのアクション (メッセージ) を定義します。 **TurnFanOn**, 、**TurnFanOff**, 、および **SetAirResistance**します。 各イベントには型があり、各アクションには名前 (また、必要に応じて一連のパラメーター) があります。

モデルで定義されたイベントとアクションは、IoT Hub へのイベントの送信とデバイスに送信されるメッセージへの応答に使用できる API へのアクセスを定義します。 これは例を使用すると理解しやすくなります。

### イベントの送信

このモデルは、IoT Hub に送信できるイベントを定義します。 この例ではつまり、2 つのイベントを使用して定義のいずれか、 **WITH_DATA** マクロです。 たとえば、送信する場合、 **WindSpeed** イベント IoT hub をいくつかの手順を実行する際にこのします。 最初に、送信するデータを設定します。

```
myWeather->WindSpeed = 15;
```

先ほど定義したモデルでは、これには、メンバーを設定することができる、 **構造体**します。 次に、送信するイベントをシリアル化します。

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

このコードがイベントをバッファーにシリアル化 (によって参照される **宛先**)。 最後に、次のコードで、イベントを IoT Hub に送信します。

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

これは、シリアル化されたイベントを IoT Hub に送信するサンプル アプリケーションのヘルパー関数です。

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

このコードはで見たとよく似ていますが、 **iothub\_client\_sample\_amqp** をバイト配列からメッセージを作成し、使用しているアプリケーション **IoTHubClient\_SendEventAsync** IoT Hub に送信します。 その後で必要なのは、以前に割り当てたメッセージ ハンドルとシリアル化されたデータ バッファーの解放のみです。

2 番目の最後のパラメーターに **IoTHubClient\_SendEventAsync** データが正常に送信されるときに呼び出されるコールバック関数への参照です。 コールバック関数の例を次に示します。

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

2 番目のパラメーターは、ユーザー コンテキストへのポインター渡された同じポインター **IoTHubClient\_SendEventAsync**します。 ここでは、このコンテキストは単純なカウンターですが、任意の内容にすることができます。

イベントの送信に必要なものは以上です。 残りは、メッセージを受信する方法の説明のみです。

### メッセージの受信

メッセージの動作を同じように受信方向のメッセージでも、 **IoTHubClient** ライブラリです。 まず、メッセージのコールバック関数を登録します。

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

そのうえで、メッセージを受信したときに呼び出されるコールバック関数を記述します。

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

このコードは定型句であるため、すべてのソリューションで同じになります。 この関数は、メッセージを受信し、適切な関数を呼び出すことによってへのルーティングを行います **EXECUTE\_COMMAND**します。 この時点で呼び出す関数は、モデルのアクションの定義によって異なります。

モデルのアクションを定義するとき、対応するメッセージをデバイスが受信するときに呼び出される関数を実装する必要があります。 たとえば、モデルで次のアクションが定義されているとします。

```
WITH_ACTION(SetAirResistance, int, Position)
```

この場合、次のシグネチャを持つ関数を定義する必要があります。

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

関数の名前はモデル内のアクションの名前に一致し、関数のパラメーターはアクションに対して指定されたパラメーターに一致することに注意してください。 最初のパラメーターは常に必要であり、モデルのインスタンスへのポインターが含まれます。

デバイスがこのシグネチャと一致するメッセージを受信すると、対応する関数が呼び出されます。 したがってとは別の定型コードを指定する **IoTHubMessage**, 、メッセージの受信は、モデルで定義されている各アクションに対して単純な関数を定義するだけです。

### ライブラリの初期化解除

データの送信とメッセージの受信を完了すると、次の方法で IoT ライブラリの初期化を解除することができます。

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

この 3 つの各関数は、前に説明した 3 つの初期化関数に対応しています。 これらの API を呼び出すと、以前に割り当てたリソースが確実に解放されます。

## 次のステップ

この記事でライブラリの使用の基本を説明する、 **c Azure IoT デバイス SDK**します。SDK の内容、そのアーキテクチャ、Windows サンプルの実行を開始する方法を理解するうえで十分な情報を提供しました。 次の記事を説明する、SDK の説明を続行 [IoTHubClient ライブラリに関する詳細](iot-hub-device-sdk-c-iothubclient.md)します。


