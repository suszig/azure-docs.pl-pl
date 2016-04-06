<properties
    pageTitle="Azure スレーブ プラグインを Hudson  継続的インテグレーションで使用する方法"
    description="Azure スレーブ プラグインを Hudson 継続的インテグレーションで使用する方法について説明します。"
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

#Azure スレーブ プラグインを Hudson  継続的インテグレーションで使用する方法

Hudson 用の Azure スレーブ プラグインを使用して、分散されたビルドを実行するときにスレーブ ノードを Azure にプロビジョニングできます。

## Azure スレーブ プラグインをインストールする
1. Hudson ダッシュ ボード] をクリックして **Manage Hudson**します。
2.  **Manage Hudson** ] ページで、をクリックして **Manage Plugins**します。
3. クリックして、 **利用可能な** ] タブをクリックします。
4. をクリックして **検索** と種類 **Azure** を関連するプラグインの一覧に制限します。

    使用可能なプラグインの一覧をスクロールすることを選択する場合は、Azure スレーブ プラグインが表示されます、 **Cluster Management and Distributed Build** セクション、 **他** ] タブをクリックします。
     
5. チェック ボックスをオン **Azure Slave Plugin**します。
6. クリックして **インストール**します。
7. Hudson を再起動します。

これでプラグインがインストールされたため、次の手順は Azure サブスクリプション プロファイルを使用してプラグインを構成し、スレーブ ノード用の VM の作成で使用されるテンプレートを作成することになります。


## Azure スレーブ プラグインをサブスクリプション プロファイルを使用して構成する

サブスクリプション プロファイル (発行設定) は、セキュリティで保護された資格情報と、開発環境で Azure を操作するために必要な追加情報を含む XML ファイルです。 Azure スレーブ プラグインを構成するには、以下が必要です。 

* サブスクリプション ID
* サブスクリプション用の管理証明書

これらは、サブスクリプション プロファイルで確認できます。 サブスクリプション プロファイルのコピーを取得していない場合をからダウンロードできます [ここ](https://manage.windowsazure.com/publishsettings/Index?SchemaVersion=2.0)します。 サブスクリプション プロファイルの例を以下に示します。

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

サブスクリプション プロファイルを用意したら、次の手順に従って Azure スレーブ プラグインを構成します。

1. Hudson ダッシュ ボード] をクリックして **Manage Hudson**します。
2. クリックして **システムを構成する**です。
3. ページにアクセスして、下にスクロールして、 **クラウド** セクションです。
4. クリックして **新規のクラウドを追加 > Microsoft Azure**します。



    ![add new cloud](./media/azure-slave-plugin-for-hudson/hudson-setup-addcloud.png)

    This will show the fields where you need to enter your subscription details.

    ![configure profile](./media/azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png)

5. サブスクリプション プロファイルからサブスクリプションID と管理証明書をコピーし、適切なフィールドに貼り付けます。

    サブスクリプション id と管理証明書をコピーするときに **しない** 値を囲む引用符を含めます。
    
6. をクリックして **Verify configuration**します。
7. 構成が正常に確認したら、クリックして **保存**します。

## Azure スレーブ プラグイン用の仮想マシン テンプレートを設定する

仮想マシン テンプレートは、プラグインが Azure でスレーブ ノードを作成するために使用するパラメーターを定義します。 次の手順では、Ubuntu VM 用のテンプレートを作成します。 

1. Hudson ダッシュ ボード] をクリックして **Manage Hudson**します。
2. をクリックして **システムを構成する**です。
3. ページにアクセスして、下にスクロールして、 **クラウド** セクションです。
4. 内で、 **クラウド** セクションで、検索 **Add Azure Virtual Machine Template** ] をクリックし、 **追加** ] ボタンをクリックします。



    ![add vm template](./media/azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png)

