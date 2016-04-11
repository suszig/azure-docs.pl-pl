<properties
    pageTitle="C 用 Azure IoT デバイス SDK – IoTHubClient | Microsoft Azure"
    description="C 用 Azure IoT device SDK に含まれている IoTHubClient ライブラリの使用について説明します"
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

# C 用 Microsoft Azure IoT デバイス SDK – IoTHubClient の詳細

 [最初の記事](iot-hub-device-sdk-c-intro.md) 導入されたこのシリーズでは、 **C の Microsoft Azure の IoT デバイス SDK**します。 その記事では、SDK に 2 つのアーキテクチャの層が存在することを説明しました。 ベースでは、 **IoTHubClient** IoT Hub との通信を直接管理するライブラリです。  **シリアライザー** シリアル化サービスを提供するその上にビルドするライブラリです。 この記事の内容をご案内追加の詳細で、 **IoTHubClient** ライブラリです。

前回の記事には、使用する方法が説明されている、 **IoTHubClient** IoT Hub にイベントを送信し、メッセージを受信するライブラリです。 この記事より厳密に管理する方法を説明するによって、ディスカッションを拡張 *と* 、導入すると、データを送受信する、 **下位レベルの Api**します。 プロパティをイベントにアタッチ (およびメッセージを元に戻す) 方法についても説明で機能を処理するプロパティを使用して、 **IoTHubClient** ライブラリです。 最後に、IoT Hub から受信したメッセージの別の処理方法をいくつか追加で紹介します。

終了すると、いくつかのデバイスの資格情報との動作を変更する方法に関する詳細など、その他のトピックをカバーするが、記事、 **IoTHubClient** 構成オプションを使用します。

使用して、 **IoTHubClient** SDK のサンプルの次のトピックについて説明します。 理解するを参照して、 **iothub\_client\_sample\_http** と **iothub\_client\_sample\_amqp** C. すべての次のセクションで説明されているは、これらのサンプルで示すは、Azure の IoT デバイス SDK に含まれているアプリケーションです。

## 下位レベルの API

基本的な操作が前回の記事に記載されている、 **IotHubClient** のコンテキスト内で、 **iothub\_client\_sample\_amqp** アプリケーションです。 たとえば、次のコードを使用してライブラリを初期化する方法について説明しました。

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

次の関数呼び出しを使用してイベントを送信する方法についても説明しました。

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

この記事では、コールバック関数を登録することでメッセージを受信する方法についても説明しました。

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

この記事では、次のようなコードを使用してリソースを解放する方法も示しました。

```
IoTHubClient_Destroy(iotHubClientHandle);
```

ただし、これらの API にはそれぞれ次のコンパニオン関数があります。

-   IoTHubClient\_LL\_CreateFromConnectionString

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_Destroy


これらの関数の API 名にはすべて "LL" が使用されています。 それ以外は、これらの各関数のパラメーターは、LL が付いていないその対応するものと同じです。 ただし、これらの関数の動作は、1 つの重要な点で異なります。

呼び出すと **IoTHubClient\_CreateFromConnectionString**, 、基になるライブラリは、バック グラウンドで実行されている新しいスレッドを作成します。 このスレッドは、IoT Hub にイベントを送信し、IoT Hub ハブからメッセージを受信します。 "LL" API を使用するときは、このようなスレッドは作成されません。 バックグラウンド スレッドが作成されるのは、開発者にとって便利です。 IoT Hub とのイベントの送信とメッセージの受信を明示的に実行することについて心配する必要はありません。自動的にバックグラウンドで実行されます。 これに対して、"LL" API では、必要に応じて IoT Hub との通信を明示的に制御することができます。

理解を深めるために、例を確認してみましょう。

呼び出すと **IoTHubClient\_SendEventAsync**, 、バッファー内のイベントを挿入実際に行っています。 呼び出すときに作成されたバック グラウンド スレッド **IoTHubClient\_CreateFromConnectionString** 継続的にこのバッファーを監視し、それに含まれるデータ IoT Hub に送信します。 これは、メイン スレッドが他の作業を実行しているときに同時にバックグラウンドで実行されます。

使用してメッセージのコールバック関数を登録すると同様に、 **IoTHubClient\_SetMessageCallback**, 、メッセージがメイン スレッドは関係なしに受信したときに、コールバック関数を呼び出すバック グラウンド スレッドに SDK に指示しています。

