<properties
    pageTitle="Azure 診断ログを Application Insights に送信する"
    description="Application Insights ポータルに送信される Azure Cloud Services診断ログの詳細を構成します。"
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/17/2015"
    ms.author="awills"/>


# Application Insights に対する Azure 診断ログの構成

クラウド サービス プロジェクト、または Microsoft Azure の仮想マシンを設定すると [Azure 診断のログを生成できます](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)します。 これを Application Insights に送信し、Application Insights SDK によってアプリ内から送信された診断および使用テレメトリと共に分析できます。 Azure のログには、開始、停止、クラッシュ、パフォーマンス カウンターなど、アプリの管理でのイベントが含まれます。 また、ログにはアプリでの System.Diagnostics.Trace の呼び出しも含まれます。

この記事では、診断キャプチャの構成について詳しく説明します。

Azure SDK 2.8 を Visual Studio にインストールする必要があります。

## Application Insights リソースを取得する

最良の結果 [Application Insights SDK をクラウド サービス アプリの各ロールに追加](app-insights-cloudservices.md), 、または [どのようなアプリには、仮想マシンで実行](app-insights-get-started.md)します。 分析する診断データを送信することができ、同じ Application Insights リソースを表示します。

または、- SDK を使用しない場合など、アプリが既に実行中の場合すれば [Application Insights リソースを新規作成する](app-insights-create-new-resource.md) Azure ポータルで。 アプリケーションの種類として **Azure 診断**を選択します。


## Azure 診断を Application Insights に送信する

アプリ プロジェクトを更新できる場合は、Visual Studio で各ロールを選択し、[プロパティ] を選択して、[構成] タブで **[診断を Application Insights に送信する]** をオンにします。

アプリがまだ活動中の場合は、Visual Studio のサーバー エクスプローラーまたは Cloud Services エクスプローラーを使用して、アプリのプロパティを開きます。 **[診断を Application Insights に送信する]** をオンにします。

いずれの場合も、作成した Application Insights リソースの詳細を求められます。

[クラウド サービス アプリの Application Insights の設定に関する詳細については](app-insights-cloudservices.md)します。

## Azure 診断アダプターを構成する

ここでは、Application Insights に送信するログの部分を選択する方法を説明します。 既定では、Microsoft Azure イベント、パフォーマンス カウンター、アプリから System.Diagnostics.Trace のトレース呼び出しなど、すべてのデータが送信されます。

Azure 診断のデータは、Azure Storage のテーブルに格納されます。 ただし、Azure 診断の拡張機能 1.5 以降を使用する場合、その構成に "シンク" と "チャネル" を構成することで、すべてのデータまたはデータのサブセットを Application Insights にパイプすることもできます。

### シンクとして Application Insights を構成する

Azure SDK (2.8 またはそれ以降) を追加、役割のプロパティを使用すると、「Application Insights にデータを送信する」を設定するときに、 `< SinksConfig >` 公開要素 [Azure 診断構成ファイル](https://msdn.microsoft.com/library/azure/dn782207.aspx) ロールのです。

`< SinksConfig >` Azure 診断データを送信できる追加のシンクを定義します。例 `SinksConfig` 次のようにします。

```xml

    <SinksConfig>
     <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
     </Sink>
    </SinksConfig> 
```

`ApplicationInsights` 要素は、Azure 診断データを送信する Application Insights リソースを識別するインストルメンテーション キーを指定します。 リソースを選択するとに基づいて、自動的に設定されます、 `APPINSIGHTS_INSTRUMENTATIONKEY` サービスの構成。 (手動で設定する場合は、リソースの [Essentials] ドロップダウンからキーを取得します)。

`チャネル` シンクに送信されるデータを定義します。 チャネルはフィルターのように動作します。  `Loglevel` 属性では、チャネルが送信するログ レベルを指定することができます。 使用可能な値: `{詳細、情報、警告、エラー、"重大"}`します。

### データをシンクに送信する

DiagnosticMonitorConfiguration ノードにシンク属性を追加することで、Application Insights シンクにデータを送信します。 シンク要素を各ノードに追加すると、そのノードとその下にあるすべてのノードから収集したデータが指定したシンクに送信されるように指定されます。

たとえば、Azure SDK によって作成される既定値では、すべての Azure 診断データが送信されます。

```xml

    <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```

ただし、エラー ログのみを送信する場合は、チャネル名でシンク名を修飾します。

```xml

    <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```

定義したシンクの名前と共に上で定義したチャネル名を使用していることに注意してください。

かどうかは、Application Insights に詳細なアプリケーションのログを送信するだけだったとしてシンク属性を追加して、 `ログ` ノードです。

```xml

    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```

構成の階層内のさまざまなレベルに複数のシンクを含めることもできます。 その場合、階層の最上位に指定されたシンクはグローバルな設定として機能し、個々の要素のレベルに指定されたシンクはそのグローバル設定よりも優先されます。

Application Insights にすべてのエラー (`DiagnosticMonitorConfiguration` ノードで指定) とさらにアプリケーション ログの "詳細" レベルのログ (`Logs` ノードで指定) を送信するパブリック構成ファイルの例の全体を次に示します。

```xml

    <WadCfg>
     <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> 
      <DiagnosticInfrastructureLogs />
      <PerformanceCounters>
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="ApplicationInsights.MyLogData/>
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
        <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
      </PerformanceCounters>
      <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Application!*" />
      </WindowsEventLog>
      <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> 
       
     </DiagnosticMonitorConfiguration>

     <SinksConfig>
      <Sink name="ApplicationInsights">
        <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
        <Channels>
          <Channel logLevel="Error" name="MyTopDiagData"  />
          <Channel logLevel="Verbose" name="MyLogData"  />
        </Channels>
      </Sink>
     </SinksConfig>
    </WadCfg>
```

![](./media/app-insights-azure-diagnostics/diagnostics-publicconfig.png)

この機能には制限事項がいくつかあります。

* チャネルは、ログの種類を操作することのみを目的としています。パフォーマンス カウンターは操作できません。 パフォーマンス カウンターの要素を含むチャネルを指定した場合、そのチャネルは無視されます。
* チャネルのログ レベルには、Azure 診断によって収集されるデータのログ レベルを超えるログを指定することはできません。 たとえば、Logs 要素でアプリケーション ログのエラーを収集できないため、Application Insight シンクに "詳細" ログを送信することにします。 scheduledTransferLogLevelFilter 属性では、シンクに送信するログと同じかそれを超えるレベルのログを常に収集する必要があります。
* Azure 診断の拡張機能によって収集される BLOB データは Application Insights に送信できません。 たとえば、Directories ノードの下で指定されたデータです。 クラッシュ ダンプの場合、実際のクラッシュ ダンプは Blob Storage に送信され、Application Insights にはクラッシュ ダンプが生成されたという通知のみが送信されます。

## 関連トピック

* [Application Insights での Azure クラウド サービスの監視](app-insights-cloudservices.md)
* [PowerShell を使用して、Azure 診断を Application Insights に送信するには](app-insights-powershell-azure-diagnostics.md)
* [Azure の診断構成ファイル](https://msdn.microsoft.com/library/azure/dn782207.aspx)







