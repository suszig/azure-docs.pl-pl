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

[最初の記事](iot-hub-device-sdk-c-intro.md) 導入されたこのシリーズでは、 **C の Microsoft Azure の IoT デバイス SDK**します。 その記事では、SDK に 2 つのアーキテクチャの層が存在することを説明しました。 その基礎となるのが、IoT Hub との通信を直接管理する **IoTHubClient** ライブラリです。 これを土台にしシリアル化サービスを提供する **serializer** ライブラリもあります。 この記事では、**IoTHubClient** ライブラリについてさらに詳しく説明します。

前の記事では、**IoTHubClient** ライブラリを使用して IoT Hub にイベントを送信してメッセージを受信する方法を説明しました。 この記事では、**下位レベルの API** を紹介し、データを送受信する*タイミング*をより厳密に管理する方法について掘り下げて説明します。 **IoTHubClient** ライブラリでプロパティ処理機能を使用してプロパティをイベントに添付する (およびプロパティをメッセージから取得する) 方法についても説明します。 最後に、IoT Hub から受信したメッセージの別の処理方法をいくつか追加で紹介します。

この記事は、デバイスの資格情報の詳細や構成オプションで **IoTHubClient** の動作を変更する方法など、その他のトピックの説明で締めくくります。

**IoTHubClient** SDK のサンプルを使用して、これらのトピックを説明します。 理解するを参照して、 **iothub\_client\_sample\_http** と **iothub\_client\_sample\_amqp** C. すべての次のセクションで説明されているは、これらのサンプルで示すは、Azure の IoT デバイス SDK に含まれているアプリケーションです。

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

However there are companion functions to each of these APIs:

-   IoTHubClient\_LL\_CreateFromConnectionString

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_Destroy


These functions all include “LL” in the API name. Other than that, the parameters of each of these functions are identical to their non-LL counterparts. However, the behavior of these functions is different in one important way.

When you call **IoTHubClient\_CreateFromConnectionString**, the underlying libraries create a new thread that runs in the background. This thread sends events to, and receives messages from, IoT Hub. No such thread is created when working with the "LL" APIs. The creation of the background thread is a convenience to the developer. You don’t have to worry about explicitly sending events and receiving messages from IoT Hub -- it happens automatically in the background. In constrast, the "LL" APIs give you explicit control over communication with IoT Hub, if you need it.

To understand this better, let’s look at an example:

When you call **IoTHubClient\_SendEventAsync**, what you're actually doing is putting the event in a buffer. The background thread created when you call **IoTHubClient\_CreateFromConnectionString** continually monitors this buffer and sends any data that it contains to IoT Hub. This happens in the background at the same time that the main thread is performing other work.

Similarly, when you register a callback function for messages using **IoTHubClient\_SetMessageCallback**, you're instructing the SDK to have the background thread invoke the callback function when a message is received, independent of the main thread.

The "LL" APIs don’t create a background thread. Instead, a new API must be called to explicitly send and receive data from IoT Hub. This is demonstrated in the following example.

The **iothub\_client\_sample\_http** application that’s included in the SDK demonstrates the lower-level APIs. In that sample, we send events to IoT Hub with code such as the the following:
```
EVENT_INSTANCE メッセージです。
sprintf_s (msgText、sizeof(msgText)、"message _ %d_from_iothubclient_ll_over_http"i) です。
message.messageHandle IoTHubMessage_CreateFromByteArray = ((const unsigned char *)、msgText strlen(msgText)) です。

IoTHubClient_LL_SendEventAsync (iotHubClientHandle message.messageHandle SendConfirmationCallback、& メッセージ)
```

The first three lines create the message, and the last line sends the event. However, as mentioned previously, "sending" the event means that the data is simply placed in a buffer. Nothing is transmitted on the network when we call **IoTHubClient\_LL\_SendEventAsync**. In order to actually ingress the data to IoT Hub, you must call **IoTHubClient\_LL\_DoWork**, as in this example:
```
(1) の中に
{
    IoTHubClient_LL_DoWork(iotHubClientHandle) です。
    ThreadAPI_Sleep(1000) です。
{
```

