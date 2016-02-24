<properties
    pageTitle="仮想マシンで Tomcat を実行する | Microsoft Azure"
    description="このチュートリアルでは、従来のデプロイ モデルで作成されたリソースの使用し、Windows 仮想マシンを作成し、Apache Tomcat アプリケーション サーバーを実行するように構成する方法を示します。"
    services="virtual-machines"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="11/19/2015"
    ms.author="robmcm"/>

# クラシック デプロイ モデルで作成された仮想マシンに Java アプリケーション サーバーをインストールする方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


Azure では、仮想マシンを使用してサーバー機能を実現することができます。 たとえば、Apache Tomcat などの Java アプリケーション サーバーをホストできるように、Azure で実行する仮想マシンを構成することができます。 このガイドを完了すると、Azure で実行する仮想マシンを作成して、Java アプリケーション サーバーを実行するように構成する方法を理解できます。

学習内容:

* Java Development Kit (JDK) インストール済みの仮想マシンを作成する方法
* 仮想マシンにリモート ログインする方法
* 仮想マシンに Java アプリケーション サーバーをインストールする方法
* 仮想マシンのエンドポイントを作成する方法
* ファイアウォールでアプリケーション サーバー用にポートを開く方法

このチュートリアルでは、Apache Tomcat アプリケーション サーバーを仮想マシンにインストールします。 インストールが完了した Tomcat の外観は次のようになります。

![仮想マシンで実行されている Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## 仮想マシンを作成するには

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。
2. をクリックして **新規**, 、] をクリックして **コンピューティング**, 、] をクリックして **仮想マシン**, 、] をクリックし、 **ギャラリーから**します。
3.  **仮想マシン イメージの選択** ダイアログ ボックスで、 **JDK 7 Windows Server 2012**します。
なお **JDK 6 Windows Server 2012** は JDK 7 を実行する準備ができていないレガシ アプリケーションがある場合。
4. クリックして **次**します。
5. **[仮想マシンの構成]** ダイアログ ボックスで次の作業を行います。
    1. 仮想マシンの名前を指定します。
    2. 仮想マシンに使用するサイズを指定します。
    3. 管理者の名前を入力、 **ユーザー名** フィールドです。 この名前と次に入力するパスワードは忘れないでください。仮想マシンにリモート ログインするときに使用します。
    4. パスワードを入力、 **新しいパスワード** フィールド、および再入力、 **Confirm** フィールドです。 これは、Administrator アカウントのパスワードです。
    5. クリックして **次**します。
6. 次の **[仮想マシンの構成]** ダイアログ ボックスで次の作業を行います。
    1.  **クラウド サービス**, 、既定値を使用して **新しいクラウド サービスを作成する**です。
    2. 値 **クラウド サービス DNS 名** cloudapp.net で一意である必要があります。 一意であることを示す表示になるように、必要に応じてこの値を修正してください。
    2. リージョン、アフィニティ グループ、または仮想ネットワークを指定します。 このチュートリアルの目的のリージョンを指定よう **米国西部**します。
    2.  **ストレージ アカウント**, [ **自動的に生成されたストレージ アカウントを使用して**します。
    3.  **可用性セット**, [ **(なし)**します。
    4. クリックして **次**します。
7. 最後の **[仮想マシンの構成]** ダイアログ ボックスで次の作業を行います。
    1. 既定のエンドポイント エントリをそのまま使用します。
    2. クリックして **完了**します。

## 仮想マシンにリモート ログインするには

1. ログオン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。
2. クリックして **仮想マシン**します。
3. ログインする仮想マシンの名前をクリックします。
4. 仮想マシンが起動すると、接続を許可するポップアップ メニューがページ下部に表示されます。
5. クリックして **接続**します。
6. 表示される画面で必要に応じて入力して、仮想マシンに接続します。 これにより、接続の詳細が含まれる .rdp ファイルが保存されるか、開きます。 .rdp ファイルの最初の行の最後の部分として、url: ポートをコピーし、それをリモートのログイン アプリケーション内に貼り付けなければならない場合があります。

## 仮想マシンに Java アプリケーション サーバーをインストールするには

Java アプリケーション サーバーを仮想マシンにコピーすることも、インストーラーを使用して Java アプリケーション サーバーをインストールすることもできます。

このチュートリアルでは、Tomcat をインストールします。

1. 仮想マシンにサインインしているときにブラウザー セッションを開きます [Apache Tomcat](http://tomcat.apache.org/download-70.cgi)します。
2. リンクをダブルクリック **32 ビット/64 ビット Windows サービスのインストーラー**します。 この場合、Tomcat は Windows サービスとしてインストールされます。
3. 確認メッセージが表示されたら、インストーラーを実行します。
4. 内で、 **Apache Tomcat Setup** ウィザードの手順に従い、Tomcat をインストールします。 このチュートリアルでは、既定値のまま進めて問題ありません。 達すると、 **Apache Tomcat のセットアップ ウィザードの完了** ダイアログ ボックスで、必要に応じて確認できます **Run Apache Tomcat** に Tomcat を起動します。 クリックして **完了** Tomcat のセットアップ プロセスを完了します。

## Tomcat を開始するには
Tomcat を実行するを選択しなかった場合、 **Apache Tomcat のセットアップ ウィザードの完了** ] ダイアログ ボックスで、仮想マシンでコマンド プロンプトを開き、実行されている開始 **net start Tomcat7**します。

確認 Tomcat 実行されている場合は、バーチャル マシンのブラウザーを実行して、開いた <http://localhost:8080>します。

Tomcat が実行されていることを外部コンピューターから確認するには、エンドポイントを作成してポートを開く必要があります。

## 仮想マシンのエンドポイントを作成するには
1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。
2. クリックして **仮想マシン**します。
3. Java アプリケーション サーバーを実行している仮想マシンの名前をクリックします。
4. クリックして **エンドポイント**します。
5. クリックして **追加**します。
6.  **エンドポイントを追加** ] ダイアログ ボックスで、確実 **スタンドアロン エンドポイントの追加** クリックして選択すると、 **次**します。
7. **[エンドポイントの詳細を指定します]** ダイアログ ボックスで、次の操作を行います。
    1. は、エンドポイントの名前を指定します。たとえば、 **HttpIn**します。
    2. 指定 **TCP** プロトコルです。
    3. 指定 **80** 、パブリック ポートです。
    4. 指定 **8080** プライベート ポートにします。
    5. クリックして、 **完了** ] ダイアログ ボックスを閉じます。 これでエンドポイントが作成されます。

