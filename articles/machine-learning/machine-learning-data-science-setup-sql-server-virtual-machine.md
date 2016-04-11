<properties
    pageTitle="SQL Server 仮想マシンを IPython Notebook サーバーとして設定する | Microsoft Azure"
    description="SQL Server および IPython Server のあるデータ サイエンス仮想マシンを設定します。"
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
    ms.date="10/13/2015"
    ms.author="mohabib;xibingao;bradsev" />

# 高度な分析のために Azure SQL Server 仮想マシンを IPython Notebook サーバーとして設定する

このトピックでは、クラウド ベースのデータ サイエンス環境の一部として使われる SQL Server 仮想マシンをプロビジョニングおよび構成する方法について説明します。 IPython Notebook、Azure ストレージ エクスプローラーおよび AzCopy などのサポート ツールに加えて、データ サイエンス プロジェクトに役立つ他のユーティリティのある Windows 仮想マシンを構成します。 たとえば Azure ストレージ エクスプローラーおよび AzCopy には、ローカル マシンから Azure BLOB ストレージにデータをアップロードしたり、BLOB ストレージからローカル マシンにデータをダウンロードしたりするための便利な機能が備わっています。

Azure の仮想マシン イメージ ギャラリーには、Microsoft SQL Server を含むイメージがいくつか用意されています。 実際のデータ要件に適した SQL Server VM イメージを選択してください。 推奨されるイメージは、次のとおりです。

- SQL Server 2012 SP2 Enterprise (小規模および中規模のデータ サイズ向け)
- SQL Server 2012 SP2 Enterprise Optimized for DataWarehousing Workloads (大規模、または非常に大規模なデータ サイズ向け)

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise イメージ **データ ディスクを含まない**します。 データを格納するために 1 つ以上の仮想ハード ディスクを追加またはアタッチする必要があります。 Azure の仮想マシンを作成する場合は、オペレーティング システムをディスクの C ドライブにマップし、一時ディスクを D ドライブにマップします。 データの格納に D ドライブを使用しないでください。 名前が示すとおり、D ドライブは一時的なストレージのみを提供します。 Azure Storage に配置されていないため、冗長性やバックアップは提供しません。


##<a name="Provision"></a>Azure クラシック ポータルに接続し、SQL Server 仮想マシンをプロビジョニングする

1.  ログイン、 [Azure Classic Portal](http://manage.windowsazure.com/) 、アカウントを使用します。
    Azure アカウントがないを参照してください [Azure の無料。
    試用版](http://www.windowsazure.com/pricing/free-trial/)します。

2.  Azure クラシック ポータルで、web ページの左下にあります。
    クリックして **+ 新規**, 、] をクリックして **コンピューティング**, 、] をクリックして **仮想マシン**, と
    クリックし、 **ギャラリーから**します。

