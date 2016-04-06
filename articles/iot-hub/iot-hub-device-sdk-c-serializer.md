<properties
    pageTitle="C 用 Azure IoT デバイス SDK – シリアライザー | Microsoft Azure"
    description="C 用 Azure IoT device SDK に含まれているシリアライザー ライブラリの使用について説明します"
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

# C 用 Microsoft Azure IoT device SDK – シリアライザーの詳細

 [最初の記事](iot-hub-device-sdk-c-intro.md) 導入されたこのシリーズでは、 **c Azure IoT デバイス SDK**します。次の記事のより詳細な説明を提供する、 [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md)します。 この記事の残りのコンポーネントの詳細な説明を提供することにより、SDK のカバレッジが完了すると: **シリアライザー** ライブラリです。

導入の記事には、使用する方法が説明されている、 **シリアライザー** ライブラリへのイベントを送信し、IoT Hub からメッセージを受信します。 この記事で使用してデータをモデル化する方法の詳細な説明を提供することで、ディスカッションを拡張、 **シリアライザー** マクロ言語です。 また、ライブラリがメッセージをシリアル化する方法 (場合によっては、シリアル化の動作を制御する方法) についても詳しく説明します。 また、作成するモデルのサイズを決定するいくつかの変更可能なパラメーターについても説明します。

最後に、メッセージとプロパティの処理など、以前の記事で説明したトピックにも、もう一度触れます。 いるという事実を同じこれら機能の動作を確認方法を使用して、 **シリアライザー** ライブラリが使用された場合、 **IoTHubClient** ライブラリです。

この記事で説明されているすべてのものに基づきます、 **シリアライザー** SDK サンプルです。 理解するを参照して、 **simplesample\_amqp** と **simplesample\_http** C の Azure の IoT デバイス SDK に含まれるアプリケーション

## モデリング言語

 [の入門向け記事](iot-hub-device-sdk-c-intro.md) 導入されたこのシリーズでは、 **c Azure IoT デバイス SDK** モデリング言語で提供される例を **simplesample\_amqp** アプリケーション。

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

見るとわかるように、モデリング言語は、C マクロに基づいています。 常に、定義を開始する **BEGIN\_NAMESPACE** 、常に末尾に **END\_NAMESPACE**します。 通常は、名前空間には、会社か、またはこの例のように作業しているプロジェクトに関連した名前を付けます。

名前空間の内部で行われるのは、モデル定義です。 この例では、風速計の 1 つのモデルを使用します。 この場合もモデルに任意の名前を付けることができますが、通常は、デバイスまたは IoT Hub と交換するデータの種類に関連した名前を付けます。  

モデルには定義が含まれて、イベントの受信 IoT Hub を実行できます (、 *データ*) IoT Hub から受信できるメッセージだけでなく (、 *アクション*)。 例からわかるように、イベントには、型と名前があります。アクションには、名前と省略可能なパラメーター (それぞれ型を持つ) があります。

このサンプルで示していないのは、SDK がサポートする追加のデータ型です。 これについては、次に説明します。

> [AZURE.NOTE] IoT Hub を指すように、デバイスが送信されるデータは *イベント*, モデリング言語を指すようになる一方で *データ* (を使用して定義 **WITH_DATA**)。 IoT Hub がデバイスに送信するデータを指す同様に、 *メッセージ*, モデリング言語を指すようになる一方で *アクション* (を使用して定義 **WITH_ACTION**)。 この記事では、これらの用語が区別されずに使われる場合があります。

### サポートされているデータ型

作成されたモデルでは、次のデータ型がサポートされて、 **シリアライザー** ライブラリ。

| 型                    | 説明                            |
|-------------------------|----------------------------------------|
| double                  | 倍精度浮動小数点数 |
| int                     | 32 ビット整数                         |
| float                   | 単精度浮動小数点数 |
| long                    | 長整数                           |
| int8\_t                 | 8 ビット整数                          |
| int16\_t                | 16 ビット整数型                         |
| int32\_t                | 32 ビット整数                         |
| int64\_t                | 64 ビット整数                         |
| bool                    | boolean                                |
| ascii\_char\_ptr        | ASCII 文字列                           |
| EDM\_DATE\_TIME\_OFFSET | 日時オフセット                       |
| EDM\_GUID               | GUID                                   |
| EDM\_BINARY             | binary                                 |
| DECLARE\_STRUCT         | 複合データ型                      |