## ファイアウォールで仮想マシン用にポートを開くには
1. 仮想マシンにログインします。
2. クリックして **Windows スタート**します。
3. クリックして **コントロール パネルの [**します。
4. クリックして **システムとセキュリティ**, 、] をクリックして **Windows ファイアウォール**, 、順にクリック **の詳細設定**します。
5. クリックして **受信の規則**, 、] をクリックし、 **新しいルール**します。
 ![新しい受信の規則][NewIBRule]
6.  **規則の種類**, [ **ポート**, 、順にクリック **次**します。
 ![新しい受信の規則のポート][NewRulePort]
7.  **プロトコルおよびポート** 画面で、[ **TCP**, 、指定 **8080** として、 **特定のローカル ポート**, 、順にクリック **次**します。
 ![新しい受信の規則 ][NewRuleProtocol]
8.  **アクション** 画面で、[ **接続を許可する**, 、] をクリックし、 **次**します。
 ![新しい受信の規則の操作][NewRuleAction]
9.  **プロファイル** 画面であることを確認 **ドメイン**, 、**プライベート**, 、および **パブリック** クリックして選択すると、 **次**します。
 ![新しい受信の規則のプロファイル][NewRuleProfile]
10.  **名前** 画面で、規則の名前を指定します。 **HttpIn** (ルールの名前は必要ありませんただしエンドポイント名を一致するように)、順にクリック **完了**します。  
 ![新しい受信の規則の名前][NewRuleName]

この時点では、Tomcat の web サイトを表示するか、外部のブラウザーからフォームの URL を使用して **http://*\_dns\_name*. *.cloudapp.net**、ここで ***\_dns\_name*** 仮想マシンの作成時に指定した DNS 名です。

## アプリケーションのライフサイクルについて
* 独自の web アプリケーション アーカイブ (WAR) を作成しを追加する可能性があります、 **webapps** フォルダーです。 たとえば、Java Service Page (JSP) の基本的な動的 web プロジェクトを作成し、WAR ファイルとしてエクスポートし、WAR をコピーして、Apache Tomcat を **webapps** 、ブラウザーで実行すること、仮想マシン上のフォルダーです。
* 既定では、Tomcat サービスがインストールされると、手動で開始する設定が適用されます。 この設定は、サービス スナップインを使用して、自動的に開始する設定に変更することができます。 サービス スナップインをクリックして開始 **スタート**, 、**管理ツール**, 、し **サービス**します。 ダブルクリックして、 **Apache Tomcat** サービスを提供し、設定 **スタートアップの種類** に **自動**です。

    ![サービスが自動的に起動するように設定する][service_automatic_startup]

    自動的に開始する設定の利点は、仮想マシンが再起動された場合 (再起動を伴うソフトウェア更新プログラムのインストール後など) に、Tomcat を開始できる点です。

## 次のステップ
利用できる情報を表示することによって、Java アプリケーションに含めるをする可能性があります (Azure Storage、service bus、SQL データベースなど) には、他のサービスについて、 [Java デベロッパー センター](http://azure.microsoft.com/develop/java/)します。

[virtual_machine_tomcat]: ./media/virtual-machines-java-run-tomcat-application-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-java-run-tomcat-application-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-java-run-tomcat-application-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-java-run-tomcat-application-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleProfile.png

