<properties
   pageTitle="Linux、Unix、OS X から Linux ベースの Hadoop で SSH キーを使用する |Microsoft Azure"
   description=" Secure Shell (SSH) を使用して Linux ベースの HDInsight にアクセスできます。 このドキュメントでは、Linux、Unix、または OS X の各クライアントから HDInsight で SSH を使用する方法について説明します。"
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

#Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux、Unix、OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) コマンド ライン インターフェイスを使用して、Liux ベースの HDInsight クラスターでの操作をリモートで実行することができます。 このドキュメントでは、Linux、Unix、または OS X の各クライアントから HDInsight で SSH を使用する方法について説明します。

> [AZURE.NOTE] この記事の手順では、Linux、Unix、または OS X クライアントを使用するいると仮定します。 提供するパッケージをインストールしている場合、Windows ベースのクライアントで次の手順を実行できますが `ssh` と `ssh-keygen` (Windows の Git) などをお勧めする Windows ベースのクライアントの手順で [SSH Windows から Linux ベースの HDInsight (Hadoop) を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)です。

##前提条件

* **問題** と **ssh** Linux、Unix、および OS X クライアントです。 通常、このユーティリティはオペレーティング システムに付属しています。また、パッケージ管理システムから入手することもできます。

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

1. ターミナル セッションを開き、次のコマンドを使用して、既存の SSH キーがあるかどうかを確認します。

        ls -al ~/.ssh

    ディレクトリ一覧で、次のファイルを探します。 これらは SSH 公開キーの共通名です。

    * id\_dsa.pub
    * id\_ecdsa.pub
    * id\_ed25519.pub
    * id\_rsa.pub

2. 既存のファイルを使用しない場合や、既存の SSH キーがない場合は、次のコマンドを使用して新しいファイルを生成します。

        ssh-keygen -t rsa

    次の情報の入力を求められます。

    * ファイルの場所 - 既定値は ~/.ssh/id_rsa'/id\_rsa します。
    * パスフレーズ - 1 回入力した後に再入力を求められます。

        > [AZURE.NOTE] キーのセキュリティで保護されたパスフレーズを使用することを強くお勧めします。 ただし、パスフレーズを忘れた場合、それを回復する方法はありません。

    コマンドが完了すると、2 つの新しいファイル、秘密キーが用意されます (たとえば、 **id\_rsa**) と公開キー (たとえば、 **id\_rsa.pub**)。

##Linux ベースの HDInsight クラスターの作成

Linux ベースの HDInsight クラスターを作成するときには、以前に作成した公開キーを指定する必要があります。 Linux、Unix、または OS X の各クライアントからは、2 つの方法で HDInsight クラスターを作成できます。

* **Azure ポータル** -web ベースのポータルを使用して、クラスターを作成します。

* **Mac、Linux および Windows 用の azure CLI** -コマンドラインでコマンドを使用してクラスターを作成します。

これらの方法では、それぞれパスワードまたは公開キーが必要です。 Linux ベースの HDInsight クラスターを作成する方法の詳細については、「[HDInsight での Linux クラスターのプロビジョニング](hdinsight-hadoop-provision-linux-clusters.md)」に関するページをご覧ください。

###Azure ポータル

使用する場合、 [Azure ポータル][preview-portal] を Linux ベースの HDInsight クラスターを作成する入力してください、 **SSH ユーザー名**, を入力するかを選択、 **パスワード** または **SSH パブリック キー**します。

選択した場合 **SSH 公開キー**, 、公開キーを貼り付けることができますか (を使用してファイルに含まれている、 **.pub** 拡張機能) に、 __SSH 公開キー__ フィールド、または選択 __ファイルを選択して__ を参照して、公開キー ファイルを選択します。

![公開キーの入力を求めるフォームの画像](./media/hdinsight-hadoop-linux-use-ssh-unix/ssh-key.png)

