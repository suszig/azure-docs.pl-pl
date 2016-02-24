<properties
    pageTitle="C および Apache Storm での Event Hubs の使用 | Microsoft Azure"
    description="このチュートリアルでは、C でイベントを送信し、Apache Storm クラスターでそれを受信するための Azure Event Hubs の使用方法について説明します。"
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article" 
    ms.date="12/09/2015"
    ms.author="sethm"/>

# Event Hubs の使用

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## はじめに

Event Hubs は、拡張性の高いインジェスト システムで、1 秒あたり何百万ものイベントを取り込むことができます。そのためアプリケーションは、接続されているデバイスやアプリケーションによって生成された大量のデータを処理し、分析できます。 Event Hubs に収集されたデータは、任意のリアルタイム分析プロバイダーやストレージ クラスターを使用して転送と格納できます。

詳細については、[Event Hubs の概要] を参照してください。

このチュートリアルでは、C のコンソール アプリケーションを使用して、イベント ハブにメッセージをインジェストし、Apache Storm を使用して並列で取得する方法を学習します。

このチュートリアルを完了するには、以下が必要です。

+ C の開発環境。 このチュートリアルではとみなし、gcc スタックを [Azure Linux VM](../virtual-machines/virtual-machines-linux-tutorial.md) と Ubuntu 14.04 を使用します。 他の環境用の手順は、外部リンクで提供されます。

+ Java 開発環境を実行するように構成 [Maven](http://maven.apache.org/)します。 このチュートリアルではものと [Eclipse](https://www.eclipse.org/)します。

+ アクティブな Azure アカウント アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](https://azure.microsoft.com/pricing/free-trial/)をご覧ください。

## Event Hub を作成する

1. [Azure クラシック ポータル] に、ログオンし、クリックして **新規** 、画面の下部にあります。

2. をクリックして **App Services**, 、し **Service Bus**, 、し **Event Hub**, 、し **簡易作成**します。

    ![][1]

3. Event Hub の名前を入力を目的のリージョンを選択してクリックして **新しい Event Hub 作成**します。

    ![][2]

4. 作成した名前空間をクリックして (通常 ***イベント ハブ名*-ns**)。

    ![][3]

5. クリックして、 **Event Hubs** 、ページの上部にあるタブを作成した Event Hub をクリックします。

    ![][4]

6. をクリックして、 **構成** という名前のルールを追加、ページの上部にあるタブで、 **SendRule** と *送信* 権限でという別のルールを追加する **ReceiveRule** と *リッスン* 権限、およびクリック **保存**します。

    ![][5]

7. 同じページに対して生成されたキーを書き留めます **SendRule** と **ReceiveRule**します。

    ![6 c]

イベント ハブが作成され、イベントの送受信に必要な接続文字列が手元にあります。

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## アプリケーションの実行

これで、アプリケーションを実行する準備が整いました。

1.  実行、 **LogTopology** eclipse でクラスし、すべてのパーティションに対してレシーバーが起動するまで待機します。

2.  実行、 **送信者** プログラム、および受信側ウィンドウに表示されるイベントを確認します。

    ![][23]

> [AZURE.NOTE] このチュートリアルだけで、ローカル モードで開発目的で Storm を使用します。 参照してください、 [HDInsight Storm overview] と公式 [Apache Storm] Storm の開発とパターンの詳細についてはドキュメントです。

## 次のステップ

Event Hubs と Storm を統合するアプリケーションの開発には、次のリソースを使用できます。

- [Storm と HDInsight を使用したセンサー データを分析する][] は Hadoop クラスター内のセンサー データを取り込むには、Event Hubs、Storm、および HBase を使用して完全なシナリオ チュートリアルです。
- [SCP.NET と c# Storm と HDInsight をストリーミング データ処理アプリケーションの開発][] は c# を使用して Storm のパイプラインを作成する方法に関するチュートリアルです。

<!-- Images. -->
[1]: ./media/event-hubs-c-storm-getstarted/create-event-hub1.png
[2]: ./media/event-hubs-c-storm-getstarted/create-event-hub2.png
[3]: ./media/event-hubs-c-storm-getstarted/create-event-hub3.png
[4]: ./media/event-hubs-c-storm-getstarted/create-event-hub4.png
[5]: ./media/event-hubs-c-storm-getstarted/create-event-hub5.png
[6]: ./media/event-hubs-getstarted/create-event-hub6.png
[6c]: ./media/event-hubs-c-storm-getstarted/create-event-hub6c.png

[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md

[Apache Storm]: https://storm.incubator.apache.org
[HDInsight Storm overview]: ../hdinsight/hdinsight-storm-overview.md/
[Analyzing sensor data with Storm and HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Develop streaming data processing applications with SCP.NET and C# on Storm and HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
 
