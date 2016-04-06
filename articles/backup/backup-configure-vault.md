<properties
    pageTitle="Azure Backup サービスを Windows Server のバックアップ用に構成する | Microsoft Azure"
    description="このチュートリアルでは、Microsoft の Azure クラウド ソリューションの Backup サービスを使って、Windows Server をクラウドにバックアップする方法について学習します。"
    services="backup"
    documentationCenter=""
    authors="Jim-Parker"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="11/26/2015"
    ms.author="jimpark"; "aashishr"/>

# Windows コンピューターをバックアップする環境の準備

この記事では、Windows サーバーで Azure Backup を使用するための準備手順について説明します。 Windows サーバーまたは Windows クライアントを Azure にバックアップするには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](https://azure.microsoft.com/pricing/free-trial/)をご覧ください。
>[AZURE.NOTE] 以前の作成や、バックアップ サーバーを登録するために X.509 v3 証明書を取得する必要があります。 証明書は現在でもサポートされていますが、サーバーへの Azure 資格情報コンテナーの登録を容易にするために、[クイック スタート] ページから資格情報コンテナーの資格情報を作成できます。

## 開始する前に
ファイルやデータを Windows Server から Azure にバックアップするには、まず次のことを行う必要があります。

- **バックアップ コンテナーの作成** — でコンテナーの作成、 [Azure Backup の管理ポータル](http://manage.windowsazure.com)します。
- **資格情報コンテナーの資格情報をダウンロード** — Azure Backup コンテナーの [ダッシュ ボード] ページから、Windows マシン バックアップ コンテナーに登録するために使用する資格情報コンテナーの資格情報をダウンロードします。
- **Azure Backup エージェントをインストールし、サーバー登録** -ダッシュ ボード ページからダウンロードするリンクをクリックして、 [Azure Backup エージェント](http://aka.ms/azurebackup_agent)します。 エージェントをインストールし、コンテナー資格情報を使用してサーバーをバックアップ コンテナーに登録します。

[AZURE.INCLUDE [backup-create-vault-wgif](../../includes/backup-create-vault-wgif.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## 次のステップ
- [Windows Server または Windows クライアントのバックアップ](backup-azure-backup-windows-server.md)
- [Windows Server または Windows クライアントの管理](backup-azure-manage-windows-server.md)
- [Azure からの Windows Server または Windows クライアントの復元](backup-azure-restore-windows-server.md)
- [Azure Backup FAQ](backup-azure-backup-faq.md)
- [Azure Backup フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=290933)


