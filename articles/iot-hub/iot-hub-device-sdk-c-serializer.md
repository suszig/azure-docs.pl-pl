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


# Microsoft Azure の IoT デバイス SDK をシリアライザーに関する詳細情報の C:

[最初の記事](iot-hub-device-sdk-c-intro.md) 導入されたこのシリーズでは、 **c Azure IoT デバイス SDK**します。 次の記事のより詳細な説明を提供する、 [* * * * IoTHubClient](iot-hub-device-sdk-c-iothubclient.md)します。 この記事の残りのコンポーネントの詳細な説明を提供することにより、SDK のカバレッジが完了すると: **シリアライザー** ライブラリです。

導入の記事には、使用する方法が説明されている、 **シリアライザー** にイベントを送信して、IoT Hub からメッセージを受信するライブラリです。 この記事で使用してデータをモデル化する方法の詳細な説明を提供することで、ディスカッションを拡張、 **シリアライザー** マクロ言語です。 記事もライブラリがメッセージにシリアル化の詳細が含まれています (および場合によっては、シリアル化の動作を制御する方法)。 また、作成するモデルのサイズを決定するいくつかの変更可能なパラメーターについても説明します。

最後に、アーティクルでは、メッセージやプロパティの処理などの以前の記事で紹介されているいくつかのトピックを再び取り上げています。 いるという事実を同じこれら機能の動作を確認方法を使用して、 **シリアライザー** ライブラリが使用された場合、 **IoTHubClient** ライブラリです。

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

名前空間の内部で行われるのは、モデル定義です。 ここで、anemometer の 1 つのモデルがあります。 ここでも、何もモデルを指定することもできますが、通常この名前が付けられてデバイスまたは IoT Hub と交換するデータの種類。

モデルには、IoT Hub に入力できるイベントの定義 (*データ*) と IoT Hub から受信できるメッセージ (*アクション*) の定義が含まれます。 例からわかるように、イベントには、型と名前があります。アクションには、名前と省略可能なパラメーター (それぞれ型を持つ) があります。

このサンプルで示していないのは、SDK がサポートする追加のデータ型です。 これについては、次に説明します。
> [AZURE.NOTE] IoT Hub を指すように、デバイスが送信されるデータは *イベント*, モデリング言語を指すようになる一方で *データ* (を使用して定義 **WITH_DATA**)。 IoT Hub がデバイスに送信するデータを指す同様に、 *メッセージ*, モデリング言語を指すようになる一方で *アクション* (を使用して定義 **WITH_ACTION**)。 この記事でこれらの用語を区別しないで使用することがあります注意してください。

### サポートされているデータ型

次のデータの型は、**シリアライザー** ライブラリを使用して作成されたモデルでサポートされます。

| 型| 説明|
|-------------------------|----------------------------------------|
| double| 倍精度浮動小数点数|
| int| 32 ビット整数|
| float| 単精度浮動小数点数|
| long| long integer|
| int8\_t| 8 ビット整数|
| int16\_t| 16 ビット整数型|
| int32\_t| 32 ビット整数|
| int64\_t| 64 ビット整数|
| bool| ブール値|
| ascii\_char\_ptr| ASCII 文字列|
| EDM\_DATE\_TIME\_OFFSET| 日時オフセット|
| EDM\_GUID| GUID|
| EDM\_BINARY| binary|
| DECLARE\_STRUCT| 複合データ型|

最後のデータ型から説明していきます。  **DECLARE\_STRUCT** 他のプリミティブ型のグループの複雑なデータ型を定義することができます。 これらのグループ化は、次のようなモデルを定義することを許可します。

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

このモデルには、型 **TestType** のデータ イベントが 1 つだけ含まれています。 **TestType** 総称でサポートされているプリミティブの種類を示すいくつかのメンバーを含む複合型です、 **シリアライザー** モデリング言語です。

モデルでは、次のように、次のように表示される IoT Hub にデータを送信するコードを作成できます。

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

基本的には、値のメンバー全員に割り当てている、 **テスト** 構造し、呼び出す **SendAsync** を送信する、 **テスト** クラウドに移行するデータのイベントです。 **SendAsync** は、IoT Hub に 1 つのデータ イベントを送信するヘルパー関数です。

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

このコードを実行する場合、次のメッセージは、IoT Hub に送信されます。

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

