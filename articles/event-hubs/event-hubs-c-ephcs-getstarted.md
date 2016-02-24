<properties
    pageTitle="C および C# でのイベント ハブの使用 | Microsoft Azure"
    description="このチュートリアルでは、C でイベントを送信し、EventProcessorHost を使用して C# でそのイベントを受信するための Azure Event Hubs の使用方法について説明します。"
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="12/09/2015"
    ms.author="sethm"/>

# Event Hubs の使用

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## はじめに

Event Hubs は、拡張性の高いインジェスト システムで、1 秒あたり何百万ものイベントを取り込むことができます。そのためアプリケーションは、接続されているデバイスやアプリケーションによって生成された大量のデータを処理し、分析できます。 Event Hubs に収集されたデータは、任意のリアルタイム分析プロバイダーやストレージ クラスターを使用して転送と格納できます。

詳細については、[イベント ハブの概要に関するページを参照してください。

このチュートリアルでは、C では、コンソール アプリケーションを使用して Event Hub にメッセージをインジェストし、c# の [イベント プロセッサ ホスト] ライブラリを使用して並列で取得する方法を学習します。

このチュートリアルを完了するには、以下が必要です。

+ C の開発環境。 このチュートリアルではとみなし、gcc スタックを [Azure Linux VM](../virtual-machines/virtual-machines-linux-tutorial.md) と Ubuntu 14.04 を使用します。 他の環境用の手順は、外部リンクで提供されます。

+ Microsoft Visual Studio Express 2013 for Windows

+ アクティブな Azure アカウント <br/>アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「 <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 無料評価版</a>.

## Event Hub を作成する

1. [Azure クラシック ポータル] にログオンし、クリックして **新規** 、画面の下部にあります。

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

    ![6b]

8. クリックして、 **ダッシュ ボード** 、ページの上部にあるタブをクリックして **接続情報**します。 2 つの接続文字列をメモします。

    ![][6]

イベント ハブが作成され、イベントの送受信に必要な接続文字列が手元にあります。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  実行、 **受信者** 、Visual Studio 内からプロジェクトし、すべてのパーティションに対してレシーバーが起動するまで待機します。

    ![][21]

2.  実行、 **送信者** プログラム、および受信側ウィンドウに表示されるイベントを確認します。

    ![][24]

## 次のステップ

Event Hub を作成し、データを送受信する実用的なアプリケーションが構築できたので、次のシナリオに移動します。

- A 完了 [Event Hub を使用するサンプル アプリケーション]。
- [Event Hubs でイベント処理のスケール] サンプルです。
- [キューに置かれたメッセージング ソリューション] のサービス バス キューを使用します。
- [Event Hubs の概要][]

<!-- Images. -->
[1]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub1.png
[2]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub2.png
[3]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub3.png
[4]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub4.png
[5]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub5.png
[6]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub6.png
[6b]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub6b.png


[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

