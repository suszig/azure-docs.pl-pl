<properties
    pageTitle="Azure 向け Hello World アプリケーションを Eclipse で作成する"
    description="Azure Toolkit for Eclipse を使用してシンプルな Hello World アプリケーションを作成する方法について説明します。"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="11/19/2015" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# Azure 向け Hello World アプリケーションを Eclipse で作成する #

以降の手順では、Azure Toolkit for Eclipse を使って基本的な JSP アプリケーションを作成し Azure にデプロイする方法を説明しています。 簡潔にするために JSP の例を紹介していますが、Azure のデプロイに関する限り、Java サーブレットの場合も手順はほぼ同じです。

ここでは次のようなアプリケーションを作成します。

![][ic600360]

## 前提条件 ##

* A Java Developer Kit (JDK) v 1.7 以降。
* Eclipse IDE for Java EE Developers Indigo 以降。 これはからダウンロードできる <http://www.eclipse.org/downloads/>します。
* Java ベースの Web サーバーまたはアプリケーション サーバーのディストリビューション (Apache Tomcat、GlassFish、JBoss Application Server、Jetty、IBM® WebSphere® Application Server Liberty Core など)。
* Azure サブスクリプション。 から入手できます <http://azure.microsoft.com/pricing/purchase-options/>します。
* Azure Toolkit for Eclipse。 詳細については、次を参照してください。 [Azure Toolkit for Eclipse をインストールする][]です。

## Hello World アプリケーションを作成するには ##

最初に、Java プロジェクトを作成します。

*  Eclipse を起動し、順にクリックして **ファイル**, 、クリックして **新規**, 、順にクリック **[Dynamic Web Project**します。 (表示されない場合 **[Dynamic Web Project** をクリックすると、使用可能なプロジェクトとして表示されている **ファイル**, 、**新規**, 、次の操作:] をクリックして **ファイル**, 、] をクリックして **新規**, 、] をクリックして **プロジェクト…]**, 、展開 **Web**, 、] をクリックして **[Dynamic Web Project**, 、] をクリック **次**.)
*  このチュートリアルの目的で、プロジェクトに名前を **MyHelloWorld**します。 (この名前を必ず使用してください。以降の手順は、WAR ファイルの名前が MyHelloWorld であることを前提にしています)。 画面は次のようになります。
    ![][ic589576]
* クリックして **完了**します。
* Eclipse の Project Explorer ビューでは、展開 **MyHelloWorld**します。 右クリック **WebContent**, をクリックして **新規**, 、順にクリック **JSP ファイル**します。
*  **New JSP File** ダイアログ ボックスで、[ファイル名 **index.jsp**します。 親フォルダーを保持 **Myhelloworld/webcontent**, 、次に示すように。
    ![][ic659262]
*  **Select JSP Template** ダイアログ ボックスで、このチュートリアルの選択のため **New JSP File (html)** ] をクリック **完了**します。
* Index.jsp ファイルが Eclipse で開いたら、動的に表示するテキストの追加 **Hello World!** 既存 `<body>` 要素。 更新した `<body>` の内容は次のようになります。
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* index.jsp を保存します。

## アプリケーションをすばやく簡単に Azure にデプロイするには ##

Java Web アプリケーションをテストする準備が整ったらすぐに、以下に示す簡単な手順に従い、Azure クラウド上で直接アプリケーションをテストすることができます。

1. Eclipse のプロジェクト エクスプ ローラーでクリックして **MyHelloWorld**します。
1. Eclipse のツールバーをクリックして、 **Publish to Azure Cloud** ] ボタン
    ![][ic710882]
1. このアプリケーションを Azure に発行するのが初めてで、アプリケーションの Azure デプロイ プロジェクトを過去に作成したことがない場合、Azure デプロイ プロジェクトが自動的に作成されます。 次のプロンプトが表示され、アプリケーションを実行する目的で自動的にデプロイされる JDK パッケージとアプリケーション サーバーが表示されます。
    ![][ic789598]
    このショートカットを使う方法 Azure で特定のサーバーまたは、既定の設定とは異なる JDK を構成することがなく、アプリケーションをテストする迅速かつ簡単な方法です。 既定で問題がなければをクリックする **OK** 、次の手順を続行します。
    ただし、JDK を変更する場合またはアプリケーション サーバー、アプリケーションの使用をそれには後で、自動的に作成された Azure デプロイ プロジェクトを編集することによって、またはクリックすることができます **キャンセル** ここで、読み取り、 **は Azure デプロイ プロジェクト セクション** このチュートリアルのです。