生成された形式である JSON をシリアル化を確認して、 **シリアライザー** ライブラリです。 メンバーをシリアル化された JSON オブジェクトの各メンバーと一致することにも注意して、 **TestType** 私たちのモデルで定義しました。 値では、コードで使用されるものとも正確に一致します。 ただし、バイナリ データが base64 でエンコードされた:"AQID"は、base64 のエンコード {0x01、0x02、0x03} します。

次の例で使用する利点、 **シリアライザー** ライブラリ - これにより、アプリケーションでのシリアル化を明示的に処理することがなく JSON をクラウドに送信します。 必要なのは、このモデルにデータ イベントの値を設定し、クラウドにそれらのイベントを送信するためにシンプルな API を呼び出すことだけです。

この情報により、複雑な型の (おでしたも含めるその他の複合型内で複合型) を含む、サポートされているデータ型の範囲が含まれるモデルを定義できます。 ただし、彼にによって生成された JSON シリアル化された上記の例は、重要なポイントを表示します。 それは、**シリアライザー** ライブラリを使用したデータの送信*方法*によって、JSON の形式が決定されるということです。 このポイントについては、次で説明します。

## シリアル化の詳細

前のセクションでは、**シリアライザー** ライブラリによって生成される出力の例を紹介しました。 このセクションで、ライブラリがデータにシリアル化し、シリアル化 Api を使用してその動作を制御する方法を説明します。

シリアル化についての議論を進めるためには、サーモスタットに基づいた新しいモデルで操作を行います。 します。 最初に、アドレスを付けようとシナリオについて説明しましょう。

ここでモデル化するのは、気温と湿度を測定するサーモスタットです。 データの各部分が異なる方法で IoT Hub に送信しようとしています。 既定では、サーモスタット ingresses 温度イベント 2 分ごとです。湿度イベントは、15 分ごとに 1 回 ingressed。 どちらのイベントが ingressed、対応する温度、湿度を測定した時間を示すタイムスタンプを含める必要があります。

このシナリオを考慮して、データをモデル化する 2 つの方法を紹介し、効果を用いて説明モデリングがシリアル化された出力を持つことです。

### モデル 1

前のシナリオをサポートするモデルの最初のバージョンを次に示します。

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

モデルがデータの 2 つのイベントが含まれることに注意してください: **温度** と **湿度**します。 各イベントの種類を使用して定義された構造体を前の例とは異なり **DECLARE\_STRUCT**します。 **TemperatureEvent** には温度の測定値とタイムスタンプが含まれており、**HumidityEvent** には湿度の測定値とタイムスタンプが含まれています。 このモデルにより、自然な方法で上のシナリオのデータをモデル化できます。 クラウドに移行するイベントを送信したとき、温度/タイムスタンプまたは湿度/タイムスタンプ ペアか、送信されます。

温度イベント、次のようなコードを使用してクラウドに送信できます。

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

温度と湿度のサンプル コードにハード コーディングされた値を使用をしますが、サーモスタットに対応するセンサーをサンプリングすることによってこれらの値を実際に取得したことを想像してください。

上のコードでは、以前に紹介した **GetDateTimeOffset** ヘルパーを使用します。 後でチェック ボックスをオフになる理由から、このコードは、シリアル化して、イベントを送信するタスクを明示的に分離します。 前のコードは、温度のイベントをバッファーにシリアル化します。 次に、 **sendMessage** ヘルパー関数は、(に含まれる **simplesample\_amqp**) IoT Hub にイベントを送信します。

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

このコードは、前のセクションで説明した **SendAsync** ヘルパーのサブセットであるため、ここでは改めて説明しません。

前のコードを実行して Temperature イベントを送信する場合、このシリアル化されたイベントの形式は、IoT Hub に送信されます。

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

タイプの温度をお送りして **TemperatureEvent** し、その構造体を含む、 **温度** と **時間** メンバーです。 これは、シリアル化されたデータに直接反映されます。

同様に、次のコードで湿度イベントを送信することができます。

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

IoT Hub に送信されるシリアル化形式は次のとおりです。

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

これも、想定どおりの処理です。

このモデルで、簡単に他のイベントを追加できることがわかります。 使用して複数の構造を定義する **DECLARE\_STRUCT**, を使用してモデルに対応するイベントを含めると **WITH\_DATA**します。

