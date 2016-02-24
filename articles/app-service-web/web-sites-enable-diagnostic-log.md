<properties
    pageTitle="Azure App Service の Web アプリの診断ログの有効化"
    description="診断ログを有効にしてインストルメンテーションをアプリケーションに追加する方法と、Azure によってログ記録された情報にアクセスする方法を説明します。"
    services="app-service"
    documentationCenter=".net"
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2015"
    ms.author="cephalin"/>

# Azure App Service の Web アプリの診断ログの有効化

## 概要

Azure は、デバッグを容易に組み込みの診断機能を提供する [App Service web アプリ](http://go.microsoft.com/fwlink/?LinkId=529714)します。 この記事では、診断ログを有効にしてインストルメンテーションをアプリケーションに追加する方法と、Azure によってログに記録された情報にアクセスする方法について説明します。

この記事では、 [Azure ポータル](https://portal.azure.com), 、診断ログを使用するには、Azure PowerShell と Azure コマンド ライン インターフェイス (Azure CLI)。 Visual Studio を使用して、診断ログの使用方法の詳細については、次を参照してください。 [Visual Studio での Azure のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Web サーバーの診断とアプリケーションの診断

App Service Web Apps は、Web サーバーと Web アプリケーションの両方のログ情報を診断する機能を備えています。 これらは論理的に分けられます **web サーバー診断** と **アプリケーション診断**します。

### Web サーバー診断

次の種類のログを有効または無効にできます。

- **詳細なエラー ログ** -障害 (ステータス コード 400 以上) を示す HTTP ステータス コードのエラー情報を詳しく説明します。 このログには、サーバーがエラー コードを返した理由を特定するために役立つ情報が記録されている場合があります。
- **失敗した要求トレース** -各コンポーネントにかかった時間、要求を処理するために使用する IIS コンポーネントのトレースなど、失敗した要求に関する詳細情報。 このログが便利なのは、サイトのパフォーマンスを向上させたり、特定の HTTP エラーが返される理由を特定したりする場合です。
- **Web サーバーのログ記録** -を使用して、HTTP トランザクションに関する情報、 [W3C 拡張ログ ファイル形式](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx)します。 このレポートが便利なのは、全体的なサイト メトリック、たとえば、サイトで処理された要求の数や、特定の IP アドレスからの要求の数を特定するときです。

### アプリケーション診断

アプリケーション診断では、Web アプリケーションによって生成された情報を取り込むことができます。 ASP.NET アプリケーションで使用できる、 [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) 情報をアプリケーション診断ログに記録するクラス。 次に例を示します。

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

実行時にこれらのログを取得してトラブルシューティングに役立てることができます。 詳細については、次を参照してください。 [Visual Studio で web アプリを Azure のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

App Service Web Apps は、Web アプリにコンテンツをパブリッシュしたときのデプロイ情報もログに記録します。 これは自動的に行われ、デプロイ ログの構成設定はありません。 デプロイ ログでは、デプロイが失敗した理由を特定できます。 たとえば、カスタムのデプロイ スクリプトを使用している場合は、デプロイ ログを使用して、スクリプトでエラーが発生する理由を特定できることがあります。

## <a name="enablediag"></a>方法: 診断を有効にする

診断を有効にする、 [Azure ポータル](https://portal.azure.com), web アプリのブレードに移動し、をクリックして、 **設定 > 診断ログ**します。

<!-- todo:cleanup dogfood addresses in screenshot -->
![Logs part](./media/web-sites-enable-diagnostic-log/logspart.png)

有効にすると **アプリケーション診断** 選択することも、 **レベル**します。 この設定では、取得された情報をフィルター処理できます。 **情報**, 、**警告** または **エラー** 情報。 これを設定する **詳細** アプリケーションによって生成されたすべての情報がログに記録します。

> [AZURE.NOTE] Web.config ファイルを変更するとは異なりアプリケーション診断を有効化や診断ログ レベルを変更リサイクルはしません内で、アプリケーションが実行されるアプリケーション ドメイン。

 [旧ポータル](https://manage.windowsazure.com) Web アプリ **構成** ] タブを選択できます **ストレージ** または **ファイル システム** の **web サーバーのログ記録**します。 選択すると **ストレージ** では、ストレージ アカウントと、ログの書き込み先の blob コンテナーを選択できます。 その他のすべてのログを **サイト診断** ファイル システムのみに書き込まれます。

 [旧ポータル](https://manage.windowsazure.com) Web アプリ **構成** タブには、アプリケーション診断用の他の設定もがあります。

* **ファイル システム** -アプリケーション診断情報は、web アプリのファイル システムに保存します。 これらのファイルは、FTP によってアクセスするか、Azure PowerShell または Azure コマンド ライン ツールを使用して Zip アーカイブとしてダウンロードできます。
* **テーブル ストレージ** -指定された Azure ストレージ アカウントとテーブル名にアプリケーション診断情報を格納します。
* **Blob ストレージ** -指定された Azure ストレージ アカウントおよび blob コンテナーにアプリケーション診断情報を格納します。
* **保有期間** -既定では、ログは自動的に削除されませんから **blob ストレージ**します。 選択 **の保有期間設定** とログを自動的に削除するには、ログを保有する日数を入力します。

>[AZURE.NOTE] 場合、 [ストレージ アカウントのアクセス キーを再生成](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys), 、更新されたキーを使用するそれぞれのログの構成をリセットする必要があります。 これを行うには、次の手順を実行します。
>
> 1.  **構成** に、それぞれのログ機能の設定] タブで、 **オフ**します。 設定を保存します。
> 2. ストレージ アカウントの BLOB またはテーブルへのログを再び有効にします。 設定を保存します。

ファイル システム、テーブル ストレージ、BLOB ストレージへのログ記録は、任意に組み合わせて同時に有効にすることができます。また、それぞれ個別にログ レベルを設定できます。 たとえば、BLOB ストレージへのエラーと警告の長期間のログ記録、ファイル システムへの詳細レベルのログ記録を同時に有効にすることができます。

次の 3 つの記憶域の場所をすべてログに記録されたイベントについて同じ基本的な情報を提供する **テーブル ストレージ** と **blob ストレージ** インスタンス ID、スレッド ID、およびログ記録するよりもより詳細なタイムスタンプ (目盛り形式) などの追加情報をログ **ファイル システム**します。

> [AZURE.NOTE] 格納された情報 **テーブル ストレージ** または **blob ストレージ** ストレージ クライアントまたはこれらのストレージ システムを直接操作できる、アプリケーションを使用してのみアクセスできます。 たとえば、Visual Studio 2013 のストレージ エクスプローラーを使用すると、テーブル ストレージまたは BLOB ストレージを操作できます。HDInsight を使用すると、BLOB ストレージに格納されているデータにアクセスできます。 いずれかを使用して Azure Storage にアクセスするアプリケーションを記述することも、 [Azure Sdk](/downloads/#)します。

> [AZURE.NOTE] Azure PowerShell から診断も有効にするを使用して、 **Set-azurewebsite** コマンドレットです。 Azure PowerShell をインストールしていないか、Azure サブスクリプションを使用するように構成していない場合は、次を参照してください。 [Azure powershell の使用方法](/develop/nodejs/how-to-guides/powershell-cmdlets/)します。

##<a name="download"></a> 方法: ログをダウンロードする

Web アプリケーション ファイル システムに保存された診断情報には、FTP を使用して直接アクセスできます。 さらに、Azure PowerShell または Azure コマンド ライン インターフェイスを使用して Zip アーカイブとしてダウンロードすることもできます。

ログが保存されるディレクトリ構造は次のとおりです。

* **アプリケーション ログ** -/logfiles/application/。 このフォルダーには、アプリケーション ログによって生成された情報を含む 1 つ以上のテキスト ファイルが格納されます。

* **失敗した要求トレース** -LogFiles/W3SVC ###/です。 このフォルダーには、1 つの XSL ファイルと 1 つ以上の XML ファイルが格納されます。 この XSL ファイルは、XML ファイルが Internet Explorer で表示されるときに、コンテンツの書式設定とフィルター処理を行う役割を果たすため、必ず XML ファイルと同じディレクトリにダウンロードしてください。

* **エラー ログの詳細な** -/logfiles/detailederrors/。 このフォルダーには、発生した HTTP エラーに関する詳細な情報を記録した 1 つ以上の .htm ファイルが格納されます。

* **Web サーバー ログ** -/LogFiles/http/RawLogs。 このフォルダーには、いずれかが含まれているか、以上のテキスト ファイル形式を使用して、 [W3C 拡張ログ ファイル形式](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx)します。

* **デプロイ ログ** -/logfiles/git。 このフォルダーには、Git デプロイのログだけでなく、Azure Web Apps が使用する内部デプロイ プロセスによって生成されたログも格納されます。

### FTP

FTP を使用して診断情報にアクセスするを参照してください。、 **ダッシュ ボード** で web アプリの、 [旧ポータル](https://manage.windowsazure.com)します。  **概要** セクションで、使用して、 **FTP 診断ログ** FTP を使用してログ ファイルにアクセスするリンク。  **デプロイ/FTP ユーザー** エントリには、FTP サイトへのアクセスに使用するユーザー名が一覧表示します。

> [AZURE.NOTE] 場合、 **デプロイ/FTP ユーザー** エントリが設定されていないか、このユーザーのパスワードを忘れた場合を使用して、新しいユーザー名とパスワードを作成することができます、 **展開資格情報のリセット** 内のリンク、 **概要** のセクションで、 **ダッシュ ボード**します。

### Azure PowerShell を使用してダウンロードする

ログ ファイルをダウンロードするには、Azure PowerShell の新しいインスタンスを開始し、次のコマンドを使用します。

    Save-AzureWebSiteLog -Name webappname

これによって指定された web アプリのログを保存する、 **-名前** パラメーターという名前のファイルを **logs.zip** 現在のディレクトリにします。

> [AZURE.NOTE] Azure PowerShell をインストールしていないか、Azure サブスクリプションを使用するように構成していない場合は、次を参照してください。 [Azure powershell の使用方法](/develop/nodejs/how-to-guides/powershell-cmdlets/)します。

### Azure コマンド ライン インターフェイスを使用してダウンロードする

Azure コマンド ライン インターフェイスを使用してログ ファイルをダウンロードするには、新しいコマンド プロンプト、PowerShell、Bash、ターミナル セッションを開き、次のコマンドを入力します。

    azure site log download webappname

これは、という名前のファイルには、"webappname"という名前の web アプリのログを保存する **diagnostics.zip** 現在のディレクトリにします。

> [AZURE.NOTE] Azure コマンド ライン インターフェイス (Azure CLI) をインストールしていないか、Azure サブスクリプションを使用するように構成していない場合は、次を参照してください。 [Azure cli の使用方法](../xplat-cli-install.md)します。

## Application Insights でログを表示する方法

Visual Studio Application Insights には、ログをフィルターおよび検索したり、要求やその他のイベントにログを関連付けたりするためのツールが用意されています。

1. Application Insights SDK を Visual Studio のプロジェクトに追加します。
 * ソリューション エクスプローラーでプロジェクトを右クリックし、[Application Insights の追加] を選択します。 Application Insights リソースの作成などの手順が示されます。 [詳細については](../application-insights/app-insights-start-monitoring-app-health-usage.md)
2. トレース リスナーのパッケージをプロジェクトに追加します。
 * プロジェクトを右クリックし、[NuGet パッケージの管理] を選択します。 選択 `Microsoft.ApplicationInsights.TraceListener` [の詳細](../application-insights/app-insights-asp-net-trace-logs.md)
3. プロジェクトをアップロードして実行し、ログ データを生成します。
4.  [Azure ポータル](http://portal.azure.com/), 、新しい Application Insights リソースを参照し、開く **検索**します。 ログ データが、要求、使用状況、およびその他の製品利用統計情報と共に表示されます。 一部の製品利用統計情報については、表示されるまで数分かかる場合があります。[更新] をクリックします。 [詳細については](../application-insights/app-insights-diagnostic-search.md)

[Application Insights でのパフォーマンス追跡についての詳細情報](../insights-perf-analytics.md)

##<a name="streamlogs"></a> 方法: ログをストリーミングする

アプリケーションの開発中に、ログ情報をほぼリアルタイムで参照すると役立つことがよくあります。 これは、Azure PowerShell または Azure コマンド ライン インターフェイスを使用して開発環境にログ情報をストリーミングすることで実現できます。

> [AZURE.NOTE] いくつかの種類のログ バッファーは、発生する可能性が順不同のイベント ストリームのログ ファイルに書き込みます。 たとえば、ユーザーがページにアクセスしたときに発生するアプリケーション ログ エントリは、ページ要求の該当する HTTP ログ エントリより前のストリームに表示されることがあります。

> [AZURE.NOTE] ログのストリーミングに格納されている任意のテキスト ファイルに書き込まれた情報のストリーミングも、 **D:\\home\\LogFiles\\** フォルダーです。

### Azure PowerShell を使用してストリーミングする

ログ情報をストリーミングするには、Azure PowerShell を新たに起動して次のコマンドを使用します。

    Get-AzureWebSiteLog -Name webappname -Tail

これで指定された web アプリに接続、 **-名前** パラメーターし、ログ イベントが web アプリで発生したら、PowerShell ウィンドウへの情報のストリーミングを開始します。 /LogFiles ディレクトリ (d:/home/logfiles) に格納されており、末尾が .txt、.log、.htm のいずれかになっているファイルに書き込まれた情報は、ローカル コンソールにストリーミングされます。

エラーなどの特定のイベントをフィルター処理を使用して、 **-メッセージ** パラメーター。 次に例を示します。

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

HTTP などの特定のログの種類のフィルターを使用して、 **-パス** パラメーター。 次に例を示します。

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

使用可能なパスの一覧を表示するには、-ListPath パラメーターを使用します。

> [AZURE.NOTE] Azure PowerShell をインストールしていないか、Azure サブスクリプションを使用するように構成していない場合は、次を参照してください。 [Azure powershell の使用方法](/develop/nodejs/how-to-guides/powershell-cmdlets/)します。

### Azure コマンド ライン ツールを使用してストリーミングする

ログ情報をストリーミングするには、新しいコマンド プロンプト、PowerShell、Bash、またはターミナル セッションを開き、次のコマンドを入力します。

    azure site log tail webappname

"webappname" という名前の Web アプリに接続され、ログ イベントが Web アプリで発生したら、ウィンドウへの情報のストリーミングが開始されます。 /LogFiles ディレクトリ (d:/home/logfiles) に格納されており、末尾が .txt、.log、.htm のいずれかになっているファイルに書き込まれた情報は、ローカル コンソールにストリーミングされます。

エラーなどの特定のイベントをフィルター処理を使用して、 **--フィルター** パラメーター。 次に例を示します。

    azure site log tail webappname --filter Error

HTTP などの特定のログの種類のフィルターを使用して、 **--パス** パラメーター。 次に例を示します。

    azure site log tail webappname --path http

> [AZURE.NOTE] Azure コマンド ライン インターフェイスをインストールしていないか、Azure サブスクリプションを使用するように構成していない場合は、次を参照してください。 [Azure コマンド ライン インターフェイスの使用方法](../xplat-cli-install.md)します。

##<a name="understandlogs"></a> 方法: 診断ログを読む

### アプリケーション診断ログ

アプリケーション診断では、ファイル システム、テーブル ストレージ、BLOB ストレージのうち、どれにログを保存するかに応じて、.NET アプリケーション向けの一定の形式で情報が保存されます。 格納される一連の基本的なデータは、3 種類のすべてのストレージ間で同じで、イベントが発生した日時、イベントを生成したプロセスの ID、イベントの種類 (情報、警告、エラー)、イベントのメッセージです。

__ファイル システム__

ファイル システムにログが記録される行またはストリーミングによって受信する行は、それぞれ以下の形式になります。

    {Date}  PID[{process id}] {event type/level} {message}

たとえば、エラー イベントは次のようになります。

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

ファイル システムにログ記録する場合は、使用できる 3 つのログ記録方法のうちで最も基本的な情報が提供され、時間、プロセス ID、イベント レベル、メッセージのみを確認できます。

__テーブル ストレージ__

テーブル ストレージにログ記録する場合は、追加のプロパティを使用して、テーブルに格納されているデータだけでなく、イベントに関するより詳細な情報も簡単に検索できます。 テーブルに格納される各エンティティ (行) に次のプロパティ (列) が使用されます。

プロパティ名|値/形式
---|---
PartitionKey|yyyyMMddHH の形式によるイベントの日時
RowKey|このエンティティを一意に識別する GUID 値
Timestamp|イベントが発生した日時
EventTickCount|イベントが発生した目盛り形式 (高精度) の日時
ApplicationName|Web アプリケーション名
Level|イベント レベル (例: エラー、警告、情報)
EventId|このイベントのイベント ID<p><p>指定されていない場合、既定値は 0
InstanceId|イベントが発生した Web アプリケーションのインスタンス
Pid|プロセス ID
Tid|イベントを生成したスレッドの ID
メッセージ|イベントの詳細メッセージ

__BLOB ストレージ__

BLOB ストレージにログを記録するときには、値をコンマで区切った (CSV) 形式で格納されます。 テーブル ストレージと同様、追加のフィールドがログに記録されて、イベントについてより詳細な情報が提供されます。 CSV 内の各行に次のプロパティが使用されます。

プロパティ名|値/形式
---|---
Date|イベントが発生した日時
Level|イベント レベル (例: エラー、警告、情報)
ApplicationName|Web アプリケーション名
InstanceId|イベントが発生した Web アプリケーションのインスタンス
EventTickCount|イベントが発生した目盛り形式 (高精度) の日時
EventId|このイベントのイベント ID<p><p>指定されていない場合、既定値は 0
Pid|プロセス ID
Tid|イベントを生成したスレッドの ID
メッセージ|イベントの詳細メッセージ

BLOB に格納されるデータは次のようになります。

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [AZURE.NOTE] この例で表されるように、ログの最初の行は列ヘッダーを含めます。

### 失敗した要求トレース

失敗した要求トレースはという名前の XML ファイルに保存 __fr ### .xml__します。 簡単に記録された情報を表示する、XSL スタイル シートという名前の __freb.xsl__ は XML ファイルと同じディレクトリに含まれています。 Internet Explorer でいずれかの XML ファイルを開くと、トレース情報が XSL スタイルシートを使用して書式設定されて表示されます。 たとえば、次のように表示されます。

![失敗した要求をブラウザーで表示したところ](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### 詳細なエラー ログ

詳細なエラー ログは、発生した HTTP エラーに関する詳細な情報を提供する HTML ドキュメントです。 単なる HTML ドキュメントであるため、Web ブラウザーを使用して表示できます。

### Web サーバーのログ

使用して web サーバーのログが書式設定、 [W3C 拡張ログ ファイル形式](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx)します。 この情報は、テキスト エディターを使用して読み取ることができるなどのユーティリティを使用してで解析したり [Log Parser](http://go.microsoft.com/fwlink/?LinkId=246619)します。

> [AZURE.NOTE] Azure web apps によって生成されるログをサポートしていない、 __では、s-computername__, 、__s ip__, 、または __cs-version の__ フィールドです。

##<a name="nextsteps"></a> 次のステップ

- [Web アプリを監視する方法](/manage/services/web-sites/how-to-monitor-websites/)
- [Visual Studio での Azure の Web Apps のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)
- [HDInsight での Web アプリケーション ログの分析](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [プレビュー ポータル内の移動に関するリファレンス。](http://go.microsoft.com/fwlink/?LinkId=529715)
 

