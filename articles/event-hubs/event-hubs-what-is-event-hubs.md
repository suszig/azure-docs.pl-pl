<properties
    pageTitle="Azure Event Hubs とは | Microsoft Azure"
    description="Azure Event Hubs の概要"
    services="event-hubs"
    documentationCenter=".net"
    authors="nberdy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/05/2015"
    ms.author="sethm"/>

# Azure Event Hubs とは

Azure Event Hubs は、1 秒間に数百万件のイベントを取り込むことができる高度にスケーラブルなデータ受信サービスであり、接続されたデバイスとアプリケーションで生成される大量のデータを処理および分析できます。 Event Hubs はイベント パイプラインの「玄関」として機能し、Event Hubs に収集されたデータは、任意のリアルタイム分析プロバイダーまたはバッチ処理/ストレージ アダプターを使用して変換および格納できます。 Event Hubs はイベント ストリームの生成とイベントの使用を分離し、イベント コンシューマーが独自のスケジュールでイベントにアクセスできるようにします。

## Event Hubs の機能

Event Hubs は、低遅延の動作と高い信頼性を確保しながら、クラウドに対する膨大なイベントとテレメトリ データの入口として利用できるイベント処理サービスです。 このサービスは次のような場合に特に便利です。

* アプリケーションのインストルメンテーション
* ユーザー エクスペリエンスやワークフロー処理
* モノのインターネット (IoT) のシナリオ

Event Hubs の他の重要な機能としては、モバイル アプリでのビヘイビアー追跡、Web ファームからのトラフィック情報、コンソール ゲームにおけるゲーム中のイベント キャプチャ、産業機器またはコネクテッド カーから収集されたテレメトリ データなどがあります。

異なり [Service Bus キューおよびトピック](../service-bus/service-bus-messaging-overview.md), 、Event Hubs は大規模にメッセージ ストリーム処理を提供することに重点を置きます。 Event Hubs の機能は、高スループットおよびイベント処理のシナリオに重きが置かれているという点で、トピックとは異なります。 その結果、Event Hubs 実装しないされる利用可能なメッセージング機能のいくつか [トピック](../service-bus/service-bus-fundamentals-hybrid-solutions.md#topics)します。 それらの機能が必要な場合、トピックが最適な選択肢となります。

## 次のステップ

Event Hubs の詳細については、次のトピックを参照してください。

- [Event Hubs の概要](event-hubs-overview.md)
- [Event Hubs のプログラミング ガイド](event-hubs-programming-guide.md)
- [Event Hubs の可用性とサポートに関する FAQ](event-hubs-availability-and-support-faq.md)
- 使ってみる、 [Event Hubs のチュートリアル][]
- 完全な [Event Hub を使用するサンプル アプリケーション][]

[Event Hubs tutorial]: event-hubs-csharp-ephcs-getstarted.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