1.  **Publish to Azure** ダイアログ。
    1. 選択するサブスクリプションがない場合、 **サブスクリプション** まだを一覧表示すると、サブスクリプション情報をインポートする次の手順します。
        1. クリックして **発行設定ファイルからインポート**します。
        1.  **Import Subscription Information** ダイアログ ボックスで、をクリックして **発行設定ファイルのダウンロード**します。 まだ Azure アカウントにログインしていない場合は、ログインするように求められます。 その後、Azure 発行設定ファイルを保存するよう求めるメッセージが表示されます。 このファイルは、ローカル コンピューターに保存してください。
        1. 引き続き、 **サブスクリプション情報のインポート** ダイアログ ボックスで、をクリックして、 **参照** ボタンをクリックし、前の手順でローカルに保存した発行設定ファイルを選択し、をクリックして **開く**します。 画面は次のようになります。
            ![][ic644267]
        1. Click **OK**.
    1.  **サブスクリプション**, 、展開に使用サブスクリプションを選択します。
    1.  **ストレージ アカウント**, を使用して、またはをクリックするストレージ アカウントを選択 **新規** 新しいストレージ アカウントを作成します。
    1.  **サービス名**, を使用して、またはをクリックするクラウド サービスを選択して **新規** 新しいクラウド サービスを作成します。
    1.  **ターゲット OS**, 、展開に使用するオペレーティング システムのバージョンを選択します。
    1.  **対象となる環境**, このチュートリアルでの選択、 **ステージング**します。 (この値を変更を運用サイトにデプロイする準備ができたら、 **実稼働**.)
    1. 省略可能: であることを確認 **以前のデプロイ上書き** 新しいデプロイを自動的に以前のデプロイを上書きするかどうかはオンになっています。 このオプションを有効にしておくと、同じ場所に発行したときに "409 競合" の問題が発生しなくなります。
        注意してください、 **Publish to Azure** ダイアログには、セクションが含まれています。 **リモート アクセス**します。 既定ではリモート アクセスが無効になっており、この例では有効にしません。 リモート アクセスを有効にする場合は、リモートからログインする際に使用するユーザー名とパスワードを入力します。 リモート アクセスの詳細については、次を参照してください。 [Eclipse での Azure のデプロイのリモート アクセスを有効にすると][]です。
         **Publish to Azure** 次のようなダイアログが表示されます。
        ![][ic719488]
1. クリックして **発行** ステージング環境に発行します。
    フル ビルドを実行するメッセージが表示されたら、クリックして **はい**します。 初回ビルドには数分かかる場合があります。
     **Azure のアクティビティ ログ** Eclipse のタブ付きビューのセクションが表示されます。
    ![][ic719489]
    このログを使用するだけでなく **コンソール** ビューが、展開の進行状況を確認します。 代わりにログインする方法が、 [Azure 管理ポータル][], を使用して、 **クラウド サービス** 状態を監視する」セクション。
1. 展開が正常に展開される、 **Azure のアクティビティ ログ** の状態で表示されます **公開**します。 クリックして **公開済み**, 次の図のようとお使いのブラウザーがデプロイのインスタンスを開きます。
    ![][ic719490]

フォーム http://&lt; の DNS 名になりますので、これは、ステージング環境にデプロイされた、*guid*& gt;。DNS 名のほか、アプリケーションのサフィックス.cloudapp.net と URL が含まれます。 たとえば、http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld です。 (、 **MyHelloWorld** 部分は大文字小文字を区別します)。Azure Platform Management Portal (管理ポータルの [Cloud Services] の部分) でデプロイ名をクリックして、DNS 名を確認することもできます。

このチュートリアルは、ステージング環境に展開するでしたが、運用環境に展開する同じ手順以外に従います、 **Publish to Azure** ダイアログで、 **運用** の代わりに **ステージング** の **対象となる環境**します。 運用環境にデプロイした場合、ステージング環境へのデプロイで使用された GUID ではなく、選択した DNS 名が URL に使用されます。

>[AZURE.WARNING] この時点で、Azure アプリケーションをクラウドに展開しておきます。 ただし、先に進む前に注意してください。デプロイされたアプリケーションは実行中以外の時間もサブスクリプションの課金の対象になります。 そのため、不要なデプロイを Azure サブスクリプションから削除することがきわめて重要となります。