これで、同じデータが含まれるようにが別の構造、モデルを変更しましょう。

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

少しの間だけ、**Time** イベントは無視します。 それを無視した場合、**Temperature** を受信するコードは次のとおりです。

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

And the code for sending the Humidity event appears as follows:
```
サーモスタット湿度]-> [= 45 です。
場合 (シリアル化 (& 湿度]-> [変換先、および destinationSize、サーモスタット) IOT_AGENT_OK = =)
{
    sendMessage (iotHubClientHandle、対象、destinationSize)
{
```

This code sends this to IoT Hub:
```
{"Humidity":45}
```

So far there are still no surprises. Now let's change how we use the SERIALIZE macro.

The **SERIALIZE** macro can take multiple data events as arguments. This enables us to serialize the **Temperature** and **Humidity** event together and send them to IoT Hub in one call:
```
場合 (シリアル化 (& -> 温度の宛先、および destinationSize、サーモスタット、サーモスタット湿度]-> [) IOT_AGENT_OK = =)
{
    sendMessage (iotHubClientHandle、対象、destinationSize)
{
```

You might guess that the result of this code is that two data events are sent to IoT Hub:

[

{"Temperature":75},

{"Humidity":45}

]

In other words, you might expect that this code is the same as sending **Temperature** and **Humidity** separately. It’s just a convenience to pass both events to **SERIALIZE** in the same call. However, that’s not the case. Instead, the code above sends this single data event to IoT Hub:

{"Temperature":75, "Humidity":45}

This may seem strange because our model defines **Temperature** and **Humidity** as two *separate* events:
```
DECLARE_MODEL (サーモスタット、
WITH_DATA (int、温度)
WITH_DATA (int、湿度)
WITH_DATA EDM_DATE_TIME_OFFSET (時間)
);
```

More to the point, we didn’t model these events where **Temperature** and **Humidity** are in the same structure:
```
DECLARE_STRUCT (TemperatureAndHumidityEvent、
int、温度、
int、湿度、
);

DECLARE_MODEL (サーモスタット、
WITH_DATA (TemperatureAndHumidityEvent、TemperatureAndHumidity)
);
```

If we used this model, it would be easier to understand how **Temperature** and **Humidity** would be sent in the same serialized message. However it may not be clear why it works that way when you pass both data events to **SERIALIZE** using model 2.

This behavior is easier to understand if you know the assumptions that the **serializer** library is making. To make sense of this let’s go back to our model:
```
DECLARE_MODEL (サーモスタット、
WITH_DATA (int、温度)
WITH_DATA (int、湿度)
WITH_DATA EDM_DATE_TIME_OFFSET (時間)
);
```

Think of this model in object-oriented terms. In this case we’re modeling a physical device (a thermostat) and that device includes attributes like **Temperature** and **Humidity**.

We can send the entire state of our model with code such as the following:
```
場合 (シリアル化 (& -> 温度の宛先、および destinationSize、サーモスタット、サーモスタット湿度]-> [、サーモスタット時間]-> [) IOT_AGENT_OK = =)
{
    sendMessage (iotHubClientHandle、対象、destinationSize)
{
```

Assuming the values of Temperature, Humidity and Time are set, we would see an event like this sent to IoT Hub:
```
{「温度」75、"湿度": 45、"Time":"2015-09-17T18:45:56Z"}。
```

Sometimes you may only want to send *some* properties of the model to the cloud (this is especially true if your model contains a large number of data events). It’s useful to send only a subset of data events, such as in our earlier example:
```
{「温度」75、"Time":"2015-09-17T18:45:56Z"}。
```

This generates exactly the same serialized event as if we had defined a **TemperatureEvent** with a **Temperature** and **Time** member, just as we did with model 1. In this case we were able to generate exactly the same serialized event by using a different model (model 2) because we called **SERIALIZE** in a different way.

The important point is that if you pass multiple data events to **SERIALIZE,** then it assumes each event is a property in a single JSON object.

The best approach depends on you and how you think about your model. If you’re sending "events" to the cloud and each event contains a defined set of properties, then the first approach makes a lot of sense. In that case you would use **DECLARE\_STRUCT** to define the structure of each event and then include them in your model with the **WITH\_DATA** macro. Then you send each event as we did in the first example above. In this approach you would only pass a single data event to **SERIALIZER**.