> [AZURE.NOTE] キー ファイルは、単なるテキスト ファイルです。 内容は、次のように表示されます。
> ```
ssh rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCelfkjrpYHYiks4TM + r1LVsTYQ4jAXXGeOAF9Vv/KGz90pgMk3VRJk4PEUSELfXKxP3NtsVwLVPN1l09utI/tKHQ6WL3qy89WVVVLiwzL7tfJ2B08Gmcw8mC/YoieT/YG + 4I4oAgPEmim + 6/F9S0lU2I2CuFBX9JzauX8n1Y9kWzTARST + ERx2hysyA5ObLv97Xe4C2CQvGE01LGAXkw2ffP9vI + emUM VeYrf0q3w/b1o/COKbFVZ2IpEcJ8G2SLlNsHWXofWhOKQRi64TMxT7LLoohD61q2aWNKdaE4oQdiuo8TGnt4zWLEPjzjIYIEIZGk00HiQD + KCB5pxoVtp user@system
> ```

これで、入力したパスワードまたは公開キーを使用して、指定されたユーザーのログインが作成されます。

###Mac、Linux、Windows の Azure コマンド ライン インターフェイス

使用することができます、 [Mac、Linux および Windows 用 Azure CLI](../xplat-cli.md) を使用して新しいクラスターを作成する、 `azure hdinsight cluster create` コマンドです。

このコマンドの使用方法の詳細については、「[カスタム オプションを使用した HDInsight での Hadoop クラスターのプロビジョニング](hdinsight-hadoop-provision-linux-clusters.md)」をご覧ください。

##Linux ベースの HDInsight クラスターへの接続

ターミナル セッションから、SSH コマンドを使用して、アドレスとユーザー名を入力し、クラスター ヘッド ノードに接続します。

* **SSH アドレス** -クラスター名に続けて **-ssh.azurehdinsight.net**します。 たとえば、 **mycluster-ssh.azurehdinsight.net**します。

* **ユーザー名** -SSH ユーザー名、クラスターの作成時に指定します。

次の例は、クラスターに接続 **mycluster** ユーザーとして **me**:

    ssh me@mycluster-ssh.azurehdinsight.net

ユーザー アカウントにパスワードを使用した場合は、そのパスワードの入力を求められます。

パスフレーズで保護された SSH キーを使用した場合は、そのパスフレーズの入力を求められます。 パスフレーズを入力しない場合、SSH はローカルの秘密キーのいずれかをクライアントで使用して自動的に認証を試行します。

> [AZURE.NOTE] SSH は自動的に認証を行わない場合、正しい秘密キーを使用して、 **-i** パラメーターし、秘密キーへのパスを指定します。 次の例は、`~/.ssh/id_rsa` から秘密キーを読み込みます。
>
> `ssh -i ~/.ssh/id_rsa me@mycluster-ssh.azurehdinsight.net`

ポートが指定されない場合、SSH のポートは既定でポート 22 に設定され、HDInsight クラスターのヘッド ノード 0 に接続されます。 ポート 23 を使用した場合は、ヘッド ノード 1 に接続します。 ヘッド ノードの詳細については、次を参照してください。 [HDInsight クラスターの Hadoop の可用性と信頼性](hdinsight-high-availability-linux.md)します。

###ワーカー ノードへの接続

ワーカー ノードは、Azure データセンターの外部からは直接アクセスできませんが、SSH を使用してクラスター ヘッド ノードからアクセスできます。

ユーザー アカウントの認証に SSH キーを使用する場合は、クライアントで次の手順を完了する必要があります。

1. テキスト エディターを使用して `~/.ssh/config` を開きます。 このファイルが存在しない場合は、ターミナルで `touch ~/.ssh/config` と入力して作成できます。

2. このファイルに次のコードを追加します。 置換 *CLUSTERNAME* 、HDInsight クラスターの名前に置き換えます。

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    これは、HDInsight クラスターに SSH エージェント転送を構成します。

