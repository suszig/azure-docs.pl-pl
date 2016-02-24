<properties 
   pageTitle="Logic Apps での AS2 コネクタの使用 | Microsoft Azure App Service" 
   description="AS2 コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="rajeshramabathiran" 
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

# AS2 コネクタの使用開始とロジック アプリへの追加
AS2 コネクタを使って、AS2 (Applicability Statement 2) トランスポート プロトコルにより企業間通信でメッセージの送受信を行います。 データはインターネット経由で安全かつ確実に転送されます。 セキュリティはデジタル証明書と暗号化を使用して実現されています。

AS2 コネクタをビジネス ワークフローに追加し、ロジック アプリ内の企業間ワークフローの一部としてデータを処理できます。 

## トリガーとアクション
トリガーは、パートナーからの AS2 メッセージの着信などの特定のイベントに応じて新しいインスタンスを起動します。 アクションは、AS2 メッセージを受け取った後に、AS2 を使用してこのメッセージを送信するなどの結果を指します。

AS2 コネクタは、ロジック アプリでトリガーまたはアクションとして使用でき、JSON 形式と XML 形式のデータをサポートします。 AS2 コネクタでは、次のトリガーとアクションを使用できます。 

トリガー | アクション
--- | ---
Receive & Decode | エンコードと送信

## 前提要件
AS2 コネクタで使用する前に、次の項目を自分で作成する必要があります。

要件 | 説明
--- | ---
TPM API アプリ | AS2 コネクタを作成する前に、[BizTalk 取引先管理コネクタ] [1] を作成する必要があります。 <br/><br/>**注** TPM API アプリの名前を確認します。 
Azure SQL Database | パートナー、スキーマ、証明書、契約などの B2B 項目を格納します。 B2B API Apps ごとに専用の Azure SQL Database が必要です。 <br/><br/>**注** このデータベースに接続文字列をコピーします<br/><br/>[。Azure SQL データベースを作成します。](../sql-database-get-started.md)
Azure BLOB Storage コンテナー | AS2 アーカイブが有効な場合に、メッセージのプロパティを格納します。 AS2 メッセージをアーカイブする必要がない場合は、Storage コンテナーは必要ありません。 <br/><br/>**注** アーカイブが有効にする場合は、この Blob ストレージに接続文字列をコピーします<br/><br/>[。Azure ストレージ アカウントについて](../storage-create-storage-account.md)します。

## AS2 コネクタの作成

コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。  

1. Azure のスタート画面では、次のように選択します。 **Marketplace**します。
2. 「AS2 コネクタ」を検索して選択し、選択 **作成**します。
3. 名前、App Service プラン、その他のプロパティを入力します。
4. 次のパッケージの設定を入力します。

    プロパティ |説明
--- | --- 
データベース接続文字列 |作成した Azure SQL Database への ADO.NET 接続文字列を入力します。 接続文字列をコピーしても、パスワードは接続文字列に追加されません。 接続文字列を貼り付ける前に、接続文字列にパスワードを必ず入力してください。
受信メッセージのアーカイブを有効にする |省略可能です。 このプロパティを有効にすると、パートナーから受け取った受信 AS2 メッセージのメッセージ プロパティが格納されます。 
Azure Blob ストレージの接続文字列 |作成した Azure Blob ストレージ コンテナーへの接続文字列を入力します。 アーカイブが有効な場合は、エンコードまたはデコードされたメッセージがこの Storage コンテナーに格納されます。
TPM インスタンス名 |名前を入力、 **BizTalk 取引先管理** 以前に作成した API アプリ。 AS2 コネクタを作成すると、このコネクタではこの特定の TPM インスタンス内の AS2 契約のみを実行します。

5. 選択 **作成**します。 

取引先は、B2B (企業間) コミュニケーションに含まれるエンティティです。 2 つのパートナーがある関係を確立した場合、この関係は "契約" と呼ばれます。 契約は、2 つのパートナーが達成したいと考えているコミュニケーションに基づいて定義され、プロトコルまたはトランスポートに固有のものです。

取引先アグリーメントの作成に必要な手順についてはここで [2]。

## コネクタをトリガーとして使用する

1. を作成またはロジック アプリを編集する場合は、右側のペインから作成した AS2 コネクタを選択します。  
    ![トリガーの設定][3]

2. → 右向きの矢印をクリックします。  
    ![トリガーのオプション][4]

3. AS2 コネクタでは 1 つのトリガーが表示されます。 選択 *Receive & Decode*:  
    ![受信し、デコードの入力][5]

4. このトリガーには、入力がありません。 → 右向きの矢印をクリックします。  
    ![受信およびデコード構成][6]

出力の一部として、コネクタは、AS2 固有のメタデータだけでなく AS2 ペイロードも返します。

AS2 ペイロードが https://{Host URL への post の場合は、トリガーが起動}/decode します。  Host URL は、API アプリ設定で確認できます。  また、アプリケーション設定で API アプリのアクセス レベルをパブリック (認証済みまたは匿名) に変更する必要があります。

## コネクタをアクションとして使用する
1. トリガーの後ろに (または 'このロジックを手動で実行を選択)、右側のペインから作成した AS2 コネクタを追加します。  
    ![アクションの設定][7]

2. → 右向きの矢印をクリックします。  
    ![アクションの一覧][8]

3. AS2 のコネクタはアクションを 1 つのみサポートします。 選択 *エンコードおよび送信*:  
    ![エンコードおよび送信の入力][9]

4. アクションに対する入力値を入力し、それを構成します。  
    ![エンコードおよび送信の構成][10]

    パラメーターは、次のとおりです。 

    パラメーター |型 |説明
--- | --- | ---
ペイロード |素材ペイロードにエンコードし、構成済みのエンドポイントへの投稿の内容。 ペイロードは、JSON オブジェクトとして指定する必要があります。
AS2 から |文字列 |AS2 メッセージの送信側の AS2 の id。 このパラメーターは、メッセージを送信するための適切な契約の参照に使用されます。
AS2 に |文字列 |AS2 メッセージの受信側の AS2 の id。 このパラメーターは、メッセージを送信するための適切な契約の参照に使用されます。
パートナーの URL |文字列 |メッセージが送信する必要があるパートナーのエンドポイント。
アーカイブの有効化 |ブール値 |送信メッセージをアーカイブするかどうかを判断します。

アクションが正常に終了すると、HTTP 200 の応答コードを返します。

## コネクタでできること
ロジック アプリでの詳細について [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。

<!--References -->
[1]: app-service-logic-connector-tpm.md
[2]: app-service-logic-create-a-trading-partner-agreement.md
[3]: ./media/app-service-logic-connector-as2/TriggerSettings.PNG
[4]: ./media/app-service-logic-connector-as2/TriggerOptions.PNG
[5]: ./media/app-service-logic-connector-as2/ReceiveAndDecodeInput.PNG
[6]: ./media/app-service-logic-connector-as2/ReceiveAndDecodeConfigured.PNG
[7]: ./media/app-service-logic-connector-as2/ActionSettings.PNG
[8]: ./media/app-service-logic-connector-as2/ListOfActions.PNG
[9]: ./media/app-service-logic-connector-as2/EncodeAndSendInput.PNG
[10]: ./media/app-service-logic-connector-as2/EncodeAndSendConfigured.PNG

