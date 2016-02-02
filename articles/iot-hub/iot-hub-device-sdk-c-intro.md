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

**Azure IoT デバイス SDK** は、**Azure IoT Hub** サービスとのイベントの送信とメッセージの受信のプロセスを簡略化するために設計された一連のライブラリです。 特定のプラットフォームを対象とする各 SDK のさまざまなバリエーションがありますが、この記事で説明、 **c Azure IoT デバイス SDK**します。

C 用 Azure IoT デバイス SDK は、移植性を最大限まで高めるために ANSI C (C99) で記述されています。 これによって、適してディスクを最小限に抑えることが特にさまざまなプラットフォームおよびデバイスに動作するようになり、メモリ使用量は、優先度。

プラットフォーム SDK はテスト済みの範囲を広げてがある (を参照してください、 [SDK ドキュメント](https://github.com/Azure/azure-iot-sdks/tree/master/c) 詳細)。 この記事には、Windows プラットフォームで実行されているサンプル コードのチュートリアルが含まれますは、この記事で説明したコードであることに留意してください、サポートされているプラットフォームの範囲でまったく同じです。

この記事では、C 用 Azure IoT デバイス SDK のアーキテクチャについて紹介します。また、デバイスのライブラリを初期化する方法、IoT Hub にイベントを送信する方法や IoT Hub からメッセージを受信する方法の例を示します。 この記事の情報は、だけで、SDK の使用を開始する必要がありますが、ライブラリに関する追加情報へのポインターも提供します。

## SDK のアーキテクチャ

次の GitHub リポジトリ内で **C 用 Azure IoT デバイス SDK** を見つけることができます。

[azure の iot-sdk](https://github.com/Azure/azure-iot-sdks)

最新バージョンのライブラリは、このリポジトリの **master** ブランチにあります。

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

このリポジトリには、Azure IoT デバイス SDK のファミリ全体が含まれています。 ただし、この記事では、Azure の IoT デバイス SDK について *c* 内に含まれている、 **c** フォルダーです。

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

SDK の中核となる実装を参照して、 **共通**, 、**iothub\_client**, 、および **シリアライザー** リポジトリ内のフォルダーです。 **common** フォルダーには、ライブラリ全体で使用される共有コードが格納されています (通常、**common** フォルダーのコードは直接使用しないでください)。 ただし、 **iothub\_client** と **シリアライザー** フォルダーには、コードで使用する SDK の 2 つの独立した層の実装が含まれています。

-   **IoTHubClient** - **iothub\_client** フォルダーには、SDK では、最下位の API 層の実装が含まれています。 **IoTHubClient** ライブラリです。 **IoTHubClient** ライブラリには、IoT Hub にイベントを送信するだけでなく IoT Hub からメッセージを受信する API が含まれます。 このライブラリを使用する場合はメッセージのシリアル化を実装する責任が、IoT Hub との通信には、その他の詳細処理されます。

-   **serializer**: **serializer** フォルダーには、**serializer** ライブラリの実装が格納されています。 **serializer** ライブラリは、**IoTHubClient** が提供する機能の上にモデリング機能を追加します。 **serializer** ライブラリを使用する場合は、まず、IoT Hub から受信するメッセージだけでなく IoT Hub に送信するイベントを指定するモデルを定義します。 モデルを定義すると、SDK にイベントおよびメッセージのシリアル化の詳細について心配することがなく簡単に処理することができます API サーフェイスが提供します。

これはすべてのコード例を見て、理解しやすくします。 次のセクションでは、SDK に含まれているいくつかのサンプル アプリケーションについて説明します。 API の動作のしくみの概要だけでなく、SDK のアーキテクチャの層のさまざまな機能を理解するうえで役立つ内容です。

## サンプルを実行する前に

C 用 Azure IoT デバイス SDK でサンプルを実行する前に、開発環境の準備とデバイスの資格情報の取得の 2 つのタスクを完了する必要があります。  [Readme ファイル](https://github.com/Azure/azure-iot-sdks/tree/master/c) に含まれている、SDK には、両方のタスクに関する指示が用意されています。 次のセクションにには、これらの命令にいくつか追加のコメントが含まれます。

### 開発環境の準備

最初に、GitHub から SDK のコピーを入手して、ソースを作成する必要があります。 元のコピーを取得する必要があります、 **マスター** の分岐、 [GitHub リポジトリ](https://github.com/Azure/azure-iot-sdks)します。

ソースのコピーをダウンロードしたときに、SDK の記事で説明する手順を行う必要があります [開発環境を準備](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)します。 Windows が含まれる場合に実行する必要があります作業のほとんど [Qpid Proton ライブラリを準備する](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md#preparing-qpid-proton-libraries-in-windows), 、および [、環境の検証](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md#verify-your-environment)します。 準備ガイドで説明する手順を完了するのに役立ついくつかのヒントを次に示します。

-   作成するとき、 **PROTON\_PATH** 環境変数やすく、 **システム** 環境変数は、ここに示すようにします。

  ![](media/iot-hub-device-sdk-c-intro/07-EnvironmentVariables.PNG)

-   **CMake** ユーティリティをインストールするとき、**すべてのユーザー**の PATH システム環境変数に **CMake** を追加するオプションを選択します (**現在のユーザー**に追加しても効果は同じです)。

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)

-   必ず、適切なバージョンの Python をインストールしてください。 Windows では通常、x86 バージョンをインストールします。

  ![](media/iot-hub-device-sdk-c-intro/09-Python.PNG)

インストーラーの既定のオプションを使用できます。

-Python のディレクトリをシステムに追加する **パス** 環境変数です。Windows では、次のようになります。
-------------------------------------------------------------

  ![](media/iot-hub-device-sdk-c-intro/16-PythonPath.PNG)

-   開く前に、 **VS2015 の開発者コマンド プロンプト**, 、Git コマンド ライン ツールをインストールします。 これらのツールをインストールするには、次の手順を実行します。

    1. **Visual Studio 2015** セットアップ プログラムを起動します (または **[プログラムと機能]** コントロール パネルの **[Microsoft Visual Studio 2015]** を選択して **[変更]** をクリックします)。

    2. 確認、 **Git for Windows** 機能は、インストーラーの選択されていますが、確認することも、 **GitHub Extension for Visual Studio** IDE の統合を提供するオプション。

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. セットアップ ウィザードを実行してツールをインストールします。

    4. Git ツールの **bin** ディレクトリを **PATH** システム環境変数に追加します。 Windows では、次のようになります。

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)

実行すると、 **build\_proton.cmd** コマンド プロンプトで、スクリプトからのスクリプトは、GitHub のリポジトリから Proton ソースをダウンロードし、Proton ライブラリをビルドします。

MQTT サンプルを実行する場合、指示に従って [Apache MQTT ライブラリ構築](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md#building-the-apache-mqtt-library-in-windows) SDK ドキュメントです。 最後は必ず **build.cmd** を実行します。

以上の手順が済んだら、サンプル アプリケーションをコンパイルする準備は完了です。

### デバイスの資格情報の取得

これで開発環境をセットアップできたので、最後にデバイスの資格情報のセットを取得します。 IoT hub にアクセスできるデバイスの場合は、まず IoT Hub デバイス レジストリに、デバイスを追加する必要があります。 デバイスを追加すると、そのデバイスを IoT Hub に接続するために必要な、デバイスの資格情報のセットが得られます。 次のセクションで確認するサンプル アプリケーションでは、これらの資格情報は**デバイスの接続文字列**の形式であると想定されています。

[デバイス エクスプ ローラー](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) ツール (Azure の IoT デバイス SDK に付属) ライブラリを使用して、Azure の IoT サービス デバイスを追加するなど、IoT Hub でさまざまな機能を実行します。 デバイス エクスプローラーを使用してデバイスを追加すると、対応する接続文字列が表示されます。 この接続文字列は、サンプル アプリケーションを実行するために必要です。

このプロセスにまだ詳しくない方のために、次の手順で、デバイス エクスプローラーを使用してデバイスを追加し、デバイスの接続文字列を取得する方法について説明します。

開いている * *[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)* * [ **Visual Studio 2015** ソリューションをビルドします。 プログラムを実行すると、次のインターフェイスが表示されます。

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

入力、 **IoT Hub の接続文字列** 最初のフィールドをクリック **更新**します。 これにより、IoT Hub と通信できるようにツールが構成されます。

IoT Hub の接続文字列を構成したら、 **管理** ] タブをクリックします。

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

ここで、IoT Hub に登録されたデバイスを管理します。

**[作成]** をクリックしてデバイスを作成することができます。 ダイアログ ボックスには、あらかじめ設定されているキー (プライマリおよびセカンダリ) のセットが表示されます。 入力行う必要があるは、 **デバイス ID** ] をクリックし、 **作成**します。

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

デバイスが作成されたら、先ほど作成した 1 つを含むすべての登録済みデバイスとデバイスの一覧が更新されます。 新しいデバイスを右クリックした場合は、このメニューが表示されます。

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

選択した場合、 **選択したデバイスの接続文字列をコピー** オプションの場合は、デバイスが、クリップボードにコピーされたため、接続文字列。 接続文字列のコピーを保持します。 次のセクションで説明されているサンプル アプリケーションを実行するときに必要があります。

前の手順が完了すると、コードを実行する準備が整います。 どちらのサンプルを使用する接続文字列を入力するとメイン ソース ファイルの上部にある定数であることです。 たとえば、対応するコード行から、 **iothub\_client\_sample\_amqp** アプリケーションが次のように表示されます。

```
static const char* connectionString = "[device connection string]";
```

ここにご使用のデバイスの接続文字列を入力し、ソリューションを再コンパイルすると、サンプルを実行することができます。

## IoTHubClient

内で、 **iothub\_client** フォルダー azure iot-sdk リポジトリでは、 **サンプル** という名前のアプリケーションを含むフォルダー **iothub\_client\_sample\_amqp**します。

Windows バージョン、 **iothub\_client\_sample\_ampq** アプリケーションには、次の Visual Studio ソリューションが含まれています。

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

このソリューションには単一のプロジェクトが含まれています。 このソリューションではインストールされている 4 つの NuGet パッケージがあることに注意しています。

  ![](media/iot-hub-device-sdk-c-intro/17-iothub-client-sample-amqp-githubpackages.PNG)

SDK を使った作業には必ず **Microsoft.Azure.IoTHub.Common** パッケージが必要となります。 このサンプルは、AMQP に依存するためも含める必要があります、 **Apache.QPID.Proton.AzureIoT** と **Microsoft.Azure.IoTHub.AmqpTransport** パッケージ (HTTP の場合と同じパッケージがあります)。 このサンプルを使用するため、 **IoTHubClient** ライブラリを含める必要があります、 **Microsoft.Azure.IoTHub.IoTHubClient** 、ソリューション内のパッケージです。

サンプル アプリケーションでの実装を見つけることができます、 **iothub\_client\_sample\_amqp.c** ソース ファイル。

  ![](media/iot-hub-device-sdk-c-intro/13-iothub_client_sample_amqp_c.PNG)

このサンプル アプリケーションを使用して、**IoTHubClient** ライブラリの使用に必要な内容を説明します。

### ライブラリの初期化

ライブラリで作業を開始するには、最初に、IoT Hub クライアントのハンドルを割り当てる必要があります。

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

(のいずれかのデバイスのエクスプ ローラーから取得した) この関数に、デバイスの接続文字列のコピーを渡しているだけことに注意してください。 また、使用するプロトコルを指定します。 この例では AMQP を使用しますが、HTTP も使用できます。

ある場合、有効な **IOTHUB\_CLIENT\_HANDLE**, 、イベントを送信して、IoT Hub からメッセージを受信する Api の呼び出しを開始することができます。 次で確認します。

### イベントの送信

IoT Hub にイベントを送信するには、次の手順を完了することが必要です。

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

呼び出しに注意してください、 **IoTHubMessage\_Destroy** メッセージが終わったときに機能します。 メッセージの作成時に割り当てられたリソースを解放するには、この呼び出しを行う必要があります。

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

送信側のイベントとメッセージの受信が完了したら、ときに、IoT ライブラリの初期化を解除することができます。 これを行うには、次の関数呼び出しを発行します。

```
IoTHubClient_Destroy(iotHubClientHandle);
```

This frees up the resources previously allocated by the **IoTHubClient\_CreateFromConnectionString** function.

As you can see, it’s easy to send events and receive messages with the **IoTHubClient** library. The library handles the details of communicating with IoT Hub, including which protocol to use (from the perspective of the developer, this is a simple configuration option).

The **IoTHubClient** library also provides precise control over how to serialize the events your device sends to IoT Hub. In some cases this is an advantage, but in other cases this is an implementation detail with which you don’t want to be concerned. If that's the case, you might consider using the **serializer** library, which we'll describe in the next section.

## Serializer

Conceptually the **serializer** library sits on top of the **IoTHubClient** library in the SDK. It uses the **IoTHubClient** library for the underlying communication with IoT Hub, but it adds modeling capabilities that remove the burden of dealing with message serialization from the developer. How this library works is best demonstrated by an example.

Within the **serializer** folder in the azure-iot-sdks repository is a **samples** folder that contains an application called **simplesample\_amqp**. The Windows version of this sample includes the following Visual Studio solution:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

As with the previous sample, this one includes several NuGet packages:

  ![](media/iot-hub-device-sdk-c-intro/18-simplesample_amqp-githubpackages.PNG)

We've seen most of these in the previous sample, but **Microsoft.Azure.IoTHub.Serializer** is new. This is required when we use the **serializer** library.

You can find the implementation of the sample application in the **simplesample\_amqp.c** file:

  ![](media/iot-hub-device-sdk-c-intro/15-simplesample_amqp_c.PNG)

The following sections walk you through the key parts of this sample.

### Initializing the library

To start working with the **serializer** library, you must call the initialization APIs:
```
serializer_init(NULL) です。

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString (connectionString、AMQP_Protocol) です。

ContosoAnemometer * myWeather = CREATE_MODEL_INSTANCE (WeatherStation、ContosoAnemometer) です。
```

The call to the **serializer\_init** function is a one-time call and is used to initialize the underlying library. Then, you call the **IoTHubClient\_CreateFromConnectionString** function, which is the same API as in the **IoTHubClient** sample. This call sets your device connection string (this is also where you choose the protocol you want to use). Note that this sample uses AMQP as the transport, but could have used HTTP.

Finally, call the **CREATE\_MODEL\_INSTANCE** function. Note that **WeatherStation** is the namespace of the model and **ContosoAnemometer** is the name of the model. Once the model instance is created, you can use it to start sending events and receiving messages. However, it's important to understand what a model is.

### Defining the model

A model in the **serializer** library defines the events that your device can send to IoT Hub and the messages, called *actions* in the modeling language, which it can receive. You define a model using a set of C macros as in the **simplesample\_amqp** sample application:
```
BEGIN_NAMESPACE(WeatherStation) です。

DECLARE_MODEL (ContosoAnemometer、
WITH_DATA (ascii_char_ptr、DeviceId)
WITH_DATA (double、風速)
WITH_ACTION(TurnFanOn)、
WITH_ACTION(TurnFanOff)、
WITH_ACTION (SetAirResistance、int、位置)
);

END_NAMESPACE(WeatherStation) です。
```

The **BEGIN\_NAMESPACE** and **END\_NAMESPACE** macros both take the namespace of the model as an argument. It’s expected that anything between these macros is the definition of your model(s) and the data structures that the models use.

In this example, there is a single model called **ContosoAnemometer**. This model defines two events that your device can send to IoT Hub: **DeviceId** and **WindSpeed**. It also defines three actions (messages) that your device can receive: **TurnFanOn**, **TurnFanOff**, and **SetAirResistance**. Each event has a type, and each action has a name (and optionally a set of parameters).

The events and actions defined in the model define an API surface that you can use to send events to IoT Hub, as well as respond to messages being sent to the device. This is best understood through an example.

### Sending events

The model defines the events that you can send to IoT Hub. In this example, that means one of the two events defined using the **WITH_DATA** macro. For example, if you want to send a **WindSpeed** event to an IoT hub, there are a few steps involved in making this happen. The first is to set the data we want to send:
```
myWeather WindSpeed]-> [= 15 です。
```

The model we defined earlier allows us to do this by setting a member of a **struct**. Next, we serialize the event we want to send:
```
unsigned char * 先です。
size_t destinationSize です。

シリアル化 (& WindSpeed]-> [変換先、および destinationSize、myWeather) です。
```

This code serializes the event to a buffer (referenced by **destination**). Finally, we’ll send the event to IoT hub with this code:
```
sendMessage (iotHubClientHandle、対象、destinationSize)
```

This is a helper function in the sample application that sends our serialized event to IoT Hub:
```
静的の void sendMessage (IOTHUB_CLIENT_HANDLE iotHubClientHandle、const char * が署名されていないバッファー、size_t サイズ)
{
    静的な符号なし int messageTrackingId です。
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray バッファー (サイズ) です。
    場合 (messageHandle! = NULL)
    {
        場合 (IoTHubClient_SendEventAsync (iotHubClientHandle、messageHandle、sendCallback、(void*)(uintptr_t)messageTrackingId)! = IOTHUB_CLIENT_OK)
        {
            printf ("failed IoTHubClient を受け取り、メッセージの上に") です。
        {
        else
        {
            printf ("IoTHubClient がメッセージを受け付けた delivery\r\n の") です。
        {

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;

{
```

This code is very similar to what we saw in the **iothub\_client\_sample\_amqp** application, in which we created a message from a byte array and then used **IoTHubClient\_SendEventAsync** to send it to IoT Hub. After that we just have to free the message handle and serialized data buffer we allocated earlier.

The second to last parameter of **IoTHubClient\_SendEventAsync** is a reference to a callback function that’s called when the data is successfully sent. Here's an example of a callback function:
```
sendCallback (IOTHUB_CLIENT_CONFIRMATION_RESULT 結果、void * userContextCallback) を無効にします。
{
    int messageTrackingId = (intptr_t) userContextCallback です。

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);
    
    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));

{
```

The second parameter is a pointer to user context; the same pointer we passed to **IoTHubClient\_SendEventAsync**. In this case the context is a simple counter, but it can be anything you want.

That's all there is to sending events. The only thing left to cover is how to receive messages.

### Receiving messages

Receiving a message works similarly to the way messages work in the **IoTHubClient** library. First, you register a message callback function:
```
IoTHubClient_SetMessageCallback (iotHubClientHandle IoTHubMessage、myWeather)
```

Then, you write the callback function that's invoked when a message is received:
```
静的 IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT 結果です。
    const char * が署名されていないバッファーです。
    size_t サイズです。
    場合 (IoTHubMessage_GetByteArray (メッセージ、&、バッファーのサイズ)! = IOTHUB_MESSAGE_OK)
    {
        printf ("IoTHubMessage_GetByteArray\r\n"できません)。
        結果 = EXECUTE_COMMAND_ERROR です。
    {
    else
    {
        /*バッファーが 0 で終了ではない*/
        char * temp = malloc (サイズ + 1) です。
        場合 (temp NULL = =)
        {
            printf ("failed to malloc\r\n") です。
            結果 = EXECUTE_COMMAND_ERROR です。
        {
        else
        {
            memcpy (temp、バッファー サイズ、)。
            temp [サイズ] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = 事; 全体の (userContextCallback、一時)
            結果 =
                (executeCommandResult EXECUTE_COMMAND_ERROR = =) ですか? IOTHUBMESSAGE_ABANDONED:
                (executeCommandResult EXECUTE_COMMAND_SUCCESS = =) ですか? IOTHUBMESSAGE_ACCEPTED:
                IOTHUBMESSAGE_REJECTED です。
            free(temp) です。
        {
    {
    結果を返す
{
```

This code is boilerplate -- it's the same for any solution. This function receives the message and takes care of routing it to the appropriate function through the call to **EXECUTE\_COMMAND**. The function called at this point depends on the definition of the actions in our model.

When you define an action in your model, you're required to implement a function that's called when your device receives the corresponding message. For example, if your model defines this action:
```
WITH_ACTION (SetAirResistance、int、位置)
```

You must define a function with this signature:
```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void) デバイスです。
    (void) printf (「空気抵抗位置の設定に %d.\r\n」、位置)。
    EXECUTE_COMMAND_SUCCESS; を返す
{
```

Note that the name of the function matches the name of the action in the model and that the parameters of the function match the parameters specified for the action. The first parameter is always required and contains a pointer to the instance of our model.

When the device receives a message that matches this signature, the corresponding function is called. Therefore, aside from having to include the boilerplate code from **IoTHubMessage**, receiving messages is just a matter of defining a simple function for each action defined in your model.

### Uninitializing the library

When you’re done sending data and receiving messages, you can uninitialize the IoT library:
```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);

{
serializer_deinit() です。
```

この 3 つの各関数は、前に説明した 3 つの初期化関数に対応しています。 これらの API を呼び出すと、以前に割り当てたリソースが確実に解放されます。

## 次のステップ

この記事でライブラリの使用の基本を説明する、 **c Azure IoT デバイス SDK**します。 SDK に含まれるものを理解するのに十分な情報の提供されているアーキテクチャ、および Windows サンプルの実行を開始する方法です。 次の記事を説明する、SDK の説明を続行 [IoTHubClient ライブラリに関する詳細](iot-hub-device-sdk-c-iothubclient.md)します。





