<properties
    pageTitle="Apache Storm チュートリアル: Storm 入門 |Microsoft Azure"
    description="HDInsight での  Apache Storm および Storm Starter サンプルを使用したビッグ データ分析の概要 Storm を使用してデータをリアルタイムに処理する方法について説明します。"
    keywords="apache storm,apache storm tutorial,big data analytics,storm starter"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/04/2015"
   ms.author="larryfr"/>


# Apache Storm チュートリアル: Storm Starter サンプルを使用した HDInsight でのビッグ データ分析の概要

Apache Storm は、データ ストリームの処理を目的とし、スケーラビリティとフォールト トレランスに優れた、分散型のリアルタイム計算システムです。 Microsoft Azure HDInsight の Storm を使用して、Storm でリアルタイムで ビッグ データ分析を実行するクラウドベースの Storm クラスターを作成できます。 

> [AZURE.NOTE] この記事の手順では、Windows ベースの HDInsight クラスターを作成します。 HDInsight クラスター上の Linux ベースの Storm を作成する手順については、次を参照してください [Apache Storm チュートリアル: データ分析を使用して HDInsight で Storm Starter サンプルを使ってみる。](hdinsight-apache-storm-tutorial-get-started-linux.md)

## 前提条件

Apache Storm チュートリアルを正常に完了するには、次の条件を満たす必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

## Storm クラスターを作成する

HDInsight の Storm では、ログ ファイルとクラスターに送信されるトポロジを格納する Azure Blob Storage を使用します。 次の手順を使用して、クラスターに使用する Azure ストレージ アカウントを作成します。

1. サインイン、 [Azure ポータル][preview-portal]します。

