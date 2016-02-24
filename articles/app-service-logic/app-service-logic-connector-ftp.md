<properties
    pageTitle="Logic Apps での FTP コネクタの使用 | Microsoft Azure App Service"
    description="FTP コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
    authors="rajram"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/30/2015"
    ms.author="rajram"/>

# FTP コネクタの使用開始とロジック アプリへの追加
FTP サーバーに接続して、データまたはファイルを移動します。 FTP コネクタの主な機能は、次のとおりです。

- 必要に応じて FTP サーバーからファイルを取得する
- 構成可能なスケジュールに基づいてポーリングを実行する
- FTP サーバーにポーリングし、FTP サーバー上の新しいドキュメントに基づいてロジック フローをトリガーする
- IP アドレス、ポート、パスワード、およびホスト名として FTP サーバーを指定する
- 必要に応じて送信を実行する
- 必要に応じて FTP サーバー上のファイルを削除する

この FTP コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。 

## 新しい FTP コネクタの作成
新しい FTP コネクタを作成するには、次の手順に従います。
- Azure ポータルを起動します。
- 開いている Azure marketplace を使用して + (ページの下部にある) で新規 Web + モバイル]-> [Azure Marketplace を-->:  
![Azure Marketplace の起動][1]

- [API Apps] をクリックします。
- FTP を検索し、[FTP コネクタを選択します。  
![FTP コネクタの選択][2]

- [作成] をクリックします。
- FTP コネクタのブレードが開いたらでは、次のデータを指定します。  
![FTP コネクタの作成][3]

- **場所** -コネクタをデプロイするように、地理的な場所を選択します
- **サブスクリプション** -を作成するには、このコネクタを使用したサブスクリプションを選択します。
- **リソース グループ** - 選択するか、コネクタが属するリソース グループの作成
- **Web ホスティング プラン** - 選択するか、web ホスティング プランの作成
- **価格レベル** -コネクタの価格レベルを選択します
- **名前** -FTP コネクタの名前を指定
- **[パッケージ設定]**
    - **サーバーのアドレス** -FTP サーバーの名前または IP アドレス指定
    - **ユーザー名** -FTP サーバーに接続するユーザー名を指定
    - **パスワード** -FTP サーバーへの接続にパスワードを指定します。
    - **ルート フォルダー** -ルート フォルダーのパスを指定します。
    - **バイナリを使用して** -バイナリ転送モードを使用して、ASCII の場合は false の場合は true を指定
    - **SSL を使用して** -セキュリティで保護された SSL/TLS チャネル経由で FTP を使用する場合は true を指定します。
    - **サーバー ポート** -FTP サーバーのポート番号を指定
- [作成] をクリックします。 新しい FTP コネクタが作成されます。

## ロジック アプリでの FTP コネクタの使用
作成した FTP コネクタはフローから使用できます。

新しいフローを作成するには、[+ 新規]、[Web + モバイル]、[ロジック アプリ] の順に選択します。 リソース グループを含むフローのメタデータを提供します。  
![ロジック アプリの作成][4]

をクリックして *トリガーとアクション*します。 フローのデザイナーが開きます。  
![ロジック アプリの空のフロー デザイナー][5]

FTP コネクタは、トリガーとアクションの両方として使用することができます。

### トリガー
空のフロー デザイナーでは、右側のギャラリー ウィンドウの [FTP コネクタ] をクリックします。  
![FTP トリガーの選択][6]

FTP コネクタには、"File Available (Read then Delete)" というトリガーが 1 つあります。 このトリガーによって以下が実行されます。

- 新しいファイルのフォルダー パスをポーリングする
- 新しいファイルごとにロジック フローをインスタンス化する
- ロジック フローをインスタンス化した後、フォルダー パスからファイルを削除する

をクリックして"File Available (Read then Delete)' トリガー。  
![基本的な入力の FTP トリガー][7]

入力すると、スケジュールされた頻度でポーリングする特定のフォルダー パスを構成できます。 基本的な入力内容は次のとおりです。
- [頻度] - FTP ポーリングの頻度を指定します。
- [間隔] - スケジュールした頻度の間隔を指定します。
- [フォルダーのパス] - FTP サーバーのフォルダーのパスを指定します。
- [ファイルの種類] - ファイルの種類がテキストであるかまたはバイナリであるかを指定します

クリックすると、省略記号 [...] 詳細な入力を示しています。  
![基本的な入力の FTP トリガー][8]

詳細な入力は次のとおりです。
- [ファイル マスク] - ポーリング時のファイル マスクを指定します。
- [除外ファイル マスク] - ポーリング時に除外するファイル マスクを指定します。

入力を指定し、入力の構成を完了するチェック マークをクリックします。  
![基本的な入力の FTP トリガー][9]

構成した FTP トリガーには、出力パラメーターと入力パラメーターの両方が表示されています。

#### 後続のアクションでの FTP トリガーの出力の使用
FTP コネクタの出力は、フロー内の他のアクションの入力として使用できます。

[...] をクリックすることができます。 アクションを選択、ドロップ ダウン ボックスを直接から FTP の出力の入力] ダイアログ ボックス。

アクションの入力ボックスに直接式を記述することもできます。 FTP トリガーの出力を参照するためのフロー式を次に示します。

    @triggers('ftpconnector').outputs.body.Content

### アクション
右側のウィンドウの [FTP コネクタ] をクリックします。 サポートされているアクションを FTP コネクタのリスト:  
![FTP のアクションの一覧][10]

FTP コネクタでは、次の操作がサポートされています。

- **ファイルを入手** -特定のファイルの内容を取得
- **ファイルをアップロード** -FTP フォルダー パスにファイルをアップロードします
- **ファイルを削除** -FTP フォルダー パスからファイルを削除
- **ファイルを一覧表示** -FTP フォルダー パス内のすべてのファイルを一覧表示

ここでは、1 つの例として [ファイルのアップロード] を使用します。 [ファイルのアップロード] をクリックします。

基本的な入力内容が表示されます。  
![ファイルのアップロード アクションの基本的な入力内容][11]


- **コンテンツ** -アップロードするファイルの内容を指定
- **コンテンツ転送エンコード** -none を指定または [base64
- **ファイル パス** -アップロードするファイルのファイル パスを指定

クリックして... の詳細な入力。  
![ファイルのアップロード アクションの基本的な入力内容][12]


- **追加が存在する** - True または False '場合に追加する存在' です。 有効に設定すると、ファイルが存在する場合にファイルにデータが追加されます。 無効に設定すると、ファイルが存在する場合にファイルは上書きされます。
- **一時フォルダー** - 省略可能です。 指定した場合、アダプターは、ファイルを "一時フォルダー パス" にアップロードします。アップロードが完了すると、ファイルは "フォルダー パス" に移動されます。 移動操作がアトミックであるためには、"一時フォルダー パス" が "フォルダー パス" と同じ物理ディスクにある必要があります。 一時フォルダーは、"存在する場合に追加" プロパティが無効の場合にのみ使用できます。

入力を指定し、入力の構成を完了するチェック マークをクリックします。  
![構成ファイルのアップロード アクション][13]

[ファイル パス] のパラメーターは次のようになります。

    @concat('/Output/',triggers().outputs.body.FileName)

構成した FTP のファイルのアップロード アクションには、出力パラメーターと入力パラメーターの両方が示されています。

#### FTP アクションへの入力として前のアクションの出力を使用する
構成済みのスクリーンショットでは、コンテンツは式に設定される値です。

    @triggers().outputs.body.Content


任意の値に設定できます。 これは一例です。 式はロジック アプリのトリガーの出力を取得し、アップロードするファイルのコンテンツとして使用します。 前のアクションの出力を使用する場合は、変換を使います。 この場合、式は次のようになります。

    @actions('transformservice').outputs.body.OutputXML

## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-ftp/LaunchAzureMarketplace.PNG
[2]: ./media/app-service-logic-connector-ftp/SelectFTPConnector.PNG
[3]: ./media/app-service-logic-connector-ftp/CreateFTPConnector.PNG
[4]: ./media/app-service-logic-connector-ftp/CreateLogicApp.PNG
[5]: ./media/app-service-logic-connector-ftp/LogicAppEmptyFlowDesigner.PNG
[6]: ./media/app-service-logic-connector-ftp/ChooseFTPTrigger.PNG
[7]: ./media/app-service-logic-connector-ftp/BasicInputsFTPTrigger.PNG
[8]: ./media/app-service-logic-connector-ftp/AdvancedInputsFTPTrigger.PNG
[9]: ./media/app-service-logic-connector-ftp/ConfiguredFTPTrigger.PNG
[10]: ./media/app-service-logic-connector-ftp/ListOfFTPActions.PNG
[11]: ./media/app-service-logic-connector-ftp/BasicInputsUploadFile.PNG
[12]: ./media/app-service-logic-connector-ftp/AdvancedInputsUploadFile.PNG
[13]: ./media/app-service-logic-connector-ftp/ConfiguredUploadFile.PNG
 