## Azure デプロイ プロジェクトについて ##

1 つ以上の Java アプリケーションを Azure にデプロイするには、Azure デプロイ プロジェクトが必要です。 このプロジェクトは、アプリケーションを Azure に発行するためにラップする必要のある "パッケージ" の役割を果たします。

Azure デプロイ プロジェクトには、アプリケーションの情報に加え、デプロイの重要な構成要素についての情報が格納されます。とりわけ重要なのは、Web アプリを実行するためのアプリケーション サーバー コンテナーとその実行環境となる Java ランタイムです。 その選択肢として、Azure は、豊富な Java ランタイムと Java アプリケーション サーバーをサポートしています。

ここでは、わかりやすくするためにごく単純な例を用いていますが、Azure デプロイ プロジェクトには他にも重要な構成情報を含めることができます。実際のアプリケーションではそれらの構成情報を使って、さまざまな要素をほぼ制限なく組み合わせ、スケーラビリティと可用性に優れた多層クラウド サービスを作成することが可能です。 有効にできます **セッション アフィニティ ("sticky sessions")**, 、**キャッシュ高速**, 、**リモート デバッグ**, 、**SSL オフロード**, 、**ファイアウォールとポートのルーティング**, 、**リモート アクセス**, 、およびその他の強力な機能の数。

(「を展開する、アプリケーションを Azure に迅速かつ簡単な方法」) には、このチュートリアルの前のセクションを完了した時点では今すぐ表示に自動的に生成され、という名前のプロジェクト エクスプ ローラーで新しい Azure デプロイ プロジェクト"**MyHelloWorld_onAzure**"です。

このチュートリアルと同様の手順に従えば、空の Azure デプロイ プロジェクトを自分で作成し、既存のアプリケーションを追加することもできます。 その場合、必要な作業は増えますが、その分、当初から初期構成を細かく制御することができます。

新しい Azure デプロイ プロジェクトを最初から作成するにはクリックして、 **新しい Azure デプロイ プロジェクト** ボタン ![][ic710876]します。

既存の Azure デプロイ プロジェクトを使用しているか、ゼロから作成しているかに関係なく、その構成設定やコンポーネント (JDK やアプリケーション サーバーなど) は、いつでも同じように簡単に変更することができます。

既存の Azure デプロイ プロジェクトで、JDK、アプリケーション サーバー、またはアプリケーションの一覧を変更するには、次の操作を実行します。

1. プロジェクト ノードを展開 (例: **MyHelloWorld_onAzure**) プロジェクト エクスプ ローラーで
2. 右クリック **WorkerRole1**
3. 展開、 **Azure** コンテキスト メニューのサブメニュー
4. クリックして **サーバーの構成**

ここで紹介した手順に従って既存の Azure デプロイ プロジェクトを編集することによってサーバーを構成した場合も、新しいプロジェクトをゼロから作成してサーバーを構成した場合も、同じ種類のダイアログが表示され、そのダイアログで JDK、サーバー、アプリケーション コンポーネントを構成することができます。 JDK、アプリケーション サーバーを変更し、追加または展開では、アプリケーションを削除する例については、これらのダイアログで設定を変更する方法の詳細をご覧ください、 [サーバー構成プロパティ][] 記事です。

## Windows のみ: アプリケーションをコンピューティング エミュレーターにデプロイするには ##

>[AZURE.NOTE] Azure エミュレーターは Windows でのみ使用できます。 Windows 以外のオペレーティング システムをご利用の場合、このセクションはスキップしてください。

前述の手順に従いアプリケーションを Azure に発行することによって暗黙的に新しい Azure デプロイ プロジェクトを作成した場合、JDK とアプリケーション サーバーはクラウド用に構成されます。つまり、ローカル エミュレーション用には構成されていません。 プロジェクトをローカル エミュレーターでテストするには、次の手順に従います。

