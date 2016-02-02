<properties
   pageTitle="Cloud Cruiser と Microsoft Azure Billing API の統合 |Microsoft Azure"
   description="Microsoft Azure 課金パートナーの Cloud Cruiser が Azure Billing API を製品に統合した独自の事例について説明します。この記事は、Microsoft Azure Pack の使用/試用に関心を持っている Azure と Cloud Cruiser のユーザーには特に役立ちます。"
   services="billing"
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="11/02/2015"
   ms.author="mobandyo;sirishap;bryanla"/>


# Cloud Cruiser と Microsoft Azure Billing API の統合

この記事では、新しい Azure Billing API から収集した情報を Cloud Cruiser で使用して、ワークフローのコスト シミュレーションと分析を行う方法について説明します。

## Azure RateCard API

RateCard API は、Azure の料金情報を提供します。 適切な資格情報を認証した後は、API を照会し、Azure で使用できるサービスに関するメタデータや、自分のプラン ID に関連付けられた料金を収集できます。

次に、A0 (Windows) インスタンスの料金を表示する API の応答例を示します。

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0
    },

### Azure RateCard API に対する Cloud Cruiser のインターフェイス

Cloud Cruiser は、RateCard API をさまざまな方法で利用できます。 この記事では、IaaS ワークロード コストのシミュレーションと分析に使用する方法を説明します。

この使用事例を読むにあたって、Microsoft Azure Pack (WAP) でいくつかのインスタンスが実行されているワークロードを想定してください。 目標は、この同じワークロードを Azure でシミュレートし、移行した場合のコストを見積もることです。 このシミュレーションを作成するために、主に次の 2 つの作業を実行します。

1. **RateCard API から収集したサービス情報のインポートと処理** - この作業はブックでも実行されます。この場合、RateCard API からの抽出を変換し、新しい料金プランに発行します。 この新しい料金プランは、Azure の料金を見積もるシミュレーションに使用されます。

2. **正規化 WAP サービスや Azure IaaS サービス** -既定では、WAP サービスは、個々 のリソース (CPU、メモリ サイズ、ディスク サイズなど) に基づく、 Azure のサービスは、インスタンスのサイズ (A0、A1、A2 など) に基づいています。 この 1 つ目の作業は、Cloud Cruiser の ETL エンジン (ブックと呼ばれます) で実行できます。これらのリソースは、Azure インスタンス サービスと同様に、インスタンス サイズに基づいてまとめることができます。

### RateCard API からのデータのインポート

Cloud Cruiser のブックには、RateCard API の情報の収集と処理を自動化する機能があります。 ETL (抽出-変換-読み込み) ブックを使用して、データのコレクション、変換、および Cloud Cruiser データベースへの発行を構成できます。

各ブックには、1 つ以上のコレクションを含めることができます。 そのため、複数のソースの情報を関連付けて、使用状況データの補完と拡大を行うことができます。 次の 2 つのスクリーンショットでは、既存のブックに新しい*コレクション*を作成する方法と、RateCard API から情報を*コレクション*にインポートする方法を示しています。

![図 1 - 新しいコレクションの作成][1]

![図 2 - 新しいコレクションからのデータのインポート][2]

データをブックにインポートすると、複数のステップで変換プロセスを作成して、データを変更し、モデルを作成することができます。 この例では、サービスとしてのインフラストラクチャ (IaaS) にのみ関心があるので、変換ステップを使用して不要な行や、IaaS 以外のサービスに関連するレコードを削除することができます。

次のスクリーン ショットは、RateCard API から収集されたデータの処理に使用する変換ステップを示しています。

![図 3 - RateCard API から収集したデータを処理する変換ステップ][3]

### 新しいサービスと料金プランの定義

Cloud Cruiser には、サービスを定義する方法が複数あります。 選択肢の 1 つは、使用状況データからサービスをインポートすることです。 一般的に、この方法は、プロバイダーがサービスを定義済みであるパブリック クラウドを使用する場合にのみ使用されます。

