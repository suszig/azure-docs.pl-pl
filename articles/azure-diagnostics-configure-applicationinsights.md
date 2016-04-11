<properties
   pageTitle="Application Insights にデータを送信するための Azure 診断の構成 | Microsoft Azure"
   description="Application Insights にデータを送信するように Azure 診断のパブリック構成を更新します。"
   services="multiple"
   documentationCenter=".net"
   authors="sbtron"
   manager=""
   editor="" />
<tags
   ms.service="application-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />

# Application Insights にデータを送信するための Azure 診断の構成

Azure 診断のデータは、Azure Storage のテーブルに格納されます。  ただし、Azure 診断の拡張機能 1.5 以降を使用する場合、その構成に "シンク" と "チャネル" を構成することで、すべてのデータまたはデータのサブセットを Application Insights にパイプすることもできます。 

この記事では、Application Insights にデータを送信するように構成された Azure 診断の拡張機能のパブリック構成を作成する方法を説明します。

## シンクとしての Application Insights の構成

1.5 Azure 診断の拡張機能が導入されています、 **<SinksConfig>** パブリック構成内の要素。 これは、追加定義 *シンク* Azure 診断データを送信できます。 その一環として、Azure 診断データを送信する Application Insights リソースの詳細を指定する **<SinksConfig>**します。
例 **SinksConfig** 次のようになります  

    <SinksConfig>
        <Sink name="ApplicationInsights">
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
          <Channels>
            <Channel logLevel="Error" name="MyTopDiagData"  />
            <Channel logLevel="Verbose" name="MyLogData"  />
          </Channels>
        </Sink>
      </SinksConfig> 

 **シンク** 要素、 *名前* 属性が、シンクを一意に参照するために使用する文字列値を指定します。
 **ApplicationInsights** 要素は、Azure 診断データを送信する Application insights リソースのインストルメンテーション キーを指定します。 既存の Application Insights リソースを取得していない場合は、次を参照してください。 [Application Insights リソースを新規作成する](app-insights-create-new-resource.md) 詳細については、リソースを作成すると、インストルメンテーション キーを取得します。

このインストルメンテーション キーを自動的に作成に基づいて、パブリック構成に Azure SDK 2.8 でクラウド サービス プロジェクトを開発している場合、 **APPINSIGHTS_INSTRUMENTATIONKEY** サービス構成設定をクラウド サービス プロジェクトにパッケージ化します。 参照してください [クラウド サービスの問題をトラブルシューティングする Azure 診断で使用する Application Insights](cloud-services-dotnet-diagnostics-applicationinsights.md)します。

 **チャネル** 要素では、1 つまたは複数を定義できます。 **チャネル** シンクに送信されるデータの要素。 チャネルはフィルターのように機能し、シンクに送信する特定のログ レベルを選択するために使用できます。 たとえば、詳細ログを収集してストレージに送信できますが、ログ レベルが Error のチャネルを定義して、そのチャネル経由でログを送信した場合はエラー ログのみがそのシンクに送信されるようにできます。
 **チャネル** 、 *名前* 属性を使用して、そのチャネルを一意に参照してください。 
 *Loglevel* 属性では、チャネルによって、ログ レベルを指定することができます。 情報が最も少ないの順序で利用可能なログ レベルは、します。
 - 詳細
 - 情報
 - 警告
 - エラー
 - 重要です

## Application Insights のシンクへのデータの送信
Application Insights のシンクを定義した後データを送信できますそのシンクに追加することで、 *シンク* 属性にある要素を **DiagnosticMonitorConfiguration** ノードです。 追加する、 *シンク* 要素の各ノードには、そのノードと、指定したシンクに送信されるその下にあるすべてのノードから収集されたデータをすることを指定します。 

追加できるように、Azure 診断によって収集されているすべてのデータを送信する場合など、 *シンク* 属性に直接、 **DiagnosticMonitorConfiguration** ノードです。 値を設定、 *シンク* で指定したシンクの名前に、 **SinkConfig**します。

    <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">

唯一のエラー ログで、Application Insights を設定することができますしのシンクを送信する必要がある場合、 *シンク* に続けて、ピリオドで区切られたチャネル名シンクの名前を指定する値 ("です。") です。 たとえば、Application Insights のシンクにエラー ログのみを送信するには、上の SinksConfig で定義した MyTopDiagdata チャネルを使用します。  
 
    <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">

のみを Application Insights に詳細なアプリケーションのログを送信したいかどうかは、追加する、 *シンク* 属性を **ログ** ノードです。 
    
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>

構成の階層内のさまざまなレベルに複数のシンクを含めることもできます。 その場合、階層の最上位に指定されたシンクはグローバルな設定として機能し、個々の要素のレベルに指定されたシンクはそのグローバル設定よりも優先されます。    

Application Insights にすべてのエラーを送信する、パブリック構成ファイルの完全な例を次に示します (時に指定された、 **DiagnosticMonitorConfiguration** ノード) とアプリケーション ログについてさらに詳細なレベルのログ (時に指定された、 **ログ** ノード)。 

    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
           sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent to this channel -->
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
                sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent to this channel -->
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

![Diagnostics Public Configuration](./media/azure-diagnostics-configure-applicationinsights/diagnostics-publicconfig.png)

この機能には制限事項がいくつかあります。

- チャネルは、ログの種類を操作することのみを目的としています。パフォーマンス カウンターは操作できません。 パフォーマンス カウンターの要素を含むチャネルを指定した場合、そのチャネルは無視されます。 
- チャネルのログ レベルには、Azure 診断によって収集されるデータのログ レベルを超えるログを指定することはできません。 たとえば、Logs 要素でアプリケーション ログのエラーを収集できないため、Application Insight シンクに "詳細" ログを送信することにします。  *ScheduledTransferLogLevelFilter* 属性が同じで常に収集する必要がありますか、シンクに送信しようとしているログよりも多くのログ。 
- Azure 診断の拡張機能によって収集される BLOB データは Application Insights に送信できません。 指定した内容は次に例を *ディレクトリ* ノードです。 クラッシュ ダンプの場合、実際のクラッシュ ダンプは Blob Storage に送信され、Application Insights にはクラッシュ ダンプが生成されたという通知のみが送信されます。 


## 次のステップ

- 使用 [PowerShell](cloud-services-diagnostics-powershell.md) 、アプリケーションの Azure 診断拡張機能を有効にします。 
- 使用 [Visual Studio](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) 、アプリケーションの Azure 診断拡張機能を有効にするには 
