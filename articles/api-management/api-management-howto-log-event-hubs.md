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

新しい Event Hub を作成するにサインインする、 [Azure クラシック ポータル](https://manage.windowsazure.com) ] をクリック **新規**]-> [**App Services**]-> [**Service Bus**]-> [**Event Hub**]-> [**簡易作成**します。 イベント ハブの名前とリージョンを入力し、サブスクリプションを選択して、名前空間を選択します。 以前、名前空間を作成していない場合は、1 つの名前を入力して、作成、 **名前空間** ] ボックスに貼り付けます。 すべてのプロパティを構成すると、クリックして **新しい Event Hub 作成** イベント ハブを作成します。

![イベント ハブの作成][create-event-hub]

次に移動し、 **構成** 新しいイベント ハブのタブし、2 つ作成 **共有アクセス ポリシー**します。 1 つ目の名前 **送信** し付けます **送信** アクセス許可。

![Sending policy][sending-policy]

名前、もう 1 つ **受信**, を付けます **リッスン** アクセス許可、およびクリック **保存**します。

![Receiving policy][receiving-policy]

アプリケーションは、この 2 つの共有アクセス ポリシーによって、イベント ハブにイベントを送信したり、イベント ハブからイベントを受信したりすることができます。 これらのポリシーの接続文字列にアクセスするに移動、 **ダッシュ ボード** ] タブをクリックして、Event Hub の **接続情報**します。

![接続文字列][event-hub-dashboard]

 **送信** イベント、ログの記録時に、接続文字列が使用されて、 **受信** Event Hub からイベントをダウンロードするときに、接続文字列を使用します。

![接続文字列][event-hub-connection-string]

## API Management ロガーの作成

Event Hub がある場合は、次の手順がのでを構成する、 [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx) API Management でサービスを Event Hub にイベントが記録できるようにします。

使用して API Management のロガーが構成されて、 [API Management REST API](http://aka.ms/smapi)します。 最初に、REST API を使用する前に確認、 [の前提条件](https://msdn.microsoft.com/library/azure/dn776326.aspx#Prerequisites) できることを確認および [REST API へのアクセスを有効になっている](https://msdn.microsoft.com/library/azure/dn776326.aspx#EnableRESTAPI)します。

ロガーを作成するには、次の URL テンプレートを使用して HTTP PUT 要求を送信します。

    https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview

-   `{your service}` は、実際の API Management サービス インスタンスの名前です。
-   `{new logger name}` は、新しいロガーに付ける名前です。 構成するときに、この名前を参照する、 [ログ-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) ポリシー

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

-   `type` は `AzureEventHub` に設定する必要があります。
-   `description` は、ロガーの説明です (省略可能)。必要に応じて、長さゼロの文字列にしてください。
-   `credentials` には、Azure Event Hubs の `name` と `connectionString` が含まれます。

要求を実行したとき、ロガーが作成されると、状態コード `201 Created` が返されます。 

>[AZURE.NOTE] 可能なリターン コードとその理由は、次を参照してください。 [Logger の作成](https://msdn.microsoft.com/library/azure/mt592020.aspx#PUT)します。 表示するリスト、更新、および削除、参照などの他の操作を実行する方法、 [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx) エンティティ ドキュメントです。

## log-to-eventhub ポリシーの構成

API Management でロガーを構成したら、必要なイベントを記録するための構成を log-to-eventhub ポリシーに対して行います。 log-to-eventhub ポリシーは、inbound と outbound のどちらかのポリシー セクションで使用します。

ポリシーを構成するにサインインする、 [Azure クラシック ポータル](https://manage.windowsazure.com), いずれかをクリックし、API Management サービスを移動して **パブリッシャー ポータル** または **管理** 、パブリッシャー ポータルにアクセスします。

![パブリッシャー ポータル][publisher-portal]

クリックして **ポリシー** 左側の [API Management] メニューで、目的の成果物と API を選択し、クリックして **ポリシーを追加する**です。 この例では、ポリシーを追加する、 **Echo API** で、 **無制限** 製品です。

![ポリシーの追加][add-policy]

カーソルを置き、 `inbound` ポリシー] セクションをクリック、 **EventHub へのログ** を挿入するポリシー、 `log-to-eventhub` ポリシー ステートメントのテンプレートです。

![ポリシー エディター][event-hub-policy]

    <log-to-eventhub logger-id ='logger-id'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
    </log-to-eventhub>

`logger-id` の部分は、先行する手順で構成した API Management ロガーの名前に置き換えてください。

`log-to-eventhub` 要素の値は、文字列を返す式であれば何でもかまいません。 この例では、日付と時刻、サービス名、要求 ID、要求の IP アドレス、操作の名前から成る文字列が記録されます。

クリックして **保存** 更新されたポリシー構成を保存します。 保存後すぐポリシーが有効となり、指定したイベント ハブにイベントが記録されます。

## 次のステップ

-   Azure Event Hubs の詳細
    -   [Azure Event Hubs の使用](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [EventProcessorHost を使用したメッセージの受信](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Event Hubs のプログラミング ガイド](../event-hubs/event-hubs-programming-guide.md)
-   API Management と Event Hubs の統合の詳細
    -   [ロガーのエンティティ リファレンス](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [log-to-eventhub ポリシー リファレンス](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    -   [Azure API Management、Event Hubs、Runscope を使用した API の監視](api-management-log-to-eventhub-sample.md)    

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







