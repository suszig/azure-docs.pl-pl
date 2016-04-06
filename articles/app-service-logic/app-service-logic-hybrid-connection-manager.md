<properties 
    pageTitle="Hybrid Connection Manager の使用 | Microsoft Azure App Service" 
    description="Hybrid Connection Manager をインストールして構成し、Azure App Service 内のオンプレミス コネクターに接続する" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="dwrede" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="mandia"/>

# Hybrid Connection Manager を使用して Azure App Service 内のオンプレミス コネクターに接続する
オンプレミス システムを使用するため、Azure App Service は Hybrid Connection Manager を使用します。 一部のコネクタは、SQL Server、SAP、SharePoint などのオンプレミス システムに接続できます。 

Hybrid Connection Manager (HCM) は、ワンクリック インストーラーであり、ネットワーク内のファイアウオールの背後にある IIS サーバーにインストールされます。 HCM は、Azure Service Bus リレーを使用して、Azure 内にコネクタを持つオンプレミス システムを認証します。 

> [AZURE.NOTE] ハイブリッド接続マネージャーが、ファイアウォールの背後にある内部設置型リソースに接続している場合にのみ必要です。 オンプレミス システムに接続しない場合、Hybrid Connection Manager は必要ありません。

開始するには、以下が必要です。

- Azure Service Bus リレー名前空間の SAS 接続文字列。 参照してください [Service Bus 料金](http://azure.microsoft.com/pricing/details/service-bus/) を決定する層には、リレーが含まれています。
- オンプレミス システムのサインイン情報 (ユーザー名とパスワード)。 たとえば、オンプレミスの SQL Server に接続する場合は、SQL Server のログイン アカウントとパスワードが必要です。
- オンプレミス サーバー情報 (ポート番号とサーバー名) たとえば、オンプレミスの SQL Server に接続する場合は SQL Server の名前と TCP ポート番号が必要です。

## Service Bus 接続文字列を取得する

Azure ポータルで、Service Bus のルート SAS 接続文字列をコピーします。 この接続文字列は、Azure コネクタをオンプレミス システムに接続します。 

1.  [Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885), 、Service Bus 名前空間を選択してから選択 **接続情報**:

    ![][SB_ConnectInfo]

2. SAS 接続文字列をコピーします。

    ![][SB_SAS]

## Hybrid Connection Manager をインストールする

1.  [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040), 、作成したコネクタを選択します。 開くにを選択できます **参照**, [ **API Apps**, 、コネクタまたは API アプリを選択します。 
<br/><br/>
 **ハイブリッド接続**, 、セットアップは **不完全な**:
<br/>
![][2] 

2. 選択 **ハイブリッド接続**します。 前に入力した Service Bus 接続文字列が表示されます。
3. コピー、 **プライマリ構成文字列**:
<br/>
![][PrimaryConfigString]

4.  **内部設置型 Hybrid Connection Manager**, 、Hybrid Connection Manger をダウンロードするか、ポータルから直接インストールします。 
<br/><br/>
を、ポータルから直接インストールするには、内部設置型の IIS サーバー、ポータルを参照して選択を参照してください **をダウンロードして構成**します。
<br/><br/>
Hybrid Connection Manager をダウンロードする、内部設置型の IIS サーバーに移動しに移動、 **ClickOnce アプリケーション** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application)。 アプリケーションを実行できるように、インストールが自動的に開始されます。

5.  **リスナーのセットアップ** ウィンドウで、入力、 **プライマリ構成文字列** 以前 (ステップ 3) に貼り付け、選択 **インストール**します。

セットアップが完了すると、次が表示されます。
<br/>
![][3] 

ここでもう一度コネクタを参照すると、ハイブリッド接続ステータス **接続**します。 コネクタを閉じて、再度開く必要があります。 
<br/>
![][4] 

> [AZURE.NOTE] セカンダリ接続文字列に切り替えるには、ハイブリッド接続のセットアップを再実行し、入力、 **セカンダリ構成文字列**します。


## TCP ポートとセキュリティ

ハイブリッド接続を作成すると、Web サイトがローカルのオンプレミスの IIS サーバーに作成されます。 IIS サーバーは、DMZ 内に存在できます。 IIS サーバーの TCP ポート要件は次のとおりです。

TCP ポート | 理由
--- | ---
 | 着信 TCP 受信ポートは必要ありません。
9350 ～ 9354 | これらのポートはデータ転送に使用されます。 Service Bus Relay マネージャーはポート 9350 を調べて、TCP 接続を利用できるかどうかを決定します。 利用できる場合は、ポート 9352 も利用可能であるとみなされます。 データ トラフィックはポート 9352 を経由します。 <br/><br/>これらのポートへの発信接続を許可します。
5671 | ポート 9352 がデータ トラフィックに使用される場合、ポート 5671 は制御チャンネルとして使用されます。 <br/><br/>このポートへの発信接続を許可します。 
80、443 | ポート 9352 と 5671 を使用しない場合、 *、* ポート 80 と 443 がデータ転送と制御チャネルに使用される代替のポートです。<br/><br/>これらのポートへの発信接続を許可します。
オンプレミス システムのポート | オンプレミス システムでは、システムによって使用されているポートが開きます。 たとえば、SQL Server では、通常はポート 1433 を使用します。 この TCP ポートを開きます。

[Service Bus を使用したファイアウォールの背後でのホスティング](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## トラブルシューティング

![][HCMFlow]

### オンプレミスでのトラブルシューティング

1. IIS サーバーで、IIS Web ロールがインストールされ、すべての IIS サービスが開始されていることを確認します。
2. IIS サーバーで、Hybrid Connection Manager がインストールされ、実行されていることを確認します。
 - IIS マネージャー (inetmgr) で、 ***MicrosoftAzureBizTalkHybridListener*** web サイトが一覧表示されが実行されています。 
 - この web サイトを使用して、 ***HybridListenerAppPool*** として実行される、 *NetworkService* ローカル組み込みユーザー アカウントです。 この AppPool も開始されている必要があります。
3. IIS サーバーで、コネクタがインストールされ、実行されていることを確認します。 
 - App Service コネクタ用の Web サイトが作成されます。 たとえば、SQL コネクタを作成する場合は、 ***MicrosoftSqlConnector_nnn*** web サイトです。 IIS マネージャー (inetmgr) で、この web サイトが表示され、開始されていることを確認します。 
 - この web サイトは、名前、独自の IIS アプリケーション プールを使用して ***、HybridAppPoolnnn***します。 この AppPool として実行され、 *NetworkService* ローカル組み込みユーザー アカウントです。 この Web サイトと AppPool の両方が開始されている必要があります。 
 - ローカル コネクタを参照します。 たとえば、コネクタ web サイトがポート 6569 を使用する場合は、http://localhost:6569 を参照します。 既定のドキュメントが構成されていないため、`HTTP Error 403.14 - Forbidden error` が予期されます。
4. ファイアウォールで、このトピックに示されている TCP ポートが開いていることを確認します。
5. 送信元または送信先のシステムを調べます。
 - 一部のオンプレミス システムでは、追加の依存関係ファイルが必要です。 たとえば、オンプレミスの SAP に接続する場合は、追加の SAP ファイルをいくつか IIS サーバーにインストールする必要があります。
 - ログイン アカウントを使用してシステムへの接続を確認します。 たとえば、システムによって使用される TCP ポート (SQL Server 用のポート 1433 など) が開いている必要があります。 Azure ポータルで入力したログイン アカウントによってシステムにアクセスできる必要があります。
6. IIS サーバーのイベント ログで、エラーがないことを確認します。 
7. クリーンアップを行い、Hybrid Connection Manager を再インストールします。 
 - IIS で、コネクタ Web サイトとそのアプリケーション プールを手動で削除します。 
 - Hybrid Connection Manager を再実行し、正しい入力していることを確認 **プライマリ構成文字列** 、コネクタ用です。



### Azure クラシック ポータルで

1. Service Bus 名前空間が確認できる、 **Active** 状態です。
2. コネクタを作成するときに、Service Bus の SAS 接続文字列を入力します。 ACS 接続文字列を入力しないでください。


## FAQ

**質問**: 2 つのハイブリッド接続マネージャーがあります。 違いは何ですか。<br/>
**応答**: がある、 [ハイブリッド接続](../integration-hybrid-connection-overview.md) Web Apps (以前の web サイト) と Mobile Apps (以前のモバイル サービス) によって、主に内部設置型への接続に使用されるテクノロジです。 この Hybrid Connections Manager には独自 [セットアップ](../integration-hybrid-connection-create-manage.md) し (バック グラウンドで) Azure BizTalk サービスを使用します。 それは、TCP プロトコルと HTTP プロトコルのみをサポートします。

Azure App Service コネクタにも、Hybrid Connection Manager があります。  この Hybrid Connection Manager は *いない* (バック グラウンドで)、Azure BizTalk サービスとサポートをよりも、TCP および HTTP プロトコルを使用します。 参照してください、 [コネクタと API Apps の一覧と](app-service-logic-connectors-list.md)です。

どちらも、Azure Service Bus を使用してオンプレミス システムに接続します。

**質問**: カスタム API App を作成したときに使えます App Service Hybrid Connection Manager 内部設置型への接続にでしょうか。 <br/>
**応答**: 従来の意味ではできません。 組み込みコネクタを使用し、App Service Hybrid Connection Manager を構成することで、オンプレミス システムに接続できます。 その後、カスタム API App の中で、Logic App を利用してこのコネクタを使用します。 現時点では、独自のハイブリッド API App (SQL コネクタやファイル コネクタなど) は開発することも作成することもできません。

使用することができます、カスタム API は、TCP または HTTP ポートを使用している場合 [ハイブリッド接続](../integration-hybrid-connection-overview.md) とその Hybrid Connection Manager です。 このシナリオでは、Azure BizTalk Service が使用されます。 [Web アプリから内部設置型 SQL Server に接続](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) 役立つ場合があります。  


## 詳細

[Logic Apps を監視する](app-service-logic-monitor-your-logic-apps.md)<br/>
[サービス バスの料金](http://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 


