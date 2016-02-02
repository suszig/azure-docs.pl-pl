<properties 
    pageTitle="Service Bus AMQP の概要 | Microsoft Azure" 
    description="Azure での Advanced Message Queuing Protocol (AMQP) 1.0 の使用について説明します。" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor="mattshel"/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/05/2015" 
    ms.author="sethm"/>




# Service Bus での AMQP 1.0 サポート

Azure Service Bus クラウド サービスと内部設置型の両方 [Service Bus for Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) 、Advanced Message Queueing Protocol (AMQP) 1.0 をサポートします。 AMQP を使用すると、オープンな標準プロトコルを使用したクロス プラットフォームのハイブリッド アプリケーションをビルドできます。 異なる言語とフレームワークを使用して作成され、異なるオペレーティング システムで実行可能であるコンポーネントを使用して、アプリケーションを構築できます。 これらのコンポーネントはすべて Service Bus に接続でき、構造化されたビジネス メッセージを効率よく完全な忠実度でシームレスに交換できます。

## 概要: AMQP 1.0 の紹介とその重要な理由

これまで、メッセージ指向のミドルウェア製品では、クライアント アプリケーションとブローカーの間の通信に独自プロトコルが使用されてきました。 つまり、特定のベンダーのメッセージング ブローカーを選択すると、そのベンダーのライブラリを使用してクライアント アプリケーションをそのブローカーに接続する必要があります。 この結果、アプリケーションを別の製品に移植するには、接続するすべてのアプリケーションをコード変更する必要があるため、そのベンダーへの依存の度合いが高くなります。

さらに、異なるベンダーのメッセージング ブローカーを接続するのは面倒であり、 通常は、システム間でメッセージを移動し、独自のメッセージ形式間で変換を行うためにアプリケーション レベルのブリッジが必要になります。 これは頻繁に見られる要件です。たとえば、以前の異種システムに新しい統合インターフェイスを提供した場合や、合併後に IT システムを統合した場合などです。

ソフトウェア業界は動きの速いビジネスです。新しいプログラミング言語やアプリケーション フレームワークがときには困惑するほどのペースで導入されます。 同様に、IT システムの要件は時間の経過と共に進化しているため、開発者は最新のプラットフォームの機能を活用しようとします。 また、選択したメッセージング ベンダーがプラットフォームをサポートしていないこともあります。 独自のメッセージング プロトコルのため、他のベンダーが新しいプラットフォームのライブラリを提供することはできません。 このため、メッセージング製品を継続して使用できるようにするには、ゲートウェイまたはブリッジの構築などの方法を使用する必要があります。

AMQP (Advanced Message Queuing Protocol) 1.0 はこのような問題に促されて開発されました。 ほとんどの金融サービス企業と同じようにメッセージ指向ミドルウェアのヘビー ユーザーである JP Morgan Chase で生み出されました。 目的はシンプルです。つまり、さまざまな言語、フレームワーク、オペレーティング システムを使用して構築されたコンポーネント (すべて広範なサプライヤーの最良のコンポーネントを使用) を使用してメッセージ ベースのアプリケーションを作成できるようにする、オープン標準メッセージング プロトコルを作ることです。

## AMQP 1.0 の技術的な特徴

AMQP 1.0 は、堅牢なクロス プラットフォーム メッセージング アプリケーションを作成するために使用できる、効率的で信頼性の高い回線レベルのメッセージング プロトコルです。 このプロトコルには、安全で信頼性の高い効率的なメッセージ転送を二者間で行うメカニズムを定義するという、シンプルな目的があります。 メッセージ自体は、異種混在の送信者と受信者が構造化されたビジネス メッセージを完全な忠実度で交換でき、移植できるデータ表現を使用してエンコード化されます。 最も重要な特徴の概要を次に示します。

*    **Efficient**: AMQP 1.0 is a connection-oriented protocol that uses a binary encoding for the protocol instructions and the business messages transferred over it. It incorporates sophisticated flow-control schemes to maximize the utilization of the network and the connected components. That said, the protocol was designed to strike a balance between efficiency, flexibility and interoperability.

*    **Reliable**: The AMQP 1.0 protocol allows messages to be exchanged with a range of reliability guarantees, from fire-and-forget to reliable, exactly-once acknowledged delivery.

*    **Flexible**: AMQP 1.0 is a flexible protocol that can be used to support different topologies. The same protocol can be used for client-to-client, client-to-broker, and broker-to-broker communications.