3. 端末から次のコマンドを使用して、SSH エージェント転送をテストします。

        echo "$SSH_AUTH_SOCK"

    次のような情報が返されます。

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    示します何も返されない場合 **で ssh-agent** が実行されていません。 インストールと構成に関する具体的な手順について、オペレーティング システムのマニュアルを参照してください **で ssh-agent**, 、または参照してください [ssh を使用して ssh エージェントで](http://mah.everybody.org/docs/ssh)します。

4. 確認後 **で ssh-agent** を実行している、次を使用して SSH 秘密キーをエージェントに追加します。

        ssh-add ~/.ssh/id_rsa

    秘密キーを別のファイルに格納している場合は、`~/.ssh/id_rsa` をそのファイルのパスに置き換えます。

次の手順を使用して、クラスターのワーカー ノードに接続します。

> [AZURE.IMPORTANT] アカウントの認証に SSH キーを使用する場合は、転送するエージェントが動作していることを確認する前の手順を完了する必要があります。

1. 前述のように、SSH を使用して HDInsight クラスターに接続します。

2. 接続したら、次のコマンドを使用してクラスター内のノードの一覧を取得します。 置換 *ADMINPASSWORD* 、クラスターの管理者アカウントのパスワードを使用します。 置換 *CLUSTERNAME* 、クラスターの名前に置き換えます。

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    クラスター内のノードの `host_name` などの情報が JSON 形式で返されます。これには、各ノードの完全修飾ドメイン名 (FQDN) が含まれています。 次の例に示します、 `host_name` によって返されるエントリ、 **curl** コマンド。

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

3. 接続するワーカー ノードの一覧を取得したら、SSH セッションからサーバーに次のコマンドを使用して、ワーカー ノードへの接続を開きます。

        ssh USERNAME@FQDN

    置換 *USERNAME* を SSH ユーザー名と *FQDN* ワーカー ノードの FQDN を使用します。 たとえば、「`workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`」のように入力します。

    > [AZURE.NOTE] 認証するためのパスワードを使用する場合、SSH セッションをもう一度パスワードを入力するように求められます。 SSH キーを使用する場合は、何も表示されずに接続が完了します。

4. セッションが確立されると、ターミナルのプロンプトが `username@hn0-clustername` から `username@wk0-clustername` に変わり、ワーカー ノードに接続したことを示します。 この時点で実行するすべてのコマンドは、ワーカー ノードで実行されます。

4. ワーカー ノードでの操作が終了したら、`exit` コマンドを使用してワーカー ノードのセッションを閉じます。 これにより、`username@hn0-clustername` プロンプトが表示されます。

##複数のアカウントの追加

1. 」の説明に従って、新しい公開キーと、新しいユーザー アカウントの秘密キーの生成、 [SSH キーを作成](#create-an-ssh-key-optional) セクションです。

    > [AZURE.NOTE] 秘密キーは、ユーザーが、クラスターへの接続に使用または作成後にこのようなクライアントに安全に転送するクライアントで生成するか。

1. クラスターへの SSH セッションから、次のコマンドを使用して新しいユーザーを追加します。

        sudo adduser --disabled-password <username>

    これにより、新しいユーザー アカウントが作成されますが、パスワード認証は無効になります。

2. 次のコマンドを使用して、キーを格納するディレクトリとファイルを作成します。

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

3. ナノ エディターが開いたら、新しいユーザー アカウントのパブリック キーの内容をコピーして貼り付けます。 最後に、使用して **CTRL + X** ファイルを保存してエディターを終了します。

    ![サンプル キーとナノ エディターの画像](./media/hdinsight-hadoop-linux-use-ssh-unix/nano.png)

4. 次のコマンドを使用して、新しいユーザー アカウントに対する .ssh フォルダーの所有権をとコンテンツを変更します。

        sudo chown -hR <username>:<username> /home/<username>/.ssh

5. これで、新しいユーザー アカウントおよびプライベート キーを使用してサーバーへの認証を行えるようになります。

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