"LL" API では、バックグラウンド スレッドは作成されません。 代わりに、新しい API を呼び出し、明示的にデータを送信して IoT Hub からデータを受信する必要があります。 これを次の例に示します。

 **Iothub\_client\_sample\_http** SDK に含まれているアプリケーションは、低レベルの Api を示します。 そのサンプルでは、次のようなコードを使用して、イベントを IoT Hub に送信します。

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

最初の 3 行でメッセージを作成して、最後の 1 行でイベントを送信します。 ただし、先ほど説明したように、イベントの "送信" は、データが単にバッファーに配置されることを意味します。 呼び出すときにネットワーク上で転送が何も **IoTHubClient\_LL\_SendEventAsync**します。 IoT Hub にデータを実際に受信するには、呼び出す必要があります **IoTHubClient\_LL\_DoWork**, 、この例のようにします。

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

このコード (から、 **iothub\_client\_sample\_http** アプリケーション) を繰り返し呼び出す **IoTHubClient\_LL\_DoWork**します。 毎回 **IoTHubClient\_LL\_DoWork** が呼び出されると、バッファーから IoT Hub にいくつかのイベントを送信し、デバイスに送信されるキューに置かれたメッセージを取得します。 後者では、メッセージのコールバック関数が登録されている場合、(メッセージがキューに登録されていれば) そのコールバック関数が呼び出されます。 次のようなコードを使用して、そのようなコールバック関数を登録しています。

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

理由を **IoTHubClient\_LL\_DoWork** 一般的に、送信、ループが呼び出されるたび、 *いくつか* IoT Hub にイベントをバッファーし、取得 *次* デバイスのメッセージ キューに追加します。 呼び出すたびに、バッファリングされたすべてのイベントが送信され、キューに登録されたすべてのメッセージが取得されるという保証はありません。 バッファー内のすべてのイベントを送信してから他の処理を続けるには、前のループ処理を次のようなコードで置き換えます。

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

このコードは呼び出し **IoTHubClient\_LL\_DoWork** IoT Hub をバッファー内のすべてのイベントが送信されるまでです。 これは、キューのすべてのメッセージが受信されたことを意味するわけではありません。 その理由の 1 つは、すべてのメッセージを確認することは、決定性のあるアクションではないためです。 メッセージをすべて取得した直後に、別のものがデバイスに送信された場合を考えてください。 より優れた処理方法に、プログラムでタイムアウトを設定する方法があります。 たとえば、メッセージのコールバック関数を使用すると、呼び出すたびに、タイマーをリセットできます。 、たとえば、メッセージが受信されていない場合、最後の処理を続行するためのロジックを記述することができますし、 *X* 秒です。

イベントの入力とメッセージの受信が完了したら、該当する関数を必ず呼び出してリソースをクリーンアップしてください。

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

基本的に、バックグラウンド スレッドを使用してデータを送受信する API のセットが 1 つのみと、バックグラウンド スレッドを使用せず同じ動作を実行する別の API のセットがあります。 多くの開発者は LL でない API を好みますが、下位レベルの API は開発者がネットワーク転送を明示的に制御する場合に便利です。 たとえば、一部のデバイスは時間をかけてデータを収集しますが、指定された間隔 (1 時間に 1 回や 1 日に 1 回など) でのみイベントを入力します。 下位レベルの API を使用すると、IoT Hub からデータを送受信するタイミングを明確に制御できるようになります。 下位レベルの API が提供する単純さを好む開発者もいるでしょう。 バック グラウンドでいくつかの動作が行われるのではなく、すべてがメイン スレッドで発生します。

どちらのモデルを選択するにしても、使用する API に一貫性を持たせる必要があります。 呼び出しを開始する場合 **IoTHubClient\_LL\_CreateFromConnectionString**, 、フォロー アップ作業用のみに対応する低レベルの Api を使用することを確認します。

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_Destroy

-   IoTHubClient\_LL\_DoWork

逆の場合も同様です。 使用する場合 **IoTHubClient\_CreateFromConnectionString**, 、さらに処理を非 LL Api を使用します。

SDK c 言語用の Azure の IoT デバイスで、次を参照してください。、 **iothub\_client\_sample\_http** 低レベルの Api の完全な例のアプリケーションです。  **Iothub\_client\_sample\_amqp** アプリケーションは、非-LL Api の完全な例を参照できます。

