<properties 
    pageTitle="Azure Machine Learning Web サービスのパラメーターの使用 | Microsoft Azure" 
    description="Azure Machine Learning Web サービスを使用して、Web サービス アクセス時のモデルの動作を変更する方法です。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/04/2015" 
    ms.author="raymondl;garye"/>

#Azure Machine Learning Web サービスのパラメーターの使用

Azure Machine Learning Web サービスは、変更可能なパラメーターを持つモジュールを含む実験を発行すると作成されます。 状況によっては、Web サービスの実行中にモジュールの動作変更が必要になる場合がありますが、 *Web サービスのパラメーター* これを行うことができます。 

一般的な例が [リーダー] [リーダー] モジュールで設定を web サービスにアクセスするときに公開された web サービスのユーザーが別のデータ ソースを指定できるようにします。 または、別の宛先を指定できるように、[ライター] [ライター] モジュールを構成します。 その他のいくつかの例には、[特徴ハッシュ]、[特徴ハッシュ] モジュールのビット数または [フィルターに基づく特徴の選択] の [フィルター ベースの機能の選択] モジュールの目的の特徴の数の変更が含まれます。 

Web サービスのパラメーターを設定し、実験の 1 つまたは複数のモジュール パラメーターに関連付けて、必須か任意かを指定することができます。 Web サービスのユーザーは、Web サービスの呼び出し時にこれらのパラメーターの値を指定できます。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##Web サービスのパラメーターを設定して使用する方法

Web サービスのパラメーターを定義するには、モジュールのパラメーターの横にあるアイコンをクリックして、[Set as web service parameter] を選択します。 すると、新しい Web サービスのパラメーターが作成されて、モジュール パラメーターにつながります。 これで、Web サービスにアクセスしたときにユーザーが Web サービスのパラメーターの値を指定できるようになり、それがモジュール パラメーターに適用されます。

Web サービスのパラメーターを 1 回定義すれば、その実験の他のモジュール パラメーターでも使用できます。 1 つのモジュールのパラメーターに関連付けられている Web サービスのパラメーターを定義する場合、Web サービスのパラメーターに同じ種類の値が想定されるのであれば、その同じパラメーターを他のモジュールでも使用できます。 たとえば、Web サービスのパラメーターが数値の場合、数値が想定されるモジュール パラメーターでのみ使用できます。 Web サービスのパラメーターの値を設定すると、すべての関連付けられたモジュール パラメーターに適用されます。

Web サービスのパラメーターの既定値を指定するかどうかを設定できます。 指定した場合、Web サービスのユーザー向けのパラメーターはオプションとなります。 既定値を指定しない場合、ユーザーは Web サービスにアクセスしたときに値の入力を求められます。

Web サービスのマニュアル (によって提供される、 **API ヘルプ ページ** web サービス内のリンク **ダッシュ ボード** Machine Learning Studio で) web サービスにアクセスするときにプログラムを使用して Web サービスのパラメーターを指定する方法を web サービスのユーザー情報が含まれます。


##例

たとえばがある Azure blob ストレージに情報を送信する [ライター] [ライター] モジュールで実験をします。 Web サービスのユーザーがサービスにアクセスしたときに BLOB ストレージへのパスを変更できるようにする "Blob path" という名前の Web サービスのパラメーターを定義しましょう。

1.  Machine Learning Studio では、それを選択する [ライター] [ライター] モジュールをクリックします。 実験キャンバスの右側の [プロパティ] ウィンドウにプロパティが表示されます。

2.  ストレージの種類を指定します。

    - [ **データの転送先を指定してください**, 、"Azure Blob Storage] を選択します。
    -  **認証の種類を指定してください**, 、"Account"を選択します。
    - Azure BLOB ストレージのアカウント情報を入力します。 
    <p />

3.  右側のアイコンをクリックして、 **コンテナー パラメーターで始まる blob へのパス**します。 次のように表示されています。

    ![Web サービスのパラメーター アイコン][アイコン]

    [Set as web service parameter] を選択します。

    [エントリが追加 **Web サービス パラメーター** Path to blob beginning with container] という名前のプロパティ] ウィンドウの下部にあります。 これが、Web サービスのパラメーターは、この [ライター] [ライター] モジュールのパラメーターに関連付けられています。

4.  Web サービスのパラメーターの名前を変更、名前をクリックして、「Blob path」を入力してキーを押します、 **Enter** キー。 
 
5.  で Web サービスのパラメーターを既定値を指定する名前の右側のアイコンをクリックして、[既定値を提供する] を選択、(たとえば、「container1/output1.csv」)、値を入力し、キーを押して、 **Enter** キー。

    ![Web サービス パラメーター][パラメーター]

6.  クリックして **実行**します。 

7.  クリックして **WEB サービスの発行** web サービスを発行します。

Web サービスのユーザーは、web サービスにアクセスするときに、[ライター] [ライター] モジュールの新しい宛先を指定できるようことができます。

##詳細情報

詳細な例については、 [Web サービス パラメーター](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) 内のエントリ、 [Machine Learning ブログ](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx)します。

Machine Learning web サービスにアクセスする方法の詳細については、次を参照してください。 [発行済みの machine learning web サービスを使用する方法について](machine-learning-consume-web-services.md)します。



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 

