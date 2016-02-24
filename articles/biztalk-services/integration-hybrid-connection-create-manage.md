<properties 
    pageTitle="ハイブリッド接続の作成と管理 | Microsoft Azure" 
    description="ハイブリッド接続の作成、接続の管理、Hybrid Connection Manager のインストールの方法について説明します。 MABS、WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="dwrede" 
    editor="cgronlun"/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/03/2015" 
    ms.author="mandia"/>


# ハイブリッド接続の作成と管理


## 手順の概要
1. プライベート ネットワークのオンプレミス リソースのホスト名または IP アドレスを入力してハイブリッド接続を作成する。
2. ハイブリッド接続に Azure Web Apps または Azure Mobile Apps を関連付ける。
3. オンプレミス リソースに Hybrid Connection Manager をインストールし、特定のハイブリッド接続に接続する。 Azure ポータルでは 1 回のクリックでインストールと接続を実行できます。
4. ハイブリッド接続を管理し、その接続キーを管理します。

このトピックでは次の手順を示します。 


## <a name="CreateHybridConnection"></a>ハイブリッド接続を作成する

Web アプリを使用して Azure ポータルには、ハイブリッド接続を作成できます **または** BizTalk サービスを使用します。 

**Web アプリを使用してハイブリッド接続を作成する**, を参照してください [内部設置型リソースへの Azure Web アプリの接続](../web-sites-hybrid-connection-get-started.md)します。

**BizTalk サービスでハイブリッド接続を作成する**:

1. サインイン、 [Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)します。
2. 左側のナビゲーション ウィンドウで [ **BizTalk サービス** し、BizTalk サービスを選択します。 

    既存の BizTalk サービスを取得していない場合は、 [BizTalk サービスを作成する](biztalk-provision-services.md)です。
3. 選択、 **ハイブリッド接続** ] タブをクリックします。  
![ハイブリッド接続] タブ][HybridConnectionTab]

4. 選択 **ハイブリッド接続の作成** または選択、 **追加** タスク バーのボタンをクリックします。 次のように入力します。

    プロパティ |説明
--- | ---
名前 |ハイブリッド接続の名前は一意である必要があり、BizTalk サービスと同じ名前にすることはできません。 任意の名前を入力できますが、目的を具体的に示した名前にしてください。 これに当たります<br/><br/>給与*SQLServer*<br/>SupplyList*SharepointServer*<br/>顧客*OracleServer。*
ホスト名 |完全修飾ホスト名を入力して、ホスト名のみ、または内部設置型リソースの IPv4 アドレスです。 これに当たります<br/><br/>mySQLServer<br/>*mySQLServer*。*。ドメイン*. corp.*yourCompany*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*.*yourCompany*.com<br/>10.100.10.10
ポート |内部設置型リソースのポート番号を入力します。 たとえば、Web Appsを使用している場合、ポート 80 またはポート 443 を入力します。 SQL Server を使用している場合、ポート 1433 を入力します。

5. チェック マークをオンにしてセットアップを完了します。 

#### その他

- 複数のハイブリッド接続を作成することができます。 参照してください、 [BizTalk サービス: エディションのチャート](biztalk-editions-feature-chart.md) で許可される接続の数。 
- それぞれのハイブリッド接続は、送信用のアプリケーション キーとリッスン用のオンプレミス キーで構成される接続文字列のペアと共に作成されます。 各ペアにはプライマリ キーとセカンダリ キーが含まれています。 


## <a name="LinkWebSite"></a>Azure の Web アプリまたは Azure モバイル アプリを関連付け

Azure Web アプリを既存のハイブリッド接続にリンクするには、次のように選択します。 **既存のハイブリッド接続を使用して** ハイブリッド接続ブレードでします。 参照してください [Azure Web アプリを内部設置型リソースに接続](../web-sites-hybrid-connection-get-started.md)します。

Azure Mobile Apps を既存のハイブリッド接続にリンクするには、次のように選択します。 **ハイブリッド接続の追加** 変更またはモバイル サービスを作成するとき。 参照してください [Azure Mobile Services とハイブリッド接続](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)します。


## <a name="InstallHCM"></a>内部設置型への Hybrid Connection Manager のインストール

ハイブリッド接続の作成後、オンプレミス リソースに Hybrid Connection Manager をインストールします。 Hybrid Connection Manager は Azure Web Apps または BizTalk サービスからダウンロードできます。 BizTalk Services の手順は以下のとおりです。 

