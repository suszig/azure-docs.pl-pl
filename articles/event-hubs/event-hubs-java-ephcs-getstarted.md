<properties
    pageTitle="Java での Event Hubs の使用 | Microsoft Azure"
    description="このチュートリアルでは、Java でイベントを送信し、EventProcessorHost を使用して C# でイベントを受信するための Azure Event Hubs の使用方法について説明します。"
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/05/2015"
    ms.author="sethm"/>

# Event Hubs の使用

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## はじめに

Event Hubs は、拡張性の高いインジェスト システムで、1 秒あたり何百万ものイベントを取り込むことができます。そのためアプリケーションは、接続されているデバイスやアプリケーションによって生成された大量のデータを処理し、分析できます。 Event Hubs に収集されたデータは、任意のリアルタイム分析プロバイダーやストレージ クラスターを使用して転送と格納できます。

詳細については、次を参照してください。、 [Event Hubs の概要][]します。

このチュートリアルは、Java では、コンソール アプリケーションを使用して Event Hub にメッセージをインジェストし、c# を使用して並列で取得する方法を示しています。 [イベント プロセッサ ホスト][] ライブラリです。

このチュートリアルを完了するには、以下が必要です。

+ Java 開発環境。 このチュートリアルではものと [Eclipse](https://www.eclipse.org/)します。

+ Microsoft Visual Studio Express for Windows

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure の無料試用版サイト</a>をご覧ください。

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

6. をクリックして、 **構成** という名前のルールを追加、ページの上部にあるタブで、 **SendRule** と *送信* 権限でという別のルールを追加する **ReceiveRule** と *管理、送信、リッスン* 権限、およびクリック **保存**します。

    ![][5]

7. 同じページに対して生成されたキーを書き留めます **SendRule**します。

    ![][6b]

8. クリックして、 **ダッシュ ボード** 、ページの上部にあるタブをクリックして **接続情報**します。 2 つの接続文字列をメモします。

    ![][6]

イベント ハブが作成され、イベントの送受信に必要な接続文字列が手元にあります。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  実行、 **受信者** Visual Studio からプロジェクトし、すべてのパーティションに対してレシーバーが起動するまで待機します。

    ![][21]

2.  実行、 **送信者** プロジェクトで、キーを押して **Enter** イベントが受信側ウィンドウに表示し、コンソール ウィンドウにします。

    ![][22]

## 次のステップ

Event Hub を作成し、データを送受信する実用的なアプリケーションが構築できたので、次のシナリオに移動します。

- 完全な [Event Hub を使用するサンプル アプリケーション][]します。
-  [Event Hubs でイベント処理のスケール][] サンプルです。
- A [メッセージング ソリューションをキューに置かれた][] Service Bus キューを使用します。

詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。

<!-- Images. -->
[1]: ./media/event-hubs-java-ephcs-getstarted/create-event-hub1.png
[2]: ./media/event-hubs-java-ephcs-getstarted/create-event-hub2.png
[3]: ./media/event-hubs-java-ephcs-getstarted/create-event-hub3.png
[4]: ./media/event-hubs-java-ephcs-getstarted/create-event-hub4.png
[5]: ./media/event-hubs-java-ephcs-getstarted/create-event-hub5.png
[6]: ./media/event-hubs-java-ephcs-getstarted/create-event-hub6.png
[6b]: ./media/event-hubs-java-ephcs-getstarted/create-event-hub6b.png


[21]: ./media/event-hubs-java-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-java-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 

