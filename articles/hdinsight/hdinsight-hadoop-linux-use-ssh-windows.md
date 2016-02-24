<properties
   pageTitle="Windows と Linux ベースのクラスターの Hadoop の間で SSH キーを使用する | Microsoft Azure"
   description="SSH キーを作成して、Linux ベースの HDInsight クラスターに対する認証に使用する方法について説明します。 PuTTY SSH クライアントを使用して Windows ベースのクライアントからクラスターに接続します。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="11/16/2015"
   ms.author="larryfr"/>

#HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux、Unix、OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) コマンド ライン インターフェイスを使用して、Liux ベースの HDInsight クラスターでの操作をリモートで実行することができます。 このドキュメントでは、PuTTY SSH クライアントを使用した Windows ベースのクライアントからの HDInsight への接続について説明します。

> [AZURE.NOTE] この記事の手順では、Windows ベースのクライアントを使用するいると仮定します。 Linux、Unix、または OS X クライアントを使用している場合は、次を参照してください。 [SSH Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)です。

##前提条件

* **PuTTY** と **PuTTYGen** Windows ベースのクライアントです。 これらのユーティリティは [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) から入手できます。

* HTML5 をサポートする最新の Web ブラウザー

または

* [Mac、Linux および Windows 用の azure CLI](../xplat-cli-install.md)します。

##SSH とは

SSH は、リモート サーバーにログインしたり、リモート サーバーでコマンドをリモート実行したりするためのユーティリティです。 Linux ベースの HDInsight では、SSH によりクラスター ヘッド ノードへの暗号化された接続が確立され、コマンドの入力に使用するコマンド ラインが提供されます。 コマンドは、直接サーバーで実行されます。

###SSH ユーザー名

SSH ユーザー名は HDInsight クラスターの認証に使用する名前です。 クラスター作成中に SSH ユーザー名を指定するとき、このユーザーがクラスターのすべてのノードで作成されます。 クラスターを作成したら、このユーザー名を使用し、HDInsight クラスター ヘッド ノードに接続できます。 ヘッド ノードから、個々のワーカー ノードに接続できます。

###SSH パスワードまたは公開キー

SSH ユーザーはパスワードと公開キーのいずれかを認証に利用できます。 パスワードはユーザーが考えたテキスト文字列です。公開キーはユーザーを一意に識別するために生成された暗号化キー ペアの片割れです。

キーはパスワードより安全ですが、追加の手順でキーを生成する必要があり、キーが入っているファイルを安全な場所に保管しなければなりません。 そのキー ファイルにアクセスされると、アカウントにもアクセスされます。 また、キー ファイルをなくした場合、アカウントにログインできなくなります。

キー ペアは公開キー (HDInsight サーバーに送信されます) と秘密キー (クライアント コンピューターで保存されます) から構成されます。SSH を利用して HDInsight に接続すると、SSH はコンピューターに保存されている秘密キーを利用し、サーバーで認証します。

##SSH キーの作成

クラスターで SSH キーを使用する場合は、次の手順を実行します。 パスワードを使用する場合、このセクションを省略できます。

1. PuTTYGen を開きます。