最後のデータ型から説明していきます。  **DECLARE\_STRUCT** 他のプリミティブ型のグループの複雑なデータ型を定義することができます。 これらのグループ化により、次のようなモデルを定義できます。

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

モデル完成させるにはには、型の 1 つのデータのイベントが含まれています。 **TestType**します。 **TestType** 総称でサポートされているプリミティブの種類を示すいくつかのメンバーを含む複合型です、 **シリアライザー** モデリング言語です。

このようなモデルを使用して、IoT Hub にデータを送信するためのコードを記述できます。以下に例を示します。

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

基本的には、値のメンバー全員に割り当てている、 **テスト** 構造し、呼び出す **SendAsync** を送信する、 **テスト** クラウドに移行するデータのイベントです。 **SendAsync** IoT Hub に 1 つのデータ イベントを送信するヘルパー関数です。

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

この関数は、指定されたデータ イベントをシリアル化し、それを使用して IoT Hub に送信 **IoTHubClient\_SendEventAsync**します。 これは、以前の記事で説明した同じコード (**SendAsync** 便利な関数にロジックをカプセル化) します。

上記のコードで使用されるその他の 1 つのヘルパー関数は、 **GetDateTimeOffset**します。 この関数は、指定された時間を型の値に変換 **EDM\_DATE\_TIME\_OFFSET**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

このコードを実行すると、次のメッセージが IoT Hub に送信されます。

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

生成された形式である JSON をシリアル化を確認して、 **シリアライザー** ライブラリです。 メンバーをシリアル化された JSON オブジェクトの各メンバーと一致することにも注意して、 **TestType** 私たちのモデルで定義しました。 値も、コードで使用されている値と正確に一致します。 ただし、バイナリ データが Base64 でエンコードされていることに注意してください。"AQID" は、{0x01、0x02、0x03} の Base64 エンコードです。

次の例で使用する利点、 **シリアライザー** ライブラリ - これにより、アプリケーションでのシリアル化を明示的に処理することがなく JSON をクラウドに送信します。 必要なのは、このモデルにデータ イベントの値を設定し、クラウドにそれらのイベントを送信するためにシンプルな API を呼び出すことだけです。

この情報を使用して、複合型など、サポートするデータ型の範囲が含まれるモデルを定義できます (複合型の中に別の複合型を含めることもできます)。 ただし、上の例によって生成されたシリアル化された JSON は、重要なポイントを提起します。 *どのように* でデータを送信して、 **シリアライザー** ライブラリでは、JSON の形式を正確を決定します。 このポイントについては、次で説明します。

## シリアル化の詳細

によって生成される出力の例を前のセクションが強調表示、 **シリアライザー** ライブラリです。 このセクションでは、ライブラリがデータをシリアル化する方法およびシリアル化 API を使用してその動作を制御する方法について説明します。

シリアル化に関する説明を進めるために、サーモスタットに基づく新しいモデルを使用します。 最初に、扱うシナリオの背景について説明します。

ここでモデル化するのは、気温と湿度を測定するサーモスタットです。 データの各部分は、さまざまな方法で IoT Hub に送信されます。 既定では、サーモスタットは温度イベントを 2 分ごとに受信します。また湿度イベントを 15 分ごとに受信します。 どちらかのイベントが受信されるときに、そのイベントには、対応する温度または湿度の測定時刻を表すタイムスタンプが含まれている必要があります。

このシナリオでは、2 つの異なる方法でデータをモデル化し、シリアル化された出力に対するモデル化の影響について説明します。

### モデル 1

ここでは、前のシナリオをサポートするモデルの最初のバージョンを示します。

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