If you think about your model in an object-oriented fashion, then the second approach may suit you. In this case, the elements defined using **WITH\_DATA** are the "properties" of your object. You pass whatever subset of events to **SERIALIZE** that you like, depending on how much of your "object’s" state you want to send to the cloud.

Nether approach is right or wrong. Just be aware of how the **serializer** library works, and pick the modeling approach that best fits your needs.

## Message handling

So far this article has only discussed sending events to IoT Hub, and hasn't addressed receiving messages. The reason for this is that what we need to know about receiving messages has largely been covered in an [earlier article](iot-hub-device-sdk-c-intro.md). Recall from that article that you process messages by registering a message callback function:
```
IoTHubClient_SetMessageCallback (iotHubClientHandle IoTHubMessage、myWeather)
```

You then write the callback function that’s invoked when a message is received:
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

This implementation of **IoTHubMessage** calls the specific function for each action in your model. For example, if your model defines this action:
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

**SetAirResistance** is then called when that message is sent to your device.

What we haven't explained yet is what the serialized version of message looks like. In other words, if you want to send a **SetAirResistance** message to your device, what does that look like?

If you're sending a message to a device, you would do so through the Azure IoT service SDK. You still need to know what string to send to invoke a particular action. The general format for sending a message appears as follows:
```
{"Name":""、"Parameters":""}
```

You're sending a serialized JSON object with two properties: **Name** is the name of the action (message) and **Parameters** contains the parameters of that action.

For example, to invoke **SetAirResistance** you can send this message to a device:
```
{"Name":"SetAirResistance"、"Parameters": {「位置」: 5}}
```

The action name must exactly match an action defined in your model. The parameter names must match as well. Also note case sensitivity. **Name** and **Parameters** are always uppercase. Make sure to match the case of your action name and parameters in your model. In this example, the action name is "SetAirResistance" and not "setairresistance".

This section described everything you need to know when sending events and receiving messages with the **serializer** library. Before moving on, let's cover some parameters you can configure that control how large your model is.

## Macro configuration

If you’re using the **Serializer** library an important part of the SDK to be aware of is found here:
```
.\\c\\common\\tools\\macro\_utils\_h\_generator します。
```

This folder contains a Visual Studio solution called **macro\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

The program in this solution generates the **macro\_utils.h** file found in the .\\c\\common\\inc directory. There’s a default macro\_utils.h file included with the SDK. This solution allows you to modify some parameters and then recreate the header file based on these parameters.

