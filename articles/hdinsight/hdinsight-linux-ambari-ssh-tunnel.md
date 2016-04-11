<properties
pageTitle="SSH トンネリングを使用して Ambari Web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の Web UI にアクセスする"
description="SSH トンネルを使用して、Linux ベースの HDInsight ノードでホストされている Web リソースを安全に閲覧する方法について説明します。"
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="paulettm"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="11/02/2015"
ms.author="larryfr"/>

#SSH トンネリングを使用して Ambari Web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の Web UI にアクセスする

Linux ベースの HDInsight クラスターは、インターネット経由で Ambari Web UI にアクセスできますが、UI の一部の機能にはアクセスできません。 たとえば、Ambari 経由で表示されるその他のサービスの Web UI などです。 Ambari Web UI の全機能を使用するには、クラスター ヘッドに対して SSH トンネルを使用する必要があります。

##SSH トンネルが必要な機能

Ambari の一部のメニューは、SSH トンネルなしでは完全に表示されません。これは、クラスターで実行されている他の Hadoop サービスが公開している Web サイトとサービスに依存しているためです。 多くの場合、これらの Web サイトはセキュリティで保護されていないので、インターネットで直接公開するのは安全ではありません。 場合によっては、Zookeeper ノードなど、他のクラスター ノードで Web サイトが実行されているサービスもあります。

次のサービスは Ambari Web UI が使用しているサービスですが、SSH トンネルなしではアクセスできません。

* ResourceManager、
* JobHistory、
* NameNode、
* スレッド スタック、
* Oozie Web UI
* HBase Master と Logs の UI
* Storm UI

クラスターをカスタマイズするスクリプト アクションを使用する場合、インストールするサービスまたはユーティリティで Web UI 公開するには、SSH トンネルが必要です。 たとえば、スクリプト アクションを使用して Hue をインストールする場合、SSH トンネルを使用して Hue Web UI にアクセスする必要があります。

##SSH トンネルとは

[Secure Shell (SSH) トンネル](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) ローカル ワークステーションに、HDInsight クラスターのヘッド ノードへの SSH 接続を通じて、ヘッド ノードに送信された場合は、ここで、要求が解決し、ポートに送信されるトラフィックをルーティングします。 応答は、トンネル経由でワークステーションにルーティングされます。

##前提条件

Web トラフィックに SSH トンネルを使用するには、以下が必要です。