モデルがデータの 2 つのイベントが含まれることに注意してください: **温度** と **湿度**します。 各イベントの種類を使用して定義された構造体を前の例とは異なり **DECLARE\_STRUCT**します。 **TemperatureEvent** 、温度の測定値と、タイムスタンプが含まれています **HumidityEvent** 湿度測定とタイムスタンプが含まれています。 このモデルにより、自然な方法で上のシナリオのデータをモデル化できます。 イベントをクラウドに送信するとき、温度/タイムスタンプのペア、または湿度/タイムスタンプのペアを送信します。

次のようなコードを使用して、クラウドに温度のイベントを送信できます。

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

サンプル コードではハード コーディングされた気温および湿度の値を使用しますが、実際にはサーモスタットの対応するセンサーをサンプリングしてこれらの値を取得することを想像してください。

使用上のコード、 **GetDateTimeOffset** 以前に導入されたヘルパーです。 後で理由を説明しますが、このコードは、明示的にイベントのシリアル化とイベントの送信を分離します。 前のコードは、バッファーに温度のイベントをシリアル化します。 次に、 **sendMessage** ヘルパー関数は、(に含まれる **simplesample\_amqp**) IoT Hub にイベントを送信します。

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

このコードのサブセットである、 **SendAsync** ヘルパーは触れません。 上にもう一度ここで、前のセクションで説明しています。

前のコードを実行して Temperature イベントを送信する場合、このシリアル化されたイベントの形式は、IoT Hub に送信されます。

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

タイプの温度をお送りして **TemperatureEvent** し、その構造体を含む、 **温度** と **時間** メンバーです。 これは、シリアル化されたデータに直接反映されます。

同様に、次のコードで湿度のイベントを送信できます。

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

IoT Hub に送信されるシリアル化された形式は、次のようになります。

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

これも、想定どおりの処理です。

このモデルで、簡単に他のイベントを追加できることがわかります。 使用して複数の構造を定義する **DECLARE\_STRUCT**, を使用してモデルに対応するイベントを含めると **WITH\_DATA**します。

今度は、別の構造体を使用して同じデータが含まれるように、モデルを変更します。

### モデル 2

上のモデルに代わる、この別のモデルについて考えてみます。

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

排除したらこの場合、 **DECLARE\_STRUCT** マクロと単純型、モデリング言語を使用して、シナリオからデータ項目を定義するだけです。

ここしばらくの間だけを無視する、 **時間** イベントです。 それはともかく、ここでは受信するためのコード **温度**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

このコードでは、次のシリアル化されたイベントを IoT Hub に送信します。

```
{"Temperature":75}
```

Humidity イベントを送信するためのコードは、次のようになります。

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

このコードは、イベントを IoT Hub に送信します。

```
{"Humidity":45}
```

ここまでも、想定どおりの処理です。 今度は、SERIALIZE マクロの使用方法を変更します。

 **SERIALIZE** マクロが引数として複数のデータ イベントを取得できます。 これにより、シリアル化する、 **温度** と **湿度** イベントをまとめて 1 回の呼び出しで IoT Hub に送信します。

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

このコードを実行すると、2 つのデータ イベントが IoT Hub に送信されると考える人もいるでしょう。

[

{"Temperature":75},

{"Humidity":45}

]

このコードは、送信と同じです。 つまり、想像 **温度** と **湿度** とは別にします。 両方のイベントに渡す便利なだけである **SERIALIZE** 同じ呼び出しで。 しかし、そうではありません。 上のコードは、次の 1 つのデータ イベントを IoT Hub に送信します。

{"Temperature":75, "Humidity":45}

これは、奇妙に思えるかもしれません私たちのモデルが定義されているため **温度** と **湿度** 2 と *別* イベント。

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

複数のポイントにしていないをモデル化これらのイベント、 **温度** と **湿度** 、同じ構造では。

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

理解しやすくなりますが、このモデルを使用した場合どのように **温度** と **湿度** 同じシリアル化されたメッセージで送信されます。 ただし、わかりにくいかもしれませんイベント イベントの両方のデータを渡すときに、その方法に機能 **SERIALIZE** モデル 2 を使用します。

この動作はご存知前提条件を理解しやすく、 **シリアライザー** ライブラリを作成します。 このことを理解するために、モデルに話を戻しましょう。

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

