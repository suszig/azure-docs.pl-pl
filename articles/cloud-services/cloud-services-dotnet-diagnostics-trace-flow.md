<properties
    pageTitle="Azure 診断で Cloud Services アプリケーションのフローをトレースする | Microsoft Azure"
    description="トレース メッセージを Azure アプリケーションに追加することにより、デバッグ、パフォーマンス測定、監視、トラフィック分析などを容易に行えるようになります。"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="robb"/>



# Azure 診断で Cloud Services アプリケーションのフローをトレースする

トレースは、アプリケーションの稼働中にアプリケーションの実行を監視する手段です。 使用することができます、 [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), 、[System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), 、および [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) エラーおよびアプリケーション ログ、テキスト ファイル、またはその他のデバイスを後で分析の実行に関する情報を記録するクラス。 トレースの詳細については、次を参照してください。 [トレースとアプリケーションのインストルメント化](https://msdn.microsoft.com/library/zs6s4h68.aspx)します。


## トレース ステートメントとトレース スイッチを使用する

追加することで、クラウド サービス アプリケーションでの実装のトレース、 [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) アプリケーションの構成と、アプリケーション コードで、System.Diagnostics.Trace または System.Diagnostics.Debug の呼び出しを行います。 構成ファイルを使用して *app.config* ワーカー ロールの場合、 *web.config* web ロール。 Visual Studio テンプレートを使用して、ホストされるサービスを新規に作成する場合は、Azure 診断が自動的にプロジェクトに追加され、さらに、追加したロール用の構成ファイルに DiagnosticMonitorTraceListener が追加されます。

トレース ステートメントを配置する方法の詳細については、次を参照してください。 [方法: アプリケーション コードにトレース ステートメントを追加](https://msdn.microsoft.com/library/zd83saa2.aspx)します。

配置することで [トレース スイッチ](https://msdn.microsoft.com/library/3at424ac.aspx) コードでは、トレースが発生したかどうかと、どのくらい広範を制御することができます。 これにより、運用環境でアプリケーションの状態を監視できます。 このことは、複数のコンピューターで実行される複数のコンポーネントを使用するビジネス アプリケーションでは特に重要です。 詳細については、次を参照してください。 [方法: トレース スイッチを設定する](https://msdn.microsoft.com/library/t06xyy08.aspx)です。

## Azure アプリケーションでトレース リスナーを構成する

Trace、Debug、TraceSource では、送信されるメッセージを収集および記録するように "リスナー" をセットアップする必要があります。 リスナーでは、トレース メッセージを収集、格納、およびルーティングします。 リスナーは、トレース出力を、ログ、ウィンドウ、テキスト ファイルなど、適切なターゲットに転送します。 Azure 診断を使用して、 [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) クラスです。

次の手順を完了する前に、Azure 診断モニターを初期化する必要があります。 これを行うには、次を参照してください。 [Microsoft Azure で診断を有効にすると](cloud-services-dotnet-diagnostics.md)です。

Visual Studio で提供されるテンプレートを使用すると、リスナーの構成が自動的に追加されるので注意してください。


### トレース リスナーを追加する

1. ロールに応じて web.config または app.config ファイルを開きます。
2. 次のコードを ファイルに追加します。

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=1.0.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Microsoft.WindowsAzure.Diagnostics アセンブリへのプロジェクト参照があることを確認します。 参照先の Microsoft.WindowsAzure.Diagnostics アセンブリのバージョンと一致するように上記の xml のバージョン番号を更新します。 
    
3. 構成ファイルを保存します。

リスナーの詳細については、次を参照してください。 [トレース リスナー](https://msdn.microsoft.com/library/4y5y10s7.aspx)します。

リスナーを追加する手順を完了すると、コードにトレース ステートメントを追加できます。


### コードにトレース ステートメントを追加するには

1. アプリケーション用のソース ファイルを開きます。 たとえば、 <RoleName>ワーカー ロールまたは web ロールの .cs ファイルです。
2. 次の追加が、まだ追加していない場合は、ステートメントを使用します。
    ```
        using System.Diagnostics;
    ```
3. Trace ステートメントを追加し、アプリケーションの状態に関する情報をキャプチャします。 Trace ステートメントの出力は、さまざまな方法で書式設定できます。 詳細については、次を参照してください。 [方法: アプリケーション コードにトレース ステートメントを追加](https://msdn.microsoft.com/library/zd83saa2.aspx)します。
4. ソース ファイルを保存します。