1. サインイン、 [Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)します。
2. 左側のナビゲーション ウィンドウで [ **BizTalk サービス** し、BizTalk サービスを選択します。 
3. 選択、 **ハイブリッド接続** ] タブをクリックします。  
![ハイブリッド接続] タブ][HybridConnectionTab]
4. タスク バーで [ **内部設置型設定**:  
![内部設置型設定][HCOnPremSetup]
5. 選択 **のインストールし、構成** 実行または内部設置型システムで Hybrid Connection Manager をダウンロードします。 
6. チェック マークを選択して、インストールを開始します。 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. 内部設置型のプライマリ接続文字列をコピーします。 参照してください [ハイブリッド接続の管理](#ManageHybridConnection) 、具体的な手順については、このトピックでします。
2. Hybrid Connection Manager の MSI ファイルをダウンロードします。 
3. 内部設置型リソースでは、MSI ファイルから Hybrid Connection Manager をインストールします。 
4. Windows PowerShell を使用して入力します。 
> Add-hybridconnection-connectionstring"*内部設置型接続文字列をコピーした*" 
--> 

#### その他
- ハイブリッド接続では、以下のオペレーティング システムにインストールされているオンプレミス リソースがサポートされます。

    - Windows Server 2008 R2
    - Windows Server 2012
    - Windows Server 2012 R2


- Hybrid Connection Manager をインストールすると、以下のことが行われます。 

    - プライマリ アプリケーション接続文字列が使用されるように、Azure がホストするハイブリッド接続が自動的に構成されます。 
    - プライマリ オンプレミス接続文字列が使用されるように、オンプレミス リソースが自動的に構成されます。

- Hybrid Connection Manager は、承認のために有効なオンプレミスの接続文字列を使用する必要があります。 Azure Web Apps または Mobile Apps は、承認のために有効なアプリケーション接続文字列を使用する必要があります。
- Hybrid Connection Manager の別のインスタンスを別のサーバーにインストールすることにより、ハイブリッド接続を拡張できます。 最初のオンプレミス リスナーと同じアドレスを使用するように、オンプレミス リスナーを構成します。 この状況では、トラフィックは複数のアクティブなオンプレミス リスナーにランダムに分散されます (ラウンド ロビン方式)。 


## <a name="ManageHybridConnection"></a>ハイブリッド接続の管理
ハイブリッド接続を管理するために、以下のことを実行できます。

- Azure ポータルを使用して BizTalk サービスに移動する。 
- 使用 [REST Api](http://msdn.microsoft.com/library/azure/dn232347.aspx)します。

#### ハイブリッド接続文字列をコピー、再生成する

1. サインイン、 [Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)します。
2. 左側のナビゲーション ウィンドウで [ **BizTalk サービス** し、BizTalk サービスを選択します。 
3. 選択、 **ハイブリッド接続** ] タブをクリックします。  
![ハイブリッド接続] タブ][HybridConnectionTab]
4. [ハイブリッド接続] を選択します。 タスク バーで [ **接続の管理**:  
![オプションの制御します。][HCManageConnection]

    **接続の管理** 、アプリケーションおよび内部設置型接続文字列を一覧表示します。 接続文字列をコピーすることも、接続文字列で使用するアクセス キーを再生成することもできます。 

    **再生成を選択した場合**, 、接続文字列内で使用される共有アクセス キーを変更します。 以下の手順を実行します。
    - Azure クラシック ポータルで [ **キーの同期** Azure アプリケーションにします。
    - 再実行、 **オンプレミス セットアップ**します。 [オンプレミス設定] を再実行すると、更新されたプライマリ接続文字列がオンプレミス リソースによって使用されるように自動的に構成されます。


#### ハイブリッド接続で使用するオンプレミス リソースを管理するグループ ポリシーの使用

1. ダウンロード、 [Hybrid Connection Manager の管理用テンプレート](http://www.microsoft.com/download/details.aspx?id=42963)します。
2. ファイルを解凍します。
3. グループ ポリシーを変更するコンピューターで、以下の手順を実行します。  

    - コピーします。ADMX ファイルを *%WINROOT%\PolicyDefinitions* フォルダーです。
    - コピーします。ADML ファイルを *%WINROOT%\PolicyDefinitions\en-us* フォルダーです。

コピーが終了すると、グループ ポリシー エディターを使用してポリシーを変更できます。




## 次へ

[Azure Web アプリを内部設置型リソースに接続します。](../web-sites-hybrid-connection-get-started.md)  
[Azure Web アプリから内部設置型 SQL Server に接続します。](../web-sites-hybrid-connection-connect-on-premises-sql-server.md)  
[Azure のモバイル サービスとハイブリッド接続](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)  
[ハイブリッド接続の概要](integration-hybrid-connection-overview.md)


## 関連項目

[Microsoft Azure での BizTalk サービスを管理するための REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk サービス: エディションのチャート](biztalk-editions-feature-chart.md)  
[Azure クラシック ポータルを使用して BizTalk サービスを作成します。](biztalk-provision-services.md)  
[BizTalk サービス: ダッシュ ボード、モニター、スケール タブ](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 