This code (from the **iothub\_client\_sample\_http** application) repeatedly calls **IoTHubClient\_LL\_DoWork**. Each time **IoTHubClient\_LL\_DoWork** is called, it sends some events from the buffer to IoT Hub and it retrieves a queued message being sent to the device. The latter case means that if we registered a callback function for messages, then the callback is invoked (assuming any messages are queued up). We would have registered such a callback function with code such as the following:
```
IoTHubClient_LL_SetMessageCallback (iotHubClientHandle ReceiveMessageCallback、& receiveContext)
```

The reason that **IoTHubClient\_LL\_DoWork** is often called in a loop is that each time it’s called, it sends *some* buffered events to IoT Hub and retrieves *the next* message queued up for the device. Each call isn’t guaranteed to send all buffered events or to retrieve all queued messages. If you want to send all events in the buffer and then continue on with other processing you can replace this loop with code such as the following:
```
IOTHUB_CLIENT_STATUS 状態です。

中に (((iotHubClientHandle、およびステータス) IoTHubClient_LL_GetSendStatus IOTHUB_CLIENT_OK = =) (& a) (& a) (ステータス IOTHUB_CLIENT_SEND_STATUS_BUSY = =))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle) です。
    ThreadAPI_Sleep(1000) です。
{
```

This code calls **IoTHubClient\_LL\_DoWork** until all events in the buffer have been sent to IoT Hub. Note that this does not also imply that all queued messages have been received. Part of the reason for this is that checking for "all" messages isn’t as deterministic an action. What happens if you retrieve "all" of the messages, but then another one is sent to the device immediately after? A better way to deal with that is with a programmed timeout. For example, the message callback function could reset a timer every time it’s invoked. You can then write logic to continue processing if, for example, no messages have been received in the last *X* seconds.

When you’re finished ingressing events and receiving messages, be sure to call the corresponding function to clean up resources.
```
IoTHubClient_LL_Destroy(iotHubClientHandle) です。
```

Basically there’s only one set of APIs to send and receive data with a background thread and another set of APIs that does the same thing without the background thread. A lot of developers may prefer the non-LL APIs, but the lower level APIs are useful when the developer wants explicit control over network transmissions. For example, some devices collect data over time and only ingress events at specified intervals (for example, once an hour or once a day). The lower-level APIs give you the ability to explicitly control when you send and receive data from IoT Hub. Others will simply prefer the simplicity that the lower level APIs provide. Everything happens on the main thread rather than some work happening in the background.

Whichever model you choose, be sure to be consistent in which APIs you use. If you start by calling **IoTHubClient\_LL\_CreateFromConnectionString**, be sure you only use the corresponding lower level APIs for any follow-up work:

-   IoTHubClient\_LL\_SendEventAsync

-   IoTHubClient\_LL\_SetMessageCallback

-   IoTHubClient\_LL\_Destroy

-   IoTHubClient\_LL\_DoWork

The opposite is true as well. If you start with **IoTHubClient\_CreateFromConnectionString**, then use the non-LL APIs for any additional processing.

In the Azure IoT device SDK for C, see the **iothub\_client\_sample\_http** application for a complete example of the lower level APIs. The **iothub\_client\_sample\_amqp** application can be referenced for a full example of the non-LL APIs.

## Property handling

So far when we've described sending data, we've been referring to the body of the message. For example, consider this code:
```
EVENT_INSTANCE メッセージです。
sprintf_s (msgText、sizeof(msgText)、"Hello World") です。
message.messageHandle IoTHubMessage_CreateFromByteArray = ((const unsigned char *)、msgText strlen(msgText)) です。
IoTHubClient_LL_SendEventAsync (iotHubClientHandle message.messageHandle SendConfirmationCallback、& メッセージ)
```