5. クラウド サービス名の指定、 **名前** フィールドです。 指定した名前が既存のクラウド サービスを指す場合、VM はそのサービス内にプロビジョニングされます。 それ以外の場合、Azure は新しいものを作成します。 
6.  **説明** フィールドで、作成するテンプレートを説明するテキストを入力します。 この情報は説明用としてのみ使用され、VM のプロビジョニングでは使用されません。
7.  **ラベル** フィールドに「 **linux**です。 このラベルは作成中のテンプレートを識別するために使用され、以降は Hudson ジョブを作成するときにテンプレートを参照するために使用されます。 
8. VM が作成されるリージョンを選択します。
9. 適切な VM サイズを選択します。
10. VM が作成されるストレージ アカウントを指定します。 使用するクラウド サービスと同じリージョン内にあることを確認します。 新しいストレージを作成する場合は、このフィールドを空白のままにすることができます。
11. [Retention time] は、その時間を経過するとHudson がアイドル状態のスレーブを削除する分数を指定します。 これは、既定値 60 のままにします。
12.  **使用**, 、このスレーブ ノードを使用する場合は、適切な条件を選択します。 ここでは、次のように選択します。 **利用可能な限りこのノード**します。



    At this point, your form would look somewhat similar to this:

    ![template config](./media/azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png)

13.  **Image Family or Id** 、VM にインストールされるシステム イメージを指定する必要があります。 イメージ ファミリの一覧から選択するか、カスタム イメージを指定できます。

    イメージ ファミリの一覧から選択する場合は、イメージ ファミリ名の最初の文字を (大文字と小文字を区別して) 入力します。 たとえば、入力 **U** Ubuntu Server ファミリの一覧が表示されます。 一覧から選択すると、Jenkins は VM をプロビジョニングするときにそのファミリの最新バージョンのシステム イメージを使用します。

    ![OS ファミリの一覧](./media/azure-slave-plugin-for-hudson/hudson-oslist.png)
    
    代わりに使用するカスタム イメージがある場合は、そのカスタム イメージの名前を入力します。 カスタム イメージの名前は一覧には表示されないため、名前が正しく入力されていることを確認する必要があります。    

    このチュートリアルでは、次のように入力します。 **U** Ubuntu イメージの一覧を表示し、選択 **Ubuntu Server 14.04 LTS**します。
 
14.  **Launch method**, [ **SSH**します。
15. 次のスクリプトをコピーして貼り付け、 **Init スクリプト** フィールドです。

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

     **Init スクリプト** 、VM が作成された後に実行します。 この例では、スクリプトは、Java、git、および ant をインストールします。
    
16.  **Username** と **パスワード** フィールドに、VM 上に作成される管理者アカウントの優先値を入力します。
17. をクリックして **テンプレートのことを確認** に指定したパラメーターが有効なことを確認します。
18. をクリックして **保存**します。


## Azure のスレーブ ノードで実行される Hudson ジョブを作成する

このセクションでは、Azure のスレーブ ノードで実行される Hudson タスクを作成します。 

1. Hudson ダッシュ ボード] をクリックして **新しいジョブ**します。 
2. 作成するジョブの名前を入力します。
3. ジョブの種類を選択 **Build 空きスタイル ソフトウェア job**します。
4. クリックして **Ok**します。
5. ジョブの構成] ページで次のように選択します。 **このプロジェクトを実行できる場所だけに制限する**です。
6. 選択 **ノードとラベルのメニュー** 選択 **linux** (ラベルは指定したこの前のセクションで、バーチャル マシン テンプレートを作成する場合)。 

7.  **ビルド** ] をクリックして **ビルド ステップの追加** 選択 **シェルの実行**します。
8. 次の編集スクリプトには、置換 **(github アカウント名)**, 、**(プロジェクト名)**, と **(プロジェクト ディレクトリ)** の値を適切なし、表示されるテキスト領域で、編集後のスクリプトを貼り付けます。


        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your github account name)/(your project name).git

        fi
        
        # change directory to project

        cd $currentDir/(your project directory)



        #Execute build task

        ant
        
9. をクリックして **保存**します。
10. Hudson ダッシュ ボードで、作成したジョブを検索し、をクリックして、 **ビルドをスケジュール** アイコン。 

Hudson は、前のセクションで作成したテンプレートを使用してスレーブ ノードを作成し、このタスク用のビルド手順に指定されたスクリプトを実行します。






  

  

