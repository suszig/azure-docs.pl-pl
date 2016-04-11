<properties
   pageTitle="Windows Server または Windows クライアントのファイルとフォルダーを Azure にバックアップ | Microsoft Azure"
   description="この簡単な手順で Windows Server または Windows クライアントを Azure にバックアップします。 数回の簡単なステップで Windows のファイルとフォルダーをクラウドにバックアップできます。"
   services="backup"
   documentationCenter=""
   authors="aashishr"
   manager="jwhit"
   editor=""
   keywords="windows server backup; backup windows server"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
     ms.date="11/17/2015"
     ms.author="jimpark"; "aashishr"/>

# Windows Server または Windows クライアントのファイルとフォルダーを Azure にバックアップする 
この簡単な手順で Windows のファイルとフォルダーを Azure にバックアップできます。 まだ行っていない場合、は、完了、 [の前提条件](backup-configure-vault.md#before-you-start) 続行する前に、Windows マシンをバックアップする環境を準備します。

## ファイルのバックアップ
1. コンピューターが登録されたら、Microsoft Azure Backup mmc スナップインを開きます。

    ![Search result](./media/backup-azure-backup-windows-server/result.png)

2. クリックして **のバックアップをスケジュール**

    ![Windows Server のバックアップ スケジュールを設定します](./media/backup-azure-backup-windows-server/schedulebackup.png)

3. バックアップする項目を選択します。 Windows サーバーまたは Windows クライアント (つまりで azure のバックアップ せず System Center Data Protection Manager) では、ファイルとフォルダーを保護することができます。

    ![Windows Server のバックアップ項目](./media/backup-azure-backup-windows-server/items.png)

4. 以下で詳しく説明されているバックアップのスケジュールと保持ポリシーを指定 [記事](backup-azure-backup-cloud-as-tape.md)します。

5. 最初のバックアップを送信する方法を選択します。 最初のシード処理を実施する方法は、バックアップするデータの量と、インターネットのアップロード リンク速度によって決まります。 GB または TB 単位のデータを大きな遅延が発生する低帯域幅接続でバックアップする場合は、最寄りの Azure データ センターにディスクを送付して、最初のバックアップを完了することをお勧めします。 これし、呼ばれ「オフライン バックアップ」はこれで詳しく説明 [記事](backup-azure-backup-import-export.md)します。 十分な帯域幅接続が確保できる場合は、ネットワーク経由で最初のバックアップを完了することをお勧めします。

    ![Windows Server の初回バックアップ](./media/backup-azure-backup-windows-server/initialbackup.png)

6. スケジュール バックアップ プロセスが完了すると、移動戻り、mmc スナップインにし、クリックして **を今すぐバックアップ** をネットワーク経由で最初のシード処理を完了します。

    ![Windows Server を今すぐバックアップする](./media/backup-azure-backup-windows-server/backupnow.png)

7. 最初のシード処理が完了した後、 **ジョブ** Azure Backup コンソールのビューでは、状態を示します。

    ![IR complete](./media/backup-azure-backup-windows-server/ircomplete.png)

## 次のステップ
- [Windows Server または Windows クライアントの管理](backup-azure-manage-windows-server.md)
- [Azure からの Windows Server または Windows クライアントの復元](backup-azure-restore-windows-server.md)
- [Azure Backup FAQ](backup-azure-backup-faq.md)