モデルをオブジェクト指向の用語を使って考えてみます。 ここでモデリングの物理デバイス (サーモスタット) と、そのデバイスなどの属性が含まれています。 **温度** と **湿度**します。

次のようなコードを使用して、モデル全体の状態を送信できます。

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Temperature、Humidity、Time の値が設定されていると仮定すると、次のようなイベントが IoT Hub に送信されます。

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

場合することのみを送信する *一部* (モデルに含まれるデータのイベントの数が多い場合に特にが) クラウド モデルのプロパティです。 そのような場合には、前の例のようにデータ イベントのサブセットのみを送信すると便利です。

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

私たちが定義されている場合とまったく同じシリアル化されたイベントが生成されます、 **TemperatureEvent** で、 **温度** と **時間** するとをモデル化 1 と同様に、メンバーです。 ここでは呼び出されるために、(モデル 2) とは異なるモデルを使用して、正確に同じシリアル化されたイベントを生成することでした **SERIALIZE** さまざまな方法です。

複数のデータ イベントを渡す場合の重要なポイントは **SERIALIZE、** 各イベントは、1 つの JSON オブジェクトのプロパティと想定し、します。

最適なアプローチは、モデルについての考え方によって異なります。 "イベント" をクラウドに送信し、定義された一連のプロパティを各イベントに含める場合は、最初のアプローチが非常に役立ちます。 使用する場合は **DECLARE\_STRUCT** を各イベントの構造を定義しを使用してモデルに組み込んで、 **WITH\_DATA** マクロです。 その後、上の最初の例で行ったように各イベントを送信します。 このアプローチでは、1 つのデータのイベントをのみに渡すと **シリアライザー**します。

オブジェクト指向でモデルを考える場合は、2 番目のアプローチが適切である可能性があります。 この場合、要素は定義を使用して **WITH\_DATA** は、オブジェクトの「プロパティ」です。 イベントをどのようなサブセットを渡す **SERIALIZE** をクラウドに送信する「オブジェクト」の状態の量に応じて、必要なことです。

どちらのアプローチも正しいとか、間違っているとかということはありません。 どのように注意してください **シリアライザー** ライブラリのしくみと、ニーズに最も合ったモデリング アプローチを選択します。

## メッセージの処理

これまで、この記事では IoT Hub への送信についてのみ説明し、メッセージの受信については取り上げてきませんでした。 理由は、これをメッセージの受信について知っておく必要がある主でも取り上げてきましたが、 [前の記事](iot-hub-device-sdk-c-intro.md)します。 メッセージを処理するには、メッセージのコールバック関数を登録するということを、その記事から思い出してください。

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

この実装の **IoTHubMessage** 、モデルの各アクションに対して特定の関数を呼び出します。 たとえば、モデルで次のアクションが定義されているとします。

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

**SetAirResistance** がそのメッセージは、デバイスに送信されるときに呼び出されます。

次のようなメッセージのシリアル化されたバージョンについては、まだ説明していませんでした。 つまり、送信する場合、 **SetAirResistance** デバイスへのメッセージであり、どのはそのようでしょうか。

デバイスにメッセージを送信する場合、Azure IoT サービス SDK を使用します。 特定のアクションを呼び出すために送信する文字列は知っておく必要があります。 メッセージを送信する一般的な形式は次のようになります。

```
{"Name" : "", "Parameters" : "" }
```

2 つのプロパティをシリアル化された JSON オブジェクトを送信する: **名前** アクション (メッセージ) の名前を指定し、 **パラメーター** そのアクションのパラメーターが含まれています。

たとえばを呼び出す **SetAirResistance** デバイスにこのメッセージを送信することができます。

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

アクション名は、モデルで定義されているアクションと正確に一致する必要があります。 パラメーター名も一致する必要があります。 大文字と小文字の区別にも注意してください。 **名前** と **パラメーター** は常に大文字です。 モデル内のアクションの名前とパラメーターの大文字と小文字が一致するようにしてください。 この例では、アクションの名前は "SetAirResistance" で、"setairresistance" ではありません。

このセクションでは、イベントの送信側と受信メッセージで知っておきたいすべて記載されている、 **シリアライザー** ライブラリです。 次に進む前に、モデルの大きさを制御する、構成可能ないくつかのパラメーターについて説明します。

