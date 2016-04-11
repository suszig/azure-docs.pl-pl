<properties
    pageTitle="Python Tools 2.2 for Visual Studio による Python Web ロールと Python worker ロール | Microsoft Azure"
    description="Azure クラウド サービス (Web ロール、worker ロールを含む) を Python Tools for Visual Studio で作成する方法の概要"
    services=""
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/30/2015"
    ms.author="huvalo"/>




# Python Tools 2.2 for Visual Studio による Python Web ロールと Python worker ロール

この記事の概要を使用して Python web および worker ロールを使用して [Python Tools for Visual Studio][]します。

## 前提条件

 - Visual Studio 2013 または 2015
 - [Visual Studio 用 Python ツールの 2.2][] (PTVS)
 - [Azure SDK Tools for VS 2013][] または [Azure SDK Tools for VS 2015][]
 - [Python 2.7 32-bit][] または [Python 3.4 32 ビット][]

[AZURE.INCLUDE [create-account-and-websites-note](../includes/create-account-and-websites-note.md)]

## Python Web ロールと Python worker ロールについて

Azure には 3 つのコンピューティング アプリケーションを実行するためのモデル: [Azure App Service で Web アプリ機能][execution model-web sites], 、[Azure の仮想マシン][execution model-vms], 、および [Azure Cloud Services][execution model-cloud services]します。 これら 3 つのモデルはすべて、Python をサポートしています。 クラウド サービスは、web および worker ロールを含めるには、提供 *Platform as a Service (PaaS)*します。 クラウド サービス内で、Web ロールは、フロントエンド Web アプリケーションのホスト専用のインターネット インフォメーション サービス (IIS) Web サーバーを提供します。ワーカー ロールは、ユーザーの操作や入力とは関係なく、長期間または恒久的な非同期タスクを実行できます。

詳細については、次を参照してください。 [What is a Cloud Service?]します。

> [AZURE.NOTE] *単純な web サイトを構築するたいとお考えですか?*
シナリオが単純な Web サイトのフロントエンドにのみ関係している場合は、Azure App Service の軽量の Web Apps 機能を使用することを検討してください。 Web サイトの規模が増大し、要件が変化したときには、容易にクラウド サービスにアップグレードできます。 参照してください、 <a href="/develop/python/">Python デベロッパー センター</a> Azure App Service で Web アプリの機能の開発を取り上げた記事です。
<br />


## プロジェクトの作成

Visual Studio で選択 **Azure クラウド サービス** で、 **新しいプロジェクト** ダイアログ ボックスで、 **Python**します。

![[新しいプロジェクト] ダイアログ](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Azure クラウド サービス ウィザードでは、新しい Web ロールまたは worker ロールを作成できます。

![Azure クラウド サービス ダイアログ](./media/cloud-services-python-ptvs/new-service-wizard.png)

worker ロール テンプレートには、Azure ストレージ アカウントまたは Azure Service Bus に接続するための定型コードが含まれています。

![クラウド サービス ソリューション](./media/cloud-services-python-ptvs/worker.png)

Web ロールまたは worker ロールは、既存のクラウド サービスにいつでも追加することができます。  既存のプロジェクトをソリューションに追加するか、または新たに作成するかを選択できます。

![Add Role Command](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

クラウド サービスには、異なる言語で実装されたロールを含めることができます。  たとえば、Django を使用して実装された Python Web ロールを、Python worker ロールや C# worker ロールと共存させることができます。  ロール間のやり取りは、Service Bus キューまたはストレージ キューを使用することで簡単に行うことができます。

## ローカルで実行する

クラウド サービス プロジェクトをスタートアップ プロジェクトとして設定し、F5 キーを押した場合、ローカルの Azure エミュレーター内でクラウド サービスが実行されます。

PTVS (Python Tools for Visual Studio) はエミュレーターでの起動をサポートしていますが、デバッグ操作 (ブレークポイントなど) は機能しません。

Web ロールまたは worker ロールをデバッグするには、対象となるロール プロジェクトをスタートアップ プロジェクトに設定したうえで、デバッグするようにしてください。  複数のスタートアップ プロジェクトを設定することもできます。  ソリューションを右クリックし、[ **[スタートアップ プロジェクトの**です。

![ソリューション スタートアップ プロジェクト プロパティ](./media/cloud-services-python-ptvs/startup.png)

## Azure に発行する

発行、ソリューションのクラウド サービス プロジェクトを右クリックし、選択 **発行**します。

![Microsoft Azure 発行サインイン](./media/cloud-services-python-ptvs/publish-sign-in.png)

[設定] ページで、発行するクラウド サービスを選択します。

![Microsoft Azure 発行設定](./media/cloud-services-python-ptvs/publish-settings.png)

そのクラウド サービスがまだない場合は、新しいクラウド サービスを作成できます。

![Create Cloud Service Dialog](./media/cloud-services-python-ptvs/publish-create-cloud-service.png)

エラーをデバッグする際の利便性を高めるために、コンピューターへのリモート デスクトップ接続を有効にすることもできます。

![リモート デスクトップ構成ダイアログ](./media/cloud-services-python-ptvs/publish-remote-desktop-configuration.png)

設定の構成が完了したら、クリックして **発行**します。

出力ウィンドウにいくつかの進行状況が表示された後、[Microsoft Azure のアクティビティ ログ] ウィンドウが表示されます。

![Microsoft Azure Activity Log Window](./media/cloud-services-python-ptvs/publish-activity-log.png)

数分経過するとデプロイメントが完了し、Web ロールまたは worker ロールが Azure 上で稼働状態となります。

## 次のステップ

Python Tools for Visual Studio で Web ロールまたは worker ロールを扱う方法の詳細については、次の PTVS 関連ドキュメントを参照してください。

- [クラウド サービス プロジェクト][]

Web ロールまたは worker ロールから Azure Storage や Service Bus などの Azure サービスを使用する方法の詳細については、次の記事を参照してください。

- [BLOB サービス][]
- [テーブル サービス][]
- [キュー サービス][]
- [Service Bus キュー][]
- [Service Bus トピック][]


<!--Link references-->

[What is a Cloud Service?]: /manage/services/cloud-services/what-is-a-cloud-service/
[execution model-web sites]: fundamentals-application-models.md#WebSites
[execution model-vms]: fundamentals-application-models.md#VMachine
[execution model-cloud services]: fundamentals-application-models.md#CloudServices
[Python Developer Center]: /develop/python/

[Blob Service]: storage-python-how-to-use-blob-storage.md
[Queue Service]: storage-python-how-to-use-queue-storage.md
[Table Service]: storage-python-how-to-use-table-storage.md
[Service Bus Queues]: service-bus-python-how-to-use-queues.md
[Service Bus Topics]: service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517191

