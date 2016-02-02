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

このチュートリアルでは、Azure クラシック ポータルを使用してイベント ハブを作成する方法を説明します。 C# [イベント プロセッサ ホスト] ライブラリを使用して並列で取得する方法と、c# で記述されるコンソール アプリケーションを使用して Event Hub にメッセージを収集する方法についても説明します。

このチュートリアルを完了するには、以下が必要になります。

+ Microsoft Visual Studio 2013、Microsoft Visual Studio Express 2013 for Windows。

+ アクティブな Azure アカウント <br/>アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02 & returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target ="_blank") です。

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

6. 上部の **[構成]** タブをクリックし、**SendRule** という名前のルールに *Send* 権限を追加します。もう 1 つの R**eceiveRule** というルールには *Manage、Send、Listen* 権限を追加し、**[保存]** をクリックします。

    ![][5]

7. ページ上部の **[ダッシュボード]** タブをクリックし、**[接続情報]** をクリックします。 2 つの接続文字列をメモするか、このチュートリアルで後で使用する場所にコピーします。

    ![][6]

イベント ハブが作成され、イベントの送受信に必要な接続文字列が手元にあります。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  Visual Studio 内で **Receiver** プロジェクトを実行し、すべてのパーティションに対してレシーバーが起動するまで待機します。

    ![][21]

2.  **Sender** プロジェクトを実行し、コンソール ウィンドウで **Enter** キーを押します。すると、レシーバーのウィンドウにイベントが表示されます。

    ![][22]

## 次のステップ

Event Hub を作成し、データを送受信する実用的なアプリケーションが構築できたので、次のシナリオに移動します。

- 完全な [Event Hubs を使用するサンプル アプリケーション][]します。
- [Event Hubs でイベント処理のスケール][] サンプルです。
- A [メッセージング ソリューションのキューに置かれた][] Service Bus キューを使用します。
- [イベント ハブの概要][]




[1]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub1.png 
[2]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub2.png 
[3]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub3.png 
[4]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub4.png 
[5]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub5.png 
[6]: ./media/event-hubs-csharp-ephcs-getstarted/create-event-hub6.png 
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png 
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png 
[azure classic portal]: https://manage.windowsazure.com/ 
[event processor host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost 
[event hubs overview]: event-hubs-overview.md 
[sample application that uses event hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097 
[scale out event processing with event hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3 
[queued messaging solution]: ../service-bus-dotnet-multi-tier-app-using-service-bus-queues.md 

