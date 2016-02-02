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
2. **[新規]**、**[コンピューティング]**、**[仮想マシン]**、**[ギャラリーから]** をクリックします。
3. **[仮想マシン イメージの選択]** ダイアログ ボックスで、**[JDK 7 Windows Server 2012]** を選択します。
**[JDK 6 Windows Server 2012]** は、JDK 7 で実行する準備ができていないレガシ アプリケーションがある場合に表示されることに注意してください。
4. **[次へ]** をクリックします。
5. **[仮想マシンの構成]** ダイアログ ボックスで次の作業を行います。
    1. 仮想マシンの名前を指定します。
    2. 仮想マシンに使用するサイズを指定します。
    3. **[ユーザー名]** フィールドに、管理者の名前を入力します。 この名前と次に入力するパスワードは忘れないでください。仮想マシンにリモート ログインするときに使用します。
    4. **[新しいパスワード]** フィールドにパスワードを入力し、**[確認]** フィールドに再びパスワードを入力します。 これは、Administrator アカウントのパスワードです。
    5. **[次へ]** をクリックします。
6. 次の **[仮想マシンの構成]** ダイアログ ボックスで次の作業を行います。
    1. **[クラウド サービス]** には、既定の **[新しいクラウド サービスの作成]** を使用します。
    2. **[クラウド サービス DNS 名]** の値は cloudapp.net 全体で一意であることが必要です。 一意であることを示す表示になるように、必要に応じてこの値を修正してください。
    2. リージョン、アフィニティ グループ、または仮想ネットワークを指定します。 このチュートリアルでは、**[米国西部]** などのリージョンを指定します。
    2. **[ストレージ アカウント]** で、**[自動的に生成されたストレージ アカウントを使用]** を選択します。
    3. **[可用性セット]** は、**[(なし)]** を選択します。
    4. **[次へ]** をクリックします。
7. 最後の **[仮想マシンの構成]** ダイアログ ボックスで次の作業を行います。
    1. 既定のエンドポイント エントリをそのまま使用します。
    2. **[完了]** をクリックします。

## 仮想マシンにリモート ログインするには

1. ログオン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。
2. **[仮想マシン]** をクリックします。
3. ログインする仮想マシンの名前をクリックします。
4. 仮想マシンが起動すると、接続を許可するポップアップ メニューがページ下部に表示されます。
5. **[接続]** をクリックします。
6. 表示される画面で必要に応じて入力して、仮想マシンに接続します。 これにより、接続の詳細が含まれる .rdp ファイルが保存されるか、開きます。 .rdp ファイルの最初の行の最後の部分として、url: ポートをコピーし、それをリモートのログイン アプリケーション内に貼り付けなければならない場合があります。

## 仮想マシンに Java アプリケーション サーバーをインストールするには

Java アプリケーション サーバーを仮想マシンにコピーすることも、インストーラーを使用して Java アプリケーション サーバーをインストールすることもできます。

このチュートリアルでは、Tomcat をインストールします。

