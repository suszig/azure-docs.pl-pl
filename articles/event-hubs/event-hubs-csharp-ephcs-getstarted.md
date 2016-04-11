<properties
    pageTitle="C# での Event Hubs の使用 | Microsoft Azure"
    description="このチュートリアルでは、C# による Azure Event Hubs と EventProcessorHost の使用方法について説明します。"
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="11/05/2015"
    ms.author="sethm"/>

# Event Hubs の使用

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## はじめに

Event Hubs は、接続されているデバイスとアプリケーションからの大量のイベント データを処理するサービスです。 Event Hubs にデータを収集した後、ストレージ クラスターを使用してデータを格納したり、リアルタイムの分析プロバイダーを使用して転送できます。 この大規模なイベントの収集と処理の機能は、モノのインターネット (IoT) など最新アプリケーション アーキテクチャの重要なコンポーネントです。

このチュートリアルでは、Azure クラシック ポータルを使用してイベント ハブを作成する方法を説明します。 C# で記述されるコンソール アプリケーションを使用して Event Hub にメッセージを収集する方法として、c# を使用して並列で取得する方法も表示して [Event Processor Host] ライブラリです。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2013、Microsoft Visual Studio Express 2013 for Windows。

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target ="_blank") です。

## Event Hub を作成する

1. ログオン、 [Azure クラシック ポータル][], 、] をクリック **新規** 、画面の下部にあります。

2. をクリックして **App Services**, 、し **Service Bus**, 、し **Event Hub**, 、し **簡易作成**します。

    ![][1]

3. Event Hub の名前を入力を目的のリージョンを選択してクリックして **新しい Event Hub 作成**します。

    ![][2]

4. 作成した名前空間をクリックして (通常 ***イベント ハブ名*-ns**)。

    ![][3]

5. クリックして、 **Event Hubs** 、ページの上部にあるタブを作成した Event Hub をクリックします。

    ![][4]

6. をクリックして、 **構成** ] タブの上部にある、という名前のルールを追加 **SendRule** と *送信* 権限でという別のルールを追加する **ReceiveRule** と *管理、送信、リッスン* 権限、およびクリック **保存**します。

    ![][5]

7. クリックして、 **ダッシュ ボード** 、ページの上部にあるタブをクリックして **接続情報**します。 2 つの接続文字列をメモするか、このチュートリアルで後で使用する場所にコピーします。

    ![][6]

イベント ハブが作成され、イベントの送受信に必要な接続文字列が手元にあります。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio 内で実行、 **受信者** プロジェクトをし、すべてのパーティションに対してレシーバーが起動するまで待機します。

    ![][21]

2.  実行、 **送信者** プロジェクトで、キーを押して **Enter** イベントが受信側ウィンドウに表示し、コンソール ウィンドウにします。

    ![][22]

## 次のステップ

Event Hub を作成し、データを送受信する実用的なアプリケーションが構築できたので、次のシナリオに移動します。

- 完全な [Event Hub を使用するサンプル アプリケーション][]します。
-  [Event Hubs でイベント処理のスケール][] サンプルです。
- A [メッセージング ソリューションをキューに置かれた][] Service Bus キューを使用します。
- [Event Hubs の概要][]

<!-- Images. -->
[1]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub1.png
[2]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub2.png
[3]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub3.png
[4]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub4.png
[5]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub5.png
[6]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub6.png

[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 


