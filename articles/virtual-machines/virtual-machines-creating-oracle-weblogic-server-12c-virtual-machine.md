<properties
    pageTitle="Oracle WebLogic Server 12c VM の作成 | Microsoft Azure"
    description="リソース マネージャーのデプロイ モデルを使用して、Windows Server 2012 で実行する Oracle WebLogic Server 12c 仮想マシンを Microsoft Azure で作成します。"
    services="virtual-machines"
    authors="bbenz"
    documentationCenter=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="06/22/2015"
    ms.author="bbenz" />

#Azure での Oracle WebLogic Server 12c 仮想マシンの作成
次の例は、Windows Server 2012 で実行しているマイクロソフト提供の WebLogic Server 12c のイメージに基づいて、Azure で仮想マシンを作成する方法を示しています。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。



##Azure で Oracle WebLogic Server 12c 仮想マシンを作成するには

1. ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2. をクリックして、 **Marketplace**, 、] をクリックして **コンピューティング**, 、入力 **Oracle** 検索ボックスにします。

3.  選択、 **Windows Server 2012 での Oracle WebLogic Server 12c Standard Edition** または **Windows Server 2012 での Oracle WebLogic Server 12c Enterprise Edition** イメージ。  (最小推奨サイズなど)、このイメージに関する情報を確認し、クリックして **次**します。

4.  指定する **ホスト名**VM のです。

5.  指定する **ユーザー名** VM にします。 なお、このユーザーは VM にリモートログインするためのもので、Oracle データベースのユーザー名ではありませんのでご注意ください。

6.  VM のパスワードを指定し確認するか、または SSH 公開キーを入力します。

7.  選択、 **価格レベル**します。  既定をクリックして、すべての構成オプションを表示することをお勧めの価格レベルが表示されることに注意してください **をすべて表示** 右上にします。

8.  必要に応じてオプションの構成を設定します。このとき、次に注意してください。
    1. ままにして **ストレージ アカウント** としてでは、VM 名で新しいストレージ アカウントを作成します。
    2. ままにして **可用性セット** 「未構成」とします。
    3. いずれかを入れないように **エンドポイント** この時点でします。

9.  選択するか、作成、 [リソース グループ](resource-group-portal.md)

10. 選択、 **サブスクリプション**

11. 選択、 **場所**


##Azure で Oracle WebLogic Server 12c 仮想マシンを構成するには

1. ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2.  クリックして **仮想マシン**します。

3.  ログインする仮想マシンの名前をクリックします。

4.  クリックして **接続**します。

5.  表示される画面で必要に応じて入力して、仮想マシンに接続します。 管理者名とパスワードの入力画面が表示されたら、仮想マシンの作成時に指定した値を使用します。

6.  内で、 **WebLogic Platform クイック スタート** ダイアログ ボックスで、をクリックして **WebLogic Server の概要**します。 (場合、 **WebLogic Platform クイック スタート** ダイアログがまだ開いていないをクリックして開きます **Windows の [スタート**, 入力、 **Start Admin Server for WebLogic Server ドメイン**, 、クリックして、 **Start Admin Server for WebLogic Server ドメイン** アイコンです)。

7.   **へようこそ]** ダイアログで、 **新しい WebLogic ドメインの作成** ] をクリックし、 **次**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image10.png)

8.   **ドメイン ソースの選択** ダイアログ ボックスで、既定値をそのまま使用し、 **次**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image11.png)

9.   **ドメイン名の指定と場所** ダイアログ ボックスで、既定値をそのまま使用し、 **次**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image12.png)

10.  **[管理者ユーザー名とパスワード** ダイアログ。

    1.  [省略可能]ユーザー名を変更 **weblogic** 独自の値にします。

    2.  WebLogic Server 管理者のパスワードを指定し確認します。

    3.  クリックして **次**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image13.png)

11.  **構成サーバー開始モードと JDK** ダイアログで、 **実稼働モード**, 、使用可能な JDK (または必要に応じて JDK のブラウザー) を選択] をクリックし、 **次**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image14.png)

12.  **オプション構成の選択** ] ダイアログ ボックスではどのオプションも選択されず、単順にクリックして **次**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image15.png)

13.  **構成の概要** ダイアログ ボックスで、をクリックして **作成**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image16.png)

14.  **ドメインの作成** ダイアログ ボックスで、チェック **管理サーバーの起動** ] をクリックし、 **実行**します。

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image17.png)

15. コマンド プロンプト **startWebLogic.cmd** が開始します。 メッセージが表示されたら、WebLogic のユーザー名とパスワードを入力します。

