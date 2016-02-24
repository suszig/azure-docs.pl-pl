<properties 
   pageTitle="バックアップからの StorSimple ボリュームの復元 | Microsoft Azure"
   description="StorSimple Manager サービスの [バックアップ カタログ] ページを使用してバックアップ セットから StorSimple ボリュームを復元する方法について説明します。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="12/14/2015"
   ms.author="v-sharos" />

# バックアップ セットからの StorSimple ボリュームの復元

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## 概要

 **バックアップ カタログ** ページには、手動または自動バックアップを実行時に作成されるすべてのバックアップ セットが表示されます。 このページを使用すると、バックアップ ポリシーまたはボリュームのすべてのバックアップを一覧表示したり、バックアップを選択または削除したりできます。また、バックアップを使用してボリュームを復元または複製することもできます。

 ![[バックアップ カタログ] ページ](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

このチュートリアルを使用する方法を説明する、 **バックアップ カタログ** バックアップ セットからデバイスを復元するページです。

## バックアップ カタログの使用方法 

 **バックアップ カタログ** セットの選択をバックアップを絞り込むクエリ ページを提供します。 次のパラメーターに基づいて、取得するバックアップ セットをフィルター選択できます。

- **デバイス** – バックアップ セットが作成されたデバイス。
- **バックアップ ポリシー** または **ボリューム** – バックアップ ポリシーまたはこのバックアップ セットに関連付けられているボリューム。
- **** と **に** – バックアップ セットが作成された日付と時刻の範囲。

フィルター選択されたバックアップ セットは、次の属性に基づいて表形式で表示されます。

- **名前** – バックアップ ポリシー、バックアップ セットに関連付けられているボリュームの名前を指定します。
- **サイズ** – バックアップ セットの実際のサイズ。
- **上に作成** – バックアップが作成された日付と時刻。 
- **型** – バックアップ セットがローカル スナップショットし、クラウド スナップショット。 ローカル スナップショットは、デバイスにローカルに格納されているすべてのボリューム データのバックアップです。クラウド スナップショットは、クラウドに存在するボリューム データのバックアップを表します。 ローカル スナップショットは、より高速なアクセスを実現します。クラウド スナップショットは、データの回復力を実現するために選択されます。
- **によって開始された** –、スケジュールに従ってまたは手動でユーザーに、バックアップを自動的に開始することができます。 (バックアップ ポリシーを使用してバックアップのスケジュールを設定できます。 また、使用することができます、 **バックアップ** 対話形式でバックアップするにはオプションです)。

## StorSimple ボリュームをバックアップから復元する方法

使用することができます、 **バックアップ カタログ** 特定のバックアップから StorSimple ボリュームを復元するページです。 ただし、ボリュームを復元するとボリュームはバックアップが作成された時点の状態に戻ることに注意してください。 バックアップ操作の後に追加されたすべてのデータは失われます。

> [AZURE.WARNING] バックアップから復元すると、バックアップからの既存のボリュームが置き換えられます。 その結果、バックアップが作成された時点以降に書き込まれたすべてのデータが失われます。


### バックアップ セットから復元するには

1. StorSimple Manager サービスのページをクリックして、 **バックアップ カタログ** ] タブをクリックします。

    ![バックアップ カタログ](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. 次の手順に従って、バックアップ セットを選択します。
  1. 適切なデバイスを選択します。
  2. ボックスの一覧で、選択するバックアップのボリュームまたはバックアップのポリシーを選択します。
  3. 時間範囲を指定します。
  4. チェック マーク アイコンをクリックします。 ![チェック マーク アイコン](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) このクエリを実行します。
 
    The backups associated with the selected volume or backup policy should appear in the list of backup sets.

3. バックアップ セットを展開して、関連付けられているボリュームを表示します。 これらのボリュームは、復元する前にホストおよびデバイス上でオフラインにする必要があります。 ボリュームにアクセス、 **ボリューム コンテナー** ] ページで、次の手順に従い [ボリュームをオフライン](storsimple-manage-volumes.md#take-a-volume-offline) をオフラインにします。

    >  [AZURE.IMPORTANT] 実行したこと、ホストでボリュームをオフライン最初に、デバイスのボリュームをオフラインにする前に確認してください。 ホスト上でボリュームをオフラインにしない場合、データの破損を招く可能性があります。

4. 戻り、 **バックアップ カタログ** タブし、バックアップ セットを選択します。

5. クリックして **復元** ページの下部にあります。

6. 確認を求められます。 

    ![確認ページ](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. 復元情報を確認し、チェック アイコンをクリックして ![チェック マーク アイコン](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png)します。 これにアクセスして表示できる復元ジョブが開始されます、 **ジョブ** ページです。 

8. 復元操作が完了した後、ボリュームの内容がバックアップからのボリュームで置き換えられていることを確認できます。

![使用可能なビデオ](./media/storsimple-restore-from-backup-set/Video_icon.png) **ビデオ**

クローンを使用し、削除されたファイルを回復する StorSimple の機能を復元する方法を説明するビデオを見るには、次のようにクリックします。 [ここ](http://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/)します。

## 次のステップ

- 学習方法 [管理 StorSimple ボリューム](storsimple-manage-volumes.md)します。

- 学習方法 [StorSimple Manager サービスを使用して、StorSimple デバイスを管理する](storsimple-manager-service-administration.md)です。
