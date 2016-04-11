<properties
    pageTitle="Azure Backup コンテナーとサーバーの管理 | Microsoft Azure"
    description="このチュートリアルでは、Azure Backup コンテナーとサーバーを管理する方法について説明します。"
    services="backup"
    documentationCenter=""
    authors="aashishr"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="jimpark; aashishr; giridham"/>


#  Azure Backup コンテナーとサーバーの管理
この記事では、管理ポータルで使用できるバックアップ管理タスクの概要を説明します。

1. サインイン、 [管理ポータル](https://manage.windowsazure.com)します。
2. クリックして **Recovery Services**, 、クイック スタート ページを表示するバックアップ資格情報コンテナーの名前をクリックします。

    [クイック スタート] ページの上部にあるオプションを選択すると、利用可能な管理タスクを確認できます。

    ![保護された項目](./media/backup-azure-manage-windows-server/RS_tabs.png)

## ダッシュボード
選択 **ダッシュ ボード** 、サーバーの使用状況の概要を表示します。 [ダッシュボード] の下部で、次のタスクを実行できます。

- **証明書の管理**します。 証明書を使用してサーバーを登録した場合は、これを使用して証明書を更新します。 コンテナーの資格情報を使用している場合は使用しないで **証明書の管理**します。
- **削除**します。 現在のバックアップ コンテナーを削除します。 バックアップ コンテナーがもう使用されていない場合は、そのコンテナーを削除してストレージ領域を解放できます。 **削除** はすべての登録済みサーバーが、資格情報コンテナーから削除された後にのみ有効です。
- **コンテナーの資格情報**します。 [概要] メニュー項目を使用して資格情報コンテナーの資格情報を構成します。

## 保護された項目
選択 **保護された項目** サーバーからバックアップされたアイテムを表示します。 この一覧は、情報の提供のみを目的としています。

![保護された項目](./media/backup-azure-manage-windows-server/RS_protecteditems.png)

## 登録済みの項目
選択 **登録されている項目** この資格情報コンテナーに登録されているサーバーの名前を表示します。

![削除されたサーバー](./media/backup-azure-manage-windows-server/RS_deletedserver.png)

ここでは、次のタスクを実行できます。

- **再登録を許可** - このオプションを使用できるサーバーを選択する場合は、 **登録ウィザード** 、エージェントとバックアップ、サーバーを登録するをもう一度資格情報コンテナーのです。 証明書でエラーが発生したとき、またはサーバーを再構築する必要があった場合などに、再登録が必要になる可能性があります。
- **削除** -バックアップ資格情報コンテナーからサーバーを削除します。 サーバーに関連付けられている保存されたデータすべてがすぐに削除されます。

## 次のステップ
- [Azure からの Windows Server または Windows クライアントの復元](backup-azure-restore-windows-server.md)
- Azure Backup の詳細については、次を参照してください [Azure Backup の概要。](backup-introduction-to-azure-backup.md)
- 参照してください、 [Azure のバックアップ フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=290933)

