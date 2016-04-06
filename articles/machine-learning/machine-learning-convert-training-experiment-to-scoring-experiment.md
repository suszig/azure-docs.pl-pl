<properties
    pageTitle="Machine Learning のトレーニング実験から予測実験に変換する | Microsoft Azure"
    description="予測分析モデルのトレーニングに使用されるMachine Learning のトレーニング実験を、Web サービスとしてデプロイできる予測実験に変換する方法。"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2015"
    ms.author="garye"/>

# Machine Learning のトレーニング実験から予測実験に変換する

Azure Machine Learning では、予測分析ソリューションをビルド、テスト、およびデプロイできます。

作成してに反復処理を実行した後、 *トレーニング実験* 予測分析モデルをトレーニングして、新しいデータのスコア付けに使用する準備ができたら、準備してスコア付け実験を効率化する必要があります。 これを展開することができますし、 *予測実験* ユーザーがモデルにデータを送信して、モデルの予測を受信できるように、Azure web サービスとして。

予測実験に変換することで、トレーニングされたモデルを Web サービスとしてデプロイできるようになります。 Web サービスのユーザーがモデルに入力データを送信し、モデルは予測結果を返します。 したがって、予測実験を変換した場合は、モデルが他のユーザーに使用されることを想定しておいてください。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

トレーニング実験を予測実験に変換するプロセスには、次の 3 つの手順が含まれます。

1.  トレーニング済みの機械学習モデルを保存し、機械学習アルゴリズムを置換し、 [モデルのトレーニング][train-model] 、保存したトレーニング済みモデルでのモジュール。
2.  スコア付けに必要なモジュールのみに実験を絞り込みます。 トレーニング実験には、トレーニングに必要な多数のモジュールが含まれますが、モデルがトレーニングされてスコア付けに使用できるようになると、それらのモジュールは必要なくなります。
3.  実験の中で Web サービスのユーザーからのデータをどこで受け入れ、どのデータを返すか定義します。

## [Web サービスの設定] ボタン

実験を実行した後 (**実行** 実験キャンバスの下部にあるボタン)、 **Web サービスのセットアップ** ボタン (選択、 **予測 Web サービス** オプション) は、トレーニング実験を予測実験に変換する 3 つの手順の実行します。

1.  トレーニング済みのモデル内のモジュールとして保存、 **トレーニング済みのモデル** (実験キャンバスの左側) にあるモジュール パレットのセクションでは、次に、機械学習アルゴリズムを置き換えますと [モデルのトレーニング][train-model] を保存したトレーニング済みモデルのモジュール。
2.  これは明らかに不要なモジュールを削除します。 この例ではこれが含まれています、 [分割][split], 、2 番目 [モデルのスコア付け][score-model], 、および [モデルの評価][evaluate-model] モジュールです。
3.  Web サービスの入力モジュールと出力モジュールを作成し、実験の既定の場所に追加します。

たとえば、次の実験では、サンプルの国勢調査のデータを使用した 2 クラスのブースト デシジョン ツリー モデルをトレーニングします。

![トレーニング実験][figure1]

この実験のモジュールは、基本的に 4 つの異なる機能を実行します。

![モジュール関数][figure2]

このトレーニング実験を予測実験に変換するときにこれらのモジュールの一部が不要になったり、別の目的を持つようになったりします。

- **データ** -スコア付け中にこのサンプル データセット内のデータは使用されません - web サービスのユーザーはスコア付けされるデータを指定します。 ただし、データ型など、このデータセットからのメタデータは、トレーニング済みのモデルで使用されます。 このため、このメタデータを提供できるように、予測実験のデータセットを保持する必要があります。

- **準備** - によっては、データに送信されるスコア付け、これらのモジュールが受信したデータを処理する必要はありません。

    たとえば、この例では、サンプルのデータセットの値が欠落しており、モデルのトレーニングに必要ない列が含まれています。 したがって、 [見つからないデータのクリーンアップ][clean-missing-data] モジュールが存在しない値の処理に含まと [プロジェクト列][project-columns] モジュールがデータ フローからこれらの余分な列を除外するのには含まします。 Web サービスを通じてスコアリングのために送信されるデータには、欠損値はありませんし、削除することがわかっている場合、 [見つからないデータのクリーンアップ][clean-missing-data] モジュールです。 ただし、以降、 [プロジェクト列][project-columns] モジュールでは、スコア付けされる機能セットの定義を使用する、そのモジュールは残す必要があります。

