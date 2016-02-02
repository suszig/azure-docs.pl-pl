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

詳細については、次を参照してください [Event Hubs の概要][]します。

このチュートリアルでは、C では、コンソール アプリケーションを使用して Event Hub にメッセージをインジェストし、c# を使用して並列で取得する方法を学習します [イベント プロセッサ ホスト][] ライブラリです。

このチュートリアルを完了するには、以下が必要です。

+ C の開発環境。 このチュートリアルではとみなし、gcc スタックを [Azure Linux VM](../virtual-machines/virtual-machines-linux-tutorial.md) と Ubuntu 14.04 を使用します。 他の環境用の手順は、外部リンクで提供されます。

+ Microsoft Visual Studio Express 2013 for Windows

+ アクティブな Azure アカウント <br/>アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure 無料試用版</a>します。

## Event Hub を作成する

1. ログオン、 [Azure クラシック ポータルの [][], 、] をクリック **新規** 、画面の下部にあります。

2. **[アプリケーション サービス]**、**[Service Bus]**、**[Event Hub]**、**[簡易作成]** の順にクリックします。

    ![][1]

3. Event Hub の名前を入力して、目的のリージョンを選択し、**[Create a new Event Hub (新しい Event Hub を作成する)]** をクリックします。

    ![][2]

4. 作成した名前空間をクリックして (通常 *** イベント ハブ名 *-ns**)。

    ![][3]

5. ページ上部にある、**[Event Hubs]]** タブをクリックし、先ほど作成したイベント ハブをクリックします。

    ![][4]

6. ページの上部にある **[構成]** タブをクリックし、*Send* 権限を持つ **SendRule** という名前のルールを追加し、*Manage、Send、Listen* 権限を持つ **ReceiveRule** という別のルールを追加して、**[保存]** をクリックします。

    ![][5]

7. 同じページで、**SendRule** に対して生成されたキーをメモしておきます。

    ![][6b]

8. ページ上部の **[ダッシュボード]** タブをクリックし、**[接続情報]** をクリックします。 2 つの接続文字列をメモします。

    ![][6]

イベント ハブが作成され、イベントの送受信に必要な接続文字列が手元にあります。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio 内で **Receiver** プロジェクトを実行し、すべてのパーティションに対してレシーバーが起動するまで待機します。

    ![][21]

2.  **Sender** プログラムを実行し、受信側ウィンドウに表示されるイベントを確認します。

    ![][24]

## 次のステップ

Event Hub を作成し、データを送受信する実用的なアプリケーションが構築できたので、次のシナリオに移動します。

- 完全な [Event Hubs を使用するサンプル アプリケーション][]します。
- [Event Hubs でイベント処理のスケール][] サンプルです。
- A [メッセージング ソリューションのキューに置かれた][] Service Bus キューを使用します。
- [イベント ハブの概要][]




[1]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub1.png 
[2]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub2.png 
[3]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub3.png 
[4]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub4.png 
[5]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub5.png 
[6]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub6.png 
[6b]: ./media/event-hubs-c-ephcs-getstarted/create-event-hub6b.png 
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png 
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png 
[azure classic portal]: https://manage.windowsazure.com/ 
[event processor host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost 
[event hubs overview]: event-hubs-overview.md 
[sample application that uses event hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097 
[scale out event processing with event hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3 
[queued messaging solution]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md 

