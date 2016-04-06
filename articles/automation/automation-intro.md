<properties
    pageTitle="Azure Automation の概要 | Microsoft Azure"
    description="Azure Automation が提供する価値について説明し、よく寄せられる質問への回答を示して、Runbook と Azure Automation DSC の作成と使用を始められるようにします。"
    services="automation"
    documentationCenter=""
    authors="SnehaGunda"
    manager="stevenka"
    editor=""/>

<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="11/05/2015"
    ms.author="bwren;sngun"/>

# Azure Automation の概要


ユーザーは Microsoft Azure Automation を使用すると、クラウド環境およびエンタープライズ環境で一般的に実行される、手動で実行時間が長く、エラーが起こりやすく、頻繁に繰り返されるタスクを自動化する手段を入手できます。 時間を節約し、普段の管理タスクの信頼性が向上し、一定の間隔で自動的に実行されるようにスケジュールを設定します。 Runbook を使用してプロセスを自動化したり、Desired State Configuration を使用して構成管理を自動化したりすることができます。 この記事では、Azure Automation の概要を説明し、よく寄せられる質問に回答します。 異なるトピックの詳細については、このライブラリの他の記事を参照してください。


## Runbook でのプロセスの自動化

Runbook は、Azure Automation で自動化されたプロセスを実行する一連のタスクです。 仮想マシンの開始やログ エントリの作成といった簡単な処理だけでなく、他の小さい Runbook を結合して複数のリソースまたは複数のクラウドやオンプレミスの環境を対象とした複雑な処理を実行する複雑な Runbook も作成できます。  

たとえば、SQL Database が最大サイズに近づいたときにサイズを減らす既存の手動プロセスがあり、それにはサーバーへの接続、データベースへの接続、データベースの現在のサイズの取得、しきい値を超えたかどうかの確認、データベースの縮小とユーザーへの通知などの複数の手順が含まれる場合があります。 手動でこれらの各手順を実行するの代わりに、1 つのプロセスとしてこれらすべてのタスクを実行する Runbook を作成できます。 Runbook を開始し、SQL サーバー名、データベース名、受信者の電子メール アドレスなどの必要な情報を提供した後は、プロセスが完了するまでユーザーが手を出す必要はありません。 


## Runbook が自動化できるものは何ですか。

Azure Automation での Runbook は、Windows PowerShell または Windows PowerShell Workflow に基づいているため、PowerShell で実行できることはすべて実行できます。 アプリケーションまたはサービスに API がある場合、Runbook はそれを操作できます。 アプリケーションに対する PowerShell モジュールがある場合は、そのモジュールを Azure Automation に読み込み、コマンドレットを Runbook に組み込むことができます。 Azure Automation の Runbook は、Azure クラウド内で実行するので、クラウド リソースや、クラウドからアクセスできる外部リソースにアクセスできます。 使用して [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), 、runbook はローカル リソースを管理するローカル データ センターで実行します。 


