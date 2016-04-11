<properties
   pageTitle="Docker での Web アプリのホスト | Microsoft Azure"
   description="Visual Studio を使用して、Docker コンテナーで Web アプリケーションをホストする方法について説明します。"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/20/2015"
   ms.author="tarcher" />

# Docker での Web アプリのホスト

[Docker](https://www.docker.com/whatisdocker/) は軽量なコンテナー エンジンであり、ホスト アプリケーションおよびサービスに使用できる仮想マシンにいくつかの点で似ています。 Visual Studio は、Ubuntu、CoreOS、および Windows での Docker をサポートしています。

この例では、Visual Studio 2015 Tools for Docker 拡張機能を使用して、ASP.NET 5 Web アプリケーションと共に Docker 拡張機能がインストールされている Azure 上の Ubuntu Linux 仮想マシン (ここでは Docker ホストと呼ぶ) に、ASP.NET 5 アプリケーションを発行する方法を示します。 同じ方法を利用して、Windows コンテナーに発行することができます。

できるアプリを発行する、Azure でホストされる新しい Docker ホスト、または内部設置型サーバー、HYPER-V、または Boot2Docker ホストを使用して、 **カスタム ホスト** 設定します。 Docker ホストへのアプリケーションの発行後、Docker コマンドライン ツールを使用して、アプリケーションの発行先コンテナーと対話できます。

## 新しい Docker コンテナーの作成と発行

以下の手順で、新しい ASP.NET 5 Web アプリケーション プロジェクトを作成し、コンテナー ホストを作成してから、Docker コンテナー内に Web アプリケーション プロジェクトを構築して実行します。 開始し、ダウンロードして、インストール、 [Docker の Visual Studio 2015 Tools](https://aka.ms/DockerToolsForVS)します。

### ASP.NET 5 Web アプリケーション プロジェクトの追加

1. 新しい ASP.NET Web アプリケーション プロジェクトを作成します。 メイン メニューで **ファイル**, 、**新しいプロジェクト**します。  **C#**, 、**Web**, 、選択 **ASP.NET Web アプリケーション**します。

1. 一覧で **ASP.NET 5 プレビュー テンプレート**, 、選択 **Web サイト**します。

1. Web アプリが Docker でホストされている/実行のためのオフ、 **クラウドでホスト** チェック ボックスが選択され、順に選択した場合、 **[ok]** ] ボタンをクリックします。

  ![][0]

  この時点で、一般に Web アプリケーションにコードを追加して、何らかの有益なことを実行しますが、この例では、単に既定の設定のままにしておきます (既存の ASP.NET 5 Web アプリを使用するように選択することもできます)。

### プロジェクトの発行

1. ASP.NET プロジェクトのコンテキスト メニューで、次のように選択します。 **発行**します。

1.  **発行先を選択して** のセクションで、 **Web の発行** ] ダイアログ ボックスで、選択、 **Docker コンテナー** ] ボタンをクリックします。

    [Docker コンテナー] オプションが表示されない場合は、Visual Studio 2015 Tools for Docker をインストールしており、前の手順で、ASP.NET 5 Web サイト テンプレートを選択していることを確認してください。

    ![][1]

     **Docker 仮想マシンの選択** ] ダイアログ ボックスが表示されます。 これにより、プロジェクトを発行する Docker ホストを指定できます。 新しい Docker ホストを作成することも、Azure または他の場所でホストされている既存の VM を選択することもできます。 この例の後で、新しい Azure Docker ホストを作成します。

1. Azure アカウントに既にログインしている場合は、手順 5 にスキップします。 ログイン アカウントにない場合は、選択、 **アカウントを追加する** ] ボタンをクリックします。

    ![][2]

1.  **Visual Studio にサインイン** ] ダイアログ ボックスで、Azure サブスクリプションの電子メール アカウントを入力して、選択、 **続行** ] ボタンをクリックします。

1. 選択、 **新規** ] ボタンをクリックして新しい Azure Docker VM を作成し、 **OK** ] ボタンをクリックします。

    ![][3]

    既存の Docker ホストを使用する選択もできることに注意してください。 これを行うには、選択で、 **既存の Azure Docker 仮想マシン** ドロップダウンを選択するのではなく、リスト、 **新規** ] ボタンをクリックします。 このリストには、コンテナー ホストのみが表示されるのではなく、Azure テナントのすべての VM が一覧表示されます。

    代わりに、カスタム Docker ホストを発行するように選択することもできます。 参照してください **カスタム Docker ホストが提供され** 詳細については、このトピックで後述します。