## プロパティの処理

これまでデータ送信の説明の際は、メッセージの本文に言及してきました。 たとえば、次のコードを検討してみましょう。

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

この例では、テキスト "Hello World" を使用して IoT Hub にメッセージを送信します。 ただし、IoT Hub では、各メッセージにプロパティを添付することもできます。 プロパティは、名前と値のペアのことで、メッセージに添付することができます。 たとえば、前のコードを変更して、メッセージにプロパティを添付することができます。

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

まずを呼び出して **IoTHubMessage\_Properties** し、メッセージのハンドルを渡すことです。 返される、 **MAP\_HANDLE** プロパティの追加を開始することにより、参照します。 後者は呼び出すことにより実現 **Map\_AddOrUpdate**, 、MAP\_HANDLE、プロパティ名、およびプロパティの値への参照を取得します。 この API を使用すると、必要な数のプロパティを追加できます。

イベントの読み取り時に **Event Hub**, 、受信側はプロパティを列挙し、その対応する値を取得します。 たとえば、.NET でこれは、行われるにアクセスして、 [EventData オブジェクトのプロパティ コレクション](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx)します。

前の例では、IoT Hub に送信するイベントにプロパティを添付しています。 プロパティは IoT Hub から受信するメッセージに添付することもできます。 メッセージからプロパティを取得する必要がある場合は、メッセージのコールバック関数で次のようなコードを使用できます。

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

呼び出し **IoTHubMessage\_Properties** 返します、 **MAP\_HANDLE** 参照します。 渡しますへの参照をその **Map\_GetInternals** 名前/値ペア (およびそのプロパティの数) の配列への参照を取得します。 この時点で、プロパティを列挙して必要な値を取得するだけの簡単な処理です。

アプリケーションでプロパティを使用する必要はありません。 ただし、イベント上で設定したり、メッセージを元に戻す必要がある場合、 **IoTHubClient** ライブラリを使えば簡単です。

## メッセージの処理

既に説明したように、メッセージが到着したときから IoT Hub、 **IoTHubClient** ライブラリが登録されたコールバック関数を呼び出すことによって応答します。 この関数には追加で説明する必要がある戻り値のパラメーターがあります。 コールバック関数での抜粋を次に示します、 **iothub\_client\_sample\_http** サンプル アプリケーション。

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

戻り値の型は **IOTHUBMESSAGE\_DISPOSITION\_RESULT** とが返されます。 このケースで **IOTHUBMESSAGE\_ACCEPTED**します。 変更するその他の値がこの関数から返されますが、どのように **IoTHubClient** ライブラリは、メッセージのコールバックに反応します。 返される値は次のとおりです。

-   **IOTHUBMESSAGE\_ACCEPTED** – メッセージが正常に処理されました。  **IoTHubClient** ライブラリでは、同じメッセージを使用してコールバック関数は呼び出されません。

-   **IOTHUBMESSAGE\_REJECTED** メッセージは処理されませんでした: したくなるは、将来はありません。  **IoTHubClient** ライブラリは、同じメッセージを使用してコールバック関数を呼び出していない必要があります。

-   **IOTHUBMESSAGE\_ABANDONED** – メッセージが正常に処理されませんでしたが、 **IoTHubClient** ライブラリは、同じメッセージを使用してコールバック関数を呼び出す必要があります。

最初の 2 つの戻りコードに、 **IoTHubClient** ライブラリは、メッセージをデバイスのキューから削除して、再度配信されないことを示すの IoT Hub にメッセージを送信します。 実質的な効果は同じ (デバイスのキューからメッセージを削除する) ですが、メッセージが受け入れられるか拒否するかも記録されます。  この違いを記録することは、メッセージの送信側がフィードバックに注目し、デバイスが特定のメッセージを受け入れるか拒否するかを調べる際に役立ちます。

最後の場合もメッセージは IoT Hub に送信されますが、これは、メッセージを再配信する必要があることを示しています。 いくつかのエラーが発生したがメッセージをもう一度処理する必要がある場合、通常はメッセージを破棄します。 これに対して、回復不可能なエラーが発生した場合 (またはメッセージを処理しないことを単に決定した場合) は、メッセージを拒否するのは適切なことです。

いずれの場合は、注意するさまざまなリターン コードのようにから目的の動作を引き出すことができます、 **IoTHubClient** ライブラリです。

