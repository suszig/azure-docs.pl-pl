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

この記事では、Windows サーバーで Azure Backup を使用するための準備手順について説明します。 Windows サーバーまたは Windows クライアントを Azure にバックアップするには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版アカウントを数分で作成することができます。 詳細については、「 [Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
>[AZURE.NOTE] 以前は、バックアップ サーバーを登録するために、X.509 v3 証明書の作成や取得が必要でした。 証明書は現在でもサポートされていますが、サーバーへの Azure 資格情報コンテナーの登録を容易にするために、[クイック スタート] ページから資格情報コンテナーの資格情報を作成できます。

## 開始する前に

ファイルやデータを Windows Server から Azure にバックアップするには、まず次のことを行う必要があります。

- **バックアップ コンテナーの作成** — でコンテナーの作成、 [Azure Backup の管理ポータル](http://manage.windowsazure.com)します。
- **コンテナー資格情報のダウンロード** — Azure Backup コンテナーの [ダッシュボード] ページから、バックアップ コンテナーに Windows コンピューターを登録する際に使用するコンテナー資格情報をダウンロードします。
- **Azure Backup エージェントをインストールし、サーバー登録** -ダッシュ ボード ページからダウンロードするリンクをクリックして、 [Azure Backup エージェント](http://aka.ms/azurebackup_agent)します。 エージェントをインストールし、コンテナー資格情報を使用してサーバーをバックアップ コンテナーに登録します。

[AZURE.INCLUDE [backup-create-vault-wgif](../../includes/backup-create-vault-wgif.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## 次のステップ

- [バックアップの Windows Server または Windows クライアント](backup-azure-backup-windows-server.md)
- [Windows Server または Windows クライアントを管理します。](backup-azure-manage-windows-server.md)
- [Azure からの Windows Server または Windows クライアントを復元します。](backup-azure-restore-windows-server.md)
- [Azure Backup の FAQ](backup-azure-backup-faq.md)
- [Azure のバックアップ フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=290933)





