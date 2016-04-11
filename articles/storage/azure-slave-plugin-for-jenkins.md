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

    </PublishData>

サブスクリプション プロファイルを用意したら、次の手順に従って Azure スレーブ プラグインを構成します。

1. Jenkins ダッシュ ボードで、次のようにクリックします。 **Manage Jenkins**します。
2. クリックして **システムを構成する**です。
3. ページにアクセスして、下にスクロールして、 **クラウド** セクションです。
4. クリックして **新規のクラウドを追加 > Microsoft Azure**します。

    ![クラウド セクション](./media/azure-slave-plugin-for-jenkins/jenkins-cloud-section.png)

    サブスクリプションの詳細を入力する必要のあるフィールドが表示されます。

    ![サブスクリプションの構成](./media/azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png)

5. サブスクリプション プロファイルからサブスクリプションID と管理証明書の値をコピーし、適切なフィールドに貼り付けます。

    サブスクリプション ID と管理証明書をコピーするときは、値を囲む引用符は含めないでください。

6. クリックして **構成を確認する**です。
7. 構成が正しいことを確認] をクリックして **保存**します。

## Azure スレーブ プラグイン用の仮想マシン テンプレートを設定するには

仮想マシン テンプレートでは、プラグインが Azure でスレーブ ノードを作成するために使用するパラメーターを定義します。 次の手順では、Ubuntu 仮想マシン用のテンプレートを作成します。

1. Jenkins ダッシュ ボードで、次のようにクリックします。 **Manage Jenkins**します。
2. クリックして **システムを構成する**です。
3. ページにアクセスして、下にスクロールして、 **クラウド** セクションです。

4.  **クラウド** セクションで、検索 **Add Azure Virtual Machine Template**, 、] をクリックし、 **追加**します。

    ![VM テンプレートの追加](./media/azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png)

    作成するテンプレートの詳細を入力するフィールドが表示されます。

    ![空白の一般的な構成](./media/azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png)

5.  **名前** ボックスで、Azure クラウド サービス名を入力します。 入力した名前が既存のクラウド サービスを指す場合、仮想マシンはそのサービス内にプロビジョニングされます。 それ以外の場合、Azure は新しいものを作成します。

6.  **説明** ボックスを作成するテンプレートを説明するテキストを入力します。 これは記録のみを目的としており、仮想マシンのプロビジョニングでは使用されません。
7.  **ラベル** ボックスを作成するテンプレートを識別するために使用され、は、Jenkins ジョブを作成するときに、テンプレートを参照に使用します。 この手順では、入力 **linux** このボックスにします。
8.  **地域** 一覧で、仮想マシンを作成するリージョンをクリックします。
9.  **仮想マシンのサイズ** 一覧で、適切なサイズをクリックします。
10.  **ストレージ アカウント名** ボックスで、仮想マシンを作成するストレージ アカウントを指定します。 使用するクラウド サービスと同じリージョン内にあることを確認します。 新しいストレージを作成する場合は、このボックスを空白のままにすることができます。
11. [Retention time] は、その時間を経過するとJenkins がアイドル状態のスレーブを削除する分数を指定します。 これは、既定値 60 のままにします。 スレーブは、アイドル状態になったときに削除するのではなくシャットダウンすることもできます。 実行するには、選択、 **Shutdown Only (Do しない Delete) After Retention Time** チェック ボックスをオンします。
12.  **使用** 一覧で、このスレーブ ノードを使用する場合は、適切な条件をクリックします。 ここでは、クリックして **利用可能な限りこのノード**します。

    この時点で、フォームは次のようになります。

    ![一般的なテンプレート構成のチェックポイント](./media/azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png)

    次の手順では、スレーブが作成されるオペレーティング システム イメージの詳細を指定します。

13.  **Image Family or Id** ボックスで、仮想マシンにインストールされるシステム イメージを指定する必要があります。 イメージ ファミリの一覧から選択するか、カスタム イメージを指定できます。

    イメージ ファミリの一覧から選択する場合は、イメージ ファミリ名の最初の文字を (大文字と小文字を区別して) 入力します。 たとえば、入力 **U** Ubuntu Server ファミリの一覧が表示されます。 一覧から選択すると、Jenkins は仮想マシンをプロビジョニングするときにそのファミリの最新バージョンのシステム イメージを使用します。

    ![OS イメージの一覧の例](./media/azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png)

    代わりに使用するカスタム イメージがある場合は、そのカスタム イメージの名前を入力します。 カスタム イメージの名前は一覧には表示されないため、名前が正しく入力されていることを確認する必要があります。

    このチュートリアルでは、次のように入力します。 **U** Ubuntu イメージの一覧を表示し、[] をクリックし、 **Ubuntu Server 14.04 LTS**します。

14.  **メソッドの起動** 一覧で、クリックして **SSH**します。
15. 次のスクリプトをコピーして貼り付けることで、 **Init スクリプト** ボックス。

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

        sudo apt-get install -y ant

    init スクリプトは、仮想マシンが作成された後で実行されます。 この例では、このスクリプトによって、Java、Git、および ant がインストールされます。

16.  **Username** と **パスワード** ボックスに、仮想マシンで作成される管理者アカウントの優先値を入力します。
17. クリックして **テンプレートのことを確認** に指定したパラメーターが有効なことを確認します。
18. クリックして **保存**します。


## Azure のスレーブ ノードで実行される Jenkins ジョブを作成するには

このセクションでは、Azure のスレーブ ノードで実行される Jenkins タスクを作成します。 このためには、独自のプロジェクトを GitHub で起動する必要があります。

1. Jenkins ダッシュ ボードで、次のようにクリックします。 **[新しい項目の**です。
2. 作成するタスクの名前を入力します。
3. プロジェクトの種類をクリックして **Freestyle project**します。
4. クリックして **Ok**します。
5. タスクの構成] ページで選択 **このプロジェクトを実行できる場所だけに制限する**です。
6.  **ラベル式** ボックスに、入力 **linux**です。 前のセクションでという名前を付けて、スレーブ テンプレートを作成した **linux**, 、これは、指定していること。
7.  **ビルド** ] をクリックして **ビルド ステップの追加** 選択 **シェルの実行**します。
8. 次の編集スクリプトには、置換 **(GitHub アカウント名)**, 、**(プロジェクト名)**, と **(プロジェクト ディレクトリ)** の値を適切なし、表示されるテキスト領域で、編集後のスクリプトを貼り付けます。

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



        #Execute build task

        ant

9. クリックして **保存**します。
10. Jenkins ダッシュボードで、作成したばかりのタスクの上にマウスを移動し、ドロップダウン矢印をクリックしてタスク オプションを表示します。
11. クリックして **を今すぐビルド**します。

Jenkins は、前のセクションで作成したテンプレートを使用してスレーブ ノードを作成し、このタスク用のビルド手順で指定されたスクリプトを実行します。


