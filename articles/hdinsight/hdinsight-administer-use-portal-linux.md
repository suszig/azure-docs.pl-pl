<properties
    pageTitle="Azure ポータルを使用した HDInsight での Linux ベースの Hadoop クラスターの管理 | Microsoft Azure"
    description="Azure ポータルを使用して Linux ベースの HDInsight クラスターを作成および管理する方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/19/2015"
    ms.author="larryfr"/>

# Azure ポータルを使用した HDInsight での Hadoop クラスターの管理

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]


使用して、 [Azure ポータル][preview-portal], 、プロビジョニングや、Azure HDInsight での Linux ベースの Hadoop クラスターを管理します。

> [AZURE.NOTE] このドキュメントの手順では、Linux ベースの Hadoop クラスターの操作に固有です。 Windows ベースのクラスターを扱う方法については、次を参照してください [Azure ポータルを使用して HDInsight で Hadoop の管理のクラスター。](hdinsight-administer-use-management-portal.md)


[AZURE.INCLUDE [preview-portal](../../includes/hdinsight-azure-preview-portal-nolink.md)]


## HDInsight を管理するためのその他のツール
Azure ポータル以外にも、HDInsight を管理するツールが用意されています。

- [Azure CLI を使用して HDInsight を管理](hdinsight-administer-use-command-line.md): Azure CLI は、Azure サービスを管理できるクロスプラット フォーム コマンド ライン ツール

- [Azure PowerShell を使用して HDInsight を管理](hdinsight-administer-use-powershell.md): Azure PowerShell の Azure サービスを管理するための PowerShell コマンドレットを使用します。

## 前提条件

この記事を読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

## HDInsight クラスターのプロビジョニング

以下の手順を使用して、Azure ポータルから HDInsight クラスターをプロビジョニングできます。

1. サインイン、 [Azure ポータル][preview-portal]します。