1. Eclipse のプロジェクト エクスプ ローラーでクリックして **MyHelloWorld_onAzure**します。
1. 右クリックして **WorkerRole1**します。
1. 展開、 **Azure** コンテキスト メニューのサブメニュー。
1. クリックして **サーバー構成**します。
1.  **JDK** かどうかツールキットが事前に構成された、既定値をチェック] タブで、ローカル JDK します。 ない場合、または既定値を変更する場合は、確認、 **ローカルでテストするためにこのファイル パスから JDK を使用** オンにして使用する JDK のインストール場所を指定します。 変更する場合は、クリックして、 **参照** ボタンをクリックし、参照機能付きコントロールを使用して、使用する JDK のディレクトリの場所を選択します。
1. クリックして、 **Server** ] タブをクリックします。
1.  **ローカル サーバーのパス** ] ダイアログ ボックスの下部にあるテキスト ボックスの種類と選択した状態] ダイアログ ボックスの上部にある [サーバーのメジャー バージョン番号に一致するローカルでインストールされたサーバーのパスを入力して、 **この種類のサーバーを展開** チェック ボックスをオンします。 異なる種類または異なるメジャー バージョンのアプリケーション サーバーを使う場合は、最初に [Deploy a server of this type] チェック ボックスの選択を変更してください。
1. Click **OK**.
1. Eclipse のツールバーで、クリックして、 **Run in Azure Emulator** ボタン、 ![][ic710879]です。 場合、 **Run in Azure Emulator** ボタンが有効になっていないことを確認して **MyHelloWorld_onAzure** Eclipse の Project Explorer で選択され、現在のウィンドウとして Eclipse の Project Explorer がフォーカスを持つことを確認します。 まずプロジェクトのフル ビルドが開始され、その後、コンピューティング エミュレーターで Java Web アプリケーションが起動します (コンピューターの性能特性によっては、最初のビルドに数秒から数分かかる場合がありますが、それ以降のビルドはもっと短時間で完了します)。最初のビルド ステップの完了後、ご利用のコンピューターへの変更をコマンドに許可するかどうかの確認が、Windows ユーザー アカウント制御 (UAC) から求められます。 クリックして **はい**します。

>[AZURE.IMPORTANT] UAC プロンプトが表示されない場合は、Windows タスク バーに UAC アイコンを確認し、最初にクリックします。 UAC プロンプトが最前面に表示されない場合がありますが、その場合でも、タスク バーにはアイコンとして表示されます。

1. コンピューティング エミュレーターの UI から出力された情報を見て、プロジェクトに問題がないかどうかを確認します。 デプロイの内容によっては、コンピューティング エミュレーター内でアプリケーションが完全に起動するまでにしばらく時間がかかる場合があります。
1. ブラウザーを起動し、アドレスとして `http://localhost:8080/MyHelloWorld` という URL を指定します (URL の `MyHelloWorld` 部分は大文字と小文字が区別されます)。 MyHelloWorld アプリケーション (index.jsp の出力) が表示されます次の図のようにします。
    ![][ic589579]

クリックして、アプリケーションを Eclipse のツールバーで、コンピューティング エミュレーターで実行を停止する準備ができたら、 **Reset Azure Emulator** ボタン ![][ic710880]します。

## デプロイを削除するには ##

Azure Toolkit for Eclipse 内でデプロイを削除することを確認 **MyHelloWorld_onAzure** は Eclipse の Project Explorer で選択されていることを確認して、Eclipse のプロジェクト エクスプ ローラーと、をクリックし、現在のウィンドウ、 **発行の取り消し** ボタン、 ![][ic710883], 、Eclipse のツールバー。 (右クリックして、同じ操作を実行できます **MyHelloWorld_onAzure** をクリックすると、Eclipse の Project Explorer で **Azure** クリックし、 **Undeploy from Azure Cloud**.)これが表示されます、 **Unpublish Azure Project** ダイアログ。

![][ic719491]

配置を含むサブスクリプションとクラウド サービスを選択して、削除、およびをクリックする展開を選択 **発行の取り消し**します。

(展開を削除するこのツールキットを使用する代わりに、使用する、 **クラウド サービス** Azure 管理ポータルのセクション: に移動してデプロイを選択して、してクリックして、 **削除** ] ボタンをクリックします。 この場合、デプロイは停止されてから削除されます。 のみのデプロイを停止するおよび自体は削除されず、場合は、 **停止** の代わりにボタンをクリックして、 **削除** ] ボタンが、上で述べた場合は、展開を削除しないと、料金が継続されます、デプロイが停止している場合でも)。

## 関連項目 ##

[Azure Toolkit for Eclipse][]

[Azure Toolkit for Eclipse のインストール][] 

[Azure Toolkit for Eclipse の新機能][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センター][]します。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png