## マクロの構成

使用している場合、 **シリアライザー** が認識する SDK の重要な部分は、こちらライブラリ。

```
.\\c\\common\\tools\\macro\_utils\_h\_generator.
```

このフォルダーには、Visual Studio のソリューションと呼ばれる **macro\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

このソリューションでプログラムを生成、 **macro\_utils.h** .\\c\\common\\inc ディレクトリにファイルが存在します。 SDK に含まれる既定 macro\_utils.h ファイルがあります。 このソリューションでは、いくつかのパラメーターを変更し、これらのパラメーターに基づいて、ヘッダー ファイルを再作成することができます。

気にする 2 つのキー パラメーター **nArithmetic** と **nMacroParameters** macro\_utils.tt については、次の 2 行で定義されています。

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

これらの値は、SDK に含まれている既定のパラメーターです。 各パラメーターには、次のような意味があります。

-   nMacroParameters – は、1 つの DECLARE\_MODEL マクロ定義内ではパラメーターの数を制御します。

-   nArithmetic – モデルで使用できるメンバーの合計数を制御します。

これらのパラメーターは、モデルの規模を制御するため重要です。 たとえば、次のモデルの定義について考えてみます。

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

前述の **DECLARE\_MODEL** C マクロだけです。 モデルの名前と **WITH\_DATA** ステートメント (まだ別のマクロ) のパラメーターを **DECLARE\_MODEL**します。 **nMacroParameters** 定義でパラメーターの数を含めることのできる **DECLARE\_MODEL**します。 実質的に、これによって使用可能なデータ イベントおよびアクション宣言の数が定義されます。 124 という既定の制限値の場合、約 60 個のアクションとデータ イベントの組み合わせを使用してモデルを定義できます。 この制限を超えようとすると、次のようなコンパイラ エラーが発生します。

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

 **NArithmetic** パラメーターは、アプリケーションよりもマクロ言語の内部処理に関する詳細。  モデルではメンバーの合計数が制御を含む **DECLARE_STRUCT** マクロです。 、このようなコンパイラ エラーの表示を開始するかどうかは、増やすことを試してください **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

これらのパラメーターを変更する場合は、macro\_utils.tt ファイル内の値を変更し、macro\_utils\_h\_generator.sln ソリューションを再コンパイル コンパイル済みプログラムを実行します。 これを行うと、新しい macro\_utils.h ファイルが生成され、.\\common\\inc ディレクトリに配置されます。

Macro\_utils.h の新しいバージョンを使用するのには、削除、 **シリアライザー** ソリューションとその場所に NuGet パッケージを含む、 **シリアライザー** Visual Studio プロジェクト。 こうすることで、シリアライザー ライブラリのソース コードをコンパイルできるようになります。 これには、更新された macro\_utils.h が含まれます。 これを行う場合 **simplesample\_amqp**, 、ソリューションからシリアライザー ライブラリの NuGet パッケージを削除することで開始します。

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

その後、Visual Studio ソリューションに次のプロジェクトを追加します。

> .\\c\\serializer\\build\\windows\\serializer.vcxproj

完了すると、ソリューションは次のようになります。

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

ここで、ソリューションをコンパイルするときに更新された macro\_utils.h は、バイナリに含まれています。

これらの値を十分に増やすと、コンパイラの制限を超えることができる点に注意してください。 この時点で、 **nMacroParameters** を考慮する主なパラメーターです。 C99 仕様では、マクロ定義内で最低 127 のパラメーターが指定できると規定しています。 Microsoft コンパイラは、仕様に完全に依存 (およびは 127 制限されています) を大きくことはできないので **nMacroParameters** 既定値を超える。 このことが可能なコンパイラもあります (たとえば、GNU コンパイラは、より大きな制限をサポートします)。

これまで説明したほぼすべてのコードを記述する方法について知っておくべき、 **シリアライザー** ライブラリです。 説明を終える前に、疑問となりそうな以前の記事のいくつかのトピックについて、もう一度説明します。

## 下位レベルの API

