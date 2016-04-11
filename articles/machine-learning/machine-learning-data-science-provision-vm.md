<properties 
    pageTitle="Microsoft データ サイエンス仮想マシンのプロビジョニング | Microsoft Azure" 
    description="分析と機械学習を行うために、Azure でデータ サイエンス用仮想マシンを構成および作成します。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/26/2015" 
    ms.author="bradsev" />


# Microsoft データ サイエンス仮想マシンのプロビジョニング

## はじめに

Microsoft データ サイエンス用仮想マシンは、プレインストールの Azure 仮想マシン (VM) イメージです。データ分析と機械学習用に一般的に使用されているいくつかのツールで構成されています。 含まれているツールは、次のとおりです。

- Revolution R Open
- Anaconda Python ディストリビューション
- Visual Studio Community エディション
- Power BI Desktop
- SQL Server Express エディション
- Azure SDK


データ サイエンスでは、一連のタスクの反復処理を行います。たとえば、データの検索、読み込み、および前処理、モデルの構築とテスト、インテリジェント アプリケーションで使用するためのモデルのデプロイなどです。 これらのタスクを実行するために、さまざまなツールを使用するデータ科学者も珍しくありません。 適切なバージョンのソフトウェアを見つけ、ダウンロードしてインストールするには、非常に時間がかかる場合があります。 Microsoft データ サイエンス仮想マシンはその問題を解決します。 

Microsoft データ サイエンス仮想マシンは分析プロジェクトの活性剤となります。 これにより、R、Python、SQL、および C# を含むさまざまな言語でタスクを処理できます。 Visual Studio には、コードの開発やテストを行うための使いやすい IDE が用意されています。 VM に含まれている Azure SDK を使用すると、Microsoft のクラウド プラットフォームにあるさまざまなサービスを利用してアプリケーションを構築できます。 

このデータ サイエンス VM イメージにソフトウェア課金はありません。 Azure に対する利用料金は、この VM イメージにプロビジョニングする仮想マシンのサイズに依存するに対してのみ課金されます。 コンピューティングの料金の詳細についてはこちらの (https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) で見つかんだことができます。 


## 前提条件

Microsoft データ サイエンス仮想マシンを作成する前に、次を用意する必要があります。