料金プランは、有効期間、ユーザー グループなどのオプションに基づいて、さまざまなサービスに適用できる料金または価格のセットです。 Cloud Cruiser の料金プランを使用して、シミュレーションまたは "what-if" シナリオを作成し、サービスの変更がワークロードの総コストに与える影響を理解できます。

この例では、RateCard API からのサービス情報を使用して Cloud Cruiser に新しいサービスを定義します。 同様に、サービスに関連付けられた料金を使用して、Cloud Cruiser の新しい料金プランを作成できます。

変換プロセスが終わった後は、新しいステップを作成し、新しいサービスと料金として RateCard API からデータを発行することができます。

![図 4 - 新しい料金として RateCard API のデータを発行する][4]

### Azure サービスと料金

サービスと料金を発行したら、Cloud Cruiser の *[Services]* タブでインポートされたサービスの一覧を確認できます。

![図 5 - 新しいサービスの確認][5]

*[Rate Plans]* タブでは、“AzureSimulation” という新しい料金プランと、RateCard API からインポートされた料金を確認できます。

![図 6 - 新しい料金プランと関連付けられている料金の確認][6]

### WAP と Azure サービスの正規化

WAP の既定では、コンピューティング、メモリ、およびネットワーク リソースの使用に基づいて使用状況情報を提供します。 Cloud Cruiser では、これらのリソースの割り当てまたは測定使用量に直接基づいてサービスを定義できます。 たとえば、各時間の CPU 使用量の基本料金を設定したり、インスタンスに割り当てられたメモリの GB 数に対して課金したりすることができます。

この例では、WAP と Azure のコストを比較するために、WAP のリソース使用状況を収集してまとめる必要があります。その結果のデータは、Azure サービスに対応付けることができます。 この変換は、ブックで簡単に実装できます。

![図 7 - サービスを正規化する WAP データの変換][7]

ブックの最後のステップは、データを Cloud Cruiser データベースに発行する処理です。 このステップで、使用状況データはサービスにまとめられ (サービスは Azure サービスに対応付けられます)、既定の料金に関連付けられて料金が作成されます。

ブックが完成すると、Scheduler に新しい作業を追加し、ブックを実行する頻度と時間を指定することで、データの処理を自動化できます。

![図 8 - ブックのスケジュール設定][8]

### ワークロード コスト シミュレーション分析のレポート作成

使用状況を収集し、Cloud Cruiser データベースに読み込むと、Cloud Cruiser Insights モジュール (高度な分析レポート ツール) を利用して、目的のワークロード コスト シミュレーションを作成できます。

このシナリオを説明するために、次のレポートを作成しました。

![コストの比較][9]

上のグラフは、サービスを別のコスト比較を示し、WAP (濃い青色) と Azure (明るい青色) の間の特定のサービスごとのワークロードの実行料金を比較します。

下のグラフは同じデータですが、部門別に分けられ、WAP と Azure でワークロードを実行している各部門の料金を示しています。また、節約バー (緑色) は 2 つの差です。

## Azure Usage API

### はじめに

最近 Microsoft
プログラムを使用してプル サブスクライバーを許可する Azure Usage API が導入されました
使用状況データを使用して、その消費量を把握します。 これはクラウドの大きなニュースです。
使用可能な豊富なデータセットを利用できます cruiser 顧客
この API を使用。

Cloud Cruiser では、いくつかの方法での Usage API との統合を利用できます。 粒度
(使用状況情報を 1 時間ごと) とを通じて利用できるリソースのメタデータ情報
API は柔軟性の高いショウバックをサポートするために必要なデータセットを提供または
チャージ バック モデルです。


このチュートリアルでは
使用状況 API から Cloud Cruiser に役立つ方法の例では 1 つ
作成します。 Azure では、新しいリソース グループを作成して具体的には、
アカウント構造体のタグを関連付けるし、プロセスを説明し、
取得し、Cloud Cruiser にタグ情報を処理します。
 