##Oracle WebLogic Server 12c 仮想マシンでのアプリケーションを Azure にインストールするには
1.  仮想マシンにログインしたまま、http://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/wls/12c/12-ManageSessions--4478/files/shoppingcart.war ローカルで使用できる shoppingcart.war の例をコピーします。 たとえば、という名前のフォルダーを作成 **c:\mywar** に http://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/wls/12c/12-ManageSessions--4478/files/shoppingcart.war で WAR を保存および **c:\mywar**します。

2.  開いている、 **WebLogic Server 管理コンソール**, 、http://localhost:7001/console です。 メッセージが表示されたら、WebLogic のユーザー名とパスワードを入力します。

3.  内で、 **WebLogic Server 管理コンソール**, をクリックして **ロックし、編集**, 、] をクリックして **展開**, 、順にクリック **インストール**します。

4.   **パス**, 、型 **c:\mywar\shoppingcart.war します。**

    ![](media/virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine/image18.png)

    クリックして **次**します。

5.  を選択**インストールこのデプロイをアプリケーションとして** ] をクリックし、 **次**します。

6.  クリックして **完了**します。

7.  内で、 **WebLogic Server 管理コンソール**, 、クリックして **保存**, 、順にクリック **変更のアクティブ化**します。

8.  をクリックして **展開**, [ **ショッピングカート**, 、] をクリックして **開始**, 、順にクリック **すべての要求**します。 確認メッセージが表示されたら、クリックして **はい**します。

9.  ローカルで実行しているショッピングカート アプリケーションを表示するには、ブラウザーを開く <http://localhost:7001/shoppingcart>

10. 仮想マシンのエンドポイントを作成します。

    1. ログイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

    2.  クリックして **参照**

    3.  クリックして **仮想マシン**

    4.  仮想マシンを選択します

    5.  クリックして **設定**

    6.  クリックして **エンドポイント**します。

    7.  クリックして **追加**します。

    8.  エンドポイントの名前を指定します

        1. 使用 **TCP** プロトコル

        2. 使用 **80** 、パブリック ポート

        3. 使用 **7001** プライベート ポートにします。

    9.  残りのオプションはそのままにします

    10. クリックして **OK**

11. ポート 7001 の受信接続をファイアウォールで許可します。

    1.  仮想マシンにログインします。

    2.  をクリックして **スタート**, 、型 **セキュリティが強化された Windows ファイアウォール**, 、順にクリックし、 **セキュリティが強化された Windows ファイアウォール** アイコン。 開き、 **セキュリティが強化された Windows ファイアウォール** 管理コンソールです。

    3.  ファイアウォール管理コンソール内をクリックして **受信の規則** 、左側のウィンドウ (表示されない場合 **受信の規則**, 、左側のウィンドウの最上位ノードを展開し)、し、右側のウィンドウの [新しい規則] をクリックします。

    4.   **規則の種類**, [ **ポート** ] をクリック **次**します。

    5.   **プロトコルとポート**, を選択 **TCP**, を選択 **特定のローカル ポート**, 、入力 **7001** ] をクリックし、ポートの **次**します。

    6.  選択 **接続を許可する** ] をクリック **次**します。

    7.  規則を適用するプロファイルの既定値をそのまま使用し、クリックして **次**します。

    8.  ルールと、必要に応じて、説明の名前を指定し、クリックして **完了**します。

12. インターネット上で実行しているショッピングカート アプリケーションを表示するには、ブラウザーを開いて `http://<<unique_domain_name>>/shoppingcart` の形式の URL にアクセスします。 (値を指定することができます <<*unique_domain_name*>> 内で、 [Azure ポータル](https://ms.portal.azure.com/) をクリックして **仮想マシン** し、Oracle WebLogic Server を実行している仮想マシンを選択) します。


##その他のリソース
これで Oracle WebLogic Server を実行する仮想マシンのセットアップが終わりました。追加情報については、次のトピックを参照してください。

-   [Oracle 仮想マシン イメージ - 他の考慮事項](virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images.md)

-   [Oracle WebLogic Server 製品ドキュメント](http://www.oracle.com/technetwork/middleware/weblogic/documentation/index.html)

-   [Microsoft Azure で Linux を使用する Oracle WebLogic Server 12c](http://www.oracle.com/technetwork/middleware/weblogic/learnmore/oracle-weblogic-on-azure-wp-2020930.pdf)

-   [Azure の Oracle 仮想マシン イメージ](virtual-machines-oracle-list-oracle-virtual-machine-images.md)

