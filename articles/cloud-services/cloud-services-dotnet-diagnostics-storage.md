<properties
  pageTitle="Azure ストレージに診断データの保存と表示 |Microsoft Azure"
  description="Azure Storage に Azure 診断データを保存し、それを表示する"
  services="cloud-services"
  documentationCenter=".net"
  authors="rboucher"
  manager="jwhit"
  editor="tysonn" />
<tags
  ms.service="cloud-services"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/21/2015"
  ms.author="robb" />

# Azure ストレージに診断データの保存と表示

診断データは、Microsoft Azure ストレージ エミュレーターまたは Azure ストレージに転送しない限り、永続的に保存されません。 診断データは、いったんストレージに保存されると、用意されているいくつかのツールの 1 つを使用して確認することができます。

## ストレージ アカウントを指定します。

ServiceConfiguration.cscfg ファイル内で使用するストレージ アカウントを指定します。 アカウント情報は、構成設定で接続文字列として定義されます。 次の例は、Visual Studio で新しいクラウド サービス プロジェクト用に作成された既定の接続文字列を示しています。


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

この接続文字列を変更することで、Azure ストレージ アカウントのアカウント情報を指定できます。

収集される診断データの種類に応じて、Azure 診断では BLOB サービスまたは Table サービスのいずれかを使用します。 次の表では、保持されるデータ ソースとその形式を示します。

|データ ソース|ストレージ形式|
|---|---|
|Azure ログ|テーブル|
|IIS 7.0 ログ|BLOB|
|Azure 診断インフラストラクチャ ログ|テーブル|
|失敗した要求トレース ログ|BLOB|
|Windows イベント ログ|テーブル|
|パフォーマンス カウンター|テーブル|
|クラッシュ ダンプ|BLOB|
|カスタム エラー ログ|BLOB|

## 診断データを転送します。

SDK 2.5 以降では、診断データの転送要求は構成ファイルを介して発生します。 構成で指定したスケジュール間隔で、診断データを転送することができます。

SDK 2.4 およびそれ以前のバージョンでは、構成ファイルを介して、またプログラムによって、診断データの転送を要求することができます。 プログラムを使用した方法では、オンデマンド転送を行うこともできます。


>[AZURE.IMPORTANT] Azure ストレージ アカウントに診断データを転送する場合は、診断データを使用するストレージ リソースのコストが発生します。

## 診断データを格納します。

ログ データは、次の名前の BLOB ストレージまたはテーブル ストレージに保存されます。

**テーブル**

- **WadLogsTable** - トレース リスナーを使用して、コードで記述されたログ。

- **WADDiagnosticInfrastructureLogsTable** -診断モニターと構成の変更。

- **WADDirectoriesTable** – 診断モニターが監視しているディレクトリです。  これには、IIS ログ、IIS 失敗要求ログ、およびカスタム ディレクトリが含まれます。  BLOB ログ ファイルの場所は Container フィールドで指定され、BLOB の名前は RelativePath フィールドで指定されます。  AbsolutePath フィールドでは、Azure 仮想マシンに存在するファイルの場所と名前を示します。

- **WADPerformanceCountersTable** – パフォーマンス カウンターです。

- **WADWindowsEventLogsTable** – Windows イベント ログに記録します。

**BLOB**

- **wad コントロール コンテナー** – (SDK 2.4 ののみまたはそれ以前) Azure 診断を制御する XML 構成ファイルが含まれています。

- **wad iis-failedreqlogfiles** – IIS の失敗した要求ログからの情報が含まれています。

- **wad の iis ログファイル** – IIS ログに関する情報が含まれています。

- **"custom"** – 診断モニターによって監視されるディレクトリの構成に基づくカスタム コンテナーです。  この BLOB コンテナーの名前は WADDirectoriesTable で指定されます。

## 診断データを表示するツール
ストレージへの転送後にデータを表示するには、いくつかのツールを利用できます。 次に例を示します。

- **Visual Studio サーバー エクスプ ローラー** の場合は、Azure Tools for Microsoft Visual Studio をインストール済みノードを使用して、Azure ストレージ サーバー エクスプ ローラーで、Azure ストレージ アカウントからの読み取り専用の blob およびテーブル データを表示します。 ローカル ストレージ エミュレーター アカウントからデータを表示することができ、に加えてストレージ アカウントから、作成した Azure のします。 詳細については、次を参照してください。 [サーバー エクスプ ローラーを使用したストレージ リソース](https://msdn.microsoft.com/library/ff683677.aspx)します。

- **Neudesic の azure のストレージ エクスプ ローラー** - [Azure ストレージ エクスプ ローラー](http://azurestorageexplorer.codeplex.com/) 検査およびに Azure アプリケーションのログを含む Azure ストレージ プロジェクト内のデータを変更するための便利なグラフィカル ユーザー インターフェイス ツールです。 このツールをダウンロードするを参照してください。 [Azure ストレージ エクスプ ローラー](http://azurestorageexplorer.codeplex.com/)します。

- Cerebrata の azure Diagnostics Manager [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) Windows (WPF) ベースのクライアントは、Azure 診断を管理するためです。 これは、ため、表示、ダウンロード、および Azure で実行されているアプリケーションによって収集された診断データを管理することができます。 このツールをダウンロードするを参照してください。 [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx)します。

## 次のステップ

[Azure 診断で Cloud Services アプリケーションのフローをトレースする](cloud-services-dotnet-diagnostics-trace-flow.md)

