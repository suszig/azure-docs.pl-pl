<properties 
    pageTitle="Machine Learning Python クライアント ライブラリを使って Python のデータ セットにアクセスする | Microsoft Azure" 
    description="Python クライアント ライブラリをインストールして使用し、ローカル Python 環境から Azure Machine Learning データを安全に管理します。" 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/04/2015" 
    ms.author="huvalo;bradsev" />


#Azure Machine Learning Python クライアント ライブラリを使って Python のデータ セットにアクセスする 

Microsoft Azure Machine Learning Python クライアント ライブラリのプレビューは、ローカルの Python 環境から Azure Machine Learning データセットへの安全なアクセスを確立し、ワークスペースにおけるデータセットを作成して管理できるようにします。

このトピックでは次の方法について説明します。

* Machine Learning Python クライアント ライブラリをインストールする 
* ローカルの Python 環境から Azure Machine Learning データセットへのアクセス認証を取得するなど、データ セットにアクセスしてアップロードする
*  実験から中間データセットにアクセスする
*  Python クライアント ライブラリを使用してデータ セットを列挙、メタデータにアクセス、データ セットのコンテンツを読み込み、新しいデータ セットを作成して既存のデータ セットを更新する

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>前提条件

Python クライアント ライブラリは、次の環境でテストされています。

 - Windows、Mac、Linux
 - Python 2.7、3.3、3.4

次のパッケージに依存します。

 - requests
 - python-dateutil
 - pandas

などを含む Python ディストリビューションを使用することをお勧め [Anaconda](http://continuum.io/downloads#all) または [Canopy](https://store.enthought.com/downloads/), 、python、IPython および上に示した 3 つのパッケージをインストールします。 IPython は厳密には必要ありませんが、データをインタラクティブに操作して視覚化する優れた環境になります。


###<a name="installation"></a>Azure Machine Learning Python クライアント ライブラリをインストールする方法

また、このトピックで説明するタスクを完了するためには Azure Machine Learning Python クライアント ライブラリをインストールする必要があります。 アクセスできる、 [Python Package Index](https://pypi.python.org/pypi/azureml)します。 ご利用の Python 環境にインストールするには、ローカルの Python 環境から次のコマンドを実行します。

    pip install azureml

代わりに、ダウンロードしてインストールのソースから [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python)します。

    python setup.py install

コンピューターに git をインストールしていれば、git リポジトリから直接 pip を使ってインストールできます。

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Studio Code スニペットを使用するデータセットにアクセスします。

Python クライアント ライブラリを使うと、実行している実験から既存のデータ セットへプログラムでアクセスできるようになります。

Studio Web インターフェイスから、ローカル マシンでデータ セットを Pandas DataFrame オブジェクトとしてダウンロードして逆シリアル化するために必要なすべての情報を含むコード スニペットを生成できます。

### <a name="security"></a>データ アクセスのセキュリティ

Python クライアント ライブラリで使用するために Studio で提供されるコード スニペットには、ワークスペース ID や認証トークンが含まれます。 これらを使用すればワークスペースにフル アクセスできますので、パスワードなどで保護する必要があります。

セキュリティ上の理由から、コード スニペットの機能として設定された役割を持つユーザーに利用可能なのみ **所有者** ワークスペースのです。 Azure Machine Learning Studio でのロールが表示される、 **ユーザー** ] ページの [ **設定**します。

![セキュリティ][セキュリティ]

自分の役割が設定されていない場合 **所有者**, 、リクエスト、所有者として再度招待コード スニペットを提供するために、ワークスペースの所有者に質問するかを実行できます。

次のいずれかを実行することで認証トークンを取得できます。

1. 所有者からトークンをリクエストします。 所有者は Studio のワークスペースの [設定] ページから認証トークンを確認できます。 選択 **設定** 、左側のウィンドウをクリックしてから **認証トークン** プライマリとセカンダリのトークンを確認します。  プライマリとセカンダリの認証トークンのどちらもコード スニペットで使用できますが、所有者はセカンダリの認証トークンのみ共有することをお勧めします。

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

2. 所有者への役割の昇格を申請する  そのためにはまず、ワークスペースの現在の所有者にワークスペースから削除してもらい、その後所有者としてに再度招待してもらう必要があります。

開発者は、ワークスペース id と認証を入手したトークンでできる、役割に関係なく、コード スニペットを使用してワークスペースにアクセスします。

上で認証トークンの管理、 **認証トークン** ] ページの [ **設定**します。 認証トークンを再度生成することは可能ですが、以前のトークンへのアクセスは無効になります。

### <a name="accessingDatasets"></a>ローカル Python アプリケーションからデータセットにアクセスします。

1. Machine Learning Studio では、をクリックして **データセット** 、左側のナビゲーション バーでします。

2. アクセスするデータ セットを選択します。 データセットのいずれかを選択することができます、 **MY DATASETS** ] ボックスの一覧から、または、 **サンプル** ] ボックスの一覧です。

3. 下のツールバーからをクリックして **データ アクセス コードの生成**します。 データが Python クライアント ライブラリと互換性のない形式の場合は、このボタンが無効になることに注意してください。

    ![データセット][データセット]

4. 表示されるウィンドウからコード スニペットを選択し、クリップボードにコピーします。

    ![アクセス コード][データセットのアクセス コード]

5. ローカル Python アプリケーションのノートにコードを貼り付けます。

    ![Notebook][ipython データセット]

### <a name="accessingIntermediateDatasets"></a>Machine Learning 実験から中間データセットにアクセスします。

