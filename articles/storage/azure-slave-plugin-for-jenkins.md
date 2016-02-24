<properties
    pageTitle="Azure スレーブ プラグインを Jenkins 継続的インテグレーションで使用する方法 | Microsoft Azure"
    description="Azure スレーブ プラグインを Jenkins 継続的インテグレーションで使用する方法について説明します。"
    services="storage"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="11/19/2015"
    ms.author="v-dedomi"/>

# Azure スレーブ プラグインを Jenkins 継続的インテグレーションで使用する方法

Jenkins 用の Azure スレーブ プラグインを使用して、分散されたビルドを実行するときにスレーブ ノードを Azure にプロビジョニングできます。

## Azure スレーブ プラグインをインストールするには
1. Jenkins ダッシュ ボードで、次のようにクリックします。 **Manage Jenkins**します。
2.  **Manage Jenkins** ] ページで [ **Manage Plugins**します。
3. クリックして、 **利用可能な** ] タブをクリックします。
4. 利用可能なプラグインの一覧の上の [フィルター] フィールドに入力 **Azure** に関連するプラグインの一覧に制限します。

    利用可能なプラグインの一覧をスクロールすることを選択することがわかります Azure スレーブ プラグインで、 **Cluster Management and Distributed Build** セクションです。

5. 選択、 **Azure Slave Plugin** チェック ボックスをオンします。
6. クリックして **を再起動しなくてもインストール** または **を今すぐダウンロードして、再起動後インストール**します。

これでプラグインがインストールされたため、次の手順では、Azure サブスクリプション プロファイルを使用してプラグインを構成し、スレーブ ノード用の仮想マシンの作成で使用するテンプレートを作成します。


## サブスクリプション プロファイルを使用して Azure スレーブ プラグインを構成するには

サブスクリプション プロファイル (発行設定) は、セキュリティで保護された資格情報と、開発環境で Azure を操作するために必要な追加情報を含む XML ファイルです。 Azure スレーブ プラグインを構成するには、以下が必要です。

* サブスクリプション ID
* サブスクリプション用の管理証明書