## 代替デバイスの資格情報

以前は、最初に使用するときにすることを説明するよう、 **IoTHubClient** ライブラリは取得するため、 **IOTHUB\_CLIENT\_HANDLE** 呼び出し、次のようにします。

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

引数を **IoTHubClient\_CreateFromConnectionString** のデバイスや IoT Hub との通信に使用しているプロトコルを示すパラメーターの接続文字列します。 接続文字列の形式は次のとおりです。

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

この文字列は IoT Hub 名、IoT Hub サフィックス、デバイス ID、共有アクセス キーの 4 つの情報で構成されます。 Azure ポータルで IoT Hub インスタンスを作成するときに、IoT Hub の完全修飾ドメイン名 (FQDN) を取得します。これにより、IoT Hub 名 (FQDN の最初の部分) と IoT Hub サフィックス (FQDN の残りの部分) を取得できます。 IoT Hub にデバイスを登録すると、デバイス ID と共有アクセス キーを取得する (」の説明に従って、 [前回の記事](iot-hub-device-sdk-c-intro.md))。

**IoTHubClient\_CreateFromConnectionString** ライブラリを初期化するために 1 つの方法で表示できます。 新規に作成することを行う場合 **IOTHUB\_CLIENT\_HANDLE** 接続文字列ではなく、これら個別のパラメーターを使用しています。 これは次のコードで実現します。

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

同じ処理を行うこの **IoTHubClient\_CreateFromConnectionString**します。

使用する場合、明らかなように思えるかもしれませんが **IoTHubClient\_CreateFromConnectionString** 初期化の詳細なこのメソッドではなく。 ただし、IoT Hub にデバイスを登録するときに取得するのは、(接続文字列ではなく) デバイス ID とデバイス キーであることを留意してください。  **デバイス マネージャー** で導入された SDK ツール、 [前回の記事](iot-hub-device-sdk-c-intro.md) でライブラリを使用して、 **Azure の IoT サービス SDK** デバイス ID、デバイスのキーおよび IoT Hub のホスト名からの接続文字列を作成します。 これを呼び出す **IoTHubClient\_LL\_Create** 接続文字列を生成する手順を保存することをお勧め場合があります。 いずれかの便利な方法を使用してください。

## 構成オプション

これまでにすべての方法について説明されている、 **IoTHubClient** ライブラリ機能には、既定の動作が反映されます。 このライブラリの動作を変更するために設定できるオプションがいくつかあります。 これを行うに活用することで、 **IoTHubClient\_LL\_SetOption** API です。 次の例を考えてみましょう。

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

よく使用されるオプションがいくつかあります。

-   **SetBatching** (bool) – 場合 **true**, 、IoT Hub に送信されるデータがバッチで送信されます。 場合 **false**, 、メッセージが個別に送信されます。 既定値は **false**します。

-   **タイムアウト** (符号なし int) – この値はミリ秒単位で表されます。 HTTP 要求の送信や応答の受信にこの時間より長くかかる場合は、接続がタイムアウトします。

バッチ処理オプションは重要です。 既定では、ライブラリ ingresses イベント個別に (1 つのイベントに渡す **IoTHubClient\_LL\_SendEventAsync**)。 バッチ処理オプションが場合 **true**, 、ライブラリは、(最大 IoT Hub が受け入れるメッセージの最大サイズ) バッファーから可能な限り多くのイベントを収集します。  イベントのバッチは、(個々のイベントが JSON 配列にまとめられて) 単一の HTTP 呼び出しで IoT Hub に送信されます。 バッチ処理を有効にすると、通常はネットワーク ラウンドトリップが減少するため、パフォーマンスの大幅な向上につながります。 個別の各イベントの一連のヘッダーではなく、イベント バッチで一連の HTTP ヘッダーが送信されるため、帯域幅が大幅に削減されます。 それ以外の方法で実行する特別な理由がない限り、一般的にバッチ処理を有効にします。

## 次のステップ

この記事で詳しくの動作について説明します、 **IoTHubClient** 内で見つかったライブラリ、 **c Azure IoT デバイス SDK**します。 この情報によりの機能を十分に理解が必要、 **IoTHubClient** ライブラリです。  [次回の記事](iot-hub-device-sdk-c-serializer.md) 上のような詳細情報を提供、 **シリアライザー** ライブラリです。

