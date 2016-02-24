<properties
    pageTitle=" API Management を使用した AzureML Web サービスの管理方法について説明します | Microsoft Azure"
    description="API Management を使用した AzureML Web サービスの管理方法について説明するガイドです。"
    keywords="machine learning,api management"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="paulettm"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="roalexan" />


# API Management を使用した AzureML Web サービスの管理方法

##概要

このガイドでは、API Management を使用して AzureML Web サービスを管理する方法について簡単に説明します。

##Azure API Management とは

Azure API Management は、ユーザー アクセス、使用帯域幅の調整、ダッシュボードの監視を定義することで、REST API エンドポイントを管理できる Azure のサービスです。 クリックして [ここ](http://azure.microsoft.com/services/api-management/) Azure API Management の詳細。 クリックして [ここ](api-management/api-management-get-started.md) Azure API Management を使用する方法についてです。 このガイドがベースとなる他のガイドでは、通知の構成、価格レベル、応答の処理、ユーザー認証、製品、開発者のサブスクリプション、使用状況のダッシュボードなどのトピックについて説明します。

##Azure ML とは

AzureML は、高度な分析ソリューションを簡単に構築、デプロイ、共有できる、機械学習用の Azure サービスです。 クリックして [ここ](http://azure.microsoft.com/services/machine-learning/) AzureML の詳細。

##前提条件

このガイドを完了するには、以下が必要です。

* Azure アカウント。 Azure アカウントを取得していない場合はクリックして [ここ](http://azure.microsoft.com/pricing/free-trial/) 無料の試用アカウントを作成する方法の詳細。
* AzureML アカウント。 AzureML アカウントを取得していない場合はクリックして [ここ](https://studio.azureml.net/) 無料の試用アカウントを作成する方法の詳細。
* Web サービスとしてデプロイされる AzureML 実験用のワークスペース、サービス、api_key。 クリックして [ここ](machine-learning/machine-learning-create-experiment.md) AzureML 実験を作成する方法の詳細。 クリックして [ここ](machine-learning/machine-learning-publish-a-machine-learning-web-service.md) AzureML を展開する方法の詳細については、web サービスとして実験します。 また、シンプルな AzureML 実験を作成してテストし、Web サービスとしてデプロイする方法については付録 A をご覧ください。

##API Management インスタンスの作成

API Management を使用して、AzureML Web サービスを管理する手順を次に示します。 まず、サービス インスタンスを作成します。 ログイン、 [旧ポータル](https://manage.windowsazure.com/) ] をクリック **新規** > **App Services** > **API Management** > **作成**します。

![create-instance](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

一意な指定 **URL**します。 このガイドでは **demoazureml** – 別のものを選択する必要があります。 必要な選択 **サブスクリプション** と **地域** サービス インスタンスのです。 それらを選択したら、次に進むボタンをクリックします。

![create-service-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

値を指定して、 **組織名**します。 このガイドでは **demoazureml** – 別のものを選択する必要があります。 電子メール アドレスを入力して、 **管理者の電子メール** フィールドです。 API Management システムからの通知には、この電子メール アドレスが使用されます。

![create-service-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

チェック ボックスをクリックすると、サービス インスタンスが作成されます。 *新しいサービスを作成するには、最大で 30 分かかります*します。

##API の作成

サービス インスタンスが作成されたら、API を作成します。 API は、クライアント アプリケーションから呼び出すことのできる一連の操作で構成されます。 API の操作は、既存の Web サービスに引き渡されます。 このガイドでは、既存の AzureML RRS と BES Web サービスにプロキシする API を作成します。

API は API パブリッシャー ポータルから作成され、構成されます。このポータルには、Azure クラシック ポータルからアクセスします。 パブリッシャー ポータルに到達するには、サービス インスタンスを選択します。

![select-service-instance](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

クリックして **管理** 、API Management サービスの Azure クラシック ポータルで。

![manage-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

をクリックして **Api** から、 **API Management** 、左側のメニューをクリックし、 **API の追加**します。

![api-management-menu](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

型 **AzureML Demo API** として、 **Web API 名**します。 型 **https://ussouthcentral.services.azureml.net** として、 **Web サービスの URL**します。 型 **azureml demo** として、 **Web API URL サフィックス**します。 確認 **HTTPS** として、 **Web API URL** スキームです。 選択 **スターター** として **製品**します。 終了したら、クリックして **保存** 、API を作成します。

![add-new-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##操作の追加

をクリックして **追加操作は** この API に操作を追加します。

![add-operation](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

 **新しい操作** ウィンドウが表示されます、 **署名** タブが既定で選択されます。

##RRS 操作の追加

まず、AzureML RRS サービスの操作を作成します。 選択 **POST** として、 **HTTP 動詞**します。 型 **/workspaces/{workspace}/services/execute? api バージョン = {apiversion} & 詳細 = {details}** として、 **URL テンプレート**します。 型 **RRS Execute** として、 **表示名**します。

![add-rrs-operation-signature](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

をクリックして **応答** > **を追加** 左側に **200 OK**します。 クリックして **保存** をこの操作を保存します。

![add-rrs-operation-response](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##BES 操作の追加

RRS 操作の追加の場合とよく似ているため、BES 操作のスクリーン ショットは用意されていません。

###バッチ実行ジョブの送信 (開始はしない)

クリックして **追加操作は** 、AzureML BES 操作を API に追加します。 選択 **POST** の **HTTP 動詞**します。 型 **/workspaces/{workspace}/services/{service}/jobs? api バージョン = {apiversion}** の **URL テンプレート**します。 型 **BES Submit** の **表示名**します。 をクリックして **応答** > **を追加** 左側に **200 OK**します。 クリックして **保存** をこの操作を保存します。

###バッチ実行ジョブを送信する

クリックして **追加操作は** 、AzureML BES 操作を API に追加します。 選択 **POST** の **HTTP 動詞**します。 型 **/workspaces/{workspace}/services/jobs/{jobid}/start? api バージョン = {apiversion}** の **URL テンプレート**します。 型 **BES Start** の **表示名**します。 をクリックして **応答** > **を追加** 左側に **200 OK**します。 クリックして **保存** をこの操作を保存します。

###バッチ実行ジョブの状態または結果を取得する

クリックして **追加操作は** 、AzureML BES 操作を API に追加します。 選択 **取得** の **HTTP 動詞**します。 型 **/workspaces/{workspace}/services/jobs/{jobid} でしょうか。 api バージョン = {apiversion}** の、 **URL テンプレート**します。 型 **BES Status** の **表示名**します。 をクリックして **応答** > **を追加** 左側に **200 OK**します。 クリックして **保存** をこの操作を保存します。

###バッチ実行ジョブの削除

クリックして **追加操作は** 、AzureML BES 操作を API に追加します。 選択 **削除** の **HTTP 動詞**します。 型 **/workspaces/{workspace}/services/jobs/{jobid} でしょうか。 api バージョン = {apiversion}** の、 **URL テンプレート**します。 型 **BES Delete** の **表示名**します。 をクリックして **応答** > **を追加** 左側に **200 OK**します。 クリックして **保存** をこの操作を保存します。

##開発者ポータルから操作を呼び出す

開発者ポータルには、API の操作を見てテストするための便利が環境が用意されており、操作を直接呼び出すことができます。 このガイドの手順でを呼び出しますが、 **RRS Execute** メソッドに追加された、 **AzureML Demo API**します。 クリックして **デベロッパー ポータル** 上にあるメニューから従来のポータルの右です。

![developer-portal](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

クリックして **Api** をクリックし、上部のメニューから **AzureML Demo API** 利用可能な操作を表示します。

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

選択 **RRS Execute** 操作にします。 クリックして **やって**します。

![try-it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

要求パラメーターを入力、 **ワークスペース**,  、**サービス**, 、**2.0** の **apiversion**, 、および  **true** の **詳細**します。 検索することができます、 **ワークスペース** と **サービス** AzureML web サービス ダッシュ ボードで (を参照してください **web サービスをテスト** 付録 A の)。

要求ヘッダーをクリックして **追加ヘッダー** と種類 **コンテンツの種類** と **-application/json**, 、順にクリックして **[ヘッダーの追加** と種類 **承認** と **ベアラー <YOUR AZUREML SERVICE API-KEY>**します。 検索することができます、 **api キー** AzureML web サービス ダッシュ ボードで (を参照してください **web サービスをテスト** 「付録 A)。

型 **{「入力」: {"input1": {"ColumnNames": ["Col2"],"Values": ["This is a good day"]}}、"GlobalParameters": {}}** 要求の本体にです。

![azureml-demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

クリックして **送信**します。

![send](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

操作が呼び出されると、開発者ポータルに表示されます、 **要求された URL** バックエンド サービスから、 **応答のステータス**, 、 **応答ヘッダー**, 、および **応答内容**します。

![response-status](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##付録 A - シンプルな AzureML Web サービスを作成しテストする

###実験の作成

シンプルな AzureML 実験を作成し、Web サービスとしてデプロイする手順を次に示します。 Web サービスは、任意のテキストの列を入力として取得し、整数として表される機能のセットを返します。 次に例を示します。

テキスト | ハッシュされたテキスト
--- | ---
This is a good day | 1 1 2 2 0 2 0 1

最初に、移動、任意のブラウザーを使用して: [https://studio.azureml.net/](https://studio.azureml.net/) し、ログイン資格情報を入力します。 次に、新しい空白の実験を作成します。

![search-experiment-templates](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

名前を変更して、 **SimpleFeatureHashingExperiment**します。 展開 **保存されたデータセット** ドラッグ **Amazon の書評** を実験にします。

![simple-feature-hashing-experiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

展開 **データ変換** と **操作** ドラッグ **プロジェクト列** を実験にします。 接続 **Amazon の書評** に **列を投影**します。

![project-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

をクリックして **プロジェクト列** ] をクリックし、 **列セレクターの起動** 選択 **Col2**します。 チェック マークをクリックして、これらの変更を適用します。

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

展開 **テキスト分析** ドラッグ **特徴ハッシュ** を実験にします。 接続 **列を投影** に **特徴ハッシュ**します。

![connect-project-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

型 **3** の **ハッシュ ビットサイズ**します。 これにより、8 (23) 列が作成されます。

![hashing-bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

この時点ですることもクリックして **実行** 実験をテストします。

![run](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###Web サービスの作成

Web サービスを作成します。 展開 **Web サービス** ドラッグ **入力** を実験にします。 接続 **入力** に **特徴ハッシュ**します。 ドラッグしても **出力** を実験にします。 接続 **出力** に **特徴ハッシュ**します。

![output-to-feature-hashing](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

クリックして **web サービスを発行**します。

![publish-web-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

クリックして **はい** 実験を発行します。

![yes-to-publish](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###Web サービスをテストする

AzureML web サービスは、RSS (要求/応答サービス) と BES (バッチ実行サービス) のエンドポイントで構成されます。 RSS は、同期の実行用です。 BES は、同期ジョブの実行用です。 次のサンプルの Python ソースを使用して web サービスをテストするには、ダウンロードして、Azure SDK for Python インストールする必要があります (を参照してください: [Python をインストールする方法](python-how-to-install.md))。

必要があります、 **ワークスペース**, 、**サービス**, 、および **api_key** の次のサンプルのソース用に実験します。 いずれかをクリックして、ワークスペースとサービスを検索できます **要求/応答** または **バッチ実行** web サービス ダッシュ_ボードの実験用です。

![find-workspace-and-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

検索することができます、 **api_key** を web サービス ダッシュ_ボードの実験をクリックします。

![find-api-key](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####RRS エンドポイントのテスト

#####テスト ボタン

RRS エンドポイントをテストする簡単な方法では、[ **テスト** web サービス ダッシュ ボードにします。

![test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

型 **これは、1 日に適切な** の **col2**します。 チェック マークをクリックします。

![enter-data](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

次のような結果が表示されます。

![sample-output](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####サンプル コード

クライアント コードから RRS テストする方法もあります。 クリックすると **要求/応答** ダッシュ ボードと、最下部までスクロールでは、c#、Python、および R. のサンプル コードを表示しますを、要求 URI を含む RRS 要求の構文を表示、ヘッダー、および本文です。

このガイドでは、Python の機能例について説明します。 使用して変更する必要があります、 **ワークスペース**, 、**サービス**, 、および **api_key** 、実験のです。

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####BES エンドポイントのテスト
クリックして **バッチ実行** ダッシュ ボードで、最下部までスクロールします。 C# の場合、Python、R のサンプル コードが表示されます。また、ジョブを送信する、ジョブを送信する、ジョブを開始する、ジョブのステータスか結果を取得する、ジョブを削除するなどの BES 要求の構文も表示されます。

このガイドでは、Python の機能例について説明します。 使用して変更する必要がある、 **ワークスペース**, 、**サービス**, 、および **api_key** 、実験のです。 さらに、変更する必要があります、 **ストレージ アカウント名**, 、**ストレージ アカウント キー**, 、および **ストレージ コンテナー名**します。 場所を変更する必要がありますが、最後に、 **入力ファイル** およびの場所、 **出力ファイル**します。

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()