最終的な目標があります。
次のようなレポートを作成し、コストを分析できるようにし、
タグによって設定されますアカウント構造に基づく消費します。

![図 10 - タグを使用した分析結果のレポート][10]

### Microsoft Azure のタグ

Azure Usage API を介して使用できるデータには、消費量の情報だけでなく、関連付けられているすべてのタグを含むリソースのメタデータも含まれています。 タグを使用すると、リソースを簡単に整理できますが、効果を上げるには、次のことを確認する必要があります。

- プロビジョニング時にタグがリソースに正しく適用されている
- タグが、ショーバック/チャージバック プロセスで適切に使用されて、組織のアカウント構造と利用状況を結び付けている

この両方の要件への適合は、特にプロビジョニング側や課金側で何らかの手動のプロセスを行っていると、難しくなる場合があります。 入力ミス、タグの誤りや不足は、顧客がタグの使用時に訴える共通した不満です。このようなエラーは、課金側の対応を非常に難しくする場合があります。

新しい Azure Usage API によって、Cloud Cruiser はリソースのタグ付け情報を取得できます。また、ブックと呼ばれる非常に高度な ETL ツールを使用することで、一般的なタグ付けエラーを修正できます。 変換手順の正規表現とデータを活用することを
Cloud Cruiser の相関関係は、誤ってタグが付けられたリソースを識別し、ギャップを入力し、コンシューマーとリソースの正しい関連付けを確認することは、正しいタグを適用することです。

充電の側では、
Cloud Cruiser ショウバック/チャージ バック プロセスを自動化し、活用できる、
タグ情報を適切なコンシューマー (部門、使用状況を妨害するには
部門、プロジェクトなど)。 この自動化は、大きな進歩を提供し、ことができます。
一貫性のある監査可能な充電プロセスを確認します。



### Microsoft Azure でのタグによるリソース グループの作成

このチュートリアルの最初の手順では、Azure ポータルで新しいリソース グループを作成し、リソースに関連付ける新しいタグを作成します。 この例では、ここでは作成します。
次のタグ: 部門、環境では、所有者、プロジェクトです。

以下の Azure ポータルのスクリーン ショットに、関連付けたタグを使用するサンプル リソース グループを示します。

![図 11 - Azure ポータルで、関連付けられたタグを設定したリソース グループ][11]

次の手順では、Usage API から Cloud Cruiser に情報を取得します。 現在、Usage API は、JSON 形式で応答を提供します。 取得されるデータのサンプルを次に示します。


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8
    
      },
    },

### Usage API から Cloud Cruiser へのデータのインポート

Cloud Cruiser のブックには、Usage API の情報の収集と処理を自動化する機能があります。 ETL (抽出-変換-読み込み) ブックでは、構成することができます、
コレクション、変換、および Cloud Cruiser データベースにデータを公開します。

各ブックには、1 つ以上のコレクションを含めることができます。 そのため、複数のソースの情報を関連付けて、使用状況データの補完と拡大を行うことができます。 この例では、Azure テンプレート ブックに新しいシート (_UsageAPI)_ を作成し、新しい_コレクション_を設定して Usage API から情報をインポートします。

![図 3 - UsageAPI シートにインポートする Usage API データ][12]

既にこのブックには、サービスを Azure からインポートするシート (_ImportServices_) と Billing API の消費量情報を処理するシート (_PublishData_) があることに注意してください。

_UsageAPI_ シートで Usage API から情報を抽出して処理し、_PublishData_ シートでその情報を Billing API の消費データと関連付けます。

### Usage API からのタグ情報の処理

ブックにデータをインポートした後は、API からの情報を処理するために、_UsageAPI_ シートで変換の手順を作成します。 まず、「JSON 分割」プロセッサを使用して (ように、API からインポートする)、1 つのフィールドからタグを抽出し、それらの 1 つの新しいフィールドを作成します (プロジェクト、部門の責任者と
環境の場合)。

