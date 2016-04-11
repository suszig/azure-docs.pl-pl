<properties
 pageTitle="Azure の HPC Pack クラスターにジョブを送信する | Microsoft Azure"
 description="Azure の HPC Pack クラスターに HPC ジョブを送信するようにオンプレミス コンピューターを設定する方法について説明します。"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="09/28/2015"
 ms.author="danlep"/>

# オンプレミス コンピューターから Azure の HPC Pack クラスターに HPC ジョブを送信する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

この記事では、HTTPS で Azure の HPC Pack クラスターと通信する HPC Pack ジョブ送信ツールを実行するように、Windows を実行するオンプレミス クライアント コンピューターを構成する方法について説明します。 これにより、簡単で柔軟性の高いさまざまなクラスター ユーザーがクラウド ベースの HPC Pack へのジョブをクラスター ヘッド ノード VM に直接接続する必要はありませんを送信するには
ジョブ送信ツールを実行します。

![Azure のクラスターにジョブを送信する][jobsubmit]

## 前提条件

* **HPC Pack ヘッド ノードを Azure VM で展開** -使用することができます
自動化ツールなど、 [Azure クイック スタート テンプレート](https://azure.microsoft.com/documentation/templates/) または [Azure PowerShell スクリプト](virtual-machines-hpcpack-cluster-powershell-script.md)
ヘッド ノードとクラスターを展開するクラスターを展開できます。
手動での Azure では、オンプレミス クラスターとします。 DNS は必要があります。
ヘッド ノードとクラスター管理者の資格情報の名前
この記事の手順を完了します。

    ヘッド ノードを手動でデプロイした場合、HTTPS エンドポイントが VM で構成されていることを確認します。 構成されていない場合、HTTPS エンドポイントを設定します。 参照してください [バーチャル マシンに対してエンドポイントを設定する方法
マシン](virtual-machines-set-up-endpoints.md)します。

* **HPC Pack インストール メディア** -無料のインストール パッケージ、
HPC Pack (HPC Pack 2012 R2) の最新バージョンは、
[Microsoft ダウンロード
Center](http://go.microsoft.com/fwlink/?LinkId=328024)します。 確認します。
ヘッド ノードにインストールされている HPC Pack の同じバージョンを選択します。
VM です。

* **クライアント コンピューター** -Windows または Windows Server のクライアント コンピューターで、HPC Pack クライアント ユーティリティを実行する必要があります (を参照してください [システム要件](https://technet.microsoft.com/library/dn535781.aspx))。 ジョブを送信する目的だけで HPC Pack Web ポータルまたは REST API を使用する場合、自分で選んだクライアント コンピューターを利用できます。


## 手順 1: ヘッド ノードに Web コンポーネントをインストールし、構成する

HTTPS 経由でクラスターにジョブを送信する REST インターフェイスを有効にするには
インストールし、HPC Pack ヘッドの HPC Pack web コンポーネントを構成します。
既に構成されていない場合は、ノードです。 最初に、web をインストールします。
HpcWebComponents.msi インストール ファイルを実行してコンポーネントです。 次に、
HPC PowerShell スクリプトを実行してコンポーネントを構成します。
**Set-hpcwebcomponents.ps1**します。

詳細な手順については、次を参照してください [Microsoft HPC Pack Web インストール。
コンポーネント](http://technet.microsoft.com/library/hh314627.aspx)します。

>[AZURE.TIP] 使用する場合、 [HPC Pack IaaS デプロイメント スクリプト](virtual-machines-hpcpack-cluster-powershell-script.md) クラスターを作成するには
必要に応じて、インストールし、展開の一部として web コンポーネントを構成することができます。

**Web コンポーネントをインストールするには**

1. クラスター管理者の資格情報を使用し、ヘッド ノード VM に接続します。

2. HPC Pack セットアップ フォルダーから、ヘッド ノードで HpcWebComponents.msi を実行します。

3. ウィザードの手順に従い、Web コンポーネントをインストールします

**Web コンポーネントを構成するには**

1. ヘッド ノードで、管理者として HPC PowerShell を起動します。

2. 構成スクリプトの場所にディレクトリを変更するには、次のコマンドを入力します。

    ```
    cd $env:CCP_HOME\bin
    ```
3. REST インターフェイスを構成し、HPC Web Service を起動し、次のコマンドを入力します。

    ```
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. 証明書の選択を求められたら、ヘッド ノードのパブリック DNS 名に対応する証明書の選択 (CN = & lt;*HeadNodeDnsName*& gt;。>.cloudapp.net)。

    >[AZURE.NOTE] 内部設置型コンピューターからヘッド ノードに後でジョブを送信するこの証明書を選択する必要があります。 選択するか、Active Directory ドメインでヘッド ノードのコンピューター名に対応する証明書を構成しない (たとえば、CN =*MyHPCHeadNode.HpcAzure.local*)。

5. Web ポータルのジョブ送信を構成するには、次のコマンドを入力します。

    ```
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. スクリプトが完了したら、次を入力し、HPC Job Scheduler Service を停止し、再起動します。

    ```
    net stop hpcscheduler
    net start hpcscheduler
    ```

## 手順 2: HPC Pack クライアント ユーティリティをオンプレミス コンピューターにインストールする

まだ行っていない、互換性のあるバージョンをダウンロードします
HPC Pack のセットアップ ファイルから、 [マイクロソフト ダウンロード
Center](http://go.microsoft.com/fwlink/?LinkId=328024) クライアント
コンピューターです。 インストールを開始するとき、HPC Pack クライアント ユーティリティのセットアップ オプションを選択します。

HPC Pack クライアント ツールを使用し、ジョブをヘッド ノード VM に送信するには、ヘッド ノードから証明書をエクスポートし、それをクライアント コンピューターにインストールする必要もあります。 証明書は .CER 形式にする必要があります。

**ヘッド ノードから証明書をエクスポートするには**

1. ヘッド ノードで、ローカル コンピューター アカウントに対して Microsoft 管理コンソールに証明書スナップインを追加します。 スナップインを追加する手順については、次を参照してください。 [証明書スナップインを MMC に追加](https://technet.microsoft.com/library/cc754431.aspx)します。

2. コンソール ツリーで [ **証明書-ローカル コンピューター**, 、展開 **個人**, 、クリックして **証明書**します。

3. HPC Pack web コンポーネント用に構成した証明書を見つけます [手順 1: インストールしてヘッド ノードで、web コンポーネントを構成する](#step-1:-install-and-configure-the-web-components-on-the-head-node) (たとえば、名前付き & lt; 用HeadNodeDnsName & gt;。>.cloudapp.net)。

4. 証明書を右クリックし、をクリックして **すべてのタスク**, 、クリックして **エクスポート**します。

5. 証明書のエクスポート ウィザード] をクリックして **次**, 、いることを確認および **秘密キーをエクスポートしません** が選択されています。

6. ウィザードの残りの手順に従い、DER エンコード バイナリ X.509 (.CER) 形式で証明書をエクスポートします。


**クライアント コンピューターに証明書をインポートするには**


1. ヘッド ノードからクライアント コンピューターのフォルダーにエクスポートした証明書をコピーします。

2. クライアント コンピューターで、certmgr.msc を実行します。

3. 証明書マネージャーで、展開 **証明書-現在のユーザー**, 、展開 **信頼されたルート証明機関**, を右クリックして **証明書**, 、] をクリックして **すべてのタスク**, 、] をクリックし、 **インポート**します。

4. 証明書のインポート ウィザード] をクリックして **次** 証明書をインポートする手順に従いますをヘッド ノードからエクスポートします。



>[AZURE です。[セキュリティ] は表示セキュリティの警告、ヘッド ノード上の証明機関は、クライアント コンピューターでは認識されません。 テスト目的であるため、この警告を無視し、証明書インポートを完了します。

## 手順 3: クラスターでテスト ジョブを実行する

構成を確認するには、Azure のクラスターでジョブを実行してください。
HPC Pack クライアントを実行している内部設置型コンピューターを使用して、
ユーティリティです。 たとえば、HPC Pack GUI ツールまたはコマンドライン コマンドを利用し、ジョブをクラスターに送信できます。 Web ベース ポータルを利用してジョブを送信することもできます。


**クライアント コンピューターでジョブ送信コマンドを実行するには**


1. クライアント コンピューターで、コマンド プロンプトを起動します。

2. サンプル コマンドを入力します。 たとえば、クラスターのすべてのジョブを一覧表示するには、次を入力します。

    ```
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    >[AZURE.TIP] スケジューラの URL に IP アドレスではなく、ヘッド ノードの完全な DNS 名を使用します。 IP アドレスを指定した場合、「The server certificate needs to either have a valid chain of trust or to be placed in the trusted root store (サーバー証明書は有効な信頼チェーンを持つか、信頼されたルート ストアに置かれている必要があります)」のようなエラーが表示されます。

3. 求められたら、[ユーザー名 (フォーム & lt; DomainName & gt; \\ & lt;ユーザー名 & gt;) と、HPC クラスター管理者または構成した別のクラスター ユーザーのパスワード。 ジョブ操作が多ければ、資格情報をローカルに保存できます。

    ジョブの一覧が表示されます。


**クライアント コンピューターで HPC ジョブ マネージャーを使用するには**

1. 以前、ジョブの送信時、クライアント コンピューターでクラスター ユーザーのドメイン資格情報を保存しなかった場合、資格情報マネージャーで資格情報を追加できます。

    a. クライアント コンピューターのコントロール パネルで、資格情報マネージャーを起動します。

    b. をクリックして **Windows 資格情報**, 、] をクリックし、 **汎用資格情報の追加**します。

    c. インターネット アドレス https://&lt; を指定します。HeadNodeDnsName & gt;.cloudapp.net/HpcScheduler、ユーザー名を指定し、(フォーム & lt; DomainName & gt; \\ & lt;ユーザー名 & gt;) と、HPC クラスター管理者または構成した別のクラスター ユーザーのパスワード。

2. クライアント コンピューターで、HPC ジョブ マネージャーを起動します。

3.  **ヘッド ノードの選択** ダイアログ ボックス フォーム https://&lt; で、Azure でヘッド ノードへの URL を入力HeadNodeDnsName & gt;。cloudapp.net します。

    HPC ジョブ マネージャーが開き、ヘッド ノードでジョブの一覧が表示されます。

**ヘッド ノードで実行されている Web ポータルを使用するには**

1. クライアント コンピューターで Web ブラウザーを起動し、次のアドレスを入力します。

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
2. セキュリティ ダイアログ ボックスが表示されたら、HPC クラスター管理者のドメイン資格情報を入力します。 (さまざまなロールで他のクラスター ユーザーを追加することもできます。 詳細については、次を参照してください [クラスター ユーザーの管理](https://technet.microsoft.com/library/ff919335.aspx)。)。

    Web ポータルが開き、ジョブの一覧が表示されます。

3. クラスターから文字列"Hello World"を返すサンプル ジョブを送信するにはクリックして **新しいジョブ** 、左側のナビゲーションにします。

4.  **新しいジョブ** ] ページで、[ **送信ページから**, 、クリックして **HelloWorld**します。 ジョブの送信ページが表示されます。

5. クリックして **送信**します。 入力が求められたら、HPC クラスター管理者のドメインの資格情報を入力します。 ジョブが送信され、ジョブ ID が表示されます、 **自分のジョブ** ページです。

6. 送信したジョブの結果を表示するジョブ ID] をクリックし、をクリックして **タスクを表示する** コマンドの出力を表示する ([ **出力**)。

## 次のステップ

* 使用して、Azure クラスターにジョブを送信することも、 [HPC Pack の REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)します。

* Linux クライアントからクラスターに送信する場合は、Python のサンプルを参照してください、 [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756)します。


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-hpcpack-cluster-submit-jobs/jobsubmit.png


