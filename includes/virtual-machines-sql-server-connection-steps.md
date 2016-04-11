次の手順で、SQL Server Management Studio (SSMS) を使用してインターネット経由で SQL Server インスタンスに接続する方法を説明します。 ただし、同じ手順が、アプリケーションのアクセスが可能な SQL Server 仮想マシンを作成し、オンプレミスと Azure の両方で実行する際に適用されます。

別の VM またはインターネットから SQL Server インスタンスに接続するには、次のタスクを完了している必要があります。詳細については、この後のセクションで説明します。

- [仮想マシン用の TCP エンドポイントを作成する](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Windows ファイアウォールで TCP ポートを開く](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [TCP プロトコルでリッスンするように SQL Server を構成する](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [混合モード認証用に SQL Server を構成する](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server 認証ログインを作成する](#create-sql-server-authentication-logins)
- [仮想マシンの DNS 名を特定する](#determine-the-dns-name-of-the-virtual-machine)
- [別のコンピューターからデータベース エンジンに接続する](#connect-to-the-database-engine-from-another-computer)

次の図は、接続パスの概要を示したものです。

![SQL Server 仮想マシンに接続する](./media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

### 仮想マシン用の TCP エンドポイントを作成する

インターネットから SQL Server にアクセスするには、仮想マシンに、着信する TCP 通信をリッスンするエンドポイントが必要です。 この Azure 構成手順により、仮想マシンからアクセスできる TCP ポートに、着信する TCP ポート トラフィックが送信されます。

>[AZURE.NOTE] 同じクラウド サービスまたは仮想ネットワーク内で接続している場合、公開されたエンドポイントを作成する必要はありません。 その場合は、次のステップに進むことができます。 詳細については、次を参照してください。 [接続シナリオ](../articles/virtual-machines/virtual-machines-sql-server-connectivity.md#connection-scenarios)します。

1. Azure 管理ポータルでのクリックして **仮想マシン**します。
    
2. 新しく作成した仮想マシンをクリックします。 仮想マシンに関する情報が表示されます。
    
3. ページの上部には、選択、 **エンドポイント** ページし、ページの下部にある] をクリックして **追加**します。
    
4.  **仮想マシンにエンドポイントを追加** ] ページで [ **スタンドアロン エンドポイントの追加**, 、続行には、次へ進む矢印をクリックします。
    
5.  **エンドポイントの詳細を指定する** ] ページで、次の情報を提供します。

    -  **名前** ボックスで、エンドポイントの名前を入力します。
    -  **プロトコル** ボックスで、 **TCP**します。 入力することも **57500** で、 **パブリック ポート** ボックス。 同様に、SQL Server の既定のリスニング ポートを入力することも **1433年** で、 **プライベート ポート** ボックス。 多くの組織は、悪意のある攻撃を避けるために異なるポート番号を選択することに注意してください。 

6. チェック マークをクリックして続行します。 エンドポイントが作成されます。

### データベース エンジンの既定のインスタンス用に Windows ファイアウォールで TCP ポートを開く

1. Windows リモート デスクトップを介して仮想マシンに接続します。 ログインした後、スタート画面で入力 **WF.msc**, と入力し、ENTER を押します。 

    ![ファイアウォール プログラムを開始する](./media/virtual-machines-sql-server-connection-steps/12Open-WF.png)
2.  **セキュリティが強化された Windows ファイアウォール**, 、左側のウィンドウで右クリック **受信の規則**, 、] をクリックし、 **新しいルール** 、操作ウィンドウでします。

    ![新しい規則](./media/virtual-machines-sql-server-connection-steps/13New-FW-Rule.png)

3.  **新しい受信の規則ウィザード** ダイアログ ボックスで、 **規則の種類**, [ **ポート**, 、順にクリック **次**します。

4.  **プロトコルおよびポート** ダイアログ ボックスで、既定値を使用して **TCP**します。  **特定のローカル ポート** ボックスで、データベース エンジンのインスタンスのポート番号を入力 (**1433年** の既定のインスタンスか、エンドポイントの手順でプライベート ポート用に任意)。 

    ![TCP ポート 1433](./media/virtual-machines-sql-server-connection-steps/14Port-1433.png)

5. クリックして **次**します。

6.  **アクション** ダイアログ ボックスで、 **接続を許可する**, 、] をクリックし、 **次**します。

    **セキュリティに関する注意:** を選択すると **場合は、セキュリティで保護された接続を許可する** セキュリティを強化できます。 お使いの環境で追加のセキュリティ オプションを構成する場合はこのオプションを選択してください。

    ![接続を許可する](./media/virtual-machines-sql-server-connection-steps/15Allow-Connection.png)

7.  **プロファイル** ダイアログ ボックスで、 **パブリック**, 、**プライベート**, 、および **ドメイン**します。 クリックして **次**します。 

    **セキュリティに関する注意:**  を選択すると **パブリック** 、インターネット経由でアクセスを許可します。 可能であれば、できるだけ制限の厳しいプロファイルを選択してください。

    ![パブリック プロファイル](./media/virtual-machines-sql-server-connection-steps/16Public-Private-Domain-Profile.png)

8.  **名前** ダイアログ ボックスで、この規則の説明と名前を入力し、 **完了**します。

    ![規則の名前](./media/virtual-machines-sql-server-connection-steps/17Rule-Name.png)

必要に応じて他のコンポーネント用に追加のポートを開きます。 詳細については、次を参照してください。 [SQL Server のアクセスを許可するための Windows ファイアウォールを構成する](http://msdn.microsoft.com/library/cc646023.aspx)です。


### TCP プロトコルでリッスンするように SQL Server を構成する

1. [スタート] ページで、仮想マシンに接続されている間に「 **SQL Server 構成マネージャー** し、enter キーを押します。
    
    ![SSCM を開く](./media/virtual-machines-sql-server-connection-steps/9Click-SSCM.png)

2. SQL Server 構成マネージャーで、コンソール ペインで、展開 **SQL Server ネットワーク構成**します。

3. コンソール ウィンドウでクリックして **MSSQLSERVER のプロトコル** (既定のインスタンス名です)。詳細ウィンドウで、[TCP] を右クリックします。ギャラリー イメージの場合、その状態は既定で [有効] です。 カスタム イメージをクリックして **を有効にする** (場合、その状態は無効です)。

    ![TCP を有効にする](./media/virtual-machines-sql-server-connection-steps/10Enable-TCP.png)

5. コンソール ウィンドウでクリックして **SQL Server サービス**します。 詳細ペインで右クリックし **SQL Server (_インスタンス名_)** (既定のインスタンスは **SQL Server (MSSQLSERVER)**) を順にクリック **再起動**, を停止し、SQL Server のインスタンスを再起動します。 

    ![データベース エンジンの再起動](./media/virtual-machines-sql-server-connection-steps/11Restart.png)

7. SQL Server 構成マネージャーを閉じます。

SQL Server データベース エンジンのプロトコルを有効にする方法の詳細については、次を参照してください。 [を有効にするか、サーバー ネットワーク プロトコルを無効にする](http://msdn.microsoft.com/library/ms191294.aspx)です。

### 混合モード認証用に SQL Server を構成する

ドメイン環境がない場合、SQL Server データベース エンジンで Windows 認証を使用することはできません。 別のコンピューターからデータベース エンジンに接続するには、混合モード認証用に SQL Server を構成します。 混合モード認証では、SQL Server 認証と Windows 認証の両方が許可されます

>[AZURE.NOTE] 混合モード認証の構成は必要ありません、構成されているドメイン環境で Azure 仮想ネットワークを構成した場合。

1. [スタート] ページで、仮想マシンに接続されている間に「 **SQL Server 2014 Management Studio** 、選択したアイコンをクリックしますします。

    ![SSMS を起動する](./media/virtual-machines-sql-server-connection-steps/18Start-SSMS.png)

    初めて Management Studio を開く場合は、ユーザーの Management Studio 環境の作成が必要になります。 これには数分かかることがあります。

2. Management Studio は、 **サーバーへの接続** ] ダイアログ ボックス。  **サーバー名** ボックスに、オブジェクト エクスプ ローラーでデータベース エンジンに接続する仮想マシンの名前を入力します。 (仮想マシン名の代わりに使用することも **(ローカル)** またはとして単一のピリオド、 **サーバー名**します。 選択 **Windows 認証**, 、出入り **_使用_\your_local_administrator** で、 **ユーザー名** ボックス。 クリックして **接続**します。

    ![サーバーへの接続](./media/virtual-machines-sql-server-connection-steps/19Connect-to-Server.png)

3. SQL Server Management Studio オブジェクト エクスプ ローラーで、SQL Server (仮想マシン名) のインスタンスの名前を右クリックし、順にクリックして **プロパティ**します。

    ![サーバー プロパティ](./media/virtual-machines-sql-server-connection-steps/20Server-Properties.png)

4.  **セキュリティ** ] ページで、[ **サーバー認証**, [ **SQL Server と Windows 認証モード**, 、順にクリック **[ok]**します。

    ![認証モードを選択する](./media/virtual-machines-sql-server-connection-steps/21Mixed-Mode.png)

5. SQL Server Management Studio] ダイアログ ボックスで、[ **OK** SQL サーバーを再起動する要件を確認します。

6. オブジェクト エクスプ ローラーで、サーバーを右クリックし、クリックして **再起動**します。 (実行中であれば、SQL Server エージェントも再起動する必要があります)。

    ![再起動](./media/virtual-machines-sql-server-connection-steps/22Restart2.png)

7. SQL Server Management Studio] ダイアログ ボックスで、[ **はい** SQL Server を再起動することに同意します。

### SQL Server 認証ログインを作成する

別のコンピューターからデータベース エンジンに接続するには、1 つ以上の SQL Server 認証ログインを作成する必要があります。

1. SQL Server Management Studio のオブジェクト エクスプローラーで、新しいログインを作成するサーバー インスタンスのフォルダーを展開します。

2. 右クリックし、 **セキュリティ** フォルダーを指す **新規**, を選択し、 **ログイン]**します。

    ![新しいログイン](./media/virtual-machines-sql-server-connection-steps/23New-Login.png)

3.  **ログイン - 新規** ダイアログ ボックスの **全般** ] ページで、新しいユーザーの名前を入力、 **ログイン名** ボックス。

4. 選択 **SQL Server 認証**します。

5.  **パスワード** ボックスで、新しいユーザーのパスワードを入力します。 もう一度に、パスワードを入力してください、 **パスワードの確認** ボックス。

6. 強制と複雑さに関するパスワード ポリシー オプションが適用するのには、選択 **パスワード ポリシーを適用** (推奨)。 [SQL Server 認証] が選択されている場合、これは既定のオプションです。

7. が有効期限に関するパスワード ポリシー オプションを適用するのには、選択 **パスワードの期限を適用する** (推奨)。 このチェック ボックスをオンにする場合は、[パスワード ポリシーを適用する] がオンになっている必要があります。 [SQL Server 認証] が選択されている場合、これは既定のオプションです。

8. 最初に使用されるログイン後、新しいパスワードを作成するユーザーには、次のように選択します。 **ユーザーは次回ログイン時にパスワードを変更する必要があります** (このログインが使用する他のユーザー用の場合はお勧めします。 ログインが自分用の場合は、オンにしないでください)。このチェック ボックスをオンにする場合は、[パスワードの期限を適用する] がオンになっている必要があります。 [SQL Server 認証] が選択されている場合、これは既定のオプションです。 

9.  **既定のデータベース** 一覧で、ログインの既定のデータベースを選択します。 **マスター** このオプションの既定値です。 ユーザー データベースを作成がない場合ままにしておきます **マスター**します。

10.  **既定の言語** ボックスの一覧で、そのまま使用 **既定** 値として。
    
    ![ログインのプロパティ](./media/virtual-machines-sql-server-connection-steps/24Test-Login.png)

11. これが初めて作成するログインである場合は、このログインを SQL Server 管理者専用に使用することが考えられます。 その場合、 **サーバーの役割** ] ページで、チェック **sysadmin**します。 

    **セキュリティに関する注意:** sysadmin 固定サーバー ロールのメンバーがデータベース エンジンに対する完全な制御があります。 このロールのメンバーは、適切なユーザーのみに限定してください。

    ![sysadmin](./media/virtual-machines-sql-server-connection-steps/25sysadmin.png)

12. [OK] をクリックします。

SQL Server ログインの詳細については、次を参照してください。 [ログインを作成](http://msdn.microsoft.com/library/aa337562.aspx)します。

### 仮想マシンの DNS 名を特定する

別のコンピューターから SQL Server データベース エンジンに接続するには、仮想マシンのドメイン ネーム システム (DNS) 名が必要になります (これは、仮想マシンを識別するためにインターネットで使用される名前です。 IP アドレスを使用することもできますが、Azure で冗長化またはメンテナンスのためにリソースが移動された場合、IP アドレスは変わる可能性があります。 DNS 名は、新しい IP アドレスにリダイレクトできるため、安定しています)。  

1. Azure 管理ポータルで (または前の手順から)、[ **仮想マシン**します。 

2.  **仮想マシン インスタンス** ] ページで、[、 **[概要** 列、検索、コピー、DNS の仮想マシンの名前します。

    ![DNS name](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)
    

### 別のコンピューターからデータベース エンジンに接続する
 
1. インターネットに接続されたコンピューターで、SQL Server Management Studio を開きます。
2.  **サーバーへの接続** または **データベース エンジンへの接続** ダイアログ ボックスで、**サーバー名** の形式で (前のタスクで決定されます)、仮想マシンとパブリック エンドポイントのポート番号の DNS 名を入力ボックスで、 *DNSName, portnumber* など **tutorialtestVM.cloudapp.net,57500**します。
ポート番号を取得するには、Azure 管理ポータルにログインし、仮想マシンを検索します。 ダッシュ ボードで、をクリックして **エンドポイント** を使用して、 **パブリック ポート** に割り当てられている **MSSQL**します。
    ![パブリック ポート](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3.  **認証** ボックスで、 **SQL Server 認証**します。
5.  **ログイン** ボックスに、前のタスクで作成したログインの名前を入力します。
6.  **パスワード** ボックスで、前のタスクで作成したログインのパスワードを入力します。
7. クリックして **接続**します。

    ![SSMS を使用した接続](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)

