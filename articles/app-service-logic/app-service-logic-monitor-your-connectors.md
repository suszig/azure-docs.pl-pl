<properties 
    pageTitle="Azure App Service でコネクタと API Apps の管理と監視を行う | Microsoft Azure" 
    description="Azure App Service のコネクタと API Apps のパフォーマンスを表示する: マイクロサービス アーキテクチャ" 
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
    ms.date="12/07/2015" 
    ms.author="mandia"/>

# 組み込み API Apps とコネクタの管理と監視を実行する

組み込み API アプリを作成しました。 次は何でしょうか。 

Azure では、すべての API アプリは、Azure でホストされる独立した Web サイトです。 その結果、行われた要求の数と、コネクタで使用されたデータの量を簡単に確認できます。 さらに、API アプリのバックアップ、アラートの作成、Tinfoil Security の有効化、およびユーザーとロールの追加を実行できます。

このトピックでは、API アプリを管理するためのさまざまなオプションについて、その一部を説明します。 

これらの組み込み機能を表示するには、[API アプリを開き、 [Azure ポータル](http://go.microsoft.com/fwlink/p/?LinkID=525040)します。 API アプリがスタート画面に表示されている場合は、それを選択するとプロパティが表示されます。 選択することも **参照**, を選択 **API Apps**, 、API アプリを選択します。

![][browse]

## 入力済みのプロパティを表示する

API アプリを開くと、使用できるさまざまな機能とタスクが表示されます。

![][settings]

そのための方法は次のとおりです。 

- **設定** 、サブスクリプションの詳細を含む、API アプリに関する特定の情報が表示され、API アプリにアクセスできるユーザーを表示します。 特にスケール機能を使用して、API アプリのインスタンス数を増減させることもできます。
- 使用して、 **開始** と **停止** API アプリを制御するボタンです。
- API アプリで使用される基になるファイルには、製品の更新が行われた、クリックして **更新** を最新のバージョンを取得します。 たとえば、ある修正プログラムやセキュリティ更新プログラムが Microsoft からリリースされると場合をクリックすると **更新** をこの修正プログラムを含めるように API アプリが自動的に更新します。 
- 選択 **プランの変更** アップグレードまたはダウン グレードの API アプリのデータの使用状況を基にします。 この機能を使用して、データの使用状況を表示することもできます。
- SQL コネクタなどのテーブルを持つコネクタを作成する場合は、接続するテーブルの名前を入力できます (省略可能)。 クリックすると、テーブルに基づくスキーマが自動的に作成され、使用可能な **スキーマのダウンロード**します。 このダウンロードしたスキーマを使用して、変換またはマップを作成できます。 

## 入力済みのコネクタまたは API 構成の値を変更する

組み込みコネクタを構成または作成した後で、入力済みの値を変更できます。 たとえば、SQL コネクタを構成しているときに、SQL Server またはテーブルの名前を変更する場合は、コネクタ用の [API アプリ] ブレードでこの操作を行うことができます。 

手順は次のとおりです。

1. コネクタまたは API アプリを開きます。 この操作を実行すると、API アプリ ブレードが開きます。
2.  **Essentials**, 、ホスト プロパティの下のハイパーリンクをクリックします。 ハイパーリンクの名前は *slackconnector* または *microsoftsqlconnector123*:

    ![][apiapphost]

3. API アプリ ホスト] ブレードで、次のように選択します。 **設定**します。 [設定] ブレードで、選択 **アプリケーション設定**します。 構成値については、「 **アプリ設定**: 
    
    ![][hostsettings]

4. 設定を変更するには、新しい値を入力する] をクリックし、 **保存** 変更します。 


## Hybrid Connection Manager をインストールする (省略可能)

![][hcsetup]

ハイブリッド接続マネージャーを使用すると、SQL Server、SAP などのオンプレミスのシステムに接続できます。 このハイブリッド接続は、Azure Service Bus を使用して接続し、Azure リソースとオンプレミスのリソースの間のセキュリティを制御します。

参照してください [Azure App Service での Hybrid Connection Manager を使用して](app-service-logic-hybrid-connection-manager.md)します。 

> [AZURE.NOTE] ハイブリッド接続マネージャーが、ファイアウォールの背後にある内部設置型リソースに接続している場合にのみ必要です。 内部設置型システムに接続していない場合、Hybrid Connection Manager 可能性があります、コネクタのブレードに表示されません。