![図 4 - タグ情報に対する新しいフィールドの作成][13]

"Networking" サービスにはタグ情報がありません (黄色のボックス) が、_ResourceGroupName_ フィールドを見れば、このサービスは同じリソース グループの一部であることがわかります。 この同じリソース グループの他のリソースにはタグがあるため、以降のプロセスでは、この情報を使用して、不足しているタグをこのリソースに適用できます。

次の手順では、タグの情報を _ResourceGroupName_ に関連付けるルックアップ テーブルを作成します。 このルックアップ テーブルは、次の手順でタグ情報を使用して、消費データを強化するために使用します。

### 消費データへのタグ情報の追加

ここからは、_PublishData_ シートを使用できます。このシートでは、Billing API の消費情報を処理して、タグから抽出されるフィールドを追加します。 このプロセスは、前の手順で作成したルックアップ テーブルを参照して実行を使用して、 _ResourceGroupName_
としてのルックアップ キーです。

![図 5 - 参照からの情報を使用したアカウント構造の設定][14]

"Networking" サービスの適切なアカウント構造のフィールドが適用されて、不足タグの問題が解決されたことに注意してください。 アカウントの構造体も設定
"Other"のレポートでは、区別するために、ターゲット リソース グループ以外のリソースのフィールドです。

ここで、利用状況データを公開するために、手順がもう 1 つ必要です。 この手順で、料金プランに定義されている各サービスに対して適切なレートを使用状況に適用されます。
情報、および結果として得られる料金が、データベースに読み込まれます。

この手順の最も大きなメリットは、このプロセスを 1 回だけ行えばよいという点です。 ブックが完了するスケジューラに追加するだけとされ、1 時間ごとに実行または毎日で、
スケジュールされた時刻。 新しいレポートを作成またはカスタマイズすることのみです。
視覚化し、意味のあるを取得するデータを分析するために、既存の
クラウドの使用量の分析。

### 次のステップ

+ Cloud Cruiser のブックとレポートの作成の詳細な手順についてを参照してください Cloud Cruiser のオンライン [ドキュメント](http://docs.cloudcruiser.com/) (有効なログインが必要です)。 Cloud Cruiser の詳細については、次にお問い合わせください [info@cloudcruiser.com](mailto:info@cloudcruiser.com)します。
+ 参照してください [Microsoft Azure のリソース消費を把握](billing-usage-rate-card-overview.md) の Azure Resource Usage api と RateCard Api の概要について説明します。
+ チェック アウト、 [Azure Billing REST API リファレンス](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) 両方の Api の詳細については、これは Azure リソース マネージャーによって提供される Api のセットの一部です。
+ すぐサンプル コードにある場合をチェック アウト、Microsoft Azure Billing API コード サンプルに [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?term=billing)します。

### 詳細情報

+ 参照してください、 [Azure リソース マネージャーの概要](resource-group-overview.md) Azure リソース マネージャーについての詳細については、記事です。



[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Figure 1 - Creating a new collection"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Figure 2 - Import data from the new collection"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Figure 3 - Transformation steps to process collected data from RateCard API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Figure 4 - Publishing the data from the RateCard API as new Services and Rates"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Figure 5 - Verifying the new Services"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Figure 6 - Verifying the new Rate Plan and associated rates"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Figure 7 - Transforming WAP data to normalize services"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Figure 8 - Workbook scheduling"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Figure 9 - Sample Report for the Workload cost comparison scenario"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Figure 10 - Report with breakdowns using tags"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Figure 11 - Resource Group with associated tags on Azure Portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Figure 12 - Usage API data imported into the UsageAPI sheet"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Figure 13 - Create new fields for the tag information"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Figure 14 - Populating the account structure with the information from the lookups"

