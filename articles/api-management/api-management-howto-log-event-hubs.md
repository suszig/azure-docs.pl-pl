<properties 
    pageTitle="Azure API Management で Azure Event Hubs にイベントを記録する方法" 
    description="Azure API Management で Azure Event Hubs にイベントを記録する方法について説明します。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/07/2015" 
    ms.author="sdanie"/>


# Azure API Management で Azure Event Hubs にイベントを記録する方法

Azure Event Hubs は、1 秒間に数百万件のイベントを取り込むことができる高度にスケーラブルなデータ受信サービスであり、接続されたデバイスとアプリケーションで生成される大量のデータを処理および分析できます。 Event Hubs はイベント パイプラインの「玄関」として機能し、Event Hubs に収集されたデータは、任意のリアルタイム分析プロバイダーまたはバッチ処理/ストレージ アダプターを使用して変換および格納できます。 Event Hubs はイベント ストリームの生成とイベントの使用を分離し、イベント コンシューマーが独自のスケジュールでイベントにアクセスできるようにします。

この記事では、対応する、 [Event Hubs の使用の Azure API Management の統合](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) ビデオ Azure Event Hubs を使用して API Management のイベント ログに記録する方法について説明します。

## Azure Event Hub を作成します

新しい Event Hub を作成するにサインインする、 [Azure クラシック ポータル](https://manage.windowsazure.com) ] をクリック **新規**]-> [**App Services**]-> [**Service Bus**]-> [**Event Hub**]-> [**簡易作成**します。 イベント ハブの名前とリージョンを入力し、サブスクリプションを選択して、名前空間を選択します。 名前空間の作成がまだ済んでいない場合は、**[名前空間]** ボックスに名前を入力して作成してください。 すべてのプロパティの構成が完了したら、**[新しいイベント ハブの作成]** をクリックしてイベント ハブを作成します。

![イベント ハブの作成][create-event-hub]

次に、新しいイベント ハブの **[構成]** タブに移動し、**共有アクセス ポリシー**を 2 つ作成します。 1 つは **"Sending"** という名前を付けて、**送信**アクセス許可を与えます。

![Sending policy][sending-policy]

もう 1 つには **"Receiving"** という名前を付けて**リッスン**アクセス許可を追加し、**[保存]** をクリックします。

![Receiving policy][receiving-policy]

アプリケーションは、この 2 つの共有アクセス ポリシーによって、イベント ハブにイベントを送信したり、イベント ハブからイベントを受信したりすることができます。 これらのポリシーの接続文字列にアクセスするには、イベント ハブの **[ダッシュボード]** タブに移動し、**[接続情報]** をクリックします。

![接続文字列][event-hub-dashboard]

**Sending** 接続文字列はイベントをログに記録するときに、**Receiving** 接続文字列はイベント ハブからイベントをダウンロードするときに使用されます。

![接続文字列][event-hub-connection-string]

## API Management ロガーの作成

Event Hub がある場合は、次の手順がのでを構成する、 [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx) API Management でサービスを Event Hub にイベントが記録できるようにします。

使用して API Management のロガーが構成されて、 [API Management REST API](http://aka.ms/smapi)します。 最初に、REST API を使用する前に確認、 [の前提条件](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) できることを確認および [REST API へのアクセスを有効になっている](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI)します。

ロガーを作成するには、次の URL テンプレートを使用して HTTP PUT 要求を送信します。

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   置換 `{サービル}` 、API Management サービス インスタンスの名前に置き換えます。
-   置換 `新しい logger {name}` 、新しい logger の必要な名前を入力します。 構成するときに、この名前を参照する、 [ログ-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) ポリシー

次のヘッダーを要求に追加します。

-   Content-Type : application/json
-   Authorization : SharedAccessSignature uid=...
    -   手順については、生成する方法、 `SharedAccessSignature` を参照してください [Azure API Management REST API 認証](https://msdn.microsoft.com/library/azure/dn798668.aspx)します。

次のテンプレートを使用して要求の本文を指定します。

    {
      "type" : "AzureEventHub",
      "description" : "Sample logger description",
      "credentials" : {
        "name" : "Name of the Event Hub from the Azure Classic Portal",
        "connectionString" : "Endpoint=Event Hub Sender connection string"
        }
    }

-   `型` に設定する必要があります `AzureEventHub`します。
-   `説明` 、logger の説明 (オプション) を提供し、必要な場合は、長さ 0 の文字列を指定できます。
-   `資格情報` を含む、 `名` と `connectionString` Azure Event Hub のです。

ロガーでのステータス コードを作成する場合、要求を作成したときに `201 Created` が返されます。
>[AZURE.NOTE] 可能なリターン コードとその理由は、次を参照してください。 [Logger の作成](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT)します。 表示するリスト、更新、および削除、参照などの他の操作を実行する方法、 [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx) エンティティ ドキュメントです。

## log-to-eventhub ポリシーの構成

API Management でロガーを構成したら、必要なイベントを記録するための構成を log-to-eventhub ポリシーに対して行います。 log-to-eventhub ポリシーは、inbound と outbound のどちらかのポリシー セクションで使用します。

ポリシーを構成するにサインインする、 [Azure クラシック ポータル](https://manage.windowsazure.com), いずれかをクリックし、API Management サービスを移動して **パブリッシャー ポータル** または **管理** 、パブリッシャー ポータルにアクセスします。

![パブリッシャー ポータル][publisher-portal]

左側にある [API Management] メニューの **[ポリシー]** をクリックし、目的の製品と API を選択して、**[ポリシーの追加]** をクリックします。 この例では、全製品 (**[無制限]**) の **[Echo API]** にポリシーを追加しています。

![ポリシーの追加][add-policy]

カーソルを置き、 `受信` ポリシー] セクションをクリック、 **EventHub へのログ** を挿入するポリシー、 `ログ-eventhub` ポリシー ステートメントのテンプレートです。

![ポリシー エディター][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

置換 `logger id` 前の手順で構成されている API Management のロガーの名前に置き換えます。

値として文字列を返す任意の式を使用する、 `ログ-eventhub` 要素。 この例では、日付と時刻、サービス名、要求 ID、要求の IP アドレス、操作の名前から成る文字列が記録されます。

**[保存]** をクリックして、更新済みのポリシーの構成を保存します。 保存後すぐポリシーが有効となり、指定したイベント ハブにイベントが記録されます。

## 次のステップ

-   Azure Event Hubs の詳細
    -   [Azure Event Hubs を使用します。](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [EventProcessorHost を持つメッセージを受信します。](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Event Hub プログラミング ガイド](../event-hubs/event-hubs-programming-guide.md)
-   API Management と Event Hubs の統合の詳細
    -   [Logger エンティティ参照](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [ログに eventhub ポリシーのリファレンス](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Azure API Management、Event Hubs と Runscope をご利用の Api を監視します。](api-management-log-to-eventhub-sample.md)

## ビデオ チュートリアルを視聴する

> [AZURE.VIDEO integrate-azure-api-management-with-event-hubs]



[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png 
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png 
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png 
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png 
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png 
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png 
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png 
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png 