2. 選択 **新規**, [ __データ分析__, 、し、[ __HDInsight__

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-administer-use-portal-linux/new-cluster.png)

3. 入力、 __クラスター名__, 選択してから、 __クラスターの種類__ を作成します。 横に緑のチェック マークが表示されます、 __クラスター名__ 表示されている場合。

    ![クラスター名、クラスターの種類、および OS の種類](./media/hdinsight-administer-use-portal-linux/clustername.png)

4. 複数のサブスクリプションの場合は選択し、 __サブスクリプション__ エントリをクラスターに使用される Azure サブスクリプションを選択します。

5.  __リソース グループ__, 、既存のリソース グループの一覧を表示し、[でクラスターを作成する 1 つにエントリを選択することができます。 選択または __新規作成__ し、新しいリソース グループの名前を入力します。 新しいグループ名を使用できる場合は、緑のチェック マークが表示されます。

    > [AZURE.NOTE] このエントリは、いずれかが使用可能な場合、既存のリソース グループのいずれかに設定されます。

6. 選択 __資格情報__, を入力し、 __クラスター ログインのパスワード__ の __クラスター ログイン ユーザー名__します。 入力する必要があります、 __SSH ユーザー名__ いずれかと、 __パスワード__ または __公開キー__, 、SSH ユーザーの認証に使用されます。 最後に、使用して、 __選択__ 資格情報を設定] ボタンをクリックします。 このドキュメントではリモート デスクトップは使用しないため、無効にしておくことができます。

    ![[クラスターの資格情報] ブレード](./media/hdinsight-administer-use-portal-linux/clustercredentials.png)
    
    > [AZURE.NOTE] SSH を使用して、コマンドラインを使用して HDInsight クラスターをリモートでアクセスします。 ここで使用するユーザー名とパスワードまたは公開キーは、SSH でクラスターに接続するときに使用されます。

    HDInsight での SSH の使用方法の詳細については、次の記事をご覧ください。

    * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

7.  __データソース__, 、既存のデータ ソースを選択するエントリを選択したり、新しいものを作成します。

    ![[データ ソース] ブレード](./media/hdinsight-administer-use-portal-linux/datasource.png)

    現在、HDInsight クラスターのデータ ソースとして Azure Storage アカウントを選択できます。 エントリを理解するために、次を使用して、 __データソース__ ブレードです。

    - __選択方法__: これを設定 __すべてのサブスクリプションから__ 、サブスクリプションのストレージ アカウントの参照を有効にします。 設定 __アクセス キー__ を入力する場合、 __ストレージ名__ と __アクセス キー__ の既存のストレージ アカウント。

    - __新規作成__: 新しいストレージ アカウントの作成に使用します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。

    - __既定のコンテナーを選択して__: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、コンテナーが特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。

    - __場所__: ストレージ アカウントとなる地理的領域には、またはに作成されます。

        > [AZURE.IMPORTANT] 既定のデータ ソースの場所を選択すると、HDInsight クラスターの場所も設定されます。 クラスターと既定のデータ ソースは、同じリージョンに存在する必要があります。

    - __選択__: これを使用してデータ ソースの構成を保存します。

    
8. 選択 __ノード料金レベル__ このクラスター用に作成するノードに関する情報を表示します。 既定では、ワーカー ノードの数設定されます __4__します。 


    The estimated cost of the cluster will be shown at the bottom of this blade.

    ![Node pricing tiers blade](./media/hdinsight-administer-use-portal-linux/nodepricingtiers.png)

    Use the __Select__ button to save the __Node Pricing Tiers__ information.

9. 選択 __オプションの構成__します。 このブレードでは、以下の項目を構成できます。

    * __HDInsight Version__: HDInsight クラスターの使用のバージョン。 HDInsight のバージョンの詳細については、次を参照してください [HDInsight コンポーネントのバージョン。](hdinsight-component-versioning.md)

    * __外部メタストア__: Oozie および Hive の構成情報を格納するために使用する SQL データベースを選択できます。 これにより、クラスターを削除して再作成するときに構成を再利用できるので、Hive と Oozie の構成をその都度作成し直す必要がなくなります。

    * __仮想ネットワーク__: SQL Database、Azure の仮想マシンなどその他のリソースと同じ仮想ネットワークに HDInsight クラスターを配置できます。 仮想ネットワークにリソースを配置することにより、インターネットからの受信トラフィックを処理するパブリック ゲートウェイをバイパスして、リソースが相互に直接通信できるようになります。 Azure の仮想ネットワークからの HDInsight のメリットの詳細については、次を参照してください。 [Azure 仮想ネットワークを使用して HDInsight の拡張機能](hdinsight-extend-hadoop-virtual-network.md)です。

        > [AZURE.IMPORTANT] HDInsight 構成から新しいネットワークを作成することはできません、HDInsight クラスターを作成する前に Azure 仮想ネットワークを作成する必要があります。
        >
        > 現時点 (2015 年 8 月 25 日) では、Azure Virtual Network 上に存在できる Linux ベースの HDInsight クラスターは 1 つだけという制限があります。 
        >
        > Linux ベースの HDInsight で v1 (クラシック) Azure Virtual Network を使用することはできません。 Azure ポータルでの HDInsight クラスターの作成プロセスで Virtual Network をオプションとして表示したり、Azure CLI または Azure PowerShell からクラスターを作成するときに Virtual Network を使用できるようにしたりするには、Virtual Network が v2 (Azure リソース マネージャー) である必要があります。
        >
        > V1 ネットワーク上のリソースをしたし、HDInsight を仮想ネットワークを介してそれらのリソースに直接アクセスできるように、表示する [従来の Vnet を新しい Vnet に接続する](../virtual-network/virtual-networks-arm-asm-s2s.md) v2 の仮想ネットワークを v1 の仮想ネットワークに接続する方法の詳細。 この接続が確立されると、v2 Virtual Network で HDInsight クラスターを作成できます。

    * __アクションのスクリプトを作成__: これにより、プロビジョニング中に、HDInsight クラスターをカスタマイズする Bash スクリプトを指定します。 たとえばは、 [色合いをインストールするスクリプト](hdinsight-hadoop-hue-linux.md) (グラフィカルなクライアントと Hadoop を操作するためです)。スクリプト アクションの詳細については、次を参照してください。 [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster-linux.md)します。

    * __Azure のストレージ キー__: これにより、HDInsight サーバーに追加のストレージ アカウントを関連付けます。

        > [AZURE.NOTE] HDInsight は、この構成セクションを使用して追加、既定のデータ ストアとして使用する Azure ストレージ アカウントにのみアクセス可能またはパブリックにアクセスします。

    ![[オプションの構成] ブレード](./media/hdinsight-administer-use-portal-linux/optionalconfiguration.png)

10. いることを確認 __スタート画面にピン__ が選択されて、[ __作成__します。 これでクラスターが作成され、Azure ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターがプロビジョニング中であることを示し、プロビジョニングが完了すると、[HDInsight] アイコンを表示するように変化します。

    | プロビジョニング中 | プロビジョニング完了 |
    | ------------------ | --------------------- |
    | ![スタート画面のプロビジョニング中インジケーター](./media/hdinsight-administer-use-portal-linux/provisioning.png) | ![プロビジョニングされたクラスターのタイル](./media/hdinsight-administer-use-portal-linux/provisioned.png) |

    > [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または __通知__ プロビジョニング プロセスをチェックするページの左にあるエントリです。

## クラスターの管理

Azure ポータルでクラスターを選択すると、名前、リソース グループ、オペレーティング システム、クラスターのダッシュボードの URL (Linux クラスターの Ambari Web へのアクセスに使用) など、クラスターに関する重要な情報が表示されます。

![クラスターの詳細](./media/hdinsight-administer-use-portal-linux/clusterdetails.png)

このブレードの上部にあるアイコンを理解して、次を使用して、 __Essentials__ と __クイック リンク__ セクション。

* __設定__ と __すべての設定__: 表示、 __設定__ ブレードで、クラスターのクラスターの詳細な構成情報にアクセスすることができます。

* __ダッシュ ボード__, 、__クラスターのダッシュ ボード__, 、および __URL__: これらは、Linux ベースのクラスターの Ambari Web は、クラスターのダッシュ ボードにアクセスするすべての方法です。

* __Secure Shell__: SSH を使用してクラスターにアクセスするために必要な情報です。

* __クラスターのスケール__: このクラスターのワーカー ノードの数を変更することができます。

* __削除__: HDInsight クラスターを削除します。

* __クイック スタート (![クラウドとサンダー ボルトのアイコンのクイック スタートを =](./media/hdinsight-administer-use-portal-linux/quickstart.png))__: のに役立つ情報を表示 HDInsight の概要です。

* __ユーザー (![ユーザー アイコン](./media/hdinsight-administer-use-portal-linux/users.png))__: アクセス許可を設定することができます _ポータル管理_ 、Azure サブスクリプションに他のユーザーは、このクラスターのです。

    > [AZURE.IMPORTANT] これは、 _のみ_ アクセスと Azure ポータルでは、このクラスターへのアクセス許可に影響しへの接続または HDInsight クラスターにジョブを送信できるユーザーに影響を与えません。

* __タグ (![タグ アイコン](./media/hdinsight-administer-use-portal-linux/tags.png))__: タグでは、クラウド サービスのカスタム分類を定義するキー/値ペアを設定することができます。 たとえば、という名前のキーを作成することがあります __プロジェクト__, 、し、特定のプロジェクトに関連付けられているすべてのサービスの一般的な値を使用します。

* __ドキュメント__: Azure HDInsight のドキュメントにリンクします。

> [AZURE.IMPORTANT] HDInsight クラスターで提供されるサービスを管理するには、Ambari Web または Ambari REST API を使用する必要があります。 Ambari の使用に関する詳細については、次を参照してください。 [Ambari を使用した HDInsight の管理のクラスター](hdinsight-hadoop-manage-ambari.md)します。

### <a name="scaling"></a>スケーリング

ポータルを使用してクラスターを拡張するため、HDInsight クラスターを選択し、[ __スケール クラスター__します。 入力、 __ワーカー ノード数__ クラスターの場合、設定し、クリックする __保存__します。

![スケーリング UI の画像](./media/hdinsight-administer-use-portal-linux/scaling.png)

スケーリング操作の詳細については、次を参照してください。 [Linux で HDInsight を使用する方法については](hdinsight-hadoop-linux-information.md#scaling)です。

## クラスターの監視

 __使用__ HDInsight クラスターのブレードのセクションでは、このクラスター内のノードに割り当てられているどのようにこのクラスターに割り当てられているコアの数だけでなく、サブスクリプションで HDInsight を使用するために使用できるコアの数に関する情報を表示します。

![使用状況に関する情報](./media/hdinsight-administer-use-portal-linux/usage.png)

> [AZURE.IMPORTANT] HDInsight クラスターで提供されるサービスを監視するには、Ambari Web または Ambari REST API を使用する必要があります。 Ambari の使用に関する詳細については、次を参照してください [Ambari を使用したクラスターの HDInsight の管理。](hdinsight-hadoop-manage-ambari.md)

## 次のステップ
この記事では、Azure ポータルを使用して HDInsight クラスターを作成する方法、および Hadoop コマンド ライン ツールを開く方法について説明しました。 詳細については、次の記事を参照してください。

* [Azure PowerShell を使用した HDInsight の管理](hdinsight-administer-use-powershell.md)
* [Azure CLI を使用した HDInsight の管理](hdinsight-administer-use-command-line.md)
* [HDInsight クラスターのプロビジョニング](hdinsight-provision-clusters.md)
* [プログラムによる Hadoop ジョブの送信](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Azure HDInsight の概要](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Azure HDInsight でサポートされている Hadoop のバージョン](hdinsight-component-versioning.md)

[preview-portal]: https://portal.azure.com