## パフォーマンスを監視する
パフォーマンス メトリックは組み込み機能であり、作成するすべての API アプリに含まれます。 これらのメトリックは、Azure でホストされる API アプリに固有です。 メトリックの例:

![][monitoring]

そのための方法は次のとおりです。 

- 選択 **要求とエラー** よく知られている HTTP エラー コードを含むさまざまなパフォーマンス メトリックを追加するなどの 200 400、500 の HTTP ステータス コード。 応答時間、要求の数は、API アプリに対して実行された、データの量が有効になり、データの量がなくなる確認をもできます。 パフォーマンス メトリックに基づいて、メトリックが選択したしきい値を超えた場合に送信される電子メールアラートを作成できます。 
-  **使用状況**, 、どの程度わかり **CPU** は現在、API アプリによって使用される確認 **使用量クォータ** MB、およびコスト レベルに基づく最大データ使用します。 **推定費やす**  、API アプリの実行の潜在的なコストを判断できます。
- 選択 **プロセス** プロセス エクスプ ローラーを開きます。 プロセス エクスプローラーには、Web インスタンスとそのプロパティ (スレッド数、メモリ使用量など) が表示されます。

これらのツールを使用して、App Service プランをビジネス ニーズに基づいてスケール アップまたはスケール ダウンする必要があるかどうかを判断できます。 これらの機能は、追加ツールなしでポータルに組み込まれます。 

## セキュリティを制御する

API Apps では、ロールベースのセキュリティを使用します。 これらのロールは、API Apps とその他の Azure リソースを含む Azure のエクスペリエンス全体に適用されます。 次のロールがあります。

役割 | 説明
--- | ---
所有者 | 管理エクスペリエンスへのフル アクセスを持ち、その他のユーザーまたはグループにアクセス権を付与できます。
共同作成者 | 管理エクスペリエンスへのフル アクセスを持ちます。 その他のユーザーまたはグループにアクセス権を付与することはできません。
閲覧者 | 機密データを除くすべてのリソースを表示できます。
ユーザーアクセスの管理者 | すべてのリソースの表示、ロールの作成/管理、およびサポート チケットの作成/管理を実行できます。

参照してください [Microsoft Azure ポータルでのロールベースのアクセス制御](role-based-access-control-configure.md)します。

API アプリへのユーザーの追加と特定のロールの割り当ては簡単に実行できます。 アクセス権があるユーザーと、各自に割り当てられたロールがポータルに表示されます。

![][access]  

- 選択 **ユーザー** ユーザーを追加、役割を割り当てるユーザーを削除します。
- 選択 **ロール** 表示する特定のロール内のすべてのユーザー、ロールにユーザーを追加し、ロールからユーザーを削除します。 


## その他の便利な機能
- 選択 **API の定義** を自動的に作成された Swagger ファイルを特定の API アプリを開きます。
- 選択 **の依存関係** を API アプリに必要なファイルを表示します。 たとえば、SAP コネクタを使用している場合は、オンプレミスの Hybrid Connection Manager に追加ファイルがいくつかインストールされます。 これらの依存関係は、[API アプリ] ブレードに表示されます。 

> [AZURE.IMPORTANT] API アプリのプロパティを開くし、下を見て **Essentials**, 、ある **ホスト** と **ゲートウェイ** 新しいブレードを開くリンク。
> 
> ![][host]
> 
> これらのプロパティは、API アプリをホストする Web サイトに固有です。 これらのプロパティを更新しないことをお勧めするには、これらのプロパティのほとんどが適用されない実際に、組み込みの API アプリまたはコネクタを使用する場合。 Visual Studio で独自の API アプリを作成し、Azure サブスクリプションにデプロイした場合は、[ホスト] ブレードと [ゲートウェイ] ブレードを使用できます。 [API アプリを管理](../app-service-api/app-service-api-manage-in-portal.md) 何ができるこれらのブレードで、[カスタム作成した API アプリについて詳しく説明します。 


>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 詳細

[Logic Apps を監視する](app-service-logic-monitor-your-logic-apps.md)<br/>
[コネクタと App Service で API Apps の一覧](app-service-logic-connectors-list.md)<br/>
[Microsoft Azure ポータルでのロールベースのアクセス制御](role-based-access-control-configure.md)<br/>
[Azure App Service での Hybrid Connection Manager の使用](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png

