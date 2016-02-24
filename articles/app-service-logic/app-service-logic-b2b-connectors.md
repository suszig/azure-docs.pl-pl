<properties 
    pageTitle="Microsoft Azure App Service の企業間コネクタと API Apps | Microsoft Azure" 
    description="EDI、EDIFACT、AS2、および TPM の各コネクタを作成および構成する方法 (マイクロサービス アーキテクチャ) について説明します。" 
    services="app-service\logic" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="dwrede" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/15/2015" 
    ms.author="mandia"/> 

# Microsoft Azure App Service の企業間コネクタと API Apps
Microsoft Azure App Service (略称: App Service) には、統合環境に不可欠な数多くの BizTalk API Apps が含まれています。 これらの API Apps は、BizTalk Server 内で使用される概念およびツールに基づいていますが、Azure App Service の一部として利用できるようになりました。 

これらの API Apps のカテゴリの 1 つに企業間 (B2B) API Apps があります。 これらの B2B API Apps を使用すると、パートナーの追加、契約の作成、さらに、EDI、AS2、EDIFACT を使用してオンプレミスであらゆる操作を実行することができます。  

これらの B2B API Apps には、"トリガー" と "アクション" という機能があります。 トリガーは、パートナーからの X12 メッセージの着信などの特定のイベントに応じて新しいインスタンスを起動します。 アクションは、X12 メッセージを受け取った後に、AS2 を使用してこのメッセージを送信するなどの結果を指します。


## 企業間コネクタまたは API Apps とは
企業間 (B2B) 機能には、事前に構築された既存の API Apps が含まれています。この API Apps により、AS2、EDI、および EDIFACT を使用して、異なる企業、部門、アプリケーションなどの間で通信できるようになります。 

B2B API Apps には、以下のものがあります。 

コネクタまたは API Apps | 説明
--- | ---
BizTalk 取引先管理 | パートナーおよび契約を使用した企業間 (B2B) リレーションシップを作成する API アプリです。 このリレーションシップでは、AS2、EDIFACT、および X12 プロトコルを使用します。<br/><br/>TPM API App は、AS2 コネクタ、X12 アプリ、または EDIFACT API Apps の基本要件です。 
AS2 コネクタ | AS2 トランスポートを使用してメッセージを送受信するコネクタです。 このコネクタはインターネット経由で安全かつ確実にデータを転送します。
BizTalk EDIFACT | EDIFACT を使用してメッセージを送受信する API アプリです。 EDIFACT は、一般に UN/EDIFACT (United Nations/Electronic Data Interchange For Administration, Commerce and Transport) とも呼ばれており、さまざまな業界で広く使用されています。
BizTalk X12 | X12 プロトコルを使用してメッセージを送受信する API アプリです。 X12 は、一般に ASC X12 (Accredited Standards Committee X12) とも呼ばれており、さまざまな業界で広く使用されています。 


これらの API Apps を使用すると、さまざまな EDI メッセージング タスクを実行できます。 たとえば、AS2 コネクタを使用して、顧客、社内の人事部などの部門、または AS2 を使用するすべてのユーザーとの間で、種類の異なるメッセージ (EDI、XML、フラット ファイルなど) を安全に送受信することができます。 

API Apps は必要な数だけいくつでも簡単に作成することができます。 1 つの API アプリを複数のシナリオまたはワークフローで再利用することもできます。

これはすべてコードを記述せずに行うことができます。 それでは始めましょう。 


## 前提要件
B2B API Apps を作成するには、必要なリソースがいくつかあります。 他の API Apps 内で使用できるようにするには、それらのリソースを事前に作成する必要があります。 要件は次のとおりです。 

