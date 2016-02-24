<properties
    pageTitle="Logic Apps での File コネクタの使用 | Microsoft Azure App Service"
    description="File コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
    authors="rajeshramabathiran"
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
    ms.date="11/11/2015"
    ms.author="rajram"/>

# File コネクタの使用開始とロジック アプリへの追加
ファイル システムに接続して、アップロードやダウンロードなど、ホスト コンピューター上のファイルの操作を行います。 ロジック アプリはさまざまなデータ ソースを基にトリガーでき、データの取得と処理のためのコネクタを提供します。 この File コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。 

File コネクタは、ホスト ファイル システムへのハイブリッド接続にハイブリッド接続マネージャーを使用します。

## Logic App 用の File コネクタを作成する ##
File コネクタを使用するには、まず File コネクタ API アプリのインスタンスを作成する必要があります。 そのためには、次の手順に従います。

1.  Azure ポータルの左側にある [+ 新規] を使用して Azure Marketplace を開きます。
2.  [Marketplace]、[API Apps] の順に移動し、"File コネクタ" を検索します。
3.  次のように、File コネクタを構成します。  
![][1]

    - **名前** -File コネクタの名前を指定
    - **パッケージの設定**
        - **ルート フォルダー** -ホスト コンピューター上のルート フォルダーのパスを指定します。 例: D:\FileConnectorTest
        - **Service Bus 接続文字列** -サービス バス接続文字列を指定します。 Service Bus 名前空間の種類が、Basic ではなく Standard になっていることを確認し、Service Bus Relay を使用できるようにします。  Service Bus Relay はハイブリッド接続マネージャーへの接続に使用されます。
    - **App Service プラン** - 選択または App Service プランの作成
    - **価格レベル** -コネクタの価格レベルを選択します
    - **リソース グループ** - 選択するか、コネクタが属するリソース グループの作成
    - **サブスクリプション** -を作成するには、このコネクタを使用したサブスクリプションを選択します。
    - **場所** -コネクタをデプロイするように、地理的な場所を選択します

4. [作成] をクリックします。 新しい File コネクタが作成されます。

## ハイブリッド接続マネージャーの構成 ##
API App のインスタンスを作成したら、このダッシュボードに移動します。  [参照]、[API Apps]、[File コネクタの API Apps の選択] の順にクリックします。  ここから、ハイブリッド接続マネージャーを構成する必要があります。
構成して、Hybrid Connection Manager のトラブルシューティングの詳細については、[Hybrid Connection Manager を使用する] を参照してください。

## Logic App で File コネクタを使用する ##
API アプリが作成されたら、Logic App のアクションとして File コネクタを使用できます。 そのためには、次の手順を実行する必要があります。

1.  新しい Logic App を作成し、File コネクタと同じリソース グループを選択します。 [新しいロジック アプリの作成] の指示に従います。

2.  作成した Logic App で [トリガーとアクション] を選択して Logic Apps デザイナーを開き、フローを構成します。

3.  File コネクタが右側のギャラリーの [API Apps in this resource group (このリソース グループの API Apps)] セクションに表示されます。

4.  [File コネクタ] をクリックして、File コネクタの API アプリをエディターにドロップできます。 File コネクタは、1 つのトリガーと 4 つの操作を公開します。  
![][5]

6.  それぞれに特定のプロパティが表示されます。 トリガーと Get File アクションのプロパティの一覧を次の図に示します。  
![][6]

7. これらを構成すると、フローでトリガーとアクションを使用できるようになります。 同様に、他のアクションも構成できます。

> [AZURE.NOTE] ファイル トリガーは、フォルダーから正常に読み取った後、ファイルが削除されます。

## File コネクタの REST API ##
ロジック アプリの外部のコネクタを使用するには、そのコネクタで公開されている REST API を活用できます。 [参照]、[API App]、[File コネクタ] の順に選択して、この API 定義を表示できます。 ここでこのコネクタで公開されているすべての Api を表示する [概要] セクションの下にある [API の定義] レンズをクリックします。  
![][7]

Api の詳細については、[File コネクタの API の定義] にあります。

## コネクタでできること
コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md)します。

>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Create a new Logic App]: app-service-logic-create-a-logic-app.md
[File Connector API Definition]: https://msdn.microsoft.com/library/dn936296.aspx
[Using the Hybrid Connection Manager]: app-service-logic-hybrid-connection-manager.md