1. 次の情報を入力して、 **Microsoft Azure で仮想マシンを作成** ] ダイアログ ボックス。 完了したら、選択、 **OK** ] ボタンをクリックします。 これにより、構成済みの Docker 拡張機能で Linux 仮想マシンが作成されます。

    ![][4]

    Windows Server 2016 Technical Preview 3 (TP3) を使用して Windows コンテナー ホストを作成するオプションもあることに注意してください。

    ![][8]

    |プロパティ名|設定|
    |---|---|
    |場所|この設定をロケールに最も近いリージョンに変更します。|
    |DNS 名|仮想マシンの一意の名前を入力します。 名前が Azure によって受け入れられると、右側に白いチェックマークの付いた緑の丸が表示されます。 名前が受け入れられない場合は、白い x 印の付いた赤い丸が表示されます。 その場合は、新しい一意の名前を入力します。|
    |イメージ|Docker ホストで使用する OS イメージがある場合は、それを選択します。 この例では、Ubuntu Server イメージを選択します (使用可能なイメージの一覧で Windows Server イメージが使用できるようになったことに注意してください)。|
    |ユーザー名|仮想マシンの一意のユーザー名を入力します。|
    |パスワード|ユーザーのパスワードを入力し、それを確認します。|
    |証明書ディレクトリ |これは、Docker 証明書を格納するフォルダーを指定します。 新しいフォルダーを作成するか、既存のフォルダーをポイントし、中には、既定の証明書フォルダーを使用することをお勧め (C:\\Users\\ [*username*] \\.docker)。 そうでないと、別のプロジェクトまたはシステムで同じホストを再利用する場合に、認証オプションを自動的に取得できません。|

1. 次に、省略記号 (...) ボタンをクリックして、 **証明書ディレクトリ** 入力し、[Docker 証明書用の新しいフォルダーを作成したり、既存の Docker 証明書フォルダーに移動します。

    VM に必要な Docker 証明書が見つからない場合、エントリの横に感嘆符のアイコンが表示され、必要な証明書が見つからず、続行すると、すべての既存の証明書が削除され、再作成されることが通知されます。

1. 選択、 **OK** VM の作成を開始するボタンをクリックします。 Azure で、仮想マシンが作成されていることを示すメッセージが表示されます。

    Visual Studio によって、Azure リソース マネージャー (ARM) テンプレート ファイル、パラメーター ファイル、および PowerShell スクリプトが作成されるため、後でコマンドを再実行できます。

    ![][7]

    Visual Studio の出力を操作の進行状況、 **出力** ウィンドウです。 Visual Studio は、VM をデプロイする PowerShell スクリプトを呼び出します。 このスクリプトでは、Azure PowerShell コマンドレットを使用して、Azure リソース グループをデプロイします。 後で、ホストを手動で作成した場合とまったく同じように、別の PowerShell スクリプトで、Docker コマンドを使用して発行します。

    Docker ホストのプロビジョニングには、しばらく時間がかかるため、ジョブが完了したら、[出力] ウィンドウでステータスを確認します。

1. Azure で Docker ホストを完全にプロビジョニングしたら、Azure ポータルで自分のアカウントを確認できます。 新しい仮想マシンを表示することができます、 **仮想マシン** Azure ポータルでのカテゴリ。

1. Docker ホストの準備ができたら、戻って、Web アプリケーション プロジェクトを発行します。 Web アプリケーション プロジェクトのノードのコンテキスト メニューで **ソリューション エクスプ ローラー**, 、選択 **発行**します。 Visual Studio では、作成した VM に基づいて、発行ファイルが作成されます。

1.  **接続** ] タブで、 **Web の発行** ] ダイアログ ボックスで、選択、 **接続の検証** Docker ホストが準備ができていることを確認します。 接続が良好な場合、選択、 **発行** web アプリを発行する] ボタンをクリックします。

    初めて Docker ホストにアプリケーションを発行する時間がかかりますが、Docker ファイルで参照される基本イメージのいずれかをダウンロードする (たとえば **FROM** *imagename*)。

    Docker ファイルは、オペレーティング システムに固有であることに注意してください。 別の OS に再発行することを選択した場合は、Visual Studio が対象の OS に基づいて、新しい既定のファイルを作成できるように、Docker ファイルの名前を変更する必要があります。 たとえば、最初に Linux コンテナーに発行し、後で Windows に発行することにした場合、Docker ファイルの名前を DockerLinux などの一意の名前に変更する必要があります。 それにより、Windows に再発行すると、Visual Studio によって、Windows 用の既定の Docker ファイルが再作成されます。 後で、いずれかのファイルを再発行するときに、その OS 用の適切な Docker ファイルを選択するだけです。