要件 | 説明
--- | ---
Azure SQL Database | パートナー、スキーマ、証明書、契約などの B2B 項目を格納します。 B2B API Apps ごとに専用の Azure SQL Database が必要です。 <br/><br/>**注** このデータベースに接続文字列をコピーします<br/><br/>[。Azure SQL データベースを作成します。](../sql-database-get-started.md)
Azure BLOB Storage コンテナー | AS2 アーカイブが有効な場合に、メッセージのプロパティを格納します。 AS2 メッセージをアーカイブする必要がない場合は、Storage コンテナーは必要ありません。 <br/><br/>**注** アーカイブが有効にする場合は、この Blob ストレージに接続文字列をコピーします<br/><br/>[。Azure ストレージ アカウントについて](../storage-create-storage-account.md)
Service Bus 名前空間とそのキー値 | X12 および EDIFACT のバッチ処理データを格納します。 バッチ処理を必要としない場合、Service Bus 名前空間は必要ありません<br/><br/>**。注** バッチ処理が有効にする場合は、これらの値をコピーします<br/><br/>[。Service Bus 名前空間を作成します。](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM インスタンス | BizTalk 取引先管理 (TPM) インスタンスは、AS2 コネクタ、X12 API アプリ、または EDIFACT API アプリを作成するために必要です。 TPM API アプリを作成すると、TPM インスタンスが作成されます。 <br/><br/>**注** TPM API アプリの名前を確認します。 


## API Apps の作成
B2B API Apps は、Azure ポータルまたは REST API を使用して作成できます。 


### REST API を使用した API Apps の作成
[REST API の使用方法に関するドキュメントを参照してください。](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### Azure ポータルでの B2B API Apps の作成
Azure ポータルでは、Logic Apps、Web Apps、または Mobile Apps を作成するときに、B2B API Apps を作成することができます。 または、独自のブレードを使用して作成することもできます。 どちらの方法も簡単なため、ニーズや好みに応じて使い分けることができます。 独自のプロパティを持つ B2B API Apps を最初に作成するユーザーもいます。 その後、Logic Apps/Web Apps/Mobile Apps を作成し、作成した B2B API Apps を追加します。  

次の手順では、[API Apps] ブレードを使用して B2B API Apps を作成します。


#### BizTalk 取引先管理 (TPM) API Apps の作成

> [AZURE.NOTE] AS2 コネクタ、x12 または EDIFACT API アプリを作成するには、BizTalk 取引先管理 (TPM) インスタンスが必要です。 TPM API アプリを作成すると、TPM インスタンスが作成されます。

TPM インスタンスを作成する手順は以下のとおりです。

1. Azure ポータルのスタート画面 (ホーム ページ)、次のように選択します。 **Marketplace**します。 **API Apps** 、すべての既存の API Apps とコネクタを一覧表示します。 こともできます **検索** での特定の B2B API Apps です。
2. 選択 **BizTalk 取引先管理**します。 新しいブレードで、選択 **作成**します。 
3. 次のプロパティを入力します。 

    プロパティ |説明
--- | ---
名前 | TPM インスタンスの任意の名前を入力します。 たとえば、名前を付けます。 *AccountsPayableTPM*します。
パッケージ設定 |ADO.NET の入力 **データベース接続文字列** 作成した Azure SQL Database にします。 <br/><br/>接続文字列をコピーしても、パスワードは接続文字列に追加されません。 接続文字列にパスワードを必ず入力してください。
App Service プラン |支払プランの一覧表示します。 プランは必要なリソースの増減に応じて変更することができます。
価格レベル |Azure サブスクリプション内での価格のカテゴリを一覧表示する読み取り専用のプロパティです。 
リソース グループ |新しいプランを作成または既存のグループを使用します。 Logic Apps、Web Apps、Mobile Apps のすべての API Apps とコネクタが同じリソース グループに属している必要があります。 <br/><br/>[リソース グループを使用して](../resource-group-overview.md) このプロパティについて説明します。 
サブスクリプション |現在のサブスクリプションを一覧表示する読み取り専用のプロパティです。
場所 |Azure サービスをホストする地理的場所。 
スタート画面への追加 |スタート画面 (ホーム ページ) に B2B API アプリを追加する場合に選択します。

4. 選択 **作成**します。 

TPM API アプリ (TPM インスタンス) が作成されたら、AS2 コネクタ、X12 API アプリ、または EDIFACT API Apps を作成できます。 


#### AS2 コネクタの作成

1. Azure ポータルのスタート画面 (ホーム ページ)、次のように選択します。 **Marketplace**します。 **API Apps** 、すべての既存の API Apps とコネクタを一覧表示します。 こともできます **検索** での特定の B2B API Apps です。
2. 選択 **AS2 コネクタ**します。 新しいブレードで、選択 **作成**します。 
3. 次のプロパティを入力します。 

    プロパティ |説明
--- | ---
名前 |AS2 コネクタの任意の名前を入力します。 たとえば、名前を付けます。 *AS2Connector*します。
パッケージ設定 |TPM インスタンス名など、その API アプリに固有の設定を入力します。 <br/><br/>参照してください [AS2 パッケージ設定の追加](#AddAS2Conn) 固有のプロパティは、このトピックにします。 
App Service プラン |支払プランの一覧表示します。 プランは必要なリソースの増減に応じて変更することができます。
価格レベル |Azure サブスクリプション内での価格のカテゴリを一覧表示する読み取り専用のプロパティです。 
リソース グループ |新しいプランを作成または既存のグループを使用します。 [リソース グループを使用して](../resource-group-overview.md) このプロパティについて説明します。 
サブスクリプション |現在のサブスクリプションを一覧表示する読み取り専用のプロパティです。
場所 |Azure サービスをホストする地理的場所。 
スタート画面への追加 |スタート画面 (ホーム ページ) に B2B API アプリを追加する場合に選択します。

    **<a name="AddAS2Conn"></a>AS2 コネクタのパッケージ設定**

    プロパティ |説明
--- | --- 
データベース接続文字列 |作成した Azure SQL Database への ADO.NET 接続文字列を入力します。 接続文字列をコピーしても、パスワードは接続文字列に追加されません。 接続文字列を貼り付ける前に、接続文字列にパスワードを必ず入力してください。
受信メッセージのアーカイブを有効にする |省略可能です。 このプロパティを有効にすると、パートナーから受け取った受信 AS2 メッセージのメッセージ プロパティが格納されます。 
Azure Blob ストレージの接続文字列 |作成した Azure Blob ストレージ コンテナーへの接続文字列を入力します。 アーカイブが有効な場合は、エンコードまたはデコードされたメッセージがこの Storage コンテナーに格納されます。
TPM インスタンス名 |名前を入力、 **BizTalk 取引先管理** 以前に作成した API アプリ。 AS2 コネクタを作成すると、このコネクタではこの特定の TPM インスタンス内の AS2 契約のみを実行します。

4. 選択 **作成**します。 


#### X12 API アプリまたは EDIFACT API Apps の作成

1. Azure ポータルのスタート画面 (ホーム ページ)、次のように選択します。 **Marketplace**します。 **API Apps** 、すべての既存の API Apps とコネクタを一覧表示します。 こともできます **検索** での特定の B2B API Apps です。
2. 選択 **BizTalk x12** または **BizTalk EDIFACT**します。 新しいブレードで、選択 **作成**します。 
3. 次のプロパティを入力します。 

    プロパティ |説明
--- | ---
名前 |B2B API アプリの任意の名前を入力します。 たとえば、名前を付けます。 *EDI850APIApp*します。
パッケージ設定 |TPM インスタンス名など、その API アプリに固有の設定を入力します。 <br/><br/>参照してください [x12 または EDIFACT のパッケージ設定](#AddX12) 固有のプロパティは、このトピックにします。 
App Service プラン |支払プランの一覧表示します。 プランは必要なリソースの増減に応じて変更することができます。
価格レベル |Azure サブスクリプション内での価格のカテゴリを一覧表示する読み取り専用のプロパティです。 
リソース グループ |新しいプランを作成または既存のグループを使用します。 [リソース グループを使用して](../resource-group-overview.md) このプロパティについて説明します。 
サブスクリプション |現在のサブスクリプションを一覧表示する読み取り専用のプロパティです。
場所 |Azure サービスをホストする地理的場所。 
スタート画面への追加 |スタート画面 (ホーム ページ) に B2B API アプリを追加する場合に選択します。

    **<a name="AddX12"></a>X12 API アプリおよび EDIFACT API アプリのパッケージ設定**  

    プロパティ |説明
--- | --- 
データベース接続文字列 |作成した Azure SQL Database への ADO.NET 接続文字列を入力します。 接続文字列をコピーしても、パスワードは接続文字列に追加されません。 接続文字列を貼り付ける前に、接続文字列にパスワードを必ず入力してください。
Service Bus 名前空間 |作成した Service Bus 名前空間を入力します。 バッチ処理が有効な場合にのみ必要です。 
Service Bus 名前空間共有アクセス キー名 |Service Bus 名前空間を作成したアクセス キーを入力します。 バッチ処理が有効な場合にのみ必要です。 
Service Bus 名前空間共有アクセス キーの値 |Service Bus 名前空間を作成したアクセス キーの値を入力します。 バッチ処理が有効な場合にのみ必要です。 
TPM インスタンス名 |名前を入力、 **BizTalk 取引先管理** 以前に作成した API アプリ。 X12 API Apps または EDIFACT API Apps を作成すると、この API アプリでは、この特定の TPM インスタンス内の X12/EDFIACT 契約のみを実行します。

4. 選択 **作成**します。 


## パートナー、契約、証明書、スキーマの追加 
Azure ポータルで、TPM API アプリを開きます。  **コンポーネント** セクションで、パートナー、契約、証明書、およびスキーマを追加します。 

AS2 コネクタ、X12 API Apps、および EDIFACT API Apps にも契約を追加することができます。 


## API Apps の監視
Azure ポータルで、TPM API アプリを開きます。  **操作** ] セクションで、さまざまな管理操作を表示することができます。 たとえば、次のようなことができます。

- 情報イベントやエラー イベントの表示
- ワーカー プロセス (w3wp) のメモリ使用量とスレッド数の表示
- アプリケーションと Web サーバーのログの表示

さらに [Logic Apps を監視](app-service-logic-monitor-your-logic-apps.md)します。


## アプリケーションへの API Apps の追加 
Microsoft Azure App Service では、これらの B2B API Apps を使用できるさまざまな種類のアプリケーションを公開しています。 新しい B2B API Apps を作成することや、既存の B2B API Apps を Logic Apps、Mobile Apps、または Web Apps に追加することができます。 

アプリ内でギャラリーから B2B API Apps を選択するだけで、選択した B2B API Apps が自動的にアプリに追加されます。  

> [AZURE.IMPORTANT] コネクタと以前に作成した API Apps を追加するには、同じリソース グループにロジック アプリ、モバイル アプリ、または Web アプリを作成します。 

次の手順で、B2B API Apps を Logic Apps、Mobile Apps、または Web Apps に追加します。 

1. スタート画面 (ホーム ページ) Azure のポータルに移動、 **Marketplace**, 、ロジック、モバイル、または Web アプリを検索します。 

    新しいアプリを作成する場合は、Logic Apps、Mobile Apps、または Web Apps を検索します。 アプリを選択し、新しいブレードで、選択 **作成**します。 [ロジック アプリの作成](app-service-logic-create-a-logic-app.md) 手順について説明します。 

2. アプリを開き、選択 **トリガーとアクション**します。 

3.  **ギャラリー**, 、B2B API アプリを自動的に追加して、アプリを選択します。 新しい B2B API アプリを作成することもできます。

    > [AZURE.IMPORTANT] AS2 コネクタ、x12、EDIFACT API アプリは TPM インスタンスが必要です。 そのため、新しい B2B API Apps を作成する場合は、最初に TPM API アプリを作成し、次に AS2 コネクタ、X12 API アプリ、または EDIFACT API アプリを作成します。 

4. 選択 **OK** して変更を保存します。 

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 他の B2B リソース

[B2B プロセスの作成](app-service-logic-create-a-b2b-process.md)<br/>
[取引先アグリーメントの作成](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[コネクタと BizTalk API Apps とは](app-service-logic-what-are-biztalk-api-apps.md)


## Logic Apps と Web Apps に関するガイド
[Logic Apps とは](app-service-logic-what-are-logic-apps.md)<br/>
[Web サイトと Azure App Service Web Apps](../app-service-web/app-service-web-overview.md)


## その他のコネクタ

[コネクタと API Apps の一覧](app-service-logic-connectors-list.md)<br/><br/>
[コネクタと BizTalk API Apps とは](app-service-logic-what-are-biztalk-api-apps.md) 