- **Azure サブスクリプション**: いずれかを取得するを参照してください。 [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

*   **Azure ストレージ アカウント**: 1 つを作成するを参照してください。 [Azure ストレージ アカウントの作成](storage-create-storage-account.md#create-a-storage-account) ストレージ アカウントを作成して、既存のアカウントを使用したくない場合、VM を作成するプロセスの一部として別の方法として、します。


## Microsoft データ サイエンス仮想マシンの作成

Microsoft データ サイエンス仮想マシンのインスタンスを作成する手順を以下に示します。

1.  一覧表示する仮想マシンに移動 [Azure Classic Portal](https://portal.azure.com/#gallery/microsoft-ads.standard-data-science-vmstandard-data-science-vm)します。
2.   をクリックして、 **作成** 下部には、ウィザードにボタンをクリックします![。構成科学データ-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   次のセクションでは、提供、 **入力** ごとに、 **5 つの手順** (上の図の右にある列挙型) Microsoft のデータ サイエンス仮想マシンを作成するために使用するウィザードでします。 以下は、これらの各ステップを構成するために必要な入力項目です。

  **a.基本**: 

   - **名前**: を作成して、データ サイエンスのサーバーの名前。
   - **ユーザー名**: 管理者アカウントのログイン id
   - **パスワード**: 管理者アカウント パスワード。
   - **サブスクリプション**: 複数のサブスクリプションがある場合は、マシンを作成または請求されるを 1 つを選択
   - **リソース グループ**: 新しいを作成するか、既存のグループを使用して
   - **場所**: 最適なデータ センターを選択します。 通常は、ネットワーク アクセスを最速にするために、データの大部分があるか、物理的に最も近いデータ センターを選びます。

  **b.サイズ**: 

   - 機能の要件とコストの制約を満たしている、いずれかのサーバーの種類を選択します。 [すべて表示] を選択すると、他の VM のサイズも表示されます。

  **c. [設定]**

   - **ディスクの種類**: 他のソリッド ステート ドライブ (SSD)、[標準] を選択したい場合は、Premium を選択します。
   - **ストレージ アカウント**: サブスクリプションで新しい Azure ストレージ アカウントを作成または既存のものと同じでを使用することができます *場所* ウィザードの [基本] 手順で選択します。
   - **その他のパラメーター**: ほとんどの場合、既定値を使用する、だけです。 既定以外の値の使用を検討する場合は、情報リンクにポインターを合わせ、該当するフィールドのヘルプを表示できます。

  **d.概要**: 

   - 入力したすべての情報が正しいことを確認します。

  **e.購入**: 

   - をクリックして **購入** 、プロビジョニングを開始します。 取引条件へのリンクが用意されています。 VM で選択したサーバーのサイズのコンピューティングを超える追加料金を請求はありません、 **サイズ** 手順です。 


プロビジョニングには、10 ～ 20 分くらいかかります。 プロビジョニングの状態は、Azure クラシック ポータルに表示されます。

## Microsoft データ サイエンス仮想マシンにアクセスする方法

VM を作成した後は、リモート デスクトップを使用して、手順 4. の [基本] セクションで作成した管理者アカウントの資格情報でログインできます。 

VM が作成され、プロビジョニングされた後は、VM にインストールされて構成されたツールを使い始めることができます。 多くのツールには、デスクトップ アイコンとスタート メニュー タイルがあります。 

## Microsoft データ サイエンス仮想マシンにインストールされているツール

### R
分析に R を使用する場合のために、VM には Revolution R Open (RRO) がインストールされています。 これは、R のオープン ソース ディストリビューションであり、CRAN-R との完全な互換性があります。 最新のオープン ソース R エンジンと、Intel Math Kernel Library が含まれています。 VM には、"RRO RGui" と呼ばれる IDE もパッケージ化されています。 自由にダウンロードしても次のように他の Ide を使用する [RStudio](http://www.rstudio.com)します。 

### Python
Python を使用して開発するために、Anaconda Python ディストリビューション 2.7 がインストールされています。 このディストリビューションには、基本 Python と、約 300 の最も一般的な数学、エンジニアリング、およびデータ分析パッケージが含まれています。 Visual Studio 2015 Community 版でインストールされている、または IDLE や Spyder など、Anaconda でバンドルされている IDE の 1 つである Python Tools for Visual Studio (PTVS) を利用できます。 検索バーで検索することにより次のいずれかを起動することができます (**Win** + **S** キー)。 

### IPython Notebook
Anaconda ディストリビューションは、コードと分析を共有するための環境である IPython Notebook にも付属しています。 IPython Notebook サーバーは、あらかじめ構成されています。 Notebook サーバーにアクセスするためのブラウザーを起動するデスクトップ アイコンがあります。 リモート デスクトップ経由で VM を使用している場合は、アクセスすることも [https://localhost:9999/](https://localhost:9999/) IPython notebook サーバーにアクセスする (注: 続行して、証明書の警告が表示されます)。 

### Visual Studio 2015 Community エディション
Visual Studio Community エディションが VM にインストールされています。 これは、Microsoft の人気のある IDE の無料版で、評価用または非常に小規模なチーム用に使用できます。 ライセンス契約の条項をチェック_アウトできます [ここ](https://www.visualstudio.com/support/legal/mt171547)します。  デスクトップ アイコンをダブルクリックして Visual Studio を開き、または **開始** メニュー。 プログラムで検索することもできます。 **Win** + **S** を Visual Studio」を入力するとします。 

注: 評価期間が終了したことを示すメッセージが表示される場合があります。 Visual Studio Community Edition にアクセスするには、Microsoft アカウント資格情報を入力するか、Microsoft アカウント資格情報を作成して、それを入力します。 C#、Python などの言語でプロジェクトを作成できます。

### SQL Server Express
SQL Server の制限付きバージョンも、Visual Studio Community エディションにパッケージ化されています。 起動して SQL サーバーにアクセスできる **SQL Server Management Studio**します。 VM の名前は、サーバー名として設定されます。 Windows の管理者としてログインする場合は、Windows 認証を使用します。 SQL Server Management Studio にアクセスできたら、他のユーザーの作成、データベースの作成、データのインポート、SQL クエリの実行などを行うことができます。 

### Azure 
いくつかの Azure ツールは、VM にインストールされます。
- Azure SDK ドキュメントにアクセスするデスクトップのショートカットがあります。 
- **AzCopy** 、Microsoft Azure ストレージ アカウントとの間のデータを移動するために使用します。 
- **Azure ストレージ エクスプ ローラー** 、Azure ストレージ アカウント内に保存されているオブジェクトを参照するために使用します。 
- **Microsoft Azure Powershell** にも、スクリプト言語で Azure のリソースを管理するために使用するスクリプト言語は、VM にインストールします。 

###Power BI

ダッシュ ボードと優れた視覚エフェクトを構築するため、 **Power BI Desktop** がインストールされています。 このツールを使用すると、他のソースからデータを抽出し、ダッシュボードとレポートを作成し、それらをクラウドに公開することができます。 詳細については、次を参照してください。、 [Power BI](http://powerbi.microsoft.com) サイトです。 

注: Power BI にアクセスするには、Office 365 アカウントが必要です。 

## その他の Microsoft 開発ツール
 [**Microsoft Web Platform Installer**](https://www.microsoft.com/web/downloads/platform.aspx) を検出し、その他のマイクロソフトの開発ツールをダウンロードするために使用できます。 Microsoft データ サイエンス仮想マシンのデスクトップにはツールのショートカットもあります。  