## カスタム Docker ホストの指定

前の手順で、Azure でホストされる Docker 仮想マシンを作成しました。 ただし、既にどこかに Docker ホストが存在している場合は、Azure の代わりにそこに発行するように選択できます。

### カスタム Docker ホストを指定する方法

1.  **Docker 仮想マシンの選択** ダイアログ ボックスで、 **カスタム Docker ホスト** チェック ボックスをオンします。

    ![][5]

1. 選択、 **OK** ] ボタンをクリックします。

1.  **Web の発行** ] ダイアログ ボックスで、設定に値を追加、 **CustomDockerHost** セクション: サーバーの URL、イメージ名、Dockerfile の場所、およびホストとコンテナーのポート番号。

     **Docker の詳細オプション** ] セクションで、表示または認証および実行オプションと Docker コマンドラインを変更することができます。

    ![][6]

1. 必要なすべての値を入力したら、後で、 **接続の検証** ] ボタンをクリックして、Docker ホストへの接続が正常に機能します。

1. 接続が機能する場合、ことができます、 **次** パブリッシュされるコンポーネントの一覧を表示するボタンを選択するか、 **発行** すぐにプロジェクトを発行する] ボタンをクリックします。

## Docker ホストのテスト

これでプロジェクトは Azure 上の Docker ホストに発行されたので、その設定を確認してテストできます。 Docker コマンド ライン ツールは Visual Studio 拡張機能と共にインストールされるため、Windows コマンド プロンプトから直接 Docker にコマンドを発行できます。

次の手順は、Azure にデプロイされた Docker ホストと通信するためのものです。

### Docker ホストをテストする方法

1. Windows コマンド プロンプトを開きます。

1. 環境変数に Docker ホストを割り当て、検証します。 そのためには、コマンド プロンプトに次のコマンドを入力します (は Docker ホストの名前に置き換えます *NameofAzureVM*.)

    ```
    Set DOCKER_HOST=tcp://<NameofAzureVM>.cloudapp.net:2376
    Set DOCKER_TLS_VERIFY=1
    ```

    これらのコマンドを呼び出すことで、発行するコマンドごとに `–H (Host) tcp://<NameofAzureVM>.cloudapp.net:2376` と `--TLSVERIFY` を追加する必要がなくなります。

1. ここで、これらのようなコマンドを発行して、Docker ホストが存在し、機能しているかどうかをテストできます。 

    |コマンド ライン|説明|
    |---|---|
    |`docker info`|Docker バージョン情報を取得します。|
    |`docker ps`|実行中のコンテナーの一覧を取得します。|
    |`docker ps –a`|停止しているコンテナーを含むコンテナーの一覧を取得します。|
    |`docker logs <Docker container name>`|指定されたコンテナーのログを取得します。|
    |`docker images`|イメージの一覧を取得します。|

    すべての Docker コマンドの一覧については、コマンド プロンプトで単に `docker` コマンドを入力します。 詳細については、次を参照してください。 [Docker コマンドライン](https://docs.docker.com/reference/commandline/cli/)します。

## 次のステップ

これで Docker ホストが用意できたので、Docker コマンドをそれに対して発行できます。 Docker の詳細については、次を参照してください。、 [Docker のドキュメント](https://docs.docker.com/) と [Docker のオンライン チュートリアル](https://www.docker.com/tryit/)します。

詳細については、Azure の Linux 用 Docker VM 拡張機能を使用して、次を参照してください。 [Azure の Linux 用 Docker 仮想マシンの拡張機能 [](virtual-machines-docker-vm-extension.md)します。

Visual Studio で Docker を使用すると問題については、次を参照してください。 [Windows を使用して Visual Studio での Docker クライアント エラーのトラブルシューティング](vs-azure-tools-docker-troubleshooting-docker-errors.md)します。

[0]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796678.png
[1]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796679.png
[2]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796680.png
[3]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796681.png
[4]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796682.png
[5]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796683.png
[6]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796684.png
[7]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796685.png
[8]: ./media/vs-azure-tools-docker-hosting-web-apps-in-docker/IC796686.png