The two key parameters to be concerned with are **nArithmetic** and **nMacroParameters** which are defined in these two lines found in macro\_utils.tt:
```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

These values are the default parameters included with the SDK. Each parameter has the following meaning:

-   nMacroParameters – Controls how many parameters you can have in one DECLARE\_MODEL macro definition.

-   nArithmetic – Controls the total number of members allowed in a model.

The reason these parameters are important is because they control how large your model can be. For example, consider this model definition:
```
DECLARE_MODEL (MyModel、
WITH_DATA (int、MyData)
);
```

As mentioned previously, **DECLARE\_MODEL** is just a C macro. The name of the model and the **WITH\_DATA** statement (yet another macro) are parameters of **DECLARE\_MODEL**. **nMacroParameters** defines how many parameters can be included in **DECLARE\_MODEL**. Effectively, this defines how many data event and action declarations you can have. As such, with the default limit of 124 this means that you can define a model with a combination of about 60 actions and data events. If you try to exceed this limit, you'll receive compiler errors that look similar to this:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

The **nArithmetic** parameter is more about the internal workings of the macro language than your application.  It controls the total number of members you can have in your model, including **DECLARE_STRUCT** macros. If you start seeing compiler errors such as this, then you should try increasing **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

If you want to change these parameters, modify the values in the macro\_utils.tt file, recompile the macro\_utils\_h\_generator.sln solution, and run the compiled program. When you do so, a new macro\_utils.h file is generated and placed in the .\\common\\inc directory.

In order to use the new version of macro\_utils.h, remove the **serializer** NuGet package from your solution and in its place include the **serializer** Visual Studio project. This enables your code to compile against the source code of the serializer library. This includes the updated macro\_utils.h. If you want to do this for **simplesample\_amqp**, start by removing the NuGet package for the serializer library from the solution:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Then add this project to your Visual Studio solution:

> .\\c\\serializer\\build\\windows\\serializer.vcxproj

When you're done, your solution should look like this:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Now when you compile your solution, the updated macro\_utils.h is included in your binary.

Note that increasing these values high enough can exceed compiler limits. To this point, the **nMacroParameters** is the main parameter with which to be concerned. The C99 spec specifies that a minimum of 127 parameters are allowed in a macro definition. The Microsoft compiler follows the spec exactly (and has a limit of 127), so you won't be able to increase **nMacroParameters** beyond the default. Other compilers might allow you to do so (for example, the GNU compiler supports a higher limit).

So far we've covered just about everything you need to know about how to write code with the **serializer** library. Before concluding, let's revisit some topics from previous articles that you may be wondering about.

## The lower-level APIs

The sample application on which this article focused is **simplesample\_amqp**. This sample uses the higher level (the non-"LL") APIs to send events and receive messages. If you use these APIs, a background thread runs which takes care of both sending events and receiving messages. However, you can use the lower level (LL) APIs to eliminate this background thread and take explicit control over when you send events or receive messages from the cloud.

As described in a [previous article](iot-hub-device-sdk-c-iothubclient.md), there is a set of functions that consists of the higher level APIs:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_Destroy

These APIs are demonstrated in **simplesample\_amqp**.

There is also an analogous set of lower level APIs.

-   IoTHubClient\_LL\_CreateFromConnectionString

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_Destroy

Note that the lower level APIs work exactly the same way as described in the previous articles. You can use the first set of APIs if you want a background thread to handle sending events and receiving messages. You use the second set of APIs if you want explicit control over when you send and receive data from IoT Hub. Either set of APIs work equally well with the **serializer** library.

For an example of how the lower level APIs are used with the **serializer** library, see the **simplesample\_http** application.

## Additional topics

A few other topics worth mentioning again are property handling, using alternate device credentials, and configuration options. These are all topics covered in a [previous article](iot-hub-device-sdk-c-iothubclient.md). The main point is that all of these features work in the same way with the **serializer** library as they do with the **IoTHubClient** library. For example, if you want to attach properties to an event from your model, you use **IoTHubMessage\_Properties** and **Map**\_**AddorUpdate**, the same way as described previously:
```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle) です。
sprintf_s (propText、sizeof(propText)、"%d"、i) です。
Map_AddOrUpdate (propMap"SequenceNumber"propText) です。
```

Whether the event was generated from the **serializer** library or created manually using the **IoTHubClient** library does not matter.

For the alternate device credentials, using **IoTHubClient\_LL\_Create** works just as well as **IoTHubClient\_CreateFromConnectionString** for allocating an **IOTHUB\_CLIENT\_HANDLE**.

Finally, if you're using the **serializer** library, you can set configuration options with **IoTHubClient\_LL\_SetOption** just as you did when using the **IoTHubClient** library.

A feature that is unique to the **serializer** library are the initialization APIs. Before you can start working with the library, you must call **serializer\_init**:
```
serializer_init(NULL) です。
```

This is done just before you call **IoTHubClient\_CreateFromConnectionString**.

Similarly, when you're done working with the library, the last call you’ll make is to **serializer\_deinit**:
```
serializer_deinit() です。
```

それ以外の場合、すべての上に示したその他の機能と同じ動作、 **シリアライザー** ライブラリでも、 **IoTHubClient** ライブラリです。 これらのトピックのいずれかの詳細については、次を参照してください。、 [前回の記事](iot-hub-device-sdk-c-iothubclient.md) このシリーズのです。

## 次のステップ

この記事で詳細に固有の側面について説明します、 **シリアライザー** に含まれているライブラリ、 **c Azure IoT デバイス SDK**します。 説明した情報により、モデルを使用してイベントを送信したり、IoT Hub からメッセージを受信したりする方法についての理解が深まります。

これでアプリケーションを開発する方法を 3 部構成シリーズでは終了も、 **c Azure IoT デバイス SDK**します。 これにより、使い始めるためだけでなく、Api のしくみを十分に理解を提供するための情報がなければなりません。 追加情報として、ここで取り上げなかった SDK のサンプルがいくつかあります。 それ以外の場合、 [SDK ドキュメント](https://github.com/Azure/azure-iot-sdks) は追加情報に関する優れたリソースです。