この記事の重点を置いたサンプル アプリケーションは、 **simplesample\_amqp**します。 このサンプルでは、上位レベル ("LL" 以外) の API を使用してイベントを送信し、メッセージを受信します。 これらの API を使用する場合、バックグラウンド スレッドが実行され、イベントの送信とメッセージの受信の両方を処理します。 ただし、下位レベル (LL) API を使用してこのバックグラウンド スレッドを排除し、イベントの送信またはクラウドからのメッセージの受信のタイミングを明示的に制御できます。

」の説明に従って、 [前回の記事](iot-hub-device-sdk-c-iothubclient.md), より高レベル Api で構成される一連の関数があります。

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_Destroy

これらの Api に示されて **simplesample\_amqp**します。

下位レベル API に類似したセットもあります。

-   IoTHubClient\_LL\_CreateFromConnectionString

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_Destroy

下位レベル API が、以前の記事での説明とまったく同じ動作をすることに注意してください。 イベントの送信とメッセージの受信を処理するバックグラウンド スレッドが必要な場合、最初の API のセットを使用できます。 データの送信と IoT Hub からのデータの受信のタイミングを明示的に制御する場合は、2 番目の API のセットを使用します。 いずれかの Api の作業のセットと同様、 **シリアライザー** ライブラリです。

低レベルの Api の使用方法の例については、 **シリアライザー** ライブラリを参照してください、 **simplesample\_http** アプリケーションです。

## 関連トピック

これ以外に、もう一度説明する必要があるトピックとして、プロパティの処理、代替デバイスの資格情報の使用、および構成オプションがあります。 これらは、すべてのトピックを取り上げます、 [前回の記事](iot-hub-device-sdk-c-iothubclient.md)します。 重要な点は、すべてこれらの機能のと同じ方法で機能する、 **シリアライザー** ライブラリが使用された場合、 **IoTHubClient** ライブラリです。 たとえば、モデルからプロパティをイベントにアタッチする場合は、使用する **IoTHubMessage\_Properties** と **マップ**\_**AddorUpdate**, 、前に説明と同様。

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

イベントが生成されたかどうか、 **シリアライザー** ライブラリを使用して手動で作成、または、 **IoTHubClient** ライブラリは関係ありません。

使用して、代替のデバイスの資格情報の **IoTHubClient\_LL\_Create** でもうまく機能として **IoTHubClient\_CreateFromConnectionString** を割り当てるため、 **IOTHUB\_CLIENT\_HANDLE**します。

最後を使用している場合、 **シリアライザー** ライブラリを構成オプションを設定する **IoTHubClient\_LL\_SetOption** 場合を使用する場合と同様、 **IoTHubClient** ライブラリです。

一意の機能、 **シリアライザー** ライブラリ、Api の初期化します。 ライブラリの操作を開始する前に呼び出す必要があります **serializer\_init**:

```
serializer_init(NULL);
```

これを呼び出す直前に **IoTHubClient\_CreateFromConnectionString**します。

同様に、完了したら、ライブラリを使用すれば、最後の呼び出しが、 **serializer\_deinit**:

```
serializer_deinit();
```

それ以外の場合、すべての上に示したその他の機能と同じ動作、 **シリアライザー** ライブラリでも、 **IoTHubClient** ライブラリです。 これらのトピックのいずれかの詳細については、次を参照してください。、 [前回の記事](iot-hub-device-sdk-c-iothubclient.md) このシリーズのです。

## 次のステップ

この記事で詳細に固有の側面について説明します、 **シリアライザー** に含まれているライブラリ、 **c Azure IoT デバイス SDK**します。説明した情報により、モデルを使用してイベントを送信したり、IoT Hub からメッセージを受信したりする方法についての理解が深まります。

これでアプリケーションを開発する方法を 3 部構成シリーズでは終了も、 **c Azure IoT デバイス SDK**します。この記事を読むことで、API の概要だけでなく、API のしくみについて理解するための十分な情報を得ることができます。 追加情報として、ここで取り上げなかった SDK のサンプルがいくつかあります。 それ以外の場合、 [SDK ドキュメント](https://github.com/Azure/azure-iot-sdks) は追加情報に関する優れたリソースです。