2.  **キーの種類の生成を**, [ **ssh-2 RSA**, 、順にクリック **生成**します。

    ![PuTTYGen インターフェイス](./media/hdinsight-hadoop-linux-use-ssh-windows/puttygen.png)

3. 進行状況バーが塗りつぶされるまで、バーの下の領域にマウスを移動します。 マウスを移動すると、キーの生成に使用するランダム データが生成されます。

    ![マウスの移動](./media/hdinsight-hadoop-linux-use-ssh-windows/movingmouse.png)

    キーが生成されると、パブリック キーが表示されます。

4. セキュリティ強化のためにパスフレーズを入力することができます、 **キーのパスフレーズ** フィールドとに同じ値を入力し、 **パスフレーズの確認** フィールドです。

    ![パスフレーズ](./media/hdinsight-hadoop-linux-use-ssh-windows/key.png)

    > [AZURE.NOTE] キーのセキュリティで保護されたパスフレーズを使用することを強くお勧めします。 ただし、パスフレーズを忘れた場合、それを回復する方法はありません。

5. クリックして **秘密キーの保存** にキーを保存する、 **.ppk** ファイルです。 このキーは、Linux ベースの HDInsight クラスターへの認証に使用されます。

    > [AZURE.NOTE] Linux ベースの HDInsight クラスターへのアクセスに使用できるよう、安全な場所にこのキーを格納する必要があります。

6. クリックして **公開キーの保存** としてキーを保存する、 **.txt** ファイルです。 これにより、Linux ベースの HDInsight クラスターを今後追加で作成するときにパブリック キーを再利用できます。

    > [AZURE.NOTE] 公開キーは PuTTYGen の上部にも表示されます。 Azure ポータルを使用してクラスターを作成するときに、このフィールドを右クリックし、値をコピーして、フォームに貼り付けることができます。

##Linux ベースの HDInsight クラスターの作成

Linux ベースの HDInsight クラスターを作成するときには、以前に作成した公開キーを指定する必要があります。 Windows ベースのクライアントから、次の 2 つの方法で、Linux ベースの HDInsight クラスターを作成できます。

* **Azure ポータル** -web ベースのポータルを使用して、クラスターを作成します。

* **Mac、Linux および Windows 用の azure CLI** -コマンドラインでコマンドを使用してクラスターを作成します。

これらの各メソッドにはパブリック キーが必要です。 Linux ベースの HDInsight クラスターを作成する方法の詳細については、「[HDInsight での Linux クラスターのプロビジョニング](hdinsight-hadoop-provision-linux-clusters.md)」に関するページをご覧ください。

###Azure ポータル

使用する場合、 [Azure ポータル][preview-portal] を Linux ベースの HDInsight クラスターを作成する入力してください、 **SSH ユーザー名**, を入力するかを選択、 **パスワード** または **SSH パブリック キー**します。

選択した場合 **SSH パブリック キー**, 、公開キーを貼り付けることができますか (に表示される、 __OpenSSH authorized\_keys ファイルに貼り付けるのための公開キー__ PuttyGen、フィールド) に、 __SSH 公開キー__ フィールド、または select __ファイルを選択して__ を参照して、公開キーを含むファイルを選択します。

![公開キーの入力を求めるフォームの画像](./media/hdinsight-hadoop-linux-use-ssh-windows/ssh-key.png)

これにより、指定したユーザーのログインが作成され、パスワード認証または SSH キー認証のいずれかが有効になります。

###Mac、Linux、Windows の Azure コマンド ライン インターフェイス

使用することができます、 [Mac、Linux および Windows 用 Azure CLI](../xplat-cli-install.md) を使用して新しいクラスターを作成する、 `azure hdinsight cluster create` コマンドです。

このコマンドの使用方法の詳細については、「[カスタム オプションを使用した HDInsight での Hadoop クラスターのプロビジョニング](hdinsight-hadoop-provision-linux-clusters.md)」をご覧ください。

##Linux ベースの HDInsight クラスターへの接続

1. PuTTY を開きます。

    ![putty インターフェイス](./media/hdinsight-hadoop-linux-use-ssh-windows/putty.png)

2. ユーザー アカウントの作成時に SSH キーを指定した場合は、次の手順に従って、クラスターへの認証に使用するプライベート キーを選択する必要があります。

     **カテゴリ**, 、展開 **接続**, 、展開 **SSH**, を選択して **Auth**します。 最後に、クリックして **参照** し、秘密キーが含まれた .ppk ファイルを選択します。

    ![putty インターフェイス、秘密キーの選択](./media/hdinsight-hadoop-linux-use-ssh-windows/puttykey.png)

3.  **カテゴリ**, [ **セッション**します。  **PuTTY セッションの基本オプション** 画面で、HDInsight サーバーの SSH アドレスを入力、 **ホスト名 (または IP アドレス)** フィールドです。 SSH アドレスは、クラスター名 **-ssh.azurehdinsight.net**します。 たとえば、 **mycluster-ssh.azurehdinsight.net**します。

    ![ssh アドレスを入力した putty インターフェイス](./media/hdinsight-hadoop-linux-use-ssh-windows/puttyaddress.png)

4. 将来使用するための接続情報を保存するには、[この接続の名前を入力 **保存されたセッション**, 、クリックして **保存**します。 接続が、保存済みセッションの一覧に追加されます。

5. クリックして **開く** 、クラスターに接続します。

    > [AZURE.NOTE] クラスターに接続して初めての場合は、セキュリティの警告を受け取ります。 問題はありません。 選択 **はい** を続行する、サーバーの RSA2 キーをキャッシュします。

6. プロンプトが表示されたら、クラスターの作成時に入力したユーザーを入力します。 ユーザーにパスワードを指定している場合は、その入力も求められます。

> [AZURE.NOTE] 上記の手順では、HDInsight クラスターのヘッド ノード 0 に接続のポート 22 を使用するいると仮定します。 ポート 23 を使用した場合は、ヘッド ノード 1 に接続します。 ヘッド ノードの詳細については、次を参照してください。 [HDInsight クラスターの Hadoop の可用性と信頼性](hdinsight-high-availability-linux.md)します。

###ワーカー ノードへの接続

ワーカー ノードは、Azure データセンターの外部からは直接アクセスできませんが、SSH を使用してクラスター ヘッド ノードからアクセスできます。

ユーザー アカウントの作成時に SSH キーを指定した場合は、ワーカー ノードに接続するクラスターを認証するときに、次の手順に従ってプライベート キーを使用する必要があります。

1. [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) から Pageant をインストールします。 このユーティリティを使用して、PuTTY の SSH キーをキャッシュします。

2. Pageant を実行します。 ステータス トレイにアイコンが表示されます。 アイコンを右クリックして **キーの追加**します。

    ![キーの追加](./media/hdinsight-hadoop-linux-use-ssh-windows/addkey.png)

3. [参照] ダイアログが表示されたらをキーを含む .ppk ファイルを選択し、クリックして **開く**します。 キーは Pageant に追加され、クラスターに接続すると PuTTY に渡されます。

    > [AZURE.IMPORTANT] SSH キーを使用するアカウントを保護する場合は、ワーカー ノードに接続できる前に、前の手順を完了する必要があります。

4. PuTTY を開きます。

5. SSH キーを使用して認証する場合、 **カテゴリ** セクションで、展開 **接続**, 、展開 **SSH**, 、し、[ **Auth**します。

     **認証パラメーター** セクションで、有効にする **を転送できるようにするエージェント**します。 これにより、PuTTY はワーカー ノードに接続するときに、自動的にクラスター ヘッド ノードに接続して証明書認証を渡すことができます。

    ![エージェント転送の許可](./media/hdinsight-hadoop-linux-use-ssh-windows/allowforwarding.png)

6. 上記の説明と同様に、クラスターに接続します。 認証に SSH キーを使用する場合は、キーを選択する必要はありません。Pageant に追加された SSH キーがクラスターの認証に使用されます。

7. 接続が確立されたら、次のコマンドを使用してクラスター内のノードの一覧を取得します。 置換 *ADMINPASSWORD* 、クラスターの管理者アカウントのパスワードを使用します。 置換 *CLUSTERNAME* 、クラスターの名前に置き換えます。

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    クラスター内のノードの `host_name` などの情報が JSON 形式で返されます。これには、各ノードの完全修飾ドメイン名 (FQDN) が含まれています。 次の例に示します、 `host_name` によって返されるエントリ、 **curl** コマンド。

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

8. 接続するワーカー ノードの一覧を取得したら、PuTTY セッションから次のコマンドを使用して、ワーカー ノードへの接続を開きます。

        ssh USERNAME@FQDN

    置換 *USERNAME* を SSH ユーザー名と *FQDN* ワーカー ノードの FQDN を使用します。 たとえば、「`workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`」のように入力します。

    > [AZURE.NOTE] 認証するためのパスワードを使用する場合、SSH セッションをもう一度パスワードを入力するように求められます。 SSH キーを使用する場合は、何も表示されずに接続が完了します。

9. セッションが確立されると、PuTTY セッションのプロンプトが `username@hn0-clustername` から `username@wn0-clustername` に変わり、ワーカー ノードに接続したことを示します。 この時点で実行するすべてのコマンドは、ワーカー ノードで実行されます。

10. ワーカー ノードでの操作が終了したら、`exit` コマンドを使用してワーカー ノードのセッションを閉じます。 これにより、`username@hn0-clustername` プロンプトが表示されます。

##複数のアカウントの追加

クラスターに複数のアカウントを追加する必要がある場合は、次の手順を実行します。

1. 前述の説明に従って、新しいユーザー アカウントの新しいパブリック キーとプライベート キーを生成します。

2. クラスターへの SSH セッションから、次のコマンドを使用して新しいユーザーを追加します。

        sudo adduser --disabled-password <username>

    これにより、新しいユーザー アカウントが作成されますが、パスワード認証は無効になります。

3. 次のコマンドを使用して、キーを格納するディレクトリとファイルを作成します。

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

4. ナノ エディターが開いたら、新しいユーザー アカウントのパブリック キーの内容をコピーして貼り付けます。 最後に、使用して **CTRL + X** ファイルを保存してエディターを終了します。

    ![サンプル キーとナノ エディターの画像](./media/hdinsight-hadoop-linux-use-ssh-windows/nano.png)

5. 次のコマンドを使用して、新しいユーザー アカウントに対する .ssh フォルダーの所有権をとコンテンツを変更します。

        sudo chown -hR <username>:<username> /home/<username>/.ssh

6. これで、新しいユーザー アカウントおよびプライベート キーを使用してサーバーへの認証を行えるようになります。

##<a id="tunnel"></a>SSH トンネリング

SSH を使用して、Web 要求などのローカルの要求を HDInsight クラスターにトンネリングできます。 ここでは、最初から HDInsight クラスター ヘッド ノード上にあったかのように、要求が要求済みリソースにルーティングされます。

> [AZURE.IMPORTANT] SSH トンネルは、Hadoop サービスによって、web UI にアクセスするための要件です。 たとえば、ジョブ履歴 UI とリソース マネージャー UI は、両方とも SSH トンネルでのみアクセスできます。

作成して、SSH トンネルを使用の詳細については、次を参照してください。 [使用 SSH トンネリング Ambari web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の web UI にアクセスする](hdinsight-linux-ambari-ssh-tunnel.md)です。

##次のステップ

これで、SSH キーを使用して認証する方法、また、HDInsight 上の Hadoop で MapReduce を使用する方法についてご理解いただけたと思います。

* [HDInsight での Hive の使用](hdinsight-use-hive.md)

* [HDInsight での Pig の使用](hdinsight-use-pig.md)

* [HDInsight での MapReduce ジョブの使用](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/

