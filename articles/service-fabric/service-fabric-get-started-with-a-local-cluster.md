<properties
   pageTitle="ローカル クラスターでアプリケーションのデプロイおよびアップグレードを開始する | Microsoft Azure"
   description="Service Fabric のローカル クラスターをセットアップし、それに既存のアプリケーションをデプロイし、そのアプリケーションをアップグレードします。"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="11/20/2015"
   ms.author="seanmck"/>

# ローカル クラスターでアプリケーションのデプロイおよびアップグレードを開始する
Service Fabric SDK には、完全なローカル開発環境が含まれているので、ローカル クラスター上でアプリケーションをデプロイおよび管理する作業をすぐに開始することができます。 この記事では、ローカル クラスターを作成し、既存のアプリケーションをローカル クラスターにデプロイし、そのアプリケーションを新しいバージョンにアップグレードします。これらの作業はすべて Windows PowerShell で行います。

> [AZURE.NOTE] この記事では、あると見なさを既にお持ち [開発環境を設定する](service-fabric-get-started.md)です。

## ローカル クラスターを作成する
Service Fabric クラスターとは、アプリケーションをデプロイすることができるハードウェア リソース セットです。 通常、クラスターは 5 台から数千台の範囲のコンピューターで構成されますが、Service Fabric SDK には、1 台のコンピューター上で実行できるクラスター構成が含まれています。

Service Fabric ローカル クラスターがエミュレーターでもシミュレーターでもないことを理解しておくことは重要です。 これは、複数のコンピューターから成るクラスターで検出されるのと同じプラットフォーム コードを実行します。 唯一の違いは、通常 5 台のコンピューターに分散して実行されるプラットフォーム プロセスを 1 台のコンピューター上で実行するということです。

SDK では、2 つの方法でローカル クラスターをセットアップすることができます。1 つは Windows PowerShell スクリプトを使用する方法、もう 1 つはローカル クラスター マネージャーのシステム トレイ アプリを使用する方法です。 このチュートリアルでは、PowerShell スクリプトを使用します。

> [AZURE.NOTE] Visual Studio からアプリケーションを展開することで既にローカル クラスターを作成する場合は、このセクションを省略できます。


1. 管理者として、新しい PowerShell ウィンドウを起動します。

2. SDK フォルダーからクラスター セットアップ スクリプトを実行します。

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    クラスターのセットアップには数分かかります。セットアップ完了後、次のような出力が表示されます。

    ![クラスターのセットアップに関する出力][cluster-setup-success]

    これでクラスターにアプリケーションをデプロイする準備が整いました。

## アプリケーションをデプロイする
Service Fabric SDK には、アプリケーションを作成するための豊富なフレームワークと開発者向ツールが含まれています。 Visual Studio でアプリケーションを作成する方法の学習に関心がある場合 [Visual Studio で、最初のアプリケーションを作成する](service-fabric-create-your-first-application-in-visual-studio.md)です。 このチュートリアルでは、プラットフォームの管理面 (デプロイ、監視、およびアップグレードなど) に集中できるように、既存のサンプル アプリケーション (名前は WordCount) を使用します。


1. 管理者として、新しい PowerShell ウィンドウを起動します。

