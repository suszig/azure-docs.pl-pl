<properties
   pageTitle="Azure からデータを Windows Server または Windows クライアント復元 |Microsoft Azure"
   description="Windows Server または Windows クライアントからの復元方法について説明します。"
   services="backup"
   documentationCenter=""
   authors="aashishr"
   manager="jwhit"
   editor=""/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
     ms.date="10/21/2015"
     ms.author="jimpark"; "aashishr"/>

# Windows Server または Windows クライアント コンピューターへのファイルの復元
この記事では、復元操作に必要な 2 種類の手順を説明します。

- バックアップが実行されたのと同じマシンにデータを復元する
- その他の任意のマシンにデータを復元する

いずれの場合も、データは Azure Backup コンテナーから取得されます。

## 同じコンピューターへのデータの回復
ファイルを誤って削除してしまったため、バックアップを実行したのと同じコンピューターにそのファイルを復元するには、次の手順でデータを回復できます。

1. 開いている、 **Microsoft Azure Backup** スナップインです。
2. クリックして **データの回復** 、ワークフローを開始します。

    ![Recover Data](./media/backup-azure-restore-windows-server/recover.png)

3. 選択、 **このサーバー (*コンピューター名*) * * ファイルを同じコンピューターにバックアップを復元するにはオプションです。

    ![Same machine](./media/backup-azure-restore-windows-server/samemachine.png)

4. 選択 **ファイルの参照** または **ファイルを検索する**です。

    パスがわかっている 1 つまたは複数のファイルを復元する場合は、既定のオプションをそのまま使用します。 フォルダー構造がわからないが、ファイルを検索するには、選択、 **ファイルを検索する** オプション。 ここでは、既定のオプションを使用します。

    ![Browse files](./media/backup-azure-restore-windows-server/browseandsearch.png)

5. ファイルの復元元となるボリュームを選択します。

    任意の時点から復元できます。 表示される日付 **太字** カレンダー コントロールには、復元ポイントの可用性を示します。 日付を選択すると、バックアップ スケジュールを設定 (および、バックアップ操作が成功した場合) に基づくできる点を選択したから特定の **時間** ボックスの一覧です。

    ![Volume and Date](./media/backup-azure-restore-windows-server/volanddate.png)

6. 回復する項目を選択します。 復元するフォルダーまたはファイルを複数選択することができます。

    ![Select files](./media/backup-azure-restore-windows-server/selectfiles.png)

7. 回復パラメーターを指定します。

    ![Recovery options](./media/backup-azure-restore-windows-server/recoveroptions.png)

  - 元の場所に復元することも (ファイルまたはフォルダーが上書きされます)、同じコンピューターの別の場所に復元することもできます。
  - 復元するファイルまたはフォルダーが復元先の場所に存在する場合、コピーを作成するか (同じファイルの 2 つのバージョンが作成されます)、復元先の場所にあるファイルを上書きするか、復元先に存在するファイルの回復をスキップすることができます。
  - 復元されるファイルの ACL を復元するという既定のオプションは、そのままにしておくことを強くお勧めします。

8. これらの入力を指定すると、クリックして **次**します。 このコンピューターにファイルを復元する回復ワークフローが開始します。

## 別のコンピューターへの回復
サーバー全体が失われた場合でも、 Azure Backup から別のコンピューターにデータを回復できます。 次の手順はそのワークフローを示しています。  

この手順で使用される用語は次のとおりです。

- *ソース マシン* – 元のコンピューターが、バックアップが作成されたとは現在使用できません。
- *ターゲット マシン* – データの回復先となるコンピューター。
- *サンプルの資格情報コンテナー* – する Backup vault、 *ソース マシン* と *ターゲット マシン* 登録されています。 <br/>

> [AZURE.NOTE] 以前のバージョンのオペレーティング システムを実行しているコンピューターでは、マシンから作成されたバックアップを復元できません。 たとえば、バックアップが Windows 7 コンピューターで行われた場合、Windows 8 以上のコンピューターにそのバックアップを復元できます。 ただし、その逆は当てはまりません。

1. 開いている、 **Microsoft Azure Backup** のスナップイン、 *ターゲット マシン*します。
2. いることを確認、 *ターゲット マシン* と *ソース マシン* 同じバックアップ資格情報コンテナーに登録されます。
3. クリックして **データの回復** 、ワークフローを開始します。

    ![Recover Data](./media/backup-azure-restore-windows-server/recover.png)

4. 選択 **別のサーバー**

    ![Another Server](./media/backup-azure-restore-windows-server/anotherserver.png)

5. 対応する資格情報コンテナーの資格情報ファイルを指定して、 *サンプルの資格情報コンテナー*します。 資格情報コンテナーの資格情報ファイルが無効な (または期限切れになった) 場合から新しい資格情報コンテナー資格情報ファイルをダウンロード、 *サンプルの資格情報コンテナー* Azure ポータルで。 資格情報コンテナーの資格情報ファイルを指定すると、その資格情報コンテナーの資格情報ファイルに対するバックアップ資格情報コンテナーが表示されます。

6. 選択、 *ソース マシン* 表示されているコンピューターの一覧からです。

    ![List of machines](./media/backup-azure-restore-windows-server/machinelist.png)

7. いずれかを選択、 **ファイルを検索する** または **ファイルの参照** オプション。 使用してこのセクションの目的で、 **ファイルを検索する** オプション。

    ![Search](./media/backup-azure-restore-windows-server/search.png)

8. 次の画面で、ボリュームと日付を選択します。 復元するフォルダー名とファイル名を検索します。

    ![Search items](./media/backup-azure-restore-windows-server/searchitems.png)

9. ファイルの復元先にする場所を選択します。

    ![Restore location](./media/backup-azure-restore-windows-server/restorelocation.png)

10. 際に指定した暗号化パスフレーズを指定 *ソース マシン* 登録を *サンプルの資格情報コンテナー*します。

    ![暗号化](./media/backup-azure-restore-windows-server/encryption.png)

11. 入力されたしたら **回復**, 、指定されている場所にバックアップされたファイルの復元をトリガーします。

## 次のステップ
- [Azure Backup FAQ](backup-azure-backup-faq.md)
- 参照してください、 [Azure のバックアップ フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=290933)します。

## 詳細情報
- [Azure Backup の概要](http://go.microsoft.com/fwlink/p/?LinkId=222425)
- [Azure 仮想マシンのバックアップ](backup-azure-vms-introduction.md)
- [Microsoft ワークロードのバックアップ](backup-azure-dpm-introduction.md)


