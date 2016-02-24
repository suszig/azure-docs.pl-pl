<properties
    pageTitle="Azure App Service でオンプレミスの SAP サーバーと統合する | Microsoft Azure"
    description="オンプレミスの SAP サーバーと統合する方法について説明します。"
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
    ms.date="12/07/2015"
    ms.author="sameerch"/>


# オンプレミスの SAP サーバーと統合する
SAP コネクタを使用すると、Azure App Services を構成する Web アプリ、モバイル アプリ、ロジック アプリを既存の SAP サーバーに接続できます。 これにより、RFC、BAPI、tRFC を呼び出すだけでなく、オンプレミス ファイアウォールの背後にある場合でも、SAP サーバーに IDOC を送信することもできます。

オンプレミス SAP サーバーがある場合は、ハイブリッド リスナーを使用して、以下のように SAP コネクタとの接続を確立します。

![ハイブリッド接続のフロー][1]

クラウドの SAP コネクタはオンプレミス ファイアウォールの背後にある SAP サーバーに直接接続できませんが、ハイブリッド リスナーを使用してこれを補うことができます。 このように設定するには、リレー エンドポイントをホストします。これにより、コネクタは SAP サーバーへの接続を安全に確立できます。


## SAP と統合するさまざまな方法
次のアクションがサポートされています。

- RFC の呼び出し
- tRFC の呼び出し
- BAPI の呼び出し
- IDOC の送信

## 前提条件
ハイブリッド リスナーをインストールし、実行するクライアント コンピューターには、SAP 固有のクライアント ライブラリが必要です。 詳細については」のセクションの下にある [こちら] [9] にキャプチャされた **SAP アダプター**します。


## 新しい SAP コネクタの作成
1. Azure ポータルにサインインします。
2. 選択 **新しい**します。
3. 作成] ブレードで、選択 **コンピューティング** > **Azure Marketplace**します。
4. Marketplace のブレードで、選択 **API Apps**, 、および SAP の検索バーで検索します。  
    ![SAP コネクタ API アプリ][2]
5. 選択、 **SAP コネクタ** マイクロソフトによって発行されました。
6. SAP コネクタのブレードで、選択 **作成**します。
7. 表示された新しいブレードで、次のように入力します。  
    1. **場所** -コネクタをデプロイするように、地理的な場所を選択します
    2. **サブスクリプション** -を作成するには、このコネクタを使用したサブスクリプションを選択します。
    3. **リソース グループ** - 選択するか、コネクタが属するリソース グループの作成
    4. **Web ホスティング プラン** - 選択するか、web ホスティング プランの作成
    5. **価格レベル** -コネクタの価格レベルを選択します
    6. **名前** -SAP コネクタの名前を入力してください。
    7. **[パッケージ設定]**
        - **サーバー名** -SAP サーバーの名前を入力します。 たとえば、「SAPserver」や「SAPserver.mydomain.com」と入力します。
        - **ユーザー名** -SAP サーバーに接続する有効なユーザー名を入力します。
        - **パスワード** -SAP サーバーに接続する有効なパスワードを入力します。
        - **システム番号** -SAP アプリケーション サーバーのシステム番号を入力します。
        - **言語** -「EN」ように、ログオンの言語を入力します。 値が入力されなかった場合は EN と見なされます。
        - **内部設置型** -SAP サーバーがファイアウォールの背後にあるオンプレミスかどうかを入力します。 TRUE に設定した場合は、SAP サーバーにアクセスできるリスナー エージェントをサーバーにインストールする必要があります。 API アプリの概要ページに移動し、[ハイブリッド接続] を選択してエージェントをインストールできます。
        - **Service bus 接続文字列** -SAP サーバーが内部設置型の場合は、このパラメーターを入力します。 有効な Service Bus 名前空間の接続文字列である必要があります。
        - **Rfc** -コネクタによる呼び出しができる SAP の Rfc を入力します。
        - **Trfc** -コネクタによる呼び出しができる SAP の Trfc を入力します。
        - **BAPI** -コネクタによる呼び出しができる SAP の Bapi を入力します。
        - **Idoc** -コネクタで送信できる SAP の Idoc を入力します。
    8. [選択] を選択します。 数分で SAP コネクタが作成されます。


## ハイブリッド リスナーをインストールする
作成した SAP コネクタを参照: **参照** > **API Apps** > *、コネクタの名前*

コネクタのブレードで、ハイブリッド接続ステータスが保留になっていることを確認してください。 [ハイブリッド接続] を選択します。 ハイブリッド接続のブレードが開きます。  

![ハイブリッド接続ブレード][3]

プライマリ ゲートウェイの構成文字列をコピーします。 この文字列は、後でハイブリッド リスナーの構成の一部として使用します。

選択、 **をダウンロードして構成** リンクします。 以下のクリック ワンス インストーラーが開きます。

![ハイブリッド接続のクリックしてワンス インストーラー][4]

選択 **インストール**, 、以前にコピーしたプライマリ ゲートウェイの構成文字列を入力します。

![リレー リッスン接続文字列][5]

選択 **インストール** ハイブリッド接続マネージャーのセットアップを完了します。

![ハイブリッド接続マネージャーのインストールの進行状況][6]

![ハイブリッド接続マネージャーのインストールが完了][7]

## ハイブリッド接続を検証する
作成した SAP コネクタを参照: **参照** > **API Apps** > *、コネクタの名前*

コネクタのブレードで、ハイブリッド接続ステータスがであることを確認します *接続*:

![ハイブリッド接続状態 - 接続済み][8]


## Logic Apps での SAP コネクタの使用
作成した SAP コネクタは、Logic Apps のワークフローで使用できます。 これを行うには、を通じて新しいロジック アプリを作成 **新規** > **Logic Apps** > **作成**します。 ロジック アプリのメタデータを、リソース グループを含めて入力します。

選択 **トリガーとアクション**します。 Logic Apps のワークフロー デザイナーが開きます。

右側のウィンドウで SAP コネクタを選択し、[アクション] タブでアクションを選択します。

> [AZURE.NOTE] アクションの一覧は、SAP コネクタを作成したときに入力した構成に基づいています。

選択したアクションについては、入力と出力のパラメーターが表示されます。 アクションに対する入力項目に入力することもできます。また、他の API Apps 内の現在のアクションの出力を使用して、ワークフロー内のより詳細な意思決定の可能性に備えることもできます。

<!--Image references-->
[1]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnectivityFlow.PNG
[2]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/SAPConnector.APIApp.PNG
[3]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnection.PNG
[4]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnection.ClickOnceInstaller.PNG
[5]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnection.ClickOnceInstaller.RelayInformation.PNG
[6]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnectionManager.Install.InProgress.PNG
[7]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/HybridConnectionManager.Install.Completed.PNG
[8]: ./media/app-service-logic-integrate-with-an-on-premise-SAP-server/SAPConnector.HybridConnection.Connected.PNG
[9]: http://download.microsoft.com/download/2/D/7/2D7CE8DF-A6C5-45F0-8319-14C3F1F9A0C7/InstallationGuide.htm