3.   **仮想マシンの作成** ] ページで、仮想マシンの選択
    イメージの実際のデータ要件に応じた SQL Server を含む/にある次へ進む矢印をクリックします
    ページの右下です。 Azure では、サポートされる SQL Server イメージの最新情報について
    参照してください [Azure 仮想マシンにおける SQL Server の概要](http://go.microsoft.com/fwlink/p/?LinkId=294720) のトピック、 [Azure 仮想マシンにおける SQL Server](http://go.microsoft.com/fwlink/p/?LinkId=294719) ドキュメント セットです。

    ![SQL Server VM の選択][1]

4.  最初の **仮想マシンの構成** ページで指定します
    以下の情報:

    -   提供、 **仮想マシン名**します。
    -    **新しいユーザー名** ボックスで、VM の一意のユーザー名を入力
        ローカル管理者アカウントです。
    -    **新しいパスワード** ボックスに、強力なパスワードを入力します。 サブスクリプションの
        についてを参照してください [強力なパスワードの](http://msdn.microsoft.com/library/ms161962.aspx)です。
    -    **パスワードの確認** ボックスで、パスワードを再入力します。
    -   適切な選択 **サイズ** ドロップダウン リストからです。

     > [AZURE.NOTE] プロビジョニング中に仮想マシンのサイズを指定します A2。
    最小サイズは運用環境のワークロードを推奨します。 この
    SQL の使用時に、仮想マシンの最小推奨サイズは A3
    Server Enterprise Edition。 A3 を選択または SQL Server を使用する場合より高い。
    Enterprise エディションです。 SQL Server 2012 または 2014年を使用する場合は、A4 を選択します。
    エンタープライズ Transactional Workloads イメージに適しています。
     SQL Server 2012 または 2014 Enterprise Optimized を使用する場合は、A7 を選択します。
    Data Warehousing Workloads イメージ。 サイズが制限を選択します
    構成できるデータ ディスクの数。 ほとんどの最新の状態
    使用可能な仮想マシンのサイズと数に関する情報
    仮想マシンに接続できるデータ ディスクを参照してください [仮想
    マシンのサイズ
    Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx)します。 料金情報については、次を参照してください。 [仮想マシンの料金](http://azure.microsoft.com/pricing/details/virtual-machines/)します。

    右下にある次へ進む矢印をクリックして続行します。

    ![VM 構成][2]

5.  2 番目の **仮想マシンの構成** ] ページで、構成します。
    ネットワー キング、ストレージ、および可用性のためのリソース:

    -    **クラウド サービス** ボックスで、選択 **新しいクラウドの作成
        サービス**します。
    -    **クラウド サービス DNS 名** ボックスで、最初の部分を入力します。
        任意の DNS 名ののでその it が完了すると内の名前、
        形式 **TESTNAME.cloudapp.net**
    -    **リージョン/アフィニティ グループ/仮想ネットワーク** ボックスで、選択します。
        この仮想イメージをホストするリージョン。
    -    **ストレージ アカウント**, 、既存のストレージ アカウントを選択
        または、自動的に生成された 1 つを選択します。
    -    **可用性セット** ボックスで、 **(なし)**します。
    -   価格に関する情報を読んで同意します。

6.   **エンドポイント** セクションで、下にある空のドロップダウンでをクリックして **名前**, と
    選択 **MSSQL**  のポート番号を入力します
    データベース エンジンのインスタンス (**1433年** の既定のインスタンス)。

7.  また、SQL Server VM を、後の手順で構成される IPython Notebook Server として機能させることもできます。
    IPython Notebook サーバー用に使われるポートを指定するために新しいエンドポイントを追加してください。 名前を入力、 **名前** 列で、パブリック ポート用に任意のポート番号を選択して 9999 プライベート ポートを指定します。

    右下にある次へ進む矢印をクリックして続行します。

    ![MSSQL ポートと IPython のポートの選択][3]

8.  既定値をそのまま使用 **VM エージェントのインストール** チェック ボックスをオンしをクリックして、右下にあるチェック マーク
    クリックして、ウィザードで、VM プロビジョニング手順を完了します。

    `![VM 最終オプション][4]

9.  Azure によって仮想マシンの準備が行われるまで待ちます。 バーチャル マシンの予想されます。
    マシンの状態を進めます。

    -   開始中 (プロビジョニング)
    -   停止済み
    -   開始中 (プロビジョニング)
    -   実行中 (プロビジョニング)
    -   実行中

##<a name="RemoteDesktop"></a>リモート デスクトップを使用して仮想マシンを開き、セットアップを完了する

1.  プロビジョニングが完了したら、仮想の名前をクリックします。
    ダッシュ ボード ページに移動するコンピューター。 ページの下部にあります。
    クリックして **接続**します。

2.  Windows リモート デスクトップ プログラムを使用して rpd ファイルを開くことを選択します。
    (`%windir%\system32\mstsc.exe`).

3.   **Windows セキュリティ** ] ダイアログ ボックスで、パスワードを入力します
    前の手順で指定したローカル管理者アカウントです。
    (たずねられることがあります仮想の資格情報を確認するには
    マシンです。)

4.  最初にログオンしているをこの仮想マシンに複数のプロセス
    デスクトップのセットアップなど、Windows を実行する必要があります。
    更新プログラム、および Windows の初期構成タスクの完了
    (sysprep)。 Windows sysprep の完了後、SQL Server のセットアップ
    構成タスクを完了します。 これらのタスクの遅延が発生する可能性があります、
    これらが完了するまでの数分の間です。 `SELECT @@SERVERNAME` できません。
    SQL Server セットアップが完了するまで、適切な名前と SQL を返す
    Server Management Studio は、スタート ページに表示しない場合があります。

Windows リモート使用してバーチャル マシンに接続して 1 回
デスクトップの仮想マシンは、その他のコンピュータと同様に機能します。 接続
SQL Server Management Studio での SQL Server の既定のインスタンスに
(仮想マシンで実行されている)、通常どおりにします。

##<a name="InstallIPython"></a>IPython Notebook とその他のサポート ツールをインストールする

IPython Notebook サーバーとして機能し、追加のインストールに、新しい SQL Server VM を構成するには
このような AzCopy、Azure ストレージ エクスプ ローラー、便利な Data Science Python パッケージ、およびその他のユーザーにツールをサポートします。
特別なカスタマイズ スクリプトが支給されます。 インストールするには、次のようにします。

- Windows の [スタート] アイコンを右クリックし、クリックして **コマンド プロンプト (管理者)**
- 次のコマンドをコピーして、コマンド プロンプトに貼り付けます。

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- メッセージが表示されたら、IPython Notebook サーバーの適切なパスワードを入力します。
- カスタマイズ スクリプトは、次に示すようなインストール後の手順を自動化します。
    + IPython Notebook サーバーのインストールとセットアップ
    + 既に作成した次のエンドポイント用に Windows ファイアウォールで TCP ポートを開く
    + SQL Server リモート接続用
    + IPython Notebook サーバーのリモート接続用
    + サンプルの IPython ノートブックおよび SQL スクリプトを取得する
    + 便利な Data Science Python パッケージをダウンロードしてインストールする
    + AzCopy、Azure Storage Explorer などの Azure ツールをダウンロードしてインストールする  
<br>
- 任意のローカルまたはリモート ブラウザーから、URL 形式 `https://<virtual_machine_DNS_name>:<port>` を使用して IPython Notebook にアクセスし実行することができます (ポートは仮想マシンのプロビジョニング中に選択した IPython パブリック ポート)。
- IPython Notebook サーバーはバックグラウンド サービスとして実行され、仮想マシンの再起動時に自動的に再起動されます。

##<a name="Optional"></a>必要に応じて、データ ディスクをアタッチする

データ ディスク、つまり C ドライブ (OS ディスク) と D ドライブ (一時的なディスク) 以外のディスクが VM イメージに含まれない場合、データを格納するために 1 つ以上のデータ ディスクを追加する必要があります。 SQL Server 2012 SP2 Enterprise Optimized for DataWarehousing Workloads 用の VM イメージは、SQL Server データおよびログ ファイル用の追加のディスク付きで事前構成されています。

 > [AZURE.NOTE] データの格納に D ドライブを使わないでください。 名前が示すとおり、D ドライブは一時的なストレージのみを提供します。 Azure Storage に配置されていないため、冗長性やバックアップは提供しません。

以下で説明する手順の追加データ ディスクをアタッチする [Windows 仮想マシンへのデータ ディスクを接続する方法](storage-windows-attach-disk.md), 、これが指示に従って操作します。

1. 前の手順でプロビジョニングされた仮想マシンに 1 つ以上の空のディスクをアタッチする
2. 仮想マシンでの新しいディスクの初期化


##<a name="SSMS"></a>SQL Server Management Studio に接続し、混合モード認証を有効にする

SQL Server データベース エンジンは Windows 認証なしで使用できません。
ドメイン環境です。 別のデータベース エンジンに接続するには
コンピューターは、混合モード認証用の SQL Server を構成します。 混在モード
認証では、SQL Server 認証と Windows の両方
認証します。 データを取り込むに SQL 認証モードが必要
SQL Server VM データベースから直接、
[Azure Machine Learning Studio](https://studio.azureml.net) リーダー モジュールを使用します。

1.  リモート デスクトップを使用して、仮想マシンに接続されている場合、Windows を使用して **検索** ウィンドウと型 **SQL Server Management Studio** (SMSS)。 クリックして SQL Server Management Studio (SSMS) を開始します。 SSMS のショートカットをデスクトップ上に作成して、将来使用することができます。

    ![SSMS を起動する][5]

    Management Studio では、最初に開いた際に、そのユーザーを作成する必要があります。
    Studio 環境を管理します。 これには数分かかることがあります。

2.  Management Studio を表示を開くときに、 **サーバーへの接続**
    ダイアログ ボックス。  **サーバー名** ボックスに、バーチャル マシンの名前を入力
    オブジェクト エクスプ ローラーでデータベース エンジンに接続するコンピューター。
    (仮想マシン名の代わりに使用することも **(ローカル)** または
    単一のピリオドとして、 **サーバー名**します。 選択 **Windows
    認証**, 、出入り
    ***your \_vm\_name*\\your\_local\_administrator** で、 **ユーザー
    名前** ボックス。 クリックして **接続**します。

    ![サーバーへの接続][6]

    <br>

     > [AZURE.TIP] Windows レジストリ キーの変更するか、または SQL Server Management Studio を使用して、SQL Server の認証モードを変更することがあります。 レジストリ キーの変更を使用して認証モードを変更するには、開始、 **新しいクエリ** 次のスクリプトを実行します。

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    SQL Server Management Studio を使用して認証モードを変更するには、次のようにします。

3.  SQL Server Management Studio オブジェクト エクスプ ローラーで、右クリックし、
    SQL Server (仮想マシン名) のインスタンスの名前と
    クリックし、 **プロパティ**します。

    ![サーバー プロパティ][7]

4.   **セキュリティ** ] ページで、[ **サーバー認証**, を選択
    **SQL Server と Windows 認証モード**, 、] をクリックし、
    **[OK]**します。

    ![認証モードを選択する][8]

5.  SQL Server Management Studio] ダイアログ ボックスで、[ **OK** に
    SQL Server を再起動する要件を確認します。

6.  オブジェクト エクスプ ローラーで、サーバーを右クリックし、をクリックしてください
    **再起動**します。 (SQL Server エージェントが実行されている場合でもなければ
    再起動)。

    ![再起動][9]

7.  SQL Server Management Studio] ダイアログ ボックスで、[ **はい** に
    SQL Server を再起動することに同意します。

##<a name="Logins"></a>SQL Server 認証ログインを作成する

別のコンピューターからデータベース エンジンに接続するに作成する必要があります。
1 つ以上の SQL Server 認証ログインです。  

> [AZURE.TIP] 新しい SQL Server ログインをプログラムで作成することがありますか、SQL Server Management Studio を使用します。 SQL 認証で新しい sysadmin ユーザーをプログラムで作成するには、開始、 **新しいクエリ** 次のスクリプトを実行します。 置換 < [新しいユーザー名 \ > と < 新しいパスワード > をお好みのユーザー名とパスワード。 必要に応じてパスワード ポリシーを調整します (サンプル コードではポリシー検査とパスワード期限設定がオフになっています)。 SQL Server ログインの詳細については、次を参照してください。 [ログインを作成](http://msdn.microsoft.com/library/aa337562.aspx)します。  

    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';

SQL Server Management Studio を使用して新しい SQL Server ログインを作成するには、次のようにします。

1.  SQL Server Management Studio オブジェクト エクスプ ローラーで、フォルダーを展開します
    新しいログインを作成するサーバー インスタンス。

2.  右クリックし、 **セキュリティ** フォルダーを指す **新規**, を選択
    **ログイン]**します。

    ![新しいログイン][10]

3.   **ログイン - 新規** ] ダイアログ ボックスで、 **全般** ページで、入力
    新しいユーザーの名前、 **ログイン名** ボックス。

4.  選択 **SQL Server 認証**します。

5.   **パスワード** ボックスで、新しいユーザーのパスワードを入力します。 次に、
    もう一度に、パスワードを **パスワードの確認** ボックス。

6.  強制と複雑さに関するパスワード ポリシー オプションを適用するには
    選択 **パスワード ポリシーを適用** (推奨)。 これは、既定値です。
    SQL Server 認証が選択した場合はオプションです。

7.  が有効期限に関するパスワード ポリシー オプションを適用するのには、選択 **適用します。
    パスワードの有効期限** (推奨)。 適用するパスワード ポリシーがある必要があります
    このチェック ボックスを有効にする場合に選択します。 これは、既定のオプションを SQL
    サーバー認証が選択されます。

8.  2 回目以降後、新しいパスワードを作成するユーザーを強制的に、
    ログインを使用すると、選択 **ユーザーは次回ログイン時にパスワードを変更する必要があります**
    (このログインが使用する他のユーザー用のかどうかはお勧めします。 場合、ログイン
    独自はこのオプションを選択します)。パスワードを適用します。
    このチェック ボックスを有効にするには、有効期限を選択してください。 これは、
    SQL Server 認証が選択した場合の既定のオプション。

9.   **既定のデータベース** ボックスの一覧で、既定のデータベースの選択
    ログインです。 **マスター** このオプションの既定値です。 ある場合
    ユーザー データベースが作成されていない、ままにしておきます **マスター**します。

10.  **既定の言語** ボックスの一覧で、そのまま使用 **既定** 値として。

    ![ログインのプロパティ][11]

11. 最初のログインを作成する場合にすることがあります。
    SQL Server の管理者としてこのログインを指定します。 その場合、
    **サーバーの役割** ] ページで、チェック **sysadmin**します。

    **セキュリティに関する注意:** sysadmin 固定サーバー ロールのメンバーであります。
    データベース エンジンを完全に制御します。 慎重にする必要があります。
    このロールのメンバーシップを制限します。

    ![sysadmin][12]

12. [OK] をクリックします。

##<a name="DNS"></a>仮想マシンの DNS 名を特定する

別のコンピューターから SQL Server データベース エンジンに接続します。
仮想マシンのドメイン ネーム システム (DNS) 名を確認する必要があります。
(これは、仮想マシンを識別するためにインターネットで使用される名前です。 クラスターの
IP アドレスを使用できますが、Azure に移動すると、IP アドレスを変更することがあります。
冗長性またはメンテナンスのためのリソース。 DNS 名は安定しています
リダイレクトできる新しい IP アドレスにします)。

1.  Azure クラシック ポータルで (または前の手順から) を選択
    **仮想マシン**します。

2.   **仮想マシン インスタンス** ] ページで、 **DNS 名**
    列、検索、コピー、DNS 名、バーチャル マシン
    始まる **http://**します。 (ユーザー インターフェイスは実行できません。
    名前全体が表示されますが、それを右クリックして選択できます。
    コピーします。)