これらは、サブスクリプション プロファイルで確認できます。 サブスクリプション プロファイルのコピーを取得していない場合をからダウンロードできます [サブスクリプション サイト](https://manage.windowsazure.com/publishsettings/Index?SchemaVersion=2.0)します。 サブスクリプション プロファイルの例を以下に示します。

    <?xml version="1.0" encoding="utf-8"?>
        <PublishData>
        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">
        <Subscription
            ServiceManagementUrl="https://management.core.windows.net"
            Id="<Subscription ID value>"
            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />
        </PublishProfile>
    </Publisサブスクリプション プロファイルを用意したら、次の手順に従って Azure スレーブ プラグインを構成します。plug-Jenkins ダッシュ ボードで、次のようにクリックします。 **Manage Jenkins**します。nkinクリックして **システムを構成する**です。ysteページにアクセスして、下にスクロールして、 **クラウド** セクションです。sectクリックして **新規のクラウドを追加 > Microsoft Azure**します。Azure**.クラウド セクション section](./media/azure-slave-plugin-for-jenkins/jenkins-cloud-section.pnサブスクリプションの詳細を入力する必要のあるフィールドが表示されます。details.サブスクリプションの構成guration](./media/azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.pサブスクリプション プロファイルからサブスクリプションID と管理証明書の値をコピーし、適切なフィールドに貼り付けます。 fieldサブスクリプション ID と管理証明書をコピーするときは、値を囲む引用符は含めないでください。 valuクリックして **構成を確認する**です。atio構成が正しいことを確認] をクリックして **保存**します。*SaveAzure スレーブ プラグイン用の仮想マシン テンプレートを設定するには p仮想マシン テンプレートでは、プラグインが Azure でスレーブ ノードを作成するために使用するパラメーターを定義します。n次の手順では、Ubuntu 仮想マシン用のテンプレートを作成します。machiJenkins ダッシュ ボードで、次のようにクリックします。 **Manage Jenkins**します。nkinクリックして **システムを構成する**です。ysteページにアクセスして、下にスクロールして、 **クラウド** セクションです。sect **クラウド** セクションで、検索 **Add Azure Virtual Machine Template**, 、] をクリックし、 **追加**します。 **Add**VM テンプレートの追加 template](./media/azure-slave-plugin-for-jenkins/jenkins-add-vm-template.p作成するテンプレートの詳細を入力するフィールドが表示されます。creating空白の一般的な構成iguration](./media/azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank. **名前** ボックスで、Azure クラウド サービス名を入力します。i入力した名前が既存のクラウド サービスを指す場合、仮想マシンはそのサービス内にプロビジョニングされます。 それ以外の場合、Azure は新しいものを作成します。 new **説明** ボックスを作成するテンプレートを説明するテキストを入力します。 これは記録のみを目的としており、仮想マシンのプロビジョニングでは使用されません。l ma **ラベル** ボックスを作成するテンプレートを識別するために使用され、は、Jenkins ジョブを作成するときに、テンプレートを参照に使用します。nこの手順では、入力 **linux** このボックスにします。 thi **地域** 一覧で、仮想マシンを作成するリージョンをクリックします。e cr **仮想マシンのサイズ** 一覧で、適切なサイズをクリックします。iate  **ストレージ アカウント名** ボックスで、仮想マシンを作成するストレージ アカウントを指定します。e使用するクラウド サービスと同じリージョン内にあることを確認します。 新しいストレージを作成する場合は、このボックスを空白のままにすることができます。box b[Retention time] は、その時間を経過するとJenkins がアイドル状態のスレーブを削除する分数を指定します。dこれは、既定値 60 のままにします。lスレーブは、アイドル状態になったときに削除するのではなくシャットダウンすることもできます。i実行するには、選択、 **Shutdown Only (Do しない Delete) After Retention Time** チェック ボックスをオンします。check **使用** 一覧で、このスレーブ ノードを使用する場合は、適切な条件をクリックします。lここでは、クリックして **利用可能な限りこのノード**します。ossiblこの時点で、フォームは次のようになります。r to thi一般的なテンプレート構成のチェックポイントate config](./media/azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.次の手順では、スレーブが作成されるオペレーティング システム イメージの詳細を指定します。reated **Image Family or Id** ボックスで、仮想マシンにインストールされるシステム イメージを指定する必要があります。lイメージ ファミリの一覧から選択するか、カスタム イメージを指定できます。tom imイメージ ファミリの一覧から選択する場合は、イメージ ファミリ名の最初の文字を (大文字と小文字を区別して) 入力します。mたとえば、入力 **U** Ubuntu Server ファミリの一覧が表示されます。 一覧から選択すると、Jenkins は仮想マシンをプロビジョニングするときにそのファミリの最新バージョンのシステム イメージを使用します。l machinOS イメージの一覧の例ist sample](./media/azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.代わりに使用するカスタム イメージがある場合は、そのカスタム イメージの名前を入力します。tカスタム イメージの名前は一覧には表示されないため、名前が正しく入力されていることを確認する必要があります。correcこのチュートリアルでは、次のように入力します。 **U** Ubuntu イメージの一覧を表示し、[] をクリックし、 **Ubuntu Server 14.04 LTS**します。.04 LT **メソッドの起動** 一覧で、クリックして **SSH**します。k **S次のスクリプトをコピーして貼り付けることで、 **Init スクリプト** ボックス。ipt** box.

        # Install Java
        sudo apt-get -y update
        sudo apt-get install -y openjdk-7-jdk
        sudo apt-get -y update --fix-missing
        sudo apt-get install -y openjdk-7-jdk

        # Install git
        sudo apt-get install -y git

        #Install ant
        sudo apt-get install -y ant
        sudo apt-get -y update --fix-missing
        sudo apt-getinit スクリプトは、仮想マシンが作成された後で実行されます。lこの例では、このスクリプトによって、Java、Git、および ant がインストールされます。s Java **Username** と **パスワード** ボックスに、仮想マシンで作成される管理者アカウントの優先値を入力します。your クリックして **テンプレートのことを確認** に指定したパラメーターが有効なことを確認します。 specクリックして **保存**します。.
18. Azure のスレーブ ノードで実行される Jenkins ジョブを作成するにはa このセクションでは、Azure のスレーブ ノードで実行される Jenkins タスクを作成します。 このためには、独自のプロジェクトを GitHub で起動する必要があります。tHub Jenkins ダッシュ ボードで、次のようにクリックします。 **[新しい項目の**です。, cl作成するタスクの名前を入力します。ask プロジェクトの種類をクリックして **Freestyle project**します。*Freクリックして **Ok**します。t**.タスクの構成] ページで選択 **このプロジェクトを実行できる場所だけに制限する**です。proj **ラベル式** ボックスに、入力 **linux**です。b前のセクションでという名前を付けて、スレーブ テンプレートを作成した **linux**, 、これは、指定していること。e're **ビルド** ] をクリックして **ビルド ステップの追加** 選択 **シェルの実行**します。ct *次の編集スクリプトには、置換 **(GitHub アカウント名)**, 、**(プロジェクト名)**, と **(プロジェクト ディレクトリ)** の値を適切なし、表示されるテキスト領域で、編集後のスクリプトを貼り付けます。xt area that appears.

        # Clone from git repo
        currentDir="$PWD"
        if [ -e (your project directory) ]; then
            cd (your project directory)
            git pull origin master
        else
            git clone https://github.com/(your GitHub account name)/(your project name).git
        fi

        # change directory to project
        cd $currentDir/(your project directory)

        #Execクリックして **保存**します。
    Jenkins ダッシュボードで、作成したばかりのタスクの上にマウスを移動し、ドロップダウン矢印をクリックしてタスク オプションを表示します。n arrクリックして **を今すぐビルド**します。ptJenkins は、前のセクションで作成したテンプレートを使用してスレーブ ノードを作成し、このタスク用のビルド手順で指定されたスクリプトを実行します。in the build step for this task.