2. Service Fabric SDK PowerShell モジュールをインポートします。

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. c:\ServiceFabric のようなディレクトリを作成し、そこにダウンロードおよびデプロイするアプリケーションを格納します。

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. ワード カウント アプリケーションをダウンロード [ここ](http://aka.ms/servicefabric-wordcountapp) 作成した場所にします。

5. 次のように、ローカル クラスターに接続します。

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. SDK のデプロイメント コマンドを呼び出して新しいアプリケーションを作成し、名前とアプリケーション パッケージへのパスを指定します。

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    すべて成功すると、出力は次のように表示されます。

    ![アプリケーションをローカル クラスターにデプロイする][deploy-app-to-local-cluster]

7. アプリケーションの動作を表示するには、ブラウザーを起動し、 [http://localhost:8081/wordcount/インデックス](http://localhost:8081/wordcount/index)します。 次のような結果が表示されます。

    ![デプロイされたアプリケーション UI][deployed-app-ui]

    WordCount アプリケーションは非常に単純です。 このアプリケーションには、ランダムな 5 文字の "単語" を生成するクライアント側の JavaScript コードが含まれます。生成された単語は ASP.NET WebAPI によってアプリケーションに中継されます。 ステートフル サービスは、単語の最初の文字に基づいてパーティション分割された単語のカウント数を追跡します。 デプロイしたアプリケーションには 4 つのパーティションがあります。このため、A ～ G で始まる単語は最初のパーティションに格納され、H ～ N で始まる単語は 2 番目のパーティションに格納されるといった具合に分類されます。

## アプリケーションの詳細と状態を表示する
アプリケーションがデプロイされた状態で、アプリケーションの詳細の一部を PowerShell で見てみましょう。

1. クラスター上にあるデプロイされたすべてのアプリケーションのクエリを実行します。

    ```powershell
    Get-ServiceFabricApplication
    ```

    WordCount アプリケーションのみがデプロイされていると仮定すると、次のような結果が表示されます。

    ![PowerShell で、デプロイされたすべてのアプリケーションのクエリを実行する][ps-getsfapp]

2. WordCount アプリケーションに含まれているサービス セットのクエリを実行することにより、次のレベルに移動します。

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![PowerShell でアプリケーションのサービスを一覧表示する][ps-getsfsvc]

    アプリケーションは 2 つのサービス (Web フロント エンドと、単語を管理するステートフル サービス) から構成されていることに注意してください。

3. 最後に、WordCountService のパーティションの一覧を見てみましょう。

    ![PowerShell でサービスのパーティションを表示する][ps-getsfpartitions]

    先ほど使用した一連のコマンドは、すべての Service Fabric PowerShell コマンドと同様に、ローカルまたはリモートで接続する可能性のあるどのクラスターでも利用できます。

    移動して、web ベースの Service Fabric エクスプ ローラー ツールを使用するクラスターとの対話より視覚的に [http://localhost:19080/エクスプ ローラー](http://localhost:19080/Explorer) ブラウザーにします。

    ![Service Fabric Explorer でアプリケーションの詳細を表示する][sfx-service-overview]

    > [AZURE.NOTE] Service Fabric エクスプ ローラーの詳細については、次を参照してください [Service Fabric エクスプ ローラーでクラスターの視覚化。](service-fabric-visualizing-your-cluster.md)

## アプリケーションのアップグレード
Service Fabric では、クラスター全体にロールアウトされるときにアプリケーションの正常性を監視することにより、ダウンタイムのないアップグレードを実現します。 WordCount アプリケーションの簡単なアップグレードを実行してみましょう。

アプリケーションの新しいバージョンでは、母音で始まる単語のみをカウントするようになります。 アップグレードがロールアウトされると、アプリケーションの動作に 2 つの変化が現れます。 第 1 に、カウント数の増加のスピードが低下します。前よりもカウント対象の単語が少なくなっているためです。 第 2 に、1 番目のパーティションには 2 つの母音 (A と E) が含められ、それ以外のパーティションにはそれぞれ 1 つの母音が含められるので、1 番目のパーティションのカウント数は最終的に、その他のパーティションの値を上回るようになります。

1. V2 パッケージをダウンロード [ここ](http://aka.ms/servicefabric-wordcountappv2) v1 パッケージをダウンロードした同じ場所にします。

2. PowerShell ウィンドウに戻ります。SDK のアップグレード コマンドを使用して、新しいバージョンをクラスターに登録し、fabric:/WordCount ファブリックのアップグレードを開始します。

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    アップグレードが始まると、PowerShell に次のような出力が表示されます。

    ![PowerShell でのアップグレードの進行状況][ps-appupgradeprogress]

3. アップグレードの進行中は、Service Fabric Explorer でその状態を監視するのが簡単な場合もあります。 ブラウザー ウィンドウを起動しに移動 [http://localhost:19080/エクスプ ローラー](http://localhost:19080/Explorer)します。 をクリックして **アプリケーション** 左側のツリーでクリックして **進行中のアップグレード**します。

    ![Service Fabric Explorer でのアップグレードの進行状況][sfx-upgradeprogress]

    アップグレード進行状況インジケーターは、クラスターのアップグレード ドメインでのアップグレードの状態を表します。 アップグレードが各ドメインを進行するとき、正常性チェックを実行してアプリケーションが正常に動作していることを確認してから先に進みます。

4. fabric:/WordCount アプリケーションに含まれるサービス セットに対する以前のクエリを再実行した場合、WordCountService のバージョンは変化しているが、WordCountWebService のバージョンは変化していないことがわかります。

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![アップグレード後にアプリケーション サービスのクエリを実行する][ps-getsfsvc-postupgrade]

    これは、Service Fabric によるアプリケーションのアップグレードの管理方法に焦点を当てています。この方法では変更されたサービス セット (またはそれらのサービス内のコード/構成パッケージ) のみを対象とし、アップグレード処理にかかる時間を短縮し、アップグレード処理の信頼性を向上させます。

5. 最後に、ブラウザーに戻り、アプリケーションの新しいバージョンの動作を確認します。 予想通り、カウント数の増加のスピードは緩やかで、最終的に 1 番目のパーティションのカウント数がやや大きくなります。

    ![ブラウザーでアプリケーションの新しいバージョンを表示する][deployed-app-ui-v2]

## 次のステップ
- これで、できるを展開し、事前に構築されたアプリケーションをアップグレードしたを使用して [で独自の Visual Studio のビルドを試みます](service-fabric-create-your-first-application-in-visual-studio.md)します。
- この記事でローカル クラスターで実行される操作のすべてに対して実行できる [Azure クラスター](service-fabric-cluster-creation-via-portal.md) もします。
- この記事で実行したアップグレードは、非常に基本的なものです。 参照してください、 [ドキュメントをアップグレード](service-fabric-application-upgrade.md) に関する機能と柔軟性の Service Fabric のアップグレードの詳細。

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png