This example sends a message to IoT Hub with the text "Hello World." However, IoT Hub also allows properties to be attached to each message. Properties are name/value pairs that can be attached to the message. For example, we can modify the previous code to attach a property to the message:
```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle) です。
sprintf_s (propText、sizeof(propText)、"%d"、i) です。
Map_AddOrUpdate (propMap"SequenceNumber"propText) です。
```

We start by calling **IoTHubMessage\_Properties** and passing it the handle of our message. What we get back is a **MAP\_HANDLE** reference that enables us to start adding properties. The latter is accomplished by calling **Map\_AddOrUpdate**, which takes a reference to a MAP\_HANDLE, the property name, and the property value. With this API we can add as many properties as we like.

When the event is read from **Event Hub**, the receiver can enumerate the properties and retrieve their corresponding values. For example, in .NET this would be accomplished by accessing the [Properties collection on the EventData object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

In the previous example, we’re attaching properties to an event that we send to IoT Hub. Properties can also be attached to messages received from IoT Hub. If we want to retrieve properties from a message, we can use code such as the following in our message callback function:
```
静的 IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
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

{
```

The call to **IoTHubMessage\_Properties** returns the **MAP\_HANDLE** reference. We then pass that reference to **Map\_GetInternals** to obtain a reference to an array of the name/value pairs (as well as a count of the properties). At that point it's a simple matter of enumerating the properties to get to the values we want.

You don't have to use properties in your application. However, if you need to set them on events or retrieve them from messages, the **IoTHubClient** library makes it easy.

## Message handling

As stated previously, when messages arrive from IoT Hub the **IoTHubClient** library responds by invoking a registered callback function. There is a return parameter of this function that deserves some additional explanation. Here’s an excerpt of the callback function in the **iothub\_client\_sample\_http** sample application:
```
静的 IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    IOTHUBMESSAGE_ACCEPTED; を返す
{
```

Note that the return type is **IOTHUBMESSAGE\_DISPOSITION\_RESULT** and in this particular case we return **IOTHUBMESSAGE\_ACCEPTED**. There are other values we can return from this function that change how the **IoTHubClient** library reacts to the message callback. Here are the options.

-   **IOTHUBMESSAGE\_ACCEPTED** – The message has been processed successfully. The **IoTHubClient** library will not invoke the callback function again with the same message.

-   **IOTHUBMESSAGE\_REJECTED** – The message was not processed and there is no desire to do so in the future. The **IoTHubClient** library should not invoke the callback function again with the same message.

-   **IOTHUBMESSAGE\_ABANDONED** – The message was not processed successfully, but the **IoTHubClient** library should invoke the callback function again with the same message.

For the first two return codes, the **IoTHubClient** library sends a message to IoT Hub indicating that the message should be deleted from the device queue and not delivered again. The net effect is the same (the message is deleted from the device queue), but whether the message was accepted or rejected is still recorded.  Recording this distinction is useful to senders of the message who can listen for feedback and find out if a device has accepted or rejected a particular message.

In the last case a message is also sent to IoT Hub, but it indicates that the message should be redelivered. Typically you’ll abandon a message if you encounter some error but want to try to process the message again. In contrast, rejecting a message is appropriate when you encounter an unrecoverable error (or if you simply decide you don’t want to process the message).

In any case, be aware of the different return codes so that you can elicit the behavior you want from the **IoTHubClient** library.

## Alternate device credentials

As explained previously, the first thing to do when working with the **IoTHubClient** library is to obtain a **IOTHUB\_CLIENT\_HANDLE** with a call such as the following:
```
IOTHUB_CLIENT_HANDLE iotHubClientHandle です。
iotHubClientHandle = IoTHubClient_CreateFromConnectionString (connectionString、AMQP_Protocol) です。
```

The arguments to **IoTHubClient\_CreateFromConnectionString** are the connection string of our device and a parameter that indicates the protocol we use to communicate with IoT Hub. The connection string has a format that appears as follows:
```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

There are four pieces of information in this string: IoT Hub name, IoT Hub suffix, device ID, and shared access key. You obtain the fully qualified domain name (FQDN) of an IoT hub when you create your IoT hub instance in the Azure portal — this gives you the IoT hub name (the first part of the FQDN) and the IoT hub suffix (the rest of the FQDN). You get the Device ID and the Shared Access Key when you register your device with IoT Hub (as described in the [previous article](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** gives you one way to initialize the library. If you prefer, you can create a new **IOTHUB\_CLIENT\_HANDLE** by using these individual parameters rather than the connection string. This is achieved with the following code:
```
IOTHUB_CLIENT_CONFIG iotHubClientConfig です。
iotHubClientConfig.iotHubName =""です。
iotHubClientConfig.deviceId =""です。
iotHubClientConfig.deviceKey =""です。
iotHubClientConfig.iotHubSuffix =""です。
iotHubClientConfig.protocol = HTTP_Protocol です。
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create (& iotHubClientConfig) です。
```

This accomplishes the same thing as **IoTHubClient\_CreateFromConnectionString**.

It may seem obvious that you would want to use **IoTHubClient\_CreateFromConnectionString** rather than this more verbose method of initialization. Keep in mind, however, that when you register a device in IoT Hub what you get is a device ID and device key (not a connection string). The **Device Manager** SDK tool introduced in the [previous article](iot-hub-device-sdk-c-intro.md) uses libraries in the **Azure IoT service SDK** to create the connection string from the device ID, device key, and IoT Hub host name. So calling **IoTHubClient\_LL\_Create** may be preferable because it saves you the step of generating a connection string. Use whichever method is convenient.

## Configuration options

So far everything described about the way the **IoTHubClient** library works reflects its default behavior. However, there are a few options that you can set to change how the library works. This is accomplished by leveraging the **IoTHubClient\_LL\_SetOption** API. Consider this example:
```
符号なし int タイムアウト = 30000 です。
IoTHubClient_LL_SetOption (iotHubClientHandle、「タイムアウト」、およびタイムアウト)
```

よく使用されるオプションがいくつかあります。

-   **SetBatching** (ブール値): **true** の場合、IoT Hub に送信されるデータはバッチで送信されます。 **false** の場合は、メッセージは個別に送信されます。 既定値は **false** です。

-   **Timeout** (符号なし整数): この値はミリ秒単位で表現されます。 HTTP 要求の送信や応答の受信にこの時間より長くかかる場合は、接続がタイムアウトします。

バッチ処理オプションは重要です。 既定では、ライブラリ ingresses イベント個別に (1 つのイベントに渡す **IoTHubClient\_LL\_SendEventAsync**)。 バッチ処理オプションが **true** の場合、ライブラリはバッファーから可能な限りの (IoT Hub が許容する最大メッセージ サイズまでの) イベントを収集します。 イベントのバッチは、(個々のイベントが JSON 配列にまとめられて) 単一の HTTP 呼び出しで IoT Hub に送信されます。 バッチ処理を有効にすると、通常はネットワーク ラウンドトリップが減少するため、パフォーマンスの大幅な向上につながります。 個別の各イベントの一連のヘッダーではなく、イベント バッチで一連の HTTP ヘッダーが送信されるため、帯域幅が大幅に削減されます。 それ以外の方法で実行する特別な理由がない限り、一般的にバッチ処理を有効にします。

## 次のステップ

この記事は、**C 用 Azure IoT デバイス SDK** にある **IoTHubClient** ライブラリの動作の詳細を説明しました。 この情報は、**IoTHubClient** ライブラリの機能の理解に役立ててください。  [次回の記事](iot-hub-device-sdk-c-serializer.md) 上のような詳細情報を提供、 **シリアライザー** ライブラリです。





