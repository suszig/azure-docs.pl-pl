<properties
   pageTitle="ロジック アプリでの Azure Machine Learning コネクタの使用 | Microsoft Azure App Service"
   description="Azure Machine Learning コネクタを作成、構成し、Azure App Service のロジック アプリで使用する方法"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="11/11/2015"
   ms.author="jehollan"/>


# 概要

ロジック アプリ用 Azure ML コネクタを使用すると、バッチ スコアリング (バッチ実行サービス) および再トレーニングのために Azure ML API を呼び出すことができます。 これらの機能とロジック アプリのトリガーを使用すると、バッチ ジョブのスケジュールや、スケジュールされたモデルの再トレーニングのセットアップを行うことができます。

 ![][1]

## Azure Machine Learning コネクタの使用開始とロジック アプリへの追加

まず、Azure ML Studio で実験を作成し、Web サービスをセットアップしてデプロイします。 API の URL を使用することができますし、およびバッチ Exaction ヘルプ ページ BES Post URL のキーが見つかった。 ([詳細](https://github.com/Azure/azure-content/blob/master/articles/machine-learning/machine-learning-walkthrough-5-publish-web-service.md))

コネクタを使用して BES ジョブを実行するには、ロジック アプリに Azure ML コネクタを追加します。 次に、必要な情報を入力します (詳細については後述)。
Retraining をセットアップする 2 つ目の Azure ML コネクタを追加し、入力パラメーターを指定する (を参照してください [ここ](machine-learning-retrain-models-programmatically.md) 再トレーニングのためのモデルを設定する方法の詳細)。

## Azure ML バッチ実行ジョブの実行

Azure ML コネクタは、バッチ実行 (BES) ジョブを実行する次の 4 つのオプションを提供します。
1.  バッチ ジョブの入力と出力: 実験が web サービスの入力と出力モジュール
2.  バッチ ジョブなし入力と出力: 実験には、web サービスの入力またはモジュール (例: 使用しているリーダーとライター モジュール) の出力はありません。
3.  バッチ ジョブでのみ入力: 実験が web サービス入力モジュールが、web サービスには、モジュール出力なし (例: ライター モジュールを使用する)
4.  バッチ ジョブで出力のみ: 実験には、web サービスの入力モジュールがありませんが、web サービスはモジュールの出力 (例: リーダー モジュールを使用する)
BES は非同期の要求である、データのサイズと、モデルの複雑さに応じて時間がかかる可能性があることに注意してください。 ジョブが完了すると、コネクタは出力結果を返します。

### バッチ実行: 入力と出力がある場合

Studio の実験に web サービスの入力と出力モジュールがある場合は、blob のストレージ アカウントと活動場所に関する情報を提供する必要があります ([詳細](machine-learning-consume-web-services.md))します。 場合に、グローバル (web サービス) のパラメーターを含めるさらに、実験での設定 ([詳細](machine-learning-web-service-parameters.md))します。

![][2]

グローバル パラメーターのフィールドの表示と非表示を切り替えるには、省略記号ボタンをクリックします。 これで、フィールドと値のコンマ区切りリストによって 1 つ以上のグローバル (Web サービス) パラメーターを指定できます。

![][3]

BES ジョブの他のバリエーション (Web サービスの入力または出力がないジョブなど) も、コネクタを通じて使用できます。

## 再トレーニングのセットアップ

ML モデルの 1 回限りの再トレーニングまたはスケジュールされた再トレーニングをセットアップするには、再トレーニングのセットアップ アクションを使用します。 
コネクタから作成したバッチ実行ジョブと組み合わせて、Web サービスのモデルのトレーニングと更新の手順を完了できます。 このワークフローでは、コネクタを 2 回使用します。
1.  最初のコネクタは、BES ジョブを実行し、モデルを再トレーニングして出力を返すために使用されます。 この実行の出力には、新しいモデル (.ilearner) の URL が含まれます。 また、オプションとして、実験でセットアップしてあれば、評価モジュールの出力 (csv ファイル) の URL を返すこともできます。
次の手順では、評価モジュール出力内のデータを使用して、Web サービス内のモデルを置き換えるかどうかを判断できます (たとえば、精度 > 0.85 の場合)。
1.  2 つ目のコネクタは、再トレーニングをセットアップするために使用されます。 1 つ目のコネクタの出力パラメーターを使用して必要に応じて更新モデル条件のチェックし、新しくトレーニングを行ったモデルを使用して web サービスを更新します。
  * たとえば、を入力して、新しくトレーニングを行ったモデルへの URL の BES ジョブの出力にアクセスできます `@{body('besconnector') します。Results.output2.FullURL}` 再トレーニング モデルの URL] フィールドにします。 これは、トレーニング実験では web サービスの出力の output2 が呼び出されていると仮定します。
  * リソース名には、予測実験 MyTrainedModel [トレーニング済みのモデル] などで保存したトレーニング済みモデルの完全な名前を使用します。
  * 評価結果のキー フィールド (それが含まれている) 場合に、トレーニング実験の評価モジュールの出力で返されるパラメーターのいずれかを入力できます。 使用可能なパラメーターの一覧を表示するには、Azure ML Studio でトレーニング実験の評価モジュールの結果を視覚化します。 分類実験の場合、含まれているのは Accuracy、Precision、Recall、F-Score、AUC、Average Log Loss、Training Log Loss などです。

![][4]

### スケジュールされた再トレーニング

ロジック アプリのトリガーを使用すると、スケジュールに従って実行されるようにコネクタをセットアップできます。 そうすることで、新しいデータの取得時に定期的にモデルを再トレーニングできます。 BES ジョブはモデルを再トレーニングし、再トレーニング アクションは再トレーニングの完了後にモデルを更新します。

![][5]

## コネクタの出力

**BES**: バッチ ジョブが正常に完了した後、コネクタの出力には、各 Web サービス出力に関する以下の情報が含まれています。

 ![][6]

Web サービス出力を含めなかった場合は、これを使用できないことに注意してください (たとえば、Studio の実験の DB への書き込みにライター モジュールを使用している場合)。

**再トレーニング**: 再トレーニングが正常に完了した後、出力には以下の情報が含まれています。

![][7]

## 概要

ロジック アプリ用 Azure ML コネクタを使用すると、バッチ スコアリング ジョブと再トレーニング ジョブを、オンデマンドまたは定期的なスケジュールで実行することができます。 2 つのアクションを組み合わせると、自動的にデータをスコア付けし、Web サービスのモデルを再トレーニング、評価、更新できます。この場合、コードを記述する必要はありません。


[1]: ./media/app-service-logic-connector-azureml/img1.png 
[2]: ./media/app-service-logic-connector-azureml/img2.png 
[3]: ./media/app-service-logic-connector-azureml/img3.png 
[4]: ./media/app-service-logic-connector-azureml/img4.png 
[5]: ./media/app-service-logic-connector-azureml/img5.png 
[6]: ./media/app-service-logic-connector-azureml/img6.png 
[7]: ./media/app-service-logic-connector-azureml/img7.png 