Machine Learning Studio で実験が実行されると、モジュールの出力ノードから中間データ セットにアクセスできます。 中間データセットは、モデル ツールが実行されているときに中間手順で作成され使用されるデータです。

中間データセットは、データの形式が Python クライアント ライブラリと互換性がある限りアクセスできます。

次の形式がサポートされます (`azureml.DataTypeIds` クラスの定数)。

 - PlainText
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

モジュールの出力ノードの上にカーソルを置くと、形式を確認できます。 ヒントにノード名とともに表示されます。

いくつかの出力名の形式を [分割] [分割] モジュールなどのモジュール `Dataset`, 、Python クライアント ライブラリはサポートされていません。

![データセットの形式][データセットの形式]

[CSV に変換] などの変換モジュールを使用する必要があります [convert を-csv]、サポートされている形式で出力を取得します。

![GenericCSV 形式][csv 形式]

次の手順は、実験を作成、実行して中間データ セットにアクセスする例を示します。

1. 新しい実験を作成する

2. 挿入、 **成人国勢調査収入に関する二項分類データセット** モジュールです。

3. [分割] [分割] モジュールを挿入し、その入力値セット モジュール出力に接続します。

4. [CSV に変換] の [convert csv] モジュールを挿入し、その入力値 [分割] [分割] モジュールの出力のいずれかに接続します。

5. 実験を保存して実行し、実行が完了するまで待機します。

6. [CSV に変換] の [convert csv] モジュールの出力ノードをクリックします。

7. コンテキスト メニューが表示され、選択 **データ アクセス コードの生成**します。

    ![コンテキスト メニュー][実験]

8. ウィンドウが表示されます。 コード スニペットを選択し、クリップボードにコピーします。

    ![アクセス コード][中間にデータセットのアクセス コード]

9. ノートブックにコードを貼り付けます。

    ![Notebook][ipython 中間データセット]

10. Matplotlib を使用してデータを視覚化できます。 これは、age 列のヒストグラムで表示されます。

    ![ヒストグラム][ヒストグラム ipython]


##<a name="clientApis"></a>Machine Learning Python クライアント ライブラリを使用して、アクセス、読み取り、作成、およびデータセットの管理

### ワークスペース

ワークスペースは、Python クライアント ライブラリのエントリ ポイントです。 ワークスペース ID と認証トークンを使って `Workspace` クラスを提供し、インスタンスを作成します。

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### データセットを列挙する

特定のワークスペースですべてのデータセットを列挙するには:

    for ds in ws.datasets:
        print(ds.name)

ユーザーが作成したデータセットのみを列挙するには:

    for ds in ws.user_datasets:
        print(ds.name)

サンプルのデータセットのみを列挙するには:

    for ds in ws.example_datasets:
        print(ds.name)

名前でデータセットにアクセスするには (大文字小文字は区別されます):

    ds = ws.datasets['my dataset name']

または、インデックスでアクセスするには:

    ds = ws.datasets[0]


### Metadata

データセットには、コンテンツに加え、メタデータがあります  (中間データセットはこのルールの例外であり、メタデータはありません)。

作成時にユーザーが割り当てるメタデータの値の一部には次のものがあります。

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Azure ML によって割り当てられるその他の値には次のものがあります。

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

使用できるメタデータの詳細については、`SourceDataset` クラスをご覧ください。


### コンテンツを読み込む

Machine Learning Studio で提供されるコード スニペットは、データ セットを自動的にダウンロードして Pandas DataFrame オブジェクトに逆シリアル化します。 これは次の `to_dataframe` メソッドで実行されます。

    frame = ds.to_dataframe()

生データをダウンロードして自分で逆シリアル化を実行する場合は、上記のようにします。 現時点では、Python クライアント ライブラリで逆シリアル化できない「ARFF」などの形式に対する唯一の方法になります。

テキストとして内容を読み込むには:

    text_data = ds.read_as_text()

バイナリとして内容を読み込むには:

    binary_data = ds.read_as_binary()

内容にストリームを開くこともできます。

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### 新しいデータセットを作成する

Python クライアント ライブラリでは、Python プログラムからデータセットをアップロードできます。 これらのデータセットは、ご利用のワークスペースで使用できます。

Pandas DataFrame にデータがある場合は、次のコードを使用します。

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

ご利用のデータが既にシリアル化されている場合、次を使用できます。

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python クライアント ライブラリは、Pandas DataFrame を次の形式にシリアル化できます (`azureml.DataTypeIds` クラスの定数)。

 - PlainText
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### 既存のデータセットを更新する

既存のデータセットに一致する名前で新しいデータセットを更新しようとすると、競合エラーが発生します。

既存のデータセットを更新するには、まず既存のデータセットへの参照を取得する必要があります。

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

その後、`update_from_dataframe` を使用してをシリアル化し、Azure のデータセットの内容を置き換えます。

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

データを別の形式にシリアル化する場合は、省略可能な `data_type_id` パラメーターの値を指定します。

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

`description` パラメーターの値を指定して、新しい説明をオプションで設定できます。

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

`name` パラメーターの値を指定して、新しい名前をオプションで設定できます。 その後は、新しい名前のみでデータセットを取得することになります。 次のコードを使用すると、データ、名前、説明を更新できます。

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

`data_type_id`、`name`、`description` のパラメーターはすべて省略可能で、前の値に対する既定値になります。 `dataframe` パラメーターは常に必要です。

ご利用のデータが既にシリアル化されている場合、`update_from_dataframe` の代わりに `update_from_raw_data` を使用します。 同様に動作に渡すだけ `raw_data` の代わりに  `dataframe`します。



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