*    **Broker-model independent**: The AMQP 1.0 specification does not make any requirements on the messaging model used by a broker. This means that it's possible to easily add AMQP 1.0 support to existing messaging brokers.


## AMQP 1.0 は Standard (大文字の S で始まる場合)

AMQP 1.0 は国際標準であり、ISO および IEC によって、ISO/IEC 19464:2014 として承認されています。

AMQP 1.0 は、2008 年以降、20 社を超える企業 (テクノロジ サプライヤーとエンド ユーザー企業の両方) で構成される中核的なグループにより開発されています。 その間、ユーザー企業は実際のビジネス要件を提供し、テクノロジ ベンダーはそれらの要件に合わせてプロトコルを進化させてきました。 プロセス全般にわたり、ベンダーはワークショップに参加し、実装間の相互運用性を検証するために協力してきました。

2011 年 10 月、開発作業は構造化情報標準促進協会 (OASIS) 内の専門委員会に移され、2012 年 10 月に OASIS AMQP 1.0 Standard がリリースされました。 この標準の開発中、次の企業が専門委員会に参加しました。

*    **Technology vendors**: Axway Software, Huawei Technologies, IIT Software, INETCO Systems, Kaazing, Microsoft, Mitre Corporation, Primeton Technologies, Progress Software, Red Hat, SITA, Software AG, Solace Systems, VMware, WSO2, Zenika.

*    **User firms**: Bank of America, Credit Suisse, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.


オープン標準の利点としては、次のような点がよく挙げられます。

*    Less chance of vendor lock-in

*    Interoperability

*    Broad availability of libraries and tooling

*    Protection against obsolescence

*    Availability of knowledgeable staff

*    Lower and manageable risk


## AMQP 1.0 と Service Bus

Azure Service Bus で AMQP 1.0 がサポートされるため、仲介型メッセージング機能 (Service Bus キューとトピック発行/サブスクライブ) をさまざまなプラットフォームから効率的なバイナリ プロトコルを使って利用できるようになります。 さらに、さまざまな言語、フレームワーク、およびオペレーティング システムを使って作成されたコンポーネントで構成されたアプリケーションを作成できます。

次の図は、標準の Java Message Service (JMS) API を使用して記述された Linux で実行される Java クライアントと、Windows で実行される .NET クライアントが、AMQP 1.0 を使用して Service Bus 経由でメッセージを交換する、サンプルのデプロイメントを示しています。

![][0]

**図 1: Service Bus と AMQP 1.0 を使用したクロスプラットフォーム メッセージングを示すサンプルのデプロイメント シナリオ**

現時点では、次のクライアント ライブラリが Service Bus で機能することがわかっています。

| 言語| ライブラリ|
|----------|-------------------------------------------------------------------------------|
| Java| Apache Qpid Java Message Service (JMS) クライアント<br/>IIT Software SwiftMQ Java クライアント|
| C| Apache Qpid Proton-C|
| PHP| Apache Qpid Proton-PHP|
| Python| Apache Qpid Proton-Python|
| C#| AMQP .Net Lite|

**図 2: AMQP 1.0 クライアント ライブラリの表**

## 概要

*    AMQP 1.0 is an open, reliable messaging protocol that you can use to build cross-platform, hybrid applications. AMQP 1.0 is an OASIS standard.

*    AMQP 1.0 support is now available in Azure Service Bus as well as Service Bus for Windows Server (Service Bus 1.1). Pricing is the same as for the existing protocols.


## 次のステップ

さらに詳しい情報については、 次のリンク先を参照してください。

- [AMQP で .NET からサービス バスを使用する]
- [AMQP で Java からサービス バスを使用する]
- [AMQP を使用した Python から Service Bus の使用]
- [AMQP で PHP からサービス バスを使用する]
- [Azure の Linux VM に Apache Qpid PROTON-C をインストールする]
- [Service Bus for Windows Server での AMQP]


[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png 
[using service bus from .net with amqp]: service-bus-amqp-dotnet.md 
[using service bus from java with amqp]: service-bus-amqp-java.md 
[using service bus from python with amqp]: service-bus-amqp-python.md 
[using service bus from php with amqp]: service-bus-amqp-php.md 
[installing apache qpid proton-c on an azure linux vm]: service-bus-amqp-apache.md 
[amqp in service bus for windows server]: https://msdn.microsoft.com/library/dn574799.aspx 

