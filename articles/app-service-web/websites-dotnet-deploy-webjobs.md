<properties 
    pageTitle="Visual Studio を使用して Web ジョブを展開する" 
    description="Visual Studio を使用して Azure Web ジョブを Azure App Service の Web アプリケーションにデプロイする方法について説明します。" 
    services="app-service" 
    documentationCenter="" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="na" 
    ms.date="09/22/2015" 
    ms.author="tdykstra"/>

# Visual Studio を使用して Web ジョブを展開する

## 概要

Visual Studio を使用して、コンソール アプリケーション プロジェクトでの web アプリをデプロイする方法に関するトピック [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) として、 [Azure web ジョブの](http://go.microsoft.com/fwlink/?LinkId=390226)です。 使用して web ジョブを展開する方法については、 [Azure ポータル](https://portal.azure.com), を参照してください [web ジョブでバック グラウンドの実行タスク](web-sites-create-web-jobs.md)します。

Visual Studio では Web ジョブ対応のコンソール アプリケーション プロジェクトをデプロイする場合に、2 つのタスクを実行します。

* ランタイム ファイルを web アプリの適切なフォルダーにコピー (*App_Data/ジョブ/継続的な* 継続的な web ジョブの実行、 *App_Data/ジョブ/トリガー* スケジュールされたオンデマンド web ジョブ用)。
* 設定 [Azure Scheduler ジョブ](#scheduler) web ジョブを特定の時間に実行するようにスケジュールします。 (これは継続的な Web ジョブでは必要ありません)。

Web ジョブ対応のプロジェクトでは、次の項目が追加されています。

*  [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet パッケージ。
* A [web ジョブ発行 settings.json](#publishsettings) デプロイメントとスケジューラの設定を含むファイル。 

![Diagram showing what is added to a Console App to enable deployment as a WebJob](./media/websites-dotnet-deploy-webjobs/convert.png)

これらの項目を既存のコンソール アプリケーション プロジェクトに追加することもできれば、テンプレートを使用して新しい Web ジョブ対応のコンソール アプリケーション プロジェクトを作成することもできます。 

プロジェクト自体を Web ジョブとしてデプロイできます。また、Web プロジェクトをデプロイしたときに常に自動的にデプロイされるように、プロジェクトを Web プロジェクトにリンクすることもできます。 Visual Studio にはプロジェクトをリンクするには、web ジョブ対応のプロジェクトの名前が含まれています、 [webjobs-list.json](#webjobslist) web プロジェクト内のファイルです。

![Diagram showing WebJob project linking to web project](./media/websites-dotnet-deploy-webjobs/link.png)
 


## 前提条件

Web ジョブのデプロイメント機能は、Azure SDK Version 2.4 以降をインストールしている場合に、Visual Studio 2013 で使用できます。

* [Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409)します。

Web ジョブのデプロイメント機能にも含まれます。 [Visual Studio 2013 Update 3](http://go.microsoft.com/fwlink/?linkid=397827&clcid=0x409) 以降の更新プログラムです。

## <a id="convert"></a>既存のコンソール アプリケーション プロジェクトで Web ジョブのデプロイメントを有効にする

2 つのオプションがあります。

* [Web プロジェクトを使用した自動デプロイメントを有効にする](#convertlink)です。

    Web プロジェクトをデプロイしたときに自動的に Web ジョブとしてデプロイされるように、既存のコンソール アプリケーション プロジェクトを構成します。 Web ジョブを、関連する Web アプリケーションが実行する Web アプリケーションと同じアプリケーションで実行する場合に、このオプションを使用します。

* [Web プロジェクトなしでデプロイメントを有効にする](#convertnolink)です。

    Web プロジェクトへリンクせず、既存のコンソール アプリケーション プロジェクトがそれ自身で Web ジョブとしてデプロイされるように構成します。 Web アプリケーションで Web アプリケーションを実行せずに、Web ジョブ自身によって Web アプリケーション内で実行されようにする場合に、このオプションを使用します。 このオプションは、Web アプリケーション リソースとは関係なく Web ジョブ リソースを拡張する必要がある場合などに使用できます。

### <a id="convertlink"></a> Web プロジェクトを使用した Web ジョブの自動デプロイメントを有効にする
  
1. Web プロジェクトを右クリックして **ソリューション エクスプ ローラー**, 、クリックして **追加** > **Azure web ジョブとして既存のプロジェクト**します。

    ![Existing Project as Azure WebJob](./media/websites-dotnet-deploy-webjobs/eawj.png)
    
     [Azure web ジョブの追加](#configure) ] ダイアログ ボックスが表示されます。

1.  **プロジェクト名** ドロップダウン リストで、コンソール アプリケーションが web ジョブとして追加するプロジェクトを選択します。

    ![Selecting project in Add Azure WebJob dialog](./media/websites-dotnet-deploy-webjobs/aaw1.png)

2. 完了、 [Azure web ジョブの追加](#configure) クリックしてダイアログ ボックスで、 **OK**します。 

### <a id="convertnolink"></a> Web プロジェクトなしで Web ジョブ デプロイメントを有効にする
  
1. [コンソール アプリケーション プロジェクトを右クリックして **ソリューション エクスプ ローラー**, 、] をクリックし、 **[Azure web ジョブとして発行**します。 

    ![Publish as Azure WebJob](./media/websites-dotnet-deploy-webjobs/paw.png)
    
     [Azure web ジョブの追加](#configure) でプロジェクトを選択] ダイアログ ボックスが表示される、 **プロジェクト名** ボックス。

2.  完了、 [Azure web ジョブの追加](#configure) クリックしてダイアログ ボックスで、 **OK**します。

     **Web の発行** ウィザードが表示されます。  すぐに発行しない場合は、ウィザードを閉じます。 するときのため、入力した設定は保存 [プロジェクトを配置](#deploy)します。

## <a id="create"></a>新しい Web ジョブ対応のプロジェクトを作成する

新しい web ジョブ対応のプロジェクトを作成するコンソール アプリケーション プロジェクト テンプレートを使用して、説明したように web ジョブのデプロイメントを有効にする [前のセクション](#convert)です。 または、次のように Web ジョブの新しいプロジェクト テンプレートを使用できます。

* [独立した Web ジョブ用に Web ジョブの新しいプロジェクト テンプレートを使用する](#createnolink)

    プロジェクトを作成し、Web プロジェクトへはリンクせずに、それ自身で Web ジョブとしてデプロイされるように構成します。 Web アプリケーションで Web アプリケーションを実行せずに、Web ジョブ自身によって Web アプリケーション内で実行されようにする場合に、このオプションを使用します。 このオプションは、Web アプリケーション リソースとは関係なく Web ジョブ リソースを拡張する必要がある場合などに使用できます。

* [Web プロジェクトにリンクされた Web ジョブ用に Web ジョブの新しいプロジェクト テンプレートを使用する](#createlink)

    同じソリューション内の Web プロジェクトがデプロイされたときに Web ジョブとして自動的にデプロイされるように構成して、プロジェクトを作成します。 Web ジョブを、関連する Web アプリケーションが実行する Web アプリケーションと同じアプリケーションで実行する場合に、このオプションを使用します。

SDK 2.4 リリースでは、Web ジョブの新しいプロジェクト テンプレートは、コンソール アプリケーション プロジェクトを作成して Web ジョブのデプロイメントを有効にする方法と比較して、それほど簡単ではありません。 将来的には、web ジョブの新しいプロジェクト テンプレートをより使いやすく予定 [web ジョブ SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs) 開発、適切な web ジョブ SDK NuGet パッケージが自動的にインストールされます。 それまでの間で示すように手動でパッケージをインストールすることによって、web ジョブ SDK を使用するようプロジェクトを構成することができます、 [web ジョブ SDK のチュートリアル](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs)します。 


### <a id="createnolink"></a> 独立した Web ジョブ用に Web ジョブの新しいプロジェクト テンプレートを使用する
  
1. をクリックして **ファイル** > **新しいプロジェクト**, 、し、 **新しいプロジェクト** ] ダイアログ ボックスをクリックして **クラウド** > **Microsoft Azure web ジョブ**します。

    ![New Project dialog showing WebJob template](./media/websites-dotnet-deploy-webjobs/np.png)
    
2. 前に示された指示に従って [コンソール アプリケーションを独立した web ジョブ プロジェクトをプロジェクト](#convertnolink)します。

### <a id="createlink"></a> Web プロジェクトにリンクされた Web ジョブ用に Web ジョブの新しいプロジェクト テンプレートを使用する

1. Web プロジェクトを右クリックして **ソリューション エクスプ ローラー**, 、クリックして **追加** > **新しい Azure web ジョブ プロジェクト**します。

    ![New Azure WebJob Project menu entry](./media/websites-dotnet-deploy-webjobs/nawj.png)

     [Azure web ジョブの追加](#configure) ] ダイアログ ボックスが表示されます。

2. 完了、 [Azure web ジョブの追加](#configure) クリックしてダイアログ ボックスで、 **OK**します。

## <a id="configure"></a> [Azure Web ジョブの追加] ダイアログ ボックス

 **Azure web ジョブの追加** ] ダイアログ ボックスでは、web ジョブ名とスケジュールの web ジョブの設定を入力できます。 

![Add Azure WebJob dialog](./media/websites-dotnet-deploy-webjobs/aaw2.png)

このダイアログのフィールドにフィールドに対応しており、 **新しいジョブ** Azure ポータルのダイアログ。 詳細については、次を参照してください。 [web ジョブでバック グラウンドの実行タスク](web-sites-create-web-jobs.md)します。

スケジュールされた web ジョブ (継続的な web ジョブ) 的ではなく、Visual Studio の作成、 [Azure Scheduler](/services/scheduler/) が、存在しないし、コレクション内のジョブを作成する場合、ジョブ コレクション。

* スケジューラ ジョブ コレクションの名前は *webjobs-}* 、 *{regionname}* はで web アプリがホストされているリージョンを表します。 例: WebJobs-WestUS。
* スケジューラ ジョブの名前は *{webappname}-{webjobname}*します。 例: MyWebApp-MyWebJob。 
 
>[AZURE.NOTE]
> 
>* コマンド ライン デプロイメントについては、次を参照してください。 [Enabling Command-line or または継続的な配信の Azure web ジョブ](/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)します。
>* 構成する場合、 **定期的なジョブ** し、繰り返しの頻度の設定に分数、Azure Scheduler サービスは無料です。 その他の頻度 (時間、日など) では無料です。
>* Web ジョブをデプロイした後、その Web ジョブの種類を変更して再デプロイすることに決めた場合は、webjobs-publish-settings.json ファイルを削除する必要があります。 これにより発行オプションが再度表示され、Web ジョブの種類を変更できます。
>* Web ジョブをデプロイし、後から実行モードを継続的から継続的以外、またはその逆に変更した場合、Visual Studio は再デプロイ時に Azure 内に新しい Web ジョブを作成します。 その他のスケジュール設定を変更し、実行モードを同じままにするか、または [スケジュール済み] と [オンデマンド] の間で切り替えた場合は、Visual Studio は新しいジョブを作成するのではなく、既存のジョブを更新します。

## <a id="publishsettings"></a>webjob-publish-settings.json

Web ジョブのデプロイメント用にコンソール アプリケーションを構成するときに、Visual Studio がインストール、 [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet パッケージ 
およびスケジュール情報が格納、 *web ジョブ発行 settings.json* プロジェクト内のファイル *プロパティ* web ジョブ プロジェクトのフォルダーです。 次にこのファイルの例を示します。

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "2014-06-23T00:00:00-08:00",
          "endTime": "2014-06-27T00:00:00-08:00",
          "jobRecurrenceFrequency": "Minute",
          "interval": 5,
          "runMode": "Scheduled"
        }

このファイルは直接編集でき、Visual Studio で IntelliSense を使用できます。 ファイルのスキーマが格納されている [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) し、そこで表示できます。  

>[AZURE.NOTE]
>
>* 構成する場合、 **定期的なジョブ** し、繰り返しの頻度の設定に分数、Azure Scheduler サービスは無料です。 その他の頻度 (時間、日など) では無料です。

## <a id="webjobslist"></a>webjobs-list.json

Visual Studio が web ジョブ プロジェクトの名前を格納する web ジョブ対応のプロジェクトを web プロジェクトにリンクすると、 *webjobs-list.json* web プロジェクトのファイル *プロパティ* フォルダーです。 次の例に示すように、一覧には複数の Web ジョブ プロジェクトが含まれる場合があります。

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

このファイルは直接編集でき、Visual Studio で IntelliSense を使用できます。 ファイルのスキーマが格納されている [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) し、そこで表示できます。
  
## <a id="deploy"></a>Web ジョブ プロジェクトをデプロイする

Web プロジェクトにリンクされた Web ジョブ プロジェクトは、Web プロジェクトと共に自動的にデプロイされます。 Web プロジェクトのデプロイメントについては、次を参照してください。 [Web アプリをデプロイする方法](web-sites-deploy.md)します。

単独で web ジョブ プロジェクトを展開するでプロジェクトを右クリックし **ソリューション エクスプ ローラー**, 、] をクリック **[Azure web ジョブとして発行**します。 

![Publish as Azure WebJob](./media/websites-dotnet-deploy-webjobs/paw.png)
    
独立した web ジョブの場合、同じ **Web の発行** web プロジェクトが表示されますは変更可能な数の設定に使用するウィザード。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## <a id="nextsteps"></a>次のステップ

この記事では、Visual Studio を使用して Web ジョブを展開する方法について説明しました。 Visual Studio から、および継続的な配信プロセスを使用して Azure web ジョブを展開する方法については、次を参照してください。 [Azure web ジョブ - 推奨リソース - デプロイメント](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying)します。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


