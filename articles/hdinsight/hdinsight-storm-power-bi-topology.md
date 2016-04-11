<properties
 pageTitle="Apache Storm から Power BI へのデータの書き込み | Microsoft Azure"
 description="HDInsight の Apache Storm クラスター上で実行されている C# トポロジから Power BI にデータを書き込みます。 また、Power BI を使用してレポートとリアルタイムのダッシュボードも作成します。"
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="paulettm"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="12/04/2015"
 ms.author="larryfr"/>

# Power BI (プレビュー) を使用した Apache Storm トポロジのデータの視覚化

Power BI プレビューを使用すると、データをレポートとして表示したり、ダッシュボードに表示したりできます。 Power BI REST API によって、HDInsight クラスター上の Apache Storm で実行しているトポロジのデータを Power BI で簡単に使用できます。

このドキュメントでは、Power BI を使用して、Storm トポロジによって作成されたデータから、レポートとダッシュボードを作成する方法について説明します。

## 前提条件

- Azure サブスクリプション。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

* Azure Active Directory ユーザー [Power BI](https://powerbi.com) アクセス

* 下記のいずれかのバージョンの Visual Studio

    * Visual Studio 2012 と [アップデート 4](http://www.microsoft.com/download/details.aspx?id=39305)

    * Visual Studio 2013 と [アップデート 4](http://www.microsoft.com/download/details.aspx?id=44921) または [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)

    * Visual Studio 2015 [CTP6](http://visualstudio.com/downloads/visual-studio-2015-ctp-vs)

* HDInsight Tools for Visual Studio: を参照してください [Visual Studio の HDInsight ツールを使い始める](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md) インストール情報についてです。

## 動作のしくみ

この例に含まれる C# Storm トポロジは、文をランダムに生成し、その文を単語に分割し、単語をカウントし、単語とカウントを Power BI REST API に送信します。  [PowerBi.Api.Client](https://github.com/Vtek/PowerBI.Api.Client) Nuget パッケージは Power BI との通信に使用します。

このプロジェクトの次のファイルによって、Power BI 固有の機能が実装されます。

* **PowerBiBolt.cs**: Power BI にデータを送信する Storm ボルトを実装します。

* **Data.cs**: Power BI に送信されるデータのオブジェクト/行について記述します

> [AZURE.WARNING] Power BI は、同じ名前の複数のデータセットの作成を許可するようです。 これは、データセットが存在しない場合に、トポロジで Power BI ボルトの複数のインスタンスを作成すると生じる可能性があります。 これを回避するには、(この例の場合のように) ボルトの並列処理ヒントを 1 に設定するか、トポロジをデプロイする前にデータセットを作成します。
>
>  **CreateDataset** このソリューションに含まれるコンソール アプリケーションは、トポロジ外部でデータセットを作成する方法の例として提示します。

## Power BI アプリケーションの登録

1. 手順に従って、 [Power BI のクイック スタート](https://msdn.microsoft.com/library/dn931989.aspx) Power BI にサインアップします。

2. 手順に従います [アプリの登録](https://msdn.microsoft.com/library/dn877542.aspx) 、アプリケーションの登録を作成します。 登録情報は、Power BI REST API にアクセスするときに使用します。

    > [AZURE.IMPORTANT] 保存、 **クライアント ID** アプリケーションに登録します。

## 例のダウンロード

ダウンロード、 [HDInsight c# Storm Power BI の例](https://github.com/Blackmist/hdinsight-csharp-storm-powerbi)します。 これをダウンロードするフォーク/複製するかを使用して [git](http://git-scm.com/), 、不正使用を **ダウンロード** リンクしたアーカイブの zip ファイルをダウンロードします。

## サンプルの構成

1. Visual Studio でサンプルを開きます。  **ソリューション エクスプ ローラー**, 、開かれている、 **SCPHost.exe.config** ファイルを見つけて、 **< OAuth…/>** 要素。 この要素の次のプロパティの値を入力します。

    * **クライアント**: 以前に作成したアプリケーションの登録のクライアント ID。

    * **ユーザー**: Power BI へのアクセス権を持つ Azure Active Directory アカウント。

    * **パスワード**: Azure Active Directory アカウントのパスワード。

2. (省略可能)。 このプロジェクトで使用される既定のデータセット名は **単語**します。 これを変更するを右クリックして、 **WordCount** プロジェクト **ソリューション エクスプ ローラー**, を選択 **プロパティ**, 、し、[ **設定**します。 変更、 **DatasetName** 目的の値を入力します。

2. ファイルを保存して閉じます。

## サンプルのデプロイ

1.  **ソリューション エクスプ ローラー**, を右クリックし、 **WordCount** プロジェクトし、選択 **HDInsight での Storm に送信**します。 HDInsight クラスターを選択、 **Storm クラスター** ] ドロップダウン ダイアログ。

    > [AZURE.NOTE] 数秒かかる場合があります、 **Storm クラスター** ドロップダウンにサーバー名を入力します。
    >
    > メッセージが表示されたら、Azure サブスクリプションのログイン資格情報を入力します。 2 つ以上のサブスクリプションをお持ちの場合は、HDInsight クラスターの Storm があるサブスクリプションにログインします。

2. トポロジが正常に送信されたら、クラスターの Storm トポロジが表示されます。 一覧から [WordCount] トポロジを選択して、実行中のトポロジに関する情報を表示します。

    ![WordCount トポロジが選択されているトポロジ](./media/hdinsight-storm-power-bi-topology/topologysummary.png)

    > [AZURE.NOTE] サーバー エクスプ ローラーから Storm トポロジを表示することもできます。 Azure の展開]、[HDInsight]、HDInsight クラスターで Storm を右クリックし、[Storm トポロジの表示。

3. 表示するときに、 **トポロジの概要**, 、表示されるまでスクロール、 **ボルト** セクションです。 このセクションで、 **Executed** の列、 **PowerBI** ボルトです。 ページ上部にある [更新] ボタンを使用して、値が 0 以外の値を変更するまで更新を実行します。 この数値が増えると、対象項目が Power BI に書き込まれていることを示します。

## レポートの作成

1. ブラウザーで、次を参照してください。 [https://PowerBI.com](https://powerbi.com)します。 ご使用のアカウントでサインインします。

2. ページの左側にある [展開 **データセット**します。 選択、 **単語** エントリです。 これは、トポロジの例で作成したデータセットです。

    ![Words データセット エントリ](./media/hdinsight-storm-power-bi-topology/words.png)

3.  **フィールド** 領域で、展開 **WordCount**します。 ドラッグ、 **カウント** と **Word** 、ページの中央部へのエントリ。 これにより、対象の単語が何回出現したかを示す棒が単語ごとに表示された新しいグラフが作成されます。

    ![WordCount グラフ](./media/hdinsight-storm-power-bi-topology/wordcountchart.png)

4. ページの左上で、次のように選択します。 **保存** 新しいレポートを作成します。 入力 **Word Count** として、レポートの名前。

5. Power BI ロゴを選択し、ダッシュボードに戻ります。  **Word Count** レポートは今すぐ下に表示されます。 **レポート**します。

## ライブ ダッシュボードの作成

1. 横にある **ダッシュ ボード**, を選択、 **+** を新しいダッシュ ボードを作成するアイコン。 新しいダッシュ ボードの名前 **Live Word Count**します。

2. 選択、 **ワード カウント** 前に作成したを報告します。 表示されたら、グラフを選択し、グラフの右上にあるプッシュピン アイコンを選択します。 ピン留めしたことを示す通知をダッシュボードで受信します。

    ![プッシュピンが表示されているグラフ](./media/hdinsight-storm-power-bi-topology/pushpin.png)

2. Power BI ロゴを選択し、ダッシュボードに戻ります。 選択、 **Live Word Count** ダッシュ ボード。 ダッシュボードに Word Count グラフが含まれるようになり、HDInsight で実行されている WordCount トポロジから Power BI に新しいエントリが送信されるとグラフが更新されます。

    ![ライブ ダッシュボード](./media/hdinsight-storm-power-bi-topology/dashboard.png)

## WordCount トポロジの停止

トポロジの実行は、意図的に停止するか、HDInsight クラスター上で Storm を削除するまで続きます。 トポロジを停止するには、以下の手順を実行します。

1. Visual Studio で開き、 **トポロジの概要** WordCount トポロジのウィンドウです。 トポロジの概要がまだ開いていない場合は、「 **サーバー エクスプ ローラー**, 、展開、 **Azure** と **HDInsight** のエントリを選択し、HDInsight クラスターの Storm を右クリックし **View Storm Topologies**します。 最後に、選択、 **WordCount** トポロジです。

2. 選択、 **Kill** を停止するボタン、 **WordCount** トポロジです。

    ![[トポロジの概要] の [強制終了 (Kill)] ボタン](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## 次のステップ

このドキュメントでは、REST を使用して Strom トポロジのデータを Power BI に送信する方法を説明します。 他の Azure テクノロジを使用する方法については、以下をご覧ください。

* [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)