##<a name="cde"></a>別のコンピューターからデータベース エンジンに接続する

1.  インターネットに接続されたコンピューターで、SQL Server Management Studio を開きます。

2.   **サーバーへの接続** または **データベース エンジンに接続します。**
    ダイアログ ボックスで、 **サーバー名** ボックスの DNS 名を入力します
    (前のタスクで決定した) 仮想マシンとパブリック
    エンドポイントのポート番号の形式で *DNSName, portnumber* など
    **tutorialtestVM.cloudapp.net,57500**します。

3.   **認証** ボックスで、 **SQL Server 認証**します。

4.   **ログイン** ボックスで作成したログインの名前を入力
    前のタスクです。

5.   **パスワード** ボックスに、ログインのパスワードを入力します。
    前のタスクで作成します。

6.  クリックして **接続**します。

##<a name="amlconnect"></a>Azure Machine Learning からデータベース エンジンに接続します

Advanced Analytics Process and Technology の後の段階では使用して、 [Azure Machine Learning Studio](https://studio.azureml.net) をビルドし、機械学習モデルを展開します。 取り込むには SQL Server VM データベースからデータを直接 Azure Machine Learning のトレーニングまたはスコアリングのために、新しいリーダー モジュールを使用して [Azure Machine Learning Studio](https://studio.azureml.net) 実験します。 このトピックの詳細は Advanced Analytics Process and Technology のガイド リンクに記載されています。 概要については、次を参照してください。 [Azure Machine Learning Studio は何ですか?](machine-learning-what-is-ml-studio.md)します。

2.   **プロパティ** のウィンドウ、 [リーダー モジュール](https://msdn.microsoft.com/library/azure/dn905997.aspx), [ **Azure SQL Database** から、 **データ ソース**   ドロップダウン リストです。

3.   **データベース サーバー名** テキスト ボックスに、入力 `tcp:<DNS name of your virtual machine>,1433`

4.  SQL ユーザー名を入力、 **サーバーのユーザー アカウント名** テキスト ボックスです。

5.  Sql ユーザーのパスワードを入力、 **サーバー ユーザー アカウントのパスワード** テキスト ボックスです。

    ![Azure ML リーダー][13]

##<a name="shutdown"></a>使用されていないときは仮想マシンをシャット ダウンし割り当てを解除する

Azure の仮想マシンの料金は **を使用するのには、のみ支払**します。 いるを課金されないバーチャル マシンを使用していない場合、必要が内にある、 **停止 (割り当て解除)** 状態です。

> [AZURE.NOTE] 仮想マシンをシャット ダウンから VM を停止する (Windows 電源オプションを使用)、内部がまま割り当ています。 課金されないしていることを確認するには、常に仮想マシンを停止、 [Azure Classic Portal](http://manage.windowsazure.com/)します。 また、Powershell で VM を停止することもできます。その際、ShutdownRoleOperation 呼び出しで "PostShutdownAction" を "StoppedDeallocated" にしてください。

仮想マシンをシャット ダウンして割り当て解除するには、次のようにします。

1. ログイン、 [Azure Classic Portal](http://manage.windowsazure.com/) 、アカウントを使用します。  

2. 選択 **仮想マシン** 左側のナビゲーション バーからです。

3. 仮想マシンの一覧で、仮想の名前をクリックします。
   移動し、コンピューター、 **ダッシュ ボード** ページです。

4. ページの下部にあるをクリックして **シャット ダウン**します。

![VM シャットダウン][15]

仮想マシンの割り当てが解除されますが、削除はされません。 Azure クラシック ポータルから、いつでも仮想マシンを再起動することができます。

## Azure SQL Server VM を使用する準備ができました。次のステップは ...

これで、仮想マシンをデータ サイエンス演習で使用する準備ができました。 また、仮想マシンを IPython Notebook サーバーとして使用し、データの探索と処理など、Azure Machine Learning および Cortana Analytics Process (CAP) に関連するタスクを行う準備もできました。

データ サイエンス プロセスにおける次のステップがマップされて、 [学習ガイド: Azure でのデータ処理を高度な](machine-learning-data-science-advanced-data-processing.md) HDInsight にデータを移動して、Azure Machine Learning でデータから学習するための準備にサンプリングする手順が含まれています。


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 