- **トレーニング** -単一のトレーニング済みモデルのモジュールとして保存するモデルが正常にトレーニングされたらします。 次に、これらの各モジュールを、保存されているトレーニング済みのモデルに置き換えます。

- **スコア** -この例では、テスト データとトレーニング データのセットにデータ ストリームを分割する分割モジュールを使用します。 予測実験では、これは必要ないため削除できます。 同様に、2 番目 [モデルのスコア付け][score-model] モジュールと [モデルの評価][evaluate-model] モジュールを使用して、予測の実験でこれらのモジュールが不要なも、テスト データからの結果を比較します。 残りの [モデルのスコア付け][score-model] モジュールは、web サービスを通じてスコア結果を返すためただし、必要です。

次の図は、この例をクリックすると **Web サービスのセットアップ**:

![変換された予測実験][figure3]

これは、実験を Web サービスとしてデプロイするための準備として十分な場合があります。 ただし、実験に合わせて追加の作業を実行する可能性があります。

### 入力と出力のモジュールを調整する

トレーニング実験では、トレーニング データのセットを使用して、機械学習アルゴリズムが必要とする形式でデータを取得するいくつかの処理を実行しました。 不要な場合、web サービスを通じて受信するデータはこの処理を行うことができます、 **Web サービス入力モジュール** 、実験の別のノードにします。

たとえば、既定で **Web サービスのセットアップ** 配置、 **Web サービスの入力** 上の図に示すように、データ フローの上部にあるモジュール。 ただし場合は、入力データでは、この処理は必要ありませんが、手動で配置できます、 **Web サービスの入力** をデータ処理モジュールの先。

![Web サービス入力の移動][figure4]

Web サービスを通じて提供される入力データが、これで処理なしでモデルのスコア付けモジュールに直接渡されるようになりました。

既定では同様に、 **Web サービスのセットアップ** Web サービスの出力モジュールをデータ フローの下部に置きます。 この例では、web サービスはユーザーの出力を返す、 [モデルのスコア付け][score-model] を含む完全な入力データのベクトル、およびスコア付けモジュールの結果します。

ただしを希望する別のものを返す例をスコア付けの結果だけと入力データのベクトル全体ではないが挿入、 [プロジェクト列][project-columns] モジュールをスコア付けの結果以外のすべての列を除外します。 移動する、 **サービスからの出力を Web** モジュールの出力を [プロジェクト列][project-columns] モジュール。

![Web サービス出力の移動][figure5]

### データ処理モジュールを追加または削除する

スコア付け中に必要ないことがわかっているモジュールが、他にも実験に存在する場合は、それらを削除できます。 たとえば、移動したため、 **Web サービスの入力** データ処理モジュールの後の地点にモジュールを削除できる、 [見つからないデータのクリーンアップ][clean-missing-data] 予測実験からモジュールです。

これで、予測実験が次のようになります。

![その他のモジュールの取り外し][figure6]

### オプションの Web サービス パラメーターを追加する

場合によっては、Web サービスのユーザーがサービスにアクセスしたときに、モジュールの動作を変更できるようにすることもあります。 *Web サービスのパラメーター* これを行うことができます。

一般的な例をセットアップ、 [リーダー][reader] モジュールを web サービスにアクセスする場合、配置した web サービスのユーザーが別のデータ ソースを指定できるようにします。 構成するか、 [ライター][writer] モジュール、別の宛先を指定できるようにします。

Web サービスのパラメーターを定義して、1 つまたは複数のモジュール パラメーターに関連付け、必須かオプションかを指定することができます。 Web サービスのユーザーは、サービスにアクセスするときにこれらのパラメーターの値を指定することができ、モジュールの操作はそれに応じて変更されます。

Web サービス パラメーターの詳細については、次を参照してください [Azure Machine Learning Web サービス パラメーターの使用。
][webserviceparameters]

[webserviceparameters]: machine-learning-web-service-parameters.md


## Web サービスとして予測実験をデプロイする

予測実験の準備を十分に実行したので、Azure Web サービスとしてデプロイできます。 Web サービスを使用してユーザーはデータをモデルに送信でき、モデルは予測を返します。

完全な展開プロセスの詳細については、次を参照してください [Azure Machine Learning web サービスの展開。][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[project-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/