1. 仮想マシンにサインインしているときにブラウザー セッションを開きます [Apache Tomcat](http://tomcat.apache.org/download-70.cgi)します。
2. **32 ビット/64 ビット Windows サービスのインストーラー**のリンクをダブルクリックします。 この場合、Tomcat は Windows サービスとしてインストールされます。
3. 確認メッセージが表示されたら、インストーラーを実行します。
4. **Apache Tomcat Setup** ウィザードの指示に従って、Tomcat をインストールします。 このチュートリアルでは、既定値のまま進めて問題ありません。 **[Completing the Apache Tomcat Setup Wizard]** ダイアログ ボックスが表示されたら、必要に応じて **[Run Apache Tomcat]** チェック ボックスをオンにすることにより、Tomcat を起動することができます。 **[Finish]** をクリックして Tomcat のセットアップ プロセスを完了します。

## Tomcat を開始するには

**[Completing the Apache Tomcat Setup Wizard]** ダイアログ ボックスで Tomcat を実行するように選択しなかった場合、仮想マシンのコマンド プロンプトを開き、**net start Tomcat7** を実行することによって Tomcat を開始することができます。

確認 Tomcat 実行されている場合は、バーチャル マシンのブラウザーを実行して、開いた <http://localhost:8080>します。

Tomcat が実行されていることを外部コンピューターから確認するには、エンドポイントを作成してポートを開く必要があります。

## 仮想マシンのエンドポイントを作成するには

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。
2. **[仮想マシン]** をクリックします。
3. Java アプリケーション サーバーを実行している仮想マシンの名前をクリックします。
4. **[エンドポイント]** をクリックします。
5. **[追加]** をクリックします。
6. **[エンドポイントの追加]** ダイアログ ボックスで、**[スタンドアロン エンドポイントの追加]** チェック ボックスがオンになっていることを確認し、**[次へ]** をクリックします。
7. **[エンドポイントの詳細を指定します]** ダイアログ ボックスで、次の操作を行います。
    1. エンドポイントの [名前] (**HttpIn** など) を指定します。
    2. プロトコルに **TCP** を指定します。
    3. [パブリック ポート] に **80** を指定します。
    4. [プライベート ポート] に **8080** を指定します。
    5. **[完了]** をクリックしてダイアログ ボックスを閉じます。 これでエンドポイントが作成されます。

## ファイアウォールで仮想マシン用にポートを開くには

1. 仮想マシンにログインします。
2. **Windows の [スタート]** をクリックします。
3. **[コントロール パネル]** をクリックします。
4. **[システムとセキュリティ]**、**[Windows ファイアウォール]**、**[詳細設定]** の順にクリックします。
5. **[受信の規則]**、**[新しい規則]** の順にクリックします。
 ![新しい受信の規則][newibrule]
6. **[規則の種類]** で、**[ポート]** を選択して **[次へ]** をクリックします。
 ![新しい受信の規則のポート][newruleport]
7. **[プロトコルとポート]** 画面で、**[TCP]** を選択し、**[特定のローカル ポート]** を選択して「**8080**」と入力し、**[次へ]** をクリックします。
 ![新しい受信の規則][newruleprotocol]
8. **[操作]** 画面で、**[接続を許可する]** を選択し、**[次へ]** をクリックします。
 ![新しい受信の規則の操作][newruleaction]
9. **[プロファイル]** 画面で、**[ドメイン]**、**[プライベート]**、および **[パブリック]** の各チェック ボックスがオンになっていることを確認し、**[次へ]** をクリックします。
 ![新しい受信の規則のプロファイル][newruleprofile]
10. **[名前]** 画面で、**HttpIn** などの規則の名前を指定し (ただし、規則の名前がエンドポイント名と一致する必要はありません)、**[完了]** をクリックします。  
 ![新しい受信の規則の名前][newrulename]

この時点では、Tomcat の web サイトを表示するか、外部のブラウザーからフォームの URL を使用して **http://*your\_DNS\_name*.cloudapp.net**, ここで、 *** \_dns\_name *** 仮想マシンの作成時に指定した DNS 名です。

## アプリケーションのライフサイクルについて

* 独自の Web アプリケーション アーカイブ (WAR) を作成し、**webapps** フォルダーに追加することもできます。 たとえば、Java Service Page (JSP) で基本的な動的 Web プロジェクトを作成して WAR ファイルとしてエクスポートし、仮想マシン上の Apache Tomcat **webapps** フォルダーに WAR をコピーして、ブラウザーで実行することができます。
* 既定では、Tomcat サービスがインストールされると、手動で開始する設定が適用されます。 この設定は、サービス スナップインを使用して、自動的に開始する設定に変更することができます。 サービス スナップインを起動するには、**Windows の [スタート]**、**[管理ツール]**、**[サービス]** の順にクリックします。 ダブルクリックして、 **Apache Tomcat** サービスを提供し、設定 **スタートアップの種類** に **自動**します。

    ![サービスが自動的に起動するように設定する][service_automatic_startup]

    自動的に開始する設定の利点は、仮想マシンが再起動された場合 (再起動を伴うソフトウェア更新プログラムのインストール後など) に、Tomcat を開始できる点です。

## 次のステップ

利用できる情報を表示することによって、Java アプリケーションに含めるをする可能性があります (Azure Storage、service bus、SQL データベースなど) には、他のサービスについて、 [Java デベロッパー センター](http://azure.microsoft.com/develop/java/)します。


[virtual_machine_tomcat]: ./media/virtual-machines-java-run-tomcat-application-server/WA_VirtualMachineRunningApacheTomcat.png 
[service_automatic_startup]: ./media/virtual-machines-java-run-tomcat-application-server/WA_TomcatServiceAutomaticStart.png 
[newibrule]: ./media/virtual-machines-java-run-tomcat-application-server/NewInboundRule.png 
[newruleport]: ./media/virtual-machines-java-run-tomcat-application-server/NewRulePort.png 
[newruleprotocol]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleProtocol.png 
[newruleaction]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleAction.png 
[newrulename]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleName.png 
[newruleprofile]: ./media/virtual-machines-java-run-tomcat-application-server/NewRuleProfile.png 

