<properties 
   pageTitle="Logic Apps での Azure Storage Blob コネクタの使用 | Microsoft Azure App Service" 
   description="Azure Storage Blob コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="anuragdalmia" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="11/30/2015"
   ms.author="rajram"/>
   
# Azure Storage Blob コネクタの使用開始とロジック アプリへの追加 
BLOB をアップロード、ダウンロード、BLOB コンテナーから削除するには、Azure Storage BLOB に接続します。 コネクタは、"ワークフロー" の一部として Logic Apps で使用されます。 

## トリガーとアクション
*トリガー* は、発生するイベントです。 たとえば、注文が更新された、新しい顧客が追加された、といったイベントがあります。  *アクション* トリガーの結果を示します。 たとえば、注文が更新されたときに、営業担当者にアラートを送信します。 または、新しい顧客が追加されたときに、ウェルカム メールを新しい顧客に送信します。 

Storage BLOB コネクタは、ロジック アプリでアクションとして使用でき、JSON 形式と XML 形式のデータをサポートします。 現時点では、Storage BLOB コネクタにはトリガーはありません。 

Storage BLOB コネクタでは、次のトリガーとアクションを使用できます。 

トリガー | アクション
--- | ---
なし | <ul><li>Get Blob: コンテナーから特定の Blob を取得します。</li><li>新しい Blob をアップロードまたは既存の Blob を更新する Blob のアップロード。</li><li>Delete Blob: 特定の Blob コンテナーから削除します</li><li>Blob の一覧: ディレクトリ内のすべての blob を一覧表示します。</li><li>スナップショット Blob: 特定の Blob の読み取り専用スナップショットを作成します。</li><li>コピー Blob: では、新しい Blob を作成するために別の Blob をコピーします。  コピー元の BLOB は、同じアカウントのものであっても、別のアカウントのものであってもかまいません。</li></ul>


## Azure Storage BLOB コネクタの作成

コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。  

1. Azure のスタート画面では、次のように選択します。 **Marketplace**します。
2. "Blob"を検索します。  
    ![Azure Storage Blob コネクタの選択][2]

3. 選択し、選択 **作成**します。
4. 名前、App Service プラン、その他のプロパティを入力します。
5. 次のパッケージの設定を入力します。

    名前 |必要な | 説明
--- | --- | ---
コンテナー/SAS URI |[はい |Blob コンテナーの URI を入力します。 URI には SAS トークンを含めることもできます。 たとえば、入力 http://*storageaccountname*.blob.core.windows.net/containername または http://*storageaccountname*.blob.core.windows.net/containername?sr=c & si mypolicy & sig = = signatureblah
キーへのアクセス |いいえ |有効なプライマリまたはセカンダリ ストレージ アカウントのアクセス キーを入力します。 認証に SAS トークンを使用する場合は、このフィールドは空白のままにしてください。

    ![Azure Storage Blob コネクタの作成][3]

6. クリックして **作成**します。

## ロジック アプリでの Azure Storage BLOB コネクタの使用
Azure Storage BLOB コネクタを作成すると、ワークフローに追加できます。

1. 新しいロジック アプリを作成するには、[新規]、[Web + モバイル]、[ロジック アプリ] の順に選択します。 ロジック アプリのプロパティを入力します。  
    ![ロジック アプリの作成][4]

2. クリックして **トリガーとアクション**します。 次のようなワークフロー デザイナーが開きます。  
    ![ロジック アプリの空のフロー デザイナー][5]

3. 右側のウィンドウから [Azure Storage BLOB コネクタ] を選択します。 コネクタには、実行可能なアクションが一覧表示されます。  
    ![Azure Storage Blob アクションの一覧][10]

4. このシナリオで追加して、 **Blob のアップロード** アクション。  
    ![Blob のアップロード アクションの入力][11]

5. 入力値を入力し、チェック マークを選択して構成を完了します。

    入力 |説明
--- | ---
Blob パス |アップロードする Blob のパスを決定します。 パスは構成済みのコンテナーのパスを基準として解釈されます。
Blob コンテンツの書き込み |アップロードするには、コンテンツと、Blob のプロパティを入力します。
コンテンツ転送エンコード |[なし] または [Base64 です。
上書き |True の場合、既存の Blob に設定する場合は上書きされます。 false に設定すると、同じパスに BLOB が既に存在する場合にエラーが返されます。

構成済みの Azure Storage BLOB の BLOB のアップロードという BLOB アクションにより、入力パラメーターと出力パラメーターの両方が表示されます。

#### Azure Storage BLOB アクションへの入力として前のアクションの出力を使用する
ショットでは、前の画面で、 **コンテンツ** 値が式を指定できます。

    @triggers().outputs.body.Content

任意の値に設定できます。 式はロジック アプリのトリガーの出力を取得し、アップロードするファイルのコンテンツとして使用します。 たとえば、変換の出力を使用することができます。 このシナリオでは、次の式になります。

    @actions('transformservice').outputs.body.OutputXML

## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。

<!-- Image reference -->
[2]: ./media/app-service-logic-connector-azurestorageblob/SelectAzureStorageBlobConnector.PNG
[3]: ./media/app-service-logic-connector-azurestorageblob/CreateAzureStorageBlobConnector.PNG
[4]: ./media/app-service-logic-connector-azurestorageblob/CreateLogicApp.PNG
[5]: ./media/app-service-logic-connector-azurestorageblob/LogicAppEmptyFlowDesigner.PNG
[6]: ./media/app-service-logic-connector-azurestorageblob/ChooseBlobAvailableTrigger.PNG
[7]: ./media/app-service-logic-connector-azurestorageblob/BasicInputsBlobAvailableTrigger.PNG
[8]: ./media/app-service-logic-connector-azurestorageblob/AdvancedInputsBlobAvailableTrigger.PNG
[9]: ./media/app-service-logic-connector-azurestorageblob/ConfiguredBlobAvailableTrigger.PNG
[10]: ./media/app-service-logic-connector-azurestorageblob/ListOfAzureStorageBlobActions.PNG
[11]: ./media/app-service-logic-connector-azurestorageblob/BasicInputsUploadBlob.PNG
 