2. 選択 **新規**, [ __データ分析__, 、し、[ __HDInsight__します。

    ![Create a new cluster in the Azure Portal](./media/hdinsight-apache-storm-tutorial-get-started/new-cluster.png)

3. 入力、 __クラスター名__, 、し、[ __Storm__ の __クラスターの種類__します。 横に緑のチェック マークが表示されます、 __クラスター名__ 表示されている場合。

    ![クラスター名、クラスターの種類、および OS の種類](./media/hdinsight-apache-storm-tutorial-get-started/clustername.png)

4. 複数のサブスクリプションの場合は選択し、 __サブスクリプション__ エントリをクラスターに使用される Azure サブスクリプションを選択します。

5.  __リソース グループ__, 、既存のリソース グループの一覧を表示し、[でクラスターを作成する 1 つにエントリを選択することができます。 選択または __新規作成__ し、新しいリソース グループの名前を入力します。 新しいグループ名を使用できる場合は、緑のチェック マークが表示されます。

    > [AZURE.NOTE] このエントリは、いずれかが使用可能な場合に、既存のリソース グループのいずれかに既定値です。

6. 選択 __資格情報__, 、し、入力、 __クラスター ログイン ユーザー名__ と __クラスター ログインのパスワード__します。 最後に、使用して  __選択__ 資格情報を設定します。 このドキュメントではリモート デスクトップは使用しないため、無効にしておくことができます。

    ![[クラスターの資格情報] ブレード](./media/hdinsight-apache-storm-tutorial-get-started/clustercredentials.png)

6.  __データソース__, 、既存のデータ ソースを選択するエントリを選択したり、新しいものを作成します。

    ![[データ ソース] ブレード](./media/hdinsight-apache-storm-tutorial-get-started/datasource.png)

    現在、HDInsight クラスターのデータ ソースとして Azure ストレージ アカウントを選択できます。 エントリを理解するために、次を使用して、 __データソース__ ブレードです。

    - __選択方法__: これを設定 __すべてのサブスクリプションから__ 、サブスクリプションのストレージ アカウントの参照を有効にします。 設定 __アクセス キー__ を入力する場合、 __ストレージ名__ と __アクセス キー__ の既存のストレージ アカウント。

    - __新規作成__: 新しいストレージ アカウントの作成に使用します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。

    - __既定のコンテナーを選択して__: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。

    - __場所__: ストレージ アカウントとなる地理的領域には、またはに作成されます。

        > [AZURE.IMPORTANT] HDInsight クラスターの場所を設定も既定のデータ ソースの場所を選択します。 クラスターと既定のデータ ソースは、同じリージョンに存在する必要があります。

    - __選択__: これを使用してデータ ソースの構成を保存します。

7. 選択 __ノード料金レベル__ このクラスター用に作成するノードに関する情報を表示します。 既定では、ワーカー ノードの数に設定 __4__します。 これを設定 __1__, 、このチュートリアルでは十分で、クラスターのコストを削減します。 クラスターの推定コストがブレードの下部に表示されます。

    ![[ノード価格レベル] ブレード](./media/hdinsight-apache-storm-tutorial-get-started/nodepricingtiers.png)

    使用  __選択__ を保存する、 __ノード料金レベル__ 情報。

8. 選択 __オプションの構成__します。 このブレードでは、クラスターのバージョンを選択できるだけでなく、その他のオプションの設定を追加するなど、 __仮想ネットワーク__ を設定するか、 __外部メタストア__ Hive と Oozie のデータを保持します。

    ![[オプションの構成] ブレード](./media/hdinsight-apache-storm-tutorial-get-started/optionalconfiguration.png)

9. いることを確認 __スタート画面にピン__ が選択されて、[ __作成__します。 これでクラスターが作成され、Azure ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターがプロビジョニング中であることを示し、プロビジョニングが完了すると、[HDInsight] アイコンを表示するように変化します。

  	| プロビジョニング中 | プロビジョニング完了 |
  	| ------------------ | --------------------- |
  	| ![スタート画面のプロビジョニング中インジケーター](./media/hdinsight-apache-storm-tutorial-get-started/provisioning.png) | ![プロビジョニングされたクラスターのタイル](./media/hdinsight-apache-storm-tutorial-get-started/provisioned.png) |

    > [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または __通知__ プロビジョニング プロセスをチェックする、ページの左にあるエントリです。

## HDInsight での Storm Starter サンプルの実行

この Apache Storm チュートリアルでは、GitHub で Storm Starter サンプルを使用したビッグ データ分析の概要について説明します。

HDInsight クラスターの各 Storm には、クラスターで Storm トポロジをアップロードし実行するために使用する Storm ダッシュボードが付属しています。 各クラスターには、Storm ダッシュボードから直接実行できるトポロジのサンプルも付属しています。

### <a id="connect"></a>ダッシュボードへの接続

ダッシュ ボードはある **https://&lt;clustername>.azurehdinsight.net//**, ここで、 **clustername** 、クラスターの名前を指定します。 スタート ボードで、クラスターを選択し、ダッシュ ボードへのリンクを検索することもできる、 __ダッシュ ボード__ ブレードの上部にあるリンクです。

![Storm ダッシュボードのリンクを使用した Azure ポータル](./media/hdinsight-apache-storm-tutorial-get-started/dashboard.png)

> [AZURE.NOTE] ダッシュ ボードに接続するときに、ユーザー名とパスワードを入力するように求められます。 これは、管理者名 (**admin**) とパスワードが、クラスターの作成時に使用します。

Storm ダッシュ ボードが読み込まれると表示されます、 **トポロジの送信** フォームです。

![Storm Starter トポロジを Storm ダッシュボードで送信します。](./media/hdinsight-apache-storm-tutorial-get-started/submit.png)

 **トポロジの送信** フォームを使用してをアップロードして、Storm トポロジが含まれている .jar ファイルを実行できます。 また、これにはクラスターに付属しているいくつかの基本的なサンプルも含まれています。

### <a id="run"></a>GitHub の Storm Starter プロジェクトからワードカウント サンプルを実行する

クラスターに付属しているサンプルには、ワードカウント トポロジのいくつかのバリエーションが含まれます。 これらのサンプルが含まれて、 **スパウト** 、センテンスをランダムに出力して **ボルト** 各センテンスを個別の単語に分割し、各単語が発生した回数をカウントします。 これらのサンプルは、Apache Storm の一部である [Storm Starter サンプル](https://github.com/apache/storm/tree/master/examples/storm-starter)で提供されています。

次の手順に従って、Storm Starter サンプルを実行します。

1. 選択 **StormStarter - WordCount** から、 **Jar ファイル** ボックスの一覧です。 これによって、 **クラス名** と **追加パラメーター** このサンプル用のパラメーターを持つフィールドです。

    ![Storm ダッシュボードで選択されている Storm Starter WordCount。](./media/hdinsight-apache-storm-tutorial-get-started/submit.png)

    * **クラス名** -トポロジを送信する .jar ファイル内のクラスです。
    * **追加のパラメーター** -トポロジで必要なパラメーターです。 この例では、フィールドに、送信したトポロジのわかりやすい名前を指定しています。

2. クリックして  **送信**します。 しばらくすると、 **結果** フィールドには、ジョブだけでなく、コマンドの結果を送信するために使用されるコマンドが表示されます。  **エラー** フィールド、トポロジの送信で発生したエラーが表示されます。

    ![Storm Starter WordCount の送信ボタンと結果。](./media/hdinsight-apache-storm-tutorial-get-started/submit-results.png)

    > [AZURE.NOTE] 結果が、トポロジが完了したことで示されていません **、開始後、Storm トポロジが停止するまでを実行します。** ワードカウント トポロジは、ランダムなセンテンスを生成し、停止されるまで各単語の出現回数を計算します。

### <a id="monitor"></a>トポロジの監視

Storm UI を使用してトポロジを監視できます。

1. 選択 **Storm UI** Storm ダッシュ ボードの上部にあります。 クラスターの情報と実行中のすべてのトポロジに関する概要情報が表示されます。

    ![Storm Starter WordCount トポロジの概要を示す Storm ダッシュボード。](./media/hdinsight-apache-storm-tutorial-get-started/stormui.png)

    上記のページで、トポロジがアクティブな状態だった時間、使用されているワーカー、エグゼキュータ、タスクの数を確認できます。

    > [AZURE.NOTE]  **名前** 列で既に指定した表示名を格納する、 **追加パラメーター** フィールドです。

4.  **トポロジの概要**, を選択、 **wordcount** 内のエントリ、 **名前** 列です。 これにより、トポロジの詳細が表示されます。

    ![Storm Starter WordCount トポロジの情報が含まれている Storm ダッシュボード。](./media/hdinsight-apache-storm-tutorial-get-started/topology-summary.png)

    このページには、次の情報が表示されます。

    * **トポロジの統計** -時間枠で整理された、トポロジのパフォーマンスに関する基本的な情報です。

        > [AZURE.NOTE] 特定の時間枠を選択すると、ページの他のセクションに表示される情報の時間枠を変更します。

    * **スパウト** -各スパウトによって返された最後のエラーを含む、スパウト関する基本的な情報です。

    * **ボルト** -ボルトに関する基本的な情報です。

    * **トポロジの構成** -トポロジの構成に関する詳細情報。

    このページには、トポロジで実行できるアクションも表示されます。

    * **アクティブ化** -非アクティブ化されたトポロジの処理を再開します。

    * **非アクティブ化** -実行中のトポロジを一時停止します。

    * **再調整** -トポロジの並列処理を調整します。 クラスターのノード数を変更した場合は、実行中のトポロジを再調整する必要があります。 この操作で、クラスター内のノード数の増減に合わせて、トポロジの並列処理を調整できます。 詳細については、「[Understanding the parallelism of a Storm topology (Storm トポロジの並列処理)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)」をご覧ください。

    * **強制終了** -指定したタイムアウト後に Storm トポロジを停止します。

5. このページからエントリを選択、 **スパウト** または **ボルト** セクションです。 選択したコンポーネントに関する情報が表示されます。

    ![選択したコンポーネントに関する情報が含まれている Storm ダッシュボード。](./media/hdinsight-apache-storm-tutorial-get-started/component-summary.png)

    このページには次の情報が表示されます。

    * **スパウト/ボルトの統計** -時間枠で整理された、コンポーネントのパフォーマンスに関する基本的な情報です。

        > [AZURE.NOTE] 特定の時間枠を選択すると、ページの他のセクションに表示される情報の時間枠を変更します。

    * **入力の統計** (ボルトのみ) - ボルトによって使用されるデータを生成するコンポーネントに関する情報。

    * **出力の統計** -このボルトによって出力されるデータに関する情報。

    * **エグゼキュータ** -このコンポーネントのインスタンスに関する情報。

    * **エラー** -このコンポーネントで生成されたエラーです。

5. スパウトやボルトの詳細を表示しているときからエントリを選択して、 **ポート** 内の列、 **エグゼキュータ** を表示するでは、コンポーネントの特定のインスタンスについて詳しく説明します。

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    このデータからあることがわかります単語 **7** 1,493,957 時間が発生しました。 これは、このトポロジが開始されてから発生した回数です。

### トポロジを停止する

戻り、 **トポロジの概要** ワード カウント トポロジに関するページをクリックし [ **Kill** から、 **トポロジのアクション** セクションです。 メッセージが表示されたら、トポロジを停止するまでの待機秒数として「10」を入力します。 タイムアウト期間後、トポロジは表示されなくなりますにアクセスすると、 **Storm UI** ダッシュ ボードのセクションです。

## 概要

この Apache Storm チュートリアルでは、Storm Starter を使用して、HDInsight クラスターで Storm を作成する方法と、Storm ダッシュボードを使用して Storm トポロジをデプロイ、監視、管理する方法について説明しました。

## <a id="next"></a>次のステップ

* **HDInsight Tools for Visual Studio** -HDInsight ツールを送信して、Visual Studio を使用できるように監視、および前述の Storm ダッシュ ボードのような Storm トポロジを管理します。 また HDInsight ツールは、C# Storm のトポロジを作成する機能を提供しており、クラスター上にデプロイし、実行できるトポロジ サンプルを含んでいます。

    詳細については、「[HDInsight Hadoop Tools for Visual Studio の使用開始](hdinsight-hadoop-visual-studio-tools-get-started.md)」をご覧ください。

* **サンプル ファイル** -HDInsight Storm クラスターは、いくつかの例、 **%STORM_HOME%\contrib** ディレクトリ。 次の例がそれぞれ含まれます。

    * ソース コード - storm-starter-0.9.1.2.1.5.0-2057-sources.jar など

    * Java ドキュメント - storm-starter-0.9.1.2.1.5.0-2057-javadoc.jar など

    * 例 - storm-starter-0.9.1.2.1.5.0-2057-jar-with-dependencies.jar など

    'jar' コマンドを使用して、ソース コードまたは Java ドキュメントを抽出します。 たとえば、'jar -xvf storm-starter-0.9.1.2.1.5.0.2057-javadoc.jar' のように指定します。

    > [AZURE.NOTE] Java ドキュメントは、web ページで構成されます。 抽出後にブラウザーを使用して表示、 **index.html** ファイルです。

    これらのサンプルにアクセスするには必要があります、HDInsight クラスターの Storm のリモート デスクトップを有効にして、ファイルをコピーし、 **%STORM_HOME%\contrib**します。

* 次のドキュメントには、HDInsight の Storm と使用できるその他のサンプルの一覧が含まれています。

    * [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/


