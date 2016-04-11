<properties
   pageTitle="Application Insights を使用したクラウド サービスのトラブルシューティング | Microsoft Azure"
   description="Application Insights を使用して Azure 診断データを処理することで、クラウド サービスの問題をトラブルシューティングする方法について説明します。"
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager=""
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# Application Insights を使用したクラウド サービスのトラブルシューティング

 [Azure SDK 2.8](https://azure.microsoft.com/downloads/) と Azure の診断拡張機能 1.5 Application Insights に直接、クラウド サービスの Azure 診断データを送信できますようになりました。 アプリケーション ログ、Windows イベント ログ、ETW ログ、パフォーマンス カウンターなど、Azure 診断によって収集されるさまざまな種類のログを Application Insights に送信し、Application Insights ポータル UI で視覚化できます。 Application Insights SDK と共に使用すると、Azure 診断のシステムやインフラストラクチャ レベルのデータだけでなくアプリケーションから取得したメトリックとログの洞察を得ることができます。
  
## Application Insights にデータを送信するための Azure 診断の構成

Azure 診断データを Application Insights に送信するには、次の手順に従ってクラウド サービス プロジェクトをセットアップします。

1) Visual Studio のソリューション エクスプ ローラーでロールを右クリックし、選択 **プロパティ** ロール デザイナーを開く
    
![Solution Explorer Role Properties][1]

2) セクションで、診断の下でのロール デザイナー内にあるチェック ボックスを選択して **診断データを Application Insights に送信します。**

![ロール デザイナーによる Application Insights への診断データの送信][2]

3) ポップアップ表示されるダイアログ ボックスでは、Azure 診断データを送信する Application Insights のリソースを選択します。 このダイアログ ボックスでは、サブスクリプションから既存の Application Insights リソースを選択するか、Application Insights リソースのインストルメンテーション キーを手動で指定することができます。 既存の Application Insights のリソースがないかどうかは] をクリックして作成することができます、 **リソースを新規作成** リンクに Application Insights のリソースを作成する Azure クラシック ポータルをブラウザー ウィンドウを開きます。 Application Insights リソースの作成の詳細については「 [Application Insights リソースを新規作成する](app-insights-create-new-resource.md)

![Application Insights リソースの選択][3]

4) 名前のサービスの構成設定として、そのリソースのインストルメンテーション キーが格納されている Application Insights リソースを追加した後 **APPINSIGHTS_INSTRUMENTATIONKEY**します。 サービス構成ごとまたは環境ごとにこの構成設定を変更するには、[サービス構成] ボックスから別の構成を選択し、その構成の新しいインストルメンテーション キーを指定します。

![サービス構成の選択][4]
    
 **APPINSIGHTS_INSTRUMENTATIONKEY** 構成設定が発行時に適切な Application Insights のリソース情報で診断拡張機能を構成する Visual Studio によって使用されます。 この構成設定は、さまざまなサービス構成に異なるインストルメンテーション キーを定義するときに便利な方法です。 この設定は、発行時に Visual Studio によって変換され、診断の拡張機能のパブリック構成に挿入されます。 PowerShell を使用して診断の拡張機能を構成するプロセスを単純化するために、Visual Studio から出力されたパッケージには、該当する Application Insights インストルメンテーション キーを格納したパブリック構成 XML も含まれます。 パブリック構成ファイルでは、拡張機能フォルダーに作成され、PaaSDiagnostics パターンに従います。<RoleName>.いずれかのデプロイします。 このファイルを PowerShell ベースのデプロイで使用し、各構成をロールにマップすることができます。

5) 有効にすると、 **診断データを Application Insights に送信** は自動的にすべてのパフォーマンス カウンターと Application Insights に Azure 診断エージェントによって収集されるエラー レベルのログを送信する Azure 診断を構成します。 Application Insights に送信されるデータをさらに構成するかどうかは、手動で編集する必要があります、 *diagnostics.wadcfgx* 各ロール用のファイルです。 参照してください [データを Application Insights に送信するための Azure 診断の構成](azure-diagnostics-configure-applicationinsights.md) を手動で構成を更新する方法の詳細を参照してください。 

Azure 診断データを Application Insights に送信するようにクラウド サービスを構成したら、通常と同じように Azure にデプロイし、Azure 診断の拡張機能が有効なことを確認できます。 参照してください [Visual Studio を使用してクラウド サービスの発行](vs-azure-tools-publishing-a-cloud-service.md)します。  

## Application Insights での Azure 診断データの表示
Azure 診断テレメトリは、クラウド サービス用に構成された Application Insights リソースに表示されます。

Azure 診断のさまざまなログの種類と Application Insights の概念は次のように対応しています。  

-  パフォーマンス カウンターは、Application Insights でカスタム メトリックとして表示されます。
-  Windows イベント ログは、Application Insights でトレースとカスタム イベントとして表示されます。
-  アプリケーション ログ、ETW ログ、診断インフラストラクチャ ログは、Application Insights でトレースとして表示されます。

Application Insights で Azure 診断データを表示するには:

- 使用 [メトリックス エクスプ ローラー](../application-insights/app-insights-metrics-explorer.md) を任意のカスタム パフォーマンス カウンターまたは異なる種類の windows イベント ログのイベントの数を視覚化します。

![メトリックス エクスプローラーのカスタム メトリックス][5]

- 使用 [検索](../application-insights/app-insights-diagnostic-search.md) Azure Diagnostics によって送信された、さまざまなトレース ログを検索します。 表示、ロールがクラッシュし、その情報を再利用を原因となっているロールのハンドルされない例外がある場合などに、 *アプリケーション* のチャネル *Windows イベント ログ*します。 検索機能を使用して、Windows イベント ログのエラーを確認し、例外のスタック トレース全体を取得することで、問題の根本原因を発見できます。 

![トレースの選択][6]

## 次のステップ

- [クラウド サービスに Application Insights SDK を追加](../application-insights/app-insights-cloudservices.md) 、アプリケーションからの要求、例外、依存関係、およびカスタム テレメトリに関するデータを送信します。 Azure 診断データと組み合わせることで、同じ Application Insight リソース内のすべてのアプリケーションとシステムを包括的に確認できます。  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png