## コミュニティからの Runbook の取得

 [Runbook ギャラリー](automation-runbook-gallery.md#runbooks-in-runbook-gallery) マイクロソフトやコミュニティをそのまま使用するか、環境内から runbook を含まれているか、独自の目的用にカスタマイズできます。 独自の Runbook を作成する方法を知るための参考情報としても役に立ちます。 他のユーザーにも役に立つと思われる Runbook をギャラリーに提供して貢献することもできます。 


## Azure Automation での Runbook の作成 

実行できます [、独自の runbook を作成](automation-creating-importing-runbook.md) からスクラッチまたはから runbook を変更するには、 [Runbook ギャラリー](http://msdn.microsoft.com/library/azure/dn781422.aspx) 独自の要件です。 3 つ異なる [runbook 型](automation-runbook-types.md) 要件と PowerShell の経験に基づくからを選択できることです。 PowerShell コードを直接操作したいかどうかを使用して、 [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) または [PowerShell ワークフロー runbook](automation-runbook-types.md#powershell-workflow-runbooks) をオフラインまたは編集する、 [テキスト エディター](http://msdn.microsoft.com/library/azure/dn879137.aspx) Azure ポータルで。 基になるコードに見ずに runbook を編集したいかどうかは、作成することができます、 [グラフィカルな runbook](automation-runbook-types.md#graphical-runbooks) を使用して、 [グラフィカル エディター](automation-graphical-authoring-intro.md) Azure プレビュー ポータルで。 

読むより見る方がよければ、 2015 年 5 月に行われた Microsoft Ignite セッションのビデオをご覧ください。 注: このビデオで説明されている概念と機能は正しいのですが、このビデオが録画されたころよりも Azure Automation は大幅に改善され、現在では Azure ポータルの UI が増え、サポートされる機能も増えました。

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## Desired State Configuration での構成管理の自動化 

[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) 管理プラットフォームにより、管理、展開、および物理ホストと宣言型 PowerShell 構文を使用してバーチャル マシンの構成を適用することです。 ターゲット コンピューターが自動的に取得して適用できる構成を、1 つの DSC プル サーバーで一元的に定義できます。 DSC が提供する一連の PowerShell コマンドレットを使用して、構成とリソースを管理できます。  

[Azure Automation DSC](automation-dsc-overview.md) は、エンタープライズ環境に必要なサービスを提供する PowerShell DSC 向けのクラウド ベース ソリューションです。  DSC リソースを Azure Automation で管理し、Azure クラウド内の DSC プル サーバーから構成を取得する仮想マシンおよび物理コンピューターに構成を適用できます。  また、Azure Automation DSC が提供するレポート サービスは、ノードが割り当てられている構成から逸脱したときや、新しい構成が適用されたときなど、重要なイベントをユーザーに通知します。 


## Azure Automation での独自の DSC 構成の作成

[DSC 構成](automation-dsc-overview.md#azure-automation-dsc-terms) ノードの目的の状態を指定します。  複数のノードで同じ構成を適用し、すべてが同一の状態を維持できます。  ローカル コンピューターで任意のテキスト エディターを使用して構成を作成した後、Azure Automation にインポートしてコンパイルし、ノードに適用できます。


## モジュールと構成の取得 

すれば、 [PowerShell モジュール](automation-runbook-gallery.md#modules-in-powershell-gallery) runbook とから DSC 構成で使用できるコマンドレットが含まれている、 [PowerShell Gallery](http://www.powershellgallery.com/)します。 このギャラリーを Azure プレビュー ポータルから起動して、Azure Automation にモジュールを直接インポートできます。または、手動でダウンロードしてインポートすることもできます。 Azure ポータルからは、直接モジュールをインストールすることはできませんが、他のモジュールと同じように、ダウンロードしてインストールすることはできます。 


## Azure Automation の実際の適用例 

以下では、Azure Automation を使用して自動化できるさまざまなシナリオの例を示します。 

* 仮想マシンを作成し、異なる Azure サブスクリプションにコピーします。 
* ローカル コンピューターから Azure BLOB ストレージ コンテナーへのファイルのコピーをスケジュールします。 
* サービス拒否攻撃が検出されたときにクライアントからの要求を拒否するなど、セキュリティ機能を自動化します。 
* コンピューターが構成されているセキュリティ ポリシーに常に適合しているようにします。
* クラウドとオンプレミスのインフラストラクチャへのアプリケーション コードの継続的なデプロイメントを管理します。 
* ラボ環境用の Active Directory フォレストを Azure に作成します。 
* DB が最大サイズに近づいたら、SQL Database のテーブルを切り捨てます。 
* Azure Web サイトの環境設定をリモートから更新します。 


## Azure Automation と他のオートメーション ツールを関連付けるにはどうすればよいですか。

[Service Management Automation (SMA)](http://technet.microsoft.com/library/dn469260.aspx) の目的は、プライベート クラウドで管理タスクを自動化します。 コンポーネントとしてデータ センターにローカルにインストールされます [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/)します。 SMA と Azure Automation は、Windows PowerShell および Windows PowerShell ワークフローに基づく同じ runbook 形式を使用して、SMA はサポートしていませんが、 [グラフィカル runbook](automation-graphical-authoring-intro.md)します。  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) は内部設置型リソースの自動化の対象としています。 Azure Automation および Service Management Automation とは異なる形式の Runbook を使用し、スクリプトを作成する必要なしに Runbook を作成するためのグラフィカル インターフェイスを備えています。 その Runbook は、Orchestrator 用に特に作成された統合パックのアクティビティで構成されます。 


## 詳細な情報はどこで入手できますか。 

Azure Automation や独自の Runbook の作成方法の詳細を確認できる多数のリソースが用意されています。 

* **Azure Automation Library** is where you are right now. このライブラリの記事では、Azure Automation の構成や管理、また独自の Runbook の作成に関する完全なドキュメントが提供されます。 
* [Azure PowerShell コマンドレット](http://msdn.microsoft.com/library/jj156055.aspx) Windows PowerShell を使用した Azure の操作を自動化するための情報を提供します。 Runbook ではこれらのコマンドレットを使用して Azure リソースを操作します。 
* [管理に関するブログ](https://azure.microsoft.com/blog/tag/azure-automation/) Microsoft から Azure Automation と他の管理テクノロジの最新の情報を提供します。 このブログをサブスクライブすると、Azure Automation チームによる最新の情報を把握できます。 
* [Automation フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=390561) マイクロソフトや Automation コミュニティで対応する Azure Automation に関する質問を投稿することができます。 
* [Azure のオートメーション コマンドレット](https://msdn.microsoft.com/library/mt244122.aspx) 管理タスクを自動化するための情報を提供します。 Automation アカウント、資産、Runbook、DSC を管理するコマンドレットが含まれます。


## フィードバックを提供できますか。 

**フィードバックをお待ちしています。** Azure Automation の Runbook ソリューションや統合モジュールを探索している場合は、スクリプト センターにスクリプトの要求を投稿することができます。 Azure Automation に関するフィードバックや機能の要求がある場合に投稿して [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback)します。 よろしくお願いいたします。 




