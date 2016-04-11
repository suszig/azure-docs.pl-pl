<properties
   pageTitle="Migrating from Orchestrator to Azure Automation | Microsoft Azure"
   description="System Center Orchestrator の Runbook と統合パックを Azure Automation に移行する方法を説明します。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/11/2015"
   ms.author="bwren" />


# Orchestrator から Azure Automation (ベータ版) へ移行する

Runbook で [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) は Azure Automation の runbook は、Windows PowerShell に基づいていますに、Orchestrator 専用に記述された統合パック活動に基づいています。  [グラフィカルな runbook](automation-runbook-types#graphical-runbooks) Azure Automation の PowerShell コマンドレット、子 runbook、および資産を表す活動に Orchestrator runbook のような外観にする場合します。

 [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) runbook を Orchestrator から Azure Automation に変換するためのツールが含まれています。  Runbook 自体を変換するだけでなく、Runbook で使用される活動を含む統合パックを Windows PowerShell コマンドレットを含む統合モジュールに変換する必要もあります。  

Orchestrator Runbook を Azure Automation に変換するための基本的なプロセスを次に示します。  これらの各手順の詳細については、以降のセクションで詳しく説明します。

1.  ダウンロード、 [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) ツールとモジュールのこの記事で説明が含まれています。
2.  インポート [標準活動モジュール](#standard-activities-module) を Azure Automation にします。  このモジュールには、変換後の Runbook が使用する可能性がある、変換されたバージョンの Orchestrator 標準活動が含まれます。
3.  インポート [System Center Orchestrator 統合モジュール](#system-center-orchestrator-integration-modules) System Center にアクセスする、runbook で使用する統合パック用の Azure Automation にします。
4.  カスタムとサード パーティ製の統合パックを使用して変換、 [統合パック コンバーター](#integration-pack-converter) し、Azure Automation にインポートします。
5.  Orchestrator runbook を使用して変換、 [Runbook コンバーター](#runbook-converter) し Azure Automation にインストールします。
6.  Azure Automation で必要な Orchestrator のアセットを手動で作成します。Runbook コンバーターではこれらのリソースが変換されません。
7.  構成、 [Hybrid Runbook Worker](#hybrid-runbook-worker) ローカル リソースにアクセスする変換後の runbook を実行するローカル データ センターにします。

## Service Management Automation

[Service Management Automation](http://technet.microsoft.com/library/dn469260.aspx) は Azure Automation と同じ統合モジュールを使用して (SMA) を格納して、Orchestrator などのローカル データ センターに runbook を実行します。  [Runbook コンバーター](#runbook-converter) に変換 Orchestrator runbook グラフィカル runbook にはこれで、SMA はサポートされていません。  インストールすることも、 [標準活動モジュール](#standard-activities-module) と [System Center Orchestrator 統合モジュール](#system-center-orchestrator-integration-modules) 、SMA に手動でする必要がありますが、 [、runbook を書き直して](http://technet.microsoft.com/library/dn469262.aspx)します。

## Hybrid Runbook Worker

Orchestrator の Runbook はデータベース サーバー上に格納され、Runbook サーバーで実行されます。どちらのサーバーもローカル データセンター上にあります。  Azure Automation の Runbook は、Azure クラウドに格納され、使用してローカル データ センターで実行できる、 [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)します。  これが、Orchestrator から変換された Runbook を実行する通常の方法です。これらの Runbook はローカル サーバー上で実行されるように設計されているためです。

## 統合パック コンバーター

統合パック コンバーターを使用して作成された統合パックの変換、 [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) 統合モジュールを Azure Automation または Service Management Automation にインポートできる Windows PowerShell をベースにします。  

統合パック コンバーターを実行すると、ウィザードが表示され、統合パック (.oip) ファイルを選択できます。  次に、ウィザードに、その統合パックに含まれる活動が一覧表示され、移行する活動を選択できます。  ウィザードを完了すると、元の統合パックの各活動に対応するコマンドレットを含む統合モジュールが作成されます。


### パラメーター

統合パック内の活動のプロパティは、その統合モジュールで対応するコマンドレットのパラメーターに変換されます。  Windows PowerShell コマンドレットのセットがある [共通パラメーター](http://technet.microsoft.com/library/hh847884.aspx) すべてのコマンドレットで使用できます。  たとえば、-Verbose パラメーターを使用すると、対象操作についての詳細情報を出力するコマンドレットが実行されます。  コマンドレットに、共通パラメーターと同じ名前のパラメーターを含めることはできません。  活動に共通パラメーターと同じ名前を持つプロパティが含まれる場合、対象パラメーターに別の名前を指定するように求めるプロンプトがウィザードによって表示されます。

### モニター活動

モニター runbook に Orchestrator スタートで、 [アクティビティを監視](http://technet.microsoft.com/library/hh403827.aspx) し、特定のイベントが呼び出されるを待機継続的に実行します。  Azure Automation ではモニター Runbook がサポートされていないため、統合パックのモニター活動は変換されません。  代わりに、プレース ホルダー コマンドレットが、モニター活動用に統合モジュールに作成されます。  このコマンドレットには機能がありませんが、それを使用する変換後 Runbook がインストール可能になります。  この Runbook は、Azure Automation で実行することはできませんが、インストールが可能なので変更できます。

### 変換できない統合パック

OIT を使用して作成されていない統合パックは、統合パック コンバーターを使用して変換できません。 現在このツールを使用して変換できない、Microsoft が提供する統合パックもいくつかあります。  これらの統合パックの変換後のバージョンであった [ダウンロード用に提供](#system-center-orchestrator-integration-modules) できるように、これらは、Azure Automation または Service Management Automation にインストールできます。


## 標準活動モジュール

Orchestrator にはセットが含まれています [標準活動](http://technet.microsoft.com/library/hh403832.aspx) を統合パックには含まれませんが、複数の runbook によって使用されます。  標準活動モジュールは、これらの各活動に対応するコマンドレットが含まれている統合モジュールです。  標準活動を使用する変換された Runbook をインポートする前に、Azure Automation にこの統合モジュールをインストールする必要があります。

標準活動モジュールのコマンドレットは、変換された Runbook をサポートするだけでなく、Orchestrator に精通しているユーザーが Azure Automation で新しい Runbook をビルドするために使用できます。  標準活動すべての機能はコマンドレットで実行できますが、動作方法が異なる場合があります。  変換後の標準活動モジュール内のコマンドレットは、対応する活動と同じように動作し、同じパラメーターを使用します。  これによって、既存の Orchestrator Runbook の作成者は Azure Automation の Runbook に移行するのが容易になります。

## System Center Orchestrator 統合モジュール

Microsoft は、 [統合パック](http://technet.microsoft.com/library/hh295851.aspx) の System Center コンポーネントとその他の製品を自動化する runbook をビルドします。  現在、これらの統合パックの一部は OIT に基づいていますが、既知の問題により、統合モジュールに変換できません。  [System Center Orchestrator 統合モジュール](https://www.microsoft.com/download/details.aspx?id=49555) 変換後のバージョン Azure Automation と Service Management Automation にインポートできる統合パックにはが含まれています。  

このツールの RTM バージョンにより、統合パック コンバーターを使用して変換できる、OIT に基づく統合パックの更新版が公開されます。  OIT に基づいていない統合パックの活動を使用する Runbook を変換する場合に役立つガイダンスも提供されます。

## Runbook コンバーター

Runbook コンバーターの変換に Orchestrator runbook [グラフィカル runbook](automation-runbook-types.md#graph-runbooks) Azure Automation にインポートできます。  

Runbook コンバーターと呼ばれるコマンドレットで PowerShell モジュールとして実装される **ConvertFrom SCORunbook** 変換を実行します。  ツールをインストールすると、このコマンドレットを読み込む PowerShell セッションへのショートカットが作成されます。   

Orchestrator Runbook を変換して Azure Automation にインポートするための基本的なプロセスを次に示します。  以降のセクションでは、ツールの使用と変換後の Runbook の操作についてさらに詳しく説明します。

1. Orchestrator から 1 つ以上の Runbook をエクスポートします。
2. Runbook のすべての活動の統合モジュールを取得します。
3. エクスポートされたファイルで Orchestrator Runbook を変換します。
4. ログの情報を見て、変換を確認して、必要な手動のタスクを判断します。
4. 変換後の Runbook を Azure Automation にインポートします。
5. Azure Automation で必要なアセットを作成します。
6. Azure Automation の Runbook を編集して必要な活動を変更します。

### Runbook コンバーターの使用

構文 **ConvertFrom SCORunbook** は次のようになります。

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - 変換する Runbook を含むエクスポート ファイルへのパス。
- Module - Runbook の活動を含む統合モジュールのコンマ区切りリスト。
- OutputFolder - 変換後のグラフィカル Runbook を作成するフォルダーへのパス。 


次のコマンドの例と呼ばれるエクスポート ファイル内の runbook を変換する **MyRunbooks.ois_export**します。  これらの Runbook では、Active Directory と Data Protection Manager の統合パックが使用されています。

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### ログ ファイル

Runbook コンバーターでは、変換後の Runbook と同じ場所に次のログ ファイルが作成されます。  ファイルが既に存在する場合は、最後に行われた変換の情報で上書きされます。

| ファイル | 目次 |
|:---|:---|
| Runbook Converter - Progress.log | 変換が正常に完了した各活動の情報や、変換されなかった各活動の警告など、変換の詳細な手順。 |
| Runbook Converter - Summary.log  | 警告や、実行が必要なフォローアップ タスク (例: 変換後の Runbook に必要な変数の作成) など、最後に行われた変換の概要。  |

### Orchestrator からの Runbook のエクスポート

Runbook コンバーターは、1 つ以上の Runbook が含まれる、Orchestrator からのエクスポート ファイルを扱います。  これにより、各 Orchestrator Runbook に対応する Azure Automation Runbook がエクスポート ファイルに作成されます。  

Orchestrator から runbook をエクスポートするには、Runbook Designer で runbook の名前を右クリックして **エクスポート**します。  フォルダー内のすべての runbook をエクスポートするには、フォルダーの名前を右クリックし **エクスポート**します。


### Runbook アクティビティ

Runbook コンバーターは、Orchestrator Runbook の各活動を Azure Automation の対応する活動に変換します。  変換できない活動の場合は、プレースホルダー活動が警告テキストと共に Runbook に作成されます。  変換された Runbook を Azure Automation にインポートした後、これらの活動のいずれかを、必要な機能を実行する有効な活動に置き換える必要があります。

Orchestrator 活動、 [標準活動モジュール](#standard-activities-module) 変換されます。  ただし、Orchestrator 標準活動には、このモジュールに含まれていないため変換されないものもあります。  たとえば、 **プラットフォーム イベントの送信** イベントは、Orchestrator に固有なのでは、Azure Automation と同じがありません。

[アクティビティを監視](https://technet.microsoft.com/library/hh403827.aspx) に相当する Azure Automation ではありませんので、変換されません。  例外、モニター活動 [統合パックを変換](#integration-pack-converter) をプレース ホルダーのアクティビティに変換されます。

すべてのアクティビティから、 [統合パックを変換](#integration-pack-converter) で統合モジュールへのパスを指定する場合に変換、 **モジュール** パラメーター。  System Center の統合パックを使用することができます、 [System Center Orchestrator 統合モジュール](#system-center-orchestrator-integration-modules)します。


### Orchestrator のリソース

Runbook コンバーターで変換されるのは Runbook のみで、カウンター、変数、接続など、その他の Orchestrator リソースは変換されません。  Azure Automation では、カウンターがサポートされていません。  変数と接続はサポートされていますが、手動で作成する必要があります。  ログ ファイルでは、Runbook にこのようなリソースが必要かどうかが示され、変換後の Runbook が正常に動作するために Azure Automation に作成する必要がある対応リソースが指定されます。

たとえば、Runbook では、変数を使用して、活動の特定の値を設定することがあります。  変換後の Runbook は、活動を変換し、Orchestrator の変数と同じ名前の変数アセットを Azure Automation で指定します。  これで通知、 **Runbook コンバーター - Summary.log** 変換した後に作成されるファイル。  Runbook を使用する前に、この変数アセットを Azure Automation で手動で作成する必要があります。 

 
### 入力パラメーター

Orchestrator の Runbook の入力パラメーターを受け入れる、 **データの初期化** アクティビティ。  Runbook を変換するには、このアクティビティが含まれている場合、 [入力パラメーター](automation-graphical-authoring-intro.md#runbook-input-and-output) Azure Automation で runbook をアクティビティの各パラメーターの作成します。  A [ワークフロー スクリプト コントロール](automation-graphical-authoring-intro.md#activities) 取得し、各パラメーターを返す変換された runbook にアクティビティを作成します。  入力パラメーターを使用する、Runbook のすべての活動は、この活動からの出力を参照します。

この方法が使用される理由は、Orchestrator Runbook の機能を最もうまく反映するためです。  新しいグラフィカル Runbook の活動は、Runbook の入力データ ソースを使用して入力パラメーターを直接参照します。


### Runbook 活動の呼び出し

Orchestrator の Runbook とその他の runbook の起動、 **Runbook の呼び出し** アクティビティ。 Runbook を変換するには、このアクティビティが含まれている場合、 **の完了を待機** オプションを設定し、変換後の runbook での runbook 活動が作成されています。  場合、 **の完了を待機** オプションが設定されていないを使用するワークフローのスクリプト活動を作成し、 **Start-azureautomationrunbook** runbook を開始します。  変換後の Runbook を Azure Automation にインポートした後、活動で指定された情報を使用して、この活動を変更する必要があります。




## 関連記事:

- [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx)
- [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)
- [Orchestrator 標準活動](http://technet.microsoft.com/library/hh403832.aspx)
 