* SSH クライアント。 Linux および UNIX のディストリビューション、または Macintosh OS X の場合、オペレーティング システムに `ssh`コマンドが用意されています。 Windows でのことをお勧め [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

    > [AZURE.NOTE] 以外の場合、SSH クライアントを使用するかどうかは `ssh` または PuTTY の SSH トンネルを確立する方法をクライアントのマニュアルを参照してください。

* SOCKS プロキシを使用するように構成できる Web ブラウザー

* __(省略可能)__: などのプラグイン [FoxyProxy](http://getfoxyproxy.org/,) のみトンネル経由の特定の要求をルーティングする規則を適用することができます。

    > [AZURE.WARNING] FoxyProxy などのプラグイン、せず、ブラウザーから作成されたすべての要求は、トンネルを通じてルーティングされます。 その結果、ブラウザーの Web ページの読み込みが低速になる可能性があります。

##<a name="usessh"></a>SSH コマンドを使用してトンネルを作成する

次のように `ssh` コマンドを使用して、SSH トンネルを作成します。 置換 __USERNAME__ を HDInsight クラスターと置換の SSH ユーザー __CLUSTERNAME__ 、HDInsight クラスターの名前に置き換えます

    ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

これで、ローカル ポート 9876 へのトラフィックを SSH 経由でクラスターにルーティングする接続が作成されます。 オプションは次のとおりです。

* **D 9876** -トンネル経由でトラフィックをルーティングするローカル ポートです。

* **C** -web トラフィックは大部分がテキストであるため、すべてのデータを圧縮します。

* **2** -SSH プロトコル バージョン 2 だけを強制的にします。

* **q** -Quiet モードです。

* **T** -ポートの転送だけを行うために、pseudo-tty の割り当てを無効にします。

* **n** -ポートの転送だけを行うために、STDIN を読み取らないを防止します。

* **N** -ポートの転送だけを行うために、リモート コマンドを実行できません。

* **f** -バック グラウンドで実行します。

SSH キーを使用してクラスターを構成した場合は、SSH 秘密キーのパスを `-i` パラメーターで指定する必要があることがあります。

コマンドが完了すると、ローカル コンピューター上でポート 9876 に送信されるトラフィックは、クラスターのヘッド ノードに Secure Sockets Layer (SSL) 経由でルーティングされ、そのヘッド ノードで生成されたかのように見えます。

##<a name="useputty"></a>PuTTY を使用してトンネルを作成する

次の手順に従ってPuTTY を使用して、SSH トンネルを作成します。

1. PuTTY を開き、接続情報を入力します。 PuTTY に慣れていない場合は、次を参照してください。 [SSH Windows から Linux ベースの Hadoop の使用](hdinsight-hadoop-linux-use-ssh-windows.md) HDInsight を使用する方法の詳細。

2.  **カテゴリ** ダイアログ ボックスの左側のセクションで、展開 **接続**, 、展開 **SSH**, 、し、[ **トンネル**します。

3. 次の情報を提供、 **SSH を制御するオプションはポート フォワーディング** フォーム。

    * **ソース ポート** -を転送する場合、クライアント上のポートです。 たとえば、 **9876**します。

    * **移行先** の SSH アドレス Linux ベースの HDInsight クラスター用です。 たとえば、 **mycluster-ssh.azurehdinsight.net**します。

    * **動的** -動的な SOCKS プロキシを有効します。

    ![トンネリング オプションの画像](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. をクリックして **追加** 、設定を追加する **開く** SSH 接続を開きます。

5. プロンプトが表示されたら、サーバーにログインします。 これにより、SSH セッションが確立され、トンネルが有効になります。

##ブラウザーからトンネルを使用する

> [AZURE.NOTE] このセクションの手順は、Linux、Unix、Mac OS X、および Windows システムで自由に利用可能であるために、FireFox ブラウザーを使用します。 Google Chrome、Microsoft Edge、Apple Safari などの最近のブラウザーでも動作するはずですが、一部の手順で使用されている FoxyProxy プラグインがブラウザーによっては動作しない可能性があります。

1. 使用するブラウザーを構成する **localhost:9876** として、 **SOCKS v5** プロキシ。 Firefox の設定は次のようになります。 9876 以外のポートを使用する場合は、そのポート番号に変更します。

    ![Firefox の設定の画像](./media/hdinsight-linux-ambari-ssh-tunnel/socks.png)

    > [AZURE.NOTE] 選択すると **リモート DNS** ドメイン ネーム システム (DNS) 要求は HDInsight クラスターを使用して解決されます。 選択しないと、DNS はローカルに解決されます。

2. トラフィックがトンネルを介してサイトにアクセスするなどしてルーティングされていることを確認 [http://www.whatismyip.com/](http://www.whatismyip.com/) プロキシ設定を有効になっており、Firefox で無効になっています。 設定が有効の場合、IP アドレスは Microsoft Azure データセンター内のコンピューターの IP アドレスになります。

###ブラウザー拡張

ブラウザーをトンネル ワークを使用するように構成する場合、通常、一部のトラフィックをトンネルを経由で送信しないように設定する必要があることがあります。 [FoxyProxy](http://getfoxyproxy.org/) などのブラウザー拡張機能では、URL 要求に対するパターン マッチングがサポートされるため (FoxyProxy Standard または Plus のみ)、特定の URL の要求のみがトンネル経由で送信されます。

FoxyProxy Standard をインストール済みの場合は、次の手順を使用して、HDInsight の トラフィックだけをトンネル経由で転送するように構成します。

1. ブラウザーで FoxyProxy 拡張を開きます。 Firefox などで、アドレス フィールドの横にある FoxyProxy アイコンを選択します。

    ![foxyproxy のアイコン](./media/hdinsight-linux-ambari-ssh-tunnel/foxyproxy.png)

2. 選択 **新しいプロキシの追加**, を選択、 **全般** タブをクリックしのプロキシ名を入力し、 **HDInsightProxy**します。

    ![foxyproxy の全般](./media/hdinsight-linux-ambari-ssh-tunnel/foxygeneral.png)

3. 選択、 **プロキシの詳細** タブをクリックし、次のフィールドを設定します。

    * **ホストまたは IP アドレス** -これは、ローカル コンピューター上の SSH トンネルを使用するため、localhost です。

    * **ポート** -SSH トンネル用に使用するポートです。

    * **SOCKS プロキシ** -ブラウザーのプロキシとしてのトンネルの使用を有効にする場合に選択します。

    * **SOCKS v5** -プロキシに必要なバージョンを設定する場合に選択します。

    ![foxyproxy のプロキシ](./media/hdinsight-linux-ambari-ssh-tunnel/foxyproxyproxy.png)

4. 選択、 **URL パターン** タブをクリックし、選択 **[新しいパターンの**です。 次の使用パターンを定義し、をクリックして **OK**:

    * **パターン名** - **clusternodes** -パターンのフレンドリ名です。

    * **URL パターン** - **\*internal.cloudapp.net\*** のクラスター ノードの内部の完全修飾ドメイン名に一致するパターンを定義します。

    ![foxyproxy のパターン](./media/hdinsight-linux-ambari-ssh-tunnel/foxypattern.png)

4. をクリックして **OK** をプロキシに追加し、閉じます **プロキシ設定**します。

5. FoxyProxy ダイアログの上部にある次のように変更します。 **モードの選択** に **、定義済みのパターンと優先順位に基づいてプロキシを使用して**, 、] をクリックし、 **閉じる**します。

    ![foxyproxy の選択モード](./media/hdinsight-linux-ambari-ssh-tunnel/selectmode.png)

次の手順を実行した後で、文字列を含む Url の要求のみ __internal.cloudapp.net__ SSL トンネルを経由して行われます。

##Ambari Web UI を使用して確認する

クラスターが確立したら、次の手順で、Ambari Web からサービス Web UI にアクセスできることを確認します。

1. ブラウザーで、CLUSTERNAME は HDInsight クラスターの名前、https://CLUSTERNAME.azurehdinsight.net に移動します。

    プロンプトが表示されたら、クラスターの管理者ユーザー名 (admin) とパスワードを入力します。 Ambari Web UI からもう一度入力を求められることがあります。 その場合は、情報を再入力します。

2. Ambari Web UI でページの左側にある一覧から [YARN] を選択します。

    ![YARN が選択された画像](./media/hdinsight-linux-ambari-ssh-tunnel/yarnservice.png)

3. YARN サービス情報が表示されたら、選択 __クイック リンク__します。 クラスターのヘッド ノードの一覧が表示されます。 ヘッド ノードのいずれかを選択し、[ __リソース マネージャー UI__します。

    ![QuickLinks メニューが展開された画像](./media/hdinsight-linux-ambari-ssh-tunnel/yarnquicklinks.png)

    > [AZURE.NOTE] 選択すると、メニューではなく待機インジケーターが取得低速のインターネット接続があったり、ヘッド ノードが非常に高い場合 __クイック リンク__します。 その場合は、サーバーからデータが取得されるまで 1 ～ 2 分待ってから、改めて一覧を表示してみてください。


    > [AZURE.TIP] 低解像度のモニター、またはブラウザー ウィンドウが最大化されていない場合にいくつかのエントリ、 __クイック リンク__ メニューは、画面の右側で切り取ら可能性があります。 その場合は、マウスを使用してメニューを拡大するか、右矢印キーを使用して画面を右にスクロールして、メニューの残りを表示します。

4. 次のようなページが表示されます。

    ![YARN ResourceManager の UI の画像](./media/hdinsight-linux-ambari-ssh-tunnel/yarnresourcemanager.png)

    > [AZURE.TIP] このページの URL に注意してください。ようなことが __http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/クラスター__します。 この URL は、ノードの内部の完全修飾ドメイン名 (FQDN) を使用しているので、アクセスするには SSH トンネルを使用する必要があります。

##次のステップ

ここでは、SSH トンネルを作成し、使用する方法について説明しました。Ambari を使用してクラスターを監視および管理する方法については、次を参照してください。

* [Ambari を使用した HDInsight クラスターの管理](hdinsight-hadoop-manage-ambari.md)

HDInsight での SSH の使用方法の詳細については、以下の記事を参照してください。

* [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

* [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)


